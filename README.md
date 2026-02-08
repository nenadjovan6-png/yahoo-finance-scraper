# 📊 Analyse des Actions les Plus Actives du Marché Boursier : Une Approche par Web Scraping et Traitement de Données

---

## 📌 Introduction

Dans un contexte de globalisation financière et de démocratisation de l'investissement, l'accès à des données boursières actualisées et pertinentes constitue un enjeu majeur pour les investisseurs individuels et les analystes financiers. Ce projet s'inscrit dans une démarche d'**économie numérique appliquée** visant à automatiser la collecte, le traitement et l'analyse des données des actions les plus actives du marché, définies par leur volume d'échanges journalier.

L'objectif principal est de développer une méthodologie reproductible pour l'extraction et l'exploitation de données financières, tout en abordant les défis techniques, éthiques et réglementaires du web scraping. Ce travail contribue à la littérature sur l'**accès aux données financières** et leur utilisation pour une prise de décision éclairée.

---

## ❓ Problématique

Comment automatiser la collecte de données boursières sur les actions les plus actives, tout en garantissant la robustesse technique, le respect des contraintes légales et éthiques, et en permettant une analyse statistique pertinente pour des applications en finance de marché ?

Cette problématique se décline en trois axes de recherche :
1. **Technique** : Développement d'un pipeline d'extraction de données résilient face aux changements de structure des sites web
2. **Éthique et légale** : Conformité aux politiques de scraping et minimisation de l'impact sur les serveurs cibles
3. **Analytique** : Transformation des données brutes en indicateurs exploitables pour l'analyse financière

---

## 📚 Revue de Littérature

### 2.1 Web Scraping en Finance
- **Krotov et al. (2020)** : Cadre éthique pour le web scraping dans la recherche en systèmes d'information
- **Micheli et al. (2018)** : Approches légales du scraping de données financières
- **Chen et al. (2021)** : Applications du machine learning sur données scrapées pour la prédiction boursière

### 2.2 Indicateurs d'Activité Boursière
- **Chordia et al. (2001)** : Relation entre volume d'échanges et volatilité des prix
- **Gervais et al. (2001)** : Persistance des volumes anormaux et efficience des marchés
- **Lee et Swaminathan (2000)** : Dynamique des actions les plus actives et momentum

---

## 🧪 Méthodologie

### 3.1 Conception de la Collecte de Données
#### 3.1.1 Architecture du Système
- Approche modulaire avec séparation des préoccupations
- Gestion robuste des erreurs et reprise sur incidents
- Horodatage systématique pour l'analyse longitudinale

#### 3.1.2 Conformité Éthique et Légale
- Vérification automatique du fichier `robots.txt`
- Identification transparente (User-Agent personnalisé)
- Respect des délais entre requêtes (rate limiting)

### 3.2 Traitement des Données
#### 3.2.1 Nettoyage et Standardisation
```python
# Algorithme de conversion des formats de valeurs
def normaliser_valeurs_financieres(valeur):
    """
    Convertit les notations financières (M, B, T) en valeurs numériques
    Applique un traitement robuste aux valeurs manquantes (NA)
    """
