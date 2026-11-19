*//////////# 🌐 Architecture Multi-LAN avec Relais DHCP (`ip helper-address`) et Pools Multiples

Ce projet **Cisco Packet Tracer** présente la conception et la configuration d'une architecture réseau segmentée en deux sous-réseaux distincts (**LAN 1** et **LAN 2**) interconnectés par un routeur central. 

L'objectif principal de ce laboratoire est de démontrer comment **un seul serveur DHCP centralisé** (situé dans le LAN 1) peut distribuer dynamiquement des adresses IP à des machines situées dans un réseau distant (LAN 2) grâce à la gestion des **pools DHCP multiples** et à la configuration d'un **agent de relais DHCP** via la commande Cisco IOS `ip helper-address`.

---

## 🎯 Compétences Clés Acquises & Concepts Mis en Œuvre

* **Gestion des Pools DHCP Multiples :** Configuration d'un serveur DHCP capable de gérer simultanément plusieurs étendues d'adresses (`serverPool` pour le réseau `192.168.1.0/24` et `DHCP_LAN2` pour le réseau `192.168.2.0/24`).
* **Relais DHCP (`ip helper-address`) :** Compréhension du fonctionnement des requêtes de diffusion (*Broadcast* DHCP Discover). Par défaut, un routeur bloque les diffusions entre deux réseaux. L'utilisation de la commande `ip helper-address` sur l'interface du LAN 2 permet de convertir ces requêtes en paquets *Unicast* et de les relayer directement vers le serveur DHCP du LAN 1.
* **Routage Inter-LAN :** Configuration des interfaces d'un routeur Cisco ISR4331 pour assurer la communication entre deux sous-réseaux distincts.

---

## 📋 Table des Matières

