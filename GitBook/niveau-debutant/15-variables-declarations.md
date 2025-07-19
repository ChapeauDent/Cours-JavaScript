# 📦 Variables et Déclarations

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 45 min | **Prérequis :** Introduction à JavaScript
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** les différences entre `var`, `let` et `const`
- [ ] **Choisir** le bon type de déclaration selon le contexte
- [ ] **Identifier** les problèmes de portée (scope)
- [ ] **Appliquer** les bonnes pratiques de déclaration
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Variable** : Conteneur nommé pour stocker des données  
**Déclaration** : Action de créer une variable  
**Portée (Scope)** : Zone où une variable est accessible  
**Hoisting** : Mécanisme de remontée des déclarations  
**Mutation** : Modification du contenu d'une variable  
{% endtab %}

{% tab title="Importance" %}
📦 **Stockage de données** : Fondement de tout programme  
🔧 **Éviter les bugs** : Bonne déclaration = code fiable  
🎯 **Portée claire** : Contrôler l'accès aux variables  
✨ **Code moderne** : Utiliser les bonnes pratiques ES6+  
🛡️ **Sécurité** : Prévenir les erreurs de réassignation  
{% endtab %}
{% endtabs %}

## 📚 Les trois façons de déclarer

### 🎭 Vue d'ensemble

{% hint style="warning" %}
**JavaScript moderne** utilise principalement `let` et `const`. `var` est conservé pour la compatibilité mais **déconseillé** dans le nouveau code.
{% endhint %}

{% tabs %}
{% tab title="Comparaison rapide" %}
| Aspect | `var` | `let` | `const` |
|--------|-------|-------|---------|
| **Époque** | 🕰️ Ancien | ✨ Moderne | ✨ Moderne |
| **Portée** | Fonction | Bloc | Bloc |
| **Hoisting** | ⚠️ Oui | ⚠️ TDZ | ⚠️ TDZ |
| **Redéclaration** | ✅ Autorisée | ❌ Interdite | ❌ Interdite |
| **Réassignation** | ✅ Autorisée | ✅ Autorisée | ❌ Interdite |
| **Recommandation** | 🚫 Éviter | ✅ Variables | ✅ Constantes |
{% endtab %}

{% tab title="Règle d'or moderne" %}
En JavaScript moderne, il existe une règle simple à retenir : **commencez toujours par `const`** ! Cette règle vous évitera beaucoup d'erreurs et rendra votre code plus prévisible.

**Pourquoi cette règle fonctionne-t-elle si bien ?**
- `const` vous oblige à réfléchir : "Est-ce que cette valeur va vraiment changer ?"
- Si la valeur ne change pas → parfait, gardez `const`
- Si la valeur doit changer → utilisez `let` au lieu de `const`

Voici comment appliquer cette règle dans la pratique :

```javascript
// 🥇 Commencez par const pour tout
const nom = "Alice";
const age = 25;
const hobbies = ["lecture", "jardinage"];

// 🥈 Changez vers let seulement si nécessaire
let compteur = 0;
compteur++; // Ici on modifie, donc let était le bon choix

let statut = "en cours";
statut = "terminé"; // Ici aussi on change, donc let

// 🚫 N'utilisez jamais var en JavaScript moderne
```

**L'ordre de priorité à retenir :**
1. 🥇 `const` par défaut (90% des cas)
2. 🥈 `let` si vous devez modifier la variable (10% des cas)
3. 🚫 `var` jamais (0% des cas en code moderne)
{% endtab %}
{% endtabs %}

## 🔒 const : Variables constantes

### 📋 Principe de base

{% hint style="info" %}
**`const` = constante :** Une fois déclarée, la variable ne peut pas être **réassignée**. Parfait pour les valeurs qui ne changent pas.
{% endhint %}

{% tabs %}
{% tab title="Utilisation correcte" %}
Le mot-clé `const` est parfait pour toutes les valeurs qui ne vont pas changer. Pensez-y comme à un coffre-fort : une fois qu'on y met quelque chose, on ne peut plus le changer !

**Quand utiliser `const` :**
- Pour stocker votre nom, votre âge, des informations fixes
- Pour les calculs mathématiques (comme PI = 3.14159)
- Pour les fonctions que vous créez
- Pour les objets et tableaux (même si vous pouvez modifier leur contenu)

Voici des exemples concrets de bonne utilisation :

```javascript
// ✅ Informations personnelles fixes
const nom = "Alice";
const age = 25;
const PI = 3.14159;
const estMajeur = true;

// ✅ Objets et tableaux - la "boîte" ne change pas, mais le contenu oui
const personne = {
    nom: "Bob",
    age: 30
};

const fruits = ["pomme", "banane", "orange"];

// ✅ Fonctions - créées une fois, utilisées partout
const saluer = function(nom) {
    return `Bonjour ${nom} !`;
};

const calculerAire = (rayon) => PI * rayon * rayon;

console.log(saluer("Alice"));        // "Bonjour Alice !"
console.log(calculerAire(5));        // 78.53975
```
{% endtab %}

{% tab title="Erreurs courantes" %}
Même si `const` semble simple, il y a quelques pièges à éviter ! Voici les erreurs les plus fréquentes que font les débutants :

