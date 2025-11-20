# Projet Dana – Les footballeurs les plus chers de 2021  
## WorkFlow 
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

3. **Créer un dataset**
   - Aller sur l’interface de Fuseki.
   - Créer un **nouveau dataset** `players`.
   - Importer le fichier RDF/Turtle depuis le dossier `data/`.

```bash
   fuseki-server --file=data/players.ttl /dataset
```
4. **Exécuter les requêtes SPARQL**
   - Ouvrir l’onglet **SPARQL** dans l’interface Fuseki.
   - Copier/coller le contenu d’un fichier de `queries/`.
   - Lancer la requête pour voir les résultats (table, JSON, CSV, etc.).

5. **Utiliser les données partagées**
   - Pour les requêtes qui utilisent des données d'autres groupes, les importer de la même manière dans un dataset séparé à partir de `sharedQueriesData/`.

### exemples des requêtes SPARQL : 
####  Requête 1 : Le classement des joueurs les plus chers du FC Barcelone

```bash

PREFIX rdf:     <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dbo:     <http://dbpedia.org/ontology/>
PREFIX ex:      <http://example.com/>
PREFIX foaf:    <http://xmlns.com/foaf/0.1/>

SELECT ?rank ?playerName ?marketValue
WHERE {
  ?player rdf:type      ex:Player ;
          dbo:team      ?team ;
          foaf:name     ?playerName ;
          ex:marketValue ?marketValue ;
          ex:rank       ?rank .

  ?team   rdf:type      ex:Team ;
          foaf:name     ?playerTeamName .

  FILTER(?playerTeamName = "FC Barcelona")
}
ORDER BY ASC(?rank)
---
```
#### Requête 2 : Statistiques offensives moyennes et totales par club
```bash
PREFIX dbo:  <http://dbpedia.org/ontology/>
PREFIX ex:   <http://example.com/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

SELECT ?clubName
       (COUNT(?player) AS ?nbPlayers)
       (SUM(xsd:integer(?goals)) AS ?totalGoals)
       (AVG(xsd:integer(?goals)) AS ?avgGoals)
       (SUM(xsd:integer(?assists)) AS ?totalAssists)
       (AVG(xsd:integer(?assists)) AS ?avgAssists)
WHERE {
    ?player ex:goals   ?goals ;
            ex:assists ?assists ;
            dbo:team   ?club .

    ?club foaf:name ?clubName .
}
GROUP BY ?clubName
ORDER BY DESC(?totalGoals)
LIMIT 10

```
### exemples des requêtes SPARQL partagées : 

#### Requête 3 : Top 10 des pays selon le ratio Valeur Marchande Totale / Population
Avec groupe 6: World Population


```
PREFIX rdf:   <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs:  <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbo:   <http://dbpedia.org/ontology/>
PREFIX ex:    <http://example.com/>
PREFIX foaf:  <http://xmlns.com/foaf/0.1/>
PREFIX xsd:   <http://www.w3.org/2001/XMLSchema#>

SELECT ?countryNamePlayers ?populationMillions ?totalMarketValueRounded ?ratio
WHERE {
  {
    SELECT ?countryNamePlayers ?population (SUM(?marketValue) AS ?totalMarketValue)
    WHERE {
      SERVICE <http://localhost:3030/players/query> {
        ?player  rdf:type        ex:Player ;
                 dbo:nationality ?countryPlayers ;
                 ex:marketValue  ?marketValue .

        ?countryPlayers rdf:type  ex:Country ;
                        foaf:name ?countryNamePlayers .
      }
      SERVICE <http://localhost:3030/worldpop/query> {
        ?countryPop rdf:type            dbo:Country ;
                    rdfs:label          ?countryNamePop ;
                    dbo:populationTotal ?population .
      }
      FILTER( STR(?countryNamePlayers) = STR(?countryNamePop) )
    }
    GROUP BY ?countryNamePlayers ?population
  }
  BIND( ROUND(xsd:double(?population) / 1000000*100)/100 AS ?populationMillions )
  BIND( ROUND(xsd:double(?totalMarketValue)*100)/100 AS ?totalMarketValueRounded )
  BIND( ROUND(((xsd:double(?totalMarketValueRounded) / xsd:double(?populationMillions))) * 100) / 100 AS ?ratio)
}
ORDER BY DESC(?ratio)
LIMIT 10
```
#### Requête 4 : Footballeurs des JO 2024 présents dans notre dataset
 Avec groupe 8: Athlètes des JO 24

