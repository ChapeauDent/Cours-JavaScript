# Structures de Données Avancées en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnées du cours
- **Titre :** Structures de Données Avancées (Map, WeakMap, Set, WeakSet)
- **Niveau :** Intermédiaire
- **Durée estimée :** 90 minutes
- **Prérequis :** Arrays, objets, concepts de référence et mémoire
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Les différences entre Object, Map, Set et leurs variants
- [ ] **Appliquer** : Choisir la structure de données appropriée selon le contexte
- [ ] **Analyser** : Les avantages de performance de chaque structure
- [ ] **Créer** : Des applications utilisant ces structures efficacement

### Mots-clés à retenir
- **Map** : Structure clé-valeur avec clés de tout type
- **WeakMap** : Map avec clés objets uniquement et gestion mémoire automatique
- **Set** : Collection de valeurs uniques
- **WeakSet** : Set d'objets avec gestion mémoire automatique
- **Itérable** : Structure qu'on peut parcourir avec for...of

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les objets JavaScript classiques ont des limitations : les clés sont toujours des strings, on ne peut pas facilement connaître leur taille, et ils héritent de propriétés du prototype. Les nouvelles structures de données ES6+ offrent des solutions plus spécialisées et performantes pour des cas d'usage spécifiques.

### Définition et concepts clés

#### **1. Map vs Object**
```javascript
// Object classique - limitations
let obj = {
    "nom": "Alice",
    1: "premier", // 1 devient "1"
    true: "vrai"  // true devient "true"
};

// Map - plus flexible
let map = new Map();
map.set("nom", "Alice");
map.set(1, "premier");        // 1 reste un number
map.set(true, "vrai");        // true reste un boolean
map.set({id: 1}, "objet");    // Objet comme clé !
```

#### **2. Set vs Array**
```javascript
// Array - peut avoir des doublons
let array = [1, 2, 2, 3, 3, 3];
console.log(array.length); // 6

// Set - valeurs uniques automatiquement
let set = new Set([1, 2, 2, 3, 3, 3]);
console.log(set.size); // 3 (doublons supprimés)
```

#### **3. WeakMap et WeakSet**
- **Weak** = "faibles" références qui n'empêchent pas le garbage collection
- Utiles pour éviter les fuites mémoire
- Clés doivent être des objets

### Syntaxe de base
```javascript
// === MAP ===
let map = new Map();
map.set("clé", "valeur");
map.get("clé");              // "valeur"
map.has("clé");              // true
map.delete("clé");
map.clear();                 // Vide tout

// === SET ===
let set = new Set();
set.add("valeur");
set.has("valeur");           // true
set.delete("valeur");
set.clear();

// === WEAKMAP ===
let weakMap = new WeakMap();
let obj = {id: 1};
weakMap.set(obj, "données");
weakMap.get(obj);            // "données"

// === WEAKSET ===
let weakSet = new WeakSet();
weakSet.add(obj);
weakSet.has(obj);            // true
```

### Points d'attention
- **Piège WeakMap** : On ne peut pas itérer sur WeakMap/WeakSet
- **Piège performance** : Map est plus lent que Object pour de petites collections
- **Bonne pratique** : Utiliser Map quand les clés ne sont pas des strings

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Comparaison Object vs Map pour un cache simple
console.log("=== COMPARAISON OBJECT VS MAP ===");

// Avec Object (classique)
let cacheObject = {};
cacheObject["user_123"] = {nom: "Alice", age: 25};
cacheObject["user_456"] = {nom: "Bob", age: 30};

console.log("Cache Object:", cacheObject);
console.log("Taille Object:", Object.keys(cacheObject).length);

// Avec Map (moderne)
let cacheMap = new Map();
cacheMap.set("user_123", {nom: "Alice", age: 25});
cacheMap.set("user_456", {nom: "Bob", age: 30});

console.log("Cache Map:", cacheMap);
console.log("Taille Map:", cacheMap.size);

// Avantages de Map
console.log("\n=== AVANTAGES DE MAP ===");

// 1. Clés de tout type
let mapAvecClesVariees = new Map();
mapAvecClesVariees.set(1, "clé number");
mapAvecClesVariees.set("1", "clé string");
mapAvecClesVariees.set(true, "clé boolean");

console.log("Map avec clés variées:");
mapAvecClesVariees.forEach((valeur, clé) => {
    console.log(`${typeof clé} ${clé} => ${valeur}`);
});

