# 🎯 Structures de Contrôle en JavaScript

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 100 min | **Prérequis :** Variables, Types de données, Opérateurs logiques
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** comment prendre des décisions dans votre code
- [ ] **Appliquer** if/else pour des conditions simples et complexes
- [ ] **Analyser** quand choisir entre if/else et switch
- [ ] **Créer** des boucles pour répéter des actions efficacement
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Instructions conditionnelles" %}
**if/else** : Si... sinon (prendre une décision)  
**else if** : Chaîner plusieurs conditions  
**switch** : Aiguillage entre plusieurs choix exacts  
**Opérateur ternaire** : Condition en une ligne  
{% endtab %}

{% tab title="Boucles (loops)" %}
**for** : Répéter un nombre fixe de fois  
**while** : Répéter tant qu'une condition est vraie  
**do...while** : Faire au moins une fois, puis répéter  
**for...of** : Parcourir les éléments d'un tableau  
{% endtab %}

{% tab title="Contrôle de flux" %}
**break** : Sortir d'une boucle ou switch  
**continue** : Passer au tour suivant  
**return** : Sortir d'une fonction  
{% endtab %}
{% endtabs %}

## 💡 Pourquoi ce concept est-il important ?

{% hint style="warning" %}
**Dans la vraie vie, vous prenez constamment des décisions :**

🌧️ **"S'il pleut, je prends un parapluie, sinon je sors sans"**  
📚 **"Je révise mes maths jusqu'à ce que je comprenne"**  
🚦 **"Si le feu est rouge, je m'arrête"**

En programmation, c'est pareil ! Votre programme doit pouvoir :
- Prendre des décisions selon les situations
- Répéter des actions sans réécrire le code 100 fois  
- S'adapter selon les données qu'il reçoit
{% endhint %}

## 🔀 Instructions conditionnelles

### 1. if/else - La décision de base

{% tabs %}
{% tab title="Syntaxe simple" %}
```javascript
// Structure de base
if (condition) {
    // Code exécuté si la condition est vraie
} else {
    // Code exécuté si la condition est fausse
}

// Exemple concret
const age = 16;

if (age >= 18) {
    console.log("✅ Tu peux voter !");
} else {
    console.log("❌ Trop jeune pour voter");
}
```
{% endtab %}

{% tab title="Conditions multiples" %}
```javascript
// Structure avec else if
const note = 15;

if (note >= 16) {
    console.log("🏆 Excellente note !");
} else if (note >= 14) {
    console.log("😊 Bonne note !");
} else if (note >= 12) {
    console.log("🙂 Note correcte");
} else if (note >= 10) {
    console.log("😐 Passable");
} else {
    console.log("😟 Il faut travailler plus");
}

// Résultat : "😊 Bonne note !"
```
{% endtab %}

{% tab title="Conditions complexes" %}
```javascript
// Combiner plusieurs critères
const age = 17;
const aPermisParents = true;
const estAccompagne = false;

if (age >= 18) {
    console.log("✅ Accès libre");
} else if (age >= 16 && aPermisParents) {
    console.log("✅ Accès avec autorisation parentale");
} else if (age >= 13 && estAccompagne) {
    console.log("✅ Accès avec accompagnateur");
} else {
    console.log("❌ Accès refusé");
}

// Résultat : "✅ Accès avec autorisation parentale"
```
{% endtab %}
{% endtabs %}

### 2. switch - L'aiguillage multiple

{% hint style="info" %}
**Utilisez switch quand :** Vous devez comparer une variable à plusieurs valeurs exactes
{% endhint %}

{% tabs %}
{% tab title="Syntaxe de base" %}
```javascript
const jour = "lundi";

switch (jour) {
    case "lundi":
        console.log("😫 Début de semaine...");
        break;
    case "mardi":
        console.log("😐 Mardi, ça va");
        break;
    case "mercredi":
        console.log("🙂 Milieu de semaine");
        break;
    case "jeudi":
        console.log("😊 Presque vendredi !");
        break;
    case "vendredi":
        console.log("🎉 TGIF !");
        break;
    case "samedi":
    case "dimanche":
        console.log("😴 Weekend !");
        break;
    default:
        console.log("🤔 Jour inconnu");
        break;
}
```

**💡 Points importants :**
- `break` empêche l'exécution des cases suivants
- `default` gère tous les cas non prévus
- On peut grouper plusieurs `case` (samedi/dimanche)
{% endtab %}

{% tab title="Calculatrice avec switch" %}
```javascript
function calculer(nombre1, operateur, nombre2) {
    let resultat;
    
    switch (operateur) {
        case "+":
            resultat = nombre1 + nombre2;
            break;
        case "-":
            resultat = nombre1 - nombre2;
            break;
        case "*":
            resultat = nombre1 * nombre2;
            break;
        case "/":
            if (nombre2 !== 0) {
                resultat = nombre1 / nombre2;
            } else {
                return "❌ Division par zéro impossible !";
            }
            break;
        case "%":
            resultat = nombre1 % nombre2;
            break;
        default:
            return `❌ Opérateur "${operateur}" non supporté`;
    }
    
    return `${nombre1} ${operateur} ${nombre2} = ${resultat}`;
}

// Tests
console.log(calculer(10, "+", 3));  // "10 + 3 = 13"
console.log(calculer(10, "/", 0));  // "❌ Division par zéro impossible !"
console.log(calculer(10, "?", 3));  // "❌ Opérateur "?" non supporté"
```
{% endtab %}

