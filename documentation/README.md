# Données

## Thématiques

Les thématiques utilisées sur le site internet de la HAS sont décrites dans le document `documentation/categories_thematiques.xlsx`

Elles sont organisées selon une arborescence, que l'on peut reconstruire via l'identifiant du parent.

On peut également interroger dynamiquement cette arborescence par API (point d'API `https://www.has-sante.fr/rest/data/children/{id}`).

## Types de contenus

Les contenus du site de la HAS ont un type, parmi une liste donnée dans le document `documentation/type_contenus.xlsx`. 
Ce document donne également le nom interne (technique) d'un type.

On peut également retrouver ces informations par API  `https://www.has-sante.fr/rest/types`.

On s'interessera en particulier à catégoriser les documents des types suivants :
- Recommandation de bonne pratique
- Guide maladie chronique
- Guide méthodologique
- Outil d'amélioration des pratiques professionnelles
- Recommandation en santé publique
- Études et Rapports
- Guide usagers
- Recommandation vaccinale
- Avis sur les Médicaments
- Avis sur les dispositifs médicaux et autres produits de santé
- Evaluation des technologies de santé

## Contenus

Le site de la HAS est basée sur la plateforme Jalios, qui fonctionne comme un store XML.
Chaque objet du store (publication, catégorie, etc) a un identifiant et des attributs.

Pour effectuer la classification, on s'intéressera en priorité au résumé html des publications, disponible dans le champ `resume` (ou parfois `objectifs`), plutôt qu'aux documents pdf joints (ce qui nécessiterait plus de travail).

## Accès API

Il est possible d'interroger le backend Jalios par une API, décrite de façon générique dans [ce document](https://community.jalios.com/jcms/jx_59631/fr/services-web-restful-avec-jcms-open-api).

### Points d'API

Le principal point d'API utile dans l'exercice est 
`https://www.has-sante.fr/rest/data/{param}`, qui permet de :
- récupérer un objet si le paramètre est un identifiant (exemple https://has-sante.fr/rest/data/p_3240117)
- récupérer l'ensemble des objets d'un type particulier, si le paramètre est un type de données (exemple https://www.has-sante.fr/rest/data/RecommandationVaccinale)


D'autres points d'API existent, dont certain décrit plus haut, et d'autres a priori inutiles à l'exercices.

On pointer vers une page web ou un document sur le site de la HAS à partir de l'identifiant d'un objet `https://has-sante.fr/jcms/{id}`.

Exemples 
- https://has-sante.fr/jcms/p_3240117
- https://has-sante.fr/jcms/p_3240130

`https://www.has-sante.fr/rest/search` permet d'effectuer une recherche avec des paramètres.

### Paramètres d'API

L'API accepte des paramètres, qui peuvent être passés encodés dans l'url après un `?` et séparés par des `&`.

En voici quelques uns, sans être exhaustifs.

On peut naviguer dans les résultats d'une collection paginée avec les paramètres
- `start` : début de la page
- `pageSize` : taille de la page
- `sort` : paramètre de tri
- `reverse` : inverse ordre du tri

Exemple : https://www.has-sante.fr/rest/data/RecommandationVaccinale?start=0&pageSize=2&sort=pdate&reverse=false

On peut effectuer une recherche avec les paramètres suivants
- `text` : texte de recherche 
- `types` : type de contenu pour préciser la recherche (paramètre peut être répété)
- `cids` : catégorie pour préciser la recherche (paramètre peut être répété)
- `catMode` : comment combiner les filtres sur les catégories (`and`, `or`)
- et autres [illustrés dans cette requête](https://www.has-sante.fr/rest/search?text=&mode=all&searchedAllFields=true&catName=true&exactCat=false&catMode=and&cids=&dateType=cdate&dateSince=0&dateSince_user=0&dateSince_unit=1&beginDateStr=&endDateStr=&exactType=false&replaceFileDoc=false&types=generated.EvaluationDesTechnologiesDeSante&types=generated.GuideMedecinALD&mids=&midsChooserDisplay=&mids=&gidsChooserDisplay=&gids=&pstatus=0&pstatus=&pstatus=&langs=&wrkspcChooserDisplay=&wrkspc=&searchInSubWorkspaces=false&wrkspc=)

### Résultas en json

Par défaut, l'API envoie une réponse au format XML.

Il est possible d'obtenir une réponse au format json, en indiquant `application/json` dans l'en-tête `Accept`.

La réponse en json peut être plus simple à manipuler. 
Elle est en revanche plus pauvre que la réponse XML, notamment seulement l'identifiant des catégories est renvoyé, sans leur nom.

Exemple de requête Python

```python
import requests
r = requests.get("https://www.has-sante.fr/rest/data/RecommandationVaccinale", 
                 headers={"accept": "application/json"})
r.json()
```
