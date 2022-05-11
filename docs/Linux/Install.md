---
sidebar_position: 1
---

# 🧨 Installation

> L'installation est valable pour un serveur chez OVH (kimsufi) ou un raspberry

## Structure

| Dossier            | Description                                                                                                                     |
| ------------------ | :------------------------------------------------------------------------------------------------------------------------------ |
| /boot              | Contient le fichier qui est utilisé par le boot loader (grub.cfg)                                                               |
| /root              | Le repertoire de l'utilisateur root différent de /                                                                              |
| /dev               | les périphériques                                                                                                               |
| /etc               | configuration                                                                                                                   |
| /bin -> /usr/bin   | Les commandes quotidienne de l'utilisateur                                                                                      |
| /sbin -> /usr/sbin | commandes système/sytèle de fichier                                                                                             |
| /opt               | application complémentaire optionnelle ( pas partie de l'OS )                                                                   |
| /proc              | processus en cour  ( n'existe qu'en mémoire )                                                                                   |
| /lib -> /usr/lib   | Fichiers de librairie du langage C dont ont besoin les commandes et les applications `strace -e open pwd`                       |
| /tmp               | repertoire temporaires                                                                                                          |
| /home              | repertoire utilisateur                                                                                                          |
| /var               | log system                                                                                                                      |
| /run               | Démons système qui démarrent très tôt (systemd, udev ) pour stocker les fichiers d'exécution temporaires comme les fichiers PID |
| /mnt               | pour monter un système de fichiers externe                                                                                      |
| /media             | CDROM                                                                                                                           |

## Installation (Raspberry)

Télécharger l'image [ici](https://www.raspberrypi.org/downloads/raspbian/) la version light est suffisante

### Formatter la SD

Formater depuis un windows avec l'invite de commande

```BATCH
diskpart
list disk
select disk
clean
create partition primary
active
format fs=exfat quick
assign
exit
exit
```

### Preparer la SD

Avec rufus ou [etcher](https://etcher.io) 

### Configurer le wifi et ssh en avance

créer un fichier à la racine `ssh` et en même temps `wpa_supplicant.conf` avec ces infos

```SHELL
country=fr
update_config=1
ctrl_interface=/var/run/wpa_supplicant

network={
 scan_ssid=1
 ssid="nameofnetworknotnetwork5GHZ"
 psk="motdepasse"
}
```

Ecrire ces informations en **EOL Unix** avec Notepad++ dans le menu *Edit/EOL Conversion*, les réseau 5GHZ ne fonctionneras pas.
Aprés chaque redemarrage la configuration seras supprimé

## Fixer une adresse IP

```SHELL
sudo nano /etc/dhcpcd.conf
```

```BASH
interface wlan0
static ip_address=192.168.1.2/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```

## Mettre le clavier en français

```SHELL
sudo raspi-config
```

changer les options de locatisations (FR UTF8)

## Mettre à jour le système

```batch
sudo su
sudo apt update -y && apt upgrade -y && sudo apt full-upgrade -y 
sudo apt install ntpdate
sudo apt install ntp
sudo apt install bash-completion
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
cat /etc/apt/apt.conf.d/20auto-upgrades
```