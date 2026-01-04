# 🔧 GUIDE COMPLET DU CODE - Questions du Professeur

Ce guide t'explique chaque partie du code pour que tu puisses répondre aux questions du professeur.

---

## 📁 STRUCTURE GLOBALE DU NOTEBOOK

```
1. Importation des bibliothèques
2. Chargement des données (4.1M tweets)
3. Exploration et nettoyage
4. Filtrage (USA, date, langue)
5. Preprocessing (regex, stop words)
6. Analyse de sentiments (TextBlob)
7. Visualisations (WordCloud, pie charts)
8. Agrégation par État
9. Régression linéaire (FROM SCRATCH)
10. Visualisations géographiques (cartes)
```

---

# 📦 1. IMPORTATION DES BIBLIOTHÈQUES

```python
import pandas as pd          # Manipulation de données (DataFrames)
import numpy as np           # Calculs numériques
import matplotlib.pyplot as plt  # Graphiques
import seaborn as sns        # Graphiques avancés
from datetime import datetime
import warnings
warnings.filterwarnings('ignore')  # Ignorer les warnings pour lisibilité
```

### ❓ Questions possibles :

**Q: Pourquoi pandas ?**
> R: Pandas permet de manipuler des tableaux de données (DataFrames) efficacement. C'est la librairie standard pour le data science en Python. Elle permet de lire des CSV, filtrer, grouper, fusionner des données.

**Q: Pourquoi numpy ?**
> R: Numpy est optimisé pour les calculs numériques sur des tableaux. Je l'utilise pour les formules mathématiques de la régression linéaire (`np.sum()`, `np.mean()`).

---

# 📥 2. CHARGEMENT DES DONNÉES

```python
def load_twitter_data(filename, max_rows=None):
    """Charge les données Twitter avec gestion robuste des erreurs"""
    data = []
    skipped = 0
    
    with open(filename, 'r', encoding='utf-8', errors='ignore') as f:
        header = f.readline().strip().split(',')
        
        for i, line in enumerate(f):
            try:
                reader = csv.reader([line], quotechar='"', delimiter=',')
                row = next(reader)
                if len(row) >= 5:
                    data.append(row)
            except:
                skipped += 1
                continue
    
    return pd.DataFrame(data, columns=header)
```

### ❓ Questions possibles :

**Q: Pourquoi une fonction personnalisée et pas `pd.read_csv()` ?**
> R: Les fichiers CSV contiennent des tweets avec des caractères spéciaux, des virgules dans le texte, des guillemets mal formatés. La fonction standard `pd.read_csv()` échoue sur ces lignes malformées. Ma fonction personnalisée ignore les lignes problématiques au lieu de planter.

**Q: Que fait `errors='ignore'` ?**
> R: Cela ignore les erreurs d'encodage de caractères (certains tweets ont des emojis ou caractères spéciaux mal encodés).

**Q: Pourquoi `skipped` ?**
> R: Je compte les lignes ignorées pour savoir combien de tweets on perd à cause de problèmes de formatage. C'est une bonne pratique pour évaluer la qualité des données.

---

# 🧹 3. NETTOYAGE AVEC REGEX

```python
import re

def clean_tweet(text):
    """Nettoie un tweet avec regex"""
    # Supprimer les RT
    text = re.sub(r'^RT[\s]+', '', text)
    
    # Supprimer les mentions (@username)
    text = re.sub(r'@[A-Za-z0-9_]+', '', text)
    
    # Supprimer les hashtags
    text = re.sub(r'#', '', text)
    
    # Supprimer les URLs
    text = re.sub(r'https?://\S+|www\.\S+', '', text)
    
    # Convertir en minuscules
    text = text.lower()
    
    # Garder seulement lettres, chiffres et espaces
    text = re.sub(r'[^a-z0-9\s]', '', text)
    
    # Supprimer les espaces multiples
    text = re.sub(r'\s+', ' ', text)
    
    return text.strip()
```

### ❓ Questions possibles :

**Q: Qu'est-ce qu'une expression régulière (regex) ?**
> R: C'est un pattern qui décrit un motif de texte. Par exemple `@[A-Za-z0-9_]+` signifie "un @ suivi de un ou plusieurs caractères alphanumériques ou underscore". Ça capture toutes les mentions Twitter.

