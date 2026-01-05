# 🎤 GUIDE ORAL - Script de Soutenance

## Analyse des sentiments Twitter et résultats électoraux (US 2020)
### Durée totale visée : 12-13 minutes

---

# SLIDE 1 - Introduction (45 secondes)

> **[Afficher slide 1]**

"Bonjour à tous,

Je m'appelle Josué ADAMI, étudiant en B3 Data/IA, et je vais vous présenter mon projet de mathématiques appliquées à la Data Science.

Mon sujet porte sur l'**analyse des sentiments Twitter** pendant l'élection présidentielle américaine de 2020, opposant Donald Trump à Joe Biden.

La question centrale de cette étude est la suivante : **Peut-on prédire le résultat d'une élection en analysant les sentiments exprimés sur les réseaux sociaux ?**

C'est une question qui passionne les chercheurs et les analystes politiques depuis plusieurs années, et j'ai voulu y apporter ma propre réponse grâce aux outils mathématiques et statistiques que nous avons vus en cours."

---

# SLIDE 2 - L'Écho Numérique des Élections Américaines (1 minute)

> **[Afficher slide 2]**

"Pour répondre à cette question, j'ai analysé un dataset massif de **4,1 millions de tweets** collectés pendant la période électorale.

Ces tweets contenaient les hashtags #donaldtrump et #joebiden, ce qui m'a permis de séparer les discussions autour de chaque candidat.

Mon étude s'articule autour de **trois objectifs principaux** :

1. **Premièrement**, réaliser une analyse des sentiments sur Twitter pendant cette période électorale cruciale.

2. **Deuxièmement**, chercher une corrélation statistique entre la polarité des tweets et les résultats électoraux réels, État par État.

3. **Troisièmement**, explorer les variations géographiques des sentiments à travers les 51 États et territoires américains.

L'idée était de voir si l'opinion exprimée en ligne pouvait servir d'indicateur prédictif du vote réel."

---

# SLIDE 3 - Technologies utilisées (1 minute)

> **[Afficher slide 3]**

"Pour mener à bien ce projet, j'ai utilisé un ensemble de technologies Python que je vais vous présenter en trois catégories.

**Côté Data Science**, j'ai utilisé **Pandas** pour la manipulation de mes 4 millions de tweets, ce qui représente un vrai défi en termes de gestion mémoire. **NumPy** m'a servi pour tous les calculs mathématiques, notamment pour la régression linéaire. Et **Matplotlib** ainsi que **Seaborn** pour créer mes visualisations.

**Côté Traitement du Langage Naturel**, ou NLP, j'ai utilisé **TextBlob** qui est une bibliothèque essentielle pour l'analyse de sentiments. Elle me permet d'obtenir directement un score de polarité et de subjectivité pour chaque tweet. J'ai aussi utilisé **Langdetect** pour filtrer uniquement les tweets en anglais, et **NLTK** pour supprimer les mots vides, qu'on appelle les 'stop words'.

**Côté Cartographie**, j'ai travaillé avec **GeoPandas** et **Shapely** pour créer des cartes choroplèthes des États-Unis. J'ai utilisé la projection **EPSG:5070**, aussi appelée Albers Equal Area, qui est spécifiquement optimisée pour représenter le territoire américain continental avec une déformation minimale."

---

# SLIDE 4 - Les Données en Chiffres (45 secondes)

> **[Afficher slide 4]**

"Voici quelques chiffres clés qui illustrent l'ampleur de ce projet.

Au total, j'ai analysé **2,05 millions de tweets** après filtrage. Ce chiffre est inférieur aux 4,1 millions de tweets bruts car j'ai appliqué plusieurs filtres que je vais détailler.

Parmi ces tweets :
- **1,14 million** concernaient Donald Trump avec le hashtag #DonaldTrump
- **910 000** concernaient Joe Biden avec le hashtag #JoeBiden

La **période d'analyse** s'étend d'octobre 2020 jusqu'au **3 novembre 2020**, date de l'élection. J'ai volontairement exclu les tweets publiés après la fermeture des bureaux de vote pour ne pas biaiser l'analyse avec les réactions aux premiers résultats.

Les **résultats électoraux officiels** proviennent de l'Associated Press, une source de référence, et couvrent les **51 États et territoires** américains."

---

