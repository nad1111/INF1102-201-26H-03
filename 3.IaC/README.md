# 🏗️ Infrastructure as Code (IaC)

| #️⃣ | Groupes |
|-|-|
| 🥇 | [:tada: Participation](.scripts/Participation-group1.md) |
| 🥈 | [:tada: Participation](.scripts/Participation-group2.md) |
| 🥉 | [:tada: Participation](.scripts/Participation-group3.md) |


## 1. Introduction

Traditionnellement, l’administration des systèmes se faisait **manuellement** :

* installation à la main
* configurations faites “à la souris”
* documentation incomplète
* environnements difficiles à reproduire

👉 **Problème majeur** :

> *“Ça marche sur ce serveur, mais pas sur l’autre.”*

### Solution moderne : Infrastructure as Code (IaC)

L’**Infrastructure as Code** consiste à **décrire, déployer et gérer une infrastructure informatique à l’aide de code**, plutôt que par des actions manuelles.

---

## 2. Définition

> **Infrastructure as Code (IaC)** est une approche de programmation système qui permet de gérer les ressources informatiques (serveurs, réseaux, services, utilisateurs, stockage) à l’aide de fichiers de configuration versionnés et exécutables automatiquement.

---

## 3. Où se situe l’IaC en programmation système ?

### Position dans la pile système

```
Applications
──────────────
Services (Web, DB, DNS, AD, Containers)
──────────────
Infrastructure as Code (IaC)
──────────────
Shell / API OS / Hyperviseur / Cloud
──────────────
Noyau (Linux / Windows)
──────────────
Matériel
```

👉 L’IaC :

* **ne programme pas le noyau**
* **programme le système et son infrastructure**
* agit via des **API, des services système et des hyperviseurs**

---

## 4. Pourquoi utiliser l’IaC ?

### Problèmes sans IaC

* erreurs humaines
* incohérences entre serveurs
* déploiements lents
* documentation non fiable

### Avantages de l’IaC

| Avantage         | Description                            |
| ---------------- | -------------------------------------- |
| Reproductibilité | Même infrastructure, partout           |
| Automatisation   | Déploiement sans intervention manuelle |
| Versionnement    | Git = historique, rollback             |
| Fiabilité        | Moins d’erreurs humaines               |
| Rapidité         | Déploiement en minutes                 |
| Auditabilité     | Tout est traçable                      |

---

## 5. IaC vs scripts système classiques

### Scripts système (bash / PowerShell)

```bash
apt update
apt install nginx
systemctl start nginx
```

* impératif
* dépend de l’ordre
* difficile à maintenir

### IaC (déclaratif)

```hcl
resource "nginx_server" {
  version = "1.24"
  port    = 80
}
```

* déclaratif
* décrit **l’état final**
* l’outil décide *comment* y arriver

---

## 6. Approches de l’IaC

### 6.1 IaC déclaratif

> *Voici l’état voulu*

* Terraform / OpenTofu
* CloudFormation
* Kubernetes YAML

✔ recommandé
✔ reproductible
✔ idempotent

---

### 6.2 IaC impératif

> *Fais ceci, puis cela*

* Scripts shell
* Ansible (mixte)

✔ flexible
✖ plus complexe à maintenir

---

## 7. Que peut-on gérer avec l’IaC ?

* Machines virtuelles
* Réseaux (VLAN, bridges, firewall)
* Stockage
* Utilisateurs et permissions
* Services (web, DB, DNS)
* Containers
* Clusters
* Infrastructure cloud
* Infrastructure locale (Proxmox, VMware)

👉 **L’infrastructure devient un programme.**

---

## 8. Outils IaC courants

### Outils d’orchestration

* **Terraform / OpenTofu**
* CloudFormation
* Pulumi

### Outils de configuration

* Ansible
* Puppet
* Chef

### Plateformes ciblées

* Proxmox
* AWS / Azure / GCP
* Kubernetes

---

## 9. Exemple simple (conceptuel)

### Objectif

> Créer automatiquement une VM Linux avec un serveur web.

