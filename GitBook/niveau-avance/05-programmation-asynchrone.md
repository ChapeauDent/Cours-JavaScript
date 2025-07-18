# ⏳ Programmation Asynchrone en JavaScript

> **Niveau :** 🔴 Avancé | **Durée :** 75 minutes | **Prérequis :** Fonctions, Objets, Gestion d'erreurs

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Comprendre** le concept de programmation asynchrone et l'Event Loop
- [ ] **Maîtriser** les callbacks, promises et async/await  
- [ ] **Analyser** les performances et patterns asynchrones
- [ ] **Créer** des applications robustes avec gestion asynchrone

---

## 📚 Introduction

{% tabs %}
{% tab title="🧠 Concept" %}
JavaScript est **mono-thread** : il ne peut exécuter qu'une instruction à la fois. Sans programmation asynchrone, votre interface se figerait pendant les opérations longues.

**Pourquoi c'est crucial ?**
- Éviter le blocage de l'interface utilisateur
- Gérer les requêtes réseau efficacement
- Améliorer l'expérience utilisateur
- Optimiser les performances
{% endtab %}

{% tab title="⚡ Problème" %}
```javascript
// ❌ Code synchrone - BLOQUE tout !
console.log("Début");

// Simulation d'une opération lente (3 secondes)
for (let i = 0; i < 3000000000; i++) {
    // L'interface est gelée pendant ce temps
}

console.log("Fin"); // Attend la fin de la boucle
```

**Conséquences :**
- Interface utilisateur figée
- Aucune interaction possible
- Expérience utilisateur dégradée
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// ✅ Code asynchrone - NON bloquant
console.log("Début");

setTimeout(() => {
    console.log("Opération terminée");
}, 3000);

console.log("Fin"); // S'exécute immédiatement
```

**Avantages :**
- Interface reste réactive
- Opérations en parallèle
- Meilleure performance perçue
{% endtab %}
{% endtabs %}

---

## 🔄 L'Event Loop

{% hint style="info" %}
**L'Event Loop** est le cœur de l'asynchrone en JavaScript. Il gère l'exécution du code de manière non-bloquante.
{% endhint %}

### Architecture

{% tabs %}
{% tab title="🏗️ Composants" %}
1. **Call Stack** : Pile d'exécution principale
2. **Web APIs** : APIs du navigateur (setTimeout, fetch...)
3. **Callback Queue** : File d'attente des callbacks
4. **Event Loop** : Vérifie et transfère les tâches

```javascript
// Exemple de fonctionnement
console.log("1"); // Call Stack

setTimeout(() => {
    console.log("2"); // Web API → Callback Queue → Call Stack
}, 0);

console.log("3"); // Call Stack

// Résultat : 1, 3, 2
```
{% endtab %}

{% tab title="🔄 Processus" %}
```javascript
// Démonstration de l'Event Loop
function demonstrerEventLoop() {
    console.log("Début");
    
    // Macro-task (setTimeout)
    setTimeout(() => console.log("Macro-task"), 0);
    
    // Micro-task (Promise)
    Promise.resolve().then(() => console.log("Micro-task"));
    
    console.log("Fin");
}

demonstrerEventLoop();
// Résultat : Début → Fin → Micro-task → Macro-task
```

**Ordre de priorité :**
1. Call Stack
2. Micro-tasks (Promises)
3. Macro-tasks (setTimeout, setInterval)
{% endtab %}
{% endtabs %}

---

## 📞 Les Callbacks

### Concept de base

{% tabs %}
{% tab title="🎯 Définition" %}
Un **callback** est une fonction passée en paramètre, exécutée plus tard quand une opération est terminée.

```javascript
// Callback simple
function traiterDonnees(donnees, callback) {
    // Simulation d'un traitement
    setTimeout(() => {
        const resultat = donnees.map(x => x * 2);
        callback(resultat);
    }, 1000);
}