**Q: Expliquez `r'^RT[\s]+'` ?**
> R: 
> - `^` = début de la chaîne
> - `RT` = les lettres R et T
> - `[\s]+` = un ou plusieurs espaces
> - Donc ça capture les retweets qui commencent par "RT "

**Q: Pourquoi supprimer les hashtags ?**
> R: Les hashtags comme #Trump ou #Biden biaisent l'analyse de sentiments. On veut analyser le CONTENU du message, pas les hashtags.

**Q: Pourquoi convertir en minuscules ?**
> R: Pour normaliser le texte. "TRUMP" et "trump" doivent être traités comme le même mot.

**Q: Que signifie `[^a-z0-9\s]` ?**
> R: Le `^` à l'intérieur de `[]` signifie "tout SAUF". Donc `[^a-z0-9\s]` = tout ce qui n'est PAS une lettre minuscule, un chiffre ou un espace. On supprime donc les caractères spéciaux.

---

# 🌍 4. DÉTECTION DE LANGUE

```python
from langdetect import detect, LangDetectException

def detect_language(text):
    """Détecte la langue d'un texte"""
    try:
        if pd.isna(text) or text.strip() == '':
            return None
        return detect(str(text))
    except LangDetectException:
        return None

# Appliquer sur tous les tweets
df_trump['language'] = df_trump['tweet'].apply(detect_language)

# Filtrer pour garder uniquement l'anglais
df_trump = df_trump[df_trump['language'] == 'en']
```

### ❓ Questions possibles :

**Q: Comment fonctionne langdetect ?**
> R: C'est un algorithme de machine learning qui analyse les n-grammes (séquences de lettres) du texte et les compare aux patterns statistiques de chaque langue. Il retourne un code ISO (en, fr, es, etc.).

**Q: Pourquoi le try/except ?**
> R: Certains textes très courts ou avec uniquement des emojis ne peuvent pas être classifiés. `LangDetectException` est levée dans ces cas. On retourne None pour les ignorer ensuite.

**Q: Pourquoi garder uniquement l'anglais ?**
> R: TextBlob (analyse de sentiments) est optimisé pour l'anglais. Analyser des tweets en espagnol ou français avec TextBlob donnerait des résultats incorrects.

---

# 🛑 5. SUPPRESSION DES STOP WORDS

```python
from nltk.corpus import stopwords

# Charger les stop words anglais
stop_words = set(stopwords.words('english'))

# Ajouter les mots spécifiques à supprimer
additional_stop_words = {
    'donaldtrump', 'trump', 'donald', 
    'biden', 'joe', 'joebiden', 
    'amp', 'president', 'vote', 'voting', 'election'
}

stop_words = stop_words.union(additional_stop_words)

def remove_stopwords(text):
    words = text.split()
    filtered_words = [word for word in words if word not in stop_words]
    return ' '.join(filtered_words)
```

### ❓ Questions possibles :

**Q: C'est quoi un stop word ?**
> R: Ce sont des mots très fréquents qui n'apportent pas de sens : "the", "is", "a", "an", "and", etc. Ils polluent l'analyse sans apporter d'information.

**Q: Pourquoi supprimer "trump" et "biden" ?**
> R: Parce qu'ils apparaissent dans TOUS les tweets (c'est le sujet). Les garder dominerait les nuages de mots et n'apporterait aucune information intéressante.

**Q: Pourquoi "amp" ?**
> R: C'est un artefact HTML. Le "&" dans les tweets est souvent encodé "&amp;" et devient "amp" après nettoyage.

**Q: Que fait `.union()` ?**
> R: C'est une opération ensembliste. Elle fusionne deux sets (ensembles) en un seul, contenant tous les éléments des deux.

---

# 🎭 6. ANALYSE DE SENTIMENTS (TextBlob)

```python
from textblob import TextBlob

def get_sentiment(text):
    """Calcule la polarité et la subjectivité d'un texte"""
    try:
        blob = TextBlob(str(text))
        return blob.sentiment.polarity, blob.sentiment.subjectivity
    except:
        return 0.0, 0.0

def get_polarity_state(polarity):
    """Détermine si un tweet est positif, négatif ou neutre"""
    if polarity < 0:
        return 'negative'
    elif polarity > 0:
        return 'positive'
    else:
        return 'neutral'

# Appliquer sur les tweets
sentiments = df_trump['tweet_processed'].apply(get_sentiment)
df_trump['polarity'] = sentiments.apply(lambda x: x[0])
df_trump['subjectivity'] = sentiments.apply(lambda x: x[1])
df_trump['pol_state'] = df_trump['polarity'].apply(get_polarity_state)
```

