# ⚡ Fonctions en JavaScript

{% hint style="info" %}
**Niveau :** 🟡 Intermédiaire | **Durée :** 120 min | **Prérequis :** Variables, Types, Opérateurs, Structures de contrôle
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** pourquoi et comment utiliser des fonctions
- [ ] **Déclarer** des fonctions avec différentes syntaxes
- [ ] **Utiliser** des fonctions avec des paramètres et retours
- [ ] **Organiser** votre code avec des fonctions réutilisables
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Fonction** : Bloc de code réutilisable qui effectue une tâche spécifique  
**Paramètre** : Variable qu'on donne à la fonction pour qu'elle travaille  
**Argument** : Valeur concrète qu'on passe lors de l'appel  
**Return** : Mot-clé pour renvoyer un résultat de la fonction  
**Scope** : Zone où une variable existe et est accessible  
{% endtab %}

{% tab title="Avantages des fonctions" %}
🔄 **Réutiliser** le code au lieu de le copier-coller  
📦 **Organiser** votre programme en petits blocs logiques  
🐛 **Faciliter** la correction des erreurs (un seul endroit à modifier)  
👥 **Collaborer** plus facilement (chacun programme ses fonctions)  
🧪 **Tester** votre code plus facilement  
{% endtab %}

{% tab title="Types de fonctions" %}
**Fonction déclarée** : `function nomFonction() { }`  
**Fonction exprimée** : `let nomFonction = function() { }`  
**Fonction fléchée** : `let nomFonction = () => { }` (ES6+)  
**Fonction anonyme** : Fonction sans nom (dans un callback)  
**IIFE** : Immediately Invoked Function Expression  
{% endtab %}
{% endtabs %}

## 🧰 Analogie : La boîte à outils

{% hint style="info" %}
**Une fonction, c'est comme un outil dans votre boîte :**

🔨 **Marteau** = Fonction pour "enfoncer" (ajouter des éléments)  
🪚 **Scie** = Fonction pour "découper" (filtrer des données)  
📏 **Mètre** = Fonction pour "mesurer" (calculer des statistiques)  

**Au lieu de refabriquer un marteau à chaque fois, vous le prenez dans votre boîte !**

De même, au lieu de réécrire le même code, vous créez une fonction et vous l'utilisez partout où vous en avez besoin.
{% endhint %}

## 📝 Anatomie d'une fonction

```javascript
function nomFonction(parametre1, parametre2) {
    // Corps de la fonction
    let resultat = parametre1 + parametre2;
    return resultat; // Valeur de retour
}

// Appel de la fonction
let valeur = nomFonction(5, 3); // Arguments
```

{% tabs %}
{% tab title="1. Nom" %}
**Comment on l'appelle**

```javascript
function direBonjour() { } // Nom: direBonjour
function calculerTVA() { } // Nom: calculerTVA
function envoyerEmail() { } // Nom: envoyerEmail
```

**Conventions de nommage :**
- ✅ Utilisez des **verbes d'action** (calculer, afficher, vérifier)
- ✅ **camelCase** pour JavaScript
- ✅ Soyez **descriptifs** mais **concis**
{% endtab %}

{% tab title="2. Paramètres" %}
**Ce qu'on lui donne en entrée (optionnel)**

```javascript
function additionner(a, b) {     // 2 paramètres
function direBonjour(nom) {      // 1 paramètre  
function afficherMenu() {        // Aucun paramètre
```

**Points importants :**
- Les paramètres sont des **variables locales** à la fonction
- **Ordre important** : `additionner(5, 3)` ≠ `additionner(3, 5)`
- Paramètres manquants = `undefined`
{% endtab %}

{% tab title="3. Corps" %}
**Le code qu'elle exécute**

```javascript
function calculerAge(anneeNaissance) {
    // Corps de la fonction
    let anneeActuelle = new Date().getFullYear();
    let age = anneeActuelle - anneeNaissance;
    
    if (age < 0) {
        console.log("Erreur: année future!");
        return null;
    }
    
    return age;
}
```
{% endtab %}

{% tab title="4. Retour" %}
**Ce qu'elle renvoie en sortie (optionnel)**

