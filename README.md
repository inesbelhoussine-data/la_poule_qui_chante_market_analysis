# 🌍 International Poultry Market Analysis

## 📌 Contexte

**La Poule qui Chante** est une entreprise française du secteur agroalimentaire spécialisée dans la volaille. Jusqu'à présent principalement présente sur le marché français, l'entreprise souhaite étudier des opportunités de développement à l'international.

L'objectif de ce projet est d'exploiter des données économiques, démographiques et sectorielles afin d'identifier des marchés présentant des caractéristiques intéressantes pour une future stratégie d'exportation.

L'analyse est réalisée sans présélection de pays ou de continent afin de laisser les données faire émerger les différents profils de marchés.

---

## 🎯 Problématique

> **Quels pays présentent des caractéristiques favorables à une stratégie d'exportation de volaille ?**

L'objectif n'est pas simplement d'identifier les pays important les plus gros volumes de volaille.

Un grand pays peut présenter des importations importantes en raison de sa population, tandis qu'un marché plus petit peut être particulièrement dépendant des importations.

L'analyse adopte donc une approche multidimensionnelle combinant :

- la taille et la dynamique démographique ;
- le niveau économique ;
- la stabilité politique ;
- la production locale de volaille ;
- les importations ;
- la disponibilité de volaille ;
- la dépendance aux importations ;
- l'intensité des importations par habitant.

---

## 📊 Données

Les données utilisées proviennent principalement de sources internationales telles que la **FAO** et la **Banque mondiale**.

Après préparation, nettoyage et fusion des différentes sources, le jeu de données final comprend :

- **161 pays**
- **9 variables quantitatives utilisées pour l'analyse multivariée**
- aucune valeur manquante sur les variables finales

### Variables retenues

| Variable | Intérêt dans l'analyse |
|---|---|
| Population 2017 | Taille potentielle du marché |
| Croissance de la population 2012–2017 | Dynamique démographique |
| Disponibilité de volaille par habitant | Importance de la volaille dans le marché local |
| Importations de volaille | Volume d'approvisionnement extérieur |
| Production de volaille | Niveau de production domestique |
| Dépendance aux importations | Intensité du recours aux importations |
| PIB par habitant en PPA | Indicateur du niveau économique |
| Stabilité politique | Indicateur du risque politique et institutionnel |
| Importations de volaille par habitant | Intensité des importations corrigée de la taille du pays |

---

## 🛠️ Feature Engineering

Trois nouvelles variables ont été créées afin d'enrichir l'analyse.

### Croissance démographique

```text
(Population 2017 - Population 2012) / Population 2012 × 100
```

Cette variable permet de distinguer les marchés démographiquement dynamiques des marchés stagnants ou en décroissance.

### Dépendance aux importations

```text
Importations de volaille / Disponibilité intérieure de volaille × 100
```

Cet indicateur permet d'évaluer l'importance des importations dans l'approvisionnement du marché.

### Importations de volaille par habitant

Cette variable permet de comparer l'intensité des importations entre des pays de tailles démographiques très différentes.

---

# 🔎 Méthodologie

L'analyse suit plusieurs étapes :

```text
Collecte des données
        ↓
Nettoyage et préparation
        ↓
Feature Engineering
        ↓
Analyse exploratoire
        ↓
Standardisation
        ↓
ACP
        ↓
CAH + K-Means
        ↓
Interprétation des clusters
        ↓
161 pays → 21 marchés candidats
        ↓
Application de critères métier
        ↓
7 marchés à approfondir
```

---

## 1. Préparation et exploration des données

Les différentes sources ont été nettoyées et harmonisées avant leur fusion.

Plusieurs contrôles ont été réalisés :

- valeurs manquantes ;
- doublons ;
- cohérence des pays ;
- distributions des variables ;
- valeurs atypiques ;
- corrélations entre les variables.

L'analyse des corrélations a notamment montré une très forte corrélation entre la **production de volaille** et la **disponibilité intérieure en volume**.

Afin d'éviter une redondance excessive dans l'analyse multivariée, la disponibilité intérieure en volume n'a pas été conservée parmi les variables finales de l'ACP.

---

## 2. Standardisation

Les variables utilisées présentent des unités et des ordres de grandeur très différents : population, PIB, tonnes de volaille, pourcentages, score de stabilité politique, etc.

Une standardisation avec `StandardScaler` a donc été appliquée avant l'analyse multivariée.

Elle permet de rendre les variables comparables et d'éviter qu'une variable influence l'analyse uniquement en raison de son échelle numérique.

---

## 3. Analyse en Composantes Principales — ACP

Une **Analyse en Composantes Principales (ACP)** a été réalisée afin de synthétiser les informations contenues dans les 9 variables et d'identifier les principales dimensions différenciant les pays.

Les trois premières composantes représentent environ :

> **67 % de la variance totale**

Le choix des composantes repose notamment sur :

- l'éboulis des valeurs propres ;
- le critère de Kaiser ;
- la variance expliquée cumulée ;
- l'interprétabilité des axes.

Des cercles de corrélation ainsi qu'une projection des pays ont ensuite permis d'interpréter les principales dimensions de l'analyse.

---

## 4. Classification Ascendante Hiérarchique — CAH

Une **Classification Ascendante Hiérarchique** utilisant la méthode de **Ward** a été appliquée afin d'identifier des groupes de pays présentant des profils similaires.

Le choix du nombre de groupes s'est appuyé sur :

- le dendrogramme ;
- le coefficient de silhouette ;
- l'interprétabilité des profils obtenus.

La segmentation retenue comporte :

> **5 clusters**

Les profils moyens standardisés de chaque cluster ont ensuite été analysés afin d'identifier leurs principales caractéristiques.

