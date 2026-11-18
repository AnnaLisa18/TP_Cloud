# TP DevOps 1 — Déployer une application sur Kubernetes

L’objectif du TP est de déployer l’application Calculator sur un cluster Kubernetes hébergé sur OpenStack. L’application comprend un frontend qui reçoit les calculs et quatre microservices qui réalisent les opérations.

## Étape 1 — Mettre en place l’infrastructure Cloud (§ 1.8)

Dans le projet OpenStack `ens-insa-tp3-07`, nous avons créé le réseau privé `net-devops` (`192.168.20.0/24`) et l’avons relié à `routerdefault`. Conformément à la consigne donnée en séance, notre cluster utilise deux workers.

| Machine | Adresse privée | Gabarit | Rôle |
|---|---|---|---|
| VM-K8s | `192.168.20.214` | `insa-medium-4.4.20` | Master Kubernetes |
| VM-Registry | `192.168.20.207` | `insa-tiny-1.1.10` | Docker Registry |
| VM-1 | `192.168.20.234` | `insa-tiny-1.1.10` | Worker Kubernetes |
| VM-2 | `192.168.20.62` | `insa-tiny-1.1.10` | Worker Kubernetes |

Le master et le Registry disposent chacun d’une IP flottante. Pour accéder aux workers en SSH, nous passons par le master grâce à `ProxyJump`. Depuis le master, nous avons vérifié que le Registry répondait à l’adresse `http://192.168.20.207:5000/v2/`.

### 1. Quel est le rôle de VM-K8s et quelle est la différence avec les workers ?

VM-K8s héberge le plan de contrôle du cluster. Son serveur d’API reçoit les commandes envoyées avec `kubectl`, et le scheduler choisit le nœud sur lequel placer chaque nouveau Pod. Sur les workers, le kubelet veille à l’exécution des Pods affectés à la machine et s’appuie sur containerd pour démarrer leurs conteneurs.

### 2. Pourquoi installer le Registry et le master sur deux VM différentes ?

Le Registry stocke les images Docker et les fournit aux nœuds qui en ont besoin. Une VM dédiée évite que le stockage et le téléchargement de ces images mobilisent les ressources du master. Les deux services peuvent aussi être maintenus séparément. Par exemple, pendant un redémarrage du Registry, le plan de contrôle reste disponible et les conteneurs déjà lancés continuent de fonctionner.

### 3. Quelles opérations importantes le script d’installation du master réalise-t-il ?

- Il installe **containerd**, le moteur chargé d’exécuter les conteneurs.
- Il installe **kubeadm**, **kubelet** et **kubectl**, qui servent respectivement à initialiser le cluster, à gérer les Pods d’un nœud et à interagir avec l’API Kubernetes.
- Il initialise le plan de contrôle avec `kubeadm` et génère le fichier de configuration administrateur `/etc/kubernetes/admin.conf`.

Lors de notre premier test, `kubectl` cherchait à joindre `localhost:8080`. Nous avons vérifié que le master répondait avec son fichier de configuration administrateur, puis copié ce fichier dans `~/.kube/config` et attribué les droits au compte `ubuntu`. La commande `kubectl get nodes` a ensuite fonctionné sans `sudo`.

Après le rattachement des deux workers et l’installation de Calico, nous avons obtenu le résultat suivant :

| Nœud | État | Rôle affiché | Version |
|---|---|---|---|
| `vm-k8s` | `Ready` | `control-plane` | `v1.36.6` |
| `vm-1` | `Ready` | `<none>` | `v1.36.6` |
| `vm-2` | `Ready` | `<none>` | `v1.36.6` |

## Étape 2 — Déployer un premier microservice (§ 2.5)

### 1. Quel est le rôle d’un Deployment ? Pourquoi l’utiliser pour déployer un Pod ?

Un Deployment décrit l’état souhaité de l’application, notamment l’image à utiliser et le nombre de répliques à maintenir. Pour `calculator-calc`, la cible est d’une réplique. Si le Pod est supprimé, Kubernetes en crée un nouveau pour revenir à cette cible. Un Pod créé seul ne bénéficie pas de ce remplacement automatique après sa suppression.

### 2. Quelle est la différence entre une image Docker, un Pod et un Deployment ?

