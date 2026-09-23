# Journal de bord

Ceci est le journal de réalisation étape par étape de ce lab
couvrant tout ce que j'ai fait, de la configuration au dépannage jusqu'aux
tests et à la documentation.

J'y note les différentes étapes de la configuration, les problèmes que j'ai
rencontrés, les corrections que j'ai faites ainsi que certaines vérifications
qui m'ont semblé intéressantes à conserver.


## Note

Pendant la réalisation du projet, je suis souvent passé de la configuration
au troubleshooting, puis à la documentation. Les dates et les heures indiquées
dans ce journal ne représentent donc pas forcément le temps réel passé sur
chaque phase.

Par exemple, si une phase relativement simple apparaît comme ayant pris
plusieurs heures, cela ne signifie pas forcément que j'ai passé tout ce temps
à la configurer. Une partie du temps a également été consacrée aux tests,
aux captures d'écran, à la documentation et au reste de mes activités car oui j'ai pas que ça a faire (haha)


---

# Phase 1 — Configuration initiale des équipements
**22/08/2026 — 10:36**

La première étape consistait à préparer les différents équipements du réseau
avant de commencer les configurations plus avancées.

J'ai commencé par :

- configurer le hostname de chaque routeur et switch suivant la topologie ;
- configurer les mots de passe de sécurité ;
- créer un compte utilisateur pour l'accès aux équipements ;
- configurer la déconnexion automatique après 30 minutes ;
- activer `logging synchronous` sur les consoles.

### Identifiants

Pour cette partie, je n'ai pas repris exactement les identifiants proposés
dans le lab. J'ai choisi mes propres username et mots de passe afin d'avoir
ma propre configuration.

### Premier problème rencontré

Je n'arrivais pas à entrer correctement la commande :

`logging synchronous`

Après quelques recherches, j'ai compris que je n'étais simplement pas dans
le bon mode de configuration.

La commande doit être configurée dans le contexte de la ligne console et non
directement dans le mode de configuration globale.

C'était une petite erreur, mais elle m'a rappelé qu'avec Cisco IOS, certaines
commandes dépendent fortement du contexte dans lequel on se trouve.

### Autre observation

J'ai également remarqué que le niveau de chiffrement de type 9 n'était pas
disponible de la même manière sur tous les équipements de Packet Tracer.
Certains équipements, notamment les multilayer switches, permettent cette
configuration alors que ce n'était pas le cas pour certains routeurs et
switches de couche 2 utilisés dans le lab.

### Captures

- `captures/phase-01/configuration-mot-de-passe.png`
- `captures/phase-01/show-running-config.png`


---

# Phase 2 — VLAN, trunking et EtherChannel
**22/08/2026 — 11:40**

Une fois les équipements préparés, j'ai commencé la configuration de la
couche 2 du réseau.

J'ai d'abord travaillé sur l'Office A, puis sur l'Office B.

Les principales tâches réalisées pendant cette phase étaient :

- création des VLAN ;
- désactivation de DTP ;
- configuration des trunks ;
- configuration des EtherChannel ;
- mise en place de VTP ;
- désactivation des interfaces inutilisées.

### VTP

J'ai configuré les DSW comme serveurs VTP et les switches d'accès comme
clients VTP.

Cela m'a permis de ne pas avoir à créer manuellement tous les VLAN sur
chaque switch d'accès. Les VLAN sont propagés depuis les switches de
distribution.

### Sécurisation des interfaces inutilisées

J'ai également désactivé les ports qui n'étaient pas utilisés.

Même si cette opération est assez simple, je trouve intéressant de la faire
dans un lab comme celui-ci car cela permet de prendre l'habitude de ne pas
laisser des interfaces inutilisées actives sans raison.

### Problème 1 — Native VLAN mismatch

J'ai rencontré une erreur de type :

`Native VLAN mismatch`

Après vérification, le problème venait d'un trunk que j'avais oublié de
configurer correctement sur ASW-A3.

Une fois le trunk configuré, le problème a disparu.

### Problème 2 — Interfaces non opérationnelles

J'ai également eu un problème avec certaines interfaces reliant les switches
d'accès aux équipements de distribution dans l'Office B.

Les interfaces ne fonctionnaient pas comme prévu.

