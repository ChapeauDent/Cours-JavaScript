# Structures de Controle en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** Structures de Controle (if/else, switch, boucles)
- **Niveau :** DEBUTANT COMPLET (14 ans, niveau zero)
- **Duree estimee :** 100 minutes
- **Prerequis :** Variables, types de donnees, conversion de types
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Comment prendre des decisions dans ton code
- [ ] **Appliquer** : Utiliser if/else pour des conditions simples
- [ ] **Analyser** : Choisir entre if/else et switch selon la situation
- [ ] **Creer** : Ecrire des boucles pour repeter des actions

### Mots-cles a retenir
- **Condition** : Test qui peut etre vrai ou faux
- **if/else** : Si... sinon (prendre une decision)
- **switch** : Aiguillage entre plusieurs choix
- **Boucle** : Repeter la meme action plusieurs fois
- **Iteration** : Un tour de boucle

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Dans la vraie vie, tu prends constamment des decisions : "S'il pleut, je prends un parapluie, sinon je sors sans". Ou tu repetes des actions : "Je revise mes maths jusqu'a ce que je comprenne".

En programmation, c'est pareil ! Ton programme doit pouvoir :
- Prendre des decisions selon les situations
- Repeter des actions sans que tu réécrives le code 100 fois
- S'adapter selon les donnees qu'il reçoit

### Definition et concepts cles

**Instructions conditionnelles :**
1. **if/else** - Si... sinon
2. **switch** - Aiguillage entre plusieurs cas
3. **Operateur ternaire** - Condition en une ligne

**Boucles (loops) :**
1. **for** - Repeter un nombre fixe de fois
2. **while** - Repeter tant qu'une condition est vraie
3. **do...while** - Faire au moins une fois, puis repeter
4. **for...in** - Parcourir les proprietes d'un objet
5. **for...of** - Parcourir les elements d'un tableau

**Mots-cles de controle :**
- **break** - Sortir d'une boucle
- **continue** - Passer au tour suivant

### Syntaxe de base
```javascript
// Condition simple
if (age >= 18) {
    console.log("Tu es majeur");
} else {
    console.log("Tu es mineur");
}

// Boucle simple
for (let i = 1; i <= 5; i++) {
    console.log("Tour numero " + i);
}

// Switch
switch (jour) {
    case "lundi":
        console.log("Debut de semaine");
        break;
    case "vendredi":
        console.log("Bientot le weekend !");
        break;
    default:
        console.log("Jour normal");
}
```

### Points d'attention
- **Piege comparaison** : Utilise == pour comparer, = pour assigner
- **Piege boucles infinies** : Assure-toi que ta condition finira par devenir fausse
- **Bonne pratique** : Utilise des accolades {} meme pour une seule instruction

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```javascript
// Test d'age simple
let monAge = 14;

if (monAge >= 18) {
    console.log("Je peux voter !");
} else {
    console.log("Je ne peux pas encore voter");
}

// Compter de 1 a 5
console.log("=== Comptage ===");
for (let i = 1; i <= 5; i++) {
    console.log("Je compte : " + i);
}

// Test de note
let maNote = 15;

if (maNote >= 16) {
    console.log("Excellente note !");
} else if (maNote >= 12) {
    console.log("Bonne note !");
} else if (maNote >= 10) {
    console.log("Note correcte");
} else {
    console.log("Il faut travailler plus");
}
```

**Resultat attendu :**
```
Je ne peux pas encore voter
=== Comptage ===
Je compte : 1
Je compte : 2
Je compte : 3
Je compte : 4
Je compte : 5
Bonne note !
```

