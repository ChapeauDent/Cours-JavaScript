# Fonctions Avancées en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnées du cours
- **Titre :** Fonctions Avancées - Closures, Callbacks et Arrow Functions
- **Niveau :** Intermédiaire
- **Durée estimée :** 120 minutes
- **Prérequis :** Fonctions de base, scope, objets
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Maîtriser** : Les closures et leur utilisation pratique
- [ ] **Appliquer** : Les callbacks pour la programmation asynchrone
- [ ] **Utiliser** : Les arrow functions et leurs avantages
- [ ] **Comprendre** : Les concepts de higher-order functions
- [ ] **Implémenter** : Des patterns fonctionnels avancés

### Mots-clés à retenir
- **Closure** : Fonction qui "capture" son environnement lexical
- **Callback** : Fonction passée en paramètre à une autre fonction
- **Arrow Function** : Syntaxe moderne et concise pour les fonctions
- **Higher-Order Function** : Fonction qui prend ou retourne d'autres fonctions
- **Lexical Scope** : Portée déterminée par l'endroit où les variables sont déclarées

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les fonctions avancées sont au cœur de JavaScript moderne. Elles permettent de créer du code plus élégant, réutilisable et puissant. Les closures résolvent des problèmes de portée, les callbacks gèrent l'asynchrone, et les arrow functions simplifient la syntaxe tout en résolvant les problèmes de `this`.

### 1. Les Closures

#### **Définition et mécanisme**
Une closure est créée quand une fonction interne accède aux variables de sa fonction externe, même après que la fonction externe ait terminé son exécution.

```javascript
// Exemple basique de closure
function fonctionExterne(x) {
    // Variable dans le scope externe
    let variableExterne = x;
    
    // Fonction interne qui "capture" variableExterne
    function fonctionInterne(y) {
        return variableExterne + y; // Accès à variableExterne
    }
    
    return fonctionInterne; // Retourner la fonction interne
}

let maClosure = fonctionExterne(10);
console.log(maClosure(5)); // 15
// La variableExterne (10) est toujours accessible !
```

#### **Cas d'usage pratiques**
```javascript
// 1. Factory functions (usines à fonctions)
function creerCompteur(initialValue = 0) {
    let compteur = initialValue;
    
    return {
        incrementer: () => ++compteur,
        decrementer: () => --compteur,
        valeur: () => compteur,
        reset: () => { compteur = initialValue; }
    };
}

const compteur1 = creerCompteur(5);
const compteur2 = creerCompteur(100);

console.log(compteur1.incrementer()); // 6
console.log(compteur2.incrementer()); // 101
// Chaque compteur a sa propre "copie" des variables

// 2. Modules privés
function creerModule() {
    // Variables privées
    let donneesPrivees = "Secret";
    let compteurPrive = 0;
    
    // Interface publique
    return {
        getDonnees: function() {
            return donneesPrivees;
        },
        setDonnees: function(nouvelleDonnee) {
            donneesPrivees = nouvelleDonnee;
            compteurPrive++;
        },
        getCompteur: function() {
            return compteurPrive;
        }
    };
}

let monModule = creerModule();
// donneesPrivees n'est pas accessible directement
console.log(monModule.getDonnees()); // "Secret"
```

### 2. Les Callbacks

#### **Concept fondamental**
Un callback est une fonction passée en argument à une autre fonction, pour être appelée plus tard.

```javascript
// Callback simple
function traiterDonnees(donnees, callback) {
    console.log("Traitement des données...");
    let resultat = donnees.map(x => x * 2);
    callback(resultat); // Appel du callback avec le résultat
}

function afficherResultat(resultat) {
    console.log("Résultat:", resultat);
}

traiterDonnees([1, 2, 3], afficherResultat);
// "Traitement des données..."
// "Résultat: [2, 4, 6]"
```

#### **Callbacks avec les méthodes d'array**
```javascript
// Callbacks intégrés dans JavaScript
let nombres = [1, 2, 3, 4, 5];

// map - transforme chaque élément
let doubles = nombres.map(function(nombre) {
    return nombre * 2;
});

// filter - filtre selon un critère
let pairs = nombres.filter(function(nombre) {
    return nombre % 2 === 0;
});

// reduce - réduit à une seule valeur
let somme = nombres.reduce(function(total, nombre) {
    return total + nombre;
}, 0);

console.log("Doubles:", doubles);   // [2, 4, 6, 8, 10]
console.log("Pairs:", pairs);       // [2, 4]
console.log("Somme:", somme);       // 15
```

### 3. Les Arrow Functions

