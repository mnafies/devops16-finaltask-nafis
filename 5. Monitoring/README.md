# Monitoring

## Node-exporter

disini saya menyematkan node exporter pada 3 server :

- appserver

![image](/5.%20Monitoring/media/2.png)

- CI/CD

![image](/5.%20Monitoring/media/1.png)

- gateway

![image](/5.%20Monitoring/media/3.png)

## Prometheus

disini prometheus sudah terinstall pada server monitoring berikut sudah disematkan file config `prometheus.yml` untuk penambahan target node-exporter

![image](/5.%20Monitoring/media/4.png)

## Setup Grafana

pertama saya menambahkan data source prometheus terlebih dahulu

![image](/5.%20Monitoring/media/5.png)

![image](/5.%20Monitoring/media/6.png)

![image](/5.%20Monitoring/media/7.png)

![image](/5.%20Monitoring/media/8.png)

kemudian untuk dashboard disini saya menggunakan template json

![image](/5.%20Monitoring/media/9.png)

![image](/5.%20Monitoring/media/10.png)

![image](/5.%20Monitoring/media/11.png)

![image](/5.%20Monitoring/media/12.png)


## Alert Grafana

untuk alert grafana disini menggunakan telegram untuk notifikasi

![image](/5.%20Monitoring/media/13.png)

![image](/5.%20Monitoring/media/17.png)

![image](/5.%20Monitoring/media/18.png)

![image](/5.%20Monitoring/media/19.png)

![image](/5.%20Monitoring/media/20.png)

### Alert rules 

alert rules yang saya buat disini untuk 3 parameter yaitu :

- CPU Usage Above 20%

![image](/5.%20Monitoring/media/21.png)

- RAM Usage Above 70%

![image](/5.%20Monitoring/media/22.png)

- Disk Usage Above 80%

![image](/5.%20Monitoring/media/23.png)

![image](/5.%20Monitoring/media/24.png)

![image](/5.%20Monitoring/media/25.png)