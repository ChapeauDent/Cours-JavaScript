# 🚨 Gestion d'Erreurs en JavaScript

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- ✅ **Comprendre** le concept d'erreur et pourquoi les gérer
- ✅ **Appliquer** les blocs try/catch/finally pour capturer les erreurs
- ✅ **Analyser** les différents types d'erreurs JavaScript
- ✅ **Créer** des erreurs personnalisées avec throw

{% hint style="info" %}
**Prérequis :** Variables, fonctions, structures de contrôle, objets
**Durée estimée :** 45 minutes
**Niveau :** Intermédiaire
{% endhint %}

## 🧠 Comprendre les erreurs

### Pourquoi gérer les erreurs ?

Dans le monde réel, votre code JavaScript peut rencontrer des situations imprévues :

{% hint style="warning" %}
- 📁 Un fichier qui n'existe pas
- 🌐 Une connexion internet coupée  
- 📊 Des données incorrectes
- 💾 Un espace disque plein
{% endhint %}

Sans gestion d'erreurs, ces problèmes feraient **planter votre application entière** ! 

### Le concept d'exception

{% hint style="danger" %}
**Une erreur (ou exception)** est un événement qui interrompt le flux normal d'exécution du programme.
{% endhint %}

### Mots-clés essentiels

| Terme | Définition | Exemple |
|-------|------------|---------|
| **Exception** | Erreur qui interrompt l'exécution | `ReferenceError` |
| **try/catch** | Structure pour gérer les erreurs | `try { ... } catch { ... }` |
| **throw** | Lancer une erreur personnalisée | `throw new Error("...")` |
| **finally** | Code exécuté dans tous les cas | `finally { ... }` |

## 💻 Syntaxe et mécanismes

### Structure try/catch de base

{% tabs %}
{% tab title="Syntaxe simple" %}
```javascript
try {
    // Code qui peut générer une erreur
    let resultat = functionRisquee();
    console.log(resultat);
} catch (error) {
    // Code exécuté si une erreur survient
    console.log("Erreur détectée :", error.message);
}
```
{% endtab %}

{% tab title="Avec finally" %}
```javascript
try {
    // Code à risque
    connecterBaseDeDonnees();
    traiterDonnees();
} catch (error) {
    // Gestion de l'erreur
    console.log("Erreur :", error.message);
} finally {
    // Code exécuté TOUJOURS
    fermerConnexion();
    console.log("Nettoyage terminé");
}
```
{% endtab %}

{% tab title="Lancer des erreurs" %}
```javascript
function diviser(a, b) {
    if (b === 0) {
        throw new Error("Division par zéro impossible");
    }
    return a / b;
}

try {
    let resultat = diviser(10, 0);
} catch (error) {
    console.log("Problème :", error.message);
}
```
{% endtab %}
{% endtabs %}

### Types d'erreurs courantes

{% tabs %}
{% tab title="ReferenceError" %}
```javascript
try {
    console.log(variableInexistante);
} catch (error) {
    console.log(error.name); // "ReferenceError"
    console.log(error.message); // "variableInexistante is not defined"
}
```

Se produit quand on utilise une variable non déclarée.
{% endtab %}

{% tab title="TypeError" %}
```javascript
try {
    let nombre = null;
    nombre.toUpperCase(); // null n'a pas de méthode toUpperCase
} catch (error) {
    console.log(error.name); // "TypeError" 
    console.log(error.message); // "Cannot read property 'toUpperCase' of null"
}
```

Se produit quand on utilise une méthode sur le mauvais type.
{% endtab %}

{% tab title="SyntaxError" %}
```javascript
try {
    eval("let x = ;"); // Syntaxe invalide
} catch (error) {
    console.log(error.name); // "SyntaxError"
    console.log(error.message); // "Unexpected token ;"
}
```

Se produit quand le code a une syntaxe incorrecte.
{% endtab %}
{% endtabs %}

## 🎮 Exercices interactifs

