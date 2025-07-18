# Opérateurs Logiques JavaScript

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** Opérateurs Logiques (&&, ||, !)
- **Niveau :** Débutant
- **Durée estimée :** 60 minutes
- **Prérequis :** Comparaisons et égalité, types de données
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Les trois opérateurs logiques ET, OU, NON
- [ ] **Appliquer** : Combiner plusieurs conditions dans une expression
- [ ] **Analyser** : Comprendre l'évaluation court-circuit (short-circuit)
- [ ] **Créer** : Construire des conditions complexes pour vos programmes

### Mots-clés à retenir
- **Opérateur ET** : && (retourne true si les deux conditions sont vraies)
- **Opérateur OU** : || (retourne true si au moins une condition est vraie)
- **Opérateur NON** : ! (inverse le résultat booléen)
- **Court-circuit** : Arrêt de l'évaluation dès que le résultat est déterminé
- **Truthy/Falsy** : Valeurs considérées comme vraies ou fausses

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les opérateurs logiques permettent de créer des conditions complexes en combinant plusieurs expressions. C'est essentiel pour :
- Vérifier plusieurs critères simultanément (âge ET pays)
- Proposer des alternatives (carte de crédit OU PayPal)
- Inverser une condition (utilisateur NON connecté)
- Optimiser les performances avec le court-circuit

**Exemple concret :** Pour accéder à un contenu adulte, il faut être majeur ET avoir accepté les conditions.

### Définition et concepts clés

**1. Opérateur ET (&&) :**
```javascript
// Retourne true seulement si TOUTES les conditions sont vraies
const age = 20;
const aAccepteConditions = true;

console.log(age >= 18 && aAccepteConditions);  // true
console.log(age >= 18 && false);               // false
```

**2. Opérateur OU (||) :**
```javascript
// Retourne true si AU MOINS UNE condition est vraie
const estWeekend = false;
const estVacances = true;

console.log(estWeekend || estVacances);  // true
console.log(false || false);            // false
```

**3. Opérateur NON (!) :**
```javascript
// Inverse le résultat booléen
const estConnecte = false;

console.log(!estConnecte);  // true (non-connecté)
console.log(!true);         // false
console.log(!!estConnecte); // false (double négation = valeur originale)
```

### Syntaxe de base
```javascript
// Combinaisons de base
if (condition1 && condition2) {
    // Exécute si TOUTES les conditions sont vraies
}

if (condition1 || condition2) {
    // Exécute si AU MOINS UNE condition est vraie
}

if (!condition) {
    // Exécute si la condition est FAUSSE
}

// Conditions complexes
if ((age >= 18 && pays === "France") || estAdmin) {
    // Accès autorisé
}
```

### Points d'attention
- **Court-circuit ET** : Si la première condition est false, la seconde n'est pas évaluée
- **Court-circuit OU** : Si la première condition est true, la seconde n'est pas évaluée
- **Priorité** : ! puis && puis ||, utiliser des parenthèses pour clarifier
- **Valeurs falsy** : false, 0, "", null, undefined, NaN

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Vérification d'accès simple
const age = 16;
const aPermisParents = true;
const estMajeur = age >= 18;

// Peut regarder le film si majeur OU a permission des parents
const peutRegarder = estMajeur || aPermisParents;
console.log("Peut regarder le film:", peutRegarder);  // true

// Peut acheter seul si majeur ET pas de restriction
const aRestriction = false;
const peutAcheterSeul = estMajeur && !aRestriction;
console.log("Peut acheter seul:", peutAcheterSeul);   // false

// Test complet
if (peutRegarder) {
    console.log("✅ Accès autorisé au film");
} else {
    console.log("❌ Accès refusé");
}
```

**Résultat attendu :**
```
Peut regarder le film: true
Peut acheter seul: false
✅ Accès autorisé au film
```

### Exemple intermédiaire
```javascript
// Système de validation de connexion
function verifierConnexion(email, motDePasse, captcha) {
    const emailValide = email.includes("@") && email.length > 5;
    const motDePasseForte = motDePasse.length >= 8;
    const captchaValide = captcha === "robot";
    
    console.log("=== VÉRIFICATION CONNEXION ===");
    console.log("Email valide:", emailValide);
    console.log("Mot de passe fort:", motDePasseForte);
    console.log("Captcha valide:", captchaValide);
    
    // Toutes les conditions doivent être vraies
    const connexionAutorisee = emailValide && motDePasseForte && captchaValide;
    
    if (connexionAutorisee) {
        console.log("🎉 Connexion réussie !");
    } else {
        console.log("❌ Échec de connexion");
        
        // Messages d'erreur spécifiques
        if (!emailValide) {
            console.log("- Email invalide");
        }
        if (!motDePasseForte) {
            console.log("- Mot de passe trop faible");
        }
        if (!captchaValide) {
            console.log("- Captcha incorrect");
        }
    }
    
    return connexionAutorisee;
}