```
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dbo:  <http://dbpedia.org/ontology/>
PREFIX ex:   <http://example.com/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT DISTINCT ?playerName
WHERE {
  SERVICE <http://localhost:3030/players/query> {
      ?player rdf:type        ex:Player ;
              foaf:name       ?playerName ;
              ex:marketValue  ?marketValue .
      OPTIONAL { ?player dbo:team ?team . }
  }
  SERVICE <http://localhost:3030/athlete/query> {
      ?athlete rdf:type   ?type ;
               foaf:name  ?athleteName ;
               dbo:sport  ?sportURI .
      FILTER(
          ?sportURI IN (
              dbr:Football
          )
      )
  }
  
  BIND(LCASE(?playerName) AS ?p)
  BIND(LCASE(?athleteName) AS ?a)
  BIND(STRAFTER(?p, " ") AS ?lastnameP)
  BIND(STRBEFORE(?p, " ") AS ?firstnameP)
  BIND(STRAFTER(?a, " ") AS ?firstnameA)
  BIND(STRBEFORE(?a, " ") AS ?lastnameA)
  
  FILTER(
      (
        ?lastnameA != "" &&
        ?lastnameP != "" &&
        ?lastnameA = ?lastnameP &&
        ?firstnameA = ?firstnameP
      )
  )
}
ORDER BY ?athleteName

```
#### Requête 5 : Correspondance pays–monnaie, total des market values par devise
Avec groupe 12: Les monnaies dans le monde
```
PREFIX rdf:   <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs:  <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbo:   <http://dbpedia.org/ontology/>
PREFIX ex:    <http://example.com/>
PREFIX foaf:  <http://xmlns.com/foaf/0.1/>
PREFIX xsd:   <http://www.w3.org/2001/XMLSchema#>
PREFIX exont: <http://example.org/ontology/> 
PREFIX schema1: <http://schema.org/>

SELECT ?currencyCode ?currencyLabel ((ROUND(SUM(?mv)*100)/100) AS ?totalMarketValue)
WHERE {
  SERVICE <http://localhost:3030/players/query> {
    ?player rdf:type      ex:Player ;
            dbo:nationality ?country ;
            ex:marketValue ?marketValue .

    ?country foaf:name ?countryName .

    BIND(xsd:double(?marketValue) AS ?mv)
  }
  SERVICE <http://localhost:3030/currencies/query> {
    ?currency rdf:type       dbo:Currency ;
              dbo:usedInCountry ?dbpCountry ;
              dbo:isoCode    ?currencyCode ;
              rdfs:label     ?currencyLabel ; 
            	dbo:authority ?auth ;
            exont:minorUnit ?mu ;
            schema1:currencySymbol ?sym .
    
    ?dbpCountry rdf:type dbo:Country ;
                rdfs:label ?dbpCountryName .
        FILTER( lang(?currencyLabel) = "" )
  }
  FILTER( STR(?countryName) = STR(?dbpCountryName) )
}
GROUP BY ?currencyCode ?currencyLabel
ORDER BY DESC(?totalMarketValue)
LIMIT 10
```

## Auteurs

- **Yanis Dabin** – @StuartGrosPecs  
- **Khalil Mhedhbi** – @KhalilMhedhbi574
- **Ilef Mhabrech** – @Ilef-mhabrech
