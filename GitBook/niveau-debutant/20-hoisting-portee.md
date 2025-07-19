# 🔄 Hoisting et Portée (Scope)

{% hint style="info" %}
**Niveau :** 🟡 Débutant avancé | **Durée :** 35 min | **Prérequis :** Variables et Déclarations
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** le mécanisme du hoisting en JavaScript
- [ ] **Différencier** les portées globale, de fonction et de bloc
- [ ] **Éviter** les pièges courants liés au hoisting
- [ ] **Choisir** entre `var`, `let` et `const` en connaissance de cause
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Hoisting** : "Remontée" des déclarations en haut de leur scope  
**Scope** : Zone de code où une variable est accessible  
**TDZ** : Temporal Dead Zone - zone où `let`/`const` ne sont pas accessibles  
**Lexical Scope** : Portée déterminée par l'emplacement dans le code  
{% endtab %}

{% tab title="Types de Scope" %}
**Global** 🌍 : Accessible partout dans le programme  
**Function** 🏠 : Accessible uniquement dans la fonction  
**Block** 📦 : Accessible uniquement dans le bloc `{}`  
**Module** 📄 : Accessible uniquement dans le module  
{% endtab %}
{% endtabs %}

## 🚀 Le Hoisting expliqué simplement

{% hint style="warning" %}
**Le hoisting, c'est comme si JavaScript "remontait" toutes les déclarations au début de leur scope !**

⚠️ **Attention :** Seules les **déclarations** sont remontées, pas les **initialisations**.
{% endhint %}

### 🎭 Démystifions le hoisting

{% tabs %}
{% tab title="Ce que vous écrivez" %}
```javascript
console.log(message); // Que va-t-il se passer ?
var message = "Hello World!";

sayHello(); // Et ça ?
function sayHello() {
  console.log("Hello!");
}
```
{% endtab %}

{% tab title="Ce que JavaScript voit" %}
```javascript
// JavaScript réorganise mentalement comme ça :
var message; // Déclaration remontée = undefined
function sayHello() { // Fonction complète remontée
  console.log("Hello!");
}

console.log(message); // undefined (pas d'erreur !)
message = "Hello World!"; // Assignation reste en place

sayHello(); // "Hello!" (fonctionne !)
```
{% endtab %}
{% endtabs %}

### 🔍 Hoisting avec `var` vs `let`/`const`

{% tabs %}
{% tab title="🟡 var : Hoisting permissif" %}
```javascript
console.log(name); // undefined (pas d'erreur)
console.log(age);  // undefined (pas d'erreur)

var name = "Alice";
var age = 25;

// Équivalent mental :
// var name; // = undefined
// var age;  // = undefined
// console.log(name); // undefined
// console.log(age);  // undefined
// name = "Alice";
// age = 25;
```

