# 🎭 Types de Données en JavaScript

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 90 min | **Prérequis :** Variables et Déclarations
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** les différents types de données JavaScript
- [ ] **Identifier** le type d'une donnée avec `typeof`
- [ ] **Manipuler** string, number, boolean efficacement
- [ ] **Éviter** les erreurs de type courantes
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Type de données** : Catégorie d'informations (texte, nombre, etc.)  
**String** : Type pour le texte et caractères  
**Number** : Type pour tous les nombres (entiers et décimaux)  
**Boolean** : Type pour vrai/faux (true/false)  
**typeof** : Opérateur pour connaître le type d'une donnée  
{% endtab %}

{% tab title="Importance" %}
📚 **Base de tout programme** : Savoir manipuler les données  
🔍 **Éviter les erreurs** : Comprendre pourquoi "5" + 3 = "53"  
⚡ **Performance** : Choisir le bon type pour chaque usage  
🎯 **Logique claire** : Structurer les informations correctement  
🛠️ **Débogage** : Identifier les problèmes de type rapidement  
{% endtab %}
{% endtabs %}

## 🏗️ Vue d'ensemble des types

### 📊 Types primitifs JavaScript

{% hint style="info" %}
**JavaScript a 8 types de données :** 7 primitifs + 1 complexe (Object)

Nous nous concentrons d'abord sur les **5 types essentiels** pour débuter.
{% endhint %}

{% tabs %}
{% tab title="Les 5 types essentiels" %}
| Type | Description | Exemples | Utilisation |
|------|-------------|----------|-------------|
| **string** | 📝 Texte et caractères | `"Bonjour"`, `'Alice'` | Noms, messages, textes |
| **number** | 🔢 Nombres | `42`, `3.14`, `-10` | Calculs, compteurs, âges |
| **boolean** | ✅❌ Vrai/Faux | `true`, `false` | Conditions, états |
| **undefined** | ❓ Non défini | `undefined` | Variables non initialisées |
| **null** | 🚫 Intentionnellement vide | `null` | "Rien" explicite |

```javascript
// Exemples des 5 types essentiels
const nom = "Alice";           // string
const age = 25;                // number  
const estMajeur = true;        // boolean
let adresse;                   // undefined
const telephone = null;        // null

console.log(typeof nom);       // "string"
console.log(typeof age);       // "number"
console.log(typeof estMajeur); // "boolean"
console.log(typeof adresse);   // "undefined"
console.log(typeof telephone); // "object" (particularité de null!)
```
{% endtab %}

{% tab title="Types avancés (aperçu)" %}
**Pour plus tard dans votre apprentissage :**

| Type | Description | Exemple |
|------|-------------|---------|
| **bigint** | 🔢 Très grands nombres | `123n` |
| **symbol** | 🔑 Identifiants uniques | `Symbol("id")` |
| **object** | 📦 Structures complexes | `{}`, `[]`, fonctions |

```javascript
// Aperçu des types avancés (ne vous inquiétez pas pour l'instant !)
const grandNombre = 123456789012345678901234567890n; // bigint
const identifiant = Symbol("unique");                // symbol
const personne = { nom: "Alice", age: 25 };         // object
const nombres = [1, 2, 3, 4, 5];                    // object (Array)
const saluer = function() { return "Bonjour"; };    // object (Function)
```

**Focus actuel :** Maîtrisez d'abord string, number, boolean ! 🎯
{% endtab %}
{% endtabs %}

## 📝 String : Le type texte

### 🎯 Principe de base

{% hint style="info" %}
**String = chaîne de caractères :** Tout ce qui est du texte, des lettres, des phrases. Toujours entouré de guillemets !
{% endhint %}

{% tabs %}
{% tab title="Création de strings" %}
```javascript
// 3 façons de créer des strings
const simple = 'Texte avec guillemets simples';
const double = "Texte avec guillemets doubles";
const template = `Texte avec backticks (template)`;

// Tous sont équivalents pour du texte simple
console.log(simple);   // "Texte avec guillemets simples"
console.log(double);   // "Texte avec guillemets doubles"
console.log(template); // "Texte avec backticks (template)"

// Vérification du type
console.log(typeof simple);  // "string"
console.log(typeof double);  // "string"
console.log(typeof template); // "string"

// ⚠️ Attention à la cohérence dans votre code
const nomComplet = "Alice Dupont";    // Choisissez un style
const ville = "Paris";                // et gardez-le dans tout le projet
const pays = "France";
```
{% endtab %}

{% tab title="Cas usage pratiques" %}
```javascript
// 🎯 Utilisations courantes des strings

// 1. Informations personnelles
const prenom = "Alice";
const nom = "Dupont";
const email = "alice.dupont@example.com";

// 2. Messages et textes
const messageBienvenue = "Bienvenue sur notre site !";
const messageErreur = "Une erreur s'est produite";
const aide = "Cliquez ici pour obtenir de l'aide";

// 3. Données formatées
const dateNaissance = "15/03/1995";
const numeroTelephone = "01 23 45 67 89";
const codePostal = "75001";

// 4. URLs et chemins
const urlSite = "https://www.monsite.com";
const cheminImage = "/images/profil.jpg";
const nomFichier = "document.pdf";

// 5. Identifiants et codes
const idUtilisateur = "USER_12345";
const codePromo = "REDUCTION20";
const referenceCommande = "CMD-2024-001";

console.log("Prénom:", prenom);
console.log("Email:", email);
console.log("Message:", messageBienvenue);
```
{% endtab %}

