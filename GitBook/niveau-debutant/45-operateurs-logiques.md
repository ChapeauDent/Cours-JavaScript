# ⚡ Opérateurs Logiques en JavaScript

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 60 min | **Prérequis :** Comparaisons et égalité, Types de données
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** les trois opérateurs logiques ET, OU, NON
- [ ] **Appliquer** des combinaisons de conditions dans une expression
- [ ] **Analyser** l'évaluation court-circuit (short-circuit)
- [ ] **Créer** des conditions complexes pour vos programmes
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Les 3 opérateurs" %}
**Opérateur ET (&&)** : Retourne `true` si TOUTES les conditions sont vraies  
**Opérateur OU (||)** : Retourne `true` si AU MOINS UNE condition est vraie  
**Opérateur NON (!)** : Inverse le résultat booléen  
{% endtab %}

{% tab title="Concepts avancés" %}
**Court-circuit** : Arrêt de l'évaluation dès que le résultat est déterminé  
**Truthy/Falsy** : Valeurs considérées comme vraies ou fausses  
**Priorité** : ! puis && puis ||, utiliser des parenthèses pour clarifier  
{% endtab %}
{% endtabs %}

## 💡 Pourquoi ce concept est-il important ?

{% hint style="warning" %}
**Exemples concrets de la vie réelle :**

🎬 **Accès cinéma :** Il faut être majeur **ET** avoir son billet  
🍕 **Livraison :** Accepte carte bancaire **OU** espèces  
🚫 **Sécurité :** Accès refusé si utilisateur **NON** autorisé  

Les opérateurs logiques permettent de créer des conditions complexes qui reflètent la logique du monde réel !
{% endhint %}

## ⚡ Les trois opérateurs logiques

### 1. Opérateur ET (&&)

{% hint style="info" %}
**Règle :** Retourne `true` seulement si **TOUTES** les conditions sont vraies
{% endhint %}

{% tabs %}
{% tab title="Syntaxe de base" %}
```javascript
// Structure de base
if (condition1 && condition2) {
    // Code exécuté si TOUTES les conditions sont vraies
}

// Exemple concret
const age = 20;
const aPermis = true;

if (age >= 18 && aPermis) {
    console.log("✅ Peut conduire");
} else {
    console.log("❌ Ne peut pas conduire");
}
```
{% endtab %}

{% tab title="Table de vérité" %}
| condition1 | condition2 | résultat |
|------------|------------|----------|
| `true`     | `true`     | `true` ✅ |
| `true`     | `false`    | `false` ❌ |
| `false`    | `true`     | `false` ❌ |
| `false`    | `false`    | `false` ❌ |

**💡 Seule la première ligne donne `true` !**
{% endtab %}

{% tab title="Exemples pratiques" %}
```javascript
// Validation d'inscription
const age = 25;
const aEmail = true;
const aAccepteConditions = true;

const peutSinscrire = age >= 18 && aEmail && aAccepteConditions;
console.log("Peut s'inscrire:", peutSinscrire); // true

// Accès sécurisé
const estConnecte = true;
const estAdmin = false;
const aCredentials = true;

const accesSensible = estConnecte && estAdmin && aCredentials;
console.log("Accès admin:", accesSensible); // false (pas admin)
```
{% endtab %}
{% endtabs %}

### 2. Opérateur OU (||)

{% hint style="info" %}
**Règle :** Retourne `true` si **AU MOINS UNE** condition est vraie
{% endhint %}

{% tabs %}
{% tab title="Syntaxe de base" %}
```javascript
// Structure de base
if (condition1 || condition2) {
    // Code exécuté si AU MOINS UNE condition est vraie
}

// Exemple concret
const estWeekend = false;
const estVacances = true;

if (estWeekend || estVacances) {
    console.log("✅ Peut se reposer");
} else {
    console.log("❌ Doit travailler");
}
```
{% endtab %}

{% tab title="Table de vérité" %}
| condition1 | condition2 | résultat |
|------------|------------|----------|
| `true`     | `true`     | `true` ✅ |
| `true`     | `false`    | `true` ✅ |
| `false`    | `true`     | `true` ✅ |
| `false`    | `false`    | `false` ❌ |

