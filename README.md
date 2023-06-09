# Analyse du projet *[Open Citations](https://opencitations.net/)* utilisant les technologies du web sémantique, du Linked Open Data et production de requêtes SPARQL

>Réalisée par Janette Mujica-Zepeda
>Dans le cadre de l'atelier HNU6054 Web Sémantique et données<br>
>À l'Université de Montréal<br>
>Présenté à David Valentine<br>


[Description de l'exercice](https://davvalent.github.io/hnu6054/exercice-02/)


### **1.**    Identifier et choisir un projet lié au web sémantique ou au LOD qui dispose d’un SPARQL endpoint
J'ai choisi *[Open Citations](https://opencitations.net/)* disposant de plusieurs *SPARQL endpoints*. J'explorerai plus précisemment [OpenCitations Meta SPARQL endpoint](https://opencitations.net/meta/sparql) avec la base de données [OpenCitations Meta](https://opencitations.net/meta).

### **2.**    Présenter le projet et ses objectifs généraux

Le projet d'OpenCitations est de collecter, publier et préserver de manière ouverte des métadonnées bibliographiques et de citations académiques. L’objectif est de fournir un accès gratuit dans le but de favoriser l'accès ouvert et la reproductibilité de la recherche. [Perroni (2022)](https://zenodo.org/record/6976670)

### **3.**    Expliquez comment les technologies du web sémantique apportent une solution pertinente au projet en vous intéressant notamment à l’ontologie et aux vocabulaires utilisés

OpenCitation utilise l’ontologie [*Open Citation Ontology*](https://opencitations.github.io/ontology/current/ontology.html) pour modéliser les métadonnées bibliographiques. Les vocabulaires utilisés dans le web sémantique sont des ensembles de termes et de relations prédéfinis. Cette ontologie décrit, entre-autre, auteurs, articles, revues, et citations. Elle définit ces termes, leurs significations et les liens entre eux. Elle rend possible et facilite l’exploration des relations entre les différentes entités bibliographiques. [Peroni (2020)](https://direct.mit.edu/qss/article/1/1/428/15580/OpenCitations-an-infrastructure-organization-for)

De plus, OpenCitation utilise des standards du web sémantique tels que RDF (Resource Description Framework) et OWL (Web Ontology Language). Les données peuvent être alors comprises par la machine et ensuite réutilisées par les chercheurs. [Peroni (2020)](https://direct.mit.edu/qss/article/1/1/428/15580/OpenCitations-an-infrastructure-organization-for)

Plus précisément,  l’ontologie [*Open Citation Ontology*](https://opencitations.github.io/ontology/current/ontology.html) contient des [*Classes*](https://opencitations.github.io/ontology/current/ontology.html#classes), des [*Object Properties*](https://opencitations.github.io/ontology/current/ontology.html#objectproperties), des [*Data Properties*](https://opencitations.github.io/ontology/current/ontology.html#dataproperties), des [*Named individuals*](https://opencitations.github.io/ontology/current/ontology.html#namedindividuals), des [*Annotation Properties*](https://opencitations.github.io/ontology/current/ontology.html#annotationproperties) et des [*Namespace Declarations*](https://opencitations.github.io/ontology/current/ontology.html#namespacedeclarations). 

**Le shéma du [DataModel](http://opencitations.net/model)**
![model](https://github.com/JanetteMujica/openCitationsProductionDeRequetesSPARQL/assets/112497575/3f273345-39e2-42a7-bc5a-6ac4eefc3161)

**L'article [The OpenCitations Data Model (Danquino 2023](https://figshare.com/articles/online_resource/Metadata_for_the_OpenCitations_Corpus/3443876)** détaille les ressources RDF des ensembles de données OpenCitation qui ont 5 niveaux de metadata: *Dataset metadata, Bibliographic entity metadata, Identifiers, Pronenance metadata et Virtual entities.* Au sein des ensembles de données, différentes classes d'informations (différents types d'entités) sont identifiées et décrites à l'aide de noms uniques et, généralement, sont accompagnées de sigles à deux lettres ("*short names*"), par exemple *Bibliographic resource* (*short: br*). On voit comment établir les relations, par exemple:

**Metadata elements that may be associated with a responsible agent’s role**
- has role type: thing
The specific type of role under consideration (e.g. author, editor or publisher).
- is held by: responsible agent (ra) 

et

**Metadata elements that may be associated with a responsible agent**
- has name string: literal
The name of an agent (for people, usually in the format: given name followed by family
name, separated by a space).

On y détaille aussi comment faire correspondre les attributs et les propriétés avec les entités liées à OWL, par exemple on utilise: foaf: http://xmlns.com/foaf/0.1/ pour

**Bibliographic entities**
- Responsible agent: foaf:Agent

### **4.**    Produire au moins cinq exemples de requêtes SPARQL suffisamment complexes et intéressantes pour démontrer l’intérêt de la ressource. Vous pouvez accompagner ces requêtes d’un court texte de présentation.

Pour les cinq requêtes, je vais utiliser le [OpenCitations Meta SPARQL endpoint](https://opencitations.net/meta/sparql). Ce endpoint fournit les métadonnées bibliographiques de toutes les publications incluses dans les index OpenCitations.

Comme expliqué dans le [Dataset de Meta](https://opencitations.net/meta), Chaque entité dans OpenCitations Meta est assignée un identifiant (OMID). 

L'OMID a une structure 
**[[entity_type_abbreviation]]/[[supplier_prefix]][[sequential_number]]**.

Par exemple, le premier article de journal traité a l'OMID br/0601 (l'URI complet est https://w3id.org/oc/meta/br/0601),
-	où br est l'abréviation de ressource bibliographique, 
-	et 060 correspond au préfixe du fournisseur (c'est-à-dire OpenCitations Meta)
-	1 indique qu'il s'agit de la première ressource bibliographique jamais créée dans l'index.

**1) Ma première requête consiste à chercher le premier article traité avec l’OMID : https://w3id.org/oc/meta/br/0601. J’ai choisi cette requête car l’OMID est fourni dans la documentation du [Dataset de Meta](https://opencitations.net/meta).**

```
PREFIX oc: <https://w3id.org/oc/meta/>

SELECT ?p ?entity WHERE {
  BIND(<https://w3id.org/oc/meta/br/0601> AS ?omid) .
  ?omid ?p ?entity .
}
```
Ensuite, j'ai tenté sans succès de chercher l’auteur de ce premier document. Comme je n'arrivais pas à avancer dans le travail, j'ai commencé à travailler avec ChatGPT pour m'aider à construire des requêtes. J'ai copié le code par défault du endpoint, puis demander de m'extraire tous les documents avec doi, title et pub_date.

**REQUÊTE PAR DÉFAULT DANS LE ENDPOINT**
```
PREFIX datacite: <http://purl.org/spar/datacite/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX literal: <http://www.essepuntato.it/2010/06/literalreification/>
PREFIX prism: <http://prismstandard.org/namespaces/basic/2.0/>

SELECT ?id ?title ?pub_date {
    
    ?identifier literal:hasLiteralValue "10.1162/qss_a_00023". # Filtrer l'identifiant spécifique "10.1162/qss_a_00023"
   
    ?br datacite:hasIdentifier ?identifier;  # Récupérer les informations de la ressource bibliographique correspondant à l'identifiant
       dcterms:title ?title;
       prism:publicationDate ?publicationDate.
    
    BIND(STR(?publicationDate) AS ?pub_date) # Convertir la date de publication en chaîne de caractères

    BIND((CONCAT("doi:", "10.1162/qss_a_00023")) AS ?id) # Construire l'identifiant au format "doi:xxxxxx"
}
```

**2) Ma deuxième requête consiste à extraire tous les documents avec doi, title et pub_date. J’ai limité ma requête à 50 parce que c’était trop demandant pour ma machine.**

```
PREFIX datacite: <http://purl.org/spar/datacite/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX literal: <http://www.essepuntato.it/2010/06/literalreification/>
PREFIX prism: <http://prismstandard.org/namespaces/basic/2.0/>

SELECT ?id ?title ?pub_date {
    ?br datacite:hasIdentifier ?identifier ;    # On récupère les identifiants des ressources bibliographiques
       dcterms:title ?title ;                   # On récupère les titres des ressources bibliographiques
       prism:publicationDate ?publicationDate .  # On récupère les dates de publication des ressources bibliographiques
    ?identifier literal:hasLiteralValue ?literalValue .  # On récupère les valeurs littérales des identifiants
    BIND(CONCAT("doi:", ?literalValue) AS ?id)  # On construit les identifiants au format "doi:xxxxxx"
    BIND(STR(?publicationDate) AS ?pub_date)    # On convertit la date de publication en chaîne de caractères
}
LIMIT 50
```
**3) Ma troisième requête consiste à extraire tous les documents avec doi, title et pub_date et puis, ajouter l'OMID et aussi trier les résultats par ordre décroissant, du plus récent au plus vieux.**

J’ai choisi d’ajouter l’OMID car je veux être en mesure d’obtenir l'entité *author* puisqu'il est associé à l'OMID. [Selon la description du dataset](https://opencitations.net/meta) : « *The entities subject to deduplication and associated with an OMID are identifiers (abbr. id), agent roles (i.e., authors, editors, publishers, abbr. ar), responsible agents (i.e., people and organisations, abbr. ra), resource embodiments (i.e., pages, abbr. re), venues, volumes, and issues (which are all bibliographic resources, abbr. br).*»

Je n'ai pas pu obtenir de réponse avec *DER BY DESC(?pub_date)* Je l'ai donc mis en commentaire.

```
PREFIX datacite: <http://purl.org/spar/datacite/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX literal: <http://www.essepuntato.it/2010/06/literalreification/>
PREFIX prism: <http://prismstandard.org/namespaces/basic/2.0/>

SELECT ?omid ?id ?title ?pub_date {   # Ajout de ?omid
    ?br datacite:hasIdentifier ?identifier ;
       dcterms:title ?title ;
       prism:publicationDate ?publicationDate .
    ?identifier literal:hasLiteralValue ?literalValue .
    BIND(CONCAT("doi:", ?literalValue) AS ?id)
  BIND(STR(?publicationDate) AS ?pub_date)
    BIND(STRAFTER(str(?br), "oc/meta/") AS ?omid)   # Extrait l'ID de la ressource après "oc/meta/"
}
#ORDER BY DESC(?pub_date)   # Trie les résultats par ordre décroissant de la date de publication
LIMIT 20
```

**4) Ma quatrième requête consiste à obtenir les *author* à travers les agents reliés à chaque OMID extrait précédemment **

J'ai fait une requête à ChatGPT d'adapter le code prédemment considérant que: 

**Metadata elements that may be associated with a responsible agent’s role**
- has role type: thing
The specific type of role under consideration (e.g. author, editor or publisher).
- is held by: responsible agent (ra) 

et

**Metadata elements that may be associated with a responsible agent**
- has name string: literal
The name of an agent (for people, usually in the format: given name followed by family
name, separated by a space).

```
PREFIX datacite: <http://purl.org/spar/datacite/>         
PREFIX dcterms: <http://purl.org/dc/terms/>              
PREFIX literal: <http://www.essepuntato.it/2010/06/literalreification/>   
PREFIX prism: <http://prismstandard.org/namespaces/basic/2.0/>   
PREFIX oc: <http://w3id.org/oc/ontology/>

SELECT ?omid ?id ?title ?pub_date ?author_name {
  ?br datacite:hasIdentifier ?identifier ;
     dcterms:title ?title ;
     prism:publicationDate ?publicationDate .  
  ?identifier literal:hasLiteralValue ?literalValue .     
  BIND(CONCAT("doi:", ?literalValue) AS ?id) 
  BIND(STR(?publicationDate) AS ?pub_date)
  ?omid oc:hasResponsibleAgent ?ra .                       # Relation entre l'OMID et l'agent responsable
  ?ra oc:hasRoleType ?role_type ;                          # Relation entre l'agent responsable et le type de rôle
     oc:hasNameString ?author_name .                       # Relation entre l'agent responsable et le nom d'auteur
  FILTER (?role_type = oc:author)                          # Filtre pour ne récupérer que les auteurs
}
LIMIT 20
```
Ce code de retourne rien. J'ai donc revisé la documentation pour 