### ❓ Questions possibles :

**Q: Comment TextBlob calcule la polarité ?**
> R: TextBlob utilise un dictionnaire de mots annotés avec leur polarité. Par exemple, "great" = +0.8, "terrible" = -0.8, "good" = +0.7. Il fait la moyenne pondérée des mots du texte.

**Q: C'est quoi la subjectivité ?**
> R: La subjectivité mesure si le texte exprime une OPINION (subjectif = 1) ou un FAIT (objectif = 0).
> - "Trump won the election" → objectif (c'est un fait)
> - "Trump is the best president ever" → subjectif (c'est une opinion)

**Q: Pourquoi 3 catégories (positive, negative, neutral) ?**
> R: C'est une classification standard en NLP. Neutre (polarité = 0) signifie que TextBlob n'a trouvé aucun mot positif ou négatif, ou que les positifs et négatifs s'annulent.

**Q: Quelles sont les limites de TextBlob ?**
> R: 
> 1. Il ne comprend pas le sarcasme ("Great job Trump, ruining the country!" serait classé positif)
> 2. Il ne comprend pas le contexte
> 3. Il est basé sur un dictionnaire anglais, pas adapté au slang Twitter
> 4. Les emojis ne sont pas pris en compte

---

# 📊 7. RÉGRESSION LINÉAIRE (FROM SCRATCH) ⭐

```python
def linear_regression(x, y):
    """
    Calcule la régression linéaire par les moindres carrés.
    Modèle (notation du cours) : y_i = a*x_i + b + ε_i
    """
    n = len(x)
    
    # Calcul des sommes nécessaires
    sum_x = np.sum(x)
    sum_y = np.sum(y)
    sum_xy = np.sum(x * y)
    sum_x2 = np.sum(x ** 2)
    
    # Calcul de la pente (a) selon la formule du cours (page 30)
    # a = [Σ(x_i*y_i) - (Σx_i*Σy_i)/n] / [Σ(x_i²) - (Σx_i)²/n]
    a = (sum_xy - (sum_x * sum_y) / n) / (sum_x2 - (sum_x ** 2) / n)
    
    # Calcul de l'intercept (b) selon la formule du cours
    # b = ȳ - a*x̄
    mean_x = sum_x / n
    mean_y = sum_y / n
    b = mean_y - a * mean_x
    
    # Calcul du coefficient de détermination R² (page 41 du cours)
    y_pred = a * x + b
    ss_res = np.sum((y - y_pred) ** 2)  # Variation inexpliquée (SC_res)
    ss_tot = np.sum((y - mean_y) ** 2)  # Variation totale (SC_tot)
    r2 = 1 - (ss_res / ss_tot)
    
    return a, b, r2
```

### ❓ Questions possibles (LES PLUS IMPORTANTES) :

**Q: Pourquoi "from scratch" et pas sklearn ?**
> R: Pour démontrer que je comprends les mathématiques du cours, exactement comme enseigné (pages 25-30). Utiliser sklearn serait une "boîte noire". Ici, je code directement le modèle : $y_i = ax_i + b + \varepsilon_i$ avec les formules du cours.

**Q: Expliquez la formule de la pente a**

$$a = \frac{\sum x_i y_i - \frac{(\sum x_i)(\sum y_i)}{n}}{\sum x_i^2 - \frac{(\sum x_i)^2}{n}} = \frac{s_{xy}}{s_x^2}$$

> R: C'est exactement la formule du cours (page 30). Elle minimise la somme des carrés des distances.
> - **Numérateur** : covariance entre x et y
> - **Dénominateur** : variance de x
> - **a représente** : le changement en y pour une augmentation unitaire de x

**Q: Expliquez la formule de b**

$$b = \bar{y} - a\bar{x}$$

> R: C'est la formule du cours (page 30). La **droite de régression passe obligatoirement par le point moyen** $(\bar{x}, \bar{y})$.

**Q: C'est quoi la méthode des moindres carrés ?**
> R: C'est la méthode du cours (pages 27-28). On cherche les paramètres **a** et **b** qui minimisent :
> $$D = \sum_{i=1}^{n} \varepsilon_i^2 = \sum_{i=1}^{n} (y_i - b - ax_i)^2$$
> En prenant les dérivées partielles (page 28) et en les posant égales à zéro, on obtient les formules exactes de **a** et **b**.

**Q: C'est quoi R² (coefficient de détermination) ?**

$$R^2 = \frac{\text{Variation expliquée}}{\text{Variation totale}} = 1 - \frac{SC_{res}}{SC_{tot}}$$

> R: C'est la formule du cours (page 41). R² mesure la proportion de la variance de y expliquée par le modèle.
> - **SC_tot** = $\sum(y_i - \bar{y})^2$ = variation totale
> - **SC_res** = $\sum(y_i - \hat{y}_i)^2$ = variation inexpliquée (résidus)
> - R² = 1 → modèle parfait
> - R² = 0 → modèle inutile
> - R² = 0.137 → modèle faible (13.7% expliqué)

**Q: Que signifient vos résultats R² = 0.137 et 0.026 ?**
> R: 
> - **Trump : R² = 0.137** → La polarité Twitter explique 13.7% de la variation des votes. 86.3% reste inexpliquée (autres facteurs).
> - **Biden : R² = 0.026** → La polarité Twitter n'explique que 2.6% des votes. C'est quasi-nul.
> - **Conclusion** : Twitter ne prédit pas efficacement les élections (corrélation faible comme vu page 15 du cours).

**Q: Interprétez votre équation de régression pour Trump**
> R: Selon le modèle $\hat{y} = 160.47x + 44.51$ :
> - **x** = polarité moyenne Twitter par État
> - **y** = pourcentage de votes Trump prédit
> - **a = 160.47** : si polarité augmente de 0.01, votes Trump +1.6%
> - **b = 44.51** : si polarité = 0, Trump aurait ~44.5% (théorique)
> - **IMPORTANT** : R² = 0.137 montre que cette relation est TRÈS FAIBLE → le modèle n'est pas fiable pour prédire

---

# 🗺️ 8. CARTES CHOROPLÈTHES (GeoPandas)

```python
import geopandas as gpd

# Charger le shapefile des États américains
usa = gpd.read_file('data/cb_2018_us_state_500k/cb_2018_us_state_500k.shp')

# Fusionner avec les données de polarité
usa_merged = usa.merge(polarity_by_state, left_on='STUSPS', right_on='state_code')

# Projeter en Albers Equal Area
usa_merged = usa_merged.to_crs(epsg=5070)

# Créer la carte
fig, ax = plt.subplots(figsize=(15, 10))
usa_merged.plot(column='mean_polarity', cmap='RdYlGn', ax=ax, legend=True)
```

### ❓ Questions possibles :

**Q: C'est quoi un shapefile ?**
> R: C'est un format de fichier géospatial qui contient les géométries (polygones) des États américains avec leurs coordonnées. Développé par ESRI pour les SIG (Systèmes d'Information Géographique).

**Q: C'est quoi une carte choroplèthe ?**
> R: C'est une carte où chaque région est colorée selon une valeur numérique. Ici, la couleur représente la polarité moyenne des tweets dans chaque État.

**Q: Pourquoi EPSG:5070 ?**
> R: C'est la projection "Albers Equal Area Conic" optimisée pour les États-Unis continentaux. Elle préserve les surfaces (les États ont leur taille relative correcte) et minimise les déformations pour cette zone géographique.

**Q: Que signifie `cmap='RdYlGn'` ?**
> R: C'est la colormap (palette de couleurs) : Rouge → Jaune → Vert. Rouge = négatif, Jaune = neutre, Vert = positif.

---

# 🔗 9. FUSION DES DONNÉES

```python
def merge_vote_twitter_data(polarity_by_state, vote_data, candidate_name):
    """Fusionne les données Twitter avec les résultats de vote"""
    merged = pd.merge(
        polarity_by_state,
        vote_data,
        left_on='state_code',
        right_on='state_abr',
        how='inner'
    )
    return merged
```

### ❓ Questions possibles :

**Q: C'est quoi `pd.merge()` ?**
> R: C'est comme un JOIN en SQL. On fusionne deux tables sur une colonne commune (ici le code d'État).

