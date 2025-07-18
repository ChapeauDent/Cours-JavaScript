# Opérateur Ternaire

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** L'Opérateur Ternaire (Condition ? vrai : faux)
- **Niveau :** 🟢 Débutant
- **Durée estimée :** 20 minutes
- **Prérequis :** Structures de contrôle (if/else), Comparaisons
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : La syntaxe et le fonctionnement de l'opérateur ternaire
- [ ] **Appliquer** : Remplacer des if/else simples par l'opérateur ternaire
- [ ] **Analyser** : Quand utiliser ternaire vs if/else classique
- [ ] **Créer** : Du code plus concis et lisible avec l'opérateur ternaire

### Mots-clés à retenir
- **Opérateur ternaire** : condition ? valeurSiVrai : valeurSiFaux
- **Expression conditionnelle** : Une condition qui retourne une valeur
- **Concision** : Écrire moins de code pour le même résultat
- **Lisibilité** : Équilibre entre concis et compréhensible

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
L'opérateur ternaire est un raccourci élégant pour les conditions simples ! Il t'aide à :
- Écrire du code plus concis et moderne
- Assigner des valeurs conditionnellement en une ligne
- Améliorer la lisibilité pour les cas simples
- Adopter un style de programmation plus professionnel

### Définition et concepts clés

**L'opérateur ternaire** est le seul opérateur JavaScript qui prend 3 opérandes :
1. **Une condition** (qui sera vraie ou fausse)
2. **Une valeur si vrai** (retournée si condition = true)
3. **Une valeur si faux** (retournée si condition = false)

**Syntaxe :**
```javascript
condition ? valeurSiVrai : valeurSiFaux
```

**Comparaison avec if/else :**
```javascript
// Avec if/else (4 lignes)
let message;
if (age >= 18) {
    message = "Majeur";
} else {
    message = "Mineur";
}

// Avec opérateur ternaire (1 ligne)
let message = age >= 18 ? "Majeur" : "Mineur";
```

### Points d'attention
- **Lisibilité** : Parfait pour les cas simples, évite pour les logiques complexes
- **Imbrication** : Possible mais peut devenir difficile à lire
- **Type de retour** : Toujours retourne une valeur (contrairement à if/else)
- **Précédence** : Attention à l'ordre d'évaluation avec d'autres opérateurs

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Vérification d'âge simple
const age = 16;

// Version if/else
let statut;
if (age >= 18) {
    statut = "Adulte";
} else {
    statut = "Adolescent";
}
console.log("If/else:", statut);

// Version ternaire (équivalente)
const statutTernaire = age >= 18 ? "Adulte" : "Adolescent";
console.log("Ternaire:", statutTernaire);

// Utilisation directe dans console.log
console.log("Tu es", age >= 18 ? "majeur" : "mineur");
```

**Résultat attendu :**
```
If/else: Adolescent
Ternaire: Adolescent
Tu es mineur
```

### Exemple intermédiaire
```javascript
// Système de notation avec plusieurs conditions

const note = 15;

// Version if/else traditionnelle
let appreciation;
if (note >= 16) {
    appreciation = "Très bien";
} else if (note >= 12) {
    appreciation = "Bien";
} else if (note >= 8) {
    appreciation = "Passable";
} else {
    appreciation = "Insuffisant";
}

// Version avec ternaires imbriqués (attention à la lisibilité !)
const appreciationTernaire = note >= 16 ? "Très bien" 
                           : note >= 12 ? "Bien"
                           : note >= 8 ? "Passable"
                           : "Insuffisant";

console.log(`Note: ${note}/20 - ${appreciation}`);
console.log(`Ternaire: ${appreciationTernaire}`);

// Utilisation pour des calculs
const points = 150;
const bonus = points > 100 ? 50 : 0;
const totalPoints = points + bonus;

console.log(`Points: ${points}, Bonus: ${bonus}, Total: ${totalPoints}`);