### 🧮 Exercice 1 : Calculatrice sécurisée

Créons une calculatrice qui gère tous les cas d'erreur !

{% tabs %}
{% tab title="🎯 Objectif" %}
**Créer une fonction calculatrice qui gère :**
- ➕ Opérations de base (+, -, *, /)
- 🚫 Division par zéro
- ❌ Opération invalide  
- 🔢 Paramètres non-numériques

**Niveau :** ⭐⭐⭐ (3/5)
{% endtab %}

{% tab title="🏗️ Code de départ" %}
```javascript
function calculatrice(operation, a, b) {
    // TODO: Ajouter la gestion d'erreurs
    // Operations supportées: +, -, *, /
    // Gérer: division par zéro, opération invalide, paramètres non-numériques
}

// Tests
console.log(calculatrice("+", 5, 3)); // Devrait retourner 8
console.log(calculatrice("/", 10, 0)); // Erreur: Division par zéro
console.log(calculatrice("x", 4, 2)); // Erreur: Opération invalide
console.log(calculatrice("+", "a", 3)); // Erreur: Paramètres non numériques
```
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
function calculatrice(operation, a, b) {
    try {
        // Vérification des types
        if (typeof a !== "number" || typeof b !== "number") {
            throw new Error("Les paramètres doivent être des nombres");
        }
        
        // Vérification de l'opération
        switch (operation) {
            case "+":
                return a + b;
            case "-":
                return a - b;
            case "*":
                return a * b;
            case "/":
                if (b === 0) {
                    throw new Error("Division par zéro impossible");
                }
                return a / b;
            default:
                throw new Error(`Opération "${operation}" non supportée`);
        }
    } catch (error) {
        console.log("❌ Erreur:", error.message);
        return null; // Valeur de retour pour les erreurs
    }
}

// Tests complets
console.log("=== TESTS CALCULATRICE ===");
console.log("5 + 3 =", calculatrice("+", 5, 3)); // 8
console.log("10 - 4 =", calculatrice("-", 10, 4)); // 6
console.log("7 * 2 =", calculatrice("*", 7, 2)); // 14
console.log("15 / 3 =", calculatrice("/", 15, 3)); // 5

console.log("\n=== TESTS D'ERREURS ===");
calculatrice("/", 10, 0); // Division par zéro
calculatrice("x", 4, 2); // Opération invalide
calculatrice("+", "a", 3); // Type incorrect
calculatrice("^", 2, 3); // Opération inconnue
```

**Résultat attendu :**
```
=== TESTS CALCULATRICE ===
5 + 3 = 8
10 - 4 = 6
7 * 2 = 14
15 / 3 = 5

=== TESTS D'ERREURS ===
❌ Erreur: Division par zéro impossible
❌ Erreur: Les paramètres doivent être des nombres
❌ Erreur: Opération "x" non supportée
❌ Erreur: Opération "^" non supportée
```
{% endtab %}
{% endtabs %}

### 📁 Exercice 2 : Gestionnaire de fichiers simulé

{% tabs %}
{% tab title="🎯 Objectif" %}
**Simuler la lecture de fichiers avec :**
- 📂 Vérification d'existence
- 🔒 Gestion des permissions
- 🧹 Nettoyage avec finally
- 💡 Messages d'erreur clairs

**Niveau :** ⭐⭐⭐⭐ (4/5)
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
// Simulation d'un système de fichiers
const systemeFileNotFound = {
    fichiers: {
        "document.txt": {
            contenu: "Ceci est le contenu du document.",
            permissions: "lecture"
        },
        "secret.txt": {
            contenu: "Informations confidentielles",
            permissions: "aucune"
        },
        "config.json": {
            contenu: '{"theme": "dark", "langue": "fr"}',
            permissions: "lecture"
        }
    }
};

function lireFichier(nomFichier) {
    let fichierOuvert = false;
    let verrous = [];
    
    try {
        console.log(`📂 Tentative d'ouverture: ${nomFichier}`);
        fichierOuvert = true;
        verrous.push("file_handle");
        
        // Vérifier l'existence
        if (!systemeFileNotFound.fichiers.hasOwnProperty(nomFichier)) {
            throw new Error(`Fichier "${nomFichier}" introuvable`);
        }
        
        const fichier = systemeFileNotFound.fichiers[nomFichier];
        
        // Vérifier les permissions
        if (fichier.permissions === "aucune") {
            throw new Error(`Accès refusé au fichier "${nomFichier}"`);
        }
        
        console.log("✅ Lecture réussie");
        return {
            succes: true,
            contenu: fichier.contenu,
            taille: fichier.contenu.length
        };
        
    } catch (error) {
        console.log(`❌ Erreur de lecture: ${error.message}`);
        return {
            succes: false,
            erreur: error.message,
            contenu: null
        };
        
    } finally {
        // Nettoyage obligatoire
        if (fichierOuvert) {
            console.log("🔒 Fermeture du fichier");
        }
        
        // Libérer les verrous
        while (verrous.length > 0) {
            verrous.pop();
            console.log("🔓 Verrou libéré");
        }
        
        console.log("🧹 Nettoyage terminé\n");
    }
}