{% tab title="if vs switch - Quand utiliser quoi ?" %}
| Critère | if/else | switch |
|---------|---------|--------|
| **Type de comparaison** | Conditions complexes (`>`, `<`, `&&`, `||`) | Valeurs exactes (`===`) |
| **Nombre de conditions** | Peu ou beaucoup | Beaucoup (>3) |
| **Lisibilité** | Bonne pour logique complexe | Excellente pour choix multiples |
| **Performance** | Correcte | Meilleure pour beaucoup de cas |

**Exemples d'usage :**
```javascript
// ✅ Utilisez if/else pour :
if (age >= 18 && aPermis && voitureDisponible) {
    conduire();
}

// ✅ Utilisez switch pour :
switch (codeErreur) {
    case 404: return "Page non trouvée";
    case 500: return "Erreur serveur";
    case 403: return "Accès interdit";
    // ...
}
```
{% endtab %}
{% endtabs %}

## 🔄 Les boucles

### 1. Boucle for - Répétition contrôlée

{% hint style="info" %}
**Utilisez for quand :** Vous savez combien de fois répéter l'action
{% endhint %}

{% tabs %}
{% tab title="Syntaxe et bases" %}
```javascript
// Structure : for (initialisation; condition; incrémentation)
for (let i = 1; i <= 5; i++) {
    console.log(`Tour numéro ${i}`);
}

// Résultat :
// Tour numéro 1
// Tour numéro 2  
// Tour numéro 3
// Tour numéro 4
// Tour numéro 5

// Compter à rebours
for (let i = 5; i >= 1; i--) {
    console.log(`${i}...`);
}
console.log("🚀 Décollage !");
```
{% endtab %}

{% tab title="Exemples pratiques" %}
```javascript
// Table de multiplication
function tableMultiplication(nombre) {
    console.log(`=== Table de ${nombre} ===`);
    
    for (let i = 1; i <= 10; i++) {
        const resultat = nombre * i;
        console.log(`${nombre} × ${i} = ${resultat}`);
    }
}

tableMultiplication(7);

// Calculer une somme
let total = 0;
for (let i = 1; i <= 100; i++) {
    total += i;
}
console.log(`Somme de 1 à 100 : ${total}`); // 5050

// Chercher dans un tableau
const prenoms = ["Alice", "Bob", "Charlie", "Diana"];
const cherche = "Charlie";

for (let i = 0; i < prenoms.length; i++) {
    if (prenoms[i] === cherche) {
        console.log(`✅ ${cherche} trouvé à l'index ${i}`);
        break; // Sortir de la boucle
    }
}
```
{% endtab %}

{% tab title="Variations avancées" %}
```javascript
// Parcourir par pas de 2
for (let i = 0; i <= 20; i += 2) {
    console.log(`Nombre pair : ${i}`);
}

// Boucles imbriquées (tableau 2D)
for (let ligne = 1; ligne <= 3; ligne++) {
    for (let colonne = 1; colonne <= 3; colonne++) {
        console.log(`Case [${ligne},${colonne}]`);
    }
}

// for...of pour parcourir un tableau (moderne)
const fruits = ["pomme", "banane", "orange"];

for (const fruit of fruits) {
    console.log(`J'aime les ${fruit}s`);
}

// for...in pour parcourir un objet
const personne = { nom: "Alice", age: 25, ville: "Paris" };

for (const propriete in personne) {
    console.log(`${propriete}: ${personne[propriete]}`);
}
```
{% endtab %}
{% endtabs %}

### 2. Boucle while - Répétition conditionnelle

{% hint style="info" %}
**Utilisez while quand :** Vous ne savez pas combien de fois répéter, mais avez une condition d'arrêt
{% endhint %}

{% tabs %}
{% tab title="Syntaxe de base" %}
```javascript
// Structure : while (condition)
let compteur = 1;

while (compteur <= 5) {
    console.log(`Compteur : ${compteur}`);
    compteur++; // Important : modifier la variable !
}

// ⚠️ DANGER : Boucle infinie si on oublie l'incrémentation !
// while (compteur <= 5) {
//     console.log(compteur); // compteur ne change jamais = infini
// }
```
{% endtab %}

{% tab title="Exemples pratiques" %}
```javascript
// Jeu de devinette
function jeuDevinette() {
    const nombreSecret = Math.floor(Math.random() * 10) + 1;
    let tentative = 0;
    let trouve = false;
    
    console.log("🎯 Devine un nombre entre 1 et 10 !");
    
    while (!trouve && tentative < 3) {
        tentative++;
        // En vrai, on utiliserait prompt()
        const guess = Math.floor(Math.random() * 10) + 1;
        
        console.log(`Tentative ${tentative}: ${guess}`);
        
        if (guess === nombreSecret) {
            console.log("🎉 Bravo ! Vous avez trouvé !");
            trouve = true;
        } else if (guess < nombreSecret) {
            console.log("📈 Plus grand !");
        } else {
            console.log("📉 Plus petit !");
        }
    }
    
    if (!trouve) {
        console.log(`😞 Perdu ! C'était ${nombreSecret}`);
    }
}

