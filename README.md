# Projet Dana – Graphe RDF sur les footballeurs

Ce dépôt contient les **données** et les **requêtes SPARQL** utilisées dans le cadre du projet Dana.  
L’objectif est de transformer un jeu de données tabulaire (CSV) sur des footballeurs en **graphe RDF**, puis de l’interroger via **Apache Jena Fuseki**.

---

## Arborescence du projet

```text
.
├── data/
│   ├── players.csv          # Jeu de données brut (CSV)
│   └── players.ttl          # RDF généré
│
├── queries/           # Requêtes SPARQL
│   ├── querie1.sparql
│   ├── querie2.sparql
│   ├── sharedQuerie1.sparql
│   ├── sharedQuerie2.sparql
│   └── sharedQuerie3.sparql        
│
├── sharedQueriesData/ # Données partagées pour les requêtes avec d'autres datasets
│   ├── athletes_with_owl.ttl
│   ├── currencies.ttl
│   └── world_population_objects_v2.ttl           
│
└── README.md          # Fichier de documentation (ce fichier)
```

> Les noms exacts des fichiers peuvent varier, mais la structure générale reste la même.

---

## Contenu du dépôt

- **`data/`**
  - Contient le **CSV d’origine** pour le projet.
  - Contient un fichier **RDF/Turtle (`.ttl`, `.rdf`)** générés à partir du CSV.

- **`queries/`**
  - Contient les **requêtes SPARQL** créees.

- **`sharedQueriesData/`**
  - Contient les données d'autres groupes nécessaires pour les rêquetes partagés.

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

   ```bash
   fuseki-server
   ```
   Par défaut, l’interface web est disponible sur :  
   `http://localhost:3030`

2. **Créer un dataset**

   - Aller sur l’interface de Fuseki.
   - Créer un **nouveau dataset**  "players".
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
- **Ilef Mhabrech** 
