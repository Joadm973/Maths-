# 🎤 PLAN DE SOUTENANCE - Projet Twitter & Vote 2020

**Durée : 10-15 minutes + 10 minutes de questions**

---

## 📋 STRUCTURE DE LA PRÉSENTATION

### 🎯 SLIDE 1 : Page de Titre (30 secondes)
```
Analyse des Corrélations entre 
Sentiments Twitter et Résultats Électoraux
Élection Présidentielle USA 2020

Trump vs Biden

[Votre Nom]
B3 Ynov - Mathématiques Appliquées
Janvier 2026
```

---

### 🌍 SLIDE 2 : Contexte et Problématique (1-2 minutes)

**Points clés à mentionner :**

1. **Contexte historique**
   - Élection présidentielle USA 2020 : événement majeur mondial
   - ~4.1 millions de tweets analysés (#donaldtrump, #joebiden)
   - Période : jusqu'au 3 novembre 2020, 20h (fermeture bureaux de vote)

2. **Problématiques de recherche**
   - ❓ Les sentiments Twitter prédisent-ils les résultats électoraux ?
   - ❓ Existe-t-il une corrélation entre polarité des tweets et votes ?
   - ❓ Comment les sentiments varient-ils géographiquement ?

3. **Enjeux**
   - Comprendre le rôle des réseaux sociaux dans la politique moderne
   - Évaluer Twitter comme outil prédictif électoral
   - Identifier les limites et biais

**Slide visuel suggéré :**
- Image carte USA avec logos Twitter + drapeaux électoraux
- Chiffres clés : 4.1M tweets, 50 États, 2 candidats

---

### 🛠️ SLIDE 3 : Technologies et Outils Utilisés (1-2 minutes)

**Stack Technique Python :**

**Data Science & Analyse**
- 📊 **pandas** : Manipulation de 4.1M tweets
- 🔢 **numpy** : Calculs numériques et statistiques
- 📈 **matplotlib / seaborn** : Visualisations

**NLP (Natural Language Processing)**
- 💬 **TextBlob** : Analyse de sentiments (polarité, subjectivité)
- 🌐 **langdetect** : Détection langue automatique
- 📚 **nltk** : Suppression stop words
- ☁️ **wordcloud** : Nuages de mots

**Géospatial**
- 🗺️ **geopandas** : Cartes choroplèthes
- 🌎 **shapely** : Manipulation géométries
- 📍 **Projection EPSG:5070** : Albers Equal Area

**Autres**
- 🔍 **re (regex)** : Expressions régulières pour nettoyage
- 📁 **csv** : Parsing données massives

**Slide visuel suggéré :**
- Logos des bibliothèques
- Architecture du pipeline de traitement
- Diagramme flux de données

---

### 📐 SLIDE 4 : Concepts Mathématiques Appliqués (2-3 minutes)

**1. Analyse de Sentiments (TextBlob)**

**Polarité :** 
```
P ∈ [-1, 1]
-1 = très négatif
 0 = neutre
+1 = très positif
```

**Subjectivité :**
```
S ∈ [0, 1]
0 = objectif (faits)
1 = subjectif (opinions)
```

**Classification :**
```
Si P > 0  → Tweet POSITIF
Si P < 0  → Tweet NÉGATIF
Si P = 0  → Tweet NEUTRE
```

---

**2. Régression Linéaire (Moindres Carrés) - FROM SCRATCH**

**Objectif :** Trouver la droite y = mx + b qui minimise l'erreur

**Formules implémentées :**

**Pente :**
```
m = (n·Σ(xy) - Σx·Σy) / (n·Σ(x²) - (Σx)²)
```

**Ordonnée à l'origine :**
```
b = (Σy - m·Σx) / n
```

**Coefficient de détermination :**
```
R² = 1 - (SS_res / SS_tot)

où :
SS_res = Σ(y - ŷ)²  (somme carrés résidus)
SS_tot = Σ(y - ȳ)²  (somme totale carrés)
```

**Interprétation R² :**
- R² ≈ 0 : Aucune corrélation
- R² ≈ 0.5 : Corrélation modérée
- R² ≈ 1 : Forte corrélation

---

**3. Statistiques Descriptives**

**Moyenne :**
```
μ = (1/n) · Σx_i
```

**Écart-type :**
```
σ = √[(1/n) · Σ(x_i - μ)²]
```

Utilisés pour comparer États gagnés vs États perdus

**Slide visuel suggéré :**
- Équations clés bien formatées
- Schéma explicatif de la régression linéaire
- Exemple visuel d'une droite de régression

---

### 📊 SLIDE 5-8 : Résultats - Objectif 1 (3-4 minutes)

**SLIDE 5 : Pipeline de Prétraitement**

**Étapes de nettoyage :**
```
4.1M tweets bruts
    ↓ Filtrage géographique (USA uniquement)
    ↓ Filtrage temporel (avant 3 nov 20h)
    ↓ Détection langue (anglais)
    ↓ Nettoyage regex (mentions, hashtags, URLs)
    ↓ Suppression stop words (NLTK)
    ↓
Tweets propres pour analyse
```

**Taux de rétention :** ~XX% des tweets conservés

---

**SLIDE 6 : Nuages de Mots**

**Afficher côte à côte :**
- WordCloud Trump : mots dominants (vote, maga, american, great)
- WordCloud Biden : mots dominants (healthcare, plan, people, change)

**Observation clé :**
- Trump : Langage mobilisateur, patriotique
- Biden : Focus sur politiques publiques

---

**SLIDE 7 : Distribution des Polarités**

**Afficher :**
- Pie charts Trump vs Biden
- Pourcentages positif/négatif/neutre

**Statistiques clés :**
```
Trump : XX% positif, YY% négatif, ZZ% neutre
Biden : XX% positif, YY% négatif, ZZ% neutre
```

**Observation :** Majorité de tweets neutres, polarisation modérée

---

**SLIDE 8 : Polarité et Subjectivité Moyennes**

**Tableau comparatif :**
```
                 Trump    Biden
Polarité moy.    0.XX     0.YY
Subjectivité     0.XX     0.YY
```

**Interprétation :** Les deux candidats génèrent des discussions émotionnelles modérées

---

### 📈 SLIDE 9-11 : Résultats - Objectif 2 (3-4 minutes)

**SLIDE 9 : Régression Linéaire - Résultats**

**Afficher les scatterplots :**
- Trump : Polarité vs % votes (avec droite régression)
- Biden : Polarité vs % votes (avec droite régression)

**Coefficients R² obtenus :**
```
Trump : R² = 0.XX
Biden : R² = 0.YY
```

**Interprétation :**
- Si R² < 0.3 : ⚠️ Faible pouvoir prédictif de Twitter
- Si R² > 0.5 : ✅ Corrélation modérée à forte

---

**SLIDE 10 : Analyse Gagnants vs Perdants**

**Afficher le graphique en barres comparatif**

**Résultats attendus :**
```
                    Polarité moyenne    Écart-type
Trump États gagnés      0.XX              ±0.YY
Trump États perdus      0.XX              ±0.YY
Biden États gagnés      0.XX              ±0.YY
Biden États perdus      0.XX              ±0.YY
```

**Question clé :** Les États gagnés ont-ils une polarité significativement plus positive ?

**Si OUI :** ✅ Twitter reflète partiellement les tendances électorales
**Si NON :** ⚠️ Twitter ne capte pas l'intention de vote

---

**SLIDE 11 : Synthèse Corrélations**

**Points clés :**
- ✅ Corrélation détectée (si R² > 0.3)
- ⚠️ Corrélation faible (si R² < 0.3)
- 📊 Grande variabilité entre États
- 🔍 Nécessité d'analyses complémentaires

---

### 🗺️ SLIDE 12-14 : Résultats - Objectif 3 (2-3 minutes)

**SLIDE 12 : Cartes Choroplèthes - Polarité**

**Afficher : Comparaison Trump vs Biden (polarité)**

**Points à mentionner :**
- États rouges = polarité positive
- États bleus = polarité négative
- Patterns géographiques régionaux visibles
- Comparaison avec résultats électoraux réels

**Question :** Les cartes Twitter correspondent-elles aux cartes électorales ?

---

**SLIDE 13 : Cartes Choroplèthes - Subjectivité**

**Afficher : Comparaison Trump vs Biden (subjectivité)**

**Observations :**
- Quels États ont les discussions les plus émotionnelles ?
- Relation avec les "swing states" ?
- Différences Trump/Biden

---

**SLIDE 14 : Cartes des Résultats Électoraux**

**Afficher : Résultats officiels (% votes)**

**Comparaison finale :**
- Concordance Twitter ↔ Vote réel
- États où Twitter a "prédit" correctement
- États avec divergence significative

---

### 💬 SLIDE 15 : Discussion et Analyse Globale (2 minutes)

**Synthèse des 3 objectifs :**

**✅ Ce que l'étude confirme :**
1. Twitter capture l'**énergie émotionnelle** autour des candidats
2. **Patterns géographiques** partiellement cohérents
3. **Polarisation politique** visible dans les sentiments

**⚠️ Limites identifiées :**
1. **Biais démographique** : Twitter ≠ Population générale
   - Plus jeune, urbain, éduqué
2. **Bots et manipulation** : Présence de comptes automatisés
3. **Echo chambers** : Bulles de filtre renforcent opinions
4. **Expression ≠ Action** : Tweeter ≠ Voter

**Facteurs explicatifs :**
```
Divergence Twitter / Vote réel due à :
├── Biais démographiques
├── Amplification algorithmique
├── Bots et faux comptes
└── Différence engagement online / action civique
```

---

### 🚧 SLIDE 16 : Limites et Difficultés (1-2 minutes)

**Défis Techniques :**
- ⏱️ Volume de données : Traitement de 4.1M tweets (temps important)
- 🧹 Qualité données : Parsing CSV complexe, données manquantes
- 🌐 Géolocalisation : Tweets sans état identifié
- 📝 Nettoyage : Variabilité du langage Twitter

**Défis Méthodologiques :**
- 🔬 Causalité vs Corrélation : Impossible de conclure sur causalité
- 👥 Représentativité : Biais de sélection (Twitter ≠ Électeurs)
- 📅 Temporalité : Analyse statique, pas de dimension temps fine
- 🔍 Interprétation : R² faible, grande hétérogénéité entre États

**Leçons apprises :**
- Twitter = Indicateur d'**engagement**, pas de **prédiction**
- Nécessité de croiser avec sondages traditionnels
- Importance du contexte et des événements externes

---

### 🔮 SLIDE 17 : Perspectives et Améliorations (1-2 minutes)

**Court terme :**
1. 📈 **Analyse temporelle**
   - Évolution sentiments dans le temps
   - Pics d'activité (débats, scandales)
   - Série temporelle avant/après événements clés

2. 🤖 **Modèles avancés**
   - Machine Learning (Random Forest, XGBoost)
   - Deep Learning (BERT, transformers)
   - Régression multiple (plusieurs variables)

3. 📊 **Enrichissement des données**
   - Données démographiques par État
   - Données économiques
   - Historique électoral

**Long terme :**
1. 🕸️ **Analyse de réseaux sociaux**
   - Graphes de propagation
   - Identification influenceurs
   - Détection communautés

2. 🤖 **Détection de bots**
   - Classification humains vs bots
   - Mesure impact sur sentiments
   - Nettoyage données artificielles

3. 🔄 **Multimodalité**
   - Analyse images/vidéos
   - Émojis et réactions
   - Fusion multi-plateformes (Facebook, Instagram, TikTok)

4. ⚡ **Temps réel**
   - Dashboard interactif
   - Monitoring live
   - Système d'alertes

---

### 🎯 SLIDE 18 : Conclusion (1 minute)

**Messages clés :**

**🔍 Résultats principaux :**
- Twitter capture l'**énergie émotionnelle**, pas nécessairement le vote
- **Faible/Modéré pouvoir prédictif** (selon R²)
- **Biais significatifs** limitent la généralisation

**📚 Compétences acquises :**
- ✅ Traitement de Big Data (4.1M tweets)
- ✅ NLP et analyse de sentiments
- ✅ Régression linéaire from scratch
- ✅ Visualisations géospatiales professionnelles
- ✅ Pensée critique sur limites méthodologiques

**💡 Recommandations :**
- Twitter = Outil **complémentaire**, pas de remplacement des sondages
- Toujours contextualiser et croiser avec autres sources
- Prudence dans l'interprétation des corrélations

**Citation de clôture :**
> "Les réseaux sociaux reflètent l'énergie, pas nécessairement la réalité électorale. 
> C'est un thermomètre de l'engagement, pas un bulletin de vote."

---

### ❓ SLIDE 19 : Questions ? (10 minutes)

**Préparez-vous à répondre à :**

1. **Questions techniques :**
   - Pourquoi régression linéaire et pas autre modèle ?
   - Comment gérer les tweets ambigus (polarité = 0) ?
   - Validation du modèle de régression ?

2. **Questions méthodologiques :**
   - Comment différencier bots vs humains ?
   - Choix de TextBlob vs autres outils NLP ?
   - Représentativité de l'échantillon ?

3. **Questions sur résultats :**
   - Pourquoi R² est faible ?
   - Quels États montrent plus/moins de corrélation ?
   - Impact des événements (débats, COVID) ?

4. **Questions sur limites :**
   - Comment améliorer la prédiction ?
   - Autres sources de données à intégrer ?
   - Généralisable à d'autres élections ?

---

## 🎨 CONSEILS DE PRÉSENTATION

### ✅ À FAIRE :
- 🎤 Parler clairement, pas trop vite
- 👁️ Regarder l'audience, pas les slides
- 📊 Expliquer chaque graphique (axes, légende)
- 💡 Insister sur les insights, pas le code
- 🤔 Montrer esprit critique (limites)
- ⏱️ Respecter le timing (10-15 min)

### ❌ À ÉVITER :
- 📄 Lire les slides mot à mot
- 🔬 Trop de détails techniques
- 💻 Montrer du code (sauf si demandé)
- 🤷 Dire "je ne sais pas" → "Bonne question, à explorer..."
- ⏳ Dépasser le temps

---

## 📁 FICHIERS À PRÉPARER

### 1. Support de présentation (PowerPoint/PDF)
- ✅ 19 slides maximum
- ✅ Visuels clairs (graphiques haute résolution)
- ✅ Texte minimal (bullet points)
- ✅ Transition fluide entre slides

### 2. Notebook Jupyter (analyse_twitter.ipynb)
- ✅ Toutes les cellules exécutées
- ✅ Sorties visibles
- ✅ Code commenté
- ✅ Rapport d'analyse intégré

### 3. Rapport détaillé (déjà dans notebook)
- ✅ Section "Analyse" complète
- ✅ Interprétation des résultats
- ✅ Discussion critique
- ✅ Conclusion et perspectives

### 4. Backup
- 💾 PDF du notebook (export HTML → PDF)
- 💾 Images haute résolution des graphiques clés
- 💾 Présentation sur clé USB + cloud

---

## ⏱️ TIMING RECOMMANDÉ

| Slide | Durée | Temps cumulé |
|-------|-------|--------------|
| 1. Titre | 30s | 0:30 |
| 2. Contexte | 1m30 | 2:00 |
| 3. Technologies | 1m30 | 3:30 |
| 4. Maths | 2m00 | 5:30 |
| 5-8. Objectif 1 | 3m00 | 8:30 |
| 9-11. Objectif 2 | 3m00 | 11:30 |
| 12-14. Objectif 3 | 2m00 | 13:30 |
| 15. Discussion | 1m30 | 15:00 |
| 16. Limites | 1m00 | 16:00 |
| 17. Perspectives | 1m00 | 17:00 |
| 18. Conclusion | 1m00 | 18:00 |

**Total : ~15 minutes** (+ marge pour questions inline)

---

## 🎯 CHECKLIST FINALE

### 1 semaine avant :
- [ ] Créer support PowerPoint
- [ ] Exporter tous les graphiques haute résolution
- [ ] Relire rapport d'analyse
- [ ] Préparer réponses questions fréquentes

### 3 jours avant :
- [ ] Répéter présentation (chronométrer)
- [ ] Faire relire par quelqu'un
- [ ] Ajuster timing si nécessaire
- [ ] Préparer backup (PDF, USB)

### Veille :
- [ ] Dernière répétition
- [ ] Vérifier matériel (laptop, adaptateurs)
- [ ] Imprimer notes (si besoin)
- [ ] Bien dormir ! 😴

### Jour J :
- [ ] Arriver 15 min en avance
- [ ] Tester vidéoprojecteur
- [ ] Respirer, se détendre
- [ ] Confiance ! 💪

---

**Bon courage pour la soutenance ! 🚀**

**Vous avez fait un excellent travail, présentez-le avec confiance !**