// Utilisation
traiterDonnees([1, 2, 3], (resultat) => {
    console.log("Résultat :", resultat); // [2, 4, 6]
});
```
{% endtab %}

{% tab title="⚠️ Callback Hell" %}
```javascript
// ❌ Callback Hell - difficile à lire et maintenir
obtenirUtilisateur(id, (utilisateur) => {
    obtenirProfil(utilisateur.id, (profil) => {
        obtenirPreferences(profil.id, (preferences) => {
            obtenirNotifications(preferences.id, (notifications) => {
                // Code imbriqué difficile à suivre
                console.log(notifications);
            });
        });
    });
});
```

**Problèmes :**
- Code difficile à lire
- Gestion d'erreurs complexe
- Maintenance difficile
{% endtab %}

{% tab title="🔧 Gestion d'erreurs" %}
```javascript
// Convention Node.js : error-first callback
function lireFichier(nom, callback) {
    setTimeout(() => {
        if (nom.endsWith('.txt')) {
            callback(null, "Contenu du fichier");
        } else {
            callback(new Error("Format non supporté"));
        }
    }, 1000);
}

// Utilisation avec gestion d'erreur
lireFichier("document.txt", (erreur, contenu) => {
    if (erreur) {
        console.error("Erreur :", erreur.message);
        return;
    }
    console.log("Contenu :", contenu);
});
```
{% endtab %}
{% endtabs %}

### 🏃‍♂️ Exercice Pratique : Callbacks

{% hint style="warning" %}
**Exercice :** Créez un système de validation asynchrone avec callbacks
{% endhint %}

```javascript
// Complétez cette fonction
function validerFormulaire(donnees, callback) {
    // TODO: Implémenter la validation
    // - Vérifier que l'email est valide
    // - Vérifier que le mot de passe fait au moins 8 caractères
    // - Simuler une vérification en base (500ms)
    // - Appeler callback(erreur, resultat)
}

// Test
const formulaire = {
    email: "test@example.com",
    motDePasse: "motdepasse123"
};

validerFormulaire(formulaire, (erreur, valide) => {
    if (erreur) {
        console.error("Validation échouée :", erreur);
    } else {
        console.log("Validation réussie :", valide);
    }
});
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
// Structure de base
function validerFormulaire(donnees, callback) {
    // 1. Validation immédiate (synchrone)
    if (!donnees.email.includes('@')) {
        return callback(new Error("Email invalide"));
    }
    
    // 2. Validation asynchrone (base de données)
    setTimeout(() => {
        // Votre code ici
    }, 500);
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
function validerFormulaire(donnees, callback) {
    // Validation email
    if (!donnees.email || !donnees.email.includes('@')) {
        return callback(new Error("Email invalide"));
    }
    
    // Validation mot de passe
    if (!donnees.motDePasse || donnees.motDePasse.length < 8) {
        return callback(new Error("Mot de passe trop court"));
    }
    
    // Simulation vérification en base
    setTimeout(() => {
        const emailExiste = donnees.email === "admin@example.com";
        
        if (emailExiste) {
            callback(new Error("Email déjà utilisé"));
        } else {
            callback(null, {
                valide: true,
                message: "Validation réussie"
            });
        }
    }, 500);
}
```
{% endtab %}
{% endtabs %}

---

## 🤝 Les Promises

### Introduction aux Promises

{% hint style="success" %}
Les **Promises** représentent une valeur qui sera disponible maintenant, plus tard, ou jamais. Elles résolvent le problème du callback hell.
{% endhint %}

{% tabs %}
{% tab title="🎯 Concept" %}
Une Promise a 3 états :
- **Pending** : En attente
- **Fulfilled** : Résolue avec succès
- **Rejected** : Échouée avec une erreur

```javascript
// Création d'une Promise
const maPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const succes = Math.random() > 0.5;
        
        if (succes) {
            resolve("Opération réussie !");
        } else {
            reject(new Error("Opération échouée"));
        }
    }, 1000);
});

// Utilisation
maPromise
    .then(resultat => console.log(resultat))
    .catch(erreur => console.error(erreur));