**💡 Seule la dernière ligne donne `false` !**
{% endtab %}

{% tab title="Exemples pratiques" %}
```javascript
// Méthodes de paiement
const aCarteBancaire = false;
const aPaypal = true;
const aEspeces = false;

const peutPayer = aCarteBancaire || aPaypal || aEspeces;
console.log("Peut effectuer l'achat:", peutPayer); // true

// Conditions d'accès flexibles
const estMajeur = false;
const aAutorisation = true;
const estAccompagne = false;

const peutEntrer = estMajeur || aAutorisation || estAccompagne;
console.log("Peut entrer:", peutEntrer); // true (a autorisation)
```
{% endtab %}
{% endtabs %}

### 3. Opérateur NON (!)

{% hint style="info" %}
**Règle :** Inverse le résultat booléen (`true` devient `false` et vice versa)
{% endhint %}

{% tabs %}
{% tab title="Syntaxe de base" %}
```javascript
// Structure de base
if (!condition) {
    // Code exécuté si la condition est FAUSSE
}

// Exemple concret
const estConnecte = false;

if (!estConnecte) {
    console.log("❌ Veuillez vous connecter");
} else {
    console.log("✅ Bienvenue !");
}
```
{% endtab %}

{% tab title="Table de vérité" %}
| condition | !condition |
|-----------|------------|
| `true`    | `false` ❌ |
| `false`   | `true` ✅ |

**Double négation :** `!!condition` = valeur originale
{% endtab %}

{% tab title="Exemples pratiques" %}
```javascript
// Gestion d'erreurs
const erreur = null;

if (!erreur) {
    console.log("✅ Tout va bien");
} else {
    console.log("❌ Erreur détectée:", erreur);
}

// Validation inverse
const champVide = "";
const motDePasseFaible = "123";

if (!champVide || !motDePasseFaible.length >= 8) {
    console.log("❌ Formulaire invalide");
}

// Double négation pour conversion booléenne
const texte = "hello";
const estTexteVide = !texte;           // false
const estTexteRempli = !!texte;        // true
```
{% endtab %}
{% endtabs %}

## 🚀 Court-circuit (Short-circuit)

{% hint style="warning" %}
**Concept clé :** JavaScript arrête l'évaluation dès qu'il peut déterminer le résultat final.
{% endhint %}

### Court-circuit avec ET (&&)

{% tabs %}
{% tab title="Principe" %}
```javascript
// Si la première condition est false, 
// la seconde n'est PAS évaluée
let compteur = 0;

function increment() {
    compteur++;
    console.log("Fonction appelée !");
    return true;
}

console.log("Résultat:", false && increment());
console.log("Compteur:", compteur); // 0 - fonction pas appelée !
```

**💡 Économie de performance :** Évite les calculs inutiles
{% endtab %}

{% tab title="Utilisation pratique" %}
```javascript
// Vérification sécurisée d'objets
const utilisateur = null;

// ❌ Dangereux - erreur si utilisateur est null
// if (utilisateur.nom === "Admin") { ... }

// ✅ Sécurisé - court-circuit protège
if (utilisateur && utilisateur.nom === "Admin") {
    console.log("Admin connecté");
}

// Chaining sécurisé
const config = { theme: { couleur: "bleu" } };
const couleur = config && config.theme && config.theme.couleur;
console.log("Couleur:", couleur); // "bleu"
```
{% endtab %}
{% endtabs %}

### Court-circuit avec OU (||)

{% tabs %}
{% tab title="Principe" %}
```javascript
// Si la première condition est true,
// la seconde n'est PAS évaluée
let compteur = 0;

function increment() {
    compteur++;
    console.log("Fonction appelée !");
    return false;
}

console.log("Résultat:", true || increment());
console.log("Compteur:", compteur); // 0 - fonction pas appelée !
```
{% endtab %}

