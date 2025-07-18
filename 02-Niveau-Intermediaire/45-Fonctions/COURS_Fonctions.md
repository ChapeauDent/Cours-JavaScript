# Fonctions en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** Fonctions (Declaration, Execution, Parametres, Valeurs de retour)
- **Niveau :** INTERMEDIAIRE (transition du debutant)
- **Duree estimee :** 120 minutes
- **Prerequis :** Variables, types, operateurs, structures de controle
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Pourquoi et comment utiliser des fonctions
- [ ] **Declarer** : Creer des fonctions avec differentes syntaxes
- [ ] **Utiliser** : Appeler des fonctions avec des parametres
- [ ] **Organiser** : Structurer ton code avec des fonctions reutilisables

### Mots-cles a retenir
- **Fonction** : Bloc de code reutilisable qui effectue une tache specifique
- **Parametre** : Variable qu'on donne a la fonction pour qu'elle travaille
- **Argument** : Valeur concrete qu'on passe lors de l'appel
- **Return** : Mot-cle pour renvoyer un resultat de la fonction
- **Scope** : Zone ou une variable existe et est accessible

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Imagine que tu dois calculer l'aire de plusieurs rectangles dans ton programme. Tu pourrais copier-coller le meme calcul partout... ou creer UNE fonction qui fait le calcul, et l'utiliser partout !

**Les fonctions permettent de :**
- **Reutiliser** le code au lieu de le recopier
- **Organiser** ton programme en petits blocs logiques
- **Faciliter** la correction des erreurs (un seul endroit a modifier)
- **Collaborer** plus facilement (chacun programme ses fonctions)

C'est comme avoir une boite a outils : au lieu de refabriquer un marteau a chaque fois, tu le prends dans ta boite !

### Definition et concepts cles

**Une fonction est composee de :**

1. **Nom** - Comment on l'appelle
2. **Parametres** - Ce qu'on lui donne en entree (optionnel)
3. **Corps** - Le code qu'elle execute
4. **Retour** - Ce qu'elle renvoie en sortie (optionnel)

**Types de fonctions :**
- **Fonction declaree** : `function nomFonction() { }`
- **Fonction exprimee** : `let nomFonction = function() { }`
- **Fonction fleche** : `let nomFonction = () => { }` (plus avance)

### Syntaxe de base
```javascript
// Declaration d'une fonction
function direBonjour() {
    console.log("Bonjour tout le monde !");
}

// Appel de la fonction
direBonjour();  // Affiche: Bonjour tout le monde !

// Fonction avec parametres
function direBonjour(nom) {
    console.log("Bonjour " + nom + " !");
}

// Appel avec argument
direBonjour("Alice");  // Affiche: Bonjour Alice !

// Fonction avec retour
function additionner(a, b) {
    return a + b;
}

// Utilisation du resultat
let resultat = additionner(5, 3);
console.log(resultat);  // Affiche: 8
```

### Points d'attention
- **Nommage** : Utilise des verbes d'action (calculer, afficher, verifier)
- **Return** : Une fonction peut retourner une seule valeur
- **Parameters vs Arguments** : Parametres dans la declaration, arguments lors de l'appel
- **Scope** : Variables declarees dans une fonction ne sont visibles que dedans

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```javascript
// Fonction simple sans parametre
function afficherMenu() {
    console.log("=== MENU DU RESTAURANT ===");
    console.log("1. Pizza Margherita - 12 euros");
    console.log("2. Burger Classic - 8 euros");
    console.log("3. Salade Cesar - 10 euros");
    console.log("========================");
}

// Fonction avec un parametre
function calculerTTC(prixHT) {
    let tva = prixHT * 0.20;  // 20% de TVA
    let prixTTC = prixHT + tva;
    return prixTTC;
}

// Fonction avec plusieurs parametres
function calculerTotal(prix, quantite) {
    let sousTotal = prix * quantite;
    let prixFinal = calculerTTC(sousTotal);  // Appel d'une autre fonction !
    return prixFinal;
}

// Utilisation des fonctions
console.log("Bienvenue dans notre restaurant !");
afficherMenu();

let prixPizza = 12;
let quantitePizza = 2;

let totalCommande = calculerTotal(prixPizza, quantitePizza);
console.log("Votre commande: " + quantitePizza + " pizzas");
console.log("Total TTC: " + totalCommande.toFixed(2) + " euros");
```