**Erreur #1 : Essayer de changer une constante**
Une fois qu'une variable `const` est créée, vous ne pouvez pas lui donner une nouvelle valeur. C'est comme essayer de changer votre date de naissance - impossible !

**Erreur #2 : Oublier de donner une valeur**
Contrairement à `let`, vous DEVEZ donner une valeur à une variable `const` dès que vous la créez.

**Erreur #3 : Confusion avec les objets**
Attention ! `const` empêche de changer la variable elle-même, mais pas le contenu des objets ou tableaux.

Regardons ces erreurs en détail :

```javascript
// ❌ ERREUR #1 : Réassignation interdite
const nom = "Alice";
// nom = "Bob";  // ⚠️ ERREUR ! TypeError: Assignment to constant variable

// ❌ ERREUR #2 : Déclaration sans valeur
// const vide;  // ⚠️ ERREUR ! SyntaxError: Missing initializer

// ❌ ERREUR #3 : Redéclaration interdite
const couleur = "rouge";
// const couleur = "bleu";  // ⚠️ ERREUR ! Cette variable existe déjà

// ✅ MAIS : Vous pouvez modifier le CONTENU des objets
const utilisateur = { nom: "Alice", age: 25 };
utilisateur.age = 26;  // ✅ OK ! On change le contenu, pas la boîte
utilisateur.ville = "Paris";  // ✅ OK ! On ajoute dans la boîte

console.log(utilisateur);  // { nom: "Alice", age: 26, ville: "Paris" }

// ❌ Mais changer toute la boîte est interdit :
// utilisateur = { nom: "Bob" };  // ⚠️ ERREUR ! TypeError
```
{% endtab %}

{% tab title="Cas pratiques" %}
Maintenant que vous comprenez `const`, voyons dans quelles situations concrètes vous devriez l'utiliser. Ces exemples vous montrent les cas les plus courants dans de vrais programmes :

**1. Valeurs fixes et constantes**
Utilisez `const` pour toutes les valeurs qui ne changent jamais, comme les taxes, les limites, ou les tailles d'écran.

**2. Messages et URLs**
Pour tous les textes fixes de votre application, comme les messages d'accueil ou les adresses de sites web.

**3. Configuration d'applications**
Pour organiser les paramètres de votre programme dans un seul endroit.

**4. Fonctions**
Toutes vos fonctions peuvent être stockées dans des variables `const`.

**5. Éléments HTML**
Une fois que vous trouvez un bouton ou un formulaire sur votre page, stockez-le dans une `const`.

```javascript
// 1. Valeurs numériques fixes
const TAUX_TVA = 0.20;
const MAX_TENTATIVES = 3;
const LARGEUR_ECRAN = 1920;

// 2. Messages et URLs
const MESSAGE_BIENVENUE = "Bienvenue sur notre site !";
const URL_API = "https://api.monsite.com";
const VERSION = "1.2.3";

// 3. Configuration d'application
const CONFIG = {
    theme: "sombre",
    langue: "fr",
    notifications: true
};

// 4. Fonctions
const formaterDate = (date) => {
    return date.toLocaleDateString('fr-FR');
};

const validerEmail = (email) => {
    return email.includes('@') && email.includes('.');
};

// 5. Éléments HTML (une fois trouvés)
const boutonSubmit = document.getElementById('submit');
const formulaire = document.querySelector('#monFormulaire');
```
{% endtab %}
{% endtabs %}

## 🔄 let : Variables modifiables

### 📋 Principe de base

{% hint style="info" %}
**`let` = variable modifiable :** Peut être réassignée mais respecte la portée de bloc. Utilisé quand la valeur doit changer au cours du programme.
{% endhint %}

{% tabs %}
{% tab title="Utilisation typique" %}
Le mot-clé `let` est parfait quand vous savez que la valeur de votre variable va changer au cours de votre programme. Pensez-y comme à un carnet où vous pouvez effacer et réécrire !

**Quand utiliser `let` :**
- Pour les compteurs (qui augmentent ou diminuent)
- Pour les variables qui changent selon des conditions (if/else)
- Pour accumuler des résultats (comme additionner des nombres)
- Pour gérer des états temporaires (connecté/déconnecté, actif/inactif)

**Pourquoi `let` est mieux que `var` :**
`let` respecte les "blocs" de code (les accolades {}), ce qui évite des bugs bizarres !

Voici des exemples concrets où `let` est le bon choix :

```javascript
// 1. Compteurs et boucles
let compteur = 0;
for (let i = 0; i < 5; i++) {
    compteur += i;
    console.log(`Tour ${i}, compteur: ${compteur}`);
}
console.log(`Total final: ${compteur}`); // 10

// 2. Variables qui changent selon l'heure
let message;
const heure = new Date().getHours();

if (heure < 12) {
    message = "Bonjour !";
} else if (heure < 18) {
    message = "Bon après-midi !";
} else {
    message = "Bonsoir !";
}

console.log(message);

// 3. Accumuler des données
let resultat = 0;
const nombres = [1, 2, 3, 4, 5];

for (const nombre of nombres) {
    resultat += nombre; // On additionne chaque nombre
}

console.log(`Somme: ${resultat}`); // 15

// 4. États qui changent
let estConnecte = false;
let tentatives = 0;

function seConnecter(motDePasse) {
    tentatives++;
    if (motDePasse === "secret123") {
        estConnecte = true;
        console.log("Connexion réussie !");
    } else {
        console.log(`Échec. Tentative ${tentatives}`);
    }
}
```
{% endtab %}