### Exemple intermediaire
```javascript
// Calculatrice avec switch
let operation = "+";
let nombre1 = 10;
let nombre2 = 3;
let resultat;

switch (operation) {
    case "+":
        resultat = nombre1 + nombre2;
        console.log(nombre1 + " + " + nombre2 + " = " + resultat);
        break;
    case "-":
        resultat = nombre1 - nombre2;
        console.log(nombre1 + " - " + nombre2 + " = " + resultat);
        break;
    case "*":
        resultat = nombre1 * nombre2;
        console.log(nombre1 + " * " + nombre2 + " = " + resultat);
        break;
    case "/":
        if (nombre2 !== 0) {
            resultat = nombre1 / nombre2;
            console.log(nombre1 + " / " + nombre2 + " = " + resultat);
        } else {
            console.log("Erreur : division par zero !");
        }
        break;
    default:
        console.log("Operation inconnue : " + operation);
}

// Boucle while pour deviner un nombre
let nombreSecret = 7;
let tentative = 1;

while (tentative !== nombreSecret) {
    console.log("Tentative " + tentative + " : ce n'est pas le bon nombre");
    tentative++;
    
    // Securite pour eviter boucle infinie
    if (tentative > 10) {
        console.log("Trop de tentatives !");
        break;
    }
}

if (tentative === nombreSecret) {
    console.log("Bravo ! Le nombre etait " + nombreSecret);
}
```

### Exemple avance (optionnel)
```javascript
// Simulation d'un jeu de des
console.log("=== JEU DE DES ===");

let lancers = 5;
let scoreTotal = 0;

for (let tour = 1; tour <= lancers; tour++) {
    // Simuler un de (nombre aleatoire entre 1 et 6)
    let de = Math.floor(Math.random() * 6) + 1;
    scoreTotal += de;
    
    console.log("Tour " + tour + " : tu as fait " + de);
    
    // Bonus pour les 6
    if (de === 6) {
        console.log("  BONUS ! Tu relances !");
        let bonus = Math.floor(Math.random() * 6) + 1;
        scoreTotal += bonus;
        console.log("  Lancer bonus : " + bonus);
    }
    
    // Malédiction pour les 1
    if (de === 1) {
        console.log("  PAS DE CHANCE ! Tu perds 2 points");
        scoreTotal -= 2;
    }
}

console.log("=== RESULTAT ===");
console.log("Score total : " + scoreTotal);

if (scoreTotal >= 25) {
    console.log("EXCELLENT ! Tu es un maitre du de !");
} else if (scoreTotal >= 15) {
    console.log("Bien joue ! Score honorable !");
} else {
    console.log("Pas mal, mais tu peux faire mieux !");
}
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Systeme de notes
**Consigne :** Cree un programme qui donne une mention selon la note

**Code de depart :**
```javascript
let note = 14;

// Complete avec if/else if/else
// 16-20 : Tres bien
// 14-15 : Bien  
// 12-13 : Assez bien
// 10-11 : Passable
// 0-9 : Insuffisant
```

**Niveau de difficulte :** 2 etoiles sur 5

### Exercice 2 : Table de multiplication
**Consigne :** Affiche la table de multiplication d'un nombre avec une boucle for

**Contraintes :**
- Utilise une boucle for de 1 a 10
- Affiche "3 x 1 = 3", "3 x 2 = 6", etc.

**Niveau de difficulte :** 2 etoiles sur 5

### Exercice 3 : Menu interactif
**Consigne :** Cree un menu avec switch qui propose differentes actions

**Options :**
- 1 : Dire bonjour
- 2 : Calculer l'age dans 10 ans
- 3 : Afficher l'heure actuelle
- Autre : Message d'erreur

**Niveau de difficulte :** 3 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```javascript
let note = 14;

console.log("Note obtenue : " + note + "/20");

if (note >= 16 && note <= 20) {
    console.log("Mention : TRES BIEN ! Felicitations !");
} else if (note >= 14 && note <= 15) {
    console.log("Mention : BIEN ! Beau travail !");
} else if (note >= 12 && note <= 13) {
    console.log("Mention : ASSEZ BIEN ! Continue comme ca !");
} else if (note >= 10 && note <= 11) {
    console.log("Mention : PASSABLE ! Tu peux mieux faire !");
} else if (note >= 0 && note <= 9) {
    console.log("Mention : INSUFFISANT ! Il faut plus travailler !");
} else {
    console.log("ERREUR : Note invalide ! Doit etre entre 0 et 20.");
}