{% tab title="Opérations de base" %}
```javascript
// 🔧 Opérations courantes avec les strings

// 1. Concaténation (coller des textes)
const prenom = "Alice";
const nom = "Dupont";
const nomComplet = prenom + " " + nom;
console.log(nomComplet); // "Alice Dupont"

// 2. Template literals (moderne et pratique)
const age = 25;
const ville = "Paris";
const presentation = `Je m'appelle ${prenom}, j'ai ${age} ans et j'habite à ${ville}`;
console.log(presentation); // "Je m'appelle Alice, j'ai 25 ans et j'habite à Paris"

// 3. Propriétés et méthodes utiles
const texte = "Bonjour le monde";
console.log("Longueur:", texte.length);              // 16
console.log("En majuscules:", texte.toUpperCase());  // "BONJOUR LE MONDE"
console.log("En minuscules:", texte.toLowerCase());  // "bonjour le monde"
console.log("Contient 'monde':", texte.includes("monde")); // true

// 4. Extraction de parties
const phrase = "JavaScript est fantastique";
console.log("Première lettre:", phrase[0]);           // "J"
console.log("Dernière lettre:", phrase[phrase.length - 1]); // "e"
console.log("Mot 'Script':", phrase.slice(4, 10));   // "Script"

// 5. Caractères spéciaux
const avecSaut = "Première ligne\nDeuxième ligne";
const avecTab = "Colonne1\tColonne2";
const avecGuillemet = "Il a dit : \"Bonjour !\"";
console.log(avecSaut);
console.log(avecTab);
console.log(avecGuillemet);
```
{% endtab %}
{% endtabs %}

## 🔢 Number : Le type numérique

### 🎯 Principe de base

{% hint style="info" %}
**Number = tous les nombres :** Entiers, décimaux, positifs, négatifs. JavaScript n'a qu'un seul type pour tous les nombres !
{% endhint %}

{% tabs %}
{% tab title="Types de nombres" %}
```javascript
// 🔢 Tous les types de nombres sont "number"

// Entiers positifs
const age = 25;
const annee = 2024;
const nombrePages = 150;

// Entiers négatifs
const temperature = -10;
const dette = -500;
const profondeur = -50;

// Nombres décimaux (floats)
const prix = 19.99;
const moyenne = 15.75;
const pi = 3.14159;

// Notation scientifique
const grandNombre = 1e6;        // 1 million (1 000 000)
const petitNombre = 1e-6;       // 0.000001

// Vérification des types
console.log(typeof age);        // "number"
console.log(typeof temperature); // "number"
console.log(typeof prix);      // "number"
console.log(typeof grandNombre); // "number"

// ⚠️ Attention : pas de guillemets pour les nombres !
const nombreTexte = "42";      // string (texte)
const nombreReel = 42;         // number (nombre)
console.log(typeof nombreTexte); // "string"
console.log(typeof nombreReel);  // "number"
```
{% endtab %}

{% tab title="Opérations mathématiques" %}
```javascript
// 🧮 Calculs avec les numbers

// Opérations de base
const a = 10;
const b = 3;

console.log("Addition:", a + b);      // 13
console.log("Soustraction:", a - b);  // 7
console.log("Multiplication:", a * b); // 30
console.log("Division:", a / b);      // 3.3333333333333335
console.log("Modulo (reste):", a % b); // 1
console.log("Puissance:", a ** b);    // 1000

// Ordre des opérations (priorité)
const calcul1 = 2 + 3 * 4;      // 14 (pas 20 !)
const calcul2 = (2 + 3) * 4;    // 20
console.log("Sans parenthèses:", calcul1);
console.log("Avec parenthèses:", calcul2);

// Incrémentation et décrémentation
let compteur = 0;
compteur++;           // compteur = compteur + 1
console.log(compteur); // 1
compteur += 5;        // compteur = compteur + 5
console.log(compteur); // 6
compteur--;           // compteur = compteur - 1
console.log(compteur); // 5

// Fonctions mathématiques utiles
const nombre = -15.7;
console.log("Valeur absolue:", Math.abs(nombre));   // 15.7
console.log("Arrondi:", Math.round(nombre));        // -16
console.log("Entier inférieur:", Math.floor(nombre)); // -16
console.log("Entier supérieur:", Math.ceil(nombre));  // -15
console.log("Racine carrée de 16:", Math.sqrt(16));   // 4
console.log("Nombre aléatoire:", Math.random());      // 0.xxx
```
{% endtab %}

{% tab title="Cas spéciaux et pièges" %}
```javascript
// ⚠️ Cas spéciaux des numbers