// Gestion d'affichage conditionnel
const utilisateurs = ["Alice", "Bob", "Charlie"];
const messageUtilisateurs = utilisateurs.length > 0 
    ? `${utilisateurs.length} utilisateur(s) connecté(s)` 
    : "Aucun utilisateur connecté";

console.log(messageUtilisateurs);
```

### Exemple avancé (optionnel)
```javascript
// Utilisation avancée dans différents contextes

// 1. Avec des fonctions
const calculerRemise = (montant, estMembre) => {
    return montant * (estMembre ? 0.9 : 1); // 10% de remise si membre
};

console.log("Prix membre:", calculerRemise(100, true));    // 90
console.log("Prix normal:", calculerRemise(100, false));   // 100

// 2. Avec des objets et propriétés
const utilisateur = {
    nom: "Emma",
    age: 17,
    estPremium: true
};

const droits = {
    peutVoter: utilisateur.age >= 18 ? true : false,
    acces: utilisateur.estPremium ? "premium" : "standard",
    reduction: utilisateur.estPremium ? "20%" : "0%"
};

console.log("Droits:", droits);

// 3. Ternaire avec des expressions complexes
const heure = new Date().getHours();
const salutation = heure < 12 ? "Bonjour" 
                 : heure < 18 ? "Bon après-midi"
                 : "Bonsoir";

const couleurTheme = heure < 6 || heure > 20 ? "dark" : "light";

console.log(`${salutation} ! Thème: ${couleurTheme}`);

// 4. Dans des tableaux (map, filter...)
const nombres = [1, 2, 3, 4, 5];
const paritePairs = nombres.map(n => n % 2 === 0 ? "pair" : "impair");
console.log("Parité:", paritePairs);

// 5. Validation et valeurs par défaut
const saisieUtilisateur = ""; // Peut être vide
const nomAffiche = saisieUtilisateur.length > 0 ? saisieUtilisateur : "Anonyme";
console.log("Nom affiché:", nomAffiche);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Conversion if/else vers ternaire
**Consigne :** Convertis ces structures if/else en opérateur ternaire

**Code de départ :**
```javascript
// 1. Validation de mot de passe
let motDePasse = "12345";
let securite;
if (motDePasse.length >= 8) {
    securite = "Fort";
} else {
    securite = "Faible";
}

// 2. Calcul de prix avec TVA
let prixHT = 100;
let paysProfessionnel = false;
let prixFinal;
if (paysProfessionnel) {
    prixFinal = prixHT;
} else {
    prixFinal = prixHT * 1.20;
}

// 3. Message de bienvenue
let heureActuelle = 14;
let message;
if (heureActuelle < 12) {
    message = "Bonjour !";
} else {
    message = "Bonsoir !";
}
```

**Niveau de difficulté :** ⭐⭐☆☆☆

### Exercice 2 : Système de badges
**Consigne :** Crée un système de badges pour un jeu avec l'opérateur ternaire

**Spécifications :**
- Score < 100 : Badge "Débutant"
- Score 100-499 : Badge "Intermédiaire"  
- Score 500+ : Badge "Expert"
- Bonus si score > 1000 : ajouter "⭐" au badge

**Code de départ :**
```javascript
function obtenirBadge(score) {
    // Utilise uniquement des opérateurs ternaires
    // Retourne le badge approprié
}

// Tests
console.log(obtenirBadge(50));    // "Débutant"
console.log(obtenirBadge(250));   // "Intermédiaire"
console.log(obtenirBadge(750));   // "Expert"
console.log(obtenirBadge(1200));  // "Expert⭐"
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 3 : Validation de formulaire
**Consigne :** Crée une fonction de validation qui utilise plusieurs ternaires

**Contraintes :**
- Valider email (contient @ et .)
- Valider âge (entre 13 et 120)
- Valider nom (au moins 2 caractères)
- Retourner un objet avec status et message

**Niveau de difficulté :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// 1. Validation de mot de passe
let motDePasse = "12345";
let securite = motDePasse.length >= 8 ? "Fort" : "Faible";

// 2. Calcul de prix avec TVA
let prixHT = 100;
let paysProfessionnel = false;
let prixFinal = paysProfessionnel ? prixHT : prixHT * 1.20;

// 3. Message de bienvenue
let heureActuelle = 14;
let message = heureActuelle < 12 ? "Bonjour !" : "Bonsoir !";

console.log("Sécurité:", securite);
console.log("Prix final:", prixFinal);
console.log("Message:", message);
```

