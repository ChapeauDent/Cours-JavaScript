# Operateurs en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** Operateurs (Arithmetiques, Comparaison, Logiques, Assignation)
- **Niveau :** DEBUTANT COMPLET (14 ans, niveau zero)
- **Duree estimee :** 90 minutes
- **Prerequis :** Variables, types, conversion, structures de controle
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Les differents types d'operateurs et leur utilite
- [ ] **Appliquer** : Utiliser les operateurs pour faire des calculs et comparaisons
- [ ] **Analyser** : Choisir le bon operateur selon la situation
- [ ] **Creer** : Ecrire des expressions complexes avec plusieurs operateurs

### Mots-cles a retenir
- **Operateur** : Symbole qui effectue une operation (+, -, *, etc.)
- **Expression** : Combinaison de valeurs et d'operateurs
- **Precedence** : Ordre de calcul des operateurs (comme en maths)
- **Operande** : Valeur sur laquelle agit un operateur
- **Assignation** : Donner une valeur a une variable

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Les operateurs sont comme les outils d'un artisan : tu as besoin du bon outil pour chaque tache. En programmation, tu utilises :
- Les operateurs arithmetiques pour calculer
- Les operateurs de comparaison pour comparer
- Les operateurs logiques pour combiner des conditions
- Les operateurs d'assignation pour donner des valeurs

C'est grace aux operateurs que ton programme peut faire des maths, prendre des decisions intelligentes, et manipuler les donnees !

### Definition et concepts cles

**Types d'operateurs principaux :**

1. **Operateurs arithmetiques** - Pour les calculs
   - `+` addition, `-` soustraction, `*` multiplication
   - `/` division, `%` modulo (reste), `**` puissance

2. **Operateurs de comparaison** - Pour comparer
   - `==` egal, `!=` different, `===` strictement egal
   - `>` superieur, `<` inferieur, `>=` sup/egal, `<=` inf/egal

3. **Operateurs logiques** - Pour combiner des conditions
   - `&&` ET (and), `||` OU (or), `!` NON (not)

4. **Operateurs d'assignation** - Pour assigner des valeurs
   - `=` assigner, `+=` ajouter et assigner, `-=`, `*=`, `/=`

5. **Operateurs d'increment/decrement**
   - `++` ajouter 1, `--` enlever 1

### Syntaxe de base
```javascript
// Operateurs arithmetiques
let a = 10;
let b = 3;
console.log(a + b);    // 13 (addition)
console.log(a - b);    // 7 (soustraction)
console.log(a * b);    // 30 (multiplication)
console.log(a / b);    // 3.333... (division)
console.log(a % b);    // 1 (reste de 10/3)

// Operateurs de comparaison
console.log(a > b);    // true
console.log(a == b);   // false

// Operateurs logiques
let majeur = (age >= 18);
let permis = true;
console.log(majeur && permis);  // true si les deux sont vrais
```

### Points d'attention
- **Piege == vs ===** : == convertit les types, === ne le fait pas
- **Piege precedence** : * et / avant + et -, utilise des parentheses pour clarifier
- **Bonne pratique** : Utilise === plutot que == pour eviter les surprises

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```javascript
// Calculs de base
let note1 = 15;
let note2 = 12;
let note3 = 18;

// Operateurs arithmetiques
let total = note1 + note2 + note3;
let moyenne = total / 3;

console.log("Note 1: " + note1);
console.log("Note 2: " + note2);
console.log("Note 3: " + note3);
console.log("Total: " + total);
console.log("Moyenne: " + moyenne);

// Operateurs de comparaison
if (moyenne >= 16) {
    console.log("Excellent !");
} else if (moyenne >= 12) {
    console.log("Bien !");
} else {
    console.log("Peut mieux faire");
}

// Operateurs d'assignation
let score = 0;
score += 10;    // Meme chose que score = score + 10
score *= 2;     // Meme chose que score = score * 2
console.log("Score final: " + score);  // 20
```

**Resultat attendu :**
```
Note 1: 15
Note 2: 12
Note 3: 18
Total: 45
Moyenne: 15
Bien !
Score final: 20
```

