# 🔧 GUIDE COMPLET DU CODE - Questions du Professeur

Ce guide t'explique chaque partie du code pour que tu puisses répondre aux questions du professeur.

---

## 📁 STRUCTURE GLOBALE DU NOTEBOOK

```
1. Importation des bibliothèques
2. Chargement des données (~1.7M tweets)
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

## 🎯 POURQUOI CE PROJET EST PERTINENT

### Contexte scientifique
L'élection présidentielle américaine de 2020 (Trump vs Biden) a généré une activité massive sur Twitter. La question scientifique est : **les réseaux sociaux peuvent-ils servir d'indicateur prédictif des résultats électoraux ?**

### Hypothèse de départ
> Si les sentiments exprimés sur Twitter reflètent l'opinion publique réelle, alors il devrait exister une corrélation positive entre la polarité moyenne des tweets par État et le pourcentage de votes obtenu par chaque candidat.

### Méthodologie scientifique
1. **Collecte** : Tweets avec hashtags #DonaldTrump et #JoeBiden
2. **Traitement** : Nettoyage, filtrage, analyse de sentiments
3. **Analyse** : Régression linéaire pour quantifier la corrélation
4. **Conclusion** : Validation ou réfutation de l'hypothèse

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

**Q: Quelle est la différence entre pandas et numpy ?**
> R: 
> - **Numpy** : Travaille avec des arrays homogènes (tous les éléments du même type). Optimisé pour les calculs mathématiques vectorisés.
> - **Pandas** : Travaille avec des DataFrames (colonnes de types différents). Optimisé pour la manipulation de données tabulaires (filtrage, groupby, merge).
> - En pratique : pandas utilise numpy en interne pour les calculs.

**Q: Pourquoi `warnings.filterwarnings('ignore')` ?**
> R: Certaines opérations génèrent des warnings non critiques (ex: pandas avertit de modifications futures). Je les masque pour la lisibilité du notebook, mais en production je les laisserais activés pour détecter d'éventuels problèmes.

**Q: Pourquoi seaborn EN PLUS de matplotlib ?**
> R: Seaborn est construit sur matplotlib et offre des visualisations statistiques plus esthétiques avec moins de code. Par exemple, `sns.heatmap()` pour les matrices de corrélation ou des graphiques avec intervalles de confiance automatiques.

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
> R: Cela ignore les erreurs d'encodage de caractères (certains tweets ont des emojis ou caractères spéciaux mal encodés en UTF-8).

**Q: Pourquoi `skipped` ?**
> R: Je compte les lignes ignorées pour savoir combien de tweets on perd à cause de problèmes de formatage. C'est une bonne pratique pour évaluer la qualité des données et la représentativité de l'échantillon final.

**Q: Que fait `quotechar='"'` ?**
> R: Dans un CSV, les champs contenant des virgules sont entourés de guillemets. Par exemple : `"Hello, world",value2`. Le `quotechar` indique au parser que les guillemets délimitent un champ unique, même s'il contient des virgules.

**Q: Pourquoi `len(row) >= 5` ?**
> R: Chaque ligne doit avoir au minimum 5 colonnes (les colonnes essentielles du dataset). Si une ligne mal parsée a moins de colonnes, on l'ignore car elle est corrompue.

**Q: C'est quoi `encoding='utf-8'` ?**
> R: UTF-8 est un encodage de caractères qui supporte tous les caractères Unicode (emojis, accents, caractères chinois, etc.). Les tweets contiennent souvent des emojis, donc UTF-8 est nécessaire.

**Q: Quelle est la complexité temporelle de cette fonction ?**
> R: O(n) où n = nombre de lignes. On parcourt chaque ligne une seule fois. C'est optimal pour la lecture de fichier.

### 📊 Données chargées

| Dataset | Volume | Colonnes clés |
|---------|--------|---------------|
| `hashtag_donaldtrump.csv` | ~958 580 tweets | tweet, created_at, user_location, state |
| `hashtag_joebiden.csv` | ~768 423 tweets | tweet, created_at, user_location, state |
| `ap_votes.csv` | 51 États | state, trump_votes, biden_votes, total_votes |

**Q: Pourquoi ~190k tweets de différence entre Trump et Biden ?**
> R: Trump était beaucoup plus actif sur Twitter et générateur de controverses, ce qui provoquait plus de discussions. Ce n'est pas un biais de collecte mais une caractéristique du phénomène. Les deux échantillons sont suffisamment grands (>750k) pour des analyses statistiques fiables.

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
> - `r''` = raw string (les backslash ne sont pas des caractères d'échappement Python)
> - `^` = ancre de début de chaîne (le pattern doit être au DÉBUT)
> - `RT` = les lettres littérales R et T
> - `[\s]` = classe de caractères pour les espaces (espace, tab, newline)
> - `+` = quantificateur "un ou plusieurs"
> - **Résultat** : capture les retweets qui commencent par "RT "

**Q: Expliquez `r'@[A-Za-z0-9_]+'` en détail**
> R:
> - `@` = le caractère @ littéral
> - `[A-Za-z0-9_]` = classe de caractères :
>   - `A-Z` = lettres majuscules de A à Z
>   - `a-z` = lettres minuscules de a à z  
>   - `0-9` = chiffres de 0 à 9
>   - `_` = underscore
> - `+` = un ou plusieurs caractères de cette classe
> - **Résultat** : capture @john_doe123, @realDonaldTrump, etc.

**Q: Expliquez `r'https?://\S+|www\.\S+'`**
> R:
> - `https?` = "http" suivi optionnellement de "s" (le `?` rend le "s" optionnel)
> - `://` = les caractères littéraux "://"
> - `\S+` = un ou plusieurs caractères NON-espace (l'URL jusqu'au prochain espace)
> - `|` = opérateur OU (alternative)
> - `www\.` = "www." (le backslash échappe le point car `.` signifie "n'importe quel caractère" en regex)
> - **Résultat** : capture http://..., https://..., et www....

