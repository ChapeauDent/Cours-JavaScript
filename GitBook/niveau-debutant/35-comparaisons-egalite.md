# ⚖️ Comparaisons et Égalité en JavaScript

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 45 min | **Prérequis :** Variables, Types de données, Conversion de types
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** la différence entre `==` et `===` (égalité vs égalité stricte)
- [ ] **Appliquer** les opérateurs de comparaison correctement
- [ ] **Analyser** les résultats des comparaisons avec conversion automatique
- [ ] **Créer** des conditions fiables pour vos structures de contrôle
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Comparaison** : Action de comparer deux valeurs  
**Égalité simple** : `==` (avec conversion automatique)  
**Égalité stricte** : `===` (sans conversion, type ET valeur identiques)  
**Coercition** : Conversion automatique de JavaScript pour comparer  
**Truthiness** : Valeurs considérées comme "vraies" ou "fausses"  
{% endtab %}

{% tab title="Importance" %}
🎯 **Base des conditions** : Vérifier si quelqu'un est majeur (`age >= 18`)  
🏆 **Logique de jeu** : Comparer des scores  
🔐 **Sécurité** : Vérifier si un mot de passe est correct  
📊 **Tri et filtrage** : Organiser des listes par ordre  
⚠️ **Éviter les bugs** : Comprendre pourquoi `"5" == 5` mais `"5" !== 5`  
{% endtab %}
{% endtabs %}

## ⚡ Pourquoi ce concept est-il crucial ?

{% hint style="warning" %}
**Attention aux pièges de JavaScript !**

JavaScript a des règles surprenantes :
- `"5" == 5` → `true` (conversion automatique)
- `"5" === 5` → `false` (types différents)
- `"10" > "2"` → `false` (compare caractère par caractère !)

Il faut comprendre ces mécanismes avant d'apprendre les conditions `if/else`.
{% endhint %}

## 🎭 Types de comparaisons

### 1. 🔄 Égalité simple (`==`)

{% tabs %}
{% tab title="Principe" %}
**Compare en convertissant si nécessaire**

```javascript
console.log("5" == 5);      // true (convertit "5" en nombre)
console.log(true == 1);     // true (true devient 1)
console.log(0 == false);    // true (false devient 0)
console.log("" == false);   // true (string vide devient false)
```

**Avantage :** Flexible  
**Inconvénient :** Imprévisible et source de bugs
{% endtab %}

{% tab title="Mécanisme de conversion" %}
**JavaScript essaie de convertir pour comparer :**

1. **String vers Number** : `"5"` → `5`
2. **Boolean vers Number** : `true` → `1`, `false` → `0`
3. **Null et undefined** : Considérés égaux entre eux
4. **Objets vers primitifs** : Utilise `valueOf()` ou `toString()`

```javascript
// Exemples de conversions
console.log("123" == 123);     // true (string → number)
console.log(true == 1);        // true (boolean → number)
console.log(null == undefined); // true (cas spécial)
console.log([1] == 1);         // true (array → string → number)
```
{% endtab %}
{% endtabs %}

### 2. ✅ Égalité stricte (`===`)

{% tabs %}
{% tab title="Principe" %}
**Compare type ET valeur sans conversion**

```javascript
console.log("5" === 5);      // false (string vs number)
console.log(true === 1);     // false (boolean vs number)
console.log(0 === false);    // false (number vs boolean)
console.log(null === undefined); // false (types différents)
```

