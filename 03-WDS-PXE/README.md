# 🚀 Module 03 : Déploiement d'OS en Série (WDS & PXE)

## 🎯 Objectifs
- Configurer le rôle **WDS** (*Windows Deployment Services*) pour déployer des postes clients sur le réseau sans support physique (USB/DVD).
- Assurer le démarrage réseau via les options **DHCP 66 et 67**.

---

## 📋 Concepts Clés
- **Démarrage PXE** : Démarrage réseau du micrologiciel de la carte réseau permettant de charger un micro-environnement en RAM (`boot.wim`).
- **Image de Démarrage (`boot.wim`)** : Environnement Windows PE temporaire chargé en mémoire vive.
- **Image d'Installation (`install.wim`)** : Le système d'exploitation final qui sera déposé sur le disque dur.
- **Options DHCP** : 
  - `Option 66` : Adresse IP du serveur TFTP / WDS (Le "Où").
  - `Option 67` : Nom du fichier de démarrage réseau (Le "Quoi").

---

## 🛠️ Étapes de Configuration

1. Installation de la fonctionnalité WDS sur un volume dédié (`D:\RemoteInstall`).
2. Importation de l'image de démarrage `boot.wim` depuis le média Windows Server/Windows 11.
3. Importation de l'image d'installation `install.wim`.
4. Configuration du serveur WDS pour répondre aux clients PXE inconnus (en environnement de lab).

---

## 🧪 Validation
- Création d'une VM vierge (sans disque OS préinstallé).
- Démarrage sur la carte réseau (Network Boot / PXE).
- Validation du chargement de l'environnement d'installation Windows PE via le réseau.
