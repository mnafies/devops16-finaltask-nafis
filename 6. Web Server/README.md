# Web Server

## Cloudflare

![image](/6.%20Web%20Server/media/1.png)

## NGINX 

file config `install-nginx.yml` untuk isntalasi nginx pada server gateway

```
---
- become: true
  gather_facts: false
  hosts: gateway
  tasks:
    - name: Installing nginx
      apt:
        name: nginx
        state: latest
        update_cache: yes
    - name: Start nginx
      service:
        name: nginx
        state: started
    - name: Copy rproxy.conf
      copy:
        src: /home/server/ansible/config/reverse-proxy/
        dest: /etc/nginx/sites-enabled
    - name: Check nginx
      command: sudo nginx -t
    - name: Reload nginx
      service:
        name: nginx
        state: reloaded
```
![image](/6.%20Web%20Server/media/2.png)

## SSL Certbot using wildcard

saya malakukan instalasi certbot menggunakan instruksi dari [certbot.eff.org](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal&tab=wildcard)

![image](/6.%20Web%20Server/media/3.png)

![image](/6.%20Web%20Server/media/4.png)

```

sudo certbot certonly \
--manual \
--preferred-challenges=dns \
--email muhamadnafis999@gmail.com \
--server https://acme-v02.api.letsencrypt.org/directory \
--agree-tos \
-d *.studentdumbways.my.id

```

![image](/6.%20Web%20Server/media/5.png)

![image](/6.%20Web%20Server/media/6.png)

sudo certbot certonly \
--manual \
--preferred-challenges=dns \
--email muhamadnafis999@gmail.com \
--server https://acme-v02.api.letsencrypt.org/directory \
--agree-tos \
-d *.studentdumbways.my.id


![image](/6.%20Web%20Server/media/7.png)

![image](/6.%20Web%20Server/media/8.png)

![image](/6.%20Web%20Server/media/13.png)

![image](/6.%20Web%20Server/media/9.png)

![image](/6.%20Web%20Server/media/10.png)

![image](/6.%20Web%20Server/media/11.png)