```
{% endtab %}

{% tab title="⛓️ Chaînage" %}
```javascript
// Chaînage de Promises
fetch('/api/utilisateur/1')
    .then(response => response.json())
    .then(utilisateur => {
        console.log('Utilisateur:', utilisateur);
        return fetch(`/api/profil/${utilisateur.id}`);
    })
    .then(response => response.json())
    .then(profil => {
        console.log('Profil:', profil);
        return fetch(`/api/preferences/${profil.id}`);
    })
    .then(response => response.json())
    .then(preferences => {
        console.log('Préférences:', preferences);
    })
    .catch(erreur => {
        console.error('Erreur dans la chaîne:', erreur);
    });
```

**Avantages :**
- Code plus lisible
- Gestion d'erreur centralisée
- Composition facile
{% endtab %}

{% tab title="🔄 Méthodes utiles" %}
```javascript
// Promise.all - Toutes en parallèle
const promises = [
    fetch('/api/donnees1'),
    fetch('/api/donnees2'),
    fetch('/api/donnees3')
];

Promise.all(promises)
    .then(reponses => Promise.all(reponses.map(r => r.json())))
    .then(donnees => console.log('Toutes les données:', donnees))
    .catch(erreur => console.error('Une promesse a échoué:', erreur));

// Promise.race - La première qui termine
Promise.race([
    fetch('/api/serveur1'),
    fetch('/api/serveur2')
])
.then(response => console.log('Premier serveur à répondre'))
.catch(erreur => console.error('Aucun serveur ne répond'));

// Promise.allSettled - Toutes, même en cas d'erreur
Promise.allSettled(promises)
    .then(resultats => {
        resultats.forEach((resultat, index) => {
            if (resultat.status === 'fulfilled') {
                console.log(`Promise ${index} réussie:`, resultat.value);
            } else {
                console.log(`Promise ${index} échouée:`, resultat.reason);
            }
        });
    });
```
{% endtab %}
{% endtabs %}

### 🏃‍♂️ Exercice Pratique : Promises

{% hint style="warning" %}
**Exercice :** Créez un gestionnaire de téléchargements avec Promises
{% endhint %}

```javascript
// Complétez cette classe
class GestionnaireTelechargeents {
    constructor() {
        this.telechargementsEnCours = new Map();
    }
    
    // TODO: Implémenter cette méthode
    telecharger(url, options = {}) {
        // Retourner une Promise qui :
        // - Simule un téléchargement (temps aléatoire 1-3s)
        // - Gère les erreurs (URL invalide, timeout)
        // - Suit la progression
        return new Promise((resolve, reject) => {
            // Votre code ici
        });
    }
    
    // TODO: Télécharger plusieurs fichiers en parallèle
    telechargerPlusieurs(urls) {
        // Utiliser Promise.all ou Promise.allSettled
    }
    
    // TODO: Télécharger avec retry automatique
    telechargerAvecRetry(url, maxTentatives = 3) {
        // Implémenter la logique de retry
    }
}

// Test
const gestionnaire = new GestionnaireTelechargeents();