// 1. Division par zéro
console.log(5 / 0);    // Infinity
console.log(-5 / 0);   // -Infinity
console.log(0 / 0);    // NaN (Not a Number)

// 2. NaN (Not a Number) - toujours se vérifier !
const resultatInvalide = "texte" * 5;
console.log(resultatInvalide);           // NaN
console.log(typeof resultatInvalide);    // "number" (surprenant !)
console.log(isNaN(resultatInvalide));    // true

// 3. Précision des décimaux (limitation technique)
console.log(0.1 + 0.2);                 // 0.30000000000000004 (pas 0.3 !)
console.log(0.1 + 0.2 === 0.3);         // false (surprenant !)

// Solution pour les décimaux critiques
const somme = Math.round((0.1 + 0.2) * 100) / 100;
console.log(somme);                      // 0.3

// 4. Conversion string vers number
const texteNombre = "42";
const nombre1 = Number(texteNombre);     // 42 (number)
const nombre2 = parseInt(texteNombre);   // 42 (number)
const nombre3 = parseFloat("42.5");      // 42.5 (number)

console.log(typeof nombre1);             // "number"
console.log(nombre1 + 8);                // 50

// 5. Vérifications utiles
const valeur = 42;
console.log(Number.isInteger(valeur));   // true
console.log(Number.isFinite(valeur));    // true
console.log(Number.isNaN(valeur));       // false
```
{% endtab %}
{% endtabs %}

## ✅ Boolean : Le type logique

### 🎯 Principe de base

{% hint style="info" %}
**Boolean = vrai ou faux :** Seulement deux valeurs possibles : `true` ou `false`. Essentiel pour la logique et les conditions !
{% endhint %}

{% tabs %}
{% tab title="Valeurs boolean" %}
```javascript
// ✅ Les deux seules valeurs boolean
const estVrai = true;
const estFaux = false;

console.log(estVrai);           // true
console.log(estFaux);           // false
console.log(typeof estVrai);    // "boolean"
console.log(typeof estFaux);    // "boolean"

// 🎯 Utilisations pratiques
const estMajeur = true;
const estConnecte = false;
const aDesNotifications = true;
const estEnVacances = false;
const estAdministrateur = false;

// Affichage dans des messages
console.log("Est majeur:", estMajeur);
console.log("Est connecté:", estConnecte);

// ⚠️ Attention : sans guillemets !
const bonBoolean = true;        // boolean
const mauvaisBoolean = "true";  // string !

console.log(typeof bonBoolean);     // "boolean"
console.log(typeof mauvaisBoolean); // "string"
console.log(bonBoolean === true);   // true
console.log(mauvaisBoolean === true); // false !
```
{% endtab %}

{% tab title="Opérations logiques" %}
```javascript
// 🔧 Opérateurs logiques avec les booleans

// ET logique (&&) - true si les DEUX conditions sont vraies
const aPermisConduire = true;
const aVoiture = true;
const peutConduire = aPermisConduire && aVoiture;
console.log("Peut conduire:", peutConduire); // true

const estMajeur = true;
const aCarteIdentite = false;
const peutEntrer = estMajeur && aCarteIdentite;
console.log("Peut entrer:", peutEntrer); // false

// OU logique (||) - true si AU MOINS UNE condition est vraie
const estEtudiant = false;
const estSenior = true;
const aDroitReduction = estEtudiant || estSenior;
console.log("A droit à une réduction:", aDroitReduction); // true

// NON logique (!) - inverse la valeur
const estOuvert = true;
const estFerme = !estOuvert;
console.log("Est fermé:", estFerme); // false

const estConnecte = false;
const estDeconnecte = !estConnecte;
console.log("Est déconnecté:", estDeconnecte); // true

// Combinaisons complexes
const age = 17;
const aAutorisation = true;
const peutParticiper = (age >= 18) || (age >= 16 && aAutorisation);
console.log("Peut participer:", peutParticiper); // true
```
{% endtab %}

{% tab title="Comparaisons créant des booleans" %}
```javascript
// 🔍 Comparaisons qui retournent des booleans

const age = 25;
const nom = "Alice";
const salaire = 3000;

// Comparaisons numériques
console.log("Est majeur:", age >= 18);        // true
console.log("Est senior:", age >= 65);        // false
console.log("Age exact:", age === 25);        // true
console.log("N'a pas 30 ans:", age !== 30);  // true

// Comparaisons de strings
console.log("S'appelle Alice:", nom === "Alice");  // true
console.log("Ne s'appelle pas Bob:", nom !== "Bob"); // true

// Comparaisons avec calculs
console.log("Salaire élevé:", salaire > 2500);      // true
console.log("Peut emprunter:", salaire * 3 > 50000); // false

// Vérifications d'existence
let adresse;
console.log("A une adresse:", adresse !== undefined); // false

const telephone = null;
console.log("A un téléphone:", telephone !== null);   // false

