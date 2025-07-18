# Hoisting et Portée en JavaScript

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** Hoisting et Portée des Variables (var, let, const)
- **Niveau :** Débutant (concept fondamental)
- **Durée estimée :** 75 minutes
- **Prérequis :** Variables (let, const, var), types de données de base
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Le mécanisme de hoisting et pourquoi il existe
- [ ] **Différencier** : var, let et const au niveau de la portée
- [ ] **Éviter** : Les pièges courants liés au hoisting
- [ ] **Appliquer** : Les bonnes pratiques de portée des variables
- [ ] **Déboguer** : Les erreurs liées aux variables non définies

### Mots-clés à retenir
- **Hoisting** : Remontée des déclarations en haut du scope
- **Portée (Scope)** : Zone où une variable est accessible
- **Portée globale** : Accessible partout dans le programme
- **Portée de fonction** : Accessible uniquement dans la fonction
- **Portée de bloc** : Accessible uniquement dans le bloc {}

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Le hoisting et la portée sont **LES concepts** qui expliquent pourquoi JavaScript se comporte parfois de manière inattendue. C'est la différence entre "ça marche par hasard" et "je comprends pourquoi ça marche".

**Problèmes que ça résout :**
- Pourquoi `var` et `let` se comportent différemment
- Pourquoi certaines variables sont `undefined` au lieu de provoquer une erreur
- Comment organiser son code pour éviter les conflits de noms
- Pourquoi les boucles `for` peuvent causer des bugs étranges

### Définition et concepts clés

#### **1. Le Hoisting expliqué simplement**

**Hoisting** = "Hissage" ou "Remontée". JavaScript "remonte" certaines déclarations au début de leur portée.

```javascript
// Ce que vous écrivez :
console.log(maVariable); // undefined (pas d'erreur!)
var maVariable = "Hello";

// Ce que JavaScript comprend :
var maVariable; // Déclaration remontée
console.log(maVariable); // undefined
maVariable = "Hello"; // Assignation reste à sa place
```

#### **2. Les 3 types de portée**

```javascript
// PORTÉE GLOBALE (accessible partout)
var variableGlobale = "Je suis partout";

function maFonction() {
    // PORTÉE DE FONCTION (accessible dans toute la fonction)
    var variableFonction = "Je suis dans la fonction";
    
    if (true) {
        // PORTÉE DE BLOC (accessible uniquement dans ce bloc)
        let variableBloc = "Je suis dans le bloc";
        const constanteBloc = "Moi aussi";
        
        console.log(variableGlobale); // ✅ OK
        console.log(variableFonction); // ✅ OK  
        console.log(variableBloc); // ✅ OK
    }
    
    console.log(variableBloc); // ❌ ERREUR - hors du bloc
}
```

#### **3. Comportement de var vs let vs const**

| Caractéristique | var | let | const |
|------------------|-----|-----|-------|
| **Hoisting** | Oui, avec `undefined` | Oui, mais inaccessible | Oui, mais inaccessible |
| **Portée** | Fonction | Bloc | Bloc |
| **Redéclaration** | Autorisée | Interdite | Interdite |
| **Réassignation** | Autorisée | Autorisée | Interdite |

### Syntaxe et exemples détaillés

#### **Hoisting avec var**
```javascript
// Exemple 1: var est "hissée"
console.log("Avant déclaration:", nom); // undefined (pas d'erreur)
var nom = "Alice";
console.log("Après déclaration:", nom); // "Alice"

// Équivalent à :
var nom; // JavaScript fait ça automatiquement
console.log("Avant déclaration:", nom); // undefined
nom = "Alice";
console.log("Après déclaration:", nom); // "Alice"
```

#### **Hoisting avec let et const**
```javascript
// Exemple 2: let et const sont "hissées" mais inaccessibles
console.log(prenom); // ❌ ReferenceError: Cannot access before initialization
let prenom = "Bob";

console.log(age); // ❌ ReferenceError: Cannot access before initialization  
const age = 25;
```

