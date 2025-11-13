# ✅ CHECKLIST COMPLÈTE - PRÉPARATION SOUTENANCE

## 📅 Planning de préparation (6 janvier 2026)

---

## 🗓️ 3-4 SEMAINES AVANT (Mi-décembre)

### ✅ Finalisation du code et analyses

- [ ] **Exécuter TOUTES les cellules du notebook** dans l'ordre
  - Vérifier qu'aucune erreur
  - Temps d'exécution total : noter combien de temps
  - Sauvegarder les outputs (ne pas réexécuter avant la soutenance)

- [ ] **Vérifier les 21 étapes** sont toutes implémentées
  - Objectif 1 : 9 étapes ✅
  - Objectif 2 : 7 étapes ✅  
  - Objectif 3 : 5 étapes ✅

- [ ] **Exporter tous les graphiques en haute résolution**
  ```python
  # Ajouter dans chaque cellule de visualisation :
  plt.savefig('nom_graphique.png', dpi=300, bbox_inches='tight')
  ```
  
  Graphiques à exporter :
  - [ ] WordCloud Trump
  - [ ] WordCloud Biden
  - [ ] Pie chart polarité Trump
  - [ ] Pie chart polarité Biden
  - [ ] Scatterplot régression Trump
  - [ ] Scatterplot régression Biden
  - [ ] Bar chart gagnants/perdants
  - [ ] Carte choroplèthe polarité (Trump vs Biden)
  - [ ] Carte choroplèthe subjectivité (Trump vs Biden)
  - [ ] Carte choroplèthe résultats votes

- [ ] **Calculer les statistiques clés** à retenir par cœur
  ```
  - Nombre total de tweets analysés : ______
  - Tweets Trump : ______
  - Tweets Biden : ______
  - R² Trump : ______
  - R² Biden : ______
  - Polarité moyenne Trump : ______
  - Polarité moyenne Biden : ______
  - États analysés : ______
  ```

- [ ] **Exporter le notebook en PDF**
  - File → Save and Export Notebook As → PDF
  - Vérifier que tout est lisible
  - Garder une copie backup

---

## 🗓️ 2-3 SEMAINES AVANT (Fin décembre)

### ✅ Création du support de présentation