### Étapes IaC

1. Définir la VM
2. Allouer CPU / RAM
3. Créer le réseau
4. Installer le service web
5. Exposer le port

👉 **Une seule commande** :

```bash
tofu apply
```

---

## 10. IaC et bonnes pratiques

* Infrastructure versionnée (Git)
* Pas de modification manuelle en production
* Séparation dev / test / prod
* Variables et secrets sécurisés
* Documentation = code

---

## 11. IaC et DevOps

L’IaC est un **pilier du DevOps** :

* CI/CD
* déploiement continu
* scalabilité
* résilience
* SRE

👉 Sans IaC, **le DevOps n’est pas viable à grande échelle**.

---

## 12. Place de l’IaC dans le cours

### Prérequis recommandés

* Linux / Windows
* Bash / PowerShell
* Réseaux
* Virtualisation

### Position idéale dans la session

1. Scripts système
2. Services et démons
3. Virtualisation
4. **Infrastructure as Code**
5. Orchestration avancée

---

## 13. Objectifs pédagogiques

À la fin de cette leçon, l’étudiant(e) sera capable de :

* expliquer le concept d’IaC
* distinguer script système et IaC
* décrire une infrastructure de manière déclarative
* utiliser un outil IaC de base
* automatiser un déploiement système

---

## 14. Phrase de conclusion

> **L’Infrastructure as Code transforme l’administration système en une discipline de programmation structurée, reproductible et industrielle.**

# :b: Expérimentation

### 🎛️ Créer un fichier dans ce répertoire `(3.IaC)`:

:checkered_flag: Finalement,

- [ ] Créer un répertoire avec :id: (votre identifiant boreal)
   - [ ] `mkdir ` :id:
- [ ] dans votre répertoire ajouter le fichier `README.md`
  - [ ] `nano `README.md
- [ ] envoyer vers le serveur `github.com`
  - [ ] `cd ..`
  - [ ] `git add `:id: 
  - [ ] `git commit -m "mon fichier ..."`
  - [ ] `git push`

- [ ] Se diriger vers le répertoire avec :id: (votre identifiant boreal)
   - [ ] `cd ` :id:

- [ ] Continuer les 🔄 Exercices 

### 🔄 Exercices


Alright, let’s do this cleanly and *practically* 🙂
OpenTofu works **almost exactly like Terraform**, and Proxmox VE 7 is well-supported via the **Telmate Proxmox provider**.

Below is a **minimal, working, PVE-7-friendly workflow**.

---

## 0️⃣ Prereqs on Proxmox (PVE 7)

### ✔ Enable API access

You need either:

* a **user + password**, or
* **API token** (recommended)

**Recommended (API token):**

```bash
pveum user add tofu@pve
pveum aclmod / -user tofu@pve -role Administrator
pveum user token add tofu@pve opentofu --privsep 0
```

Save:

* **Token ID**: `tofu@pve!opentofu`
* **Token Secret**: (shown once)

---

## 1️⃣ Install OpenTofu

On your workstation:

- [ ] Windows 🪟

```bash
choco install opentofu
```

- [ ] Mac 🍎

```bash
brew install opentofu
```

Verify:

```bash
tofu version
```
```lua
OpenTofu v1.11.3
on darwin_arm64
+ provider registry.opentofu.org/telmate/proxmox v2.9.14
```

---

## 2️⃣ Create project structure

`mkdir` :id:

`cd` :id:

```bash
touch provider.tf main.tf variables.tf terraform.tfvars
```

---

## 3️⃣ Provider configuration (`provider.tf`)

```hcl
terraform {
  required_providers {
    proxmox = {
      source  = "telmate/proxmox"
      version = ">= 2.9.0"
    }
  }
}

provider "proxmox" {
  pm_api_url      = var.pm_url
  pm_api_token_id = var.pm_token_id
  pm_api_token_secret = var.pm_token_secret
  pm_tls_insecure = true
}
```

---

## 4️⃣ VM resource (Cloud-Init VM) (`main.tf`)