#### **Portée de fonction vs portée de bloc**
```javascript
function testPortee() {
    console.log("=== TEST PORTÉE ===");
    
    // var a une portée de FONCTION
    if (true) {
        var varVariable = "Je suis var";
        let letVariable = "Je suis let";
        const constVariable = "Je suis const";
    }
    
    console.log(varVariable); // ✅ "Je suis var" - accessible!
    console.log(letVariable); // ❌ ReferenceError - hors du bloc
    console.log(constVariable); // ❌ ReferenceError - hors du bloc
}

testPortee();
```

### Points d'attention critiques

#### **Piège 1 : Boucles for avec var**
```javascript
// PROBLÈME CLASSIQUE
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log("var i:", i); // Affiche 3, 3, 3 (pas 0, 1, 2)
    }, 100);
}

// SOLUTION avec let
for (let j = 0; j < 3; j++) {
    setTimeout(() => {
        console.log("let j:", j); // Affiche 0, 1, 2 ✅
    }, 100);
}
```

#### **Piège 2 : Redéclaration silencieuse avec var**
```javascript
var utilisateur = "Alice";
// ... 100 lignes de code ...
var utilisateur = "Bob"; // ❌ Écrase silencieusement!

console.log(utilisateur); // "Bob" - bug difficile à trouver

// Avec let/const, erreur explicite
let utilisateur2 = "Alice";
let utilisateur2 = "Bob"; // ❌ SyntaxError - bien!
```

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Démonstration visuelle du hoisting
console.log("=== DÉMONSTRATION HOISTING ===");

// Test 1: var
console.log("1. Test var avant déclaration:", typeof maVar, maVar);
var maVar = "Hello World";
console.log("1. Test var après déclaration:", typeof maVar, maVar);

// Test 2: let (va causer une erreur)
try {
    console.log("2. Test let avant déclaration:", maLet);
} catch (error) {
    console.log("2. Erreur avec let:", error.name);
}
let maLet = "Hello Let";
console.log("2. Test let après déclaration:", maLet);

// Test 3: const (va causer une erreur)
try {
    console.log("3. Test const avant déclaration:", maConst);
} catch (error) {
    console.log("3. Erreur avec const:", error.name);
}
const maConst = "Hello Const";
console.log("3. Test const après déclaration:", maConst);
```

**Résultat attendu :**
```
=== DÉMONSTRATION HOISTING ===
1. Test var avant déclaration: undefined undefined
1. Test var après déclaration: string Hello World
2. Erreur avec let: ReferenceError
2. Test let après déclaration: Hello Let
3. Erreur avec const: ReferenceError
3. Test const après déclaration: Hello Const
```

### Exemple intermédiaire
```javascript
// Simulation d'un scenario réel : gestionnaire d'utilisateurs
function gestionnaireUtilisateurs() {
    console.log("=== GESTIONNAIRE UTILISATEURS ===");
    
    // Variables globales à la fonction
    var totalUtilisateurs = 0;
    let utilisateursActifs = [];
    const MAX_UTILISATEURS = 10;
    
    console.log("Configuration initiale:");
    console.log("- Total:", totalUtilisateurs);
    console.log("- Actifs:", utilisateursActifs);
    console.log("- Maximum:", MAX_UTILISATEURS);
    
    // Fonction pour ajouter un utilisateur
    function ajouterUtilisateur(nom, age) {
        console.log(`\n--- Ajout de ${nom} ---`);
        
        // Vérification avec portée de bloc
        if (totalUtilisateurs < MAX_UTILISATEURS) {
            // Variables locales au bloc
            let nouvelUtilisateur = {
                id: totalUtilisateurs + 1,
                nom: nom,
                age: age,
                actif: true
            };
            
            // Ces variables existent seulement dans ce bloc
            let message = `Utilisateur ${nom} ajouté`;
            const timestamp = new Date().toLocaleTimeString();
            
            // Modification des variables de portée supérieure
            totalUtilisateurs++; // var accessible
            utilisateursActifs.push(nouvelUtilisateur); // let accessible
            
            console.log(message + " à " + timestamp);
            console.log("Nouvel utilisateur:", nouvelUtilisateur);
        } else {
            console.log("❌ Limite atteinte!");
        }
        
        // Tentative d'accès aux variables de bloc (erreur)
        try {
            console.log(nouvelUtilisateur); // ❌ Hors de portée
        } catch (error) {
            console.log("Variable de bloc inaccessible:", error.name);
        }
    }
    
    // Tests d'ajout
    ajouterUtilisateur("Alice", 25);
    ajouterUtilisateur("Bob", 30);
    ajouterUtilisateur("Charlie", 22);
    
    // Résumé final
    console.log("\n=== RÉSUMÉ FINAL ===");
    console.log("Total utilisateurs:", totalUtilisateurs);
    console.log("Utilisateurs actifs:", utilisateursActifs.length);
    console.log("Liste:", utilisateursActifs.map(u => u.nom));
    
    return {
        total: totalUtilisateurs,
        utilisateurs: utilisateursActifs,
        limite: MAX_UTILISATEURS
    };
}

