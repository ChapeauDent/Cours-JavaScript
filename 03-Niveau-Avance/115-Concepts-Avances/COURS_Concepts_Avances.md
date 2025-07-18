# Concepts Avances JavaScript

## **INFORMATIONS GENERALES**

### Metadonnees du cours
- **Titre :** Concepts avances - Regex, Iterateurs, Generateurs, Proxy et WeakMap/WeakSet
- **Niveau :** Avance
- **Duree estimee :** 90 minutes
- **Prerequis :** Objets, fonctions, programmation asynchrone, modules
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## **OBJECTIFS PEDAGOGIQUES**

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Les expressions regulieres et leurs patterns
- [ ] **Appliquer** : Les iterateurs et generateurs pour le controle d'iteration
- [ ] **Analyser** : Proxy et Reflect pour la metaprogrammation
- [ ] **Creer** : Des applications utilisant WeakMap et WeakSet efficacement

### Mots-cles a retenir
- **Regex** : Pattern pour rechercher et manipuler du texte
- **Iterateur** : Objet qui implemente le protocole d'iteration
- **Generateur** : Fonction qui peut etre mise en pause et reprise
- **Proxy** : Wrapper qui intercepte les operations sur les objets
- **WeakMap/WeakSet** : Collections avec references faibles pour la gestion memoire

---

## **PARTIE THEORIQUE**

### Pourquoi ce concept est-il important ?
Ces concepts avances vous permettent d'ecrire du code plus puissant et flexible. Les regex simplifient le traitement de texte, les generateurs permettent un controle fin de l'iteration, Proxy ouvre la porte a la metaprogrammation, et WeakMap/WeakSet optimisent la gestion memoire dans certains cas specifiques.

### Definition et concepts cles

#### 1. Expressions Regulieres (Regex)
Pattern de recherche utilise pour matcher, extraire ou remplacer du texte :
```javascript
// Creation d'une regex
const pattern1 = /abc/; // Litterale
const pattern2 = new RegExp("abc"); // Constructeur

// Flags communs
const regex = /pattern/gim; // g=global, i=insensible casse, m=multiline
```

#### 2. Iterateurs et Generateurs
- **Iterateur** : Objet avec une methode `next()` qui retourne `{value, done}`
- **Generateur** : Fonction avec `function*` qui peut utiliser `yield`

#### 3. Proxy et Reflect
- **Proxy** : Intercepte et personnalise les operations (get, set, etc.)
- **Reflect** : API pour les operations reflexives sur les objets

#### 4. WeakMap et WeakSet
Collections avec references faibles - les objets peuvent etre garbage collected meme s'ils sont dans la collection.

### Syntaxe de base
```javascript
// Regex
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const isEmail = emailRegex.test("user@domain.com");

// Generateur
function* compteur() {
    let i = 0;
    while (true) {
        yield i++;
    }
}

// Proxy
const proxy = new Proxy(target, {
    get(obj, prop) {
        return prop in obj ? obj[prop] : "Propriete inconnue";
    }
});

// WeakMap
const weakMap = new WeakMap();
const obj = {};
weakMap.set(obj, "valeur associee");
```

