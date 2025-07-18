# 🔄 Conversion de Types en JavaScript

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 80 min | **Prérequis :** Variables, Types de données
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** la différence entre conversion explicite et implicite
- [ ] **Appliquer** les bonnes méthodes pour convertir un type en autre
- [ ] **Analyser** comment JavaScript convertit automatiquement les types
- [ ] **Créer** du code qui gère correctement les conversions
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Conversion explicite** : Conversion que vous faites volontairement  
**Conversion implicite** : Conversion automatique par JavaScript  
**Coercition** : Autre nom pour la conversion implicite  
**Parsing** : Analyser du texte pour en extraire un nombre  
{% endtab %}

{% tab title="Méthodes principales" %}
**String()** - Convertit vers texte  
**Number()** - Convertit vers nombre  
**Boolean()** - Convertit vers true/false  
**parseInt()** - Extrait un nombre entier  
**parseFloat()** - Extrait un nombre décimal  
{% endtab %}
{% endtabs %}

## 💡 Pourquoi ce concept est-il important ?

{% hint style="warning" %}
**Analogie simple :**

Imaginez que quelqu'un vous donne son âge en disant "quatorze" (mot) et vous voulez calculer son âge dans 5 ans. Vous devez d'abord transformer "quatorze" en nombre 14, puis faire 14 + 5.

C'est pareil en programmation ! Parfois vous recevez des nombres sous forme de texte et vous devez les convertir pour faire des calculs.
{% endhint %}

## 🔄 Types de conversions

### Conversion explicite (recommandée)

{% tabs %}
{% tab title="Vers Nombre" %}
```javascript
// String vers Number
let texte = "123";
let nombre = Number(texte);        // "123" → 123

// Boolean vers Number  
let vraiNombre = Number(true);     // true → 1
let fauxNombre = Number(false);    // false → 0

// Cas spéciaux
let invalide = Number("abc");      // "abc" → NaN
let vide = Number("");             // "" → 0
```

**💡 Conseil :** Toujours vérifier avec `isNaN()` si la conversion a réussi !
{% endtab %}

{% tab title="Vers String" %}
```javascript
// Number vers String
let nombre = 456;
let texte = String(nombre);        // 456 → "456"

// Boolean vers String
let vraiTexte = String(true);      // true → "true"
let fauxTexte = String(false);     // false → "false"

// Alternative avec template literals
let age = 25;
let message = `J'ai ${age} ans`;   // Conversion automatique
```
{% endtab %}

{% tab title="Vers Boolean" %}
```javascript
// Valeurs qui deviennent false (falsy)
Boolean(0);          // false
Boolean("");         // false (string vide)
Boolean(null);       // false
Boolean(undefined);  // false
Boolean(NaN);        // false

// Tout le reste devient true (truthy)
Boolean(1);          // true
Boolean("hello");    // true
Boolean(-1);         // true
Boolean(" ");        // true (espace = pas vide)
```
{% endtab %}

{% tab title="Parsing avancé" %}
```javascript
// parseInt() - Extrait un entier
parseInt("42px");        // 42 (ignore "px")
parseInt("3.14159");     // 3 (coupe après la virgule)
parseInt("  123  ");     // 123 (ignore les espaces)

// parseFloat() - Extrait un décimal
parseFloat("3.14159");   // 3.14159
parseFloat("42.5kg");    // 42.5 (ignore "kg")
parseFloat("abc123");    // NaN (commence par lettre)

// Avec base pour parseInt()
parseInt("1010", 2);     // 10 (binaire vers décimal)
parseInt("FF", 16);      // 255 (hexadécimal)
```
{% endtab %}
{% endtabs %}

### Conversion implicite (automatique)

{% hint style="danger" %}
**Attention !** JavaScript fait parfois des conversions automatiques qui peuvent surprendre.
{% endhint %}

{% tabs %}
{% tab title="Avec l'opérateur +" %}
```javascript
// + avec au moins un string = concaténation
console.log("5" + 3);      // "53" ⚠️
console.log(5 + "3");      // "53" ⚠️
console.log("5" + "3");    // "53"

