# Types de Donnees en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** Types de Donnees (string, number, boolean, etc.)
- **Niveau :** DEBUTANT COMPLET (14 ans, niveau zero)
- **Duree estimee :** 90 minutes
- **Prerequis :** Variables et declarations (let, const)
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Les differents types de donnees en JavaScript
- [ ] **Appliquer** : Utiliser string, number, boolean dans tes programmes
- [ ] **Analyser** : Identifier le type d'une donnee avec typeof
- [ ] **Creer** : Ecrire du code qui manipule differents types de donnees

### Mots-cles a retenir
- **Type de donnee** : La "famille" d'une information (texte, nombre, etc.)
- **string** : Type pour le texte (mots, phrases)
- **number** : Type pour les nombres (entiers et decimaux)
- **boolean** : Type pour vrai/faux (true/false)
- **typeof** : Operateur pour connaitre le type d'une donnee

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Imagine que tu ranges tes affaires : tu mets les livres avec les livres, les stylos avec les stylos, etc. C'est pareil en programmation ! JavaScript a besoin de savoir si une information est du texte, un nombre, ou autre chose pour savoir comment la traiter.

Par exemple, si tu ecris "5" + "3", JavaScript va coller les textes pour donner "53". Mais si tu ecris 5 + 3, il va calculer et donner 8. La difference vient du TYPE de donnee !

### Definition et concepts cles

JavaScript a plusieurs **types primitifs** (de base) :

1. **string** - Pour le texte
   - Exemples : "Bonjour", "Alex", "J'ai 14 ans"
   - Toujours entre guillemets

2. **number** - Pour les nombres  
   - Exemples : 14, 3.14, -5, 1000
   - Pas de guillemets pour les nombres

3. **boolean** - Pour vrai/faux
   - Seulement deux valeurs : true ou false
   - Utile pour les decisions

4. **undefined** - Non defini
   - Quand une variable existe mais n'a pas de valeur

5. **null** - Volontairement vide
   - Quand on veut dire "rien" explicitement

### Syntaxe de base
```javascript
// Types de donnees de base
let monNom = "Alex";              // string (texte)
let monAge = 14;                  // number (nombre)
let suisEtudiant = true;          // boolean (vrai/faux)
let maNote;                       // undefined (pas encore defini)
let monArgentDePoche = null;      // null (volontairement vide)

// Verifier le type avec typeof
console.log(typeof monNom);       // Affiche: string
console.log(typeof monAge);       // Affiche: number
console.log(typeof suisEtudiant); // Affiche: boolean
```

### Points d'attention
- **Piege guillemets** : "14" est un texte, 14 est un nombre - attention a la difference !
- **Piege boolean** : true/false s'ecrivent sans guillemets
- **Bonne pratique** : Utilise typeof pour verifier tes types quand tu doutes

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```javascript
// Mes informations avec differents types
let prenom = "Sarah";           // string - mon prenom
let age = 14;                   // number - mon age en annees
let estEnVacances = true;       // boolean - je suis en vacances
let moyenneGenerale = 15.5;     // number - ma moyenne (decimal)

// Afficher les informations et leurs types
console.log("Prenom: " + prenom + " (type: " + typeof prenom + ")");
console.log("Age: " + age + " (type: " + typeof age + ")");
console.log("En vacances: " + estEnVacances + " (type: " + typeof estEnVacances + ")");
console.log("Moyenne: " + moyenneGenerale + " (type: " + typeof moyenneGenerale + ")");
```

**Resultat attendu :**
```
Prenom: Sarah (type: string)
Age: 14 (type: number)
En vacances: true (type: boolean)
Moyenne: 15.5 (type: number)
```

### Exemple intermediaire
```javascript
// Demonstration des differences entre types
let nombre1 = 5;        // number
let nombre2 = 3;        // number
let texte1 = "5";       // string
let texte2 = "3";       // string

// Operations avec numbers
console.log("=== Calculs avec numbers ===");
console.log(nombre1 + nombre2);    // Addition: 8
console.log(nombre1 - nombre2);    // Soustraction: 2
console.log(nombre1 * nombre2);    // Multiplication: 15

// Operations avec strings
console.log("=== Operations avec strings ===");
console.log(texte1 + texte2);      // Concatenation: "53"
console.log(texte1 + " et " + texte2);  // "5 et 3"

// Melange number et string
console.log("=== Melange types ===");
console.log(nombre1 + texte1);     // "55" (devient string)
console.log("J'ai " + age + " ans"); // "J'ai 14 ans"
```

