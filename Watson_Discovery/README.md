# Scenario Watson Discovery

1. Créer un service Watson Discovery depuis l'interface IBM Cloud
2. Lancer l'outil
3. Création d'une collection, via l'upload des documents
4. Charger les 9 documents contenu dans le répertoire data : [lien](https://ibm.box.com/s/cz5a3gyry3dc083h0zxe37ou1g677m1g)
5. Dézipper le fichier sur la machine
6. Passer un peu de temps à analyser l'écran d'accueil, les renseignements découverts par Watson dans vos documents enrichis. 
- **General sentiments** affiche la répartition en pourcentage des documents marqués comme positifs, neutres et négatifs découverts par l'enrichissement Sentiment Analysis. 
- **Top entities** affiche les personnes, les lieux et les organisations découverts dans vos documents par l'enrichissement Entity Extraction. 
- **Content hierarchy** affiche les taxonomies hiérarchiques découvertes dans vos documents par l'enrichissement Category Classification. 
- **Related concepts** affiche les concepts découverts dans vos documents par l'enrichissement Concept Tagging. 
7. Tester une requête avancée avec l'outil
-   Pour rechercher les résultats comportant des entités nommées "indonésie" :  Cliquez sur Field et sélectionnez *enriched_text.entities.text*. Sélectionnez contains pour Operator et indonésie pour Value. La requête *enriched_text.entities.text:Indonésie* s'affiche dans le générateur de requête visuelle. Cliquez sur `Run Query`. La requête renvoie 4 résultats.  
- Pour rechercher les résultats comportant des entités nommées "caféine" :  Cliquez sur Field et sélectionnez *enriched_text.entities.text*. Sélectionnez contains pour Operator et caféine pour Value. La requête *enriched_text.entities.text:caféine* s'affiche dans le générateur de requête visuelle. Cliquez sur `Run Query`. La requête renvoie 4 résultats. 
- Pour rechercher les résultats comportant à la fois des entités "indonésie" et des entités "caféine" :  Cliquez sur `Run Query`. La requête renvoie 2 résultats.

8. Tester une requête en langage naturel 
    -  Qui a introduit le thé en Inde ?

## Pour aller plus loin

### Intégration avec Watson Assistant

1. Retourner sur l'interface de configuration de Watson Assistant
2. Aller dans l'onglet `Dialog`, puis sur le noeud `Tout le reste`
3. Remplacer le contenu via le JSON Editor par le contenu ci-dessous:
```
{
  "context": {
    "private": {
      "function_credentials": {
        "user": "4ba223cc-37a0-43a6-af42-d615ac44ff62",
        "password": "bwBscazbXrb6jsDyQRJ34GKGTPHhJ7wF6vs9tBezGWxLnK0Jm9vR0vz4vHI8Uhyu"
      }
    }
  },
  "output": {
    "text": {
      "values": [Recherche sur Watson Discovery, veuillez attendre quelques secondes ...],
      "selection_policy": "sequential"
    }
  },
  "actions": [
    {
      "name": "arabenandrasana@fr.ibm.com_dev/actions/mine/CafeThe",
      "type": "server",
      "parameters": {
        "url": “xxxxx,
        "apikey": “xxxx,
        "question": "<?input.text?>",
        "collection_id": “xxx”,
        "environment_id": “xxx”
      },
      "credentials": "$private.function_credentials",
      "result_variable": "$reponse"
    }
  ]
}
```
Remplacer les xxx par vos paramètres à trouver dans l'interface de Watson Discovery

4. Créer un sous-noeud et ajouter le texte suivant $reponse.text
5. Configurer un Jump To du noeud `Tout le reste`
6. Ensuite tester la phrase suivante : "Qui a introduit le thé en France ?". Watson Assistant ne reconnaissant pas l'intention, Watson Discovery est appelé pour chercher la réponse la plus appropriée à cette question.
