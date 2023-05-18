
# Provision servers using Terraform

`main.tf`

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
    auth_token = "mTWvAHka6YmhUncBxKZyCpeL40C7Kyc0"
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
    billing_account_id = 1200157626 
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
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
    billing_account_id = 1200157626 
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
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
    billing_account_id = 1200157626 
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
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
    billing_account_id = 1200157626 
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
    backup = false
}

resource "idcloudhost_floating_ip" "ip-appserver" {
    name = "nafis-appserver"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nafis-appserver.id
}

resource "idcloudhost_floating_ip" "ip-gateway" {
    name = "nafis-gateway"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nafis-gateway.id
}

resource "idcloudhost_floating_ip" "ip-monitoring" {
    name = "nafis-monitoring"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nafis-monitoring.id
}

resource "idcloudhost_floating_ip" "ip-cicd" {
    name = "nafis-cicd"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nafis-cicd.id
}
```

# Server Configuration using Ansible

`ansible.cfg`


```
[defaults]
inventory = Inventory
private_key_file = ~/.ssh/id_rsa
host_key_checking = false
interpreter_python = auto_silent
```

`Inventory`

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

`install-docker.yml`

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

`rproxy.conf`


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

`install-jenkins.yml`

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

`install-node.yml`

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

`install-prometheus.yml`

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

`install-grafana.yml`

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