// Tests
verifierConnexion("user@test.com", "password123", "robot");     // Succès
verifierConnexion("mauvais-email", "123", "humain");           // Échec
```

### Exemple avancé (court-circuit)
```javascript
// Démonstration du court-circuit et optimisation
let compteur = 0;

function incrementer() {
    compteur++;
    console.log(`Fonction appelée, compteur: ${compteur}`);
    return true;
}

console.log("=== COURT-CIRCUIT ET (&&) ===");
compteur = 0;
console.log("Résultat:", false && incrementer());  // incrementer() n'est PAS appelée
console.log("Compteur final:", compteur);          // 0

console.log("\n=== COURT-CIRCUIT OU (||) ===");
compteur = 0;
console.log("Résultat:", true || incrementer());   // incrementer() n'est PAS appelée
console.log("Compteur final:", compteur);          // 0

console.log("\n=== SANS COURT-CIRCUIT ===");
compteur = 0;
console.log("Résultat:", false && incrementer());  // Première condition false
console.log("Puis:", true && incrementer());       // incrementer() EST appelée
console.log("Compteur final:", compteur);          // 1

// Utilisation pratique du court-circuit
function obtenirParametre(config) {
    // Valeur par défaut si config n'existe pas
    const theme = config && config.theme || "clair";
    const langue = config && config.langue || "fr";
    
    return { theme, langue };
}

console.log("\n=== VALEURS PAR DÉFAUT ===");
console.log(obtenirParametre(null));                           // { theme: "clair", langue: "fr" }
console.log(obtenirParametre({ theme: "sombre" }));            // { theme: "sombre", langue: "fr" }
console.log(obtenirParametre({ theme: "sombre", langue: "en" })); // { theme: "sombre", langue: "en" }
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Conditions d'admission
**Consigne :** Créez un système qui vérifie si un étudiant peut s'inscrire à un cours.

**Critères :**
- Âge entre 16 et 25 ans OU être diplômé
- Avoir payé les frais OU avoir une bourse
- Ne pas être exclu

**Code de départ :**
```javascript
function peutSinscrire(age, estDiplome, aPayeFrais, aBourse, estExclu) {
    // Compléter ici
    
    return /* votre condition */;
}

// Tests
console.log(peutSinscrire(20, false, true, false, false));   // true
console.log(peutSinscrire(30, true, false, true, false));    // true  
console.log(peutSinscrire(20, false, false, false, true));   // false
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Validation de formulaire
**Consigne :** Créez un validateur de formulaire d'inscription avec messages d'erreur détaillés.

**Contraintes :**
- Nom : minimum 2 caractères
- Email : contient @ et .
- Mot de passe : minimum 8 caractères avec au moins 1 chiffre
- Confirmation : identique au mot de passe
- Conditions acceptées

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Système de recommandation (optionnel)
**Consigne :** Créez un système qui recommande des films selon les préférences.

**Critères complexes :**
- Âge approprié ET (genre préféré OU acteur favori) ET durée acceptable
- Gestion des exceptions pour les classiques

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function peutSinscrire(age, estDiplome, aPayeFrais, aBourse, estExclu) {
    // Condition d'âge OU diplôme
    const conditionAge = (age >= 16 && age <= 25) || estDiplome;
    
    // Condition financière
    const conditionFinanciere = aPayeFrais || aBourse;
    
    // Pas d'exclusion
    const pasExclu = !estExclu;
    
    // Toutes les conditions doivent être remplies
    const peutSinscrire = conditionAge && conditionFinanciere && pasExclu;
    
    console.log(`Âge/Diplôme: ${conditionAge}, Finance: ${conditionFinanciere}, Pas exclu: ${pasExclu}`);
    
    return peutSinscrire;
}

// Tests avec explications
console.log("Test 1:", peutSinscrire(20, false, true, false, false));   // true - âge OK, a payé, pas exclu
console.log("Test 2:", peutSinscrire(30, true, false, true, false));    // true - diplômé, bourse, pas exclu  
console.log("Test 3:", peutSinscrire(20, false, false, false, true));   // false - exclu
console.log("Test 4:", peutSinscrire(30, false, false, false, false));  // false - trop vieux et pas diplômé
```