// + avec que des nombres = addition
console.log(5 + 3);        // 8 ✅

// Cas complexes
console.log("5" + 3 + 2);  // "532" (gauche vers droite)
console.log(3 + 2 + "5");  // "55" (3+2=5, puis "5"+"5")
```

**🎯 Règle :** Dès qu'il y a un string, + fait de la concaténation !
{% endtab %}

{% tab title="Avec autres opérateurs" %}
```javascript
// -, *, /, % convertissent toujours en nombres
console.log("5" - 3);      // 2 ✅
console.log("5" * 3);      // 15 ✅  
console.log("15" / 3);     // 5 ✅
console.log("10" % 3);     // 1 ✅

// Cas d'erreur
console.log("abc" - 3);    // NaN ⚠️

// Avec booleans
console.log(true + 1);     // 2 (true → 1)
console.log(false * 5);    // 0 (false → 0)
```
{% endtab %}

{% tab title="Dans les comparaisons" %}
```javascript
// Comparaisons avec conversion
console.log("5" == 5);     // true (conversion implicite)
console.log("5" === 5);    // false (pas de conversion)

// Cas surprenants
console.log("" == 0);      // true ⚠️
console.log(" " == 0);     // true ⚠️
console.log("0" == false); // true ⚠️

// Boolean dans conditions
if ("hello") {             // "hello" → true
  console.log("Exécuté");
}

if ("") {                  // "" → false
  console.log("Pas exécuté");
}
```
{% endtab %}
{% endtabs %}

## 💻 Exemples pratiques

### Exemple simple : Calculatrice

{% tabs %}
{% tab title="Problème" %}
```javascript
// ❌ Code incorrect
let nombre1 = "10";    // String (vient d'un formulaire)
let nombre2 = "3";     // String

let addition = nombre1 + nombre2;        // "103" ⚠️
let soustraction = nombre1 - nombre2;    // 7 (conversion auto)
let multiplication = nombre1 * nombre2;  // 30 (conversion auto)

console.log("Addition:", addition);      // Surprise !
```

**Problème :** L'addition donne "103" au lieu de 13 !
{% endtab %}

{% tab title="Solution" %}
```javascript
// ✅ Code correct
let nombre1Texte = "10";
let nombre2Texte = "3";

// Conversion explicite
let nombre1 = Number(nombre1Texte);
let nombre2 = Number(nombre2Texte);

// Maintenant tous les calculs marchent
let addition = nombre1 + nombre2;        // 13 ✅
let soustraction = nombre1 - nombre2;    // 7 ✅
let multiplication = nombre1 * nombre2;  // 30 ✅
let division = nombre1 / nombre2;        // 3.333... ✅

console.log(`${nombre1} + ${nombre2} = ${addition}`);
console.log(`${nombre1} - ${nombre2} = ${soustraction}`);
console.log(`${nombre1} × ${nombre2} = ${multiplication}`);
console.log(`${nombre1} ÷ ${nombre2} = ${division.toFixed(2)}`);
```
{% endtab %}
{% endtabs %}

### Exemple avancé : Validation d'âge

{% tabs %}
{% tab title="Code complet" %}
```javascript
function validerAge(ageTexte) {
  // Étape 1: Conversion
  let ageNombre = Number(ageTexte);
  
  // Étape 2: Vérifications
  if (isNaN(ageNombre)) {
    return {
      valide: false,
      erreur: `"${ageTexte}" n'est pas un nombre valide`
    };
  }
  
  if (ageNombre < 0) {
    return {
      valide: false, 
      erreur: "L'âge ne peut pas être négatif"
    };
  }
  
  if (ageNombre > 120) {
    return {
      valide: false,
      erreur: "L'âge semble trop élevé"
    };
  }
  
  // Succès !
  return {
    valide: true,
    age: ageNombre
  };
}

