# Premiers Algorithmes Simples en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnees du cours
- **Titre :** Premiers Algorithmes Simples (Logique et Resolution de Problemes)
- **Niveau :** Debutant (transition vers intermediaire)
- **Duree estimee :** 90 minutes
- **Prerequis :** Variables, types, operateurs, comparaisons, structures de controle
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## **OBJECTIFS PEDAGOGIQUES**

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est un algorithme et comment l'appliquer
- [ ] **Decomposer** : Un probleme complexe en etapes simples
- [ ] **Appliquer** : Tous les concepts debutants ensemble dans des mini-programmes
- [ ] **Creer** : Tes premiers algorithmes pour resoudre des problemes concrets

### Mots-cles a retenir
- **Algorithme** : Suite d'instructions pour resoudre un probleme
- **Logique** : Raisonnement structure pour arriver a une solution
- **Iteration** : Repetition d'actions dans une boucle
- **Condition** : Test qui determine le chemin a suivre
- **Variable de travail** : Variable temporaire pour stocker des calculs

---

## **PARTIE THEORIQUE**

### Pourquoi ce concept est-il important ?
Maintenant que tu connais les briques de base de JavaScript (variables, types, operateurs, conditions, boucles), il est temps de les assembler pour resoudre de vrais problemes !

Un **algorithme**, c'est comme une recette de cuisine :
1. **Ingredients** = Variables et donnees
2. **Etapes** = Instructions dans l'ordre
3. **Techniques** = Boucles, conditions, calculs
4. **Resultat** = Solution au probleme

Cette lecon fait le pont entre "connaitre la syntaxe" et "savoir programmer".

### Definition et concepts cles

**Un algorithme** est une sequence d'instructions claire et precise pour resoudre un probleme. En programmation, nous transformons des problemes en algorithmes, puis en code.

**Etapes pour creer un algorithme :**
1. **Comprendre** le probleme (qu'est-ce qu'on veut ?)
2. **Decomposer** en sous-problemes plus simples
3. **Identifier** les donnees necessaires (variables)
4. **Ecrire** les etapes logiques (pseudocode)
5. **Traduire** en code JavaScript
6. **Tester** avec differents cas

**Types d'algorithmes simples :**
- **Recherche** : Trouver un element dans une liste
- **Tri** : Classer des elements par ordre
- **Calcul** : Somme, moyenne, minimum, maximum
- **Validation** : Verifier des donnees (age, email, etc.)
- **Transformation** : Modifier des donnees

### Syntaxe de base
```javascript
// Template d'algorithme simple
function resoudreProbleme(donnees) {
    // 1. Initialiser les variables de travail
    let resultat = 0;
    let trouve = false;
    
    // 2. Traiter les donnees (boucle souvent)
    for (let i = 0; i < donnees.length; i++) {
        // 3. Appliquer la logique selon les conditions
        if (condition) {
            // Action 1
        } else {
            // Action 2
        }
    }
    
    // 4. Retourner le resultat
    return resultat;
}
```

### Points d'attention
- **Piege complexite** : Commencer simple, ajouter la complexite progressivement
- **Piege boucle infinie** : Toujours verifier que tes boucles s'arretent
- **Bonne pratique** : Decomposer en petites etapes, tester frequemment

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Algorithme 1: Trouver le plus grand nombre dans une liste
function trouverPlusGrand(nombres) {
    // Etape 1: Verifier qu'on a des donnees
    if (nombres.length === 0) {
        return "Liste vide !";
    }
    
    // Etape 2: Commencer avec le premier nombre
    let plusGrand = nombres[0];
    console.log("On commence avec:", plusGrand);
    
    // Etape 3: Comparer avec tous les autres
    for (let i = 1; i < nombres.length; i++) {
        console.log(`Comparaison: ${plusGrand} vs ${nombres[i]}`);
        
        if (nombres[i] > plusGrand) {
            plusGrand = nombres[i];
            console.log(`Nouveau plus grand: ${plusGrand}`);
        }
    }
    
    // Etape 4: Retourner le resultat
    return plusGrand;
}