### Exemple avance (optionnel)
```javascript
// Conversion entre types
let texteNombre = "42";
let vraiNombre = Number(texteNombre);  // Convertir string en number

console.log("Texte: " + texteNombre + " (type: " + typeof texteNombre + ")");
console.log("Nombre: " + vraiNombre + " (type: " + typeof vraiNombre + ")");

// Conversion en string
let nombre = 123;
let texte = String(nombre);           // Convertir number en string

console.log("Nombre: " + nombre + " (type: " + typeof nombre + ")");
console.log("Texte: " + texte + " (type: " + typeof texte + ")");

// Conversion en boolean
let zero = 0;
let unNombre = 5;
console.log("Boolean de 0: " + Boolean(zero));      // false
console.log("Boolean de 5: " + Boolean(unNombre));  // true
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Identifier les types
**Consigne :** Pour chaque variable, dis quel est son type sans utiliser typeof

**Code de depart :**
```javascript
let variable1 = "Bonjour";
let variable2 = 42;
let variable3 = true;
let variable4 = "123";
let variable5 = 3.14;

// Complete les phrases :
console.log("variable1 est de type ___");
console.log("variable2 est de type ___");
console.log("variable3 est de type ___");
console.log("variable4 est de type ___");
console.log("variable5 est de type ___");

// Verifie tes reponses avec typeof
console.log("Verification avec typeof:");
```

**Niveau de difficulte :** 1 etoile sur 5

### Exercice 2 : Calculatrice simple
**Consigne :** Cree des variables pour deux nombres et affiche toutes les operations

**Contraintes :**
- Utilise des vrais numbers (pas de strings)
- Affiche addition, soustraction, multiplication, division
- Utilise des messages clairs

**Niveau de difficulte :** 2 etoiles sur 5

### Exercice 3 : Quiz sur toi
**Consigne :** Cree un mini-quiz avec differents types de reponses

**Aide :** 
- Questions avec reponses string (nom, couleur preferee)
- Questions avec reponses number (age, note preferee)  
- Questions avec reponses boolean (aimes-tu les maths ?)

**Niveau de difficulte :** 3 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```javascript
let variable1 = "Bonjour";
let variable2 = 42;
let variable3 = true;
let variable4 = "123";
let variable5 = 3.14;

// Reponses :
console.log("variable1 est de type string");    // Texte entre guillemets
console.log("variable2 est de type number");    // Nombre entier
console.log("variable3 est de type boolean");   // true/false
console.log("variable4 est de type string");    // Nombre mais entre guillemets
console.log("variable5 est de type number");    // Nombre decimal

// Verification avec typeof
console.log("Verification avec typeof:");
console.log("variable1: " + typeof variable1);  // string
console.log("variable2: " + typeof variable2);  // number
console.log("variable3: " + typeof variable3);  // boolean
console.log("variable4: " + typeof variable4);  // string
console.log("variable5: " + typeof variable5);  // number
```

**Explication :** Le piege etait variable4 = "123" qui est un string car entre guillemets, pas un number.

### Solution Exercice 2
```javascript
// Mes deux nombres
const premier = 20;
const deuxieme = 4;

console.log("=== CALCULATRICE ===");
console.log("Premier nombre: " + premier);
console.log("Deuxieme nombre: " + deuxieme);
console.log("");

// Toutes les operations
let addition = premier + deuxieme;
let soustraction = premier - deuxieme;
let multiplication = premier * deuxieme;
let division = premier / deuxieme;

console.log("Addition: " + premier + " + " + deuxieme + " = " + addition);
console.log("Soustraction: " + premier + " - " + deuxieme + " = " + soustraction);
console.log("Multiplication: " + premier + " * " + deuxieme + " = " + multiplication);
console.log("Division: " + premier + " / " + deuxieme + " = " + division);

// Verification des types
console.log("");
console.log("Type du resultat addition: " + typeof addition);    // number
```

**Alternatives possibles :**
- Utiliser des nombres decimaux
- Ajouter des operations comme le modulo (reste de division)
- Creer une interface plus jolie

### Solution Exercice 3
```javascript
console.log("=== QUIZ SUR MOI ===");

// Reponses de differents types
const monPrenom = "Julie";           // string
const monAge = 14;                   // number
const aimeMaths = true;              // boolean
const couleurPreferee = "violet";    // string
const noteEnMaths = 16.5;           // number (decimal)
const aimeEPS = false;               // boolean

// Affichage du quiz
console.log("Question 1: Comment je m'appelle ?");
console.log("Reponse: " + monPrenom + " (type: " + typeof monPrenom + ")");

console.log("Question 2: Quel age j'ai ?");
console.log("Reponse: " + monAge + " ans (type: " + typeof monAge + ")");

console.log("Question 3: Est-ce que j'aime les maths ?");
console.log("Reponse: " + aimeMaths + " (type: " + typeof aimeMaths + ")");

console.log("Question 4: Quelle est ma couleur preferee ?");
console.log("Reponse: " + couleurPreferee + " (type: " + typeof couleurPreferee + ")");

console.log("Question 5: Quelle note j'ai en maths ?");
console.log("Reponse: " + noteEnMaths + "/20 (type: " + typeof noteEnMaths + ")");

