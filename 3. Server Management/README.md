# Server Management

## SSH

pada saat provisioning dengan menggunakan terraform saya sudah menyematkan ssh public key pada setiap server

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

![image](/3.%20Server%20Management/media/0.png)

![image](/3.%20Server%20Management/media/1.png)

![image](/3.%20Server%20Management/media/2.png)

file config `/etc/ssh/sshd_config` untuk disable password login

"PasswordAuthentication no"

![image](/3.%20Server%20Management/media/3.png)

file config `one-ssh.yml` saya buat agar semua server hanya menggunakan satu ssh dengan ansible-playbook

```
---
- become: true
  gather_facts: false
  hosts: all
  tasks:
    - name: copy ssh key
      copy:
        src: ~/.ssh/id_rsa
        dest: /home/nafis/.ssh
    - name: copy ssh pub key
      copy:
        src: ~/.ssh/id_rsa.pub
        dest: /home/nafis/.ssh
```

![image](/3.%20Server%20Management/media/5.png)

disini saya membuat sistem one gateway dengan membuat file config ssh `~/.ssh/config`, akses semua server melalui perantara server gateway

```
host gateway
    HostName 103.37.125.189
    User nafis

host appserver
    Hostname 10.36.116.115
    User nafis
    ProxyCommand ssh gateway -W %h:%p
    IdentityFile /home/server/.ssh/privatekey

host monitor
    Hostname 10.36.116.115.135
    User nafis
    ProxyCommand ssh gateway -W %h:%p
    IdentityFile /home/server/.ssh/privatekey

host cicd
    Hostname 10.36.116.115.41
    User nafis
    ProxyCommand ssh gateway -W %h:%p
    IdentityFile /home/server/.ssh/privatekey
```

![image](/3.%20Server%20Management/media/7.png)

## Firewall


file config `firewall.yml` untuk instalasi ufw pada server gateway

```
---
- become: true
  gather_facts: false
  hosts: gateway
  tasks:
    - name: install firewall
      apt:
        name: ufw
        update_cache: yes
        state: latest
    - name: enable ufw
      community.general.ufw:
        state: enabled
        policy: allow
    - name: rules
      community.general.ufw:
        rule: allow
        proto: tcp
        port: "{{ item }}"
      with_items:
      - 22
      - 80
      - 443
      - 3000
      - 5000
      - 3306
      - 9090
      - 9100
      - 8080
    - name: enable ufw
      community.general.ufw:
        state: reloaded
        policy: allow
```

![image](/3.%20Server%20Management/media/8.png)

![image](/3.%20Server%20Management/media/9.png)