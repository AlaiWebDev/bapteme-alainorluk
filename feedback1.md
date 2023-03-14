---

Author: Alain ORLUK / O'clock  
Formation : Développeur Web & Web mobile - PHP 
Lieu: Rosheim
Date : 19/03/2023  

---
# **Feedback de l'apprenant 1**

Bravo, le projet le plus abouti, même si tu pourrais aller plus loin 😁  
Voyons cela justement…

## **Préambule**

Au sein de ton projet, lorsque je me connecte sous l'utilisateur ***Helper***, je n'ai pas accès à la liste des profs.  
Pourtant l'énoncé stipulait le contraire.  
Avant d'aller plus loin dans ton feedback, je tenais à te sensibiliser à l'importance de bien lire l'énoncé.  
Vois-le comme le cahier des charges qu'a validé ton client ou le descriptif de la tâche qui t'as été transmis.  
Important de bien le respecter donc😉.  

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

## **La POO et les méthodes statiques**

Dans ton projet tu as déclaré `static` la méthode `find()` du modèle `Teacher` (ainsi que d'autres d'ailleurs).  

Ce qui veut dire que tu la rends accessible non pas depuis une instance de la classe `Teacher` mais par référence à la classe dans laquelle elle est déclarée en tant que **constante**.  

Et tu dois connaître maintenant la syntaxe relative aux constantes :  
<details>
  <summary><em><b>Syntaxe…</b></em></summary>

>
```php
Class::method();
```

Donc lorsque tu appelles la méthode `find()` dans ton `Controller`, au lieu d'écrire :  

```php
$newteacher = new Teacher();
$teacherFromDb = $newteacher->find($teacherid);
```

Tu devrais économiser une instance de `Teacher` et écrire :  

```php
$teacherFromDb = Teacher::find($teacherid);
```

</details>

## **Gestion de la session**

Pour aller plus loin, tu pourrais te pencher sur la sécurisation des ID de session lors du démarrage et lors des tests de continuité.  
Voir notamment le paramètre `use_strict_mode` de la fonction `session_start()`.  
Voici ce que dit la documentation officielle PHP :  
>Bien que l'activation de `session.use_strict_mode` soit obligatoiree pour la sécurité des sessions, cette directive est désactivée par défaut.
>
>Ce mode évite que le module de session utilise un identifiant de session non initialisé. Dit différemment, le module de session ne va accepter que les identifiants de sessions valides générés par le module de session. Il va rejeter tous les identifiants de session fournis par les utilisateurs.
>
>En raison de la spécification des cookies, les attaquants sont capable de placer des cookies contenant les identifiants de sessions en configurant localement une base de données de cookie ou par injections Javascript. `session.use_strict_mode` peut éviter qu'un attaquant n'initialise un identifiant de session.

La documentation se trouve [ICI](https://www.php.net/manual/en/session.security.ini.php).  

Exemple :  

```php
session_start([
    'use_only_cookies' => 1,
    'use_strict_mode' => 1,
    'cookie_lifetime' => 0,
    'cookie_secure' => 1,
    'cookie_httponly' => 1
  ]);
```

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
