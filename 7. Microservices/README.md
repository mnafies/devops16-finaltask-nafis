disini saya sudah instalasi kubernetes minikube sesuai instruksi dari https://minikube.sigs.k8s.io/docs/start/

Menjalankan minikube dengan perintah dibawah ini

```
minikube start
```

![image](/7.%20Microservices/media/1.png)

menjalankan minikube dashboard dengan perintah dibawah ini

```
minikube dashboard
```

![image](/7.%20Microservices/media/2.png)

![image](/7.%20Microservices/media/3.png)

disini saya membuat file configurasi untuk frontend dumbmerch

`dumbmerch.yml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dumbmerch
  labels:
    app: dumbmerch
spec:
  replicas: 3
  selector:
    matchLabels:
      app: dumbmerch
  template:
    metadata:
      labels:
        app: dumbmerch
    spec:
      containers:
      - name: dumbmerch
        image: nikymn/dumbmerch-fe
        stdin: true
        tty: true
        ports:
        - containerPort: 3000
```

`dumbmerch-svc.yml`

```
apiVersion: v1
kind: Service
metadata:
  name: dumbmerch
spec:
  type: NodePort
  selector:
    app: dumbmerch
  ports:
    - protocol: TCP
      nodePort: 31000
      port: 3000
      targetPort: 3000
```

```
minikube kubectl -- create -f dumbmerch.yml
minikube kubectl -- apply -f dumbmerch-svc.yml
```

![image](/7.%20Microservices/media/4.png)

kemudian lihat cluster yang sudah berjalan dengan perintah dibawah ini

```
minikube kubectl get all
```

![image](/7.%20Microservices/media/5.png)

jika sudah tersedia jalankan service dengan perintah dibawah ini

```
minikube service nameservice
```

![image](/7.%20Microservices/media/6.png)

![image](/7.%20Microservices/media/7.png)