// Validation d'entrée utilisateur
function demanderAge() {
    let age;
    let valide = false;
    
    while (!valide) {
        // age = prompt("Quel est votre âge ?");
        age = "25"; // Simulation
        
        if (age && !isNaN(age) && age >= 0 && age <= 120) {
            valide = true;
        } else {
            console.log("❌ Âge invalide, réessayez !");
            break; // Simulation - en vrai on rebouclerait
        }
    }
    
    return parseInt(age);
}
```
{% endtab %}

{% tab title="do...while - Au moins une fois" %}
```javascript
// do...while : exécute AU MOINS une fois
let reponse;

do {
    console.log("🎮 Bienvenue dans le jeu !");
    // reponse = prompt("Voulez-vous jouer ? (oui/non)");
    reponse = "non"; // Simulation
    
    if (reponse === "oui") {
        console.log("🎯 Lancement du jeu...");
        // logique du jeu
    }
    
} while (reponse === "oui");

console.log("👋 À bientôt !");

// Différence avec while normal :
// - do...while : demande au moins une fois
// - while : peut ne jamais demander si condition false dès le début
```
{% endtab %}
{% endtabs %}

## 🛑 Contrôle de flux avancé

### break et continue

{% tabs %}
{% tab title="break - Sortir de la boucle" %}
```javascript
// break : arrête complètement la boucle
console.log("=== Recherche du premier nombre pair ===");

for (let i = 1; i <= 10; i++) {
    console.log(`Test : ${i}`);
    
    if (i % 2 === 0) {
        console.log(`✅ Premier pair trouvé : ${i}`);
        break; // Sort de la boucle
    }
}
console.log("🏁 Boucle terminée");

// Utilisation dans switch (vu précédemment)
switch (choix) {
    case "a":
        console.log("Option A");
        break; // Empêche l'exécution des cases suivants
    case "b":
        console.log("Option B");
        break;
}
```
{% endtab %}

{% tab title="continue - Passer au tour suivant" %}
```javascript
// continue : passe au tour suivant sans exécuter le reste
console.log("=== Afficher seulement les nombres impairs ===");

for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        continue; // Passe au tour suivant pour les pairs
    }
    
    console.log(`Nombre impair : ${i}`);
}

// Résultat : 1, 3, 5, 7, 9

// Filtrer des données
const notes = [12, 8, 16, 3, 14, 9, 18];
let sommeReussites = 0;
let nbReussites = 0;

for (const note of notes) {
    if (note < 10) {
        continue; // Ignorer les échecs
    }
    
    sommeReussites += note;
    nbReussites++;
}

const moyenne = sommeReussites / nbReussites;
console.log(`Moyenne des réussites : ${moyenne.toFixed(2)}`);
```
{% endtab %}

{% tab title="Boucles imbriquées" %}
```javascript
// break et continue dans boucles imbriquées
console.log("=== Recherche dans une grille ===");

const grille = [
    [1, 2, 3],
    [4, 5, 6], 
    [7, 8, 9]
];

const cherche = 5;
let trouve = false;

// Labels pour contrôler les boucles imbriquées
boucleExterne: for (let ligne = 0; ligne < grille.length; ligne++) {
    for (let colonne = 0; colonne < grille[ligne].length; colonne++) {
        console.log(`Vérification [${ligne}][${colonne}] = ${grille[ligne][colonne]}`);
        
        if (grille[ligne][colonne] === cherche) {
            console.log(`✅ ${cherche} trouvé en [${ligne}][${colonne}]`);
            trouve = true;
            break boucleExterne; // Sort des DEUX boucles
        }
    }
}