// 2. Taille directe
console.log("Taille Map:", mapAvecClesVariees.size);

// 3. Itération facile
console.log("\nItération avec for...of:");
for (let [clé, valeur] of mapAvecClesVariees) {
    console.log(`${clé} => ${valeur}`);
}
```

**Résultat attendu :**
```
=== COMPARAISON OBJECT VS MAP ===
Cache Object: { user_123: {nom: "Alice", age: 25}, user_456: {nom: "Bob", age: 30} }
Taille Object: 2
Cache Map: Map(2) { "user_123" => {nom: "Alice", age: 25}, "user_456" => {nom: "Bob", age: 30} }
Taille Map: 2

=== AVANTAGES DE MAP ===
Map avec clés variées:
number 1 => clé number
string 1 => clé string
boolean true => clé boolean
Taille Map: 3

Itération avec for...of:
1 => clé number
1 => clé string
true => clé boolean
```

### Exemple intermédiaire
```javascript
// Utilisation pratique : Système de suivi des utilisateurs connectés
console.log("=== SYSTÈME DE SUIVI UTILISATEURS ===");

// Set pour les utilisateurs uniques connectés
let utilisateursConnectes = new Set();

function connecterUtilisateur(nom) {
    utilisateursConnectes.add(nom);
    console.log(`${nom} connecté. Total: ${utilisateursConnectes.size}`);
}

function deconnecterUtilisateur(nom) {
    if (utilisateursConnectes.delete(nom)) {
        console.log(`${nom} déconnecté. Total: ${utilisateursConnectes.size}`);
    } else {
        console.log(`${nom} n'était pas connecté`);
    }
}

// Tests
connecterUtilisateur("Alice");
connecterUtilisateur("Bob");
connecterUtilisateur("Alice");    // Pas de doublon !
connecterUtilisateur("Charlie");

console.log("Utilisateurs connectés:", [...utilisateursConnectes]);

deconnecterUtilisateur("Bob");
deconnecterUtilisateur("Bob");    // Déjà déconnecté

// Map pour stocker des données par utilisateur
let donneesUtilisateurs = new Map();

function stockerDonnees(utilisateur, donnees) {
    donneesUtilisateurs.set(utilisateur, {
        ...donnees,
        derniereActivite: new Date().toLocaleTimeString()
    });
    console.log(`Données mises à jour pour ${utilisateur}`);
}

function obtenirDonnees(utilisateur) {
    return donneesUtilisateurs.get(utilisateur) || null;
}

// Tests avec données utilisateur
stockerDonnees("Alice", {score: 150, niveau: 5});
stockerDonnees("Charlie", {score: 200, niveau: 7});

console.log("\n=== DONNÉES UTILISATEURS ===");
donneesUtilisateurs.forEach((donnees, utilisateur) => {
    console.log(`${utilisateur}:`, donnees);
});
```

### Exemple avancé (optionnel)
```javascript
// WeakMap pour éviter les fuites mémoire
console.log("=== WEAKMAP POUR MÉTADONNÉES ===");

// Problème avec Map classique
let metadonneesMap = new Map();
let elements = [];

function creerElement(nom) {
    let element = {nom: nom, id: Math.random()};
    elements.push(element);
    
    // Stocker des métadonnées privées
    metadonneesMap.set(element, {
        dateCreation: new Date(),
        nombreAcces: 0,
        statut: "actif"
    });
    
    return element;
}

function accederElement(element) {
    let meta = metadonneesMap.get(element);
    if (meta) {
        meta.nombreAcces++;
        meta.derniereUtilisation = new Date();
    }
}

// Créer des éléments
let elem1 = creerElement("Element1");
let elem2 = creerElement("Element2");

accederElement(elem1);
accederElement(elem1);
accederElement(elem2);

console.log("Métadonnées avec Map classique:");
metadonneesMap.forEach((meta, element) => {
    console.log(`${element.nom}:`, meta);
});

// Solution avec WeakMap (pas de fuite mémoire)
let metadonneesWeakMap = new WeakMap();

function creerElementSecurise(nom) {
    let element = {nom: nom, id: Math.random()};
    
    // WeakMap - sera nettoyé automatiquement si element est supprimé
    metadonneesWeakMap.set(element, {
        dateCreation: new Date(),
        nombreAcces: 0,
        statut: "actif"
    });
    
    return element;
}