### Solution Exercice 2
```javascript
function validerFormulaire(nom, email, motDePasse, confirmation, conditionsAcceptees) {
    // Validation nom
    const nomValide = nom && nom.length >= 2;
    
    // Validation email
    const emailValide = email && email.includes("@") && email.includes(".");
    
    // Validation mot de passe (8 chars + 1 chiffre)
    const motDePasseValide = motDePasse && 
                             motDePasse.length >= 8 && 
                             /\d/.test(motDePasse);  // Contient au moins un chiffre
    
    // Confirmation identique
    const confirmationValide = confirmation === motDePasse;
    
    // Conditions acceptées
    const conditionsOK = conditionsAcceptees === true;
    
    // Résultat global
    const formulaireValide = nomValide && 
                            emailValide && 
                            motDePasseValide && 
                            confirmationValide && 
                            conditionsOK;
    
    // Messages d'erreur détaillés
    console.log("=== VALIDATION FORMULAIRE ===");
    if (!nomValide) console.log("❌ Nom trop court (minimum 2 caractères)");
    if (!emailValide) console.log("❌ Email invalide (doit contenir @ et .)");
    if (!motDePasseValide) console.log("❌ Mot de passe faible (8 chars + 1 chiffre)");
    if (!confirmationValide) console.log("❌ Confirmation ne correspond pas");
    if (!conditionsOK) console.log("❌ Conditions non acceptées");
    
    if (formulaireValide) {
        console.log("✅ Formulaire valide !");
    } else {
        console.log("❌ Formulaire invalide");
    }
    
    return {
        valide: formulaireValide,
        erreurs: {
            nom: !nomValide,
            email: !emailValide,
            motDePasse: !motDePasseValide,
            confirmation: !confirmationValide,
            conditions: !conditionsOK
        }
    };
}

// Tests
validerFormulaire("Alice", "alice@test.com", "motdepasse123", "motdepasse123", true);
validerFormulaire("A", "email-invalide", "123", "456", false);
```

