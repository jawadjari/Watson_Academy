# Scenario Watson Discovery

1. Créer un service Watson Discovery depuis l'interface IBM Cloud
2. Lancer l'outil
3. Création d'une collection, via l'upload des documents
4. Charger les 9 documents
5. Passer un peu de temps à analyser l'écran d'accueil, les renseignements découverts par Watson dans vos documents enrichis. 
- General sentiments affiche la répartition en pourcentage des documents marqués comme positifs, neutres et négatifs découverts par l'enrichissement Sentiment Analysis. 
- Top entities affiche les personnes, les lieux et les organisations découverts dans vos documents par l'enrichissement Entity Extraction. 
- Content hierarchy affiche les taxonomies hiérarchiques découvertes dans vos documents par l'enrichissement Category Classification. 
- Related concepts affiche les concepts découverts dans vos documents par l'enrichissement Concept Tagging. 
6. Tester une requête avancée avec l'outil
-   Pour rechercher les résultats comportant des entités nommées "indonésie" :  Cliquez sur Field et sélectionnez enriched_text.entities.text. Sélectionnez contains pour Operator et indonésie pour Value. La requête enriched_text.entities.text:IBM s'affiche dans le générateur de requête visuelle. Cliquez sur `Run Query`. La requête renvoie 4 résultats.  
- Pour rechercher les résultats comportant des entités nommées "caféine" :  Cliquez sur Field et sélectionnez enriched_text.entities.text. Sélectionnez contains pour Operator et caféine pour Value. La requête enriched_text.entities.text:watson s'affiche dans le générateur de requête visuelle. Cliquez sur `Run Query`. La requête renvoie 4 résultats. 
- Pour rechercher les résultats comportant à la fois des entités "indonésie" et des entités "caféine" :  Cliquez sur `Run Query`. La requête renvoie 2 résultats.

6. Tester une requête en langage naturel 
    -  Qui a introduit le thé en Inde ?

## Pour aller plus loin

### Intégration avec Watson Assistant