# SLIDE 5 - Le Processus de Transformation des Données (1 minute 30)

> **[Afficher slide 5]**

"Je vais maintenant vous expliquer mon pipeline de traitement des données, qui comporte 5 étapes principales.

**Étape 1 : Chargement des données**
J'ai importé les tweets bruts à partir de deux fichiers CSV volumineux, un pour chaque candidat.

**Étape 2 : Filtrage stratégique**
C'est une étape cruciale. J'ai conservé uniquement les tweets :
- Originaires des **États-Unis** (grâce au champ de localisation)
- Publiés **avant la fermeture des bureaux de vote**, soit avant 20h le 3 novembre
- Rédigés **en anglais**, détectés automatiquement avec la bibliothèque Langdetect

Ce filtrage est essentiel pour avoir des données pertinentes et comparables aux résultats électoraux.

**Étape 3 : Nettoyage préalable**
J'ai supprimé les **mentions** (@utilisateur), les **hashtags** (#sujet) et les **URLs** pour isoler uniquement le contenu textuel pertinent. Ces éléments n'apportent pas d'information sur le sentiment.

**Étape 4 : Suppression des mots vides**
Avec la bibliothèque NLTK, j'ai éliminé les 'stop words', c'est-à-dire les mots comme 'the', 'is', 'a', 'and'... Ces mots n'ont pas de valeur sémantique forte pour l'analyse de sentiment.

**Étape 5 : Analyse de sentiments**
Enfin, j'ai appliqué TextBlob pour attribuer un **score de polarité** à chaque tweet, de -1 pour très négatif à +1 pour très positif, avec 0 indiquant un sentiment neutre."

---

# SLIDE 6 - Dévoilement des Sentiments sur Twitter (1 minute 15)

> **[Afficher slide 6]**

"Passons maintenant aux résultats de l'analyse de sentiments.

Vous pouvez voir ici deux **nuages de mots** que j'ai générés, un pour chaque candidat.

Pour **Donald Trump**, on retrouve des mots comme 'people', 'say', 'trumps', 'election2020', 'think', 'know'. On remarque la présence de 'covid19' qui était évidemment un sujet central de la campagne.

Pour **Joe Biden**, les mots dominants sont 'people', 'win', 'know', 'america', 'kamalaharris', 'election2020'. On note la présence de 'kamalaharris', sa colistière.

Concernant l'**analyse de la polarité et de la subjectivité** :

La **polarité** mesure l'orientation émotionnelle du tweet sur une échelle de -1 à +1 :
- -1 signifie un sentiment très négatif
- 0 indique un sentiment neutre
- +1 représente un sentiment très positif

La **subjectivité**, elle, mesure si le tweet exprime un fait objectif (proche de 0) ou une opinion personnelle (proche de 1).

Ce que révèle mon analyse, c'est que la **majorité des tweets sont neutres**, avec une polarisation modérée. C'est un premier indice que Twitter ne capture peut-être pas bien les intentions de vote réelles."

---

# SLIDE 7 - Distribution des Sentiments : Chiffres Concrets (1 minute)

> **[Afficher slide 7]**

"Voici maintenant les chiffres concrets de la distribution des sentiments pour chaque candidat.

Pour **Donald Trump** :
- Environ **35%** des tweets sont positifs
- Environ **25%** sont négatifs
- Et environ **40%** sont neutres
- La **polarité moyenne** est de **0.05**, donc très légèrement positive

Pour **Joe Biden** :
- Environ **38%** des tweets sont positifs
- Environ **22%** sont négatifs
- Et environ **40%** également sont neutres
- La **polarité moyenne** est de **0.07**, légèrement supérieure à Trump

**Qu'est-ce que cela nous dit ?**

La distribution est **relativement équilibrée** pour les deux candidats. Biden affiche une polarité légèrement plus favorable que Trump, mais la différence est minime : 0.07 contre 0.05.

Le fait que **40% des tweets soient neutres** pour les deux candidats montre que la discussion politique sur Twitter reste modérée, contrairement à ce qu'on pourrait penser.

Mais est-ce que cette légère différence de polarité se traduit dans les urnes ? C'est ce que nous allons voir avec la régression linéaire."

---

# SLIDE 8 - La Régression Linéaire : Mon Modèle Prédictif (1 minute 30)