#### **Syntaxe et avantages**
```javascript
// Syntaxe traditionnelle
function addition(a, b) {
    return a + b;
}

// Arrow function équivalente
const additionArrow = (a, b) => {
    return a + b;
};

// Version encore plus concise (return implicite)
const additionConcise = (a, b) => a + b;

// Un seul paramètre (parenthèses optionnelles)
const carre = x => x * x;

// Aucun paramètre
const obtenirTimestamp = () => Date.now();

// Retour d'objet (attention aux parenthèses)
const creerPersonne = (nom, age) => ({
    nom: nom,
    age: age,
    saluer: () => `Bonjour, je suis ${nom}`
});
```

#### **Différences avec les fonctions classiques**
```javascript
// 1. Pas de binding de 'this'
const objetAvecMethodes = {
    nom: "MonObjet",
    
    methodeClassique: function() {
        console.log("Dans méthode classique, this.nom =", this.nom);
        
        // Problème avec setTimeout et function
        setTimeout(function() {
            console.log("Dans setTimeout function, this.nom =", this.nom); // undefined
        }, 100);
        
        // Solution avec arrow function
        setTimeout(() => {
            console.log("Dans setTimeout arrow, this.nom =", this.nom); // "MonObjet"
        }, 200);
    }
};

// 2. Pas d'objet 'arguments'
function fonctionClassique() {
    console.log("Arguments:", arguments); // Fonctionne
}

const fonctionArrow = () => {
    console.log("Arguments:", arguments); // Erreur !
};

// Utiliser rest parameters à la place
const fonctionArrowRest = (...args) => {
    console.log("Arguments:", args); // Fonctionne
};
```

### 4. Higher-Order Functions

#### **Fonctions qui retournent des fonctions**
```javascript
// Fonction qui retourne une fonction configurée
function creerMultiplicateur(facteur) {
    return function(nombre) {
        return nombre * facteur;
    };
}

const doubler = creerMultiplicateur(2);
const tripler = creerMultiplicateur(3);

console.log(doubler(5));  // 10
console.log(tripler(5));  // 15

// Version avec arrow functions
const creerMultiplicateurArrow = facteur => nombre => nombre * facteur;
const quintupler = creerMultiplicateurArrow(5);
```

#### **Fonctions qui prennent des fonctions en paramètre**
```javascript
// Fonction générique pour répéter une action
function repeter(fois, action) {
    for (let i = 0; i < fois; i++) {
        action(i);
    }
}

repeter(3, index => console.log(`Action ${index + 1}`));
// "Action 1"
// "Action 2" 
// "Action 3"

// Fonction pour mesurer le temps d'exécution
function mesurer(nom, fonctionAMesurer) {
    console.time(nom);
    fonctionAMesurer();
    console.timeEnd(nom);
}

mesurer("Calcul lourd", () => {
    let resultat = 0;
    for (let i = 0; i < 1000000; i++) {
        resultat += i;
    }
});
```

---

## **PARTIE PRATIQUE**

### Exemple starter : Cache avec closure
```javascript
// Créer un système de cache simple avec closure
function creerCache() {
    // Données privées stockées dans la closure
    const cache = {};
    let hits = 0;
    let misses = 0;
    
    return {
        // Récupérer une valeur
        get: function(cle) {
            if (cache[cle] !== undefined) {
                hits++;
                console.log(`✅ Cache HIT pour "${cle}"`);
                return cache[cle];
            } else {
                misses++;
                console.log(`❌ Cache MISS pour "${cle}"`);
                return null;
            }
        },
        
        // Stocker une valeur
        set: function(cle, valeur) {
            cache[cle] = valeur;
            console.log(`💾 Valeur mise en cache: "${cle}" => ${valeur}`);
        },
        
        // Statistiques
        stats: function() {
            const total = hits + misses;
            const tauxReussite = total > 0 ? (hits / total * 100).toFixed(1) : 0;
            
            return {
                hits: hits,
                misses: misses,
                total: total,
                tauxReussite: tauxReussite + '%',
                tailleDuCache: Object.keys(cache).length
            };
        },
        
        // Vider le cache
        clear: function() {
            Object.keys(cache).forEach(cle => delete cache[cle]);
            hits = 0;
            misses = 0;
            console.log("🗑️ Cache vidé");
        }
    };
}

// Tests du cache
console.log("=== TESTS DU CACHE ===");
const monCache = creerCache();

// Première série de tests
monCache.set("user:123", {nom: "Alice", age: 25});
monCache.set("user:456", {nom: "Bob", age: 30});

console.log("\n--- Accès aux données ---");
console.log("User 123:", monCache.get("user:123"));
console.log("User 456:", monCache.get("user:456"));
console.log("User 789:", monCache.get("user:789")); // N'existe pas

console.log("\n--- Statistiques ---");
console.log(monCache.stats());
```