{% tab title="Valeurs par défaut" %}
```javascript
// Technique classique pour valeurs par défaut
function saluer(nom) {
    nom = nom || "Inconnu";  // Si nom est falsy, utilise "Inconnu"
    console.log(`Bonjour ${nom} !`);
}

saluer("Alice");  // "Bonjour Alice !"
saluer("");       // "Bonjour Inconnu !"
saluer(null);     // "Bonjour Inconnu !"

// Configuration avec défauts
function creerUtilisateur(options) {
    options = options || {};
    const nom = options.nom || "Utilisateur";
    const theme = options.theme || "clair";
    const langue = options.langue || "fr";
    
    return { nom, theme, langue };
}

console.log(creerUtilisateur());                    // Tous les défauts
console.log(creerUtilisateur({ nom: "Alice" }));    // Nom personnalisé
```
{% endtab %}
{% endtabs %}

## 🔀 Combinaisons complexes

### Priorité des opérateurs

{% hint style="warning" %}
**Ordre de priorité :**
1. `!` (NON) - priorité la plus haute
2. `&&` (ET) - priorité moyenne  
3. `||` (OU) - priorité la plus basse

**Conseil :** Utilisez des parenthèses pour clarifier !
{% endhint %}

{% tabs %}
{% tab title="Sans parenthèses" %}
```javascript
// Attention à la priorité !
const a = true;
const b = false;
const c = true;

// ! a priorité sur &&, qui a priorité sur ||
const resultat1 = !a && b || c;  // Peut être confus

// Équivaut à :
const resultat2 = ((!a) && b) || c;
console.log("Résultat:", resultat2); // true
```
{% endtab %}

{% tab title="Avec parenthèses (recommandé)" %}
```javascript
// Plus clair avec parenthèses
const age = 20;
const aPermis = true;
const voitureDisponible = false;
const urgence = true;

// Complexe mais lisible
const peutConduire = (age >= 18 && aPermis) && 
                     (voitureDisponible || urgence);

console.log("Peut conduire:", peutConduire); // true

// Conditions d'accès sophistiquées
const estMember = true;
const aPayeAbonnement = false;
const estPeriodeEssai = true;
const estVIP = false;

const acces = (estMember && aPayeAbonnement) || 
              (estPeriodeEssai && !estVIP) ||
              estVIP;

console.log("A accès:", acces); // true (période d'essai)
```
{% endtab %}
{% endtabs %}

## 💻 Exemples pratiques avancés

### Système d'authentification

{% tabs %}
{% tab title="Code complet" %}
```javascript
function verifierAcces(utilisateur) {
    // Vérifications de base
    const estConnecte = utilisateur && utilisateur.estConnecte;
    const aRole = utilisateur && utilisateur.role;
    const tokenValide = utilisateur && utilisateur.tokenExpire > Date.now();
    
    // Vérifications métier
    const estActif = utilisateur && utilisateur.statutCompte === "actif";
    const pasBloque = utilisateur && !utilisateur.estBloque;
    
    // Logique d'accès
    const acceBase = estConnecte && tokenValide && estActif && pasBloque;
    
    // Niveaux d'accès selon le rôle
    const estAdmin = aRole === "admin";
    const estModerator = aRole === "moderator";
    const estUtilisateur = aRole === "user";
    
    return {
        peutSeConnecter: acceBase,
        peutModifier: acceBase && (estAdmin || estModerator),
        peutAdministrer: acceBase && estAdmin,
        niveauAcces: estAdmin ? "admin" : estModerator ? "moderator" : "user"
    };
}

// Tests
const utilisateur1 = {
    estConnecte: true,
    role: "admin",
    tokenExpire: Date.now() + 3600000, // 1h dans le futur
    statutCompte: "actif",
    estBloque: false
};

const utilisateur2 = {
    estConnecte: true,
    role: "user", 
    tokenExpire: Date.now() - 3600000, // 1h dans le passé
    statutCompte: "actif",
    estBloque: false
};

console.log("Admin:", verifierAcces(utilisateur1));
console.log("User token expiré:", verifierAcces(utilisateur2));
```
{% endtab %}

{% tab title="Résultat" %}
```
Admin: {
  peutSeConnecter: true,
  peutModifier: true,
  peutAdministrer: true,
  niveauAcces: "admin"
}

User token expiré: {
  peutSeConnecter: false,
  peutModifier: false,
  peutAdministrer: false,
  niveauAcces: "user"
}
```
{% endtab %}
{% endtabs %}