// Test de l'algorithme
let mesNombres = [3, 8, 1, 15, 4, 9];
let resultat = trouverPlusGrand(mesNombres);
console.log("Le plus grand nombre est:", resultat);
```

**Resultat attendu :**
```
On commence avec: 3
Comparaison: 3 vs 8
Nouveau plus grand: 8
Comparaison: 8 vs 1
Comparaison: 8 vs 15
Nouveau plus grand: 15
Comparaison: 15 vs 4
Comparaison: 15 vs 9
Le plus grand nombre est: 15
```

### Exemple intermediaire
```javascript
// Algorithme 2: Calculer des statistiques sur des notes
function analyserNotes(notes) {
    console.log("=== ANALYSE DES NOTES ===");
    console.log("Notes a analyser:", notes);
    
    // Variables de travail pour nos calculs
    let somme = 0;
    let minimum = notes[0];
    let maximum = notes[0];
    let nombreNotes = notes.length;
    
    // Parcourir toutes les notes une seule fois
    for (let i = 0; i < notes.length; i++) {
        let noteActuelle = notes[i];
        
        // Additionner pour la moyenne
        somme = somme + noteActuelle;
        
        // Chercher le minimum
        if (noteActuelle < minimum) {
            minimum = noteActuelle;
        }
        
        // Chercher le maximum  
        if (noteActuelle > maximum) {
            maximum = noteActuelle;
        }
        
        console.log(`Note ${i+1}: ${noteActuelle} | Somme actuelle: ${somme}`);
    }
    
    // Calculer la moyenne
    let moyenne = somme / nombreNotes;
    
    // Determiner la mention
    let mention;
    if (moyenne >= 16) {
        mention = "Tres bien";
    } else if (moyenne >= 14) {
        mention = "Bien";  
    } else if (moyenne >= 12) {
        mention = "Assez bien";
    } else if (moyenne >= 10) {
        mention = "Passable";
    } else {
        mention = "Insuffisant";
    }
    
    // Afficher les resultats
    console.log("\n=== RESULTATS ===");
    console.log("Nombre de notes:", nombreNotes);
    console.log("Somme totale:", somme);
    console.log("Note minimum:", minimum);
    console.log("Note maximum:", maximum);
    console.log("Moyenne:", moyenne.toFixed(2));
    console.log("Mention:", mention);
    
    return {
        moyenne: moyenne,
        minimum: minimum,
        maximum: maximum,
        mention: mention
    };
}

// Test avec des vraies notes
let notesEleve = [14, 16, 12, 18, 13, 15, 17];
analyserNotes(notesEleve);
```

### Exemple avance (optionnel)
```javascript
// Algorithme 3: Jeu de devinette avec logique complete
function jeuDevinette() {
    console.log("=== JEU DE DEVINETTE ===");
    console.log("Je pense a un nombre entre 1 et 100...");
    
    // Generer le nombre secret
    let nombreSecret = Math.floor(Math.random() * 100) + 1;
    console.log("(Psst... le nombre secret est:", nombreSecret, "mais ne le dis a personne !)");
    
    // Variables du jeu
    let tentatives = 0;
    let maxTentatives = 7;
    let trouve = false;
    
    // Simulation de tentatives (normalement viendraient de l'utilisateur)
    let tentativesSimulees = [50, 75, 63, 69, 66, 68, 67]; // Exemple pour 67
    
    console.log("\nCommence a deviner !");
    
    // Boucle principale du jeu
    while (tentatives < maxTentatives && !trouve) {
        tentatives++;
        let proposition = tentativesSimulees[tentatives - 1]; // Simulation
        
        console.log(`\nTentative ${tentatives}: ${proposition}`);
        
        // Logique de comparaison
        if (proposition === nombreSecret) {
            trouve = true;
            console.log("🎉 BRAVO ! Tu as trouve en " + tentatives + " tentatives !");
        } else if (proposition < nombreSecret) {
            console.log("❌ Trop petit ! Essaie plus grand.");
        } else {
            console.log("❌ Trop grand ! Essaie plus petit.");
        }
        
        // Afficher les tentatives restantes
        if (!trouve && tentatives < maxTentatives) {
            let restantes = maxTentatives - tentatives;
            console.log(`Il te reste ${restantes} tentative(s).`);
        }
    }
    
    // Fin de jeu
    if (!trouve) {
        console.log(`\n💔 Dommage ! Le nombre etait ${nombreSecret}. Ressaie !`);
    }
    
    return {
        gagne: trouve,
        tentatives: tentatives,
        nombreSecret: nombreSecret
    };
}