if (!trouve) {
    console.log(`❌ ${cherche} non trouvé`);
}
```
{% endtab %}
{% endtabs %}

## 💻 Exemples pratiques complexes

### Système de gestion de notes

{% tabs %}
{% tab title="Code complet" %}
```javascript
function analyserNotes(notes) {
    let somme = 0;
    let nbNotes = 0;
    let nbReussites = 0;
    let nbEchecs = 0;
    let noteMax = -Infinity;
    let noteMin = Infinity;
    
    console.log("=== ANALYSE DES NOTES ===");
    
    for (let i = 0; i < notes.length; i++) {
        const note = notes[i];
        
        // Validation de la note
        if (note < 0 || note > 20) {
            console.log(`⚠️ Note invalide ignorée : ${note}`);
            continue;
        }
        
        // Statistiques
        somme += note;
        nbNotes++;
        
        if (note >= 10) {
            nbReussites++;
        } else {
            nbEchecs++;
        }
        
        if (note > noteMax) noteMax = note;
        if (note < noteMin) noteMin = note;
        
        // Mention
        let mention;
        if (note >= 16) {
            mention = "Très bien";
        } else if (note >= 14) {
            mention = "Bien";
        } else if (note >= 12) {
            mention = "Assez bien";
        } else if (note >= 10) {
            mention = "Passable";
        } else {
            mention = "Insuffisant";
        }
        
        console.log(`Note ${i + 1}: ${note}/20 - ${mention}`);
    }
    
    // Résultats finaux
    if (nbNotes === 0) {
        console.log("❌ Aucune note valide à analyser");
        return;
    }
    
    const moyenne = somme / nbNotes;
    const tauxReussite = (nbReussites / nbNotes) * 100;
    
    console.log("\n=== STATISTIQUES ===");
    console.log(`📊 Nombre de notes : ${nbNotes}`);
    console.log(`📈 Moyenne : ${moyenne.toFixed(2)}/20`);
    console.log(`🏆 Note max : ${noteMax}/20`);
    console.log(`📉 Note min : ${noteMin}/20`);
    console.log(`✅ Réussites : ${nbReussites} (${tauxReussite.toFixed(1)}%)`);
    console.log(`❌ Échecs : ${nbEchecs}`);
    
    // Appréciation globale
    let appreciation;
    if (moyenne >= 16) {
        appreciation = "🏆 Excellente classe !";
    } else if (moyenne >= 14) {
        appreciation = "😊 Bonne classe !";
    } else if (moyenne >= 12) {
        appreciation = "🙂 Classe correcte";
    } else if (moyenne >= 10) {
        appreciation = "😐 Classe en difficulté";
    } else {
        appreciation = "😟 Classe en grande difficulté";
    }
    
    console.log(`\n${appreciation}`);
}

// Test avec des données
const notesClasse = [15, 12, 18, 8, 16, 25, 14, 6, 17, 11, -2, 13];
analyserNotes(notesClasse);
```
{% endtab %}

{% tab title="Résultat" %}
```
=== ANALYSE DES NOTES ===
⚠️ Note invalide ignorée : 25
⚠️ Note invalide ignorée : -2
Note 1: 15/20 - Bien
Note 2: 12/20 - Assez bien  
Note 3: 18/20 - Très bien
Note 4: 8/20 - Insuffisant
Note 5: 16/20 - Très bien
Note 6: 14/20 - Bien
Note 7: 6/20 - Insuffisant
Note 8: 17/20 - Très bien
Note 9: 11/20 - Passable
Note 10: 13/20 - Assez bien

=== STATISTIQUES ===
📊 Nombre de notes : 10
📈 Moyenne : 13.00/20  
🏆 Note max : 18/20
📉 Note min : 6/20
✅ Réussites : 8 (80.0%)
❌ Échecs : 2

🙂 Classe correcte
```
{% endtab %}
{% endtabs %}

### Jeu Pierre-Feuille-Ciseaux

{% tabs %}
{% tab title="Version complète" %}
```javascript
function jouerPierreFeuillesCiseaux() {
    const choix = ["pierre", "feuille", "ciseaux"];
    let scoreJoueur = 0;
    let scoreOrdinateur = 0;
    let manche = 1;
    const manchesMax = 3;
    
    console.log("🎮 === PIERRE FEUILLE CISEAUX ===");
    console.log(`Premier à ${Math.ceil(manchesMax / 2)} victoires gagne !`);
    
    while (scoreJoueur < Math.ceil(manchesMax / 2) && 
           scoreOrdinateur < Math.ceil(manchesMax / 2)) {
        
        console.log(`\n🎯 === MANCHE ${manche} ===`);
        
        // Choix du joueur (simulation)
        const choixJoueur = choix[Math.floor(Math.random() * 3)];
        
        // Choix de l'ordinateur
        const choixOrdinateur = choix[Math.floor(Math.random() * 3)];
        
        console.log(`👤 Vous : ${choixJoueur}`);
        console.log(`🤖 Ordinateur : ${choixOrdinateur}`);
        
        // Déterminer le gagnant
        let resultat;
        
        if (choixJoueur === choixOrdinateur) {
            resultat = "égalité";
            console.log("🤝 Égalité !");
        } else if (
            (choixJoueur === "pierre" && choixOrdinateur === "ciseaux") ||
            (choixJoueur === "feuille" && choixOrdinateur === "pierre") ||
            (choixJoueur === "ciseaux" && choixOrdinateur === "feuille")
        ) {
            resultat = "victoire";
            scoreJoueur++;
            console.log("🎉 Vous gagnez cette manche !");
        } else {
            resultat = "défaite";
            scoreOrdinateur++;
            console.log("😞 L'ordinateur gagne cette manche");
        }
        
        // Afficher le score
        console.log(`📊 Score : Vous ${scoreJoueur} - ${scoreOrdinateur} Ordinateur`);
        
        // Si égalité, on ne compte pas la manche
        if (resultat !== "égalité") {
            manche++;
        }
    }
    
    // Résultat final
    console.log("\n🏁 === RÉSULTAT FINAL ===");
    if (scoreJoueur > scoreOrdinateur) {
        console.log("🏆 FÉLICITATIONS ! Vous avez gagné !");
    } else {
        console.log("🤖 L'ordinateur a gagné ! Réessayez !");
    }
    
    console.log(`Score final : Vous ${scoreJoueur} - ${scoreOrdinateur} Ordinateur`);
}

