# Tutorial Pas à Pas : Comment construire un agent conversationnel 
Nous allons créer un chatbot qui permet la prise de commande de café ou thé et renseigne le client sur les produits et leurs prix.

## Création d'un compte ou accès à la plateforme IBM Cloud
1. Aller sur le lient et créer un compte : https://console.bluemix.net/registration/
2. Ou, aller à  https://console.bluemix.net/ et identifiez vous avec votre compte.

## Création d'une instance IBM Watson Assistant (ex- Watson Conversation)
1. Une fois authentifié, cliquez sur `Catalog` dans la barre supérieure de l'écran
2. Recherchez `Assistant`
3. Dans la catégorie `AI ou IA`, cliquez sur `Watson Assistant`
5. Cliquez `Create/Créer`
6. Puis, cliquez sur `Launch tool/Outil de lancement`

## Création d'un Skill
1. Une fois dans l'outil d'administration de Watson Assistant, cliquez sur `Create` pour ajouter un nouveau skill
2. Donnez un nom à ce skill par exemple `Coffee-bot` et sélectionnez la langue Française (sauf si vous souhaitez essayer dans une autre langue)

## Création des Intents/Intentions
1. Cliquez `Add intent`
2. Nommez la nouvelle intention `commande-boisson`
3. Ajouter une description expliquant le but de cette intention. Par exemple, "Le client veut commander une boisson."
4. Cliquez `Enter` pour créer l'intention.
5. Commencer par ajouter quelques exemples de phrases qu'un client pourrait écrire pour commander une boisson. (A minima 5 phrases)
  - Je souhaiterai commander un expresso
  - J'ai besoin de cafféine
  - un latte 
  - un cappuccino serait apprécié
  - Un mocha s'il vous plait
6. Ouvrir le panneau `Try it Out` en cliquant sur la bulle en haut à droite. Cela vous permet de tester la réaction de votre bot

