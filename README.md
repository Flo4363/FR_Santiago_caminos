# FR_Santiago_caminos
Analyse avancée et correction du biais statistique des flux pèlerins des 4 voies françaises (2018-2024)

Nous entamons une étude rigoureuse sur la fréquentation des chemins de Saint-Jacques-de-Compostelle au départ de la France.

1. Le Contexte & La Problématique :
Le point de données central (Saint-Jean-Pied-de-Port - SJPP) souffre d'un biais majeur : il ne capte pas les pèlerins de la Voie d'Arles (Via Tolosana) qui rejoignent le tronc commun à Puente la Reina via le Somport. L'objectif est de construire un dataset consolidé, nettoyé et enrichi pour obtenir une vision holistique (quantitative et qualitative) du flux réel.

2. Les Sources de Données :

Données Primaires : 10 fichiers PDF (Association Française des Amis du Chemin de Saint-Jacques - AFFF) couvrant la période 2018-2024.

Données Secondaires : Registres espagnols (FEAACS/Jaca) pour le Chemin Aragonais.

Données Exogènes : API Open-Meteo (Historique météo au Col du Somport et à SJPP).

3. La Méthodologie attendue (Approche en 3 temps) :

Étape 1 : Data Engineering & Extraction. Extraction des tableaux des 10 PDF (via pdfplumber ou tabula-py), normalisation des labels (Gestion des types, harmonisation des noms de voies et nationalités).

Étape 2 : Data Cleaning & Preprocessing. Identification et traitement des NaNs. Analyse des Outliers. Mise en place d'une stratégie d'imputation via IterativeImputer de Scikit-Learn pour combler les manques de données sur la Voie d'Arles en utilisant les corrélations temporelles et géographiques.

Étape 3 : Enrichissement & Analyse. * Interrogation de l'API Meteo pour corréler les flux au Somport avec l'enneigement/température.

Calcul de l'Index de Saturation par voie.

Modélisation de l'impact économique réel (incluant la part 'invisible' de 12% identifiée en simulation).

4. Livrables souhaités :

Un script de preprocessing robuste (KISS & DRY).

Un dataset final consolidé (CSV) avec son dictionnaire de métadonnées.

Une série de visualisations (Seaborn/Plotly) montrant le rééquilibrage des flux après correction du biais.