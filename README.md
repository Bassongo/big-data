# Analyse de l'Impact Météorologique sur la Consommation Énergétique Européenne

[![PySpark](https://img.shields.io/badge/PySpark-3.5.1-orange.svg)](https://spark.apache.org/)
[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Academic-green.svg)]()

**Projet BDCC 2025 - Groupe 2** - Analyse des données météorologiques avec Apache Spark pour comprendre leur impact sur l'économie énergétique européenne.

---

## Table des matières

- [Vue d'ensemble](#vue-densemble)
- [Objectifs du projet](#objectifs-du-projet)
- [Architecture technique](#architecture-technique)
- [Structure du projet](#structure-du-projet)
- [Installation et configuration](#installation-et-configuration)
- [Utilisation](#utilisation)
- [Résultats clés](#résultats-clés)
- [Technologies utilisées](#technologies-utilisées)
- [Équipe](#équipe)
- [Documentation](#documentation)
- [Licence](#licence)

---

## Vue d'ensemble

Ce projet analyse l'impact des conditions météorologiques (température, vent, rayonnement solaire) sur la consommation électrique et la production d'énergies renouvelables en Europe. En utilisant Apache Spark pour le traitement distribué de données massives, nous avons identifié des corrélations significatives et réalisé des analyses de scénarios pour différentes conditions climatiques.

### Contexte

Dans un contexte de transition énergétique et de changement climatique, comprendre les relations entre météo et énergie est crucial pour :
- Anticiper les pics de consommation
- Optimiser la production d'énergies renouvelables
- Planifier les infrastructures énergétiques
- Adapter les politiques énergétiques aux conditions climatiques

---

## Objectifs du projet

### Objectif principal
Apprendre à utiliser Apache Spark pour le traitement de grandes quantités de données et analyser l'impact économique des phénomènes météorologiques sur le secteur énergétique.

### Objectifs spécifiques

1. **Traitement Big Data**
   - Manipuler des datasets de plusieurs centaines de milliers d'observations
   - Implémenter des pipelines de traitement distribué avec PySpark
   - Optimiser les performances de calcul

2. **Analyse statistique**
   - Identifier les corrélations météo-énergie
   - Valider statistiquement les différences observées (tests ANOVA, t-test)
   - Quantifier l'impact des températures extrêmes

3. **Analyse de scénarios**
   - Analyser l'impact des canicules sur la consommation
   - Étudier l'impact du grand froid
   - Comparer les comportements énergétiques entre pays

4. **Visualisation et interprétation**
   - Créer des visualisations professionnelles
   - Interpréter les résultats pour des décideurs

---

## Architecture technique

### Pipeline de traitement des données

```
┌─────────────────────┐
│  Sources de données │
│  • OPSD (énergie)   │
│  • Open-Meteo (API) │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Ingestion          │
│  • Téléchargement   │
│  • Chunking         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Transformation     │
│  • PySpark          │
│  • Unpivot          │
│  • Agrégations      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Fusion             │
│  • Join météo-éner. │
│  • Feature engineer │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Analyses           │
│  • Corrélations     │
│  • Tests stats      │
│  • Scénarios        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Visualisations     │
│  • Matplotlib       │
│  • Seaborn          │
└─────────────────────┘
```

### Stack technique détaillée

**Big Data & Traitement distribué**
- Apache Spark 3.5.1
- PySpark (DataFrame API)
- Spark SQL

**Data Science & Analyse statistique**
- pandas, numpy
- scipy (tests statistiques)

**Visualisation**
- matplotlib, seaborn
- PIL (visualisations professionnelles)

**Infrastructure**
- Google Colab (environnement d'exécution)
- Google Drive (stockage des résultats)

---

## Structure du projet

```
big-data/
│
├── README.md                          # Ce fichier
├── requirements.txt                   # Dépendances Python
│
├── notebooks/
│   └── projet_big_data.ipynb         # Notebook principal d'analyse
│
├── data/
│   ├── raw/                          # Données brutes (non versionnées)
│   │   ├── opsd_time_series.csv
│   │   └── meteo_*.csv
│   └── processed/                    # Données traitées
│       └── energy_meteo_merged.parquet
│
├── outputs/                          # Résultats et visualisations
│   ├── figures/
│   │   ├── visual_1_correlations.png
│   │   ├── visual_2_significance.png
│   │   ├── visual_3_models.png
│   │   └── visual_4_impact.png
│   └── reports/
│       └── resultats_essentiels.md
│
└── presentation/                     # Support de présentation
    └── slides_projet_bdcc.pdf
```

---

## Installation et configuration

### Prérequis

- Python 3.9 ou supérieur
- Java 8 ou 11 (pour PySpark)
- Minimum 8 GB RAM recommandés
- Connexion internet (pour téléchargement des données)

### Installation

#### Option 1 : Google Colab (recommandé)

```python
# Installer PySpark
!pip install pyspark==3.5.1 openpyxl delta-spark -q

# Monter Google Drive
from google.colab import drive
drive.mount('/content/drive')

# Cloner le repository
!git clone https://github.com/Bassongo/big-data.git
```

#### Option 2 : Installation locale

```bash
# Cloner le repository
git clone https://github.com/Bassongo/big-data.git
cd big-data

# Créer un environnement virtuel
python -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate

# Installer les dépendances
pip install -r requirements.txt
```

### Configuration

Le notebook utilise les configurations suivantes :

```python
# Configuration Spark
spark = SparkSession.builder \
    .appName("MeteoEnergie") \
    .config("spark.driver.memory", "4g") \
    .master("local[*]") \
    .getOrCreate()

# Sources de données
OPSD_URL = "https://data.open-power-system-data.org/time_series/2020-10-06/time_series_60min_singleindex.csv"
METEO_API = "https://archive-api.open-meteo.com/v1/archive"
```

---

## Utilisation

### Exécution du notebook

Le notebook est structuré en sections principales :

1. **Installation et configuration**
   - Installation de PySpark
   - Configuration de l'environnement Spark

2. **Téléchargement des données**
   - Téléchargement des données énergétiques (OPSD)
   - Téléchargement des données météorologiques (Open-Meteo API)
   - Nettoyage et sauvegarde

3. **Analyse des données**
   - Exploration et description des données
   - Analyse des corrélations
   - Impact des phénomènes météorologiques
   - Tests statistiques de significativité

4. **Analyse des scénarios**
   - Scénarios de températures extrêmes
   - Comparaisons par pays
   - Visualisations

### Exécution complète

```bash
# Ouvrir le notebook dans Jupyter
jupyter notebook notebooks/projet_big_data.ipynb

# Ou dans Google Colab
# Fichier > Ouvrir le notebook > GitHub > https://github.com/Bassongo/big-data
```

---

## Résultats clés

### 1. Corrélations identifiées

| Relation | Coefficient | Interprétation |
|----------|------------|----------------|
| Température × Consommation | 0.0066 | Très faible (varie selon saison) |
| Rayonnement × Production solaire | 0.2306 | Modérée |
| Vent × Production éolienne | 0.2621 | Modérée |

**Corrélations saisonnières** (Température × Consommation) :
- Hiver : 0.1076 (positive - chauffage)
- Été : 0.1267 (positive - climatisation)
- Intersaison : 0.0199 (quasi nulle)

**Interprétation** : La corrélation température-consommation est très faible globalement car elle varie selon les saisons. En hiver, plus il fait froid, plus on consomme (chauffage). En été, plus il fait chaud, plus on consomme (climatisation).

### 2. Impact des températures extrêmes

#### Analyse multi-pays (moyenne européenne)

Cette analyse porte sur 32 pays européens du dataset.

**Consommation moyenne par période**
```
Normale (0-25°C) :  10,955 MW
Canicule (>25°C) :  12,794 MW  (+16.8%)
Froid (<0°C) :       9,610 MW  (-12.3%)
```

**Tests statistiques**

Test ANOVA global
- F-statistique : 48.00
- p-value : 1.48e-21 (< 0.001)
- **Conclusion** : Les différences sont TRÈS significatives

**Comparaisons par paires** (tests t de Student)

| Comparaison | Différence | p-value | Significativité |
|-------------|-----------|---------|-----------------|
| Normale vs Canicule | +16.8% (+1,840 MW) | 2.66e-08 | *** |
| Normale vs Froid | -12.3% (-1,344 MW) | 4.17e-14 | *** |
| Canicule vs Froid | -24.9% (-3,184 MW) | 2.54e-25 | *** |

*Note : *** signifie p < 0.001 (différence très significative)*

**Interprétation** : 
- En moyenne sur les 32 pays européens analysés, les canicules entraînent une hausse de 16.8% de la consommation électrique (climatisation, refroidissement)
- Le grand froid entraîne paradoxalement une baisse de 12.3% de la consommation moyenne
- La différence entre canicule et froid est de 24.9%, hautement significative
- Ces tendances moyennes masquent d'importantes variations géographiques, comme le montre l'analyse de l'Allemagne ci-dessous

#### Analyse spécifique : Allemagne

L'Allemagne, plus grand consommateur d'électricité du dataset, présente un comportement différent.

**Consommation moyenne par période**
```
Normale (0-25°C) : 55,352 MW
Canicule (>25°C) : 55,719 MW (+0.7%)
Froid (<0°C) :     59,987 MW (+8.4%)
```

**Interprétation** :
- L'Allemagne réagit très différemment de la moyenne européenne
- Impact canicule limité (+0.7%) : climat plus tempéré, moins de climatisation
- Impact froid important (+8.4%) : chauffage électrique massif nécessaire
- Consommation maximale en période de grand froid, contrairement à la tendance moyenne
- Cela illustre l'importance d'analyses géographiques différenciées

### 3. Événements météorologiques extrêmes identifiés

**Jours de canicule (>25°C)** : 2,079 jours sur la période

**TOP 3 des jours les plus chauds** :
1. 02/07/2017 - Chypre : 35.3°C
2. 03/07/2017 - Chypre : 34.5°C
3. 03/08/2015 - Chypre : 34.3°C

**Jours de grand froid (<0°C)** : 7,444 jours

**TOP 3 des jours les plus froids** :
1. 18/01/2016 - Finlande : -23.4°C
2. 20/01/2016 - Finlande : -23.1°C
3. 08/01/2016 - Estonie : -23.1°C

### 4. Statistiques du dataset

**Volume de données**
- Observations totales : 590,352 (après fusion)
- Période : Janvier 2015 - Juin 2020 (5.5 ans)
- Pays analysés : 32 pays européens
- Liste des pays : AT, BE, BG, CH, CY, CZ, DE, DK, EE, ES, FI, FR, GB, HR, HU, IE, IT, LT, LU, LV, ME, NL, NO, PL, PT, RO, RS, SE, SI, SK, UA
- Granularité : Données journalières

**Métriques disponibles**
- Consommation électrique : 62,248 observations
- Production éolienne : 56,224 observations
- Production solaire : 42,168 observations

**Répartition des observations par période climatique**
- Période normale (0-25°C) : 50,717 jours (84.3%)
- Canicule (>25°C) : 2,079 jours (3.5%)
- Grand froid (<0°C) : 7,444 jours (12.4%)

---

## Technologies utilisées

### Big Data & Processing

![PySpark](https://img.shields.io/badge/PySpark-3.5.1-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-3.5.1-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)

### Data Science & Analyse

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)

### Visualisation

![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge)
![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=for-the-badge)

### Infrastructure

![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=google-colab&logoColor=white)
![Google Drive](https://img.shields.io/badge/Google%20Drive-4285F4?style=for-the-badge&logo=google-drive&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

### Version Control

![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

---

## Équipe

**Cours** : Big Data & Cloud Computing (BDCC 2025)  
**Formatrice** : Mously DIAW  
**Institution** : Tech et Cie  
**Groupe** : Groupe 2

**Membres de l'équipe** :
- **Mouhammadou DIA** - Analyste Statisticien
- **Emmanuel DOSSEKOU** - Analyste Statisticien
- **Marc MARE** - Analyste Statisticien
- **Ndeye Salla TOURE** - Analyste Statisticien

---

## Documentation

### Ressources externes

**Sources de données**
- [Open Power System Data (OPSD)](https://data.open-power-system-data.org/)
- [Open-Meteo Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api)

**Documentation technique**
- [Apache Spark Documentation](https://spark.apache.org/docs/latest/)
- [PySpark API Reference](https://spark.apache.org/docs/latest/api/python/)

**Articles scientifiques**
- Pfenninger, S. & Staffell, I. (2016). Long-term patterns of European PV output using 30 years of validated hourly reanalysis and satellite data. *Energy*, 114, 1251-1265.

---

## Méthodes d'analyse

### 1. Prétraitement des données

**Nettoyage**
- Gestion des valeurs manquantes (imputation/suppression)
- Détection et traitement des outliers
- Vérification de la cohérence temporelle

**Transformation**
- Unpivot des colonnes pays en lignes
- Agrégation horaire vers journalière
- Feature engineering (jour de la semaine, saison, etc.)

### 2. Analyses statistiques

**Analyses descriptives**
- Statistiques univariées (moyenne, écart-type, min, max)
- Distributions par pays et par saison
- Identification des valeurs extrêmes

**Analyses bivariées**
- Corrélations de Pearson
- Corrélations saisonnières
- Scatter plots avec tendances

**Tests d'hypothèses**
- Test ANOVA (comparaison de 3+ groupes)
- Tests t de Student (comparaisons par paires)
- Seuil de significativité : alpha = 0.05

### 3. Analyse de scénarios

**Scénarios étudiés**
- Impact des canicules (>25°C) sur la consommation
- Impact du grand froid (<0°C) sur la consommation
- Comparaison avec période normale (0-25°C)
- Analyse par pays (focus Allemagne)

**Méthodologie**
- Segmentation des données par seuils de température
- Calcul des moyennes par segment
- Tests statistiques de différences entre groupes
- Quantification des écarts en pourcentage et en MW

---

## Compétences développées

### Techniques

- Manipulation de datasets massifs avec PySpark
- Traitement distribué avec Apache Spark
- Feature engineering pour analyses
- Tests statistiques (ANOVA, t-test)
- Analyse de scénarios
- Visualisation de données complexes

### Méthodologiques

- Démarche scientifique rigoureuse
- Analyse critique des résultats
- Documentation technique
- Travail collaboratif (Git/GitHub)
- Gestion de projet Big Data

### Transversales

- Communication de résultats techniques
- Interprétation métier
- Formulation de recommandations
- Présentation orale

---

## Livrables du projet

### Livrables obligatoires

- [x] README.md détaillé (ce fichier)
- [x] Code source (notebook)
- [x] Support de présentation (15-25 minutes)
- [x] Documentation technique

## Perspectives et améliorations

### Améliorations techniques

1. **Données**
   - Intégrer des données de capacités installées (solaire/éolien)
   - Ajouter des variables météo complémentaires (nébulosité, humidité)
   - Étendre la période d'analyse (2010-2024)

2. **Analyses**
   - Développer des modèles prédictifs (Machine Learning)
   - Analyser l'évolution temporelle des corrélations
   - Étudier d'autres scénarios (vents forts, précipitations extrêmes)
   - Analyses spécifiques pour chaque pays

3. **Infrastructure**
   - Déployer sur un cluster Spark multi-nœuds
   - Automatiser le pipeline avec Apache Airflow
   - Créer une API REST pour les analyses

### Extensions métier

1. **Impact changement climatique**
   - Analyser l'évolution des extrêmes sur 20-30 ans
   - Projections futures basées sur scénarios climatiques

2. **Optimisation réseau**
   - Algorithmes de prédiction des pics de consommation
   - Stratégies de stockage énergétique
   - Recommandations pour la gestion de charge

3. **Analyse économique**
   - Calcul du coût des extrêmes climatiques
   - Optimisation du mix énergétique
   - ROI des investissements en renouvelable

---

## Problèmes connus et limitations

### Limitations techniques

1. **Données manquantes**
   - Production solaire incomplète pour plusieurs pays
   - Certaines périodes avec données météo manquantes

2. **Granularité temporelle**
   - Agrégation journalière masque les pics horaires
   - Perte d'information sur la variabilité intra-journalière

3. **Couverture géographique**
   - 31 pays européens analysés (couverture large de l'Europe)
   - Analyses pays par pays limitées (seule l'Allemagne analysée en détail)
   - Différences de qualité et de complétude des données selon les pays

### Limitations méthodologiques

1. **Causalité**
   - Corrélations ne signifient pas causalité
   - Variables confondantes non contrôlées (jours fériés, événements)

2. **Scénarios**
   - Pas de prise en compte du stockage énergétique
   - Hypothèses simplificatrices sur les seuils de température
   - Périodes d'analyse limitées (5.5 ans)

3. **Hétérogénéité géographique**
   - Analyses moyennes peuvent masquer des variations importantes
   - Différences de mix énergétique entre pays non prises en compte

---

## Contribution

Les contributions sont les bienvenues. Si vous souhaitez améliorer ce projet :

1. Fork le projet
2. Créer une branche (`git checkout -b feature/amelioration`)
3. Commit vos changements (`git commit -m 'Ajout fonctionnalité X'`)
4. Push vers la branche (`git push origin feature/amelioration`)
5. Ouvrir une Pull Request

---

## Licence

Ce projet est réalisé dans un cadre académique pour le cours Big Data & Cloud Computing 2025.

**Usage académique uniquement** - Non destiné à une utilisation commerciale.

---

## Contact

**Projet** : Analyse Météo-Énergie avec Spark  
**Cours** : BDCC 2025 - Groupe 2  
**Formatrice** : Mously DIAW (mouslydiaw@gmail.com)

**Repository** : [https://github.com/Bassongo/big-data](https://github.com/Bassongo/big-data)

**Date de soutenance** : Samedi 13 décembre 2025

---

## Remerciements

- **Mously DIAW** pour l'encadrement du projet et son expertise
- **Open Power System Data** pour les données énergétiques ouvertes
- **Open-Meteo** pour l'API météorologique gratuite et fiable
- **Apache Spark community** pour l'excellente documentation
- **Tech et Cie** pour l'infrastructure et les ressources mises à disposition

---

## Statistiques du projet

![Lines of Code](https://img.shields.io/badge/Lines%20of%20Code-3000+-blue)
![Notebook Cells](https://img.shields.io/badge/Notebook%20Cells-67-orange)
![Data Processed](https://img.shields.io/badge/Data%20Processed-500MB+-green)
![Execution Time](https://img.shields.io/badge/Execution%20Time-~15min-yellow)

---

**Dernière mise à jour** : 11 décembre 2024  
**Version** : 1.0.0  
**Statut** : Projet finalisé - Prêt pour soutenance

---

<div align="center">

**Si ce projet vous a été utile, n'hésitez pas à lui donner une étoile**

Made with dedication by Groupe 2 - BDCC 2025

</div>