L’image Docker contient le code du microservice et ses dépendances. Le Pod est l’unité d’exécution dans laquelle Kubernetes fait tourner le conteneur issu de cette image. Le Deployment décrit le modèle de Pod à utiliser et le nombre d’exemplaires souhaité. Il s’appuie sur un ReplicaSet pour maintenir ce nombre.

### 3. Comment fonctionne la réconciliation et quelle composante en est responsable ?

Le contrôleur du Deployment gère les ReplicaSets. Le contrôleur du ReplicaSet compare le nombre de Pods présents au nombre demandé. Si le seul Pod de `calculator-calc` est supprimé, le ReplicaSet détecte qu’il manque une réplique et crée un nouveau Pod à partir du même modèle. L’application retrouve ainsi une réplique avec un nouveau nom de Pod.

Après la suppression du Pod `calculator-calc-547949dc54-b7gvx`
(`192.168.217.130`), le ReplicaSet a créé automatiquement le Pod
`calculator-calc-547949dc54-g2vh6` pour maintenir la réplique demandée.
Le nouveau Pod est passé de `ContainerCreating` à `Running` en environ
deux secondes. Il s’exécute sur le même worker (`vm-2`), avec une nouvelle
adresse IP (`192.168.217.131`).

## Étape 3 — Exposer le microservice (§ 3.4)

### 1. Pourquoi utiliser un Service pour donner accès au Pod de calculator-calc ?

L’adresse IP du Pod appartient au réseau interne du cluster et peut changer si le Pod est remplacé. Un Service fournit un point d’accès stable. Avec le type `NodePort`, le client peut joindre le frontend en utilisant l’adresse d’un nœud et le port attribué au Service.

### 2. Comment Kubernetes sait-il à quel Pod transmettre les requêtes ?

Le Service utilise un sélecteur qui correspond au label du Pod, ici `app: calculator-calc`. Kubernetes actualise la liste des Pods prêts qui correspondent à ce sélecteur. Si le frontend est recréé, le Service peut ainsi acheminer les requêtes vers son remplaçant.

### 3. Quelle est la différence entre le port exposé, le port cible et le NodePort ?

| Champ | Rôle dans notre déploiement |
|---|---|
| `port` | Port du Service à l’intérieur du cluster. Pour le frontend, il vaut `80`. |
| `targetPort` | Port sur lequel le conteneur écoute dans le Pod. Il vaut également `80`. |
| `nodePort` | Port ouvert sur les nœuds pour accéder au Service. Le test du TP utilise l’IP flottante de VM-K8s et ce port. |

Le Service `calculator-calc` a reçu le NodePort `32172`. Depuis
`vm-k8s`, les tests sur l’IP flottante du master puis sur son IP privée
ont tous deux expiré après 5 secondes. Aucune réponse HTTP n’a été reçue.

## Étape 4 — Déployer l’application Calculator (§ 4.4)

### 1. Pourquoi utiliser un NodePort pour le frontend et des ClusterIP pour les quatre backends ?

Le frontend est le point d’entrée des utilisateurs. Son NodePort permet de lui envoyer un calcul depuis l’extérieur du cluster. Les quatre backends reçoivent leurs requêtes du frontend. Un Service de type `ClusterIP` donne à chacun une adresse accessible à l’intérieur du cluster, sur le port correspondant à son opération.

### 2. Comment le frontend contacte-t-il un backend sans connaître l’adresse IP de son Pod ?

Le frontend utilise le nom DNS du Service et son port. Par exemple, pour une addition, il contacte le Service de somme. Le DNS de Kubernetes résout ce nom vers l’adresse interne du Service. La requête est ensuite acheminée vers un Pod prêt correspondant au sélecteur.

### 3. Pourquoi éviter de configurer directement les adresses IP des Pods dans le frontend ?

Après le remplacement d’un Pod backend, son adresse IP peut changer. Si l’ancienne adresse est inscrite dans la configuration du frontend, les calculs destinés à ce backend risquent d’échouer. Avec le nom du Service, le frontend conserve la même destination même lorsque Kubernetes remplace un Pod ou ajoute une réplique.

*À compléter après le § 4.3 — Calcul envoyé à Calculator, réponse exacte reçue et comparaison avec le résultat attendu.*
