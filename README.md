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
   - [`💿`︲Installation de Windows 11 (client)](#installation-de-windows)
   - [`💿`︲Installation de Windows Server 2025 (serveur)](#installation-de-windows-server)
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

  ---
  <img width="1023" height="770" alt="Screenshot_5" src="https://github.com/user-attachments/assets/a164ea6f-4915-429d-a664-0cbd76103a77" />

  Sur cette capture, on peut voir la **sélection du disque d’installation**.  
  Il faut choisir l’espace non alloué de 80 Go et cliquer sur *Suivant* pour lancer l’installation sur ce disque.

</details>

---

3️⃣・**Accepter les conditions de licence**  
   - Choisir **Installation personnalisée (Custom Install)**  

<details>
  <summary>📸︲Conditions de licence</summary>

  <!-- Ici tu peux insérer la capture si dispo -->
  Sur cette capture, on vérifie et accepte les conditions de licence.  
  Il faut cliquer sur *Suivant* pour continuer.

</details>

---

4️⃣・**Sélection du disque final**  
   - Disque : **80 Go**  

<details>
  <summary>📸︲Sélection finale du disque</summary>

  <img width="1023" height="770" alt="Screenshot_5" src="https://github.com/user-attachments/assets/2711928a-a7ba-45fd-8c45-51dbb51058e3" />

  Sur cette capture, on peut voir l’**emplacement exact pour l’installation**.  
  Il faut sélectionner l’espace disque non alloué de 80 Go et cliquer sur *Suivant* pour lancer l’installation.

</details>

---

5️⃣・**Configuration réseau**  
   - IP : `172.16.0.x`  
   - Masque : `255.255.255.0`  
   - DNS : `172.16.0.1`  

<details>
  <summary>📸︲Paramètres réseau</summary>

  <!-- Insérer capture réseau -->
  Sur cette capture, on configure les paramètres réseau pour la VM.  
  Il faut entrer l’IP, le masque et le DNS comme indiqué.

</details>

---

6️⃣・**Création de l’utilisateur**  
   - Nom : `btssio`  
   - Mot de passe : `btssio`  

<details>
  <summary>📸︲Création de l’utilisateur</summary>

  <!-- Insérer capture utilisateur -->
  Sur cette capture, on peut voir la **création du compte utilisateur**.  
  Il faut remplir le nom et le mot de passe et valider.

</details>

---

7️⃣・**Vérification de l’installation**  
   - Redémarrer et se connecter avec l’utilisateur  

<details>
  <summary>📸︲Vérification finale</summary>

  <!-- Insérer capture vérification -->
  Sur cette capture, on peut voir que l’installation est terminée et que l’utilisateur peut se connecter.  

</details>

<details>
  <summary><strong>💡︲Conseils pour Windows 11</strong></summary>
  - Assurez-vous que la machine virtuelle a accès à Internet pour les mises à jour.  
  - Prenez des captures d’écran de chaque étape importante pour la documentation.
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

  <!-- Insérer capture serveur 1 -->
  Sur cette capture, on vérifie la configuration initiale de la VM.  
  Il faut s’assurer que disque, RAM et CPU sont corrects avant d’installer.

</details>

---

2️⃣・**Partitionnement du disque**  
   - 40 Go pour l’OS  
   - 40 Go pour DATA  

<details>
  <summary>📸︲Partitionnement</summary>

  <!-- Insérer capture partition -->
  Sur cette capture, on peut voir la **répartition du disque**.  
  Il faut créer deux partitions : 40 Go pour l’OS et 40 Go pour les données.

</details>

---

3️⃣・**Installation depuis l’ISO**  
   - Sélectionner langue, clavier et région  

<details>
  <summary>📸︲Sélection ISO serveur</summary>

  <!-- Insérer capture ISO -->
  Sur cette capture, on choisit la langue et le clavier pour le serveur.  

</details>

---

4️⃣・**Accepter les conditions de licence**  
   - Choisir **Installation personnalisée (Custom Install)**  

<details>
  <summary>📸︲Conditions de licence serveur</summary>

  <!-- Insérer capture licence serveur -->
  Sur cette capture, on accepte les conditions de licence.

</details>

---

5️⃣・**Sélection de la partition OS**  
   - Partition : 40 Go  

<details>
  <summary>📸︲Partition OS</summary>

  <!-- Insérer capture partition OS -->
  Sur cette capture, on sélectionne la partition de 40 Go pour l’OS et on clique sur *Suivant*.

</details>

---

6️⃣・**Configuration réseau**  
   - IP : `172.16.0.1`  
   - Masque : `255.255.255.0`  

<details>
  <summary>📸︲Paramètres réseau serveur</summary>

  <!-- Insérer capture réseau serveur -->
  Sur cette capture, on configure l’IP et le masque pour le serveur.

</details>

---

7️⃣・**Création du compte administrateur**  
   - Nom : `Administrator`  
   - Mot de passe : `btssio-lmc25`  

<details>
  <summary>📸︲Création administrateur</summary>

  <!-- Insérer capture admin -->
  Sur cette capture, on crée le compte administrateur.

</details>

---

8️⃣・**Vérification de l’installation**  
   - Redémarrer et se connecter avec le compte administrateur  

<details>
  <summary>📸︲Vérification finale serveur</summary>

  <!-- Insérer capture vérification serveur -->
  Sur cette capture, on peut voir que le serveur est prêt et que l’administrateur peut se connecter.

</details>

<details>
  <summary><strong>💡︲Conseils pour Windows Server</strong></summary>
  - Vérifiez que la partition DATA est correctement détectée après l’installation.  
  - Prenez des captures d’écran pour documenter chaque étape.
</details>

---