function obtenirMetadonnees(element) {
    return metadonneesWeakMap.get(element);
}

// Tests WeakMap
let elemSecure1 = creerElementSecurise("ElementSecure1");
let elemSecure2 = creerElementSecurise("ElementSecure2");

console.log("\nMétadonnées avec WeakMap:");
console.log("Element1:", obtenirMetadonnees(elemSecure1));
console.log("Element2:", obtenirMetadonnees(elemSecure2));

// Quand elemSecure1 n'est plus référencé ailleurs,
// ses métadonnées sont automatiquement supprimées de WeakMap
elemSecure1 = null; // Les métadonnées seront nettoyées automatiquement

// Exemple pratique : Déduplication avec Set
function dedupliquerArray(array) {
    return [...new Set(array)];
}

console.log("\n=== DÉDUPLICATION ===");
let arrayAvecDoublons = [1, 2, 2, 3, 4, 4, 5, 1];
let arraySansDoublons = dedupliquerArray(arrayAvecDoublons);
console.log("Avant:", arrayAvecDoublons);
console.log("Après:", arraySansDoublons);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Cache intelligent
**Consigne :** Créez un système de cache qui stocke les résultats de calculs coûteux.

**Code de départ :**
```javascript
// TODO: Implémenter un cache avec Map
function creerCache() {
    // Utiliser Map pour stocker les résultats
    // Méthodes: get(clé), set(clé, valeur), has(clé), clear()
}

function calculComplexe(n) {
    // Simulation d'un calcul coûteux
    console.log(`Calcul complexe pour ${n}...`);
    let résultat = 0;
    for (let i = 0; i < n * 1000; i++) {
        résultat += Math.sqrt(i);
    }
    return résultat;
}

// Utiliser le cache pour éviter les recalculs
```

**Résultat attendu :**
- Premier appel : calcul effectué
- Deuxième appel avec même paramètre : résultat depuis cache

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Gestionnaire d'tags uniques
**Consigne :** Créez un système de gestion de tags pour articles avec Set.

**Contraintes :**
- Pas de tags dupliqués
- Recherche rapide si un tag existe
- Opérations union, intersection, différence entre sets de tags

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 3 : Système de notifications avec WeakMap (optionnel)
**Consigne :** Créez un système qui attache des données privées aux objets DOM.

**Niveau de difficulté :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function creerCache() {
    const cache = new Map();
    
    return {
        get(clé) {
            if (cache.has(clé)) {
                console.log(`✅ Cache HIT pour ${clé}`);
                return cache.get(clé);
            }
            console.log(`❌ Cache MISS pour ${clé}`);
            return null;
        },
        
        set(clé, valeur) {
            cache.set(clé, valeur);
            console.log(`💾 Mise en cache: ${clé} => ${valeur}`);
        },
        
        has(clé) {
            return cache.has(clé);
        },
        
        clear() {
            cache.clear();
            console.log("🗑️ Cache vidé");
        },
        
        size() {
            return cache.size;
        },
        
        keys() {
            return [...cache.keys()];
        }
    };
}

function calculComplexe(n) {
    console.log(`🔄 Calcul complexe pour ${n}...`);
    let résultat = 0;
    for (let i = 0; i < n * 1000; i++) {
        résultat += Math.sqrt(i);
    }
    return Math.round(résultat);
}

// Fonction avec cache intégré
function calculAvecCache(n, cache) {
    // Vérifier le cache
    let résultat = cache.get(n);
    
    if (résultat === null) {
        // Calculer et mettre en cache
        résultat = calculComplexe(n);
        cache.set(n, résultat);
    }
    
    return résultat;
}

// Tests
const monCache = creerCache();

console.log("=== TESTS CACHE ===");
console.log("Résultat 1:", calculAvecCache(100, monCache));
console.log("Résultat 2:", calculAvecCache(100, monCache)); // Depuis cache
console.log("Résultat 3:", calculAvecCache(200, monCache));
console.log("Résultat 4:", calculAvecCache(100, monCache)); // Depuis cache

console.log("Taille du cache:", monCache.size());
console.log("Clés en cache:", monCache.keys());
```

**Explication :** Le cache utilise Map pour des performances optimales sur les opérations get/set/has.

### Solution Exercice 2
```javascript
class GestionnaireTags {
    constructor() {
        this.articlesAvecTags = new Map(); // articleId => Set de tags
        this.tousLesTags = new Set(); // Tous les tags existants
    }
    