// Lancer le jeu
jouerPierreFeuillesCiseaux();
```
{% endtab %}

{% tab title="Version avec menu" %}
```javascript
function menuPrincipal() {
    let continuer = true;
    
    while (continuer) {
        console.log("\n🎮 === MENU PRINCIPAL ===");
        console.log("1. Jouer Pierre-Feuille-Ciseaux");
        console.log("2. Voir les règles");
        console.log("3. Quitter");
        
        // Simulation du choix utilisateur
        const choix = ["1", "2", "3"][Math.floor(Math.random() * 3)];
        console.log(`Votre choix : ${choix}`);
        
        switch (choix) {
            case "1":
                jouerPierreFeuillesCiseaux();
                break;
                
            case "2":
                afficherRegles();
                break;
                
            case "3":
                console.log("👋 Merci d'avoir joué ! À bientôt !");
                continuer = false;
                break;
                
            default:
                console.log("❌ Choix invalide. Réessayez !");
                break;
        }
    }
}

function afficherRegles() {
    console.log("\n📜 === RÈGLES DU JEU ===");
    console.log("🗿 Pierre bat Ciseaux");
    console.log("📄 Feuille bat Pierre");  
    console.log("✂️ Ciseaux bat Feuille");
    console.log("\nPremier à 2 victoires gagne !");
}

// Lancer l'application
menuPrincipal();
```
{% endtab %}
{% endtabs %}

## 🎮 Quiz interactif

{% hint style="info" %}
**Question 1 :** Combien de fois cette boucle s'exécute-t-elle ?
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```
A) 4 fois  B) 5 fois  C) 6 fois  D) Infiniment
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) 5 fois**

La boucle s'exécute avec i = 0, 1, 2, 3, 4 (5 valeurs).
- Condition : `i < 5`
- Quand i = 5, la condition devient false et la boucle s'arrête.

</details>

{% hint style="info" %}
**Question 2 :** Que se passe-t-il si on oublie `break` dans un `switch` ?
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Fall-through (passage au case suivant)**

Sans `break`, JavaScript continue d'exécuter les cases suivants jusqu'à rencontrer un `break` ou la fin du switch.

```javascript
switch (x) {
    case 1:
        console.log("Un");
        // Pas de break !
    case 2:
        console.log("Deux");
        break;
}

// Si x = 1, affiche "Un" puis "Deux"
```

</details>

{% hint style="info" %}
**Question 3 :** Quelle est la différence entre `while` et `do...while` ?
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Nombre minimum d'exécutions :**

- **while** : Peut ne jamais s'exécuter si la condition est false dès le début
- **do...while** : S'exécute AU MOINS une fois avant de vérifier la condition

```javascript
// while : peut ne pas s'exécuter
let x = 10;
while (x < 5) {
    console.log("Ne s'affiche jamais");
}

// do...while : s'exécute au moins une fois  
do {
    console.log("S'affiche une fois");
} while (x < 5);
```

</details>

## 💻 Exercices pratiques

### Exercice 1 : Système de notation intelligent

{% tabs %}
{% tab title="Consigne" %}
Créez un système qui :
- Calcule la moyenne d'une liste de notes
- Détermine la mention selon la moyenne
- Indique si l'étudiant passe dans la classe supérieure
- Propose des conseils personnalisés

**Critères :**
- Moyenne ≥ 10 : passage en classe supérieure
- Mention selon moyenne : <10 (Insuffisant), 10-12 (Passable), 12-14 (Assez bien), 14-16 (Bien), ≥16 (Très bien)
- Conseils selon les points faibles
{% endtab %}

{% tab title="Code de départ" %}
```javascript
function evaluerEtudiant(notes, matieres) {
    // Votre code ici
    // Calculer moyenne, mention, passage, conseils
    
    return {
        moyenne: /* moyenne calculée */,
        mention: /* mention obtenue */,
        passe: /* true/false */,
        conseils: /* array de conseils */
    };
}

// Test
const notes = [12, 8, 15, 16, 9, 14];
const matieres = ["Math", "Français", "Histoire", "Anglais", "SVT", "Sport"];

console.log(evaluerEtudiant(notes, matieres));
```
{% endtab %}

