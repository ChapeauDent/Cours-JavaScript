# Closures et Lexical Scope en JavaScript

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** Closures et Portée Lexicale (Concepts Fondamentaux)
- **Niveau :** Intermédiaire (concept clé)
- **Durée estimée :** 120 minutes
- **Prérequis :** Fonctions, hoisting, portées (var/let/const), fonctions imbriquées
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est une closure et pourquoi elle existe
- [ ] **Maîtriser** : Le lexical scope (portée lexicale)
- [ ] **Créer** : Des fonctions avec données privées
- [ ] **Appliquer** : Les patterns courants utilisant les closures
- [ ] **Déboguer** : Les problèmes liés aux closures et à la mémoire

### Mots-clés à retenir
- **Closure** : Fonction qui "capture" des variables de son environnement
- **Lexical Scope** : Portée déterminée par où les variables sont déclarées
- **Environnement lexical** : Contexte contenant les variables accessibles
- **Capture** : Conservation d'une référence vers une variable externe
- **Factory Function** : Fonction qui crée d'autres fonctions avec closures

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les closures sont **LA** caractéristique qui rend JavaScript unique et puissant. C'est ce qui permet :
- ✅ **Encapsulation** : Créer des données privées
- ✅ **Factory functions** : Fonctions qui créent d'autres fonctions
- ✅ **Callbacks avancés** : Préserver le contexte lors d'appels asynchrones
- ✅ **Modules** : Organiser le code avec des APIs publiques/privées
- ✅ **React/Vue** : Hooks, composants, state management

**Sans closures**, JavaScript serait juste un langage de script basique !

### Définition et concepts clés

#### **1. Lexical Scope (Portée Lexicale)**

Le **lexical scope** signifie que la portée d'une variable est déterminée par **où elle est déclarée** dans le code, pas par où elle est appelée.

```javascript
// Exemple de base
let message = "Je suis globale";

function externe() {
    let secret = "Je suis dans externe()";
    
    function interne() {
        // interne() "voit" les variables de externe() et globales
        console.log(message); // ✅ Accès à la variable globale
        console.log(secret);  // ✅ Accès à la variable de externe()
    }
    
    return interne;
}

let maFonction = externe();
maFonction(); // Affiche: "Je suis globale" puis "Je suis dans externe()"
```

#### **2. Qu'est-ce qu'une Closure ?**

Une **closure** = fonction + environnement lexical dans lequel elle a été créée.

```javascript
function creerCompteur() {
    let compteur = 0; // Variable "capturée" par la closure
    
    return function() {
        compteur++; // La fonction "se souvient" de cette variable
        return compteur;
    };
}

let monCompteur = creerCompteur();
console.log(monCompteur()); // 1
console.log(monCompteur()); // 2
console.log(monCompteur()); // 3

// La variable 'compteur' est toujours accessible !
// C'est ça, une closure !
```

#### **3. Chaîne de portées (Scope Chain)**

JavaScript remonte la chaîne des portées pour trouver une variable :

```javascript
let niveau1 = "Global";

function fonctionA() {
    let niveau2 = "Fonction A";
    
    function fonctionB() {
        let niveau3 = "Fonction B";
        
        function fonctionC() {
            // Recherche dans l'ordre :
            // 1. Portée locale (fonctionC)
            // 2. Portée parent (fonctionB) 
            // 3. Portée grand-parent (fonctionA)
            // 4. Portée globale
            
            console.log(niveau3); // Trouvée en 1
            console.log(niveau2); // Trouvée en 3
            console.log(niveau1); // Trouvée en 4
        }
        
        return fonctionC;
    }
    
    return fonctionB();
}

let maFonction = fonctionA();
maFonction(); // Accède à toutes les variables grâce aux closures !
```

### Syntaxe et patterns courants

#### **Pattern 1 : Factory Functions**
```javascript
// Créer des fonctions personnalisées
function creerSalutation(prefixe) {
    return function(nom) {
        return `${prefixe} ${nom}!`;
    };
}

let salutationFormelle = creerSalutation("Bonjour");
let salutationAmicale = creerSalutation("Salut");

console.log(salutationFormelle("Madame Dupont")); // "Bonjour Madame Dupont!"
console.log(salutationAmicale("Paul")); // "Salut Paul!"
```