Example **Ubuntu VM** cloned from a template:

```hcl
resource "proxmox_vm_qemu" "vm1" {
  name        = "tofu-vm01"
  target_node = "pve"
  clone       = "ubuntu-2204-template"

  cores   = 2
  sockets = 1
  memory  = 2048

  scsihw = "virtio-scsi-pci"

  disk {
    size    = "20G"
    type    = "scsi"
    storage = "local-lvm"
  }

  network {
    model  = "virtio"
    bridge = "vmbr0"
  }

  os_type = "cloud-init"

  ipconfig0 = "ip=dhcp"

  ciuser  = "ubuntu"
  sshkeys = file("~/.ssh/id_rsa.pub")
}
```

---

## 5️⃣ Variables (`variables.tf`)

```hcl
variable "pm_token_id" {
  type = string
}

variable "pm_token_secret" {
  type      = string
  sensitive = true
}
```

---

## 6️⃣ Secrets (`terraform.tfvars`)

```hcl
pm_token_id     = "tofu@pve!opentofu"
pm_token_secret = "PASTE_SECRET_HERE"
```

⚠️ **Do not commit this file**

---

## 7️⃣ Initialize & apply

```bash
tofu init
tofu plan
tofu apply
```

Type `yes`.

🎉 VM will appear in Proxmox.

---

## 8️⃣ Common gotchas (PVE 7 specific)

### ❌ “Clone source does not exist”

➡ You **must create a template first**

Example:

```bash
qm create 9000 --name ubuntu-2204-template
qm set 9000 --scsi0 local-lvm:0,import-from=/var/lib/vz/template/iso/jammy-server-cloudimg-amd64.img
qm template 9000
```

---

### ❌ Cloud-init not working

Make sure:

```bash
qm set VMID --ide2 local-lvm:cloudinit
```

---

## 9️⃣ Advanced (when you’re ready)

You can:

* auto-generate **MAC addresses**
* attach **PCI passthrough**
* define **multiple disks**
* integrate with **Ansible**
* manage **LXC containers**
* create **entire Proxmox clusters declaratively**

---

## TL;DR

* OpenTofu = Terraform replacement ✅
* Telmate Proxmox provider works perfectly on **PVE 7**
* Best approach: **cloud-init template + clone**
* API token is safer than passwords

---

```bash
pveum user add tofu@pve
pveum aclmod / -user tofu@pve -role Administrator
pveum user token add tofu@pve opentofu --privsep 0
┌──────────────┬──────────────────────────────────────┐
│ key          │ value                                │
╞══════════════╪══════════════════════════════════════╡
│ full-tokenid │ tofu@pve!opentofu                    │
├──────────────┼──────────────────────────────────────┤
│ info         │ {"privsep":"0"}                      │
├──────────────┼──────────────────────────────────────┤
│ value        │ 4fa24fc3-bd8c-4916-ba6e-09xxxxxxxx00 │
└──────────────┴──────────────────────────────────────┘
```