// Lancer le jeu
jeuDevinette();
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Compteur de voyelles
**Consigne :** Cree un algorithme qui compte les voyelles dans un mot.

**Code de depart :**
```javascript
function compterVoyelles(mot) {
    // TODO: 
    // 1. Definir les voyelles (a, e, i, o, u)
    // 2. Parcourir chaque lettre du mot
    // 3. Compter si c'est une voyelle
    // 4. Retourner le nombre total
}

// Tests
console.log(compterVoyelles("bonjour"));    // Doit donner 3 (o, o, u)
console.log(compterVoyelles("javascript")); // Doit donner 3 (a, a, i)
console.log(compterVoyelles("xyz"));        // Doit donner 0
```

**Resultat attendu :**
```
3
3  
0
```

**Niveau de difficulte :** 2/5

### Exercice 2 : Calculateur de prix avec remise
**Consigne :** Cree un algorithme qui calcule le prix final avec remises selon le montant.

**Contraintes :**
- 0-50€ : pas de remise
- 50-100€ : 5% de remise
- 100-200€ : 10% de remise
- Plus de 200€ : 15% de remise
- Gerer la TVA (20%)

**Niveau de difficulte :** 3/5

### Exercice 3 : Validateur de mot de passe (optionnel)
**Consigne :** Cree un algorithme qui verifie qu'un mot de passe respecte les regles de securite.

**Niveau de difficulte :** 4/5

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function compterVoyelles(mot) {
    // Etape 1: Definir les voyelles (minuscules et majuscules)
    let voyelles = "aeiouAEIOU";
    let compteur = 0;
    
    console.log(`Analyse du mot: "${mot}"`);
    
    // Etape 2: Parcourir chaque caractere du mot
    for (let i = 0; i < mot.length; i++) {
        let lettre = mot[i];
        
        // Etape 3: Verifier si c'est une voyelle
        let estVoyelle = false;
        for (let j = 0; j < voyelles.length; j++) {
            if (lettre === voyelles[j]) {
                estVoyelle = true;
                break; // Sortir de la boucle des voyelles
            }
        }
        
        // Etape 4: Compter si c'est une voyelle
        if (estVoyelle) {
            compteur++;
            console.log(`Position ${i}: "${lettre}" est une voyelle. Total: ${compteur}`);
        } else {
            console.log(`Position ${i}: "${lettre}" n'est pas une voyelle.`);
        }
    }
    
    console.log(`Resultat final: ${compteur} voyelles dans "${mot}"\n`);
    return compteur;
}

// Version plus simple avec includes()
function compterVoyellesSimple(mot) {
    let voyelles = "aeiouAEIOU";
    let compteur = 0;
    
    for (let i = 0; i < mot.length; i++) {
        if (voyelles.includes(mot[i])) {
            compteur++;
        }
    }
    
    return compteur;
}

// Tests
console.log("=== VERSION DETAILLEE ===");
compterVoyelles("bonjour");
compterVoyelles("javascript");
compterVoyelles("xyz");