**Q: Que signifie `how='inner'` ?**
> R: On garde uniquement les États présents dans LES DEUX tables. Si un État n'a pas de tweets ou pas de résultats de vote, il est exclu.

**Q: Pourquoi fusionner ?**
> R: Pour créer un dataset avec DEUX colonnes :
> - X = polarité moyenne Twitter par État
> - Y = pourcentage de votes réel par État
> - Puis on peut faire la régression linéaire

---

# 📈 10. RÉSUMÉ DES FONCTIONS CLÉS

| Fonction | But | Input | Output |
|----------|-----|-------|--------|
| `load_twitter_data()` | Charger les CSV | filename | DataFrame |
| `clean_tweet()` | Nettoyer le texte | text | text nettoyé |
| `detect_language()` | Détecter la langue | text | 'en', 'fr', etc. |
| `remove_stopwords()` | Supprimer mots vides | text | text filtré |
| `get_sentiment()` | Analyser sentiment | text | (polarité, subjectivité) |
| `get_polarity_state()` | Classifier sentiment | polarité | 'positive'/'negative'/'neutral' |
| `getWordCloud()` | Créer nuage de mots | DataFrame | affiche image |
| `getInfoPolarity()` | Statistiques sentiments | DataFrame | affiche pie chart |
| `linear_regression()` | Régression linéaire | x, y | m, b, r² |
| `scatterplot()` | Visualiser régression | x, y | affiche graphique |

