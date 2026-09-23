# Cisco CCNA Mega Lab — Infrastructure réseau d'entreprise

Projet personnel réalisé avec **Cisco Packet Tracer** dans le cadre de ma mise en pratique des notions étudiées durant le parcours **CCNA de Jeremy's IT Lab**.

L'objectif de ce projet était de construire progressivement une infrastructure réseau d'entreprise complète, puis de la configurer, la dépanner et documenter les différentes étapes de sa réalisation.

Je ne me suis pas limité à reproduire les configurations du lab. J'ai également pris le temps de comprendre les choix effectués, d'identifier les erreurs rencontrées pendant la configuration et de conserver une trace du dépannage et des vérifications réalisées.

> **Projet :** Cisco CCNA Mega Lab 
> **Environnement :** Cisco Packet Tracer 
> **Type :** Projet personnel / laboratoire réseau 
> **Statut :** Configuration principale terminée — validation et documentation en cours

J'ai terminé le projet à 88% (car c'est un fichier .pka)

---

## Présentation du projet

Après avoir suivi les cours CCNA de Jeremy's IT Lab, j'ai choisi de réaliser son Mega Lab afin de mettre en pratique une grande partie des concepts étudiés pendant le parcours.

Le lab reproduit une infrastructure réseau composée de plusieurs zones, de switches d'accès et de distribution, de switches multilayer, d'un routeur central, de services réseau et d'une partie sans fil.

Le projet m'a permis de travailler non seulement sur la configuration des équipements Cisco, mais également sur des aspects importants comme la redondance, le routage, la sécurité de niveau 2, les services réseau, la traduction d'adresses et IPv6.

Une partie importante du travail a également consisté à effectuer du **troubleshooting** lorsque le comportement obtenu ne correspondait pas à celui attendu.

---

## Technologies et concepts utilisés

### Infrastructure réseau

- Cisco Packet Tracer
- VLAN
- Trunking 802.1Q
- VTP
- EtherChannel
- EtherChannel de couche 2
- EtherChannel de couche 3
- Inter-VLAN Routing
- SVI
- Routage statique
- OSPF
- HSRP v2
- RPVST+

### Services réseau

- DHCP
- DNS
- NTP
- SNMP
- Syslog
- FTP
- SSH
- NAT
- PAT
- DHCP Relay

### Sécurité réseau

- ACL étendues
- Port Security
- Sticky MAC
- DHCP Snooping
- Dynamic ARP Inspection (DAI)
- BPDU Guard
- PortFast
- Désactivation des interfaces inutilisées

### IPv6

- IPv6 addressing
- IPv6 EUI-64
- IPv6 routing
- Routes par défaut IPv6

### Wireless

- Wireless LAN Controller (WLC)
- WLAN
- SSID
- WPA2 / AES / PSK
- Lightweight Access Point (LWAP)

---

## Architecture

L'infrastructure est organisée autour de plusieurs niveaux :

- un routeur central **R1** ;
- des switches multilayer de cœur **CSW1** et **CSW2** ;
- des switches multilayer de distribution pour les différents bureaux ;
- des switches d'accès ;
- des postes clients et téléphones IP ;
- des serveurs ;
- un contrôleur WLAN et des points d'accès sans fil.

La conception utilise notamment la redondance avec **HSRP**, l'agrégation de liens avec **EtherChannel** et le routage dynamique avec **OSPF**.

Les différents VLAN sont utilisés afin de séparer les postes utilisateurs, la téléphonie, les serveurs, le Wi-Fi et le réseau de management.

---

## Principales fonctionnalités mises en œuvre

### Segmentation réseau

Plusieurs VLAN ont été configurés afin de séparer logiquement les différents types de trafic.

Parmi eux :

- VLAN 10 — Postes utilisateurs
- VLAN 20 — Téléphonie
- VLAN 30 — Serveurs
- VLAN 40 — Wi-Fi
- VLAN 99 — Management

Les VLAN sont transportés entre les différents niveaux de l'infrastructure à l'aide de trunks 802.1Q.

### Redondance

La disponibilité du réseau a été travaillée à plusieurs niveaux :

- EtherChannel pour l'agrégation des liens ;
- HSRP pour la redondance des passerelles ;
- RPVST+ pour la gestion de la topologie de niveau 2 ;
- routage redondant vers les réseaux externes.

La priorité HSRP et la priorité STP ont également été réparties entre les équipements afin de distribuer les rôles actifs et standby.

