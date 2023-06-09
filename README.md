# Analyse du projet *[Open Citations](https://opencitations.net/)* utilisant les technologies du web sémantique, du Linked Open Data et production de requêtes SPARQL


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

**[The OpenCitations Data Model](https://figshare.com/articles/online_resource/Metadata_for_the_OpenCitations_Corpus/3443876)** détaille les ressources RDF des ensembles de données OpenCitation qui ont 5 niveaux de metadata: *Dataset metadata, Bibliographic entity metadata, Identifiers, Pronenance metadata et Virtual entities.* Au sein des ensembles de données, différentes classes d'informations (différents types d'entités) sont identifiées et décrites à l'aide de noms uniques et, généralement, sont accompagnées de sigles à deux lettres ("*short names*"), par exemple *Bibliographic resource* (*short: br*). On voit comment établir les relations, par exemple:

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

[Daquino (2023)](https://figshare.com/articles/online_resource/Metadata_for_the_OpenCitations_Corpus/3443876)

### **4.**    Produire au moins cinq exemples de requêtes SPARQL suffisamment complexes et intéressantes pour démontrer l’intérêt de la ressource. Vous pouvez accompagner ces requêtes d’un court texte de présentation.

Pour les cinq requêtes, je vais utiliser le [OpenCitations Meta SPARQL endpoint](https://opencitations.net/meta/sparql). Ce endpoint fournit les métadonnées bibliographiques de toutes les publications incluses dans les index OpenCitations.

Comme expliqué dans le [Dataset de Meta](https://opencitations.net/meta), Chaque entité dans OpenCitations Meta est assignée un identifiant (OMID). 

L'OMID a une structure 
**[[entity_type_abbreviation]]/[[supplier_prefix]][[sequential_number]]**.

Par exemple, le premier article de journal traité a l'OMID br/0601 (l'URI complet est https://w3id.org/oc/meta/br/0601),
-	où br est l'abréviation de ressource bibliographique, 
-	et 060 correspond au préfixe du fournisseur (c'est-à-dire OpenCitations Meta)
-	1 indique qu'il s'agit de la première ressource bibliographique jamais créée dans l'index.