**Avantage :** Prévisible et sûr  
**Inconvénient :** Plus strict (mais c'est bien !)
{% endtab %}

{% tab title="Bonnes pratiques" %}
{% hint style="success" %}
**Règle d'or : Utilisez toujours `===` par défaut !**

**Pourquoi ?**
- ✅ Pas de surprise avec les conversions
- ✅ Code plus prévisible
- ✅ Évite les bugs difficiles à trouver
- ✅ Standard dans l'industrie

**Exception rare :** Vérifier null ET undefined
```javascript
if (value == null) {
  // Vérifie à la fois null et undefined
}

// Équivaut à :
if (value === null || value === undefined) {
  // Plus explicite mais plus long
}
```
{% endhint %}
{% endtab %}
{% endtabs %}

### 3. 🚫 Inégalités (`!=` et `!==`)

```javascript
// Inégalité simple (avec conversion)
console.log(5 != "5");       // false (conversion puis comparaison)

// Inégalité stricte (sans conversion)
console.log(5 !== "5");      // true (types différents)
```

### 4. 📏 Comparaisons numériques (`>`, `<`, `>=`, `<=`)

{% tabs %}
{% tab title="Principe" %}
```javascript
let age = 16;
console.log(age >= 18);    // false
console.log(age < 18);     // true

// Avec conversion automatique
console.log("10" > 2);     // true (convertit "10" en 10)
console.log("abc" > 5);    // false (NaN n'est jamais > quelque chose)
```
{% endtab %}

{% tab title="⚠️ Piège avec les strings" %}
```javascript
// ATTENTION : Comparaison alphabétique !
console.log("10" > "2");   // false (compare "1" avec "2")
console.log("10" > "9");   // false (compare "1" avec "9")
console.log("abc" > "xyz"); // false (ordre alphabétique)

// Solution : Convertir explicitement
console.log(Number("10") > Number("2"));  // true
// ou
console.log(parseInt("10") > parseInt("2")); // true
```

{% hint style="danger" %}
**Règle importante :** Quand vous comparez des nombres qui peuvent être des strings, convertissez-les d'abord !
{% endhint %}
{% endtab %}
{% endtabs %}

## 🎮 Quiz interactif

{% hint style="info" %}
**Testez vos connaissances ! Devinez le résultat avant de regarder la réponse.**

**Question 1 :** Que donne `"5" == 5` ?
A) `true`  B) `false`  C) Erreur
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : A) `true`**

JavaScript convertit automatiquement `"5"` en `5` pour la comparaison avec `==`.

</details>

{% hint style="info" %}
**Question 2 :** Que donne `"10" > "2"` ?
A) `true`  B) `false`  C) Erreur
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) `false`**

Quand on compare deux strings, JavaScript compare caractère par caractère. `"1"` est inférieur à `"2"` dans l'ordre alphabétique.

</details>

{% hint style="info" %}
**Question 3 :** Que donne `0 === false` ?
A) `true`  B) `false`  C) Erreur
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) `false`**

Avec `===`, pas de conversion. `0` est un number et `false` est un boolean, donc ils ne sont pas égaux.

</details>

## 💻 Exercices pratiques

### Exercice 1 : Prédictions

{% hint style="info" %}
**Devinez le résultat de ces comparaisons AVANT de les tester !**
{% endhint %}

```javascript
// Écrivez vos prédictions dans les commentaires
console.log("Test 1:", 5 == "5");        // Votre prédiction : ___
console.log("Test 2:", 5 === "5");       // Votre prédiction : ___
console.log("Test 3:", 0 == false);      // Votre prédiction : ___
console.log("Test 4:", 0 === false);     // Votre prédiction : ___
console.log("Test 5:", "10" > "2");      // Votre prédiction : ___
console.log("Test 6:", "10" > 2);        // Votre prédiction : ___
console.log("Test 7:", null == undefined); // Votre prédiction : ___
console.log("Test 8:", null === undefined); // Votre prédiction : ___
```

<details>
<summary>💡 Solutions avec explications</summary>

```javascript
console.log("Test 1:", 5 == "5");        // true - conversion de "5" en 5
console.log("Test 2:", 5 === "5");       // false - types différents
console.log("Test 3:", 0 == false);      // true - false devient 0
console.log("Test 4:", 0 === false);     // false - types différents
console.log("Test 5:", "10" > "2");      // false - compare "1" et "2"
console.log("Test 6:", "10" > 2);        // true - conversion de "10" en 10
console.log("Test 7:", null == undefined); // true - cas spécial JavaScript
console.log("Test 8:", null === undefined); // false - types différents
```

**Scores :**
- 8/8 : Expert ! 🏆
- 6-7/8 : Très bien ! 👍
- 4-5/8 : Bon début, révisez les conversions
- 0-3/8 : Relisez le cours et réessayez

</details>

### Exercice 2 : Vérificateur d'accès

{% hint style="info" %}
**Créez une fonction qui vérifie les droits selon l'âge :**
- Conduite : 16 ans
- Vote : 18 ans
- Alcool : 21 ans (simulation US)

**Contrainte :** Gérer les entrées string ET number !
{% endhint %}

<details>
<summary>🚀 Code de départ</summary>

```javascript
function verifierDroits(age) {
    // TODO: Convertir age en nombre
    
    // TODO: Vérifier si l'âge est valide
    
    // TODO: Vérifier chaque droit et afficher le résultat
}

// Tests
verifierDroits(14);      // Number
verifierDroits("16");    // String
verifierDroits("18");    // String
verifierDroits(25);      // Number
verifierDroits("abc");   // String invalide
```