// EXPLICATION DES OPERATEURS :
// >= signifie "superieur ou egal"
// <= signifie "inferieur ou egal"  
// && signifie "ET" (les deux conditions doivent etre vraies)
```

**Explication :** L'ordre des conditions est important : on teste d'abord les notes les plus hautes.

### Solution Exercice 2
```javascript
let nombre = 7;  // Table de multiplication du 7

console.log("=== TABLE DE MULTIPLICATION DE " + nombre + " ===");

for (let i = 1; i <= 10; i++) {
    let resultat = nombre * i;
    console.log(nombre + " x " + i + " = " + resultat);
}

// Version plus jolie avec formatage
console.log("=== VERSION FORMATEE ===");
for (let i = 1; i <= 10; i++) {
    let resultat = nombre * i;
    // Ajouter des espaces pour aligner
    let ligne = nombre + " x ";
    if (i < 10) ligne += " ";  // Espace supplementaire pour 1-9
    ligne += i + " = " + resultat;
    console.log(ligne);
}

// EXPLICATION DE LA BOUCLE FOR :
// for (initialisation; condition; increment)
// let i = 1 : on commence a 1
// i <= 10 : on continue tant que i est <= 10
// i++ : on ajoute 1 a i a chaque tour
```

**Alternatives possibles :**
- Demander le nombre a l'utilisateur avec prompt()
- Faire plusieurs tables d'affilee
- Ajouter de la couleur ou du formatage

### Solution Exercice 3
```javascript
// Simulation du choix utilisateur (en realite vient de prompt())
let choixUtilisateur = "2";

console.log("=== MENU PRINCIPAL ===");
console.log("1. Dire bonjour");
console.log("2. Calculer l'age dans 10 ans");  
console.log("3. Afficher l'heure actuelle");
console.log("Votre choix : " + choixUtilisateur);
console.log("");

switch (choixUtilisateur) {
    case "1":
        console.log("=== OPTION 1 ===");
        console.log("Bonjour ! Ravi de te rencontrer !");
        console.log("Passe une excellente journee !");
        break;
        
    case "2":
        console.log("=== OPTION 2 ===");
        let ageActuel = 14;  // En realite demande a l'utilisateur
        let ageFutur = ageActuel + 10;
        console.log("Tu as actuellement " + ageActuel + " ans");
        console.log("Dans 10 ans, tu auras " + ageFutur + " ans");
        break;
        
    case "3":
        console.log("=== OPTION 3 ===");
        let maintenant = new Date();
        console.log("Il est actuellement : " + maintenant.toLocaleTimeString());
        console.log("Date du jour : " + maintenant.toLocaleDateString());
        break;
        
    default:
        console.log("=== ERREUR ===");
        console.log("Choix invalide : " + choixUtilisateur);
        console.log("Veuillez choisir 1, 2 ou 3");
        break;
}

console.log("=== FIN DU PROGRAMME ===");

// POINTS IMPORTANTS :
// - Chaque case se termine par break;
// - default gere tous les cas non prevus
// - On peut mettre plusieurs instructions dans chaque case
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Variables et types** : Pour stocker les valeurs a tester
- **Operateurs de comparaison** : >, <, >=, <=, ==, !=
- **Conversion de types** : Pour comparer des donnees converties
- **Boolean** : Les conditions donnent true ou false

### Prochaines etapes  
- **Operateurs logiques** : &&, ||, ! pour des conditions complexes
- **Fonctions** : Organiser le code conditionnel en blocs reutilisables
- **Arrays** : Utiliser les boucles pour parcourir des listes
- **Evenements** : Reagir aux actions utilisateur avec des conditions

### Concepts complementaires
- **Math.random()** : Generer des nombres aleatoires pour les conditions
- **Date** : Travailler avec les dates dans les conditions

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer un jeu "Pierre, Feuille, Ciseaux" contre l'ordinateur

**Fonctionnalites requises :**
- [ ] Menu pour choisir pierre, feuille ou ciseaux
- [ ] L'ordinateur fait un choix aleatoire
- [ ] Comparer les choix et determiner le gagnant
- [ ] Compter les scores sur plusieurs manches
- [ ] Demander si on veut rejouer