#### **Pattern 2 : Variables privées**
```javascript
function creerBanque() {
    let solde = 0; // Variable privée !
    
    return {
        deposer: function(montant) {
            if (montant > 0) {
                solde += montant;
                return `Dépôt de ${montant}€. Nouveau solde: ${solde}€`;
            }
            return "Montant invalide";
        },
        
        retirer: function(montant) {
            if (montant > 0 && montant <= solde) {
                solde -= montant;
                return `Retrait de ${montant}€. Nouveau solde: ${solde}€`;
            }
            return "Retrait impossible";
        },
        
        consulter: function() {
            return `Solde actuel: ${solde}€`;
        }
        
        // Note: Pas d'accès direct à 'solde' !
    };
}

let monCompte = creerBanque();
console.log(monCompte.deposer(100)); // "Dépôt de 100€. Nouveau solde: 100€"
console.log(monCompte.retirer(30));  // "Retrait de 30€. Nouveau solde: 70€"
console.log(monCompte.consulter());  // "Solde actuel: 70€"

// Impossible d'accéder directement au solde
console.log(monCompte.solde); // undefined - protégé !
```

#### **Pattern 3 : Callbacks avec état**
```javascript
function creerTimer(nom) {
    let debut = Date.now();
    
    return function(action) {
        let maintenant = Date.now();
        let duree = maintenant - debut;
        
        console.log(`[${nom}] ${action} après ${duree}ms`);
        
        if (action === "reset") {
            debut = Date.now();
        }
    };
}

let timer1 = creerTimer("Process 1");
let timer2 = creerTimer("Process 2");

setTimeout(() => timer1("Première étape"), 100);
setTimeout(() => timer2("Démarrage"), 150);
setTimeout(() => timer1("Fin"), 200);
```

### Points d'attention et pièges

#### **Piège 1 : Boucles et closures**
```javascript
// PROBLÈME CLASSIQUE
var fonctions = [];
for (var i = 0; i < 3; i++) {
    fonctions[i] = function() {
        console.log(i); // Toutes affichent 3 !
    };
}

fonctions[0](); // 3
fonctions[1](); // 3  
fonctions[2](); // 3

// SOLUTION 1: Utiliser let (crée une nouvelle liaison)
var fonctions2 = [];
for (let i = 0; i < 3; i++) {
    fonctions2[i] = function() {
        console.log(i); // Affichent 0, 1, 2 ✅
    };
}

// SOLUTION 2: IIFE pour capturer la valeur
var fonctions3 = [];
for (var i = 0; i < 3; i++) {
    fonctions3[i] = (function(index) {
        return function() {
            console.log(index); // Affichent 0, 1, 2 ✅
        };
    })(i);
}
```

#### **Piège 2 : Fuites mémoire**
```javascript
// PROBLÈME : Référence circulaire
function creerProbleme() {
    let grossesDonnees = new Array(1000000).fill("data");
    
    return function() {
        // Cette closure garde une référence vers grossesDonnees
        // même si on ne l'utilise pas !
        console.log("Function appelée");
    };
}

// SOLUTION : Nettoyer les références inutiles
function creerSolution() {
    let grossesDonnees = new Array(1000000).fill("data");
    let petiteInfo = grossesDonnees.length; // Extraire ce qu'on a besoin
    
    grossesDonnees = null; // Nettoyer la référence
    
    return function() {
        console.log(`Taille était: ${petiteInfo}`);
    };
}
```

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Démonstration basique des closures
console.log("=== DÉMONSTRATION CLOSURES ===");

// Exemple 1: Compteur simple
function exempleCompteur() {
    let compteur = 0;
    
    return function incrementer() {
        compteur++;
        console.log(`Compteur: ${compteur}`);
        return compteur;
    };
}

let compteur1 = exempleCompteur();
let compteur2 = exempleCompteur();

console.log("Compteur 1:");
compteur1(); // 1
compteur1(); // 2
compteur1(); // 3

console.log("Compteur 2 (indépendant):");
compteur2(); // 1
compteur2(); // 2

