# .MDPlayground
Ce repo servira à tester les documentations pour le lycée en .MD, en espérant que ça passe crème autant que la README de la réinstallation Windows que j'ai élaborée.

# `💻`︲Documentation TP Active Directory

---

Ce repo GitHub présente un guide complet pour réaliser le TP **Active Directory**, étape par étape. Les instructions sont claires et détaillées afin que tu puisses suivre sans problème, **même si tu es débutant**.

---

## `📑`︲Sommaire (Cliquez sur la catégorie souhaitée pour y être redirigé en un éclair ! )

1. [`📘`︲Introduction](#introduction)
   - [`📄`︲Contexte et objectifs du TP](#contexte-et-objectifs-du-tp)
   - [`🖥️`︲Présentation de l'architecture réseau et des outils utilisés](#presentation-de-larchitecture-reseau-et-des-outils-utilises)

2. [`🛠️`︲Préparation de l'environnement](#preparation-de-lenvironnement)
   - [`💿`︲Installation de Windows 11 (client)](#installation-de-windows)
   - [`💿`︲Installation de Windows Server 2025 (serveur)](#installation-de-windows-server)

3. [`🏛️`︲Installation et configuration du contrôleur de domaine](#installation-et-configuration-du-controleur-de-domaine)
   - [`🔧`︲Installation des rôles AD DS et DNS](#installation-roles-ad-ds-et-dns)
   - [`🌐`︲Promotion du serveur et création du domaine descartesbleu.org](#promotion-du-serveur-et-creation-du-domaine)

4. [`🗂️`︲Administration de l'annuaire Active Directory](#administration-de-lannuaire-active-directory)
   - [`🔑`︲Simplification de la stratégie de mots de passe](#simplification-strategie-mots-de-passe)
   - [`👥`︲Création des OU, groupes et utilisateurs](#creation-ou-groupes-utilisateurs)

5. [`💻`︲Intégration d'un client au domaine](#integration-dun-client-au-domaine)
   - [`🌐`︲Configuration réseau et paramètres DNS](#configuration-reseau-dns)
   - [`🔗`︲Joindre le domaine descartesbleu.org](#joindre-domaine)

6. [`📁`︲Gestion des partages de fichiers](#gestion-des-partages-de-fichiers)
   - [`📂`︲Création des dossiers partagés et permissions](#creation-dossiers-partages)
   - [`💾`︲Redirection des dossiers utilisateur et mode hors connexion](#redirection-dossiers-mode-hors-ligne)

7. [`📜`︲Automatisation via PowerShell](#automatisation-via-powershell)
   - [`⚡`︲Script pour créer des OU, groupes et utilisateurs à partir d'un CSV](#script-ou-groupes-utilisateurs)

8. [`🖱️`︲Déploiement de stratégies de groupe (GPO)](#deploiement-de-strategies-de-groupe)
   - [`📂`︲Redirection du dossier Documents, mappage lecteurs réseau et déploiement Firefox](#redirection-documents-mappage-firefox)

9. [`🔒`︲Restrictions et fonctionnalités avancées](#restrictions-fonctionnalites-avancees)
   - [`⏱️`︲Limitation des horaires de connexion, bureau à distance et BgInfo](#limitation-horaires-bureau-bginfo)

10. [`✅`︲Conclusion](#conclusion)
    - [`📝`︲Résumé des tâches et résultats`](#resume-taches-resultats)
    - [`🌟`︲Impact des configurations sur collaboration et organisation`](#impact-configurations)

---

<a id="introduction"></a>
## `📘`︲Introduction

Bienvenue dans ce TP Active Directory. Ici, tu apprendras à configurer un domaine, gérer les utilisateurs, appliquer des GPO et automatiser certaines tâches via PowerShell.

<a id="contexte-et-objectifs-du-tp"></a>
### `📄`︲Contexte et objectifs du TP
- Comprendre le rôle d’un contrôleur de domaine
- Mettre en place un environnement réseau fonctionnel
- Automatiser certaines tâches d’administration

<a id="presentation-de-larchitecture-reseau-et-des-outils-utilises"></a>
### `🖥️`︲Présentation de l'architecture réseau et des outils utilisés
- **Serveur :** Windows Server 2025
- **Client :** Windows 11
- **Outils :** Active Directory, DNS, PowerShell, GPO

---

<a id="preparation-de-lenvironnement"></a>
## `🛠️`︲Préparation de l'environnement

---

<a id="installation-de-windows"></a>
### `💿`︲Installation de Windows 11 (client)

1️⃣・**Configuration de la VM**  
   - **Disque :** 80 Go  
   - **RAM :** 4 Go  
   - **CPU :** 1 cœur  

<details>
  <summary>📸︲Configuration initiale (VMware)</summary>

---

<img width="761" height="733" alt="Screenshot_29" src="https://github.com/user-attachments/assets/8e838f92-9bf5-445a-b6e1-61ea1c5d9e1b" />

Sur cette capture, on peut voir la **configuration de la mémoire de la VM sous VMware**.  
Il faut régler la mémoire à **4096 Mo (4 Go)**, soit en utilisant le curseur, soit en entrant la valeur manuellement.  
Enfin, cliquer sur **OK** pour valider les paramètres et sauvegarder la configuration.

---
</details>

---

2️⃣・**Installation depuis l’ISO**  
   - Sélectionner langue, clavier et région  

<details>
  <summary>📸︲Sélection clavier et disque</summary>

  ---
  <img width="1022" height="769" alt="Screenshot_2" src="https://github.com/user-attachments/assets/4013d7fe-1cf0-4e5c-8d7d-b4cf663a85e1" />

  Sur cette capture, on peut voir la **sélection du clavier**.  
  Il faut s’assurer que la méthode d’entrée est **Français (Traditionnel, AZERTY)** avant de cliquer sur *Suivant*.

  ---
  <img width="1026" height="771" alt="Screenshot_3" src="https://github.com/user-attachments/assets/4b8cf19c-df8b-443c-9127-bc6d3805b8a7" />

  Sur cette capture, on peut voir le **type d’installation**.  
  Il faut choisir *Installer Windows 11* et cocher la suppression de tous les fichiers, applications et paramètres avant de cliquer sur *Suivant*.

</details>

---

3️⃣・**Création de l’utilisateur**  
   - **Nom :** `btssio`  
   - **Mot de passe :** `btssio`  

<details>
  <summary>📸︲Création de l’utilisateur</summary>

<img width="1022" height="769" alt="Screenshot_11" src="https://github.com/user-attachments/assets/603eca66-704a-4aa0-8b73-7ed9f5db21c1" />

➡️ Entrer le **nom d’utilisateur `btssio`**, cliquer sur **Suivant**

<img width="1024" height="770" alt="Screenshot_13" src="https://github.com/user-attachments/assets/73800d3f-d047-4310-91e1-c5b03380349b" />

➡️ Entrer le **mot de passe `btssio`** et confirmer  

</details>

<details>
  <summary>📸︲OPTIONEL CHOIX OOBE (Pour réduire la télémetrie !)</summary>
  
<img width="1026" height="770" alt="Screenshot_18" src="https://github.com/user-attachments/assets/4004e27f-c2c2-46b7-9460-b3ddda233c92" />
<img width="1022" height="771" alt="Screenshot_17" src="https://github.com/user-attachments/assets/720c73cd-2ad4-465e-b58a-ca5906f895f3" />
<img width="1027" height="771" alt="Screenshot_16" src="https://github.com/user-attachments/assets/592a58d9-d7e7-4497-b808-62d184f0e42f" />
<img width="994" height="771" alt="Screenshot_15" src="https://github.com/user-attachments/assets/6cd89070-4682-46e5-9c8d-714d89b30ec8" />
<img width="1026" height="770" alt="Screenshot_14" src="https://github.com/user-attachments/assets/3ac9e60f-a7db-4ad0-ba02-7af20f2f2ee9" />

</details>

---

<a id="installation-de-windows-server"></a>
### `💿`︲Installation de Windows Server 2025 (serveur)

1️⃣・**Configuration de la VM**  
   - Disque : **80 Go**  
   - RAM : **2 Go**  
   - CPU : **1 cœur**

<details>
  <summary>📸︲Configuration initiale serveur</summary>

<img width="759" height="731" alt="Screenshot_31" src="https://github.com/user-attachments/assets/09f78ba3-4e73-4579-b685-746b19399063" />

Vérifier que disque, RAM et CPU sont corrects avant l’installation.

</details>

---

2️⃣・**Partitionnement du disque**  
   - 40 Go pour l’OS  
   - 40 Go pour DATA  

<details>
  <summary>📸︲Partitionnement</summary>

<img width="1022" height="772" alt="Screenshot_5" src="https://github.com/user-attachments/assets/3d76f21a-6dca-4641-903a-3f5ddcb6db0f" />

Créer deux partitions : 40 Go pour l’OS et 40 Go pour les données.

</details>

---

3️⃣・**Installation depuis l’ISO**  
   - Sélectionner langue, clavier et région  

<details>
  <summary>📸︲Sélection ISO serveur</summary>

<img width="1018" height="771" alt="Screenshot_1" src="https://github.com/user-attachments/assets/0a8564ef-ff3f-43d4-ba0c-2fbee5e9de43" />
<img width="1026" height="767" alt="Screenshot_19" src="https://github.com/user-attachments/assets/e36dceae-aa06-4a4b-ba8d-cc3a935823ba" />

Choisir langue Français et clavier pour le serveur.  

</details>

---

4️⃣・**Sélection de la méthode d’installation**  
   - Choisir **Installation personnalisée** (Custom Install)  
   - Sélectionner **Méthode de licence** et entrer la **clé produit**  
   - Sélectionner l’image : **Windows Server 2025 Standard (expérience utilisateur)**  

<details>
  <summary>📸︲Méthode d’installation et image</summary>

<img width="1026" height="774" alt="Screenshot_2" src="https://github.com/user-attachments/assets/dff9d49b-90ce-418f-bf61-931849ae3b6b" />
Vérifier la méthode d’installation, entrer la clé produit et choisir l’image correcte.

<img width="1026" height="770" alt="Screenshot_3" src="https://github.com/user-attachments/assets/350e3289-aca5-4c5b-8440-e6a4807825fb" />
Entrer la **clé produit**.

<img width="1022" height="773" alt="Screenshot_4" src="https://github.com/user-attachments/assets/f96bac0c-f279-4a23-b65e-b9f92ebd888d" />
Sélectionner l’image : **Windows Server 2025 Standard (expérience utilisateur)**

<img width="1019" height="768" alt="Screenshot_6" src="https://github.com/user-attachments/assets/b09467b8-122a-407d-913d-1b060b14b1c5" />


</details>

---

5️⃣・**Création du compte administrateur**  
   - Nom : `btssio`  
   - Mot de passe : `btssio-lmc25`  

---

6️⃣・**Configuration réseau**  
   - IP : `172.16.0.1`  
   - Masque : `255.255.255.0`  

<details>
  <summary>📸︲Paramètres réseau serveur</summary>

<img width="1024" height="769" alt="Screenshot_11" src="https://github.com/user-attachments/assets/d8ecdf27-9dfd-4eb5-8405-c74a969fc1e8" />
Configurer IP et masque pour le serveur.

</details>

---

7️⃣・**Vérification de l’installation**  
   - Redémarrer et se connecter avec le compte administrateur  

<details>
  <summary>📸︲Vérification finale serveur</summary>

<img width="1027" height="774" alt="Screenshot_30" src="https://github.com/user-attachments/assets/2d4565cd-66e0-454e-be4c-6f002718e385" />
Le serveur est prêt et l’administrateur peut se connecter !

</details>

---

<details>
  <summary><strong>💡︲Conseils pour Windows Server</strong></summary>

  💡 - Vérifiez que la partition DATA est correctement détectée après l’installation.
  
</details>

---

<a id="installation-et-configuration-du-controleur-de-domaine"></a>
### `🏛️`︲Installation et configuration du contrôleur de domaine
---
<a id="installation-roles-ad-ds-et-dns"></a>
### `🔧`︲Installation des rôles AD DS et DNS...

1️⃣ ︲ Accéder à l’ajout de rôles et fonctionnalités

Ouvrez le Gestionnaire de serveur.

Dans le Tableau de bord, cliquez sur Gérer.

Sélectionnez ensuite Ajouter des rôles et des fonctionnalités.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
  <img width="1029" height="773" alt="Screenshot_10" src="https://github.com/user-attachments/assets/b8bcf212-d61f-4e17-b618-cf23f5ad5e82" />

</details>

---

2️⃣︲Lancer l’assistant d’ajout de rôles et de fonctionnalités

La fenêtre Assistant Ajout de rôles et de fonctionnalités s’ouvre automatiquement.

Cliquez sur Suivant pour continuer.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
<img width="1027" height="767" alt="Screenshot_1" src="https://github.com/user-attachments/assets/59314f25-34eb-4084-8768-34021fd2179b" />
</details>

---

3️⃣︲Choisir l'installation basée sur un rôle ou une fonctionnalité

Dans la fenêtre suivante, sélectionnez Installation basée sur un rôle ou une fonctionnalité.

Vous verrez une liste avec des options sous forme de puces.

Cliquez sur Suivant pour continuer.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
<img width="1028" height="767" alt="Screenshot_2" src="https://github.com/user-attachments/assets/974e4085-45c6-4ebd-8d84-9572d2f404f9" />
</details>

---

4️⃣︲Choisir le serveur pour l'installation

L’assistant vous demande ensuite où installer la fonctionnalité.

Cliquez sur Sélectionner un serveur du pool de serveurs.

Dans la liste, choisissez le serveur SRV-AD1.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
<img width="1025" height="773" alt="Screenshot_3" src="https://github.com/user-attachments/assets/1f1a653f-6a8f-4934-8ee9-87679b9f353d" />
</details>

---

5️⃣︲Sélectionner les fonctionnalités à installer

Un menu s'ouvre avec des cases à cocher pour sélectionner les fonctionnalités.

Cherchez et cochez la fonctionnalité Service de domaine Active Directory.

Une nouvelle fenêtre s'ouvre.

Cochez la case Inclure les outils de gestion, si applicable (cette option est cochée par défaut).

Cliquez sur Ajouter des fonctionnalités.

Et ensuite faire "Suivant"
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
<img width="1025" height="773" alt="Screenshot_4" src="https://github.com/user-attachments/assets/1d7493b3-89ad-4107-b894-4e9979339b02" />
   
---

<img width="1026" height="771" alt="Screenshot_5" src="https://github.com/user-attachments/assets/4b8aab9d-6982-4250-94f9-b055739aa7f0" />
</details>

---

6️⃣︲Confirmer et lancer l'installation

Vérifiez que toutes les options d'installation sont correctes pour éviter toute erreur.

Une fois la vérification effectuée, cliquez sur Suivant pour commencer l'installation du rôle.

L'installation prendra quelques minutes.

Une fois terminée, il sera nécessaire de redémarrer le serveur pour appliquer les changements.

<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1024" height="770" alt="Screenshot_7" src="https://github.com/user-attachments/assets/afa101e0-3f52-4df5-92df-7f4ea79750fb" />
   <img width="1025" height="771" alt="Screenshot_8" src="https://github.com/user-attachments/assets/624efe48-fd2b-4425-848f-23b69dff1f2a" />
   <img width="1027" height="770" alt="Screenshot_9" src="https://github.com/user-attachments/assets/1b5b2339-2f70-4f99-809f-c5bf919d4da6" />
</details>

---
<a id="promotion-du-serveur-et-creation-du-domaine"></a>
### `🌐`︲Promotion du serveur et création du domaine descartesbleu.org
---

1️⃣︲Vérification de l'installation et promotion du serveur

Une fois l'installation terminée et le serveur redémarré, ouvrez le Gestionnaire de serveur.

Une icône jaune avec un signe de danger/attention devrait apparaître, indiquant qu'une action est nécessaire.

Nous allons maintenant procéder à la promotion du serveur et à la création du domaine descartesbleu.org.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1026" height="768" alt="Screenshot_12" src="https://github.com/user-attachments/assets/a3169f35-e235-4044-a60d-5737daf172f3" />
</details>

---

2️⃣︲Configurer les services de domaine Active Directory

La fenêtre de l’assistant Configuration des services de domaine Active Directory apparaît.

Dans cette fenêtre, sélectionnez Ajouter une nouvelle forêt.

Dans le champ de texte, saisissez le nom du domaine : descartesbleu.org.

Cliquez ensuite sur Suivant pour continuer.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1024" height="775" alt="image" src="https://github.com/user-attachments/assets/f0226bab-e78b-4a08-bca4-ff9d8ee1d8ed" />
   </details>

---

3️⃣︲Configurer les options du contrôleur de domaine

Dans les options du Contrôleur de domaine, vérifiez que l'option Serveur DNS est cochée. Si ce n'est pas le cas, cochez-la manuellement.

Dans les champs de texte pour les mots de passe, saisissez le mot de passe suivant :
btssio-lmc25
(Note : assurez-vous de le saisir deux fois, dans les deux champs de mot de passe).
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
      <img width="1022" height="770" alt="Screenshot_14" src="https://github.com/user-attachments/assets/16c3bef0-54e8-4aba-90a7-ff75ad8f97a3" />
</details>

---

4️⃣︲Configurer les options DNS et le domaine NetBIOS

Une fois les informations saisies, cliquez sur Suivant pour passer à la fenêtre des options DNS.

Dans le champ de texte du domaine NetBIOS, saisissez DESCARTESBLEU.

En général, ce champ se remplit automatiquement, mais si ce n’est pas le cas, entrez-le manuellement.

Cliquez ensuite sur Suivant pour continuer.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1025" height="770" alt="Screenshot_15" src="https://github.com/user-attachments/assets/0cb4593f-715b-4e5e-80d9-06db2f61c9b1" />
   <img width="1024" height="776" alt="Screenshot_16" src="https://github.com/user-attachments/assets/f9e62c9d-7c22-4b1a-830c-bd15b337df2f" />
</details>

---

5️⃣︲Vérification des options et lancement de l'installation

Dans la fenêtre suivante, il vous sera demandé de définir le chemin d’accès pour le stockage des fichiers. Laissez les valeurs par défaut.

Un récapitulatif des options sélectionnées apparaîtra. Relisez attentivement ce récapitulatif pour éviter toute erreur.

Vérifiez bien toutes les options, notamment le nom du domaine et les paramètres du contrôleur de domaine.

Une fois que vous avez effectué toutes les vérifications nécessaires, validez toutes les options et cliquez sur Installer pour lancer l’installation.

Le processus d’installation commencera et le serveur sera promu en tant que contrôleur de domaine.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1034" height="783" alt="Screenshot_20" src="https://github.com/user-attachments/assets/1357a4a6-6561-47b3-bfc4-7ae523bceef9" />
   <img width="1027" height="771" alt="Screenshot_19" src="https://github.com/user-attachments/assets/fe3f5c3c-b585-4356-9013-9ddd17278323" />
   <img width="1027" height="774" alt="Screenshot_18" src="https://github.com/user-attachments/assets/3483084e-e5ed-4b0c-8839-c367ac750562" />
   <img width="1029" height="775" alt="Screenshot_17" src="https://github.com/user-attachments/assets/db11bbf5-1537-498c-bd50-5164e187fa33" />
</detail>

---