**Résultat :** `undefined` (pas d'erreur, mais pas idéal !)
{% endtab %}

{% tab title="🔴 let/const : Hoisting strict" %}
```javascript
console.log(name); // ReferenceError!
console.log(age);  // ReferenceError!

let name = "Alice";
const age = 25;

// Ces variables existent mais sont dans la "Temporal Dead Zone"
// = inaccessibles avant leur déclaration
```

**Résultat :** `ReferenceError` (erreur explicite = meilleur debug !)
{% endtab %}
{% endtabs %}

## 🏠 Les différents types de portée

### 1. 🌍 Portée globale

{% hint style="info" %}
**Variables globales :** accessibles partout dans le programme
{% endhint %}

```javascript
// Variables globales
var globalVar = "Je suis global";
let globalLet = "Moi aussi";
const GLOBAL_CONST = "Et moi !";

function anywhere() {
  console.log(globalVar);    // ✅ Accessible
  console.log(globalLet);    // ✅ Accessible  
  console.log(GLOBAL_CONST); // ✅ Accessible
}

console.log(globalVar); // ✅ Accessible ici aussi
```

{% hint style="warning" %}
**Attention :** Évitez trop de variables globales ! Elles peuvent créer des conflits.
{% endhint %}

### 2. 🏠 Portée de fonction

{% tabs %}
{% tab title="Scope de fonction" %}
```javascript
function myFunction() {
  var functionVar = "Je suis dans la fonction";
  let functionLet = "Moi aussi";
  
  console.log(functionVar); // ✅ Accessible
  console.log(functionLet); // ✅ Accessible
  
  function innerFunction() {
    console.log(functionVar); // ✅ Accessible (scope parent)
    console.log(functionLet); // ✅ Accessible (scope parent)
  }
  
  innerFunction();
}

myFunction();
console.log(functionVar); // ❌ ReferenceError!
console.log(functionLet); // ❌ ReferenceError!
```
{% endtab %}

{% tab title="Paramètres de fonction" %}
```javascript
function greet(name, age) {
  // name et age sont dans le scope de la fonction
  console.log(`Hello ${name}, you are ${age}`);
  
  if (age >= 18) {
    var status = "adult"; // Function scope (même dans if)
    let mood = "happy";   // Block scope (seulement dans if)
  }
  
  console.log(status); // ✅ "adult" (var = function scope)
  console.log(mood);   // ❌ ReferenceError! (let = block scope)
}
```
{% endtab %}
{% endtabs %}

### 3. 📦 Portée de bloc (ES6+)

{% hint style="success" %}
**Nouveauté ES6 :** `let` et `const` ont une portée de bloc `{}`
{% endhint %}

{% tabs %}
{% tab title="Block scope avec let/const" %}
```javascript
{
  let blockLet = "Je suis dans le bloc";
  const BLOCK_CONST = "Moi aussi";
  var blockVar = "Je déborde du bloc"; // ⚠️ var ignore les blocs
}

console.log(blockVar);   // ✅ "Je déborde du bloc"
console.log(blockLet);   // ❌ ReferenceError!
console.log(BLOCK_CONST);// ❌ ReferenceError!
```
{% endtab %}

{% tab title="Cas pratique : boucles" %}
```javascript
// ❌ Problème classique avec var
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i); // Affiche 3, 3, 3 (i vaut 3 à la fin)
  }, 100);
}

// ✅ Solution avec let
for (let j = 0; j < 3; j++) {
  setTimeout(() => {
    console.log(j); // Affiche 0, 1, 2 (chaque j est dans son bloc)
  }, 100);
}
```
{% endtab %}
{% endtabs %}

## ⚡ Temporal Dead Zone (TDZ)

{% hint style="warning" %}
**TDZ :** Zone où les variables `let`/`const` existent mais ne sont pas encore accessibles
{% endhint %}

```javascript
console.log(typeof name); // "undefined" pour une variable inexistante
console.log(typeof age);  // ReferenceError! age est dans la TDZ

let age = 25; // Fin de la TDZ pour age
```

### 🕰️ Visualisation de la TDZ

{% tabs %}
{% tab title="Timeline avec let" %}
```javascript
function example() {
  // 🚫 TDZ pour myVar commence ici
  console.log(myVar); // ReferenceError!
  
  // 🚫 TDZ continue...
  console.log(myVar); // ReferenceError!
  
  let myVar = "Hello"; // ✅ TDZ se termine ici
  
  // ✅ Accessible maintenant
  console.log(myVar); // "Hello"
}
```
{% endtab %}

{% tab title="Comparaison var vs let" %}
```javascript
function varExample() {
  console.log(myVar); // undefined (hoisted)
  var myVar = "Hello";
  console.log(myVar); // "Hello"
}

function letExample() {
  console.log(myVar); // ReferenceError! (TDZ)
  let myVar = "Hello";
  console.log(myVar); // "Hello"
}
```
{% endtab %}
{% endtabs %}

## 🎮 Exercices interactifs

### 🧩 Puzzle 1 : Prédisez le résultat

{% hint style="info" %}
**Que va afficher ce code ?**
{% endhint %}

```javascript
console.log(a);
console.log(b);
console.log(c);

var a = 1;
let b = 2;
const c = 3;
```

<details>
<summary>💡 Voir la réponse</summary>

```
undefined
ReferenceError: Cannot access 'b' before initialization
(Le code s'arrête à la 2ème ligne)
```

**Explication :**
- `var a` est hoisted comme `undefined`
- `let b` est dans la TDZ
- `const c` n'est jamais atteint

</details>

### 🧩 Puzzle 2 : Scope de boucle

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs.push(function() {
    return i;
  });
}

console.log(funcs[0]()); // ?
console.log(funcs[1]()); // ?
console.log(funcs[2]()); // ?
```

<details>
<summary>💡 Voir la réponse</summary>

```
3
3
3
```

**Pourquoi ?** 
- `var i` a une portée de fonction
- Toutes les fonctions partagent la même variable `i`
- Quand les fonctions s'exécutent, `i` vaut 3

**Solution avec `let` :**
```javascript
var funcs = [];

for (let i = 0; i < 3; i++) { // let au lieu de var
  funcs.push(function() {
    return i; // Chaque i est dans son propre scope
  });
}