// Méthodes qui retournent des booleans
const email = "alice@example.com";
console.log("Email valide:", email.includes("@"));    // true
console.log("Email .com:", email.endsWith(".com"));   // true
console.log("Commence par alice:", email.startsWith("alice")); // true

// Stockage dans des variables
const estValide = age >= 18 && nom.length > 0 && salaire > 0;
const peutSInscrire = estValide && email.includes("@");
console.log("Peut s'inscrire:", peutSInscrire); // true
```
{% endtab %}
{% endtabs %}

## ❓ undefined et null : Les "vides"

### 🎯 Différences importantes

{% hint style="warning" %}
**undefined vs null :** Deux façons de représenter "rien", mais avec des nuances importantes !
{% endhint %}

{% tabs %}
{% tab title="undefined : Non défini" %}
```javascript
// ❓ undefined : JavaScript dit "je ne sais pas"

// 1. Variable déclarée mais pas initialisée
let prenom;
console.log(prenom);        // undefined
console.log(typeof prenom); // "undefined"

// 2. Propriété qui n'existe pas
const personne = { nom: "Alice" };
console.log(personne.age);  // undefined (propriété absente)

// 3. Fonction sans return
function direBonjour() {
    console.log("Bonjour !");
    // Pas de return
}
const resultat = direBonjour(); // Affiche "Bonjour !"
console.log(resultat);          // undefined

// 4. Paramètre de fonction non fourni
function presenter(nom, age) {
    console.log(`Je suis ${nom}, j'ai ${age} ans`);
}
presenter("Alice"); // "Je suis Alice, j'ai undefined ans"

// 5. Vérifications d'undefined
let variable;
console.log(variable === undefined);     // true
console.log(typeof variable === "undefined"); // true

// Bonne pratique : vérifier avant utilisation
if (prenom !== undefined) {
    console.log("Prénom défini:", prenom);
} else {
    console.log("Prénom non défini");
}
```
{% endtab %}

{% tab title="null : Intentionnellement vide" %}
```javascript
// 🚫 null : Le développeur dit "c'est vide volontairement"

// 1. Assignation explicite
let adresse = null;           // Je sais qu'il n'y a pas d'adresse
let telephone = null;         // Je sais qu'il n'y a pas de téléphone
let photo = null;             // Je sais qu'il n'y a pas de photo

console.log(adresse);         // null
console.log(typeof adresse);  // "object" (bizarrerie de JavaScript !)

// 2. Réinitialisation d'une variable
let dataUtilisateur = { nom: "Alice", age: 25 };
console.log(dataUtilisateur); // { nom: "Alice", age: 25 }

// Plus tard : l'utilisateur se déconnecte
dataUtilisateur = null;
console.log(dataUtilisateur); // null

// 3. Valeur par défaut "vide"
function creerProfil(nom, photo = null) {
    return {
        nom: nom,
        photo: photo,
        creeLe: new Date()
    };
}

const profil1 = creerProfil("Alice");
const profil2 = creerProfil("Bob", "bob.jpg");

console.log(profil1.photo);   // null
console.log(profil2.photo);   // "bob.jpg"

// 4. Vérifications de null
console.log(telephone === null);        // true
console.log(telephone == null);         // true
console.log(telephone == undefined);    // true (attention !)
console.log(telephone === undefined);   // false
```
{% endtab %}

{% tab title="Comparaison undefined vs null" %}
```javascript
// 🔍 Comparaison détaillée undefined vs null

console.log("=== Comparaison undefined vs null ===");

// Déclarations
let variableUndefined;           // undefined automatique
let variableNull = null;         // null explicite

// Types
console.log("Type undefined:", typeof variableUndefined); // "undefined"
console.log("Type null:", typeof variableNull);          // "object" ⚠️

// Comparaisons
console.log("undefined == null:", variableUndefined == variableNull);   // true
console.log("undefined === null:", variableUndefined === variableNull); // false

// Conversions en boolean
console.log("undefined en boolean:", Boolean(variableUndefined)); // false
console.log("null en boolean:", Boolean(variableNull));          // false

// Dans des conditions
if (!variableUndefined) {
    console.log("undefined est falsy"); // S'exécute
}

if (!variableNull) {
    console.log("null est falsy"); // S'exécute
}

// Bonnes pratiques
function obtenirValeurSure(valeur) {
    // Vérifier les deux
    if (valeur === null || valeur === undefined) {
        return "Valeur manquante";
    }
    return valeur;
}

// Ou plus court avec nullish coalescing (??)
function obtenirValeurOuDefaut(valeur, defaut = "Non spécifié") {
    return valeur ?? defaut; // null ou undefined → défaut
}

console.log(obtenirValeurSure(undefined)); // "Valeur manquante"
console.log(obtenirValeurSure(null));      // "Valeur manquante"
console.log(obtenirValeurSure("Alice"));   // "Alice"

