# 🌐 Configuration d'un Réseau Local avec Serveur DHCP (Cisco Packet Tracer)

Ce projet présente la mise en place et la configuration d'un petit réseau local (LAN) sous **Cisco Packet Tracer**. L'objectif principal est d'automatiser l'attribution des paramètres réseau (adresses IPv4, masques de sous-réseau et passerelle par défaut) aux postes clients grâce à un **serveur DHCP dédié**.

---

## 📋 Table des matières
1. [Architecture et Topologie du Réseau](#-architecture-et-topologie-du-réseau)
2. [Plan d'Adressage IP](#-plan-dadressage-ip)
3. [Étapes de Configuration](#-étapes-de-configuration)
   - [1. Configuration du Routeur (Passerelle)](#1-configuration-du-routeur-passerelle)
   - [2. Configuration Réseau du Serveur](#2-configuration-réseau-du-serveur)
   - [3. Configuration du Service DHCP](#3-configuration-du-service-dhcp)
   - [4. Configuration des Postes Clients (PC)](#4-configuration-des-postes-clients-pc)
4. [Tests et Vérification de la Connectivité](#-tests-et-vérification-de-la-connectivité)

---

## 🗺️ Architecture et Topologie du Réseau

Le réseau est structuré autour d'un commutateur central (Switch) reliant l'ensemble des équipements :
* **1 Routeur** (`Router0` ) faisant office de passerelle par défaut.
* **1 Switch** (`Switch0` ) assurant l'interconnexion locale.
* **1 Serveur** (`Server0`) hébergeant le service DHCP.
* **3 Postes clients** (`PC0`, `PC1`, `PC2`).

![Topologie du réseau](topologie_du_reseaux.png)

---

## 📊 Plan d'Adressage IP

| Équipement | Interface | Adresse IPv4 | Masque de sous-réseau | Passerelle par défaut | Mode d'attribution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Router0** | `GigabitEthernet0/0/0` | `192.168.1.1` | `255.255.255.0` | N/A | Statique |
| **Server0** | `FastEthernet0` | `192.168.1.100` | `255.255.255.0` | `192.168.1.1` | Statique |
| **PC0** | `FastEthernet0` | `192.168.1.2` | `255.255.255.0` | `192.168.1.1` | Dynamique (DHCP) |
| **PC1** | `FastEthernet0` | `192.168.1.3` | `255.255.255.0` | `192.168.1.1` | Dynamique (DHCP) |
| **PC2** | `FastEthernet0` | `192.168.1.4` | `255.255.255.0` | `192.168.1.1` | Dynamique (DHCP) |

---

## ⚙️ Étapes de Configuration

### 1. Configuration du Routeur (Passerelle)
L'interface `GigabitEthernet0/0/0` du routeur est configurée via l'interface en ligne de commande (CLI) pour servir de passerelle par défaut (`192.168.1.1`) au réseau local.

```ios
Router> enable
Router# configure terminal
Router(config)# interface gigabitEthernet 0/0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
```

![Configuration du routeur](configuration_du_routeur.png)

---

### 2. Configuration Réseau du Serveur
Un serveur DHCP doit impérativement posséder une adresse IP fixe. Nous attribuons donc manuellement l'adresse `192.168.1.100` à `Server0` ainsi que l'adresse de la passerelle par défaut (`192.168.1.1`).

* **Attribution de l'adresse IPv4 statique :**
![Attribution d'une adresse IP au serveur](attribution_d_un_address_ip_au_serveur.png)

* **Configuration de la passerelle par défaut :**
![Attribution du Default Gateway au serveur](attribution_du_default_gateway_au_serveur.png)

---

### 3. Configuration du Service DHCP
Dans l'onglet **Services > DHCP** du serveur, le service est activé (`On`) et un pool d'adresses nommé `serverPool` est configuré avec les paramètres suivants :
* **Default Gateway :** `192.168.1.1`
* **Start IP Address :** `192.168.1.2` *(l'adresse `.1` étant déjà réservée au routeur)*
* **Subnet Mask :** `255.255.255.0`
* **Maximum Number of Users :** `254`

![Configuration du serveur DHCP](configuration_du_serveur_dhcp.png)

---

### 4. Configuration des Postes Clients (PC)
Sur chaque ordinateur (`PC0`, `PC1`, `PC2`), l'option **DHCP** est sélectionnée dans les paramètres de configuration IP (`Config > FastEthernet0` et `Global Settings`).

* **Activation du mode DHCP sur les terminaux :**
![Configuration des terminaux](configuration_des_termineaux.png)

* **Réception automatique de l'adresse IP (`192.168.1.2` pour PC0) :**
![Attribution d'une adresse IP aux PC par le serveur](attribution_d'_un_address_ip_aux_pc_par_le_serveur.png)

* **Réception automatique de la passerelle par défaut (`192.168.1.1`) :**
![Attribution du Default Gateway aux PC par le serveur](attribution_du_default_gateway_aux_pc_par_le_serveur.png)

---

## ✅ Tests et Vérification de la Connectivité

Pour valider le bon fonctionnement du réseau et de l'attribution DHCP, des tests de connectivité (`ping`) ont été effectués depuis l'invite de commandes de **PC0** (`192.168.1.2`) vers **PC1** (`192.168.1.3`) et **PC2** (`192.168.1.4`).

```cmd
C:\> ping 192.168.1.3
C:\> ping 192.168.1.4
```

Les réponses reçues avec **0 % de perte de paquets** confirment que le réseau est pleinement opérationnel.

![Vérification de la connectivité](verification_de_la_conectivite.png)

---