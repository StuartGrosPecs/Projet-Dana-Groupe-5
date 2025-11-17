# Projet Dana – Les footballeurs les plus chers de 2021  

### 1. Description
Dans le cadre du projet Dana, nous avons choisi d’exploiter un dataset portant sur les joueurs de football les plus chers en 2021.
Ce dataset, disponible sur Kaggle, recense plus de 500 joueurs avec leurs caractéristiques personnelles, leur valeur marchande, 
ainsi que diverses statistiques de performance.

Source :
https://www.kaggle.com/datasets/sanjeetsinghnaik/most-expensive-footballers-2021

### 2. Importation du dataset
Nous avons téléchargé le fichier `players.csv` depuis Kaggle, 
puis nous l’avons importé dans OpenRefine afin d'effectuer le nettoyage et la transformation des données.

### 3. Transformation en RDF
Nous avons utilisé l’extension **RDF Transform** d’OpenRefine pour convertir le CSV en RDF.

#### Étapes réalisées :
- Installation de l’extension RDF Transform  
- Création du modèle RDF  
- Définition des préfixes (namespaces)  
- Mappage des colonnes du CSV vers des propriétés RDF  
- Nettoyage et uniformisation des données  
- Export du résultat en format Turtle (`.ttl`)  

#### Fichier obtenu : 
- `players.ttl`

### 4. Visualisation du graphe RDF
Voici le graphe RDF obtenu à partir du fichier Turtle :  
![Projet-Dana-Groupe-5](visuals/ttlGraph.png)

### 5. Résumé du workflow
Nous avons réalisé l’ensemble du workflow de transformation :
CSV ➜ Nettoyage OpenRefine ➜ Modèle RDF ➜ Export Turtle ➜ Visualisation du graphe

---

## Arborescence du projet

```text
.
├── data/
│   ├── players.csv          # Jeu de données brut (CSV)
│   ├── players.ttl          # RDF généré
│   └── void.ttl             # Description Void 
│
├── queries/                 # Requêtes SPARQL
│   ├── querie1.sparql
│   ├── querie2.sparql
│   ├── sharedQuerie1.sparql
│   ├── sharedQuerie2.sparql
│   └── sharedQuerie3.sparql        
│
├── sharedQueriesData/       # Données partagées pour les requêtes avec d'autres datasets
│   ├── athletes_with_owl.ttl
│   ├── currencies.ttl
│   └── world_population_objects_v2.ttl           
│
├── visuals/                 # Graphes générés en PNG  
│   ├── ttlGraph.png
│   └── voidGraph.png 
│
└── README.md          
```

---

## Contenu du dépôt

- **`data/`**
  - Contient le **CSV d’origine** pour le projet.
  - Contient un fichier **Turtle (`.ttl`)** généré à partir du CSV.

- **`queries/`**
  - Contient les **requêtes SPARQL** créées.

- **`sharedQueriesData/`**
  - Contient les données d'autres groupes nécessaires pour les requêtes partagées.

- **`visuals/`**
  - Contient les graphes générés en PNG pour visualiser les entités du RDF et du void.
---

## Installation du projet

Cloner le dépôt en local :

```bash
git clone https://github.com/StuartGrosPecs/Projet-Dana-Groupe-5.git
cd Projet-Dana-Groupe-5
```

---

## Utilisation avec Apache Jena Fuseki

1. **Lancer le serveur Fuseki**

2. **Créer un dataset**
   - Aller sur l’interface de Fuseki.
   - Créer un **nouveau dataset** `players`.
   - Importer le fichier RDF/Turtle depuis le dossier `data/`.

3. **Exécuter les requêtes SPARQL**
   - Ouvrir l’onglet **SPARQL** dans l’interface Fuseki.
   - Copier/coller le contenu d’un fichier de `queries/`.
   - Lancer la requête pour voir les résultats (table, JSON, CSV, etc.).

4. **Utiliser les données partagées**
   - Pour les requêtes qui utilisent des données d'autres groupes, les importer de la même manière dans un dataset séparé à partir de `sharedQueriesData/`.

---

## Auteurs

- **Yanis Dabin** – @StuartGrosPecs  
- **Khalil Mhedhbi** – @KhalilMhedhbi574
- **Ilef Mhabrech** – @Ilef-mhabrech