```javascript
function additionner(a, b) {
    return a + b;        // Retourne un nombre
}

function validerEmail(email) {
    return email.includes("@"); // Retourne un boolean
}

function creerPersonne(nom, age) {
    return {             // Retourne un objet
        nom: nom,
        age: age
    };
}

function afficherMessage() {
    console.log("Hello!");
    // Pas de return = retourne undefined
}
```
{% endtab %}
{% endtabs %}

## 🚀 Exemples progressifs

### 1. 📱 Fonction simple - Menu de restaurant

{% tabs %}
{% tab title="Problème" %}
**Besoin :** Afficher le menu plusieurs fois dans l'application

**Sans fonction (répétitif) :**
```javascript
// Dans l'accueil
console.log("=== MENU DU RESTAURANT ===");
console.log("1. Pizza Margherita - 12€");
console.log("2. Burger Classic - 8€");
console.log("3. Salade César - 10€");

// Dans une autre page... même code !
console.log("=== MENU DU RESTAURANT ===");
console.log("1. Pizza Margherita - 12€");
console.log("2. Burger Classic - 8€");
console.log("3. Salade César - 10€");
```
{% endtab %}

{% tab title="Avec fonction" %}
```javascript
// ✅ Déclaration UNE FOIS
function afficherMenu() {
    console.log("=== MENU DU RESTAURANT ===");
    console.log("1. Pizza Margherita - 12€");
    console.log("2. Burger Classic - 8€");
    console.log("3. Salade César - 10€");
    console.log("========================");
}

// ✅ Utilisation PARTOUT
console.log("🏠 Page d'accueil");
afficherMenu();

console.log("\n📱 Application mobile");
afficherMenu();

console.log("\n📄 Facture");
afficherMenu();
```

**Avantages :**
- ✅ Code plus court et lisible
- ✅ Modification du menu en un seul endroit
- ✅ Moins de risque d'erreur
{% endtab %}
{% endtabs %}

### 2. 🧮 Fonction avec paramètres - Calcul de prix

{% tabs %}
{% tab title="Problème" %}
**Besoin :** Calculer le prix TTC selon différents prix HT

**Solution progressive :**
{% endtab %}

{% tab title="Code complet" %}
```javascript
// Fonction avec UN paramètre
function calculerTTC(prixHT) {
    const tva = prixHT * 0.20;  // 20% de TVA
    const prixTTC = prixHT + tva;
    return prixTTC;
}

// Fonction avec PLUSIEURS paramètres
function calculerTotal(prix, quantite) {
    const sousTotal = prix * quantite;
    const prixFinal = calculerTTC(sousTotal);  // Appel d'une autre fonction !
    return prixFinal;
}

// 📋 Utilisation pratique
console.log("🏪 === CAISSE ENREGISTREUSE ===");

const prixPizza = 12;
const quantitePizza = 2;

console.log(`🍕 ${quantitePizza} pizzas à ${prixPizza}€ l'unité`);

const totalCommande = calculerTotal(prixPizza, quantitePizza);
console.log(`💰 Total TTC: ${totalCommande.toFixed(2)}€`);

// Tests avec d'autres produits
console.log(`🍔 1 burger: ${calculerTTC(8).toFixed(2)}€`);
console.log(`🥗 3 salades: ${calculerTotal(10, 3).toFixed(2)}€`);
```

**Résultat :**
```
🏪 === CAISSE ENREGISTREUSE ===
🍕 2 pizzas à 12€ l'unité
💰 Total TTC: 28.80€
🍔 1 burger: 9.60€
🥗 3 salades: 36.00€
```
{% endtab %}
{% endtabs %}

### 3. 🎓 Fonction complexe - Analyse d'élève

{% tabs %}
{% tab title="Problème" %}
**Besoin :** Analyser les données d'un élève (validation + calculs + affichage)

**Défis :**
- Valider l'email
- Calculer la moyenne des notes
- Déterminer la mention
- Gérer les erreurs
{% endtab %}

{% tab title="Solution modulaire" %}
```javascript
// 📧 Fonction de validation
function estEmailValide(email) {
    const contientArobase = email.includes("@");
    const contientPoint = email.includes(".");
    const assezLong = email.length >= 5;
    
    return contientArobase && contientPoint && assezLong;
}