// Lancement du test
let resultat = gestionnaireUtilisateurs();
console.log("\nRésultat retourné:", resultat);
```

### Exemple avancé (diagnostic complet)
```javascript
// Outil de diagnostic des variables et portées
function diagnosticPortee() {
    console.log("=== DIAGNOSTIC COMPLET PORTÉE ===");
    
    // Test 1: Hoisting de fonctions
    console.log("\n1. HOISTING DES FONCTIONS:");
    
    try {
        console.log("Appel de fonctionDeclaree:", fonctionDeclaree());
    } catch (e) {
        console.log("Erreur:", e.message);
    }
    
    try {
        console.log("Appel de fonctionExpression:", fonctionExpression());
    } catch (e) {
        console.log("Erreur:", e.message);
    }
    
    // Déclarations de fonctions (hissées complètement)
    function fonctionDeclaree() {
        return "Je suis hissée complètement!";
    }
    
    // Expression de fonction (seulement la variable est hissée)
    var fonctionExpression = function() {
        return "Je ne suis pas hissée!";
    };
    
    // Test 2: Portées imbriquées
    console.log("\n2. PORTÉES IMBRIQUÉES:");
    
    var niveau1 = "Niveau 1 (var)";
    let niveau1Let = "Niveau 1 (let)";
    
    function testPorteeImbriquee() {
        var niveau2 = "Niveau 2 (var)";
        let niveau2Let = "Niveau 2 (let)";
        
        console.log("Dans la fonction:");
        console.log("- Accès niveau1:", niveau1); // ✅
        console.log("- Accès niveau1Let:", niveau1Let); // ✅
        console.log("- Accès niveau2:", niveau2); // ✅
        console.log("- Accès niveau2Let:", niveau2Let); // ✅
        
        if (true) {
            var niveau3 = "Niveau 3 (var)";
            let niveau3Let = "Niveau 3 (let)";
            
            console.log("Dans le bloc if:");
            console.log("- Accès niveau1:", niveau1); // ✅
            console.log("- Accès niveau2:", niveau2); // ✅
            console.log("- Accès niveau3:", niveau3); // ✅
            console.log("- Accès niveau3Let:", niveau3Let); // ✅
        }
        
        console.log("Après le bloc if:");
        console.log("- Accès niveau3:", niveau3); // ✅ var remonte
        try {
            console.log("- Accès niveau3Let:", niveau3Let); // ❌ let reste dans le bloc
        } catch (e) {
            console.log("- niveau3Let inaccessible:", e.name);
        }
    }
    
    testPorteeImbriquee();
    
    // Test 3: Temporal Dead Zone
    console.log("\n3. TEMPORAL DEAD ZONE:");
    
    function testTDZ() {
        console.log("Début de la fonction");
        
        // TDZ : la variable existe mais n'est pas accessible
        try {
            console.log("Accès à variableTDZ:", variableTDZ);
        } catch (e) {
            console.log("TDZ Error:", e.name);
        }
        
        let variableTDZ = "Maintenant accessible";
        console.log("Après déclaration:", variableTDZ);
    }
    
    testTDZ();
    
    // Test 4: Pollution globale avec var
    console.log("\n4. POLLUTION GLOBALE:");
    
    function testPollution() {
        // Sans déclaration = variable globale (❌ mauvais)
        variableGlobaleAccidentelle = "Oops, je suis globale!";
        
        // var dans la fonction = locale (✅ bon)
        var variableLocale = "Je reste dans la fonction";
        
        console.log("Dans la fonction:", variableLocale);
    }
    
    testPollution();
    
    // Vérification de la pollution
    try {
        console.log("Variable globale accidentelle:", variableGlobaleAccidentelle); // ✅ accessible
    } catch (e) {
        console.log("Pas de pollution:", e.name);
    }
    
    try {
        console.log("Variable locale:", variableLocale); // ❌ inaccessible
    } catch (e) {
        console.log("Variable locale protégée:", e.name);
    }
}