console.log(obtenirValeurOuDefaut(undefined)); // "Non spécifié"
console.log(obtenirValeurOuDefaut(null));      // "Non spécifié"
console.log(obtenirValeurOuDefaut("Bob"));     // "Bob"
```
{% endtab %}
{% endtabs %}

## 🔍 typeof : Identifier les types

### 🎯 L'opérateur typeof

{% hint style="info" %}
**`typeof` = detecteur de type :** Indispensable pour vérifier le type d'une donnée avant de l'utiliser !
{% endhint %}

{% tabs %}
{% tab title="Utilisation de base" %}
```javascript
// 🔍 typeof : connaître le type d'une donnée

// Exemples avec tous les types
const texte = "Bonjour";
const nombre = 42;
const decimal = 3.14;
const vrai = true;
const faux = false;
let indefini;
const vide = null;
const objet = { nom: "Alice" };
const tableau = [1, 2, 3];
const fonction = function() { return "test"; };

// Vérifications des types
console.log("typeof texte:", typeof texte);         // "string"
console.log("typeof nombre:", typeof nombre);       // "number"
console.log("typeof decimal:", typeof decimal);     // "number"
console.log("typeof vrai:", typeof vrai);           // "boolean"
console.log("typeof faux:", typeof faux);           // "boolean"
console.log("typeof indefini:", typeof indefini);   // "undefined"
console.log("typeof vide:", typeof vide);           // "object" ⚠️
console.log("typeof objet:", typeof objet);         // "object"
console.log("typeof tableau:", typeof tableau);     // "object" ⚠️
console.log("typeof fonction:", typeof fonction);   // "function"

// ⚠️ Bizarreries à retenir :
// - null retourne "object" (bug historique de JavaScript)
// - Les tableaux retournent "object" (techniquement correct)
// - Les fonctions retournent "function" (cas spécial d'object)
```
{% endtab %}

{% tab title="Vérifications pratiques" %}
```javascript
// 🎯 Vérifications pratiques avec typeof

function analyserDonnee(donnee) {
    const type = typeof donnee;
    
    switch (type) {
        case "string":
            console.log(`📝 C'est du texte: "${donnee}" (${donnee.length} caractères)`);
            break;
            
        case "number":
            if (Number.isInteger(donnee)) {
                console.log(`🔢 C'est un nombre entier: ${donnee}`);
            } else {
                console.log(`🔢 C'est un nombre décimal: ${donnee}`);
            }
            break;
            
        case "boolean":
            console.log(`✅ C'est un boolean: ${donnee ? "VRAI" : "FAUX"}`);
            break;
            
        case "undefined":
            console.log(`❓ C'est undefined (non défini)`);
            break;
            
        case "object":
            if (donnee === null) {
                console.log(`🚫 C'est null (vide intentionnel)`);
            } else if (Array.isArray(donnee)) {
                console.log(`📋 C'est un tableau avec ${donnee.length} éléments`);
            } else {
                console.log(`📦 C'est un objet`);
            }
            break;
            
        case "function":
            console.log(`⚙️ C'est une fonction`);
            break;
            
        default:
            console.log(`❔ Type inconnu: ${type}`);
    }
}

// Tests
analyserDonnee("Hello World");          // 📝 C'est du texte
analyserDonnee(42);                     // 🔢 C'est un nombre entier
analyserDonnee(3.14);                   // 🔢 C'est un nombre décimal
analyserDonnee(true);                   // ✅ C'est un boolean: VRAI
analyserDonnee(undefined);              // ❓ C'est undefined
analyserDonnee(null);                   // 🚫 C'est null
analyserDonnee([1, 2, 3]);             // 📋 C'est un tableau avec 3 éléments
analyserDonnee({ nom: "Alice" });       // 📦 C'est un objet
analyserDonnee(function() {});         // ⚙️ C'est une fonction
```
{% endtab %}

{% tab title="Fonctions de validation" %}
```javascript
// 🛡️ Fonctions utilitaires pour valider les types

// Vérifications de type sécurisées
function estString(valeur) {
    return typeof valeur === "string";
}

function estNombre(valeur) {
    return typeof valeur === "number" && !isNaN(valeur);
}

function estBoolean(valeur) {
    return typeof valeur === "boolean";
}

function estDefini(valeur) {
    return valeur !== undefined && valeur !== null;
}

function estVide(valeur) {
    return valeur === undefined || valeur === null;
}

function estTableau(valeur) {
    return Array.isArray(valeur);
}

function estObjet(valeur) {
    return typeof valeur === "object" && valeur !== null && !Array.isArray(valeur);
}

// Fonction complète de validation
function validerDonnees(nom, age, email, estActif) {
    const erreurs = [];
    
    if (!estString(nom) || nom.trim().length === 0) {
        erreurs.push("Le nom doit être un texte non vide");
    }
    
    if (!estNombre(age) || age < 0 || age > 150) {
        erreurs.push("L'âge doit être un nombre entre 0 et 150");
    }
    
    if (!estString(email) || !email.includes("@")) {
        erreurs.push("L'email doit être un texte contenant @");
    }
    
    if (!estBoolean(estActif)) {
        erreurs.push("Le statut actif doit être true ou false");
    }
    
    if (erreurs.length > 0) {
        console.log("❌ Erreurs de validation:");
        erreurs.forEach(erreur => console.log(`  - ${erreur}`));
        return false;
    }
    
    console.log("✅ Toutes les données sont valides");
    return true;
}

