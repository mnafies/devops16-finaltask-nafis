# Web Server


## NGINX 

`reverse proxy conf`

```
server { 
    server_name node-appserver.nafis.studentdumbways.my.id; 
    
    location / { 
             proxy_pass http://103.37.125.112:9100;
    }
}
server { 
    server_name node-gateway.nafis.studentdumbways.my.id; 
    
    location / { 
             proxy_pass http://103.37.125.189:9100;
    }
}
server { 
    server_name node-cicd.nafis.studentdumbways.my.id; 
    
    location / { 
             proxy_pass http://103.37.125.86:9100;
    }
}
server { 
    server_name node-monitoring.nafis.studentdumbways.my.id; 
    
    location / { 
             proxy_pass http://103.37.124.52:9100;
    }
}
server { 
    server_name prom.nafis.studentdumbways.my.id; 
    
    location / { 
             proxy_pass http://103.37.124.52:9090;
    }
}
server { 
    server_name dashboard.nafis.studentdumbways.my.id; 
    
    location / { 
             proxy_set_header Host dashboard.nafis.studentdumbways.my.id;
             proxy_pass http://103.37.124.52:3000;
    }
}
server { 
    server_name cicd.studentdumbways.my.id; 
    
    location / { 
             proxy_pass http://103.37.125.86:8080;
    }
}
server { 
    server_name nafis.studentdumbways.my.id; 
    
    location / { 
             proxy_pass http://103.37.125.112:3000;
    }
}

```

`install-nginx.yml`

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

## Certbot using wildcard

sudo certbot certonly \
--manual \
--preferred-challenges=dns \
--email muhamadnafis999@gmail.com \
--server https://acme-v02.api.letsencrypt.org/directory \
--agree-tos \
-d *.nafis.studentdumbways.my.id



