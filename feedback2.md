---

Author: Alain ORLUK / O'clock  
Formation : Développeur Web & Web mobile - PHP 
Lieu: Rosheim
Date : 19/03/2023  

---
# **Feedback de l'apprenant 2**

J'ai remarqué que tu sembles avoir des difficultés avec certains concepts de la Programmation Orientée Objet et notamment le polymorphisme et l'abstraction.  
C'est peut-être pour cette raison que tu n'as pas pu aller très loin dans l'implémentation de ce que l'énoncé demandait.  
Tu n'as notamment implémenté aucune méthode d'écriture.  
Problème de temps ou inconfort dans la conception ?  
N'hésite pas à m'en dire plus si tu le souhaites et nous mettrons en oeuvre une action si nécessaire.  

Jetons un oeil au code que tu as rendu et notamment à la classe `CoreModel` :  

```php
abstract public static function find($id);
abstract public static function findAll();
```

Ici dans une classe mère **abstraite**, `CoreModel`, tu déclares une méthode `statique`que tu déclares également **abstraite**.  

Hors ces 2 implémentations sont *mutuellement exclusives*.  

En effet en déclarant une méthode en `static`, tu rends impossible son héritage et sa redéfinition par des classes filles.  
Une méthode déclarée `abstract` signifie qu'elle devra être surchargée au sein d'une classe fille. Ce qui encore une fois est rendu impossible par la déclaration `static`.  

Et cela t'oblige à contourner le problème en déclarant la méthode `findAll()` en tant que `static` au sein de la classe fille et donc de ne pas utiliser l'héritage, ce qui est contraire au concept de la POO, surtout que la classe `Student` est censée **hériter** les méthodes et propriétés de sa classe mère `CoreModel`.  
Sans ce contournement, tu aurais dû coder ton appel à la méthode `findAll()` ainsi :  

```php
$newstudent = new Student();
$studentsList = $newstudent->findAll();
```

Et tu aurais alors reçu un message comme celui-ci :  
> `Cannot make static method…`

## **Et pour finir sans oublier les bases…**

### **La sémantique et le référencement naturel**

Le code de ton fichier `header.tpl.php` :  

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
