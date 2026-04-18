# FR_Santiago_caminos

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow)
![Status](https://img.shields.io/badge/Status-Active%20Development-green)
![Last Update](https://img.shields.io/badge/Last%20Update-April%202026-blue)

**Observatoire des flux pèlerins sur les chemins de Compostelle en France (2018-2024)**

Un projet personnel de data science visant à construire un observatoire territorial multi-sources avec dimension prédictive, à partir des données publiques de l'Agence française des chemins de Compostelle (AFCC) et de sources complémentaires (Accueil Saint-Jacques Saint Jean Pied de Port, Oficina del Peregrino, Open-Meteo).

---

## 🎯 Objectif du projet

Caractériser, modéliser et prédire la fréquentation des chemins de Compostelle sur le territoire français entre 2018 et 2024, à partir de sources hétérogènes, en tenant compte de la non-linéarité des comportements pèlerins (pèlerinage par tronçons étalé sur plusieurs années) et des ruptures contextuelles (COVID-19, Année Jacquaire 2021).

Ce projet a été **reformulé en cours de route** : initialement conçu comme une correction de biais statistique entre SJPP et l'Oficina de Santiago, il a été repositionné en observatoire territorial après identification que les deux sources mesurent des phénomènes non comparables temporellement (passages annuels vs arrivées cumulées à Santiago).

Voir [`docs/charter_projet.docx`](docs/charter_projet.docx) pour le cadrage méthodologique complet.

## 🧭 Sous-questions opérationnelles

- Quelle est la dynamique spatio-temporelle des flux sur chaque voie principale (GR65, GR653, GR654, GR655, GR78) ?
- Quels facteurs explicatifs (météo, saisonnalité, contexte macro) influencent la fréquentation par tronçon ?
- Peut-on construire un modèle prédictif fiable de la fréquentation 2025-2026 à partir des données historiques ?
- Quelles sont les retombées économiques estimées pour les territoires traversés, et comment varient-elles selon les voies ?

---

## 🗂️ Structure du dépôt

```
FR_Santiago_caminos/
├── data/
│   ├── raw/                    # Sources primaires (PDFs AFCC, non versionnées)
│   │   └── afcc/               # Notes de conjoncture AFCC 2018-2024
│   ├── processed/              # Datasets consolidés
│   │   └── dataset_compostelle_2018_2024.xlsx
│   └── external/               # Données tierces (non versionnées)
│       └── oficina/            # Extraction Oficina del Peregrino
├── docs/
│   └── charter_projet.docx     # Cadrage méthodologique complet
├── notebooks/
│   └── NB06_extract_oficina.ipynb  # Notebook d'extraction API Oficina
├── scripts/                    # Scripts Python exécutables (à venir)
├── .gitignore
├── LICENSE
└── README.md
```

### Conventions

- **Fichiers versionnés** : code source, documentation, datasets consolidés légers (< 10 MB).
- **Fichiers non versionnés** : PDFs AFCC (téléchargeables directement depuis les sources officielles), données brutes volumineuses, environnement virtuel Python.
- **Notebooks** : convention de nommage `NB0X_description.ipynb` par continuité avec les autres projets data science de l'auteur.

---

## 📊 État d'avancement

| Phase | Étape | Statut |
|-------|-------|:---:|
| **1. Data Engineering** | Extraction et structuration des 7 PDFs AFCC | ✅ |
| | Construction du dataset consolidé (107 observations) | ✅ |
| | Parsing du PDF Oficina 2018 (71 observations) | ✅ |
| | Acquisition Open-Meteo (Somport, SJPP) | ⏳ |
| **2. Data Cleaning** | Harmonisation nomenclatures voies (triple clé) | ✅ |
| | Documentation divergences inter-sources | ✅ |
| | Encodage dummies COVID / Année Jacquaire | ⏳ |
| **3. Exploration** | Séries temporelles par point de mesure | 🔜 |
| | Cartographie interactive des flux | 🔜 |
| | Clustering des points de mesure | 🔜 |
| **4. Modélisation** | Baselines naïfs + régression | 🔜 |
| | SARIMA, Prophet | 🔜 |
| | LSTM multi-variate | 🔜 |
| **5. Restitution** | Rapport final méthodologique | 🔜 |
| | Dashboard interactif (Streamlit) | 🔜 |

**Légende** : ✅ fait · ⏳ en cours · 🔜 à venir

---

## 📚 Sources de données

### AFCC — Agence française des chemins de Compostelle

7 notes de conjoncture 2018-2024 publiées sur [chemins-compostelle.com](https://www.chemins-compostelle.com/). Les PDFs ne sont pas versionnés (volumétrie, droits d'auteur). Pour reproduire l'analyse :

1. Télécharger les notes de conjoncture depuis [la page observatoire de l'AFCC](https://www.chemins-compostelle.com/observatoire-des-chemins)
2. Placer les PDFs dans `data/raw/afcc/`

### Oficina del Peregrino — Bureau d'accueil Santiago

Rapport annuel PDF 2018 disponible sur [oficinadelperegrino.com/estadisticas/](https://oficinadelperegrino.com/estadisticas/). Les données récentes (2019+) sont publiées via un dashboard Power BI embedded, non extractibles programmatiquement sans accord institutionnel.

### Open-Meteo — Historique météo (à venir)

Coordonnées cibles :
- Col du Somport : lat 42.7898, lng -0.5519 (1632 m)
- Saint-Jean-Pied-de-Port : lat 43.1639, lng -1.2386 (180 m)

API publique documentée : [open-meteo.com/en/docs/historical-weather-api](https://open-meteo.com/en/docs/historical-weather-api)

---

## 🚀 Installation et usage

### Prérequis

- Python 3.11 ou supérieur
- Git

### Installation

```bash
# Cloner le dépôt
git clone https://github.com/Flo4363/FR_Santiago_caminos.git
cd FR_Santiago_caminos

# Créer un environnement virtuel
python -m venv .venv

# Activer (Windows PowerShell)
.\.venv\Scripts\Activate.ps1

# Activer (Linux / macOS)
source .venv/bin/activate

# Installer les dépendances
python -m pip install --upgrade pip
python -m pip install -r requirements.txt    # À créer
```

### Téléchargement des données sources

Les PDFs AFCC ne sont pas inclus dans le dépôt. Voir la section [Sources de données](#-sources-de-données) pour les instructions de téléchargement.

### Exploration du dataset consolidé

Le fichier `data/processed/dataset_compostelle_2018_2024.xlsx` contient 8 onglets documentés :
- `00_README` : méthodologie et conventions
- `01_data_long` : 107 observations en format long
- `02_metadonnees_sources` : description des 7 PDFs sources
- `03_points_mesure` : catalogue des 24 points de mesure
- `04_voies_nomenclature` : triple nomenclature (AFCC / latin / GR)
- `05_inventaire_trous` : matrice de complétude voie × année
- `06_reconciliation` : divergences inter-sources documentées
- `07_sources_externes` : guide d'acquisition des sources complémentaires

---

## 🧭 Méthodologie

Principes directeurs appliqués tout au long du projet :

- **KISS** — simplicité et lisibilité prioritaires sur la sophistication prématurée
- **DRY** — factorisation des étapes récurrentes en fonctions réutilisables
- **Traçabilité** — chaque observation est rattachée à sa source primaire (fichier, page, date)
- **Reproductibilité** — chaque étape exécutable indépendamment via notebook ou script
- **Auditabilité** — divergences et approximations documentées explicitement

Stack technique : **pandas**, **numpy** pour la manipulation ; **matplotlib**, **seaborn**, **plotly** pour la visualisation ; **folium** pour la cartographie ; **statsmodels**, **prophet** pour les séries temporelles ; **TensorFlow/Keras** pour le deep learning.

---

## 📦 Livrables actuels

| Livrable | Emplacement | Description |
|----------|-------------|-------------|
| Charter de projet | `docs/charter_projet.docx` | Document de cadrage méthodologique (10-12 pages) |
| Dataset consolidé AFCC | `data/processed/dataset_compostelle_2018_2024.xlsx` | 107 observations, 8 onglets, dictionnaire de métadonnées inclus |
| Extraction Oficina 2018 | `data/external/oficina/2018_raw.json` | Réponse brute API Oficina pour 2018 |
| Notebook d'extraction API | `notebooks/NB06_extract_oficina.ipynb` | Processus d'extraction documenté (fonctionnel pour 2018) |
| Rapport d'extraction | `data/external/oficina/rapport.txt` | Contrôles qualité automatiques |

---

## 👤 Auteur

**Florian** — Data Analyst & Data Scientist certifié MINES Paris PSL  
🔗 [LinkedIn](https://www.linkedin.com/in/florian-berger-98949436/)

Projet personnel mené en parallèle d'une recherche active de mission en tant que Product Owner / Business Analyst / Data Steward.

---

## 📄 Licence

Ce projet est distribué sous licence MIT — voir [LICENSE](LICENSE) pour le détail.

Les données sources AFCC demeurent la propriété de l'Agence française des chemins de Compostelle et sont utilisées à des fins de recherche non commerciale. Consulter les conditions d'utilisation sur [chemins-compostelle.com](https://www.chemins-compostelle.com/).

---

## 🇬🇧 English summary

**Pilgrim flow observatory on the Way of Saint James in France (2018-2024)**

Personal data science project aimed at building a multi-source territorial observatory with predictive modeling capabilities, based on public data from the French Agency of the Ways of Compostela (AFCC) and complementary sources (Oficina del Peregrino in Santiago, Open-Meteo historical weather API).

**Research question** : How to characterize, model and predict the pilgrim flow on the French territory of the Way of Saint James between 2018 and 2024, accounting for the non-linearity of pilgrim behaviors (section-by-section pilgrimage spread over multiple years) and contextual disruptions (COVID-19, Holy Year 2021) ?

**Methodology** : multi-source data integration, biased data documentation, exploratory spatio-temporal analysis, and predictive modeling (SARIMA, Prophet, LSTM). Principles : KISS, DRY, full traceability, reproducibility, auditability.

**Current status** : Phase 1 (data engineering) and Phase 2 (cleaning) largely complete. Consolidated dataset available at `data/processed/dataset_compostelle_2018_2024.xlsx`. Project charter available at `docs/charter_projet.docx`. Exploration, modeling and reporting phases upcoming.

**Tech stack** : Python 3.11+, pandas, matplotlib, plotly, folium, statsmodels, prophet, TensorFlow/Keras.

See full documentation in `docs/charter_projet.docx` (French).
