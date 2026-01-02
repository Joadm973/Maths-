# 📚 GUIDE COMPLET DE SOUTENANCE

## Maths appliquées à la Data-Science
### Analyse des sentiments Twitter et résultats électoraux (US 2020)

---

# 🎯 PARTIE 1 : COMPRENDRE TON PROJET

## 📋 Vue d'ensemble

Tu as analysé **4.1 millions de tweets** sur l'élection américaine 2020 (Trump vs Biden) pour répondre à cette question :

> **"Les sentiments exprimés sur Twitter peuvent-ils prédire les résultats électoraux ?"**

**Spoiler : La réponse est NON** (et c'est ce qui rend ton projet intéressant !)

---

## 🔄 Le flux complet du projet (12 étapes)

```
┌─────────────────────────────────────────────────────────────────┐
│  DONNÉES BRUTES                                                  │
│  • hashtag_donaldtrump.csv (2.3M tweets)                        │
│  • hashtag_joebiden.csv (1.8M tweets)                           │
│  • ap_votes.csv (résultats électoraux officiels)                │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  ÉTAPE 1-4 : NETTOYAGE & FILTRAGE                               │
│  • Garder uniquement USA                                         │
│  • Garder uniquement avant fermeture bureaux (3 nov 20h)        │
│  • Garder uniquement tweets en anglais                          │
│  • Supprimer @mentions, #hashtags, URLs                         │
│  • Supprimer stop words (the, is, a, etc.)                      │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  ÉTAPE 5 : ANALYSE DE SENTIMENTS (TextBlob)                     │
│  • Polarité : -1 (négatif) → +1 (positif)                       │
│  • Subjectivité : 0 (factuel) → 1 (opinion)                     │
│  • Classification : Positif / Neutre / Négatif                  │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  ÉTAPE 6-7 : VISUALISATIONS                                     │
│  • Nuages de mots (WordCloud)                                    │
│  • Diagrammes circulaires (pie charts)                          │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  ÉTAPE 8-11 : AGRÉGATION PAR ÉTAT                               │
│  • Calculer polarité MOYENNE par État                           │
│  • Fusionner avec résultats de vote officiels                   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  ÉTAPE 12-14 : RÉGRESSION LINÉAIRE (FROM SCRATCH)               │
│  • Polarité Twitter (X) vs % Votes réels (Y)                    │
│  • Calcul de R² pour mesurer la corrélation                     │
│  • Scatterplots avec droite de régression                       │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  ÉTAPE 15-18 : VISUALISATIONS GÉOGRAPHIQUES                     │
│  • Cartes choroplèthes de polarité par État                     │
│  • Cartes de subjectivité par État                              │
│  • Cartes des résultats électoraux réels                        │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  CONCLUSION                                                      │
│  • R² Trump = 0.137 (13.7% de variance expliquée)               │
│  • R² Biden = 0.026 (2.6% seulement !)                          │
│  ➡️ Twitter NE PRÉDIT PAS les élections                         │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Les technologies utilisées

| Catégorie | Librairie | Utilisation |
|-----------|-----------|-------------|
| **Data** | `pandas` | Manipulation des 4.1M tweets |
| **Data** | `numpy` | Calculs mathématiques |
| **Visualisation** | `matplotlib`, `seaborn` | Graphiques |
| **NLP** | `TextBlob` | Analyse de sentiments |
| **NLP** | `langdetect` | Détection langue (anglais) |
| **NLP** | `nltk` | Stop words |
| **NLP** | `wordcloud` | Nuages de mots |
| **Cartographie** | `geopandas`, `shapely` | Cartes USA |

---

## 📐 Les concepts mathématiques clés

### 1. **Polarité (TextBlob)**

```
Polarité ∈ [-1, +1]
  -1 = très négatif ("Trump is terrible")
   0 = neutre ("Trump spoke today")
  +1 = très positif ("Trump is amazing")
```

### 2. **Régression linéaire (moindres carrés)**

```
Équation : y = mx + b

m (pente) = [n×Σ(xy) - Σx×Σy] / [n×Σ(x²) - (Σx)²]
b (ordonnée) = [Σy - m×Σx] / n
```

### 3. **Coefficient R²**

```
R² = 1 - (SS_res / SS_tot)

SS_res = Σ(y - ŷ)²   ← erreur du modèle
SS_tot = Σ(y - ȳ)²   ← variance totale

R² proche de 1 = bon modèle
R² proche de 0 = mauvais modèle
```

---

## 🎯 Les résultats clés à retenir

| Candidat | R² | Interprétation |
|----------|-----|----------------|
| **Trump** | 0.137 | Twitter explique 13.7% des votes |
| **Biden** | 0.026 | Twitter explique seulement 2.6% |

**Conclusion principale :** Twitter ne prédit PAS les élections !

**Pourquoi ?**
1. Biais démographique (utilisateurs jeunes, urbains)
2. Bots et manipulation
3. Tweeter ≠ Voter
4. Amplification algorithmique

---

# 🎤 PARTIE 2 : PLAN DE SOUTENANCE (par slide)

## **SLIDE 1 - Introduction** (30 sec)

**Ce que tu dis :**
> "Bonjour, je vais vous présenter mon projet d'analyse des sentiments Twitter lors de l'élection américaine 2020. La question centrale est : peut-on prédire le vote via les réseaux sociaux ?"

**Points clés :**
- Titre du projet clair
- Question de recherche
- Contexte historique

---

## **SLIDE 2 - Contexte** (1 min)

**Ce que tu dis :**
> "J'ai analysé 4.1 millions de tweets contenant les hashtags #donaldtrump et #joebiden. L'objectif était triple : analyser les sentiments, chercher une corrélation avec les votes réels, et explorer les variations géographiques."

**Points à mentionner :**
- 2.3M tweets Trump + 1.8M tweets Biden
- Période : jusqu'au 3 novembre 2020, 20h
- Trois objectifs clairs

---

## **SLIDE 3 - Technologies** (1 min)

**Ce que tu dis :**
> "Pour ce projet Big Data, j'ai utilisé Python avec pandas et numpy pour la manipulation des données, TextBlob pour l'analyse de sentiments, et GeoPandas pour les cartes."

**Si le jury demande :**
- "Pourquoi TextBlob ?" → "Simple à utiliser, donne polarité et subjectivité directement"
- "Pourquoi EPSG:5070 ?" → "C'est la projection Albers Equal Area, optimisée pour les USA continentaux"
- "Vous avez considéré d'autres outils ?" → "TextBlob est idéale pour les analyses rapides, même si BERT serait plus performant pour du deep learning"

---

## **SLIDE 4 - Pipeline** (1-2 min)

**Ce que tu dis :**
> "Voici mon pipeline de traitement. J'ai d'abord chargé les tweets bruts, puis filtré pour garder uniquement les USA, les tweets avant la fermeture des bureaux de vote, et ceux en anglais. Ensuite j'ai nettoyé le texte et appliqué l'analyse de sentiments."

**Détails à développer :**
- Importation des 4.1M tweets
- Filtrage géographique et temporel
- Filtrage linguistique avec langdetect
- Nettoyage regex (@mentions, #hashtags, URLs)
- Suppression des stop words (NLTK)
- Application de TextBlob

**Questions possibles :**
- "Pourquoi filtrer avant 20h ?" → "Pour ne pas inclure les réactions aux premiers résultats"
- "Comment détectez-vous l'anglais ?" → "Avec la librairie langdetect"
- "Combien de tweets avez-vous perdus lors du filtrage ?" → "Réfléchir : prob 30-40% pour garder que USA + anglais"

---

## **SLIDE 5 - Sentiments & Nuages de mots** (1-2 min)

**Ce que tu dis :**
> "L'analyse de sentiments avec TextBlob donne deux métriques : la polarité de -1 à +1, et la subjectivité de 0 à 1. Les nuages de mots révèlent des différences de vocabulaire : Trump avec 'maga, great, american', Biden avec 'healthcare, plan, change'."

**Observation importante :**
> "La majorité des tweets sont neutres, avec une polarisation modérée. Les distributions pie charts montrent un équilibre entre positif/négatif pour les deux candidats."

**Questions possibles :**
- "Pourquoi ces mots dominent ?" → "Parce qu'ils sont répétés dans les campagnes principales"
- "Y a-t-il des mots toxiques ?" → "Oui, mais ils sont moins nombreux que prévu"

---

## **SLIDE 6 - Régression linéaire** (2 min) ⭐ SLIDE CLÉ

**Ce que tu dis :**
> "J'ai implémenté la régression linéaire from scratch, sans utiliser sklearn. Cette formule calcule la pente m qui minimise l'erreur entre les prédictions et les valeurs réelles. Le coefficient R² mesure la qualité du modèle."

**Explique la formule :**

$$m = \frac{n \times \sum(xy) - \sum x \times \sum y}{n \times \sum(x^2) - (\sum x)^2}$$

> "Cette formule calcule la pente m. Le numérateur représente la covariance entre x et y, le dénominateur la variance de x."

**Si le jury demande "Pourquoi from scratch ?" :**
> "Pour démontrer ma compréhension mathématique du modèle, pas juste appeler une fonction magique. Cela montre que je maîtrise les moindres carrés."

**Détails sur R² :**
> "Le R² mesure la proportion de variance dans les votes expliquée par la polarité Twitter. Il vaut 1 si le modèle est parfait, 0 si inutile."

---

## **SLIDE 7 - Scatterplots** (1-2 min) ⭐ SLIDE RÉSULTATS

**Ce que tu dis :**
> "Voici les résultats : pour Trump, R² = 0.137, ce qui signifie que Twitter n'explique que 13.7% de la variance des votes. Pour Biden, c'est encore pire avec R² = 0.026, soit 2.6%. Les points sont très dispersés autour de la droite."

**Analyse des graphiques :**
- Trump : y = 160.47x + 44.51 (pente positive mais faible R²)
- Biden : y = 80.92x + 43.86 (pente très faible et R² quasi-nul)

**Conclusion à dire :**
> "Twitter ne prédit pas efficacement les élections. La variance massive des points autour de la droite confirme que d'autres facteurs dominent."

**Questions possibles :**
- "Pourquoi R² Biden < R² Trump ?" → "Biden's support was broader, less polarized on Twitter"
- "C'est surprenant ?" → "Pas vraiment, Twitter est un echo chamber qui ne représente pas l'électorat"

---

## **SLIDE 8 - Corrélations : Twitter et l'Urne** (1 min)

**Ce que tu dis :**
> "Ces résultats confirment trois choses : la corrélation est faible (R² < 0.3), il n'y a pas de différence significative entre États gagnés et perdus, et le sentiment n'est pas le vote."

**Points à expliquer :**
- Corrélation très faible confirmée
- États gagnés/perdus : polarité similaire dans les deux cas
- Conclusion : l'expression Twitter ≠ l'action de vote

---

## **SLIDE 9 - Cartes géographiques** (1-2 min)

**Ce que tu dis :**
> "J'ai créé des cartes choroplèthes avec GeoPandas. On voit des patterns géographiques partiellement cohérents avec les résultats réels, mais pas suffisamment pour prédire."

**Description des cartes :**
- Polarité moyenne par État (Trump et Biden)
- Subjectivité moyenne par État
- Résultats électoraux officiels
- Comparaison visuelle

**Observations :**
- Patterns régionaux visibles (côte est vs côte ouest)
- Mais divergences importantes dans les swing states
- Les cartes révèlent la tendance générale sans prédire l'issue

**Si le jury demande :**
> "Les cartes montrent la polarité moyenne par État, pas le vainqueur. La comparaison avec la carte des résultats réels montre des divergences importantes, surtout dans les États compétitifs."

---

## **SLIDE 10 - Limites** (1 min) ⭐ SLIDE ESPRIT CRITIQUE

**Ce que tu dis :**
> "L'étude révèle quatre limites majeures : le biais démographique des utilisateurs Twitter, l'amplification algorithmique des contenus polarisants, la présence de bots et manipulation, et surtout le fait que tweeter n'est pas voter."

**Détaille chaque limite :**

### 1. **Biais démographique**
> "Les utilisateurs de Twitter sont en moyenne plus jeunes, urbains et éduqués que l'électorat général. Ils ne représentent pas la population."

### 2. **Amplification algorithmique**
> "Les algorithmes favorisent les contenus polarisants et engageants. Les bulles de filtre isolent les utilisateurs dans des univers informationnels fermés."

### 3. **Bots et manipulation**
> "Des comptes automatisés et des campagnes de désinformation peuvent fausser les sentiments réels."

### 4. **Expression ≠ Action**
> "Tweeter c'est s'exprimer, voter c'est agir. Ce ne sont pas la même chose. Quelqu'un peut tweeter son opposition tout en ne votant pas (ou l'inverse)."

**C'est la slide qui montre ton esprit critique !** Le jury apprécie l'honnêteté.

---

## **SLIDE 11 - Synthèse** (1 min)

**Ce que tu dis :**
> "En conclusion, Twitter capture bien l'énergie émotionnelle des campagnes et révèle des patterns géographiques, mais il n'est pas représentatif de l'électorat et les sentiments ne prédisent pas les votes. Twitter doit être vu comme un outil complémentaire, pas prédictif."

**Points clés :**

**Ce que Twitter CAPTURE :**
- ✅ Énergie émotionnelle autour des candidats
- ✅ Patterns géographiques partiels
- ✅ Dynamique de polarisation politique

**Ce que Twitter NE PRÉDIT PAS :**
- ❌ Les résultats électoraux réels
- ❌ L'intention de vote réelle
- ❌ Le comportement électoral

---

## **SLIDE 12 - Compétences & Perspectives** (1 min)

**Ce que tu dis :**
> "Ce projet m'a permis de développer mes compétences en Big Data, NLP, modélisation mathématique et cartographie. Pour aller plus loin, on pourrait intégrer une analyse temporelle, du Machine Learning avancé, ou de la détection de bots."

**Compétences acquises :**
- ✓ Big Data (gestion de 4.1M tweets)
- ✓ NLP (sentiment analysis, preprocessing)
- ✓ Régression linéaire from scratch
- ✓ Cartographie spatiale (GeoPandas)
- ✓ Analyse critique et esprit scientifique

**Perspectives d'amélioration :**
- Analyse temporelle (évolution des sentiments dans le temps)
- Modèles ML avancés (Random Forest, XGBoost, BERT)
- Détection de bots
- Fusion avec données démographiques
- Dashboard interactif

---

## **SLIDE 13 - Merci** 

**Ce que tu dis :**
> "Merci pour votre attention, je suis prêt à répondre à vos questions."

---

# ⏱️ Timing total estimé : 12-15 minutes

---

# 🎓 ANNEXE : Questions Potentielles du Jury

## Questions techniques

**Q1 : "Pourquoi avoir implémenté la régression from scratch ?"**

R : "Pour démontrer que je comprends les mathématiques derrière le modèle, pas juste utiliser une black box. Cela montre ma maîtrise des moindres carrés et des formules statistiques."

---

**Q2 : "Pourquoi R² pour Biden est si faible ?"**

R : "Plusieurs raisons : le support de Biden était plus dispersé géographiquement, moins polarisé sur Twitter, et Twitter favorise les contenus émotionnels (plus en accord avec le style de Trump)."

---

**Q3 : "Avez-vous considéré d'autres modèles ?"**

R : "Oui, j'ai commencé par une régression linéaire simple pour établir une baseline. Pour aller plus loin, on pourrait explorer des modèles non-linéaires ou du Machine Learning, mais la question était d'abord de vérifier s'il existe une corrélation."

---

**Q4 : "Comment gérez-vous les tweets en d'autres langues ?"**

R : "J'ai utilisé langdetect pour identifier les tweets en anglais. Les autres langues (espagnol, français, etc.) sont exclues pour garantir une analyse de sentiments cohérente."

---

**Q5 : "Avez-vous validé votre modèle ?"**

R : "La validation principale est le R² qui mesure la qualité d'ajustement. J'ai aussi visuellement inspécté les scatterplots pour détecter des patterns non-linéaires ou des outliers."

---

## Questions conceptuelles

**Q6 : "Twitter est-il vraiment inutile pour prédire les élections ?"**

R : "Twitter n'est pas inutile, mais il n'est pas prédictif des votes réels. C'est un excellent outil pour mesurer l'engagement, l'énergie émotionnelle, et les tendances, mais pas un sondage. Il faut le combiner avec d'autres données."

---

**Q7 : "Pourquoi les résultats sont si décevants ?"**

R : "C'est en réalité un résultat scientifique important ! Montrer que Twitter ne prédit pas les élections est une conclusion honnête et utile. Cela remet en question les hypothèses naïves sur les réseaux sociaux."

---

**Q8 : "Avez-vous envisagé l'impact des bots ?"**

R : "Pas dans ce projet, mais c'est une perspective future intéressante. La présence de bots peut artificialiser les sentiments et fausser l'analyse."

---

## Questions méthodologiques

**Q9 : "Pourquoi filtrer les tweets avant 20h ?"**

R : "Pour isoler l'impact de l'engagement pré-électoral. Les tweets après 20h (fermeture des bureaux) contiendraient les réactions aux premiers résultats, ce qui biaiserait l'analyse."

---

**Q10 : "Comment gérez-vous les valeurs manquantes ?"**

R : "J'ai utilisé un filtrage strict : si un État n'a pas de données Twitter ou de résultats électoraux, il est exclu. Cela réduit le dataset mais garantit la qualité."

---

## Questions de données

**Q11 : "D'où viennent les données ?"**

R : "Les tweets proviennent de collections historiques #donaldtrump et #joebiden. Les résultats électoraux viennent de l'Associated Press (AP), la source officielle."

---

**Q12 : "Avez-vous des biais dans les données ?"**

R : "Oui, plusieurs biais identifiés : la couverture Twitter est inégale entre États, les utilisateurs ne représentent pas l'électorat, et il y a probablement de la manipulation. Ce sont des limites à garder en tête."

---

# 💡 Conseils finaux pour la soutenance

1. **Soyez honnête sur les limites** → Le jury apprécie l'esprit critique
2. **Maîtrisez les formules** → Surtout la régression linéaire
3. **Connaissez vos chiffres** → R² = 0.137 et 0.026, 4.1M tweets, etc.
4. **Montrez l'enthousiasme** → C'est un projet intéressant !
5. **Ayez des réponses prêtes** → Les questions ci-dessus vont probablement être posées
6. **N'hésitez pas à dire "je ne sais pas"** → Mieux que d'improviser faux
7. **Montrez votre code** → Si on vous le demande, vous pouvez montrer le notebook

---

**Bonne chance pour ta soutenance ! 🚀**