console.log(funcs[0]()); // 0
console.log(funcs[1]()); // 1  
console.log(funcs[2]()); // 2
```

</details>

## 🔧 Guide de bonnes pratiques

### ✅ Recommandations modernes

{% tabs %}
{% tab title="🎯 Choix de déclaration" %}
**Ordre de préférence :**

1. **`const`** par défaut (valeurs immutables)
```javascript
const API_URL = "https://api.example.com";
const users = []; // Le contenu peut changer, pas la référence
```

2. **`let`** quand vous devez réassigner
```javascript
let counter = 0;
let currentUser = null;
```

3. **`var`** presque jamais (sauf cas très spécifiques)
{% endtab %}

{% tab title="🚫 Anti-patterns à éviter" %}
```javascript
// ❌ Utiliser une variable avant sa déclaration
console.log(message);
var message = "Hello";

// ❌ Redéclarer avec var
var name = "Alice";
var name = "Bob"; // Pas d'erreur, mais confus

// ❌ var dans les boucles
for (var i = 0; i < 3; i++) {
  // i "fuit" hors de la boucle
}
console.log(i); // 3

// ❌ Oublier de déclarer (crée une globale)
function bad() {
  undeclared = "Oops"; // Variable globale accidentelle !
}
```
{% endtab %}

{% tab title="✅ Bonnes pratiques" %}
```javascript
// ✅ Déclarer les variables au début
function goodFunction() {
  const CONFIG = { api: "/api/v1" };
  let data = null;
  let error = null;
  
  // Logique de la fonction...
}

// ✅ Une déclaration par ligne
const name = "Alice";
const age = 25;
const email = "alice@example.com";

// ✅ Noms descriptifs
const isUserLoggedIn = false;
const maxRetryAttempts = 3;

// ✅ Éviter les variables globales
(function() {
  // Code encapsulé
})();
```
{% endtab %}
{% endtabs %}

## 🔍 Cas d'usage avancés

### 🏗️ Module Pattern et Scope

```javascript
const MyModule = (function() {
  // Variables privées (scope de fonction)
  let privateVar = "Je suis privée";
  const privateMethod = () => "Méthode privée";
  
  // Interface publique
  return {
    publicMethod() {
      return privateMethod(); // Accès aux variables privées
    },
    
    getPrivateVar() {
      return privateVar;
    }
  };
})();

console.log(MyModule.publicMethod()); // "Méthode privée"
console.log(MyModule.privateVar);     // undefined (privée !)
```

### 🔄 Closures et Scope

```javascript
function createCounter() {
  let count = 0; // Variable "capturée" par la closure
  
  return function() {
    count++; // Accès à la variable du scope parent
    return count;
  };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1()); // 1
console.log(counter1()); // 2
console.log(counter2()); // 1 (scope séparé !)
```

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Quelle est la différence principale entre `var` et `let` ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Portée (scope) :**
- `var` : portée de fonction
- `let` : portée de bloc

**Hoisting :**
- `var` : hoisted comme `undefined`
- `let` : hoisted mais dans la TDZ (inaccessible)

</details>

{% hint style="info" %}
**Question 2 :** Ce code affiche-t-il une erreur ?
```javascript
function test() {
  console.log(x);
  let x = 5;
}
test();
```
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Oui, ReferenceError !**

`x` est dans la Temporal Dead Zone au moment du `console.log`.

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="../variables-declarations.md" %}
[variables-declarations.md](../variables-declarations.md)
{% endcontent-ref %}

{% content-ref url="../../niveau-intermediaire/closures-lexical-scope.md" %}
[closures-lexical-scope.md](../../niveau-intermediaire/closures-lexical-scope.md)
{% endcontent-ref %}

{% content-ref url="../../niveau-intermediaire/fonctions-avancees.md" %}
[fonctions-avancees.md](../../niveau-intermediaire/fonctions-avancees.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Maintenant que vous maîtrisez le hoisting et la portée :**

👉 **Explorez** les [Closures et Lexical Scope](../../niveau-intermediaire/closures-lexical-scope.md)  
👉 **Approfondissez** les [Fonctions Avancées](../../niveau-intermediaire/fonctions-avancees.md)  
👉 **Découvrez** les [Types de Données](../types-donnees.md) et leur scope  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🔄 **Le hoisting "remonte" les déclarations** mais pas les initialisations  
🏠 **Scope de fonction pour `var`**, scope de bloc pour `let`/`const`  
⚡ **TDZ protège contre l'usage prématuré** de `let`/`const`  
✅ **Préférez `const` puis `let`**, évitez `var`  
🎯 **Déclarez vos variables en début de scope** pour plus de clarté  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Découvrez les [Closures et Lexical Scope](../../niveau-intermediaire/closures-lexical-scope.md) !
{% endhint %}