**Explication :** Chaque if/else simple se convertit facilement en ternaire, le code devient plus concis.

### Solution Exercice 2
```javascript
function obtenirBadge(score) {
    // Badge de base selon le score
    const badgeBase = score < 100 ? "Débutant"
                    : score < 500 ? "Intermédiaire"
                    : "Expert";
    
    // Ajout du bonus étoile
    const badgeFinal = score > 1000 ? badgeBase + "⭐" : badgeBase;
    
    return badgeFinal;
}

// Version ultra-concise (moins lisible)
const obtenirBadgeConcis = (score) => 
    (score < 100 ? "Débutant" : score < 500 ? "Intermédiaire" : "Expert") + 
    (score > 1000 ? "⭐" : "");

// Tests
console.log(obtenirBadge(50));    // "Débutant"
console.log(obtenirBadge(250));   // "Intermédiaire"
console.log(obtenirBadge(750));   // "Expert"
console.log(obtenirBadge(1200));  // "Expert⭐"
```

**Alternatives possibles :**
- Utiliser un objet pour mapper les scores aux badges
- Créer une fonction séparée pour le bonus étoile

### Solution Exercice 3
```javascript
function validerFormulaire(email, age, nom) {
    // Validations individuelles avec ternaires
    const emailValide = email.includes("@") && email.includes(".") ? true : false;
    const ageValide = age >= 13 && age <= 120 ? true : false;
    const nomValide = nom.length >= 2 ? true : false;
    
    // Status global
    const statusGlobal = emailValide && ageValide && nomValide ? "valide" : "invalide";
    
    // Message détaillé
    const message = statusGlobal === "valide" ? "Formulaire accepté !" 
                  : !emailValide ? "Email invalide"
                  : !ageValide ? "Âge invalide (13-120 ans)"
                  : !nomValide ? "Nom trop court (min 2 caractères)"
                  : "Erreur inconnue";
    
    return {
        status: statusGlobal,
        message: message,
        details: {
            email: emailValide ? "✅" : "❌",
            age: ageValide ? "✅" : "❌", 
            nom: nomValide ? "✅" : "❌"
        }
    };
}

// Tests
console.log(validerFormulaire("test@email.com", 16, "Alice"));
console.log(validerFormulaire("email-invalide", 16, "Bob"));
console.log(validerFormulaire("ok@test.com", 200, "Charlie"));
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **If/else** : L'opérateur ternaire est un raccourci pour if/else
- **Comparaisons** : Conditions utilisées dans le ternaire
- **Variables** : Assignation de valeurs conditionnelles

### Prochaines étapes
- **Fonctions avancées** : Ternaires dans les arrow functions
- **Array methods** : map, filter avec conditions ternaires
- **JSX React** : Rendu conditionnel avec ternaires

### Concepts complémentaires
- **Short-circuit evaluation** : && et || comme alternatives
- **Nullish coalescing** : ?? pour les valeurs nulles
- **Optional chaining** : ?. pour les propriétés optionnelles

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un "Calculateur de statut étudiant" avec l'opérateur ternaire

**Fonctionnalités requises :**
- [ ] Calcul de la moyenne à partir de 3 notes
- [ ] Attribution d'une mention (TB, B, AB, Passable, Échec)
- [ ] Détermination du passage en classe supérieure
- [ ] Calcul de bourse selon critères sociaux
- [ ] Interface simple avec formulaire

**Structure de fichiers suggérée :**
```
📁 calculateur-statut/
├── 📄 index.html
├── 📄 style.css
└── 📄 script.js
```

**Temps estimé :** 25 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Opérateur conditionnel (ternaire)](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)

### Vidéos recommandées
- "Ternary Operator in JavaScript" - Web Dev Simplified (6 min)
- Explication claire avec exemples pratiques

### Articles approfondis
- "When to Use Ternary Operators" - JavaScript.info
- Bonnes pratiques et cas d'usage appropriés

### Outils de pratique
- [CodePen Collection - Ternary Examples](https://codepen.io)
- Exemples interactifs pour pratiquer

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre ternaire et if/else ?
   - **Réponse :** Le ternaire retourne toujours une valeur, if/else peut juste exécuter du code

2. **Question pratique :** Que fait ce code ?
   ```javascript
   const resultat = score > 50 ? "Réussi" : "Échoué";
   ```
   - **Réponse :** Assigne "Réussi" si score > 50, sinon "Échoué"

3. **Question d'application :** Quand éviter l'opérateur ternaire ?
   - **Réponse :** Pour des logiques complexes, imbrications profondes, ou quand ça nuit à la lisibilité

### Checklist de maîtrise
- [ ] Je comprends la syntaxe condition ? vrai : faux
- [ ] Je peux convertir if/else simples en ternaire
- [ ] Je sais quand utiliser ternaire vs if/else
- [ ] J'évite les imbrications trop complexes
- [ ] Je peux utiliser ternaire dans différents contextes
- [ ] J'équilibre concision et lisibilité

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 - Syntaxe incomplète
**Code problématique :**
```javascript
const message = age >= 18 ? "Majeur"; // Manque la partie "faux"
```

**Message d'erreur :** `SyntaxError: Unexpected token ';'`

**Solution :**
```javascript
const message = age >= 18 ? "Majeur" : "Mineur"; // Complet
```

**Explication :** L'opérateur ternaire nécessite obligatoirement les 3 parties.

### Erreur fréquente 2 - Précédence des opérateurs
**Code problématique :**
```javascript
const resultat = 5 + 3 > 6 ? "vrai" : "faux" + "!"; 
// Peut être confus : (5 + 3 > 6) ? "vrai" : ("faux" + "!")
```

**Solution :**
```javascript
const resultat = (5 + 3 > 6) ? "vrai" : ("faux" + "!");
// Ou plus clair :
const condition = 5 + 3 > 6;
const resultat = condition ? "vrai" : "faux!";
```

**Explication :** Utilise des parenthèses pour clarifier l'ordre d'évaluation.

### Erreur fréquente 3 - Ternaires trop imbriqués
**Code problématique :**
```javascript
const note = score > 90 ? "A" : score > 80 ? "B" : score > 70 ? "C" : score > 60 ? "D" : "F";
// Difficile à lire !
```

**Solution :**
```javascript
// Plus lisible avec if/else ou fonction séparée
function obtenirNote(score) {
    if (score > 90) return "A";
    if (score > 80) return "B";
    if (score > 70) return "C";
    if (score > 60) return "D";
    return "F";
}
```

**Explication :** Quand c'est trop complexe, revenir à if/else ou fonctions.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- L'opérateur ternaire rend le code plus concis
- Il faut équilibrer concision et lisibilité
- Parfait pour les assignations conditionnelles simples

### Questions à approfondir
- Autres opérateurs conditionnels (&&, ||, ??) 
- Utilisation dans les frameworks comme React

### Révision programmée
- [ ] Révision dans 1 jour (syntaxe de base)
- [ ] Révision dans 1 semaine (exercices complexes)
- [ ] Révision dans 1 mois (application en projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #operateur-ternaire #conditions #if-else #concision #debutant

**Temps de completion :** ___

**Difficulté ressentie :** ⭐⭐☆☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 37/126*