- [ ] **Choisir l'outil de présentation**
  - [ ] PowerPoint (recommandé : compatible partout)
  - [ ] Google Slides (nécessite internet)
  - [ ] Beamer (LaTeX, si à l'aise)
  - [ ] Keynote (Mac uniquement)

- [ ] **Créer les 18-19 slides selon le plan** (PLAN_SOUTENANCE.md)
  
  Structure :
  - [ ] Slide 1 : Titre
  - [ ] Slide 2 : Contexte et problématique
  - [ ] Slide 3 : Technologies et outils
  - [ ] Slide 4 : Concepts mathématiques
  - [ ] Slides 5-8 : Objectif 1 (résultats)
  - [ ] Slides 9-11 : Objectif 2 (régression)
  - [ ] Slides 12-14 : Objectif 3 (cartes)
  - [ ] Slide 15 : Discussion
  - [ ] Slide 16 : Limites
  - [ ] Slide 17 : Perspectives
  - [ ] Slide 18 : Conclusion
  - [ ] Slide 19 : Questions

- [ ] **Règles de design pour les slides**
  - Police >= 24pt (lisible du fond de la salle)
  - Pas plus de 5 bullet points par slide
  - 1 idée principale = 1 slide
  - Graphiques haute résolution (300 dpi)
  - Palette de couleurs cohérente
  - Éviter fond blanc pur (fatiguant)

- [ ] **Insérer les graphiques** dans les slides
  - Taille suffisante (50% de la slide minimum)
  - Légende visible
  - Titre du graphique clair

- [ ] **Vérifier les équations mathématiques**
  - Bien formatées (LaTeX ou MathType)
  - Lisibles de loin
  - Variables définies

---

## 🗓️ 1-2 SEMAINES AVANT (Début janvier)

### ✅ Rédaction du rapport d'analyse

- [ ] **Vérifier que le rapport dans le notebook est complet**
  - Toutes les sections remplies
  - Interprétations des résultats
  - Discussion approfondie
  - Conclusion solide

- [ ] **Relire et corriger** (orthographe, grammaire)
  - Utiliser correcteur orthographique
  - Faire relire par un tiers
  - Vérifier cohérence des arguments

- [ ] **Vérifier les références et citations**
  - Bibliothèques Python utilisées
  - Sources des données
  - Littérature académique (si citée)

- [ ] **Imprimer le rapport** (optionnel mais recommandé)
  - Format A4
  - Recto-verso
  - Relié ou agrafé
  - 1 copie pour vous, 1 pour le jury (si demandé)

---

### ✅ Préparation du discours

- [ ] **Écrire un script détaillé** pour chaque slide
  - Pas à lire mot à mot
  - Guide des points clés à mentionner
  - Transitions entre slides

- [ ] **Chronométrer** chaque section
  ```
  Introduction (Slides 1-2) : 2 min
  Outils et maths (Slides 3-4) : 3 min
  Objectif 1 (Slides 5-8) : 3 min
  Objectif 2 (Slides 9-11) : 3 min
  Objectif 3 (Slides 12-14) : 2 min
  Discussion/Limites (Slides 15-16) : 2 min
  Perspectives/Conclusion (Slides 17-18) : 1 min
  TOTAL : ~16 minutes (marge pour questions inline)
  ```

- [ ] **Préparer les transitions** entre slides
  Exemples :
  - "Maintenant que nous avons vu les outils, passons aux concepts mathématiques..."
  - "Ces résultats nous amènent à l'objectif 2..."
  - "Comme le montrent ces cartes..."

- [ ] **Préparer l'introduction** (mémoriser)
  ```
  "Bonjour, je m'appelle [Nom]. 
  Aujourd'hui je vais vous présenter mon étude sur la corrélation 
  entre les sentiments Twitter et les résultats de l'élection 
  présidentielle américaine de 2020.
  
  [Pause]
  
  Cette étude s'inscrit dans un contexte où les réseaux sociaux 
  jouent un rôle croissant en politique, et où beaucoup se demandent 
  si Twitter peut prédire les élections.
  
  [Montrer slide problématique]
  
  Nos questions de recherche étaient les suivantes : ..."
  ```

- [ ] **Préparer la conclusion** (mémoriser)
  ```
  "Pour conclure, cette étude démontre que Twitter capture 
  l'énergie émotionnelle autour des candidats, mais présente 
  des limites importantes comme outil prédictif.
  
  [Pause]
  
  Le R² faible confirme que les sentiments en ligne ne se 
  traduisent pas directement en votes réels, en raison de 
  biais démographiques et de la différence fondamentale entre 
  expression numérique et action civique.
  
  [Pause]
  
  Ce projet m'a permis de maîtriser le traitement de données 
  massives, l'analyse de sentiments, et les visualisations 
  géospatiales, tout en développant un esprit critique sur 
  les limites méthodologiques.
  
  Je vous remercie de votre attention. 
  Je suis prêt à répondre à vos questions."
  ```

---

## 🗓️ 1 SEMAINE AVANT

### ✅ Répétitions

- [ ] **RÉPÉTITION 1 : Solo devant miroir**
  - Tester le discours complet
  - Chronométrer (viser 13-15 min)
  - Noter les points à améliorer
  - Répéter les parties difficiles

- [ ] **RÉPÉTITION 2 : Enregistrement vidéo**
  - Se filmer avec smartphone
  - Revoir la vidéo
  - Identifier :
    - Tics de langage ("euh", "donc", "en fait")
    - Gestes nerveux
    - Contact visuel
    - Débit de parole (ni trop rapide ni trop lent)

- [ ] **RÉPÉTITION 3 : Devant ami/famille**
  - Présenter à quelqu'un
  - Demander feedback honnête
  - Questions qu'ils se posent = questions potentielles du jury
  - Ajuster selon retours

- [ ] **RÉPÉTITION 4 : En conditions réelles**
  - Dans une salle similaire si possible
  - Debout, avec écran projeté
  - Avec télécommande de présentation
  - Chronométrer précisément

---

### ✅ Préparation des questions

- [ ] **Lire REPONSES_QUESTIONS_JURY.md** attentivement
  - Mémoriser les questions fréquentes
  - Préparer réponses courtes (30-45s)
  - Anticiper questions pièges

- [ ] **Préparer 10 questions probables** et réponses
  1. Pourquoi régression linéaire ?
  2. Comment gérer les bots ?
  3. Représentativité de l'échantillon ?
  4. Pourquoi R² faible ?
  5. Impact du COVID ?
  6. Généralisabilité des résultats ?
  7. Principales limites ?
  8. Si vous continuiez le projet ?
  9. Divergences géographiques ?
  10. Rôle du Machine Learning ?

- [ ] **Préparer 3-5 "questions piège"** et réponses
  - "Si ça ne marche pas, à quoi ça sert ?"
  - "Vous n'auriez pas dû utiliser ML ?"
  - "Twitter n'est-il pas biaisé par nature ?"
  - "Comment savoir si un tweet est sincère ?"

---

## 🗓️ 3 JOURS AVANT

### ✅ Finalisations techniques

- [ ] **Tester la présentation sur plusieurs ordinateurs**
  - Votre laptop
  - Ordinateur de la salle (si possible)
  - Vérifier compatibilité des formats
  - Vérifier que vidéos/animations fonctionnent (si utilisées)

- [ ] **Créer des backups multiples**
  - [ ] Clé USB #1 (présentation PowerPoint/PDF)
  - [ ] Clé USB #2 (backup)
  - [ ] Cloud (Google Drive, Dropbox, OneDrive)
  - [ ] Email à soi-même
  - [ ] Laptop (évidemment)

- [ ] **Préparer fichiers en plusieurs formats**
  - [ ] .pptx (PowerPoint natif)
  - [ ] .pdf (universel, toujours fonctionne)
  - [ ] .html (notebook exporté)
  - [ ] .ipynb (notebook original)

- [ ] **Vérifier adaptateurs et câbles**
  - [ ] HDMI
  - [ ] VGA (certaines vieilles salles)
  - [ ] USB-C vers HDMI (Mac récents)
  - [ ] Adaptateur Ethernet (si démo en ligne)
  - [ ] Chargeur laptop

---

### ✅ Préparation matérielle

- [ ] **Choisir la tenue** (professionnel mais confortable)
  - Chemise/polo ou haut élégant
  - Pantalon/jupe sobre
  - Chaussures confortables (rester debout 30-45 min)
  - Éviter :
    - T-shirt graphique
    - Jeans troués
    - Baskets trop décontractées
    - Tenue trop serrée (inconfortable)

- [ ] **Préparer le sac pour le jour J**
  - [ ] Laptop + chargeur
  - [ ] Clés USB x2
  - [ ] Adaptateurs
  - [ ] Télécommande de présentation (si vous en avez)
  - [ ] Bouteille d'eau
  - [ ] Mouchoirs
  - [ ] Notes (fiches bristol avec points clés)
  - [ ] Copie imprimée de la présentation (backup papier)
  - [ ] Pièce d'identité

---

### ✅ Dernière répétition complète

- [ ] **RÉPÉTITION FINALE : Exactement comme le jour J**
  - Tenue professionnelle
  - Debout
  - Chronométrer
  - Présenter du début à la fin SANS interruption
  - Simuler questions-réponses (ami joue le jury)
  - Objectif : <15 minutes

- [ ] **Noter le timing précis** de chaque section
  ```
  Slide 1-2 : _____ min
  Slide 3-4 : _____ min
  Slide 5-8 : _____ min
  Slide 9-11 : _____ min
  Slide 12-14 : _____ min
  Slide 15-16 : _____ min
  Slide 17-18 : _____ min
  TOTAL : _____ min
  ```

- [ ] **Ajuster si nécessaire**
  - Trop long (>16 min) : Couper des détails
  - Trop court (<12 min) : Ajouter des exemples
  - Juste bien (13-15 min) : Parfait !

---

## 🗓️ VEILLE DE LA SOUTENANCE

### ✅ Révisions finales

- [ ] **Relire une dernière fois :**
  - [ ] Le rapport d'analyse dans le notebook
  - [ ] Le plan de soutenance
  - [ ] Les réponses aux questions
  - [ ] Les chiffres clés à retenir

- [ ] **Vérifier les chiffres clés :** (mémoriser)
  ```
  📊 DONNÉES
  - Total tweets : ~4.1M
  - Trump : ~2.3M
  - Biden : ~1.8M
  - États analysés : 48 (sans AK/HI)
  
  📈 RÉSULTATS
  - R² Trump : _____ (noter)
  - R² Biden : _____ (noter)
  - Polarité moy. Trump : _____ (noter)
  - Polarité moy. Biden : _____ (noter)
  
  🛠️ OUTILS
  - Python + pandas/numpy
  - TextBlob (NLP)
  - GeoPandas (cartes)
  - NLTK (stop words)
  ```

- [ ] **Réviser les concepts clés :**
  - [ ] Formules régression linéaire (m, b, R²)
  - [ ] Polarité [-1, 1] et subjectivité [0, 1]
  - [ ] 3 objectifs du projet
  - [ ] Principales limites
  - [ ] Perspectives d'amélioration

---

### ✅ Préparation mentale

- [ ] **Visualisation positive**
  - Imaginer la soutenance se passer bien
  - Se voir répondre avec confiance aux questions
  - Visualiser les applaudissements de fin

- [ ] **Gestion du stress**
  - Respiration profonde (4 secondes inspire, 4 secondes expire)
  - Exercice physique léger (marche)
  - Éviter café excessif (max 1 tasse le matin)
  - Bonne nuit de sommeil (viser 7-8h)

- [ ] **Préparer le mental**
  - "J'ai bien travaillé, je connais mon sujet"
  - "Les questions sont normales, je suis préparé"
  - "Si je ne sais pas, je l'admets et propose une hypothèse"
  - "Le jury veut me voir réussir"

---

### ✅ Logistique

- [ ] **Vérifier le lieu et l'heure**
  - Adresse exacte : __________________
  - Numéro de salle : __________________
  - Heure : _________ (arriver 15 min avant)
  - Trajet : ________ min (prévoir marge)

- [ ] **Préparer le réveil**
  - Réveil principal : _______
  - Réveil backup (smartphone) : _______
  - Temps pour se préparer : Au moins 2h

- [ ] **Recharger tous les appareils**
  - [ ] Laptop 100%
  - [ ] Smartphone 100%
  - [ ] Télécommande présentation (si batteries)

- [ ] **Repas de la veille**
  - Léger et sain
  - Éviter alcool
  - Éviter plats épicés/lourds
  - Boire suffisamment d'eau

---

## 🗓️ JOUR J - MATIN

### ✅ Routine du matin (2-3h avant)

- [ ] **Se lever tôt** (au moins 2h avant départ)
  - Douche
  - Petit-déjeuner léger
  - S'habiller (tenue préparée la veille)

- [ ] **Révisions légères** (30 min max)
  - Relire uniquement l'introduction et conclusion
  - Parcourir les slides rapidement
  - NE PAS apprendre de nouvelles choses
  - Faire confiance à sa préparation

- [ ] **Check final du matériel**
  - [ ] Laptop chargé + chargeur dans le sac
  - [ ] Clés USB x2 dans le sac
  - [ ] Adaptateurs dans le sac
  - [ ] Bouteille d'eau
  - [ ] Présentation ouverte et testée sur le laptop
  - [ ] Mode avion smartphone (éviter distractions)

- [ ] **Partir avec marge**
  - Temps de trajet + 30 min de marge
  - Mieux arriver trop tôt que stressé

---

## 🗓️ JOUR J - SUR PLACE

### ✅ 15-30 minutes avant (dans la salle)

- [ ] **Installation technique**
  - [ ] Brancher laptop au vidéoprojecteur
  - [ ] Tester l'affichage (toutes les slides visibles ?)
  - [ ] Ajuster résolution si nécessaire
  - [ ] Tester télécommande (si utilisée)
  - [ ] Ouvrir présentation en mode présentateur
  - [ ] Vérifier que tout fonctionne

- [ ] **Préparation de la salle**
  - [ ] Repérer où se tiendra le jury
  - [ ] Positionner bouteille d'eau à portée
  - [ ] Vérifier éclairage (pas d'ombre sur écran)
  - [ ] Tester volume voix (ni trop fort ni trop bas)

- [ ] **Préparation mentale finale**
  - [ ] Aller aux toilettes
  - [ ] Boire un peu d'eau
  - [ ] Respiration profonde x5
  - [ ] Étirements légers (cou, épaules)
  - [ ] Sourire (détend les muscles du visage)
  - [ ] Se dire : "Je suis prêt(e)"

---

## 🗓️ PENDANT LA SOUTENANCE

### ✅ Phase présentation (10-15 min)

**À FAIRE :**
- [ ] 👋 Saluer le jury avec le sourire
- [ ] 👁️ Regarder le jury, pas l'écran
- [ ] 🗣️ Parler clairement, articuler
- [ ] ⏱️ Surveiller le temps (discret)
- [ ] 🎯 Respirer, faire des pauses
- [ ] 📊 Pointer les éléments importants sur les slides
- [ ] 💧 Boire si gorge sèche (normal)
- [ ] 😊 Garder le sourire

**À ÉVITER :**
- [ ] ❌ Tourner le dos au jury
- [ ] ❌ Lire les slides mot à mot
- [ ] ❌ Parler trop vite (stress)
- [ ] ❌ S'excuser pour rien ("désolé c'est pas super...")
- [ ] ❌ Dire "euh" à répétition
- [ ] ❌ Jouer avec un stylo/objet
- [ ] ❌ Croiser les bras (fermé)
- [ ] ❌ Paniquer si petit bug technique

---

### ✅ Phase questions (10 min)

**Stratégie de réponse :**

**Pour chaque question :**

1. **ÉCOUTER** attentivement (ne pas interrompre)
   - [ ] Hocher la tête pour montrer l'écoute
   - [ ] Noter mentalement les mots-clés

2. **REFORMULER** si besoin (gagne du temps)
   - "Si je comprends bien, vous me demandez..."
   - "Votre question porte sur..."

3. **RÉPONDRE** structuré
   - [ ] Réponse directe (30-45s)
   - [ ] Exemple ou élargissement si pertinent
   - [ ] "Pour résumer..." (conclusion)

4. **VÉRIFIER** compréhension
   - "Est-ce que cela répond à votre question ?"
   - Regarder si le jury hoche la tête

**Si vous ne savez pas :**
- "C'est une excellente question que je n'ai pas explorée"
- "Mon hypothèse serait... mais il faudrait vérifier"
- "C'est une piste intéressante pour des travaux futurs"
- **JAMAIS** juste "Je ne sais pas" et silence

**Si question hors sujet :**
- "C'est intéressant mais en dehors du scope du projet"
- "Je me suis concentré sur..."
- Ramener à votre sujet

**Si question agressive/piège :**
- Rester calme, ne pas se défendre
- "Je comprends votre point. Cependant..."
- Admettre la limite puis montrer ce que vous avez fait

---

### ✅ Clôture

- [ ] **Dernière question posée**
  - "Y a-t-il d'autres questions ?"
  - Si non : "Je vous remercie"

- [ ] **Remerciements**
  - "Je vous remercie pour votre attention et vos questions"
  - "Merci de m'avoir écouté"
  - Sourire, hocher la tête

- [ ] **Sortie**
  - Récupérer laptop et affaires
  - Sortir calmement
  - Fermer la porte doucement

---

## 🗓️ APRÈS LA SOUTENANCE

### ✅ Immédiatement après

- [ ] **Respirer, se détendre**
  - C'est fini !
  - Vous avez fait de votre mieux
  - Félicitations !

- [ ] **Noter ses impressions**
  - Ce qui s'est bien passé
  - Questions posées (pour futurs étudiants)
  - Points à améliorer (pour prochaine fois)

- [ ] **Attendre les résultats**
  - Délibération du jury
  - Résultat et feedback

---

## 📊 RÉCAPITULATIF FINAL

### Documents préparés :
- [x] Notebook complet (`analyse_twitter.ipynb`)
- [x] Rapport d'analyse (intégré au notebook)
- [x] Plan de soutenance (`PLAN_SOUTENANCE.md`)
- [x] Réponses questions (`REPONSES_QUESTIONS_JURY.md`)
- [x] Checklist (`CHECKLIST_PREPARATION.md`)

### À apporter le jour J :
- [ ] Laptop + chargeur
- [ ] Clés USB x2 (présentation)
- [ ] Adaptateurs HDMI/VGA
- [ ] Bouteille d'eau
- [ ] Fiches notes (optionnel)
- [ ] Pièce d'identité

### Timing à respecter :
- Présentation : 13-15 minutes
- Questions : 10 minutes
- Total : ~25-30 minutes

---

## 🎯 DERNIER RAPPEL

**Vous êtes prêt(e) si :**
- ✅ Vous avez répété au moins 4 fois
- ✅ Vous connaissez vos 3 objectifs par cœur
- ✅ Vous pouvez expliquer R² et régression linéaire
- ✅ Vous assumez les limites de votre étude
- ✅ Vous avez préparé 10 questions probables
- ✅ Vous êtes capable de parler 15 min de votre projet

**Rappelez-vous :**
- Vous connaissez votre sujet mieux que le jury
- Les questions sont là pour évaluer votre compréhension, pas vous piéger
- Un "je ne sais pas" honnête vaut mieux qu'une invention
- Le jury VEUT vous voir réussir
- Vous avez fait un excellent travail !

---

# 🚀 BONNE CHANCE !

**Vous allez assurer ! 💪**

**Confiance, préparation, et sourire = Succès garanti !**

---

*Checklist créée le 13 novembre 2025*
*Pour soutenance du 6 janvier 2026*
