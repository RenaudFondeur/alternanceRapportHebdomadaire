# alternanceRapportHebdomadaire

## contexte/reprise de mon stage de Licence 3

Je travail sur le projet Slang qui traduit le code de la machine virtuelle Pharo(VM) du Pharo au C en lui appliquant des tranformations et optimisations propre au VM (typage, inlining).

Le code obtenu est répartie entre plusieurs fichiers C selon la compilation et l'architecture (mac, windows, linux) ciblé allant jusqu'à + de 90000 de code, vu la taille et la complexité de la VM, je n'ai qu'une très vague idée des optimisations et transformations qui sont appliqué sur la VM. J'ai principalement touché lors de mon stage à la partie traduction de modèle de code Pharo vers C (priorité de l'exécutions des éléments, types et équivalence de syntaxe et du modèle objet en C)

## 4/11/2024 au 10/11/2024 

pricipalement une refamiliarisation avec les différentes étapes et classes de Slang et Pharo/le débugueur Pharo.

j'ai commencé dans cette optique à normaliser les variables sortis en C de la VM (les macro en majuscule, un _ devant les variables locales, les globales ne changent pas) et à faire les tests associés. Les variables sont introduite/remplacé dans la plupart des étapes de la traduction en C, c'était une idée de mon stage que je n'avais pas eu le temps de faire.

## 11/11/2024 au 17/11/2024

fin de la normalisation des variables mais pas des tests , résolutions de bugs avec l'aide de guille.

semaine courte : lundi férié et Dojo sur la VM mardi aprem 

## 18/11/2024 au 24/11/2024

j'ai trouvé en codant ces dernières semaine que dans l'AST générer par Slang depuis celui de Pharo, nil apparaît principalement comme un noeud de constant avec la valeur nil et très rarement comme un noeud de type variable avec le nom nil.

Les deux méthodes produisent une traduction équivalente mais toute les partie de Slang qui gèrent les variables ignorent celle qui ont le nom nil par un check, de plus nil n'apparaît pas dans la liste des locales à tout les coup et seul les tests les plus vieux de la VM utilisent la notation avec des noeuds de variables.
J'ai mis à jour la traduction d'AST, les tests et supprimé les checks inutiles

encore des bugs avec la normalisation, résolu avec Guille :

Certains plugin de la VM ne suivent pas les mêmes étapes que les autres fichiers (inférences de type plus tard), les étapes que j'ai rajouter sont sensé arriver après celle-ci. J'ai faire en sorte que la génération suivent les mêmes étapes que le reste de Slang

Les noms liés à des librairies sont supprimé des dictionnaire/set qui stocke les variables réservés lorsque le .h est inclut pour ne pas les redéfinir. On perd donc l'informations dans l'AST lorsque l'on normalise plus tard et l'on a jamais les dictionnaire/set complet contrairement à ce que je pensais. On a rajouté un attribut shoulBeGenerated au feuilles de l'AST concerné qui passent à false lorsque le .h est inclus au lieu de supprimeé l'information.

Egalement des petits bugs liés aux noms de variables de la partie C non générer de la VM partagé par la partie qui l'est (occurences a fix manuellement dans les fichiers C).

Test presque finie.

## 25/11/2024 au 01/12/2024

Fin de la normalisation des tests et pull request 

pair programming le mardi pour introduire des bases de polymorphisme dans la VM (changer de garbage collector pendant l'exécution)

## 02/12/2024 au 08/12/2024

début de la résolution des warnings self assign (x = x) dù au polymorphisme et à l'inlining.

l'idée est d'étendre le visiteur d'ast qui élimine le code mort pour prendre en compte ce cas, cela permet de supprimer le besoin de savoir d'ou vient l'erreur.

la plupart des self assign sont dans des do, while et if, ce sont des selecteurs qui ne sont pas pris en compte par l'élimination de code mort qui ne les considèrent pas comme des méthode sans effets de bord. j'ai donc commencer à les intégrer égalements.

début égalements des tests associés 

## 09/12/2024 au 15/12/2024

à l'université toute la semaine 

## 16/12/2024 au 22/12/2024

à l'université toute la semaine 

## 23/12/2024 au 29/12/2024

en vacances toute la semaine 

## 30/12/2024 au 05/01/2025

en vacances jusque jeudi 

continuation dans le fix des warnings liés au self assignement

## 06/01/2025 au 12/01/2025

fin du fix sur les self assign et pull request.

## 13/01/2025 au 19/01/2025

début de l'implémentation d'un algorithme d'élimination de code mort afin de compléter/remplacer celui que Slang a.
assistance à un séminaire sur la représentation graphique de session de débuguage.

## 20/01/2025 au 17/02/2025

implémentation de l'algorithme

adaptation au spécificité de Slang découverte au fur et à mesure:
 
Tout le code n'est pas un noeud dans l'AST, les définitions de variables locales et globales et certaines macros ne sont pas dedans.

Beaucoup de correction dans Slang de transformations rendant le parcours de l'AST incorrect (mauvaise mise à jour du parent, même noeud utilisé à plusieurs endroit).

Réfactor de la génération de fichiers pour séparer clairement les transformation d'AST et l'écriture de code C et déplacement du pass de l'algorithme juste avant l'émission de code dans un fichier.


Début de la migration de tous les tests vers le nouvel algorithme.


## 17/02/2025 au 23/02/2025

en vacances toutes la semaine

## 24/02/2025 au 02/03/2025

à l'université toute la semaine

## 03/03/2025 au 30/03/2025

Fin de la migration des tests + ajout de nouveaux.

Assitance à un séminaire sur l'IA.

Lorsqu'une globale est utilisé seulement par une méthode, sa définition est déplacé dans celle-ci.
Ajout de tests pour cette localisation.

Fix un bug avec un vieux check qui supprime certaines globales avec 0 référence ou après leur localisation, déplacement du check dans sa super class pour le lancer sur tout les fichiers et amélioration du check pour prendre en compte toute les globales.

Certaines globales sont utilisées seulement dans des macros définis en dur ou servent seulement à définir d'autres globales ou encore ne doivent pas être localisées.
Le check gardait ces variables localisées en double ainsi que celles qui ne semblent pas utilisées du fait de son bug.
Elles sont actuellement invisible pour Slang, on a donc besoin maintenant de marquer (en dur) leurs utilisation.

Séparation clair des globales qui seront mis dans des headers et celle qui sont propre à un fichier, l'ancien moyen de marquer une variable comme utile était de la déclarer en export.

Supression manuelle de code mort et macros dépréciés dans le code non généré + petit refactor simple de certaine macros (définition de locale dans la macro).