Après vérification, j'ai simplement constaté que j'avais oublié de configurer
les trunks sur ces interfaces.

Cela m'a permis de voir une nouvelle fois l'importance de vérifier les deux
extrémités d'une liaison lorsqu'un trunk ne fonctionne pas.

### Captures

- `captures/phase-02/show-interface-trunk-ASW-B1.png`
- `captures/phase-02/show-running-config-ASW-A1.png`
- `captures/phase-02/show-running-config-exemple.png`
- `captures/phase-02/oublie-configuration-trunk-ASW-A3.png`
- `captures/phase-02/interface-des-switch-non-operationnel.png`


---

# Phase 3 — Adressage IP, EtherChannel couche 3 et HSRP
**22/08/2026 — 14:00**

Cette phase était plus importante car elle permettait de passer de la simple
configuration de couche 2 à la mise en place du routage et de la redondance.

J'ai commencé par configurer les interfaces de R1 connectées vers les
réseaux externes en DHCP.

J'ai ensuite configuré les interfaces reliant R1 aux switches multilayer,
ainsi qu'une interface Loopback sur R1.

Après cela, j'ai configuré l'EtherChannel de couche 3 entre CSW1 et CSW2.

Les différentes interfaces de niveau 3 des switches ont ensuite reçu leurs
adresses IP ainsi que leurs interfaces Loopback.

### HSRP

J'ai ensuite mis en place HSRP afin de fournir une passerelle redondante
pour les différents VLAN.

J'ai également réparti les rôles actif/standby entre les deux switches
afin que les deux équipements participent réellement au fonctionnement
du réseau au lieu de laisser un seul switch actif pour tous les VLAN.

### Problème — duplication d'adresse

Pendant la configuration d'HSRP, Packet Tracer m'a signalé un problème de
duplication d'adresse sur un VLAN.

J'ai revérifié les adresses et repris la configuration.

Après avoir refait la configuration, le problème a disparu.

Je n'ai pas réussi à déterminer avec certitude la cause exacte de cette
erreur. Je soupçonne un comportement particulier de Packet Tracer, mais
je ne peux pas l'affirmer.

### Vérification

J'ai utilisé `show standby brief` sur les switches de distribution afin de
vérifier les états Active/Standby des différents VLAN.

### Captures

- `captures/phase-03/probleme-de-duplication-d-adresse.png`
- `captures/phase-03/show-standby-brief-DSW-A1.png`
- `captures/phase-03/show-standby-brief-DSW-A2.png`


---

# Phase 4 — Rapid Spanning Tree Protocol
**22/08/2026 — 17:23**

Pour cette phase, j'ai travaillé sur le fonctionnement de RPVST+ et sur la
répartition des rôles de root bridge entre les switches de distribution.

Les équipements Cisco utilisés dans le lab fonctionnent déjà avec RPVST+,
je n'ai donc pas eu besoin de changer le mode de spanning-tree.

J'ai ensuite configuré les priorités STP.

Pour DSW-A1, j'ai choisi une priorité plus faible pour les VLAN 10 et 99 afin
qu'il soit root pour ces VLAN, tandis que les VLAN 20 et 40 utilisent une
priorité plus faible sur DSW-A2.

J'ai appliqué le même principe de répartition sur l'Office B.

### Petite erreur sur la priorité

Au début, j'ai essayé de définir une priorité de `5`.

Cela n'a pas fonctionné comme prévu.

J'ai ensuite réalisé que les valeurs de priorité STP utilisées ici suivent
des incréments de 4096.

J'ai donc corrigé la configuration avec les valeurs appropriées.

C'était une petite erreur, mais c'est le genre de détail qu'on comprend
beaucoup mieux lorsqu'on configure réellement les équipements plutôt que de
simplement lire la théorie.

### PortFast et BPDU Guard

J'ai également activé PortFast et BPDU Guard sur les ports destinés aux
équipements terminaux.

Une particularité est apparue avec la connexion entre ASW-A1 et WLC1.

Comme cette liaison est configurée en trunk, j'ai utilisé :

`spanning-tree portfast trunk`

plutôt que le PortFast classique utilisé sur un port d'accès.

### Captures

