# FR_Santiago_caminos

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow)
![Status](https://img.shields.io/badge/Status-Active%20Development-green)
![Last Update](https://img.shields.io/badge/Last%20Update-April%202026-blue)

**Observatoire des flux pèlerins sur les chemins de Compostelle en France (2018-2024)**

Un projet personnel de data science visant à construire un observatoire territorial multi-sources avec dimension prédictive, à partir des données publiques de l'Agence française des chemins de Compostelle (AFCC) et de sources complémentaires (Bureau des pèlerins de Saint-Jean-Pied-de-Port, Lot Tourisme, Oficina del Peregrino, Open-Meteo).

---

## Objectif du projet

Caractériser, modéliser et prédire la fréquentation des chemins de Compostelle sur le territoire français entre 2018 et 2024, à partir de sources hétérogènes, en tenant compte de la non-linéarité des comportements pèlerins (pèlerinage par tronçons étalé sur plusieurs années) et des ruptures contextuelles (COVID-19, Année Jacquaire 2021).

Ce projet a été **reformulé en cours de route** : initialement conçu comme une correction de biais statistique entre SJPP et l'Oficina de Santiago, il a été repositionné en **observatoire territorial + prédictif** après identification que les deux sources mesurent des phénomènes non comparables temporellement (passages annuels vs arrivées cumulées à Santiago).

Voir [`docs/charter_projet.docx`](docs/charter_projet.docx) pour le cadrage méthodologique complet.  
Voir [`docs/SOURCES.md`](docs/SOURCES.md) pour le détail de toutes les sources intégrées.

## Sous-questions opérationnelles

- Quelle est la dynamique spatio-temporelle des flux sur chaque voie principale (GR65, GR653, GR654, GR655, GR78) ?
- Quels facteurs explicatifs (météo, saisonnalité, contexte macro) influencent la fréquentation par tronçon ?
- Peut-on construire un modèle à compartiments modélisant la non-linéarité du pèlerinage (sectionnistes, flux stockés multi-saisons) ?
- Peut-on construire un modèle prédictif fiable de la fréquentation 2025-2026 à partir des données historiques ?
- Quelles sont les retombées économiques estimées pour les territoires traversés (formule F×T×D) ?

---

## Structure du dépôt

```
FR_Santiago_caminos/
├── data/
│   ├── raw/                          # Sources primaires (non versionnées)
│   │   ├── afcc/                     # Notes de conjoncture AFCC 2018-2024 (PDFs)
│   │   │   └── 6_-AFCC-SYNTHESE-ETUDE.pdf
│   │   ├── compostelle_fr/           # Bureau des pèlerins SJPP (PDFs + PNGs)
│   │   │   └── template_SJPP_saisie.xlsx
│   │   ├── lot_tourisme/             # Sources Lot Tourisme (PDFs)
│   │   │   ├── lot_focus_dataset.xlsx
│   │   │   └── bilan_rando_lot_2024.xlsx
│   │   └── qualitative/              # Études qualitatives (à venir)
│   │       └── etudes/
│   ├── processed/                    # Datasets consolidés
│   │   ├── afcc_dataset_compostelle_2018_2024.xlsx
│   │   └── afcc_synthese_comportemental.xlsx
│   └── external/                     # Données tierces (non versionnées)
│       └── oficina/                  # Extraction Oficina del Peregrino
├── docs/
│   ├── charter_projet.docx           # Cadrage méthodologique complet
│   └── SOURCES.md                    # Inventaire et statut de toutes les sources
├── notebooks/
│   └── NB06_extract_oficina_ARCHIVE.ipynb
├── scripts/                          # Scripts Python exécutables (à venir)
├── .gitignore
├── LICENSE
└── README.md
└── requirements.txt
```

### Conventions

- **Fichiers versionnés** : code source, documentation, datasets consolidés (< 10 MB).
- **Fichiers non versionnés** : PDFs sources (droits d'auteur), données brutes volumineuses, environnement virtuel Python.
- **Notebooks** : convention `NB0X_description.ipynb`. Le suffixe `_ARCHIVE` indique un notebook conservé pour traçabilité uniquement, hors pipeline actif.

---

## État d'avancement

| Phase | Étape | Statut |
|-------|-------|:---:|
| **1. Data Engineering** | Extraction et structuration des 7 PDFs AFCC | ✅ |
| | Construction du dataset consolidé (107 observations) | ✅ |
| | Parsing du PDF Oficina 2018 (71 observations) | ✅ |
| **1bis. Enrichissement quantitatif** | Dataset SJPP 2018-2024 (saisie manuelle, 303 obs) | ✅ |
| | Dataset comportemental Lot Tourisme Focus (141 obs) | ✅ |
| | Dataset éco-compteurs Lot Tourisme Bilan 2024 | ✅ |
| | Dataset comportemental AFCC Synthèse 2021 | ✅ |
| | Documentation SOURCES.md | ✅ |
| | Acquisition données brutes mensuelles Lot Tourisme | ⏳ |
| **2. Data Cleaning** | Harmonisation nomenclatures voies (triple clé) | ✅ |
| | Documentation divergences inter-sources | ✅ |
| | Encodage dummies COVID / Année Jacquaire | ⏳ |
| | Acquisition Open-Meteo (Somport, SJPP) | ⏳ |
| **3. Exploration** | Séries temporelles par point de mesure | 🔜 |
| | Cartographie interactive des flux | 🔜 |
| | Clustering des points de mesure | 🔜 |
| **4. Modélisation** | Modèle à compartiments (flux stockés multi-saisons) | 🔜 |
| | Baselines naïfs + régression | 🔜 |
| | SARIMA, Prophet | 🔜 |
| | LSTM multi-variate | 🔜 |
| **5. Restitution** | Rapport final méthodologique | 🔜 |
| | Dashboard interactif (Streamlit) | 🔜 |

**Légende** : ✅ fait · ⏳ en cours · 🔜 à venir

---

## Sources de données

Le détail complet (statut, emplacements, décisions méthodologiques, biais documentés) est disponible dans [`docs/SOURCES.md`](docs/SOURCES.md).

### Sources intégrées

| Source | Type | Couverture | Dataset |
|--------|------|-----------|---------|
| AFCC — Notes de conjoncture | Comptages flux (PDF) | 2018-2024 | `data/processed/dataset_compostelle_2018_2024.xlsx` |
| AFCC — Synthèse enquête 2021 | Comportemental (PDF 36p) | 2019-2021 | `data/processed/afcc_synthese_comportemental.xlsx` |
| Bureau des pèlerins SJPP | Accueil pèlerins (PDFs + PNGs) | 2018-2024 | `data/raw/compostelle_fr/template_SJPP_saisie_v2.xlsx` |
| Lot Tourisme — Focus cheminants | Comportemental territorial (PDF) | 2019-2021 | `data/raw/lot_tourisme/lot_focus_v2.xlsx` |
| Lot Tourisme — Bilan GR 2024 | Éco-compteurs (PDF) | 2022-2024 | `data/raw/lot_tourisme/bilan_rando_lot_2024_final.xlsx` |
| Oficina del Peregrino | Arrivées Santiago (API) | 2018 uniquement | `data/external/oficina/2018_raw.json` |

### Sources en attente ou différées

Voir [`docs/SOURCES.md`](docs/SOURCES.md) — section *Sources en attente ou différées*.

---

## Installation et usage

### Prérequis

- Python 3.11 ou supérieur
- Git

### Installation

```bash
git clone https://github.com/Flo4363/FR_Santiago_caminos.git
cd FR_Santiago_caminos

python -m venv .venv
source .venv/bin/activate        # Linux / macOS
.\.venv\Scripts\Activate.ps1     # Windows PowerShell

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### Téléchargement des données sources

Les PDFs sources ne sont pas inclus dans le dépôt. Voir [`docs/SOURCES.md`](docs/SOURCES.md) pour les instructions par source.

### Datasets disponibles

| Fichier | Onglets | Contenu |
|---------|---------|---------|
| `data/processed/dataset_compostelle_2018_2024.xlsx` | 8 | 107 observations flux physiques, métadonnées, nomenclatures, réconciliation |
| `data/processed/afcc_synthese_comportemental.xlsx` | 8 | Paramètres comportementaux, typologies, satisfaction, formule F×T×D |
| `data/raw/compostelle_fr/template_SJPP_saisie_v2.xlsx` | 5 | 303 observations SJPP 2018-2024, mensuel + nationalités + voies |
| `data/raw/lot_tourisme/lot_focus_v2.xlsx` | 5 | 141 observations comportementales Lot, cartes durée×voie, modèle compartiments |
| `data/raw/lot_tourisme/bilan_rando_lot_2024_final.xlsx` | 4 | Éco-compteurs GR65/GR6/GR46 Lot 2022-2024, caveats méthodologiques |

---

## Méthodologie

Principes directeurs appliqués tout au long du projet :

- **KISS** — simplicité et lisibilité prioritaires sur la sophistication prématurée
- **DRY** — factorisation des étapes récurrentes en fonctions réutilisables
- **Traçabilité** — chaque observation est rattachée à sa source primaire (fichier, page, date)
- **Reproductibilité** — chaque étape exécutable indépendamment via notebook ou script
- **Auditabilité** — divergences, biais et approximations documentés explicitement

Stack technique : **pandas**, **numpy** pour la manipulation ; **matplotlib**, **seaborn**, **plotly** pour la visualisation ; **folium** pour la cartographie ; **statsmodels**, **prophet** pour les séries temporelles ; **TensorFlow/Keras** pour le deep learning.

---

## Auteur

**Florian** — Data Analyst & Data Scientist certifié MINES Paris PSL  
🔗 [LinkedIn](https://www.linkedin.com/in/florian-berger-98949436/)

Projet personnel mené en parallèle d'une recherche active de mission en tant que Product Owner / Business Analyst / Data Steward.

---

## Licence

Ce projet est distribué sous licence MIT — voir [LICENSE](LICENSE) pour le détail.

Les données sources AFCC demeurent la propriété de l'Agence française des chemins de Compostelle et sont utilisées à des fins de recherche non commerciale. Consulter les conditions d'utilisation sur [chemins-compostelle.com](https://www.chemins-compostelle.com/).

---

## 🇬🇧 English summary

**Pilgrim flow observatory on the Way of Saint James in France (2018-2024)**

Personal data science project building a multi-source territorial observatory with predictive modeling capabilities, based on public data from the French Agency of the Ways of Compostela (AFCC) and complementary sources.

**Research question** : How to characterize, model and predict pilgrim flows on the French territory of the Way of Saint James between 2018 and 2024, accounting for the non-linearity of pilgrim behaviors (section-by-section pilgrimage spread over multiple years) and contextual disruptions (COVID-19, Holy Year 2021)?

**Current status** : Phase 1 and Phase 1bis (data engineering + quantitative enrichment) complete. Five datasets available covering flow counts, behavioral surveys, and territorial eco-counter data. Exploration, modeling and reporting phases upcoming.

**Tech stack** : Python 3.11+, pandas, matplotlib, plotly, folium, statsmodels, prophet, TensorFlow/Keras.

See [`docs/SOURCES.md`](docs/SOURCES.md) for the complete source inventory.