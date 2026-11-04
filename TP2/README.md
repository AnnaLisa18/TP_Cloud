## Étape 1

### 1. Quel est le rôle du réseau privé et du routeur externe ?

Le réseau privé `net-discovery` permet aux VM du projet de communiquer entre elles sur le sous-réseau `192.168.10.0/24`. Pour joindre une adresse en dehors de ce sous-réseau, une VM envoie ses paquets à sa passerelle `192.168.10.1`. Cette adresse appartient au routeur `routerdefault`, qui transmet les paquets vers le réseau de destination.

### 2. Pourquoi le Cloud propose-t-il différentes catégories de ressources ?

Chaque catégorie répond à un besoin distinct. Une instance exécute les applications, un volume conserve les données, un réseau relie les machines et un groupe de sécurité filtre le trafic. Les gérer séparément permet, par exemple, de connecter une deuxième VM au réseau existant sans avoir à recréer le réseau ni ses règles de sécurité.

### 3. Quel chemin suit un paquet envoyé depuis l’extérieur vers une VM sur net-discovery ?

Sur notre topologie, `routerdefault` relie le réseau externe `vpn-external` au réseau privé `net-discovery`. Une fois qu’une IP flottante sera associée à la VM, un paquet envoyé à cette adresse arrivera par le réseau externe. Le routeur traduira l’IP flottante en adresse privée de la VM et transmettra le paquet sur `net-discovery`. Le groupe de sécurité associé à la VM autorisera le trafic entrant s’il correspond à une règle, par exemple SSH sur le port TCP 22.

## Étape 2 — Création de la VM et connexion SSH

### 1. Quelles étapes permettent de se connecter en SSH à la VM ?

Nous avons créé une VM Ubuntu sur `net-discovery` avec la paire de clés
`vm-discovery-key` et le groupe de sécurité `TP2_security_group`, qui autorise
les connexions TCP entrantes sur le port 22. Nous avons ensuite associé
l’IP flottante `194.199.106.226` à l’IP privée `192.168.10.155`.
Depuis PowerShell, nous nous sommes connectés avec la clé privée et le compte `ubuntu`.

Les commandes exécutées dans la VM ont donné les résultats suivants :

| Élément vérifié | Résultat |
|---|---|
| Système | Ubuntu 24.04.2 LTS |
| Adresse de l’interface `enp3s0` | 192.168.10.155/24 |
| Processeurs visibles | 1 vCPU |
| Mémoire visible par Ubuntu | 961 MiB au total, dont 656 MiB disponibles au moment du test |

### 2. Quel est le rôle de chaque élément dans la connexion ?

Depuis le PC, le client SSH contacte l’IP flottante `194.199.106.226`.
Le routeur traduit cette adresse vers l’IP privée `192.168.10.155`,
qui identifie la VM sur `net-discovery`. C’est pourquoi la commande `ip`
exécutée dans Ubuntu affiche l’adresse privée. Le groupe de sécurité
autorise le trafic entrant sur le port 22, puis SSH authentifie
l’utilisateur grâce à sa clé.

### 3. Pourquoi distinguer IP privée et IP flottante ?

Chaque VM peut communiquer sur son réseau privé. On réserve les adresses
publiques aux VM qui ont besoin d’un accès extérieur. Une IP flottante
peut ensuite être réattribuée à une autre VM sans modifier son adresse privée.