**Resultat attendu :**
```
Bienvenue dans notre restaurant !
=== MENU DU RESTAURANT ===
1. Pizza Margherita - 12 euros
2. Burger Classic - 8 euros
3. Salade Cesar - 10 euros
========================
Votre commande: 2 pizzas
Total TTC: 28.80 euros
```

### Exemple intermediaire
```javascript
// Fonction de validation avec retour boolean
function estEmailValide(email) {
    // Verification simple : doit contenir @ et un point
    let contientArobase = email.includes("@");
    let contientPoint = email.includes(".");
    let assezLong = email.length >= 5;
    
    return contientArobase && contientPoint && assezLong;
}

// Fonction de calcul complexe
function calculerNoteMoyenne(notes) {
    let total = 0;
    let nombreNotes = notes.length;
    
    // Additionner toutes les notes
    for (let i = 0; i < nombreNotes; i++) {
        total = total + notes[i];
    }
    
    let moyenne = total / nombreNotes;
    return moyenne;
}

// Fonction de determination de mention
function obtenirMention(moyenne) {
    if (moyenne >= 16) {
        return "Tres Bien";
    } else if (moyenne >= 14) {
        return "Bien";
    } else if (moyenne >= 12) {
        return "Assez Bien";
    } else if (moyenne >= 10) {
        return "Passable";
    } else {
        return "Insuffisant";
    }
}

// Fonction principale qui utilise toutes les autres
function analyserEleve(nom, email, listeNotes) {
    console.log("=== ANALYSE DE L'ELEVE ===");
    console.log("Nom: " + nom);
    
    // Verification email
    if (estEmailValide(email)) {
        console.log("Email: " + email + " (valide)");
    } else {
        console.log("Email: " + email + " (INVALIDE)");
        return;  // Sortie anticipee de la fonction
    }
    
    // Calcul des notes
    let moyenne = calculerNoteMoyenne(listeNotes);
    let mention = obtenirMention(moyenne);
    
    console.log("Notes: " + listeNotes.join(", "));
    console.log("Moyenne: " + moyenne.toFixed(2) + "/20");
    console.log("Mention: " + mention);
    
    return {
        nom: nom,
        moyenne: moyenne,
        mention: mention
    };
}

// Test des fonctions
let resultats = analyserEleve("Marie Durand", "marie@lycee.fr", [15, 12, 18, 14, 16]);
console.log("Resultats retournes:", resultats);
```

