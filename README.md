# Exercice NLP

Un petit problème ouvert de NLP sur les données d'avis de la HAS.

## Description des données 

Une requête rest permet d'interroger le site de la HAS pour récupérer les avis sous la forme de xml.Chacune des pages renvoyées contient un champ `resume` avec du texte : 

```
https://www.has-sante.fr/rest/search?
text=&
mode=all&
searchedAllFields=true
&catName=true&exactCat=false&catMode=and&
cids=&dateType=cdate&
dateSince=0&dateSince_user=0&
dateSince_unit=1&beginDateStr=&endDateStr=&
exactType=false&replaceFileDoc=false&
types=generated.EvaluationDesTechnologiesDeSante&
types=generated.GuideMedecinALD&types=generated.GuidePatient&
types=generated.EvaluationDesPratiques&
types=generated.RecommandationsProfessionnelles&
types=generated.EvaluationDesProgrammesEtPolitiq&
types=generated.RecommandationVaccinale&
mids=&midsChooserDisplay=&
mids=&gidsChooserDisplay=&
gids=&pstatus=0&
pstatus=&
pstatus=&
langs=&wrkspcChooserDisplay=&
wrkspc=&searchInSubWorkspaces=false&
wrkspc=&
start=0&pageSize=2
&sort=pdate&reverse=true
```

## Problématique

La HAS utilise des labels tirés de la nomenclature MESH pour indexer ces publications et les référencer au travers de son moteur de recherche. Ces catégories sont regroupées dans le champ `categories`. NB: Une même page peut avoir plusieurs catégories.

Une proposition de catégorie(s) pourrait être proposé par un algorithme afin de faciliter l'intégration dans le site. 
En s'appuyant sur des requêtes rest interrogeant le site de la HAS, développer un tel alogrithme.

Différentes étapes pourront être développées :

- Acquisition et nettoyage des données
- Description des données
- Entraînement de modèles 
- Restitution des résultats (métriques)
- Description de la solution envisagée dans la section README ci-dessous.

La restitution du code se fera sur une présentation d'environ 30 min mettant en perspective le problème, les codes développés (scripts et/ou notebooks) et les résultats. Le principal langage à utiliser est python mais d'autres langages pourront être utilisés si proprement justifiés.

## Disclaimer

Nous n'attendons pas forcément de solution complète ou très performante mais un début d'implémentation et de réflexion (architecture de la solution, librairies employées, premiers scripts).  

# Solution apportée
