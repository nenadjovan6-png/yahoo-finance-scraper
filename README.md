# 📊 Projet de Scraping : Yahoo Finance - Actions les plus actives

Ce projet a pour objectif de récupérer automatiquement les données des **actions les plus actives** du jour sur Yahoo Finance à l'aide du web scraping. Il est conçu pour être éducatif, éthique et facile à utiliser, même pour les débutants en programmation ou en finance.

---

## 🎯 Objectifs du projet

- **Apprendre le web scraping** de manière pratique et responsable.
- **Extraire des données financières** utiles pour les investisseurs débutants.
- **Promouvoir l'autonomie financière** en rendant l'information accessible.
- **Automatiser la collecte de données** et générer des fichiers structurés (CSV, DataFrame).

---

## 🚀 Fonctionnalités

- ✅ Récupération des **200 actions les plus actives** (volume d'échanges journalier)
- ✅ Extraction des informations clés :
  - Symbole boursier (ticker)
  - Nom de l'entreprise
  - Prix actuel
  - Variation en valeur et en pourcentage
  - Volume échangé
  - Capitalisation boursière
  - Ratio Prix/Bénéfice (PE)
- ✅ **Vérification éthique** du fichier `robots.txt`
- ✅ Gestion des erreurs et robustesse face aux changements de structure du site
- ✅ Export des données au format **CSV** avec horodatage
- ✅ Analyse et visualisation simplifiée avec **Pandas** et **Matplotlib**
- ✅ Pauses entre les requêtes pour respecter les serveurs de Yahoo Finance

---

## 🛠️ Technologies utilisées

- **Python 3**
- **Bibliothèques principales :**
  - `requests` – pour télécharger les pages web
  - `BeautifulSoup4` – pour analyser le HTML
  - `pandas` – pour manipuler les données
  - `matplotlib` – pour les visualisations basiques
- **Environnement :** Google Colab (ou tout environnement Python)

---

## 📦 Installation et configuration

1. **Cloner le dépôt** (ou télécharger le notebook Colab) :
```bash
git clone https://github.com/ton-utilisateur/ton-repo.git