### Exemple avance (optionnel)
```javascript
// Fonction avec parametres par defaut (ES6)
function creerProfile(nom, age = 18, ville = "Paris") {
    return {
        nom: nom,
        age: age,
        ville: ville,
        description: "Je m'appelle " + nom + ", j'ai " + age + " ans et j'habite a " + ville
    };
}

// Fonction qui retourne une autre fonction (closure)
function creerCompteur(valeurInitiale = 0) {
    let compteur = valeurInitiale;
    
    return function() {
        compteur++;
        return compteur;
    };
}

// Fonction recursive (qui s'appelle elle-meme)
function factorielle(n) {
    if (n <= 1) {
        return 1;  // Cas d'arret
    } else {
        return n * factorielle(n - 1);  // Appel recursif
    }
}

// Fonction generique de tri (callback)
function trierNombres(tableau, croissant = true) {
    return tableau.sort(function(a, b) {
        if (croissant) {
            return a - b;  // Tri croissant
        } else {
            return b - a;  // Tri decroissant
        }
    });
}

// Demonstrations
console.log("=== FONCTIONS AVANCEES ===");

// Parametres par defaut
let profile1 = creerProfile("Alice");
let profile2 = creerProfile("Bob", 25, "Lyon");
console.log(profile1.description);
console.log(profile2.description);

// Compteur avec closure
let monCompteur = creerCompteur(10);
console.log("Compteur:", monCompteur());  // 11
console.log("Compteur:", monCompteur());  // 12

// Factorielle recursive
console.log("5! =", factorielle(5));  // 120

// Tri avec callback
let nombres = [3, 1, 4, 1, 5, 9, 2, 6];
console.log("Original:", nombres);
console.log("Croissant:", trierNombres([...nombres], true));
console.log("Decroissant:", trierNombres([...nombres], false));
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Calculatrice modulaire
**Consigne :** Cree des fonctions pour chaque operation mathematique

**Fonctions a creer :**
- `additionner(a, b)` - retourne a + b
- `soustraire(a, b)` - retourne a - b  
- `multiplier(a, b)` - retourne a * b
- `diviser(a, b)` - retourne a / b (attention division par zero !)
- `calculer(operation, a, b)` - fonction principale qui appelle les autres

**Niveau de difficulte :** 2 etoiles sur 5

### Exercice 2 : Validateur de formulaire
**Consigne :** Cree des fonctions pour valider differents champs

**Fonctions a creer :**
- `validerNom(nom)` - au moins 2 caracteres, que des lettres
- `validerAge(age)` - entre 13 et 99 ans
- `validerEmail(email)` - format valide
- `validerFormulaire(nom, age, email)` - valide tout et retourne le resultat

**Niveau de difficulte :** 4 etoiles sur 5

### Exercice 3 : Jeu de des
**Consigne :** Cree un systeme de jeu avec plusieurs fonctions

**Fonctions a creer :**
- `lancerDe()` - retourne un nombre entre 1 et 6
- `jouerTour(joueur)` - lance 2 des pour un joueur
- `determinerGagnant(score1, score2)` - compare et retourne le gagnant
- `jouerPartie(nom1, nom2)` - fonction complete du jeu

**Niveau de difficulte :** 3 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```javascript
// Fonctions mathematiques de base
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
        console.log("Erreur: Division par zero impossible !");
        return null;  // Valeur speciale pour indiquer l'erreur
    }
    return a / b;
}

// Fonction principale qui utilise les autres
function calculer(operation, a, b) {
    console.log("Calcul: " + a + " " + operation + " " + b);
    
    let resultat;
    
    if (operation === "+") {
        resultat = additionner(a, b);
    } else if (operation === "-") {
        resultat = soustraire(a, b);
    } else if (operation === "*") {
        resultat = multiplier(a, b);
    } else if (operation === "/") {
        resultat = diviser(a, b);
    } else {
        console.log("Operation inconnue !");
        return null;
    }
    
    if (resultat !== null) {
        console.log("Resultat: " + resultat);
    }
    
    return resultat;
}

// Tests de la calculatrice
console.log("=== CALCULATRICE MODULAIRE ===");
calculer("+", 10, 5);     // 15
calculer("-", 10, 5);     // 5
calculer("*", 10, 5);     // 50
calculer("/", 10, 5);     // 2
calculer("/", 10, 0);     // Erreur
calculer("%", 10, 5);     // Operation inconnue

// Version amelioree avec objet retourne
function calculerAvance(operation, a, b) {
    let resultat = calculer(operation, a, b);
    
    return {
        operation: operation,
        operande1: a,
        operande2: b,
        resultat: resultat,
        valide: (resultat !== null)
    };
}

let test = calculerAvance("*", 7, 8);
console.log("Test avance:", test);
```

**Explication :** Chaque fonction a une responsabilite precise. La fonction principale orchestre les autres.

### Solution Exercice 2
```javascript
// Fonctions de validation individuelles
function validerNom(nom) {
    // Verification longueur
    if (nom.length < 2) {
        return {
            valide: false,
            erreur: "Le nom doit avoir au moins 2 caracteres"
        };
    }
    
    // Verification caracteres (simplifie)
    let charactersValides = /^[a-zA-Z\s-]+$/.test(nom);
    if (!charactersValides) {
        return {
            valide: false,
            erreur: "Le nom ne peut contenir que des lettres, espaces et tirets"
        };
    }
    
    return { valide: true, erreur: null };
}

