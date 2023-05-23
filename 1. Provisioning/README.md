
# Provision servers using Terraform



Pertama-tama saya membuat file config `main.tf` untuk provision idch server menggunakan terraform

```
terraform {
  required_providers {
    idcloudhost = {
      source = "bapung/idcloudhost"
      version = "0.1.3"
    }
  }
}

provider "idcloudhost" {
    auth_token = " "
    region = "sgp01"
}

resource "idcloudhost_vm" "nafis-appserver" {
    name = "nafis-appserver"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 2
    memory = 2048
    username = "nafis"
    initial_password = "Katasand1" 
    billing_account_id =  
    public_key = " " 
    backup = false
}

resource "idcloudhost_vm" "nafis-gateway" {
    name = "nafis-gateway"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 1
    memory = 1024
    username = "nafis"
    initial_password = "Katasand1" 
    billing_account_id =  
    public_key = " " 
    backup = false
}

resource "idcloudhost_vm" "nafis-monitoring" {
    name = "nafis-monitoring"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 2
    memory = 2048
    username = "nafis"
    initial_password = "Katasand1" 
    billing_account_id =  
    public_key = " " 
    backup = false
}

resource "idcloudhost_vm" "nafis-cicd" {
    name = "nafis-cicd"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 2
    memory = 2048
    username = "nafis"
    initial_password = "Katasand1" 
    billing_account_id =  
    public_key = " " 
    backup = false
}

resource "idcloudhost_floating_ip" "ip-appserver" {
    name = "nafis-appserver"
    billing_account_id =  
    assigned_to = idcloudhost_vm.nafis-appserver.id
}

resource "idcloudhost_floating_ip" "ip-gateway" {
    name = "nafis-gateway"
    billing_account_id =  
    assigned_to = idcloudhost_vm.nafis-gateway.id
}

resource "idcloudhost_floating_ip" "ip-monitoring" {
    name = "nafis-monitoring"
    billing_account_id =  
    assigned_to = idcloudhost_vm.nafis-monitoring.id
}

resource "idcloudhost_floating_ip" "ip-cicd" {
    name = "nafis-cicd"
    billing_account_id =  
    assigned_to = idcloudhost_vm.nafis-cicd.id
}
```

selanjutnya saya melakukan inisiasi pada folder terraform dengan perintah :

```
terraform init
```

![image](/1.%20Provisioning/media/1.png)

perintah dibawah ini saya gunakan untuk melihat spesifikasi server yang akan di provisioning

```
terraform plan
```

![image](/1.%20Provisioning/media/2.png)

![image](/1.%20Provisioning/media/3.png)

jika sudah sesuai gunakan perintah dibawah ini

```
terraform apply
```

![image](/1.%20Provisioning/media/4.png)

![image](/1.%20Provisioning/media/5.png)

![image](/1.%20Provisioning/media/6.png)

![image](/1.%20Provisioning/media/8.png)

![image](/1.%20Provisioning/media/9.png)

![image](/1.%20Provisioning/media/10.png)

# Server Configuration using Ansible

disini saya membuat file config `ansible.cfg` untuk konfigurasi dari ansible

```
[defaults]
inventory = Inventory
private_key_file = ~/.ssh/id_rsa
host_key_checking = false
interpreter_python = auto_silent
```

lalu file config `Inventory` untuk library dari server yang akan dikonfigurasi dengan ansible

```
[appserver]
103.37.125.112

[gateway]
103.37.125.189

[monitoring]
103.37.124.52

[cicd]
103.37.125.86

[all:vars]
ansible_user="nafis"
ansible_pythone_interpreter=/usr/bin/python3
```

selanjutnya instalasi konfigurasi server akan menggunakan perintah dibawah ini

```
ansible-playbook file-name.yml
```

file config `install-docker.yml` untuk instalasi docker pada semua server

