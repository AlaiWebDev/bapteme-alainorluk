---

Author: Alain ORLUK / O'clock  
Formation : Développeur Web & Web mobile - PHP 
Lieu: Rosheim
Date : 19/03/2023  

---
# **Feedback de l'apprenant 4**

Une fois installé, l'absence de l'implémentation du `MainController` engendre une erreur lorsque l'application est servie.  

Voyons quelques uns des points qui je pense nécessitent une reprise dans les prochains jours.  

## **la POO et Polymoprhisme & l'Abstraction**

Comme l'un(e) de tes camarades, tu sembles encore incertain(e) dans ta compréhension de ces 2 concepts.  

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

Ailleurs dans le projet j'ai rencontré le code suivant :  

```php
public function students() {

        // VERIFICATION QUE L'UTILISATEUR AIT LES DROITS
        $studentsModel = new Students
        students();
        $students = $studentsdModel->findAll();

        $this->show("students/students_list", [
            'students' => $students
        ]);
        
    }
```

Première petite chose qui poursuit chaque développeur tout au long de sa carrière, vérifier ce que l'on écrit :  
`$studentsModel` est différent de `$studentsdModel`.  
C'est le genre de coquille qui te fait perdre du temps et te fait parfois tourner en bourrique si tu ne la détecte pas rapidement😂.  

Ensuite tu créés une instance de la classe Students et tu appelles la méthode `findAll()` par référence à cette instance.  
Hors dans la définition de ta classe `Students`, tu as déclaré la méthode `findAll()` en tant que ***constante*** avec le mot-clé `static`.  
Tu n'as donc pas à instancier la classe `Students`. Il te suffit d'utiliser la syntaxe des **constantes** :  

```php
public function students() {

        // VERIFICATION QUE L'UTILISATEUR AIT LES DROITS
        $students = Students::findAll();

        $this->show("students/students_list", [
            'students' => $students
        ]);
        
    }
```

## **La sécurité**

Voici le code que tu as implémenté pour l'update d'un prof :  

```php
$sql = "
            UPDATE `teacher`
            SET

            firstname = '{$this->firstname}',
            lastname = '{$this->lastname}',
            job = '{$this->job}',
            status = '{$this->status}',
                updated_at = NOW()
            WHERE id = {$this->id}
        ";
```

Ici tu n'as utilisé aucune méthode de sécurisation de l'opération d'écriture sur la BDD.  
Je t'invite à jeter un oeil à la correction et notamment à l'implémentation des requêtes préparées qui protègent tes scripts des injections SQL (Voir [ICI](https://www.php.net/manual/fr/pdostatement.execute.php)).  
Idem concernant le traitement des chaînes d'input envoyées via le formulaire.  
Tu pourrais envisager d'ajouter un petit quelque chose à ton code :  

De :

```php
$firstname = filter_input(INPUT_POST, 'firstname');
```

À :  

```php
$firstname = filter_input(INPUT_POST, 'firstname', FILTER_SANITIZE_SPECIAL_CHARS);
```

Je te laisse aller voir sur la [documentation officielle PHP](https://www.php.net/manual/en/filter.filters.sanitize.php) la description des filtres de "sanitization" si tu n'es pas trop au fait des options disponibles.  

## **Implémentation des setters**  

Dans tes setters, tu implémentes systématiquement un `return` de l'objet complet via `$this`.  
Afin d'optimiser les traitements et la consommation mémoire, je t'invite à ne renvoyer que ce dont tu as besoin et en l'occurence ici tu n'as qu'à renvoyer la propriété concernée par le setter :  

```php
return $this->firstname;
```

## **Routage dynamique**

Dans la déclaration des routes dans ton script `index.php`, tu désignes la même route `/students/add` pour les 2 méthodes **HTTP** `GET` et `POST`.  
Tu exposes ainsi cette route à une méthode **GET** "accidentelle" qu'elle n'est pas censée implémenter puisqu'elle ne doit être utilisée QUE pour une opération d'écriture **POST**.  

Il est préférable d'être aussi précis que possible lors de la définition des routes.  
Ainsi tu augmentes la clarté et la compréhension du code et évites les comportement imprévus.  
  
Dans ton projet, tu n'appliques aucune convention de nommage personnelle :  

```php
'Students_update'
'main-home'
'teachers_delete'
```

Il est impératif soit de respecter les conventions couramment admises, soit de définir des convetions qui s'appliqueront sur le projet, via une section dédiée au sein de la documentation interne.  
Par ailleurs il est beaucoup plus facile d'implémenter un code qui "*parse*" une route donnée en paramètre quand toutes les routes respectent la même convention de nommage.  

En outre lors de la déclaration des routes, tu as implémenté le code suivant :  

```php
$router->map(
    'GET||POST',
    '/students/update/[i:studentid]',
    [
        'method' => 'studentsUpdate',
        'controller' => StudentsController::class // On indique le FQCN de la classe
    ],
    'Students_update'
);
```

Ici tu as nommé la route `Students_update`.  
Les conventions de nommage s'accordent à affubler la première lettre en majuscules sur les noms de classes (***PascalCase***).  

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