// Tests
const tests = ["25", "abc", "150", "-5", "18"];

tests.forEach(test => {
  const resultat = validerAge(test);
  if (resultat.valide) {
    console.log(`✅ ${test} → ${resultat.age} ans`);
  } else {
    console.log(`❌ ${test} → ${resultat.erreur}`);
  }
});
```
{% endtab %}

{% tab title="Résultat" %}
```
✅ 25 → 25 ans
❌ abc → "abc" n'est pas un nombre valide  
❌ 150 → L'âge semble trop élevé
❌ -5 → L'âge ne peut pas être négatif
✅ 18 → 18 ans
```
{% endtab %}
{% endtabs %}

## 🎮 Quiz interactif

{% hint style="info" %}
**Question 1 :** Que donne `"5" + 3` ?
A) 8  B) "53"  C) 53  D) Erreur
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) "53"**

Avec l'opérateur `+`, dès qu'il y a un string, JavaScript fait de la concaténation (coller les textes) au lieu de l'addition.

</details>

{% hint style="info" %}
**Question 2 :** Que donne `parseInt("42px")` ?
A) NaN  B) 42  C) "42"  D) Erreur
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) 42**

`parseInt()` lit le texte du début jusqu'au premier caractère non-numérique et renvoie le nombre trouvé. Il ignore "px".

</details>

{% hint style="info" %}
**Question 3 :** Quelle est la différence entre `Number()` et `parseInt()` ?
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Différences principales :**

- **Number()** : Convertit la chaîne entière ou échoue (NaN)
- **parseInt()** : Lit jusqu'au premier caractère invalide

Exemples :
- `Number("42px")` → NaN
- `parseInt("42px")` → 42

</details>

## 💻 Exercices pratiques

### Exercice 1 : Convertisseur d'unités

{% tabs %}
{% tab title="Consigne" %}
Créez un convertisseur qui transforme :
- Mètres → Centimètres (× 100)
- Celsius → Fahrenheit (× 9/5 + 32)  
- Kilos → Livres (× 2.205)

**Contraintes :**
- Gérer les erreurs de saisie
- Afficher les résultats avec 2 décimales
- Permettre les nombres décimaux
{% endtab %}

{% tab title="Code de départ" %}
```javascript
// Données d'entrée (simuler saisie utilisateur)
const conversions = [
  { valeur: "1.80", de: "metres", vers: "centimetres" },
  { valeur: "25", de: "celsius", vers: "fahrenheit" },
  { valeur: "70.5", de: "kilos", vers: "livres" },
  { valeur: "abc", de: "metres", vers: "centimetres" }
];

// Votre code ici
function convertir(valeurTexte, de, vers) {
  // À compléter !
}

// Tester toutes les conversions
conversions.forEach(conv => {
  const resultat = convertir(conv.valeur, conv.de, conv.vers);
  console.log(resultat);
});
```
{% endtab %}

{% tab title="Solution" %}
```javascript
function convertir(valeurTexte, de, vers) {
  // Conversion en nombre
  const valeur = parseFloat(valeurTexte);
  
  // Vérification
  if (isNaN(valeur)) {
    return `❌ "${valeurTexte}" n'est pas un nombre valide`;
  }
  
  let resultat;
  let unite;
  
  // Calculs selon le type de conversion
  if (de === "metres" && vers === "centimetres") {
    resultat = valeur * 100;
    unite = "cm";
  } else if (de === "celsius" && vers === "fahrenheit") {
    resultat = (valeur * 9/5) + 32;
    unite = "°F";
  } else if (de === "kilos" && vers === "livres") {
    resultat = valeur * 2.205;
    unite = "lbs";
  } else {
    return `❌ Conversion ${de} → ${vers} non supportée`;
  }
  
  return `✅ ${valeur} ${de} = ${resultat.toFixed(2)} ${unite}`;
}