console.log("=== VERSION SIMPLE ===");
console.log("bonjour:", compterVoyellesSimple("bonjour"));
console.log("javascript:", compterVoyellesSimple("javascript"));
console.log("xyz:", compterVoyellesSimple("xyz"));
```

**Explication :** Cet algorithme montre comment decomposer un probleme : identifier les voyelles, parcourir le mot, comparer chaque lettre, compter les correspondances.

### Solution Exercice 2
```javascript
function calculerPrixFinal(prixInitial, inclureTVA = true) {
    console.log(`=== CALCUL PRIX FINAL ===`);
    console.log(`Prix initial: ${prixInitial}€`);
    
    // Etape 1: Determiner le pourcentage de remise
    let remisePourcentage = 0;
    
    if (prixInitial >= 200) {
        remisePourcentage = 15;
    } else if (prixInitial >= 100) {
        remisePourcentage = 10;
    } else if (prixInitial >= 50) {
        remisePourcentage = 5;
    } else {
        remisePourcentage = 0;
    }
    
    console.log(`Remise applicable: ${remisePourcentage}%`);
    
    // Etape 2: Calculer le montant de la remise
    let montantRemise = (prixInitial * remisePourcentage) / 100;
    console.log(`Montant de la remise: ${montantRemise.toFixed(2)}€`);
    
    // Etape 3: Prix apres remise
    let prixApresRemise = prixInitial - montantRemise;
    console.log(`Prix apres remise: ${prixApresRemise.toFixed(2)}€`);
    
    // Etape 4: Ajouter la TVA si demandee
    let prixFinal = prixApresRemise;
    let montantTVA = 0;
    
    if (inclureTVA) {
        montantTVA = (prixApresRemise * 20) / 100;
        prixFinal = prixApresRemise + montantTVA;
        console.log(`TVA (20%): ${montantTVA.toFixed(2)}€`);
    }
    
    console.log(`Prix final: ${prixFinal.toFixed(2)}€`);
    console.log("");
    
    // Retourner un objet avec tous les details
    return {
        prixInitial: prixInitial,
        remisePourcentage: remisePourcentage,
        montantRemise: montantRemise,
        prixApresRemise: prixApresRemise,
        montantTVA: montantTVA,
        prixFinal: prixFinal
    };
}