{% tab title="Solution" %}
```javascript
function evaluerEtudiant(notes, matieres) {
    let somme = 0;
    let notesValides = 0;
    const matieresEnDifficulte = [];
    const matieresExcellentes = [];
    
    // Calcul de la moyenne et analyse par matière
    for (let i = 0; i < notes.length; i++) {
        const note = notes[i];
        const matiere = matieres[i] || `Matière ${i + 1}`;
        
        if (note >= 0 && note <= 20) {
            somme += note;
            notesValides++;
            
            if (note < 10) {
                matieresEnDifficulte.push({ matiere, note });
            } else if (note >= 16) {
                matieresExcellentes.push({ matiere, note });
            }
        }
    }
    
    if (notesValides === 0) {
        return {
            moyenne: 0,
            mention: "Aucune note valide",
            passe: false,
            conseils: ["Aucune évaluation disponible"]
        };
    }
    
    const moyenne = somme / notesValides;
    
    // Détermination de la mention
    let mention;
    if (moyenne >= 16) {
        mention = "Très bien";
    } else if (moyenne >= 14) {
        mention = "Bien";
    } else if (moyenne >= 12) {
        mention = "Assez bien";
    } else if (moyenne >= 10) {
        mention = "Passable";
    } else {
        mention = "Insuffisant";
    }
    
    // Passage en classe supérieure
    const passe = moyenne >= 10;
    
    // Génération des conseils
    const conseils = [];
    
    if (moyenne >= 16) {
        conseils.push("🏆 Excellent travail ! Continuez sur cette lancée.");
    } else if (moyenne >= 14) {
        conseils.push("😊 Très bon niveau ! Quelques efforts pour viser l'excellence.");
    } else if (moyenne >= 12) {
        conseils.push("🙂 Bon travail global, mais des améliorations sont possibles.");
    } else if (moyenne >= 10) {
        conseils.push("😐 Juste la moyenne. Il faut travailler plus régulièrement.");
    } else {
        conseils.push("😟 Résultats insuffisants. Un soutien scolaire est recommandé.");
    }
    
    // Conseils spécifiques par matière
    if (matieresEnDifficulte.length > 0) {
        conseils.push(`📚 Matières à travailler : ${matieresEnDifficulte.map(m => `${m.matiere} (${m.note}/20)`).join(", ")}`);
    }
    
    if (matieresExcellentes.length > 0) {
        conseils.push(`⭐ Points forts : ${matieresExcellentes.map(m => m.matiere).join(", ")}`);
    }
    
    // Conseils sur le passage
    if (passe) {
        conseils.push("✅ Passage en classe supérieure acquis !");
    } else {
        conseils.push("❌ Redoublement probable. Travaillez davantage cet été.");
    }
    
    return {
        moyenne: parseFloat(moyenne.toFixed(2)),
        mention,
        passe,
        conseils
    };
}

// Tests
const notes1 = [12, 8, 15, 16, 9, 14];
const matieres1 = ["Math", "Français", "Histoire", "Anglais", "SVT", "Sport"];

const notes2 = [18, 17, 19, 16, 20, 17];
const matieres2 = ["Math", "Physique", "Chimie", "Info", "Anglais", "Philo"];

console.log("=== ÉTUDIANT 1 ===");
console.log(evaluerEtudiant(notes1, matieres1));

console.log("\n=== ÉTUDIANT 2 ===");  
console.log(evaluerEtudiant(notes2, matieres2));
```

**Résultat :**
```
=== ÉTUDIANT 1 ===
{
  moyenne: 12.33,
  mention: "Assez bien", 
  passe: true,
  conseils: [
    "🙂 Bon travail global, mais des améliorations sont possibles.",
    "📚 Matières à travailler : Français (8/20), SVT (9/20)",
    "⭐ Points forts : Anglais",
    "✅ Passage en classe supérieure acquis !"
  ]
}
```
{% endtab %}
{% endtabs %}

### Exercice 2 : Générateur de mots de passe

{% tabs %}
{% tab title="Consigne" %}
Créez un générateur qui produit des mots de passe selon des critères :