function validerAge(age) {
    // Verification type number
    if (typeof age !== "number" || isNaN(age)) {
        return {
            valide: false,
            erreur: "L'age doit etre un nombre"
        };
    }
    
    // Verification intervalle
    if (age < 13 || age > 99) {
        return {
            valide: false,
            erreur: "L'age doit etre entre 13 et 99 ans"
        };
    }
    
    return { valide: true, erreur: null };
}

function validerEmail(email) {
    // Verification presence caracteres obligatoires
    if (!email.includes("@")) {
        return {
            valide: false,
            erreur: "L'email doit contenir le caractere @"
        };
    }
    
    if (!email.includes(".")) {
        return {
            valide: false,
            erreur: "L'email doit contenir au moins un point"
        };
    }
    
    // Verification longueur minimum
    if (email.length < 5) {
        return {
            valide: false,
            erreur: "L'email est trop court"
        };
    }
    
    // Verification format basique
    let parties = email.split("@");
    if (parties.length !== 2 || parties[0].length === 0 || parties[1].length === 0) {
        return {
            valide: false,
            erreur: "Format d'email invalide"
        };
    }
    
    return { valide: true, erreur: null };
}

// Fonction principale de validation
function validerFormulaire(nom, age, email) {
    console.log("=== VALIDATION FORMULAIRE ===");
    console.log("Nom: '" + nom + "'");
    console.log("Age: " + age);
    console.log("Email: '" + email + "'");
    
    let resultats = {
        nom: validerNom(nom),
        age: validerAge(age),
        email: validerEmail(email)
    };
    
    // Verification globale
    let toutValide = resultats.nom.valide && resultats.age.valide && resultats.email.valide;
    
    console.log("\n=== RESULTATS ===");
    
    // Affichage des erreurs
    if (!resultats.nom.valide) {
        console.log("ERREUR Nom: " + resultats.nom.erreur);
    } else {
        console.log("Nom: VALIDE");
    }
    
    if (!resultats.age.valide) {
        console.log("ERREUR Age: " + resultats.age.erreur);
    } else {
        console.log("Age: VALIDE");
    }
    
    if (!resultats.email.valide) {
        console.log("ERREUR Email: " + resultats.email.erreur);
    } else {
        console.log("Email: VALIDE");
    }
    
    console.log("\nFormulaire " + (toutValide ? "VALIDE" : "INVALIDE"));
    
    return {
        valide: toutValide,
        details: resultats
    };
}

// Tests du validateur
validerFormulaire("Alice Dupont", 16, "alice@lycee.fr");
console.log("\n" + "=".repeat(40) + "\n");
validerFormulaire("A", 150, "email-invalide");
```

### Solution Exercice 3
```javascript
// Fonction pour lancer un de
function lancerDe() {
    return Math.floor(Math.random() * 6) + 1;  // Nombre entre 1 et 6
}

// Fonction pour jouer un tour (2 des)
function jouerTour(nomJoueur) {
    let de1 = lancerDe();
    let de2 = lancerDe();
    let total = de1 + de2;
    
    console.log(nomJoueur + " lance les des:");
    console.log("  De 1: " + de1);
    console.log("  De 2: " + de2);
    console.log("  Total: " + total);
    
    // Bonus pour double
    if (de1 === de2) {
        console.log("  DOUBLE ! Bonus de 5 points !");
        total += 5;
        console.log("  Nouveau total: " + total);
    }
    
    return total;
}

