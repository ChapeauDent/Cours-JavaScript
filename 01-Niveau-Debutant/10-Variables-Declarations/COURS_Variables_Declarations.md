# Variables et Déclarations JavaScript

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** Variables et Déclarations (var, let, const)
- **Niveau :** 🟢 Débutant
- **Durée estimée :** 45 minutes
- **Prérequis :** Introduction à JavaScript
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Les différences entre var, let et const
- [ ] **Appliquer** : Choisir le bon type de déclaration selon le contexte
- [ ] **Analyser** : Identifier les problèmes de portée (scope)
- [ ] **Créer** : Déclarer et utiliser des variables correctement

### Mots-clés à retenir
- **Variable** : Conteneur pour stocker des données
- **Déclaration** : Action de créer une variable
- **Portée (Scope)** : Zone où une variable est accessible
- **Hoisting** : Mécanisme de remontée des déclarations

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les variables sont la base de tout programme JavaScript. Bien comprendre comment les déclarer et les utiliser est essentiel pour éviter les bugs et écrire du code maintenant. JavaScript propose trois façons de déclarer des variables, chacune avec ses spécificités.

### Définition et concepts clés
Une **variable** est un espace de stockage nommé qui peut contenir une valeur. En JavaScript moderne, nous avons trois mots-clés pour déclarer des variables :

- **`var`** : Ancienne syntaxe, portée de fonction
- **`let`** : Syntaxe moderne, portée de bloc, réassignable
- **`const`** : Syntaxe moderne, portée de bloc, non réassignable

### Syntaxe de base
```javascript
// Déclaration avec var (éviter en JavaScript moderne)
var ancienneVariable = "valeur";

// Déclaration avec let (variable modifiable)
let variableModifiable = "valeur initiale";
variableModifiable = "nouvelle valeur"; // OK

// Déclaration avec const (variable constante)
const variableConstante = "valeur définitive";
// variableConstante = "autre"; // ERREUR !
```

### Points d'attention
- **Piège var** : `var` a une portée de fonction, pas de bloc, ce qui peut créer des bugs
- **Piège const** : `const` empêche la réassignation, mais les objets restent mutables
- **Bonne pratique** : Utilisez `const` par défaut, `let` quand vous devez réassigner, évitez `var`

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Déclaration de différents types de variables
const nom = "Marie";           // Constante string
let age = 25;                  // Variable numérique modifiable
let estEtudiant = true;        // Variable booléenne

console.log(nom);              // Affiche: Marie
console.log(age);              // Affiche: 25

age = 26;                      // Modification possible avec let
console.log(age);              // Affiche: 26
```

**Résultat attendu :**
```
Marie
25
26
```

### Exemple intermédiaire
```javascript
// Démonstration de la portée (scope)
function exempleScope() {
    if (true) {
        var varVariable = "Je suis accessible partout dans la fonction";
        let letVariable = "Je suis accessible seulement dans ce bloc";
        const constVariable = "Moi aussi, seulement dans ce bloc";
    }
    
    console.log(varVariable);    // ✅ Fonctionne
    // console.log(letVariable); // ❌ Erreur : letVariable is not defined
    // console.log(constVariable); // ❌ Erreur : constVariable is not defined
}

exempleScope();
```

### Exemple avancé (optionnel)
```javascript
// Hoisting avec var vs let/const
console.log(varHoisted);     // undefined (pas d'erreur grâce au hoisting)
// console.log(letHoisted);  // ReferenceError: Cannot access before initialization

var varHoisted = "Je suis remontée";
let letHoisted = "Je ne suis pas remontée";

// const avec objets (mutabilité vs réassignation)
const personne = { nom: "Alice", age: 30 };
personne.age = 31;           // ✅ Modification de propriété possible
personne.ville = "Paris";    // ✅ Ajout de propriété possible
// personne = {};            // ❌ Réassignation impossible
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Mise en pratique directe
**Consigne :** Corrigez ce code en utilisant les bonnes déclarations (let/const)

**Code de départ :**
```javascript
var PI = 3.14159;           // Cette valeur ne devrait jamais changer
var rayon = 5;              // Cette valeur peut changer
var aire;                   // Sera calculée

PI = 3.14;                  // Oups, erreur de frappe !
aire = PI * rayon * rayon;
console.log("Aire du cercle:", aire);
```

**Niveau de difficulté :** ⭐⭐☆☆☆

### Exercice 2 : Application créative
**Consigne :** Créez un système de compteur avec increment/decrement