**Options :**
- Longueur du mot de passe
- Inclure majuscules (A-Z)
- Inclure minuscules (a-z)  
- Inclure chiffres (0-9)
- Inclure symboles (!@#$%...)

**Validation :**
- Au moins un type de caractère sélectionné
- Longueur entre 4 et 50 caractères
- Vérifier la force du mot de passe généré
{% endtab %}

{% tab title="Solution" %}
```javascript
function genererMotDePasse(options) {
    const {
        longueur = 12,
        majuscules = true,
        minuscules = true,
        chiffres = true,
        symboles = false
    } = options;
    
    // Validation des options
    if (longueur < 4 || longueur > 50) {
        return {
            motDePasse: "",
            erreur: "La longueur doit être entre 4 et 50 caractères"
        };
    }
    
    if (!majuscules && !minuscules && !chiffres && !symboles) {
        return {
            motDePasse: "",
            erreur: "Au moins un type de caractère doit être sélectionné"
        };
    }
    
    // Définir les caractères disponibles
    let caracteres = "";
    const sets = {
        majuscules: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
        minuscules: "abcdefghijklmnopqrstuvwxyz", 
        chiffres: "0123456789",
        symboles: "!@#$%^&*()_+-=[]{}|;:,.<>?"
    };
    
    const typesSelectionnes = [];
    
    if (majuscules) {
        caracteres += sets.majuscules;
        typesSelectionnes.push("majuscules");
    }
    if (minuscules) {
        caracteres += sets.minuscules;
        typesSelectionnes.push("minuscules");
    }
    if (chiffres) {
        caracteres += sets.chiffres;
        typesSelectionnes.push("chiffres");
    }
    if (symboles) {
        caracteres += sets.symboles;
        typesSelectionnes.push("symboles");
    }
    
    // Générer le mot de passe
    let motDePasse = "";
    
    // S'assurer qu'au moins un caractère de chaque type sélectionné est présent
    for (const type of typesSelectionnes) {
        const setCaracteres = sets[type];
        const indexAleatoire = Math.floor(Math.random() * setCaracteres.length);
        motDePasse += setCaracteres[indexAleatoire];
    }
    
    // Compléter avec des caractères aléatoires
    for (let i = motDePasse.length; i < longueur; i++) {
        const indexAleatoire = Math.floor(Math.random() * caracteres.length);
        motDePasse += caracteres[indexAleatoire];
    }
    
    // Mélanger le mot de passe pour éviter les patterns
    motDePasse = melangerChaine(motDePasse);
    
    // Évaluer la force
    const force = evaluerForceMotDePasse(motDePasse);
    
    return {
        motDePasse,
        force,
        longueur: motDePasse.length,
        typesInclus: typesSelectionnes,
        erreur: null
    };
}

function melangerChaine(chaine) {
    const tableau = chaine.split("");
    
    for (let i = tableau.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [tableau[i], tableau[j]] = [tableau[j], tableau[i]];
    }
    
    return tableau.join("");
}

function evaluerForceMotDePasse(motDePasse) {
    let score = 0;
    const tests = [
        { regex: /[a-z]/, points: 1, nom: "minuscules" },
        { regex: /[A-Z]/, points: 1, nom: "majuscules" },
        { regex: /[0-9]/, points: 1, nom: "chiffres" },
        { regex: /[^a-zA-Z0-9]/, points: 2, nom: "symboles" }
    ];
    
    // Points pour les types de caractères
    for (const test of tests) {
        if (test.regex.test(motDePasse)) {
            score += test.points;
        }
    }
    
    // Points pour la longueur
    if (motDePasse.length >= 8) score += 1;
    if (motDePasse.length >= 12) score += 1;
    if (motDePasse.length >= 16) score += 1;
    
    // Malus pour les patterns répétitifs
    if (/(.)\1{2,}/.test(motDePasse)) score -= 1; // 3+ caractères identiques
    if (/012|123|234|345|456|567|678|789|890/.test(motDePasse)) score -= 1; // Séquences
    
    // Déterminer le niveau
    let niveau, couleur, description;
    
    if (score <= 2) {
        niveau = "Très faible";
        couleur = "🔴";
        description = "Facilement cassable";
    } else if (score <= 4) {
        niveau = "Faible";
        couleur = "🟠";
        description = "Amélioration nécessaire";
    } else if (score <= 6) {
        niveau = "Moyen";
        couleur = "🟡";
        description = "Acceptable";
    } else if (score <= 8) {
        niveau = "Fort";
        couleur = "🟢";
        description = "Bien sécurisé";
    } else {
        niveau = "Très fort";
        couleur = "🟢";
        description = "Excellente sécurité";
    }
    
    return {
        score,
        niveau,
        couleur,
        description
    };
}

// Tests
console.log("=== GÉNÉRATEUR DE MOTS DE PASSE ===");

const tests = [
    { longueur: 8, majuscules: true, minuscules: true, chiffres: false, symboles: false },
    { longueur: 12, majuscules: true, minuscules: true, chiffres: true, symboles: false },
    { longueur: 16, majuscules: true, minuscules: true, chiffres: true, symboles: true },
    { longueur: 3, majuscules: true, minuscules: true, chiffres: true, symboles: true }, // Erreur
];

tests.forEach((options, i) => {
    console.log(`\n--- Test ${i + 1} ---`);
    console.log("Options:", options);
    
    const resultat = genererMotDePasse(options);
    
    if (resultat.erreur) {
        console.log("❌ Erreur:", resultat.erreur);
    } else {
        console.log(`✅ Mot de passe: ${resultat.motDePasse}`);
        console.log(`🔒 Force: ${resultat.force.couleur} ${resultat.force.niveau} (${resultat.force.description})`);
        console.log(`📊 Types inclus: ${resultat.typesInclus.join(", ")}`);
    }
});
```
{% endtab %}
{% endtabs %}

## ⚠️ Pièges courants et solutions

### Piège 1 : Confusion entre = et ==

{% tabs %}
{% tab title="❌ Erreur commune" %}
```javascript
let age = 18;

// ERREUR : Assignation au lieu de comparaison !
if (age = 16) {  // Assigne 16 à age !
    console.log("Sera toujours exécuté");
}

console.log(age); // 16 - la variable a été modifiée !
```
{% endtab %}

{% tab title="✅ Solution correcte" %}
```javascript
let age = 18;

// Comparaison correcte
if (age === 16) {  // ou age == 16
    console.log("Ne sera pas exécuté");
} else {
    console.log("Sera exécuté");
}

console.log(age); // 18 - inchangé
```

**💡 Astuce :** Utilisez `===` (égalité stricte) pour éviter les conversions automatiques.
{% endtab %}
{% endtabs %}

### Piège 2 : Boucles infinies

{% tabs %}
{% tab title="❌ Boucles qui ne finissent jamais" %}
```javascript
// ERREUR 1 : Condition jamais false
let i = 1;
while (i > 0) {  // i est toujours > 0
    console.log(i);
    i++; // i augmente, donc toujours > 0 !
}

// ERREUR 2 : Oubli d'incrémentation
let j = 0;
while (j < 5) {  // j ne change jamais
    console.log(j);
    // j++ oublié !
}

// ERREUR 3 : Mauvaise direction
for (let k = 0; k >= 0; k++) {  // k sera toujours >= 0
    console.log(k);
}
```
{% endtab %}

{% tab title="✅ Solutions sécurisées" %}
```javascript
// ✅ Condition qui finira par être false
let i = 10;
while (i > 0) {
    console.log(i);
    i--; // i diminue, finira par être <= 0
}

// ✅ Sécurité avec compteur maximum
let tentatives = 0;
const maxTentatives = 100;

while (quelqueCondition() && tentatives < maxTentatives) {
    // logique de la boucle
    tentatives++;
}

if (tentatives >= maxTentatives) {
    console.log("⚠️ Boucle arrêtée pour éviter l'infini");
}

// ✅ Boucle for avec condition claire
for (let k = 0; k < 5; k++) {  // k sera 0,1,2,3,4 puis 5 < 5 = false
    console.log(k);
}
```
{% endtab %}
{% endtabs %}

### Piège 3 : Oubli du break dans switch

{% tabs %}
{% tab title="❌ Fall-through non désiré" %}
```javascript
const note = 15;

switch (true) {
    case note >= 16:
        console.log("Très bien");
        // Oubli du break !
    case note >= 14:
        console.log("Bien");
        // Oubli du break !
    case note >= 12:
        console.log("Assez bien");
        break;
    case note >= 10:
        console.log("Passable");
        break;
    default:
        console.log("Insuffisant");
}

// Résultat pour note=15 : affiche "Bien" ET "Assez bien" !
```
{% endtab %}

{% tab title="✅ Avec break appropriés" %}
```javascript
const note = 15;

// Solution 1 : switch avec break
switch (true) {
    case note >= 16:
        console.log("Très bien");
        break;
    case note >= 14:
        console.log("Bien");
        break;
    case note >= 12:
        console.log("Assez bien");
        break;
    case note >= 10:
        console.log("Passable");
        break;
    default:
        console.log("Insuffisant");
}

// Solution 2 : if/else (plus naturel pour ranges)
if (note >= 16) {
    console.log("Très bien");
} else if (note >= 14) {
    console.log("Bien");
} else if (note >= 12) {
    console.log("Assez bien");
} else if (note >= 10) {
    console.log("Passable");
} else {
    console.log("Insuffisant");
}

// Résultat pour note=15 : affiche seulement "Bien" ✅
```
{% endtab %}
{% endtabs %}

## 🔗 Liens avec d'autres concepts

{% content-ref url="operateurs-logiques.md" %}
[operateurs-logiques.md](operateurs-logiques.md)
{% endcontent-ref %}

{% content-ref url="operateur-ternaire.md" %}
[operateur-ternaire.md](operateur-ternaire.md)
{% endcontent-ref %}

{% content-ref url="../niveau-intermediaire/fonctions.md" %}
[fonctions.md](../niveau-intermediaire/fonctions.md)
{% endcontent-ref %}

## 🚀 Projet pratique

{% hint style="success" %}
**Défi : Système de gestion d'un quiz interactif**

**Fonctionnalités à implémenter :**
- [ ] Base de questions avec réponses multiples
- [ ] Système de score avec niveaux de difficulté
- [ ] Timer pour chaque question
- [ ] Statistiques détaillées en fin de partie
- [ ] Possibilité de rejouer avec nouvelles questions

**Technologies :** HTML, CSS, JavaScript  
**Temps estimé :** 2-3 heures
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🎯 **if/else :** Pour les décisions simples et conditions complexes  
🔀 **switch :** Pour choisir entre plusieurs valeurs exactes  
🔄 **for :** Quand vous savez combien de fois répéter  
⏳ **while :** Quand vous avez une condition d'arrêt  
🛑 **break/continue :** Pour contrôler le flux des boucles  
⚠️ **Évitez :** Boucles infinies, confusion ==/=, oubli de break  
📏 **Bonnes pratiques :** Conditions claires, code lisible, gestion d'erreurs  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Passez aux [Fonctions](../niveau-intermediaire/fonctions.md) pour organiser votre code avec des blocs réutilisables !
{% endhint %}