// Tests de validation
validerDonnees("Alice", 25, "alice@example.com", true);    // ✅ Valide
validerDonnees("", 25, "alice@example.com", true);         // ❌ Nom vide
validerDonnees("Bob", "vingt-cinq", "bob@example.com", true); // ❌ Age pas un nombre
validerDonnees("Charlie", 30, "charlieexample.com", true); // ❌ Email sans @
validerDonnees("David", 35, "david@example.com", "oui");   // ❌ Boolean invalide
```
{% endtab %}
{% endtabs %}

## 🧪 Exercices pratiques

### 🎯 Exercice 1 : Analyseur de profil utilisateur

{% hint style="info" %}
**Objectif :** Créer une fonction qui analyse et valide un profil utilisateur
{% endhint %}

```javascript
// Créez une fonction analyserProfil() qui :
// - Prend un objet profil en paramètre
// - Vérifie le type de chaque propriété
// - Affiche un rapport détaillé
// - Retourne true si tout est valide

const profil1 = {
    nom: "Alice Dupont",
    age: 28,
    estActif: true,
    email: "alice@example.com"
};

const profil2 = {
    nom: 123,
    age: "vingt-huit",
    estActif: "oui",
    email: null
};
```

<details>
<summary>💡 Solution</summary>

```javascript
// 🔍 Analyseur de profil utilisateur complet

function analyserProfil(profil) {
    console.log("🔍 Analyse du profil utilisateur");
    console.log("=" .repeat(40));
    
    // Vérifier que le paramètre est un objet
    if (typeof profil !== "object" || profil === null) {
        console.log("❌ Le profil doit être un objet");
        return false;
    }
    
    // Configuration des validations attendues
    const validations = {
        nom: {
            type: "string",
            requis: true,
            validator: (valeur) => valeur.trim().length >= 2,
            message: "Le nom doit être un texte d'au moins 2 caractères"
        },
        age: {
            type: "number",
            requis: true,
            validator: (valeur) => Number.isInteger(valeur) && valeur >= 0 && valeur <= 150,
            message: "L'âge doit être un nombre entier entre 0 et 150"
        },
        estActif: {
            type: "boolean",
            requis: true,
            validator: () => true,
            message: "Le statut actif doit être true ou false"
        },
        email: {
            type: "string",
            requis: false,
            validator: (valeur) => valeur.includes("@") && valeur.includes("."),
            message: "L'email doit contenir @ et un point"
        }
    };
    
    let profilValide = true;
    const resultats = [];
    
    // Analyser chaque propriété attendue
    for (const [propriete, config] of Object.entries(validations)) {
        const valeur = profil[propriete];
        const typeActuel = typeof valeur;
        const estDefini = valeur !== undefined && valeur !== null;
        
        let statut = "✅";
        let details = "";
        
        // Vérifier si la propriété est présente
        if (!estDefini) {
            if (config.requis) {
                statut = "❌";
                details = "Propriété manquante (requise)";
                profilValide = false;
            } else {
                statut = "⚠️";
                details = "Propriété optionnelle absente";
            }
        }
        // Vérifier le type
        else if (typeActuel !== config.type) {
            statut = "❌";
            details = `Type incorrect: ${typeActuel} (attendu: ${config.type})`;
            profilValide = false;
        }
        // Vérifier avec le validator personnalisé
        else if (!config.validator(valeur)) {
            statut = "❌";
            details = config.message;
            profilValide = false;
        }
        // Tout est bon
        else {
            details = `Valide (${config.type}): ${JSON.stringify(valeur)}`;
        }
        
        resultats.push({
            propriete,
            statut,
            valeur,
            type: typeActuel,
            details
        });
    }
    
    // Vérifier les propriétés supplémentaires
    const proprietesInconnues = Object.keys(profil).filter(
        prop => !(prop in validations)
    );
    
    if (proprietesInconnues.length > 0) {
        proprietesInconnues.forEach(prop => {
            resultats.push({
                propriete: prop,
                statut: "⚠️",
                valeur: profil[prop],
                type: typeof profil[prop],
                details: "Propriété non attendue"
            });
        });
    }
    
    // Afficher les résultats
    resultats.forEach(resultat => {
        console.log(
            `${resultat.statut} ${resultat.propriete}: ${resultat.details}`
        );
    });
    
    console.log("=" .repeat(40));
    
    if (profilValide) {
        console.log("🎉 Profil valide !");
        
        // Statistiques supplémentaires
        const stats = {
            nombreProprietes: Object.keys(profil).length,
            typesUtilises: [...new Set(Object.values(profil).map(v => typeof v))],
            tailleNom: profil.nom ? profil.nom.length : 0
        };
        
        console.log("\n📊 Statistiques:");
        console.log(`  - Nombre de propriétés: ${stats.nombreProprietes}`);
        console.log(`  - Types utilisés: ${stats.typesUtilises.join(", ")}`);
        if (stats.tailleNom > 0) {
            console.log(`  - Longueur du nom: ${stats.tailleNom} caractères`);
        }
    } else {
        console.log("❌ Profil invalide - vérifiez les erreurs ci-dessus");
    }
    
    return profilValide;
}

