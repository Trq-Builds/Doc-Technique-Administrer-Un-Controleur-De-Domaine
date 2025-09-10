# .MDPlayground
Ce repos servira à tester les documentations pour le lycée en .MD, en espérant que ça passe crème autant que la README de la réinstallation Windows que j'ai élaborée.
# `💻`︲Documentation TP Active Directory

---

Ce repo GitHub présente un guide complet pour réaliser le TP **Active Directory**, étape par étape. Les instructions sont claires et détaillées afin que tu puisses suivre sans problème, **même si tu es débutant**.

---

> Ce guide a été élaboré pour le TP. Si tu rencontres une partie difficile à comprendre ou si tu as une suggestion, n'hésite pas à m'envoyer un mail. Pour toute aide, tu peux aussi rejoindre notre serveur [Discord](https://dsc.gg/optimize-alot).

---

## `📑`︲Sommaire

1. [`📘`︲Introduction](#introduction)
   - [`📄`︲Contexte et objectifs du TP](#contexte-et-objectifs-du-tp)
   - [`🖥️`︲Présentation de l'architecture réseau et des outils utilisés](#presentation-de-larchitecture-reseau-et-des-outils-utilises)

2. [`🛠️`︲Préparation de l'environnement](#preparation-de-lenvironnement)
   - [`💿`︲Installation de Windows 11 (client) et Windows Server 2025 (serveur)](#installation-de-windows)
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

<!-- Introduction -->

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

<!-- Préparation de l'environnement -->

<a id="preparation-de-lenvironnement"></a>
## `🛠️`︲Préparation de l'environnement

<a id="installation-de-windows"></a>
### `💿`︲Installation de Windows 11 (client) et Windows Server 2025 (serveur)
- Installer Windows Server sur le serveur
- Installer Windows 11 sur la machine client

<a id="configuration-materielle-et-utilisateur"></a>
### `⚙️`︲Configuration matérielle, paramètres réseau et création d'un utilisateur
- Configurer l’adresse IP statique sur le serveur
- Créer un utilisateur local sur le serveur et le client

---

<!-- Installation du contrôleur de domaine -->

<a id="installation-et-configuration-du-controleur-de-domaine"></a>
## `🏛️`︲Installation et configuration du contrôleur de domaine

<a id="installation-roles-ad-ds-et-dns"></a>
### `🔧`︲Installation des rôles AD DS et DNS
- Ajouter les rôles **Active Directory Domain Services** et **DNS** via le gestionnaire de serveur

<a id="promotion-du-serveur-et-creation-du-domaine"></a>
### `🌐`︲Promotion du serveur en contrôleur de domaine et création du domaine `descartesbleu.org`
- Promouvoir le serveur
- Créer le domaine et définir les options nécessaires

---

<!-- Administration Active Directory -->

<a id="administration-de-lannuaire-active-directory"></a>
## `🗂️`︲Administration de l'annuaire Active Directory

<a id="simplification-strategie-mots-de-passe"></a>
### `🔑`︲Simplification de la stratégie de mots de passe
- Modifier les paramètres de politique de mot de passe pour les rendre plus simples

<a id="creation-ou-groupes-utilisateurs"></a>
### `👥`︲Création des OU, groupes et utilisateurs
- Créer des Unités d’Organisation (OU)
- Ajouter des groupes et utilisateurs selon le TP

---

<!-- Intégration client -->

<a id="integration-dun-client-au-domaine"></a>
## `💻`︲Intégration d'un client au domaine

<a id="configuration-reseau-dns"></a>
### `🌐`︲Configuration réseau et paramètres DNS
- Configurer l’adresse IP du client
- Définir le serveur DNS principal comme le serveur AD

<a id="joindre-domaine"></a>
### `🔗`︲Joindre le domaine `descartesbleu.org`
- Depuis le client, joindre le domaine via les propriétés système

---

<!-- Gestion partages de fichiers -->

<a id="gestion-des-partages-de-fichiers"></a>
## `📁`︲Gestion des partages de fichiers

<a id="creation-dossiers-partages"></a>
### `📂`︲Création des dossiers partagés et permissions
- Créer les dossiers
- Appliquer les permissions adéquates pour chaque groupe

<a id="redirection-dossiers-mode-hors-ligne"></a>
### `💾`︲Redirection de dossiers utilisateur et mode hors connexion
- Configurer la redirection des dossiers Documents
- Activer le mode hors connexion si nécessaire

---

<!-- Automatisation PowerShell -->

<a id="automatisation-via-powershell"></a>
## `📜`︲Automatisation via PowerShell

<a id="script-ou-groupes-utilisateurs"></a>
### `⚡`︲Script pour créer des OU, groupes et utilisateurs à partir d'un fichier CSV
- Exemple : `.\CreateUsers.ps1 -CSV users.csv`

<details>
  <summary><strong>💡︲Conseil</strong></summary>
  Vérifie bien la structure du CSV pour éviter les erreurs lors de l’exécution du script.
</details>

---

<!-- GPO -->

<a id="deploiement-de-strategies-de-groupe"></a>
## `🖱️`︲Déploiement de stratégies de groupe (GPO)

<a id="redirection-documents-mappage-firefox"></a>
### `📂`︲Redirection du dossier Documents, mappage lecteurs réseau et déploiement Firefox
- Créer et lier une GPO pour redirection Documents
- Mappage des lecteurs réseau
- Déploiement de Firefox via GPO

---

<!-- Restrictions avancées -->

<a id="restrictions-fonctionnalites-avancees"></a>
## `🔒`︲Restrictions d'accès et fonctionnalités avancées

<a id="limitation-horaires-bureau-bginfo"></a>
### `⏱️`︲Limitation des horaires de connexion, activation du bureau à distance et configuration BgInfo
- Définir les plages horaires pour les utilisateurs
- Activer le bureau à distance
- Installer et configurer BgInfo

---

<!-- Conclusion -->

<a id="conclusion"></a>
## `✅`︲Conclusion

<a id="resume-taches-resultats"></a>
### `📝`︲Résumé des tâches et résultats obtenus
- Installation et configuration du serveur et client
- Création du domaine, utilisateurs et OU
- Gestion des partages et GPO

<a id="impact-configurations"></a>
### `🌟`︲Impact des configurations sur la collaboration et l'organisation
- Facilite la gestion centralisée des utilisateurs
- Optimise le travail collaboratif et la sécurité du réseau