> **[Afficher slide 8]**

"Pour établir une corrélation entre les sentiments Twitter et les votes réels, j'ai implémenté un modèle de **régression linéaire par les moindres carrés**.

**Pourquoi la régression linéaire ?**

L'objectif est de déterminer s'il existe une **relation mathématique** entre deux variables :
- La **variable X** : la polarité moyenne des tweets par État
- La **variable Y** : le pourcentage de votes réels obtenus par chaque candidat dans cet État

La régression linéaire me permet de :
1. **Modéliser cette relation** par une droite d'équation y = mx + b
2. **Quantifier la force de cette relation** avec le coefficient R²
3. **Tester l'hypothèse** : est-ce que la polarité Twitter peut prédire le vote ?

Et j'insiste sur un point important : j'ai codé cette régression **from scratch**, sans utiliser de bibliothèque préexistante comme scikit-learn. L'objectif était de maîtriser chaque aspect du calcul et de vraiment comprendre les fondements mathématiques.

Voici les **formules fondamentales** que j'ai utilisées :

Pour la **pente m** :
$$m = \frac{n \sum xy - \sum x \sum y}{n \sum x^2 - (\sum x)^2}$$

Pour l'**ordonnée à l'origine b** :
$$b = \frac{\sum y - m \sum x}{n}$$

Ces formules sont exactement celles de la régression linéaire par les moindres carrés que nous avons vues en cours.

Le **coefficient R²**, ou coefficient de détermination, est essentiel pour évaluer la qualité de mon modèle. Il mesure la **proportion de la variance des votes** qui est expliquée par la polarité des tweets.

- Un R² proche de 1 signifie que le modèle explique bien les données
- Un R² proche de 0 signifie que le modèle n'explique presque rien

L'**objectif** était d'établir si la polarité moyenne des tweets par État pouvait prédire le pourcentage de votes obtenus par chaque candidat dans ces mêmes États."

---

# SLIDE 9 - Scatterplots : Résultats de la Régression (1 minute 15)

> **[Afficher slide 9]**

"Voici les résultats de ma régression linéaire, présentés sous forme de scatterplots.

Sur le **graphique de gauche**, pour Trump :
- Chaque point représente un État américain
- L'axe X montre la **polarité moyenne des tweets** dans cet État
- L'axe Y montre le **pourcentage de votes pour Trump**
- La droite rouge est ma **droite de régression**
- L'équation est : y = 190.47x + 44.51
- Le **R² = 0.137**

Sur le **graphique de droite**, pour Biden :
- Même principe
- L'équation est : y = 86.52x + 43.86
- Le **R² = 0.026**

**Interprétation des résultats :**

Les corrélations sont **très faibles** :
- Pour **Trump**, R² = 0.137, ce qui signifie que la polarité Twitter n'explique que **13,7%** de la variance du vote
- Pour **Biden**, R² = 0.026, soit seulement **2,6%** de variance expliquée, c'est quasi nul

La **forte dispersion des points** autour de la droite confirme visuellement ces résidus importants. Le modèle ne s'ajuste pas bien aux données.

**Conclusion** : La polarité des tweets ne permet PAS de prédire le vote."

---

# SLIDE 10 - Corrélations : Twitter et l'Urne (45 secondes)

> **[Afficher slide 10]**

"Résumons les corrélations que j'ai trouvées entre Twitter et les résultats électoraux.

**Corrélation faible** : Avec des R² inférieurs à 0.3 pour les deux candidats, Twitter ne constitue pas un indicateur fiable de l'intention de vote. En statistiques, on considère généralement qu'un R² doit être supérieur à 0.5 pour avoir une corrélation significative.

**États gagnés vs perdus** : J'ai également comparé les sentiments Twitter entre les États remportés et perdus par chaque candidat. Résultat : il n'y a **aucune différence significative** dans les sentiments entre ces deux groupes.

**Le sentiment n'est pas le vote** : C'est la conclusion majeure de mon étude. Les sentiments exprimés en ligne ne prédisent pas efficacement les résultats électoraux.

Un tweet positif sur un candidat ne signifie pas que la personne va voter pour lui, et inversement."

---

# SLIDE 11 - Paysage des Sentiments : Perspective Géographique (1 minute)

> **[Afficher slide 11]**