### Exemple intermediaire
```javascript
// Calculatrice avancee avec tous les operateurs
let x = 17;
let y = 5;

console.log("=== CALCULATRICE COMPLETE ===");
console.log("x = " + x + ", y = " + y);

// Operateurs arithmetiques
console.log("Addition: " + x + " + " + y + " = " + (x + y));
console.log("Soustraction: " + x + " - " + y + " = " + (x - y));
console.log("Multiplication: " + x + " * " + y + " = " + (x * y));
console.log("Division: " + x + " / " + y + " = " + (x / y));
console.log("Modulo: " + x + " % " + y + " = " + (x % y));
console.log("Puissance: " + x + " ** 2 = " + (x ** 2));

// Operateurs de comparaison
console.log("\n=== COMPARAISONS ===");
console.log(x + " > " + y + " : " + (x > y));
console.log(x + " < " + y + " : " + (x < y));
console.log(x + " >= " + y + " : " + (x >= y));
console.log(x + " <= " + y + " : " + (x <= y));
console.log(x + " == " + y + " : " + (x == y));
console.log(x + " != " + y + " : " + (x != y));

// Operateurs logiques
let estPair = (x % 2 == 0);
let estPositif = (x > 0);
console.log("\n=== LOGIQUE ===");
console.log(x + " est pair: " + estPair);
console.log(x + " est positif: " + estPositif);
console.log("Pair ET positif: " + (estPair && estPositif));
console.log("Pair OU positif: " + (estPair || estPositif));
console.log("NOT pair: " + (!estPair));
```

### Exemple avance (optionnel)
```javascript
// Jeu : Devine le nombre avec operateurs complexes
let nombreSecret = 42;
let tentativeUtilisateur = 35;

console.log("=== JEU : DEVINE LE NOMBRE ===");
console.log("Nombre secret: ???");
console.log("Ta tentative: " + tentativeUtilisateur);

// Calcul de la difference
let difference = Math.abs(nombreSecret - tentativeUtilisateur);
let pourcentageProche = ((10 - difference) / 10) * 100;

// Operateurs complexes pour determiner les indices
let treProche = (difference <= 2);
let proche = (difference <= 5) && !treProche;
let moyen = (difference <= 10) && !proche && !treProche;
let loin = !treProche && !proche && !moyen;

console.log("Difference: " + difference);

// Utilisation des operateurs logiques pour les indices
if (tentativeUtilisateur === nombreSecret) {
    console.log("BRAVO ! Tu as trouve !");
} else if (treProche) {
    console.log("TRES CHAUD ! Tu es tout pres !");
} else if (proche) {
    console.log("Chaud ! Tu approches !");
} else if (moyen) {
    console.log("Tiede... Continue a chercher");
} else {
    console.log("Froid ! Tu es loin du compte");
}

// Operateurs ternaires (condition en une ligne)
let direction = (tentativeUtilisateur < nombreSecret) ? "plus haut" : "plus bas";
console.log("Indice: Cherche " + direction);

// Increment/decrement
let compteurTentatives = 1;
compteurTentatives++;  // Devient 2
console.log("Tentative numero: " + compteurTentatives);
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Calculatrice personnelle
**Consigne :** Cree une calculatrice qui calcule ton age en jours, heures, minutes

**Code de depart :**
```javascript
let monAge = 14;  // En annees

// Calculate en utilisant les operateurs arithmetiques :
// - Age en jours (365 jours par an)
// - Age en heures (24 heures par jour)  
// - Age en minutes (60 minutes par heure)
// - Age en secondes (60 secondes par minute)
```

**Niveau de difficulte :** 2 etoiles sur 5

### Exercice 2 : Validateur de mot de passe
**Consigne :** Verifie si un mot de passe respecte les criteres

**Contraintes a verifier :**
- Au moins 8 caracteres
- Contient au moins un chiffre
- Contient au moins une majuscule
- Utilise les operateurs logiques (&&, ||, !)

**Niveau de difficulte :** 4 etoiles sur 5

### Exercice 3 : Systeme de points de jeu
**Consigne :** Cree un systeme qui gere les points d'un joueur

**Fonctionnalites :**
- Points de base
- Bonus selon le niveau
- Malus si erreurs
- Utilise les operateurs d'assignation (+=, -=, *=)

**Niveau de difficulte :** 3 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```javascript
let monAge = 14;  // En annees

console.log("=== CALCULATRICE D'AGE ===");
console.log("Mon age: " + monAge + " ans");

// Calculs avec operateurs arithmetiques
let ageEnJours = monAge * 365;
let ageEnHeures = ageEnJours * 24;
let ageEnMinutes = ageEnHeures * 60;
let ageEnSecondes = ageEnMinutes * 60;

// Affichage des resultats
console.log("En jours: " + ageEnJours + " jours");
console.log("En heures: " + ageEnHeures + " heures");
console.log("En minutes: " + ageEnMinutes + " minutes");
console.log("En secondes: " + ageEnSecondes + " secondes");

// Calculs supplementaires avec modulo
let anneesCompletes = Math.floor(ageEnJours / 365);
let joursRestants = ageEnJours % 365;

