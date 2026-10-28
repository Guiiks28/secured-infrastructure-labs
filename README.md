# 🛡️ Infrastructure Systems & Network Security Portfolio

Bienvenue sur mon portfolio technique d'**Administrateur d'Infrastructures Sécurisées**. 

Ce dépôt rassemble l'ensemble de mes procédures techniques, schémas d'architecture et documentations pratiques réalisés sur un environnement de laboratoire **Windows Server 2025** sous hyperviseur **Proxmox VE**.

---

## 📌 Sommaire des Modules

| Dossier | Sujet principal | Technologies clés | Documentation |
| :--- | :--- | :--- | :--- |
| **`01-DNS-IIS/`** | Résolution de noms & Publication Web mutualisée | DNS (SRV, PTR), IIS (Host Headers), IPv4/IPv6 | [Consulter le Readme](./01-DNS-IIS/README.md) |
| **`02-Sauvegarde-VSS/`** | Plan de reprise d'activité & Continuité | Windows Server Backup, VSS, RPO/RTO, DSRM | [Consulter le Readme](./02-Sauvegarde-VSS/README.md) |
| **`03-WDS-PXE/`** | Déploiement automatisé d'OS par le réseau | WDS, Démarrage PXE, Options DHCP 66/67, Images WIM | [Consulter le Readme](./03-WDS-PXE/README.md) |
| **`04-RDS-VDI/`** | Virtualisation de services, RemoteApp & PKI | RDS (5 rôles), RemoteApp, VDI vs Sessions, Certificats GPO | [Consulter le Readme](./04-RDS-VDI/README.md) |

---

## 🛠️ Environnement de Laboratoire

- **Hyperviseur** : Proxmox VE 8.x
- **Contrôleur de Domaine** : Windows Server 2025 (`WS2026.oclock.lan` - IP: `192.168.X.10`)
- **Routeur / Pare-feu** : OPNsense
- **Poste Client** : Windows 11 Enterprise (Client de test en VM)

---

## 👤 À propos
- **Auteur** : Guillaume GEREMY
- **Objectif** : Documentation d'infrastructures sécurisées, automatisation et validation de compétences d'administration système et réseau.
