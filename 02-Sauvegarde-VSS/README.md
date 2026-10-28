# 💾 Module 02 : Sauvegarde, Restauration & Service VSS

## 🎯 Objectifs
- Mettre en place une stratégie de sauvegarde du serveur complet (*Bare Metal Recovery*).
- Comprendre les mécanismes du service de cliché instantané **VSS** (*Volume Shadow Copy Service*).
- Maîtriser la restauration d'éléments individuels et de l'annuaire Active Directory (DSRM).

---

## 📋 Concepts Clés
- **RPO (Recovery Point Objective)** : Quantité maximale de données que l'entreprise accepte de perdre entre deux sauvegardes.
- **RTO (Recovery Time Objective)** : Durée maximale d'interruption de service tolérée.
- **VSS** : Cliché instantané à chaud permettant la sauvegarde cohérente des bases actives (NTDS.dit, registre, IIS) sans interruption de service.
- **Règle 3-2-1** : 3 copies, 2 supports différents, 1 copie hors site.

---

## 🛠️ Procédures de Restauration

### A. Restauration d'un fichier supprimé
1. Lancer l'assistant Windows Server Backup.
2. Sélectionner la date/heure de sauvegarde.
3. Choisir **Restaurer vers un autre emplacement** pour contrôler le fichier avant remise en production.

### B. Restauration de l'Annuaire AD (Mode DSRM)
1. Passer le serveur en mode de restauration des services d'annuaire :
   ```cmd
   bcdedit /set safeboot dsrepair
   shutdown /r /t 0
   ```
2. Se connecter avec le compte local DSRM.
3. Effectuer la restauration de l'état du système (*System State*).
4. Rétablir le démarrage normal **avant** de redémarrer :
   ```cmd
   bcdedit /deletevalue safeboot
   shutdown /r /t 0
   ```