console.log("\n=== VERIFICATION ===");
console.log("Annees completes: " + anneesCompletes);
console.log("Jours restants: " + joursRestants);

// Comparaisons interessantes
let unMillionSecondes = 1000000;
if (ageEnSecondes > unMillionSecondes) {
    console.log("Tu as vecu plus d'un million de secondes !");
} else {
    let reste = unMillionSecondes - ageEnSecondes;
    console.log("Il te reste " + reste + " secondes avant le million !");
}
```

**Explication :** Les operateurs arithmetiques permettent de faire des conversions d'unites facilement.

### Solution Exercice 2
```javascript
// Simulation d'un mot de passe a tester
let motDePasse = "MonMotDePasse123";

console.log("=== VALIDATEUR DE MOT DE PASSE ===");
console.log("Mot de passe a tester: " + motDePasse);

// Criteres a verifier avec operateurs de comparaison
let assezLong = (motDePasse.length >= 8);
let contientChiffre = /[0-9]/.test(motDePasse);  // Expression reguliere simple
let contientMajuscule = /[A-Z]/.test(motDePasse);
let contientMinuscule = /[a-z]/.test(motDePasse);

// Affichage des verifications individuelles
console.log("\n=== CRITERES ===");
console.log("Au moins 8 caracteres: " + assezLong);
console.log("Contient un chiffre: " + contientChiffre);
console.log("Contient une majuscule: " + contientMajuscule);
console.log("Contient une minuscule: " + contientMinuscule);

// Operateurs logiques pour la validation finale
let motDePasseValide = assezLong && contientChiffre && contientMajuscule && contientMinuscule;

console.log("\n=== RESULTAT ===");
if (motDePasseValide) {
    console.log("VALIDE ! Ton mot de passe est securise !");
} else {
    console.log("INVALIDE ! Il manque des criteres :");
    
    // Operateur logique NOT pour montrer ce qui manque
    if (!assezLong) console.log("- Il faut au moins 8 caracteres");
    if (!contientChiffre) console.log("- Il faut au moins un chiffre");
    if (!contientMajuscule) console.log("- Il faut au moins une majuscule");
    if (!contientMinuscule) console.log("- Il faut au moins une minuscule");
}

// Bonus: niveau de securite avec operateurs ternaires
let niveauSecurite = motDePasseValide ? "Fort" : "Faible";
console.log("Niveau de securite: " + niveauSecurite);
```

**Alternatives possibles :**
- Ajouter d'autres criteres (caracteres speciaux, longueur minimum plus elevee)
- Creer un score de securite avec points
- Verifier contre une liste de mots de passe courants

### Solution Exercice 3
```javascript
// Systeme de points de jeu
let joueur = "Alex";
let niveau = 3;
let erreurs = 2;

console.log("=== SYSTEME DE POINTS ===");
console.log("Joueur: " + joueur);
console.log("Niveau: " + niveau);
console.log("Erreurs: " + erreurs);

// Points de base
let points = 100;
console.log("Points de base: " + points);

// Bonus selon le niveau avec operateurs d'assignation
points += niveau * 50;  // Equivalent a: points = points + (niveau * 50)
console.log("Apres bonus niveau: " + points);

// Malus pour les erreurs
points -= erreurs * 25;  // Equivalent a: points = points - (erreurs * 25)
console.log("Apres malus erreurs: " + points);

// Bonus de performance si peu d'erreurs
if (erreurs <= 1) {
    points *= 1.5;  // Equivalent a: points = points * 1.5
    console.log("BONUS PERFORMANCE ! Points multiplies: " + points);
}

// Arrondir le score final
points = Math.round(points);

// Determination du rang avec operateurs de comparaison
let rang;
if (points >= 200) {
    rang = "LEGEND";
} else if (points >= 150) {
    rang = "EXPERT";
} else if (points >= 100) {
    rang = "AVANCE";
} else if (points >= 50) {
    rang = "NOVICE";
} else {
    rang = "DEBUTANT";
}

console.log("\n=== RESULTAT FINAL ===");
console.log("Score final: " + points + " points");
console.log("Rang: " + rang);

// Statistiques avec operateurs arithmetiques
let moyenneParNiveau = points / niveau;
let efficacite = (points / (100 + erreurs * 10)) * 100;

console.log("Moyenne par niveau: " + Math.round(moyenneParNiveau));
console.log("Efficacite: " + Math.round(efficacite) + "%");
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Variables** : Pour stocker les operandes et resultats
- **Types de donnees** : Pour savoir quels operateurs utiliser
- **Conditions** : Avec les operateurs de comparaison et logiques
- **Boucles** : Avec les operateurs d'increment (i++)