---

## 5. K-Means

Une seconde méthode de clustering, **K-Means**, a été utilisée afin de comparer la structure obtenue avec la CAH.

Plusieurs valeurs de `K` ont été testées.

Le choix final s'est appuyé sur :

- l'évolution de l'inertie ;
- la méthode du coude ;
- le coefficient de silhouette ;
- l'interprétation des clusters.

La segmentation retenue comporte :

> **7 clusters**

Les résultats de la CAH et de K-Means ne sont pas identiques mais font apparaître plusieurs structures communes.

L'**Adjusted Rand Index (ARI)** obtenu est d'environ **0,56**, indiquant une concordance notable mais imparfaite entre les deux segmentations.

---

# 🎯 Identification des marchés candidats

L'objectif du clustering n'était pas de désigner automatiquement les « meilleurs » pays, mais de faire émerger différents profils de marchés.

L'interprétation des clusters issus de la CAH et de K-Means a permis d'identifier plusieurs profils présentant un intérêt commercial, notamment :

- des marchés combinant niveau économique élevé, stabilité et recours important aux importations ;
- des marchés présentant une forte dépendance aux importations ;
- des pays caractérisés par des volumes d'importations particulièrement importants.

Cette étape permet de réduire l'analyse de :

> **161 pays → 21 marchés candidats**

---

# 📈 Sélection finale

Afin de transformer les résultats statistiques en une présélection exploitable d'un point de vue métier, les 21 marchés candidats ont été comparés selon quatre critères :

1. **Volume d'importations de volaille**
2. **Dépendance aux importations**
3. **PIB par habitant en PPA**
4. **Stabilité politique**

La médiane a été utilisée comme seuil de comparaison afin de limiter l'influence des valeurs extrêmes.

Les marchés présentant un profil favorable sur **au moins 3 des 4 critères** ont été conservés.

Cette démarche aboutit à une shortlist de **7 marchés à approfondir** :

| Marché | Positionnement |
|---|---|
| 🇳🇱 Pays-Bas | Profil favorable sur les 4 critères |
| 🇦🇪 Émirats arabes unis | Profil favorable sur les 4 critères |
| 🇯🇵 Japon | Marché important et volumes d'importations élevés |
| 🇭🇰 Hong Kong | Forte intensité des importations |
| 🇧🇪 Belgique | Marché fortement connecté aux importations |
| 🇲🇴 Macao | Marché à forte dépendance et niveau économique élevé |
| 🇱🇺 Luxembourg | Marché à niveau économique élevé et dépendant des importations |

Ces résultats constituent une **présélection analytique** et non un classement définitif des marchés.

---

# 💡 Principaux enseignements

L'analyse montre l'intérêt de combiner plusieurs dimensions plutôt que de sélectionner les marchés uniquement selon leurs volumes d'importation.

Les **Pays-Bas** et les **Émirats arabes unis** répondent favorablement aux quatre critères de présélection.

Le **Japon** présente un profil différent : sa dépendance aux importations est moins importante, mais la taille du marché et ses volumes d'importation en font un marché pertinent à étudier davantage.

Les autres marchés sélectionnés présentent également des caractéristiques intéressantes, notamment en matière de dépendance aux importations ou de niveau économique.

---

# ⚠️ Limites de l'analyse

Cette étude constitue une première étape de sélection de marchés.

Plusieurs éléments devraient être approfondis avant toute décision d'implantation ou d'exportation :

- actualisation des données ;
- réglementation sanitaire ;
- droits de douane ;
- contraintes d'importation ;
- coûts logistiques ;
- chaîne du froid ;
- concurrence locale et internationale ;
- prix pratiqués ;
- habitudes de consommation ;
- demande spécifique pour la volaille biologique.

Les résultats permettent donc avant tout d'identifier **où approfondir l'étude**, et non de remplacer une analyse complète de faisabilité commerciale.

---

# 🧰 Technologies utilisées

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **Scikit-learn**
- **SciPy**
- **Jupyter Notebook**

### Méthodes statistiques et Machine Learning

- Analyse exploratoire des données
- Feature Engineering
- Standardisation
- Analyse de corrélation
- Analyse en Composantes Principales
- Classification Ascendante Hiérarchique
- Méthode de Ward
- K-Means
- Méthode du coude
- Coefficient de silhouette
- Adjusted Rand Index

---

### `01_nettoyage_donnees.ipynb`

Préparation des données :

- import des différentes sources ;
- nettoyage ;
- harmonisation ;
- fusion ;
- analyse exploratoire ;
- feature engineering ;
- sélection des variables ;
- préparation du dataset analytique.

### `02_acp_clustering.ipynb`

Analyse multivariée :

- standardisation ;
- ACP ;
- variance expliquée ;
- cercles de corrélation ;
- projection des pays ;
- CAH ;
- K-Means ;
- comparaison des segmentations ;
- interprétation des clusters ;
- sélection des marchés candidats ;
- shortlist finale.

---

## 🚀 Résultat

```text
161 pays
   ↓
9 variables
   ↓
ACP
   ↓
CAH (5 clusters) + K-Means (7 clusters)
   ↓
21 marchés candidats
   ↓
4 critères métier
   ↓
7 marchés à approfondir
```

**Pays-Bas • Émirats arabes unis • Japon • Hong Kong • Belgique • Macao • Luxembourg**

---

## 👩‍💻 À propos

Projet réalisé dans le cadre de ma formation **Data Analyst**, avec pour objectif de mettre en pratique la préparation de données, l'analyse multivariée, la réduction dimensionnelle, le clustering et la traduction de résultats statistiques en recommandations métier.