```
---
- hosts: all
  become: true
  vars:
    container_count: 4
    default_container_name: docker
    default_container_image: ubuntu
    default_container_command: sleep 1d

  tasks:
    - name: Install aptitude
      apt:
        name: aptitude
        state: latest
        update_cache: true
    
    - name: Correct Problem
      command: sudo dpkg --configure -a

    - name: Install required system packages
      apt:
        pkg:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
          - python3-pip
          - virtualenv
          - python3-setuptools
        state: latest
        update_cache: true

    - name: Add Docker GPG apt Key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker Repository
      apt_repository:
        repo: deb https://download.docker.com/linux/ubuntu focal stable
        state: present

    - name: Update apt and install docker-ce
      apt:
        name: docker-ce
        state: latest
        update_cache: true

    - name: Install Docker Module for Python
      pip:
        name: docker

    - name: add user docker group
      user:
        name: nafis
        groups: docker
        append: yes
```

![image](/1.%20Provisioning/media/11.png)

![image](/1.%20Provisioning/media/12.png)

saya buat file config nginx `rproxy.conf` untuk kebutuhan reverse proxy

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

file config `install-nginx.yml` untuk instalasi nginx pada server gateway

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

![image](/1.%20Provisioning/media/13.png)

![image](/1.%20Provisioning/media/14.png)

![image](/1.%20Provisioning/media/15.png)

file config `install-jenkins.yml` untuk instalasi jenkins pada server cicd

```
---
- become: true
  gather_facts: false
  hosts: cicd
  tasks:
    - name: jenkins on top docker
      community.docker.docker_container:
        name: jenkins
        image: jenkins/jenkins:latest
        ports: 
          - 8080:8080
        volumes:
          - ~/ansible/config/jenkins:/var/jenkins_home
        restart_policy: unless-stopped
```

![image](/1.%20Provisioning/media/16.png)

![image](/1.%20Provisioning/media/17.png)

![image](/1.%20Provisioning/media/18.png)

file config `install-node.yml` untuk instalasi node-exporter pada semua server

```
---
- become: true
  gather_facts: false
  hosts: all
  tasks:
    - name: node-exporter on top docker
      community.docker.docker_container:
        name: node-exporter
        image: prom/node-exporter
        ports: 
          - 9100:9100
        restart_policy: unless-stopped
```

![image](/1.%20Provisioning/media/19.png)

![image](/1.%20Provisioning/media/20.png)

file config `install-prometheus.yml` untuk instalasi prometheus pada server monitoring

```
---
- become: true
  gather_facts: false
  hosts: monitoring
  tasks:
    - name: prometheus.yml 
      copy:
        dest: ~/prometheus
        content: |
          scrape_configs:
            - job_name: grafana   
              scrape_interval: 5s
              static_configs:
              - targets:
                - node-appserver.nafis.studentdumbways.my.id
                - node-gateway.nafis.studentdumbways.my.id
                - node-cicd.nafis.studentdumbways.my.id
                - node-monitoring.nafis.studentdumbways.my.id
    - name: prometheus on top docker
      community.docker.docker_container:
        name: prometheus
        image: prom/prometheus
        ports: 
          - 9090:9090
        restart_policy: unless-stopped      
        volumes:
          - ~/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
```

![image](/1.%20Provisioning/media/21.png)

![image](/1.%20Provisioning/media/22.png)

file config `install-grafana.yml` untuk instalasi grafana pada server monitoring

```
---
- become: true
  gather_facts: false
  hosts: monitoring
  tasks:
    - name: permission directory 
      ansible.builtin.file:
        path: ~/grafana
        state: directory
        mode: "0755"
    - name: grafana on top docker
      community.docker.docker_container:
        name: grafana
        image: grafana/grafana
        ports: 
          - 3000:3000
        restart_policy: unless-stopped
        volumes:
          - ~/grafana:/var/lib/grafana
        user: root
```

![image](/1.%20Provisioning/media/23.png)
![image](/1.%20Provisioning/media/24.png)