</details>

<details>
<summary>💡 Solution complète</summary>

```javascript
function verifierDroits(age) {
    // Convertir en nombre si c'est une string
    const ageNombre = Number(age);
    
    // Vérifier si l'âge est valide
    if (isNaN(ageNombre) || ageNombre < 0 || ageNombre > 120) {
        console.log("❌ Âge invalide");
        return;
    }
    
    console.log(`\n🔍 Vérification des droits pour ${ageNombre} ans`);
    
    // Conduite (16 ans)
    if (ageNombre >= 16) {
        console.log("🚗 ✅ Peut conduire");
    } else {
        console.log(`🚗 ❌ Trop jeune pour conduire (dans ${16 - ageNombre} ans)`);
    }
    
    // Vote (18 ans)  
    if (ageNombre >= 18) {
        console.log("🗳️ ✅ Peut voter");
    } else {
        console.log(`🗳️ ❌ Trop jeune pour voter (dans ${18 - ageNombre} ans)`);
    }
    
    // Alcool (21 ans)
    if (ageNombre >= 21) {
        console.log("🍺 ✅ Peut acheter de l'alcool");
    } else {
        console.log(`🍺 ❌ Trop jeune pour l'alcool (dans ${21 - ageNombre} ans)`);
    }
}

// Tests
verifierDroits(14);      // Aucun droit
verifierDroits("16");    // Conduite seulement
verifierDroits("18");    // Conduite + Vote
verifierDroits(25);      // Tous les droits
verifierDroits("abc");   // Âge invalide
```

**Points clés :**
- ✅ Conversion explicite avec `Number()`
- ✅ Validation de l'entrée avec `isNaN()`
- ✅ Messages informatifs avec temps d'attente
- ✅ Gestion des cas d'erreur

</details>

### Exercice 3 : Comparateur de scores

{% hint style="info" %}
**Créez un programme qui compare 3 scores de joueurs et détermine le gagnant.**

**Bonus :** Gérer les égalités et afficher un classement complet !
{% endhint %}

<details>
<summary>💡 Solution avancée</summary>

```javascript
function comparerScores(joueur1, score1, joueur2, score2, joueur3, score3) {
    // Convertir tous les scores en nombres
    const s1 = Number(score1);
    const s2 = Number(score2);  
    const s3 = Number(score3);
    
    // Vérifier que tous les scores sont valides
    if (isNaN(s1) || isNaN(s2) || isNaN(s3)) {
        console.log("❌ Erreur: Un ou plusieurs scores sont invalides");
        return;
    }
    
    console.log("🏆 === COMPARAISON DES SCORES ===");
    console.log(`${joueur1}: ${s1} points`);
    console.log(`${joueur2}: ${s2} points`);
    console.log(`${joueur3}: ${s3} points`);
    
    // Créer un array avec tous les joueurs
    const joueurs = [
        {nom: joueur1, score: s1},
        {nom: joueur2, score: s2}, 
        {nom: joueur3, score: s3}
    ];
    
    // Trier par score décroissant
    joueurs.sort((a, b) => b.score - a.score);
    
    // Trouver le score maximum
    const scoreMax = joueurs[0].score;
    
    // Trouver tous les gagnants (gestion égalités)
    const gagnants = joueurs.filter(joueur => joueur.score === scoreMax);
    
    console.log("\n🎯 === RÉSULTATS ===");
    
    if (gagnants.length > 1) {
        const nomsGagnants = gagnants.map(g => g.nom).join(" et ");
        console.log(`🤝 Égalité entre ${nomsGagnants} avec ${scoreMax} points !`);
    } else {
        console.log(`🏆 Le gagnant est ${gagnants[0].nom} avec ${scoreMax} points !`);
    }
    
    // Afficher le classement complet
    console.log("\n📊 === CLASSEMENT COMPLET ===");
    joueurs.forEach((joueur, index) => {
        const position = index + 1;
        const emoji = position === 1 ? "🥇" : position === 2 ? "🥈" : "🥉";
        console.log(`${emoji} ${position}. ${joueur.nom}: ${joueur.score} points`);
    });
}

// Tests
console.log("🎮 Test 1:");
comparerScores("Alice", 95, "Bob", "87", "Charlie", 92);

console.log("\n🎮 Test 2 (avec égalité):");
comparerScores("Marie", "100", "Paul", 100, "Julie", "95");

