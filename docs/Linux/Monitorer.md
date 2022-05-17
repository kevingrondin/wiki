---
sidebar_position: 2
---

# 🔬 Monitorer

> Plusieurs outils et façon de faire

## Netdata

> Une communauté grandissante

Installation

```Shell
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

Interface web htt://127.0.0.1:19999

```Shell
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

# Espace disque

## ncdu

ce placer à la racine, l'argument -x permet de ignorer les média

```Shell
cd /
sudo ncdu -x
```

# Fichier temporaire

## tmpreaper

Installer tmpreaper avec apt

lors de l'installation avec apt, il faut commenter la ligne showing dans le fichier etc/tmpreaper

# Journal et Log

## log monitor

```SHELL
cd var/log

ls -ltr
ll | more
```

Voir les log de demarrage toujours dans `var/log`

```SHELL
more boot.log
```

voir quand est-ce que boot.log a été modifié

```SHELL
ls -l boot.log
```

voir les log du matériel

```SHELL
dmesg
```

## lnav

Analyser les log avec lnav installation simple avec apt

```Shell
lnav /var/log/messages*

journalctl | lnav

journalctl -f | lnav

journalctl -o short-iso | lnav

journalctl -o json | lnav

journalctl -a -o json | lnav

journalctl -o json --output-fields=MESSAGE,PRIORITY,_PID,SYSLOG_IDENTIFIER,_SYSTEMD_UNIT | lnav
```

## logrotate

> Si on travail avec des outils qui générent des fichiers _log_

**logrotate** est déja présent sur les distribution linux, il permet d'effecctuer des actions de rotations et de compression sur les logs

Assurons-nous que cette ligne n'est pas commentez

```Shell
cat /etc/logrotate.conf | grep "include"
```

Voici un exemple

```Bash
/var/log/mylogs/auth.log {
     su <user> <group>
     monthly
     rotate 3
     compress
     missingok
     create 644 <user> <group>
}

/var/log/mylogs/errors.log {
     su <user> <group>
     monthly
     rotate 3
     compress
     missingok
     create 644 <user> <group>
}
```

1. **monthly** : la rotation se fait mensuellement
1. **rotate 3** : le nombre de fichiers qu’on souhaite conserver
1. **compress** : les anciens fichiers sont compressés
1. **missingok** : ne considère pas l’absence du fichier comme une erreur
1. **create 644 root root** : créer le fichier de log immédiatement après la rotation avec les droits adéquats

Petite verification

```Shell
logrotate --force /etc/logrotate.d/mylogs
```

# Supprimer ces traces

```BATCH
history
```

supprimer une ligne génante

```BATCH
history -d numeroligne
```

Tout nettoyer

```BATCH
history -c
```

Si je veux l'historique sans les ligne de commande ls -l

```BATCH
export HISTIGNORE='ls -l:pwd:history'
```

tout l'historique sont sauvegardé ici

`/home/yourname/.bash_history`

_ctrl+r_ permet de faire une recherche dans les lignes de commande tapé

# Message

Pour afficher un message il faudra l'écrire la dedans

`/etc/motd`
`/etc/nologin`

## Suppression en masse

Il peux arriver qu'un dossier comporte tellement de fichier que vider le dossier deviens impossible meme avec un rm

```Shell
find . -name "*.toto" -exec rm {} \;
```

## Creer en masse

Cette ligne de commande creer 9 fichiers

```Shell
touch abcd{1..9}-wyz
```

# Rechercher

Rechercher une chaine de caractère dans tout les fichiers du repertoire courant

```Shell
grep -rnw './' -e 'machainearechercher'
```

# Process and Jobs

> Crtl-z, jobs, bg, fg, nohup &, ps, pkill, nice

systemctl
ps -aux
top
crontab -e pour creer un crontab
crontab -l pour lister les crontab

on peux definir plus facilement les cron dans /etc/

ls -l | grep cron

# Mot de passe

```Shell
passwd
```

# Commande sed

Remplacer tout les mots `ancien` par les mots `nouveau` du fichier `MonFichier`

```Shell
sed 's/ancien/nouveau/g' MonFichier
```

Rajouter l'argument -i pour que cela soit actif

```Shell
sed -i 's/ancien/nouveau/g' MonFichier
```

Supprimer toutes les lignes dans le fichier contenant le mot `ancien` dans `MonFichier`

```Shell
sed '/ancien/d' MonFichier
```

Supprimer toutes les lignes vide dans `MonFichier`

```Shell
sed -i '/^$/d' MonFichier
```

# Tester chiffrement TSL/SSL (STARTTLS)

```BATCH
git clone –depth 1 https://github.com/drwetter/testssl.sh.git
cd testssl.sh
./testssl.sh https://qwant.com
```

# Detecter le traffic malicieux

https://github.com/stamparm/maltrail

# Voir quel paquets sont installés

```SHELL
dpkg -l | grep <paquet>
```

# Gestion utilisateurs

Creer l'utilisateur `toto` et l'ajouter au group sudo

```SHELL
useradd toto
usermod -aG sudo toto
```

Supprimer un utilisateur

```SHELL
userdel -r toto
```

Chercher toto dans /etc/group

```SHELL
grep toto /etc/group
```

Creer un groupe TEST

```SHELL
groupadd TEST
```

Voir la liste de tout les groupes

```SHELL
cat /etc/group/
```

# rsync

Synchroniser deux repertoire sur un serveur local

```BATCH
rsync -zvr /var/opt/installation/inventory /root/temp
```

# Récupération de disque dur

Recupérer [dd_rescue](http://www.garloff.de/kurt/linux/ddrescue/)

Décompressez, puis make pour créer le binaire

Si le disque enfommagé est /dev/sdb1
Si le disque libre est /dev/sdc1

Information du disque

```BATCH
fdisk -l
```

Se placer dans le repertoire dd_rescue

```BATCH
dd_rescue -l transfert_errors.log /dev/sdb1 /dev/sdc1
```

Le fichier log va être créé et qui va lister les blocs qu'il n'a pas plus traiter

Choisisser un des lignes de commande ci-dessous selon votre système de fichiers

```BATCH
fsck.ext2
fsck.ext3
fsck.ext4
fsck.vfat
```

# Autre 

```Shell
df -h
top
dmesg
iostat 1
free
cat /proc/cpuinfo
cat /proc/meminfo
```

```Shell
iftop
```

```Shell
nethogs
```

```Shell
iptraf-ng
```

```Shell
apt install sysstat

# Regarder la crontab
cat /etc/cron.d/sysstat

# les fichiers générés
ls /var/log/sa

# CPU
sar -u

# Memoire
sar -r

# Swap
sar -W

# Etat des IO disque
sar -b

# Entree sortie
sar -d

# Une période
sadf -s 05:00:01 -e 06:00:01 -dT /var/log/sa/sa15 -- -A
```

