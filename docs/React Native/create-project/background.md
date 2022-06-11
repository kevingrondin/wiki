---
sidebar_position: 3
---

# ⏳ Background

> Tâche de fond quand un programme se lance sans que l'utilisateur puisse l'intérompre ou le lancer manuellement

Assez spécifique à un projet mobile, voici un [article](https://itnext.io/react-native-background-location-tracking-without-timeout-and-with-app-killed-3dbfbc80ad01) qui traite de ce sujet

## Historique

Depuis l'arrive de Android 8, les service d'arrière plan sont tué si ça consomme trop de batterie, ou qui demande beaucoup de ressource.

Plein de problématique se pose :

  - Maintenir un service en cours d'exécution lorsque l'application est arrêtée, sans désactiver l'optimisation de la batterie.
  - Exécuter du code toutes les X secondes, même lorsque l'application est arrêtée.
  - Si un service est en premier plan en boucle continues, cela ne laissait pas l'application se fermer ou la tuer.