// 📊 Fonction de calcul
function calculerNoteMoyenne(notes) {
    let total = 0;
    const nombreNotes = notes.length;
    
    // Additionner toutes les notes
    for (let i = 0; i < nombreNotes; i++) {
        total += notes[i];
    }
    
    return total / nombreNotes;
}

// 🏅 Fonction de détermination
function obtenirMention(moyenne) {
    if (moyenne >= 16) return "Très Bien 🏆";
    if (moyenne >= 14) return "Bien 👍";
    if (moyenne >= 12) return "Assez Bien 👌";
    if (moyenne >= 10) return "Passable 😐";
    return "Insuffisant 😞";
}

// 🎯 Fonction principale qui orchestre tout
function analyserEleve(nom, email, listeNotes) {
    console.log("🎓 === ANALYSE DE L'ÉLÈVE ===");
    console.log(`👤 Nom: ${nom}`);
    
    // ✅ Validation email
    if (!estEmailValide(email)) {
        console.log(`📧 Email: ${email} (❌ INVALIDE)`);
        return null; // Sortie anticipée
    }
    console.log(`📧 Email: ${email} (✅ Valide)`);
    
    // 📊 Calculs
    const moyenne = calculerNoteMoyenne(listeNotes);
    const mention = obtenirMention(moyenne);
    
    // 📋 Résultats
    console.log(`📝 Notes: ${listeNotes.join(", ")}`);
    console.log(`📊 Moyenne: ${moyenne.toFixed(2)}/20`);
    console.log(`🏅 Mention: ${mention}`);
    
    // 📦 Retour structuré
    return {
        nom: nom,
        moyenne: moyenne,
        mention: mention,
        success: true
    };
}

// 🧪 Tests
const resultats = analyserEleve(
    "Marie Durand", 
    "marie@lycee.fr", 
    [15, 12, 18, 14, 16]
);

console.log("📋 Objet retourné:", resultats);
```
{% endtab %}

{% tab title="Résultat" %}
```
🎓 === ANALYSE DE L'ÉLÈVE ===
👤 Nom: Marie Durand
📧 Email: marie@lycee.fr (✅ Valide)
📝 Notes: 15, 12, 18, 14, 16
📊 Moyenne: 15.00/20
🏅 Mention: Bien 👍
📋 Objet retourné: {
  nom: "Marie Durand",
  moyenne: 15,
  mention: "Bien 👍",
  success: true
}
```

**Concepts illustrés :**
- ✅ **Modularité** : Chaque fonction a une responsabilité
- ✅ **Réutilisabilité** : `estEmailValide()` peut servir ailleurs
- ✅ **Composition** : Une fonction qui utilise d'autres fonctions
- ✅ **Gestion d'erreurs** : Sortie anticipée si email invalide
{% endtab %}
{% endtabs %}

## 💻 Exercices pratiques

### Exercice 1 : Calculatrice modulaire

{% hint style="info" %}
**Créez des fonctions pour chaque opération mathématique.**

**Fonctions à créer :**
- `additionner(a, b)` - retourne a + b
- `soustraire(a, b)` - retourne a - b  
- `multiplier(a, b)` - retourne a * b
- `diviser(a, b)` - retourne a / b (attention division par zéro !)
- `calculer(operation, a, b)` - fonction principale qui appelle les autres
{% endhint %}

```javascript
// À vous de jouer !
function additionner(a, b) {
    // TODO
}

function soustraire(a, b) {
    // TODO
}

// ... autres fonctions

// Tests
console.log(calculer("+", 10, 5));  // Devrait afficher 15
console.log(calculer("/", 10, 0));  // Devrait gérer l'erreur
```

<details>
<summary>💡 Solution complète</summary>

```javascript
// 🧮 Fonctions mathématiques de base
function additionner(a, b) {
    return a + b;
}

function soustraire(a, b) {
    return a - b;
}

function multiplier(a, b) {
    return a * b;
}

function diviser(a, b) {
    if (b === 0) {
        console.log("❌ Erreur: Division par zéro impossible !");
        return null;
    }
    return a / b;
}