### Validateur de formulaire intelligent

{% tabs %}
{% tab title="Validateur complet" %}
```javascript
function validerFormulaire(donnees) {
    const { nom, email, motDePasse, confirmation, age, conditions } = donnees;
    
    // Validations individuelles
    const nomValide = nom && nom.trim().length >= 2;
    const emailValide = email && 
                       email.includes("@") && 
                       email.includes(".") &&
                       !email.startsWith("@") &&
                       !email.endsWith("@");
    
    const motDePasseForte = motDePasse &&
                           motDePasse.length >= 8 &&
                           /[A-Z]/.test(motDePasse) &&    // Majuscule
                           /[a-z]/.test(motDePasse) &&    // Minuscule
                           /\d/.test(motDePasse);         // Chiffre
    
    const confirmationCorrecte = confirmation === motDePasse;
    const ageValide = age && age >= 13 && age <= 120;
    const conditionsAcceptees = conditions === true;
    
    // Validation globale
    const formulaireValide = nomValide && 
                            emailValide && 
                            motDePasseForte && 
                            confirmationCorrecte && 
                            ageValide && 
                            conditionsAcceptees;
    
    // Messages détaillés
    const erreurs = [];
    if (!nomValide) erreurs.push("Nom requis (min. 2 caractères)");
    if (!emailValide) erreurs.push("Email invalide");
    if (!motDePasseForte) erreurs.push("Mot de passe faible (8 chars, maj, min, chiffre)");
    if (!confirmationCorrecte) erreurs.push("Confirmations différentes");
    if (!ageValide) erreurs.push("Âge invalide (13-120 ans)");
    if (!conditionsAcceptees) erreurs.push("Conditions non acceptées");
    
    return {
        valide: formulaireValide,
        erreurs: erreurs,
        score: ((6 - erreurs.length) / 6 * 100).toFixed(0) + "%"
    };
}

// Tests
const formulaire1 = {
    nom: "Alice Dupont",
    email: "alice@test.com", 
    motDePasse: "MonMotDePasse123",
    confirmation: "MonMotDePasse123",
    age: 25,
    conditions: true
};

const formulaire2 = {
    nom: "A",
    email: "email-invalide",
    motDePasse: "123",
    confirmation: "456",
    age: 12,
    conditions: false
};

console.log("Formulaire valide:", validerFormulaire(formulaire1));
console.log("Formulaire invalide:", validerFormulaire(formulaire2));
```
{% endtab %}

{% tab title="Résultats" %}
```
Formulaire valide: {
  valide: true,
  erreurs: [],
  score: "100%"
}

Formulaire invalide: {
  valide: false,
  erreurs: [
    "Nom requis (min. 2 caractères)",
    "Email invalide", 
    "Mot de passe faible (8 chars, maj, min, chiffre)",
    "Confirmations différentes",
    "Âge invalide (13-120 ans)",
    "Conditions non acceptées"
  ],
  score: "0%"
}
```
{% endtab %}
{% endtabs %}

## 🎮 Quiz interactif

{% hint style="info" %}
**Question 1 :** Que donne `true && false || true` ?
A) `true`  B) `false`  C) Erreur  D) `undefined`
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : A) `true`**

Grâce à la priorité des opérateurs :
1. `true && false` = `false` (ET a priorité sur OU)
2. `false || true` = `true`

</details>

{% hint style="info" %}
**Question 2 :** Dans quel cas la fonction `verifier()` n'est-elle PAS appelée ?
```javascript
const resultat = false && verifier();
```
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**La fonction n'est jamais appelée !**

Avec l'opérateur `&&`, si la première condition est `false`, JavaScript utilise le court-circuit et n'évalue pas la seconde partie.

</details>

{% hint style="info" %}
**Question 3 :** Comment simplifier ce code ?
```javascript
if (utilisateur !== null && utilisateur !== undefined) {
    // utiliser utilisateur
}
```
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Plusieurs solutions :**

