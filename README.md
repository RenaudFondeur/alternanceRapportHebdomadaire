# alternanceRapportHebdomadaire

## contexte/reprise de mon stage de Licence 3

Je travail sur le projet Slang qui traduit le code de la machine virtuelle Pharo(VM) du Pharo au C en lui appliquant des tranformations/optimisations propre au VM (typage, inlining).

Le code obtenu est répartie entre plusieurs fichiers C selon la compilation et l'architecture (mac, windows, linux) ciblé allant jusqu'à + de 90000 de code, vu la taille et la complexité de la VM, je n'ai qu'une très vague idée des optimisations et transformations qui sont appliqué sur la VM. J'ai principalement touché lors de mon stage à la partie traduction de modèle de code Pharo vers C (priorité de l'exécutions des éléments, types et équivalence de syntaxe/du modèle objet en C)

## 4/11/2024 au 10/11/2024 

pricipalement une refamiliarisation avec les différentes étapes/classes de Slang et Pharo/le débugueur Pharo.

j'ai commencé dans cette optique à normaliser les variables sortis en C de la VM (les macro en majuscule, un _ devant les variables locales, les globales ne changent pas) et à faire les tests associés. Les variables sont introduite/remplacé dans la plupart des étapes de la traduction en C, c'était une idée de mon stage que je n'avais pas eu le temps de faire.

## 11/11/2024 au 17:11/2024

