# OverTheWire Bandit – Levels 7 to 12

## Présentation

Ce document présente mon parcours de progression sur **[OverTheWire – Bandit](https://overthewire.org/wargames/bandit/)**, à travers les niveaux 7 à 12.

L'objectif de ces exercices est de développer mes compétences en ligne de commande Linux, en apprenant à rechercher, examiner, manipuler et transformer des fichiers.

Chaque niveau m'a permis de découvrir ou de renforcer l'utilisation de différentes commandes Linux.

---

## Niveaux réalisés

### Level 7 → Level 8

**Objectif :** Trouver le mot de passe suivant dans un fichier contenant plusieurs lignes de texte.

**Commande utilisée :**

```bash
grep "millionth" data.txt
```

**Notions apprises :**
- Recherche de texte dans un fichier.
- Utilisation de `grep`.
- Identification d'une information précise dans un grand fichier.

**Capture d'écran :**

![Bandit Level 7](screenshots_bandit/bandit7.png)

---

### Level 8 → Level 9

**Objectif :** Identifier la ligne qui apparaît une seule fois dans un fichier.

**Commandes utilisées :**

```bash
sort data.txt | uniq -u
```

**Notions apprises :**
- Trier le contenu d'un fichier avec `sort`.
- Identifier les lignes uniques avec `uniq`.
- Comprendre le fonctionnement du pipe (`|`) pour combiner plusieurs commandes.

**Capture d'écran :**

![Bandit Level 8](screenshots_bandit/bandit8.png)

---

### Level 9 → Level 10

**Objectif :** Extraire une information lisible située parmi des données non textuelles.

**Commandes utilisées :**

```bash
strings data.txt
```

```bash
strings data.txt | grep "==="
```

**Notions apprises :**
- Extraction de chaînes de caractères lisibles avec `strings`.
- Recherche de motifs avec `grep`.
- Première approche de l'analyse de fichiers contenant des données binaires.

**Captures d'écran :**

![Bandit Level 9 - Recherche](screenshots_bandit/bandit9-1.png)

![Bandit Level 9 - Résultat](screenshots_bandit/bandit9-2.png)

---

### Level 10 → Level 11

**Objectif :** Décoder un contenu encodé en Base64.

**Commandes utilisées :**

```bash
base64 -d data.txt
```

**Notions apprises :**
- Comprendre le principe de l'encodage Base64.
- Décoder des données avec `base64 -d`.
- Faire la différence entre encodage et chiffrement.

**Capture d'écran :**

![Bandit Level 10](screenshots_bandit/bandit10.png)

---

### Level 11 → Level 12

**Objectif :** Retrouver une information transformée à l'aide d'un décalage de caractères (ROT13).

**Commande utilisée :**

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Notions apprises :**
- Comprendre le principe de ROT13.
- Transformer des caractères avec `tr`.
- Comprendre qu'une transformation de caractères peut être inversée en appliquant la même transformation.

**Capture d'écran :**

![Bandit Level 11](screenshots_bandit/bandit11.png)

---

### Level 12 → Level 13

**Objectif :** Retrouver une information cachée dans un fichier contenant un hexdump d'un fichier compressé plusieurs fois avec différents formats.

**Commandes et outils utilisés :**

```bash
mktemp -d
```

```bash
xxd -r
```

```bash
file
```

```bash
gzip
```

```bash
bzip2
```

```bash
tar
```

**Notions apprises :**
- Création d'un répertoire temporaire avec `mktemp -d`.
- Conversion d'un hexdump en données binaires avec `xxd -r`.
- Identification du format réel d'un fichier avec `file`.
- Décompression de fichiers avec `gzip` et `bzip2`.
- Extraction d'archives avec `tar`.
- Compréhension des fichiers compressés et des transformations successives.

**Une petite difficulté rencontrée :**

Lors de ma première tentative, j'ai utilisé `mktemp -d` pour créer un répertoire temporaire. Ma session s'est ensuite interrompue.

Lors de ma deuxième tentative, j'ai exécuté à nouveau `mktemp -d`. Un nouveau répertoire temporaire a donc été créé avec un nom différent.

J'ai poursuivi les manipulations dans ce nouveau répertoire.

Cette expérience m'a permis de mieux comprendre que `mktemp -d` génère un nouveau nom unique à chaque exécution et qu'il faut conserver le chemin du répertoire utilisé pendant le travail.

**Captures d'écran :**

![Bandit Level 12 - Étape 1](screenshots_bandit/bandit12-1.png)

![Bandit Level 12 - Étape 2](screenshots_bandit/bandit12-2.png)

![Bandit Level 12 - Étape 3](screenshots_bandit/bandit12-3.png)

![Bandit Level 12 - Étape 4](screenshots_bandit/bandit12-4.png)

![Bandit Level 12 - Étape 5](screenshots_bandit/bandit12-5.png)

---

## Commandes Linux découvertes ou renforcées

| Commande | Utilité |
|---|---|
| `grep` | Rechercher du texte |
| `sort` | Trier des lignes |
| `uniq` | Identifier les lignes uniques ou les doublons |
| `strings` | Extraire les chaînes lisibles |
| `base64` | Encoder et décoder en Base64 |
| `tr` | Transformer des caractères |
| `mktemp -d` | Créer un répertoire temporaire |
| `xxd` | Afficher des données en hexadécimal |
| `file` | Identifier le type réel d'un fichier |
| `gzip` | Compresser et décompresser |
| `bzip2` | Compresser et décompresser |
| `tar` | Créer et extraire des archives |

## Bilan personnel

Ces niveaux m'ont permis de renforcer mes connaissances en Linux et de découvrir de nouvelles méthodes pour manipuler et examiner des fichiers.

J'ai notamment appris à combiner plusieurs commandes grâce au pipe (`|`), à décoder du Base64, à utiliser ROT13 et à travailler avec des fichiers compressés.

Je continue à pratiquer Linux et à documenter progressivement mon apprentissage dans le cadre de mon parcours en cybersécurité.

---

**Plateforme :** OverTheWire – Bandit  
**Progression documentée :** Level 7 → Level 12  
**Environnement :** Kali Linux / Ubuntu  
**Domaine :** Linux Fundamentals & Cybersecurity