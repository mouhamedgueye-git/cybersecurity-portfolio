# TP : Hashage MD5 et récupération d'un mot de passe avec Hashcat

## 1. Objectif

L'objectif de ce TP est de comprendre le fonctionnement du hashage et de montrer comment un mot de passe peut être retrouvé lorsqu'il est faible et présent dans un dictionnaire.

Cette manipulation permet également de comprendre pourquoi le hashage ne doit pas être confondu avec le chiffrement.

## 2. Environnement utilisé

- **HashCalc** : génération du hash MD5.
- **Hashcat 8.0.0** : comparaison du hash avec des mots candidats.
- **Windows PowerShell** : exécution des commandes.
- **Dictionnaire personnalisé** : liste de mots candidats.

## 3. Déroulement de la manipulation

### Étape 1 : Génération du hash

J'ai utilisé HashCalc pour calculer le hash MD5 du texte `SIMAC`.

Le résultat obtenu est :

```text
78b4699583727462324ed84f21f96134
```

### Étape 2 : Préparation des fichiers

J'ai enregistré le hash dans un fichier texte nommé `hash.txt`.

J'ai également créé un dictionnaire nommé `dictionnaire_tp.txt`, contenant plusieurs mots candidats, dont le mot d'origine `SIMAC`.

### Étape 3 : Utilisation de Hashcat

J'ai utilisé Hashcat pour comparer le hash avec les mots présents dans mon dictionnaire.

Commande utilisée :

```powershell
.\hashcat.exe -m 0 -a 0 "C:\Users\GO\OneDrive\Documents\hash.txt" "C:\Users\GO\OneDrive\Documents\dictionnaire_tp.txt"
```

- `-m 0` : indique le type de hash MD5.
- `-a 0` : sélectionne le mode d'attaque par dictionnaire.
- `hash.txt` : contient le hash à tester.
- `dictionnaire_tp.txt` : contient les mots candidats.

### Étape 4 : Résultat

Hashcat a retrouvé le mot `SIMAC`.

Le résultat indique :

```text
Status: Cracked
Recovered: 1/1
```

## 4. Observations et explication

Le résultat montre qu'un hash MD5 peut être associé à son texte d'origine lorsque celui-ci figure dans un dictionnaire testé.

Il est important de comprendre que Hashcat n'a pas inversé mathématiquement le hash.

Le logiciel a calculé le hash de chaque candidat, puis a comparé les résultats avec le hash recherché. Lorsqu'une correspondance a été trouvée, le mot candidat a été identifié.

## 5. Conclusion

Ce TP m'a permis de comprendre qu'un mot de passe faible peut être retrouvé par une attaque utilisant un dictionnaire, même si le mot de passe est stocké sous forme de hash.

Cela montre également les limites de l'utilisation de MD5 pour protéger des mots de passe.

Pour le stockage des mots de passe, il est préférable d'utiliser des fonctions adaptées comme Argon2id, scrypt ou bcrypt, avec un salt unique.

**Note :** cette démonstration a été réalisée avec un texte et un dictionnaire de test personnels.