![Try_it Panel](https://github.com/vperrinfr/Watson_Academy/blob/master/pictures/Try_It.png)


7. Attendez que le bot termine son entraînement, puis tapez `Pourrais-je commander un expresso`. Le bot doit classifier cette phrase `commande-boisson`. Même si vous n’avez pas appris l’intention sur cette phrase exacte, Watson peut toujours la comprendre.
8. Ajoutez quelques intentions supplémentaires pour couvrir d'autres demandes. Essayez de créer les intentions suivantes et d’ajouter quelques exemples à chacune d’elles
  - #voir-produits (Le client veut connaitre les produits disponibles)
  - #prix (Le client veut connaitre le prix des produits)
  - #recherche_magasin (Le Client cherche la localisation des magasins)
  
Voila les intentions finies:
![finished intents](https://github.com/vperrinfr/Watson_Academy/blob/master/pictures/Liste_Intent.png)

## Création des Entities
1. Cliquez sur l'onglet `Entities`
2. Cliquez `Add entity` and add the name `boissons`
3. Activez le `Fuzzy Matching` si vous souhaitez que Watson comprenne les erreurs de frappe.
4. Ajoutez la valeur `espresso` avec comme synonyme `cafe`. 
5. Ajoutez d'autres valeurs correspondant aux produits au catalogue :
  - Cappuccino
  - Latte
  - Menthe
  - Mocha
  - Ceylan

Voici l'entité `@boisson` finalisée:
![finished intents](https://github.com/vperrinfr/Watson_Academy/blob/master/pictures/Entite_boisson.png)

6. Quittez la page, et cliquez sur `System entities` dans l'onglet `Entities` 
7. Activez`@sys-number` pour permettre la détection des nombres.

Voici l'entité système finalisée:
![finished intents](https://github.com/vperrinfr/Watson_Academy/blob/master/pictures/System_Entities.png)

8. Créez d'autres entités.
9. Cliquez `Add entity` and add the name `Modèle_REGEX`
10. Désactivez le `Fuzzy Matching`
11. Saississez `Téléphone`comme nom
11. Sélectionnez `Patterns` au lien de `Synonyms`
12. Copiez l'expression régulière pour détecter/valider un numéro de téléphone : ^(\\\\+33|0|0033)[0-9]{9}$
13. Tester la reconnaissance d'un numéro de téléphone dans le panneau `Try it out`
14. Cliquez `Add entity` and add the name `Livraison`
14. Cliquez `Add entity` and add the name `Validation` (Oui, Non)
15. Cliquez `Add entity` and add the name `Ville` (Paris, Bordeaux, Lille...), la 

Voici la liste des entités finalisée:
![finished intents](https://github.com/vperrinfr/Watson_Academy/blob/master/pictures/Liste_Entities.png)


## Construction du dialogue
1. Cliquez sur l'onglet `Dialog`
2. Cliquez `Create`
3. Cliquez sur le node `Welcome` si vous souhaitez changer/personnaliser le message d'accueil
4. Cliquez `Add node`, and name it `commande-boisson`
5. Ajoutez votre intention `#commande-boisson` comme valeur dans le champ `If bot recognizes`
6. En haut à droite du noeud, cliquez sur `Customize`
7. Activez les `Slots` et appuyez sur `Apply`
12. Dans la rubrique `Check for`, ajouter `@boissons`
13. Dans la rubrique `If not present, ask` ajoutez une question comme "Que souhaitez-vous boire ?"
14. Cliquez `Add slot`, et ajoutez une condition "prompt for" `@sys-number` avec la question "Combien de tasses de $boissons souhaitez-vous ?"
(Note: La syntaxe `$variable` permet l'accès au contenu du variable.)
14. Cliquez `Add slot`, et ajoutez une condition "prompt for" `@Livraison` avec la question "A emporter ou sur place ?"
15. Ajoutez la réponse "Ok, j'ai donc une $number $boissons pour vous, $Livraison ! Cela vous va ?  "
16. Créez un noeud fils, avec le nom `validation`
17. Dans le champ `If assistant recognizes`, saissisez `@Validation`. Ce noeud attend une entité de validation.
18. En haut à droite du noeud, cliquez sur `Customize`
19. Activez les `Multiples Responses` et appuyez sur `Apply`. 
20. Dans le champ `If assistant recognizes`, ajouter `@Validation:Oui`, saissisez la réponse dans le champ `Respond with` suite à la confirmation.
21. Dans le champ `If assistant recognizes`, ajouter `@Validation:Non`, saissisez la réponse dans le champ `Respond with`
22. Cliquez `Add node`, and name it `prix-boisson`
23. Ajoutez votre intention `#prix-boisson` comme valeur dans le champ `If bot recognizes`. Cliquez sur le signe `+` et rajouter l'entité `boissons` avdec le séparateur `AND`.
24. En haut à droite du noeud, cliquez sur `Customize`
25. Activez les `Multiples Responses` et appuyez sur `Apply`. 
26. Dans le champ `If assistant recognizes`, ajouter `@boissons:Latte`, saissisez la réponse dans le champ `Respond with`
27. Rajoutez d'autres conditions comme `@boissons:espresso`, `@boissons:capuccino`
28. Vous pouvez ensuite faire de même pour la demande de localisation

Arborescence du dialogue :
![finished dialog](https://github.com/vperrinfr/Watson_Academy/blob/master/pictures/dialog.png)

Bravo, votre premier chatbot est en oeuvre :clap: :clap:

## Pour aller plus loin...
Vous avez fini et vous voulez tester d'autres fonctionnalités...

### Help - IBM Cloud Functions
Watson Assistant permet de faire des appels de programmation à des applications ou des services externes et renvoyer un résultat dans le cadre du traitement qui se produit au sein d'un échange de dialogue.
Dans cette exemple, nous allons exécuter un appel pour recuperer le prix de la commande.

1. Dans le sous-node de Validation, cliquer sur la roue dentée à coté de "Validation:Oui" 
2. Copier le code suivant dans le `JSON editor` (remplacer le contenu actuel)
```
{
  "context": {
    "private": {
      "function_credentials": {
        "user": "XXXXXXXX",
        "password": "YYYYYY"
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
      "name": "/vincent.perrin@fr.ibm.com_dev/vperrin/Watson_Pricing",
      "type": "server",
      "parameters": {
        "nombre": "$number",
        "boisson": "$boissons",
        "livraison": "$Livraison"
      },
      "credentials": "$private.function_credentials",
      "result_variable": "context.extraction"
    }
  ]
}
```
Lien vers le user/password de la IBM Cloud Function : [lien](https://ibm.ent.box.com/notes/353062724451)

3. Créer un sous-node à `commande-boisson` avec pour texte:
`La commande est faite, cela vous coutera $extraction.prix . Merci pour votre commande.`
4. Changer l'option de `And finally` pour utiliser "Jump To" au niveau du critère "Validation:Oui"

![finished dialog](https://github.com/vperrinfr/Watson_Academy/blob/master/pictures/JumpTo.png)

### Help - Digressions
Parfois, vous voudrez qu'une intention soit gérée, peu importe où se trouve l'utilisateur dans leur flux. Pensez aux digressions, ils vous permettent de répondre à une intention même si un utilisateur se trouve au milieu d'un flux de processus, puis de revenir à son flux précédent. Si votre utilisateur a besoin d'aide pour parler au bot n'importe où dans son bot, il est judicieux d'activer les digressions.
1. Créez l'intention `#Aide` avec des exemples comme "J'ai besoin d'aide"
2. Créez un noeud `Aide`à la racine.
3. Ajouter la condition `#Aide` : "
Je peux vous aider à commander un verre dans mon café. Il suffit de dire commander un café pour commencer!"
4. Aller dans la partie `Customize` du noeud
5. Cliquez sur l'onglet `Digressions` 
6. Activez `Return after digression` (Les digressions doivent être activées par défaut, ce paramètre vous permet de gérer l'intention puis de revenir au flux.)
7. Maintenant, pour tester cela, nous devons nous situer au milieu du flux de boissons de notre commande. Mais d’abord, puisque c’est un slot, il faut aller dans l’onglet `Digressions` du nœud `commande-boisson`
8. Activez `Allow digressions away while slot filling` et cliquez sur le bouton n'autorisant que les noeuds dont les retours sont activés. Cela vous aidera à contrôler les nœuds sur lesquels vous souhaitez autoriser l’écart.
9. Essayez-le en disant «commander un verre, puis, quand on vous le demandera ce que vous voulez boire, dites «Aide». Vous devriez voir une réponse de votre noeud d’aide avec un autre message de suivi pour la prochaine question de remplissage d’emplacement.

### Help - Contenu général

Les catalogues vous permettent d'ajouter facilement des intentions courantes à votre espace de travail de service Watson Assistant.

1. Cliquez sur `Content Catalog` dans la barre supérieur
2. Cliquez sur le terme `Général` pour voir les différentes intentions pré-entrainées de cette catégorie.
3. Cliquez sur `Add to skill` dans le coin supérieur droit de l'écran. L'ensemble des 10 intentions ont été rajouté à la liste de vos intentions.
4. Vous pouvez maintenant créer de nouveaux noeuds pour répondre aux questions liées à ces nouvelles intentions.




