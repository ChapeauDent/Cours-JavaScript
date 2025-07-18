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
```javascript
// 🥇 RÈGLE D'OR : Toujours commencer par const
const nom = "Alice";
const age = 25;
const hobbies = ["lecture", "jardinage"];

// 🥈 Si réassignation nécessaire, utiliser let
let compteur = 0;
compteur++; // Modification OK

let statut = "en cours";
statut = "terminé"; // Changement OK

// 🚫 Éviter var en JavaScript moderne
// var ancienneVariable = "déconseillé";
```

**Ordre de priorité :**
1. 🥇 `const` par défaut
2. 🥈 `let` si modification nécessaire  
3. 🚫 `var` jamais (sauf cas très spécifiques)
{% endtab %}
{% endtabs %}

## 🔒 const : Variables constantes

### 📋 Principe de base

{% hint style="info" %}
**`const` = constante :** Une fois déclarée, la variable ne peut pas être **réassignée**. Parfait pour les valeurs qui ne changent pas.
{% endhint %}

{% tabs %}
{% tab title="Utilisation correcte" %}
```javascript
// ✅ Déclarations valides avec const
const nom = "Alice";
const age = 25;
const PI = 3.14159;
const estMajeur = true;

// ✅ Objets et tableaux avec const
const personne = {
    nom: "Bob",
    age: 30
};

const fruits = ["pomme", "banane", "orange"];

// ✅ Fonctions avec const
const saluer = function(nom) {
    return `Bonjour ${nom} !`;
};

const calculerAire = (rayon) => PI * rayon * rayon;

console.log(saluer("Alice"));        // "Bonjour Alice !"
console.log(calculerAire(5));        // 78.53975
```
{% endtab %}

{% tab title="Erreurs courantes" %}
```javascript
// ❌ Erreurs avec const

// 1. Réassignation interdite
const nom = "Alice";
// nom = "Bob";  // TypeError: Assignment to constant variable

// 2. Déclaration sans valeur
// const vide;  // SyntaxError: Missing initializer

// 3. Redéclaration interdite
const couleur = "rouge";
// const couleur = "bleu";  // SyntaxError: Identifier 'couleur' has already been declared

// ⚠️ Attention : const n'empêche pas la mutation des objets !
const utilisateur = { nom: "Alice", age: 25 };
utilisateur.age = 26;  // ✅ OK ! On modifie le contenu, pas la référence
utilisateur.ville = "Paris";  // ✅ OK ! Ajout de propriété

console.log(utilisateur);  // { nom: "Alice", age: 26, ville: "Paris" }

// Mais réassigner l'objet entier est interdit :
// utilisateur = { nom: "Bob" };  // ❌ TypeError
```
{% endtab %}

{% tab title="Cas d'usage" %}
```javascript
// 🎯 Utilisez const pour :

// 1. Valeurs numériques fixes
const TAUX_TVA = 0.20;
const MAX_TENTATIVES = 3;
const LARGEUR_ECRAN = 1920;

// 2. Chaînes de caractères fixes
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

// 5. Sélecteurs DOM (une fois trouvés)
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
```javascript
// ✅ Cas d'usage classiques de let

// 1. Compteurs et itérateurs
let compteur = 0;
for (let i = 0; i < 5; i++) {
    compteur += i;
    console.log(`Itération ${i}, compteur: ${compteur}`);
}
console.log(`Compteur final: ${compteur}`); // 10

// 2. Variables conditionnelles
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

// 3. Accumulation de données
let resultat = 0;
const nombres = [1, 2, 3, 4, 5];

for (const nombre of nombres) {
    resultat += nombre;
}

console.log(`Somme: ${resultat}`); // 15

// 4. États temporaires
let estConnecte = false;
let tentatives = 0;

function seConnecter(motDePasse) {
    tentatives++;
    if (motDePasse === "secret123") {
        estConnecte = true;
        console.log("Connexion réussie !");
    } else {
        console.log(`Échec de connexion. Tentative ${tentatives}`);
    }
}
```
{% endtab %}

{% tab title="Portée de bloc" %}
```javascript
// 🎯 let respecte la portée de bloc

function demonstrationPortee() {
    console.log("=== Portée de bloc avec let ===");
    
    // 1. Variable de fonction
    let fonctionVariable = "Accessible dans toute la fonction";
    
    if (true) {
        // 2. Variable de bloc
        let blocVariable = "Accessible seulement dans ce bloc if";
        console.log(fonctionVariable); // ✅ Accessible
        console.log(blocVariable);     // ✅ Accessible
    }
    
    console.log(fonctionVariable); // ✅ Accessible
    // console.log(blocVariable);  // ❌ ReferenceError: blocVariable is not defined
    
    // 3. Boucles avec portée propre
    for (let i = 0; i < 3; i++) {
        let boucleVariable = `Valeur ${i}`;
        console.log(boucleVariable);
    }
    
    // console.log(i);             // ❌ ReferenceError: i is not defined
    // console.log(boucleVariable); // ❌ ReferenceError: boucleVariable is not defined
}

demonstrationPortee();

// 🎯 Comparaison avec des blocs multiples
{
    let bloc1 = "Je suis dans le bloc 1";
    console.log(bloc1); // ✅ OK
}

{
    let bloc2 = "Je suis dans le bloc 2";
    console.log(bloc2); // ✅ OK
    // console.log(bloc1); // ❌ ReferenceError: bloc1 is not defined
}
```
{% endtab %}

{% tab title="Réassignation vs redéclaration" %}
```javascript
// ✅ Réassignation autorisée avec let
let score = 0;
console.log(score); // 0

score = 10;
console.log(score); // 10

score = score + 5;
console.log(score); // 15