{% tab title="Portée de bloc" %}
Voici l'une des différences les plus importantes entre `let` et l'ancien `var` : **`let` respecte les blocs de code** ! 

**Qu'est-ce qu'un bloc ?** C'est tout ce qui est entre des accolades `{}` : les if, les boucles, ou même des accolades seules.

**Pourquoi c'est important ?** Imaginez que vous ayez plusieurs pièces dans une maison. Avec `let`, chaque variable reste dans sa pièce. Avec `var`, les variables se promènent partout dans la maison, ce qui crée du désordre !

**Règle simple :** Une variable `let` créée dans un bloc (entre `{}`) n'existe QUE dans ce bloc. Dès qu'on sort du bloc, elle disparaît.

Voici comment ça fonctionne en pratique :

```javascript
function demonstrationPortee() {
    console.log("=== Variables let et les blocs ===");
    
    // 1. Variable de fonction - accessible partout dans la fonction
    let fonctionVariable = "Je suis accessible dans toute la fonction";
    
    if (true) {
        // 2. Variable de bloc - accessible seulement ici
        let blocVariable = "Je suis prisonnière de ce bloc if";
        console.log(fonctionVariable); // ✅ OK, la variable de fonction est visible
        console.log(blocVariable);     // ✅ OK, on est dans le bon bloc
    }
    
    console.log(fonctionVariable); // ✅ OK, toujours dans la fonction
    // console.log(blocVariable);  // ❌ ERREUR ! Elle a disparu !
    
    // 3. Chaque boucle a sa propre "bulle"
    for (let i = 0; i < 3; i++) {
        let messageSecret = `Message secret ${i}`;
        console.log(messageSecret);
    }
    
    // console.log(i);             // ❌ ERREUR ! i n'existe plus
    // console.log(messageSecret); // ❌ ERREUR ! messageSecret non plus
}

demonstrationPortee();

// 4. Même nom, blocs différents = variables différentes !
{
    let temperature = "Chaud dans ce bloc !";
    console.log(temperature);
}

{
    let temperature = "Froid dans cet autre bloc !"; // ✅ OK, bloc différent
    console.log(temperature);
}
```
{% endtab %}

{% tab title="Réassignation vs redéclaration" %}
Avec `let`, il y a deux choses importantes à comprendre : la différence entre **changer la valeur** (réassignation) et **recréer la variable** (redéclaration).

**Réassignation = Changer le contenu de la boîte** ✅ Autorisé
C'est comme vider votre sac à dos et y mettre autre chose. La boîte reste la même, mais le contenu change.

**Redéclaration = Créer une nouvelle boîte du même nom** ❌ Interdit dans le même endroit
C'est comme avoir deux sacs à dos avec exactement la même étiquette dans la même chambre. JavaScript ne sait plus lequel choisir !

**Mais attention :** Vous pouvez avoir le même nom dans des endroits différents (comme avoir un sac "école" dans votre chambre ET un sac "école" dans le salon).

Voici comment ça marche :

```javascript
// ✅ RÉASSIGNATION : Changer le contenu - Autorisé
let score = 0;
console.log(score); // 0

score = 10;         // Je change le contenu
console.log(score); // 10

score = score + 5;  // Je calcule et je change
console.log(score); // 15

score += 10;        // Raccourci pour ajouter
console.log(score); // 25

// ❌ REDÉCLARATION : Même nom, même endroit - Interdit
let joueur = "Alice";
// let joueur = "Bob"; // ⚠️ ERREUR ! Ce nom est déjà pris ici

// ✅ Mais dans des fonctions différentes, c'est OK
function equipe1() {
    let capitaine = "Sarah"; // OK, on est dans equipe1
    console.log(capitaine);
}

function equipe2() {
    let capitaine = "Lucas"; // ✅ OK, on est dans equipe2 maintenant
    console.log(capitaine);
}

equipe1(); // "Sarah"
equipe2(); // "Lucas"

// ✅ Dans des blocs séparés aussi, c'est OK
{
    let animal = "Chat";
    console.log(animal);
}

{
    let animal = "Chien"; // ✅ OK, nouveau bloc, nouvel animal !
    console.log(animal);
}
```
{% endtab %}
{% endtabs %}

## ⚠️ var : L'ancienne méthode (à éviter)

### 📋 Pourquoi éviter var ?

{% hint style="warning" %}
**`var` pose plusieurs problèmes :** portée de fonction, hoisting déroutant, redéclaration autorisée. Bien comprendre ses défauts aide à éviter les bugs.
{% endhint %}

{% tabs %}
{% tab title="Problèmes de portée" %}
`var` était l'ancienne façon de créer des variables en JavaScript. Mais il a des comportements bizarres qui peuvent créer des bugs difficiles à comprendre ! Voici pourquoi vous devez l'éviter :