### Points d'attention
- **Piege courant 1** : Regex trop complexes difficiles a maintenir
- **Piege courant 2** : Generateurs infinis sans controle d'arret
- **Bonne pratique** : Documenter les patterns regex complexes

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Validateur avec expressions regulieres
class ValidateurTexte {
    constructor() {
        // Patterns regex courants
        this.patterns = {
            email: /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/,
            telephone: /^(\+33|0)[1-9](\d{8})$/,
            codePostal: /^\d{5}$/,
            motDePasse: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/,
            url: /^https?:\/\/([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
        };
    }
    
    // Valider selon un type
    valider(type, valeur) {
        const pattern = this.patterns[type];
        if (!pattern) {
            throw new Error(`Type de validation inconnu: ${type}`);
        }
        
        return {
            valide: pattern.test(valeur),
            pattern: pattern.source,
            valeur
        };
    }
    
    // Extraire des informations d'un texte
    extraireEmails(texte) {
        const matches = texte.match(this.patterns.email.source.replace(/^\^|\$$/g, ''));
        return matches || [];
    }
    
    // Nettoyer un numero de telephone
    nettoyerTelephone(numero) {
        // Supprimer tout sauf les chiffres et le +
        return numero.replace(/[^\d+]/g, '');
    }
    
    // Masquer des donnees sensibles
    masquerEmail(email) {
        return email.replace(/^(.{2})[^@]*(@.*)$/, '$1***$2');
    }
}

// Generateur simple pour sequences
function* fibonacci() {
    let a = 0, b = 1;
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

// Demonstration
function demonstrationBase() {
    const validateur = new ValidateurTexte();
    
    // Tests de validation
    console.log("=== Tests de validation ===");
    const tests = [
        { type: "email", valeur: "user@example.com" },
        { type: "email", valeur: "invalid-email" },
        { type: "telephone", valeur: "0123456789" },
        { type: "motDePasse", valeur: "MonMotDePasse123!" }
    ];
    
    tests.forEach(test => {
        const resultat = validateur.valider(test.type, test.valeur);
        console.log(`${test.type}: ${test.valeur} -> ${resultat.valide ? "✓" : "✗"}`);
    });
    
    // Test generateur Fibonacci
    console.log("\n=== Suite de Fibonacci ===");
    const fib = fibonacci();
    for (let i = 0; i < 10; i++) {
        console.log(`F(${i}) = ${fib.next().value}`);
    }
    
    // Test masquage email
    console.log("\n=== Masquage email ===");
    console.log(validateur.masquerEmail("alice.dupont@example.com"));
}

demonstrationBase();
```

**Resultat attendu :**
```
=== Tests de validation ===
email: user@example.com -> ✓
email: invalid-email -> ✗
telephone: 0123456789 -> ✓
motDePasse: MonMotDePasse123! -> ✓

=== Suite de Fibonacci ===
F(0) = 0
F(1) = 1
F(2) = 1
F(3) = 2
F(4) = 3
...
```

### Exemple intermediaire
```javascript
// Systeme d'observateur avec Proxy
class ObservableObject {
    constructor(target = {}) {
        this.observers = new Set();
        this.history = [];
        
        return new Proxy(target, {
            get: (obj, prop) => {
                // Intercepter l'acces aux proprietes
                if (prop === 'addObserver') return this.addObserver.bind(this);
                if (prop === 'removeObserver') return this.removeObserver.bind(this);
                if (prop === 'getHistory') return () => this.history;
                
                // Enregistrer l'acces
                this.notifyObservers('get', prop, obj[prop]);
                return obj[prop];
            },
            
            set: (obj, prop, value) => {
                const oldValue = obj[prop];
                obj[prop] = value;
                
                // Enregistrer le changement
                this.history.push({
                    operation: 'set',
                    property: prop,
                    oldValue,
                    newValue: value,
                    timestamp: new Date()
                });
                
                // Notifier les observateurs
                this.notifyObservers('set', prop, value, oldValue);
                return true;
            },
            
            deleteProperty: (obj, prop) => {
                if (prop in obj) {
                    const oldValue = obj[prop];
                    delete obj[prop];
                    
                    this.history.push({
                        operation: 'delete',
                        property: prop,
                        oldValue,
                        timestamp: new Date()
                    });
                    
                    this.notifyObservers('delete', prop, undefined, oldValue);
                }
                return true;
            }
        });
    }
    
    addObserver(callback) {
        this.observers.add(callback);
    }
    
    removeObserver(callback) {
        this.observers.delete(callback);
    }
    
    notifyObservers(operation, property, newValue, oldValue) {
        this.observers.forEach(observer => {
            try {
                observer({ operation, property, newValue, oldValue });
            } catch (error) {
                console.error('Erreur dans observateur:', error);
            }
        });
    }
}

// Gestionnaire d'iteration personnalise
class IterateurPersonnalise {
    constructor(data, options = {}) {
        this.data = Array.isArray(data) ? data : Object.entries(data);
        this.index = 0;
        this.filter = options.filter || (() => true);
        this.transform = options.transform || (x => x);
        this.reverse = options.reverse || false;
        
        if (this.reverse) {
            this.data = this.data.slice().reverse();
        }
    }
    
    [Symbol.iterator]() {
        return this;
    }
    
    next() {
        while (this.index < this.data.length) {
            const value = this.data[this.index++];
            
            if (this.filter(value)) {
                return {
                    value: this.transform(value),
                    done: false
                };
            }
        }
        
        return { done: true };
    }
}

// Generateur pour traitement de donnees
function* processeurDonnees(donnees, batchSize = 5) {
    for (let i = 0; i < donnees.length; i += batchSize) {
        const batch = donnees.slice(i, i + batchSize);
        
        // Simuler un traitement asynchrone
        yield new Promise(resolve => {
            setTimeout(() => {
                const processed = batch.map(item => ({
                    ...item,
                    processed: true,
                    processedAt: new Date()
                }));
                resolve(processed);
            }, 100);
        });
    }
}

// Cache avec WeakMap pour eviter les fuites memoire
class CacheAvecWeakMap {
    constructor() {
        this.cache = new WeakMap();
        this.metadata = new Map(); // Pour les stats
    }
    
    // Calculer et cacher le resultat d'une fonction
    memoize(fn) {
        return (obj, ...args) => {
            // Utiliser l'objet comme cle (reference faible)
            if (this.cache.has(obj)) {
                const cached = this.cache.get(obj);
                const key = JSON.stringify(args);
                
                if (cached.has(key)) {
                    this.metadata.set('hits', (this.metadata.get('hits') || 0) + 1);
                    return cached.get(key);
                }
            } else {
                this.cache.set(obj, new Map());
            }
            
            // Calculer et cacher
            const result = fn(obj, ...args);
            const objCache = this.cache.get(obj);
            objCache.set(JSON.stringify(args), result);
            
            this.metadata.set('misses', (this.metadata.get('misses') || 0) + 1);
            return result;
        };
    }
    
    getStats() {
        return {
            hits: this.metadata.get('hits') || 0,
            misses: this.metadata.get('misses') || 0,
            hitRate: (this.metadata.get('hits') || 0) / 
                    ((this.metadata.get('hits') || 0) + (this.metadata.get('misses') || 0))
        };
    }
}

// Demonstration avancee
async function demonstrationAvancee() {
    console.log("=== Observable Object ===");
    
    // Creer un objet observable
    const observable = new ObservableObject({ nom: "Alice", age: 25 });
    
    // Ajouter un observateur
    observable.addObserver(event => {
        console.log(`Changement detecte: ${event.operation} ${event.property}`);
    });
    
    // Modifier l'objet
    observable.nom = "Bob";
    observable.age = 30;
    observable.ville = "Paris";
    delete observable.age;
    
    console.log("Historique:", observable.getHistory());
    
    console.log("\n=== Iterateur personnalise ===");
    
    const donnees = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    const iterateur = new IterateurPersonnalise(donnees, {
        filter: x => x % 2 === 0, // Nombres pairs
        transform: x => x * x,    // Au carre
        reverse: true             // Ordre inverse
    });
    
    for (const valeur of iterateur) {
        console.log(`Valeur traitee: ${valeur}`);
    }
    
    console.log("\n=== Processeur de donnees (generateur) ===");
    
    const donneesATraiter = Array.from({ length: 12 }, (_, i) => ({ id: i, valeur: i * 2 }));
    const processeur = processeurDonnees(donneesATraiter, 3);
    
    for await (const batch of processeur) {
        const results = await batch;
        console.log(`Batch traite: ${results.length} elements`);
    }
    
    console.log("\n=== Cache avec WeakMap ===");
    
    const cache = new CacheAvecWeakMap();
    
    // Fonction couteuse a memoizer
    const calculerComplexe = cache.memoize((obj, multiplier) => {
        console.log(`Calcul complexe pour ${obj.name} * ${multiplier}`);
        return obj.value * multiplier * Math.random(); // Simulation calcul
    });
    
    const objet1 = { name: "Object1", value: 100 };
    const objet2 = { name: "Object2", value: 200 };
    
    // Premiers appels (cache miss)
    console.log("Resultat 1:", calculerComplexe(objet1, 2));
    console.log("Resultat 2:", calculerComplexe(objet2, 3));
    
    // Deuxiemes appels (cache hit)
    console.log("Resultat 1 (cache):", calculerComplexe(objet1, 2));
    console.log("Resultat 2 (cache):", calculerComplexe(objet2, 3));
    
    console.log("Stats cache:", cache.getStats());
}

demonstrationAvancee();
```

### Exemple avance (optionnel)
```javascript
// Meta-classe avec Proxy pour creation dynamique
class MetaClass {
    constructor(className, properties = {}, methods = {}) {
        this.className = className;
        this.properties = properties;
        this.methods = methods;
        this.instances = new WeakSet();
    }
    
    create(initialData = {}) {
        const instance = { ...initialData };
        
        // Ajouter les proprietes par defaut
        Object.entries(this.properties).forEach(([key, config]) => {
            if (!(key in instance)) {
                instance[key] = typeof config.default === 'function' 
                    ? config.default() 
                    : config.default;
            }
        });
        
        const proxy = new Proxy(instance, {
            get: (target, prop) => {
                // Methodes de la metaclasse
                if (prop in this.methods) {
                    return this.methods[prop].bind(target);
                }
                
                // Proprietes speciales
                if (prop === '__class__') return this.className;
                if (prop === '__instanceof__') return (cls) => cls === this;
                
                return target[prop];
            },
            
            set: (target, prop, value) => {
                // Valider selon la configuration
                if (prop in this.properties) {
                    const config = this.properties[prop];
                    
                    if (config.type && typeof value !== config.type) {
                        throw new Error(`${prop} doit etre de type ${config.type}`);
                    }
                    
                    if (config.validator && !config.validator(value)) {
                        throw new Error(`Valeur invalide pour ${prop}: ${value}`);
                    }
                }
                
                target[prop] = value;
                return true;
            }
        });
        
        this.instances.add(proxy);
        return proxy;
    }
    
    // Compter les instances actives
    getInstanceCount() {
        // Note: WeakSet ne permet pas de compter directement
        // Ceci est une approximation pour la demonstration
        return "Instances actives (nombre exact non disponible avec WeakSet)";
    }
}

// Generateur de mots de passe avec patterns regex
class GenerateurMotDePasse {
    constructor() {
        this.patterns = {
            minuscules: 'abcdefghijklmnopqrstuvwxyz',
            majuscules: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
            chiffres: '0123456789',
            symboles: '@$!%*?&'
        };
        
        this.regexValidation = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&]).{8,}$/;
    }
    
    * genererCandidats(longueur = 12, options = {}) {
        const {
            includeMinuscules = true,
            includeMajuscules = true,
            includeChiffres = true,
            includeSymboles = true,
            maxTentatives = 1000
        } = options;
        
        let charset = '';
        if (includeMinuscules) charset += this.patterns.minuscules;
        if (includeMajuscules) charset += this.patterns.majuscules;
        if (includeChiffres) charset += this.patterns.chiffres;
        if (includeSymboles) charset += this.patterns.symboles;
        
        if (!charset) {
            throw new Error('Au moins un type de caractere doit etre inclus');
        }
        
        let tentatives = 0;
        while (tentatives < maxTentatives) {
            let motDePasse = '';
            for (let i = 0; i < longueur; i++) {
                motDePasse += charset[Math.floor(Math.random() * charset.length)];
            }
            
            const force = this.evaluerForce(motDePasse);
            yield { motDePasse, force, tentative: ++tentatives };
            
            if (force.score >= 80) break; // Mot de passe suffisamment fort
        }
    }
    
    evaluerForce(motDePasse) {
        let score = 0;
        const checks = {
            longueur: motDePasse.length >= 8,
            minuscules: /[a-z]/.test(motDePasse),
            majuscules: /[A-Z]/.test(motDePasse),
            chiffres: /\d/.test(motDePasse),
            symboles: /[@$!%*?&]/.test(motDePasse),
            longueurBonus: motDePasse.length >= 12
        };
        
        // Calcul du score
        Object.values(checks).forEach(passed => {
            if (passed) score += 20;
        });
        
        // Penalites pour patterns communs
        if (/123|abc|password/i.test(motDePasse)) score -= 30;
        if (/(.)\1{2,}/.test(motDePasse)) score -= 20; // Caracteres repetitifs
        
        return {
            score: Math.max(0, Math.min(100, score)),
            checks,
            valideRegex: this.regexValidation.test(motDePasse)
        };
    }
}

// Demonstration complete
async function demonstrationComplete() {
    console.log("=== Meta-classe dynamique ===");
    
    // Definir une classe User avec metaclasse
    const UserClass = new MetaClass('User', {
        nom: { type: 'string', default: '' },
        age: { 
            type: 'number', 
            default: 0,
            validator: (val) => val >= 0 && val <= 150
        },
        email: {
            type: 'string',
            validator: (val) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(val)
        }
    }, {
        saluer() {
            return `Bonjour, je suis ${this.nom}`;
        },
        
        vieillir() {
            this.age++;
            return this.age;
        }
    });
    
    try {
        const user = UserClass.create({ nom: 'Alice', age: 25 });
        console.log('User cree:', user.nom, user.age);
        console.log('Salutation:', user.saluer());
        console.log('Apres vieillissement:', user.vieillir());
        
        // Test validation
        user.email = 'alice@example.com';
        console.log('Email valide assigne');
        
        // Ceci devrait lever une erreur
        // user.age = -5;
        
    } catch (error) {
        console.error('Erreur metaclasse:', error.message);
    }
    
    console.log("\n=== Generateur de mots de passe ===");
    
    const generateur = new GenerateurMotDePasse();
    const candidats = generateur.genererCandidats(10, {
        includeSymboles: true
    });
    
    let meilleurMotDePasse = null;
    let meilleureForce = 0;
    
    for (const candidat of candidats) {
        console.log(`Tentative ${candidat.tentative}: ${candidat.motDePasse} (Score: ${candidat.force.score})`);
        
        if (candidat.force.score > meilleureForce) {
            meilleureForce = candidat.force.score;
            meilleurMotDePasse = candidat;
        }
        
        // Arreter apres 5 tentatives pour la demo
        if (candidat.tentative >= 5) break;
    }
    
    console.log('\nMeilleur mot de passe:', meilleurMotDePasse.motDePasse);
    console.log('Details force:', meilleurMotDePasse.force);
}

demonstrationComplete();
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Parseur de logs avec regex
**Consigne :** Creez un analyseur de fichiers de logs qui extrait les informations importantes avec des expressions regulieres.

**Code de depart :**
```javascript
class AnalyseurLogs {
    constructor() {
        // TODO: Definir les patterns regex pour differents types de logs
        this.patterns = {
            // Format: [2025-07-17 10:30:45] ERROR User not found (ID: 123)
            error: null,
            // Format: 192.168.1.1 - - [17/Jul/2025:10:30:45] "GET /api/users HTTP/1.1" 200 1234
            access: null,
            // Format: DEBUG database.js:42 Query executed in 150ms
            debug: null
        };
    }
    
    analyser(lignesLog) {
        // TODO: Analyser chaque ligne et extraire les informations
    }
    
    genererRapport(donnees) {
        // TODO: Generer un rapport statistique
    }
}

// Test avec des logs d'exemple
const logs = [
    "[2025-07-17 10:30:45] ERROR User not found (ID: 123)",
    "192.168.1.1 - - [17/Jul/2025:10:30:45] \"GET /api/users HTTP/1.1\" 200 1234",
    "DEBUG database.js:42 Query executed in 150ms"
];
```

**Niveau de difficulte :** ⭐⭐⭐☆☆

### Exercice 2 : Systeme de cache intelligent avec Proxy
**Consigne :** Implementez un cache qui intercepte automatiquement les appels de methodes et met en cache les resultats.

**Contraintes :**
- Utiliser Proxy pour intercepter les appels
- Gestion de l'expiration du cache
- Possibilite d'invalider le cache pour certaines methodes

**Niveau de difficulte :** ⭐⭐⭐⭐☆

### Exercice 3 : Generateur de donnees de test (optionnel)
**Consigne :** Creez un systeme de generation de donnees de test avec generateurs et regex pour valider les formats.

**Niveau de difficulte :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
class AnalyseurLogs {
    constructor() {
        this.patterns = {
            // [2025-07-17 10:30:45] ERROR User not found (ID: 123)
            error: /^\[(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})\] (ERROR|WARN|INFO) (.+?)(?:\s\((.+?)\))?$/,
            
            // 192.168.1.1 - - [17/Jul/2025:10:30:45] "GET /api/users HTTP/1.1" 200 1234
            access: /^(\d+\.\d+\.\d+\.\d+) - - \[(.+?)\] "(\w+) (.+?) HTTP\/[\d.]+" (\d+) (\d+)$/,
            
            // DEBUG database.js:42 Query executed in 150ms
            debug: /^(DEBUG|INFO|TRACE) (.+?):(\d+) (.+)$/
        };
        