// Tests
const conversions = [
  { valeur: "1.80", de: "metres", vers: "centimetres" },
  { valeur: "25", de: "celsius", vers: "fahrenheit" },
  { valeur: "70.5", de: "kilos", vers: "livres" },
  { valeur: "abc", de: "metres", vers: "centimetres" }
];

conversions.forEach(conv => {
  const resultat = convertir(conv.valeur, conv.de, conv.vers);
  console.log(resultat);
});
```

**Résultat attendu :**
```
✅ 1.8 metres = 180.00 cm
✅ 25 celsius = 77.00 °F  
✅ 70.5 kilos = 155.45 lbs
❌ "abc" n'est pas un nombre valide
```
{% endtab %}
{% endtabs %}

### Exercice 2 : Parser des données complexes

{% tabs %}
{% tab title="Consigne" %}
Vous recevez des données sous forme "25kg", "1.80m", "18ans". Extrayez les nombres et les unités.

**Objectif :** Créer une fonction qui renvoie `{ nombre: 25, unite: "kg" }`
{% endtab %}

{% tab title="Solution" %}
```javascript
function extraireDonnees(texte) {
  // Méthode 1: parseFloat + regex pour l'unité
  const nombre = parseFloat(texte);
  
  if (isNaN(nombre)) {
    return { erreur: `Impossible d'extraire un nombre de "${texte}"` };
  }
  
  // Trouver l'unité (tout ce qui n'est pas nombre/point/espace)
  const unite = texte.replace(/[\d\.\s]/g, '');
  
  return {
    nombre: nombre,
    unite: unite || "sans unité",
    original: texte
  };
}

// Tests
const donnees = ["25kg", "1.80m", "18ans", "100%", "3.14", "abc123"];

console.log("=== EXTRACTION DE DONNÉES ===");
donnees.forEach(donnee => {
  const resultat = extraireDonnees(donnee);
  console.log(`"${donnee}" →`, resultat);
});
```

**Résultat :**
```
"25kg" → { nombre: 25, unite: "kg", original: "25kg" }
"1.80m" → { nombre: 1.8, unite: "m", original: "1.80m" }  
"18ans" → { nombre: 18, unite: "ans", original: "18ans" }
"100%" → { nombre: 100, unite: "%", original: "100%" }
"3.14" → { nombre: 3.14, unite: "sans unité", original: "3.14" }
"abc123" → { erreur: 'Impossible d\'extraire un nombre de "abc123"' }
```
{% endtab %}
{% endtabs %}

## ⚠️ Pièges courants et solutions

### Piège 1 : L'opérateur + avec strings

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
// Code qui semble correct mais ne l'est pas
let a = prompt("Premier nombre:");  // "5"
let b = prompt("Deuxième nombre:"); // "3"  
let somme = a + b;                  // "53" !!

console.log(`${a} + ${b} = ${somme}`); // "5 + 3 = 53"
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Conversion explicite avant calcul
let a = prompt("Premier nombre:");
let b = prompt("Deuxième nombre:");

// Méthode 1: Number()
let somme1 = Number(a) + Number(b);

// Méthode 2: parseFloat()
let somme2 = parseFloat(a) + parseFloat(b);

// Méthode 3: opérateur unaire +
let somme3 = +a + +b;

console.log(`${a} + ${b} = ${somme1}`); // "5 + 3 = 8"
```
{% endtab %}
{% endtabs %}

### Piège 2 : Comparaisons avec ==

{% tabs %}
{% tab title="❌ Résultats surprenants" %}
```javascript
// Comparaisons avec conversion implicite
console.log("" == 0);        // true ⚠️
console.log(" " == 0);       // true ⚠️
console.log("0" == false);   // true ⚠️
console.log(null == undefined); // true ⚠️

// Dans les conditions
if ("0") {                   // "0" est truthy
  console.log("Exécuté");    // Sera exécuté !
}

if ("0" == false) {          // Mais "0" == false !
  console.log("Confus ?");   // Sera aussi exécuté !
}
```
{% endtab %}

