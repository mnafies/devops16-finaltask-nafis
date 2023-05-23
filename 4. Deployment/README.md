# Deployment

## Staging

Pada branch staging saya membuat docker images sekecil mungkin dengan .dockerignore

tentunya saya sudah `git clone` pada repository github saya untuk frontend dan backend dumbmerch

### frontend dumbmerch staging (A lightweight docker image)

```
git clone git@github.com:nikymn/fe-dumbmerch.git
```

`git checkout staging`

![image](/4.%20Deployment/media/1.png)

`.dockerignore`

```
**/node_modules/
```

![image](/4.%20Deployment/media/2.png)

file config `Dockerfile` untuk kebutuhan membuat docker image `nikymn/fe-dumbmerch-staging`

```
FROM node:16-slim
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "npm", "start" ]
```

kemudian saya buat docker image dengan perintah dibawah ini

```
docker build -t nikymn/fe-dumbmerch-staging:latest .
```
![image](/4.%20Deployment/media/3.png)

### backend dumbmerch staging (A lightweight docker image)

```
git clone git@github.com:nikymn/be-dumbmerch.git
```

`git checkout staging`

![image](/4.%20Deployment/media/5.png)

`.dockerignore`

```
.git
**/*.tar.gz
**/*.tgz
**/*.iso
**/*.raw
env/*/work-*
```

![image](/4.%20Deployment/media/6.png)

file config `Dockerfile` untuk kebutuhan membuat docker image `nikymn/be-dumbmerch-staging`

```
FROM golang:1.16-alpine
RUN mkdir /app   
COPY . /app   
WORKDIR /app   
RUN go get ./ && go build && go mod download
EXPOSE 5000
CMD ["go", "run", "main.go"]
```

kemudian saya buat docker image dengan perintah dibawah ini

```
docker build -t nikymn/be-dumbmerch-staging:latest .
```

![image](/4.%20Deployment/media/7.png)

### docker compose

file config `docker-compose.yml` untuk kebutuhan docker compose

```
version: "3.8"
services:
   db:
    image: postgres:latest
    container_name: db-dumbmerch
    ports:
      - 5432:5432
    volumes:
      - ~/postgresql:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=nafis
      - POSTGRES_PASSWORD=katasandi
      - POSTGRES_DB=dumbmerch
   backend:
    depends_on:
      - db
    image: nikymn/be-dumbmerch-staging:latest
    container_name: be-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 5000:5000
   frontend:
    image: nikymn/be-dumbmerch-staging:latest
    container_name: fe-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 3000:3000
```

lalu saya jalankan docker compose dengan perintah dibawah ini

```
docker compose up -d
```

![image](/4.%20Deployment/media/8.png)

![image](/4.%20Deployment/media/9.png)

![image](/4.%20Deployment/media/10.png)

![image](/4.%20Deployment/media/11.png)

## Production

Pada branch staging saya membuat docker images yang sudah siap deploy

tentunya saya sudah `git clone` pada repository github saya untuk frontend dan backend dumbmerch

### frontend dumbmerch 

`git checkout production`

![image](/4.%20Deployment/media/12.png)

file config `Dockerfile` untuk kebutuhan membuat docker image `nikymn/fe-dumbmerch-production`

```
FROM node:16-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npx","serve","build","-l","3000"]
```

kemudian saya buat docker image dengan perintah dibawah ini

```
docker build -t nikymn/fe-dumbmerch-production:latest .
```

![image](/4.%20Deployment/media/13.png)

### backend dumbmerch

`git checkout production`

![image](/4.%20Deployment/media/14.png)

file config `Dockerfile` untuk kebutuhan membuat docker image `nikymn/be-dumbmerch-production`

```
#distroless
FROM golang:1.18 as build

WORKDIR /go/src/app
COPY . .

RUN go mod download
RUN CGO_ENABLED=0 go build -o /go/bin/app

FROM gcr.io/distroless/static-debian11
COPY --from=build /go/bin/app /
CMD ["/app"]
```

kemudian saya buat docker image dengan perintah dibawah ini

```
docker build -t nikymn/be-dumbmerch-production:latest .
```

![image](/4.%20Deployment/media/15.png)

### docker compose

file config `docker-compose.yml` untuk kebutuhan docker compose

```
version: "3.8"
services:
   db:
    image: postgres:latest
    container_name: db-dumbmerch
    ports:
      - 5432:5432
    volumes:
      - ~/postgresql:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=nafis
      - POSTGRES_PASSWORD=katasandi
      - POSTGRES_DB=dumbmerch
   backend:
    depends_on:
      - db
    image: nikymn/be-dumbmerch-production:latest
    container_name: be-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 5000:5000
   frontend:
    image: nikymn/be-dumbmerch-production:latest
    container_name: fe-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 3000:3000
```