// Exemple 2: Configuration avec closure
function creerConfiguration(environnement) {
    const config = {
        dev: { apiUrl: "http://localhost:3000", debug: true },
        prod: { apiUrl: "https://api.monsite.com", debug: false }
    };
    
    const configActuelle = config[environnement];
    
    return {
        getApiUrl: function() {
            return configActuelle.apiUrl;
        },
        isDebug: function() {
            return configActuelle.debug;
        },
        info: function() {
            return `Environnement: ${environnement}`;
        }
    };
}

let configDev = creerConfiguration("dev");
let configProd = creerConfiguration("prod");

console.log("Config Dev:", configDev.getApiUrl()); // http://localhost:3000
console.log("Config Prod:", configProd.getApiUrl()); // https://api.monsite.com
console.log("Debug Dev:", configDev.isDebug()); // true
console.log("Debug Prod:", configProd.isDebug()); // false
```

### Exemple intermédiaire
```javascript
// Gestionnaire d'événements avec closures
function creerGestionnaireEvenements() {
    console.log("=== GESTIONNAIRE D'ÉVÉNEMENTS ===");
    
    let evenements = {}; // Stockage privé des événements
    let compteurEvenements = 0;
    
    // Fonction pour enregistrer un événement
    function on(nom, callback) {
        if (!evenements[nom]) {
            evenements[nom] = [];
        }
        
        // Chaque callback garde une référence vers son ID
        let callbackId = ++compteurEvenements;
        
        // Wrapper qui capture l'ID et le callback
        let wrapper = function(...args) {
            console.log(`[Événement ${callbackId}] ${nom} déclenché`);
            return callback.apply(this, args);
        };
        
        wrapper.id = callbackId;
        evenements[nom].push(wrapper);
        
        console.log(`Événement '${nom}' enregistré avec ID ${callbackId}`);
        return callbackId;
    }
    
    // Fonction pour déclencher un événement
    function emit(nom, ...args) {
        if (evenements[nom]) {
            console.log(`Déclenchement de '${nom}' avec ${evenements[nom].length} listener(s)`);
            evenements[nom].forEach(callback => {
                callback(...args);
            });
        } else {
            console.log(`Aucun listener pour '${nom}'`);
        }
    }
    
    // Fonction pour supprimer un événement
    function off(nom, callbackId) {
        if (evenements[nom]) {
            let index = evenements[nom].findIndex(cb => cb.id === callbackId);
            if (index !== -1) {
                evenements[nom].splice(index, 1);
                console.log(`Listener ${callbackId} supprimé pour '${nom}'`);
            }
        }
    }
    
    // Fonction pour voir l'état
    function debug() {
        console.log("État des événements:", evenements);
        Object.keys(evenements).forEach(nom => {
            console.log(`- ${nom}: ${evenements[nom].length} listener(s)`);
        });
    }
    
    // API publique
    return { on, emit, off, debug };
}

// Test du gestionnaire
let gestionnaire = creerGestionnaireEvenements();

// Enregistrement d'événements
let id1 = gestionnaire.on('userLogin', (nom) => {
    console.log(`👋 Bienvenue ${nom}!`);
});

let id2 = gestionnaire.on('userLogin', (nom) => {
    console.log(`📊 Connexion de ${nom} enregistrée`);
});

gestionnaire.on('userLogout', (nom) => {
    console.log(`👋 Au revoir ${nom}!`);
});

// Déclenchement d'événements
gestionnaire.emit('userLogin', 'Alice');
gestionnaire.emit('userLogout', 'Alice');

// Suppression d'un listener
gestionnaire.off('userLogin', id1);
gestionnaire.emit('userLogin', 'Bob'); // Un seul listener maintenant