    ajouterTag(articleId, tag) {
        // Initialiser le Set pour cet article si nécessaire
        if (!this.articlesAvecTags.has(articleId)) {
            this.articlesAvecTags.set(articleId, new Set());
        }
        
        // Ajouter le tag (Set évite automatiquement les doublons)
        this.articlesAvecTags.get(articleId).add(tag);
        this.tousLesTags.add(tag);
        
        console.log(`✅ Tag "${tag}" ajouté à l'article ${articleId}`);
    }
    
    obtenirTags(articleId) {
        return this.articlesAvecTags.get(articleId) || new Set();
    }
    
    supprimerTag(articleId, tag) {
        const tagsArticle = this.articlesAvecTags.get(articleId);
        if (tagsArticle && tagsArticle.delete(tag)) {
            console.log(`🗑️ Tag "${tag}" supprimé de l'article ${articleId}`);
            
            // Vérifier si le tag existe encore ailleurs
            this.mettreAJourTousLesTags();
        }
    }
    
    mettreAJourTousLesTags() {
        this.tousLesTags.clear();
        for (let tagsArticle of this.articlesAvecTags.values()) {
            for (let tag of tagsArticle) {
                this.tousLesTags.add(tag);
            }
        }
    }
    
    // Opérations sur les ensembles
    union(articleId1, articleId2) {
        const tags1 = this.obtenirTags(articleId1);
        const tags2 = this.obtenirTags(articleId2);
        return new Set([...tags1, ...tags2]);
    }
    
    intersection(articleId1, articleId2) {
        const tags1 = this.obtenirTags(articleId1);
        const tags2 = this.obtenirTags(articleId2);
        return new Set([...tags1].filter(tag => tags2.has(tag)));
    }
    
    difference(articleId1, articleId2) {
        const tags1 = this.obtenirTags(articleId1);
        const tags2 = this.obtenirTags(articleId2);
        return new Set([...tags1].filter(tag => !tags2.has(tag)));
    }
    
    rechercherParTag(tag) {
        const articlesAvecTag = [];
        for (let [articleId, tagsArticle] of this.articlesAvecTags) {
            if (tagsArticle.has(tag)) {
                articlesAvecTag.push(articleId);
            }
        }
        return articlesAvecTag;
    }
    
    statistiques() {
        return {
            nombreArticles: this.articlesAvecTags.size,
            nombreTagsUniques: this.tousLesTags.size,
            tousLesTags: [...this.tousLesTags].sort()
        };
    }
}

// Tests
const gestionnaire = new GestionnaireTags();

// Ajouter des tags
gestionnaire.ajouterTag("article1", "javascript");
gestionnaire.ajouterTag("article1", "programmation");
gestionnaire.ajouterTag("article1", "web");
gestionnaire.ajouterTag("article1", "javascript"); // Doublon ignoré

gestionnaire.ajouterTag("article2", "javascript");
gestionnaire.ajouterTag("article2", "css");
gestionnaire.ajouterTag("article2", "html");

console.log("\n=== TAGS PAR ARTICLE ===");
console.log("Article 1:", [...gestionnaire.obtenirTags("article1")]);
console.log("Article 2:", [...gestionnaire.obtenirTags("article2")]);

console.log("\n=== OPÉRATIONS SUR ENSEMBLES ===");
console.log("Union:", [...gestionnaire.union("article1", "article2")]);
console.log("Intersection:", [...gestionnaire.intersection("article1", "article2")]);
console.log("Différence:", [...gestionnaire.difference("article1", "article2")]);

console.log("\n=== RECHERCHE ===");
console.log("Articles avec 'javascript':", gestionnaire.rechercherParTag("javascript"));
console.log("Articles avec 'css':", gestionnaire.rechercherParTag("css"));

console.log("\n=== STATISTIQUES ===");
console.log(gestionnaire.statistiques());
```

**Alternatives possibles :**
- Indexation inverse pour des recherches plus rapides
- Hiérarchie de tags avec héritage

### Solution Exercice 3
```javascript
// Système de notifications avec WeakMap
class SystemeNotifications {
    constructor() {
        // WeakMap pour attacher des données privées aux éléments DOM
        this.donneesPrivees = new WeakMap();
        this.compteurNotifications = 0;
    }
    