**Regles du jeu :**
- Pierre bat Ciseaux
- Ciseaux bat Feuille  
- Feuille bat Pierre

**Structure de fichiers suggeree :**
```
pierre-feuille-ciseaux/
├── index.html
├── style.css  
└── script.js
```

**Temps estime :** 75 minutes

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - if...else](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/if...else)
- [MDN - switch](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/switch)
- [MDN - for](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/for)
- [MDN - while](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/while)

### Videos recommandees
- "Conditions et boucles JavaScript" - Grafikart (45min)
- "Eviter les boucles infinies" - conseils pratiques pour debutants

### Articles approfondis
- "Optimiser ses conditions" - techniques pour du code plus lisible
- "Quand utiliser for vs while" - guide de choix entre types de boucles

### Outils de pratique
- [CodePen - Conditions playground](https://codepen.io) - tester tes conditions
- [Visualizer.js](http://pythontutor.com/javascript.html) - voir tes boucles en action

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre if et switch ?
   - Reponse : if pour des conditions vraies/fausses, switch pour choisir entre plusieurs valeurs exactes

2. **Question pratique :** Combien de fois cette boucle s'execute ?
   ```javascript
   for (let i = 0; i < 5; i++) {
       console.log(i);
   }
   ```
   - Reponse : 5 fois (i = 0, 1, 2, 3, 4)

3. **Question d'application :** Comment ferais-tu pour repeter une action jusqu'a ce que l'utilisateur tape "stop" ?
   - Reponse : Boucle while avec condition sur la reponse utilisateur

### Checklist de maitrise
- [ ] Je sais ecrire une condition if/else simple
- [ ] Je comprends comment utiliser switch avec break
- [ ] Je peux creer une boucle for qui compte
- [ ] Je comprends la difference entre for et while
- [ ] Je sais eviter les boucles infinies
- [ ] Je peux combiner conditions et boucles

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
let age = 14;
if (age = 18) {    // = au lieu de ==
    console.log("Majeur");
}
```

**Resultat inattendu :** S'execute toujours car age = 18 assigne 18 a age

**Solution :**
```javascript
let age = 14;
if (age == 18) {    // == pour comparer
    console.log("Majeur");
} else {
    console.log("Mineur");
}
```

**Explication :** = assigne une valeur, == compare deux valeurs. Attention a ne pas les confondre !

### Erreur frequente 2
**Code problematique :**
```javascript
for (let i = 1; i > 0; i++) {  // Condition toujours vraie
    console.log(i);             // Boucle infinie !
}
```

**Solution :**
```javascript
for (let i = 1; i <= 5; i++) {  // Condition qui finira par etre fausse
    console.log(i);
}
```

**Explication :** Dans une boucle, assure-toi que la condition finira par devenir fausse, sinon boucle infinie !

### Erreur frequente 3
**Code problematique :**
```javascript
switch (jour) {
    case "lundi":
        console.log("Debut de semaine");
        // Oubli du break !
    case "mardi":
        console.log("Mardi");
        break;
}
```

**Resultat inattendu :** Si jour = "lundi", affiche les deux messages

**Solution :**
```javascript
switch (jour) {
    case "lundi":
        console.log("Debut de semaine");
        break;    // Important !
    case "mardi":
        console.log("Mardi");
        break;
}
```

**Explication :** Sans break, JavaScript continue d'executer les cases suivants. C'est le "fall-through".

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Les conditions permettent a mon programme de prendre des decisions]
- [Les boucles permettent de repeter du code sans le recopier]  
- [Il faut faire attention aux boucles infinies et aux break oublies]

### Questions a approfondir
- [Comment combiner plusieurs conditions avec && et || ?]
- [Quelles sont les autres types de boucles avancees ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices de conditions)
- [ ] Revision dans 1 semaine (creer un petit jeu avec boucles)
- [ ] Revision dans 1 mois (expliquer if/else et for a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript conditions if else switch boucles for while controle flux decision

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 5/126*