// Gestionnaire de lecture multiple
function lirePlusieursFichiers(listeFichiers) {
    const resultats = [];
    
    console.log("=== LECTURE MULTIPLE DE FICHIERS ===");
    
    for (let i = 0; i < listeFichiers.length; i++) {
        const nomFichier = listeFichiers[i];
        console.log(`📋 Traitement ${i + 1}/${listeFichiers.length}`);
        
        const resultat = lireFichier(nomFichier);
        resultats.push({
            fichier: nomFichier,
            ...resultat
        });
    }
    
    // Rapport final
    const succes = resultats.filter(r => r.succes).length;
    const echecs = resultats.length - succes;
    
    console.log("📊 === RAPPORT FINAL ===");
    console.log(`✅ Succès: ${succes}`);
    console.log(`❌ Échecs: ${echecs}`);
    
    return resultats;
}

// Tests du gestionnaire
const fichiers = [
    "document.txt",    // Existe et accessible
    "inexistant.txt",  // N'existe pas
    "secret.txt",      // Existe mais pas de permissions
    "config.json"      // Existe et accessible
];

lirePlusieursFichiers(fichiers);
```
{% endtab %}
{% endtabs %}

### 🎨 Exercice 3 : Validateur de formulaire avec erreurs personnalisées

{% tabs %}
{% tab title="🎯 Objectif" %}
**Créer un système de validation avec :**
- 📧 Validation email
- 🔐 Validation mot de passe
- 🎂 Validation âge
- 🏷️ Erreurs personnalisées par champ

**Niveau :** ⭐⭐⭐⭐⭐ (5/5)
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
// Classe d'erreur personnalisée
class ErreurValidation extends Error {
    constructor(message, champ, valeur, code = null) {
        super(message);
        this.name = "ErreurValidation";
        this.champ = champ;
        this.valeur = valeur;
        this.code = code;
        this.timestamp = new Date().toISOString();
    }
    
    toString() {
        return `${this.name} [${this.champ}]: ${this.message}`;
    }
}

class ValidateurFormulaire {
    constructor() {
        this.regles = {
            email: {
                regex: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
                requis: true,
                message: "Adresse email invalide"
            },
            motDePasse: {
                longueurMin: 8,
                requis: true,
                caractereSpecial: /[!@#$%^&*(),.?":{}|<>]/,
                majuscule: /[A-Z]/,
                minuscule: /[a-z]/,
                chiffre: /[0-9]/
            },
            age: {
                min: 13,
                max: 120,
                requis: true
            }
        };
    }
    
    validerEmail(email) {
        if (!email || email.trim() === "") {
            throw new ErreurValidation("Email requis", "email", email, "REQUIRED");
        }
        
        if (!this.regles.email.regex.test(email)) {
            throw new ErreurValidation("Format d'email invalide", "email", email, "INVALID_FORMAT");
        }
        
        // Domaines interdits (exemple)
        const domainesInterdits = ["temp-mail.org", "10minutemail.com"];
        const domaine = email.split("@")[1];
        
        if (domainesInterdits.includes(domaine)) {
            throw new ErreurValidation("Domaine email non autorisé", "email", email, "FORBIDDEN_DOMAIN");
        }
        
        return true;
    }
    
    validerMotDePasse(motDePasse) {
        if (!motDePasse || motDePasse.length === 0) {
            throw new ErreurValidation("Mot de passe requis", "motDePasse", "", "REQUIRED");
        }
        
        if (motDePasse.length < this.regles.motDePasse.longueurMin) {
            throw new ErreurValidation(
                `Mot de passe trop court (minimum ${this.regles.motDePasse.longueurMin} caractères)`,
                "motDePasse",
                motDePasse.length,
                "TOO_SHORT"
            );
        }
        
        if (!this.regles.motDePasse.majuscule.test(motDePasse)) {
            throw new ErreurValidation("Le mot de passe doit contenir au moins une majuscule", "motDePasse", motDePasse, "NO_UPPERCASE");
        }
        
        if (!this.regles.motDePasse.minuscule.test(motDePasse)) {
            throw new ErreurValidation("Le mot de passe doit contenir au moins une minuscule", "motDePasse", motDePasse, "NO_LOWERCASE");
        }
        
        if (!this.regles.motDePasse.chiffre.test(motDePasse)) {
            throw new ErreurValidation("Le mot de passe doit contenir au moins un chiffre", "motDePasse", motDePasse, "NO_NUMBER");
        }
        
        if (!this.regles.motDePasse.caractereSpecial.test(motDePasse)) {
            throw new ErreurValidation("Le mot de passe doit contenir au moins un caractère spécial", "motDePasse", motDePasse, "NO_SPECIAL");
        }
        
        return true;
    }
    
    validerAge(age) {
        if (age === undefined || age === null || age === "") {
            throw new ErreurValidation("Âge requis", "age", age, "REQUIRED");
        }
        
        const ageNumerique = Number(age);
        
        if (isNaN(ageNumerique)) {
            throw new ErreurValidation("L'âge doit être un nombre", "age", age, "INVALID_TYPE");
        }
        
        if (ageNumerique < this.regles.age.min) {
            throw new ErreurValidation(`Âge minimum requis: ${this.regles.age.min} ans`, "age", ageNumerique, "TOO_YOUNG");
        }
        
        if (ageNumerique > this.regles.age.max) {
            throw new ErreurValidation(`Âge maximum autorisé: ${this.regles.age.max} ans`, "age", ageNumerique, "TOO_OLD");
        }
        
        return true;
    }
    
    validerFormulaire(donnees) {
        const erreurs = [];
        const champsValides = {};
        
        console.log("🔍 === VALIDATION DU FORMULAIRE ===");
        
        // Validation de chaque champ
        const validations = [
            { champ: "email", validateur: () => this.validerEmail(donnees.email) },
            { champ: "motDePasse", validateur: () => this.validerMotDePasse(donnees.motDePasse) },
            { champ: "age", validateur: () => this.validerAge(donnees.age) }
        ];
        
        validations.forEach(({ champ, validateur }) => {
            try {
                validateur();
                champsValides[champ] = true;
                console.log(`✅ ${champ}: Valide`);
            } catch (error) {
                if (error instanceof ErreurValidation) {
                    erreurs.push({
                        champ: error.champ,
                        message: error.message,
                        valeur: error.valeur,
                        code: error.code,
                        timestamp: error.timestamp
                    });
                    console.log(`❌ ${champ}: ${error.message}`);
                } else {
                    // Erreur inattendue
                    erreurs.push({
                        champ: champ,
                        message: "Erreur de validation inattendue",
                        valeur: donnees[champ],
                        code: "UNEXPECTED_ERROR"
                    });
                    console.log(`⚠️ ${champ}: Erreur inattendue`, error);
                }
            }
        });
        
        const estValide = erreurs.length === 0;
        
        console.log(`\n📊 Résultat: ${estValide ? '✅ VALIDE' : '❌ INVALIDE'}`);
        console.log(`Erreurs trouvées: ${erreurs.length}`);
        
        return {
            valide: estValide,
            erreurs: erreurs,
            champsValides: champsValides,
            resume: {
                total: validations.length,
                succes: Object.keys(champsValides).length,
                echecs: erreurs.length
            }
        };
    }
}

// Tests du validateur
const validateur = new ValidateurFormulaire();

// Cas de test
const casDeTest = [
    {
        nom: "Utilisateur valide",
        donnees: {
            email: "alice@example.com",
            motDePasse: "MonMotDePasse123!",
            age: 25
        }
    },
    {
        nom: "Email invalide",
        donnees: {
            email: "email-invalide",
            motDePasse: "MotDePasse123!",
            age: 30
        }
    },
    {
        nom: "Mot de passe faible",
        donnees: {
            email: "bob@example.com",
            motDePasse: "123",
            age: 22
        }
    },
    {
        nom: "Âge trop jeune",
        donnees: {
            email: "enfant@example.com",
            motDePasse: "MotDePasse123!",
            age: 10
        }
    },
    {
        nom: "Données complètement invalides",
        donnees: {
            email: "",
            motDePasse: "",
            age: "abc"
        }
    }
];

// Exécuter tous les tests
casDeTest.forEach((casTest, index) => {
    console.log(`\n${'='.repeat(50)}`);
    console.log(`TEST ${index + 1}: ${casTest.nom}`);
    console.log(`${'='.repeat(50)}`);
    
    const resultat = validateur.validerFormulaire(casTest.donnees);
    
    if (!resultat.valide) {
        console.log("\n📋 DÉTAILS DES ERREURS:");
        resultat.erreurs.forEach((erreur, i) => {
            console.log(`${i + 1}. [${erreur.champ}] ${erreur.message} (Code: ${erreur.code})`);
        });
    }
    
    console.log(`\n📈 Score: ${resultat.resume.succes}/${resultat.resume.total} champs valides`);
});
```
{% endtab %}
{% endtabs %}