lalu saya jalankan docker compose dengan perintah dibawah ini

```
docker compose up -d
```

![image](/4.%20Deployment/media/16.png)

![image](/4.%20Deployment/media/17.png)

# CICD

`Jenkinsfile` for frontend

```
def branch = "cicd"
def repo = "git@github.com:nikymn/fe-dumbmerch.git"
def cred = "appserver"
def dir = "~/fe-dumbmerch"
def server = "nafis@103.37.125.112"
def imagename = "dumbmerch-fe"
def dockerusername = "nikymn"

pipeline {
    agent any

    stages {
        stage('Repository pull') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
		                mkdir ${dir}
                        cd ${dir}
			            git init
                        git remote add origin ${repo}
			            git pull origin master
			            git branch ${branch}
			            git checkout ${branch}
			            git pull origin ${branch}
                        exit
                        EOF
                    """
                }
            }
        }

    stage('Image build') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${dir}
                        docker build -t ${imagename}:latest .
                        exit
                        EOF
                    """
                }
            }
        }

        stage('Running the image in a container') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${dir}
                        docker container stop ${imagename}
                        docker container rm ${imagename}
                        docker run -d -p 3000:3000 --name="${imagename}"  ${imagename}:latest
                        docker container stop ${imagename}
                        docker container rm ${imagename}
                        exit
                        EOF
                    """
                }
            }
        }

        stage('Image push') {
            steps {
               sshagent([cred]) {
			    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
				    docker tag ${imagename}:latest ${dockerusername}/${imagename}:latest
				    docker image push ${dockerusername}/${imagename}:latest
				    docker image rm ${dockerusername}/${imagename}:latest
				    docker image rm ${imagename}:latest
				    exit
                    EOF
			"""
		        }
            }
        }
    }
}
```


`Jenkinsfile` for backend

```
def branch = "cicd"
def repo = "git@github.com:nikymn/be-dumbmerch.git"
def cred = "appserver"
def dir = "~/be-dumbmerch"
def server = "nafis@103.37.125.112"
def imagename = "dumbmerch-be"
def dockerusername = "nikymn"

pipeline {
    agent any

    stages {
        stage('Repository pull') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
		                mkdir ${dir}
                        cd ${dir}
			            git init
                        git remote add origin ${repo}
			            git pull origin master
			            git branch ${branch}
			            git checkout ${branch}
			            git pull origin ${branch}
                        exit
                        EOF
                    """
                }
            }
        }

    stage('Image build') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${dir}
                        docker build -t ${imagename}:latest .
                        exit
                        EOF
                    """
                }
            }
        }

        stage('Running the image in a container') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${dir}
                        docker container stop ${imagename}
                        docker container rm ${imagename}
                        docker run -d -p 3000:3000 --name="${imagename}"  ${imagename}:latest
                        docker container stop ${imagename}
                        docker container rm ${imagename}
                        exit
                        EOF
                    """
                }
            }
        }

        stage('Image push') {
            steps {
               sshagent([cred]) {
			    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
				    docker tag ${imagename}:latest ${dockerusername}/${imagename}:latest
				    docker image push ${dockerusername}/${imagename}:latest
				    docker image rm ${dockerusername}/${imagename}:latest
				    docker image rm ${imagename}:latest
				    exit
                    EOF
			"""
		        }
            }
        }
    }
}
```

# Setup Jenkins

Platform CI/CD yang saya pakai kali yaitu jenkins

adapun instalasi yang saya lakukan sebagai berikut

disini saya memilih install suggested plugin

![image](/4.%20Deployment/media/18.png)

![image](/4.%20Deployment/media/19.png)

![image](/4.%20Deployment/media/20.png)

tidak lupa setelah selesai saya menambahkan plugin ssh agent 

![image](/4.%20Deployment/media/25.png)

Kemudian untuk credentials saya menambahkan credential untuk server appserver saya dengan menambahkan ID, User dan ssh public key dari appserver

![image](/4.%20Deployment/media/21.png)

![image](/4.%20Deployment/media/22.png)

![image](/4.%20Deployment/media/23.png)

![image](/4.%20Deployment/media/24.png)

Selanjutnya saya membuat pipeline untuk frontend dan backend 

![image](/4.%20Deployment/media/26.png)

![image](/4.%20Deployment/media/27.png)

![image](/4.%20Deployment/media/28.png)

![image](/4.%20Deployment/media/29.png)

jika sudah Build Now untuk menjalankan CI/CD

![image](/4.%20Deployment/media/30.png)

## Result

### CICD frontend dumbmerch

![image](/4.%20Deployment/media/31.png)

### CICD backend dumbmerch

![image](/4.%20Deployment/media/32.png)