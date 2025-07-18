# Conversion de Types en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** Conversion de Types (Explicit vs Implicit)
- **Niveau :** DEBUTANT COMPLET (14 ans, niveau zero)
- **Duree estimee :** 80 minutes
- **Prerequis :** Variables, Types de donnees (string, number, boolean)
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : La difference entre conversion explicite et implicite
- [ ] **Appliquer** : Convertir un type en autre avec les bonnes methodes
- [ ] **Analyser** : Predire comment JavaScript va convertir automatiquement
- [ ] **Creer** : Ecrire du code qui gere correctement les conversions

### Mots-cles a retenir
- **Conversion** : Changer un type de donnee en autre
- **Conversion explicite** : Conversion qu'on fait volontairement
- **Conversion implicite** : Conversion automatique par JavaScript
- **Coercition** : Autre nom pour conversion implicite
- **Parsing** : Analyser du texte pour en extraire un nombre

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Imagine que quelqu'un te donne son age en disant "quatorze" (mot) et tu veux calculer son age dans 5 ans. Tu dois d'abord transformer "quatorze" en nombre 14, puis faire 14 + 5. C'est pareil en programmation !

Parfois tu reçois des nombres sous forme de texte (comme quand quelqu'un tape dans un formulaire), et tu dois les convertir en vrais nombres pour faire des calculs.

### Definition et concepts cles

**Conversion explicite** = TU decides de changer le type
- Tu utilises des fonctions comme Number(), String(), Boolean()
- Tu controles exactement ce qui se passe
- Plus sur et plus clair

**Conversion implicite** = JavaScript decide tout seul
- Arrive automatiquement dans certaines situations
- Peut donner des resultats surprenants
- Aussi appelee "coercition"

**Methodes principales de conversion explicite :**

1. **String()** - Convertit vers texte
2. **Number()** - Convertit vers nombre
3. **Boolean()** - Convertit vers true/false
4. **parseInt()** - Extrait un nombre entier d'un texte
5. **parseFloat()** - Extrait un nombre decimal d'un texte

### Syntaxe de base
```javascript
// Conversion explicite (tu le fais volontairement)
let texte = "123";
let nombre = Number(texte);        // "123" devient 123
let nouveauTexte = String(456);    // 456 devient "456"
let boolean = Boolean(1);          // 1 devient true

// Conversion implicite (JavaScript le fait automatiquement)
let resultat1 = "5" + 3;          // "53" (concatenation)
let resultat2 = "5" - 3;          // 2 (soustraction)
let resultat3 = "5" * 3;          // 15 (multiplication)
```

### Points d'attention
- **Piege + avec strings** : + colle les textes au lieu d'additionner
- **Piege nombres invalides** : Number("abc") donne NaN (Not a Number)
- **Bonne pratique** : Toujours convertir explicitement pour eviter les surprises

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```javascript
// Conversion de texte vers nombre
let ageTexte = "14";              // String recu d'un formulaire
let ageNombre = Number(ageTexte); // Conversion explicite

console.log("Age en texte: " + ageTexte + " (type: " + typeof ageTexte + ")");
console.log("Age en nombre: " + ageNombre + " (type: " + typeof ageNombre + ")");

// Maintenant on peut faire des calculs
let ageDans5Ans = ageNombre + 5;
console.log("Dans 5 ans, j'aurai: " + ageDans5Ans + " ans");

// Conversion de nombre vers texte
let score = 150;
let scoreTexte = String(score);
console.log("Score: " + scoreTexte + " points (type: " + typeof scoreTexte + ")");
```

**Resultat attendu :**
```
Age en texte: 14 (type: string)
Age en nombre: 14 (type: number)
Dans 5 ans, j'aurai: 19 ans
Score: 150 points (type: string)
```

### Exemple intermediaire
```javascript
// Demonstration des conversions implicites
console.log("=== Conversions implicites ===");

// Avec l'operateur +
console.log("5" + 3);      // "53" - concatenation car string + number
console.log(5 + "3");      // "53" - meme resultat
console.log("5" + "3");    // "53" - concatenation de deux strings

// Avec les autres operateurs
console.log("5" - 3);      // 2 - JavaScript convertit en nombres
console.log("5" * 3);      // 15 - multiplication
console.log("15" / 3);     // 5 - division

// Cas speciaux
console.log("abc" - 3);    // NaN - impossible de convertir "abc"
console.log(true + 1);     // 2 - true devient 1
console.log(false + 1);    // 1 - false devient 0
```

### Exemple avance (optionnel)
```javascript
// Fonctions de parsing avancees
let texteComplexe1 = "42px";       // Texte avec unité
let texteComplexe2 = "3.14159";    // Nombre decimal en texte
let texteComplexe3 = "  123  ";    // Nombre avec espaces

// parseInt() - extrait un entier
let entier1 = parseInt(texteComplexe1);    // 42 (ignore "px")
let entier2 = parseInt(texteComplexe2);    // 3 (coupe apres la virgule)

// parseFloat() - extrait un decimal  
let decimal1 = parseFloat(texteComplexe2); // 3.14159
let decimal2 = parseFloat(texteComplexe3); // 123 (ignore les espaces)

console.log("=== Parsing avance ===");
console.log("parseInt('" + texteComplexe1 + "') = " + entier1);
console.log("parseInt('" + texteComplexe2 + "') = " + entier2);
console.log("parseFloat('" + texteComplexe2 + "') = " + decimal1);
console.log("parseFloat('" + texteComplexe3 + "') = " + decimal2);

// Verification des types
console.log("Type de entier1: " + typeof entier1);
console.log("Type de decimal1: " + typeof decimal1);
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Calculatrice avec textes
**Consigne :** L'utilisateur saisit deux nombres sous forme de texte. Convertis-les et fais les 4 operations.

**Code de depart :**
```javascript
// Simule la saisie utilisateur (en realite ca vient d'un formulaire)
let nombre1Texte = "10";
let nombre2Texte = "3";

// A toi de convertir et calculer !
// Complete le code ici
```

**Resultat attendu :** Addition, soustraction, multiplication, division des nombres

**Niveau de difficulte :** 2 etoiles sur 5

### Exercice 2 : Verification d'age
**Consigne :** Verifie si un age saisi en texte est valide (nombre entre 0 et 120)

**Contraintes :**
- Convertir le texte en nombre
- Verifier que c'est un vrai nombre (pas NaN)
- Verifier que c'est dans la bonne gamme

**Niveau de difficulte :** 3 etoiles sur 5

### Exercice 3 : Parsing de donnees complexes
**Consigne :** Tu reçois des donnees sous forme "25kg", "1.80m", "18ans". Extrais les nombres.

**Aide :** Utilise parseInt() et parseFloat()

**Niveau de difficulte :** 4 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```javascript
// Simule la saisie utilisateur
let nombre1Texte = "10";
let nombre2Texte = "3";

// Conversion explicite en nombres
let nombre1 = Number(nombre1Texte);
let nombre2 = Number(nombre2Texte);

// Verification que les conversions ont marche
console.log("Premier nombre: " + nombre1 + " (type: " + typeof nombre1 + ")");
console.log("Deuxieme nombre: " + nombre2 + " (type: " + typeof nombre2 + ")");

// Calculs
let addition = nombre1 + nombre2;
let soustraction = nombre1 - nombre2;
let multiplication = nombre1 * nombre2;
let division = nombre1 / nombre2;

// Affichage des resultats
console.log("=== CALCULATRICE ===");
console.log(nombre1 + " + " + nombre2 + " = " + addition);
console.log(nombre1 + " - " + nombre2 + " = " + soustraction);
console.log(nombre1 + " * " + nombre2 + " = " + multiplication);
console.log(nombre1 + " / " + nombre2 + " = " + division);
```

**Explication :** Number() convertit les strings en vrais nombres, permettant les calculs mathematiques.

### Solution Exercice 2
```javascript
// Simulation d'ages saisis par l'utilisateur
let agesAVerifier = ["25", "abc", "150", "-5", "18"];

for (let i = 0; i < agesAVerifier.length; i++) {
    let ageTexte = agesAVerifier[i];
    let ageNombre = Number(ageTexte);
    
    console.log("=== Verification de: " + ageTexte + " ===");
    
    // Verifier si c'est un nombre valide
    if (isNaN(ageNombre)) {
        console.log("ERREUR: '" + ageTexte + "' n'est pas un nombre valide");
    } else if (ageNombre < 0) {
        console.log("ERREUR: L'age ne peut pas etre negatif");
    } else if (ageNombre > 120) {
        console.log("ERREUR: L'age semble trop eleve");
    } else {
        console.log("OK: Age valide = " + ageNombre + " ans");
    }
}

// FONCTION UTILE :
// isNaN() verifie si quelque chose n'est PAS un nombre
// NaN = Not a Number
```

**Alternatives possibles :**
- Utiliser parseInt() au lieu de Number()
- Ajouter d'autres validations (age minimum pour certaines actions)
- Creer une fonction reutilisable pour valider les ages

### Solution Exercice 3
```javascript
// Donnees complexes a analyser
let donnees = ["25kg", "1.80m", "18ans", "100%", "3.14cm"];

console.log("=== EXTRACTION DE NOMBRES ===");

for (let i = 0; i < donnees.length; i++) {
    let donnee = donnees[i];
    
    // Essayer parseInt (pour entiers)
    let entier = parseInt(donnee);
    
    // Essayer parseFloat (pour decimaux)
    let decimal = parseFloat(donnee);
    
    console.log("Donnee originale: " + donnee);
    console.log("  parseInt() = " + entier);
    console.log("  parseFloat() = " + decimal);
    
    // Determiner le meilleur resultat
    if (entier === decimal) {
        console.log("  Meilleur resultat: " + entier + " (nombre entier)");
    } else {
        console.log("  Meilleur resultat: " + decimal + " (nombre decimal)");
    }
    console.log("");
}

// EXPLICATION :
// parseInt() s'arrete au premier caractere non-numerique
// parseFloat() fait pareil mais garde les decimales
// Tous deux ignorent les espaces au debut
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Types de donnees** : string, number, boolean pour savoir quoi convertir
- **Variables** : Pour stocker les resultats de conversion
- **Operateurs** : Pour voir les effets des conversions sur les calculs

### Prochaines etapes  
- **Operateurs de comparaison** : Comparer des donnees converties
- **Conditions if/else** : Prendre des decisions selon les conversions
- **Fonctions** : Creer tes propres fonctions de conversion
- **Validation de donnees** : Verifier que les conversions sont valides

### Concepts complementaires
- **Formulaires HTML** : Recuperer des donnees utilisateur (toujours en string)
- **APIs et JSON** : Recevoir des donnees externes a convertir

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer un convertisseur d'unites interactif

**Fonctionnalites requises :**
- [ ] Convertir metres en centimetres
- [ ] Convertir celsius en fahrenheit  
- [ ] Convertir kilos en livres
- [ ] Gerer les erreurs de saisie
- [ ] Afficher les resultats clairement

**Formules utiles :**
- Centimetres = metres * 100
- Fahrenheit = (celsius * 9/5) + 32
- Livres = kilos * 2.205

**Structure de fichiers suggeree :**
```
convertisseur/
├── index.html
├── style.css  
└── script.js
```

**Temps estime :** 60 minutes

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - Number()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Number)
- [MDN - String()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/String)
- [MDN - parseInt()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
- [MDN - parseFloat()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)

### Videos recommandees
- "Conversion de types JavaScript expliquee" - Grafikart (30min)
- "Pieges des conversions implicites" - exemples concrets des erreurs courantes

### Articles approfondis
- "Type coercion en JavaScript" - comprendre les conversions automatiques
- "Validation de donnees utilisateur" - techniques pour verifier les conversions

### Outils de pratique
- [CodePen - Type conversion playground](https://codepen.io) - pour experimenter
- Console du navigateur - tester Number(), String(), etc.

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre Number("5") et parseInt("5px") ?
   - Reponse : Number() echoue avec "5px", parseInt() extrait 5 et ignore "px"

2. **Question pratique :** Que fait ce code ?
   ```javascript
   let result = "10" - "3";
   console.log(result);
   ```
   - Reponse : Affiche 7 (conversion implicite des strings en numbers)

3. **Question d'application :** Comment convertirais-tu une note "15.5" en nombre ?
   - Reponse : Number("15.5") ou parseFloat("15.5")

### Checklist de maitrise
- [ ] Je sais utiliser Number() pour convertir en nombre
- [ ] Je sais utiliser String() pour convertir en texte
- [ ] Je comprends la difference entre parseInt() et parseFloat()
- [ ] Je peux predire les conversions implicites
- [ ] Je sais detecter les conversions qui echouent (NaN)
- [ ] Je peux choisir la bonne methode selon la situation

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
let age = prompt("Ton age ?");  // Renvoie toujours un string
let ageFutur = age + 5;         // Concatenation au lieu d'addition
console.log(ageFutur);          // "145" si age = "14"
```

**Solution :**
```javascript
let age = prompt("Ton age ?");
let ageNombre = Number(age);    // Conversion explicite
let ageFutur = ageNombre + 5;   // Vraie addition
console.log(ageFutur);          // 19 si age = "14"
```

**Explication :** prompt() renvoie toujours un string. Il faut convertir avant de calculer.

### Erreur frequente 2
**Code problematique :**
```javascript
let resultat = Number("abc");
console.log(resultat + 10);     // NaN + 10 = NaN
```

**Solution :**
```javascript
let texte = "abc";
let nombre = Number(texte);
if (isNaN(nombre)) {
    console.log("Erreur: impossible de convertir '" + texte + "' en nombre");
} else {
    console.log(nombre + 10);
}
```

**Explication :** Toujours verifier si une conversion a reussi avec isNaN() avant d'utiliser le resultat.

### Erreur frequente 3
**Code problematique :**
```javascript
let prix1 = "10.50";
let prix2 = "5.25";
let total = prix1 + prix2;      // "10.505.25" au lieu de 15.75
```

**Solution :**
```javascript
let prix1 = "10.50";
let prix2 = "5.25";
let total = parseFloat(prix1) + parseFloat(prix2);  // 15.75
console.log("Total: " + total + " euros");
```

**Explication :** Avec +, JavaScript colle les strings. Il faut convertir en numbers d'abord.

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Il faut convertir les types pour faire des calculs corrects]
- [Number(), String(), parseInt(), parseFloat() sont mes outils principaux]  
- [JavaScript peut convertir automatiquement mais c'est risque]

### Questions a approfondir
- [Quand utiliser parseInt() vs parseFloat() vs Number() ?]
- [Comment bien valider les donnees utilisateur ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices de conversion)
- [ ] Revision dans 1 semaine (creer un projet avec formulaires)
- [ ] Revision dans 1 mois (expliquer les conversions a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript conversion types explicite implicite Number String parseInt parseFloat coercition

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 4/126*
