# .MDPlayground
Ce repo servira à tester les documentations pour le lycée en .MD, en espérant que ça passe crème autant que la README de la réinstallation Windows que j'ai élaborée.

# `💻`︲Documentation TP Active Directory

---

Ce repo GitHub présente un guide complet pour réaliser le TP **Active Directory**, étape par étape. Les instructions sont claires et détaillées afin que tu puisses suivre sans problème, **même si tu es débutant**.

---

## `📑`︲Sommaire

1. [`📘`︲Introduction](#introduction)
   - [`📄`︲Contexte et objectifs du TP](#contexte-et-objectifs-du-tp)
   - [`🖥️`︲Présentation de l'architecture réseau et des outils utilisés](#presentation-de-larchitecture-reseau-et-des-outils-utilises)

2. [`🛠️`︲Préparation de l'environnement](#preparation-de-lenvironnement)
   - [`💿`︲Installation de Windows 11 (client)`](#installation-de-windows)
   - [`💿`︲Installation de Windows Server 2025 (serveur)`](#installation-de-windows-server)
   - [`⚙️`︲Configuration matérielle, paramètres réseau et création d'un utilisateur](#configuration-materielle-et-utilisateur)

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

---

1️⃣・**Configuration de la VM**  
   - Disque : **80 Go**  
   - RAM : **4 Go**  
   - CPU : **1 cœur**
     
<details>
  <summary>📸︲Configuration de Windows 11</summary>

  <img width="1022" height="769" alt="Screenshot_2" src="https://github.com/user-attachments/assets/e3686b5a-2faf-4877-8f88-68ce860288f8" />

  **Explications :**  
  Sélection de la langue et du format régional :  
  - Langue : Français (France)  
  - Format heure/devise : Français (France)  
  Bouton *Suivant* activé pour poursuivre l’installation.
</details>

---

2️⃣・**Installation depuis l’ISO**  
   - Sélectionner langue, clavier et région  

<details>
  <summary>📸︲Capture d’écran</summary>

  <img width="1022" height="769" alt="Screenshot_2" src="https://github.com/user-attachments/assets/4013d7fe-1cf0-4e5c-8d7d-b4cf663a85e1" />

  **Configuration de Windows 11**  
  Choix du clavier :  
  - Méthode d’entrée : Français (Traditionnel, AZERTY)  
  Bouton *Suivant* activé pour continuer l’installation.
</details>


---

3️⃣・**Accepter les conditions de licence**  
   - Choisir **Installation personnalisée (Custom Install)**  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 3 -->
</details>

4️⃣・**Sélection du disque**  
   - Disque : **80 Go**  

<details>
  <summary>📸︲Capture d’écran</summary>

  <img width="1023" height="770" alt="Screenshot_5" src="https://github.com/user-attachments/assets/2711928a-a7ba-45fd-8c45-51dbb51058e3" />

  **Sélection de l’emplacement d’installation – Windows 11**  
  L’utilisateur choisit un disque pour installer le système :  
  - Espace disque 0 non alloué : 800,0 Go disponibles  
  - Actions possibles : Créer, formater, supprimer ou étendre une partition  
  Bouton *Suivant* cliqué pour lancer l’installation sur l’espace sélectionné.
</details>


5️⃣・**Configuration réseau**  
   - IP : `172.16.0.x`  
   - Masque : `255.255.255.0`  
   - DNS : `172.16.0.1`  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 5 -->
</details>

---

6️⃣・**Création de l’utilisateur**  
   - Nom : `btssio`  
   - Mot de passe : `btssio`  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 6 -->
</details>

---

7️⃣・**Vérification de l’installation**  
   - Redémarrer et se connecter avec l’utilisateur  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 7 -->
</details>

<details>
  <summary><strong>💡︲Conseils pour Windows 11</strong></summary>
  - Assure-toi que la machine virtuelle a accès à Internet pour les mises à jour.  
  - Prends des captures d’écran de chaque étape importante pour la documentation.
</details>

---

<a id="installation-de-windows-server"></a>
### `💿`︲Installation de Windows Server 2025 (serveur)

---

1️⃣・**Configuration de la VM**  
   - Disque : **80 Go**  
   - RAM : **2 Go**  
   - CPU : **1 cœur**  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 1 -->
</details>

---

2️⃣・**Partitionnement du disque**  
   - 40 Go pour l’OS  
   - 40 Go pour DATA  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 2 -->
</details>

---

3️⃣・**Installation depuis l’ISO**  
   - Sélectionner langue, clavier et région  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 3 -->
</details>

---

4️⃣・**Accepter les conditions de licence**  
   - Choisir **Installation personnalisée (Custom Install)**  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 4 -->
</details>

---

5️⃣・**Sélection de la partition OS**  
   - Partition : 40 Go  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 5 -->
</details>

---

6️⃣・**Configuration réseau**  
   - IP : `172.16.0.1`  
   - Masque : `255.255.255.0`  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 6 -->
</details>

---

7️⃣・**Création du compte administrateur**  
   - Nom : `Administrator`  
   - Mot de passe : `btssio-lmc25`  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 7 -->
</details>

---

8️⃣・**Vérification de l’installation**  
   - Redémarrer et se connecter avec le compte administrateur  

<details>
  <summary>📸︲Capture d’écran</summary>
  <!-- Insérer capture écran 8 -->
</details>

<details>
  <summary><strong>💡︲Conseils pour Windows Server</strong></summary>
  - Vérifie que la partition DATA est correctement détectée après l’installation.  
  - Prends des captures d’écran pour documenter chaque étape.
</details>

---