        this.statistiques = {
            total: 0,
            parType: {},
            erreurs: [],
            performance: [],
            acces: []
        };
    }
    
    analyser(lignesLog) {
        const resultats = [];
        
        lignesLog.forEach((ligne, index) => {
            this.statistiques.total++;
            
            let match = null;
            let type = null;
            let donnees = null;
            
            // Tester chaque pattern
            for (const [typeName, pattern] of Object.entries(this.patterns)) {
                match = ligne.match(pattern);
                if (match) {
                    type = typeName;
                    donnees = this.extraireDonnees(type, match);
                    break;
                }
            }
            
            if (donnees) {
                // Compter par type
                this.statistiques.parType[type] = (this.statistiques.parType[type] || 0) + 1;
                
                // Categoriser pour statistiques
                this.categoriserDonnees(type, donnees);
                
                resultats.push({
                    ligne: index + 1,
                    type,
                    donnees,
                    ligneOriginale: ligne
                });
            } else {
                resultats.push({
                    ligne: index + 1,
                    type: 'inconnu',
                    ligneOriginale: ligne
                });
            }
        });
        
        return resultats;
    }
    
    extraireDonnees(type, match) {
        switch (type) {
            case 'error':
                return {
                    timestamp: match[1],
                    niveau: match[2],
                    message: match[3],
                    details: match[4] || null
                };
                
            case 'access':
                return {
                    ip: match[1],
                    timestamp: match[2],
                    methode: match[3],
                    url: match[4],
                    statut: parseInt(match[5]),
                    taille: parseInt(match[6])
                };
                
            case 'debug':
                return {
                    niveau: match[1],
                    fichier: match[2],
                    ligne: parseInt(match[3]),
                    message: match[4]
                };
                
            default:
                return null;
        }
    }
    