"J'ai également réalisé une analyse géographique avec des cartes choroplèthes créées avec GeoPandas.

**À gauche**, vous voyez les cartes de **polarité moyenne par État** pour Trump et Biden. Les couleurs vont du rouge (négatif) au vert (positif). On observe des variations entre les États, mais ces variations ne sont pas directement corrélées avec les résultats électoraux.

**À droite**, j'ai créé des cartes similaires pour la **subjectivité moyenne**, ainsi qu'une carte des **résultats électoraux officiels** de 2020.

Ce qui est intéressant, c'est qu'on observe des **patterns géographiques partiellement cohérents**. Par exemple, certains États traditionnellement démocrates comme la Californie montrent effectivement une polarité plus favorable à Biden.

Mais ces patterns ne sont pas suffisamment prononcés pour permettre une prédiction fiable. L'analyse de sentiment seule ne suffit pas.

Ces visualisations géographiques confirment visuellement ce que les statistiques nous ont montré : il y a bien des tendances, mais elles sont trop faibles pour être prédictives."

---

# SLIDE 12 - Twitter vs Vote : Les Limites d'une Prédiction (1 minute)

> **[Afficher slide 12]**

"Pourquoi Twitter ne prédit-il pas les élections ? J'ai identifié quatre raisons principales.

**1. Biais démographique**
Les utilisateurs de Twitter sont majoritairement jeunes, urbains et éduqués. Cette population n'est **pas représentative** de l'électorat américain dans son ensemble. Les zones rurales et les personnes âgées, qui votent massivement, sont sous-représentées sur Twitter.

**2. Amplification algorithmique**
Les algorithmes de Twitter favorisent les contenus polarisants car ils génèrent plus d'engagement. Cela **fausse la perception générale** et amplifie les voix les plus extrêmes, qui ne sont pas représentatives de l'opinion moyenne.

**3. Bots et manipulation**
Les comptes automatisés et les campagnes de désinformation influencent le volume et la tonalité des tweets. Pendant l'élection 2020, de nombreuses études ont documenté la présence massive de bots.

**4. Expression ≠ Action**
C'est peut-être le point le plus important : **tweeter n'équivaut pas à voter**. L'expression en ligne ne traduit pas une intention de vote concrète. Quelqu'un peut être très actif sur Twitter sans jamais aller voter."

---

# SLIDE 13 - Synthèse : Le Rôle de Twitter en Politique (45 secondes)

> **[Afficher slide 13]**

"Pour conclure sur le rôle de Twitter en politique, voici ce que mon étude révèle.

**Ce que Twitter capture avec succès :**

- L'**énergie émotionnelle** : Twitter est un excellent baromètre de l'intensité émotionnelle autour des campagnes. On peut mesurer l'engagement, les réactions aux événements.

- Les **patterns géographiques** : Twitter révèle des tendances géographiques dans la discussion politique, même si elles ne sont pas prédictives.

**Les limites de prédiction :**

- Twitter est **non représentatif** de l'électorat en raison des biais démographiques importants.

- Les **sentiments ne sont pas des votes** : l'expression en ligne ne prédit pas l'intention de vote réelle.

**Ma conclusion générale** : Twitter doit être considéré comme un outil **complémentaire** pour comprendre les dynamiques politiques, mais il n'est **pas prédictif** des résultats électoraux."

---

# SLIDE 14 - Leçons Apprises et Horizons Futurs (1 minute)

> **[Afficher slide 14]**

"Pour terminer, je voudrais partager les compétences que j'ai acquises et les pistes d'amélioration.

**Compétences acquises :**

Ce projet m'a permis de renforcer mes compétences dans plusieurs domaines :

- **Big Data** : J'ai appris à gérer et manipuler un dataset de 4,1 millions de tweets, ce qui représente un vrai défi technique.

- **NLP** : J'ai pratiqué l'analyse de sentiments et le nettoyage de texte à grande échelle.

- **Régression linéaire** : J'ai implémenté l'algorithme from scratch, ce qui m'a permis de vraiment comprendre les mathématiques sous-jacentes.

- **Cartographie spatiale** : J'ai appris à utiliser GeoPandas pour créer des visualisations géographiques professionnelles.

**Axes d'amélioration pour des recherches futures :**

