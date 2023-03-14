---

Author: Alain ORLUK / O'clock  
Formation : Développeur Web & Web mobile - L'API Fetch de JavaScript
Lieu: Rosheim
Date : 19/03/2023  

---
# **le conseil technique avancé à un.e apprenant.e**

## **La méthode `fetch()` de l'API Fetch de JavaScript**

La méthode JavaScript `fetch()` permet d'envoyer une requête HTTP à un serveur web (local ou distant, donc aussi bien vers notre très cher `http://localhost/` que vers l'API d'un serveur tel que `https://jsonplaceholder.typicode.com/todos`).  

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