**Problème #1 : Les variables "fuient" des blocs**
Avec `var`, les variables ne restent pas dans leur bloc (entre `{}`). Elles se promènent partout dans la fonction ! C'est comme si toutes vos affaires sortaient de votre chambre pour envahir toute la maison.

**Problème #2 : Les boucles dysfonctionnent**
Dans les boucles avec `var`, toutes les variables partagent la même "boîte", ce qui crée des résultats inattendus.

**Problème #3 : Pollution de l'environnement global**
Les variables `var` globales se collent à l'objet `window` du navigateur, ce qui peut casser d'autres scripts.

Regardons ces problèmes en action :

```javascript
// ⚠️ PROBLÈME #1 : Les variables fuient des blocs

function problemeVar() {
    console.log("=== Les problèmes de var ===");
    
    if (true) {
        var varVariable = "Je m'échappe du bloc !";
        let letVariable = "Moi je reste bien sage dans mon bloc";
    }
    
    console.log(varVariable); // ✅ "Je m'échappe du bloc !" - PROBLÉMATIQUE !
    // console.log(letVariable); // ❌ ERREUR - C'est normal et bien !
}

// ⚠️ PROBLÈME #2 : Boucles cassées avec var
console.log("=== Boucle avec var (ne marche pas) ===");
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log("var i:", i); // Affiche 3, 3, 3 (pas 0, 1, 2 !)
    }, 100);
}

console.log("=== Boucle avec let (marche bien) ===");
for (let j = 0; j < 3; j++) {
    setTimeout(() => {
        console.log("let j:", j); // Affiche 0, 1, 2 (correct !)
    }, 200);
}

// ⚠️ PROBLÈME #3 : Pollution globale
var variableSale = "Je pollue l'environnement global";
console.log(window.variableSale); // "Je pollue..." (dans le navigateur)

let variablePropre = "Moi je reste propre";
console.log(window.variablePropre); // undefined (bien !)
```
{% endtab %}

{% tab title="Hoisting déroutant" %}
Le "hoisting" est un comportement étrange de JavaScript avec `var`. En gros, JavaScript fait semblant de "remonter" toutes les variables `var` au début de la fonction, mais sans leur valeur ! C'est comme si vous disiez "J'ai une boîte quelque part" avant même de l'avoir fabriquée.

**Ce qui se passe dans votre tête :**
Vous écrivez le code de haut en bas, pensant que les variables n'existent que quand vous les déclarez.

**Ce qui se passe réellement :**
JavaScript "remonte" secrètement toutes les déclarations `var` au début, mais les valeurs restent où vous les avez mises. Résultat : vous pouvez utiliser une variable avant de lui donner une valeur !

**Avec `let` et `const` :**
Pas de hoisting bizarre ! Si vous utilisez une variable avant de la déclarer, JavaScript vous dit clairement "Cette variable n'existe pas encore". C'est beaucoup plus clair !

```javascript
// ⚠️ COMPORTEMENT BIZARRE AVEC var

console.log("=== Le hoisting bizarre de var ===");

function testHoisting() {
    console.log("Valeur de maVariable:", maVariable); // undefined (pas d'erreur !)
    
    // ... du code ...
    
    var maVariable = "Je suis là !";
    
    console.log("Valeur après affectation:", maVariable); // "Je suis là !"
}

// Ce que JavaScript fait RÉELLEMENT dans les coulisses :
function ceQueJavaScriptVoit() {
    var maVariable; // Déclaration "remontée" au début
    
    console.log("Valeur de maVariable:", maVariable); // undefined
    
    // ... du code ...
    
    maVariable = "Je suis là !"; // Seule l'affectation reste ici
    
    console.log("Valeur après affectation:", maVariable);
}

// ✅ COMPORTEMENT NORMAL AVEC let/const

function testSansHoisting() {
    // console.log(maVariableLet); // ❌ ERREUR - C'est clair !
    
    let maVariableLet = "Je suis déclarée proprement";
    console.log("Valeur normale:", maVariableLet);
}

console.log("=== Test avec var ===");
testHoisting();

console.log("=== Test avec let ===");
testSansHoisting();
```
{% endtab %}

{% tab title="Redéclaration problématique" %}
Avec `var`, JavaScript vous laisse créer plusieurs variables avec le même nom ! C'est comme avoir plusieurs boîtes avec la même étiquette dans votre chambre - vous ne savez plus laquelle utiliser. Et souvent, c'est accidentel et ça crée des bugs !

**Le problème :**
Quand vous écrivez du code long, vous pouvez oublier qu'une variable existe déjà. Avec `var`, JavaScript ne vous avertit pas - il écrase silencieusement l'ancienne valeur !

**La solution avec let/const :**
Si vous essayez de créer deux variables avec le même nom, JavaScript vous dit immédiatement "Stop ! Ce nom est déjà pris !" C'est comme un gardien qui vérifie qu'il n'y a pas de doublons.

**Pourquoi c'est important :**
Dans un vrai projet, vous pouvez avoir des milliers de lignes de code. Sans protection contre les noms en double, vous risquez d'écraser des variables importantes par accident !