    attacherAElement(element, options = {}) {
        const donnees = {
            id: ++this.compteurNotifications,
            notifications: [],
            options: {
                maxNotifications: options.maxNotifications || 5,
                autoHide: options.autoHide || false,
                delaiAutoHide: options.delaiAutoHide || 3000
            },
            derniereActivite: new Date()
        };
        
        this.donneesPrivees.set(element, donnees);
        console.log(`📎 Système attaché à l'élément (ID: ${donnees.id})`);
        
        return donnees.id;
    }
    
    ajouterNotification(element, message, type = "info") {
        const donnees = this.donneesPrivees.get(element);
        
        if (!donnees) {
            console.error("❌ Élément non initialisé");
            return false;
        }
        
        const notification = {
            id: Date.now(),
            message: message,
            type: type,
            timestamp: new Date(),
            vue: false
        };
        
        // Limiter le nombre de notifications
        if (donnees.notifications.length >= donnees.options.maxNotifications) {
            donnees.notifications.shift(); // Supprimer la plus ancienne
        }
        
        donnees.notifications.push(notification);
        donnees.derniereActivite = new Date();
        
        this.afficherNotification(element, notification);
        
        // Auto-hide si configuré
        if (donnees.options.autoHide) {
            setTimeout(() => {
                this.masquerNotification(element, notification.id);
            }, donnees.options.delaiAutoHide);
        }
        
        return notification.id;
    }
    
    afficherNotification(element, notification) {
        // Créer l'élément de notification
        const notifElement = document.createElement("div");
        notifElement.className = `notification notification-${notification.type}`;
        notifElement.dataset.notificationId = notification.id;
        notifElement.innerHTML = `
            <span class="notification-message">${notification.message}</span>
            <button class="notification-close" onclick="this.parentElement.remove()">×</button>
        `;
        
        // Styles basiques
        notifElement.style.cssText = `
            position: relative;
            padding: 10px;
            margin: 5px 0;
            border-radius: 4px;
            background: ${this.getCouleurType(notification.type)};
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
        `;
        
        element.appendChild(notifElement);
        console.log(`🔔 Notification affichée: ${notification.message}`);
    }
    
    masquerNotification(element, notificationId) {
        const notifElement = element.querySelector(`[data-notification-id="${notificationId}"]`);
        if (notifElement) {
            notifElement.remove();
            console.log(`🔇 Notification masquée: ${notificationId}`);
        }
    }
    
    getCouleurType(type) {
        const couleurs = {
            info: "#17a2b8",
            success: "#28a745",
            warning: "#ffc107",
            error: "#dc3545"
        };
        return couleurs[type] || couleurs.info;
    }
    
    obtenirStatistiques(element) {
        const donnees = this.donneesPrivees.get(element);
        if (!donnees) return null;
        
        return {
            id: donnees.id,
            nombreNotifications: donnees.notifications.length,
            derniereActivite: donnees.derniereActivite,
            options: donnees.options,
            notifications: donnees.notifications.map(n => ({
                id: n.id,
                message: n.message,
                type: n.type,
                timestamp: n.timestamp
            }))
        };
    }
    
    nettoyer(element) {
        // Le WeakMap se nettoie automatiquement quand l'élément est supprimé
        // Mais on peut nettoyer les notifications visuelles
        const notifications = element.querySelectorAll(".notification");
        notifications.forEach(notif => notif.remove());
        
        console.log("🧹 Notifications nettoyées pour l'élément");
    }
}

// Tests (nécessite un environnement avec DOM)
if (typeof document !== "undefined") {
    // Créer un élément de test
    const conteneur = document.createElement("div");
    conteneur.id = "test-notifications";
    document.body.appendChild(conteneur);
    
    // Initialiser le système
    const systeme = new SystemeNotifications();
    const elementId = systeme.attacherAElement(conteneur, {
        maxNotifications: 3,
        autoHide: true,
        delaiAutoHide: 5000
    });
    
    // Ajouter des notifications
    systeme.ajouterNotification(conteneur, "Bienvenue !", "success");
    systeme.ajouterNotification(conteneur, "Nouvelle mise à jour disponible", "info");
    systeme.ajouterNotification(conteneur, "Attention: quota presque atteint", "warning");
    
    // Statistiques
    console.log("Statistiques:", systeme.obtenirStatistiques(conteneur));
    
    // Test de nettoyage automatique WeakMap
    setTimeout(() => {
        console.log("Test: suppression élément et nettoyage automatique WeakMap");
        conteneur.remove(); // Les données WeakMap seront automatiquement nettoyées
    }, 10000);
} else {
    console.log("Tests DOM non disponibles dans cet environnement");
}
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Arrays** : Comparaison avec Set pour collections
- **Objets** : Comparaison avec Map pour associations clé-valeur
- **Gestion mémoire** : Importance pour WeakMap/WeakSet
- **Itération** : for...of, forEach avec les nouvelles structures