- `captures/phase-04/mini-erreur-priorite.png`
- `captures/phase-04/show-spanning-tree-vlan-20.png`
- `captures/phase-04/configuration-priorite.png`
- `captures/phase-04/configuration-portfast-trunk.png`


---

# Phase 5 — Routage statique et dynamique
**22/08/2026 — 17:43**

Cette phase était consacrée au routage.

J'ai commencé par configurer OSPF sur R1 ainsi que les routes statiques
nécessaires vers les réseaux externes.

J'ai également configuré une route statique flottante afin de disposer
d'une solution de secours en cas de perte du chemin principal.

### OSPF

J'ai configuré OSPF sur les différents équipements de couche 3.

Les interfaces SVI et Loopback ont été configurées en passive lorsque je ne
voulais pas qu'OSPF établisse de voisinage sur ces interfaces.

Les Loopback ont également été utilisées comme Router ID.

Pour la configuration de l'ASBR, j'ai dû consulter quelques ressources
complémentaires car je n'avais pas encore une maîtrise complète de cette
partie.

### Problème — absence de route par défaut

J'ai passé un certain temps à chercher pourquoi DSW-B2 ne recevait pas la
route par défaut.

Après plusieurs vérifications, j'ai trouvé que la configuration OSPF sur la
liaison concernée n'était pas complète.

La commande :

`ip ospf network point-to-point`

manquait sur la liaison concernée.

Après l'avoir ajoutée, le comportement attendu a été obtenu.

C'était probablement l'un des problèmes qui m'a demandé le plus de
réflexion jusqu'à cette étape, car la configuration générale d'OSPF semblait
correcte au premier regard.

### Vérification finale

La campagne de tests permettra de vérifier :

- les voisinages OSPF ;
- la présence des routes OSPF ;
- la présence de la route par défaut ;
- la connectivité entre les différents réseaux.

### Captures 

- `captures/phase-05/etherchannel-verification.png`
- `captures/phase-05/interfaces-csw1.png`
- `captures/phase-05/hsrp-verification.png`
- `captures/phase-05/ospf-neighbors.png`
- `captures/phase-05/default-route-dsw-b2.png`
- `captures/phase-05/connectivity-tests.png`


---

# Phase 6 — Services réseau et NAT

Cette phase était consacrée aux différents services nécessaires au
fonctionnement de l'infrastructure.

J'ai notamment travaillé sur :

- DHCP ;
- DNS ;
- NTP ;
- SNMP ;
- Syslog ;
- FTP ;
- SSH ;
- NAT/PAT.

## DHCP

Le serveur DHCP a été configuré sur R1 pour les différents réseaux clients.

J'ai configuré les plages correspondant notamment aux postes utilisateurs,
aux téléphones et au réseau Wi-Fi, tout en excluant les adresses réservées
aux passerelles et aux équipements d'infrastructure.

Le DHCP relay a également été utilisé afin de permettre aux différents
réseaux de joindre le serveur DHCP central.

## DNS

Le serveur SRV1 a été utilisé pour la résolution DNS.

J'ai choisi d'utiliser mon propre nom de domaine dans le lab :

`mamyratianalison.com`

Au départ, ma configuration DNS n'était pas correcte : les enregistrements
pour `www.mamyratianalison.com` ne correspondaient pas à ce que je voulais
faire.

J'ai donc supprimé les anciens enregistrements et configuré :

- `mamyratianalison.com` → enregistrement A ;
- `www.mamyratianalison.com` → CNAME vers `mamyratianalison.com`.

Après correction, la résolution fonctionnait correctement.

## NTP

J'ai configuré R1 comme source NTP pour le reste de l'infrastructure.

Les autres équipements utilisent R1 comme serveur NTP avec authentification.

La validation finale permettra de vérifier la synchronisation de l'heure sur
les équipements.

## SNMP et Syslog

SNMP a été configuré afin de permettre la supervision des équipements.

Syslog a également été configuré avec SRV1 comme serveur central de
journalisation.

La campagne de tests permettra de vérifier que les équipements utilisent
bien SRV1 pour ces services.

## FTP

J'ai tenté d'utiliser FTP pour transférer la nouvelle image IOS vers R1.

Le transfert vers le fichier :

`c2900-universalk9-mz.SPA.155-3.M4a.bin`