// 🎯 Fonction principale (orchestrateur)
function calculer(operation, a, b) {
    console.log(`🔢 Calcul: ${a} ${operation} ${b}`);
    
    let resultat;
    
    switch (operation) {
        case "+":
            resultat = additionner(a, b);
            break;
        case "-":
            resultat = soustraire(a, b);
            break;
        case "*":
            resultat = multiplier(a, b);
            break;
        case "/":
            resultat = diviser(a, b);
            break;
        default:
            console.log("❌ Opération inconnue !");
            return null;
    }
    
    if (resultat !== null) {
        console.log(`✅ Résultat: ${resultat}`);
    }
    
    return resultat;
}

// 🧪 Tests de la calculatrice
console.log("🧮 === CALCULATRICE MODULAIRE ===");
calculer("+", 10, 5);     // 15
calculer("-", 10, 5);     // 5
calculer("*", 10, 5);     // 50
calculer("/", 10, 5);     // 2
calculer("/", 10, 0);     // Erreur
calculer("%", 10, 5);     // Opération inconnue

// 🚀 Version améliorée avec objet retourné
function calculerAvance(operation, a, b) {
    const resultat = calculer(operation, a, b);
    
    return {
        operation: operation,
        operande1: a,
        operande2: b,
        resultat: resultat,
        valide: (resultat !== null)
    };
}

const test = calculerAvance("*", 7, 8);
console.log("📊 Test avancé:", test);
```

**Concepts maîtrisés :**
- ✅ Fonctions spécialisées avec responsabilités claires
- ✅ Gestion d'erreurs avec valeurs de retour spéciales
- ✅ Fonction orchestratrice qui coordonne les autres
- ✅ Validation des entrées et feedback utilisateur

</details>

### Exercice 2 : Validateur de formulaire

{% hint style="info" %}
**Créez des fonctions pour valider différents champs d'un formulaire.**

**Fonctions à créer :**
- `validerNom(nom)` - au moins 2 caractères, que des lettres
- `validerAge(age)` - entre 13 et 99 ans
- `validerEmail(email)` - format valide
- `validerFormulaire(nom, age, email)` - valide tout et retourne le résultat
{% endhint %}

<details>
<summary>🚀 Code de départ</summary>

```javascript
function validerNom(nom) {
    // TODO: 
    // - Vérifier longueur >= 2
    // - Vérifier que des lettres (et espaces/tirets)
    // - Retourner objet {valide: boolean, erreur: string}
}

function validerAge(age) {
    // TODO:
    // - Vérifier que c'est un nombre
    // - Vérifier intervalle 13-99
    // - Retourner objet {valide: boolean, erreur: string}
}

function validerEmail(email) {
    // TODO:
    // - Vérifier présence @ et .
    // - Vérifier longueur minimum
    // - Retourner objet {valide: boolean, erreur: string}
}

function validerFormulaire(nom, age, email) {
    // TODO:
    // - Appeler toutes les fonctions de validation
    // - Compiler les résultats
    // - Afficher les erreurs
    // - Retourner le résultat global
}

// Tests
validerFormulaire("Alice Dupont", 16, "alice@lycee.fr");
validerFormulaire("A", 150, "email-invalide");
```

</details>

<details>
<summary>💡 Solution complète</summary>

```javascript
// 👤 Validation du nom
function validerNom(nom) {
    if (nom.length < 2) {
        return {
            valide: false,
            erreur: "Le nom doit avoir au moins 2 caractères"
        };
    }
    
    // Vérification caractères (lettres, espaces, tirets)
    const formatValide = /^[a-zA-ZÀ-ÿ\s-]+$/.test(nom);
    if (!formatValide) {
        return {
            valide: false,
            erreur: "Le nom ne peut contenir que des lettres, espaces et tirets"
        };
    }
    
    return { valide: true, erreur: null };
}

// 🎂 Validation de l'âge
function validerAge(age) {
    if (typeof age !== "number" || isNaN(age)) {
        return {
            valide: false,
            erreur: "L'âge doit être un nombre"
        };
    }
    
    if (age < 13 || age > 99) {
        return {
            valide: false,
            erreur: "L'âge doit être entre 13 et 99 ans"
        };
    }
    
    return { valide: true, erreur: null };
}