### Routage

Le routage combine plusieurs mécanismes :

- routes statiques ;
- routes statiques flottantes ;
- OSPF ;
- redistribution nécessaire au fonctionnement de l'architecture ;
- routage inter-VLAN sur les switches multilayer.

### Services réseau

Le routeur central fournit notamment les services DHCP pour les différents réseaux clients.

Un serveur interne est également utilisé pour plusieurs services réseau, notamment DNS, NTP, SNMP, Syslog et FTP.

La résolution DNS a été configurée avec un nom de domaine personnalisé utilisé dans le projet :

`mamyratianalison.com`

### NAT / PAT

Le routeur R1 utilise le NAT/PAT afin de permettre aux réseaux privés d'accéder aux réseaux externes.

La configuration comprend notamment :

- une translation dynamique avec PAT ;
- une pool d'adresses publiques ;
- une translation statique pour le serveur DNS.

Le fonctionnement du PAT a été vérifié à l'aide de tests de connectivité et de commandes de vérification des traductions NAT.

### Sécurité de niveau 2

Plusieurs mécanismes de sécurité ont été configurés sur les switches d'accès :

- Port Security ;
- Sticky MAC ;
- DHCP Snooping ;
- Dynamic ARP Inspection ;
- BPDU Guard ;
- PortFast ;
- désactivation des interfaces inutilisées.

Une ACL étendue a également été mise en place afin de contrôler les communications entre certains réseaux utilisateurs.

### IPv6

Une partie dédiée à IPv6 a été réalisée avec :

- adressage IPv6 sur les interfaces du routeur ;
- adressage EUI-64 ;
- activation du routage IPv6 ;
- routes par défaut IPv6 ;
- activation d'IPv6 sur le réseau de niveau 3.

---

## Troubleshooting

Le projet n'a pas été réalisé sans erreurs.

Plusieurs problèmes ont été rencontrés au cours de la configuration, notamment :

- erreur de contexte lors de l'utilisation de `logging synchronous` ;
- oubli de configuration de certains trunks ;
- problème de Native VLAN mismatch ;
- interfaces qui ne montaient pas correctement ;
- problème lié aux priorités STP ;
- problème d'adressage lors de la configuration de HSRP ;
- absence de route par défaut sur un équipement à cause d'une configuration OSPF incomplète ;
- vérification et correction de la configuration DNS ;
- vérification du fonctionnement du NAT/PAT ;
- problèmes rencontrés lors de la configuration de la partie Wireless.

Ces problèmes et leurs corrections sont détaillés dans le journal de bord et, lorsque cela est pertinent, illustrés par des captures d'écran.

---

## État de la partie Wireless

La partie Wireless du lab a été configurée jusqu'à l'étape de mise en place du WLC et du réseau Wi-Fi.

Cependant, dans mon environnement Cisco Packet Tracer, l'accès à l'interface de gestion du **WLC1 (10.0.0.7)** depuis les postes clients n'a pas pu être établi correctement.

Plusieurs vérifications ont été effectuées :

- adresse IP du WLC ;
- passerelle ;
- état de l'interface GigabitEthernet ;
- VLAN de management ;
- trunk entre ASW-A1 et WLC1 ;
- VLAN 40 et VLAN 99 sur le trunk ;
- état STP ;
- routage inter-VLAN ;
- DHCP Snooping ;
- Dynamic ARP Inspection.

La liaison **ASW-A1 ↔ WLC1** a notamment été vérifiée et le port Fa0/2 fonctionne bien en trunk 802.1Q avec les VLAN 40 et 99.

Malgré ces vérifications, l'accès au WLC depuis les réseaux clients n'a pas pu être finalisé dans Packet Tracer.

Je conserve volontairement cette limitation dans la documentation plutôt que de considérer cette partie comme fonctionnelle sans validation.

---

## Documentation du projet

Le dépôt contient plusieurs documents permettant de suivre la réalisation du lab.

```text
.
├── README.md
├── mega_lab.pka
│
├── docs/
│   ├── connexions.xlsx
│   └── plan-de-tests.md
│   
│
├── configs/
│   └── configurations des équipements
│
├── captures/
│   ├── phase-01/
│   ├── phase-02/
│   ├── phase-03/
│   ├── phase-04/
│   ├── phase-05/
│   ├── phase-06/
│   ├── phase-07/
│   ├── phase-08/
│   └── phase-09/
│   └── TOPOLOGIE.png
│
└── journal-de-bord.md
