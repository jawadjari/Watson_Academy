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

## Pour aller plus loin...
Vous avez fini et vous voulez tester d'autres fonctionnalités...

### Help - IBM Cloud Functions


### Help - Digressions
Sometimes, you will want an intent to be handled no matter where the user is in their flow. Think of Digressions as a global 'manage handlers': they allow you to respond to an intent even if a user is in the middle of a process flow, and then it allows them to return to their prior flow. If your user wants some help talking to the bot anywhere in your bot, this is a good intent to have digressions enabled.
1. Create a `#help` intent with examples like: "I need help"
2. Create a node below your `Order Drink` node
3. Add the condition of `#help` with a response like: "I can help you order a drink from my coffee shop. Just say order drink to get started!"
4. Go into the `Customize` portion of the node by clicking in the upper right
5. Click on the `Digressions` tab
6. Enable `Return after digression` (Digressions should be on by default, this setting allows you to handle the intent and then return back to the flow)
7. Now to test this out, we need to get in the middle of our order drink flow. But first, since it is a slot, we need to go into the `Digressions` tab in the `Order Drink` slots node
8. Turn on `Allow digressions away while slot filling` and click the button that only allows nodes with returns enabled. This will help you to control which nodes you want to allow to digress to
9. Try it out by saying "order drink", then when asked for what kind of drink you want, say "help". You should see a response from your help node with another follow up message for the next slot filling question