    categoriserDonnees(type, donnees) {
        switch (type) {
            case 'error':
                if (['ERROR', 'WARN'].includes(donnees.niveau)) {
                    this.statistiques.erreurs.push(donnees);
                }
                break;
                
            case 'access':
                this.statistiques.acces.push(donnees);
                break;
                
            case 'debug':
                // Extraire les metriques de performance
                const perfMatch = donnees.message.match(/(\d+)ms/);
                if (perfMatch) {
                    this.statistiques.performance.push({
                        fichier: donnees.fichier,
                        duree: parseInt(perfMatch[1]),
                        message: donnees.message
                    });
                }
                break;
        }
    }
    
    genererRapport(donnees) {
        const rapport = {
            resume: {
                totalLignes: this.statistiques.total,
                lignesAnalysees: donnees.filter(d => d.type !== 'inconnu').length,
                repartitionTypes: this.statistiques.parType
            },
            
            erreurs: {
                nombre: this.statistiques.erreurs.length,
                parNiveau: this.grouperPar(this.statistiques.erreurs, 'niveau'),
                dernieresErreurs: this.statistiques.erreurs.slice(-5)
            },
            
            acces: {
                nombre: this.statistiques.acces.length,
                parStatut: this.grouperPar(this.statistiques.acces, 'statut'),
                topIPs: this.topValeurs(this.statistiques.acces, 'ip', 5),
                topURLs: this.topValeurs(this.statistiques.acces, 'url', 5)
            },
            
            performance: {
                nombreMetriques: this.statistiques.performance.length,
                moyenneDuree: this.calculerMoyenne(this.statistiques.performance, 'duree'),
                maxDuree: Math.max(...this.statistiques.performance.map(p => p.duree)),
                parFichier: this.grouperPar(this.statistiques.performance, 'fichier')
            }
        };
        
        return rapport;
    }
    