// Tests avec differents montants
calculerPrixFinal(30);     // Pas de remise
calculerPrixFinal(75);     // 5% de remise
calculerPrixFinal(150);    // 10% de remise
calculerPrixFinal(250);    // 15% de remise
calculerPrixFinal(100, false); // Sans TVA
```

**Alternatives possibles :**
- Utiliser un tableau d'objets pour les tranches de remise
- Ajouter des remises cumulatives pour les gros montants
- Gerer differents taux de TVA selon les pays

### Solution Exercice 3
```javascript
function validerMotDePasse(motDePasse) {
    console.log(`=== VALIDATION MOT DE PASSE ===`);
    console.log(`Mot de passe a tester: "${motDePasse}"`);
    
    // Variables pour tracker les criteres
    let criteres = {
        longueur: false,
        majuscule: false,
        minuscule: false,
        chiffre: false,
        caractereSpecial: false
    };
    
    let erreurs = [];
    
    // Critere 1: Longueur minimum 8 caracteres
    if (motDePasse.length >= 8) {
        criteres.longueur = true;
        console.log("✅ Longueur OK (8+ caracteres)");
    } else {
        erreurs.push("Doit contenir au moins 8 caracteres");
        console.log("❌ Trop court (minimum 8 caracteres)");
    }
    
    // Caracteres speciaux autorises
    let caracteresSpeciaux = "!@#$%^&*()_+-=[]{}|;:,.<>?";
    
    // Parcourir chaque caractere pour verifier les criteres
    for (let i = 0; i < motDePasse.length; i++) {
        let caractere = motDePasse[i];
        
        // Verifier majuscule (A-Z)
        if (caractere >= 'A' && caractere <= 'Z') {
            criteres.majuscule = true;
        }
        
        // Verifier minuscule (a-z)
        if (caractere >= 'a' && caractere <= 'z') {
            criteres.minuscule = true;
        }
        
        // Verifier chiffre (0-9)
        if (caractere >= '0' && caractere <= '9') {
            criteres.chiffre = true;
        }
        
        // Verifier caractere special
        if (caracteresSpeciaux.includes(caractere)) {
            criteres.caractereSpecial = true;
        }
    }
    
    // Verifier chaque critere et ajouter les erreurs
    if (criteres.majuscule) {
        console.log("✅ Contient au moins une majuscule");
    } else {
        erreurs.push("Doit contenir au moins une majuscule");
        console.log("❌ Manque une majuscule");
    }
    
    if (criteres.minuscule) {
        console.log("✅ Contient au moins une minuscule");
    } else {
        erreurs.push("Doit contenir au moins une minuscule");
        console.log("❌ Manque une minuscule");
    }
    
    if (criteres.chiffre) {
        console.log("✅ Contient au moins un chiffre");
    } else {
        erreurs.push("Doit contenir au moins un chiffre");
        console.log("❌ Manque un chiffre");
    }
    
    if (criteres.caractereSpecial) {
        console.log("✅ Contient au moins un caractere special");
    } else {
        erreurs.push("Doit contenir au moins un caractere special (!@#$%^&*...)");
        console.log("❌ Manque un caractere special");
    }
    
    // Resultat final
    let estValide = erreurs.length === 0;
    
    console.log(`\n=== RESULTAT ===`);
    if (estValide) {
        console.log("🎉 Mot de passe VALIDE !");
    } else {
        console.log("❌ Mot de passe INVALIDE");
        console.log("Problemes a corriger:");
        erreurs.forEach((erreur, index) => {
            console.log(`${index + 1}. ${erreur}`);
        });
    }
    console.log("");
    
    return {
        valide: estValide,
        criteres: criteres,
        erreurs: erreurs
    };
}

// Tests avec differents mots de passe
validerMotDePasse("123");              // Trop court, manque tout
validerMotDePasse("motdepasse");       // Manque maj/chiffre/special
validerMotDePasse("MotDePasse");       // Manque chiffre/special
validerMotDePasse("MotDePasse123");    // Manque special
validerMotDePasse("MotDePasse123!");   // VALIDE
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts prealables utilises
- **Variables** : Stocker donnees et resultats temporaires
- **Types** : Manipuler strings, numbers, booleans
- **Operateurs** : Calculs et comparaisons
- **Conditions** : Prendre des decisions dans l'algorithme
- **Boucles** : Traiter des collections de donnees

### Prochaines etapes
- **Fonctions** : Organiser ces algorithmes en blocs reutilisables
- **Arrays** : Travailler avec des listes plus complexes
- **Objets** : Structurer les donnees de facon plus elegante

### Concepts complementaires
- **Debugging** : Identifier et corriger les erreurs dans les algorithmes
- **Optimisation** : Rendre les algorithmes plus rapides et efficaces

---

## **PROJET MINI**

### Defi pratique
**Objectif :** Creer une calculatrice de notes complete avec interface HTML

**Fonctionnalites requises :**
- [ ] Saisir plusieurs notes via un formulaire
- [ ] Calculer automatiquement moyenne, min, max
- [ ] Determiner la mention selon la moyenne
- [ ] Identifier les notes en dessous de la moyenne
- [ ] Afficher un graphique simple (barres texte)
- [ ] Sauvegarder les resultats dans le navigateur

**Structure suggeree :**
```
📁 calculatrice-notes/
├── 📄 index.html
├── 📄 style.css
├── 📄 calculatrice.js
└── 📄 algorithmes.js
```

**Temps estime :** 120 minutes

---

## **RESSOURCES COMPLEMENTAIRES**