**Q: Pourquoi supprimer les hashtags ?**
> R: Les hashtags comme #Trump ou #Biden biaisent l'analyse de sentiments. On veut analyser le CONTENU du message, pas les hashtags qui sont déjà connus (c'est le critère de sélection des tweets).

**Q: Pourquoi convertir en minuscules ?**
> R: Pour normaliser le texte. "TRUMP", "Trump" et "trump" doivent être traités comme le même mot. Cela réduit la dimensionnalité du vocabulaire et améliore la cohérence de l'analyse.

**Q: Que signifie `[^a-z0-9\s]` ?**
> R: Le `^` à l'INTÉRIEUR de `[]` signifie "tout SAUF". Donc `[^a-z0-9\s]` = tout ce qui n'est PAS une lettre minuscule, un chiffre ou un espace. On supprime donc la ponctuation et les caractères spéciaux.

**Q: Pourquoi `re.sub()` et pas `str.replace()` ?**
> R: `str.replace()` ne fait que des remplacements littéraux. `re.sub()` permet des patterns flexibles. Par exemple, `str.replace()` ne peut pas supprimer "toutes les mentions" en une seule opération.

**Q: L'ordre des opérations de nettoyage est-il important ?**
> R: Oui ! Par exemple :
> 1. Supprimer les URLs AVANT de convertir en minuscules (sinon les URLs sont altérées)
> 2. Supprimer les mentions AVANT de supprimer les caractères spéciaux (sinon le @ est supprimé mais pas le username)
> 3. Convertir en minuscules AVANT de supprimer les caractères non-alphabétiques

**Q: Que fait `.strip()` ?**
> R: Supprime les espaces au début et à la fin de la chaîne. Après toutes les substitutions, il peut rester des espaces en début/fin.

### 🔍 Exemples concrets de nettoyage

| Tweet original | Tweet nettoyé |
|----------------|---------------|
| `RT @CNN: Trump says...` | `trump says` |
| `I love #Biden! https://t.co/abc` | `i love biden` |
| `@realDonaldTrump is TERRIBLE!!!` | `is terrible` |
| `Vote for Biden 2020 🇺🇸` | `vote for biden 2020` |

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
> R: L'algorithme utilise les **n-grammes de caractères** (séquences de n lettres consécutives). Chaque langue a une distribution statistique unique de n-grammes.
> 
> **Exemple** : Le trigramme "the" est très fréquent en anglais mais rare en français. "tion" est commun aux deux mais "que" est plus français.
> 
> L'algorithme compare les n-grammes du texte aux profils statistiques de ~55 langues et retourne celle avec la meilleure correspondance (distance de Naive Bayes).

**Q: C'est quoi un n-gramme ?**
> R: Une séquence de n éléments consécutifs. Pour le mot "hello" :
> - **Unigrammes** (n=1) : h, e, l, l, o
> - **Bigrammes** (n=2) : he, el, ll, lo
> - **Trigrammes** (n=3) : hel, ell, llo
> 
> Langdetect utilise principalement des trigrammes car ils capturent bien les patterns linguistiques.

**Q: Pourquoi le try/except ?**
> R: Certains textes très courts (1-2 mots), avec uniquement des emojis, ou des URLs ne permettent pas une classification fiable. `LangDetectException` est levée dans ces cas. On retourne None pour les filtrer ensuite.

**Q: Pourquoi garder uniquement l'anglais ?**
> R: **TextBlob** (analyse de sentiments) est entraîné sur un corpus anglais. Son dictionnaire de sentiments ne contient que des mots anglais. Analyser "C'est génial !" avec TextBlob donnerait une polarité de 0 (neutre) car les mots français ne sont pas dans son dictionnaire.

**Q: Quelle est la précision de langdetect ?**
> R: Environ 99% sur des textes de plus de 50 caractères. La précision diminue pour les textes courts (<20 caractères) car il y a moins de n-grammes à analyser. C'est une limite connue pour les tweets courts.

**Q: Pourquoi `pd.isna(text)` ?**
> R: Vérifie si la valeur est NaN (Not a Number) ou None. Certains tweets peuvent avoir un champ texte vide dans le CSV. Appeler `detect()` sur None provoquerait une erreur.

**Q: Pourquoi `.apply()` ?**
> R: C'est la méthode pandas pour appliquer une fonction à chaque élément d'une colonne (comme un `map` en programmation fonctionnelle). Plus efficace que de boucler avec `for`.

**Q: Combien de tweets sont perdus par ce filtrage ?**
> R: Typiquement 15-25% des tweets ne sont pas en anglais (espagnol, français, etc.) ou ne sont pas détectables. C'est un compromis nécessaire pour la qualité de l'analyse de sentiments.

### 📊 Distribution typique des langues (avant filtrage)

| Langue | Code | Pourcentage estimé |
|--------|------|-------------------|
| Anglais | en | ~75-80% |
| Espagnol | es | ~8-10% |
| Indéterminé | None | ~5-8% |
| Autres | fr, de, pt... | ~5-7% |

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
> R: Ce sont des mots très fréquents qui n'apportent pas de valeur sémantique pour l'analyse : "the", "is", "a", "an", "and", "of", "to", "in", etc. Ils représentent ~20-30% des mots d'un texte mais n'apportent aucune information distinctive.

**Q: Pourquoi NLTK et pas une liste manuelle ?**
> R: NLTK fournit une liste de ~179 stop words anglais, validée par la communauté NLP. Créer une liste manuelle risquerait d'oublier des mots ou d'en inclure de mauvais.

**Q: Quels sont les stop words de NLTK ?**
> R: Articles (a, an, the), prépositions (in, on, at, to), conjonctions (and, but, or), pronoms (I, you, he, she, it, we, they), verbes auxiliaires (is, are, was, were, be, been), etc.

**Q: Pourquoi supprimer "trump" et "biden" ?**
> R: Parce qu'ils apparaissent dans TOUS les tweets (c'est le critère de sélection). Les garder dominerait les nuages de mots et n'apporterait aucune information nouvelle. On sait déjà que les tweets parlent de Trump et Biden.