```javascript
// ⚠️ PROBLÈME : var autorise la redéclaration

console.log("=== Redéclaration avec var (dangereux) ===");

var utilisateur = "Alice";
console.log("Premier utilisateur:", utilisateur); // "Alice"

// Plus loin dans le code... (accidentellement)
var utilisateur = "Bob"; // ⚠️ Aucune erreur, mais c'était peut-être involontaire !
console.log("Utilisateur écrasé:", utilisateur); // "Bob" - Alice a disparu !

// ✅ SOLUTION : let/const empêchent les redéclarations

console.log("=== Protection avec let/const ===");

let produit = "Ordinateur";
console.log("Produit original:", produit);

// Cette ligne provoquerait une erreur immédiate :
// let produit = "Tablette"; // ❌ SyntaxError: Identifier 'produit' has already been declared

// Pour changer la valeur, on fait simplement :
produit = "Tablette neuve"; // ✅ Modification autorisée
console.log("Produit modifié:", produit);

// Avec const, même la modification est interdite :
const marque = "Apple";
// marque = "Samsung"; // ❌ TypeError: Assignment to constant variable
console.log("Marque protégée:", marque);
```
{% endtab %}
{% endtabs %}

## 📏 Portée (Scope) expliquée

### 🎯 Les différents types de portée

{% tabs %}
{% tab title="Portée globale" %}
La portée globale, c'est comme la salle commune de votre maison : tout le monde peut y accéder ! Les variables globales sont créées en dehors de toute fonction ou bloc, et elles sont visibles partout dans votre programme.

**Avantages :**
- Accessibles depuis n'importe quelle fonction
- Pratiques pour des valeurs utilisées partout (comme le nom du site, la langue, etc.)

**Inconvénients :**
- Risque de conflits entre différentes parties du code
- Plus difficile de déboguer quand il y a des problèmes
- Peuvent être modifiées n'importe où (risqué !)

**Conseil important :**
Utilisez les variables globales avec parcimonie ! C'est comme la télé dans le salon - si tout le monde peut la changer, ça peut créer des disputes ! 😄

```javascript
// 🌍 VARIABLES GLOBALES (accessibles partout)

const SITE_NAME = "Mon Super Site"; // Constante globale (recommandé)
let utilisateurConnecte = null;      // Variable globale (à utiliser avec prudence)

function afficherSite() {
    console.log(`Bienvenue sur ${SITE_NAME}`); // ✅ Accessible partout
}

function connecterUtilisateur(nom) {
    utilisateurConnecte = nom; // ✅ Modification possible depuis n'importe où
    console.log(`${nom} est maintenant connecté`);
}

function obtenirUtilisateur() {
    return utilisateurConnecte; // ✅ Lecture possible depuis n'importe où
}

function deconnecterUtilisateur() {
    utilisateurConnecte = null; // ✅ Remise à zéro possible
    console.log("Utilisateur déconnecté");
}

// Test des fonctions
console.log("=== Test des variables globales ===");
afficherSite();                        // "Bienvenue sur Mon Super Site"
connecterUtilisateur("Alice");         // "Alice est maintenant connecté"
console.log("Utilisateur actuel:", obtenirUtilisateur()); // "Alice"
deconnecterUtilisateur();              // "Utilisateur déconnecté"
console.log("Utilisateur après déconnexion:", obtenirUtilisateur()); // null

// ⚠️ Problème potentiel : modification accidentelle
function fonctionMalicieuse() {
    utilisateurConnecte = "Hacker"; // Oops ! Modification non désirée
}

console.log("=== Avant la fonction malicieuse ===");
connecterUtilisateur("Bob");
console.log("Utilisateur:", obtenirUtilisateur()); // "Bob"

console.log("=== Après la fonction malicieuse ===");
fonctionMalicieuse();
console.log("Utilisateur:", obtenirUtilisateur()); // "Hacker" - Problème !
```
{% endtab %}

{% tab title="Portée de fonction" %}
```javascript
// 🏠 Portée de fonction : accessible dans toute la fonction

function calculatrice() {
    // Variables de fonction
    const nom = "Calculatrice Pro";
    let historique = [];
    
    function additionner(a, b) {
        const resultat = a + b;
        // Ces variables sont accessibles car dans la même fonction
        historique.push(`${a} + ${b} = ${resultat}`);
        return resultat;
    }
    
    function multiplier(a, b) {
        const resultat = a * b;
        // Accès aux variables de la fonction parent
        historique.push(`${a} × ${b} = ${resultat}`);
        return resultat;
    }
    
    function afficherHistorique() {
        console.log(`=== ${nom} ===`);
        historique.forEach(operation => console.log(operation));
    }
    
    // Interface publique
    return {
        additionner,
        multiplier,
        afficherHistorique
    };
}

// Test
const calc = calculatrice();
calc.additionner(5, 3);    // 8
calc.multiplier(4, 7);     // 28
calc.afficherHistorique(); 

// ❌ Variables internes inaccessibles depuis l'extérieur
// console.log(nom);       // ReferenceError
// console.log(historique); // ReferenceError
```
{% endtab %}