// Lancement du diagnostic
diagnosticPortee();
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Prédire le résultat
**Consigne :** Sans exécuter le code, prédisez ce qui sera affiché.

```javascript
console.log(a); // Que va afficher cette ligne ?
var a = 5;

console.log(b); // Et cette ligne ?
let b = 10;

console.log(c); // Et cette ligne ?
const c = 15;
```

**Niveau de difficulté :** ⭐⭐☆☆☆

### Exercice 2 : Déboguer la boucle
**Consigne :** Corrigez ce code pour qu'il affiche 0, 1, 2 au lieu de 3, 3, 3.

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i);
    }, 100);
}
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 3 : Créer un compteur sûr
**Consigne :** Créez une fonction qui retourne un compteur. Le compteur doit être protégé (pas accessible de l'extérieur).

```javascript
function creerCompteur() {
    // Votre code ici
    // Le compteur doit être privé
    
    return function() {
        // Retourner la prochaine valeur
    };
}

let compteur1 = creerCompteur();
console.log(compteur1()); // 1
console.log(compteur1()); // 2
console.log(compteur1()); // 3

let compteur2 = creerCompteur();
console.log(compteur2()); // 1 (nouveau compteur)
```

**Niveau de difficulté :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
console.log(a); // undefined (var est hissée, mais valeur pas encore assignée)
var a = 5;

console.log(b); // ReferenceError (let est dans la TDZ)
let b = 10;

console.log(c); // ReferenceError (const est dans la TDZ)
const c = 15;
```

**Explication :** `var` est hissée avec `undefined`, `let` et `const` sont hissées mais inaccessibles.

### Solution Exercice 2
```javascript
// PROBLÈME : var a une portée de fonction
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // Affiche 3, 3, 3
    }, 100);
}

// SOLUTION 1: Utiliser let (portée de bloc)
for (let i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // Affiche 0, 1, 2 ✅
    }, 100);
}

// SOLUTION 2: Capture avec closure (si on doit garder var)
for (var i = 0; i < 3; i++) {
    (function(index) {
        setTimeout(function() {
            console.log(index); // Affiche 0, 1, 2 ✅
        }, 100);
    })(i);
}

// SOLUTION 3: bind pour capturer la valeur
for (var i = 0; i < 3; i++) {
    setTimeout(function(index) {
        console.log(index); // Affiche 0, 1, 2 ✅
    }.bind(null, i), 100);
}
```

### Solution Exercice 3
```javascript
function creerCompteur() {
    // Variable privée grâce à la portée de fonction
    let compteur = 0;
    
    // Retourner une fonction qui a accès à la variable privée
    return function() {
        compteur++; // Incrémente la variable privée
        return compteur; // Retourne la nouvelle valeur
    };
}

// Test de la solution
let compteur1 = creerCompteur();
console.log(compteur1()); // 1
console.log(compteur1()); // 2
console.log(compteur1()); // 3

let compteur2 = creerCompteur();
console.log(compteur2()); // 1 (nouveau compteur indépendant)

// Vérification que la variable est privée
console.log(compteur); // ReferenceError - pas accessible

// Version avancée avec reset
function creerCompteurAvance() {
    let compteur = 0;
    
    return {
        incrementer: function() {
            compteur++;
            return compteur;
        },
        valeur: function() {
            return compteur;
        },
        reset: function() {
            compteur = 0;
            return compteur;
        }
    };
}

let compteurAvance = creerCompteurAvance();
console.log(compteurAvance.incrementer()); // 1
console.log(compteurAvance.incrementer()); // 2
console.log(compteurAvance.valeur()); // 2
console.log(compteurAvance.reset()); // 0
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Variables** : let, const, var et leurs différences
- **Fonctions** : Portée locale des fonctions
- **Types** : undefined, ReferenceError