## 🔧 Bonnes pratiques

### Types d'erreurs à gérer

{% tabs %}
{% tab title="🌐 Erreurs réseau" %}
```javascript
async function recupererDonnees(url) {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`Erreur HTTP: ${response.status} ${response.statusText}`);
        }
        
        const donnees = await response.json();
        return donnees;
        
    } catch (error) {
        if (error.name === "TypeError") {
            console.log("🌐 Problème de connexion réseau");
        } else {
            console.log("📡 Erreur du serveur:", error.message);
        }
        return null;
    }
}
```
{% endtab %}

{% tab title="📊 Erreurs de données" %}
```javascript
function traiterDonneesUtilisateur(donnees) {
    try {
        // Validation stricte
        if (!donnees || typeof donnees !== 'object') {
            throw new Error("Données utilisateur invalides");
        }
        
        if (!donnees.id || typeof donnees.id !== 'number') {
            throw new Error("ID utilisateur manquant ou invalide");
        }
        
        // Traitement...
        return donnees;
        
    } catch (error) {
        console.log("📊 Erreur de traitement:", error.message);
        return null;
    }
}
```
{% endtab %}

{% tab title="💾 Erreurs de stockage" %}
```javascript
function sauvegarderDonnees(cle, valeur) {
    try {
        const donneesJson = JSON.stringify(valeur);
        localStorage.setItem(cle, donneesJson);
        return true;
        
    } catch (error) {
        if (error.name === "QuotaExceededError") {
            console.log("💾 Espace de stockage plein");
        } else {
            console.log("💾 Erreur de sauvegarde:", error.message);
        }
        return false;
    }
}
```
{% endtab %}
{% endtabs %}