{% tab title="✅ Solutions" %}
```javascript
// Solution 1: Utiliser === (pas de conversion)
console.log("" === 0);        // false ✅
console.log(" " === 0);       // false ✅
console.log("0" === false);   // false ✅

// Solution 2: Conversion explicite avant comparaison
let input = "0";
if (Number(input) === 0) {
  console.log("C'est vraiment zéro");
}

// Solution 3: Vérifier le type ET la valeur
function estVide(valeur) {
  return valeur === "" || valeur === null || valeur === undefined;
}
```
{% endtab %}
{% endtabs %}

### Piège 3 : NaN et les calculs

{% tabs %}
{% tab title="❌ Propagation d'erreurs" %}
```javascript
let age = Number("abc");     // NaN
let ageFutur = age + 5;      // NaN (NaN + anything = NaN)
let message = `Dans 5 ans: ${ageFutur}`;  // "Dans 5 ans: NaN"

// NaN est contagieux !
console.log(NaN + 1);        // NaN
console.log(NaN * 2);        // NaN
console.log(NaN / 10);       // NaN
```
{% endtab %}

{% tab title="✅ Gestion d'erreurs" %}
```javascript
function calculerAgeFutur(ageTexte, annees) {
  const age = Number(ageTexte);
  
  // Vérification explicite
  if (isNaN(age)) {
    return `Erreur: "${ageTexte}" n'est pas un âge valide`;
  }
  
  if (age < 0 || age > 120) {
    return `Erreur: âge ${age} non réaliste`;
  }
  
  const ageFutur = age + annees;
  return `Dans ${annees} ans, vous aurez ${ageFutur} ans`;
}

// Tests
console.log(calculerAgeFutur("25", 5));    // "Dans 5 ans, vous aurez 30 ans"
console.log(calculerAgeFutur("abc", 5));   // "Erreur: "abc" n'est pas un âge valide"
console.log(calculerAgeFutur("-5", 5));    // "Erreur: âge -5 non réaliste"
```
{% endtab %}
{% endtabs %}

## 🔗 Liens avec d'autres concepts

{% content-ref url="25-types-donnees.md" %}
[25-types-donnees.md](25-types-donnees.md)
{% endcontent-ref %}

{% content-ref url="15-variables-declarations.md" %}
[15-variables-declarations.md](15-variables-declarations.md)
{% endcontent-ref %}

{% content-ref url="../niveau-intermediaire/gestion-erreurs.md" %}
[gestion-erreurs.md](../niveau-intermediaire/gestion-erreurs.md)
{% endcontent-ref %}

## 🚀 Projet pratique

{% hint style="success" %}
**Défi : Créer un convertisseur universel**

**Fonctionnalités requises :**
- [ ] Interface simple avec prompt() ou formulaire HTML
- [ ] Conversions multiples (température, distance, poids)  
- [ ] Validation complète des entrées
- [ ] Messages d'erreur clairs
- [ ] Résultats formatés proprement

**Temps estimé :** 60-90 minutes
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🔄 **Conversion explicite = plus sûre** que la conversion implicite  
🎯 **Number(), String(), Boolean()** pour les conversions de base  
⚡ **parseInt(), parseFloat()** pour extraire des nombres du texte  
⚠️ **Toujours vérifier avec isNaN()** après conversion vers nombre  
🚨 **L'opérateur + avec strings** fait de la concaténation, pas de l'addition  
✅ **Utiliser === au lieu de ==** pour éviter les conversions surprises  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Passez aux [Comparaisons et Égalité](35-comparaisons-egalite.md) pour maîtriser les comparaisons avec les types !
{% endhint %}