**Q: Pourquoi "amp" ?**
> R: C'est un artefact d'encodage HTML. Le caractère "&" est souvent encodé "&amp;" dans les données web. Après nettoyage du caractère "&", il reste "amp" qui pollue l'analyse.

**Q: Pourquoi "vote", "election", "president" ?**
> R: Ces mots sont communs aux deux candidats et n'apportent pas d'information distinctive. Ils apparaissent dans presque tous les tweets politiques.

**Q: Que fait `.union()` ?**
> R: C'est une opération ensembliste. Elle crée un nouveau set contenant tous les éléments du set original PLUS tous les éléments du set ajouté. `{a, b}.union({c, d})` → `{a, b, c, d}`

**Q: Pourquoi un `set` et pas une `list` ?**
> R: La recherche dans un set est O(1) en moyenne (table de hachage), contre O(n) pour une liste. Avec ~200 stop words et des millions de mots à vérifier, c'est une optimisation importante.

**Q: Que fait la list comprehension `[word for word in words if word not in stop_words]` ?**
> R: C'est équivalent à :
> ```python
> filtered_words = []
> for word in words:
>     if word not in stop_words:
>         filtered_words.append(word)
> ```
> Mais en une ligne, plus pythonique et légèrement plus rapide.

**Q: Pourquoi `.split()` sans argument ?**
> R: Sans argument, `.split()` divise sur TOUS les types d'espaces (espace, tab, newline, espaces multiples) et ignore les espaces vides. C'est plus robuste que `.split(' ')`.

### 🔍 Exemples de filtrage

| Avant | Après |
|-------|-------|
| "the president is speaking today" | "speaking today" |
| "trump and biden debate on economy" | "debate economy" |
| "i think this is a great election" | "think great" |

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
> R: TextBlob utilise un **lexique de sentiments** (dictionnaire de mots annotés). Chaque mot a un score de polarité pré-défini basé sur des annotations humaines.
> 
> **Exemples de scores** :
> - "excellent" → +1.0
> - "good" → +0.7
> - "bad" → -0.7
> - "terrible" → -1.0
> - "the" → 0.0 (neutre)
> 
> La polarité finale est la **moyenne pondérée** des polarités de chaque mot, avec des modificateurs pour la négation ("not good" → négatif) et l'intensification ("very good" → plus positif).

**Q: D'où vient le dictionnaire de sentiments de TextBlob ?**
> R: TextBlob utilise le lexique **Pattern** (développé par l'Université d'Anvers). Il contient ~2900 adjectifs anglais annotés manuellement avec leur polarité et subjectivité.

**Q: C'est quoi la subjectivité ?**
> R: La subjectivité mesure si le texte exprime une **OPINION** (subjectif = 1) ou un **FAIT** (objectif = 0).
> 
> | Exemple | Subjectivité | Explication |
> |---------|--------------|-------------|
> | "Trump won the election" | ~0.0 | C'est un fait vérifiable |
> | "Trump is the best president" | ~0.9 | C'est une opinion |
> | "The economy grew by 3%" | ~0.1 | Fait avec chiffre |
> | "I love this candidate" | ~1.0 | Opinion personnelle |

