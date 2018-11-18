# Tutorial Pas à Pas

# Comment construire un agent conversationnel 
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
7. Attendez que le bot termine son entraînement, puis tapez `Pourrais-je commander un expresso`. Le bot doit classifier cette phrase `commande-boisson`. Même si vous n’avez pas appris l’intention sur cette phrase exacte, Watson peut toujours la comprendre.
8. Ajoutez quelques intentions supplémentaires pour couvrir d'autres demandes. Essayez de créer les intentions suivantes et d’ajouter quelques exemples à chacune d’elles
  - #voir-produits (Le client veut connaitre les produits disponibles)
  - #prix (Le client veut connaitre le prix des produits)
  - #recherche_magasin (Le Client cherche la localisation des magasins)
  
Voila les intentions finies:
![finished intents](https://github.com/desmarchris/think-lab/blob/master/pictures/finished-intents.png)

## Création des Entities
1. Cliquez sur l'onglet `Entities`
2. Cliquez `Add entity` and add the name `boisson`
3. Activez `Fuzzy Matching` si vous souhaitez que Watson comprenne les erreurs de frappe.
4. Add a value `coffee` with the synonym of `cafe`. 
5. Add some additional values that you allow your users to order and any synonyms, for example:
  - espresso
  - cappuccino
  - latte
  - tea
6. Quittez la page, et cliquez sur `System entities` dans l'onglet `Entities` 
7. Activez`@sys-number` pour permettre la détection des nombres.

Voici l'entité `@boisson` finalisée:
![finished entity](https://github.com/desmarchris/think-lab/blob/master/pictures/finished-entity.png)

## Building Dialog
1. Click on the `Dialog` tab at the top of the page
2. Click `Create`
3. Click on the `Welcome` node if you would like to change the intro message
4. Click `Add node`, and name it `Greetings`
5. Add your `#greetings` intent as the field for `If bot recognizes`
6. Fill in a response that says something like "Hi! How can I help you today?"
7. Create two more nodes for `#thanks` and `#see-menu` and add responses
8. Create another node and name it `Order Drink`
9. To the right of the name, click on `Customize`
10. Turn on `Slots` and hit `Apply`
11. Add the intent `#order-drink`to `If bot recognizes`
12. Under `Check for`, add the entity `@drink`
13. Under `If not present, ask` add a question like "What would you like to drink?"
14. Click `Add slot`, and add a condition and prompt for `@sys-number`: "How many cups of $drink would you like?" (Note: the syntax `$variable` is short hand for accessing Context variables. Context variables allow you to pass information between your application and Conversation.)
15. Add in the response, "Ok, I have $number $drink coming right up!"

Finished dialog tree with `Order Drink` open:
![finished dialog](https://github.com/desmarchris/think-lab/blob/master/pictures/finished-dialog.png)

## Test in Slack
Now that you have a functioning bot, let's do a quick deploy to see it working in a channel (Slack). You must be an administrator to add a bot. To create a Slack workspace, follow this tutorial: https://get.slack.help/hc/en-us/articles/206845317-Create-a-Slack-workspace
1. Click on the `Deploy` tab from the left nav bar (it's the second icon)
2. Under the Slack card, click `Deploy`
3. Click `Test in Slack` (Note: this uses an IBM hosted app and is meant for testing. To actually deploy this to Slack, you need to use `Deploy to Slack App`)
4. Click on `Authorize Slack`
5. If you are sent to slack.com and see an error about adding a workspace, make sure you are an administrator or click in the top right to sign into your own workspace
6. Once in Slack, click `Authorize` to give access to your bot. If done correctly, you will be brought back to the WA tooling and see your Slack workspace has been authorized.
7. Go into your Slack workspace (I use the web app), and rather invite the bot to a channel or find them on direct message as `@ibmwatson_bot`
8. Say hello! (If it seems like the bot isn't replying and you are in a channel, try mentioning the bot by first typing `@ibmwatson_bot` followed by your message)

![finished bot](https://github.com/desmarchris/think-lab/blob/master/pictures/finished-bot.png)

## If you want more...
Did you finish the above and want to learn more? Try some of the following methods to bolster your CoffeeBot.

### Resetting context
If your user orders a drink and completes the flow, and they try to make another order, the values found from the first flow will still be there so they will not be able to order something else. To fix this, we need to clear the context after a successful order so the values are not stored for the next order.
1. Create a node above the Slots node `Order Drink` called `Order Drink - Clear Context`
2. Set the condition to `#order-drink`
3. In the response section, click on the three button menu on the right and click on `Open context editor`
4. Fill in both of the variables (`drink` and `number`) and set the values to `null`
5. Click on the three dot menu on the right side of original Slots node `Order Drink`, and select `Move`. Then, click the new context clearing node and move to `As Child Node` (So, the parent node is the context clearing node, and the slots node is the child)
6. Go to the section called `And finally` at the bottom of the context clearing node. Select `Jump to` and click the slots node, then `If bot recognizes condition`
7. Change the condition of the slots node from `#order-drink` to `true` (Use this condition if you want the node to always fire)
8. Try it out! Without clearing the try it out panel, order a drink. Once finished, try ordering another drink and it should prompt you for the two needed variables again. Here's what the finished context clearing node will look like:
![clear context](https://github.com/desmarchris/think-lab/blob/master/pictures/clear-context.png)

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