### Solution Exercice 3
```javascript
function recommanderFilm(film, utilisateur) {
    const {
        ageFilm, genre, acteurs, duree, estClassique,
        ageUtilisateur, genresPreferés, acteursFavoris, dureeMaximale
    } = { ...film, ...utilisateur };
    
    // Vérification âge approprié
    const ageAproprie = ageUtilisateur >= ageFilm;
    
    // Genre préféré OU acteur favori
    const correspondPreferences = genresPreferés.includes(genre) || 
                                 acteurs.some(acteur => acteursFavoris.includes(acteur));
    
    // Durée acceptable
    const dureeAcceptable = duree <= dureeMaximale;
    
    // Condition normale
    const recommandationNormale = ageAproprie && correspondPreferences && dureeAcceptable;
    
    // Exception pour les classiques (plus de tolérance sur la durée)
    const exceptionClassique = estClassique && ageAproprie && correspondPreferences;
    
    // Recommandation finale
    const recommande = recommandationNormale || exceptionClassique;
    
    console.log(`Film: ${film.titre}`);
    console.log(`Âge approprié: ${ageAproprie}, Préférences: ${correspondPreferences}, Durée OK: ${dureeAcceptable}`);
    console.log(`Classique: ${estClassique}, Recommandé: ${recommande}`);
    
    return recommande;
}

// Test avec données
const utilisateur = {
    ageUtilisateur: 16,
    genresPreferés: ["action", "science-fiction"],
    acteursFavoris: ["Ryan Gosling"],
    dureeMaximale: 120
};

const films = [
    { titre: "Blade Runner 2049", ageFilm: 13, genre: "science-fiction", 
      acteurs: ["Ryan Gosling"], duree: 164, estClassique: true },
    { titre: "Film d'horreur", ageFilm: 18, genre: "horreur", 
      acteurs: ["Acteur inconnu"], duree: 90, estClassique: false }
];

films.forEach(film => recommanderFilm(film, utilisateur));
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Comparaisons et égalité** : Base des conditions logiques
- **Types de données** : Comprendre truthy/falsy
- **Variables** : Stocker les résultats d'expressions

### Prochaines étapes
- **Structures de contrôle** : Utiliser ces opérateurs dans if/else
- **Fonctions** : Combiner logique complexe dans des fonctions
- **Boucles** : Conditions d'arrêt avec opérateurs logiques

### Concepts complémentaires
- **Opérateur ternaire** : Forme condensée de if/else
- **Nullish coalescing** : ?? pour les valeurs null/undefined

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un système de contrôle d'accès intelligent

**Fonctionnalités requises :**
- [ ] Vérification d'identité (âge, statut, badges)
- [ ] Gestion des horaires (ouverture, fermeture, pauses)
- [ ] Exceptions spéciales (VIP, maintenance, urgences)
- [ ] Journal d'accès avec raisons de refus

**Structure suggérée :**
```
📁 controle-acces/
├── 📄 index.html
├── 📄 script.js
├── 📄 style.css
└── 📄 data.js (utilisateurs et règles)
```

**Temps estimé :** 60 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Opérateurs logiques](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/Logical_Operators)
- [MDN - Truthy et Falsy](https://developer.mozilla.org/fr/docs/Glossary/Truthy)

### Vidéos recommandées
- "JavaScript Logical Operators" - Programming with Mosh (18 min)
- Excellent pour comprendre le court-circuit avec exemples visuels

### Articles approfondis
- "Short-Circuit Evaluation in JavaScript" - JavaScript.info
- Techniques avancées d'optimisation avec les opérateurs logiques

### Outils de pratique
- [Logical Operators Playground](https://codepen.io/search/pens?q=javascript%20logical%20operators)
- Exercices interactifs pour maîtriser les combinaisons

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre && et || ?
   - Réponse : && exige toutes les conditions vraies, || nécessite au moins une condition vraie

2. **Question pratique :** Que fait ce code ?
   ```javascript
   const x = null;
   const y = x && x.propriete;
   ```
   - Réponse : Retourne null sans erreur grâce au court-circuit (x est falsy)

3. **Question d'application :** Quand utiliser ! vs === false ?
   - Réponse : ! pour inverser n'importe quelle valeur, === false pour comparer strictement à false

### Checklist de maîtrise
- [ ] Je comprends les trois opérateurs &&, ||, !
- [ ] Je sais combiner plusieurs conditions
- [ ] Je comprends le mécanisme de court-circuit
- [ ] Je reconnais les valeurs truthy/falsy
- [ ] Je peux créer des conditions complexes lisibles
- [ ] Je sais optimiser avec le court-circuit

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
const age = 18;
if (age >= 18 && < 65) {  // Syntaxe incorrecte
    console.log("Adulte");
}
```

**Message d'erreur :** `SyntaxError: Unexpected token '<'`

**Solution :**
```javascript
const age = 18;
if (age >= 18 && age < 65) {  // Répéter la variable
    console.log("Adulte");
}
```

**Explication :** Chaque condition doit être complète et séparée.

### Erreur fréquente 2
**Code problématique :**
```javascript
const utilisateur = null;
if (utilisateur && utilisateur.nom === "Admin") {  // OK
    console.log("Admin connecté");
}

// Mais attention à ceci :
if (utilisateur.nom && utilisateur.nom === "Admin") {  // ERREUR !
    console.log("Admin connecté");
}
```

**Message d'erreur :** `TypeError: Cannot read property 'nom' of null`

**Solution :**
```javascript
// Toujours vérifier l'objet avant ses propriétés
if (utilisateur && utilisateur.nom && utilisateur.nom === "Admin") {
    console.log("Admin connecté");
}

// Ou utiliser l'optional chaining (moderne)
if (utilisateur?.nom === "Admin") {
    console.log("Admin connecté");
}
```

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les trois opérateurs logiques fondamentaux
- Le mécanisme de court-circuit et ses avantages
- Comment combiner des conditions complexes

### Questions à approfondir
- Opérateurs logiques avec valeurs non-booléennes
- Techniques d'optimisation avancées

### Révision programmée
- [ ] Révision dans 1 jour (opérateurs de base)
- [ ] Révision dans 1 semaine (court-circuit et optimisation)
- [ ] Révision dans 1 mois (conditions complexes en projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #operateurs #logique #conditions #ET #OU #NON #courtcircuit

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 6/126*
