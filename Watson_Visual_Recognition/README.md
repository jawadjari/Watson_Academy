## Pour aller plus loin 

### Intégration avec Watson Assistant

1. Retourner sur l'interface de configuration de Watson Assistant
2. Créez une intention `#image` avec l'exemple "J'ai une image, peux-tu m'aider à reconnaitre le produit ?" 
3. Créez une entité `@URL` avec un modèle Pattern
`^(http:\/\/www\\.|https:\/\/www\\.|http:\/\/|https:\/\/)?[a-z0-9]+([\\-\\.]{1}[a-z0-9]+)*\\.[a-z]{2,5}(:[0-9]{1,5})?(\\/.*)?$`
2. Créez un noeud pour la détection des images avec le texte "Bien sûr, peux-tu me fournir un url ?"
3. Créez un sous-noeud avec `@URL` comme condition
3. Remplacer le contenu via le JSON Editor par le contenu ci-dessous:
```
{
  "context": {
    "private": {
      "function_credentials": {
        "user": "XXXXXXX",
        "password": "YYYYY"
      }
    }
  },
  "output": {
    "text": {
      "values": [],
      "selection_policy": "sequential"
    }
  },
  "actions": [
    {
      "name": "/vincent.perrin@fr.ibm.com_dev/vperrin/WVR",
      "type": "server",
      "parameters": {
        "url": "<? input.text ?>"
      },
      "credentials": "$private.function_credentials",
      "result_variable": "context.wvr"
    }
  ]
}
```
Remplacer les xxx par vos paramètres à trouver dans l'interface de Watson Discovery
4. Créer un sous-noeud et ajouter le texte suivant "Je reconnais : <? $wvr.images[0].classifiers[0].classes[0].class ?>"
5. Configurer un Jump To du noeud `URL`
5. Ensuite tester 