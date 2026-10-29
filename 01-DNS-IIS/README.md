# 🌐 Module 01 : DNS & Service Web IIS

## 🎯 Objectifs
- Configurer la résolution de noms interne Active Directory et la zone inversée (PTR).
- Configurer un serveur web IIS pour héberger **plusieurs sites sur une seule IP et un même port (80)** grâce aux en-têtes d'hôtes.

---

## 📋 Concepts Clés
- **DNS** : Enregistrements A/AAAA, CNAME, PTR, et rôles des enregistrements `SRV` (`_ldap._tcp`) pour Active Directory.
- **Zone Inverse (PTR)** : Indispensable pour la traçabilité dans les journaux, les sauvegardes et la supervision.
- **IIS** : Isolation par *Pool d'applications* (`w3wp.exe`) et configuration des documents par défaut.

---

## 🛠️ Étapes de Configuration

### 1. Configuration DNS
1. Création des enregistrements `A` pour `intranet.oclock.lan` et `promo.oclock.lan`.
2. Création de l'alias `CNAME` : `wiki.oclock.lan` -> `intranet.oclock.lan`.
3. Création de la zone de recherche inversée `X.0.10.in-addr.arpa` et ajout des enregistrements `PTR`.

### 2. Configuration IIS
1. Suppression du site par défaut (*Default Web Site*).
2. Création des répertoires sous `C:\inetpub\`.
3. Liaison des sites avec l'en-tête d'hôte (*Host Header*) correspondant à chaque nom DNS.

---

## 🧪 Validation & Troubleshooting
- Validation client via `nslookup intranet.oclock.lan`.
- Purge du cache client si nécessaire : `ipconfig /flushdns`.
- Test du comportement sur l'IP nue (`http://10.0.0.10`) : Retourne une erreur `400 Bad Request` car aucun en-tête d'hôte ne correspond.
