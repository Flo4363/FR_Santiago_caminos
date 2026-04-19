# Sources de données — Projet FR_Santiago_caminos

Ce document recense l'ensemble des sources intégrées dans le projet,
leur statut, leur emplacement dans le dépôt et les décisions
méthodologiques associées.

---

## Sources actives

### 1. AFCC — Notes de conjoncture 2018-2024

| Attribut | Valeur |
|---|---|
| Producteur | Agence française des chemins de Compostelle (AFCC) |
| Type | Notes de conjoncture nationales (PDF) |
| Couverture | 2018, 2019, 2020-2021, 2022-2023, 2024 + 2 cartes |
| Granularité | Annuelle (mensuelle ponctuelle SJPP) |
| Emplacement dataset | `data/processed/dataset_compostelle_2018_2024.xlsx` |
| Feuille principale | `01_data_long` (107 observations) |
| URL source | https://www.chemins-compostelle.com/observatoire-des-chemins |
| PDFs bruts | `data/raw/afcc/` (non versionnés) |
| Fiabilité | Élevée pour SJPP/SJC, moyenne pour éco-compteurs France |
| Statut | ✅ Intégré |

**Notes** : Les PDFs ne sont pas versionnés (volumétrie, droits d'auteur).
Divergences inter-sources documentées dans l'onglet `06_reconciliation`.
Cas suspect RC_05 (Saugues/Conques 2022 = valeurs identiques) à
investiguer auprès de l'AFCC.

---

### 2. AFCC — Synthèse enquête 2021 sur les publics

| Attribut | Valeur |
|---|---|
| Producteur | AFCC / ACIR, avec partenaires territoriaux |
| Type | Enquête comportementale nationale (PDF 36 pages) |
| Couverture | Données collectées 2019-2021, publication 2021 |
| Périmètre | 3 565 questionnaires, 500 points de diffusion |
| Emplacement dataset | `data/processed/afcc_synthese_comportemental.xlsx` |
| Fichier source | `data/raw/afcc/6_-AFCC-SYNTHESE-ETUDE.pdf` |
| Fiabilité | Moyenne à élevée (biais année jacquaire 2021 documenté) |
| Statut | ✅ Intégré |

**Notes** : Source comportementale complémentaire du dataset flux — ne pas
fusionner avec `01_data_long`. Contient les paramètres critiques pour le
modèle à compartiments : durée moyenne 27.8 jours, distance 25.7 km/jour,
dépense 45.4 €/jour, formule d'impact F×T×D. 10 biais documentés dans
la feuille `00_biais_caveats`. Saisonnalité biaisée (année jacquaire) :
utiliser les séries SJPP mensuelles comme référence de saisonnalité.

---

### 3. Bureau des pèlerins — Saint-Jean-Pied-de-Port (SJPP)

| Attribut | Valeur |
|---|---|
| Producteur | Association des Amis de Saint-Jacques, bureau SJPP |
| Type | Statistiques annuelles et mensuelles (PDFs + PNGs) |
| Couverture | 2018-2024 (7 PDFs + compléments visuels) |
| Granularité | Mensuelle + annuelle par nationalité/voie |
| Emplacement dataset | `data/raw/compostelle_fr/template_SJPP_saisie.xlsx` |
| URL source | https://www.compostelle.fr/hebergements-285/accueils-et-hebergements-de-notre-association/accueil-saint-jacques-a-saint-jean-pied-de-port/ |
| Fiabilité | Élevée (comptage réel accueil bureau) |
| Statut | ✅ Intégré (saisie manuelle) |

**Notes** : Extraction manuelle via template Excel format long tidy data
(303 observations après nettoyage des lignes exemples).
Données 2023 : écart de -3 814 détecté entre total mensuel saisi (53 524)
et référence AFCC (57 338) — mois manquant à investiguer dans les PDFs
sources. Données 2024 : 119 nationalités recensées, total = 58 451
pèlerins certifiés. Série mensuelle complète 2018-2024 (hors lacune 2023).

---

### 4. Lot Tourisme / ACIR — Focus cheminants dans le Lot

| Attribut | Valeur |
|---|---|
| Producteur | Lot Tourisme (ADT 46) / ACIR |
| Type | Enquête comportementale territoriale (PDF 6 pages) |
| Couverture | Données collectées 2019-2021, publication 2021 |
| Périmètre | ~1 390 répondants ayant traversé le Lot (39% de 3 565) |
| Emplacement dataset | `data/raw/lot_tourisme/lot_focus_dataset.xlsx` |
| Fichier source | `data/raw/lot_tourisme/focus_st_jacques-Lot-tourisme.pdf` |
| Fiabilité | Élevée pour indicateurs clés, faible pour lectures de cartes |
| Statut | ✅ Intégré |

**Notes** : Même enquête de base que la source AFCC Synthèse 2021 (3 565
questionnaires communs), focalisée sur le sous-échantillon Lot.
Indicateurs clés : 65% déplorent le manque de places en hébergement
(vs 47% national), budget hébergement +25% vs moyenne nationale
(848 € vs 659 €), 56% primo-cheminants (vs 50% national).
Signal critique pour le modèle à compartiments : le Lot est non atteint
en moins de 15 jours depuis Le Puy (cartes durée × voie, p.3).

---

### 5. Lot Tourisme — Bilan fréquentation GR & Voies Vertes 2024

| Attribut | Valeur |
|---|---|
| Producteur | Lot Tourisme (ADT 46) |
| Type | Bilan annuel éco-compteurs (PDF 7 pages) |
| Couverture | 2022, 2023, 2024 |
| Granularité | Annuelle + mensuelle (graphiques) + mensuelle voie verte |
| Emplacement dataset | `data/raw/lot_tourisme/bilan_rando_lot_2024.xlsx` |
| Fichier source | `data/raw/lot_tourisme/250312-Bilan-rando-Lot-Tourisme-2024.pdf` |
| Fiabilité | Élevée pour GR65 Felzins, altérée pour Limogne et Labastide-Marnhac |
| Statut | ✅ Intégré |

**Notes** : 7 éco-compteurs sur le département du Lot. Données en sens
confondus (IN+OUT) sans distinction pèlerins/randonneurs locaux.
Anomalie séquence GR65 documentée : Felzins (20 270) > Labastide-Marnhac
(17 600) > Limogne (14 400) — incohérent avec un flux linéaire pur.
Explication : GR651 Célé contourne Limogne (données 2024 manquantes),
départs locaux depuis Cahors gonflent Labastide-Marnhac.
Seul Felzins utilisable comme proxy fiable des flux pèlerins entrant dans
le Lot. Contact identifié pour données brutes mensuelles :
Matthieu Besalduch (matthieu.besalduch@tourisme-lot.com).

---

## Sources abandonnées ou en attente

### 6. Oficina del Peregrino — Santiago de Compostelle

| Attribut | Valeur |
|---|---|
| Producteur | Oficina del Peregrino, archidiocèse de Santiago |
| Type | Rapport statistique annuel |
| Couverture | 2018 intégré (71 obs) ; 2019+ non viable |
| Emplacement | `data/external/oficina/2018_raw.json` |
| URL source | https://oficinadelperegrino.com/estadisticas/ |
| Statut | ⚠️ Partiellement intégré — 2019+ abandonné |

**Décision** : Les données 2019+ sont publiées via un dashboard Power BI
embedded non extractible programmatiquement sans accord institutionnel.
Tentative documentée dans `notebooks/NB06_extract_oficina_ARCHIVE.ipynb`.
La source 2018 reste intégrée comme point de référence historique.

---

### 7. Accueil SJPP — Données 2019+ (Oficina)

| Attribut | Valeur |
|---|---|
| Producteur | Oficina del Peregrino (Compostelle) |
| Statut | 🔜 À acquérir (contact institutionnel requis) |

**Note** : Voir `data/processed/dataset_compostelle_2018_2024.xlsx`,
onglet `07_sources_externes` pour le guide d'acquisition complet.

---

### 8. Données éco-compteurs Lot Tourisme (brutes mensuelles)

| Attribut | Valeur |
|---|---|
| Producteur | Lot Tourisme / ADT 46 |
| Statut | 🔜 Demande en cours (mail envoyé à G. Fablet et M. Besalduch) |

**Note** : Données sources des graphiques du Bilan 2024 (p.3).
Actuellement disponibles sous forme de courbes uniquement — fiabilité
FAIBLE pour les valeurs mensuelles extraites visuellement.
La demande porte également sur la méthode de nettoyage des éco-compteurs
(coefficient pèlerins vs randonneurs locaux).

---

## Sources différées (sessions ultérieures)

| Source | Priorité | Raison du différé |
|---|---|---|
| La Malle Postale / Compostel'Bus | MOYENNE | Proxy indirect — opportuniste |
| Associations locales Amis de Saint-Jacques | MOYENNE | Contact à activer |
| SNCF (billets gares de départ) | BASSE | Accord institutionnel requis |
| CRT Occitanie (enquêtes baromètriques) | HAUTE | Pour Phase 4 modélisation |
| Centre d'Études Compostellanes | HAUTE | Corpus qualitatif sociologique |
| Pierre Swalus 2021 (30 000 pèlerins) | HAUTE | Déjà en possession — à positionner |
| Open-Meteo (Somport, SJPP) | MOYENNE | Phase 3 exploration |

---

*Dernière mise à jour : avril 2026 — Phase 1bis enrichissement quantitatif*