console.log("Question 6: Est-ce que j'aime l'EPS ?");
console.log("Reponse: " + aimeEPS + " (type: " + typeof aimeEPS + ")");
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Variables (let/const)** : Pour stocker les donnees de differents types
- **console.log()** : Pour afficher et tester nos types
- **Operateurs mathematiques** : Pour calculer avec les numbers

### Prochaines etapes  
- **Conversion de types** : Changer un type en autre (string vers number)
- **Operateurs de comparaison** : Comparer des donnees de meme type
- **Conditions** : Prendre des decisions selon les types et valeurs
- **Arrays** : Stocker plusieurs donnees du meme type

### Concepts complementaires
- **Template literals** : Autre facon d'ecrire des strings
- **Objets** : Stocker plusieurs donnees liees ensemble

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer une "carte d'identite" complete avec tous les types

**Fonctionnalites requises :**
- [ ] Informations personnelles (strings)
- [ ] Donnees numeriques (age, taille, poids, notes)
- [ ] Preferences (booleans pour aimer/pas aimer)
- [ ] Verification des types avec typeof
- [ ] Calculs simples (IMC, age dans X annees)

**Structure de fichiers suggeree :**
```
carte-identite/
├── index.html
├── style.css  
└── script.js
```

**Temps estime :** 50 minutes

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - Types de donnees](https://developer.mozilla.org/fr/docs/Web/JavaScript/Data_structures)
- [MDN - typeof](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/typeof)
- [MDN - String](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/String)

### Videos recommandees
- "Types de donnees JavaScript expliques simplement" - Grafikart (25min)
- "String vs Number : les pieges a eviter" - explications pratiques

### Articles approfondis
- "Conversion de types en JavaScript" - quand et comment convertir
- "Boolean en JavaScript : truthy et falsy" - concepts avances mais utiles

### Outils de pratique
- [CodePen - Types playground](https://codepen.io) - pour experimenter avec les types
- Console du navigateur - tester typeof sur tout et n'importe quoi

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre "5" et 5 ?
   - Reponse : "5" est un string (texte), 5 est un number (nombre)

2. **Question pratique :** Que fait ce code ?
   ```javascript
   let x = "10";
   let y = 5;
   console.log(x + y);
   ```
   - Reponse : Affiche "105" (concatenation car x est un string)

3. **Question d'application :** Quel type utiliserais-tu pour stocker si tu aimes les pizza ?
   - Reponse : boolean (true si j'aime, false si j'aime pas)

### Checklist de maitrise
- [ ] Je reconnais un string (entre guillemets)
- [ ] Je reconnais un number (pas de guillemets)
- [ ] Je reconnais un boolean (true/false)
- [ ] Je sais utiliser typeof
- [ ] Je comprends la difference entre types
- [ ] Je peux predire le resultat d'operations entre types

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
let age = "14";  // String au lieu de number
let ageDansUnAn = age + 1;
console.log(ageDansUnAn);  // Affiche "141" au lieu de 15
```

**Message d'erreur :** Pas d'erreur technique mais resultat incorrect

**Solution :**
```javascript
let age = 14;  // Number sans guillemets
let ageDansUnAn = age + 1;
console.log(ageDansUnAn);  // Affiche 15 correctement
```

**Explication :** Avec un string, + fait de la concatenation. Avec un number, + fait de l'addition.

### Erreur frequente 2
**Code problematique :**
```javascript
let estVrai = "true";  // String au lieu de boolean
if (estVrai) {
    console.log("C'est vrai !");  // S'execute toujours !
}
```

**Message d'erreur :** Pas d'erreur mais comportement inattendu

**Solution :**
```javascript
let estVrai = true;  // Boolean sans guillemets
if (estVrai) {
    console.log("C'est vrai !");  // S'execute seulement si true
}
```

**Explication :** "true" (string) est toujours considere comme vrai, meme si ce n'est pas le boolean true.

### Erreur frequente 3
**Code problematique :**
```javascript
console.log(typeof 42.5);     // Qu'est-ce que ca affiche ?
```

**Confusion possible :** Penser que ca affiche "decimal" ou "float"

**Solution et explication :**
```javascript
console.log(typeof 42.5);     // Affiche "number"
// En JavaScript, les entiers et decimaux sont tous des "number"
```

**Explication :** JavaScript ne fait pas la difference entre entiers et decimaux, tout est "number".

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Il y a differents types de donnees en JavaScript]
- [typeof me dit le type d'une donnee]  
- [Les guillemets font toute la difference entre string et number]

### Questions a approfondir
- [Comment convertir facilement un type en autre ?]
- [Quels sont les autres types plus avances ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices avec de nouvelles donnees)
- [ ] Revision dans 1 semaine (creer un programme utilisant tous les types)
- [ ] Revision dans 1 mois (expliquer les types a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript types donnees string number boolean typeof primitifs debutant

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 3/126*