```terraform
tofu apply

OpenTofu used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

OpenTofu will perform the following actions:

  # proxmox_vm_qemu.vm1 will be created
  + resource "proxmox_vm_qemu" "vm1" {
      + additional_wait           = 5
      + automatic_reboot          = true
      + balloon                   = 0
      + bios                      = "seabios"
      + boot                      = (known after apply)
      + bootdisk                  = (known after apply)
      + ciuser                    = "ubuntu"
      + clone                     = "ubuntu-jammy-template"
      + clone_wait                = 10
      + cores                     = 2
      + cpu                       = "host"
      + default_ipv4_address      = (known after apply)
      + define_connection_info    = true
      + force_create              = false
      + full_clone                = true
      + guest_agent_ready_timeout = 100
      + hotplug                   = "network,disk,usb"
      + id                        = (known after apply)
      + ipconfig0                 = "ip=dhcp"
      + kvm                       = true
      + memory                    = 2048
      + name                      = "tofu-vm01"
      + nameserver                = (known after apply)
      + onboot                    = false
      + oncreate                  = true
      + os_type                   = "cloud-init"
      + preprovision              = true
      + reboot_required           = (known after apply)
      + scsihw                    = "virtio-scsi-pci"
      + searchdomain              = (known after apply)
      + sockets                   = 1
      + ssh_host                  = (known after apply)
      + ssh_port                  = (known after apply)
      + sshkeys                   = <<-EOT
            ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD2pLhMqFGKffSdYvNCMAyM7598oBY+m/3q5AMXmb7IE6vq42+yGzqEUzZu9WrFckFD4Hq52rIU5DeOvi83DCF3uroXjNTEtCKdi+tY7cV18bHmsDsBHMqTnpuvroofgFWA0Pi++b2kGW2I5eyy1Qjv5rOp7y11Xe6XeZFEz7qQO1/xNiBMJEruG9Xldgooe4hkaOF39qnbqD4ui3LxYaTUTEulstw4wN70dSB8Zu9YQP7A7KU2zIEwJ1aw8whfO1CAM/AVvoDyqMtV8VXoaZSHOBgluMtinQfyyt473S2ZZeJlnmhK0F1gdOhO4SVZNRMj96m30ryYkYBFWvvLRP5N b300098957@ramena
        EOT
      + tablet                    = true
      + target_node               = "labinfo"
      + unused_disk               = (known after apply)
      + vcpus                     = 0
      + vlan                      = -1
      + vmid                      = (known after apply)

      + disk {
          + backup             = true
          + cache              = "none"
          + file               = (known after apply)
          + format             = (known after apply)
          + iops               = 0
          + iops_max           = 0
          + iops_max_length    = 0
          + iops_rd            = 0
          + iops_rd_max        = 0
          + iops_rd_max_length = 0
          + iops_wr            = 0
          + iops_wr_max        = 0
          + iops_wr_max_length = 0
          + iothread           = 0
          + mbps               = 0
          + mbps_rd            = 0
          + mbps_rd_max        = 0
          + mbps_wr            = 0
          + mbps_wr_max        = 0
          + media              = (known after apply)
          + replicate          = 0
          + size               = "20G"
          + slot               = (known after apply)
          + ssd                = 0
          + storage            = "local-lvm"
          + storage_type       = (known after apply)
          + type               = "scsi"
          + volume             = (known after apply)
        }

      + network {
          + bridge    = "vmbr0"
          + firewall  = false
          + link_down = false
          + macaddr   = (known after apply)
          + model     = "virtio"
          + queues    = (known after apply)
          + rate      = (known after apply)
          + tag       = -1
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  OpenTofu will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

proxmox_vm_qemu.vm1: Creating...
proxmox_vm_qemu.vm1: Still creating... [10s elapsed]
proxmox_vm_qemu.vm1: Still creating... [20s elapsed]
proxmox_vm_qemu.vm1: Still creating... [30s elapsed]
proxmox_vm_qemu.vm1: Still creating... [40s elapsed]
proxmox_vm_qemu.vm1: Still creating... [50s elapsed]
proxmox_vm_qemu.vm1: Still creating... [1m0s elapsed]
proxmox_vm_qemu.vm1: Still creating... [1m10s elapsed]
proxmox_vm_qemu.vm1: Still creating... [1m20s elapsed]
proxmox_vm_qemu.vm1: Still creating... [1m30s elapsed]
proxmox_vm_qemu.vm1: Still creating... [1m40s elapsed]
proxmox_vm_qemu.vm1: Still creating... [1m50s elapsed]
proxmox_vm_qemu.vm1: Still creating... [2m0s elapsed]
proxmox_vm_qemu.vm1: Still creating... [2m10s elapsed]
proxmox_vm_qemu.vm1: Still creating... [2m20s elapsed]
proxmox_vm_qemu.vm1: Still creating... [2m30s elapsed]
proxmox_vm_qemu.vm1: Still creating... [2m40s elapsed]
proxmox_vm_qemu.vm1: Still creating... [2m50s elapsed]
proxmox_vm_qemu.vm1: Still creating... [3m0s elapsed]
proxmox_vm_qemu.vm1: Still creating... [3m10s elapsed]
proxmox_vm_qemu.vm1: Still creating... [3m20s elapsed]
proxmox_vm_qemu.vm1: Still creating... [3m30s elapsed]
proxmox_vm_qemu.vm1: Still creating... [3m40s elapsed]
proxmox_vm_qemu.vm1: Still creating... [3m50s elapsed]
proxmox_vm_qemu.vm1: Still creating... [4m0s elapsed]
proxmox_vm_qemu.vm1: Still creating... [4m10s elapsed]
proxmox_vm_qemu.vm1: Still creating... [4m20s elapsed]
proxmox_vm_qemu.vm1: Still creating... [4m30s elapsed]
proxmox_vm_qemu.vm1: Still creating... [4m41s elapsed]
proxmox_vm_qemu.vm1: Still creating... [4m51s elapsed]
proxmox_vm_qemu.vm1: Still creating... [5m1s elapsed]
proxmox_vm_qemu.vm1: Still creating... [5m11s elapsed]
proxmox_vm_qemu.vm1: Still creating... [5m21s elapsed]
proxmox_vm_qemu.vm1: Still creating... [5m31s elapsed]
proxmox_vm_qemu.vm1: Still creating... [5m41s elapsed]
proxmox_vm_qemu.vm1: Still creating... [5m51s elapsed]
proxmox_vm_qemu.vm1: Still creating... [6m1s elapsed]
proxmox_vm_qemu.vm1: Still creating... [6m11s elapsed]
proxmox_vm_qemu.vm1: Still creating... [6m21s elapsed]
proxmox_vm_qemu.vm1: Still creating... [6m31s elapsed]
proxmox_vm_qemu.vm1: Still creating... [6m41s elapsed]
proxmox_vm_qemu.vm1: Still creating... [6m51s elapsed]
proxmox_vm_qemu.vm1: Still creating... [7m1s elapsed]
proxmox_vm_qemu.vm1: Still creating... [7m11s elapsed]
proxmox_vm_qemu.vm1: Still creating... [7m21s elapsed]
proxmox_vm_qemu.vm1: Still creating... [7m31s elapsed]
proxmox_vm_qemu.vm1: Still creating... [7m41s elapsed]
proxmox_vm_qemu.vm1: Still creating... [7m51s elapsed]
proxmox_vm_qemu.vm1: Still creating... [8m1s elapsed]
proxmox_vm_qemu.vm1: Still creating... [8m11s elapsed]
proxmox_vm_qemu.vm1: Still creating... [8m21s elapsed]
proxmox_vm_qemu.vm1: Still creating... [8m31s elapsed]
proxmox_vm_qemu.vm1: Still creating... [8m41s elapsed]
proxmox_vm_qemu.vm1: Still creating... [8m51s elapsed]
proxmox_vm_qemu.vm1: Still creating... [9m1s elapsed]
proxmox_vm_qemu.vm1: Still creating... [9m11s elapsed]
proxmox_vm_qemu.vm1: Still creating... [9m21s elapsed]
proxmox_vm_qemu.vm1: Still creating... [9m31s elapsed]
proxmox_vm_qemu.vm1: Still creating... [9m41s elapsed]
proxmox_vm_qemu.vm1: Still creating... [9m51s elapsed]
proxmox_vm_qemu.vm1: Still creating... [10m1s elapsed]
proxmox_vm_qemu.vm1: Still creating... [10m11s elapsed]
proxmox_vm_qemu.vm1: Still creating... [10m21s elapsed]
proxmox_vm_qemu.vm1: Still creating... [10m31s elapsed]
proxmox_vm_qemu.vm1: Creation complete after 10m36s [id=labinfo/qemu/100]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```


```lua
ssh -i ~/.ssh/b300098957@ramena \
  -o StrictHostKeyChecking=no \
  -o UserKnownHostsFile=/tmp/ssh_known_hosts_empty \
  ubuntu@10.7.237.193
```