**Résultat attendu :**
```
=== TESTS DU CACHE ===
💾 Valeur mise en cache: "user:123" => [object Object]
💾 Valeur mise en cache: "user:456" => [object Object]

--- Accès aux données ---
✅ Cache HIT pour "user:123"
User 123: {nom: "Alice", age: 25}
✅ Cache HIT pour "user:456"
User 456: {nom: "Bob", age: 30}
❌ Cache MISS pour "user:789"
User 789: null

--- Statistiques ---
{hits: 2, misses: 1, total: 3, tauxReussite: "66.7%", tailleDuCache: 2}
```

### Exemple intermédiaire : Système d'événements avec callbacks
```javascript
// Gestionnaire d'événements personnalisé
function creerGestionnaireEvenements() {
    const evenements = {};
    
    return {
        // S'abonner à un événement
        on: function(nomEvenement, callback) {
            if (!evenements[nomEvenement]) {
                evenements[nomEvenement] = [];
            }
            evenements[nomEvenement].push(callback);
            console.log(`📡 Abonnement à l'événement "${nomEvenement}"`);
        },
        
        // Déclencher un événement
        emit: function(nomEvenement, ...args) {
            if (evenements[nomEvenement]) {
                console.log(`🚀 Déclenchement de l'événement "${nomEvenement}"`);
                evenements[nomEvenement].forEach((callback, index) => {
                    console.log(`  └── Exécution du callback ${index + 1}`);
                    callback(...args);
                });
            } else {
                console.log(`⚠️ Aucun listener pour l'événement "${nomEvenement}"`);
            }
        },
        
        // Se désabonner
        off: function(nomEvenement, callback) {
            if (evenements[nomEvenement]) {
                const index = evenements[nomEvenement].indexOf(callback);
                if (index > -1) {
                    evenements[nomEvenement].splice(index, 1);
                    console.log(`🔇 Désabonnement de l'événement "${nomEvenement}"`);
                }
            }
        },
        
        // Lister les événements
        listEvents: function() {
            return Object.keys(evenements).map(nom => ({
                nom: nom,
                nombreListeners: evenements[nom].length
            }));
        }
    };
}

// Utilisation du gestionnaire d'événements
console.log("=== SYSTÈME D'ÉVÉNEMENTS ===");
const eventManager = creerGestionnaireEvenements();

// Définir des callbacks
const onUserLogin = (username) => {
    console.log(`👤 Utilisateur connecté: ${username}`);
};

const onUserLogin2 = (username) => {
    console.log(`📊 Mise à jour des statistiques pour: ${username}`);
};

const onUserLogout = (username) => {
    console.log(`👋 Au revoir ${username} !`);
};

// S'abonner aux événements
eventManager.on('userLogin', onUserLogin);
eventManager.on('userLogin', onUserLogin2);
eventManager.on('userLogout', onUserLogout);

console.log("\n--- Événements enregistrés ---");
console.log(eventManager.listEvents());

console.log("\n--- Tests des événements ---");
eventManager.emit('userLogin', 'Alice');
eventManager.emit('userLogout', 'Alice');
eventManager.emit('unknownEvent'); // Événement inexistant
```

### Exemple avancé : Programmation fonctionnelle avec composition
```javascript
// Fonctions utilitaires avec arrow functions
const add = x => y => x + y;
const multiply = x => y => x * y;
const subtract = x => y => x - y;
const divide = x => y => x / y;

// Fonction de composition
const compose = (...functions) => (value) => {
    return functions.reduceRight((acc, fn) => fn(acc), value);
};

// Fonction de pipe (composition inversée)
const pipe = (...functions) => (value) => {
    return functions.reduce((acc, fn) => fn(acc), value);
};

// Fonctions de transformation de données
const parseToNumber = str => Number(str);
const addTax = rate => amount => amount * (1 + rate);
const formatCurrency = amount => `$${amount.toFixed(2)}`;
const toUpperCase = str => str.toUpperCase();
const addPrefix = prefix => str => `${prefix}${str}`;

console.log("=== PROGRAMMATION FONCTIONNELLE ===");

// Calcul de prix avec taxe
const calculerPrixFinal = pipe(
    parseToNumber,           // "100" => 100
    addTax(0.20),           // 100 => 120 (20% de taxe)
    formatCurrency          // 120 => "$120.00"
);

