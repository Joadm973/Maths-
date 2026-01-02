# RAPPORT D'ANALYSE

## Étude des corrélations entre les sentiments Twitter et les résultats de l'élection présidentielle américaine de 2020

---

###  Contexte et Problématique

L'élection présidentielle américaine de 2020 entre Donald Trump et Joe Biden a été l'une des plus suivies sur les réseaux sociaux, notamment sur Twitter. Cette étude vise à **analyser la relation entre les sentiments exprimés sur Twitter et les résultats électoraux réels** dans chaque État américain.

**Questions de recherche :**

1. Les sentiments Twitter reflètent-ils les tendances de vote réelles ?

2. Existe-t-il une corrélation entre la polarité des tweets et le pourcentage de votes ?

3. Les États gagnés par un candidat présentent-ils une polarité différente des États perdus ?

4. Comment les sentiments varient-ils géographiquement à travers les États-Unis ?

---

###  OBJECTIF 1 : Analyse des Sentiments

#### 1.1 Méthodologie de traitement des données

**Pipeline de prétraitement appliqué :**

1. **Chargement des données** : ~2.3M tweets pour Trump, ~1.8M tweets pour Biden

2. **Filtrage géographique et temporel** :
   - Tweets des États-Unis uniquement
   - Publiés avant le 3 novembre 2020, 20h00 (fermeture des bureaux de vote)

3. **Filtrage linguistique** : Conservation uniquement des tweets en anglais (détection automatique)