restait bloqué au niveau de l'accès au serveur FTP dans Packet Tracer.

Après plusieurs vérifications, j'ai décidé de ne pas poursuivre cette partie
au hasard et de conserver cette limitation dans la documentation.

La mise à niveau IOS par FTP n'est donc pas considérée comme validée.

## SSH

SSH a été configuré sur les routeurs et switches afin de permettre leur
administration à distance.

La configuration utilise SSH version 2, l'authentification locale et une ACL
limitant les sources autorisées.

La validation finale permettra de vérifier l'accès SSH depuis un poste
autorisé.

## NAT / PAT

J'ai ensuite configuré le NAT sur R1.

Une pool d'adresses publiques a été créée et le PAT a été configuré afin de
permettre aux réseaux privés de partager des adresses publiques pour les
connexions sortantes.

Une translation statique a également été configurée pour SRV1.

Lors d'un premier test, j'ai utilisé :

`show ip nat translations`

afin de vérifier les traductions créées.

Une traduction dynamique utilisant l'adresse `203.0.113.201` est apparue
pour PC1, ce qui confirmait que la translation fonctionnait.

J'ai également utilisé :

`show ip nat statistics`

pour vérifier les statistiques du NAT.

### Observation

Lors du premier test de connectivité externe, j'ai obtenu quelques pertes
de paquets.

Le test n'était donc pas parfait à 100 %, mais les traductions NAT
apparaissaient correctement et le fonctionnement du PAT était confirmé.

### Captures 

- `captures/phase-06/dhcp-verification.png`
- `captures/phase-06/dns-resolution.png`
- `captures/phase-06/ntp-synchronization.png`
- `captures/phase-06/ssh-connection.png`
- `captures/phase-06/nat-translations.png`
- `captures/phase-06/syslog-verification.png`
- `captures/phase-06/snmp-verification.png`
- `captures/phase-06/ftp-transfer.png`


---

# Phase 7 — ACL et sécurité de niveau 2

Cette phase était consacrée au contrôle du trafic et à plusieurs mécanismes
de sécurité des switches d'accès.

## ACL

J'ai configuré une ACL étendue appelée :

`OfficeA_to_OfficeB`

L'objectif était de permettre aux postes du réseau Office A de joindre les
postes du réseau Office B en ICMP, tout en bloquant les autres types de
trafic entre ces deux réseaux.

Le reste du trafic est ensuite autorisé.

La campagne de tests permettra de vérifier séparément le trafic autorisé et
le trafic bloqué.

## Port Security

J'ai configuré Port Security sur les ports d'accès concernés.

J'ai utilisé le mode `restrict` afin que les trames provenant d'une adresse
MAC non autorisée soient bloquées sans couper complètement le port.

Les adresses MAC sécurisées sont apprises avec Sticky MAC.

Pendant la configuration, j'ai également rencontré un détail important sur
ASW-A1.

Le port `Fa0/1` est utilisé pour un poste avec un téléphone IP. Une seule
adresse MAC était initialement autorisée, alors que deux équipements peuvent
être présents sur ce port.

J'ai donc corrigé la limite maximale à deux adresses MAC.

## DHCP Snooping

J'ai activé DHCP Snooping sur les VLAN concernés.

Les ports considérés comme fiables ont été configurés comme tels et les ports
clients ont conservé leur comportement non fiable.

J'ai également désactivé l'insertion de l'Option 82 et configuré une limite
de débit sur les ports non fiables.

Une limite plus élevée a été utilisée pour la connexion vers le WLC.

## Dynamic ARP Inspection

DAI a ensuite été configuré sur les VLAN concernés.

Les interfaces appropriées ont été déclarées comme trusted et les contrôles
de validation des adresses MAC et IP ont été activés.

Les vérifications effectuées sur ASW-A1 ne montraient pas de blocage de trafic
par DAI.

### Captures prévues

- `captures/phase-07/acl-verification.png`
- `captures/phase-07/port-security.png`
- `captures/phase-07/dhcp-snooping.png`
- `captures/phase-07/dai.png`


---

# Phase 8 — IPv6

La phase suivante était consacrée à l'introduction d'IPv6 dans
l'infrastructure.

J'ai commencé par activer le routage IPv6 sur les équipements de couche 3.