- Intégrer une **analyse temporelle** des tweets pour voir l'évolution des sentiments au fil de la campagne.

- Explorer des modèles de **Machine Learning plus avancés** comme Random Forest ou les réseaux de neurones qui pourraient capturer des relations non linéaires.

- Implémenter des méthodes de **détection de bots** pour affiner les données et exclure les comptes automatisés.

Je vous remercie pour votre attention."

---

# SLIDE 15 - Merci / Questions

> **[Afficher slide 15]**

"Merci beaucoup pour votre écoute. Je suis maintenant disponible pour répondre à vos questions."

---

# 📋 AIDE-MÉMOIRE : Questions fréquentes du jury

## Sur la méthodologie

**Q: Pourquoi avoir choisi TextBlob plutôt qu'un autre outil ?**
> "TextBlob est simple d'utilisation et donne directement polarité et subjectivité. Pour une analyse plus poussée, j'aurais pu utiliser BERT ou VADER, mais TextBlob était suffisant pour cette étude exploratoire."

**Q: Pourquoi filtrer les tweets avant 20h ?**
> "Pour ne pas inclure les réactions aux premiers résultats qui auraient biaisé l'analyse. Je voulais mesurer les sentiments AVANT le vote, pas APRÈS."

**Q: Comment avez-vous géré les 4 millions de tweets ?**
> "J'ai utilisé pandas avec un chargement par chunks pour optimiser la mémoire. Le filtrage précoce a réduit la taille du dataset de moitié."

## Sur les mathématiques

**Q: Expliquez le coefficient R² ?**
> "R² mesure la proportion de variance expliquée par le modèle. Un R² de 0.137 signifie que 13.7% de la variation des votes est expliquée par la polarité Twitter. C'est très faible."

**Q: Pourquoi avoir implémenté la régression from scratch ?**
> "Pour vraiment comprendre les mathématiques derrière l'algorithme. Calculer m et b manuellement m'a permis de maîtriser chaque étape du processus."

**Q: Qu'est-ce qu'un bon R² ?**
> "Généralement, R² > 0.7 est considéré comme bon, R² > 0.5 comme acceptable. Mes R² de 0.137 et 0.026 sont donc très faibles."

## Sur les résultats

**Q: Pourquoi le R² de Biden est-il plus faible que celui de Trump ?**
> "Possiblement parce que les supporters de Biden étaient moins expressifs sur Twitter, ou que le dataset Biden contient plus de bruit. C'est une piste d'investigation."

**Q: Êtes-vous surpris par ces résultats ?**
> "Pas vraiment. La littérature scientifique montre que les réseaux sociaux ne prédisent pas bien les élections. Mon étude confirme ces travaux."

---

# ⏱️ TIMING RÉCAPITULATIF

| Slide | Durée | Cumul |
|-------|-------|-------|
| 1 - Introduction | 45s | 0:45 |
| 2 - Contexte | 1:00 | 1:45 |
| 3 - Technologies | 1:00 | 2:45 |
| 4 - Données | 0:45 | 3:30 |
| 5 - Pipeline | 1:30 | 5:00 |
| 6 - Sentiments | 1:15 | 6:15 |
| 7 - Distribution | 1:00 | 7:15 |
| 8 - Régression | 1:30 | 8:45 |
| 9 - Scatterplots | 1:15 | 10:00 |
| 10 - Corrélations | 0:45 | 10:45 |
| 11 - Cartes | 1:00 | 11:45 |
| 12 - Limites | 1:00 | 12:45 |
| 13 - Synthèse | 0:45 | 13:30 |
| 14 - Leçons | 1:00 | 14:30 |
| 15 - Merci | - | - |

**✅ Total : ~14 minutes 30 secondes**

> 💡 **Conseil** : Parle calmement et fais des pauses. Si tu vas trop vite, tu peux développer les slides 5, 8 et 9 qui sont les plus techniques.

---

# 🎯 CONSEILS DE DERNIÈRE MINUTE

1. **Regarde le jury**, pas tes slides
2. **Pointe les éléments** sur l'écran quand tu les mentionnes
3. **Fais des pauses** après les points importants
4. **Si tu oublies quelque chose**, passe à la suite, ne reste pas bloqué
5. **Pour les questions** : prends 2 secondes pour réfléchir avant de répondre

**Bonne chance ! 🍀**
