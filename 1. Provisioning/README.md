
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

resource "idcloudhost_vm" "nfs-appserver" {
    name = "nfs-appserver"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 2
    memory = 2048
    username = "nfs"
    initial_password = "Katasand1" 
    billing_account_id = 1200157626 
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
    backup = false
}

resource "idcloudhost_vm" "nfs-gateway" {
    name = "nfs-gateway"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 1
    memory = 1024
    username = "nfs"
    initial_password = "Katasand1" # Combination of Uppercase, Lowercase & Numbers
    billing_account_id = 1200157626 # Billing ID from idcloudhost.com
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
    backup = false
}

resource "idcloudhost_vm" "nfs-monitoring" {
    name = "nfs-monitoring"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 2
    memory = 2048
    username = "nfs"
    initial_password = "Katasand1" # Combination of Uppercase, Lowercase & Numbers
    billing_account_id = 1200157626 # Billing ID from idcloudhost.com
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
    backup = false
}

resource "idcloudhost_vm" "nfs-CICD" {
    name = "nfs-CICD"
    os_name = "ubuntu"
    os_version= "20.04"
    disks = 20
    vcpu = 2
    memory = 2048
    username = "nfs"
    initial_password = "Katasand1" # Combination of Uppercase, Lowercase & Numbers
    billing_account_id = 1200157626 # Billing ID from idcloudhost.com
    public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbFhouYP76rswiBFhBlQVh7PaVbe7HHKvuFE/s/WCRJCfahLNU6XFwy2kZ3o4P7JYKwYjMaLGVH9NjUJln8dTNHgZRWFScK+O4sHIdjtdGlK6rAf6ok5O+AOsTe1oVVx1HsWFJLR1RpDwh64nfSeKdePP1gvPM2KhPb9mQ8RwmDQOO39e1ne6uQViwKFkKfuqgaVkXFABF7K5mA4jyvlGsJgzsefH/srjPILzsv0Teh2r+xTjUWPrPpPM9NVMwMmzAza7FjZOAyxHcM+i2Mjtjvvv7MUcZ0fnit3pCLiJVXQ/YepI/GaW3aPJPYxh/PlTCr2UQ1b7VUybDDcg7VK5LhIvAEkn7ib8MDn7iwAddxMD8/dFKCmToDtRduIZfe8mJZOTLCZiVi/AehXqg/xCI71wNQpasgh8mucnu83V23gAkQxAtSs6V2+McwVVuIdEO9oLAcXp3Z7Gmh5KXhrn4vuAZkssYSENvTRMZoC0Ij7G2gNMCqQRFpylTPriAIps= ubuntu@primary" 
    backup = false
}

resource "idcloudhost_floating_ip" "ip-appserver" {
    name = "nfs-appserver"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nfs-appserver.id
}

resource "idcloudhost_floating_ip" "ip-gateway" {
    name = "nfs-gateway"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nfs-gateway.id
}

resource "idcloudhost_floating_ip" "ip-monitoring" {
    name = "nfs-monitoring"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nfs-monitoring.id
}

resource "idcloudhost_floating_ip" "ip-CICD" {
    name = "nfs-CICD"
    billing_account_id = 1200157626 
    assigned_to = idcloudhost_vm.nfs-CICD.id
}
```