// 📧 Validation de l'email
function validerEmail(email) {
    if (!email.includes("@")) {
        return {
            valide: false,
            erreur: "L'email doit contenir le caractère @"
        };
    }
    
    if (!email.includes(".")) {
        return {
            valide: false,
            erreur: "L'email doit contenir au moins un point"
        };
    }
    
    if (email.length < 5) {
        return {
            valide: false,
            erreur: "L'email est trop court (minimum 5 caractères)"
        };
    }
    
    // Vérification format basique
    const parties = email.split("@");
    if (parties.length !== 2 || parties[0].length === 0 || parties[1].length === 0) {
        return {
            valide: false,
            erreur: "Format d'email invalide"
        };
    }
    
    return { valide: true, erreur: null };
}

// 🎯 Fonction principale de validation
function validerFormulaire(nom, age, email) {
    console.log("📋 === VALIDATION FORMULAIRE ===");
    console.log(`👤 Nom: '${nom}'`);
    console.log(`🎂 Âge: ${age}`);
    console.log(`📧 Email: '${email}'`);
    
    // Appel de toutes les validations
    const resultats = {
        nom: validerNom(nom),
        age: validerAge(age),
        email: validerEmail(email)
    };
    
    // Vérification globale
    const toutValide = resultats.nom.valide && 
                      resultats.age.valide && 
                      resultats.email.valide;
    
    console.log("\n🎯 === RÉSULTATS ===");
    
    // Affichage des résultats
    if (resultats.nom.valide) {
        console.log("👤 Nom: ✅ VALIDE");
    } else {
        console.log(`👤 Nom: ❌ ${resultats.nom.erreur}`);
    }
    
    if (resultats.age.valide) {
        console.log("🎂 Âge: ✅ VALIDE");
    } else {
        console.log(`🎂 Âge: ❌ ${resultats.age.erreur}`);
    }
    
    if (resultats.email.valide) {
        console.log("📧 Email: ✅ VALIDE");
    } else {
        console.log(`📧 Email: ❌ ${resultats.email.erreur}`);
    }
    
    console.log(`\n📋 Formulaire: ${toutValide ? "✅ VALIDE" : "❌ INVALIDE"}`);
    
    return {
        valide: toutValide,
        details: resultats
    };
}

// 🧪 Tests du validateur
validerFormulaire("Alice Dupont", 16, "alice@lycee.fr");
console.log("\n" + "=".repeat(50) + "\n");
validerFormulaire("A", 150, "email-invalide");
```

**Concepts avancés :**
- ✅ Retour d'objets structurés pour les résultats
- ✅ Validation en cascade avec gestion d'erreurs détaillée
- ✅ Expressions régulières pour valider les formats
- ✅ Séparation claire des responsabilités

</details>

### Exercice 3 : Jeu de dés

{% hint style="info" %}
**Créez un système de jeu complet avec plusieurs fonctions.**

**Fonctions à créer :**
- `lancerDe()` - retourne un nombre entre 1 et 6
- `jouerTour(joueur)` - lance 2 dés pour un joueur
- `determinerGagnant(score1, score2)` - compare et retourne le gagnant
- `jouerPartie(nom1, nom2)` - fonction complète du jeu
{% endhint %}

<details>
<summary>💡 Solution complète</summary>

```javascript
// 🎲 Fonction pour lancer un dé
function lancerDe() {
    return Math.floor(Math.random() * 6) + 1;
}

// 🎯 Fonction pour jouer un tour (2 dés)
function jouerTour(nomJoueur) {
    const de1 = lancerDe();
    const de2 = lancerDe();
    let total = de1 + de2;
    
    console.log(`🎲 ${nomJoueur} lance les dés:`);
    console.log(`   🎲 Dé 1: ${de1}`);
    console.log(`   🎲 Dé 2: ${de2}`);
    console.log(`   📊 Total: ${total}`);
    
    // Bonus pour double !
    if (de1 === de2) {
        console.log(`   🎊 DOUBLE ! Bonus de 5 points !`);
        total += 5;
        console.log(`   🚀 Nouveau total: ${total}`);
    }
    
    return total;
}