Sur R1, j'ai configuré les interfaces connectées aux deux réseaux externes
avec les préfixes IPv6 prévus dans le lab.

Les liaisons entre R1 et les switches multilayer utilisent également
l'adressage EUI-64.

Sur CSW1 et CSW2, l'interface Port-Channel de niveau 3 a simplement été
activée pour IPv6 conformément aux instructions du lab.

Enfin, deux routes par défaut IPv6 ont été configurées sur R1 :

- une route principale ;
- une route flottante avec une distance administrative supérieure.

Cette configuration permet de conserver un chemin de secours vers
l'extérieur.

### Captures prévues

- `captures/phase-08/ipv6-interface.png`
- `captures/phase-08/ipv6-routing-table.png`
- `captures/phase-08/ipv6-default-route.png`

---

# Phase 9 — Wireless

La dernière phase du lab concernait la partie sans fil avec le WLC1 et les
points d'accès légers.

L'objectif était notamment de configurer :

- l'interface dynamique Wi-Fi ;
- le VLAN 40 ;
- le réseau `10.6.0.0/24` ;
- le WLAN ;
- le SSID `Wi-Fi` ;
- la sécurité WPA2/AES/PSK.

## Vérification de la liaison WLC

La liaison entre ASW-A1 et WLC1 a été vérifiée.

Le port `Fa0/2` de ASW-A1 est bien :

- opérationnel ;
- en trunk 802.1Q ;
- avec le VLAN natif 99 ;
- avec les VLAN 40 et 99 autorisés ;
- avec les VLAN 40 et 99 actifs ;
- en état forwarding pour ces VLAN.

La configuration du trunk est donc cohérente.

## Problème rencontré avec le WLC

Malgré cela, je n'ai pas réussi à accéder à l'interface de gestion du WLC1
depuis les postes clients.

Le WLC utilise :

`10.0.0.7/28`

avec :

`10.0.0.1`

comme passerelle.

Plusieurs vérifications ont été réalisées :

- état de l'interface du WLC ;
- adresse IP et masque ;
- passerelle ;
- VLAN de management ;
- trunk ASW-A1 ↔ WLC1 ;
- VLAN 40 et VLAN 99 ;
- routage inter-VLAN ;
- table ARP ;
- table MAC ;
- DHCP Snooping ;
- Dynamic ARP Inspection ;
- état STP.

Le WLC était joignable depuis DSW-A1 dans le VLAN de management.

En revanche, même en utilisant une adresse source du VLAN 10 depuis DSW-A1,
le ping vers `10.0.0.7` échouait.

J'ai donc décidé de ne pas modifier davantage la configuration au hasard.

### État final

La partie Wireless n'a donc pas pu être validée complètement dans mon
environnement Packet Tracer.

La liaison physique et le trunk vers le WLC ont été vérifiés, mais l'accès
à l'interface de gestion et la finalisation complète du WLAN n'ont pas pu
être validés.

### Captures

- `captures/phase-09/wlc-trunk-verification.png`
- `captures/phase-09/wlc-connectivity.png`
- `captures/phase-09/wlc-access.png`

---

# Suite du projet

Après les différentes phases de configuration, je vais effectuer une
campagne de tests globale afin de vérifier le fonctionnement de
l'infrastructure.

Cette étape permettra notamment de vérifier :

- la connectivité entre les différents VLAN ;
- HSRP ;
- OSPF ;
- DHCP ;
- DNS ;
- NAT/PAT ;
- ACL ;
- Port Security ;
- DHCP Snooping ;
- Dynamic ARP Inspection ;
- IPv6 ;
- les différents services réseau.

Les résultats de ces tests seront regroupés dans le document
`docs/plan-de-tests.md`.

Les configurations finales des équipements seront également exportées dans
le dossier `configs/`.


# État final du projet

La configuration principale du Mega Lab a été réalisée.

Les dernières étapes consistent principalement à effectuer les tests prévus et à prendre les captures d'écran finales.

Certaines fonctionnalités que j'ai pas réussis à faire notamment l'accès au WLC et pour le transfert FTP de l'image IOS, c'est due à la limitation 
de cisco packet tracer ou de mon PC qui fait que ça prend beaucoup de temps lors du transfet. 

