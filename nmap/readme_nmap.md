# 🔍 Apprentissage de Nmap — Commande de Base & Options

Ce document récapitule les compétences et commandes de base que j'ai acquises lors de mon apprentissage de **Nmap** (*Network Mapper*).

---

## 💻 Environnement de Laboratoire (Home Lab)

Toutes ces commandes et expérimentations sont réalisées exclusivement dans un environnement virtuel local sécurisé et contrôlé. **Aucun réseau ou machine tierce n'est scanné sans autorisation préalable.**

Mon *lab* de test se compose de 3 machines virtuelles :
* **Kali Linux** : Machine d'attaque / Machine de scan principale.
* **Ubuntu Server/Desktop** : Cible Linux.
* **Windows 11** : Cible Windows.

---

## 🛠️ Options & Commutateurs Appris

### 1. Découverte d'hôtes & Gestion des Pings

* `-sn` (**Ping Scan / No Port Scan**) : Effectue uniquement une découverte d'hôtes sur le réseau local sans scanner les ports.

![Scan Nmap avec option -sn](scan_nmap_avec_option_-sn.png)

* `-Pn` (**No Ping**) : Désactive la découverte d'hôte par ping. Nmap considère que la cible est en ligne et lance directement le scan de ports (très utile si le pare-feu de Windows 11 ou Ubuntu bloque les requêtes ICMP).

![Scan Nmap avec option -Pn](scan_nmap_avec_option_-Pn.png)

### 2. Spécification des Ports

* `-p <port(s)>` : Permet de cibler un ou plusieurs ports spécifiques (ex: `-p 80`, `-p 22,80,443`, `-p 1-1000`).

![Scan Nmap avec option -p 80](scan_nmap_avec_option_-p_80.png)

![Scan Nmap avec option -p](scan_nmap_avec_option_-p.png)

* `-p-` : Scanne **la totalité** des 65 535 ports TCP (au lieu du top 1000 par défaut).

### 3. Détection de Services & Système d'Exploitation

* `-sV` (**Service Version Detection**) : Interroge les ports ouverts pour déterminer la version exacte des services/applications en cours d'exécution.

![Scan Nmap avec option -sV](scan_nmap_avec_option_-sV.png)

* `-O` (**OS Detection**) : Tente d'identifier le système d'exploitation de la machine cible (ex: faire la différence entre l'hôte Ubuntu et l'hôte Windows 11).

![Scan Nmap avec option -O](scan_nmap_avec_option_-O.png)

* `-A` (**Aggressive Scan**) : Active la détection du système d'exploitation (`-O`), la détection de version (`-sV`), le balayage par scripts (`-sC`) et le traceroute complet en une seule option.

![Scan Nmap avec option -A](scan_nmap_avec_option_-A.png)

### 4. Types de Scans

* `-sS` (**SYN Scan / Stealth Scan**) : Scan TCP à demi-ouvert (*Stealth scan*). C'est le scan le plus populaire, rapide et discret (requiert les privilèges `root` sous Kali).

![Scan Nmap avec option -sS](scan_nmap_avec_option_-sS.png)

* `-sT` (**TCP Connect Scan**) : Effectue une connexion TCP complète (Handshake à 3 voies). Utilisé par défaut si l'utilisateur n'a pas les privilèges administrateur.

![Scan Nmap avec option -sT](scan_nmap_avec_option_-sT.png)

* `-sU` (**UDP Scan**) : Scanne les ports UDP (plus lent que le scan TCP, mais indispensable pour découvrir des services comme DNS, DHCP, SNMP).

![Scan Nmap avec option -sU](scan_nmap_avec_option_-sU.png)

### 5. Sauvegarde des Résultats

* `-oN <fichier>` (**Normal Output**) : Redirige et sauvegarde le résultat du scan dans un fichier texte brut lisible pour archivage et analyse ultérieure.

![Scan Nmap avec option -oN](scan_nmap_avec_option_-oN.png)

---

## 💡 Exemple de Commande Exécutée depuis Kali Linux

```bash
sudo nmap -sS -sV -O -Pn -p- -oN scan.txt 192.168.56.102 192.168.56.103