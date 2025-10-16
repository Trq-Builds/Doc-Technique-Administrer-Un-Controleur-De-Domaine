# `💻`︲Documentation TP : Administrer un controleur de domaine

---

Ce Repo GitHub présente un guide complet mais simple pour réaliser le TP Administrer un contrôleur de domaine, étape par étape. Les instructions sont claires et détaillées, de manière à ce que tu puisses suivre sans problème. Pour ne pas te perdre, tu seras aidé avec des captures d'écran ainsi que de courtes vidéos pour te guider visuellement !

---

## `📑`︲Sommaire (Le sommaire est cliquable pour vous rediriger vers la partie souhaitée.)

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
   - [`👥`︲Création des UOs, groupes et utilisateurs](#creation-ou-groupes-utilisateurs)

5. [`💻`︲Intégration d'un client au domaine](#integration-dun-client-au-domaine)
   - [`🌐`︲Configuration réseau et paramètres DNS](#configuration-reseau-dns)
   - [`🔗`︲Joindre le domaine descartesbleu.org](#joindre-domaine)

6. [`📁`︲Gestion des partages de fichiers](#gestion-des-partages-de-fichiers)
   - [`📂`︲Création des dossiers partagés et permissions](#creation-dossiers-partages)
   - [`💾`︲Redirection des dossiers utilisateur et mode hors connexion](#redirection-dossiers-mode-hors-ligne)

7. [`📜`︲Automatisation via PowerShell](#automatisation-via-powershell)
   - [`⚡`︲Script pour créer des UOs, groupes et utilisateurs à partir d'un CSV](#script-ou-groupes-utilisateurs)

8. [`🖱️`︲Déploiement de stratégies de groupe (GPO)](#deploiement-de-strategies-de-groupe)
   - [`📂`︲Redirection du dossier Documents, mappage lecteurs réseau et déploiement Firefox](#redirection-documents-mappage-firefox)

9. [`🔒`︲Restrictions et fonctionnalités avancées](#restrictions-fonctionnalites-avancees)
   - [`⏱️`︲Limitation des horaires de connexion, bureau à distance et BgInfo](#limitation-horaires-bureau-bginfo)

10. [`✅`︲Conclusion](#conclusion)
    - [`📝`︲Résumé des tâches et résultats](#resume-taches-resultats)
    - [`🌟`︲Impact des configurations sur collaboration et organisation](#impact-configurations)

---

<a id="introduction"></a>
## `📘`︲Introduction

Bienvenue dans ce TP Active Directory. Ici, tu apprendras à configurer un domaine, gérer les utilisateurs, appliquer des GPO et automatiser certaines tâches via PowerShell !

---

<a id="contexte-et-objectifs-du-tp"></a>
> [!NOTE]
> - **Comprendre le rôle d’un contrôleur de domaine.**
> - **Mettre en place un environnement réseau fonctionnel.**
> - **Automatiser certaines tâches d’administration.**

---

<a id="presentation-de-larchitecture-reseau-et-des-outils-utilises"></a>
> [!IMPORTANT]
> Présentation de l'architecture réseau et des outils utilisés :
> - **Serveur :** Windows Server 2025.
> - **Client :** Windows 11.
> - **Outils :** Active Directory, DNS, PowerShell, GPO.

---

<a id="preparation-de-lenvironnement"></a>
## `🛠️`︲Préparation de l'environnement

---

<a id="installation-de-windows"></a>
### `💿`︲Installation de Windows 11 (client)

---

1️⃣︲**Configuration de la VM**  
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

2️⃣︲**Installation depuis l’ISO**  
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

3️⃣︲**Création de l’utilisateur**  
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
  <summary>📸︲OPTIONEL CHOIX OOBE</summary>
  
<img width="1026" height="770" alt="Screenshot_18" src="https://github.com/user-attachments/assets/4004e27f-c2c2-46b7-9460-b3ddda233c92" />
<img width="1022" height="771" alt="Screenshot_17" src="https://github.com/user-attachments/assets/720c73cd-2ad4-465e-b58a-ca5906f895f3" />
<img width="1027" height="771" alt="Screenshot_16" src="https://github.com/user-attachments/assets/592a58d9-d7e7-4497-b808-62d184f0e42f" />
<img width="994" height="771" alt="Screenshot_15" src="https://github.com/user-attachments/assets/6cd89070-4682-46e5-9c8d-714d89b30ec8" />
<img width="1026" height="770" alt="Screenshot_14" src="https://github.com/user-attachments/assets/3ac9e60f-a7db-4ad0-ba02-7af20f2f2ee9" />

</details>

---

<a id="installation-de-windows-server"></a>
### `💿`︲Installation de Windows Server 2025 (serveur)

---

1️⃣︲**Configuration de la VM**  
   - Disque : **80 Go**  
   - RAM : **2 Go**  
   - CPU : **1 cœur**

<details>
  <summary>📸︲Configuration initiale serveur</summary>

<img width="759" height="731" alt="Screenshot_31" src="https://github.com/user-attachments/assets/09f78ba3-4e73-4579-b685-746b19399063" />

Vérifier que disque, RAM et CPU sont corrects avant l’installation.

</details>

---

2️⃣︲**Partitionnement du disque**  
   - 40 Go pour l’OS  
   - 40 Go pour DATA  

<details>
  <summary>📸︲Partitionnement</summary>

<img width="1022" height="772" alt="Screenshot_5" src="https://github.com/user-attachments/assets/3d76f21a-6dca-4641-903a-3f5ddcb6db0f" />

Créer deux partitions : 40 Go pour l’OS et 40 Go pour les données.

</details>

---

3️⃣︲**Installation depuis l’ISO**  
   - Sélectionner langue, clavier et région  

<details>
  <summary>📸︲Sélection ISO serveur</summary>

<img width="1018" height="771" alt="Screenshot_1" src="https://github.com/user-attachments/assets/0a8564ef-ff3f-43d4-ba0c-2fbee5e9de43" />
<img width="1026" height="767" alt="Screenshot_19" src="https://github.com/user-attachments/assets/e36dceae-aa06-4a4b-ba8d-cc3a935823ba" />

Choisir langue Français et clavier pour le serveur.  

</details>

---

4️⃣︲**Sélection de la méthode d’installation**  
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

5️⃣︲**Création du compte administrateur**  
   - Nom : `btssio`  
   - Mot de passe : `btssio-lmc25`  

---

6️⃣︲**Configuration réseau**  
   - IP : `172.16.0.1`  
   - Masque : `255.255.255.0`  

<details>
  <summary>📸︲Paramètres réseau serveur</summary>

<img width="1024" height="769" alt="Screenshot_11" src="https://github.com/user-attachments/assets/d8ecdf27-9dfd-4eb5-8405-c74a969fc1e8" />
Configurer IP et masque pour le serveur.

</details>

---

7️⃣︲**Vérification de l’installation**  
   - Redémarrer et se connecter avec le compte administrateur  

<details>
  <summary>📸︲Vérification finale serveur</summary>

<img width="1027" height="774" alt="Screenshot_30" src="https://github.com/user-attachments/assets/2d4565cd-66e0-454e-be4c-6f002718e385" />
Le serveur est prêt et l’administrateur peut se connecter !

</details>

---

<details>
  <summary><strong>💡︲Conseils pour Windows Server</strong></summary>

  **Vérifiez que la partition `DATA` est correctement créée et détectée après l’installation.**
  
</details>

---

<a id="installation-et-configuration-du-controleur-de-domaine"></a>
### `🏛️`︲Installation et configuration du contrôleur de domaine
---

<a id="installation-roles-ad-ds-et-dns"></a>
### `🔧`︲Installation des rôles AD DS et DNS...

---

1️⃣ ︲**Accéder à l’ajout de rôles et fonctionnalités**

Ouvrez le Gestionnaire de serveur.

Dans le Tableau de bord, cliquez sur Gérer.

Sélectionnez ensuite Ajouter des rôles et des fonctionnalités.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
  <img width="1029" height="773" alt="Screenshot_10" src="https://github.com/user-attachments/assets/b8bcf212-d61f-4e17-b618-cf23f5ad5e82" />

</details>

---

2️⃣︲**Lancer l’assistant d’ajout de rôles et de fonctionnalités**

La fenêtre Assistant Ajout de rôles et de fonctionnalités s’ouvre automatiquement.

Cliquez sur Suivant pour continuer.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
<img width="1027" height="767" alt="Screenshot_1" src="https://github.com/user-attachments/assets/59314f25-34eb-4084-8768-34021fd2179b" />
</details>

---

3️⃣︲**Choisir l'installation basée sur un rôle ou une fonctionnalité**

Dans la fenêtre suivante, sélectionnez Installation basée sur un rôle ou une fonctionnalité.

Vous verrez une liste avec des options sous forme de puces.

Cliquez sur Suivant pour continuer.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
<img width="1028" height="767" alt="Screenshot_2" src="https://github.com/user-attachments/assets/974e4085-45c6-4ebd-8d84-9572d2f404f9" />
</details>

---

4️⃣︲**Choisir le serveur pour l'installation**

L’assistant vous demande ensuite où installer la fonctionnalité.

Cliquez sur Sélectionner un serveur du pool de serveurs.

Dans la liste, choisissez le serveur `SRV-AD1`.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
<img width="1025" height="773" alt="Screenshot_3" src="https://github.com/user-attachments/assets/1f1a653f-6a8f-4934-8ee9-87679b9f353d" />
</details>

---

5️⃣︲**Sélectionner les fonctionnalités à installer**

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

6️⃣︲**Confirmer et lancer l'installation**

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

1️⃣︲**Vérification de l'installation et promotion du serveur**

Une fois l'installation terminée et le serveur redémarré, ouvrez le Gestionnaire de serveur.

Une icône jaune avec un signe de danger/attention devrait apparaître, indiquant qu'une action est nécessaire.

Nous allons maintenant procéder à la promotion du serveur et à la création du domaine descartesbleu.org.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1026" height="768" alt="Screenshot_12" src="https://github.com/user-attachments/assets/a3169f35-e235-4044-a60d-5737daf172f3" />
</details>

---

2️⃣︲**Configurer les services de domaine Active Directory**

La fenêtre de l’assistant Configuration des services de domaine Active Directory apparaît.

Dans cette fenêtre, sélectionnez Ajouter une nouvelle forêt.

Dans le champ de texte, saisissez le nom du domaine : descartesbleu.org.

Cliquez ensuite sur Suivant pour continuer.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1024" height="775" alt="image" src="https://github.com/user-attachments/assets/f0226bab-e78b-4a08-bca4-ff9d8ee1d8ed" />
   </details>

---

3️⃣︲**Configurer les options du contrôleur de domaine**

Dans les options du Contrôleur de domaine, vérifiez que l'option Serveur DNS est cochée. Si ce n'est pas le cas, cochez-la manuellement.

Dans les champs de texte pour les mots de passe, saisissez le mot de passe suivant :
`btssio-lmc25`
(Note : assurez-vous de le saisir deux fois, dans les deux champs de mot de passe).
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
      <img width="1022" height="770" alt="Screenshot_14" src="https://github.com/user-attachments/assets/16c3bef0-54e8-4aba-90a7-ff75ad8f97a3" />
</details>

---

4️⃣︲**Configurer les options DNS et le domaine NetBIOS**

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

5️⃣︲**Vérification des options et lancement de l'installation**

Dans la fenêtre suivante, il vous sera demandé de définir le chemin d’accès pour le stockage des fichiers. Laissez les valeurs par défaut.

Un récapitulatif des options sélectionnées apparaîtra. Relisez attentivement ce récapitulatif pour éviter toute erreur.

Vérifiez bien toutes les options, notamment le nom du domaine et les paramètres du contrôleur de domaine.

Une fois que vous avez effectué toutes les vérifications nécessaires, validez toutes les options et cliquez sur Installer pour lancer l’installation.

Le processus d’installation commencera et le serveur sera promu en tant que contrôleur de domaine.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1029" height="775" alt="Screenshot_17" src="https://github.com/user-attachments/assets/db11bbf5-1537-498c-bd50-5164e187fa33" />
   <img width="1027" height="774" alt="Screenshot_18" src="https://github.com/user-attachments/assets/3483084e-e5ed-4b0c-8839-c367ac750562" />
   <img width="1027" height="771" alt="Screenshot_19" src="https://github.com/user-attachments/assets/fe3f5c3c-b585-4356-9013-9ddd17278323" />
   <img width="1034" height="783" alt="Screenshot_20" src="https://github.com/user-attachments/assets/1357a4a6-6561-47b3-bfc4-7ae523bceef9" />
</details>

---

<a id="administration-de-lannuaire-active-directory"></a>
### `🗂️`︲Administration de l'annuaire Active Directory

---

<a id="simplification-strategie-mots-de-passe"></a>
### `🔑`︲Simplification de la stratégie de mots de passe

---

1️⃣︲**Ouvrir le menu Démarrer** : Cliquez sur le bouton "Démarrer".

Rechercher "Gestion des stratégies de groupe" : Dans le champ de recherche, tapez "`Gestion des stratégies de groupe`".

Lancer l'application : Appuyez sur la touche Entrée pour ouvrir l'outil "`Gestion des stratégies de groupe`".

---

2️⃣︲**Modifiez la politique de groupe par défaut**

1. Ouvrez **Gestion de stratégie de groupe**.
2. Clic droit sur **Default Domain Policy** > **Modifier**.
3. Allez dans : `Configuration ordinateur` → `Stratégies` → `Paramètres Windows` → `Paramètres de sécurité` → `Stratégies de comptes` → `Stratégie de mot de passe`

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici](https://easyfiles.cc/8_5Ii65raSA)

---

3️⃣︲**Configurez les paramètres**

1. Le mot de passe ne doit jamais expirer.
2. La longueur du mot de passe peut être limitée à 5 caractères. 
3. La complexité du mot de passe doit être désactivée.

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici](https://easyfiles.cc/0ECS-OgWETy)

---

 4️⃣︲**Appliquez les modifications**

1. Ouvrez **Invite de commandes** en tant qu'administrateur.
2. Tapez `gpupdate /force` et appuyez sur **Entrée**.
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1023" height="767" alt="image" src="https://github.com/user-attachments/assets/90ac5f74-5b23-4e5f-9c3d-59f67e888815" />
</details>

---

<a id="creation-ou-groupes-utilisateurs"></a>
### `👥`︲Création des OU, groupes et utilisateurs

---

> [!NOTE]
> - Les Unités d'Organisation (UO) permettent de structurer l'entreprise en fonction de ses services et de ses départements. Les noms des UO doivent respecter les règles suivantes : **pas de majuscules ni d’accents**.
> - Référez-vous à l'Annexe 1 pour créer la structure demandée. 
> - À ce stade, **les branches "Stagiaires" et "Salles" ne doivent pas être créées**.

---

### Accéder à la console "Utilisateurs et ordinateurs Active Directory"

Ouvrez la console **Utilisateurs et ordinateurs Active Directory**.
Vous pouvez y accéder via le **Gestionnaire de serveur**, sous **AD DS**. Faites un clic droit sur votre serveur et sélectionnez **Ouvrir**.

#### 2️⃣.1️⃣ Créer l'UO principale (racine)

- 1️⃣. Faites un clic droit sur le domaine `descartesbleu.org`, puis sélectionnez **Nouveau** > **Unité d'organisation**.
- 2️⃣. Donnez un nom à cette UO (Ici, `Centre De Formation Descartes-bleu`).
- 3️⃣. *Optionnel :* Activez l'option **Protéger contre la suppression accidentelle** pour éviter toute suppression accidentelle (disponible depuis Windows Server 2012).

---

#### 2️⃣.2️⃣ Créer les UO secondaires (Services et Bureaux)

1️⃣. Dans l'UO principale, faites un clic droit et sélectionnez **Nouveau** > **Unité d'organisation** pour ajouter des UO pour chaque service (par exemple, `services`, `formateurs`, `administration`, `bureaux`).

**Structure demandée dans le TP** :

 - `🗂️`︲`Service Administratif `
 - `🗂️`︲`Service Scolarité `
 - `🗂️`︲`Formateurs`
 - `🗂️`︲`Service Comptabilité`

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici](https://easyfiles.cc/foSU5ug92ZJ)

---

### Étape 3 : Créer des Groupes d'Utilisateurs
> [!NOTE]
> Les groupes d'utilisateurs permettent de regrouper les utilisateurs ayant des droits similaires. Les **Groupes de sécurité** sont utilisés pour gérer les permissions d'accès aux ressources  partagées.
> L'option **Globale** est recommandée pour les groupes ayant des membres à maintenir quotidiennement.

* 1️⃣. Dans l'UO appropriée (par exemple, dans l'UO `formateurs`), faites un clic droit, puis sélectionnez **Nouveau** > **Groupe**.
* 2️⃣. Dans la fenêtre de création, sélectionnez l'**Étendue** du groupe (par exemple, **Globale**) et le **Type** de groupe (**Sécurité**).
* 3️⃣. Créez les groupes suivants comme affiché ci-dessus.
   *    Par exemple, dans l'UO "Formateur", créez le groupe `GRPFormateur`.
   *    Répétez l'opération pour les autres UOs.
     
---

* `📁`︲Service Administratif ➡️ GRPAdministratif
* `📁`︲Service Scolarité ➡️ GRPScolarité
* `📁`︲Formateurs ➡️ GRPFormateurs
* `📁`︲Service Comptabilité ➡️ GRP Comptabilité
  
  <details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
  <img width="1024" height="768" alt="image" src="https://github.com/user-attachments/assets/719d42af-65ae-4085-9a38-ca09d0d2a27a" />
</details>

---

### Étape 4 : Créer des Utilisateurs

Vous allez maintenant créer les comptes utilisateurs dans les UO respectives, en suivant la politique de nomenclature.

### `📁`︲Service Administratif
- `👤`︲**Patrick SALRIE**
- `👤`︲**Christine ALLONGUI**

### `📁` Service Scolarité
- `👤`︲**Amina ZOUBIER**

### `📁`︲Formateurs
- `👤`︲**Gaëlle CASTEL**
- `👤`︲**Sylvain CHERRIER**
- `👤`︲**Valéry PERRIN**

### `📁`︲Service Comptabilité
- `👤`︲**Cécile LE BOHEC**

---

#### 4️⃣.1️⃣ Créer un utilisateur

* 1️⃣. Dans l'UO voulue, faites un clic droit et sélectionnez **Nouveau** > **Utilisateur**.
* 2️⃣. Remplissez les informations :

* **Nom** et **Prénom** de l'utilisateur.
* **Login** (nom d’ouverture de session) : Format **`prenom.nom`**.
* **Mot de passe** : Format **`prenom.nom`**.

---

#### 4️⃣.2️⃣ Configuration du mot de passe

1️⃣. Dans la fenêtre de définition du mot de passe, configurez les options suivantes :

* **Décocher** : « L'utilisateur doit changer son mot de passe à la prochaine connexion ».
* **Cocher** : « Le mot de passe n’expire jamais ».

2️⃣. Cliquez sur **Terminer** pour créer l'utilisateur.

---

#### 4️⃣.3️⃣ Modifier les propriétés de l'utilisateur (facultatif)

1️⃣. Une fois l'utilisateur créé, vous pouvez modifier ses propriétés (par exemple : bureau, numéro de téléphone, programmes à lancer à l’ouverture de session) en faisant un clic droit sur le compte et en sélectionnant **Propriétés**.

---

### Étape 5 : Ajouter des utilisateurs aux Groupes

Une fois les utilisateurs créés, vous devez les ajouter aux groupes appropriés (par exemple, **formateurs**).

- 1️⃣. Double-cliquez sur le groupe concerné ou faites un clic droit et choisissez **Propriétés**.
- 2️⃣. Dans l'onglet **Membres**, cliquez sur **Ajouter**.
- 3️⃣. Entrez le **login** (ex : `prenom.nom`), ou utilisez **Avancé** pour rechercher les utilisateurs.
- 4️⃣. Cliquez sur **Vérifier les noms** pour valider, puis sur **OK** pour ajouter les utilisateurs au groupe.

---

<a id="integration-dun-client-au-domaine"></a>
### `💻`︲Intégration d'un client au domaine.

---

<a id="configuration-reseau-dns"></a>
### `🌐`︲Configuration réseau et paramètres DNS

---
Avant de commencer, assurez-vous que les conditions suivantes sont remplies :

- 1️⃣. **Nom du domaine**
   Vous devez connaître le **nom exact du domaine** (ex : `descartesbleu.org`).


- 2️⃣. **Adresse IP configurée**
   Le PC doit avoir une **adresse IP fixe** ou bien définie par le DHCP.

- 3️⃣. **DNS pointant vers le contrôleur de domaine**
   Le **serveur DNS** du PC doit être **l’adresse IP du contrôleur de domaine**.

- 4️⃣. **Tester la connectivité réseau**
   Vérifiez que le poste peut **communiquer avec le serveur** :

   * **Ping de l’adresse IP du serveur DNS**

     ```bash
     ping 172.16.0.1
     ```

   * **Ping du nom de domaine complet (FQDN)**

     ```bash
     ping descartesbleu.org
     ```

   * **Vérification DNS avec nslookup**

     ```bash
     nslookup descartesbleu.org
     ```

     Cela doit afficher une réponse avec l’adresse IP du contrôleur de domaine.

---

<a id="joindre-domaine"></a>
### `🔗`︲Joindre le domaine descartesbleu.org

---

### `🖼️`︲Avec l'interface graphique (facile)

- 1️⃣. Faites un clic droit sur **Ce PC** > **Propriétés**.

- 2️⃣. Cliquez sur **Domaine ou groupe de travail**.

- 3️⃣. Dans les **Propriétés système**, cliquez sur **Modifier**.

- 4️⃣. Sélectionnez **Domaine** et entrez le **nom du domaine**.

- 5️⃣. Entrez les **identifiants du compte autorisé**.

- 6️⃣. Redémarrez le PC quand demandé.

<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1023" height="768" alt="image" src="https://github.com/user-attachments/assets/3437a0db-0531-4a54-b5f2-e7a9bdb266ce" />
</details>

---

### `💻`︲Avec PowerShell (plus fiable si changement de GUI)

- 1️⃣. Lancez **PowerShell en tant qu’administrateur**.
- 2️⃣. Tapez la commande suivante (remplacez `nom-du-domaine.local`) :

   ```powershell
   Add-Computer -DomainName nom-du-domaine.local
   ```
   
- 3️⃣. Entrez les **identifiants du compte autorisé**.
- 4️⃣. Redémarrez le PC avec cette commande :

   ```powershell
   Restart-Computer
   ```

- 1️⃣. Une fois redémarré, le PC est membre du domaine.
- 2️⃣. Essayez de vous connecter avec un **compte utilisateur du domaine** (ex : `prenom.nom`).

---

## `📝`︲En résumé

| Étape              | À faire                                           |
| ------------------ | ------------------------------------------------- |
| Prérequis          | Vérifiez édition, IP, DNS, nom de domaine, droits |
| Connectivité       | Testez `ping`, `nslookup` vers le serveur         |
| Méthode GUI        | Interface Windows, simple pour débutants          |
| Méthode PowerShell | Plus rapide, utile pour automatisation            |
| Après redémarrage  | Testez avec un compte domaine                     |

---

<a id="gestion-des-partages-de-fichiers"></a>
### `📁`︲Gestion des partages de fichiers

---

<a id="creation-dossiers-partages"></a>
## `📂`︲Création des dossiers partagés et permissions

- 1️⃣. **Créer un dossier partagé sur le serveur**

   * Exemple : `D:\profils`
   * Faire un clic droit → **Propriétés** → **Partage** → **Partage avancé**
   * Cochez **Partager ce dossier**
   * Nom du partage : `profils$` (le `$` le rend invisible sur le réseau)

- 2️⃣. **Définir les permissions**

   * **Partage** : Donner **Contrôle total** à **Tout le monde**
   * **Sécurité (NTFS)** :

     * Désactiver l’héritage
     * Ajouter **Utilisateurs du domaine** (ou groupe ciblé, ex : `formateurs`)
     * Donner les droits : **Lecture, Écriture, Modification**

<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
     <img width="1024" height="775" alt="image" src="https://github.com/user-attachments/assets/35daa6dc-691c-41b2-a364-b5bf478a76c2" />
     <img width="1026" height="768" alt="image" src="https://github.com/user-attachments/assets/5cfa00c6-3b26-4493-b26e-f2daff572b8a" />
     <img width="1020" height="771" alt="image" src="https://github.com/user-attachments/assets/85281443-0e5c-4f0e-8e6a-9d0b3c5fd8a5" />
</details>

---

<a id="redirection-dossiers-mode-hors-ligne"></a>
## `💾`︲Redirection des dossiers utilisateur et mode hors connexion

- 1️⃣. **Créer une GPO**

   * Dans la console **Gestion des stratégies de groupe (GPMC)**
   * Clic droit sur l’OU des utilisateurs (ex : `formateurs`) → **Créer une GPO et la lier ici**

- 2️⃣. **Configurer la redirection des dossiers**

   * Éditer la GPO → Aller dans :
     `Configuration utilisateur > Stratégies > Paramètres Windows > Redirection de dossiers`

   * Pour chaque dossier à rediriger (**Documents**, **Bureau**, etc.) :

     * Clic droit → **Propriétés**
     * Onglet **Cible** :

       * Type : **De base**
       * Emplacement : **Rediriger vers l’emplacement suivant**
       * Chemin UNC : `\\SRV-AD1\profils$\%USERNAME%\Documents`
     * Onglet **Paramètres** :

       * Décocher **Accorder à l’utilisateur des droits exclusifs**

- 3️⃣. **Appliquer la stratégie**

   * Sur les postes clients : ouvrir une invite de commande et exécuter :

     ```
     gpupdate /force
     ```
     
> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici](https://easyfiles.cc/Oxus8427i7P)
---

## `🔄` ︲Activer le **mode Hors-Connexion**

Ce mode permet à l’utilisateur d’accéder à ses fichiers redirigés même sans connexion réseau.

- 1️⃣. **Configurer dans la GPO (Configuration ordinateur)**

   * Aller dans :
     `Configuration ordinateur > Stratégies > Modèles d’administration > Réseau > Fichiers hors connexion`
     
- 2️⃣.Cliquez sur `Autoriser ou interdire l'utilisation de la fonctionalité de fihiers hors connexion`

- 3️⃣. Cliquez sur **Activer** à gauche. Apliquer - OK.
     
<details>
  <summary><strong>💡︲Captures d'écran</strong></summary>
   <img width="1024" height="770" alt="image" src="https://github.com/user-attachments/assets/11530e18-3e7e-4e47-8854-4779ae765612" />
</details>

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici](https://easyfiles.cc/7kZ-EWXAKOd)
   
---

<a id="automatisation-via-powershell"></a>
### `📜`︲Automatisation via PowerShell

---
<a id="script-ou-groupes-utilisateurs"></a>
### `⚡`︲Script pour créer des UOs, groupes et utilisateurs à partir d'un .CSV
---

> [!NOTE]
> Voici le script que j'ai utilisé pour la procédure. Il suffit de le copier-coller dans la VM ; il devrait fonctionner sans problème. Assurez-vous toutefois que le chemin vers le fichier CSV 
> est correct, sinon le script ne pourra pas trouver les entrées et échouera.

```Shell
# Importation du module Active Directory (si nécessaire)
Import-Module ActiveDirectory

# Définition des chemins
$DomaineDN = "DC=descartesbleu,DC=org"
$OU_Parent = "OU=stagiaires,$DomaineDN"
$OU_Child = "stagiaires_sisr"

# 1. Création de l'OU 'stagiaires_sisr'
New-ADOrganizationalUnit -Name $OU_Child -Path $OU_Parent -ProtectedFromAccidentalDeletion $false
Write-Host "OU '$OU_Child' créée avec succès dans '$OU_Parent'." -ForegroundColor Cyan


# Définition du chemin de l'OU "stagiaires_sisr"
$OU_Path = "OU=$OU_Child," + $OU_Parent

# Nom du groupe à créer
$GroupName = "grp_stagiaires_sisr"

# Création du groupe
New-ADGroup -Name $GroupName `
            -GroupScope Global `
            -GroupCategory Security `
            -Path $OU_Path `
            -Description "Groupe de sécurité global pour les stagiaires SISR"
    
Write-Host "Le groupe '$GroupName' a été créé avec succès dans l'OU 'stagiaires_sisr'." -ForegroundColor Cyan

# Définition des constantes
$OU_Path = "OU=stagiaires_sisr,OU=stagiaires,DC=descartesbleu,DC=org"
$GroupeNom = "grp_stagiaires_sisr"
$CsvPath = "C:\Users\Administrateur\Documents\script\stagiaires_sisr.csv"


# Chemin de l'OU et nom du groupe
$OU_Path = "OU=stagiaires_sisr,OU=stagiaires,DC=descartesbleu,DC=org"
$GroupeNom = "grp_stagiaires_sisr"

# Lecture du fichier CSV (délimité par ;)
$Utilisateurs = Import-Csv -Path $CsvPath -Delimiter ';'

foreach ($user in $Utilisateurs) {
    $Prenom = $user.prenom
    $Nom = $user.nom
    $Mail = $user.mail
    $MotDePasse = $user.motdepasse

    # Génération de l'identifiant (samAccountName) à partir du prénom et du nom
    $Identifiant = ($Prenom + "." + $Nom).ToLower()

    # Création de l'utilisateur
    New-ADUser `
        -Name "$Prenom $Nom" `
        -GivenName $Prenom `
        -Surname $Nom `
        -SamAccountName $Identifiant `
        -UserPrincipalName $Mail `
        -EmailAddress $Mail `
        -AccountPassword (ConvertTo-SecureString $MotDePasse -AsPlainText -Force) `
        -Enabled $true `
        -Path $OU_Path `
        -ChangePasswordAtLogon $false `
        -PasswordNeverExpires $true


    # Ajout de l'utilisateur au groupe
    Add-ADGroupMember -Identity $GroupeNom -Members $Identifiant

    # Affichage
    Write-Host "Utilisateur $Identifiant créé et ajouté au groupe $GroupeNom." -ForegroundColor Green
}
```
<details>
  <summary><strong>💡︲Instructions pour l’élaboration de ce script, accompagnées de sa documentation.</strong></summary>
   Voici une documentation pour ton fichier README, qui décrit chaque étape du script et son fonctionnement. J'ai essayé d'être aussi clair et détaillé que possible pour que toute personne qui l'utilise puisse comprendre ce qu'il fait.

---

## ︲Script PowerShell pour la gestion des stagiaires SISR dans Active Directory

Ce script PowerShell est conçu pour automatiser la création d'une **Unité Organisationnelle (OU)** et d'un **groupe de sécurité** dans Active Directory, ainsi que la gestion des utilisateurs stagiaires dans l'OU spécifiée.

### ︲Prérequis

* **PowerShell** : Ce script doit être exécuté sur une machine ayant PowerShell installé.
* **Module Active Directory** : Le module Active Directory doit être installé et disponible. Il peut être importé avec `Import-Module ActiveDirectory`.
* **Accès administrateur** : Vous devez avoir des droits administratifs sur le domaine Active Directory pour exécuter ce script.

### Description du script

1. **Création d'une Unité Organisationnelle (OU)**
   Le script crée une nouvelle OU nommée `stagiaires_sisr` sous une OU parente nommée `stagiaires`, dans le domaine `descartesbleu.org`.

2. **Création d'un Groupe de Sécurité Global**
   Un groupe de sécurité global appelé `grp_stagiaires_sisr` est créé dans l'OU `stagiaires_sisr`.

3. **Lecture d'un fichier CSV**
   Le script lit un fichier CSV contenant les informations des stagiaires (prénom, nom, adresse e-mail et mot de passe) et crée un utilisateur pour chaque stagiaire dans l'OU `stagiaires_sisr`.

4. **Création des utilisateurs et ajout au groupe**
   Pour chaque stagiaire, un utilisateur est créé dans Active Directory, et l'utilisateur est ajouté au groupe `grp_stagiaires_sisr`.

### ︲Structure du script

#### 1. Importation du module Active Directory

```Shell
Import-Module ActiveDirectory
```

Cette ligne importe le module Active Directory, nécessaire pour interagir avec le domaine Active Directory.

#### 2. Définition des chemins et des variables

Le script commence par définir des variables essentielles telles que :

* **$DomaineDN** : Le nom du domaine (ici `descartesbleu.org`).
* **$OU_Parent** : L'OU parente sous laquelle l'OU `stagiaires_sisr` sera créée.
* **$OU_Child** : Le nom de l'OU enfant à créer (`stagiaires_sisr`).
* **$OU_Path** : Le chemin complet de l'OU.
* **$GroupName** : Le nom du groupe de sécurité à créer.

#### 3. Création de l'OU `stagiaires_sisr`

```Shell
New-ADOrganizationalUnit -Name $OU_Child -Path $OU_Parent -ProtectedFromAccidentalDeletion $false
Write-Host "OU '$OU_Child' créée avec succès dans '$OU_Parent.'"
```

Cette section crée l'OU `stagiaires_sisr` sous l'OU parent `stagiaires` dans le domaine spécifié.

#### 4. Création du groupe de sécurité

```Shell
New-ADGroup -Name $GroupName -GroupScope Global -GroupCategory Security -Path $OU_Path -Description "Groupe de sécurité global pour les stagiaires SISR"
Write-Host "Le groupe '$GroupName' a été créé avec succès."
```

Le script crée ensuite un groupe de sécurité global dans l'OU `stagiaires_sisr`. Ce groupe sera utilisé pour regrouper tous les stagiaires dans Active Directory.

#### 5. Lecture du fichier CSV et création des utilisateurs

```Shell
$Utilisateurs = Import-Csv -Path $CsvPath -Delimiter ';'

foreach ($user in $Utilisateurs) {
    $Prenom = $user.prenom
    $Nom = $user.nom
    $Mail = $user.mail
    $MotDePasse = $user.motdepasse
    $Identifiant = ($Prenom + "." + $Nom).ToLower()

    New-ADUser -Name "$Prenom $Nom" -GivenName $Prenom -Surname $Nom -SamAccountName $Identifiant -UserPrincipalName $Mail -EmailAddress $Mail -AccountPassword (ConvertTo-SecureString $MotDePasse -AsPlainText -Force) -Enabled $true -Path $OU_Path -ChangePasswordAtLogon $false -PasswordNeverExpires $true
    Add-ADGroupMember -Identity $GroupeNom -Members $Identifiant
    Write-Host "Utilisateur $Identifiant créé et ajouté au groupe $GroupeNom."
}
```

Cette section lit un fichier CSV (`stagiaires_sisr.csv`), où chaque ligne contient des informations sur un stagiaire (prénom, nom, email, mot de passe). Pour chaque stagiaire :

* Un utilisateur est créé dans Active Directory avec un **samAccountName** composé du prénom et du nom (en minuscules).
* L'utilisateur est ajouté au groupe de sécurité `grp_stagiaires_sisr`.

Les informations de mot de passe sont converties en texte sécurisé.

#### 6. Résultats

À chaque création d'utilisateur et ajout au groupe, un message de confirmation est affiché à l'écran avec les détails de l'utilisateur créé.

### ︲Format du fichier CSV

Le fichier CSV utilisé pour importer les stagiaires doit être formaté comme suit :

```csv
prenom;nom;mail;motdepasse
Jean;Dupont;jean.dupont@descartesbleu.org;MotDePasse123
Marie;Lemoine;marie.lemoine@descartesbleu.org;MotDePasse456
```

Assurez-vous que les colonnes sont séparées par des points-virgules (`;`).

### ︲Variables et personnalisations

Voici un aperçu des variables que vous pouvez modifier dans le script pour l'adapter à votre environnement :

* **$DomaineDN** : Modifiez ce champ pour correspondre à votre domaine.
* **$OU_Parent** et **$OU_Child** : Changez ces valeurs si vous souhaitez créer l'OU dans une autre structure.
* **$CsvPath** : Assurez-vous que le chemin du fichier CSV est correct pour votre environnement.

### ︲Sécurité et bonnes pratiques

* **Mot de passe** : Le mot de passe des utilisateurs est défini comme *"motdepasse"* dans le fichier CSV. Assurez-vous de configurer un mot de passe sécurisé pour chaque stagiaire.
* **Protection contre les suppressions accidentelles** : L'OU `stagiaires_sisr` est créée sans protection contre la suppression accidentelle. Vous pouvez modifier cela en ajustant le paramètre `$ProtectedFromAccidentalDeletion`.
</details>

---

## `📝`︲**Utilisation du script**

> [!IMPORTANT]
> * Assurez-vous d’avoir :
> * Un serveur **Active Directory** avec des **droits d’administrateur**.
> * Le module PowerShell **ActiveDirectory** installé.
> * Un compte avec le droit de **créer des utilisateurs** dans AD (ex. : administrateur de domaine).

---

### 1. `📄`︲**Fichier .CSV requis**

Le script utilise un fichier CSV pour créer les comptes utilisateurs.

```csv
nom;prenom;mail;motdepasse
ANASSI;Cedric;cedric.anassi@descartesbleu.org;cedric.anassi
BARSACQ;Hugo;hugo.barsacq@descartesbleu.org;hugo.barsacq
BESSE;Lucas;lucas.besse@descartesbleu.org;lucas.besse
CAMPAN;Anthony;anthony.campan@descartesbleu.org;anthony.campan
CHABROL;Thomas;thomas.chabrol@descartesbleu.org;thomas.chabrol
COURDIL;Theo;theo.courdil@descartesbleu.org;theo.courdil
LALANNE;Andoni;andoni.lalanne@descartesbleu.org;andoni.lalanne
LAMARQUE;Andony;andony.lamarque@descartesbleu.org;andony.lamarque
LOTZ;Bryan;bryan.lotz@descartesbleu.org;bryan.lotz
MORALES;Enaut;enaut.morales@descartesbleu.org;enaut.morales
PAVE;Mathilde;mathilde.pave@descartesbleu.org;mathilde.pave
TEARIKI;Virgile;virgile.teariki@descartesbleu.org;virgile.teariki

```

> [!WARNING]
> Les mots de passe sont visibles (en clair). Ne laissez pas ce fichier accessible à tout le monde.

---

### 2. `📁`︲**Où placer le fichier .CSV**

Le fichier .CSV doit être à l’emplacement suivant (ou à adapter dans le script) :

```powershell
$CsvPath = "C:\Users\Administrateur\Documents\script\stagiaires_sisr.csv"
```

---

### 3. `▶️`︲**Lancer le script**

1. **Ouvrez PowerShell ISE en mode administrateur**.
2. Lancez le script.

Le script va automatiquement :

* Créer une unité d’organisation : `OU=stagiaires_sisr` dans `OU=stagiaires`.
* Créer un **groupe de sécurité** : `grp_stagiaires_sisr`.
* Lire chaque ligne du fichier .CSV.
* Créer un utilisateur pour chaque personne.
* Ajouter chaque utilisateur au groupe.

> [!IMPORTANT]
> Sur la vidéo de démonstration, le script avait déja été executer une fois ce qui explique les mesage d'erreur sur la vidéo, dans votre cas si c'est la première fois que vous le lancer il n'y > aura rien, juste des messages en vert pour confimer la création des utilisateur avec succés.

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici](https://easyfiles.cc/ODeJNl2AZFQ)

---

<a id="deploiement-de-strategies-de-groupe"></a>
### `🖱️`︲Déploiement de stratégies de groupe (GPO)

---