1. [Architecture et Topologie du Réseau](#-architecture-et-topologie-du-réseau)
2. [Plan d'Adressage IP](#-plan-dadressage-ip)
3. [Étapes de Configuration](#-étapes-de-configuration)
   * [1. Configuration des Interfaces du Routeur](#1-configuration-des-interfaces-du-routeur)
   * [2. Configuration de l'Agent de Relais DHCP (`ip helper-address`)](#2-configuration-de-lagent-de-relais-dhcp-ip-helper-address)
   * [3. Configuration Réseau du Serveur Central](#3-configuration-réseau-du-serveur-central)
   * [4. Création des Pools sur le Serveur DHCP](#4-création-des-pools-sur-le-serveur-dhcp)
   * [5. Configuration des Postes Clients (PC)](#5-configuration-des-postes-clients-pc)
4. [Vérification et Tests de Connectivité](#-vérification-et-tests-de-connectivité)

---

## 🗺️ Architecture et Topologie du Réseau

L'infrastructure est divisée en deux réseaux locaux distincts reliés par `Router0` :

* **LAN 1 (`192.168.1.0/24` - Zone Bleue) :** Contient le serveur DHCP central (`Server0`) ainsi que deux postes clients (`PC0`, `PC1`) reliés via `Switch0`.
* **LAN 2 (`192.168.2.0/24` - Zone Verte) :** Contient trois postes clients (`PC2`, `PC3`, `PC4`) reliés via `Switch1`, sans serveur DHCP local.

![Topologie du réseau multi-LAN](topologie_du_reseaux_2.png)

---

## 📊 Plan d'Adressage IP

| Équipement | Réseau | Interface | Adresse IPv4 | Masque | Passerelle | DNS | Attribution |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Router0** | LAN 1 | `GigabitEthernet0/0/0` | `192.168.1.1` | `255.255.255.0` | N/A | N/A | Statique |
| **Router0** | LAN 2 | `GigabitEthernet0/0/1` | `192.168.2.1` | `255.255.255.0` | N/A | N/A | Statique |
| **Server0** | LAN 1 | `FastEthernet0` | `192.168.1.2` | `255.255.255.0` | `192.168.1.1` | `8.8.8.8` | Statique |
| **PC0 / PC1** | LAN 1 | `FastEthernet0` | `192.168.1.3` - `.4` | `255.255.255.0` | `192.168.1.1` | `8.8.8.8` | DHCP (`serverPool`) |
| **PC2 / PC3 / PC4** | LAN 2 | `FastEthernet0` | `192.168.2.2` - `.4` | `255.255.255.0` | `192.168.2.1` | `8.8.8.8` | DHCP (`DHCP_LAN2`) |

---

## ⚙️ Étapes de Configuration

### 1. Configuration des Interfaces du Routeur

Chaque interface du routeur est configurée pour servir de passerelle par défaut à son réseau respectif :

```cisco
Router> enable
Router# configure terminal

! Configuration de la passerelle du LAN 1
Router(config)# interface gigabitEthernet 0/0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

! Configuration de la passerelle du LAN 2
Router(config)# interface gigabitEthernet 0/0/1
Router(config-if)# ip address 192.168.2.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# end
```

![Configuration des interfaces du routeur](configuration_des_interface_du_rouiteur.png)

---

### 2. Configuration de l'Agent de Relais DHCP (`ip helper-address`)

> **💡 Pourquoi cette étape est cruciale ?**  
> Lorsqu'un PC du **LAN 2** démarre, il envoie une requête `DHCP Discover` en *broadcast* (`255.255.255.255`). Les routeurs bloquant par défaut les diffusions, cette requête n'atteindrait jamais `Server0` situé dans le **LAN 1**. 
> 
> En configurant `ip helper-address 192.168.1.2` sur l'interface d'entrée du LAN 2 (`GigabitEthernet0/0/1`), le routeur intercepte les requêtes DHCP et les transfère directement en *unicast* vers l'adresse IP du serveur DHCP.

```cisco
Router> enable
Router# configure terminal
Router(config)# interface gigabitEthernet 0/0/1
Router(config-if)# ip helper-address 192.168.1.2
Router(config-if)# end
```

![Configuration de la commande ip helper-address](commande_ip_helper.png)

---

### 3. Configuration Réseau du Serveur Central

Le serveur `Server0` est configuré avec une adresse IP fixe (`192.168.1.2`) afin d'être joignable de manière fiable par les clients locaux et par l'agent de relais du routeur.

* **Configuration de l'adresse IPv4 statique (`192.168.1.2`) :**

![Attribution d'adresse IP au serveur](attribution_d_address_ip_aux_serveur.png)

* **Configuration de la passerelle par défaut (`192.168.1.1`) et du serveur DNS (`8.8.8.8`) :**

![Attribution du Default Gateway et du DNS au serveur](attribution_du_default_gateway_et_du_dns_au_serveur.png)

---

### 4. Création des Pools sur le Serveur DHCP

Dans l'onglet **Services > DHCP** de `Server0`, le service est activé et **deux pools distincts** sont créés. Le serveur saura automatiquement dans quel pool puiser en fonction de l'interface du routeur qui a relayé la requête :

1. **`serverPool` (pour le LAN 1) :**
   * **Default Gateway :** `192.168.1.1`
   * **DNS Server :** `8.8.8.8`
   * **Start IP Address :** `192.168.1.3` *(les adresses `.1` et `.2` étant réservées au routeur et au serveur)*
   * **Subnet Mask :** `255.255.255.0`

2. **`DHCP_LAN2` (pour le LAN 2) :**
   * **Default Gateway :** `192.168.2.1`
   * **DNS Server :** `8.8.8.8`
   * **Start IP Address :** `192.168.2.2` *(l'adresse `.1` étant réservée à la passerelle du LAN 2)*
   * **Subnet Mask :** `255.255.255.0`

![Configuration des deux pools du serveur DHCP](configuration_des_deux_pool_du_serveur.png)

---

### 5. Configuration des Postes Clients (PC)

Sur l'ensemble des terminaux des deux sous-réseaux, l'attribution automatique par **DHCP** est activée.

* **Activation du mode DHCP sur les postes clients :**

![Configuration DHCP des PC](configuration_des_pc.png)

* **Réception automatique de l'adresse IP (`192.168.2.2`) et du masque sur PC2 (LAN 2) :**

![Obtention de l'adresse IP et du masque sur PC2](obtention_d_un_address_ip_et_du_masque_de_sous_reseau_aux_pc.png)

* **Réception automatique de la passerelle (`192.168.2.1`) et du DNS (`8.8.8.8`) sur PC2 :**

![Obtention du DNS et du Default Gateway sur PC2](obtention_du_dns_et_du_default_gateway_au_pc.png)

---

## ✅ Vérification et Tests de Connectivité

### 1. Vérification de la configuration IP via `ipconfig`

Depuis l'invite de commandes de **PC2** (situé dans le LAN 2), la commande `ipconfig` confirme que la machine a bien reçu une adresse du pool `DHCP_LAN2` (`192.168.2.2`) grâce au relais DHCP du routeur :

![Vérification via la commande ipconfig](commande_ipconfig.png)

### 2. Tests de communication Inter-LAN (`ping`)

Depuis **PC0** (`192.168.1.3` dans le LAN 1), des requêtes ICMP (`ping`) sont envoyées vers la passerelle du LAN 2 (`192.168.2.1`) ainsi que vers **PC2** (`192.168.2.2` dans le LAN 2) :

```cmd
C:\> ping 192.168.2.1
C:\> ping 192.168.2.2
```

![Vérification de la connectivité inter-LAN](verification_de_la_connectivite.png)

Les réponses reçues confirment que le routage inter-LAN, les pools DHCP multiples et l'agent de relais `ip helper-address` fonctionnent parfaitement.

---

## 🛠️ Prérequis

* **Cisco Packet Tracer** (version 8.0 ou supérieure).
* Ouvrir le fichier de projet `.pkt` associé dans ce dépôt.