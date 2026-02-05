# 🎌 Proxmox

### **Proxmox VE (Virtual Environment) 🌐💻**

**Proxmox** est une **plateforme open-source 🆓** qui permet de **virtualiser des serveurs 🖥️**.
Avec Proxmox, tu peux créer, gérer et surveiller à la fois :

* des **machines virtuelles (VMs) 🖥️➡️🖥️**
* des **containers légers 🐳**

Il est basé sur **Linux Debian 🐧** et combine plusieurs technologies de virtualisation dans **un seul environnement centralisé 🗂️**.

---

### **1. Définition**

**Proxmox VE (Virtual Environment)** est une **plateforme open-source de virtualisation** qui permet de créer, gérer et superviser des **machines virtuelles (VMs) et des containers** sur un serveur physique.
Il est basé sur **Debian Linux** et combine plusieurs technologies de virtualisation dans un seul environnement.

---

### **2. Ses composants principaux**

1. **Hyperviseur** :

   * Proxmox utilise **KVM** pour la virtualisation complète des machines (VMs) et **LXC** pour les containers légers.
   * KVM = Hyperviseur type 1 intégré au noyau Linux.
   * LXC = Conteneurs Linux isolés, plus légers qu’une VM complète.

2. **Interface Web (GUI)** :

   * Proxmox fournit une **interface web complète** pour créer, gérer et surveiller vos VMs et containers, sans passer par la ligne de commande.

3. **Services intégrés** :

   * Gestion des snapshots, backups et restauration.
   * Réplication et haute disponibilité (HA).
   * Gestion de stockage local et distant (ZFS, Ceph, NFS, etc.).

4. **API et outils CLI** :

   * Vous pouvez automatiser les tâches avec l’**API REST** ou les commandes en ligne (`pve*`).

---

### **3. Avantages**

* **Open-source** et gratuit (avec option d’abonnement pour support officiel).
* **Gestion centralisée** de plusieurs serveurs Proxmox (cluster).
* Supporte **KVM + LXC** dans un seul outil.
* **Snapshots, backups et migration à chaud** des VMs.

---

### **4. Comparaison simple**

| Proxmox       | VMware ESXi       | VirtualBox    |
| ------------- | ----------------- | ------------- |
| Open-source   | Propriétaire      | Open-source   |
| Serveur Linux | Hyperviseur dédié | Desktop/local |
| KVM + LXC     | VM uniquement     | VM uniquement |
| Cluster et HA | Oui               | Non           |


## 1️⃣ Les services Proxmox essentiels (qui fait quoi)

### 🧠 Cœur Proxmox

| Service        | Rôle                                           |
| -------------- | ---------------------------------------------- |
| `pve-cluster`  | Gère la config partagée (`/etc/pve`)           |
| `pvedaemon`    | API backend (création VM, permissions, tâches) |
| `pveproxy`     | Interface Web (HTTPS :8006)                    |
| `pvestatd`     | Stats CPU/RAM/disques                          |
| `pve-firewall` | Pare-feu Proxmox                               |

👉 **Sans `pve-cluster`, Proxmox est cassé** (même en mono-nœud).

---

### 🖥️ Virtualisation

| Service       | Rôle                       |
| ------------- | -------------------------- |
| `qemu-server` | Gestion des VM KVM         |
| `lxc`         | Gestion des conteneurs LXC |
| `ksmtuned`    | Optimisation mémoire       |

---

### 🌐 Cluster (si applicable)

| Service    | Rôle                              |
| ---------- | --------------------------------- |
| `corosync` | Communication entre nœuds         |
| `pmxcfs`   | FS cluster (monté sur `/etc/pve`) |

---

## 2️⃣ Proxmox est-il un *service systemd* ?

👉 **Non**, Proxmox **n’est pas un service unique**, mais une **suite de services systemd**.

Il n’existe PAS :

```bash
systemctl restart proxmox ❌
```

Mais OUI :

```bash
systemctl restart pveproxy
```

---

## 3️⃣ Redémarrage propre (sans arrêter les VM)

### 🔄 Redémarrer uniquement l’interface Web

```bash
systemctl restart pveproxy
```

✔️ Aucun impact sur les VM

---

### 🔄 Redémarrer les services Proxmox (safe)

```bash
systemctl restart pvedaemon
systemctl restart pvestatd
systemctl restart pveproxy
```

✔️ Les VM continuent de tourner

---

### ⚠️ Redémarrage plus lourd (attention)

```bash
systemctl restart pve-cluster
```

⚠️ Peut bloquer l’UI temporairement
⚠️ À éviter en prod si cluster actif

---

## 4️⃣ Vérifier l’état global

```bash
systemctl list-units --type=service | grep pve
```

Ou plus ciblé :

```bash
systemctl status pveproxy pvedaemon pve-cluster
```

---

## 5️⃣ Phrase parfaite pour un examen 💯

> *Proxmox VE est une plateforme de virtualisation basée sur Debian qui repose sur plusieurs services systemd, notamment pve-cluster, pvedaemon et pveproxy, permettant la gestion des machines virtuelles, des conteneurs et du clustering.*

---

Si tu veux, je peux :

* 🧪 te faire **un schéma mental / résumé 1 page**
* 🔧 t’aider à **dépanner un service Proxmox qui refuse de démarrer**
* 🎓 te préparer **des questions-réponses type examen**

Dis-moi 👌
