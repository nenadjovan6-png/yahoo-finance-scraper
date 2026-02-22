

https://github.com/user-attachments/assets/8f99cfd9-f037-4acd-a9a3-715d04417a68


[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)([https://colab.research.google.com/drive/1Ib4H27ScR0PXdPGVY6cone8xNxlnfcOQ#scrollTo=wmQKva1JhlFH])
# 📊 Analyse des Actions les Plus Actives du Marché Boursier : Une Approche par Web Scraping et Traitement de Données

# 📈 Yahoo Finance Scraper — Most Active Stocks

> **De la donnée brute à l'intelligence financière** : un outil automatisé pour collecter, nettoyer et visualiser les actions les plus actives du marché américain.

---

## 📌 Description et objectif du projet

Ce projet est un **outil de web scraping entièrement automatisé** qui extrait les données boursières en temps réel depuis [Yahoo! Finance](https://finance.yahoo.com/markets/stocks/most-active/), plus précisément la page des **"Most Active Stocks"** — les 200 actions présentant le volume d'échange quotidien le plus élevé sur le marché américain.

### 🎯 Pourquoi ce projet ?

L'investissement est souvent perçu comme réservé à une élite disposant des bons outils et des bonnes informations. Ce projet a une double ambition :

1. **Démocratiser l'accès à la donnée financière** en rendant les informations boursières accessibles à tous, sans abonnement payant.
2. **Servir de support pédagogique** pour apprendre à collecter, traiter et analyser des données réelles avec Python.

### 📊 Quelles données sont collectées ?

Pour chaque action, le scraper extrait automatiquement :

| Champ | Description |
|---|---|
| `Stock ticker` | Symbole boursier (ex: NVDA, AAPL) |
| `Stock name` | Nom complet de l'entreprise |
| `Last price of stock` | Dernier prix coté en USD |
| `Stock change` | Variation absolue du jour (ex: -4.19) |
| `Stock percentage change` | Variation en pourcentage (ex: -2.24%) |
| `Volume` | Nombre de titres échangés dans la journée |
| `Average volume over 3 months` | Volume moyen sur 3 mois (référence) |
| `Market cap` | Capitalisation boursière totale |
| `PE ratio` | Ratio Cours/Bénéfice |

---

## 🗂️ Structure du projet

```
Projet-Yahoo-Finance-Scraping/
│
├── Projet_Technique_scraping.py   # Script principal automatisé
├── Projet_Technique_scraping.ipynb # Notebook Jupyter (version détaillée/pédagogique)
│
├── requirements.txt               # Dépendances Python
├── .gitignore                     # Fichiers exclus de Git
└── README.md                      # Ce fichier
```

---

## ⚙️ Instructions d'installation

### 1. Cloner le dépôt

```bash
git clone https://github.com/votre-username/yahoo-finance-scraper.git
cd yahoo-finance-scraper
```

### 2. Créer un environnement virtuel (recommandé)

```bash
python -m venv venv
source venv/bin/activate        # Linux / Mac
venv\Scripts\activate           # Windows
```

### 3. Installer les dépendances

```bash
pip install -r requirements.txt
```

---

## 🚀 Exemples d'utilisation

### Lancer le scraper automatisé

```bash
python Projet_Technique_scraping.py
```

**Résultat attendu dans le terminal :**
```
--- Démarrage du Scraper Automatisé ---
✅ Autorisation Robots.txt accordée pour : https://finance.yahoo.com/most-active
📥 Téléchargement : https://finance.yahoo.com/most-active?count=100&offset=0...
⏳ Pause éthique de 3 secondes...
📥 Téléchargement : https://finance.yahoo.com/most-active?count=100&offset=100...
⏳ Pause éthique de 3 secondes...
✅ Succès ! 50 actions récupérées.
📁 Données sauvegardées dans : data/yahoo_stocks_2026-02-16.csv
```

### Utiliser le notebook Jupyter (version pédagogique)

```bash
jupyter notebook Projet_Technique_scraping.ipynb
```

Le notebook détaille chaque étape avec des explications, des exemples intermédiaires et des visualisations.

---

## 🧠 Explication détaillée du code

### Étape 1 — Configuration et imports

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time, os, urllib.robotparser
from datetime import datetime

BASE_URL = "https://finance.yahoo.com/most-active"
FILENAME = f"yahoo_stocks_{datetime.now().strftime('%Y-%m-%d')}.csv"
```

Avant toute chose, on importe les bibliothèques nécessaires et on définit les constantes globales. Le nom du fichier CSV inclut la date du jour pour conserver un historique automatique à chaque exécution.

**Résultat :** Un environnement de travail structuré, avec des paramètres centralisés faciles à modifier.

**Importance :** Une bonne configuration en amont évite de "coder en dur" des valeurs éparpillées dans le script — c'est la base d'un outil réutilisable et maintenable.

---

### Étape 2 — Headers éthiques

```python
HEADERS = {
    'User-Agent': 'Mozilla/5.0 ... (Student Project; Contact: email@gmail.com)',
    'Accept-Language': 'en-US,en;q=0.9',
}
```

Lorsqu'un programme accède à un site web, le serveur voit la requête. Sans header, la requête ressemble à celle d'un robot malveillant et peut être bloquée. En déclarant un `User-Agent` honnête avec ses coordonnées, on s'identifie de façon transparente.

**Résultat :** Le serveur Yahoo reçoit notre requête et la traite normalement, sans nous bloquer.

**Importance :** C'est à la fois une bonne pratique technique (éviter les blocages) et une démarche éthique (honnêteté vis-à-vis du site).

---

### Étape 3 — Vérification du fichier robots.txt

```python
def check_robots_txt(url, user_agent):
    rp = urllib.robotparser.RobotFileParser()
    rp.set_url(ROBOTS_URL)
    rp.read()
    allowed = rp.can_fetch(user_agent, url)
    if allowed:
        print(f"✅ Autorisation Robots.txt accordée pour : {url}")
    return allowed
```

Chaque site web dispose d'un fichier `robots.txt` (ex: `https://finance.yahoo.com/robots.txt`) qui indique quelles pages les robots peuvent ou ne peuvent pas visiter. Le respecter est une obligation légale et éthique.

**Résultat :** `✅ Autorisation Robots.txt accordée pour : https://finance.yahoo.com/most-active` — la page cible est bien autorisée.

**Importance :** Ne pas vérifier le `robots.txt` expose à des poursuites légales et à l'IP ban. C'est la première chose à faire avant tout scraping.

---

### Étape 4 — Fonction de nettoyage `parse_value`

```python
def parse_value(value_str):
    if value_str.endswith('T'):
        return float(value_str[:-1]) * 1_000_000_000_000
    elif value_str.endswith('B'):
        return float(value_str[:-1]) * 1_000_000_000
    elif value_str.endswith('M'):
        return float(value_str[:-1]) * 1_000_000
```

Yahoo Finance affiche les grands nombres avec des abréviations : `4.45T` (trillions), `161.88M` (millions). Ces chaînes de caractères ne peuvent pas être comparées directement. Il faut les convertir en valeurs numériques exploitables.

**Résultat :** `"4.45T"` → `4 450 000 000 000` | `"161.88M"` → `161 880 000`

**Importance :** Sans cette conversion, il est impossible de trier les actions par capitalisation ou volume. C'est le cœur du nettoyage des données.

---

### Étape 5 — Téléchargement de la page avec gestion d'erreurs

```python
def get_page_data(offset=0):
    url = f"{BASE_URL}?count=100&offset={offset}"
    response = requests.get(url, headers=HEADERS, timeout=10)
    response.raise_for_status()  # Lève une exception si erreur HTTP
    soup = BeautifulSoup(response.text, 'html.parser')
```

`requests.get()` envoie une requête HTTP GET au serveur. Le paramètre `offset` permet la **pagination** : Yahoo n'affiche que 100 résultats par page, donc `offset=0` donne les rangs 1-100 et `offset=100` donne les rangs 101-200. `raise_for_status()` stoppe proprement le script en cas d'erreur (404, 500, etc.) plutôt que de continuer avec des données vides.

**Résultat :** Le contenu HTML de la page est téléchargé. Un code de statut `200` confirme le succès.

**Importance :** La gestion d'erreurs rend l'outil robuste. Sans elle, le script peut planter silencieusement et produire des données incomplètes sans que l'utilisateur s'en rende compte.

---

### Étape 6 — Parsing HTML avec BeautifulSoup

```python
table = soup.find('table')
rows = table.find('tbody').find_all('tr')
```

BeautifulSoup "parse" (analyse) le HTML brut pour le rendre navigable comme un arbre. On cherche d'abord la balise `<table>` (le tableau de données principal), puis toutes les lignes `<tr>` dans son corps `<tbody>`. Chaque ligne `<tr>` correspond à une action.

**Résultat :** Une liste de 25 à 100 objets `<tr>`, chacun contenant toutes les informations d'une action.

**Importance :** C'est l'étape centrale du scraping. Sans parsing, le HTML est une chaîne de texte illisible de plusieurs millions de caractères.

---

### Étape 7 — Extraction structurée avec `parse_stocks`

```python
def parse_stocks(tr_class_tag):
    td_tag = tr_class_tag.find_all('td')
    ticker_name = td_tag[0].find('a', attrs={'data-testid': 'table-cell-ticker'}).text.strip()
    price_span = td_tag[3].find('span', attrs={'data-testid': 'change'})
    # ... etc.
    return {'Stock ticker': ticker_name, 'Last price of stock': price_tag, ...}
```

Chaque cellule `<td>` d'une ligne contient une information spécifique. On utilise les attributs `data-testid` (plus stables que les classes CSS qui changent fréquemment) pour cibler précisément chaque donnée.

**Résultat :**
```python
{
  'Stock ticker': 'NVDA',
  'Stock name': 'NVIDIA Corporation',
  'Last price of stock': 182.78,
  'Volume': 161888000,
  'Market cap': 4450000000000,
  'PE ratio': 45.25
}
```

**Importance :** Transformer du HTML non structuré en dictionnaire Python propre est l'objectif final du scraping. Ce dictionnaire peut ensuite être analysé, stocké ou exporté facilement.

---

### Étape 8 — Boucle de pagination automatisée

```python
for offset in [0, 100]:
    data = get_page_data(offset)
    all_stocks.extend(data)
    time.sleep(3)  # Pause éthique
```

Yahoo affiche les données par pages de 100. Pour obtenir les 200 actions, il faut faire deux requêtes avec des offsets différents. Le `time.sleep(3)` est une **pause éthique** obligatoire entre les requêtes.

**Résultat :** Une liste consolidée de 200 actions (ou moins selon ce que Yahoo retourne).

**Importance :** Sans pagination, on ne récupère qu'une partie des données. Sans la pause, on risque de surcharger le serveur et de se faire bloquer — c'est contraire aux bonnes pratiques et potentiellement illégal.

---

### Étape 9 — Sauvegarde en CSV et analyse Pandas

```python
df = pd.DataFrame(all_stocks)
df.to_csv(filepath, index=False)

# Tris pour analyse
order_by_market_cap = df.sort_values('Market cap', ascending=False)
order_by_volume = df.sort_values('Volume', ascending=False)
order_by_price = df.sort_values('Last price of stock')
order_by_pe = df.sort_values('PE ratio', ascending=False)
```

Le CSV offre une persistance des données : les résultats sont disponibles après l'exécution, avec un nom horodaté pour conserver l'historique. Les DataFrames Pandas permettent ensuite d'analyser les données selon différents critères d'investissement.

**Résultat :** Un fichier `data/yahoo_stocks_2026-02-XX.csv` contenant toutes les données, et plusieurs vues triées.

**Importance :** C'est la livraison finale de l'outil. Un investisseur peut instantanément identifier les meilleures opportunités selon son profil : cherche-t-il les grandes capitalisations stables (NVDA, AAPL) ? Les penny stocks accessibles (PLUG à 1.89$) ? Les actions sous-évaluées (PE ratio bas) ?

---

### Étape 10 — Visualisations Matplotlib

```python
order_by_vol_df.plot(x='Stock ticker', y='Volume', kind='bar', figsize=(20,10), color='orange')
plt.show()
```

Un graphique en barres permet de visualiser immédiatement les disparités entre actions — ce qu'un tableau de chiffres ne rend pas aussi clairement.

**Résultats obtenus :**
- **Graphique Volume (orange)** → NVDA domine massivement avec 161M de titres échangés, loin devant RIVN (127M) et RIG (98M). Quelques géants captent la majorité de l'attention du marché.
- **Graphique PE Ratio (bleu)** → TSLA (386) et PLTR (208) ont des ratios extrêmement élevés, signe d'attentes de croissance importantes ou d'une possible surévaluation.
- **Graphique Prix (rose)** → Disparité totale entre PLUG (1.89$ — penny stock) et TSLA (417$). Le prix seul ne reflète pas la taille de l'entreprise.

**Importance :** La visualisation transforme des données brutes en **insights actionnables**. C'est la dernière étape du pipeline complet : Web → Python → CSV → Décision.

---

## 📦 Liste des dépendances

```
requests==2.31.0
beautifulsoup4==4.12.2
pandas==2.1.0
matplotlib==3.7.2
```

Installez-les via :

```bash
pip install -r requirements.txt
```

---

## ⚖️ Mentions légales et éthique

Ce projet respecte scrupuleusement les règles suivantes :

- ✅ **robots.txt vérifié** avant chaque session de scraping (`finance.yahoo.com/most-active` est autorisé)
- ✅ **Pauses entre requêtes** : minimum 3 secondes entre chaque appel serveur
- ✅ **Identification transparente** dans le User-Agent (nom + contact)
- ✅ **Aucune surcharge serveur** : maximum 2 requêtes par session
- ✅ **Données publiques uniquement** : toutes les informations sont librement accessibles sur Yahoo Finance
- ✅ **Usage éducatif et non commercial**
- ⚠️ Ne jamais stocker d'identifiants dans Git (`.gitignore` configuré en conséquence)
- ⚠️ Ce projet est fourni à titre informatif uniquement et ne constitue pas un conseil en investissement financier

> **Note :** Yahoo Finance propose une API officielle (`yfinance`) qui peut être préférable pour certains usages. Ce projet utilise le scraping direct à des fins pédagogiques, pour illustrer les techniques de parsing HTML.

---

## 👤 Auteurs : M'Bemba CAMARA , Nenad JOVANOVIC

Projet réalisé dans le cadre du cours de **Technique de Programmation**.
