---

Author: Alain ORLUK / O'clock  
Formation : Développeur Web & Web mobile - L'API Fetch de JavaScript
Lieu: Rosheim
Date : 15/03/2023  

---
# **Retour sur un MCD d’un.e apprenant.e**

Tout d'abord constatons que le dictionnaire de données sous forme de d'énoncé est pour le moins concis.  
Gageons qu'il est sujet à questionnement et qu'en situation professionnelle il devra être complété consécutivement à une collecte d'informations plus conséquente auprès du client.  

Toujours est-il que nous n'avons que cela pour concevoir le Modèle conceptuel de données.  
D'ailleurs voici ma version :  
![Ma version](/assets/img/MCD.png)

Laisse moi t'expliquer ma vision.  
À cette fin, décortiquons l'énoncé afin d'identifier les associations :  

1. **Un utilisateur peut créer une ou plusieurs adresses de livraisons**  
    Déjà ici nous pouvons nous questionner et tenter de trouver une réponse cohérente ou, à défaut, faire des choix qui seront validés ou non lors du retour au client.  
    - Le compte client contient-il obligatoirement au moins une adresse (par défaut dite de "facturation") ?  
        Personnellement je pense que c'est effectivement le cas.  
        Sans cela il faudrait ajouter une étape lors du parcours d'achat, ce qui n'est jamais une bonne idée. Un client trop impatient n'irait pas au bout.  
        Donc pour moi un client a au moins 1 adresse renseignée sur son compte, celle qui lui a été demandée lors de son inscription.  
    - Doit-on donc faire la distinction entre adresse de faturation et de livaison dans la modélisation ?  
        Je ne pense pas.  
        Qu'est ce qui différencie une adresse de livraison d'une adresse de facturation ? À mon sens c'est le client qui crééra cette association au moment de l'achat.  
        Pour autant il peut être judicieux de "flagger" une adresse de façon à l'identifier comme adresse de livraison "possible". Cela pourrait s'avérer utile lors d'un achat via le préremplissage d'une adresse de livraison par défaut par exemple.  
2. **Un utilisateur peut "liker" un ou plusieurs produits**  
    Ici tu as indiqué des cardinaltés en 1,1 de chaque côté de l'association.  
    C'est un cas très rare et le plus souvent il résulte d'une mauvaise interprétation du dictionnaire. Dans la plupart des cas les associations de cardinalités 1,1 de chaque côté indiquent que les 2 entités sont étroitement liées et qu'elles pourraient probablement être fusionnées en une seule.  
    Ici ce n'est clairement pas le cas puisqu'il n'existe pas de lien strcturel étroit entre un utilisateur et un produit.  
    D'ailleurs l'énoncé dit bien qu'un utilisateur peut "liker" 1 ou **plusieurs** produits. Le maximum de produit qu'un utilisateur peut liker est donc n. Et le minimum est 0 puisque rien n'oblige un utilisateur à liker un produit.  
3. **Un utilisateur peut passer commande d'un produit**  
    Ce n'est pas explicitement dit mais il est correct de définir l'association comme "un utilisateur peut commander 0 ou n produits". On ne l'oblige pas à commander et on ne limite pas le nombre de produits qu'il peut commander (Hé oh, le client doit gagner de l'argent aussi, y'a pas que toi🤣).  

Voyons maintenant le MCD que je t'ai proposé.  

## **Détail des associations**

### **Association USER<->ADDRESS**

Comme tu le vois j'ai défini l'association **USER<->ADDRESS** tel que "Un utilisateur est lié à une ou plusieurs adresse" et une adresse est liée à 1 et un seul utilisateur.  J'ai ajouté la propriété `shipment` dans l'entité **ADDRESS**.  
Cela permettra comme je te l'ai expliqué plus haut de pré-remplir notamment les formulaires lors d'une procédure d'achat. Cela permet également de formaliser l'association **ORDER<->ADDRESS**.  
En effet une commande doit disposer de l'adresse du client, que ce soit pour l'envoi d'une facture papier ou la désignation d'une adresse de livraison.  

Nous retrouvons l'association exprimée dans l'énoncé par *Un utilisateur peut "liker" un produit ou plusieurs*.  

### **Association USER<->PRODUCT**

Dans cette association tu constates que selon mon interprétation, les cardinalités sont de type x,n de chaque côté.  
Une des règles communémment admise dans l'interprétation d'une association est qu'une cardinalité de type x,n de part et d'autre sera porteuse de données.  
Dans le cas présent la donnée portée évoque le nombre "like" qu'un utilisateur a placé tout autant que le nombre de "like" qu'a reçu un produit.  

### **Association PRODUCT<->ORDER**

Comme pour l'association précédente, nous avons une cardinalité x,n de chaqque côté et donc une donnée portée qui évoque le nombre de produits par commande.  

### **Association USER<->ORDER**

Rien à dire ici, tu as parfaitement représenté l'association entre les 2 entités, à savoir qu'un utilisateur peut générer 0 ou n commandes et qu'une commande est liée à 1 utilisateur et un seul.  

Pour finir, sache que tu trouveras sur le net un certain nombre de plateforme web et d'applications pour faciliter la conception d'un MCD.  

Personnellement celles que j'utilise le plus régulièrement sont :  

- [MoCoDo](https://www.mocodo.net/)
- [Lucidchart](https://www.lucidchart.com/pages/fr/exemple/logiciel-mcd)
- L'application Paint sous Windows. Ou bon ok c'est loin d'être facile et agréable 😜
- Une simple feuille de papier. Ben oui quoi, si c'est juste pour vous c'est suffisant, non ?😊

***Note à l'intention des correcteurs O'clock :***  
Je n'ai pas utilisé ces applis car d'une part je ne me souviens plus de la syntaxe sur **MoCoDo** (plus utilisé depuis…pff, au moins !😁) et que j'ai résilié mon abonnement à **Lucidchart** pour l'instant.  
Je me suis donc servi de Paint 😄.  