    grouperPar(array, propriete) {
        return array.reduce((acc, item) => {
            const key = item[propriete];
            acc[key] = (acc[key] || 0) + 1;
            return acc;
        }, {});
    }
    
    topValeurs(array, propriete, limite) {
        const comptes = this.grouperPar(array, propriete);
        return Object.entries(comptes)
            .sort(([,a], [,b]) => b - a)
            .slice(0, limite)
            .map(([valeur, compte]) => ({ valeur, compte }));
    }
    
    calculerMoyenne(array, propriete) {
        if (array.length === 0) return 0;
        const somme = array.reduce((acc, item) => acc + item[propriete], 0);
        return Math.round(somme / array.length);
    }
}

// Demonstration
function demonstrationAnalyseurLogs() {
    const analyseur = new AnalyseurLogs();
    
    const logsTest = [
        "[2025-07-17 10:30:45] ERROR User not found (ID: 123)",
        "[2025-07-17 10:31:12] WARN Database connection slow",
        "192.168.1.100 - - [17/Jul/2025:10:30:45] \"GET /api/users HTTP/1.1\" 200 1234",
        "192.168.1.101 - - [17/Jul/2025:10:31:15] \"POST /api/login HTTP/1.1\" 401 89",
        "DEBUG database.js:42 Query executed in 150ms",
        "INFO auth.js:28 User logged in successfully",
        "TRACE cache.js:15 Cache hit in 2ms",
        "[2025-07-17 10:32:00] ERROR Database connection failed (Connection timeout)",
        "192.168.1.100 - - [17/Jul/2025:10:32:30] \"GET /api/dashboard HTTP/1.1\" 500 156",
        "Cette ligne ne correspond a aucun pattern"
    ];
    
    console.log("=== Analyse des logs ===");
    const resultats = analyseur.analyser(logsTest);
    
    console.log("\n=== Resultats d'analyse ===");
    resultats.forEach(resultat => {
        if (resultat.type !== 'inconnu') {
            console.log(`Ligne ${resultat.ligne} (${resultat.type}):`, JSON.stringify(resultat.donnees, null, 2));
        } else {
            console.log(`Ligne ${resultat.ligne}: Format non reconnu`);
        }
    });
    
    console.log("\n=== Rapport statistique ===");
    const rapport = analyseur.genererRapport(resultats);
    console.log(JSON.stringify(rapport, null, 2));
}

