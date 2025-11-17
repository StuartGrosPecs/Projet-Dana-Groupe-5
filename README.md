# Projet Dana – Les footballeurs les plus chers de 2021


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
└── README.md          
```

---

## Contenu du dépôt

- **`data/`**
  - Contient le **CSV d’origine** pour le projet.
  - Contient un fichier **Turtle (`.ttl`)** générés à partir du CSV.

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