### Documentation officielle
- [MDN - Boucles et iteration](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Loops_and_iteration)
- [Introduction aux algorithmes](https://fr.wikipedia.org/wiki/Algorithme)

### Videos recommandees
- "Apprendre les algorithmes" - Grafikart (30 min)
- Pourquoi cette video est utile : Approche progressive et exemples concrets

### Articles approfondis
- "Thinking in algorithms" - Khan Academy
- Ce que cet article apporte en plus : Methodologie pour decomposer les problemes

### Outils de pratique
- [Codewars - Algorithmes JavaScript](https://www.codewars.com/) - Defis progressifs
- [LeetCode Easy Problems](https://leetcode.com/) - Problemes d'algorithmes

---

## **AUTO-EVALUATION**

### Questions de comprehension
1. **Question conceptuelle :** Quelles sont les etapes pour creer un algorithme ?
   - Reponse : Comprendre le probleme, decomposer, identifier les donnees, ecrire les etapes, traduire en code, tester

2. **Question pratique :** Comment trouverais-tu le deuxieme plus grand nombre dans une liste ?
   - Reponse : Parcourir la liste en gardant track du plus grand ET du deuxieme plus grand

3. **Question d'application :** Quand utiliser une boucle while plutot qu'une boucle for ?
   - Reponse : Quand on ne connait pas a l'avance le nombre d'iterations (ex: deviner un nombre)

### Checklist de maitrise
- [ ] Je peux decomposer un probleme en etapes simples
- [ ] Je combine variables, conditions et boucles naturellement
- [ ] Je teste mes algorithmes avec differents cas
- [ ] Je comprends la logique avant d'ecrire le code
- [ ] Je peux expliquer mes algorithmes a quelqu'un d'autre
- [ ] Je reconnais les patterns algorithmiques courants

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur frequente 1
**Code problematique :**
```javascript
// Boucle infinie - oubli d'incrementer
let i = 0;
while (i < 10) {
    console.log(i);
    // Oubli de i++
}
```

**Message d'erreur :** Page qui freeze (boucle infinie)

**Solution :**
```javascript
let i = 0;
while (i < 10) {
    console.log(i);
    i++; // Incrementer pour eviter la boucle infinie
}
```

**Explication :** Toujours verifier que les conditions de sortie de boucle sont atteintes.

### Erreur frequente 2
**Code problematique :**
```javascript
// Mauvaise initialisation
function trouverMinimum(nombres) {
    let minimum = 0; // ERREUR: et si tous les nombres sont positifs > 0 ?
    for (let nombre of nombres) {
        if (nombre < minimum) {
            minimum = nombre;
        }
    }
    return minimum;
}
```

**Solution :**
```javascript
function trouverMinimum(nombres) {
    let minimum = nombres[0]; // Commencer avec le premier element
    for (let i = 1; i < nombres.length; i++) {
        if (nombres[i] < minimum) {
            minimum = nombres[i];
        }
    }
    return minimum;
}
```

**Explication :** Initialiser avec une valeur du probleme, pas une valeur arbitraire.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Comment transformer un probleme en algorithme
- L'importance de decomposer en etapes simples
- Utiliser tous les concepts debutants ensemble

### Questions a approfondir
- Algorithmes de tri plus complexes
- Optimisation des performances des boucles

### Revision programmee
- [ ] Revision dans 1 jour (rappel rapide)
- [ ] Revision dans 1 semaine (exercices)
- [ ] Revision dans 1 mois (application dans un projet)

---

## **TAGS ET METADONNEES**

**Mots-cles :** #javascript #algorithmes #logique #resolution-problemes #debutant

**Temps de completion :** [Temps reellement passe]

**Difficulte ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Pret pour le concept suivant :** ✅ Oui / ❌ Non (revision necessaire)

---

*📅 Fiche creee le : 17/07/2025*
*🔄 Derniere revision : 17/07/2025*
*📍 Position dans la roadmap : 6.5/126*