```javascript
// Solution 1: Truthy check
if (utilisateur) {
    // utilisateur n'est ni null, ni undefined, ni falsy
}

// Solution 2: Explicit null/undefined check  
if (utilisateur != null) {
    // != vérifie null ET undefined
}

// Solution 3: Modern (optional chaining)
utilisateur?.propriete; // Pas d'erreur si null/undefined
```

</details>

## 💻 Exercices pratiques

### Exercice 1 : Système d'admission universitaire

{% tabs %}
{% tab title="Consigne" %}
Créez un système qui détermine si un étudiant peut s'inscrire selon ces critères :

**Critères obligatoires :**
- Âge entre 16 et 25 ans OU être diplômé du bac
- Avoir payé les frais OU avoir une bourse
- Ne pas être dans la liste des exclus

**Critères bonus :**
- Mention au bac donne priorité
- Résidence locale donne des points

{% hint style="warning" %}
Retournez un objet avec : `{ admis: boolean, raison: string, priorite: number }`
{% endhint %}
{% endtab %}

{% tab title="Code de départ" %}
```javascript
function evaluerCandidature(candidat) {
    const { age, diplome, aPaye, aBourse, estExclu, mention, estLocal } = candidat;
    
    // Votre code ici
    
    return {
        admis: /* votre logique */,
        raison: /* explication */,
        priorite: /* 1-5 */
    };
}

// Tests
const candidats = [
    { age: 20, diplome: "bac", aPaye: true, aBourse: false, estExclu: false, mention: "bien", estLocal: true },
    { age: 30, diplome: null, aPaye: false, aBourse: true, estExclu: false, mention: null, estLocal: false },
    { age: 17, diplome: null, aPaye: true, aBourse: false, estExclu: true, mention: null, estLocal: true }
];

candidats.forEach((candidat, i) => {
    console.log(`Candidat ${i + 1}:`, evaluerCandidature(candidat));
});
```
{% endtab %}

{% tab title="Solution" %}
```javascript
function evaluerCandidature(candidat) {
    const { age, diplome, aPaye, aBourse, estExclu, mention, estLocal } = candidat;
    
    // Critères obligatoires
    const ageValide = (age >= 16 && age <= 25) || diplome === "bac";
    const financement = aPaye || aBourse;
    const pasExclu = !estExclu;
    
    // Admission de base
    const admissible = ageValide && financement && pasExclu;
    
    // Calcul priorité
    let priorite = 1; // Base
    if (mention === "très bien") priorite += 2;
    else if (mention === "bien") priorite += 1;
    if (estLocal) priorite += 1;
    if (aBourse) priorite += 1; // Critère social
    
    // Détermination raison
    let raison = "";
    if (!admissible) {
        const problemes = [];
        if (!ageValide) problemes.push("âge/diplôme");
        if (!financement) problemes.push("financement");
        if (!pasExclu) problemes.push("exclusion");
        raison = `Refusé: ${problemes.join(", ")}`;
    } else {
        raison = `Admis (priorité ${priorite})`;
    }
    
    return {
        admis: admissible,
        raison: raison,
        priorite: Math.min(priorite, 5) // Max 5
    };
}

// Tests
const candidats = [
    { age: 20, diplome: "bac", aPaye: true, aBourse: false, estExclu: false, mention: "bien", estLocal: true },
    { age: 30, diplome: null, aPaye: false, aBourse: true, estExclu: false, mention: null, estLocal: false },
    { age: 17, diplome: null, aPaye: true, aBourse: false, estExclu: true, mention: null, estLocal: true }
];

candidats.forEach((candidat, i) => {
    console.log(`Candidat ${i + 1}:`, evaluerCandidature(candidat));
});
```

**Résultat :**
```
Candidat 1: { admis: true, raison: "Admis (priorité 4)", priorite: 4 }
Candidat 2: { admis: false, raison: "Refusé: âge/diplôme", priorite: 2 }  
Candidat 3: { admis: false, raison: "Refusé: âge/diplôme, exclusion", priorite: 2 }
```
{% endtab %}
{% endtabs %}

### Exercice 2 : Moteur de recommandation de films

