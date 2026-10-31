# TP : Comprendre les collisions Ethernet avec un hub et un switch

## 1. Objectif

L'objectif de cette simulation est de comprendre le phénomène de collision dans un réseau Ethernet partagé et d'observer comment l'utilisation d'un switch permet de réduire ce problème.

## 2. Environnement utilisé

- **Cisco Packet Tracer** : simulation du réseau.
- **Deux ordinateurs (PC)** : équipements terminaux.
- **Un hub** : équipement permettant de relier les deux ordinateurs.

## 3. Déroulement de la manipulation

### Étape 1 : Création de la topologie

J'ai créé une topologie réseau composée de deux ordinateurs reliés à un hub.

Le hub permet aux deux ordinateurs de partager le même support de communication Ethernet.

### Étape 2 : Observation des échanges

À l'aide du mode Simulation de Cisco Packet Tracer, j'ai observé les échanges de trames entre les équipements.

Dans un réseau utilisant un hub, les signaux reçus sont répétés vers les autres ports. Les équipements partagent donc le même domaine de collision.

Lorsque deux équipements transmettent simultanément, une collision peut se produire.

### Étape 3 : Comprendre le rôle du switch

Contrairement au hub, le switch apprend les adresses MAC des équipements connectés à ses ports.

Il utilise sa table MAC pour transmettre les trames vers le port correspondant à leur destination, lorsque celle-ci est connue.

Chaque port du switch constitue un domaine de collision distinct.

Avec des connexions Ethernet en full-duplex, les équipements peuvent émettre et recevoir simultanément sans collisions Ethernet.

## 4. Observations

Cette simulation permet de comprendre les différences entre les deux équipements :

| Hub | Switch |
|---|---|
| Répète les signaux vers les autres ports. | Transmet les trames selon les adresses MAC. |
| Tous les équipements partagent le même domaine de collision. | Chaque port constitue un domaine de collision distinct. |
| Ne possède pas de table MAC. | Apprend et utilise les adresses MAC. |
| Fonctionnement généralement half-duplex. | Peut fonctionner en full-duplex. |

## 5. Conclusion

Ce TP m'a permis de comprendre pourquoi les hubs ont été remplacés progressivement par les switches dans les réseaux Ethernet.

Le switch améliore la communication en séparant les domaines de collision et en transmettant les trames de manière plus ciblée.

Cette manipulation m'a également permis de mieux comprendre le rôle des adresses MAC dans la communication au sein d'un réseau local.

## 6. Vidéo de démonstration

La vidéo ci-dessous présente la simulation réalisée dans Cisco Packet Tracer.

[Regarder la démonstration : collision_de_trame.mp4](video/collision_de_trame.mp4)