### Erreurs personnalisées avancées

{% tabs %}
{% tab title="Hiérarchie d'erreurs" %}
```javascript
// Classe de base
class AppError extends Error {
    constructor(message, code = null) {
        super(message);
        this.name = this.constructor.name;
        this.code = code;
        this.timestamp = new Date().toISOString();
    }
}

// Erreurs spécialisées
class ValidationError extends AppError {
    constructor(message, field, value) {
        super(message, "VALIDATION_ERROR");
        this.field = field;
        this.value = value;
    }
}

class NetworkError extends AppError {
    constructor(message, status = null) {
        super(message, "NETWORK_ERROR");
        this.status = status;
    }
}

class BusinessError extends AppError {
    constructor(message, rule) {
        super(message, "BUSINESS_ERROR");
        this.rule = rule;
    }
}
```
{% endtab %}

{% tab title="Gestionnaire centralisé" %}
```javascript
class GestionnaireErreurs {
    static gerer(error) {
        // Log détaillé pour le développement
        console.group(`🚨 ${error.name}`);
        console.log("Message:", error.message);
        console.log("Stack:", error.stack);
        
        if (error.code) console.log("Code:", error.code);
        if (error.timestamp) console.log("Timestamp:", error.timestamp);
        
        console.groupEnd();
        
        // Notification utilisateur adaptée
        if (error instanceof ValidationError) {
            this.afficherErreurUtilisateur(`Données invalides: ${error.message}`);
        } else if (error instanceof NetworkError) {
            this.afficherErreurUtilisateur("Problème de connexion. Veuillez réessayer.");
        } else {
            this.afficherErreurUtilisateur("Une erreur inattendue s'est produite.");
        }
    }
    
    static afficherErreurUtilisateur(message) {
        // Interface utilisateur (toast, modal, etc.)
        console.log(`👤 Message utilisateur: ${message}`);
    }
}

// Usage
try {
    throw new ValidationError("Email requis", "email", "");
} catch (error) {
    GestionnaireErreurs.gerer(error);
}
```
{% endtab %}
{% endtabs %}

