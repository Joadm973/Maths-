# TP Maths - Étude des corrélations dans les élections sur Twitter

## 📊 Description du projet

Analyse statistique et géographique des tweets liés aux élections américaines de 2020, étudiant les corrélations entre le sentiment Twitter et les résultats de vote.

## 🎯 Objectifs du TP

1. **Analyse de sentiment** : Calcul de la polarité et subjectivité des tweets
2. **Corrélations statistiques** : Régressions linéaires entre sentiment Twitter et résultats électoraux
3. **Visualisations géographiques** : Cartes choroplèthes par État

## 📁 Structure du projet

- `analyse_twitter.ipynb` : Notebook principal contenant toute l'analyse
- `cb_2018_us_state_500k.zip` : Shapefile des États américains
- Documentation :
  - `PLAN_SOUTENANCE.md` : Structure de la présentation orale
  - `REPONSES_QUESTIONS_JURY.md` : Réponses aux questions potentielles du jury
  - `CHECKLIST_PREPARATION.md` : Liste de vérification pour la préparation
  - `README_SOUTENANCE.md` : Guide complet pour la soutenance

## 📥 Données requises

**⚠️ Les fichiers CSV sont trop volumineux pour GitHub (> 100 MB)**

Vous devez télécharger les fichiers de données suivants :
- `hashtag_donaldtrump.csv` (461 MB)
- `hashtag_joebiden.csv` (363 MB)
- `ap_votes.csv`

Placez ces fichiers dans le même répertoire que le notebook avant d'exécuter l'analyse.

## 🚀 Installation et exécution

### Prérequis

```bash
pip install pandas numpy matplotlib seaborn textblob langdetect nltk wordcloud geopandas shapely
```

### Téléchargement des ressources NLTK

```python
import nltk
nltk.download('stopwords')
```

### Exécution

1. Clonez ce dépôt
2. Téléchargez les fichiers CSV (voir section "Données requises")
3. Ouvrez `analyse_twitter.ipynb` dans Jupyter
4. Exécutez toutes les cellules

## 📈 Technologies utilisées

- **Python 3.x**
- **Analyse de données** : pandas, numpy
- **Visualisation** : matplotlib, seaborn, wordcloud
- **NLP** : TextBlob, langdetect, nltk
- **Cartographie** : GeoPandas, shapely

## 👤 Auteur

Projet réalisé dans le cadre du cours de Mathématiques - B3 Ynov

## 📝 Licence

Projet académique - Tous droits réservés