### Prochaines étapes importantes
- **Closures** : Utilisation avancée des portées imbriquées
- **Fonctions** : Paramètres et variables locales
- **Modules** : Encapsulation et portées privées
- **Classes** : Propriétés privées et publiques

### Concepts complémentaires
- **Temporal Dead Zone** : Zone morte temporelle de let/const
- **Strict Mode** : Prévention de la pollution globale
- **Module Scope** : Portée des modules ES6

---

## **PROJET MINI**

### Défi pratique : Gestionnaire de variables sécurisé
**Objectif :** Créer un système de gestion de variables avec différents niveaux d'accès.

**Fonctionnalités requises :**
- [ ] Variables publiques (accessibles partout)
- [ ] Variables privées (accessibles seulement dans le module)
- [ ] Variables temporaires (accessibles seulement dans leur bloc)
- [ ] Système de debug pour voir les portées
- [ ] Prévention des fuites de variables globales

**Structure suggérée :**
```
📁 gestionnaire-variables/
├── 📄 index.html
├── 📄 gestionnaire.js
└── 📄 debug-portee.js
```

**Temps estimé :** 90 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Hoisting](https://developer.mozilla.org/fr/docs/Glossary/Hoisting)
- [MDN - Portée](https://developer.mozilla.org/fr/docs/Glossary/Scope)
- [MDN - Temporal Dead Zone](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz)

### Articles approfondis
- "Understanding Hoisting in JavaScript" - JavaScript.info
- "Let vs Var vs Const" - Guide complet des différences
- "JavaScript Scopes and Closures" - Préparation aux closures

### Outils de pratique
- [JavaScript Visualizer](http://pythontutor.com/visualize.html) - Voir les portées en action
- [Repl.it](https://replit.com/) - Tester le hoisting en temps réel

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Qu'est-ce que le hoisting ?**
   - Réponse : Mécanisme qui remonte les déclarations de variables et fonctions en haut de leur portée

2. **Quelle est la différence principale entre var et let ?**
   - Réponse : var a une portée de fonction, let a une portée de bloc

3. **Pourquoi ce code affiche-t-il 'undefined' ?**
   ```javascript
   console.log(x);
   var x = 5;
   ```
   - Réponse : var est hissée mais l'assignation reste à sa place

### Checklist de maîtrise
- [ ] Je comprends pourquoi var et let se comportent différemment
- [ ] Je peux expliquer ce qu'est la Temporal Dead Zone
- [ ] Je sais éviter les pièges du hoisting dans les boucles
- [ ] Je comprends les différents types de portées
- [ ] Je peux créer des variables privées avec les portées
- [ ] Je reconnais les erreurs liées au hoisting

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 : Accès avant déclaration
**Code problématique :**
```javascript
console.log(maVariable);
let maVariable = "Hello";
```

**Message d'erreur :** `ReferenceError: Cannot access 'maVariable' before initialization`

**Solution :**
```javascript
let maVariable = "Hello";
console.log(maVariable);
```

**Explication :** let et const sont dans la TDZ avant leur déclaration.

### Erreur fréquente 2 : Variable globale accidentelle
**Code problématique :**
```javascript
function maFonction() {
    variableOubliee = "Je suis globale !";
}
```

**Solution :**
```javascript
function maFonction() {
    var variableOubliee = "Je reste locale";
    // ou let variableOubliee = "Je reste locale";
}
```

**Explication :** Sans déclaration, la variable devient globale.

### Erreur fréquente 3 : Boucle for avec callbacks
**Code problématique :**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // 3, 3, 3
}
```

**Solution :**
```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // 0, 1, 2
}
```

**Explication :** let crée une nouvelle liaison pour chaque itération.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Le hoisting explique beaucoup de comportements "bizarres" de JavaScript
- let et const apportent plus de prévisibilité que var
- La portée est cruciale pour organiser son code

### Questions à approfondir
- Comment utiliser les portées pour créer des modules
- Lien entre portées et closures
- Performance : impact des différentes portées

### Révision programmée
- [ ] Révision dans 1 jour (refaire les exercices)
- [ ] Révision dans 1 semaine (créer des exemples personnels)
- [ ] Révision dans 1 mois (lien avec les closures)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #hoisting #scope #portee #var #let #const #tdz

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 2.5/126*