gestionnaire.debug();
```

### Exemple avancé (Module pattern)
```javascript
// Module avancé avec closures pour un système de cache
const CacheModule = (function() {
    console.log("=== MODULE CACHE AVANCÉ ===");
    
    // Variables privées du module
    let cache = new Map();
    let maxSize = 100;
    let hits = 0;
    let misses = 0;
    let accessLog = [];
    
    // Fonction privée pour nettoyer le cache
    function cleanup() {
        if (cache.size >= maxSize) {
            let oldestKey = cache.keys().next().value;
            cache.delete(oldestKey);
            console.log(`🧹 Cache nettoyé, suppression de: ${oldestKey}`);
        }
    }
    
    // Fonction privée pour logger les accès
    function logAccess(key, type) {
        accessLog.push({
            key: key,
            type: type, // 'hit' ou 'miss'
            timestamp: Date.now()
        });
        
        // Garder seulement les 50 derniers accès
        if (accessLog.length > 50) {
            accessLog.shift();
        }
    }
    
    // Fonction publique pour configurer la taille max
    function setMaxSize(newSize) {
        if (newSize > 0) {
            maxSize = newSize;
            console.log(`📏 Taille max du cache: ${maxSize}`);
            
            // Nettoyer si nécessaire
            while (cache.size > maxSize) {
                cleanup();
            }
        }
    }
    
    // Fonction publique pour stocker une valeur
    function set(key, value, ttl = null) {
        cleanup(); // Nettoyer avant d'ajouter
        
        let item = {
            value: value,
            created: Date.now(),
            accessed: Date.now(),
            ttl: ttl
        };
        
        cache.set(key, item);
        console.log(`💾 Stocké: ${key}`);
    }
    
    // Fonction publique pour récupérer une valeur
    function get(key) {
        let item = cache.get(key);
        
        if (!item) {
            misses++;
            logAccess(key, 'miss');
            console.log(`❌ Cache miss: ${key}`);
            return null;
        }
        
        // Vérifier TTL
        if (item.ttl && (Date.now() - item.created) > item.ttl) {
            cache.delete(key);
            misses++;
            logAccess(key, 'miss');
            console.log(`⏰ TTL expiré: ${key}`);
            return null;
        }
        
        // Mettre à jour l'accès
        item.accessed = Date.now();
        hits++;
        logAccess(key, 'hit');
        console.log(`✅ Cache hit: ${key}`);
        
        return item.value;
    }
    
    // Fonction publique pour les statistiques
    function getStats() {
        let hitRate = hits + misses > 0 ? (hits / (hits + misses) * 100).toFixed(2) : 0;
        
        return {
            size: cache.size,
            maxSize: maxSize,
            hits: hits,
            misses: misses,
            hitRate: `${hitRate}%`,
            recentAccess: accessLog.slice(-10) // 10 derniers accès
        };
    }
    
    // Fonction publique pour vider le cache
    function clear() {
        cache.clear();
        hits = 0;
        misses = 0;
        accessLog = [];
        console.log("🗑️ Cache vidé");
    }
    
    // API publique (seules ces fonctions sont accessibles)
    return {
        set: set,
        get: get,
        setMaxSize: setMaxSize,
        getStats: getStats,
        clear: clear
    };
})(); // IIFE pour créer le module immédiatement

// Test du module cache
console.log("Test du cache:");

CacheModule.set("user:1", { nom: "Alice", age: 25 });
CacheModule.set("user:2", { nom: "Bob", age: 30 });
CacheModule.set("config", { theme: "dark", lang: "fr" });

// Accès aux données
console.log("Données user:1:", CacheModule.get("user:1"));
console.log("Données inexistantes:", CacheModule.get("user:999"));
console.log("Config:", CacheModule.get("config"));

// Test TTL (Time To Live)
CacheModule.set("temp", "données temporaires", 1000); // Expire dans 1 seconde
console.log("Données temp:", CacheModule.get("temp"));

setTimeout(() => {
    console.log("Données temp après 1.5s:", CacheModule.get("temp"));
}, 1500);

// Statistiques
console.log("Statistiques:", CacheModule.getStats());

// Test de la limite de taille
CacheModule.setMaxSize(2);
CacheModule.set("nouveau", "test"); // Devrait déclencher un nettoyage

console.log("Stats finales:", CacheModule.getStats());
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Créateur de fonctions mathématiques
**Consigne :** Créez une factory function qui génère des fonctions mathématiques personnalisées.

```javascript
function creerFonctionMath(operation) {
    // Votre code ici
    // Retourner une fonction qui applique l'opération
}

let double = creerFonctionMath("double");
let carre = creerFonctionMath("carre");
let increment = creerFonctionMath("increment");

console.log(double(5)); // 10
console.log(carre(4)); // 16
console.log(increment(10)); // 11
```

**Niveau de difficulté :** ⭐⭐☆☆☆

### Exercice 2 : Système de thèmes
**Consigne :** Créez un gestionnaire de thèmes avec closures pour mémoriser les préférences.

