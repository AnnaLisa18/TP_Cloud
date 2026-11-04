## Étape 1

### 1. Quel est le rôle du réseau privé et du routeur externe ?

Le réseau privé `net-discovery` permet aux VM du projet de communiquer entre elles sur le sous-réseau `192.168.10.0/24`. Pour joindre une adresse en dehors de ce sous-réseau, une VM envoie ses paquets à sa passerelle `192.168.10.1`. Cette adresse appartient au routeur `routerdefault`, qui transmet les paquets vers le réseau de destination.

### 2. Pourquoi le Cloud propose-t-il différentes catégories de ressources ?

Chaque catégorie répond à un besoin distinct. Une instance exécute les applications, un volume conserve les données, un réseau relie les machines et un groupe de sécurité filtre le trafic. Les gérer séparément permet, par exemple, de connecter une deuxième VM au réseau existant sans avoir à recréer le réseau ni ses règles de sécurité.

### 3. Quel chemin suit un paquet envoyé depuis l’extérieur vers une VM sur net-discovery ?

Sur notre topologie, `routerdefault` relie le réseau externe `vpn-external` au réseau privé `net-discovery`. Une fois qu’une IP flottante sera associée à la VM, un paquet envoyé à cette adresse arrivera par le réseau externe. Le routeur traduira l’IP flottante en adresse privée de la VM et transmettra le paquet sur `net-discovery`. Le groupe de sécurité associé à la VM autorisera le trafic entrant s’il correspond à une règle, par exemple SSH sur le port TCP 22.


-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAxRr6+/ysTscBuQJPejX1jhqLsuabPWH/gxJotapsThRfUhP+
Axanxxr5ygEDHn7NUS6rvjdqteH7GtoPosoo5Zg0dm+abGMHQ7RPkD9Ce9fQ4hf3
FK4rlme5651bRuR5YVXjtIgwraBorzqu7swGXnacDG8juGidZn1DwjUMI7cv8yod
vSbe1Hsw0jHQQ+Cdx5Pm5oHZo5JebBa69J/4BvMcewLOcOJiD9g0VomCal1su3mA
Rxln1bzcUPFq+S1vGJYQS4JM7SYVwKmouTEyDfXShKBmCcxs5Lpv3kxaKS/u2G5U
5CqzVmFQyWl4JhgfVtwiNZfI9Ttob94c/oMmHwIDAQABAoIBAArsxmmOJTzOkf1f
r+kNsZkZRpH5bzRVSLTKoewj0JH5Vhk2C3e/WzhRefPk33fivl2YESPfbXY8Ws8v
s/spal/jclD4OZ51VLM/fMN5hiVYHndRrbvDNZ5GDIuPMr54rnOjL5ZTkCMnh3Xi
8kxBGb9Forubs2amwcaq/cT3bL9T8IfqAMIMg9NQblQN62n08q1KSXIOT1LBszQT
ZwDj6wQiXULbQq4es752qcwPrACfsKuaptWFIsb2Q7c2Vop/Xyws0vgSvTQmFL4g
vBjEBHyYTq7Rhwzfaliq4ZAA+9g4iUdYLrMDmg99VnX8MXRAHO3hjEMBXcGi9jUE
PXogcCECgYEA8cbSV0/XrqBVJ8856iQSBi5pVhMWQE3k3aLGRSw6oaZjIe6805ob
og+sBJX54MPMsEGUQqF3CfiK0bHi4ERf5Sck9moMmdD+qfX6uKSUIiDG1q/nQfZ3
rH1M89kDxWYLEPZ/QHYgo9izDjBoqmjtOzxDDhCWxsKhAW1lOVFs4f8CgYEA0LNn
1MzaobcttPipW14v35JVL7j5ZybheyoImvkQdP6uZV9PEYpRxXNn0zzzj3KBkDuA
pGwqAoWOmQR3Y/3tPZSOX41nX6z1tWxatFpgJ3UJ0ulSgOXgjuzAB5GqjUcL0Bz1
mHxlG9pTvRhyM7hV1TLp1TTsGCaAVnMYRMzFe+ECgYAb0AZUWu/xu6nwGWuNHrcg
FOizKSDvJsbEJDxwqqTYAZ73wVHheaHiI7FjGamUDJIYWILoo1BhSkEV3EPADr2F
S9p5qjtm2l7hSFWeNfiAcIrPGY8dzG953TQj/PvtEk8FlYWP8cbRclUutazkpYtc
vBLupEPj+3GwC1iQnmhLGwKBgDe84msr9uwfvv5JNvnm8sQ0Ebo4HavV73x0Ym35
PNLyPeNnC/f0SBiC1OTOr4HNJBtAOQAwvxuDTe1CCxzScWzz4v5UBpq/17OHZ8Yp
KOsA0AnbNKSmQE4LntGmrrUil14SjekWt+4zZg/nmoCDp7DT8y10/+UHS1lLTdz6
D3ghAoGAbzzLl8nixTieiF5wcm6Rkxvdtl9HWo1MhEHr05UleR9PCOusZmcittOZ
4tAaolLz4xfuweqlZ/tqHyD9Mw/YdNrcSRlH+Nw07jWR81oTnAq9FzPev6q5pWHe
9xnNDPb0yGfqytxgB/Qr/hYDkwi21XHbLkkLQlFbZ2Zbmynxdkc=
-----END RSA PRIVATE KEY-----