console.log("Prix avec taxe:", calculerPrixFinal("100"));

// Traitement de texte composé
const traiterTexte = compose(
    addPrefix(">>> "),      // "HELLO WORLD" => ">>> HELLO WORLD"
    toUpperCase            // "hello world" => "HELLO WORLD"
);

console.log("Texte traité:", traiterTexte("hello world"));

// Array processing avec fonctions pures
const donnees = [
    { nom: "Alice", age: 25, salaire: 50000 },
    { nom: "Bob", age: 30, salaire: 60000 },
    { nom: "Charlie", age: 35, salaire: 70000 },
    { nom: "Diana", age: 28, salaire: 55000 }
];

// Fonctions de traitement
const estAdulte = personne => personne.age >= 30;
const calculerSalaireNet = taux => personne => ({
    ...personne,
    salaireNet: personne.salaire * (1 - taux)
});
const extraireNom = personne => personne.nom;
const formaterNomAge = personne => `${personne.nom} (${personne.age} ans)`;

console.log("\n--- Traitement de données ---");

// Pipeline de traitement
const adultesSalaireNet = donnees
    .filter(estAdulte)
    .map(calculerSalaireNet(0.25)) // 25% d'impôts
    .map(formaterNomAge);

console.log("Adultes avec salaire net:");
adultesSalaireNet.forEach(info => console.log(`  • ${info}`));

// Calculs statistiques avec reduce
const statistiques = donnees.reduce((stats, personne) => ({
    nombrePersonnes: stats.nombrePersonnes + 1,
    ageTotal: stats.ageTotal + personne.age,
    salaireTotal: stats.salaireTotal + personne.salaire,
    ageMoyen: (stats.ageTotal + personne.age) / (stats.nombrePersonnes + 1),
    salaireMoyen: (stats.salaireTotal + personne.salaire) / (stats.nombrePersonnes + 1)
}), {
    nombrePersonnes: 0,
    ageTotal: 0,
    salaireTotal: 0,
    ageMoyen: 0,
    salaireMoyen: 0
});

console.log("\n--- Statistiques ---");
console.log(`Nombre de personnes: ${statistiques.nombrePersonnes}`);
console.log(`Âge moyen: ${statistiques.ageMoyen.toFixed(1)} ans`);
console.log(`Salaire moyen: $${statistiques.salaireMoyen.toLocaleString()}`);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Générateur de fonctions mathématiques
**Consigne :** Créez une factory function qui génère des fonctions mathématiques configurables.

