# K2000 - Clignotant Connecté pour Vélo

## Contexte du Projet

Ce projet a été réalisé dans le cadre d'une mise en compétition de 4 groupes lors de la seconde année de DUT (Diplôme Universitaire de Technologie). L'objectif était de proposer une solution innovante de **clignotant connecté pour vélo**.

## Objectif

Développer un système complet permettant aux cyclistes de signaler leurs changements de direction via une application smartphone, avec un boîtier de feux arrière contrôlé par Bluetooth.

## Fonctionnalités

### Application Android
- **Contrôle des clignotants** : gauche, droite, warnings
- **Feux arrière** : allumage/extinction avec réglage de la luminosité
- **Interface flottante** : popup superposée aux autres applications pour un accès rapide
- **Configuration** : transparence de la fenêtre, luminosité des feux, options diverses
- **Capteur de luminosité** : allumage automatique des feux selon la luminosité ambiante
- **Niveau de batterie** : affichage du niveau de batterie du module

### Module Arduino
- **Contrôle des LEDs** via registres à décalage 74HC595
- **Communication Bluetooth** avec le smartphone (module HC-05)
- **Gestion de la batterie** : surveillance du niveau et extinction automatique
- **Animation K2000** : effet "Knight Rider" au démarrage
- **Mise en veille automatique** après une période sans connexion

### Protocole de Communication

Les commandes sont envoyées sous la forme `XXk` où :
- Premier chiffre = commande
- Second chiffre = paramètre (0 = off, 1 = on)

| Commande | Description |
|----------|-------------|
| 10/11 | Clignotant droit off/on |
| 20/21 | Clignotant gauche off/on |
| 30/31 | Warnings off/on |
| 40/41 | Feux stop off/on |
| 50/51 | Feux de position off/on |
| 60-64 | Niveau de luminosité |
| 70/71 | Ping/pong (test connexion) |
| 80 | Extinction du module |
| 99 | Authentification |

## Architecture Technique

```
┌─────────────────┐     Bluetooth      ┌─────────────────┐
│   Application   │◄──────────────────►│    Module HC-05 │
│     Android     │                    └────────┬────────┘
└─────────────────┘                             │
                                                │ Serial
                                        ┌───────▼───────┐
                                        │  Arduino Pro  │
                                        │    Mini 3.3V  │
                                        └───────┬───────┘
                                                │
                    ┌───────────────────────────┼───────────────────────────┐
                    │                           │                           │
            ┌───────▼───────┐           ┌───────▼───────┐           ┌───────▼───────┐
            │   74HC595 #1  │           │   74HC595 #2  │           │   74HC595 #3  │
            │  (Cligno D)   │           │  (Cligno G)   │           │    (Feux)     │
            └───────┬───────┘           └───────┬───────┘           └───────┬───────┘
                    │                           │                           │
                8 LEDs                       8 LEDs                     8 LEDs
```

## Matériel Utilisé

- **Arduino Pro Mini 3.3V** : microcontrôleur principal
- **Module Bluetooth HC-05** : communication sans fil
- **3x 74HC595** : registres à décalage pour contrôler les LEDs
- **TP4056** : circuit de charge Li-ion
- **Batterie Li-ion** : alimentation du système
- **LEDs** : clignotants (gauche/droite) et feux arrière

## Structure du Projet

```
clignotant/
├── Android/main/           # Application Android
│   ├── java/biblio/        # Bibliothèque MVC
│   │   ├── Controler/      # Contrôleurs
│   │   ├── Model/          # Modèles (Bluetooth, Capteurs)
│   │   └── View/           # Vues
│   └── res/                # Ressources Android
├── Arduino/
│   ├── k2000/k2000.ino     # Firmware Arduino
│   ├── datasheets/         # Documentations techniques
│   └── schemas/            # Schémas électroniques
└── Notice d'utilisation.pdf
```

## Installation

### Application Android
Importer le projet `Android/main/` dans Android Studio et compiler l'APK.

### Module Arduino
1. Ouvrir `Arduino/k2000/k2000.ino` dans l'IDE Arduino
2. Sélectionner "Arduino Pro Mini 3.3V" comme carte
3. Téléverser le programme

## Auteur

Projet réalisé dans le cadre du DUT par F. Leboulanger.