// 🧪 Tests avec différents profils

console.log("Test 1 - Profil valide:");
const profil1 = {
    nom: "Alice Dupont",
    age: 28,
    estActif: true,
    email: "alice@example.com"
};
analyserProfil(profil1);

console.log("\n" + "=".repeat(50) + "\n");

console.log("Test 2 - Profil avec erreurs:");
const profil2 = {
    nom: 123,
    age: "vingt-huit",
    estActif: "oui",
    email: null
};
analyserProfil(profil2);

console.log("\n" + "=".repeat(50) + "\n");

console.log("Test 3 - Profil incomplet:");
const profil3 = {
    nom: "Bob",
    age: 35
    // estActif et email manquants
};
analyserProfil(profil3);

console.log("\n" + "=".repeat(50) + "\n");

console.log("Test 4 - Profil avec propriétés supplémentaires:");
const profil4 = {
    nom: "Charlie Martin",
    age: 42,
    estActif: false,
    email: "charlie@example.com",
    ville: "Paris",           // Propriété supplémentaire
    telephone: "0123456789"   // Propriété supplémentaire
};
analyserProfil(profil4);
```

</details>

### 🎯 Exercice 2 : Calculatrice intelligente

{% hint style="info" %}
**Objectif :** Créer une calculatrice qui gère différents types d'entrées
{% endhint %}

```javascript
// Créez une fonction calculer() qui :
// - Accepte deux paramètres de n'importe quel type
// - Convertit automatiquement en nombres si possible
// - Gère les erreurs de type gracieusement
// - Retourne un objet avec le résultat et des informations

calculer("5", "3");      // Devrait convertir et additionner
calculer(10, true);      // true = 1 en nombre
calculer("abc", 5);      // Erreur de conversion
```

<details>
<summary>💡 Solution</summary>

```javascript
// 🧮 Calculatrice intelligente avec gestion des types

function calculer(a, b, operation = "addition") {
    console.log(`🧮 Calcul: ${JSON.stringify(a)} ${operation} ${JSON.stringify(b)}`);
    
    // Analyser les types d'entrée
    const typeA = typeof a;
    const typeB = typeof b;
    
    console.log(`   Types reçus: ${typeA}, ${typeB}`);
    
    // Résultat par défaut
    const resultat = {
        valeurA: a,
        valeurB: b,
        typeOriginalA: typeA,
        typeOriginalB: typeB,
        valeurConvertieA: null,
        valeurConvertieB: null,
        conversionReussie: false,
        resultatCalcul: null,
        erreurs: [],
        avertissements: []
    };
    
    // Fonction de conversion intelligente
    function convertirEnNombre(valeur, nom) {
        const typeOriginal = typeof valeur;
        let converti = null;
        let succes = false;
        let avertissement = null;
        
        switch (typeOriginal) {
            case "number":
                if (isNaN(valeur)) {
                    resultat.erreurs.push(`${nom} est NaN`);
                } else if (!isFinite(valeur)) {
                    resultat.erreurs.push(`${nom} est infini`);
                } else {
                    converti = valeur;
                    succes = true;
                }
                break;
                
            case "string":
                const trimmed = valeur.trim();
                if (trimmed === "") {
                    resultat.erreurs.push(`${nom} est une chaîne vide`);
                } else {
                    converti = Number(trimmed);
                    if (isNaN(converti)) {
                        resultat.erreurs.push(`${nom} "${valeur}" ne peut pas être converti en nombre`);
                    } else {
                        succes = true;
                        avertissement = `${nom} converti de string vers number`;
                    }
                }
                break;
                
            case "boolean":
                converti = valeur ? 1 : 0;
                succes = true;
                avertissement = `${nom} converti: ${valeur} → ${converti}`;
                break;
                
            case "undefined":
                resultat.erreurs.push(`${nom} est undefined`);
                break;
                
            case "object":
                if (valeur === null) {
                    resultat.erreurs.push(`${nom} est null`);
                } else {
                    resultat.erreurs.push(`${nom} est un objet complexe`);
                }
                break;
                
            default:
                resultat.erreurs.push(`${nom} a un type non supporté: ${typeOriginal}`);
        }
        
        if (avertissement) {
            resultat.avertissements.push(avertissement);
        }
        
        return { converti, succes };
    }
    
    // Convertir les deux valeurs
    const conversionA = convertirEnNombre(a, "Première valeur");
    const conversionB = convertirEnNombre(b, "Deuxième valeur");
    
    resultat.valeurConvertieA = conversionA.converti;
    resultat.valeurConvertieB = conversionB.converti;
    resultat.conversionReussie = conversionA.succes && conversionB.succes;
    
    // Effectuer le calcul si les conversions ont réussi
    if (resultat.conversionReussie) {
        const numA = resultat.valeurConvertieA;
        const numB = resultat.valeurConvertieB;
        
        switch (operation) {
            case "addition":
                resultat.resultatCalcul = numA + numB;
                break;
            case "soustraction":
                resultat.resultatCalcul = numA - numB;
                break;
            case "multiplication":
                resultat.resultatCalcul = numA * numB;
                break;
            case "division":
                if (numB === 0) {
                    resultat.erreurs.push("Division par zéro impossible");
                } else {
                    resultat.resultatCalcul = numA / numB;
                }
                break;
            default:
                resultat.erreurs.push(`Opération inconnue: ${operation}`);
        }
    }
    
    // Afficher le rapport
    console.log("📊 Rapport de conversion:");
    if (resultat.avertissements.length > 0) {
        resultat.avertissements.forEach(warning => 
            console.log(`   ⚠️ ${warning}`)
        );
    }
    
    if (resultat.erreurs.length > 0) {
        console.log("❌ Erreurs:");
        resultat.erreurs.forEach(erreur => 
            console.log(`   • ${erreur}`)
        );
    }
    
    if (resultat.conversionReussie && resultat.resultatCalcul !== null) {
        console.log(`✅ Résultat: ${resultat.valeurConvertieA} + ${resultat.valeurConvertieB} = ${resultat.resultatCalcul}`);
    } else {
        console.log("❌ Calcul impossible");
    }
    
    console.log("");
    return resultat;
}

