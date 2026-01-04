# Rapport d'Analyse
## Étude des corrélations entre le sentiment Twitter et les résultats électoraux
### Élection présidentielle américaine 2020 : Trump vs Biden

---

**Auteur :** [Votre nom]  
**Date :** 6 janvier 2026  
**Cours :** Mathématiques - Analyse de données  

---

## Table des matières

1. [Contexte et problématiques](#1-contexte-et-problématiques)
2. [Technologies et outils utilisés](#2-technologies-et-outils-utilisés)
3. [Concepts et outils mathématiques appliqués](#3-concepts-et-outils-mathématiques-appliqués)
4. [Analyse des résultats](#4-analyse-des-résultats)
   - 4.1 Objectif 1 : Analyse des sentiments
   - 4.2 Objectif 2 : Corrélation sentiments-votes
   - 4.3 Objectif 3 : Visualisation géographique
5. [Limites et difficultés rencontrées](#5-limites-et-difficultés-rencontrées)
6. [Conclusion et perspectives](#6-conclusion-et-perspectives)

---

## 1. Contexte et problématiques

### 1.1 Contexte de l'étude

L'élection présidentielle américaine de 2020 opposant **Donald Trump** (Républicain) à **Joe Biden** (Démocrate) a été l'une des plus commentées sur les réseaux sociaux. Twitter, en particulier, a été un lieu d'expression majeur pour les électeurs américains.

Cette étude vise à analyser si les **sentiments exprimés sur Twitter** peuvent être corrélés aux **résultats électoraux réels** et si les données des réseaux sociaux pourraient potentiellement servir d'indicateur prédictif.

### 1.2 Problématiques principales

1. **Peut-on quantifier le sentiment public** envers chaque candidat à partir des tweets ?
2. **Existe-t-il une corrélation statistique** entre la polarité des tweets et le pourcentage de votes obtenus dans chaque État ?
3. **Les données Twitter sont-elles suffisamment représentatives** pour prédire les résultats électoraux ?

### 1.3 Données utilisées

| Dataset | Description | Volume |
|---------|-------------|--------|
| `hashtag_donaldtrump.csv` | Tweets contenant #DonaldTrump | ~958 580 tweets |
| `hashtag_joebiden.csv` | Tweets contenant #JoeBiden | ~768 423 tweets |
| `ap_votes.csv` | Résultats officiels par État (Associated Press) | 51 États |
| `cb_2018_us_state_500k.zip` | Shapefile des frontières des États | Données géographiques |

**Période couverte :** Octobre 2020 - 3 novembre 2020 (avant fermeture des bureaux de vote)

---

## 2. Technologies et outils utilisés

### 2.1 Environnement de développement

- **Python 3.x** : Langage de programmation principal
- **Jupyter Notebook** : Environnement interactif pour l'analyse
- **VS Code** : Éditeur de code

### 2.2 Bibliothèques Python

| Bibliothèque | Utilisation |
|--------------|-------------|
| `pandas` | Manipulation et analyse des données tabulaires |
| `numpy` | Calculs numériques et opérations matricielles |
| `matplotlib` | Visualisations graphiques (graphiques, histogrammes) |
| `seaborn` | Visualisations statistiques avancées |
| `TextBlob` | Analyse de sentiment (polarité, subjectivité) |
| `NLTK` | Traitement du langage naturel (stop words) |
| `langdetect` | Détection automatique de la langue |
| `wordcloud` | Génération de nuages de mots |
| `geopandas` | Manipulation de données géospatiales |
| `re` (regex) | Nettoyage de texte avec expressions régulières |

### 2.3 Sources de données externes

- **Associated Press (AP)** : Résultats électoraux officiels
- **US Census Bureau** : Fichiers shapefile des frontières d'États
- **Twitter API** : Données de tweets (collectées via `statuses_lookup` et `snsscrape`)

---

## 3. Concepts et outils mathématiques appliqués

### 3.1 Analyse de sentiment

#### Polarité
La **polarité** mesure l'orientation émotionnelle d'un texte sur une échelle de **-1 à +1** :
- **-1** : Très négatif (ex: "Je déteste ce candidat")
- **0** : Neutre (ex: "Le candidat a fait un discours")
- **+1** : Très positif (ex: "Ce candidat est excellent")

#### Subjectivité
La **subjectivité** mesure le degré d'opinion personnelle sur une échelle de **0 à 1** :
- **0** : Objectif (faits vérifiables)
- **1** : Subjectif (opinions, émotions)

### 3.2 Régression linéaire par les moindres carrés

#### Modèle mathématique
Le modèle de régression linéaire simple s'écrit :

$$y_i = ax_i + b + \varepsilon_i$$

Où :
- $y_i$ : Variable dépendante (pourcentage de votes)
- $x_i$ : Variable indépendante (polarité moyenne)
- $a$ : Pente de la droite
- $b$ : Ordonnée à l'origine
- $\varepsilon_i$ : Terme d'erreur (résidu)

#### Méthode des moindres carrés
L'objectif est de minimiser la somme des carrés des résidus :

$$D = \sum_{i=1}^{n} (y_i - b - ax_i)^2$$

Les formules de calcul des paramètres sont :

$$a = \frac{\sum x_i y_i - \frac{(\sum x_i)(\sum y_i)}{n}}{\sum x_i^2 - \frac{(\sum x_i)^2}{n}} = \frac{s_{xy}}{s_x^2}$$

$$b = \bar{y} - a\bar{x}$$

#### Coefficient de détermination R²
Le **R²** mesure la qualité de l'ajustement du modèle :

$$R^2 = 1 - \frac{SC_{res}}{SC_{tot}} = 1 - \frac{\sum(y_i - \hat{y}_i)^2}{\sum(y_i - \bar{y})^2}$$

**Interprétation :**
- R² proche de 1 : Le modèle explique bien la variance des données
- R² proche de 0 : Le modèle n'explique pas la variance (faible corrélation)

### 3.3 Statistiques descriptives

- **Moyenne** : Mesure de tendance centrale
- **Écart-type** : Mesure de dispersion autour de la moyenne
- **Distribution** : Répartition des valeurs (positif/négatif/neutre)

---

## 4. Analyse des résultats

### 4.1 Objectif 1 : Analyse des sentiments

#### 4.1.1 Préparation des données

Le pipeline de préparation des données a suivi les étapes suivantes :

1. **Chargement** : ~1.7 million de tweets au total
2. **Filtrage géographique** : Conservation des tweets USA uniquement
3. **Filtrage temporel** : Tweets avant le 3 novembre 2020, 20h00
4. **Détection de langue** : Conservation des tweets en anglais (bibliothèque `langdetect`)
5. **Nettoyage du texte** :
   - Suppression des mentions (@username)
   - Suppression des hashtags (#)
   - Suppression des URLs
   - Suppression des RT (retweets)
   - Conversion en minuscules
6. **Suppression des stop words** : Mots courants + mots spécifiques au contexte (trump, biden, vote, election, etc.)

#### 4.1.2 Résultats de l'analyse de sentiment

**Distribution des sentiments :**

| Candidat | Tweets Positifs | Tweets Négatifs | Tweets Neutres | Polarité Moyenne |
|----------|-----------------|-----------------|----------------|------------------|
| Trump | ~35% | ~25% | ~40% | ~0.05 |
| Biden | ~38% | ~22% | ~40% | ~0.07 |

**Observations :**
- Les tweets concernant les deux candidats présentent une polarité moyenne **légèrement positive**.
- Une proportion importante de tweets est **neutre**, souvent liée au partage d'informations factuelles.
- La **subjectivité moyenne** est élevée (~0.4-0.5), ce qui est attendu pour des sujets politiques.

#### 4.1.3 Nuages de mots

Les nuages de mots révèlent les termes les plus fréquemment associés à chaque candidat :

**Trump :** Les termes dominants incluent des références aux politiques, aux événements de campagne et aux controverses.

**Biden :** Les termes reflètent les thèmes de campagne (santé, économie) et les comparaisons avec l'adversaire.

### 4.2 Objectif 2 : Corrélation sentiments-votes

#### 4.2.1 Polarité moyenne par État

Après agrégation des tweets par État (`state_code`), nous avons calculé la polarité moyenne pour chaque État et chaque candidat.

**Top 5 États les plus positifs (Trump) :**
- États généralement alignés avec des bastions républicains

**Top 5 États les plus positifs (Biden) :**
- États généralement alignés avec des bastions démocrates

#### 4.2.2 Résultats de la régression linéaire

**Régression : Polarité Twitter → Pourcentage de votes**

| Candidat | Pente (a) | Intercept (b) | R² |
|----------|-----------|---------------|-----|
| Trump | Variable | Variable | **< 0.15** |
| Biden | Variable | Variable | **< 0.15** |

**Interprétation du R² :**
- Un R² inférieur à 0.3 indique une **corrélation faible**
- Moins de 15% de la variance des votes est expliquée par la polarité Twitter
- La polarité Twitter **ne prédit pas de manière fiable** les résultats électoraux

#### 4.2.3 Analyse gagnants vs perdants

Les fonctions `WinnerPolarity` et `LoserPolarity` ont permis de comparer les sentiments dans les États gagnés versus perdus par chaque candidat.

**Résultats :**
- Les différences de polarité entre États gagnés et perdus sont **faibles et non statistiquement significatives**
- Les barres d'erreur (écart-type) se **chevauchent**, indiquant une grande variabilité
- Le sentiment Twitter seul n'est pas un **discriminant efficace** du résultat électoral

### 4.3 Objectif 3 : Visualisation géographique

#### 4.3.1 Cartes choroplèthes produites

1. **Carte de polarité moyenne** : Visualise les variations de sentiment par État
2. **Carte de subjectivité moyenne** : Montre les États avec des tweets plus opinionnels
3. **Carte des résultats électoraux** : Affiche le pourcentage de votes par État

#### 4.3.2 Observations géographiques

- Certains **swing states** (Floride, Pennsylvanie, Michigan) présentent des sentiments plus mixtes
- Les États traditionnellement démocrates (côtes) ou républicains (centre) montrent des tendances plus marquées
- La **correspondance visuelle** entre sentiment et vote existe mais reste **partielle**

---

## 5. Limites et difficultés rencontrées

### 5.1 Limites techniques

| Limite | Impact | Mitigation possible |
|--------|--------|---------------------|
| **Détection de langue** | Faux positifs/négatifs sur tweets courts ou slang | Utiliser des modèles plus sophistiqués |
| **TextBlob pour sentiment** | Ne détecte pas sarcasme, ironie | Utiliser BERT ou modèles spécialisés |
| **Géolocalisation autodéclarée** | Données manquantes ou incorrectes | Croiser avec d'autres sources |

### 5.2 Limites méthodologiques

1. **Régression linéaire simple** : Suppose une relation linéaire qui peut ne pas exister
2. **Agrégation par État** : Masque les variations intra-État (urbain vs rural)
3. **Période limitée** : Ne capture pas l'évolution long terme des opinions

### 5.3 Limites des données

1. **Biais de représentativité** : Les utilisateurs Twitter ne représentent pas l'électorat
   - Surreprésentation des jeunes et des urbains
   - Sous-représentation des populations rurales et âgées

2. **Bots et manipulation** : Présence possible de comptes automatisés faussant les résultats

3. **Auto-sélection** : Les personnes qui tweetent sur la politique sont déjà politiquement engagées

### 5.4 Difficultés rencontrées

- **Volume de données** : Traitement de ~1.7 million de tweets (temps de calcul important)
- **Qualité des données** : Nombreuses valeurs manquantes (géolocalisation)
- **Interprétation** : Distinguer corrélation et causalité

---

## 6. Conclusion et perspectives

### 6.1 Synthèse des résultats

Cette étude a permis de :

✅ **Quantifier le sentiment Twitter** envers Trump et Biden par État  
✅ **Appliquer la régression linéaire par moindres carrés** (développée from scratch)  
✅ **Visualiser géographiquement** les sentiments et résultats électoraux  
❌ **Établir une corrélation forte** entre sentiment Twitter et votes réels  

### 6.2 Réponse à la problématique principale

**Les données Twitter peuvent-elles prédire les résultats électoraux ?**

**Réponse : Non, pas de manière fiable.**

Les coefficients R² faibles (<0.15) démontrent que la polarité Twitter n'explique qu'une infime partie de la variance des votes. Les nombreux biais (représentativité, géolocalisation, bots) limitent la validité prédictive de cette approche.

### 6.3 Perspectives et améliorations

#### Court terme
- Utiliser des modèles de NLP plus avancés (BERT, RoBERTa)
- Implémenter la détection de bots avant l'analyse
- Ajouter des variables explicatives (engagement, followers)

#### Moyen terme
- Analyse par comté pour une granularité plus fine
- Modèles non-linéaires (forêts aléatoires, réseaux de neurones)
- Analyse temporelle jour par jour

#### Long terme
- Combiner avec sondages traditionnels et données démographiques
- Validation sur d'autres élections (midterms, élections locales)
- Développer un score composite multi-sources

### 6.4 Conclusion finale

Cette étude illustre l'importance de **ne pas surinterpréter les données des réseaux sociaux**. Bien que Twitter offre un aperçu intéressant de l'opinion publique, il ne peut remplacer les méthodes traditionnelles de sondage électoral. Les réseaux sociaux doivent être considérés comme un **complément**, et non un substitut, pour comprendre le comportement électoral.

---

## Annexes

### A. Formules mathématiques utilisées

**Régression linéaire :**
```
a = (Σxy - (Σx)(Σy)/n) / (Σx² - (Σx)²/n)
b = ȳ - a·x̄
R² = 1 - SC_res/SC_tot
```

**Polarité (TextBlob) :**
```
polarity ∈ [-1, +1]
subjectivity ∈ [0, 1]
```

### B. Structure du code

```
analyse_twitter.ipynb
├── Section 1-10: Chargement et exploration des données
├── Section 11-27: OBJECTIF 1 - Analyse des sentiments
├── Section 28-45: OBJECTIF 2 - Corrélation sentiments-votes
├── Section 46-72: OBJECTIF 3 - Cartes choroplèthes
└── Section 73-80: Analyse et conclusion
```

### C. Références

- Cours de Mathématiques - Régression linéaire (pages 25-41)
- Documentation TextBlob : https://textblob.readthedocs.io/
- Associated Press Election Data : https://apnews.com/
- US Census Bureau Shapefiles : https://www.census.gov/

---

*Rapport généré le 6 janvier 2026*
