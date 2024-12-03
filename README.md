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