console.log("\n🎮 Test 3 (avec erreur):");
comparerScores("Tom", "abc", "Lisa", 85, "Max", 90);
```

</details>

## 🧠 Pièges courants à éviter

### 1. 🪤 Le piège des strings numériques

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
// Code problématique
const age = "18";  // Vient d'un formulaire
if (age === 18) {
    console.log("Majeur");  // Ne s'exécute jamais !
}

const scores = ["10", "5", "20"];
console.log("20" > "5");  // false ! (compare "2" et "5")
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Solutions correctes
const age = "18";

// Option 1: Conversion explicite
if (Number(age) === 18) {
    console.log("Majeur");
}

// Option 2: Comparaison flexible mais contrôlée
if (age == 18) {  // Utilisation rare de ==
    console.log("Majeur");
}

// Option 3: Conversion avec parseInt
if (parseInt(age) === 18) {
    console.log("Majeur");
}

// Pour les arrays de nombres en strings
const scores = ["10", "5", "20"];
const scoresNombres = scores.map(score => Number(score));
console.log(20 > 5);  // true
```
{% endtab %}
{% endtabs %}

### 2. 🪤 Valeurs "falsy" surprenantes

{% tabs %}
{% tab title="Valeurs falsy" %}
```javascript
// Ces valeurs sont considérées comme "false"
console.log(Boolean(false));     // false
console.log(Boolean(0));         // false
console.log(Boolean(-0));        // false
console.log(Boolean(0n));        // false (BigInt)
console.log(Boolean(""));        // false (string vide)
console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false
console.log(Boolean(NaN));       // false

// Tout le reste est "truthy"
console.log(Boolean("0"));       // true (string non-vide)
console.log(Boolean([]));        // true (array vide)
console.log(Boolean({}));        // true (objet vide)
console.log(Boolean(-1));        // true (nombre négatif)
```
{% endtab %}

{% tab title="Pièges en pratique" %}
```javascript
// Pièges courants
console.log(0 == false);         // true
console.log("" == false);        // true
console.log([] == false);        // true (!!)
console.log({} == false);        // false

// Comparaisons strictes plus sûres
console.log(0 === false);        // false
console.log("" === false);       // false
console.log([] === false);       // false
console.log({} === false);       // false
```
{% endtab %}
{% endtabs %}

## 🔗 Liens avec d'autres concepts

### Concepts utilisés
{% content-ref url="variables-declarations.md" %}
[variables-declarations.md](variables-declarations.md)
{% endcontent-ref %}

{% content-ref url="types-donnees.md" %}
[types-donnees.md](types-donnees.md)
{% endcontent-ref %}

{% content-ref url="conversion-types.md" %}
[conversion-types.md](conversion-types.md)
{% endcontent-ref %}

### Prochaines étapes
{% content-ref url="structures-controle.md" %}
[structures-controle.md](structures-controle.md)
{% endcontent-ref %}

{% content-ref url="operateurs-logiques.md" %}
[operateurs-logiques.md](operateurs-logiques.md)
{% endcontent-ref %}

{% content-ref url="operateur-ternaire.md" %}
[operateur-ternaire.md](operateur-ternaire.md)
{% endcontent-ref %}

## 🎯 Résumé

{% hint style="success" %}
**Points clés à retenir :**

⚖️ **`===` est votre ami** : Utilisez-le par défaut pour éviter les surprises  
🔄 **`==` convertit automatiquement** : Source de bugs, à éviter sauf cas spécial  
📏 **Attention aux strings numériques** : `"10" > "2"` donne `false` !  
🔢 **Convertissez explicitement** : `Number()`, `parseInt()` pour les comparaisons numériques  
⚠️ **Testez les entrées** : Vérifiez avec `isNaN()` et des plages valides  
{% endhint %}

## 🚀 Prochaines étapes

{% hint style="info" %}
**Maintenant que vous maîtrisez les comparaisons :**

👉 **Apprenez** les [Structures de Contrôle](structures-controle.md) pour utiliser ces comparaisons  
👉 **Découvrez** les [Opérateurs Logiques](operateurs-logiques.md) pour combiner les conditions  
👉 **Maîtrisez** l'[Opérateur Ternaire](operateur-ternaire.md) pour des conditions concises  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Passez aux [Structures de Contrôle](structures-controle.md) pour mettre ces comparaisons en pratique avec `if/else` !
{% endhint %}
