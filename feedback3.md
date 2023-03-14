---

Author: Alain ORLUK / O'clock  
Formation : Développeur Web & Web mobile - PHP 
Lieu: Rosheim
Date : 19/03/2023  

---
# **Feedback de l'apprenant 3**

Lorsque j'ai installé ton projet, la page d'accueil affichait une erreur via la **view** `error404`.  
Dans ton code tu n'avais pas implémenté la route initiale, c'est-à-dire celle qui acceuille le visiteur et qui correspond à l'url racine de ton application sur le serveur, soit `http://localhost/`.  

Cette route doit toujours être déclarée et affichée par défaut lorsque ton application est servie.  
C'est la **homepage** de ton application.  

Ton implémentation via la view `home.tpl.php` stipulait la route `/home` en lieu et place de `/`.  
Rien de grave mais c'est balot 😁.  

## **Implémentation des setters**  

Dans tes setters, tu implémentes systématiquement un `return` de l'objet complet via `$this`.  
Afin d'optimiser les traitements et la consommation mémoire, je t'invite à ne renvoyer que ce dont tu as besoin et en l'occurence ici tu n'as qu'à renvoyer la propriété concernée par le setter :  

```php
return $this->firstname;
```

## **Routage dynamique**

Il semble que si tu l'as bien fait, tu n'as pas saisi l'utilité de déclarer les routes dans un projet.  
Cette implémentation permet de mapper dynamiquement les routes lors des redirection et notamment par l'emploi de la méthode `generate()` de l'objet `$router` du composant **AltoRouter** :  

```php
public function generate($routeName, array $params = [])
```

Dans ton projet tu gère les redirection en "**dur**" en désignant littérallement les fichiers html :  

```html
<a class="navbar-brand" href="../index.html">
```

Tu as parfaitement implémenté les routes dans ton fichier `index.php`.  
Tu aurais pu déplacer le code dans un script à part qu'il te suffisait d'importer afin de ne pas surcharger visuellement le script et de faciliter la maintenance de ton code, mais c'est un détail.  
N'oublie pas que plus un projet est segmenté et structuré, plus il est facile de se plonger dedans pour le maintenir plusieurs mois plus tard.  
Quoi de mieux que de maintenir 3 scripts de 30 lignes un par un plutôt qu'un script de 90 lignes😄.  

Autre chose conernant le routing.  

Dans la déclaration des routes dans ton script `index.php`, tu désignes la même route `/student/add` pour les 2 méthodes **HTTP** `GET` et `POST`.  
Tu exposes ainsi cette route à une méthode **GET** "accidentelle" qu'elle n'est pas censée implémenter puisqu'elle ne doit être utilisée QUE pour une opération d'écriture **POST**.  

Il est préférable d'être aussi précis que possible lors de la définition des routes.  
Ainsi tu augmentes la clarté et la compréhension du code et évites les comportement imprévus.  

## **Et pour finir sans oublier les bases…**

### **La sémantique et le référencement naturel**

Le code de ton fichier `home.tpl.php` :  

```html
<html lang="en">
```

Le contenu de ton application est en français.  
Pense donc à préciser la langue utilisée afin d'améliorer le référencement naturel de tes pages.  

Toujours pour le référencement naturel, au lieu d'utiliser la balise `<div>` en tant que conteneur principal n'hésite pas à utiliser une balise à la sémantique plus forte telle que `<main>`.  

```html
<main class="container my-4">
    …
</main>
```

Tu peux aller voir [ICI](https://developer.mozilla.org/fr/docs/Glossary/Semantics) pour plus d'informations concernant la sémanique au sein d'un document HTML😊.  