```javascript
function creerGestionnaireThemes() {
    // Variables privées pour les thèmes et le thème actuel
    // Fonctions pour changer, obtenir le thème, ajouter des thèmes
    
    return {
        setTheme: function(nom) { /* ... */ },
        getTheme: function() { /* ... */ },
        addTheme: function(nom, config) { /* ... */ },
        applyTheme: function() { /* ... */ }
    };
}
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 3 : Debounce function
**Consigne :** Implémentez une fonction debounce utilisant les closures.

```javascript
function debounce(func, delay) {
    // Utiliser une closure pour mémoriser le timer
    // Retourner une fonction qui retarde l'exécution
}

// Test
let searchFunction = debounce((query) => {
    console.log("Recherche:", query);
}, 300);

searchFunction("a");
searchFunction("ab");
searchFunction("abc"); // Seul celui-ci s'exécute après 300ms
```

**Niveau de difficulté :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function creerFonctionMath(operation) {
    // Chaque fonction créée "capture" l'opération
    return function(nombre) {
        switch(operation) {
            case "double":
                return nombre * 2;
            case "carre":
                return nombre * nombre;
            case "increment":
                return nombre + 1;
            case "decrement":
                return nombre - 1;
            case "inverse":
                return -nombre;
            default:
                return nombre;
        }
    };
}

// Version avancée avec opérations personnalisées
function creerFonctionMathAvancee(operation, ...params) {
    return function(nombre) {
        switch(operation) {
            case "multiplier":
                return nombre * (params[0] || 1);
            case "puissance":
                return Math.pow(nombre, params[0] || 2);
            case "ajouter":
                return nombre + (params[0] || 0);
            case "modulo":
                return nombre % (params[0] || 2);
            default:
                return nombre;
        }
    };
}

// Tests
let double = creerFonctionMath("double");
let carre = creerFonctionMath("carre");
let increment = creerFonctionMath("increment");

console.log("Double de 5:", double(5)); // 10
console.log("Carré de 4:", carre(4)); // 16
console.log("Incrément de 10:", increment(10)); // 11

// Tests avancés
let multiplierPar3 = creerFonctionMathAvancee("multiplier", 3);
let puissance3 = creerFonctionMathAvancee("puissance", 3);

console.log("5 × 3:", multiplierPar3(5)); // 15
console.log("2³:", puissance3(2)); // 8
```

### Solution Exercice 2
```javascript
function creerGestionnaireThemes() {
    // Variables privées capturées par les closures
    let themes = {
        dark: {
            background: "#1a1a1a",
            text: "#ffffff",
            accent: "#4a90e2"
        },
        light: {
            background: "#ffffff", 
            text: "#000000",
            accent: "#007bff"
        },
        sunset: {
            background: "#ff6b35",
            text: "#ffffff", 
            accent: "#f7931e"
        }
    };
    
    let themeActuel = "light";
    let callbacks = []; // Callbacks à appeler lors du changement
    
    return {
        setTheme: function(nom) {
            if (themes[nom]) {
                let ancienTheme = themeActuel;
                themeActuel = nom;
                console.log(`🎨 Thème changé: ${ancienTheme} → ${nom}`);
                
                // Notifier les callbacks
                callbacks.forEach(callback => {
                    callback(nom, themes[nom], ancienTheme);
                });
                
                return true;
            } else {
                console.log(`❌ Thème '${nom}' introuvable`);
                return false;
            }
        },
        
        getTheme: function() {
            return {
                nom: themeActuel,
                config: themes[themeActuel]
            };
        },
        
        addTheme: function(nom, config) {
            if (config && config.background && config.text) {
                themes[nom] = config;
                console.log(`➕ Nouveau thème ajouté: ${nom}`);
                return true;
            } else {
                console.log("❌ Configuration thème invalide");
                return false;
            }
        },
        
        getAvailableThemes: function() {
            return Object.keys(themes);
        },
        
        applyTheme: function() {
            let config = themes[themeActuel];
            
            // Simulation d'application du thème
            console.log(`🎨 Application du thème '${themeActuel}':`);
            console.log(`- Background: ${config.background}`);
            console.log(`- Text: ${config.text}`);
            console.log(`- Accent: ${config.accent}`);
            
            // Dans un vrai projet, on modifierait le CSS
            if (typeof document !== 'undefined') {
                document.body.style.backgroundColor = config.background;
                document.body.style.color = config.text;
            }
        },
        
        onChange: function(callback) {
            callbacks.push(callback);
            console.log(`🔔 Callback d'écoute ajouté`);
        }
    };
}

