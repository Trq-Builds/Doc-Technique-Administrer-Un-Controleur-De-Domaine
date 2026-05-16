# ` 🦺 `︲Documentation TP : Administrer un contrôleur de domaine.

<p align="center">
  <img src="https://img.shields.io/badge/Windows_Server-2025-0078D6?logo=windows&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/Role-Domain_Controller-0056B3?style=for-the-badge">
  <img src="https://img.shields.io/badge/Service-Active_Directory-8A2BE2?style=for-the-badge">
  <img src="https://img.shields.io/badge/Feature-Group_Policy-FFA500?style=for-the-badge">
  <img src="https://img.shields.io/badge/Markdown-made%20with-2B7489?logo=markdown&style=for-the-badge">
</p>

---

Ce dépôt présente un guide complet pour administrer un contrôleur de domaine sous Windows Server 2025. Il couvre l'installation, la configuration et l'administration d'un environnement Active Directory. Tu y apprendras à déployer un domaine, gérer des utilisateurs et des GPO, automatiser des tâches via PowerShell, et appliquer des restrictions avancées !

---

## `📑`︲Sommaire (cliquez pour accéder directement à la section souhaitée)

1. [`📘`︲Introduction.](#introduction)

   * [`❔`︲Contexte et objectifs du TP.](#contexte-et-objectifs)
   * [`🧰`︲Présentation de l'architecture réseau et des outils.](#presentation-outils)

   ---

2. [`🛠️`︲Préparation de l'environnement.](#preparation-environnement)

   * [`💿`︲Installation de Windows 11 (client).](#installation-windows-client)
   * [`💿`︲Installation de Windows Server 2025 (serveur).](#installation-windows-server)

   ---

3. [`🏛️`︲Installation et configuration du contrôleur de domaine.](#installation-dc)

   * [`🔧`︲Installation des rôles AD DS et DNS.](#installation-roles)
   * [`🌐`︲Promotion du serveur et création du domaine descartesbleu.org.](#promotion-domaine)

   ---

4. [`🗂️`︲Administration de l'annuaire Active Directory.](#administration-ad)

   * [`🔑`︲Simplification de la stratégie de mots de passe.](#strategie-mdp)
   * [`👥`︲Création des UOs, groupes et utilisateurs.](#creation-ou-groupes-users)

   ---

5. [`💻`︲Intégration d'un client au domaine.](#integration-client)

   * [`🌐`︲Configuration réseau et paramètres DNS.](#config-reseau-dns)
   * [`🔗`︲Joindre le domaine descartesbleu.org.](#joindre-domaine)

   ---

6. [`📁`︲Gestion des partages de fichiers.](#partages-fichiers)

   * [`📂`︲Création des dossiers partagés et permissions.](#dossiers-partages)
   * [`💾`︲Redirection des dossiers utilisateur et mode hors connexion.](#redirection-hors-connexion)

   ---

7. [`📜`︲Automatisation via PowerShell.](#automatisation-powershell)

   * [`⚡`︲Script pour créer des UOs, groupes et utilisateurs depuis un fichier `.CSV`.](#script-csv)

   ---

8. [`🖱️`︲Déploiement de stratégies de groupe (GPO).](#deploiement-gpo)

   * [`📂`︲Redirection Documents, mappage de lecteurs et déploiement Firefox.](#redirection-mappage-firefox)

   ---

9. [`🔒`︲Restrictions et fonctionnalités avancées.](#restrictions-avancees)

   * [`⏱️`︲Horaires de connexion, Bureau à distance et BgInfo.](#horaires-rdp-bginfo)

   ---

10. [`✅`︲Conclusion et Annexes.](#conclusion)
11. [`🧰`︲Outils et ressources utilisées.](#outils-ressources)

---

> [!TIP]
> **Pour naviguer rapidement dans le dépôt, vous pouvez également utiliser le sommaire généré automatiquement par GitHub, situé en haut à droite de ce cadre !**

---

> [!IMPORTANT]
> Les vidéos de démonstration sont hébergées sur `dona.one`.
> En cas d'indisponibilité, un miroir est disponible sur **Mega.nz** (lien alternatif fourni à chaque étape).

---

<a id="introduction"></a>
# `📘`︲Introduction.

---

<a id="contexte-et-objectifs"></a>
### `❔`︲Contexte et objectifs du TP.

> [!NOTE]
> Tu vas apprendre à configurer un domaine Active Directory, comprendre le rôle d'un contrôleur de domaine, gérer efficacement les utilisateurs et les groupes, appliquer des stratégies de groupe (GPO) et automatiser certaines tâches courantes grâce à PowerShell.
> L'objectif est de te permettre de mettre en place un environnement réseau fonctionnel et de maîtriser les bases essentielles de l'administration système dans un contexte professionnel.

---

<a id="presentation-outils"></a>
### `🧰`︲Présentation de l'architecture réseau et des outils.

> [!IMPORTANT]
> **Présentation des outils et prérequis :**
>
> - ` 🖥️ `︲**Serveur :** *Windows Server 2025* ︲[`🌐`](https://www.microsoft.com/fr-fr/windows-server)
>
> - ` 💻 `︲**Client :** *Windows 11* ︲[`🌐`](https://www.microsoft.com/fr-fr/software-download/windows11)
>
> - ` 🏛️ `︲**Rôle :** *Active Directory Domain Services (AD DS)*
>
> - ` 🌐 `︲**Service :** *DNS intégré au contrôleur de domaine*
>
> - ` 📜 `︲**Automatisation :** *PowerShell*
>
> - ` 🖱️ `︲**Stratégies :** *Group Policy Objects (GPO)*
>
> - ` 📦 `︲**VMWare :** ︲[`🌐`](https://www.vmware.com/)
>
> - ` 👤 `︲**Interface Chaise-Clavier fonctionnelle.** 🫵
>
> - ` ☕ `︲**Un peu de patience !**

---

<a id="preparation-environnement"></a>
# ` 🛠️ `︲Préparation de l'environnement.

---

<a id="installation-windows-client"></a>
## ` 💿 `︲Installation de Windows 11 (client).

> [!IMPORTANT]
> * **Les captures d'écran seront affichées dans les menus déroulants ci-dessous.**
> * **Si une image est peu lisible, cliquez dessus : elle s'ouvrira en taille réelle dans un nouvel onglet.**

> [!TIP]
> - **Pour afficher les captures d'écran, clique sur le menu déroulant avec l'émoji : `  📸  `.**
> - **Le menu s'ouvrira et affichera la ou les captures d'écran !**

---

` ⚙️ `︲**Configuration de la VM.**

* ` 💾 `︲**Disque :** `80 Go`.
* ` 🧠 `︲**Mémoire :** `4 Go (4096 Mo)`.
* ` ❤️ `︲**Cœurs :** `2`.

<details>
  <summary>📸︲Configuration initiale (VMware).</summary>

<img width="761" height="733" alt="Screenshot_29" src="https://github.com/user-attachments/assets/8e838f92-9bf5-445a-b6e1-61ea1c5d9e1b" />

Sur cette capture, on peut voir la **configuration de la mémoire de la VM sous VMware**.
Il faut régler la mémoire à **4096 Mo (4 Go)**, soit en utilisant le curseur, soit en entrant la valeur manuellement.
Cliquer sur **OK** pour valider.

</details>

> [!WARNING]
> **Prends un snapshot de ta VM après validation de cette configuration.**

---

1️⃣︲**Installation depuis l'ISO.**

* Sélectionner `Français` comme langue.
* Choisir le clavier **Français (Traditionnel, AZERTY)**.
* Choisir *Installer Windows 11* et cocher la suppression de tous les fichiers, applications et paramètres.

<details>
  <summary>📸︲Sélection du clavier et méthode d'installation.</summary>

<img width="1022" height="769" alt="Screenshot_2" src="https://github.com/user-attachments/assets/4013d7fe-1cf0-4e5c-8d7d-b4cf663a85e1" />

Vérifier que la méthode d'entrée est **Français (Traditionnel, AZERTY)** avant de cliquer sur *Suivant*.

---

<img width="1026" height="771" alt="Screenshot_3" src="https://github.com/user-attachments/assets/4b8cf19c-df8b-443c-9127-bc6d3805b8a7" />

Choisir *Installer Windows 11* et cocher la suppression de tous les fichiers avant de cliquer sur *Suivant*.

</details>

---

2️⃣︲**Création de l'utilisateur local.**

Créer le compte avec les identifiants suivants :

```
Nom d'utilisateur : btssio
Mot de passe      : btssio
```

<details>
  <summary>📸︲Création de l'utilisateur.</summary>

<img width="1022" height="769" alt="Screenshot_11" src="https://github.com/user-attachments/assets/603eca66-704a-4aa0-8b73-7ed9f5db21c1" />

Entrer le **nom d'utilisateur `btssio`**, cliquer sur **Suivant**.

---

<img width="1024" height="770" alt="Screenshot_13" src="https://github.com/user-attachments/assets/73800d3f-d047-4310-91e1-c5b03380349b" />

Entrer le **mot de passe `btssio`** et confirmer.

</details>

<details>
  <summary>📸︲Choix OOBE (optionnel).</summary>

<img width="1026" height="770" alt="Screenshot_18" src="https://github.com/user-attachments/assets/4004e27f-c2c2-46b7-9460-b3ddda233c92" />
<img width="1022" height="771" alt="Screenshot_17" src="https://github.com/user-attachments/assets/720c73cd-2ad4-465e-b58a-ca5906f895f3" />
<img width="1027" height="771" alt="Screenshot_16" src="https://github.com/user-attachments/assets/592a58d9-d7e7-4497-b808-62d184f0e42f" />
<img width="994" height="771" alt="Screenshot_15" src="https://github.com/user-attachments/assets/6cd89070-4682-46e5-9c8d-714d89b30ec8" />
<img width="1026" height="770" alt="Screenshot_14" src="https://github.com/user-attachments/assets/3ac9e60f-a7db-4ad0-ba02-7af20f2f2ee9" />

</details>

---

<a id="installation-windows-server"></a>
## ` 💿 `︲Installation de Windows Server 2025 (serveur).

---

> [!NOTE]
> Cette partie couvre **l'installation de Windows Server 2025**.
> Objectif : obtenir un serveur stable, prêt à être promu contrôleur de domaine !

---

` ⚙️ `︲**Configuration de la VM.**

* ` 💾 `︲**Disque :** `80 Go` (2 partitions : 40 Go OS + 40 Go DATA).
* ` 🧠 `︲**Mémoire :** `2 Go`.
* ` ❤️ `︲**Cœurs :** `2`.

<details>
  <summary>📸︲Configuration initiale serveur.</summary>

<img width="759" height="731" alt="Screenshot_31" src="https://github.com/user-attachments/assets/09f78ba3-4e73-4579-b685-746b19399063" />

Vérifier que disque, RAM et CPU sont corrects avant l'installation.

</details>

> [!WARNING]
> **Prends un snapshot de ta VM après validation de cette configuration.**

---

1️⃣︲**Installation depuis l'ISO.**

* Sélectionner langue **Français** et clavier **AZERTY**.
* Méthode d'installation : **Installation personnalisée** (Custom Install).
* Entrer la **clé produit** puis sélectionner l'image : **Windows Server 2025 Standard (expérience utilisateur)**.

<details>
  <summary>📸︲Sélection ISO, méthode d'installation et image.</summary>

<img width="1018" height="771" alt="Screenshot_1" src="https://github.com/user-attachments/assets/0a8564ef-ff3f-43d4-ba0c-2fbee5e9de43" />
<img width="1026" height="767" alt="Screenshot_19" src="https://github.com/user-attachments/assets/e36dceae-aa06-4a4b-ba8d-cc3a935823ba" />

---

<img width="1026" height="774" alt="Screenshot_2" src="https://github.com/user-attachments/assets/dff9d49b-90ce-418f-bf61-931849ae3b6b" />

Vérifier la méthode d'installation et entrer la clé produit.

---

<img width="1022" height="773" alt="Screenshot_4" src="https://github.com/user-attachments/assets/f96bac0c-f279-4a23-b65e-b9f92ebd888d" />

Sélectionner : **Windows Server 2025 Standard (expérience utilisateur)**.

</details>

---

2️⃣︲**Partitionnement du disque.**

* `40 Go` → Partition OS.
* `40 Go` → Partition DATA.

<details>
  <summary>📸︲Partitionnement.</summary>

<img width="1022" height="772" alt="Screenshot_5" src="https://github.com/user-attachments/assets/3d76f21a-6dca-4641-903a-3f5ddcb6db0f" />

Créer deux partitions : 40 Go pour l'OS et 40 Go pour les données.

</details>

---

3️⃣︲**Création du compte administrateur.**

```
Nom       : Administrateur
Mot de passe : btssio-lmc25
```

> [!IMPORTANT]
> **Enregistrer le couple login/mot de passe (`Administrateur / btssio-lmc25`) dans la description de la VM.**

---

4️⃣︲**Configuration réseau.**

* **IP :** `172.16.0.1`
* **Masque :** `255.255.255.0`

<details>
  <summary>📸︲Paramètres réseau serveur.</summary>

<img width="1024" height="769" alt="Screenshot_11" src="https://github.com/user-attachments/assets/d8ecdf27-9dfd-4eb5-8405-c74a969fc1e8" />

Configurer IP et masque pour le serveur.

</details>

> [!WARNING]
> **Prends un snapshot de ta VM après validation de cette configuration.**

---

5️⃣︲**Vérification de l'installation.**

* Redémarrer et se connecter avec le compte administrateur.

<details>
  <summary>📸︲Vérification finale serveur.</summary>

<img width="1027" height="774" alt="Screenshot_30" src="https://github.com/user-attachments/assets/2d4565cd-66e0-454e-be4c-6f002718e385" />

Le serveur est prêt et l'administrateur peut se connecter !

</details>

> [!TIP]
> **Vérifiez que la partition `DATA` est correctement créée et détectée après l'installation.**

---

<a id="installation-dc"></a>
# ` 🏛️ `︲Installation et configuration du contrôleur de domaine.

---

<a id="installation-roles"></a>
## ` 🔧 `︲Installation des rôles AD DS et DNS.

---

> [!NOTE]
> L'installation s'effectue via le **Gestionnaire de serveur**.
> Les rôles **AD DS** et **DNS** sont installés ensemble : le DNS est indispensable au fonctionnement d'Active Directory.

---

1️⃣︲**Accéder à l'ajout de rôles et fonctionnalités.**

* Ouvrir le **Gestionnaire de serveur**.
* Dans le Tableau de bord → cliquer sur **Gérer** → **Ajouter des rôles et des fonctionnalités**.

<details>
  <summary>📸︲Gestionnaire de serveur.</summary>

<img width="1029" height="773" alt="Screenshot_10" src="https://github.com/user-attachments/assets/b8bcf212-d61f-4e17-b618-cf23f5ad5e82" />

</details>

---

2️⃣︲**Lancer l'assistant.**

* Sélectionner **Installation basée sur un rôle ou une fonctionnalité** → cliquer sur **Suivant**.

<details>
  <summary>📸︲Assistant d'ajout de rôles.</summary>

<img width="1027" height="767" alt="Screenshot_1" src="https://github.com/user-attachments/assets/59314f25-34eb-4084-8768-34021fd2179b" />

---

<img width="1028" height="767" alt="Screenshot_2" src="https://github.com/user-attachments/assets/974e4085-45c6-4ebd-8d84-9572d2f404f9" />

</details>

---

3️⃣︲**Sélectionner le serveur cible.**

* Choisir **Sélectionner un serveur du pool de serveurs** → sélectionner `SRV-AD1`.

<details>
  <summary>📸︲Sélection du serveur.</summary>

<img width="1025" height="773" alt="Screenshot_3" src="https://github.com/user-attachments/assets/1f1a653f-6a8f-4934-8ee9-87679b9f353d" />

</details>

---

4️⃣︲**Sélectionner les rôles à installer.**

* Cocher **Services de domaine Active Directory (AD DS)**.
* Une fenêtre s'ouvre : cocher **Inclure les outils de gestion** (coché par défaut) → cliquer sur **Ajouter des fonctionnalités** → **Suivant**.

<details>
  <summary>📸︲Sélection du rôle AD DS.</summary>

<img width="1025" height="773" alt="Screenshot_4" src="https://github.com/user-attachments/assets/1d7493b3-89ad-4107-b894-4e9979339b02" />

---

<img width="1026" height="771" alt="Screenshot_5" src="https://github.com/user-attachments/assets/4b8aab9d-6982-4250-94f9-b055739aa7f0" />

</details>

---

5️⃣︲**Confirmer et lancer l'installation.**

* Vérifier les options → cliquer sur **Installer**.
* L'installation prendra quelques minutes.
* Une fois terminée, un **redémarrage est nécessaire**.

<details>
  <summary>📸︲Installation en cours.</summary>

<img width="1024" height="770" alt="Screenshot_7" src="https://github.com/user-attachments/assets/afa101e0-3f52-4df5-92df-7f4ea79750fb" />
<img width="1025" height="771" alt="Screenshot_8" src="https://github.com/user-attachments/assets/624efe48-fd2b-4425-848f-23b69dff1f2a" />
<img width="1027" height="770" alt="Screenshot_9" src="https://github.com/user-attachments/assets/1b5b2339-2f70-4f99-809f-c5bf919d4da6" />

</details>

---

<a id="promotion-domaine"></a>
## ` 🌐 `︲Promotion du serveur et création du domaine descartesbleu.org.

---

> [!NOTE]
> Une fois le rôle AD DS installé, le serveur doit être **promu contrôleur de domaine** pour que l'Active Directory soit opérationnel.

---

1️⃣︲**Détecter la notification de promotion.**

* Ouvrir le **Gestionnaire de serveur** après redémarrage.
* Une icône d'alerte jaune apparaît : cliquer dessus → **Promouvoir ce serveur en contrôleur de domaine**.

<details>
  <summary>📸︲Notification de promotion.</summary>

<img width="1026" height="768" alt="Screenshot_12" src="https://github.com/user-attachments/assets/a3169f35-e235-4044-a60d-5737daf172f3" />

</details>

---

2️⃣︲**Configurer la nouvelle forêt.**

* Sélectionner **Ajouter une nouvelle forêt**.
* Nom du domaine racine : `descartesbleu.org` → cliquer sur **Suivant**.

<details>
  <summary>📸︲Création de la forêt.</summary>

<img width="1024" height="775" alt="image" src="https://github.com/user-attachments/assets/f0226bab-e78b-4a08-bca4-ff9d8ee1d8ed" />

</details>

---

3️⃣︲**Configurer les options du contrôleur de domaine.**

* Vérifier que l'option **Serveur DNS** est cochée.
* Mot de passe DSRM : `btssio-lmc25` (saisir deux fois).

<details>
  <summary>📸︲Options du contrôleur de domaine.</summary>

<img width="1022" height="770" alt="Screenshot_14" src="https://github.com/user-attachments/assets/16c3bef0-54e8-4aba-90a7-ff75ad8f97a3" />

</details>

---

4️⃣︲**Configurer les options DNS et le nom NetBIOS.**

* Passer les options DNS par défaut → **Suivant**.
* Nom de domaine NetBIOS : `DESCARTESBLEU` (se remplit automatiquement en général).

<details>
  <summary>📸︲Options DNS et NetBIOS.</summary>

<img width="1025" height="770" alt="Screenshot_15" src="https://github.com/user-attachments/assets/0cb4593f-715b-4e5e-80d9-06db2f61c9b1" />
<img width="1024" height="776" alt="Screenshot_16" src="https://github.com/user-attachments/assets/f9e62c9d-7c22-4b1a-830c-bd15b337df2f" />

</details>

---

5️⃣︲**Vérifier et lancer l'installation.**

* Laisser les chemins par défaut pour la base de données AD.
* Vérifier le récapitulatif attentivement → cliquer sur **Installer**.
* Le serveur redémarre automatiquement à la fin.

<details>
  <summary>📸︲Récapitulatif et installation.</summary>

<img width="1029" height="775" alt="Screenshot_17" src="https://github.com/user-attachments/assets/db11bbf5-1537-498c-bd50-5164e187fa33" />
<img width="1027" height="774" alt="Screenshot_18" src="https://github.com/user-attachments/assets/3483084e-e5ed-4b0c-8839-c367ac750562" />
<img width="1027" height="771" alt="Screenshot_19" src="https://github.com/user-attachments/assets/fe3f5c3c-b585-4356-9013-9ddd17278323" />
<img width="1034" height="783" alt="Screenshot_20" src="https://github.com/user-attachments/assets/1357a4a6-6561-47b3-bfc4-7ae523bceef9" />

</details>

> [!TIP]
> À ce stade, ton serveur est **promu contrôleur de domaine** et le domaine `descartesbleu.org` est opérationnel !
> Tu peux maintenant passer à l'administration de l'annuaire.

---

<a id="administration-ad"></a>
# ` 🗂️ `︲Administration de l'annuaire Active Directory.

---

<a id="strategie-mdp"></a>
## ` 🔑 `︲Simplification de la stratégie de mots de passe.

---

> [!NOTE]
> Par défaut, Windows Server impose une politique de mot de passe stricte.
> Dans le cadre du TP, on la simplifie pour faciliter la création des comptes utilisateurs.

---

1️⃣︲**Ouvrir la Gestion des stratégies de groupe.**

* Cliquer sur **Démarrer** → taper `Gestion des stratégies de groupe` → **Entrée**.

---

2️⃣︲**Modifier la politique de groupe par défaut.**

* Clic droit sur **Default Domain Policy** → **Modifier**.
* Naviguer jusqu'à : `Configuration ordinateur → Stratégies → Paramètres Windows → Paramètres de sécurité → Stratégies de comptes → Stratégie de mot de passe`

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (dona.one)](https://dona.one/f/8_5Ii65raSA)

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (mega.nz)](https://mega.nz/file/dqBRmKAQ#L_Ak1SKhfHACe-pYV1mwFha3WAaI0d7ptCoB6ZdKQAw)

---

3️⃣︲**Configurer les paramètres.**

* Durée de vie maximale du mot de passe → **0** (n'expire jamais).
* Longueur minimale → **5 caractères**.
* Le mot de passe doit respecter des exigences de complexité → **Désactivé**.

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (dona.one)](https://dona.one/f/0ECS-OgWETy)

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (mega.nz)](https://mega.nz/file/YqZW0JID#9W8xB5BJz-Q4s2zM1SdWqPmhhVHNFvfZc98MtnyxjCM)

---

4️⃣︲**Appliquer les modifications.**

* Ouvrir une **Invite de commandes** en tant qu'administrateur.
* Exécuter :

```cmd
gpupdate /force
```

<details>
  <summary>📸︲Application de la stratégie.</summary>

<img width="1023" height="767" alt="image" src="https://github.com/user-attachments/assets/90ac5f74-5b23-4e5f-9c3d-59f67e888815" />

</details>

---

<a id="creation-ou-groupes-users"></a>
## ` 👥 `︲Création des UOs, groupes et utilisateurs.

---

> [!NOTE]
> - Les Unités d'Organisation (UO) permettent de structurer l'entreprise par service et département.
> - Les noms des UO doivent respecter la convention : **pas de majuscules ni d'accents**.
> - À ce stade, **les branches "Stagiaires" et "Salles" ne doivent pas encore être créées**.

---

1️⃣︲**Accéder à la console Utilisateurs et ordinateurs Active Directory.**

* Ouvrir le **Gestionnaire de serveur** → onglet **AD DS** → clic droit sur le serveur → **Ouvrir**.

---

2️⃣︲**Créer l'UO principale (racine).**

* Clic droit sur le domaine `descartesbleu.org` → **Nouveau** → **Unité d'organisation**.
* Nom : `Centre De Formation Descartes-bleu`.
* Activer l'option **Protéger contre la suppression accidentelle** *(recommandé)*.

---

3️⃣︲**Créer les UOs secondaires (services).**

Dans l'UO principale, créer les 4 unités suivantes :

* `🗂️`︲`service_administratif`
* `🗂️`︲`service_scolarite`
* `🗂️`︲`formateurs`
* `🗂️`︲`service_comptabilite`

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (dona.one)](https://dona.one/f/foSU5ug92ZJ)

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (mega.nz)](https://mega.nz/file/hyJW3ZZK#il98RMCD7KSRnqAi3cWTSM4SW3tWOXJaZ6bBPp-OHVg)

---

4️⃣︲**Créer les groupes de sécurité.**

> [!NOTE]
> Les groupes de sécurité de type **Global** sont utilisés pour gérer les permissions d'accès aux ressources partagées.

Dans chaque UO, clic droit → **Nouveau** → **Groupe**.
Sélectionner l'**Étendue : Globale** et le **Type : Sécurité**.

| Unité d'organisation    | Groupe à créer     |
| :---------------------- | :----------------- |
| `service_administratif` | `GRPAdministratif` |
| `service_scolarite`     | `GRPScolarite`     |
| `formateurs`            | `GRPFormateurs`    |
| `service_comptabilite`  | `GRPComptabilite`  |

<details>
  <summary>📸︲Création des groupes.</summary>

<img width="1024" height="768" alt="image" src="https://github.com/user-attachments/assets/719d42af-65ae-4085-9a38-ca09d0d2a27a" />

</details>

---

5️⃣︲**Créer les utilisateurs.**

Dans chaque UO, clic droit → **Nouveau** → **Utilisateur**.

* **Nom / Prénom** de l'utilisateur.
* **Login (samAccountName) :** format `prenom.nom`.
* **Mot de passe :** format `prenom.nom`.
* **Décocher** : *L'utilisateur doit changer son mot de passe à la prochaine connexion*.
* **Cocher** : *Le mot de passe n'expire jamais*.

**Utilisateurs à créer par UO :**

| UO                      | Utilisateurs                                   |
| :---------------------- | :--------------------------------------------- |
| `service_administratif` | Patrick SALRIE, Christine ALLONGUI             |
| `service_scolarite`     | Amina ZOUBIER                                  |
| `formateurs`            | Gaëlle CASTEL, Sylvain CHERRIER, Valéry PERRIN |
| `service_comptabilite`  | Cécile LE BOHEC                                |

---

6️⃣︲**Ajouter les utilisateurs à leurs groupes.**

* Double-cliquer sur le groupe → onglet **Membres** → **Ajouter**.
* Entrer le login (ex : `prenom.nom`) → **Vérifier les noms** → **OK**.

---

<a id="integration-client"></a>
# ` 💻 `︲Intégration d'un client au domaine.

---

<a id="config-reseau-dns"></a>
## ` 🌐 `︲Configuration réseau et paramètres DNS.

---

> [!NOTE]
> Avant de joindre le domaine, s'assurer que les prérequis réseau sont satisfaits : IP fixe, DNS pointant sur le contrôleur de domaine, et connectivité validée.

---

1️⃣︲**Prérequis à vérifier.**

* Connaître le **nom exact du domaine** : `descartesbleu.org`.
* Le PC possède une **adresse IP fixe** (ou DHCP si configuré).
* Le **serveur DNS** du PC pointe vers l'IP du contrôleur de domaine : `172.16.0.1`.

---

2️⃣︲**Tester la connectivité réseau.**

Depuis une invite de commandes sur le client, exécuter :

```cmd
ping 172.16.0.1
```

```cmd
ping descartesbleu.org
```

```cmd
nslookup descartesbleu.org
```

La réponse `nslookup` doit afficher l'IP du contrôleur de domaine.

---

<a id="joindre-domaine"></a>
## ` 🔗 `︲Joindre le domaine descartesbleu.org.

---

### ` 🖼️ `︲Méthode 1 : Interface graphique.

* Clic droit sur **Ce PC** → **Propriétés** → **Domaine ou groupe de travail** → **Modifier**.
* Sélectionner **Domaine** → entrer `descartesbleu.org`.
* Entrer les identifiants d'un compte autorisé.
* **Redémarrer** le PC quand demandé.

<details>
  <summary>📸︲Jonction au domaine (GUI).</summary>

<img width="1023" height="768" alt="image" src="https://github.com/user-attachments/assets/3437a0db-0531-4a54-b5f2-e7a9bdb266ce" />

</details>

---

### ` 💻 `︲Méthode 2 : PowerShell (plus fiable).

* Lancer **PowerShell en tant qu'administrateur** et exécuter :

```powershell
Add-Computer -DomainName descartesbleu.org
```

* Entrer les identifiants du compte autorisé.
* Redémarrer :

```powershell
Restart-Computer
```

---

### ` 📝 `︲Récapitulatif.

| Étape              | Action                                            |
| :----------------- | :------------------------------------------------ |
| Prérequis          | Vérifier IP, DNS, nom de domaine, droits          |
| Connectivité       | Tester `ping` et `nslookup` vers le serveur       |
| Méthode GUI        | Interface Windows, simple pour débutants          |
| Méthode PowerShell | Plus rapide, utile pour automatisation            |
| Après redémarrage  | Se connecter avec un compte utilisateur du domaine |

> [!TIP]
> À ce stade, le poste client est **membre du domaine** `descartesbleu.org` !

---

<a id="partages-fichiers"></a>
# ` 📁 `︲Gestion des partages de fichiers.

---

<a id="dossiers-partages"></a>
## ` 📂 `︲Création des dossiers partagés et permissions.

---

> [!NOTE]
> Les dossiers partagés centralisent les données utilisateurs sur le serveur, permettant la sauvegarde et l'accès depuis n'importe quel poste du domaine.

---

1️⃣︲**Créer un dossier partagé sur le serveur.**

* Exemple de chemin local : `D:\profils`
* Clic droit → **Propriétés** → **Partage** → **Partage avancé**.
* Cocher **Partager ce dossier**.
* Nom du partage : `profils$` *(le `$` le rend invisible sur le réseau)*.

---

2️⃣︲**Définir les permissions.**

* **Autorisations de partage :** donner **Contrôle total** à *Tout le monde*.
* **Sécurité (NTFS) :**
  * Désactiver l'héritage.
  * Ajouter *Utilisateurs du domaine* (ou groupe ciblé, ex : `formateurs`).
  * Droits : **Lecture, Écriture, Modification**.

<details>
  <summary>📸︲Configuration du partage et permissions NTFS.</summary>

<img width="1024" height="775" alt="image" src="https://github.com/user-attachments/assets/35daa6dc-691c-41b2-a364-b5bf478a76c2" />
<img width="1026" height="768" alt="image" src="https://github.com/user-attachments/assets/5cfa00c6-3b26-4493-b26e-f2daff572b8a" />
<img width="1020" height="771" alt="image" src="https://github.com/user-attachments/assets/85281443-0e5c-4f0e-8e6a-9d0b3c5fd8a5" />

</details>

---

<a id="redirection-hors-connexion"></a>
## ` 💾 `︲Redirection des dossiers utilisateur et mode hors connexion.

---

1️⃣︲**Créer une GPO de redirection.**

* Dans **Gestion des stratégies de groupe (GPMC)**.
* Clic droit sur l'OU des utilisateurs (ex : `formateurs`) → **Créer une GPO et la lier ici**.

---

2️⃣︲**Configurer la redirection de dossiers.**

* Éditer la GPO → naviguer vers :
  `Configuration utilisateur → Stratégies → Paramètres Windows → Redirection de dossiers`
* Pour chaque dossier à rediriger (**Documents**, **Bureau**, **Images**, **Favoris**) :
  * Clic droit → **Propriétés** → onglet **Cible**.
  * Type : **De base**.
  * Emplacement : **Rediriger vers l'emplacement suivant**.
  * Chemin UNC : `\\SRV-AD1\profils$\%USERNAME%\Documents`
  * Onglet **Paramètres** : décocher **Accorder à l'utilisateur des droits exclusifs**.

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (dona.one)](https://easyfiles.cc/Oxus8427i7P)

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (mega.nz)](https://mega.nz/file/MmpiCJaA#LBTkhEsESV8L3dguxT9C_xb61P5RVD_4K3YsWylAjAA)

---

3️⃣︲**Appliquer la stratégie.**

* Sur les postes clients, exécuter :

```cmd
gpupdate /force
```

---

4️⃣︲**Activer le mode Hors-Connexion.**

Ce mode permet à l'utilisateur d'accéder à ses fichiers redirigés même sans réseau.

* Dans la GPO → naviguer vers :
  `Configuration ordinateur → Stratégies → Modèles d'administration → Réseau → Fichiers hors connexion`
* Paramètre **Autoriser ou interdire l'utilisation des fichiers hors connexion** → **Activer** → **Appliquer** → **OK**.

<details>
  <summary>📸︲Mode hors connexion (GPO).</summary>

<img width="1024" height="770" alt="image" src="https://github.com/user-attachments/assets/11530e18-3e7e-4e47-8854-4779ae765612" />

</details>

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (dona.one)](https://easyfiles.cc/7kZ-EWXAKOd)

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (mega.nz)](https://mega.nz/file/A3RgBISJ#ubX5UjW7gNNeCw1CsqZ-SBzdgwHhHhYPQyDtt0krISA)

---

<a id="automatisation-powershell"></a>
# ` 📜 `︲Automatisation via PowerShell.

---

<a id="script-csv"></a>
## ` ⚡ `︲Script pour créer des UOs, groupes et utilisateurs depuis un fichier `.CSV`.

---

> [!NOTE]
> Ce script automatise la création d'une UO, d'un groupe de sécurité et de l'ensemble des comptes utilisateurs à partir d'un fichier `.CSV`.
> Assurez-vous que le chemin vers le fichier CSV est correct, sinon le script ne trouvera pas les entrées et échouera.

---

1️⃣︲**Prérequis.**

> [!IMPORTANT]
> - Un serveur **Active Directory** avec des **droits d'administrateur de domaine**.
> - Le module PowerShell **ActiveDirectory** installé et importable.
> - Un fichier `.CSV` correctement formaté placé au bon emplacement.

---

2️⃣︲**Fichier `.CSV` requis.**

Le fichier doit être structuré comme suit (délimiteur `;`) :

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
> Les mots de passe sont visibles en clair dans ce fichier. Ne le laissez pas accessible à tous.

---

3️⃣︲**Emplacement du fichier `.CSV`.**

Placer le fichier à l'emplacement défini dans le script (modifiable) :

```powershell
$CsvPath = "C:\Users\Administrateur\Documents\script\stagiaires_sisr.csv"
```

---

4️⃣︲**Script complet.**

```powershell
# Importation du module Active Directory
Import-Module ActiveDirectory

# Définition des chemins
$DomaineDN = "DC=descartesbleu,DC=org"
$OU_Parent  = "OU=stagiaires,$DomaineDN"
$OU_Child   = "stagiaires_sisr"

# 1. Création de l'OU 'stagiaires_sisr'
New-ADOrganizationalUnit -Name $OU_Child -Path $OU_Parent -ProtectedFromAccidentalDeletion $false
Write-Host "OU '$OU_Child' créée dans '$OU_Parent'." -ForegroundColor Cyan

# Chemin complet de la nouvelle OU
$OU_Path   = "OU=$OU_Child," + $OU_Parent
$GroupName = "grp_stagiaires_sisr"

# 2. Création du groupe de sécurité global
New-ADGroup -Name $GroupName `
            -GroupScope Global `
            -GroupCategory Security `
            -Path $OU_Path `
            -Description "Groupe de sécurité global pour les stagiaires SISR"
Write-Host "Groupe '$GroupName' créé dans '$OU_Child'." -ForegroundColor Cyan

# 3. Lecture du fichier CSV et création des utilisateurs
$CsvPath   = "C:\Users\Administrateur\Documents\script\stagiaires_sisr.csv"
$GroupeNom = "grp_stagiaires_sisr"
$Utilisateurs = Import-Csv -Path $CsvPath -Delimiter ';'

foreach ($user in $Utilisateurs) {
    $Prenom     = $user.prenom
    $Nom        = $user.nom
    $Mail       = $user.mail
    $MotDePasse = $user.motdepasse
    $Identifiant = ($Prenom + "." + $Nom).ToLower()

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

    Add-ADGroupMember -Identity $GroupeNom -Members $Identifiant
    Write-Host "Utilisateur $Identifiant créé et ajouté au groupe $GroupeNom." -ForegroundColor Green
}
```

---

5️⃣︲**Lancer le script.**

* Ouvrir **PowerShell ISE en mode administrateur**.
* Coller le script et l'exécuter.

Le script va automatiquement :

* Créer l'OU `stagiaires_sisr` dans `OU=stagiaires`.
* Créer le groupe de sécurité `grp_stagiaires_sisr`.
* Lire chaque ligne du `.CSV` et créer le compte utilisateur correspondant.
* Ajouter chaque utilisateur au groupe.

> [!IMPORTANT]
> Sur la vidéo de démonstration, le script avait déjà été exécuté une première fois, ce qui explique les messages d'erreur visibles. Si c'est votre première exécution, vous ne verrez que des messages verts confirmant la création des comptes.

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (easyfiles.cc)](https://easyfiles.cc/ODeJNl2AZFQ)

> [!TIP]
> [🎥︲Démo vidéo – Cliquez-ici (mega.nz)](https://mega.nz/file/ljQn3RrZ#2WqZiSNodYpo_jC5LyyZsuvLH2SPMvW8j1zn_XjTfwI)

<details>
  <summary><strong>💡︲Documentation du script (variables et personnalisation).</strong></summary>

**Variables modifiables :**

* **`$DomaineDN`** : Adapter au nom de votre domaine.
* **`$OU_Parent` / `$OU_Child`** : Modifier pour une autre structure d'UO.
* **`$CsvPath`** : S'assurer que le chemin du fichier CSV est correct.

**Sécurité :**

* Le mot de passe des utilisateurs est défini en clair dans le CSV. Configurer un mot de passe sécurisé en production.
* L'OU est créée sans protection contre la suppression accidentelle : ajuster le paramètre `$ProtectedFromAccidentalDeletion` si nécessaire.

</details>

---

<a id="deploiement-gpo"></a>
# ` 🖱️ `︲Déploiement de stratégies de groupe (GPO).

---

> [!NOTE]
> Les GPO permettent une gestion centralisée d'Active Directory : sécurité, configuration utilisateur, déploiement de logiciels ou de ressources réseau.
> Accès via `GPMC.msc` (Outils d'administration).

---

## ` 🔐 `︲Créer et modifier une GPO.

1. Ouvrir la console **GPMC**.
2. Naviguer : `Forêt → Domaines → descartesbleu.org`.
3. Clic droit sur une OU → **Créer un objet GPO dans ce domaine, et le lier ici…** → Nommer → **OK**.
4. Clic droit → **Modifier** pour ouvrir l'éditeur.

**Commandes essentielles :**

| Commande          | Description                         |
| :---------------- | :---------------------------------- |
| `gpupdate /force` | Appliquer immédiatement les GPO     |
| `gpresult /R`     | Vérifier les GPO appliquées         |
| `rsop.msc`        | Résultat de la stratégie de groupe  |

---

<a id="redirection-mappage-firefox"></a>
## ` 📂 `︲Redirection Documents, mappage de lecteurs et déploiement Firefox.

---

> [!NOTE]
> **Objectifs de cette section :**
> - Rediriger le dossier *Documents* vers le serveur.
> - Mapper un lecteur réseau par GPO.
> - Déployer Firefox automatiquement par GPO.

---

### ` 📁 `︲1. Rediriger le dossier Documents.

1️⃣︲**Créer le dossier partagé sur le serveur.**

* Créer le dossier (ex. : `E:\profils`).
* Clic droit → **Propriétés → Partage avancé** → cocher **Partager ce dossier** → ajouter `$` pour masquer (ex. `\\SRV-AD1\Profils$`).
* Autorisations de partage : **Contrôle total** à *Tout le monde*.
* Sécurité (NTFS) : désactiver l'héritage → donner **Lecture/Écriture** aux *Utilisateurs du domaine*.

2️⃣︲**Configurer la GPO.**

* Dans **GPMC** → créer une GPO liée à l'OU des utilisateurs → l'éditer.
* Naviguer vers :
  `Configuration utilisateur → Stratégies → Paramètres Windows → Redirection de dossiers`
* Clic droit sur **Documents → Propriétés** :
  * Type : **De base**.
  * Emplacement : `\\SRV-AD1\Profils$\%USERNAME%\Documents`.
  * Décocher **Droits exclusifs**.
* Appliquer sur les postes :

```cmd
gpupdate /force
```

---

### ` 🔧 `︲2. Mapper un lecteur réseau.

* Dans **GPMC** → éditer la GPO de votre choix → naviguer vers :
  `Configuration utilisateur → Préférences → Paramètres Windows → Mappages de lecteur`
* Nouveau → **Lecteur mappé** :
  * **Action :** Mettre à jour.
  * **Emplacement :** `\\SRV-AD1\Partage$`.
  * **Lettre :** `P:`.
  * **Nom :** `PARTAGE`.
  * **Reconnecter :** coché.
* *(Optionnel)* Onglet **Commun** → **Ciblage** pour limiter le mappage à un groupe spécifique.

> [!TIP]
> Ajouter une action **Supprimer** avec ciblage inverse pour retirer le lecteur si l'utilisateur quitte le groupe.

---

### ` 🦊 `︲3. Déployer Firefox (.MSI).

1️⃣︲**Préparer le partage.**

* Créer un dossier partagé (ex. `D:\Applications$`).
* Copier **Firefox.msi** dedans.
* Droits **Lecture** pour les *Ordinateurs du domaine*.

2️⃣︲**Configurer la GPO.**

* Dans **GPMC** → créer une GPO liée à l'OU contenant les **ordinateurs** → l'éditer.
* Naviguer vers :
  `Configuration ordinateur → Stratégies → Paramètres du logiciel → Installation de logiciel`
* Clic droit → **Nouveau → Package** → indiquer le chemin UNC :

```
\\SRV-AD1\Applications$\Firefox.msi
```

* Choisir **Attribué** (installation automatique).
* Redémarrer les postes ou exécuter :

```cmd
gpupdate /force
```

---

<a id="restrictions-avancees"></a>
# ` 🔒 `︲Restrictions et fonctionnalités avancées.

---

<a id="horaires-rdp-bginfo"></a>
## ` ⏱️ `︲Horaires de connexion, Bureau à distance et BgInfo.

---

> [!NOTE]
> **Objectifs de cette section :**
> 1. Limiter les horaires de connexion des utilisateurs.
> 2. Activer le Bureau à distance (RDP) pour les administrateurs.
> 3. Déployer l'outil BgInfo via GPO.

---

### ` 🚩 `︲1. Limiter les horaires de connexion.

Cette configuration empêche certains utilisateurs (ex. : stagiaires) de se connecter en dehors des plages définies.

1. Ouvrir **Utilisateurs et ordinateurs Active Directory (ADUC)**.
2. Localiser l'utilisateur concerné dans son UO.
3. Clic droit → **Propriétés** → onglet **Compte** → bouton **Horaires d'accès**.
4. Dans le tableau, cliquer sur les heures autorisées ou refusées (ex. : Lundi–Vendredi, 8h–18h).
5. Valider par **OK**.

---

### ` ⚫ `︲2. Activer le Bureau à distance (RDP).

Permet aux administrateurs d'accéder au serveur à distance.

1. Ouvrir le **Gestionnaire de serveur** → **Serveur local**.
2. Cliquer sur **Bureau à distance : Désactivé**.
3. Cocher **Autoriser les connexions à distance à cet ordinateur**.
4. *(Optionnel)* Cocher **Autoriser uniquement les connexions avec NLA**.
5. Le pare-feu s'ajuste automatiquement → **OK** → **Appliquer**.

---

### ` 📂 `︲3. Déployer BgInfo via GPO.

BgInfo affiche automatiquement sur le Bureau des informations système (nom du PC, IP, domaine...).

1️⃣︲**Préparer les fichiers.**

* Télécharger **Bginfo.exe** (outil Microsoft SysInternals).
* L'exécuter pour personnaliser l'affichage → enregistrer sous : `Template_Bginfo.bgi`.
* Copier `Bginfo.exe` et `Template_Bginfo.bgi` dans le partage Netlogon du domaine :

```
\\descartesbleu.org\Netlogon\BgInfo
```

* Créer un script Batch `bginfo.bat` dans ce même dossier :

```batch
@echo off
%logonserver%\netlogon\BgInfo\Bginfo.exe %logonserver%\netlogon\BgInfo\Template_Bginfo.bgi /accepteula /silent /timer 0
exit
```

2️⃣︲**Déployer par GPO.**

* Dans **GPMC** → créer une GPO (ex. : *BgInfo*) → l'éditer.
* Naviguer vers :
  `Configuration ordinateur → Préférences → Paramètres Windows → Fichiers`
* Clic droit → **Nouveau → Fichier** :
  * **Source :** `\\descartesbleu.org\Netlogon\BgInfo\bginfo.bat`
  * **Destination :** `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\bginfo.bat`
* Onglet **Commun** → cocher **Ciblage au niveau de l'élément** → sélectionner les UO concernées.
* Fermer et appliquer la GPO.

> [!TIP]
> **Alternative :** déployer BgInfo via un **script d'ouverture de session** :
> `Configuration utilisateur → Stratégies → Scripts (ouverture/fermeture)`, en le réservant à un groupe spécifique (ex. : `Adm-BgInfo`).

> [!TIP]
> À ce stade, ton environnement Active Directory est **complet et opérationnel** !
> Redirection de dossiers, GPO, scripts PowerShell, restrictions avancées : tout est en place.

---

<a id="conclusion"></a>
# ` ✅ `︲Conclusion et Annexes.

---

> [!IMPORTANT]
> Cette documentation rassemble tout le nécessaire pour reproduire l'environnement demandé par le TP : installation et configuration du serveur, promotion en contrôleur de domaine, création des UO et comptes, déploiement des GPO, scripts d'import d'utilisateurs, et procédures de vérification. Si tu as suivi les étapes et appliqué les corrections indiquées, tu peux être confiant : la structure est fonctionnelle et prête.

---

### ` 💡 `︲Points de contrôle indispensables.

1. **Snapshots :** prendre un snapshot après chaque étape majeure (OS installé, DC promu, client joint, GPO appliquée).
2. **Vérifications :** captures d'écran des commandes `nslookup`, `dcdiag`, `gpresult` et d'un login utilisateur.
3. **Conventions de nommage :** UO en minuscules sans accents, chemins DATA sur `E:\`, admin = `Administrateur / btssio-lmc25`.
4. **CSV & script :** vérifier que l'en-tête CSV correspond exactement aux champs lus par le script.

---

**Annexe — Chemins UNC de référence.**

| Type                         | Chemin UNC                                | Description                                                  |
| :--------------------------- | :---------------------------------------- | :----------------------------------------------------------- |
| **Serveur**                  | `\\SRV-AD1`                               | Contrôleur de domaine principal                              |
| **Racine de profils**        | `\\SRV-AD1\profils$`                      | Dossier partagé (masqué) contenant les profils utilisateurs  |
| **Sous-dossier utilisateur** | `\\SRV-AD1\profils$\%USERNAME%`           | Dossier personnel de chaque utilisateur                      |
| **Documents**                | `\\SRV-AD1\profils$\%USERNAME%\Documents` | Dossier *Documents* redirigé                                 |
| **Bureau**                   | `\\SRV-AD1\profils$\%USERNAME%\Bureau`    | Dossier *Bureau* redirigé (formateurs)                       |
| **Images**                   | `\\SRV-AD1\profils$\%USERNAME%\Images`    | Dossier *Images* redirigé                                    |
| **Favoris**                  | `\\SRV-AD1\profils$\%USERNAME%\Favoris`   | Dossier *Favoris* redirigé                                   |

---

| Type                    | Chemin UNC                   | Description                                               |
| :---------------------- | :--------------------------- | :-------------------------------------------------------- |
| **Partage commun**      | `\\SRV-AD1\depot`            | Zone d'échange ou dépôt commun                            |
| **Ressources**          | `\\SRV-AD1\ressources`       | Dossier pour les fichiers pédagogiques ou administratifs  |
| **Partage personnel**   | `\\SRV-AD1\perso`            | Racine des dossiers personnels stagiaires                 |
| **Dossier utilisateur** | `\\SRV-AD1\perso\prenom.nom` | Dossier personnel d'un stagiaire (Documents redirigés)    |

---

| Type                   | Chemin UNC                            | Description                                                                 |
| :--------------------- | :------------------------------------ | :-------------------------------------------------------------------------- |
| **Applications (MSI)** | `\\SRV-AD1\applications$`             | Source des fichiers d'installation déployés par GPO (ex : Firefox)          |
| **Scripts Netlogon**   | `\\descartesbleu.org\Netlogon`        | Répertoire système du domaine contenant les scripts d'ouverture de session  |
| **BgInfo**             | `\\descartesbleu.org\Netlogon\BgInfo` | Contient `Bginfo.exe`, le modèle `.bgi` et le script `bginfo.bat`           |

---

<a id="outils-ressources"></a>
# ` 🧰 `︲Outils et ressources utilisées.

---

> [!TIP]
> Vous trouverez ici la liste de tous les outils, ressources et services utilisés pour la création de cette documentation.
> Les liens correspondants sont accessibles en cliquant sur l'emoji : `  🌐  `.

---

* `📄` **︲Documents d'annexes fournis dans le TP.**

---

* ` 🤖 ` **︲`DeepSeek V3.2`** ︲[`🌐`](https://www.deepseek.com/)
* ` 🌐 ` **︲`Donarev419.com`** ︲[`🌐`](https://donarev419.com/)
* ` 🤖 ` **︲`GPT-5`** ︲[`🌐`](https://chatgpt.com/)
* ` 🍍 ` **︲`HandBrake`** ︲[`🌐`](https://handbrake.fr/)
* ` 🤖 ` **︲`KimiK2`** ︲[`🌐`](https://www.kimi.com/)
* ` 🤖 ` **︲`NoteBookLM`** ︲[`🌐`](https://notebooklm.google.com/)
* ` 📦 ` **︲`Notion`** ︲[`🌐`](https://www.notion.com/fr)
* ` 🤖 ` **︲`Qwen3-VL-235B-A22B`** ︲[`🌐`](https://chat.qwen.ai/)
* ` ✂️ ` **︲`Screenpresso`** ︲[`🌐`](https://www.screenpresso.com/fr/)
* ` 🛠️ ` **︲`VMWare`** ︲[`🌐`](https://www.vmware.com/)
* ` 😀 ` **︲`Smiley.cool`** ︲[`🌐`](https://smiley.cool/emoji-list.php)
* ` ❓ ` **︲`Syntaxe de base pour l'écriture et la mise en forme`** ︲[`🌐`](https://docs.github.com/fr/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
* ` ❓ ` **︲`Markdownguide.org`** ︲[`🌐`](https://www.markdownguide.org/)

---

> * ` 🎚️ `︲Nora Player ︲[`🌐`](https://noramusic.netlify.app/)
> * ` 🎶 `︲Mac DeMarco - 20200229 2 ︲[`🌐`](https://youtu.be/Y_KLjGEQTgY)
> * ` ☕ `︲De la patience.

---
