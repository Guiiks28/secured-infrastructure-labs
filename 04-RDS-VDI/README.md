# 📌 Procédure Technique : Déploiement RDS, RemoteApp & Certificats de Confiance

## 🎯 Objectif
Mettre en place une infrastructure de virtualisation de services sous **Windows Server 2025** permettant :
- La publication d'applications métiers via **RemoteApp**.
- La gestion des accès sécurisés en HTTPS sans avertissement de sécurité sur les postes clients.
- La préparation à l'architecture **VDI** (Virtual Desktop Infrastructure).

---

## 🛠️ Prérequis & Environnement de Lab

- **Serveur** : Windows Server 2025 (`WS2026.oclock.lan` - IP: `10.0.0.10`).
- **Client** : Windows 11 / Client léger de test.
- **Hyperviseur** : Proxmox VE (utilisé pour la prise de Snapshots).
- **Domaine AD** : `oclock.lan`.

---

## 📐 Architecture du Service RDS (5 Rôles Clés)

1. **RD Session Host (RDSH)** : Héberge et exécute les sessions et applications.
2. **RD Connection Broker** : Aiguilleur de connexions et gestionnaire de reconnexion.
3. **RD Web Access** : Portail web d'accès aux applications.
4. **RD Gateway** : Tunneling RDP dans le flux HTTPS (port 443).
5. **RD Licensing** : Gestion des licences CALs RDS (Période de grâce de 120 jours).

> **Note d'isolation** : Les architectures *Sessions* et *VDI* ne devant pas cohabiter sur le même contrôleur de domaine en production, un snapshot préventif a été réalisé avant le déploiement.

---

## 🚀 Étapes de Déploiement

### 1️⃣ Installation du Rôle RDS
1. Dans le **Gestionnaire de serveur**, sélectionner *Ajouter des rôles et fonctionnalités*.
2. Choisir **Installation des Services Bureau à distance** -> **Démarrage rapide** -> **Basé sur des sessions**.
3. Valider le serveur cible et effectuer le redémarrage requis.

### 2️⃣ Publication d'une Application RemoteApp (ex: GIMP)
1. Installer le logiciel requis sur le serveur (`WS2026`) pour *Tous les utilisateurs*.
2. Ouvrir la console **RDS** -> `QuickSessionCollection` -> **Programmes RemoteApp** -> *Publier*.
3. Sélectionner l'application dans la liste scannée et valider la publication.

### 3️⃣ Génération & Déploiement du Certificat de Confiance

#### A. Création du certificat auto-signé (PowerShell)
```powershell
# Génération du certificat avec l'usage Signature Numérique obligatoire
$cert = New-SelfSignedCertificate -DnsName "ws2026.oclock.lan" -CertStoreLocation "cert:\LocalMachine\My" -KeyUsage DigitalSignature
```

#### B. Export des fichiers (.pfx et .cer)
- **Export `.pfx`** (avec clé privée) pour signer le déploiement côté serveur.
- **Export `.cer`** (sans clé privée) pour la distribution côté clients.

#### C. Signature du déploiement
Dans la console **RDS** -> *Modifier les propriétés de déploiement* -> *Certificats* -> Appliquer le fichier `.pfx`.

#### D. Déploiement par GPO sur les postes clients
1. Créer la GPO `GP_Certificat_RDS`.
2. Naviguer vers : `Configuration ordinateur` -> `Stratégies` -> `Paramètres Windows` -> `Paramètres de sécurité` -> `Stratégies de clé publique` -> `Autorités de certification racines de confiance`.
3. Importer le fichier `.cer` et appliquer un `gpupdate /force` sur les clients.

---

## ✅ Validation & Tests

1. Connexion au portail client : `https://ws2026.oclock.lan/RDWeb`.
2. Authentification avec un compte utilisateur du domaine (`OCLOCK\utilisateur`).
3. Lancement de l'application **RemoteApp** : Vérification de l'absence totale d'avertissement de sécurité (Éditeur inconnu / Certificat non approuvé).

---

## ⚠️ Erreurs Fréquentes & Troubleshooting

| Symptôme | Cause probable | Solution |
| :--- | :--- | :--- |
| Avertissement *"Éditeur inconnu"* persiste sur le client | Certificat déposé dans le mauvais magasin ou pour l'utilisateur actuel | Vérifier l'emplacement via `certlm.msc` : Doit être dans **Autorités de certification racines de confiance** pour **Ordinateur local**. |
| Erreur 401 / Refus d'accès | Mauvais format de domaine saisi à l'authentification | Saisir le FQDN complet `DOMAINE\Utilisateur` ou configurer le domaine par défaut. |
| Avertissement au lancement RDP | Certificat généré sans l'usage *DigitalSignature* | Régénérer le certificat via le script PowerShell dédié. |

---

## 📌 Conclusion & Bonnes Pratiques
- **Signer ne suffit pas** : La confiance doit être explicitement accordée par le poste client.
- **Principe de moindre privilège** : Ne jamais exécuter un pool d'application ou un service RDS avec des privilèges *Administrateur du Domaine*].