// Test du gestionnaire
let themeManager = creerGestionnaireThemes();

// Ajouter un callback d'écoute
themeManager.onChange((nouveauTheme, config, ancienTheme) => {
    console.log(`📢 Notification: Passage de ${ancienTheme} à ${nouveauTheme}`);
});

// Tests
console.log("Thème initial:", themeManager.getTheme());
console.log("Thèmes disponibles:", themeManager.getAvailableThemes());

themeManager.setTheme("dark");
themeManager.applyTheme();

// Ajouter un thème personnalisé
themeManager.addTheme("ocean", {
    background: "#006994",
    text: "#e1f5fe", 
    accent: "#00bcd4"
});

themeManager.setTheme("ocean");
themeManager.applyTheme();
```

### Solution Exercice 3
```javascript
function debounce(func, delay) {
    let timeoutId; // Variable capturée par la closure
    
    return function(...args) {
        // Capturer le contexte (this)
        let context = this;
        
        // Annuler le timer précédent s'il existe
        clearTimeout(timeoutId);
        
        // Créer un nouveau timer
        timeoutId = setTimeout(function() {
            func.apply(context, args);
        }, delay);
    };
}

// Version avancée avec option immediate
function debounceAvance(func, delay, immediate = false) {
    let timeoutId;
    
    return function(...args) {
        let context = this;
        let callNow = immediate && !timeoutId;
        
        clearTimeout(timeoutId);
        
        timeoutId = setTimeout(function() {
            timeoutId = null;
            if (!immediate) func.apply(context, args);
        }, delay);
        
        if (callNow) func.apply(context, args);
    };
}

// Fonction throttle bonus
function throttle(func, delay) {
    let lastCall = 0;
    let timeoutId;
    
    return function(...args) {
        let now = Date.now();
        let context = this;
        
        if (now - lastCall >= delay) {
            lastCall = now;
            func.apply(context, args);
        } else {
            clearTimeout(timeoutId);
            timeoutId = setTimeout(function() {
                lastCall = Date.now();
                func.apply(context, args);
            }, delay - (now - lastCall));
        }
    };
}

// Tests
console.log("=== TESTS DEBOUNCE ===");

let searchFunction = debounce((query) => {
    console.log(`🔍 Recherche: "${query}" à ${new Date().toLocaleTimeString()}`);
}, 300);

console.log("Saisie rapide simulée:");
searchFunction("a");
searchFunction("ab");
searchFunction("abc");
searchFunction("abcd"); // Seul celui-ci s'exécutera

// Test throttle
let scrollFunction = throttle(() => {
    console.log(`📜 Scroll event à ${new Date().toLocaleTimeString()}`);
}, 100);

console.log("\nSimulation de scroll (throttle):");
for(let i = 0; i < 5; i++) {
    setTimeout(() => scrollFunction(), i * 50); // Calls rapides
}
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Fonctions** : Déclaration, appel, paramètres
- **Portées** : var, let, const, hoisting
- **Objets** : Méthodes, propriétés
- **Arrays** : map, filter (utilisent des closures)

### Prochaines étapes importantes
- **Modules ES6** : Import/export avec encapsulation
- **Async/Await** : Closures dans le contexte asynchrone
- **React Hooks** : useState, useEffect utilisent les closures
- **Design Patterns** : Module, Factory, Observer avec closures

### Concepts complémentaires
- **IIFE** : Immediately Invoked Function Expression
- **Currying** : Transformation de fonctions avec closures
- **Memoization** : Cache de résultats avec closures

---

## **PROJET MINI**

### Défi pratique : Système de notifications avec closures
**Objectif :** Créer un gestionnaire de notifications avancé utilisant tous les concepts de closures.

**Fonctionnalités requises :**
- [ ] Système de priorités (info, warning, error)
- [ ] Auto-suppression après délai (configurable)
- [ ] Limite du nombre de notifications actives
- [ ] Callbacks de cycle de vie (onShow, onHide, onExpire)
- [ ] Groupement par type
- [ ] Persistance temporaire en cas de fermeture

