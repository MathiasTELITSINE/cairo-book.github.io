# Introduction

## Qu'est-ce que Cairo ?

Cairo est un langage de programmation conçu pour un processeur virtuel du même nom. L'aspect unique de ce processeur est qu'il n'a pas été créé pour les contraintes physiques de notre monde, mais pour des contraintes cryptographiques, ce qui lui permet de prouver efficacement l'exécution de n'importe quel programme s'exécutant sur celui-ci. Cela signifie que vous pouvez effectuer des opérations longues sur une machine en laquelle vous n'avez pas confiance, et vérifier le résultat très rapidement sur une machine moins coûteuse. Alors que Cairo 0 était directement compilé en CASM, l'assembleur du processeur Cairo, Cairo 1 est un langage plus élevé. Il se compile d'abord en Sierra, une représentation intermédiaire de Cairo qui se compile ensuite en une sous-ensemble sûr de CASM. L'objectif de Sierra est de garantir que votre CASM sera toujours vérifiable, même en cas d'échec de calcul.

## Que pouvez-vous faire avec ?

Cairo permet de calculer des valeurs fiables sur des machines non fiables. Un cas d'utilisation majeur est Starknet, une solution de scalabilité pour Ethereum. Ethereum est une plateforme de blockchain décentralisée qui permet la création d'applications décentralisées où chaque interaction entre un utilisateur et une application décentralisée est vérifiée par tous les participants. Starknet est une seconde couche construite au-dessus d'Ethereum. Au lieu de demander à tous les participants du réseau de vérifier toutes les interactions des utilisateurs, seul un nœud, appelé le "prover", exécute les programmes et génère des preuves que les calculs ont été effectués correctement. Ces preuves sont ensuite vérifiées par un contrat intelligent Ethereum, ce qui nécessite une puissance de calcul considérablement inférieure par rapport à l'exécution des interactions elles-mêmes. Cette approche permet d'augmenter le débit et de réduire les coûts des transactions tout en préservant la sécurité d'Ethereum.

## Quelles sont les différences avec d'autres langages de programmation ?

Cairo est assez différent des langages de programmation traditionnels, notamment en ce qui concerne les coûts supplémentaires et ses avantages principaux. Votre programme peut être exécuté de deux manières différentes :

- Lorsqu'il est exécuté par le "prover", le langage Cairo est similaire à n'importe quel autre langage. En raison de sa virtualisation et du fait que les opérations n'ont pas été spécifiquement conçues pour une efficacité maximale, cela peut entraîner une surcharge de performance, mais ce n'est pas la partie la plus pertinente à optimiser.

- Lorsque la preuve générée est vérifiée par un vérificateur, le processus est un peu différent. Cette vérification doit être aussi peu coûteuse que possible, car elle peut éventuellement être effectuée sur de nombreuses machines très petites. Heureusement, la vérification est plus rapide que le calcul, et Cairo présente quelques avantages uniques pour l'améliorer encore davantage. L'un de ces avantages notables est le non-déterminisme. Ce sujet sera abordé en détail ultérieurement dans ce livre, mais l'idée est que vous pouvez théoriquement utiliser un algorithme différent pour la vérification que pour le calcul. Actuellement, l'écriture de code non déterministe personnalisé n'est pas prise en charge pour les développeurs, mais la bibliothèque standard tire parti du non-déterminisme pour améliorer les performances. Par exemple, trier un tableau dans Cairo coûte le même prix que le copier. En effet, le vérificateur ne trie pas le tableau, il vérifie simplement s'il est trié, ce qui est moins coûteux en termes de calcul.

Un autre aspect qui distingue le langage est son modèle de mémoire. Dans Cairo, l'accès à la mémoire est immuable, ce qui signifie qu'une fois qu'une valeur est écrite en mémoire, elle ne peut pas être modifiée. Cairo 1 propose des abstractions qui aident les développeurs à travailler avec ces contraintes, mais il ne simule pas entièrement la mutabilité. Par conséquent, les développeurs doivent réfléchir attentivement à la gestion de la mémoire et des structures de données dans leurs programmes pour optimiser les performances.

## Références

- Architecture du processeur Cairo : <https://eprint.iacr.org/2021/1063>
- Cairo, Sierra et Casm: <https://medium.com/nethermind-eth/under-the-hood-of-cairo-1-0-exploring-sierra-7f32808421f5>
- État de non-déterminisme : <https://twitter.com/PapiniShahar/status/1638203716535713798>