{% tab title="Portée de bloc" %}
```javascript
// 📦 Portée de bloc : accessible seulement dans le bloc {}

function demonstrationBlocs() {
    const niveau = "fonction";
    
    // Bloc if
    if (true) {
        const niveau = "bloc if";        // Masque la variable de fonction
        let messageIf = "Je suis dans le if";
        console.log(`Dans if: ${niveau}`);     // "bloc if"
        console.log(messageIf);                 // "Je suis dans le if"
    }
    
    // Bloc else
    if (false) {
        // Ce bloc ne s'exécute pas
    } else {
        const niveau = "bloc else";      // Nouvelle variable, même nom
        let messageElse = "Je suis dans le else";
        console.log(`Dans else: ${niveau}`);   // "bloc else"
        console.log(messageElse);               // "Je suis dans le else"
    }
    
    // Bloc for
    for (let i = 0; i < 2; i++) {
        const niveau = "bloc for";       // Encore une nouvelle variable
        let messageBoucle = `Itération ${i}`;
        console.log(`Dans boucle: ${niveau}`); // "bloc for"
        console.log(messageBoucle);             // "Itération X"
    }
    
    // Bloc arbitraire
    {
        const niveau = "bloc arbitraire";
        let messageBloc = "Je suis dans un bloc simple";
        console.log(`Dans bloc: ${niveau}`);   // "bloc arbitraire"
        console.log(messageBloc);               // "Je suis dans un bloc simple"
    }
    
    // Retour au niveau fonction
    console.log(`Niveau fonction: ${niveau}`); // "fonction"
    
    // ❌ Ces variables ne sont plus accessibles
    // console.log(messageIf);      // ReferenceError
    // console.log(messageElse);    // ReferenceError
    // console.log(messageBoucle);  // ReferenceError
    // console.log(messageBloc);    // ReferenceError
}

demonstrationBlocs();
```
{% endtab %}
{% endtabs %}

## 🧪 Exercices pratiques

### 🎯 Exercice 1 : Gestionnaire de score

{% hint style="info" %}
**Objectif :** Créer un système de gestion de score avec les bonnes déclarations
{% endhint %}

```javascript
// Créez un gestionnaire de score qui :
// - Stocke le score actuel
// - Permet d'ajouter des points
// - Garde un historique des modifications
// - Affiche un résumé

// Utilisez const, let appropriés et évitez var
```

<details>
<summary>💡 Solution</summary>

```javascript
// 🎯 Gestionnaire de score avec bonnes pratiques

function creerGestionnaireScore() {
    // Configuration constante
    const CONFIG = {
        scoreInitial: 0,
        scoreMaximum: 1000,
        bonusMultiplicateur: 1.5
    };
    
    // État modifiable
    let scoreActuel = CONFIG.scoreInitial;
    let nombreActions = 0;
    
    // Historique (const car on ne réassigne jamais le tableau)
    const historique = [];
    
    function ajouterPoints(points, raison = "Points ajoutés") {
        // Validation locale
        const pointsValides = Math.max(0, points);
        const ancienScore = scoreActuel;
        
        // Modification du score
        scoreActuel = Math.min(scoreActuel + pointsValides, CONFIG.scoreMaximum);
        nombreActions++;
        
        // Enregistrement dans l'historique
        historique.push({
            action: raison,
            points: pointsValides,
            ancienScore,
            nouveauScore: scoreActuel,
            timestamp: new Date().toLocaleTimeString()
        });
        
        console.log(`✅ ${raison}: +${pointsValides} points (Score: ${scoreActuel})`);
        return scoreActuel;
    }
    
    function retirerPoints(points, raison = "Points retirés") {
        const pointsValides = Math.max(0, points);
        const ancienScore = scoreActuel;
        
        scoreActuel = Math.max(scoreActuel - pointsValides, 0);
        nombreActions++;
        
        historique.push({
            action: raison,
            points: -pointsValides,
            ancienScore,
            nouveauScore: scoreActuel,
            timestamp: new Date().toLocaleTimeString()
        });
        
        console.log(`❌ ${raison}: -${pointsValides} points (Score: ${scoreActuel})`);
        return scoreActuel;
    }
    
    function appliquerBonus() {
        const bonus = Math.floor(scoreActuel * (CONFIG.bonusMultiplicateur - 1));
        return ajouterPoints(bonus, "Bonus appliqué");
    }
    
    function obtenirResume() {
        return {
            scoreActuel,
            nombreActions,
            scoreMaximumAtteint: scoreActuel === CONFIG.scoreMaximum,
            historique: [...historique] // Copie pour éviter les modifications
        };
    }
    
    function afficherHistorique() {
        console.log("\n📊 Historique des actions:");
        console.log("=" .repeat(50));
        
        if (historique.length === 0) {
            console.log("Aucune action effectuée");
            return;
        }
        
        historique.forEach((action, index) => {
            const signe = action.points >= 0 ? "+" : "";
            console.log(
                `${index + 1}. [${action.timestamp}] ${action.action}: ` +
                `${signe}${action.points} (${action.ancienScore} → ${action.nouveauScore})`
            );
        });
        
        console.log("=" .repeat(50));
        console.log(`Score final: ${scoreActuel} | Actions: ${nombreActions}`);
    }
    
    function reinitialiser() {
        scoreActuel = CONFIG.scoreInitial;
        nombreActions = 0;
        historique.length = 0; // Vider le tableau sans le réassigner
        console.log("🔄 Score réinitialisé");
    }
    
    // Interface publique
    return {
        ajouterPoints,
        retirerPoints,
        appliquerBonus,
        obtenirResume,
        afficherHistorique,
        reinitialiser,
        
        // Getters pour lecture seule
        get score() { return scoreActuel; },
        get actions() { return nombreActions; },
        get historique() { return [...historique]; }
    };
}

// 🧪 Test du gestionnaire
console.log("🎮 Test du Gestionnaire de Score");
console.log("=" .repeat(40));

const jeu = creerGestionnaireScore();

// Scénario de jeu
jeu.ajouterPoints(50, "Ennemi vaincu");
jeu.ajouterPoints(100, "Niveau terminé");
jeu.retirerPoints(25, "Dégâts subis");
jeu.ajouterPoints(200, "Boss vaincu");
jeu.appliquerBonus();
jeu.retirerPoints(50, "Piège activé");

// Affichage des résultats
jeu.afficherHistorique();

const resume = jeu.obtenirResume();
console.log("\n📈 Résumé final:", resume);

// Test de l'interface en lecture seule
console.log(`\nScore actuel: ${jeu.score}`);
console.log(`Nombre d'actions: ${jeu.actions}`);
```

</details>

### 🎯 Exercice 2 : Configuration d'application

{% hint style="info" %}
**Objectif :** Créer un système de configuration avec validation
{% endhint %}

```javascript
// Créez un système qui :
// - Gère des paramètres d'application
// - Valide les modifications
// - Garde les valeurs par défaut
// - Permet la réinitialisation