**Contraintes :**
- Utilisez const pour les fonctions
- Utilisez let pour le compteur
- Le compteur ne doit pas descendre en dessous de 0

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 3 : Défi (optionnel)
**Consigne :** Identifiez et corrigez les problèmes dans ce code

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log("Compteur:", i);
    }, 1000);
}
// Problème : affiche 3, 3, 3 au lieu de 0, 1, 2
```

**Niveau de difficulté :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
const PI = 3.14159;         // const car PI ne change jamais
let rayon = 5;              // let car le rayon peut varier
let aire;                   // let car sera calculée et peut changer

// PI = 3.14;               // Impossible maintenant, erreur empêchée !
aire = PI * rayon * rayon;
console.log("Aire du cercle:", aire);
```

**Explication :** Utiliser `const` pour PI empêche les modifications accidentelles.

### Solution Exercice 2
```javascript
let compteur = 0;

const increment = function() {
    compteur++;
    console.log("Compteur:", compteur);
};

const decrement = function() {
    if (compteur > 0) {
        compteur--;
    }
    console.log("Compteur:", compteur);
};

// Test
increment(); // 1
increment(); // 2
decrement(); // 1
decrement(); // 0
decrement(); // 0 (ne descend pas en dessous)
```

### Solution Exercice 3
```javascript
// Solution avec let (portée de bloc)
for (let i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log("Compteur:", i); // Affiche correctement 0, 1, 2
    }, 1000);
}

// Alternative avec const et fonction immédiatement invoquée
for (var i = 0; i < 3; i++) {
    (function(index) {
        setTimeout(function() {
            console.log("Compteur:", index);
        }, 1000);
    })(i);
}
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Types de données** : Les variables stockent différents types
- **Syntaxe JavaScript** : Compréhension de base de la syntaxe

### Prochaines étapes
- **Fonctions** : Portée et variables locales/globales
- **Objets** : Propriétés constantes vs mutables
- **Boucles** : Variables de contrôle avec let

### Concepts complémentaires
- **Hoisting** : Mécanisme de remontée des déclarations
- **Scope** : Portée des variables en détail

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer une calculatrice simple avec gestion d'état

**Fonctionnalités requises :**
- [ ] Variables pour stocker les nombres et l'opération
- [ ] Fonctions constantes pour les opérations
- [ ] Gestion des erreurs (division par zéro)

**Structure de fichiers suggérée :**
```
📁 calculatrice-simple/
├── 📄 index.html
├── 📄 style.css
└── 📄 script.js
```

**Temps estimé :** 30 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - var](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/var)
- [MDN - let](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/let)
- [MDN - const](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/const)

### Vidéos recommandées
- "var vs let vs const" - Grafikart (15min)
- Excellente explication des différences avec exemples concrets

### Articles approfondis
- "JavaScript Hoisting Explained" - JavaScript.info
- Comprendre le mécanisme de hoisting en détail

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence principale entre let et const ?
   - Réponse : let permet la réassignation, const l'interdit

2. **Question pratique :** Que fait ce code ?
   ```javascript
   const tableau = [1, 2, 3];
   tableau.push(4);
   console.log(tableau);
   ```
   - Réponse : Affiche [1, 2, 3, 4]. const empêche la réassignation mais pas la mutation

3. **Question d'application :** Quand utiliseriez-vous var en JavaScript moderne ?
   - Réponse : Pratiquement jamais. let et const sont préférables.

### Checklist de maîtrise
- [ ] Je comprends la syntaxe de var, let, const
- [ ] Je sais choisir entre let et const
- [ ] Je comprends la portée de bloc vs fonction
- [ ] Je peux expliquer le hoisting
- [ ] Je sais identifier les problèmes de scope
- [ ] J'évite les pièges courants avec var

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
const age = 25;
age = 26; // Tentative de réassignation
```

**Message d'erreur :** `TypeError: Assignment to constant variable.`

**Solution :**
```javascript
let age = 25;  // Utiliser let si réassignation nécessaire
age = 26;      // Maintenant ça fonctionne
```

**Explication :** const empêche toute réassignation. Utilisez let quand la variable doit changer.

### Erreur fréquente 2
**Code problématique :**
```javascript
console.log(maVariable); // Utilisation avant déclaration
let maVariable = "Hello";
```

**Message d'erreur :** `ReferenceError: Cannot access 'maVariable' before initialization`

**Solution :**
```javascript
let maVariable = "Hello"; // Déclarer avant d'utiliser
console.log(maVariable);
```

**Explication :** Contrairement à var, let et const ne sont pas "hoistées" de manière utilisable.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- [Différences entre var, let, const]
- [Importance de la portée de bloc]
- [Éviter var en JavaScript moderne]

### Questions à approfondir
- [Comment fonctionne exactement le hoisting ?]
- [Cas avancés d'utilisation de const avec objets]

### Révision programmée
- [ ] Révision dans 1 jour (rappel var/let/const)
- [ ] Révision dans 1 semaine (exercices de scope)
- [ ] Révision dans 1 mois (application en projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #variables #var #let #const #scope #hoisting

**Temps de completion :** [À remplir]

**Difficulté ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 2/126*