demonstrationAnalyseurLogs();
```

**Explication :** Cette solution utilise des regex pour parser differents formats de logs et genere des statistiques detaillees.

### Solution Exercice 2
```javascript
class CacheIntelligent {
    constructor(options = {}) {
        this.cache = new Map();
        this.metadata = new Map();
        this.defaultTTL = options.ttl || 300000; // 5 minutes
        this.maxSize = options.maxSize || 100;
        this.stats = {
            hits: 0,
            misses: 0,
            invalidations: 0
        };
        
        // Nettoyage automatique
        this.cleanupInterval = setInterval(() => this.cleanup(), 60000);
    }
    
    createProxy(target, options = {}) {
        const cache = this;
        
        return new Proxy(target, {
            get(obj, prop) {
                const originalMethod = obj[prop];
                
                // Si ce n'est pas une fonction, retourner la valeur
                if (typeof originalMethod !== 'function') {
                    return originalMethod;
                }
                
                // Methodes speciales du cache
                if (prop === 'clearCache') {
                    return () => cache.clear();
                }
                if (prop === 'getCacheStats') {
                    return () => cache.getStats();
                }
                if (prop === 'invalidateCache') {
                    return (pattern) => cache.invalidate(pattern);
                }
                
                // Wrapper pour les methodes
                return function(...args) {
                    const cacheKey = cache.generateKey(prop, args);
                    const methodOptions = options[prop] || {};
                    
                    // Verifier si le cache est desactive pour cette methode
                    if (methodOptions.noCache) {
                        return originalMethod.apply(this, args);
                    }
                    
                    // Verifier le cache
                    if (cache.has(cacheKey)) {
                        cache.stats.hits++;
                        return cache.get(cacheKey);
                    }
                    
                    // Executer la methode originale
                    cache.stats.misses++;
                    const result = originalMethod.apply(this, args);
                    
                    // Mettre en cache si pas d'option contraire
                    if (!methodOptions.noCache) {
                        const ttl = methodOptions.ttl || cache.defaultTTL;
                        cache.set(cacheKey, result, ttl);
                    }
                    
                    return result;
                };
            }
        });
    }
    
    generateKey(method, args) {
        return `${method}:${JSON.stringify(args)}`;
    }
    
    set(key, value, ttl = this.defaultTTL) {
        // Gestion de la taille maximale
        if (this.cache.size >= this.maxSize) {
            this.evictOldest();
        }
        
        const expiration = Date.now() + ttl;
        this.cache.set(key, value);
        this.metadata.set(key, {
            expiration,
            createdAt: Date.now(),
            accessCount: 0
        });
    }
    
    get(key) {
        if (!this.has(key)) {
            return undefined;
        }
        
        // Mettre a jour les metadonnees d'acces
        const meta = this.metadata.get(key);
        meta.accessCount++;
        meta.lastAccess = Date.now();
        
        return this.cache.get(key);
    }
    
    has(key) {
        if (!this.cache.has(key)) {
            return false;
        }
        
        // Verifier l'expiration
        const meta = this.metadata.get(key);
        if (Date.now() > meta.expiration) {
            this.cache.delete(key);
            this.metadata.delete(key);
            return false;
        }
        
        return true;
    }
    
    invalidate(pattern) {
        let count = 0;
        
        if (typeof pattern === 'string') {
            // Pattern simple
            for (const key of this.cache.keys()) {
                if (key.includes(pattern)) {
                    this.cache.delete(key);
                    this.metadata.delete(key);
                    count++;
                }
            }
        } else if (pattern instanceof RegExp) {
            // Pattern regex
            for (const key of this.cache.keys()) {
                if (pattern.test(key)) {
                    this.cache.delete(key);
                    this.metadata.delete(key);
                    count++;
                }
            }
        }
        
        this.stats.invalidations += count;
        return count;
    }
    