// Utilisez const pour les valeurs fixes, let pour les modifiables
```

<details>
<summary>💡 Solution</summary>

```javascript
// 🔧 Système de configuration d'application

function creerGestionnaireConfig() {
    // Valeurs par défaut (constantes)
    const DEFAULTS = {
        theme: "clair",
        langue: "fr",
        notifications: true,
        volumeSon: 50,
        qualiteGraphique: "moyenne",
        sauvegardeAuto: true,
        timeoutSession: 30
    };
    
    // Validation rules (constantes)
    const VALIDATION = {
        theme: ["clair", "sombre", "automatique"],
        langue: ["fr", "en", "es", "de"],
        volumeSon: { min: 0, max: 100 },
        qualiteGraphique: ["basse", "moyenne", "haute", "ultra"],
        timeoutSession: { min: 5, max: 120 }
    };
    
    // Configuration actuelle (modifiable)
    let configActuelle = { ...DEFAULTS };
    let modificationsNonSauvees = false;
    
    // Historique des modifications
    const historiqueModifications = [];
    
    function validerValeur(cle, valeur) {
        const regle = VALIDATION[cle];
        
        if (!regle) {
            return true; // Pas de validation spécifique
        }
        
        if (Array.isArray(regle)) {
            return regle.includes(valeur);
        }
        
        if (typeof regle === "object" && regle.min !== undefined) {
            return valeur >= regle.min && valeur <= regle.max;
        }
        
        return true;
    }
    
    function modifier(cle, nouvelleValeur) {
        if (!(cle in DEFAULTS)) {
            console.error(`❌ Clé de configuration inconnue: ${cle}`);
            return false;
        }
        
        if (!validerValeur(cle, nouvelleValeur)) {
            const regle = VALIDATION[cle];
            let messageErreur = `❌ Valeur invalide pour ${cle}: ${nouvelleValeur}`;
            
            if (Array.isArray(regle)) {
                messageErreur += `. Valeurs autorisées: ${regle.join(", ")}`;
            } else if (typeof regle === "object") {
                messageErreur += `. Plage autorisée: ${regle.min}-${regle.max}`;
            }
            
            console.error(messageErreur);
            return false;
        }
        
        const ancienneValeur = configActuelle[cle];
        
        if (ancienneValeur !== nouvelleValeur) {
            configActuelle[cle] = nouvelleValeur;
            modificationsNonSauvees = true;
            
            historiqueModifications.push({
                cle,
                ancienneValeur,
                nouvelleValeur,
                timestamp: new Date().toISOString()
            });
            
            console.log(`✅ ${cle}: ${ancienneValeur} → ${nouvelleValeur}`);
        }
        
        return true;
    }
    
    function obtenirValeur(cle) {
        if (cle) {
            return configActuelle[cle];
        }
        return { ...configActuelle }; // Copie pour éviter les modifications
    }
    
    function reinitialiser(cles = null) {
        const clesPourReinit = cles || Object.keys(DEFAULTS);
        let nombreReinit = 0;
        
        for (const cle of clesPourReinit) {
            if (cle in configActuelle) {
                const ancienneValeur = configActuelle[cle];
                const valeurDefaut = DEFAULTS[cle];
                
                if (ancienneValeur !== valeurDefaut) {
                    configActuelle[cle] = valeurDefaut;
                    nombreReinit++;
                    
                    historiqueModifications.push({
                        cle,
                        ancienneValeur,
                        nouvelleValeur: valeurDefaut,
                        timestamp: new Date().toISOString(),
                        type: "reinitialisation"
                    });
                }
            }
        }
        
        if (nombreReinit > 0) {
            modificationsNonSauvees = true;
            console.log(`🔄 ${nombreReinit} paramètre(s) réinitialisé(s)`);
        } else {
            console.log("ℹ️ Aucune modification nécessaire");
        }
    }
    
    function sauvegarder() {
        // Simulation de sauvegarde
        const configPourSauvegarde = JSON.stringify(configActuelle, null, 2);
        console.log("💾 Configuration sauvegardée:");
        console.log(configPourSauvegarde);
        
        modificationsNonSauvees = false;
        return true;
    }
    
    function afficherConfig() {
        console.log("\n⚙️ Configuration actuelle:");
        console.log("=" .repeat(40));
        
        Object.entries(configActuelle).forEach(([cle, valeur]) => {
            const estDefaut = valeur === DEFAULTS[cle];
            const indicateur = estDefaut ? "📘" : "📙";
            console.log(`${indicateur} ${cle}: ${valeur}`);
        });
        
        if (modificationsNonSauvees) {
            console.log("\n⚠️ Modifications non sauvegardées");
        }
        
        console.log("=" .repeat(40));
    }
    
    function afficherHistorique() {
        console.log("\n📜 Historique des modifications:");
        console.log("=" .repeat(50));
        
        if (historiqueModifications.length === 0) {
            console.log("Aucune modification effectuée");
            return;
        }
        
        historiqueModifications.forEach((modif, index) => {
            const date = new Date(modif.timestamp).toLocaleString();
            const type = modif.type ? ` [${modif.type}]` : "";
            console.log(
                `${index + 1}. [${date}] ${modif.cle}${type}: ` +
                `${modif.ancienneValeur} → ${modif.nouvelleValeur}`
            );
        });
    }
    
    function exporterConfig() {
        return {
            configuration: { ...configActuelle },
            estSauvegarde: !modificationsNonSauvees,
            valeursParDefaut: { ...DEFAULTS },
            nombreModifications: historiqueModifications.length
        };
    }
    
    // Interface publique
    return {
        modifier,
        obtenirValeur,
        reinitialiser,
        sauvegarder,
        afficherConfig,
        afficherHistorique,
        exporterConfig,
        
        // Getters pour lecture seule
        get modifiee() { return modificationsNonSauvees; },
        get defaults() { return { ...DEFAULTS }; }
    };
}

