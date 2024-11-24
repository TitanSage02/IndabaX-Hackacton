<h2><center> Welcome to the Cryptojacking Detection Challenge </h2></center>
<figure>
 <center><img src ="https://drive.google.com/uc?export=view&id=1afxycLZz1AovI0MjqEIiXyziEVWnUkNG" width = "375" height = '250' alt="Cryptojacking Detection Challenge"/> 

*About the problem*
> Cryptojacking is a cyber-attack utilizing malicious scripts similar to those from large cryptocurrency houses to illegally mine data without users being aware. These attacks are stealthy and difficult to detect or analyze, often leading to decreased computing speeds for users as well as crashes due to straining of computational resources.


*The objective of this challenge is*:
> classify network activity from various websites as either cryptojacking or not based on features related to both network-based and host-based data.



## 1. **Présentation et objectifs :**

### **Description de la solution :**
Le code implémente une solution de classification binaire pour prédire un label cible à partir de plusieurs caractéristiques présentes dans le jeu de données fourni dans le cadre de l'IndabaX Challenge. La solution utilise un modèle d'apprentissage supervisé basé sur l'algorithme **XGBoost**, un puissant classificateur basé sur l'optimisation des arbres de décision. Les étapes principales comprennent l'extraction des données, l'ingénierie des caractéristiques, la préparation des données, l'entraînement du modèle, l'évaluation des performances et la génération des soumissions pour le challenge.

### **Objectifs principaux du projet :**
L'objectif principal de ce projet est de prédire une variable cible (nommée `Label`) à partir des différentes caractéristiques extraites du jeu de données. Le code résout les problèmes suivants :
- **Préparation des données** : Chargement des données, nettoyage, gestion des valeurs manquantes et création de nouvelles caractéristiques.
- **Modélisation** : Utilisation de XGBoost pour entraîner un modèle de classification binaire.
- **Évaluation** : Mesure de la performance du modèle à travers des métriques standard comme le F1-score, la précision, et la matrice de confusion.

### **Résultats attendus :**
Les résultats attendus incluent un modèle capable de faire des prédictions sur des données de test et de générer une soumission prête à être utilisée pour un concours de prédiction. L'évaluation du modèle repose sur la métrique **F1-score** .

---

## 2. **Diagramme d'architecture :**
Voici un diagramme pour représenter l'architecture du processus :

```plaintext
+--------------------+
|   Chargement des    |
|       données       |
+--------------------+
           |
           v
+--------------------+
|  Prétraitement des  |
|      données        |
| (Nettoyage, ajout   |
|  de nouvelles      |
|    caractéristiques)|
+--------------------+
           |
           v
+--------------------+
|  Séparation des    |
|    données (Train  |
|     et Test)       |
+--------------------+
           |
           v
+--------------------+
| Entraînement du    |
|    modèle XGBoost  |
+--------------------+
           |
           v
+--------------------+
| Prédiction sur les |
|  données de test   |
+--------------------+
           |
           v
+--------------------+
|  Évaluation du     |
| modèle (F1-score,  |
|  matrice de conf.) |
+--------------------+
           |
           v
+--------------------+
|  Génération de la  |
|   soumission (CSV) |
+--------------------+
```

### Explication du flux :
1. **Chargement des données** : Les données sont extraites.
2. **Prétraitement des données** : Les données sont nettoyées et des caractéristiques supplémentaires sont générées.
3. **Séparation des données** : Le jeu de données est divisé en ensembles d'entraînement et de test.
4. **Entraînement du modèle XGBoost** : Le modèle est entraîné sur les données d'entraînement.
5. **Prédiction sur les données de test** : Le modèle effectue des prédictions sur les données de test.
6. **Évaluation du modèle** : Le modèle est évalué avec des métriques comme le F1-score et la matrice de confusion.
7. **Génération de la soumission** : Les résultats sont formatés en un fichier CSV pour soumission.


## 3. **Processus ETL :**

### **Extraction :**
Les données sont extraites à partir de fichiers CSV téléchargés depuis un dataset Kaggle via `kagglehub.dataset_download()`. Ces fichiers sont ensuite chargés dans des DataFrames Pandas.

### **Transformation :**
Des transformations sont appliquées sur les données pour ajouter de nouvelles caractéristiques basées sur les ratios calculés entre différentes colonnes, comme par exemple :
- `page_error_read_ratio` (le ratio de pages lues par rapport aux erreurs de pages).
- `bytes_ratio` (le ratio des bytes reçus par rapport aux bytes envoyés).

Ces transformations permettent de générer des caractéristiques qui peuvent aider à améliorer la performance du modèle.

### **Chargement :**
Les données transformées sont stockées dans des DataFrames Pandas. Les données de test sont préparées pour être utilisées dans la phase de prédiction, et le modèle prédit les valeurs des données de test avant de générer un fichier CSV pour la soumission.

---

## 4. **Modélisation des données :**

### **Modèle utilisé :**
Le modèle principal utilisé pour la classification est le **XGBoost** (`xgb.XGBClassifier`), un modèle puissant pour des tâches de classification binaire. Les principaux paramètres du modèle sont :
- **max_depth** : 6
- **learning_rate** : 0.05
- **n_estimators** : 1500
- **scale_pos_weight** : pondération pour les classes déséquilibrées.
- **min_child_weight** : 0.6

Le modèle est entraîné sur les données d'entraînement après une séparation des données en ensembles d'entraînement et de validation (80% et 20%).

### **Validation :**
L'évaluation est réalisée via un **classification_report**, **F1-score** et la **matrice de confusion**. Cela permet de mesurer la précision et d'identifier les erreurs de classification entre les classes 0 et 1.

---

## 5. **Inférence :**

### **Prédiction :**
Une fois que le modèle est entraîné, il est utilisé pour prédire les étiquettes (`Label`) du jeu de données de test. Les données de test sont transformées pour correspondre aux mêmes caractéristiques que celles utilisées pour entraîner le modèle.

### **Services impliqués :**
L'inférence est réalisée par le modèle XGBoost, et les résultats sont utilisés pour générer un fichier CSV de soumission, qui peut ensuite être téléchargé pour une évaluation dans un environnement de compétition.

---

## 6. **Durée d'exécution :**

L'exécution complète de tout le code ne dépasse pas 3 minutes.

---

## 7. **Indicateurs de performance :**

Les métriques utilisées pour évaluer la performance du modèle comprennent :
- **F1-score** : Mesure de la précision et du rappel, adaptée aux jeux de données déséquilibrés.
- **Matrice de confusion** : Pour visualiser les vraies et fausses classifications.

Exemple de sortie pour l'évaluation :
```
F1_score:  0.964509394572025

Confusion Matrix: 
 [[1286   15]
 [  19  462]]
```

---
