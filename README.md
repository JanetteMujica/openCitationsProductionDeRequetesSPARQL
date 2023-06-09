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

De plus, OpenCitation utilise des standards du web sémantique tels que RDF (Resource Description Framework) et OWL (Web Ontology Language). Les données peuvent être alors comprises par la machine et ensuite réutilisées par les chercheurs.[Peroni (2020)](https://direct.mit.edu/qss/article/1/1/428/15580/OpenCitations-an-infrastructure-organization-for)

Plus précisément, l’ontologie, OpenCitation, contient des Classes, des Object Properties, Data Properties, Named individuals, Annotation Properties et des Namespace Declarations pour lesquelles 


### **4.**    Produire au moins cinq exemples de requêtes SPARQL suffisamment complexes et intéressantes pour démontrer l’intérêt de la ressource. Vous pouvez accompagner ces requêtes d’un court texte de présentation.

