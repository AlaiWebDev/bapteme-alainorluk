---

Author: Alain ORLUK / O'clock  
Formation : Développeur Web & Web mobile - L'API Fetch de JavaScript
Lieu: Rosheim
Date : 19/03/2023  

---
# **Le conseil technique avancé à un.e apprenant.e**

## **La méthode `fetch()` de l'API Fetch de JavaScript**

La méthode JavaScript `fetch()` permet d'envoyer une requête HTTP à un serveur web (local ou distant, donc aussi bien vers notre très cher `http://localhost/` que vers l'API d'un serveur tel que `https://jsonplaceholder.typicode.com/todos`).  
Elle est souvent utilisée pour récupérer des données à partir d'une API ou pour envoyer des données à un serveur.

D'ailleurs vous devez savoir maintenant que lorsque vous tapez une url dans la barre d'adresse de votre navigateur préféré, vous envoyez également une requête au serveur qui héberge la ressource demandée via l'URL.  

Faites le test.  
Ouvre votre navigateur et dans la barre d'adresse, tapez `https://jsonplaceholder.typicode.com/todos` puis appuyez sur **ENTRÉE**.

Voici ce que vous devriez voir s'afficher sur la page renvoyée via la réponse du serveur :  

![La réponse à la requête](/assets/img/json_placeholder_result.png)

Vous avez donc envoyé une requête au serveur qui héberge la ressource liée à l'URL et le serveur vous a renvoyé la réponse qu'il est "programmé" pour renvoyer.  

À présent ouvrez un nouvel onglet sur votre navigateur puis déployez les outils de développement via ***CTRL + MAJ + J*** (sous windows) ou ***Cmd + Option + C*** (sur Mac).  
Dans la partie haute des outils, cliquez sur l'onglet **Réseau**.  
Maintenant re tapez `https://jsonplaceholder.typicode.com/todos` dans votre barre d'adresse puis **ENTRÉE**.  
Vous devriez voir une requête du nom de `todos` dans la colonne "*Nom*".  
Enfin en cliquant sur l'onglet "*Aperçu*" à droite, vous devriez voir quelque chose comme cela :  
![Les informations réseau de la requête](/assets/img/json_placeholder_result_reseau.png)

Nous voyons que la page web qui a été affichée en réponse à notre requête `https://jsonplaceholder.typicode.com/todos` n'est que la retranscription du contenu de la réponse du serveur, contenu renvoyé en tant qu'objet JavaScript, au format **JSON** (**J**avaScript **O**bject **N**otation).  

Et bien la méthode `fetch()` permet de faire exactement la même chose à la différence près que vous allez pouvoir recueillir la réponse du serveur (OK, 404, 403, 500, …) ainsi que le contenu de ce qu'il a renvoyé, ici un objet JSON.  

Maintenant nous allons arrêter de jouer à l'utilisateur de base et nous allons remettre notre costume de développeur.  

À la racine de votre dossier de travail, créez un dossier que vous nommerez `exemple-fetch`.  
Ouvrez ce dossier avec votre éditeur de code préféré.  
Créez un fichier `index.html` ainsi qu'un sous-dossier `assets/js` que vous nommerez `fetch.js`.  
Implémentez l'appel au script JS dans l'`index.html`.  
Dans le script JS, ajoutez la ligne :  

```js
console.log("Script lié");
```

Sauvegardez le tout puis démarrez un serveur local pour charger ce petit projet.  
Une fois la page ouverte, vérifiez que le message `Script lié` s'affiche bien dans la console des outils de développement (faîtes un refresh/F5 si nécessaire).  

C'est bon, tout est en ordre ?  

Magnifïïïïque comme dirait l'autre 😄.  

Vous pouvez supprimer la ligne du console.log.  

Nous allons à présent effectuer la même requête que précédemment dans le navigateur mais cette fois-ci nous allons l'envoyer par le biais de la fonction `fetch()` via notre script JS.  

Ajoutez ces lignes dans le script, nous allons les détailler :  

```js
fetch('https://jsonplaceholder.typicode.com/todos')
  .then(response => response.json())
  .then(data => {
    console.log("Voici les données récupérées : \n", data);
    console.log("Voici le contenu de l'objet du premier élément du tableau d'objets : \n", "Id : " ,data[0].id, "\n", "Title : ", data[0].title, "\n", "Completed : ", data[0].completed);
    }
    )
  .catch(error => console.error("Oups, erreur détectée : \n", error));
```

Dans cet exemple, nous appelons la fonction `fetch()` avec l'URL de l'API que nous voulons interroger. `fetch()` renvoie une ***promesse*** ou ***promise*** pour les anglophones  (qui est un objet qui fera certainement… l'objet😁 d'une séance avec votre formateur) qui représente la réponse du serveur.  

J'utilise ensuite la méthode `then()` pour traiter la réponse du serveur. La méthode `then()` prend une fonction de rappel (**callback**) qui sera appelée lorsque la réponse du serveur sera disponible. Dans cet exemple, on appelle la méthode `json()` sur la réponse pour extraire les données JSON de la réponse. Ensuite, on affiche les données dans la console.  

Si quelque chose se passe mal lors de la récupération des données, on peut utiliser la méthode `catch()` pour traiter l'erreur. Dans cet exemple, j'affiche dans la console un petit message personnalisé et puis le libellé de l'erreur.  

Il est important de noter que la méthode `fetch()` ne gère pas les erreurs HTTP comme les codes 404 ou 500. Elle traite uniquement les erreurs réseau ou de communication. Si on souhaite gérer les erreurs HTTP, il faut vérifier le code de statut HTTP de la réponse dans la méthode `then()`.  

L'API `fetch()` prend en charge plusieurs options pour personnaliser les requêtes. Par exemple, on peut spécifier la méthode HTTP utilisée pour la requête (GET, POST, PUT, DELETE, etc.), les en-têtes HTTP, les données à envoyer avec la requête, et encore many more stuff😆.  

Voici un exemple d'utilisation de l'API `fetch()` avec des options personnalisées :  

```js
fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ title: 'Mon titre',
  body: 'Le blabla',
  userId: 25, })
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("Oups, erreur détectée : \n", error));
```

Dans cet exemple, j'envoies une requête POST avec des données JSON en utilisant l'option `body`. Je précise également l'en-tête "Content-Type" pour indiquer au serveur que nous envoyons des données JSON.  
Par contre la requête n'est pas réellement exécutée sur le serveur (ben oui les gens qui gère l'API vont pas VRAIMENT laisser n'importe qui écrire sur leur BDD😂). Mais le serveur check tout de même la conformité de la requète et envoie une réponse, en l'occurence le statut 201 et un rappel des données envoyées.  