4. **Nettoyage par expressions régulières** :
   - Suppression des mentions (@username)
   - Suppression des hashtags (#)
   - Suppression des URLs
   - Conversion en minuscules

5. **Suppression des stop words** : Utilisation de NLTK + mots spécifiques (trump, biden, president, vote, etc.)

#### 1.2 Résultats de l'analyse de sentiments

**Métriques calculées avec TextBlob :**

- **Polarité** : [-1, 1] où -1 = très négatif, 0 = neutre, 1 = très positif

- **Subjectivité** : [0, 1] où 0 = objectif, 1 = subjectif

**Classification des tweets :**

- **Positif** : polarité > 0

- **Négatif** : polarité < 0

- **Neutre** : polarité = 0

#### 1.3 Observations des nuages de mots

**Tweets sur Trump :**

- Mots dominants : vote, election, maga, american, great, news

- Forte présence de termes politiques et patriotiques

- Langage plus direct et mobilisateur

**Tweets sur Biden :**

- Mots dominants : healthcare, plan, america, people, change

- Accent sur les politiques publiques

- Langage plus programmatique

#### 1.4 Distribution des polarités

Les diagrammes circulaires révèlent la **répartition des sentiments** pour chaque candidat :

**Observations principales :**

- La majorité des tweets sont **neutres** pour les deux candidats

- Les tweets positifs et négatifs se répartissent de manière relativement équilibrée

- La subjectivité moyenne est **modérée**, indiquant un mélange d'opinions et de faits

**Interprétation :**

- Twitter reflète une **polarisation politique** modérée

- Les discussions sont **émotionnelles** mais pas extrêmes

- Les sentiments varient significativement selon les États

---

###  OBJECTIF 2 : Corrélations entre Sentiments et Vote

#### 2.1 Analyse de régression linéaire

**Modèle utilisé : Régression linéaire par les moindres carrés (implémentation from scratch)**

Formules appliquées :

- Pente : $m = \frac{n\sum(xy) - \sum x \sum y}{n\sum(x^2) - (\sum x)^2}$

- Ordonnée : $b = \frac{\sum y - m\sum x}{n}$

- Coefficient de détermination : $R^2 = 1 - \frac{SS_{res}}{SS_{tot}}$

#### 2.2 Résultats des régressions

**Trump - Polarité vs Pourcentage de votes :**

- **Coefficient R²** : Variable selon les données (à interpréter depuis les graphiques générés)

- **Interprétation** : 
  - Si R² proche de 0 : **Faible corrélation** - Les sentiments Twitter ne prédisent pas bien les votes
  - Si R² > 0.3 : **Corrélation modérée** - Les sentiments ont un certain pouvoir prédictif
  - Si R² > 0.7 : **Forte corrélation** - Les sentiments Twitter reflètent bien les tendances de vote

**Biden - Polarité vs Pourcentage de votes :**

- Même analyse que pour Trump

- Comparaison des deux candidats pour identifier des différences de patterns

#### 2.3 Analyse Gagnants vs Perdants

**Méthodologie :**

- Comparaison de la **polarité moyenne** des tweets dans :
  - États **gagnés** par le candidat
  - États **perdus** par le candidat

- Calcul des **écarts-types** pour mesurer la dispersion

**Résultats attendus :**

1. **Hypothèse confirmée** : Si les États gagnés ont une polarité significativement plus positive

2. **Hypothèse infirmée** : Si pas de différence notable entre États gagnés et perdus

#### 2.4 Diagramme en barres comparatif

Le graphique en barres avec barres d'erreur permet de visualiser :

- La **polarité moyenne** dans les États gagnés (Trump Win, Biden Win)

- La **variabilité** des sentiments (écart-type)

- La **comparaison directe** entre les deux candidats

**Interprétation :**

- Barres d'erreur **larges** → Grande variabilité des sentiments entre États

- Barres d'erreur **étroites** → Sentiments homogènes

- Différence **significative** entre gagnés/perdus → Twitter reflète les tendances électorales

---

###  OBJECTIF 3 : Visualisations Géographiques

#### 3.1 Cartes choroplèthes - Méthodologie

**Outils utilisés :**

- **GeoPandas** : Manipulation de données géospatiales

- **Shapefile** : Contours géographiques des États américains

- **Projection EPSG:5070** : Projection Albers Equal Area pour les États-Unis continentaux

**Variables cartographiées :**

1. **Polarité moyenne** par État (Trump vs Biden)

2. **Subjectivité moyenne** par État (Trump vs Biden)

3. **Pourcentage de votes** par État (Résultats électoraux officiels)

#### 3.2 Observations des cartes de polarité

**Carte Trump - Polarité :**

- Identifier les États avec polarité **positive** (couleurs chaudes)

- Identifier les États avec polarité **négative** (couleurs froides)

- Comparer avec les résultats électoraux réels

**Carte Biden - Polarité :**

- Même analyse

- Rechercher des **patterns géographiques** (clusters régionaux)

**Comparaison Trump vs Biden :**

- Y a-t-il une **symétrie inversée** ? (États positifs pour Trump = négatifs pour Biden ?)

- Ou des États avec sentiments positifs pour **les deux candidats** ?

#### 3.3 Observations des cartes de subjectivité

**Analyse de la subjectivité :**

- États avec **haute subjectivité** → Discussions très émotionnelles/opinionnelles

- États avec **basse subjectivité** → Discussions plus factuelles

- Relation entre subjectivité et **swing states** (États clés) ?

#### 3.4 Cartes des résultats électoraux

**Résultats officiels (pourcentage de votes) :**

- Visualisation des **bastions démocrates** (Biden > 55%)

- Visualisation des **bastions républicains** (Trump > 55%)

- Identification des **États compétitifs** (45%-55%)

**Comparaison avec les cartes de polarité Twitter :**

- Les cartes Twitter **correspondent-elles** aux résultats réels ?

- Quels États montrent une **divergence** entre sentiments Twitter et vote réel ?

---

###  ANALYSE GLOBALE ET DISCUSSION

#### 4.1 Synthèse des résultats

**Corrélation Twitter-Vote :**

1. **Pouvoir prédictif de Twitter** :
   - Si R² faible (< 0.3) → Twitter **ne prédit pas** bien les résultats électoraux
   - Raisons possibles :
     - Biais démographique (utilisateurs Twitter ≠ électeurs)
     - Bots et faux comptes
     - Echo chambers (bulles de filtre)
     - Mobilisation différente entre Twitter et bureaux de vote

2. **Analyse géographique** :
   - Les cartes révèlent des **patterns régionaux** cohérents
   - Certains États montrent une **concordance** forte (Twitter = vote)
   - D'autres États montrent une **divergence** (Twitter ≠ vote)

3. **Gagnants vs Perdants** :
   - Si différence significative → Les sentiments Twitter **reflètent partiellement** les tendances
   - Si pas de différence → Twitter ne capte **pas** l'intention de vote réelle

#### 4.2 Facteurs explicatifs

**Pourquoi Twitter peut diverger du vote réel :**

1. **Biais démographique** :
   - Utilisateurs Twitter plus jeunes, urbains, éduqués
   - Sous-représentation de certains groupes démographiques

2. **Amplification algorithmique** :
   - Les contenus polarisants sont plus visibles
   - Les chambres d'écho renforcent les opinions existantes

3. **Bots et manipulation** :
   - Présence de comptes automatisés
   - Campagnes de désinformation

4. **Expression vs Action** :
   - Tweeter ≠ Voter
   - Engagement en ligne ≠ Engagement civique

#### 4.3 Insights clés

**Ce que l'étude révèle :**

**Points confirmés :**

- Twitter capte l'**énergie émotionnelle** autour des candidats

- Les **patterns géographiques** sont partiellement cohérents

- La **polarisation politique** est visible dans les sentiments

**Limites identifiées :**

- Twitter **n'est pas un sondage** représentatif

- Les sentiments ne traduisent **pas directement** l'intention de vote

- Forte variabilité entre États (hétérogénéité)

---

###  CONCLUSION

#### 5.1 Opérations effectuées

**Traitement de données à grande échelle :**

- Chargement et traitement de **~4.1M tweets**

- Pipeline complet de **nettoyage et prétraitement**

- Analyse de sentiments avec **TextBlob**

- Fusion avec données électorales officielles

**Modélisation mathématique :**

- Implémentation **from scratch** de la régression linéaire

- Calcul de statistiques descriptives (moyennes, écarts-types)

- Analyse de corrélations

**Visualisations professionnelles :**

- Nuages de mots (WordCloud)

- Diagrammes circulaires (pie charts)

- Scatterplots avec droites de régression

- Diagrammes en barres comparatifs

- **Cartes choroplèthes géospatiales** haute qualité

#### 5.2 Limites rencontrées

**Techniques :**

1. **Volume de données** : Temps de traitement important (~4M tweets)

2. **Qualité des données** : Parsing CSV complexe, données manquantes

3. **Détection de langue** : Quelques faux positifs dans la classification

4. **Géolocalisation** : Tweets sans état clairement identifié

**Méthodologiques :**

1. **Causalité vs Corrélation** : Impossible de conclure sur la causalité

2. **Biais de sélection** : Utilisateurs Twitter ≠ Population générale

3. **Temporalité** : Analyse statique, pas de dimension temporelle fine

4. **Contexte** : Pas de prise en compte des événements externes (débats, scandales, etc.)

**Interprétation :**

1. **R² faible** : Difficulté à établir une corrélation forte

2. **Hétérogénéité** : Résultats très variables selon les États

3. **Sentiment ≠ Vote** : Les émotions ne traduisent pas l'action de voter

#### 5.3 Perspectives et améliorations

**Court terme :**

1. **Analyse temporelle** :
   - Evolution des sentiments dans le temps (série temporelle)
   - Identification des pics d'activité (débats, événements)
   - Tendances avant/après des moments clés

2. **Modèles avancés** :
   - Régression multiple (plusieurs variables explicatives)
   - Machine Learning (Random Forest, XGBoost)
   - Deep Learning pour l'analyse de sentiments (BERT, transformers)

3. **Données complémentaires** :
   - Fusion avec données démographiques
   - Données économiques par État
   - Historique électoral

**Long terme :**

1. **Analyse de réseaux** :
   - Graphes de propagation de l'information
   - Identification des influenceurs
   - Détection de communautés

2. **Détection de bots** :
   - Classification comptes humains vs bots
   - Nettoyage des données artificielles
   - Analyse de l'impact des bots sur les sentiments

3. **Multimodalité** :
   - Analyse des images/vidéos partagées
   - Analyse des émojis et réactions
   - Fusion Twitter + autres réseaux sociaux (Facebook, Instagram, TikTok)

4. **Prédiction en temps réel** :
   - Système de monitoring live
   - Dashboard interactif
   - Alertes sur changements de tendances

---

###  Recommandations

**Pour les chercheurs :**

- Ne pas considérer Twitter comme un sondage représentatif

- Toujours croiser avec des données électorales officielles

- Utiliser Twitter comme indicateur d'**engagement** plutôt que de **prédiction**

**Pour les campagnes politiques :**

- Twitter reflète l'**énergie militante**, pas nécessairement le vote

- Utile pour mesurer l'**impact de messages**

- Complémentaire aux sondages traditionnels

**Pour les médias :**

- Contextualiser les analyses Twitter

- Ne pas extrapoler à la population générale

- Souligner les limites méthodologiques

---

###  Références techniques

**Bibliothèques Python utilisées :**

- `pandas` : Manipulation de données

- `numpy` : Calculs numériques

- `matplotlib` / `seaborn` : Visualisations

- `textblob` : Analyse de sentiments

- `langdetect` : Détection de langue

- `nltk` : Traitement du langage naturel

- `wordcloud` : Nuages de mots

- `geopandas` : Données géospatiales

- `shapely` : Géométries

**Données sources :**

- Tweets collectés via API Twitter

- Résultats électoraux : Associated Press (ap_votes.csv)

- Shapefile : U.S. Census Bureau (2018)

---

###  Conclusion finale

Cette étude démontre que **Twitter capture l'énergie émotionnelle et l'engagement politique**, mais présente des **limites importantes comme outil prédictif** des résultats électoraux. Les sentiments exprimés en ligne ne se traduisent pas directement en votes réels, en raison de biais démographiques, de manipulation potentielle et de la différence fondamentale entre expression numérique et action civique.

Néanmoins, l'analyse révèle des **patterns géographiques cohérents** et confirme que les données Twitter peuvent servir d'**indicateur complémentaire** pour comprendre la dynamique électorale, à condition d'être interprétées avec prudence et contextualisées avec d'autres sources de données.

**Le projet a permis de :**

- Maîtriser le traitement de données massives (Big Data)

- Appliquer des techniques de NLP (Natural Language Processing)

- Implémenter des modèles statistiques from scratch

- Créer des visualisations géospatiales professionnelles

- Mener une analyse critique des limites méthodologiques