**Code de départ :**
```javascript
// TODO: Créer une fonction creerCalculateur qui prend une opération
// et retourne une fonction qui applique cette opération
function creerCalculateur(operation) {
    // Votre code ici
}

// Tests attendus:
// const additionneur = creerCalculateur('+');
// console.log(additionneur(5, 3)); // 8
// 
// const multiplicateur = creerCalculateur('*');
// console.log(multiplicateur(4, 7)); // 28
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Debounce function
**Consigne :** Implémentez une fonction debounce qui limite la fréquence d'exécution d'une fonction.

**Contraintes :**
- La fonction ne doit s'exécuter qu'après X millisecondes de "silence"
- Si appelée avant la fin du délai, remettre le timer à zéro

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Pipeline de validation
**Consigne :** Créez un système de validation avec composition de fonctions.

**Contraintes :**
- Chaque fonction de validation retourne true/false
- Composer plusieurs validations
- Retourner un rapport détaillé

**Niveau de difficulté :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function creerCalculateur(operation) {
    // Map des opérations supportées
    const operations = {
        '+': (a, b) => a + b,
        '-': (a, b) => a - b,
        '*': (a, b) => a * b,
        '/': (a, b) => {
            if (b === 0) {
                throw new Error("Division par zéro impossible");
            }
            return a / b;
        },
        '%': (a, b) => a % b,
        '**': (a, b) => Math.pow(a, b),
        'sqrt': (a) => Math.sqrt(a),
        'abs': (a) => Math.abs(a)
    };
    
    // Vérifier que l'opération existe
    if (!operations[operation]) {
        throw new Error(`Opération "${operation}" non supportée`);
    }
    
    // Retourner une fonction qui utilise l'opération choisie
    return function(...args) {
        try {
            return operations[operation](...args);
        } catch (error) {
            console.error(`Erreur dans le calcul ${operation}:`, error.message);
            return null;
        }
    };
}

// Tests complets
console.log("=== GÉNÉRATEUR DE CALCULATEURS ===");

try {
    // Opérations binaires
    const additionneur = creerCalculateur('+');
    const soustracteur = creerCalculateur('-');
    const multiplicateur = creerCalculateur('*');
    const diviseur = creerCalculateur('/');
    const modulateur = creerCalculateur('%');
    const puissance = creerCalculateur('**');
    
    // Opérations unaires
    const racineCarree = creerCalculateur('sqrt');
    const valeurAbsolue = creerCalculateur('abs');
    
    console.log("Addition 5 + 3 =", additionneur(5, 3));
    console.log("Soustraction 10 - 4 =", soustracteur(10, 4));
    console.log("Multiplication 4 * 7 =", multiplicateur(4, 7));
    console.log("Division 15 / 3 =", diviseur(15, 3));
    console.log("Modulo 17 % 5 =", modulateur(17, 5));
    console.log("Puissance 2 ** 8 =", puissance(2, 8));
    console.log("Racine carrée de 16 =", racineCarree(16));
    console.log("Valeur absolue de -42 =", valeurAbsolue(-42));
    
    // Test d'erreur
    console.log("Division par zéro:", diviseur(10, 0));
    
} catch (error) {
    console.error("Erreur:", error.message);
}

// Calculateur avancé avec historique
function creerCalculateurAvance(operation) {
    const calculateur = creerCalculateur(operation);
    const historique = [];
    
    return {
        calculer: function(...args) {
            const resultat = calculateur(...args);
            
            // Enregistrer dans l'historique
            historique.push({
                operation: operation,
                arguments: args,
                resultat: resultat,
                timestamp: new Date().toLocaleTimeString()
            });
            
            return resultat;
        },
        
        obtenirHistorique: function() {
            return [...historique]; // Copie pour éviter les modifications
        },
        
        viderHistorique: function() {
            historique.length = 0;
            console.log("Historique vidé");
        },
        
        dernierCalcul: function() {
            return historique[historique.length - 1] || null;
        }
    };
}

console.log("\n=== CALCULATEUR AVEC HISTORIQUE ===");
const calcAvance = creerCalculateurAvance('*');

console.log("Résultat 1:", calcAvance.calculer(3, 4));
console.log("Résultat 2:", calcAvance.calculer(5, 6));
console.log("Résultat 3:", calcAvance.calculer(7, 8));

console.log("\nHistorique complet:");
calcAvance.obtenirHistorique().forEach((entree, index) => {
    console.log(`${index + 1}. ${entree.arguments.join(' ' + entree.operation + ' ')} = ${entree.resultat} (${entree.timestamp})`);
});

console.log("\nDernier calcul:", calcAvance.dernierCalcul());
```

