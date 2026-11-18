## Étape 1

### 1. Quel est le rôle de VM-K8s dans l’architecture mise en place ? Quelle est la différence entre cette machine et les nœuds workers ?


### 2. Pourquoi le Docker Registry et le master Kubernetes sont-ils installés sur deux VMs différentes ?


### 3. Le script d’installation vous a permis d’installer Kubernetes sans exécuter manuellement toutes les commandes nécessaires. Citez les opérations importantes réalisées par le script master et expliquez très brièvement en quoi elles sont nécessaires au fonctionnement du cluster.


## Étape 2

### 1. Quel est le rôle d’un Deployment dans Kubernetes ? Pourquoi ne déploie-t-on pas directement un Pod ?


### 2. Quelle est la différence entre une image Docker, un Pod et un Deployment ? Comment les 3 sont-ils liés ?


### 3. Expliquez le principe de réconciliation. Quelle composante Kubernetes est responsable de ce comportement ?


## Étape 3

### 1. Pourquoi ne peut-on pas simplement utiliser l’adresse IP du Pod calculator-calc pour permettre à un utilisateur d’accéder à l’application ?


### 2. Comment Kubernetes sait-il que le Service calculator-calc doit transmettre les requêtes au Pod de calculator-calc ?


### 3. Quelle est la différence entre le port exposé, le port cible et le NodePort utilisés dans votre architecture ?


## Étape 4 

### 1. Pourquoi les quatre microservices utilisent-ils un port de type ClusterIP, tandis que calcservice utilise un port de type NodePort ?


### 2. comment Kubernetes permet-il au frontend de contacter le backend sans connaître l’adresse IP du Pod ?


### 3. On propose de configurer le frontend avec l’adresse IP actuelle des Pods backend (sum, sub...), par exemple "http ://10.x.x.x :<PORT>". Pourquoi cette solution n’est-elle pas adaptée à Kubernetes ?