// Fonction pour determiner le gagnant
function determinerGagnant(score1, nom1, score2, nom2) {
    console.log("\n=== RESULTATS FINAUX ===");
    console.log(nom1 + ": " + score1 + " points");
    console.log(nom2 + ": " + score2 + " points");
    
    if (score1 > score2) {
        console.log("GAGNANT: " + nom1 + " !");
        return nom1;
    } else if (score2 > score1) {
        console.log("GAGNANT: " + nom2 + " !");
        return nom2;
    } else {
        console.log("EGALITE ! Match nul !");
        return "Egalite";
    }
}

// Fonction principale du jeu
function jouerPartie(nom1, nom2, nombreTours = 3) {
    console.log("=== JEU DE DES ===");
    console.log("Joueurs: " + nom1 + " vs " + nom2);
    console.log("Nombre de tours: " + nombreTours);
    console.log("Regle: 2 des par tour, bonus +5 pour les doubles\n");
    
    let scoreTotal1 = 0;
    let scoreTotal2 = 0;
    
    // Boucle pour chaque tour
    for (let tour = 1; tour <= nombreTours; tour++) {
        console.log("--- TOUR " + tour + " ---");
        
        let scoreTour1 = jouerTour(nom1);
        let scoreTour2 = jouerTour(nom2);
        
        scoreTotal1 += scoreTour1;
        scoreTotal2 += scoreTour2;
        
        console.log("Scores cumules: " + nom1 + " = " + scoreTotal1 + ", " + nom2 + " = " + scoreTotal2);
        console.log("");  // Ligne vide
    }
    
    let gagnant = determinerGagnant(scoreTotal1, nom1, scoreTotal2, nom2);
    
    // Objet avec tous les resultats
    return {
        joueur1: { nom: nom1, score: scoreTotal1 },
        joueur2: { nom: nom2, score: scoreTotal2 },
        gagnant: gagnant,
        nombreTours: nombreTours
    };
}

// Fonction bonus: championnat
function jouerChampionnat(listeJoueurs) {
    console.log("=== CHAMPIONNAT DE DES ===");
    let resultats = [];
    
    // Jouer tous contre tous
    for (let i = 0; i < listeJoueurs.length; i++) {
        for (let j = i + 1; j < listeJoueurs.length; j++) {
            console.log("\n" + "=".repeat(50));
            let match = jouerPartie(listeJoueurs[i], listeJoueurs[j], 2);
            resultats.push(match);
        }
    }
    
    return resultats;
}

// Tests du jeu
let partie1 = jouerPartie("Alice", "Bob", 2);
console.log("\nResultats finaux:", partie1);

// Test championnat
// let joueurs = ["Alice", "Bob", "Charlie"];
// jouerChampionnat(joueurs);
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Variables** : Pour stocker parametres et resultats
- **Types** : Pour valider les parametres et retours
- **Operateurs** : Pour effectuer des calculs dans les fonctions
- **Conditions** : Pour controler l'execution selon les parametres
- **Boucles** : Pour repeter des operations dans les fonctions

### Prochaines etapes  
- **Arrays** : Fonctions pour manipuler des tableaux
- **Objets** : Methodes (fonctions liees aux objets)
- **Programmation fonctionnelle** : map(), filter(), reduce()
- **Fonctions asynchrones** : async/await pour operations longues

### Concepts complementaires
- **Closures** : Fonctions qui "se souviennent" de leurs variables
- **Callbacks** : Fonctions passees en parametre
- **Recursivite** : Fonctions qui s'appellent elles-memes

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer un systeme de gestion de bibliotheque avec fonctions

