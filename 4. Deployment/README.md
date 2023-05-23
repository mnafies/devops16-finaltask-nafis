# Deployment

## Staging

### frontend dumbmerch staging (A lightweight docker image)

Pada branch staging saya membuat docker images sekecil mungkin dengan .dockerignore untuk node_modules

```
git clone git@github.com:nikymn/fe-dumbmerch.git
```

`git checkout staging`

![image](/4.%20Deployment/media/1.png)

`.dockerignore`

```
**/node_modules/
```

`Dockerfile`

```
FROM node:16-slim
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "npm", "start" ]
```

```
docker build -t nikymn/fe-dumbmerch-staging:latest .
```

### backend dumbmerch staging (A lightweight docker image)

```
git clone git@github.com:nikymn/be-dumbmerch.git
```

`git checkout staging`

`.dockerignore`

```
.git
**/*.tar.gz
**/*.tgz
**/*.iso
**/*.raw
env/*/work-*
```

`Dockerfile`

```
FROM golang:1.16-alpine
RUN mkdir /app   
COPY . /app   
WORKDIR /app   
RUN go get ./ && go build && go mod download
EXPOSE 5000
CMD ["go", "run", "main.go"]
```

```
docker build -t nikymn/be-dumbmerch-staging:latest .
```

### docker compose

`docker-compose.yml`

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

```
docker compose up -d
```


## Production

### frontend dumbmerch 

`git checkout production`

`Dockerfile`

```
FROM node:16-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "npm", "start" ]
```

```
docker build -t nikymn/fe-dumbmerch-production:latest .
```

### backend dumbmerch

`git checkout production`

`Dockerfile`

```
FROM golang:1.16-alpine
RUN mkdir /app   
COPY . /app   
WORKDIR /app   
RUN go get ./ && go build && go mod download
EXPOSE 5000
CMD ["go", "run", "main.go"]
```

```
docker build -t nikymn/be-dumbmerch-production:latest .
```

### docker compose

`docker-compose.yml`

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

```
docker compose up -d
```

# CICD

# Setup Jenkins

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