// Fonction helper pour tester différentes opérations
function testerCalculs() {
    const tests = [
        // [valeurA, valeurB, operation, description]
        ["5", "3", "addition", "Strings numériques"],
        [10, true, "multiplication", "Nombre et boolean"],
        ["12.5", false, "soustraction", "String décimal et boolean false"],
        ["abc", 5, "addition", "String non numérique"],
        [null, "10", "addition", "null et string"],
        [undefined, 7, "addition", "undefined et nombre"],
        ["", "5", "addition", "String vide"],
        [15, 0, "division", "Division par zéro"],
        [" 8 ", " 2 ", "multiplication", "Strings avec espaces"],
        [{}, [], "addition", "Objets complexes"]
    ];
    
    console.log("🧪 Tests de la calculatrice intelligente");
    console.log("=".repeat(60));
    
    tests.forEach((test, index) => {
        console.log(`\nTest ${index + 1}: ${test[3]}`);
        console.log("-".repeat(30));
        calculer(test[0], test[1], test[2]);
    });
}

// Lancer les tests
testerCalculs();

// Tests supplémentaires pour toutes les opérations
console.log("🔄 Tests des opérations mathématiques");
console.log("=".repeat(40));

const valeursTest = [6, 3];
const operations = ["addition", "soustraction", "multiplication", "division"];

operations.forEach(op => {
    console.log(`\nOpération: ${op}`);
    calculer(valeursTest[0], valeursTest[1], op);
});
```

</details>

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Que va afficher ce code ?
```javascript
console.log(typeof "42");
console.log(typeof 42);
console.log("42" + 42);
console.log(42 + 42);
```
{% endhint %}

<details>
<summary>💡 Réponse</summary>

```
"string"
"number"
"4242"
84
```

**Explication :**
- `"42"` est un string → `typeof` retourne `"string"`
- `42` est un number → `typeof` retourne `"number"`
- `"42" + 42` → concaténation car l'un est string → `"4242"`
- `42 + 42` → addition mathématique car les deux sont numbers → `84`

</details>

{% hint style="info" %}
**Question 2 :** Quelle est la différence entre `null` et `undefined` ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**`undefined` :** JavaScript dit "je ne sais pas"
- Variable déclarée mais pas initialisée
- Propriété qui n'existe pas
- Valeur automatique

**`null` :** Le développeur dit "c'est vide volontairement"
- Assignation explicite
- Représente "rien" intentionnellement
- Valeur choisie par le programmeur

**Bizarre :** `typeof null` retourne `"object"` (bug historique de JavaScript)

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="conversion-types.md" %}
[conversion-types.md](conversion-types.md)
{% endcontent-ref %}

{% content-ref url="comparaisons-egalite.md" %}
[comparaisons-egalite.md](comparaisons-egalite.md)
{% endcontent-ref %}

{% content-ref url="operateurs-logiques.md" %}
[operateurs-logiques.md](operateurs-logiques.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Maintenant que vous maîtrisez les types :**

👉 **Explorez** la [Conversion de Types](conversion-types.md)  
👉 **Approfondissez** les [Comparaisons et Égalité](comparaisons-egalite.md)  
👉 **Découvrez** les [Structures de Contrôle](structures-controle.md)  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

📝 **string** : Texte entre guillemets `"Hello"`  
🔢 **number** : Tous les nombres `42`, `3.14`  
✅ **boolean** : Seulement `true` ou `false`  
❓ **undefined** : Non défini automatiquement  
🚫 **null** : Vide volontairement  
🔍 **typeof** : Pour vérifier le type d'une donnée  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Découvrons la [Conversion de Types](conversion-types.md) !
{% endhint %}