### Solution Exercice 2
```javascript
function debounce(func, delay) {
    let timeoutId;
    let lastArgs;
    let lastThis;
    
    const debouncedFunction = function(...args) {
        lastArgs = args;
        lastThis = this;
        
        // Annuler le précédent timeout s'il existe
        if (timeoutId) {
            clearTimeout(timeoutId);
            console.log("⏰ Timeout précédent annulé");
        }
        
        // Programmer la nouvelle exécution
        timeoutId = setTimeout(() => {
            console.log(`🚀 Exécution après ${delay}ms de silence`);
            func.apply(lastThis, lastArgs);
            timeoutId = null;
        }, delay);
        
        console.log(`⏳ Fonction programmée pour dans ${delay}ms`);
    };
    
    // Méthodes utilitaires
    debouncedFunction.cancel = function() {
        if (timeoutId) {
            clearTimeout(timeoutId);
            timeoutId = null;
            console.log("🚫 Debounce annulé manuellement");
        }
    };
    
    debouncedFunction.flush = function() {
        if (timeoutId) {
            clearTimeout(timeoutId);
            func.apply(lastThis, lastArgs);
            timeoutId = null;
            console.log("⚡ Exécution immédiate forcée");
        }
    };
    
    debouncedFunction.pending = function() {
        return timeoutId !== null;
    };
    
    return debouncedFunction;
}

// Test du debounce
console.log("=== TEST DEBOUNCE ===");

// Fonction à débouncer
function sauvegarderDonnees(donnees) {
    console.log(`💾 Sauvegarde des données: ${JSON.stringify(donnees)}`);
}

const sauvegardeDebounced = debounce(sauvegarderDonnees, 1000);

console.log("Test 1: Appels rapides (ne devrait sauvegarder qu'une fois)");
sauvegardeDebounced({nom: "Alice", version: 1});
setTimeout(() => sauvegardeDebounced({nom: "Alice", version: 2}), 300);
setTimeout(() => sauvegardeDebounced({nom: "Alice", version: 3}), 600);
setTimeout(() => sauvegardeDebounced({nom: "Alice", version: 4}), 900); // Final

setTimeout(() => {
    console.log("\nTest 2: Annulation manuelle");
    const sauvegardeTest = debounce(sauvegarderDonnees, 2000);
    sauvegardeTest({nom: "Bob", version: 1});
    
    setTimeout(() => {
        sauvegardeTest.cancel(); // Annuler avant l'exécution
    }, 1000);
}, 3000);

setTimeout(() => {
    console.log("\nTest 3: Exécution forcée");
    const sauvegardeTest2 = debounce(sauvegarderDonnees, 2000);
    sauvegardeTest2({nom: "Charlie", version: 1});
    
    setTimeout(() => {
        sauvegardeTest2.flush(); // Forcer l'exécution immédiate
    }, 500);
}, 6000);

// Cas d'usage pratique: recherche en temps réel
function simulerRechercheAPI(terme) {
    console.log(`🔍 Recherche API pour: "${terme}"`);
    // Simuler une requête réseau
    return new Promise(resolve => {
        setTimeout(() => {
            const resultats = [
                `Résultat 1 pour "${terme}"`,
                `Résultat 2 pour "${terme}"`,
                `Résultat 3 pour "${terme}"`
            ];
            console.log(`✅ Résultats trouvés:`, resultats);
            resolve(resultats);
        }, 200);
    });
}

const rechercheDebounced = debounce(simulerRechercheAPI, 500);

console.log("\n=== SIMULATION RECHERCHE EN TEMPS RÉEL ===");
setTimeout(() => {
    console.log("Utilisateur tape 'jav'...");
    rechercheDebounced("jav");
}, 8000);

setTimeout(() => {
    console.log("Utilisateur tape 'javas'...");
    rechercheDebounced("javas");
}, 8200);

setTimeout(() => {
    console.log("Utilisateur tape 'javascript'...");
    rechercheDebounced("javascript"); // Celui-ci sera exécuté
}, 8400);
```

