## Étape 1 :



1\. Une machine virtuelle est un ordinateur créé à l’intérieur d’un autre ordinateur grâce à un logiciel. Elle permet d’utiliser un système d’exploitation comme Windows ou Linux sans avoir besoin d’un deuxième ordinateur.



2\. 

* Elle permet d’utiliser plusieurs systèmes d’exploitation sur un même ordinateur.
* Elle permet de faire des tests sans risquer d’endommager l’ordinateur principal.



3\. Quand on travaille directement sur son ordinateur, on utilise son système principal. Dans une machine virtuelle, on utilise un ordinateur simulé par un logiciel, avec son propre système. Cela permet de faire des tests et des expériences sans modifier directement l’ordinateur principal.



## Étape 2 : 



1. Un conteneur Docker est une manière d'émuler un environnement isolé avec ses propres accès aux application mais qui partage l'OS ainsi que les ressources physique avec la machine hôte.



2\. La différence fondamentale entre un conteneur et une VM est le fait que le conteneur partage l'OS avec la machine hôte. La VM non. 





## Étape 3 :



1. Permet de créer plusieurs conteneur identique plus rapidement



2\. Image c'est un fichier qui regroupe le code de l'app les bib, et les instructions de configuration. Alors qu'un conteneur c'est l'app qui tourne réellement, lancé à partir de l'image 



## Étape 4 :



1. Docker Compose permet de gérer plusieurs conteneurs en même temps à partir d'un seul fichier de configuration. Cela évite de devoir lancer chaque conteneur individuellement avec des commandes Docker. Il simplifie également la gestion des réseaux, des ports et des dépendances entre les services. Une seule commande suffit pour lancer tous les conteneurs " docker compose up".

2. Le fichier docker-compose.yml sert à décrire la configuration de l'ensemble des services d'une application. Il permet notamment de définir les images à utiliser ou à construire, les ports à exposer, les volumes et les réseaux. Il permet de configurer les différents microservices de la calculatrice et de les lancer ensemble de manière reproductible.

3. Une des limite que peut avoir Docker Compose est le fait que si l'un des conteneur contient un bug et ne se lance plus alors tous les autres conteneurs sont affectés et on ne peut plus rien utilisé. Les conteneurs sont interdépendants entre eux. Un bug peut affecter tous les conteneurs.









