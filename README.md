# TP Maths - Study of Correlations in Elections on Twitter

## Description

Statistical and geographical analysis of tweets related to the 2020 American elections, studying the correlations between Twitter sentiment and voting results.

## Objectives

1. **Sentiment Analysis**: Calculation of polarity and subjectivity of tweets
2. **Statistical Correlations**: Linear regressions between Twitter sentiment and electoral results
3. **Geographical Visualizations**: Choropleth maps by state

## Project Structure

- `analyse_twitter.ipynb`: Main notebook containing all analysis
- `cb_2018_us_state_500k.zip`: Shapefile of American states
- Documentation:
  - `PLAN_SOUTENANCE.md`: Structure of the oral presentation
  - `REPONSES_QUESTIONS_JURY.md`: Answers to potential jury questions
  - `CHECKLIST_PREPARATION.md`: Preparation checklist
  - `README_SOUTENANCE.md`: Complete guide for the presentation

## Required Data

**Note: CSV files are too large for GitHub (> 100 MB)**

You must download the following data files:
- `hashtag_donaldtrump.csv` (461 MB)
- `hashtag_joebiden.csv` (363 MB)
- `ap_votes.csv`

Place these files in the same directory as the notebook before running the analysis.

## Installation and Setup

### Prerequisites

```bash
pip install pandas numpy matplotlib seaborn textblob langdetect nltk wordcloud geopandas shapely
```

### Download NLTK Resources

```python
import nltk
nltk.download('stopwords')
```

### Execution

1. Clone this repository
2. Download the CSV files (see "Required Data" section)
3. Open `analyse_twitter.ipynb` in Jupyter
4. Run all cells

## Technologies Used

- **Python 3.x**
- **Data Analysis**: pandas, numpy
- **Visualization**: matplotlib, seaborn, wordcloud
- **NLP**: TextBlob, langdetect, nltk
- **Mapping**: GeoPandas, shapely

## Author

Project completed as part of the Mathematics course - B3 Ynov

## License

Academic project - All rights reserved
