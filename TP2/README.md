## Étape 1

### 1. Quel est le rôle du réseau privé et du routeur externe ?

Le réseau privé `net-discovery` permet aux VM du projet de communiquer entre elles sur le sous-réseau `192.168.10.0/24`. Pour joindre une adresse en dehors de ce sous-réseau, une VM envoie ses paquets à sa passerelle `192.168.10.1`. Cette adresse appartient au routeur `routerdefault`, qui transmet les paquets vers le réseau de destination.

### 2. Pourquoi le Cloud propose-t-il différentes catégories de ressources ?

Chaque catégorie répond à un besoin distinct. Une instance exécute les applications, un volume conserve les données, un réseau relie les machines et un groupe de sécurité filtre le trafic. Les gérer séparément permet, par exemple, de connecter une deuxième VM au réseau existant sans avoir à recréer le réseau ni ses règles de sécurité.

### 3. Quel chemin suit un paquet envoyé depuis l’extérieur vers une VM sur net-discovery ?

Sur notre topologie, `routerdefault` relie le réseau externe `vpn-external` au réseau privé `net-discovery`. Une fois qu’une IP flottante sera associée à la VM, un paquet envoyé à cette adresse arrivera par le réseau externe. Le routeur traduira l’IP flottante en adresse privée de la VM et transmettra le paquet sur `net-discovery`. Le groupe de sécurité associé à la VM autorisera le trafic entrant s’il correspond à une règle, par exemple SSH sur le port TCP 22.