    cleanup() {
        let removed = 0;
        const now = Date.now();
        
        for (const [key, meta] of this.metadata.entries()) {
            if (now > meta.expiration) {
                this.cache.delete(key);
                this.metadata.delete(key);
                removed++;
            }
        }
        
        console.log(`Cache cleanup: ${removed} entrees expirees supprimees`);
        return removed;
    }
    
    evictOldest() {
        let oldestKey = null;
        let oldestTime = Infinity;
        
        for (const [key, meta] of this.metadata.entries()) {
            const priority = meta.lastAccess || meta.createdAt;
            if (priority < oldestTime) {
                oldestTime = priority;
                oldestKey = key;
            }
        }
        
        if (oldestKey) {
            this.cache.delete(oldestKey);
            this.metadata.delete(oldestKey);
        }
    }
    
    clear() {
        this.cache.clear();
        this.metadata.clear();
        this.stats = { hits: 0, misses: 0, invalidations: 0 };
    }
    
    getStats() {
        const total = this.stats.hits + this.stats.misses;
        return {
            ...this.stats,
            hitRate: total > 0 ? (this.stats.hits / total * 100).toFixed(2) + '%' : '0%',
            cacheSize: this.cache.size,
            memoryUsage: this.calculateMemoryUsage()
        };
    }
    
    calculateMemoryUsage() {
        let size = 0;
        for (const [key, value] of this.cache.entries()) {
            size += JSON.stringify({ key, value }).length;
        }
        return `${(size / 1024).toFixed(2)} KB`;
    }
    
    destroy() {
        clearInterval(this.cleanupInterval);
        this.clear();
    }
}

// Classe d'exemple pour tester le cache
class ServiceDonnees {
    constructor() {
        this.db = {
            users: [
                { id: 1, nom: 'Alice', email: 'alice@example.com' },
                { id: 2, nom: 'Bob', email: 'bob@example.com' },
                { id: 3, nom: 'Charlie', email: 'charlie@example.com' }
            ]
        };
    }
    
    // Methode couteuse simulee
    getUserById(id) {
        console.log(`[DB] Requete getUserById(${id})`);
        // Simulation d'une requete lente
        const start = Date.now();
        while (Date.now() - start < 100) {} // Attente de 100ms
        
        return this.db.users.find(user => user.id === id) || null;
    }
    
    getAllUsers() {
        console.log('[DB] Requete getAllUsers()');
        // Simulation d'une requete lente
        const start = Date.now();
        while (Date.now() - start < 200) {} // Attente de 200ms
        
        return [...this.db.users];
    }
    
    updateUser(id, data) {
        console.log(`[DB] Mise a jour utilisateur ${id}`);
        const user = this.db.users.find(u => u.id === id);
        if (user) {
            Object.assign(user, data);
        }
        return user;
    }
    
    searchUsers(query) {
        console.log(`[DB] Recherche: ${query}`);
        return this.db.users.filter(user => 
            user.nom.toLowerCase().includes(query.toLowerCase()) ||
            user.email.toLowerCase().includes(query.toLowerCase())
        );
    }
}

// Demonstration
function demonstrationCacheIntelligent() {
    const cache = new CacheIntelligent({
        ttl: 5000,  // 5 secondes
        maxSize: 10
    });
    
    // Creer un service avec cache
    const service = cache.createProxy(new ServiceDonnees(), {
        updateUser: { noCache: true }, // Ne pas cacher les mutations
        searchUsers: { ttl: 2000 }     // TTL personnalise pour la recherche
    });
    
    console.log("=== Test du cache intelligent ===");
    
    // Premier appel (cache miss)
    console.log("\n--- Premier appel getUserById(1) ---");
    console.time("getUserById");
    const user1 = service.getUserById(1);
    console.timeEnd("getUserById");
    console.log("Resultat:", user1);
    
    // Deuxieme appel (cache hit)
    console.log("\n--- Deuxieme appel getUserById(1) ---");
    console.time("getUserById");
    const user1Cached = service.getUserById(1);
    console.timeEnd("getUserById");
    console.log("Resultat:", user1Cached);
    
    // Test avec getAllUsers
    console.log("\n--- Appel getAllUsers ---");
    console.time("getAllUsers");
    const allUsers = service.getAllUsers();
    console.timeEnd("getAllUsers");
    
    console.log("\n--- Deuxieme appel getAllUsers (cache) ---");
    console.time("getAllUsers");
    const allUsersCached = service.getAllUsers();
    console.timeEnd("getAllUsers");
    
    // Test invalidation
    console.log("\n--- Mise a jour et invalidation ---");
    service.updateUser(1, { nom: 'Alice Dupont' });
    service.invalidateCache('getUserById'); // Invalider les caches d'utilisateur
    
    console.log("\n--- Appel getUserById(1) apres invalidation ---");
    console.time("getUserById");
    const userUpdated = service.getUserById(1);
    console.timeEnd("getUserById");
    console.log("Resultat:", userUpdated);
    
    // Statistiques finales
    console.log("\n--- Statistiques du cache ---");
    console.log(service.getCacheStats());
    
    // Nettoyage
    cache.destroy();
}