---

# ⚡ RÉPONSES RAPIDES AUX QUESTIONS PIÈGES

**Q: Votre code utilise-t-il du Machine Learning ?**
> R: Oui et non. TextBlob utilise un modèle pré-entraîné pour l'analyse de sentiments. La régression linéaire est une technique de ML supervisé, mais je l'ai implémentée mathématiquement (pas avec sklearn).

**Q: Pourquoi pas Deep Learning (BERT) ?**
> R: BERT serait plus précis pour l'analyse de sentiments, mais beaucoup plus lourd à exécuter sur 4.1M tweets. TextBlob est un compromis raisonnable pour ce volume de données.

**Q: Votre analyse est-elle reproductible ?**
> R: Oui, avec les mêmes données et le même code, on obtient les mêmes résultats. Je n'utilise pas de random seed, mais il n'y a pas de composante aléatoire dans mon analyse.

**Q: Y a-t-il du data leakage ?**
> R: Non, car je filtre les tweets AVANT la fermeture des bureaux de vote. Je n'utilise aucune information postérieure à l'élection pour prédire les résultats.

**Q: Pourquoi les résultats sont "décevants" (R² faible) ?**
> R: C'est en réalité un résultat scientifique IMPORTANT. Montrer que Twitter ne prédit pas les élections est une conclusion honnête et utile. Cela réfute l'hypothèse naïve que les réseaux sociaux peuvent servir de sondages.

---

# 📝 VOCABULAIRE TECHNIQUE À CONNAÎTRE

| Terme | Définition |
|-------|------------|
| **DataFrame** | Tableau de données à 2 dimensions (lignes, colonnes) en pandas |
| **Regex** | Expression régulière, pattern pour matcher du texte |
| **NLP** | Natural Language Processing, traitement du langage naturel |
| **Stop words** | Mots vides sans valeur sémantique (the, is, a...) |
| **Polarité** | Score de sentiment de -1 (négatif) à +1 (positif) |
| **Subjectivité** | Score de 0 (factuel) à 1 (opinion) |
| **Régression** | Modèle qui prédit une variable continue (Y) à partir d'une autre (X) |
| **R² (coefficient de détermination)** | Proportion de variance expliquée par le modèle |
| **Moindres carrés** | Méthode qui minimise la somme des erreurs au carré |
| **Choroplèthe** | Carte colorée selon une variable numérique |
| **Shapefile** | Format de fichier géospatial (polygones, coordonnées) |
| **EPSG** | Code de référence pour les systèmes de coordonnées |
| **From scratch** | Implémenté manuellement sans librairie toute faite |

---

# ✅ CHECKLIST AVANT LA SOUTENANCE

- [ ] Je sais expliquer chaque regex du nettoyage
- [ ] Je comprends la différence polarité vs subjectivité
- [ ] Je peux écrire la formule de m (pente) au tableau
- [ ] Je sais ce que signifie R² = 0.137
- [ ] Je connais les 4 limites de l'étude (biais, bots, etc.)
- [ ] Je sais pourquoi j'ai fait "from scratch"
- [ ] Je comprends pourquoi Twitter ne prédit pas les élections

**Bonne chance ! 🚀**