// 🏆 Fonction pour déterminer le gagnant
function determinerGagnant(score1, nom1, score2, nom2) {
    console.log("\n🏁 === RÉSULTATS FINAUX ===");
    console.log(`🎯 ${nom1}: ${score1} points`);
    console.log(`🎯 ${nom2}: ${score2} points`);
    
    if (score1 > score2) {
        console.log(`🏆 GAGNANT: ${nom1} !`);
        return nom1;
    } else if (score2 > score1) {
        console.log(`🏆 GAGNANT: ${nom2} !`);
        return nom2;
    } else {
        console.log("🤝 ÉGALITÉ ! Match nul !");
        return "Égalité";
    }
}

// 🎮 Fonction principale du jeu
function jouerPartie(nom1, nom2, nombreTours = 3) {
    console.log("🎮 === JEU DE DÉS ===");
    console.log(`👥 Joueurs: ${nom1} vs ${nom2}`);
    console.log(`🔢 Nombre de tours: ${nombreTours}`);
    console.log("📋 Règle: 2 dés par tour, bonus +5 pour les doubles\n");
    
    let scoreTotal1 = 0;
    let scoreTotal2 = 0;
    
    // Boucle pour chaque tour
    for (let tour = 1; tour <= nombreTours; tour++) {
        console.log(`🔥 --- TOUR ${tour} ---`);
        
        const scoreTour1 = jouerTour(nom1);
        const scoreTour2 = jouerTour(nom2);
        
        scoreTotal1 += scoreTour1;
        scoreTotal2 += scoreTour2;
        
        console.log(`📊 Scores cumulés: ${nom1} = ${scoreTotal1}, ${nom2} = ${scoreTotal2}\n`);
    }
    
    const gagnant = determinerGagnant(scoreTotal1, nom1, scoreTotal2, nom2);
    
    // Retour structuré avec tous les résultats
    return {
        joueur1: { nom: nom1, score: scoreTotal1 },
        joueur2: { nom: nom2, score: scoreTotal2 },
        gagnant: gagnant,
        nombreTours: nombreTours
    };
}

// 🏟️ Fonction bonus: tournoi
function jouerTournoi(listeJoueurs) {
    console.log("🏟️ === TOURNOI DE DÉS ===");
    const resultats = [];
    
    // Jouer tous contre tous
    for (let i = 0; i < listeJoueurs.length; i++) {
        for (let j = i + 1; j < listeJoueurs.length; j++) {
            console.log("\n" + "=".repeat(50));
            const match = jouerPartie(listeJoueurs[i], listeJoueurs[j], 2);
            resultats.push(match);
        }
    }
    
    return resultats;
}

// 🧪 Tests du jeu
const partie1 = jouerPartie("Alice", "Bob", 2);
console.log("\n📋 Résultats complets:", partie1);

// Test tournoi (décommentez pour essayer)
// const joueurs = ["Alice", "Bob", "Charlie"];
// jouerTournoi(joueurs);
```

**Concepts maîtrisés :**
- ✅ Génération de nombres aléatoires
- ✅ Logique de jeu avec bonus et conditions
- ✅ Boucles et accumulation de scores
- ✅ Retour de structures de données complexes
- ✅ Extension vers des fonctionnalités avancées (tournoi)

</details>

## 🎮 Quiz interactif

{% hint style="info" %}
**Question 1 :** Quelle est la différence entre paramètre et argument ?
A) Aucune différence  B) Paramètre dans la déclaration, argument dans l'appel  C) L'inverse
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) Paramètre dans la déclaration, argument dans l'appel**

```javascript
function saluer(nom) { // "nom" est un PARAMÈTRE
    console.log("Bonjour " + nom);
}

saluer("Alice"); // "Alice" est un ARGUMENT
```

</details>

{% hint style="info" %}
**Question 2 :** Que se passe-t-il si on appelle une fonction avec moins d'arguments que de paramètres ?
A) Erreur  B) Les paramètres manquants valent `undefined`  C) Les paramètres manquants valent `null`
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) Les paramètres manquants valent `undefined`**

```javascript
function additionner(a, b) {
    console.log("a =", a, "b =", b);
    return a + b;
}