demonstrationCacheIntelligent();
```

**Alternatives possibles :**
- Implementer un cache distribue avec Redis
- Ajouter des strategies d'eviction plus sophistiquees (LRU, LFU)

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts prealables utilises
- **Objets** : Proxy et Reflect travaillent avec les objets
- **Fonctions** : Generateurs sont des fonctions speciales
- **Programmation asynchrone** : Generateurs peuvent etre async

### Prochaines etapes
- **Metaprogrammation** : Techniques avancees avec Proxy
- **Performance** : Optimisation avec WeakMap/WeakSet
- **Testing** : Tester le code utilisant ces concepts

### Concepts complementaires
- **Design patterns** : Iterator, Observer avec ces outils
- **Functional programming** : Generateurs pour lazy evaluation

---

## **PROJET MINI**

### Defi pratique
**Objectif :** Creer un framework de validation de formulaires avec regex, generateurs et proxy

**Fonctionnalites requises :**
- [ ] Systeme de validation avec patterns regex personnalisables
- [ ] Generateur de messages d'erreur adaptatifs
- [ ] Proxy pour validation automatique lors de l'assignation
- [ ] Interface de configuration intuitive
- [ ] Gestion des validations asynchrones

**Structure de fichiers suggeree :**
```
📁 validation-framework/
├── 📄 index.html
├── 📄 style.css
├── 📄 validator.js
├── 📄 patterns.js
├── 📄 generators.js
└── 📄 demo.js
```

**Temps estime :** 180 minutes

---

## **RESSOURCES COMPLEMENTAIRES**

### Documentation officielle
- [MDN - Regular Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [MDN - Iterators and Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)
- [MDN - Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
- [MDN - WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)

### Videos recommandees
- "JavaScript Generators Explained" - Fun Fun Function (15 min)
- "Proxy in JavaScript" - The Net Ninja (20 min)
- Pourquoi ces videos sont utiles : Concepts complexes expliques simplement

### Articles approfondis
- "Advanced JavaScript Patterns" - JavaScript.info
- "Metaprogramming in ES6" - Exploring JS
- Ce que ces articles apportent en plus : Patterns avances et cas d'usage

### Outils de pratique
- [Regex101](https://regex101.com/) - Testeur de regex interactif
- [Generator Examples](https://github.com/gajus/generators-examples)

---

## **AUTO-EVALUATION**

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre un iterateur et un generateur ?
   - Reponse : Un iterateur est un objet avec next(), un generateur est une fonction qui produit un iterateur

2. **Question pratique :** Que fait ce code ?
   ```javascript
   function* exemple() {
       yield 1;
       yield 2;
       return 3;
   }
   ```
   - Reponse : Cree un generateur qui produit 1, puis 2, puis termine avec 3

3. **Question d'application :** Quand utiliseriez-vous WeakMap plutot que Map ?
   - Reponse : Quand vous voulez eviter les fuites memoire en permettant au garbage collector de supprimer les objets

### Checklist de maitrise
- [ ] Je comprends les expressions regulieres et leurs patterns
- [ ] Je sais creer et utiliser des generateurs
- [ ] Je peux implementer des iterateurs personnalises
- [ ] Je comprends Proxy et ses possibilites
- [ ] Je sais quand utiliser WeakMap/WeakSet
- [ ] Je peux combiner ces concepts dans des applications complexes

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur frequente 1
**Code problematique :**
```javascript
function* generateur() {
    yield 1;
    return 2;
    yield 3; // Code mort
}
```

**Probleme :** Code apres return dans un generateur

**Solution :**
```javascript
function* generateur() {
    yield 1;
    yield 2;
    yield 3; // Maintenant accessible
}
```

**Explication :** return termine le generateur, yield suivants ne seront jamais executes.

### Erreur frequente 2
**Code problematique :**
```javascript
const regex = new RegExp("test"); // Oubli d'echapper
const text = "test.txt";
console.log(regex.test(text)); // true (inattendu)
```

**Probleme :** . en regex matche n'importe quel caractere

**Solution :**
```javascript
const regex = new RegExp("test\\.txt"); // Echapper le point
// ou
const regex = /test\.txt/;
```

**Explication :** Les caracteres speciaux en regex doivent etre echappes.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- La puissance des expressions regulieres pour le traitement de texte
- Comment les generateurs permettent un controle fin de l'iteration
- Les possibilites de metaprogrammation avec Proxy

### Questions a approfondir
- Patterns regex complexes pour des cas specifiques
- Optimisation des performances avec generateurs

### Revision programmee
- [ ] Revision dans 1 jour (rappel rapide)
- [ ] Revision dans 1 semaine (exercices)
- [ ] Revision dans 1 mois (application dans un projet)

---

## **TAGS ET METADONNEES**

**Mots-cles :** #javascript #regex #generateurs #iterateurs #proxy #weakmap #metaprogrammation #avance

**Temps de completion :** [Temps reellement passe]

**Difficulte ressentie :** ⭐⭐⭐⭐⭐

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Pret pour le concept suivant :** ✅ Oui / ❌ Non (revision necessaire)

---

*📅 Fiche creee le : 17/07/2025*
*🔄 Derniere revision : 17/07/2025*
*📍 Position dans la roadmap : 15/126*
