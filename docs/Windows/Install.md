---
sidebar_position: 1
---

# 🧨 Installation

Bon à part vous dire suivant suivant et de ne pas rentrer votre connection réseau pendant l'installation, même quand on vous le demande au début, y'a pas grand chose à savoir.

Si c'est un windows cracké vous tapez KVM sur un de vos site illégaux favoris ☠️

## Software

Windows à son propre installeur de paquet, depuis l'arrivé de Windows 10, plus besoin de __Ninite__ ou de __Chocolatey__, maintenant il faut utiliser __winget__. C'est un outil tres puissant ici on verras qu'un petite parti de sont utilité je vous laisse voir la docuementation officiel de microsoft.

Vous pouvez [ici](https://winstall.app/packs/_C1sdLo7m) voir la liste de mes soft que j'installe sur mon PC.

### Executable

Creer un fichier __soft.ps1__ avec les commande suivantes

```BASH
winget install --id=xanderfrangos.twinkletray -e  ; winget install --id=Microsoft.VisualStudioCode -e  ; winget install --id=7zip.7zip -e  ; winget install --id=Google.AndroidStudio -e  ; winget install --id=Google.Chrome -e  ; winget install --id=dbeaver.dbeaver -e  ; winget install --id=MongoDB.Compass.Community -e  ; winget install --id=Microsoft.Teams -e  ; winget install --id=TeamViewer.TeamViewer -e  ; winget install --id=SlackTechnologies.Slack -e  ; winget install --id=Discord.Discord -e  ; winget install --id=Mozilla.Firefox -e  ; winget install --id=BraveSoftware.BraveBrowser -e  ; winget install --id=mRemoteNG.mRemoteNG -e  ; winget install --id=OpenJS.NodeJS.LTS -e  ; winget install --id=Notepad++.Notepad++ -e  ; winget install --id=Postman.Postman -e  ; winget install --id=Microsoft.WindowsTerminal -e  ; winget install --id=LukasBach.Yana -e  ; winget install --id=Git.Git -e  ; winget install --id=Docker.DockerDesktop -e 
```

puis vous n'avez plus qu'a executer __soft.ps1__ avec les droit administrateur.

Pour mettre à jour les logiciels installé de cette façon sur votre PC, il vous suffit de creer un nouveau fichier __maj.ps1__ avec les commande suivantes

```BASH
winget upgrade --all -h 
```

Et ce qui est bien c'est que vous pouvez mettre à jour tout les logiciels installé sur votre PC, dans une tache planifié.