### Solution Exercice 3
```javascript
// Système de validation avec composition
function creerValidateur() {
    const validations = [];
    
    return {
        // Ajouter une règle de validation
        ajouter: function(nom, testFunction, messageErreur) {
            validations.push({
                nom: nom,
                test: testFunction,
                message: messageErreur
            });
            return this; // Pour le chaînage
        },
        
        // Valider une valeur
        valider: function(valeur) {
            const resultats = [];
            let estValide = true;
            
            for (let validation of validations) {
                const resultatTest = validation.test(valeur);
                
                resultats.push({
                    nom: validation.nom,
                    valide: resultatTest,
                    message: resultatTest ? "✅ Valide" : `❌ ${validation.message}`
                });
                
                if (!resultatTest) {
                    estValide = false;
                }
            }
            
            return {
                estValide: estValide,
                resultats: resultats,
                erreurs: resultats.filter(r => !r.valide),
                resume: `${resultats.filter(r => r.valide).length}/${resultats.length} validations réussies`
            };
        },
        
        // Obtenir la liste des règles
        obtenirRegles: function() {
            return validations.map(v => v.nom);
        }
    };
}

// Fonctions de validation réutilisables
const validations = {
    // Validation de longueur
    longueurMin: (min) => (valeur) => 
        typeof valeur === 'string' && valeur.length >= min,
    
    longueurMax: (max) => (valeur) => 
        typeof valeur === 'string' && valeur.length <= max,
    
    // Validation numérique
    estNombre: (valeur) => 
        !isNaN(valeur) && !isNaN(parseFloat(valeur)),
    
    nombreMin: (min) => (valeur) => 
        validations.estNombre(valeur) && parseFloat(valeur) >= min,
    
    nombreMax: (max) => (valeur) => 
        validations.estNombre(valeur) && parseFloat(valeur) <= max,
    
    // Validation de format
    estEmail: (valeur) => {
        const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return regexEmail.test(valeur);
    },
    
    contientMajuscule: (valeur) => 
        typeof valeur === 'string' && /[A-Z]/.test(valeur),
    
    contientMinuscule: (valeur) => 
        typeof valeur === 'string' && /[a-z]/.test(valeur),
    
    contientChiffre: (valeur) => 
        typeof valeur === 'string' && /\d/.test(valeur),
    
    contientSymbole: (valeur) => 
        typeof valeur === 'string' && /[!@#$%^&*(),.?":{}|<>]/.test(valeur),
    
    // Validation personnalisée
    nonVide: (valeur) => 
        valeur !== null && valeur !== undefined && valeur.toString().trim() !== '',
    
    estURL: (valeur) => {
        try {
            new URL(valeur);
            return true;
        } catch {
            return false;
        }
    }
};

console.log("=== SYSTÈME DE VALIDATION ===");

// Exemple 1: Validation de mot de passe
const validateurMotDePasse = creerValidateur()
    .ajouter('longueur', validations.longueurMin(8), "Le mot de passe doit contenir au moins 8 caractères")
    .ajouter('majuscule', validations.contientMajuscule, "Le mot de passe doit contenir au moins une majuscule")
    .ajouter('minuscule', validations.contientMinuscule, "Le mot de passe doit contenir au moins une minuscule")
    .ajouter('chiffre', validations.contientChiffre, "Le mot de passe doit contenir au moins un chiffre")
    .ajouter('symbole', validations.contientSymbole, "Le mot de passe doit contenir au moins un symbole");

console.log("Règles de validation mot de passe:", validateurMotDePasse.obtenirRegles());

// Tests de mots de passe
const motsDePasseTest = [
    "123",                    // Trop simple
    "motdepasse",            // Pas de majuscule, chiffre, symbole
    "MotDePasse",            // Pas de chiffre, symbole
    "MotDePasse123",         // Pas de symbole
    "MotDePasse123!"         // Valide !
];

motsDePasseTest.forEach((mdp, index) => {
    console.log(`\n--- Test mot de passe ${index + 1}: "${mdp}" ---`);
    const resultat = validateurMotDePasse.valider(mdp);
    
    console.log(`Résultat: ${resultat.estValide ? '✅ VALIDE' : '❌ INVALIDE'}`);
    console.log(`Résumé: ${resultat.resume}`);
    
    if (!resultat.estValide) {
        console.log("Erreurs:");
        resultat.erreurs.forEach(erreur => console.log(`  • ${erreur.message}`));
    }
});

// Exemple 2: Validation d'âge
const validateurAge = creerValidateur()
    .ajouter('estNombre', validations.estNombre, "L'âge doit être un nombre")
    .ajouter('ageMin', validations.nombreMin(0), "L'âge ne peut pas être négatif")
    .ajouter('ageMax', validations.nombreMax(150), "L'âge ne peut pas dépasser 150 ans");

console.log("\n=== VALIDATION D'ÂGE ===");
['25', '-5', '200', 'abc', '30'].forEach(age => {
    const resultat = validateurAge.valider(age);
    console.log(`Âge "${age}": ${resultat.estValide ? '✅' : '❌'} (${resultat.resume})`);
});

// Exemple 3: Validation d'email
const validateurEmail = creerValidateur()
    .ajouter('nonVide', validations.nonVide, "L'email ne peut pas être vide")
    .ajouter('formatEmail', validations.estEmail, "Format d'email invalide");

console.log("\n=== VALIDATION D'EMAIL ===");
['test@example.com', 'invalid-email', '', 'user@domain'].forEach(email => {
    const resultat = validateurEmail.valider(email);
    console.log(`Email "${email}": ${resultat.estValide ? '✅' : '❌'}`);
    if (!resultat.estValide) {
        resultat.erreurs.forEach(e => console.log(`  └── ${e.message}`));
    }
});

// Validation composée pour un formulaire complet
function validerFormulaire(donnees) {
    const validateurs = {
        nom: creerValidateur()
            .ajouter('nonVide', validations.nonVide, "Le nom est requis")
            .ajouter('longueur', validations.longueurMin(2), "Le nom doit contenir au moins 2 caractères"),
        
        email: validateurEmail,
        age: validateurAge,
        motDePasse: validateurMotDePasse
    };
    
    const resultatsValidation = {};
    let formulaireValide = true;
    
    for (let [champ, valeur] of Object.entries(donnees)) {
        if (validateurs[champ]) {
            resultatsValidation[champ] = validateurs[champ].valider(valeur);
            if (!resultatsValidation[champ].estValide) {
                formulaireValide = false;
            }
        }
    }
    
    return {
        valide: formulaireValide,
        champs: resultatsValidation,
        erreurs: Object.entries(resultatsValidation)
            .filter(([_, resultat]) => !resultat.estValide)
            .map(([champ, resultat]) => ({
                champ: champ,
                erreurs: resultat.erreurs.map(e => e.message)
            }))
    };
}

console.log("\n=== VALIDATION FORMULAIRE COMPLET ===");
const donneesFormulaire = {
    nom: "Alice",
    email: "alice@example.com",
    age: "25",
    motDePasse: "MonMotDePasse123!"
};

const validationFormulaire = validerFormulaire(donneesFormulaire);
console.log("Formulaire valide:", validationFormulaire.valide ? "✅ OUI" : "❌ NON");

if (!validationFormulaire.valide) {
    console.log("Erreurs détectées:");
    validationFormulaire.erreurs.forEach(erreur => {
        console.log(`  ${erreur.champ}:`);
        erreur.erreurs.forEach(msg => console.log(`    • ${msg}`));
    });
}
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables nécessaires
- **Fonctions de base** : Déclaration, paramètres, return
- **Scope et variables** : let, const, portée lexicale
- **Objets** : Propriétés, méthodes, this
- **Arrays** : Méthodes map, filter, reduce

### Prochaines étapes recommandées
- **Programmation asynchrone** : Promises, async/await avec callbacks
- **Classes ES6** : Méthodes privées avec closures
- **Modules** : Import/export de fonctions
- **Design patterns** : Observer, Factory, Module

### Applications pratiques
- **React/Vue.js** : Hooks, composants fonctionnels
- **Node.js** : Middleware, callbacks d'API
- **Programmation réactive** : RxJS, streams

---

## **PROJET MINI**

### Défi pratique : Système de notification avancé
**Objectif :** Créer un système de notifications avec toutes les techniques apprises

**Fonctionnalités requises :**
- [ ] Factory pour créer différents types de notifications
- [ ] Système d'abonnement avec callbacks
- [ ] Debounce pour éviter le spam
- [ ] Composition de filtres
- [ ] Interface fluide (method chaining)

**Structure suggérée :**
```javascript
// Usage attendu:
const notifier = creerSystemeNotification()
    .avecDebounce(500)
    .avecFiltre(notification => notification.priorite >= 2)
    .surEvenement('nouvelle', callback => { /* ... */ });