score += 10;
console.log(score); // 25

// ❌ Redéclaration interdite dans le même scope
let joueur = "Alice";
// let joueur = "Bob"; // SyntaxError: Identifier 'joueur' has already been declared

// ✅ Mais redéclaration possible dans des scopes différents
function scope1() {
    let variable = "Scope 1";
    console.log(variable);
}

function scope2() {
    let variable = "Scope 2"; // ✅ OK, scope différent
    console.log(variable);
}

scope1(); // "Scope 1"
scope2(); // "Scope 2"

// ✅ Même dans des blocs différents
{
    let temp = "Bloc A";
    console.log(temp);
}

{
    let temp = "Bloc B"; // ✅ OK, bloc différent
    console.log(temp);
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
```javascript
// ⚠️ Problème 1 : Portée de fonction au lieu de bloc

function problemeVar() {
    console.log("=== Problèmes avec var ===");
    
    if (true) {
        var varVariable = "Je 'fuite' du bloc !";
        let letVariable = "Je reste dans le bloc";
    }
    
    console.log(varVariable); // ✅ "Je 'fuite' du bloc !" - PROBLÉMATIQUE !
    // console.log(letVariable); // ❌ ReferenceError - NORMAL
}

// ⚠️ Problème 2 : Boucles for classique
console.log("=== Boucle avec var (problématique) ===");
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log("var i:", i); // Affiche 3, 3, 3 (pas 0, 1, 2 !)
    }, 100);
}

console.log("=== Boucle avec let (correct) ===");
for (let j = 0; j < 3; j++) {
    setTimeout(() => {
        console.log("let j:", j); // Affiche 0, 1, 2 (correct !)
    }, 200);
}

// ⚠️ Problème 3 : Pollution de l'objet global
var variableGlobale = "Je pollue l'objet global";
console.log(window.variableGlobale); // "Je pollue l'objet global" (dans le navigateur)

let variablePropreLet = "Je ne pollue pas";
console.log(window.variablePropreLet); // undefined (bien !)
```
{% endtab %}

{% tab title="Hoisting déroutant" %}
```javascript
// ⚠️ Hoisting avec var : comportement déroutant

function demonstrationHoisting() {
    console.log("=== Hoisting avec var ===");
    
    // Ceci fonctionne mais affiche undefined (déroutant !)
    console.log("Avant déclaration:", maVarVariable); // undefined
    
    var maVarVariable = "Maintenant j'ai une valeur";
    
    console.log("Après déclaration:", maVarVariable); // "Maintenant j'ai une valeur"
    
    // 🎯 Ce qui se passe réellement (équivalent) :
    // var maVarVariable; // Déclaration "remontée" en haut
    // console.log("Avant déclaration:", maVarVariable); // undefined
    // maVarVariable = "Maintenant j'ai une valeur"; // Assignation reste à sa place
}

function demonstrationHoistingLet() {
    console.log("=== Hoisting avec let/const ===");
    
    // Ceci produit une erreur claire (préférable !)
    try {
        console.log("Avant déclaration:", maLetVariable); // ReferenceError
    } catch (error) {
        console.log("Erreur attendue:", error.message);
    }
    
    let maLetVariable = "Je ne suis accessible qu'après ma déclaration";
    console.log("Après déclaration:", maLetVariable);
}

demonstrationHoisting();
demonstrationHoistingLet();
```
{% endtab %}

{% tab title="Redéclaration problématique" %}
```javascript
// ⚠️ var autorise la redéclaration (source d'erreurs)

var utilisateur = "Alice";
console.log("Premier utilisateur:", utilisateur); // "Alice"

// Plus loin dans le code... (accidentellement)
var utilisateur = "Bob"; // ⚠️ Aucune erreur, mais peut être involontaire !
console.log("Utilisateur écrasé:", utilisateur); // "Bob"

// ✅ Avec let/const, l'erreur est immédiate
let produit = "Ordinateur";
try {
    let produit = "Tablette"; // SyntaxError immediate
} catch (error) {
    console.log("Erreur de redéclaration détectée:", error.message);
}

// 🎯 Comparaison dans des fonctions
function avecVar() {
    var resultat = "Premier";
    var resultat = "Deuxième"; // ⚠️ Silencieusement autorisé
    return resultat;
}

function avecLet() {
    let resultat = "Premier";
    try {
        let resultat = "Deuxième"; // ❌ Erreur détectée
    } catch (error) {
        console.log("Redéclaration empêchée avec let");
    }
    return resultat;
}

console.log("Avec var:", avecVar());      // "Deuxième"
console.log("Avec let:", avecLet());      // "Premier"
```
{% endtab %}
{% endtabs %}

## 📏 Portée (Scope) expliquée

### 🎯 Les différents types de portée

{% tabs %}
{% tab title="Portée globale" %}
```javascript
// 🌍 Portée globale : accessible partout

// Variables globales
const SITE_NAME = "Mon Super Site";
let utilisateurConnecte = null;

function afficherSite() {
    console.log(`Bienvenue sur ${SITE_NAME}`); // ✅ Accessible
}

function connecterUtilisateur(nom) {
    utilisateurConnecte = nom; // ✅ Modification possible
    console.log(`${nom} est maintenant connecté`);
}

function obtenirUtilisateur() {
    return utilisateurConnecte; // ✅ Lecture possible
}

// Test
afficherSite();                    // "Bienvenue sur Mon Super Site"
connecterUtilisateur("Alice");     // "Alice est maintenant connecté"
console.log(obtenirUtilisateur()); // "Alice"

// ⚠️ Attention : éviter trop de variables globales
// Bonne pratique : utiliser des modules ou des objets pour organiser
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
