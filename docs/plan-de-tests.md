# Plan de tests — Cisco CCNA Mega Lab

Ce document regroupe la campagne de tests réalisée après les configurations. 
Je n'ai listé ici que les plus essentiels

L'objectif est de vérifier le fonctionnement réel des différentes
fonctionnalités configurées et de conserver une preuve des résultats obtenus.

Les captures associées aux tests seront conservées dans le dossier
`captures/`.


# Plan de tests

Ce document liste les tests à effectuer sur le projet afin de réaliser les captures d'écran finales.

Les résultats des tests seront ajoutés au moment de leur réalisation.

---

## Phase 1 — Configuration initiale

- Vérification de l'accès privilégié et des mots de passe configurés — `phase-01/configuration-mot-de-passe.png`
- Vérification de la configuration générale des équipements — `phase-01/show-running-config.png`

---

## Phase 2 — VLAN, Trunks, EtherChannel et VTP

- Vérification de la présence et du fonctionnement des VLAN sur les switches — `phase-02/vlan-verification.png`
- Vérification des trunks entre les switches — `phase-02/trunk-verification.png`
- Vérification du fonctionnement de l'EtherChannel — `phase-02/etherchannel-verification.png`
- Vérification du fonctionnement de VTP — `phase-02/vtp-verification.png`
- Vérification des ports d'accès et de leur appartenance aux VLAN — `phase-02/access-ports-verification.png`
- Vérification des interfaces inutilisées mises hors service — `phase-02/unused-ports-verification.png`

---

## Phase 3 — Adressage IP, EtherChannel L3 et HSRP

- Vérification de l'adressage IP des interfaces principales — `phase-03/ip-addressing-verification.png`
- Vérification de l'EtherChannel de niveau 3 entre CSW1 et CSW2 — `phase-03/l3-etherchannel-verification.png`
- Vérification de l'état HSRP sur les switches de distribution — `phase-03/hsrp-verification.png`
- Vérification de la connectivité vers les passerelles HSRP — `phase-03/ping-gateway.png`
- Vérification de la connectivité inter-VLAN — `phase-03/ping-inter-vlan.png`
- Vérification de la bascule HSRP entre les switches de distribution — `phase-03/hsrp-failover.png`

---

## Phase 4 — Rapid PVST+ et protection des ports

- Vérification de l'état Rapid PVST+ — `phase-04/spanning-tree-verification.png`
- Vérification des Root Bridges pour les différents VLAN — `phase-04/root-bridge-verification.png`
- Vérification de PortFast sur les ports d'accès — `phase-04/portfast-verification.png`
- Vérification de BPDU Guard sur les ports concernés — `phase-04/bpdu-guard-verification.png`

---

## Phase 5 — Routage OSPF et connectivité

- Vérification du fonctionnement des EtherChannel — `phase-05/etherchannel-verification.png`
- Vérification de l'état des interfaces principales — `phase-05/interfaces-csw1.png`
- Vérification de l'état HSRP — `phase-05/hsrp-verification.png`
- Vérification des voisinages OSPF — `phase-05/ospf-neighbors.png`
- Vérification de la présence de la route par défaut sur DSW-B2 — `phase-05/default-route-dsw-b2.png`
- Vérification de la connectivité entre les différents réseaux — `phase-05/connectivity-tests.png`

---

## Phase 6 — Services réseau

- Vérification de l'attribution des adresses IP par DHCP — `phase-06/dhcp-verification.png`
- Vérification de la résolution DNS — `phase-06/dns-resolution.png`
- Vérification de la synchronisation NTP — `phase-06/ntp-synchronization.png`
- Vérification de la connexion SSH aux équipements — `phase-06/ssh-connection.png`
- Vérification du fonctionnement du NAT/PAT — `phase-06/nat-translations.png`
- Vérification de la réception des messages Syslog — `phase-06/syslog-verification.png`
- Vérification de la configuration SNMP — `phase-06/snmp-verification.png`
- Vérification du transfert FTP — `phase-06/ftp-transfer.png`

---

## Phase 7 — Sécurité réseau

- Vérification du fonctionnement des ACL — `phase-07/acl-verification.png`
- Vérification de Port Security sur les ports d'accès — `phase-07/port-security.png`
- Vérification du fonctionnement de DHCP Snooping — `phase-07/dhcp-snooping.png`
- Vérification du fonctionnement de Dynamic ARP Inspection — `phase-07/dai.png`

---

## Phase 8 — IPv6

- Vérification de l'adressage IPv6 sur les interfaces — `phase-08/ipv6-interface.png`
- Vérification de la table de routage IPv6 — `phase-08/ipv6-routing-table.png`
- Vérification des routes par défaut IPv6 — `phase-08/ipv6-default-route.png`

---

## Phase 9 — Wireless / WLC

- Vérification du trunk entre ASW-A1 et le WLC — `phase-09/wlc-trunk-verification.png`
- Vérification de la connectivité vers le WLC — `phase-09/wlc-connectivity.png`
- Vérification de l'accès à l'interface Web du WLC — `phase-09/wlc-access.png`

---

# Problèmes rencontrés / tests non fonctionnels

Cette partie sera complétée après les tests.

Les problèmes qui ne fonctionnent pas ou qui présentent une limitation de Packet Tracer seront indiqués ici simplement, avec une courte explication.

Exemple :

- **WLC :** l'interface Web `https://10.0.0.7` ne s'ouvre pas dans Packet Tracer malgré les vérifications effectuées. La partie Wireless n'a donc pas pu être validée complètement.
- **FTP :** le transfert de l'image IOS reste bloqué dans Packet Tracer et n'a pas pu être terminé.

## Limitations

La mise à niveau IOS de R1 par FTP n'a notamment pas pu être finalisée.

La partie Wireless n'a également pas pu être entièrement validée en raison
du problème d'accès à l'interface de gestion du WLC1.


