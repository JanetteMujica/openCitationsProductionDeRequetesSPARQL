# Première requête

Votre requête consiste à décrire la ressource identifiée par l’URI https://w3id.org/oc/meta/br/0601.
Sachez que vous pouvez simplement effectuer ce type de requête grâce à la clause DESCRIBE :

```sparql
DESCRIBE <https://w3id.org/oc/meta/br/0601>
```

Dans le cas d’OC, vous recevrez le résultat dans le format Turtle, mais le plus souvent c’est sous la forme d’un tableau.

Voici un exemple de requête pour récupérer les auteurs (conseil : à lire en consultant le schéma du modèle) :

```sparql
PREFIX pro: <http://purl.org/spar/pro/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
SELECT *
WHERE {
  <https://w3id.org/oc/meta/br/0601> ?p ?role . # motif de départ, descriptionde la publication
  ?role a pro:RoleInTime ; # parmi ces triplets, je garde ceux dont l’objet est de type RoleInTime
    pro:withRole pro:author ; # ET dont l’objet a le rôle auteur
    pro:isHeldBy ?author . # avec rôle détenu par ?author
  ?author foaf:givenName ?givenName ; # Je récupère le nom
    foaf:familyName ?familyName .
}
```

# Troisième requête

Il n’y a normalement aucune difficulté à trier les résultats pour une requête de ce type.
Le serveur ne répond effectivement plus lorsque l’on tente de trier ou de regrouper ces résultats, ce qui m’apparait tout à fait curieux.

# Quatrième requête

Pour cette requête, il faut suivre l’approche de la première requête, présentée plus haut.
`oc:hasResponsibleAgent`, `oc:hasRoleType` et `oc:hasNameString` ne sont pas des propriétés du modèle d’OC.
Nous allons utiliser la propriété `pro:isDocumentContextFor` pour accéder aux rôles et aux responsables de la publication (voir le schéma du modèle à cet effet).

```sparql
PREFIX datacite: <http://purl.org/spar/datacite/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX literal: <http://www.essepuntato.it/2010/06/literalreification/>
PREFIX prism: <http://prismstandard.org/namespaces/basic/2.0/>
PREFIX oc: <http://w3id.org/oc/ontology/>
PREFIX pro: <http://purl.org/spar/pro/>

SELECT ?br ?id ?title ?publicationDate ?authorName {

  ?br datacite:hasIdentifier ?identifier ;
    dcterms:title ?title ;
    prism:publicationDate ?publicationDate ;
    pro:isDocumentContextFor ?role . # associer publication et rôles
  
  ?identifier literal:hasLiteralValue ?literalValue .
  BIND(CONCAT("doi:", ?literalValue) AS ?id) .

  ?role pro:withRole pro:author ; # préciser le type de rôle
    pro:isHeldBy ?author . # récupérer les personnes détenant le rôle
  ?author foaf:givenName ?givenName ; # Je récupère le nom
    foaf:familyName ?familyName .
  # finalement je concatène prénom et nom pour faire beau
  BIND(CONCAT(?givenName, " ", ?familyName) AS ?authorName) .
}
LIMIT 20
```

# Cinquième requête

Pour cette requête, il sera difficile de dire pourquoi elle ne fonctionne pas, car plusieurs version du modèle de données ont été publiées depuis la publication l’entrée de blogue à laquelle vous faites référence.
De plus, OC utilise aujourd’hui des jeux de données qui n’étaient pas constitués à l’époque.
Si je comprends bien en regardant votre requête, vous souhaitez récupérer les publications qui citent la publication identifiée par le DOI 10.1038/227680a0.
Une façon simple de commencer pour une requête semblable serait (COCI SPARQL endpoint) :

```sparql
PREFIX cito:<http://purl.org/spar/cito/>

SELECT DISTINCT * WHERE {
  GRAPH <https://w3id.org/oc/index/coci/> {
    BIND(<http://dx.doi.org/10.1038/227680a0> as ?cited_entity)
    ?citation a cito:Citation ;
      cito:hasCitedEntity ?cited_entity ;
      cito:hasCitingEntity ?citing_entity ;
      cito:hasCitationCreationDate ?creation_date ;
  }
} LIMIT 100
```