{% tabs %}
{% tab title="Consigne" %}
Créez un système qui recommande des films selon les préférences utilisateur.

**Critères de base :**
- Âge approprié pour le film
- Genre correspond aux préférences OU acteur favori présent
- Durée acceptable

**Critères spéciaux :**
- Films classiques : plus de tolérance sur la durée
- Films récents : bonus de recommandation
- Note élevée : compense un genre non préféré

{% hint style="info" %}
Retournez un score de recommandation sur 10.
{% endhint %}
{% endtab %}

{% tab title="Solution" %}
```javascript
function recommanderFilm(film, utilisateur) {
    const {
        titre, ageMinimum, genres, acteurs, duree, annee, note, estClassique
    } = film;
    
    const {
        age: ageUser, genresPreferés, acteursFavoris, dureeMax
    } = utilisateur;
    
    let score = 0;
    const criteres = [];
    
    // Critère obligatoire : âge
    if (ageUser >= ageMinimum) {
        score += 3;
        criteres.push("✅ Âge approprié");
    } else {
        criteres.push("❌ Trop jeune");
        return { score: 0, criteres, recommande: false };
    }
    
    // Correspondance genre OU acteur
    const genreCorrespond = genres.some(genre => genresPreferés.includes(genre));
    const acteurFavori = acteurs.some(acteur => acteursFavoris.includes(acteur));
    
    if (genreCorrespond || acteurFavori) {
        score += 2;
        criteres.push(genreCorrespond ? "✅ Genre préféré" : "✅ Acteur favori");
    } else if (note >= 8.0) {
        score += 1; // Compense avec une excellente note
        criteres.push("⭐ Très bien noté (compense genre)");
    } else {
        criteres.push("❌ Genre/acteur non préféré");
    }
    
    // Durée
    const dureeAcceptable = duree <= dureeMax || (estClassique && duree <= dureeMax * 1.2);
    if (dureeAcceptable) {
        score += 1;
        criteres.push("✅ Durée OK");
    } else {
        criteres.push("⚠️ Trop long");
    }
    
    // Bonus
    if (estClassique) {
        score += 1;
        criteres.push("🏆 Film classique");
    }
    
    if (annee >= 2020) {
        score += 1;
        criteres.push("🆕 Film récent");
    }
    
    if (note >= 9.0) {
        score += 2;
        criteres.push("⭐⭐ Note exceptionnelle");
    } else if (note >= 8.0) {
        score += 1;
        criteres.push("⭐ Très bien noté");
    }
    
    const scoreMax = Math.min(score, 10);
    const recommande = scoreMax >= 6;
    
    return {
        titre,
        score: scoreMax,
        criteres,
        recommande,
        niveau: scoreMax >= 8 ? "Fortement recommandé" : 
                scoreMax >= 6 ? "Recommandé" : 
                scoreMax >= 4 ? "Pourrait plaire" : "Pas recommandé"
    };
}

// Tests
const utilisateur = {
    age: 25,
    genresPreferés: ["science-fiction", "action"],
    acteursFavoris: ["Ryan Gosling", "Scarlett Johansson"],
    dureeMax: 120
};

const films = [
    {
        titre: "Blade Runner 2049",
        ageMinimum: 13,
        genres: ["science-fiction", "thriller"],
        acteurs: ["Ryan Gosling", "Harrison Ford"],
        duree: 164,
        annee: 2017,
        note: 8.0,
        estClassique: false
    },
    {
        titre: "Her",
        ageMinimum: 13,
        genres: ["romance", "drame"],
        acteurs: ["Joaquin Phoenix", "Scarlett Johansson"],
        duree: 126,
        annee: 2013,
        note: 8.0,
        estClassique: false
    }
];

films.forEach(film => {
    const recommandation = recommanderFilm(film, utilisateur);
    console.log(`\n🎬 ${recommandation.titre}`);
    console.log(`Score: ${recommandation.score}/10 - ${recommandation.niveau}`);
    console.log("Critères:", recommandation.criteres.join(", "));
});
```
{% endtab %}
{% endtabs %}

## ⚠️ Pièges courants et solutions

### Piège 1 : Conditions incomplètes