### Prochaines etapes  
- **Fonctions** : Organiser les calculs complexes en fonctions
- **Arrays** : Utiliser les operateurs pour parcourir et manipuler les tableaux
- **Objets** : Acces aux proprietes avec des operateurs speciaux
- **Expressions regulieres** : Operateurs avances pour le texte

### Concepts complementaires
- **Precedence des operateurs** : Ordre d'evaluation dans les expressions complexes
- **Operateurs bit a bit** : Manipulation au niveau des bits (niveau avance)

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer un calculateur de notes et moyenne complete

**Fonctionnalites requises :**
- [ ] Saisie de plusieurs notes
- [ ] Calcul de moyenne avec operateurs arithmetiques
- [ ] Comparaisons pour determiner les mentions
- [ ] Bonus/malus selon les criteres
- [ ] Statistiques avancees (ecart, pourcentages)

**Operateurs a utiliser :**
- Arithmetiques : +, -, *, /, %
- Comparaison : ==, !=, >, <, >=, <=
- Logiques : &&, ||, !
- Assignation : +=, -=, *=, /=

**Structure de fichiers suggeree :**
```
calculateur-notes/
├── index.html
├── style.css  
└── script.js
```

**Temps estime :** 60 minutes

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - Operateurs](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Expressions_and_Operators)
- [MDN - Operateurs arithmetiques](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators#operateurs_arithmetiques)
- [MDN - Operateurs de comparaison](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators#operateurs_de_comparaison)
- [MDN - Operateurs logiques](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators#operateurs_logiques)

### Videos recommandees
- "Tous les operateurs JavaScript" - Grafikart (35min)
- "Precedence des operateurs" - comprendre l'ordre d'evaluation

### Articles approfondis
- "== vs === en JavaScript" - differences importantes pour debutants
- "Operateurs d'assignation avances" - techniques pour optimiser le code

### Outils de pratique
- [CodePen - Operateurs playground](https://codepen.io) - tester toutes les combinaisons
- [JSFiddle](https://jsfiddle.net) - experimenter avec des expressions complexes

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre = et == ?
   - Reponse : = assigne une valeur, == compare deux valeurs

2. **Question pratique :** Que donne cette expression ?
   ```javascript
   let result = 10 + 5 * 2;
   console.log(result);
   ```
   - Reponse : 20 (multiplication avant addition : 10 + (5*2))

3. **Question d'application :** Comment verifierais-tu qu'un nombre est pair ET positif ?
   - Reponse : (nombre % 2 === 0) && (nombre > 0)

### Checklist de maitrise
- [ ] Je sais utiliser +, -, *, /, % correctement
- [ ] Je comprends la difference entre == et ===
- [ ] Je peux utiliser &&, ||, ! dans des conditions
- [ ] Je sais utiliser +=, -=, *=, /= pour simplifier
- [ ] Je comprends la precedence des operateurs
- [ ] Je peux combiner plusieurs operateurs dans une expression

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
let a = "5";
let b = 3;
let result = a + b;    // "53" au lieu de 8
```

**Solution :**
```javascript
let a = "5";
let b = 3;
let result = Number(a) + b;    // 8 (conversion explicite)
// ou
let result = +a + b;           // 8 (conversion avec +)
```

**Explication :** L'operateur + colle les strings. Convertis d'abord en number.

### Erreur frequente 2
**Code problematique :**
```javascript
if (age = 18) {        // = au lieu de ==
    console.log("Majeur");
}
```

**Solution :**
```javascript
if (age === 18) {      // === pour comparer
    console.log("Majeur");
}
```

**Explication :** = assigne, === compare. Utilise toujours === pour les comparaisons.

### Erreur frequente 3
**Code problematique :**
```javascript
let x = 10;
let y = 5;
console.log(x + y * 2);    // Donne 20, pas 30
```

**Solution :**
```javascript
let x = 10;
let y = 5;
console.log((x + y) * 2);  // 30 avec parentheses
```

**Explication :** * a priorite sur +. Utilise des parentheses pour changer l'ordre.

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Les operateurs sont les outils pour manipuler les donnees]
- [Il faut faire attention a la precedence et aux types]  
- [=== est plus sur que == pour les comparaisons]

### Questions a approfondir
- [Comment marchent les operateurs bit a bit ?]
- [Quels sont les operateurs avances pour les objets ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices d'operateurs)
- [ ] Revision dans 1 semaine (creer un projet utilisant tous les types)
- [ ] Revision dans 1 mois (expliquer la precedence a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript operateurs arithmetiques comparaison logiques assignation precedence calculs

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 6/126*