**Q: Comment la subjectivité est-elle calculée ?**
> R: Comme la polarité, chaque mot du lexique a aussi un score de subjectivité. Les mots factuels (verbes d'action, noms concrets) ont une subjectivité basse. Les adjectifs évaluatifs et les expressions d'opinion ont une subjectivité haute.

**Q: Pourquoi 3 catégories (positive, negative, neutral) ?**
> R: C'est une classification standard en NLP appelée **sentiment ternaire**. 
> - **Positif** (polarité > 0) : opinion favorable
> - **Négatif** (polarité < 0) : opinion défavorable  
> - **Neutre** (polarité = 0) : pas de sentiment détecté, ou positifs/négatifs équilibrés

**Q: Un tweet neutre, c'est quoi exactement ?**
> R: Trois cas possibles :
> 1. **Aucun mot de sentiment** : "Trump spoke at the rally" → pas de mot avec score
> 2. **Équilibre** : "Good speech but bad policy" → +0.7 et -0.7 = 0
> 3. **Mots inconnus** : argot, acronymes, ou typos non reconnus par le lexique

**Q: Quelles sont les LIMITES de TextBlob ?**
> R: 
> 1. **Sarcasme non détecté** : "Great job Trump, ruining the country!" → classé positif (à cause de "great")
> 2. **Contexte ignoré** : "Trump is not bad" → peut être mal interprété
> 3. **Argot Twitter** : "lit", "goat", "slay" → non reconnus ou mal interprétés
> 4. **Emojis ignorés** : 😡🤬 ne sont pas analysés
> 5. **Comparaisons implicites** : "Biden is better" → "better" est positif mais pour qui ?
> 6. **Négation complexe** : "I don't think he's not good" → difficilement analysé

**Q: Pourquoi pas VADER à la place de TextBlob ?**
> R: VADER (Valence Aware Dictionary and sEntiment Reasoner) est spécialisé pour les réseaux sociaux et gère mieux les emojis, l'argot et la ponctuation (!!!). C'est un choix alternatif valide. TextBlob est plus généraliste et suffisant pour cette analyse.

**Q: Pourquoi pas BERT ou un modèle deep learning ?**
> R: 
> - **BERT** serait plus précis (~90% vs ~70% pour TextBlob)
> - **MAIS** : beaucoup plus lent (GPU nécessaire), plus complexe à implémenter
> - Sur 1.7M tweets, TextBlob prend quelques minutes ; BERT prendrait des heures
> - Pour une analyse exploratoire, TextBlob est un bon compromis vitesse/précision

**Q: Que signifie `lambda x: x[0]` ?**
> R: C'est une **fonction anonyme**. `get_sentiment()` retourne un tuple `(polarity, subjectivity)`. `lambda x: x[0]` extrait le premier élément (index 0 = polarité).

### 📊 Métriques typiques des résultats

| Candidat | Positifs | Négatifs | Neutres | Polarité Moyenne |
|----------|----------|----------|---------|------------------|
| Trump | ~35% | ~25% | ~40% | ~0.05 |
| Biden | ~38% | ~22% | ~40% | ~0.07 |

**Observation clé** : Les deux candidats ont des distributions similaires, avec une légère tendance positive et beaucoup de tweets neutres (souvent des partages d'informations factuelles).

---

# 📊 7. RÉGRESSION LINÉAIRE (FROM SCRATCH) ⭐⭐⭐

**C'est la partie la plus importante pour la soutenance !**

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

---

## 📐 DÉMONSTRATION MATHÉMATIQUE COMPLÈTE

### Le modèle de régression linéaire simple

$$y_i = ax_i + b + \varepsilon_i$$

**Où :**
- $y_i$ = variable dépendante (ce qu'on veut prédire : % de votes)
- $x_i$ = variable indépendante (ce qu'on utilise : polarité Twitter)
- $a$ = pente de la droite (coefficient directeur)
- $b$ = ordonnée à l'origine (intercept)
- $\varepsilon_i$ = terme d'erreur (résidu) pour chaque observation

### La méthode des moindres carrés

**Objectif** : Trouver $a$ et $b$ qui minimisent la somme des carrés des résidus.

$$D = \sum_{i=1}^{n} \varepsilon_i^2 = \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 = \sum_{i=1}^{n} (y_i - b - ax_i)^2$$

**Pourquoi les carrés ?**
1. Élimine les signes (erreurs positives et négatives ne s'annulent pas)
2. Pénalise davantage les grandes erreurs (erreur de 10 compte 100, pas 10)
3. Permet une résolution analytique (dérivable)

### Dérivation des formules (ce que le prof peut demander)

Pour minimiser D, on calcule les dérivées partielles et on les pose égales à 0 :

**Dérivée par rapport à b :**
$$\frac{\partial D}{\partial b} = -2\sum_{i=1}^{n}(y_i - b - ax_i) = 0$$

**Dérivée par rapport à a :**
$$\frac{\partial D}{\partial a} = -2\sum_{i=1}^{n}x_i(y_i - b - ax_i) = 0$$

En résolvant ce système d'équations (équations normales), on obtient :

### Formule de la pente a

$$a = \frac{\sum_{i=1}^{n} x_i y_i - \frac{(\sum_{i=1}^{n} x_i)(\sum_{i=1}^{n} y_i)}{n}}{\sum_{i=1}^{n} x_i^2 - \frac{(\sum_{i=1}^{n} x_i)^2}{n}} = \frac{s_{xy}}{s_x^2} = \frac{\text{Cov}(X,Y)}{\text{Var}(X)}$$

**Interprétation** : 
- **Numérateur** = covariance entre X et Y (comment ils varient ensemble)
- **Dénominateur** = variance de X (comment X varie seul)
- **a** = changement moyen de Y pour une augmentation d'une unité de X

### Formule de l'intercept b

$$b = \bar{y} - a\bar{x}$$

**Interprétation** : La droite de régression passe TOUJOURS par le point moyen $(\bar{x}, \bar{y})$.

---

## 📏 COEFFICIENT DE DÉTERMINATION R²

$$R^2 = 1 - \frac{SC_{res}}{SC_{tot}} = 1 - \frac{\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}{\sum_{i=1}^{n}(y_i - \bar{y})^2}$$

**Où :**
- $SC_{tot}$ = Somme des Carrés Totale = variance totale de Y
- $SC_{res}$ = Somme des Carrés Résiduelle = variance non expliquée par le modèle
- $\hat{y}_i = ax_i + b$ = valeur prédite par le modèle

### Interprétation de R²

| Valeur R² | Interprétation | Qualité du modèle |
|-----------|----------------|-------------------|
| 0.90 - 1.00 | Excellent | Modèle très prédictif |
| 0.70 - 0.90 | Bon | Corrélation forte |
| 0.50 - 0.70 | Modéré | Corrélation moyenne |
| 0.30 - 0.50 | Faible | Corrélation faible |
| 0.00 - 0.30 | Très faible | Quasi pas de corrélation |

**Nos résultats :**
- Trump : R² = 0.137 → **Très faible** (13.7% expliqué)
- Biden : R² = 0.026 → **Quasi nul** (2.6% expliqué)

---

## ❓ QUESTIONS POSSIBLES (LES PLUS IMPORTANTES)

**Q: Pourquoi "from scratch" et pas sklearn ?**
> R: Pour **démontrer ma compréhension des mathématiques** du cours (pages 25-41). Utiliser `sklearn.linear_model.LinearRegression` serait une "boîte noire". Ici, je code directement les formules : $a = \frac{s_{xy}}{s_x^2}$ et $b = \bar{y} - a\bar{x}$. Je peux expliquer chaque ligne de code mathématiquement.

**Q: Écrivez la formule de la pente au tableau**
> R: $$a = \frac{\sum x_i y_i - \frac{(\sum x_i)(\sum y_i)}{n}}{\sum x_i^2 - \frac{(\sum x_i)^2}{n}}$$

**Q: Que représente le numérateur ?**
> R: C'est la **covariance empirique** entre X et Y, multipliée par n. Elle mesure comment X et Y varient ensemble. Si quand X augmente, Y augmente aussi → covariance positive → pente positive.

**Q: Que représente le dénominateur ?**
> R: C'est la **variance empirique** de X, multipliée par n. Elle mesure la dispersion de X autour de sa moyenne. On divise par la variance pour normaliser et obtenir un coefficient indépendant de l'échelle de X.

**Q: Pourquoi la droite passe par $(\bar{x}, \bar{y})$ ?**
> R: C'est une propriété mathématique des moindres carrés. Si on remplace $x$ par $\bar{x}$ dans l'équation $\hat{y} = a\bar{x} + b = a\bar{x} + (\bar{y} - a\bar{x}) = \bar{y}$. Le point moyen est toujours sur la droite.

**Q: C'est quoi la méthode des moindres carrés ?**
> R: C'est une méthode d'optimisation qui trouve les paramètres (a, b) minimisant la somme des erreurs au carré : $D = \sum(y_i - ax_i - b)^2$. On prend les dérivées partielles, on les pose égales à zéro, et on résout le système d'équations.

**Q: Pourquoi minimiser les carrés et pas les valeurs absolues ?**
> R: 
> 1. Les carrés sont **différentiables partout** (pas la valeur absolue en 0)
> 2. Permet une **solution analytique** (formule exacte vs algorithme itératif)
> 3. Pénalise **plus fortement les grandes erreurs** (outliers)

**Q: Interprétez l'équation y = 160.47x + 44.51 pour Trump**
> R: 
> - **a = 160.47** : Si la polarité moyenne d'un État augmente de 0.01, le % de votes Trump prédit augmente de 1.6 points
> - **b = 44.51** : Si la polarité était 0 (neutre), Trump aurait théoriquement ~44.5% des votes
> - **ATTENTION** : R² = 0.137 montre que ce modèle est PEU FIABLE. La relation existe faiblement mais n'est pas prédictive.

**Q: Que signifie R² = 0.137 pour Trump ?**
> R: 
> - La polarité Twitter explique **seulement 13.7%** de la variance des votes
> - **86.3%** de la variance est due à d'autres facteurs (démographie, économie, événements locaux, etc.)
> - C'est une corrélation **très faible** selon les standards statistiques (<0.3)

**Q: Pourquoi R² est si faible ?**
> R: Plusieurs raisons :
> 1. **Twitter n'est pas représentatif** : surreprésentation des jeunes/urbains
> 2. **Bots et manipulation** : comptes automatisés qui faussent les sentiments
> 3. **Expression ≠ vote** : tweeter n'équivaut pas à voter
> 4. **Agrégation par État** : masque les variations intra-État
> 5. **Limites de TextBlob** : sarcasme, contexte non compris

**Q: Est-ce que des R² faibles sont un "échec" ?**
> R: **NON !** C'est un résultat scientifique important. Démontrer que Twitter NE prédit PAS les élections est une conclusion valide et utile. Cela réfute l'hypothèse naïve que les réseaux sociaux peuvent remplacer les sondages.

---

## 🔬 HYPOTHÈSES DE LA RÉGRESSION LINÉAIRE

Pour que la régression soit valide, plusieurs hypothèses doivent être respectées :

| Hypothèse | Description | Vérifiée ? |
|-----------|-------------|------------|
| **Linéarité** | Relation linéaire entre X et Y | Partiellement (R² faible suggère non-linéarité possible) |
| **Indépendance** | Les résidus sont indépendants | Oui (chaque État est indépendant) |
| **Homoscédasticité** | Variance constante des résidus | À vérifier visuellement |
| **Normalité** | Résidus normalement distribués | Moins critique avec n>30 |
| **Pas de multicolinéarité** | Variables X non corrélées entre elles | N/A (une seule variable X) |

**Q: Qu'est-ce que l'homoscédasticité ?**
> R: C'est l'hypothèse que la variance des erreurs est constante pour toutes les valeurs de X. Si les points sont plus dispersés pour certaines valeurs de X, c'est de l'**hétéroscédasticité**, ce qui invalide les tests statistiques.

---

## 📈 COMPARAISON AVEC SKLEARN (si on vous demande)

```python
# Version sklearn (pour comparaison)
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score

model = LinearRegression()
model.fit(X.reshape(-1, 1), y)

print(f"Pente (sklearn): {model.coef_[0]}")
print(f"Intercept (sklearn): {model.intercept_}")
print(f"R² (sklearn): {r2_score(y, model.predict(X.reshape(-1, 1)))}")

# Les résultats sont IDENTIQUES à notre implémentation from scratch
```

**Q: Pourquoi sklearn donne le même résultat ?**
> R: Parce que sklearn utilise exactement les mêmes formules mathématiques (moindres carrés ordinaires). C'est la preuve que mon implémentation est correcte.

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
> R: C'est un format de fichier géospatial (.shp) qui contient des **géométries vectorielles** (points, lignes, polygones) avec leurs coordonnées géographiques. Développé par ESRI pour les SIG (Systèmes d'Information Géographique). Un shapefile des États-Unis contient 51 polygones (les frontières de chaque État).

**Q: C'est quoi une carte choroplèthe ?**
> R: C'est une carte thématique où chaque région (ici, État) est colorée selon une valeur numérique. Le mot vient du grec : "choros" (région) + "plethos" (multitude). Exemples : cartes de densité de population, de revenus moyens, de résultats électoraux.

**Q: Pourquoi EPSG:5070 ?**
> R: C'est le code du système de coordonnées **"NAD83 / Conus Albers"** (Albers Equal Area Conic), optimisé pour les États-Unis continentaux.
> 
> **Avantages** :
> - **Préserve les surfaces** : les États gardent leur taille relative correcte (Alaska n'est pas 5x plus grand)
> - **Minimise les déformations** pour cette zone géographique
> - **Standard** pour les cartes gouvernementales américaines
> 
> **Alternative** : EPSG:4326 (WGS84) est le système GPS global mais déforme les surfaces.

**Q: Que signifie `to_crs()` ?**
> R: "CRS" = Coordinate Reference System. `to_crs(epsg=5070)` **reprojette** les coordonnées du shapefile vers le système EPSG:5070. C'est une transformation mathématique des coordonnées.

**Q: Que signifie `cmap='RdYlGn'` ?**
> R: C'est la **colormap** (palette de couleurs) : **R**ed → **Y**ellow → **G**reen.
> - Rouge = valeurs basses (polarité négative)
> - Jaune = valeurs moyennes (polarité neutre)
> - Vert = valeurs hautes (polarité positive)
> 
> C'est intuitif : vert = positif, rouge = négatif.

**Q: C'est quoi `.merge()` ?**
> R: C'est l'équivalent d'un **JOIN SQL** en pandas/geopandas. On fusionne deux tables sur une colonne commune :
> - `left_on='STUSPS'` : colonne de la table de gauche (shapefile) contenant le code État (CA, TX, NY...)
> - `right_on='state_code'` : colonne de la table de droite (nos données) contenant le code État
> - Résultat : une table avec les géométries ET les données de polarité

**Q: Que signifie `how='inner'` dans merge ?**
> R: Type de jointure :
> - `inner` : Garde seulement les lignes présentes dans LES DEUX tables
> - `left` : Garde toutes les lignes de gauche, même sans correspondance
> - `right` : Garde toutes les lignes de droite
> - `outer` : Garde tout, avec des NaN si pas de correspondance

**Q: Pourquoi utiliser GeoPandas et pas juste Matplotlib ?**
> R: Matplotlib seul ne peut pas lire les shapefiles ni gérer les projections cartographiques. GeoPandas étend pandas avec des capacités géospatiales : lecture de shapefiles, reprojection, opérations spatiales (intersection, buffer...).

### 📊 Cartes produites

| Carte | Variable visualisée | Colormap |
|-------|---------------------|----------|
| Polarité Trump | Moyenne polarité par État | RdYlGn |
| Polarité Biden | Moyenne polarité par État | RdYlGn |
| Subjectivité Trump | Moyenne subjectivité par État | YlOrRd |
| Subjectivité Biden | Moyenne subjectivité par État | YlOrRd |
| Résultats électoraux | % votes par candidat | RdBu |

### 🔍 Observations géographiques

- Les **côtes** (Californie, New York) montrent souvent une polarité différente du **centre** (Texas, Oklahoma)
- Les **swing states** (Floride, Pennsylvanie, Michigan, Wisconsin) présentent des sentiments plus mixtes
- La correspondance visuelle sentiment/vote est **partielle**, confirmant le R² faible

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
> R: C'est l'équivalent d'un **JOIN SQL** en pandas. On fusionne deux DataFrames sur une colonne commune. C'est essentiel pour combiner des données provenant de sources différentes.

**Q: Que signifie `how='inner'` ?**
> R: C'est le type de jointure :
> - `inner` : Garde UNIQUEMENT les lignes présentes dans les DEUX tables
> - `left` : Garde toutes les lignes de la table de gauche
> - `right` : Garde toutes les lignes de la table de droite
> - `outer` : Garde toutes les lignes des deux tables
> 
> Avec `inner`, si un État n'a pas de tweets OU pas de résultats de vote, il est exclu.

**Q: Pourquoi fusionner ?**
> R: Pour créer un dataset unique permettant la régression :
> - **X** = polarité moyenne Twitter par État (source : tweets)
> - **Y** = pourcentage de votes réel par État (source : AP)
> - Sans fusion, on ne peut pas comparer les deux variables

**Q: Que font `left_on` et `right_on` ?**
> R: Ils spécifient les colonnes de jointure quand elles ont des noms différents :
> - `left_on='state_code'` : colonne de la table de gauche (polarity_by_state)
> - `right_on='state_abr'` : colonne de la table de droite (vote_data)
> - Pandas les match sur leurs valeurs (CA=CA, TX=TX, etc.)

**Q: Pourquoi les noms de colonnes sont différents ?**
> R: Parce que les données viennent de sources différentes (Twitter vs Associated Press). C'est courant en data science de devoir harmoniser les noms/formats lors de la fusion.

---

# 📊 10. AGRÉGATION PAR ÉTAT

```python
# Calculer la polarité moyenne par État
polarity_by_state = df_trump.groupby('state_code').agg({
    'polarity': 'mean',
    'subjectivity': 'mean',
    'tweet': 'count'
}).reset_index()

polarity_by_state.columns = ['state_code', 'mean_polarity', 'mean_subjectivity', 'tweet_count']
```

### ❓ Questions possibles :

**Q: Que fait `.groupby()` ?**
> R: C'est l'équivalent du **GROUP BY** en SQL. On regroupe les lignes ayant la même valeur de `state_code`, puis on applique une fonction d'agrégation sur chaque groupe.

**Q: Que fait `.agg()` ?**
> R: C'est une fonction d'agrégation flexible. On peut appliquer différentes fonctions à différentes colonnes :
> - `'polarity': 'mean'` → moyenne de la polarité
> - `'subjectivity': 'mean'` → moyenne de la subjectivité
> - `'tweet': 'count'` → nombre de tweets

**Q: Pourquoi `.reset_index()` ?**
> R: Après un `groupby()`, la colonne de groupement devient l'index. `reset_index()` la remet en colonne normale, ce qui facilite les manipulations ultérieures.

**Q: Pourquoi la moyenne et pas la médiane ?**
> R: La moyenne est plus sensible aux valeurs extrêmes mais plus facile à interpréter. La médiane serait plus robuste aux outliers. Pour cette analyse exploratoire, la moyenne suffit. On pourrait comparer les deux pour vérifier la robustesse.

**Q: Combien de tweets minimum par État pour une moyenne fiable ?**
> R: Statistiquement, avec n > 30 observations, la moyenne est généralement stable (théorème central limite). Avec des milliers de tweets par État, nos moyennes sont très fiables.

---

# 📈 11. FONCTIONS D'ANALYSE COMPLÉMENTAIRES

```python
def WinnerPolarity(df_polarity, df_votes, candidate):
    """
    Calcule la polarité moyenne dans les États GAGNÉS par le candidat
    """
    # Déterminer les États gagnés
    if candidate == 'trump':
        won_states = df_votes[df_votes['trump_pct'] > df_votes['biden_pct']]['state_abr']
    else:
        won_states = df_votes[df_votes['biden_pct'] > df_votes['trump_pct']]['state_abr']
    
    # Filtrer les polarités pour ces États
    winner_polarity = df_polarity[df_polarity['state_code'].isin(won_states)]
    
    return winner_polarity['mean_polarity'].mean(), winner_polarity['mean_polarity'].std()

def LoserPolarity(df_polarity, df_votes, candidate):
    """
    Calcule la polarité moyenne dans les États PERDUS par le candidat
    """
    # Logique inverse de WinnerPolarity
    ...
```

### ❓ Questions possibles :

**Q: Quel est l'objectif de ces fonctions ?**
> R: Comparer si les sentiments Twitter sont différents dans les États **gagnés** vs **perdus** par chaque candidat. Si Twitter était prédictif, on s'attendrait à une polarité plus positive dans les États gagnés.

**Q: Que fait `.isin()` ?**
> R: Vérifie si chaque valeur de la colonne est présente dans une liste. `df['state'].isin(['CA', 'TX', 'NY'])` retourne True pour les lignes où state est CA, TX ou NY.

**Q: Quels sont les résultats de cette analyse ?**
> R: **Les différences sont faibles et non significatives** :
> - Les barres d'erreur (écart-type) se chevauchent largement
> - Pas de différence claire entre États gagnés et perdus
> - **Conclusion** : le sentiment Twitter ne discrimine pas le résultat électoral

---

# 📈 12. RÉSUMÉ DES FONCTIONS CLÉS

| Fonction | But | Input | Output |
|----------|-----|-------|--------|
| `load_twitter_data()` | Charger les CSV | filename | DataFrame |
| `clean_tweet()` | Nettoyer le texte (regex) | text | text nettoyé |
| `detect_language()` | Détecter la langue | text | 'en', 'fr', etc. |
| `remove_stopwords()` | Supprimer mots vides | text | text filtré |
| `get_sentiment()` | Analyser sentiment | text | (polarité, subjectivité) |
| `get_polarity_state()` | Classifier sentiment | polarité | 'positive'/'negative'/'neutral' |
| `getWordCloud()` | Créer nuage de mots | DataFrame | affiche image |
| `getInfoPolarity()` | Statistiques sentiments | DataFrame | affiche pie chart |
| `linear_regression()` | Régression from scratch | x, y | a, b, r² |
| `scatterplot()` | Visualiser régression | x, y | affiche graphique |
| `WinnerPolarity()` | Polarité États gagnés | df, votes | mean, std |
| `LoserPolarity()` | Polarité États perdus | df, votes | mean, std |

---

# ⚡ RÉPONSES RAPIDES AUX QUESTIONS PIÈGES

**Q: Votre code utilise-t-il du Machine Learning ?**
> R: **Oui, plusieurs aspects** :
> - **TextBlob** : utilise un modèle pré-entraîné (lexique de sentiments)
> - **Langdetect** : utilise un classifieur Naive Bayes pour la détection de langue
> - **Régression linéaire** : c'est techniquement du ML supervisé (apprentissage d'une fonction de prédiction)
> 
> Mais la régression est implémentée **mathématiquement** (from scratch), pas avec sklearn.

**Q: Pourquoi pas Deep Learning (BERT, GPT) ?**
> R: 
> - **BERT** serait plus précis pour l'analyse de sentiments (~90% vs ~70% pour TextBlob)
> - **MAIS** : nécessite un GPU, beaucoup plus lent, plus complexe à implémenter
> - Sur ~1.7M tweets, TextBlob prend quelques minutes ; BERT prendrait des heures/jours
> - Pour une **analyse exploratoire**, TextBlob est un compromis raisonnable vitesse/précision
> - C'est une **amélioration future** mentionnée dans les perspectives

**Q: Votre analyse est-elle reproductible ?**
> R: **Oui** :
> - Mêmes données + même code = mêmes résultats
> - Pas de composante aléatoire (pas de random seed nécessaire)
> - Le code est documenté et structuré en fonctions réutilisables
> - Les données sources sont identifiées (CSV, shapefiles)

**Q: Y a-t-il du data leakage ?**
> R: **Non** :
> - Je filtre les tweets **AVANT** la fermeture des bureaux de vote (3 novembre 2020, 20h00)
> - Je n'utilise aucune information postérieure à l'élection pour l'analyse
> - Les résultats de vote (Y) sont complètement séparés des tweets (X) jusqu'à la fusion finale

**Q: Pourquoi les résultats sont "décevants" (R² faible) ?**
> R: **Ce n'est PAS décevant, c'est un résultat scientifique IMPORTANT !**
> - Montrer que Twitter NE prédit PAS les élections est une conclusion **valide et utile**
> - Cela **réfute l'hypothèse naïve** que les réseaux sociaux peuvent servir de sondages
> - Un R² faible avec une méthodologie rigoureuse est plus valuable qu'un R² artificiel élevé

**Q: Comment améliorer le R² ?**
> R: Plusieurs pistes (mais attention au surapprentissage) :
> 1. **Modèles NLP avancés** : BERT, VADER pour meilleure détection de sentiments
> 2. **Variables additionnelles** : engagement (likes, retweets), nombre de followers
> 3. **Granularité** : analyse par comté au lieu d'État
> 4. **Modèles non-linéaires** : forêts aléatoires, réseaux de neurones
> 5. **Détection de bots** : filtrer les comptes automatisés
> 6. **Pondération** : par population de l'État ou nombre de tweets

**Q: Quelle est la différence entre corrélation et causalité ?**
> R: **TRÈS IMPORTANT** :
> - **Corrélation** : deux variables varient ensemble (mesuré par R²)
> - **Causalité** : une variable CAUSE le changement de l'autre
> - **Mon étude montre une corrélation faible**, mais même si elle était forte, ça ne prouverait pas que Twitter CAUSE le vote
> - Le vote cause peut-être les tweets (les gens tweetent selon leurs intentions de vote)
> - Ou une variable cachée cause les deux (ex: événements d'actualité)

**Q: Avez-vous fait des tests statistiques de significativité ?**
> R: La régression from scratch ne calcule pas la p-value. Avec sklearn ou statsmodels, on pourrait obtenir :
> - **Test t** sur les coefficients (a et b sont-ils significativement différents de 0 ?)
> - **Test F** sur le modèle global
> - **Intervalles de confiance** à 95%
> 
> C'est une amélioration possible mais pas demandée pour ce projet.

**Q: Comment gérer le problème des bots ?**
> R: Plusieurs approches possibles (non implémentées mais à mentionner) :
> 1. **Filtrer par ancienneté du compte** : comptes créés récemment = suspects
> 2. **Filtrer par activité** : trop de tweets/jour = suspect
> 3. **Analyse du réseau** : comptes sans followers ou suivant des patterns
> 4. **Utiliser des APIs de détection** : Botometer (Twitter)
> 5. **Vérifier les patterns de texte** : tweets identiques, hashtags automatisés

---

# 🎯 LIMITES À CONNAÎTRE POUR LA SOUTENANCE

## Limites techniques

| Limite | Impact | Solution possible |
|--------|--------|-------------------|
| **TextBlob simpliste** | Ne détecte pas sarcasme/ironie | Utiliser BERT, VADER |
| **Langdetect imprécis sur textes courts** | Tweets mal classifiés | Filtrer tweets < 50 caractères |
| **Géolocalisation autodéclarée** | Données manquantes/fausses | Croiser avec IP, timezone |
| **Pas de détection de bots** | Tweets automatisés inclus | Implémenter filtrage |

## Limites méthodologiques

| Limite | Explication |
|--------|-------------|
| **Régression linéaire simple** | Suppose une relation linéaire qui peut ne pas exister |
| **Une seule variable explicative** | Régression multiple serait plus réaliste |
| **Agrégation par État** | Masque variations intra-État (urbain vs rural) |
| **Période limitée** | Ne capture pas l'évolution long terme |

## Limites des données

| Limite | Conséquence |
|--------|-------------|
| **Biais de représentativité** | Twitter ≠ population votante (jeunes, urbains surreprésentés) |
| **Bots et manipulation** | Comptes automatisés faussent les résultats |
| **Auto-sélection** | Les tweeters politiques sont déjà engagés |
| **Expression ≠ Vote** | Tweeter n'est pas voter |

---

# 📝 VOCABULAIRE TECHNIQUE À CONNAÎTRE

| Terme | Définition |
|-------|------------|
| **DataFrame** | Tableau de données à 2 dimensions (lignes, colonnes) en pandas |
| **Regex** | Expression régulière, pattern pour matcher du texte |
| **NLP** | Natural Language Processing, traitement du langage naturel |
| **Stop words** | Mots vides sans valeur sémantique (the, is, a...) |
| **N-gramme** | Séquence de n éléments consécutifs (lettres ou mots) |
| **Polarité** | Score de sentiment de -1 (négatif) à +1 (positif) |
| **Subjectivité** | Score de 0 (factuel) à 1 (opinion) |
| **Régression** | Modèle qui prédit une variable continue (Y) à partir d'une autre (X) |
| **R² (coefficient de détermination)** | Proportion de variance de Y expliquée par le modèle |
| **Moindres carrés** | Méthode qui minimise la somme des erreurs au carré |
| **Résidu** | Différence entre valeur observée et valeur prédite ($y_i - \hat{y}_i$) |
| **Covariance** | Mesure comment deux variables varient ensemble |
| **Variance** | Mesure de dispersion d'une variable autour de sa moyenne |
| **Choroplèthe** | Carte colorée selon une variable numérique |
| **Shapefile** | Format de fichier géospatial (polygones, coordonnées) |
| **EPSG** | Code de référence pour les systèmes de coordonnées |
| **CRS** | Coordinate Reference System (système de coordonnées) |
| **From scratch** | Implémenté manuellement sans librairie toute faite |
| **Data leakage** | Utilisation involontaire d'informations futures |
| **Overfitting** | Modèle trop ajusté aux données d'entraînement |

---

# 🧮 FORMULES À SAVOIR ÉCRIRE AU TABLEAU

## 1. Modèle de régression linéaire
$$y_i = ax_i + b + \varepsilon_i$$

## 2. Formule de la pente (a)
$$a = \frac{\sum_{i=1}^{n} x_i y_i - \frac{(\sum x_i)(\sum y_i)}{n}}{\sum_{i=1}^{n} x_i^2 - \frac{(\sum x_i)^2}{n}} = \frac{\text{Cov}(X,Y)}{\text{Var}(X)}$$

## 3. Formule de l'intercept (b)
$$b = \bar{y} - a\bar{x}$$

## 4. Coefficient de détermination R²
$$R^2 = 1 - \frac{SC_{res}}{SC_{tot}} = 1 - \frac{\sum(y_i - \hat{y}_i)^2}{\sum(y_i - \bar{y})^2}$$

## 5. Fonction à minimiser (moindres carrés)
$$D = \sum_{i=1}^{n} (y_i - ax_i - b)^2$$

---

# ✅ CHECKLIST AVANT LA SOUTENANCE

## Concepts mathématiques
- [ ] Je sais écrire la formule de la pente (a) au tableau
- [ ] Je sais expliquer ce que représente le numérateur (covariance)
- [ ] Je sais expliquer ce que représente le dénominateur (variance)
- [ ] Je comprends pourquoi la droite passe par $(\bar{x}, \bar{y})$
- [ ] Je sais ce que signifie R² = 0.137 (13.7% de variance expliquée)
- [ ] Je connais la différence entre corrélation et causalité

## Concepts NLP
- [ ] Je sais expliquer comment TextBlob calcule la polarité
- [ ] Je comprends la différence polarité vs subjectivité
- [ ] Je connais les limites de TextBlob (sarcasme, contexte)
- [ ] Je sais expliquer ce qu'est un n-gramme

## Code
- [ ] Je sais expliquer chaque regex du nettoyage
- [ ] Je comprends pourquoi j'ai fait "from scratch"
- [ ] Je peux expliquer le pipeline de preprocessing
- [ ] Je sais ce que font .groupby(), .merge(), .apply()

## Conclusions
- [ ] Je sais pourquoi Twitter ne prédit pas les élections
- [ ] Je connais les 4+ limites de l'étude
- [ ] Je peux proposer des améliorations futures
- [ ] Je comprends que R² faible = résultat scientifique valide

---

**Bonne chance pour ta soutenance ! 🚀**

*Tu as maintenant toutes les clés pour répondre aux questions du professeur avec confiance et précision.*