gestionnaire.telecharger('https://example.com/fichier.pdf')
    .then(resultat => console.log('Téléchargement réussi:', resultat))
    .catch(erreur => console.error('Téléchargement échoué:', erreur));
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
telecharger(url, options = {}) {
    return new Promise((resolve, reject) => {
        // Validation
        if (!url || !url.startsWith('http')) {
            return reject(new Error('URL invalide'));
        }
        
        // Simulation du téléchargement
        const tempsTelechargement = Math.random() * 2000 + 1000;
        
        setTimeout(() => {
            // Logique de succès/échec
        }, tempsTelechargement);
    });
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
class GestionnaireTelechargeents {
    constructor() {
        this.telechargementsEnCours = new Map();
    }
    
    telecharger(url, options = {}) {
        return new Promise((resolve, reject) => {
            if (!url || !url.startsWith('http')) {
                return reject(new Error('URL invalide'));
            }
            
            const id = Math.random().toString(36).substr(2, 9);
            const tempsTelechargement = Math.random() * 2000 + 1000;
            const timeout = options.timeout || 5000;
            
            // Simuler échec aléatoire
            if (Math.random() < 0.2) {
                return reject(new Error('Erreur réseau'));
            }
            
            // Timeout
            const timeoutId = setTimeout(() => {
                this.telechargementsEnCours.delete(id);
                reject(new Error('Timeout dépassé'));
            }, timeout);
            
            // Téléchargement
            this.telechargementsEnCours.set(id, { url, debut: Date.now() });
            
            setTimeout(() => {
                clearTimeout(timeoutId);
                this.telechargementsEnCours.delete(id);
                
                resolve({
                    id,
                    url,
                    taille: Math.floor(Math.random() * 1000000),
                    duree: tempsTelechargement
                });
            }, tempsTelechargement);
        });
    }
    
    telechargerPlusieurs(urls) {
        const promises = urls.map(url => this.telecharger(url));
        return Promise.allSettled(promises);
    }
    
    async telechargerAvecRetry(url, maxTentatives = 3) {
        for (let tentative = 1; tentative <= maxTentatives; tentative++) {
            try {
                const resultat = await this.telecharger(url);
                return resultat;
            } catch (erreur) {
                if (tentative === maxTentatives) {
                    throw new Error(`Échec après ${maxTentatives} tentatives: ${erreur.message}`);
                }
                
                // Attendre avant de réessayer
                await new Promise(resolve => setTimeout(resolve, 1000 * tentative));
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

---

## 🚀 Async/Await

### Syntaxe moderne

{% hint style="success" %}
**async/await** est la syntaxe la plus moderne pour gérer l'asynchrone. Elle rend le code asynchrone aussi lisible que du code synchrone.
{% endhint %}

{% tabs %}
{% tab title="🎯 Syntaxe de base" %}
```javascript
// Fonction asynchrone
async function obtenirDonnees() {
    try {
        // await suspend l'exécution jusqu'à la résolution
        const response = await fetch('/api/donnees');
        const donnees = await response.json();
        
        console.log('Données reçues:', donnees);
        return donnees;
    } catch (erreur) {
        console.error('Erreur:', erreur);
        throw erreur;
    }
}

// Utilisation
obtenirDonnees()
    .then(donnees => console.log('Succès:', donnees))
    .catch(erreur => console.log('Erreur capturée:', erreur));
```

**Règles importantes :**
- `async` devant la fonction
- `await` seulement dans une fonction `async`
- Gestion d'erreur avec `try/catch`
{% endtab %}

{% tab title="🔄 Comparaison" %}
```javascript
// Avec Promises
function obtenirUtilisateurPromises(id) {
    return fetch(`/api/utilisateur/${id}`)
        .then(response => response.json())
        .then(utilisateur => {
            return fetch(`/api/profil/${utilisateur.profilId}`);
        })
        .then(response => response.json())
        .then(profil => {
            return { utilisateur, profil };
        })
        .catch(erreur => {
            console.error('Erreur:', erreur);
            throw erreur;
        });
}

// Avec async/await - BEAUCOUP plus lisible !
async function obtenirUtilisateurAsync(id) {
    try {
        const responseUser = await fetch(`/api/utilisateur/${id}`);
        const utilisateur = await responseUser.json();
        
        const responseProfil = await fetch(`/api/profil/${utilisateur.profilId}`);
        const profil = await responseProfil.json();
        
        return { utilisateur, profil };
    } catch (erreur) {
        console.error('Erreur:', erreur);
        throw erreur;
    }
}
```
{% endtab %}

{% tab title="⚡ Parallélisme" %}
```javascript
// ❌ Séquentiel - lent
async function obtenirDonneesSequentiel() {
    const donnees1 = await fetch('/api/donnees1').then(r => r.json());
    const donnees2 = await fetch('/api/donnees2').then(r => r.json()); 
    const donnees3 = await fetch('/api/donnees3').then(r => r.json());
    
    return { donnees1, donnees2, donnees3 };
}

// ✅ Parallèle - rapide
async function obtenirDonneesParallele() {
    const [donnees1, donnees2, donnees3] = await Promise.all([
        fetch('/api/donnees1').then(r => r.json()),
        fetch('/api/donnees2').then(r => r.json()),
        fetch('/api/donnees3').then(r => r.json())
    ]);
    
    return { donnees1, donnees2, donnees3 };
}

// 🎯 Mixte - optimal
async function obtenirDonneesMixte() {
    // D'abord l'utilisateur
    const utilisateur = await fetch('/api/utilisateur').then(r => r.json());
    
    // Puis ses données en parallèle
    const [profil, preferences, notifications] = await Promise.all([
        fetch(`/api/profil/${utilisateur.id}`).then(r => r.json()),
        fetch(`/api/preferences/${utilisateur.id}`).then(r => r.json()),
        fetch(`/api/notifications/${utilisateur.id}`).then(r => r.json())
    ]);
    
    return { utilisateur, profil, preferences, notifications };
}
```
{% endtab %}
{% endtabs %}

### 🏃‍♂️ Exercice Pratique : Async/Await

{% hint style="warning" %}
**Exercice :** Créez un système de cache intelligent avec async/await
{% endhint %}

```javascript
// Complétez cette classe
class CacheIntelligent {
    constructor(dureeVie = 300000) { // 5 minutes par défaut
        this.cache = new Map();
        this.dureeVie = dureeVie;
    }
    
    // TODO: Méthode pour obtenir des données avec cache
    async obtenirDonnees(cle, fonctionChargement) {
        // 1. Vérifier si les données sont en cache et valides
        // 2. Si oui, retourner les données en cache
        // 3. Si non, charger les données et les mettre en cache
        // 4. Gérer les erreurs appropriément
    }
    
    // TODO: Précharger plusieurs clés en parallèle
    async precharger(clesFonctions) {
        // clesFonctions = [
        //   { cle: 'utilisateur', fonction: () => fetch('/api/user') },
        //   { cle: 'profil', fonction: () => fetch('/api/profile') }
        // ]
    }
    
    // TODO: Invalider le cache
    invalider(cle) {
        // Supprimer une entrée du cache
    }
    
    // TODO: Nettoyer le cache expiré
    async nettoyerCache() {
        // Supprimer les entrées expirées
    }
}

// Test
const cache = new CacheIntelligent(60000); // 1 minute

async function testerCache() {
    try {
        // Premier appel - va charger
        console.time('Premier appel');
        const donnees1 = await cache.obtenirDonnees('user-123', async () => {
            await new Promise(resolve => setTimeout(resolve, 1000));
            return { id: 123, nom: 'John Doe' };
        });
        console.timeEnd('Premier appel');
        
        // Deuxième appel - depuis le cache
        console.time('Deuxième appel');
        const donnees2 = await cache.obtenirDonnees('user-123', async () => {
            await new Promise(resolve => setTimeout(resolve, 1000));
            return { id: 123, nom: 'John Doe' };
        });
        console.timeEnd('Deuxième appel');
        
        console.log('Données identiques:', donnees1 === donnees2);
    } catch (erreur) {
        console.error('Erreur:', erreur);
    }
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
async obtenirDonnees(cle, fonctionChargement) {
    // Vérifier le cache
    const entreeCache = this.cache.get(cle);
    
    if (entreeCache) {
        const maintenant = Date.now();
        if (maintenant - entreeCache.timestamp < this.dureeVie) {
            return entreeCache.donnees; // Cache valide
        }
    }
    
    // Charger les nouvelles données
    try {
        const donnees = await fonctionChargement();
        // Mettre en cache
        this.cache.set(cle, {
            donnees,
            timestamp: Date.now()
        });
        return donnees;
    } catch (erreur) {
        // Gérer l'erreur
        throw erreur;
    }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
class CacheIntelligent {
    constructor(dureeVie = 300000) {
        this.cache = new Map();
        this.dureeVie = dureeVie;
        this.chargementsEnCours = new Map();
    }
    
    async obtenirDonnees(cle, fonctionChargement) {
        // Vérifier le cache
        const entreeCache = this.cache.get(cle);
        const maintenant = Date.now();
        
        if (entreeCache && (maintenant - entreeCache.timestamp < this.dureeVie)) {
            return entreeCache.donnees;
        }
        
        // Éviter les chargements multiples simultanés
        if (this.chargementsEnCours.has(cle)) {
            return await this.chargementsEnCours.get(cle);
        }
        
        // Créer la promesse de chargement
        const promesseChargement = this._chargerEtCacher(cle, fonctionChargement);
        this.chargementsEnCours.set(cle, promesseChargement);
        
        try {
            const resultat = await promesseChargement;
            return resultat;
        } finally {
            this.chargementsEnCours.delete(cle);
        }
    }
    
    async _chargerEtCacher(cle, fonctionChargement) {
        try {
            const donnees = await fonctionChargement();
            
            this.cache.set(cle, {
                donnees,
                timestamp: Date.now()
            });
            
            return donnees;
        } catch (erreur) {
            console.error(`Erreur lors du chargement de ${cle}:`, erreur);
            throw erreur;
        }
    }
    
    async precharger(clesFonctions) {
        const promesses = clesFonctions.map(({ cle, fonction }) => 
            this.obtenirDonnees(cle, fonction).catch(erreur => {
                console.warn(`Préchargement échoué pour ${cle}:`, erreur);
                return null;
            })
        );
        
        const resultats = await Promise.allSettled(promesses);
        
        return resultats.map((resultat, index) => ({
            cle: clesFonctions[index].cle,
            succes: resultat.status === 'fulfilled',
            donnees: resultat.status === 'fulfilled' ? resultat.value : null,
            erreur: resultat.status === 'rejected' ? resultat.reason : null
        }));
    }
    
    invalider(cle) {
        if (cle) {
            this.cache.delete(cle);
            this.chargementsEnCours.delete(cle);
        } else {
            this.cache.clear();
            this.chargementsEnCours.clear();
        }
    }
    
    async nettoyerCache() {
        const maintenant = Date.now();
        const clesSupprimees = [];
        
        for (const [cle, entree] of this.cache.entries()) {
            if (maintenant - entree.timestamp >= this.dureeVie) {
                this.cache.delete(cle);
                clesSupprimees.push(cle);
            }
        }
        
        return clesSupprimees;
    }
    
    // Bonus: Statistiques du cache
    obtenirStatistiques() {
        const maintenant = Date.now();
        let valides = 0;
        let expirées = 0;
        
        for (const entree of this.cache.values()) {
            if (maintenant - entree.timestamp < this.dureeVie) {
                valides++;
            } else {
                expirées++;
            }
        }
        
        return {
            totalEntrees: this.cache.size,
            entréesValides: valides,
            entréesExpirées: expirées,
            chargementsEnCours: this.chargementsEnCours.size
        };
    }
}
```
{% endtab %}
{% endtabs %}

---

## 🎯 Projet Final : API Manager Complet

{% hint style="danger" %}
**Projet Capstone :** Créez un gestionnaire d'API complet utilisant tous les concepts asynchrones
{% endhint %}

### Spécifications

Créez une classe `APIManager` qui doit :

1. **Gestion des requêtes** avec retry automatique
2. **Cache intelligent** avec TTL configurable  
3. **Rate limiting** pour éviter le spam
4. **Gestion d'erreurs** robuste avec fallbacks
5. **Monitoring** et statistiques en temps réel
6. **Authentification** avec refresh automatique des tokens

```javascript
class APIManager {
    constructor(options = {}) {
        this.baseURL = options.baseURL || '';
        this.maxRetries = options.maxRetries || 3;
        this.rateLimitPerMinute = options.rateLimitPerMinute || 60;
        this.cacheTTL = options.cacheTTL || 300000; // 5 minutes
        
        // TODO: Initialiser vos propriétés
    }
    
    // TODO: Implémenter toutes les méthodes requises
    async get(endpoint, options = {}) {}
    async post(endpoint, data, options = {}) {}
    async put(endpoint, data, options = {}) {}
    async delete(endpoint, options = {}) {}
    
    // Méthodes avancées
    async batchRequests(requests) {}
    async authentifier(credentials) {}
    obtenirStatistiques() {}
    configurerIntercepteurs(intercepteurs) {}
}

// Tests d'utilisation
async function testerAPIManager() {
    const api = new APIManager({
        baseURL: 'https://jsonplaceholder.typicode.com',
        maxRetries: 2,
        rateLimitPerMinute: 30
    });
    
    try {
        // Test simple
        const utilisateur = await api.get('/users/1');
        console.log('Utilisateur:', utilisateur);
        
        // Test batch
        const requests = [
            { method: 'GET', endpoint: '/users/1' },
            { method: 'GET', endpoint: '/users/2' },
            { method: 'GET', endpoint: '/posts/1' }
        ];
        
        const resultats = await api.batchRequests(requests);
        console.log('Résultats batch:', resultats);
        
        // Statistiques
        console.log('Stats:', api.obtenirStatistiques());
        
    } catch (erreur) {
        console.error('Erreur dans les tests:', erreur);
    }
}

testerAPIManager();
```

### Critères d'évaluation

{% tabs %}
{% tab title="🎯 Fonctionnalités requises" %}
- [ ] **Requêtes HTTP** complètes (GET, POST, PUT, DELETE)
- [ ] **Retry automatique** avec backoff exponentiel
- [ ] **Cache intelligent** avec invalidation
- [ ] **Rate limiting** avec queue
- [ ] **Gestion d'erreurs** avec fallbacks
- [ ] **Monitoring** en temps réel
- [ ] **Tests unitaires** complets
{% endtab %}

{% tab title="⭐ Fonctionnalités bonus" %}
- [ ] **Authentification** JWT avec refresh
- [ ] **Intercepteurs** de requête/réponse
- [ ] **Offline mode** avec synchronisation
- [ ] **Compression** automatique
- [ ] **Métriques avancées** (latence, succès rate)
- [ ] **WebSocket** pour notifications
- [ ] **Service Worker** integration
{% endtab %}

{% tab title="🏆 Excellence" %}
- [ ] **Architecture** modulaire et extensible
- [ ] **Performance** optimisée
- [ ] **Documentation** complète
- [ ] **Tests** exhaustifs avec mocks
- [ ] **TypeScript** types
- [ ] **CI/CD** pipeline
- [ ] **Monitoring** production-ready
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant la programmation asynchrone en JavaScript !
{% endhint %}

### Ce que vous avez appris

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Event Loop** : Le cœur de l'asynchrone
- **Callbacks** : La base, mais avec limitations
- **Promises** : Solution élégante avec chaînage
- **async/await** : Syntaxe moderne et lisible
- **Parallélisme** : Optimisation des performances
- **Gestion d'erreurs** : Robustesse et fiabilité
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Créer des applications non-bloquantes
- Gérer les requêtes réseau efficacement  
- Implémenter des systèmes de cache
- Optimiser les performances asynchrones
- Déboguer le code asynchrone
- Tester les fonctions asynchrones
{% endtab %}

{% tab title="🚀 Prochaines étapes" %}
- **Workers** : Calculs intensifs en arrière-plan
- **Streams** : Traitement de données en flux
- **WebRTC** : Communication peer-to-peer
- **GraphQL** : APIs modernes et efficaces
- **Observables** : Programmation réactive
- **WebAssembly** : Performance native
{% endtab %}
{% endtabs %}

### Ressources pour aller plus loin

- 📚 [MDN - Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)
- 🎥 [JavaScript Event Loop Visualizer](http://latentflip.com/loupe/)
- 📖 [You Don't Know JS: Async & Performance](https://github.com/getify/You-Dont-Know-JS)
- 🛠️ [Async/Await Best Practices](https://blog.risingstack.com/mastering-async-await-in-nodejs/)

---

**Prêt pour le niveau expert ? Continuez avec les modules avancés !** 🚀