notifier.envoyer('info', 'Message test', {priorite: 3});
```

**Temps estimé :** 90 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Closures](https://developer.mozilla.org/fr/docs/Web/JavaScript/Closures)
- [MDN - Arrow Functions](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [MDN - Array Methods](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array)

### Articles recommandés
- "Understanding Closures in JavaScript" - javascript.info
- "Functional Programming in JavaScript" - FunFunFunction
- "Debouncing vs Throttling" - CSS-Tricks

### Outils et librairies
- [Lodash](https://lodash.com/) - Utilitaires fonctionnels
- [Ramda](https://ramdajs.com/) - Programmation fonctionnelle pure
- [RxJS](https://rxjs.dev/) - Programmation réactive

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Qu'est-ce qu'une closure et pourquoi est-elle utile ?**
   - Réponse : Une fonction qui capture son environnement lexical, utile pour l'encapsulation et les factory functions

2. **Quelle est la principale différence entre function et arrow function ?**
   - Réponse : Les arrow functions n'ont pas leur propre `this` et `arguments`

3. **Que fait la méthode reduce() ?**
   - Réponse : Elle réduit un array à une seule valeur en appliquant une fonction d'accumulation

### Checklist de maîtrise
- [ ] Je comprends et utilise les closures efficacement
- [ ] Je maîtrise les différences entre function et arrow function
- [ ] Je peux implémenter des callbacks et higher-order functions
- [ ] Je compose des fonctions pour créer des pipelines de traitement
- [ ] Je reconnais quand utiliser chaque technique

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 : Confusion avec `this` dans les arrow functions
**Code problématique :**
```javascript
const objet = {
    nom: "Test",
    methode: () => {
        console.log(this.nom); // undefined (this ne pointe pas sur objet)
    }
};
```

**Solution :**
```javascript
const objet = {
    nom: "Test",
    methode: function() {
        console.log(this.nom); // "Test"
    }
};
```

### Erreur fréquente 2 : Closure dans une boucle
**Code problématique :**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // Affiche 3, 3, 3
}
```

**Solution :**
```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // Affiche 0, 1, 2
}
```

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les closures créent des "environnements privés"
- Les arrow functions simplifient la syntaxe mais changent le comportement de `this`
- La composition de fonctions permet un code plus modulaire

### Questions à approfondir
- Performance des closures vs classes
- Patterns avancés de programmation fonctionnelle

### Révision programmée
- [ ] Révision dans 1 jour (refaire les exemples de closure)
- [ ] Révision dans 1 semaine (créer un projet avec composition)
- [ ] Révision dans 1 mois (approfondir avec async/await)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #fonctions-avancees #closures #callbacks #arrow-functions #intermediaire

**Temps de completion :** [Votre temps réel]

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 10/126*