additionner(5); 
// Affiche: a = 5 b = undefined
// Retourne: NaN (5 + undefined)
```

</details>

## ⚠️ Pièges courants à éviter

### 1. 🪤 Oublier le `return`

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
function calculer(a, b) {
    let resultat = a + b;
    // Oubli du return !
}

let total = calculer(5, 3);
console.log(total); // undefined 😞
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
function calculer(a, b) {
    let resultat = a + b;
    return resultat; // N'oubliez jamais le return !
}

let total = calculer(5, 3);
console.log(total); // 8 ✅
```
{% endtab %}
{% endtabs %}

### 2. 🪤 Problème de scope (portée)

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
function direBonjour() {
    let nom = "Alice";
}

console.log(nom); // ❌ Erreur ! nom n'existe pas ici
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
function direBonjour() {
    let nom = "Alice";
    return nom; // Renvoyer la valeur
}

let nomRetourne = direBonjour();
console.log(nomRetourne); // "Alice" ✅

// Ou déclarer la variable à l'extérieur
let nom = "Alice";
function direBonjour() {
    console.log(nom); // Accès à la variable externe
}
```
{% endtab %}
{% endtabs %}

### 3. 🪤 Arguments manquants

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
function multiplier(a, b) {
    return a * b;
}

let resultat = multiplier(5); // Un seul argument !
console.log(resultat); // NaN (5 * undefined)
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Option 1: Valeurs par défaut (ES6)
function multiplier(a, b = 1) {
    return a * b;
}

// Option 2: Vérification manuelle
function multiplierSecurise(a, b) {
    if (b === undefined) {
        b = 1; // Valeur par défaut
    }
    return a * b;
}

// Option 3: Validation stricte
function multiplierStrict(a, b) {
    if (a === undefined || b === undefined) {
        throw new Error("Les deux paramètres sont obligatoires");
    }
    return a * b;
}

let resultat = multiplier(5); // 5 (5 * 1) ✅
```
{% endtab %}
{% endtabs %}

## 🔗 Liens avec d'autres concepts

### Concepts utilisés
{% content-ref url="../niveau-debutant/variables-declarations.md" %}
[variables-declarations.md](../niveau-debutant/variables-declarations.md)
{% endcontent-ref %}

{% content-ref url="../niveau-debutant/structures-controle.md" %}
[structures-controle.md](../niveau-debutant/structures-controle.md)
{% endcontent-ref %}

{% content-ref url="../niveau-debutant/premiers-algorithmes-simples.md" %}
[premiers-algorithmes-simples.md](../niveau-debutant/premiers-algorithmes-simples.md)
{% endcontent-ref %}

### Prochaines étapes
{% content-ref url="fonctions-avancees.md" %}
[fonctions-avancees.md](fonctions-avancees.md)
{% endcontent-ref %}

{% content-ref url="closures-lexical-scope.md" %}
[closures-lexical-scope.md](closures-lexical-scope.md)
{% endcontent-ref %}

{% content-ref url="arrays.md" %}
[arrays.md](arrays.md)
{% endcontent-ref %}

## 🎯 Résumé

{% hint style="success" %}
**Points clés à retenir :**

⚡ **Les fonctions = outils réutilisables** pour organiser votre code  
📝 **Syntaxe** : `function nom(paramètres) { corps; return résultat; }`  
🔄 **Paramètres vs Arguments** : Variables dans la déclaration vs valeurs dans l'appel  
↩️ **Return obligatoire** pour récupérer un résultat  
🔒 **Scope** : Variables dans une fonction ne sont visibles que dedans  
🧩 **Modularité** : Diviser le problème en petites fonctions spécialisées  
{% endhint %}

## 🚀 Prochaines étapes

{% hint style="info" %}
**Félicitations ! Vous maîtrisez les fonctions de base.**

👉 **Découvrez** les [Fonctions Avancées](fonctions-avancees.md) (arrow functions, callbacks, etc.)  
👉 **Apprenez** les [Closures et Lexical Scope](closures-lexical-scope.md)  
👉 **Explorez** les [Arrays et leurs méthodes](arrays.md) qui utilisent massivement les fonctions  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour les fonctions avancées ?** Passez aux [Fonctions Avancées](fonctions-avancees.md) pour découvrir les arrow functions, callbacks et plus encore !
{% endhint %}