**Structure suggérée :**
```
📁 notification-system/
├── 📄 index.html
├── 📄 notification-manager.js (closures)
├── 📄 ui-handler.js (interface)
└── 📄 demo.js (exemples)
```

**Temps estimé :** 150 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Closures](https://developer.mozilla.org/fr/docs/Web/JavaScript/Closures)
- [MDN - Lexical Scoping](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#lexical_scoping)

### Articles de référence
- "Master the JavaScript Interview: What is a Closure?" - Eric Elliott
- "Closures in JavaScript" - JavaScript.info (guide complet)
- "Understanding JavaScript Closures" - Dmitri Pavlutin

### Outils de pratique
- [JavaScript Visualizer](http://pythontutor.com/javascript.html) - Voir les closures en action
- [Closure Examples](https://github.com/getify/You-Dont-Know-JS) - You Don't Know JS

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Qu'est-ce qu'une closure ?**
   - Réponse : Une fonction + l'environnement lexical dans lequel elle a été créée

2. **Pourquoi ce code affiche-t-il 3, 3, 3 ?**
   ```javascript
   for (var i = 0; i < 3; i++) {
       setTimeout(() => console.log(i), 100);
   }
   ```
   - Réponse : Les closures capturent la même variable `i` qui vaut 3 à la fin de la boucle

3. **Comment créer des données privées en JavaScript ?**
   - Réponse : Utiliser des closures pour encapsuler les variables dans une portée inaccessible

### Checklist de maîtrise
- [ ] Je comprends la différence entre portée lexicale et dynamique
- [ ] Je peux expliquer ce qu'est une closure avec des exemples
- [ ] Je sais créer des factory functions
- [ ] Je maîtrise les variables privées avec closures
- [ ] Je reconnais et évite les pièges des boucles avec closures
- [ ] Je peux implémenter debounce/throttle
- [ ] Je comprends les implications mémoire des closures

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 : Variable partagée dans boucle
**Code problématique :**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
```

**Solution :**
```javascript
// Option 1: let
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}

// Option 2: IIFE
for (var i = 0; i < 3; i++) {
    (function(index) {
        setTimeout(() => console.log(index), 100);
    })(i);
}
```

### Erreur fréquente 2 : Fuite mémoire avec closures
**Code problématique :**
```javascript
function creerHandler() {
    let grossObjet = new Array(1000000).fill("data");
    
    return function() {
        console.log("Handler appelé");
        // grossObjet reste en mémoire même si inutilisé
    };
}
```

**Solution :**
```javascript
function creerHandler() {
    let grossObjet = new Array(1000000).fill("data");
    let info = grossObjet.length; // Extraire ce qu'on a besoin
    
    grossObjet = null; // Libérer la mémoire
    
    return function() {
        console.log(`Handler appelé, taille était: ${info}`);
    };
}
```

### Erreur fréquente 3 : Confusion this et closures
**Code problématique :**
```javascript
let obj = {
    nom: "Test",
    methode: function() {
        setTimeout(function() {
            console.log(this.nom); // undefined - this perdu
        }, 100);
    }
};
```

**Solution :**
```javascript
let obj = {
    nom: "Test",
    methode: function() {
        let self = this; // Capturer this
        setTimeout(function() {
            console.log(self.nom); // "Test"
        }, 100);
        
        // Ou utiliser arrow function
        setTimeout(() => {
            console.log(this.nom); // "Test" - this préservé
        }, 100);
    }
};
```

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les closures sont la clé pour comprendre JavaScript avancé
- Elles permettent l'encapsulation et les données privées
- Le lexical scope détermine quelles variables sont accessibles

### Questions à approfondir
- Performance des closures dans les applications importantes
- Liens avec les modules ES6 et l'encapsulation
- Utilisation des closures dans les frameworks modernes

### Révision programmée
- [ ] Révision dans 1 jour (refaire les exercices difficiles)
- [ ] Révision dans 1 semaine (créer des exemples personnels)
- [ ] Révision dans 1 mois (lien avec les modules et design patterns)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #closures #lexical-scope #encapsulation #factory-functions #variables-privees

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 7.5/126*