{% tabs %}
{% tab title="❌ Erreur commune" %}
```javascript
const age = 20;

// ERREUR : syntaxe incorrecte
if (age >= 18 && < 65) {
    console.log("Adulte");
}

// ERREUR : comparaison incomplète
if (18 <= age <= 65) {  // Ne marche pas en JavaScript !
    console.log("Adulte");
}
```
{% endtab %}

{% tab title="✅ Solution correcte" %}
```javascript
const age = 20;

// Chaque condition doit être complète
if (age >= 18 && age < 65) {
    console.log("Adulte");
}

// Ou avec des parenthèses pour clarifier
if ((age >= 18) && (age < 65)) {
    console.log("Adulte");
}
```
{% endtab %}
{% endtabs %}

### Piège 2 : Accès aux propriétés d'objets

{% tabs %}
{% tab title="❌ Code dangereux" %}
```javascript
const utilisateur = null;

// ERREUR : Cannot read property 'nom' of null
if (utilisateur.nom && utilisateur.nom === "Admin") {
    console.log("Admin connecté");
}
```
{% endtab %}

{% tab title="✅ Vérification sécurisée" %}
```javascript
const utilisateur = null;

// Vérifier l'objet AVANT ses propriétés
if (utilisateur && utilisateur.nom && utilisateur.nom === "Admin") {
    console.log("Admin connecté");
}

// Moderne : optional chaining
if (utilisateur?.nom === "Admin") {
    console.log("Admin connecté");
}

// Fonction utilitaire
function estAdmin(user) {
    return user && user.role === "admin" && user.actif;
}
```
{% endtab %}
{% endtabs %}

### Piège 3 : Confusion entre = et ==

{% tabs %}
{% tab title="❌ Erreur assignation" %}
```javascript
const estConnecte = true;

// ERREUR : assignation au lieu de comparaison !
if (estConnecte = false) {  // Assigne false à estConnecte
    console.log("Ne sera jamais exécuté");
}

console.log(estConnecte); // false (modifié !)
```
{% endtab %}

{% tab title="✅ Comparaison correcte" %}
```javascript
const estConnecte = true;

// Comparaison stricte (recommandée)
if (estConnecte === false) {
    console.log("Utilisateur déconnecté");
}

// Ou plus simple avec l'opérateur NON
if (!estConnecte) {
    console.log("Utilisateur déconnecté");
}

console.log(estConnecte); // true (inchangé)
```
{% endtab %}
{% endtabs %}

## 🔗 Liens avec d'autres concepts

{% content-ref url="comparaisons-egalite.md" %}
[comparaisons-egalite.md](comparaisons-egalite.md)
{% endcontent-ref %}

{% content-ref url="structures-controle.md" %}
[structures-controle.md](structures-controle.md)
{% endcontent-ref %}

{% content-ref url="operateur-ternaire.md" %}
[operateur-ternaire.md](operateur-ternaire.md)
{% endcontent-ref %}

## 🚀 Projet pratique

{% hint style="success" %}
**Défi : Système de contrôle d'accès intelligent**

**Fonctionnalités à implémenter :**
- [ ] Vérification multi-niveaux (identité, horaires, permissions)
- [ ] Gestion des exceptions (urgences, maintenance, VIP)
- [ ] Journal des tentatives d'accès avec raisons
- [ ] Interface simple avec notifications claires

**Technologies :** HTML, CSS, JavaScript vanilla  
**Temps estimé :** 90 minutes
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

⚡ **Trois opérateurs :** `&&` (ET), `||` (OU), `!` (NON)  
🚀 **Court-circuit :** Optimisation automatique des performances  
🎯 **Priorité :** `!` > `&&` > `||`, utiliser des parenthèses  
🔒 **Sécurité :** Vérifier les objets avant leurs propriétés  
✨ **Lisibilité :** Conditions complexes = parenthèses + noms explicites  
🎮 **Pratique :** Combiner avec if/else et fonctions pour la logique métier  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Passez aux [Structures de Contrôle](structures-controle.md) pour utiliser ces opérateurs dans des conditions complètes !
{% endhint %}