// 🧪 Test du gestionnaire de configuration
console.log("⚙️ Test du Gestionnaire de Configuration");
console.log("=" .repeat(45));

const config = creerGestionnaireConfig();

// Configuration initiale
config.afficherConfig();

// Tests de modification
console.log("\n🔧 Tests de modification:");
config.modifier("theme", "sombre");
config.modifier("volumeSon", 75);
config.modifier("langue", "en");

// Test de validation
config.modifier("volumeSon", 150); // Erreur attendue
config.modifier("theme", "inexistant"); // Erreur attendue

// Affichage après modifications
config.afficherConfig();

// Test de réinitialisation partielle
console.log("\n🔄 Réinitialisation du thème:");
config.reinitialiser(["theme"]);

// Affichage de l'historique
config.afficherHistorique();

// Export final
console.log("\n📤 Export de la configuration:");
const exportData = config.exporterConfig();
console.log(JSON.stringify(exportData, null, 2));
```

</details>

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Quelle est la différence principale entre `let` et `const` ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Différence principale : la réassignation**

- **`let`** : Variable **réassignable** après déclaration
- **`const`** : Variable **non réassignable** après déclaration

```javascript
let age = 25;
age = 26; // ✅ OK

const nom = "Alice";
nom = "Bob"; // ❌ TypeError: Assignment to constant variable
```

**Important :** `const` empêche la réassignation, mais pas la mutation des objets !

</details>

{% hint style="info" %}
**Question 2 :** Pourquoi éviter `var` en JavaScript moderne ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Problèmes de `var` :**

1. **Portée de fonction** au lieu de bloc → fuite de variables
2. **Hoisting déroutant** → variables utilisables avant déclaration  
3. **Redéclaration autorisée** → erreurs silencieuses
4. **Pollution globale** → variables ajoutées à `window`

**Solution :** Utiliser `const` par défaut, `let` si réassignation nécessaire.

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="types-donnees.md" %}
[types-donnees.md](types-donnees.md)
{% endcontent-ref %}

{% content-ref url="hoisting-portee.md" %}
[hoisting-portee.md](hoisting-portee.md)
{% endcontent-ref %}

{% content-ref url="../niveau-intermediaire/closures-lexical-scope.md" %}
[closures-lexical-scope.md](../niveau-intermediaire/closures-lexical-scope.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Maintenant que vous maîtrisez les déclarations :**

👉 **Explorez** les [Types de Données](types-donnees.md)  
👉 **Approfondissez** le [Hoisting et la Portée](hoisting-portee.md)  
👉 **Découvrez** les [Structures de Contrôle](structures-controle.md)  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🥇 **`const` par défaut** : Pour toutes les valeurs non réassignées  
🥈 **`let` si nécessaire** : Quand réassignation requise  
🚫 **Éviter `var`** : Problèmes de portée et hoisting  
📦 **Portée de bloc** : `let` et `const` respectent les blocs {}  
🛡️ **Validation** : Erreurs immédiates avec `let`/`const`  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Découvrons maintenant les [Types de Données](types-donnees.md) !
{% endhint %}