### Prochaines étapes
- **Programmation orientée objet** : Utiliser ces structures dans des classes
- **Performance** : Optimisation avec les bonnes structures de données
- **Patterns avancés** : Observer, Caching, Memoization

### Concepts complémentaires
- **JSON** : Sérialisation des Map/Set
- **APIs modernes** : Utilisation avec fetch, localStorage

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un système de gestion de bibliothèque avec toutes les structures

**Fonctionnalités requises :**
- [ ] Catalogue de livres (Map: ISBN => données)
- [ ] Gestion des emprunts (Set d'utilisateurs actifs)
- [ ] Tags/genres uniques (Set)
- [ ] Historique par utilisateur (Map: user => Array d'emprunts)
- [ ] Métadonnées privées sur objets DOM (WeakMap)
- [ ] Interface de recherche et statistiques

**Structure suggérée :**
```
📁 bibliotheque-moderne/
├── 📄 index.html
├── 📄 style.css
├── 📄 bibliotheque.js
├── 📄 structures-donnees.js
└── 📄 interface.js
```

**Temps estimé :** 120 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Map](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Map)
- [MDN - Set](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Set)
- [MDN - WeakMap](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)
- [MDN - WeakSet](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)

### Vidéos recommandées
- "ES6 Collections: Map and Set" - Fun Fun Function (15 min)
- Pourquoi cette vidéo est utile : Exemples pratiques et comparaisons

### Articles approfondis
- "When to use Map vs Object in JavaScript" - freeCodeCamp
- Ce que cet article apporte en plus : Critères de choix et performance

### Outils de pratique
- [JavaScript Collections Performance](https://jsperf.com/map-vs-object-performance)
- [Exemples interactifs MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Map#exemples)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence principale entre Map et Object ?
   - Réponse : Map accepte tout type de clé, Object seulement strings/symbols. Map a .size, Object nécessite Object.keys().length

2. **Question pratique :** Quand utiliser WeakMap plutôt que Map ?
   - Réponse : Quand les clés sont des objets et qu'on veut éviter les fuites mémoire

3. **Question d'application :** Comment supprimer les doublons d'un array ?
   - Réponse : `[...new Set(array)]` ou `Array.from(new Set(array))`

### Checklist de maîtrise
- [ ] Je comprends les différences entre Object/Map et Array/Set
- [ ] Je sais choisir la structure appropriée selon le contexte
- [ ] Je maîtrise l'itération sur Map et Set
- [ ] Je comprends les avantages de WeakMap/WeakSet
- [ ] Je peux combiner ces structures efficacement
- [ ] Je reconnais les cas d'usage de performance

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
let map = new Map();
map["clé"] = "valeur";  // ❌ Utilise Object, pas Map
console.log(map.get("clé"));  // undefined
```

**Solution :**
```javascript
let map = new Map();
map.set("clé", "valeur");  // ✅ Utilise les méthodes Map
console.log(map.get("clé"));  // "valeur"
```

**Explication :** Map a ses propres méthodes, n'utilisez pas la notation objet.

### Erreur fréquente 2
**Code problématique :**
```javascript
let weakMap = new WeakMap();
weakMap.set("string", "valeur");  // ❌ String n'est pas un objet
```

**Solution :**
```javascript
let weakMap = new WeakMap();
let obj = {id: 1};
weakMap.set(obj, "valeur");  // ✅ Objet comme clé
```

**Explication :** WeakMap accepte seulement des objets comme clés.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les différences performance entre structures
- L'importance de WeakMap pour éviter fuites mémoire
- Cas d'usage pratiques de Set pour déduplication

### Questions à approfondir
- Performance benchmarks détaillés
- Utilisation avec des frameworks modernes

### Révision programmée
- [ ] Révision dans 1 jour (rappel rapide)
- [ ] Révision dans 1 semaine (exercices)
- [ ] Révision dans 1 mois (application dans un projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #structures-donnees #map #set #weakmap #weakset #intermediaire

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 8.5/126*