## ⚠️ Erreurs courantes et solutions

### 1. Oublier le paramètre dans catch

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
try {
    functionRisquee();
} catch {
    // SyntaxError: Missing catch parameter
    console.log("Une erreur est survenue");
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
try {
    functionRisquee();
} catch (error) {
    // Toujours inclure le paramètre
    console.log("Erreur:", error.message);
}

// Ou si vous n'utilisez pas l'erreur
try {
    functionRisquee();
} catch (_) {
    console.log("Une erreur est survenue");
}
```
{% endtab %}
{% endtabs %}

### 2. Retourner une erreur au lieu de la lancer

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
function diviser(a, b) {
    if (b === 0) {
        return "Erreur: division par zéro"; // Type incohérent !
    }
    return a / b;
}

// L'appelant ne sait pas qu'il y a eu une erreur
let resultat = diviser(10, 0);
console.log(resultat * 2); // "Erreur: division par zéro2"
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
function diviser(a, b) {
    if (b === 0) {
        throw new Error("Division par zéro impossible");
    }
    return a / b;
}

// L'appelant peut gérer l'erreur appropriément
try {
    let resultat = diviser(10, 0);
    console.log(resultat * 2);
} catch (error) {
    console.log("Impossible de calculer:", error.message);
}
```
{% endtab %}
{% endtabs %}

### 3. Try/catch trop large

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
try {
    // Trop de code dans un seul try
    let donnees = recupererDonnees();
    let resultat = traiterDonnees(donnees);
    let sauvegarde = sauvegarder(resultat);
    let notification = envoyerNotification();
} catch (error) {
    // Impossible de savoir quelle étape a échoué !
    console.log("Quelque chose a échoué:", error.message);
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
let donnees, resultat, sauvegarde;

try {
    donnees = recupererDonnees();
} catch (error) {
    console.log("Erreur récupération:", error.message);
    return;
}

try {
    resultat = traiterDonnees(donnees);
} catch (error) {
    console.log("Erreur traitement:", error.message);
    return;
}

try {
    sauvegarde = sauvegarder(resultat);
} catch (error) {
    console.log("Erreur sauvegarde:", error.message);
    // Continuer même si la sauvegarde échoue
}

try {
    envoyerNotification();
} catch (error) {
    console.log("Erreur notification:", error.message);
    // Notification non critique
}
```
{% endtab %}
{% endtabs %}

## 🎯 Points clés à retenir

{% hint style="success" %}
**Gestion d'erreurs efficace :**

✅ **Utilisez try/catch** pour les erreurs prévisibles  
✅ **Finally** pour le nettoyage obligatoire  
✅ **Messages clairs** pour faciliter le débogage  
✅ **Erreurs typées** pour différencier les cas  
✅ **Gestion granulaire** plutôt que try/catch trop larges  
{% endhint %}

### Quand utiliser quoi ?

| Situation | Mécanisme | Exemple |
|-----------|-----------|---------|
| Validation d'entrée | `throw new Error()` | Paramètre invalide |
| Opération à risque | `try/catch` | Appel API |
| Nettoyage obligatoire | `finally` | Fermer une connexion |
| Erreur métier | Classe personnalisée | `ValidationError` |

## 🔄 Liens avec d'autres concepts

### Concepts utilisés
- **Fonctions** : Les erreurs surviennent souvent dans les fonctions
- **Objets** : Les objets Error contiennent les informations d'erreur
- **Conditions** : Vérifications avant de lancer des erreurs

### Prochaines étapes
- **Programmation asynchrone** : Gestion d'erreurs avec async/await
- **Testing** : Tester les cas d'erreur
- **Debugging** : Utiliser les erreurs pour déboguer

## ✅ Auto-évaluation

### Quiz rapide

{% tabs %}
{% tab title="Question 1" %}
**Quelle est la différence entre try/catch et if/else ?**

<details>
<summary>Voir la réponse</summary>

**Réponse :** try/catch gère les erreurs qui interrompent l'exécution, if/else gère les conditions logiques normales. try/catch est pour les situations exceptionnelles.
</details>
{% endtab %}

{% tab title="Question 2" %}
**Que fait ce code ?**
```javascript
try {
    console.log("Début");
    throw new Error("Problème");
    console.log("Fin");
} finally {
    console.log("Nettoyage");
}
```

<details>
<summary>Voir la réponse</summary>

**Réponse :** Affiche "Début", puis "Nettoyage". La ligne "Fin" n'est jamais exécutée car l'erreur interrompt l'exécution. Finally s'exécute toujours.
</details>
{% endtab %}

{% tab title="Question 3" %}
**Dans quel cas utiliser finally ?**

<details>
<summary>Voir la réponse</summary>

**Réponse :** Pour nettoyer des ressources (fermer des fichiers, connexions, libérer de la mémoire) qui doivent être libérées quoi qu'il arrive.
</details>
{% endtab %}
{% endtabs %}

### Checklist de maîtrise

- [ ] Je comprends la syntaxe try/catch/finally
- [ ] Je sais identifier quand gérer les erreurs
- [ ] Je peux créer des erreurs personnalisées avec throw
- [ ] Je connais les différents types d'erreurs JavaScript
- [ ] Je peux déboguer les erreurs courantes
- [ ] Je combine la gestion d'erreurs avec d'autres concepts

---

{% hint style="info" %}
**Félicitations !** 🎉

Vous avez terminé le niveau intermédiaire ! Vous maîtrisez maintenant :
- Les arrays et leurs méthodes
- L'Event Loop et la programmation asynchrone
- La gestion d'erreurs robuste

**Prochaine étape :** [Niveau Avancé](../niveau-avance/) pour découvrir les concepts plus poussés !
{% endhint %}
