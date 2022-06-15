---
sidebar_position: 2
---

# 🪛 Configuration

> Mot de passe _root_ d'un raspberry : **raspberry**

## Personnalisation

Changer le mot de passe

```Shell
passwd
```

Mettre a jour le Noyau

```Shell
yum update kernel -y
```

Pour activer l'auto-complétion decommenter le fichier comme ci-dessous

```Shell
nano /etc/bash.bashrc
```

```Bash
# enable bash completion in interactive shells
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
```

### Hostname

```SHELL
# afficher le hostname actuel
cat /etc/hostname

# changer le hostname
hostnamectl set-hostname <newname>
```

```Shell
sudo nano /etc/hosts
```

Modifier la première ligne en remplaçant localhost par le hostname

```Bash
127.0.0.1       <hostname>
```

## Date

> Moi j'ai que mes serveurs on la même timezone

### Timzone

```Shell
# Je cherche ma timezone
timedatectl list-timezones | grep Indian
# Je la modifie
sudo timedatectl set-timezone Indian/Reunion
systemctl status systemd-timesyncd
```

### NTP

> Pas fan du service ntp avec les différentes faille de sécurité, libre à vous d'utiliser ce service

```Shell
# verifier le statut
systemctl status systemd-timesyncd
# verifier que tout es commenté
sudo nano /etc/systemd/timesyncd.conf
# desactiver le service
sudo timedatectl set-ntp off
```

## Securité

> Banir les adresse IP qui essay de se connecter chez vous

```Shell
# Installer
sudo apt install fail2ban
# cheque de statut
sudo systemctl status fail2ban

# faire une copie des cconfig
sudo cp /etc/fail2ban/fail2ban.{conf,local}
sudo cp /etc/fail2ban/jail.{conf,local}

#Modification de ces fichiers
sudo nano /etc/fail2ban/fail2ban.local
sudo nano /etc/fail2ban/jail.local
#auto to systemd

sudo systemctl status fail2ban
sudo fail2ban-client status

# Check les intrusion
sudo fail2ban-client status sshd
tail -f /var/log/fail2ban.log
```

## Supression d'apache

```shell
# supression de apache
sudo /etc/init.d/apache2 stop
sudo systemctl disable apache2

# libre office
sudo apt-get remove --purge libreoffice*
```

## Connexion

### Pré-requis serveur

> les pré-requis ne sont pas nécessaire mais au cas ou vous êtes sur un vieux système

```shell
# normalement y'a pas besoin de télécharger
sudo apt-get install openssh-server
```

Vérifier que __PasswordAuthentication__ est à __yes__ car une fois la configuration terminé il faudra le mettre à __no__ pour eviter connexion avec le mot de passe, et vérifier que __AuthorizedKeysFile /.ssh/authorized_keys__ est décommentez

On redemarre le service à chaque modification

```shell
sudo nano /etc/ssh/sshd_config
```

### Client

Creations des clés

```shell
ssh-keygen -t rsa -b 4096 -C "email_address"
```

```shell
sudo chmod 600 ~/.ssh/id_rsa
sudo chmod 600 ~/.ssh/id_rsa.pub
```

envoyer la clée sur le serveur

```shell
cat ~/.ssh/id_rsa.pub | ssh <USERNAME>@<IP> 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'

# ou

ssh-copy-id utilisateur@ipduserveur
```

### Plusieur clé privé

Si vous avez plusieur clé, sur certaine ligne de commande il faudra spécifier la clé à utiliser

```shell
ssh -i ~/.ssh/<KEY> <USERNAME>@<IP>
```

### Windows

> WSL 1 récupérer les clée depuis windows

Avec __Pageant__ et __wsl-ssh-pageant-amd64-gui__ votre clée privée et aussi importé dans votre WSL

`win + r` tapez `shell:startup` creer un racourci __wsl-ssh-pageant-amd64-gui__ vers

__c:\user\Documents\Putty\wsl-ssh-pageant-amd64-gui.exe --force --systray --winssh ssh-pageant --wsl c:\user\Documents\Putty\ssh-agent.sock__

Modifier vos variables d'environnement

GIT_SSH : C:\Program Files\PuTTY\plink.exe
SSH_AUTH_SOCK : \\.\pipe\ssh-pageant