**Fonctionnalites requises :**
- [ ] Ajouter un livre (fonction avec parametres)
- [ ] Rechercher par titre ou auteur (fonction de recherche)
- [ ] Calculer statistiques (nombre total, par genre)
- [ ] Emprunter/rendre un livre (modification statut)
- [ ] Afficher bibliotheque complete (fonction d'affichage)

**Fonctions minimum a creer :**
- `ajouterLivre(titre, auteur, genre)`
- `rechercherLivre(motCle)`
- `emprunterLivre(titre)`
- `calculerStatistiques()`
- `afficherBibliotheque()`

**Structure de fichiers suggeree :**
```
bibliotheque/
├── index.html
├── style.css  
└── script.js
```

**Temps estime :** 90 minutes

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - Fonctions](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Functions)
- [MDN - Parametres de fonction](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions/arguments)
- [MDN - Valeurs de retour](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/return)
- [MDN - Portee des variables](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Grammar_and_types#Variable_scope)

### Videos recommandees
- "Les fonctions en JavaScript" - Grafikart (45min)
- "Scope et closures" - comprendre la portee des variables

### Articles approfondis
- "Fonctions pures vs impures" - concepts avances de programmation fonctionnelle
- "Arrow functions vs function declarations" - differences et cas d'usage

### Outils de pratique
- [CodePen - Functions playground](https://codepen.io) - tester et experimenter
- [JSFiddle](https://jsfiddle.net) - partager tes fonctions avec d'autres

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre parametre et argument ?
   - Reponse : Parametre = variable dans la declaration, argument = valeur lors de l'appel

2. **Question pratique :** Que se passe-t-il si j'appelle une fonction avec moins d'arguments que de parametres ?
   - Reponse : Les parametres manquants valent `undefined`

3. **Question d'application :** Comment faire pour qu'une fonction retourne plusieurs valeurs ?
   - Reponse : Retourner un objet ou un tableau avec plusieurs proprietes/elements

### Checklist de maitrise
- [ ] Je sais declarer une fonction avec function
- [ ] Je comprends la difference parametres/arguments
- [ ] Je peux utiliser return pour renvoyer une valeur
- [ ] Je comprends le scope (portee) des variables
- [ ] Je peux decomposer un probleme en plusieurs fonctions
- [ ] Je sais appeler une fonction depuis une autre fonction

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
function calculer(a, b) {
    let resultat = a + b;
    // Oubli du return !
}

let total = calculer(5, 3);
console.log(total);  // undefined
```

**Solution :**
```javascript
function calculer(a, b) {
    let resultat = a + b;
    return resultat;  // N'oublie jamais le return !
}

let total = calculer(5, 3);
console.log(total);  // 8
```

**Explication :** Sans return, la fonction renvoie undefined par defaut.

### Erreur frequente 2
**Code problematique :**
```javascript
function direBonjour() {
    let nom = "Alice";
}

console.log(nom);  // Erreur ! nom n'existe pas ici
```

**Solution :**
```javascript
function direBonjour() {
    let nom = "Alice";
    return nom;  // Renvoie la valeur
}

let nomRetourne = direBonjour();
console.log(nomRetourne);  // Alice
```

**Explication :** Les variables dans une fonction ne sont visibles que dans cette fonction (scope).

### Erreur frequente 3
**Code problematique :**
```javascript
function multiplier(a, b) {
    return a * b;
}

let resultat = multiplier(5);  // Un seul argument !
console.log(resultat);  // NaN (5 * undefined)
```

**Solution :**
```javascript
function multiplier(a, b = 1) {  // Valeur par defaut
    return a * b;
}

// Ou verification manuelle
function multiplierSecurise(a, b) {
    if (b === undefined) {
        b = 1;  // Valeur par defaut
    }
    return a * b;
}

let resultat = multiplier(5);  // 5 (5 * 1)
console.log(resultat);
```

**Explication :** Toujours prevoir des valeurs par defaut pour les parametres optionnels.

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Les fonctions permettent de reutiliser et organiser le code]
- [Il faut bien comprendre parametres vs arguments]  
- [Le return est crucial pour renvoyer des resultats]

### Questions a approfondir
- [Comment marchent les closures exactement ?]
- [Quand utiliser arrow functions vs function normale ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices de fonctions)
- [ ] Revision dans 1 semaine (creer un projet avec plein de fonctions)
- [ ] Revision dans 1 mois (expliquer les fonctions a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript fonctions parametres arguments return scope reutilisabilite modularite

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 7/126*
