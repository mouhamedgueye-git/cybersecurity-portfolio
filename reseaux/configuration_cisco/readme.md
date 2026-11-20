# 🔐 Administration et Sécurisation de Base des Équipements Cisco IOS

Ce laboratoire **Cisco Packet Tracer** porte sur la prise en main de la ligne de commande (**CLI Cisco IOS**) d'un routeur (`ISR4331`) et d'un commutateur (`Switch 2960`). 

L'objectif principal de ce projet est de maîtriser la configuration initiale d'un équipement réseau, sa personnalisation (nom d'hôte et bannière d'avertissement) et surtout de comprendre **l'impact sur la sécurité du choix des commandes d'authentification** (`enable password` vs `enable secret`).

---

## 🎯 Compétences Clés Acquises & Concepts de Sécurité

* **Comparaison `enable password` vs `enable secret` :** Compréhension critique de la manière dont Cisco IOS stocke les mots de passe dans la configuration active (`running-config`) :
  * **`enable password` :** Stocke le mot de passe **en texte clair** (lisible par n'importe qui ayant accès au fichier de configuration).
  * **`enable secret` :** Applique un algorithme de **hachage cryptographique** (MD5 / Type 5) avant de stocker le mot de passe, empêchant sa lecture directe.
* **Personnalisation et Identification (`hostname`) :** Attribution d'un nom unique aux équipements (`R1` et `SW1`) pour faciliter leur repérage dans la topologie et l'administration.
* **Bannière Légale (`banner motd`) :** Configuration d'un message d'avertissement affiché avant toute connexion pour informer les utilisateurs des restrictions d'accès.
* **Utilisation de la commande `do` et activation des interfaces :** Exécution de commandes de vérification (`do show ...`) directement depuis les modes de configuration, adressage IP et activation des ports (`no shutdown`).

---

## 📋 Table des Matières

1. [Topologie du Réseau](#-topologie-du-réseau)
2. [Configuration du Switch (`SW1`) et Analyse du Mot de Passe en Clair](#-configuration-du-switch-sw1-et-analyse-du-mot-de-passe-en-clair)
3. [Configuration du Routeur (`R1`) et Hachage du Mot de Passe](#-configuration-du-routeur-r1-et-hachage-du-mot-de-passe)
4. [Configuration de l'Interface Réseau et Vérification (`show ip interface brief`)](#-configuration-de-linterface-réseau-et-vérification-show-ip-interface-brief)
5. [Synthèse des Bonnes Pratiques](#-synthèse-des-bonnes-pratiques)

---

## 🗺️ Topologie du Réseau

L'infrastructure est composée d'un routeur (`Router0` renommé `R1`), d'un switch (`Switch0` renommé `SW1`) et de trois postes clients (`PC0`, `PC1`, `PC2`).

![Topologie du réseau](topologie_reseaux.png)

---

## ⚠️ Configuration du Switch (`SW1`) et Analyse du Mot de Passe en Clair

Sur le commutateur, nous avons configuré le nom d'hôte, la bannière de connexion ainsi que l'accès au mode privilégié à l'aide de la commande **`enable password`**.

### Commandes appliquées sur le Switch :

```cisco
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# banner motd #acces reserve au personnel autorise.#
SW1(config)# enable password monmotdepasse
```

![Configuration du Switch SW1](configuration_switch.png)

### Constat de sécurité (`running-config`) :

En inspectant la configuration active avec `do show running-config`, on constate une vulnérabilité majeure : **le mot de passe apparaît intégralement en clair** (`enable password monmotdepasse`).

![Mot de passe en clair dans la configuration du Switch](mot_de_passe_en_claire.png)

---

## 🛡️ Configuration du Routeur (`R1`) et Hachage du Mot de Passe

Sur le routeur, nous avons appliqué la bonne pratique de sécurité en remplaçant `enable password` par **`enable secret`**, tout en définissant son nom (`R1`) et sa bannière d'avertissement.

### Commandes appliquées sur le Routeur :

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret monmotdepasse
R1(config)# banner motd #acces reserve au personnel autorise.#
R1(config)# end
```

![Configuration du Routeur R1](configuration_routeur.png)

### Vérification de la bannière et du hachage (`running-config`) :

Lors de la reconnexion, la bannière `acces reserve au personnel autorise.` s'affiche correctement. Dans la configuration active (`do show running-config`), le mot de passe est désormais **haché** (`enable secret 5 $1$mERr$Ap/1q1yLaX9LGLKcfZfD.1`), garantissant sa confidentialité.

![Mot de passe haché et bannière sur le Routeur R1](mot_de_passe_hashe.png)

---

## 🌐 Configuration de l'Interface Réseau et Vérification (`show ip interface brief`)

Enfin, l'interface `GigabitEthernet0/0/0` du routeur `R1` a été configurée avec l'adresse IP `192.168.1.1/24` et activée via `no shutdown`. L'utilisation du préfixe **`do`** a permis de vérifier l'état des interfaces sans quitter le mode de configuration d'interface (`R1(config-if)#`).

```cisco
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# do show ip interface brief
```

![Affichage et vérification des interfaces IP](affichage_des_interface_ip.png)

---

## 📊 Synthèse des Bonnes Pratiques

| Commande Cisco IOS | Rôle | Niveau de Sécurité |
| :--- | :--- | :--- |
| `hostname <nom>` | Définit le nom de l'équipement dans la CLI | Identification |
| `banner motd #<texte>#` | Affiche un message légal avant la connexion | Dissuasion / Conformité |
| `enable password <mdp>` | Protège le mode privilégié (`#`) | ❌ **Faible** (stocké en texte clair) |
| `enable secret <mdp>` | Protège le mode privilégié (`#`) | ✅ **Élevé** (stocké sous forme de hash) |