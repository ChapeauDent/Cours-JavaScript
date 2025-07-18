# Programmation Asynchrone en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnees du cours
- **Titre :** Programmation asynchrone - Callbacks, Promises, Async/Await et Event Loop
- **Niveau :** Avance
- **Duree estimee :** 75 minutes
- **Prerequis :** Fonctions, objets, gestion d'erreurs, concepts de base du navigateur
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## **OBJECTIFS PEDAGOGIQUES**

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Le concept de programmation asynchrone et pourquoi elle est necessaire
- [ ] **Appliquer** : Les callbacks, promises et async/await dans des scenarios reels
- [ ] **Analyser** : Le fonctionnement de l'Event Loop et la pile d'execution
- [ ] **Creer** : Des applications complexes avec gestion asynchrone

### Mots-cles a retenir
- **Asynchrone** : Execution non-bloquante qui permet d'autres operations
- **Callback** : Fonction passee en parametre, executee plus tard
- **Promise** : Objet representant le resultat futur d'une operation asynchrone
- **async/await** : Syntaxe moderne pour ecrire du code asynchrone lisible
- **Event Loop** : Mecanisme qui gere l'execution du code asynchrone

---

## **PARTIE THEORIQUE**

### Pourquoi ce concept est-il important ?
JavaScript est un langage mono-thread, ce qui signifie qu'il ne peut executer qu'une instruction a la fois. Sans programmation asynchrone, votre interface utilisateur se figerait pendant les operations longues (chargement de fichiers, requetes reseau). La programmation asynchrone permet de maintenir une experience fluide en executant ces operations en arriere-plan.

### Definition et concepts cles

#### 1. Le probleme du code synchrone
```javascript
// Code synchrone - BLOQUE l'execution
console.log("Debut");
// Imaginez une operation qui prend 3 secondes
for (let i = 0; i < 3000000000; i++) {} // Simulation d'attente
console.log("Fin"); // Ne s'execute qu'apres l'attente
```

#### 2. Evolution des solutions asynchrones
- **Callbacks** : Premiere solution, mais creent le "callback hell"
- **Promises** : Solution plus elegante avec chainage
- **async/await** : Syntaxe la plus moderne et lisible

#### 3. Event Loop
Le mecanisme qui permet a JavaScript de gerer l'asynchrone :
- **Call Stack** : Pile d'execution principale
- **Web APIs** : APIs du navigateur (setTimeout, fetch, etc.)
- **Callback Queue** : File d'attente des callbacks
- **Event Loop** : Verifie si la pile est vide pour traiter la queue

### Syntaxe de base
```javascript
// Callback
setTimeout(function() {
    console.log("Callback execute");
}, 1000);

// Promise
fetch("/api/data")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));

// async/await
async function chargerDonnees() {
    try {
        const response = await fetch("/api/data");
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}
```

### Points d'attention
- **Piege courant 1** : Oublier de gerer les erreurs dans les promises
- **Piege courant 2** : Utiliser await sans async ou dans le mauvais contexte
- **Bonne pratique** : Toujours preferer async/await pour la lisibilite

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Simulation d'une operation asynchrone simple
function simulerChargement(temps, message) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (temps > 5000) {
                reject(new Error("Timeout - operation trop longue"));
            } else {
                resolve(message);
            }
        }, temps);
    });
}

// Utilisation avec .then()
console.log("Debut du chargement...");
simulerChargement(2000, "Donnees chargees !")
    .then(message => {
        console.log("Succes:", message);
    })
    .catch(error => {
        console.error("Erreur:", error.message);
    });

console.log("Cette ligne s'execute immediatement");

// Utilisation avec async/await
async function demonstrationAsync() {
    try {
        console.log("Debut async...");
        const resultat = await simulerChargement(1500, "Async termine !");
        console.log("Resultat:", resultat);
    } catch (error) {
        console.error("Erreur async:", error.message);
    }
}

demonstrationAsync();
```

**Resultat attendu :**
```
Debut du chargement...
Cette ligne s'execute immediatement
Debut async...
Succes: Donnees chargees !
Resultat: Async termine !
```

### Exemple intermediaire
```javascript
// Gestionnaire de taches asynchrones avec concurrence
class GestionnaireTaches {
    constructor() {
        this.tachesEnCours = new Map();
        this.resultats = [];
    }
    
    // Creer une tache avec ID unique
    async creerTache(id, operationAsync, timeout = 5000) {
        if (this.tachesEnCours.has(id)) {
            throw new Error(`Tache ${id} deja en cours`);
        }
        
        // Promise avec timeout
        const promiseAvecTimeout = Promise.race([
            operationAsync(),
            new Promise((_, reject) => 
                setTimeout(() => reject(new Error("Timeout")), timeout)
            )
        ]);
        
        this.tachesEnCours.set(id, promiseAvecTimeout);
        
        try {
            const resultat = await promiseAvecTimeout;
            this.resultats.push({ id, resultat, statut: "succes" });
            return resultat;
        } catch (error) {
            this.resultats.push({ id, erreur: error.message, statut: "echec" });
            throw error;
        } finally {
            this.tachesEnCours.delete(id);
        }
    }
    
    // Executer plusieurs taches en parallele
    async executerEnParallele(taches) {
        const promises = taches.map(tache => 
            this.creerTache(tache.id, tache.operation, tache.timeout)
                .catch(error => ({ erreur: error.message, id: tache.id }))
        );
        
        return await Promise.all(promises);
    }
    
    // Executer taches en sequence
    async executerEnSequence(taches) {
        const resultats = [];
        for (const tache of taches) {
            try {
                const resultat = await this.creerTache(
                    tache.id, 
                    tache.operation, 
                    tache.timeout
                );
                resultats.push({ id: tache.id, resultat });
            } catch (error) {
                resultats.push({ id: tache.id, erreur: error.message });
            }
        }
        return resultats;
    }
    
    // Obtenir le statut de toutes les taches
    obtenirStatut() {
        return {
            enCours: Array.from(this.tachesEnCours.keys()),
            terminees: this.resultats
        };
    }
}

// Demonstration
async function demonstrationGestionnaire() {
    const gestionnaire = new GestionnaireTaches();
    
    // Definition des taches
    const taches = [
        {
            id: "fetch-user",
            operation: () => simulerRequeteAPI("users/123", 1000),
            timeout: 3000
        },
        {
            id: "fetch-posts",
            operation: () => simulerRequeteAPI("posts", 1500),
            timeout: 3000
        },
        {
            id: "fetch-comments",
            operation: () => simulerRequeteAPI("comments", 800),
            timeout: 3000
        }
    ];
    
    console.log("Execution en parallele:");
    const resultatsParallele = await gestionnaire.executerEnParallele(taches);
    console.log(resultatsParallele);
    
    console.log("\nExecution en sequence:");
    const resultatsSequence = await gestionnaire.executerEnSequence(taches);
    console.log(resultatsSequence);
}

// Fonction utilitaire pour simuler une API
function simulerRequeteAPI(endpoint, delai) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({
                endpoint,
                data: `Donnees de ${endpoint}`,
                timestamp: new Date().toISOString()
            });
        }, delai);
    });
}
```

### Exemple avance (optionnel)
```javascript
// Implementation d'un cache asynchrone avec expiration
class CacheAsynchrone {
    constructor(ttlDefault = 300000) { // 5 minutes par defaut
        this.cache = new Map();
        this.ttlDefault = ttlDefault;
        this.requestsEnCours = new Map();
    }
    
    // Obtenir une valeur avec cache intelligent
    async obtenir(cle, factoryFunction, ttl = this.ttlDefault) {
        // Verifier le cache
        const entreeCache = this.cache.get(cle);
        if (entreeCache && Date.now() < entreeCache.expiration) {
            console.log(`Cache hit pour: ${cle}`);
            return entreeCache.valeur;
        }
        
        // Eviter les requetes duplicatas
        if (this.requestsEnCours.has(cle)) {
            console.log(`Attente requete en cours: ${cle}`);
            return await this.requestsEnCours.get(cle);
        }
        
        // Executer la factory function
        console.log(`Cache miss, execution factory: ${cle}`);
        const promiseRequete = this._executerAvecRetry(factoryFunction, 3);
        this.requestsEnCours.set(cle, promiseRequete);
        
        try {
            const valeur = await promiseRequete;
            
            // Mettre en cache
            this.cache.set(cle, {
                valeur,
                expiration: Date.now() + ttl,
                creeA: Date.now()
            });
            
            return valeur;
        } finally {
            this.requestsEnCours.delete(cle);
        }
    }
    
    // Retry automatique avec backoff exponentiel
    async _executerAvecRetry(fn, maxTentatives) {
        let derniereErreur;
        
        for (let tentative = 0; tentative < maxTentatives; tentative++) {
            try {
                return await fn();
            } catch (error) {
                derniereErreur = error;
                
                if (tentative < maxTentatives - 1) {
                    const delaiAttente = Math.pow(2, tentative) * 1000; // Backoff exponentiel
                    console.log(`Tentative ${tentative + 1} echouee, retry dans ${delaiAttente}ms`);
                    await new Promise(resolve => setTimeout(resolve, delaiAttente));
                }
            }
        }
        
        throw derniereErreur;
    }
    
    // Invalidation manuelle
    invalider(cle) {
        this.cache.delete(cle);
        if (this.requestsEnCours.has(cle)) {
            console.log(`Annulation requete en cours: ${cle}`);
            this.requestsEnCours.delete(cle);
        }
    }
    
    // Nettoyage automatique des entrees expirees
    nettoyer() {
        const maintenant = Date.now();
        let supprimees = 0;
        
        for (const [cle, entree] of this.cache.entries()) {
            if (maintenant >= entree.expiration) {
                this.cache.delete(cle);
                supprimees++;
            }
        }
        
        console.log(`Nettoyage: ${supprimees} entrees supprimees`);
    }
    
    // Statistiques du cache
    obtenirStats() {
        const maintenant = Date.now();
        let valides = 0;
        let expirees = 0;
        
        for (const entree of this.cache.values()) {
            if (maintenant < entree.expiration) {
                valides++;
            } else {
                expirees++;
            }
        }
        
        return {
            total: this.cache.size,
            valides,
            expirees,
            requestsEnCours: this.requestsEnCours.size
        };
    }
}

// Demonstration avancee
async function demonstrationCacheAvance() {
    const cache = new CacheAsynchrone(10000); // TTL de 10 secondes
    
    // Function factory qui simule une API lente
    const chargerUtilisateur = async (id) => {
        console.log(`Chargement utilisateur ${id} depuis l'API...`);
        await new Promise(resolve => setTimeout(resolve, 2000)); // Simule latence
        
        // Simulation d'echec aleatoire
        if (Math.random() < 0.3) {
            throw new Error("Erreur reseau temporaire");
        }
        
        return {
            id,
            nom: `Utilisateur ${id}`,
            chargeA: new Date().toISOString()
        };
    };
    
    try {
        // Premier appel - miss cache
        const user1 = await cache.obtenir("user:123", () => chargerUtilisateur(123));
        console.log("Premier appel:", user1);
        
        // Deuxieme appel immediate - hit cache
        const user2 = await cache.obtenir("user:123", () => chargerUtilisateur(123));
        console.log("Deuxieme appel:", user2);
        
        // Appels paralleles - deduplication
        const [user3, user4, user5] = await Promise.all([
            cache.obtenir("user:456", () => chargerUtilisateur(456)),
            cache.obtenir("user:456", () => chargerUtilisateur(456)),
            cache.obtenir("user:456", () => chargerUtilisateur(456))
        ]);
        
        console.log("Stats finales:", cache.obtenirStats());
        
    } catch (error) {
        console.error("Erreur finale:", error.message);
    }
}
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Gestionnaire de telechargements
**Consigne :** Creez un gestionnaire qui peut telecharger plusieurs fichiers en parallele avec suivi de progression.

**Code de depart :**
```javascript
class GestionnaireTelechargements {
    constructor() {
        // TODO: Initialiser les proprietes necessaires
    }
    
    async telecharger(url, nomFichier) {
        // TODO: Simuler telechargement avec progression
        // Utiliser setTimeout pour simuler les chunks de donnees
    }
    
    async telechargerMultiples(listeUrls) {
        // TODO: Telecharger en parallele avec Promise.all
        // Gerer les echecs individuels
    }
    
    obtenirProgression() {
        // TODO: Retourner l'etat de tous les telechargements
    }
}

// Test
const gestionnaire = new GestionnaireTelechargements();
// Implementer les tests
```

**Resultat attendu :**
```javascript
// Progression en temps reel des telechargements
// Gestion des erreurs individuelles
// Statistiques finales
```

**Niveau de difficulte :** ⭐⭐⭐☆☆

### Exercice 2 : Systeme de queue avec priorite
**Consigne :** Implementez une queue asynchrone qui execute les taches par ordre de priorite.

**Contraintes :**
- Limite du nombre de taches simultanees
- Priorites numeriques (plus bas = plus prioritaire)
- Possibilite d'annuler des taches en attente

**Niveau de difficulte :** ⭐⭐⭐⭐☆

### Exercice 3 : WebSocket avec reconnexion automatique (optionnel)
**Consigne :** Creez un wrapper WebSocket robuste avec reconnexion automatique et gestion d'erreurs.

**Niveau de difficulte :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
class GestionnaireTelechargements {
    constructor() {
        this.telechargements = new Map();
        this.evenements = new EventTarget();
    }
    
    async telecharger(url, nomFichier) {
        const id = `${nomFichier}-${Date.now()}`;
        const etat = {
            id,
            url,
            nomFichier,
            progression: 0,
            statut: "en_cours",
            tailleTotal: Math.floor(Math.random() * 1000000) + 500000, // Simulation
            tailleTelecharge: 0,
            erreur: null
        };
        
        this.telechargements.set(id, etat);
        this._emettreMiseAJour(id);
        
        try {
            // Simuler telechargement par chunks
            const chunks = 20;
            const tailleChunk = etat.tailleTotal / chunks;
            
            for (let i = 0; i < chunks; i++) {
                // Simulation d'erreur aleatoire
                if (Math.random() < 0.05) {
                    throw new Error("Erreur reseau");
                }
                
                // Attente simulee pour chunk
                await new Promise(resolve => 
                    setTimeout(resolve, Math.random() * 200 + 50)
                );
                
                etat.tailleTelecharge += tailleChunk;
                etat.progression = Math.floor((etat.tailleTelecharge / etat.tailleTotal) * 100);
                this._emettreMiseAJour(id);
            }
            
            etat.statut = "termine";
            etat.progression = 100;
            this._emettreMiseAJour(id);
            
            return {
                id,
                nomFichier,
                taille: etat.tailleTotal,
                duree: Date.now() - parseInt(id.split("-")[1])
            };
            
        } catch (error) {
            etat.statut = "erreur";
            etat.erreur = error.message;
            this._emettreMiseAJour(id);
            throw error;
        }
    }
    
    async telechargerMultiples(listeUrls) {
        const promises = listeUrls.map(async ({ url, nomFichier }) => {
            try {
                return await this.telecharger(url, nomFichier);
            } catch (error) {
                return {
                    url,
                    nomFichier,
                    erreur: error.message,
                    statut: "echec"
                };
            }
        });
        
        return await Promise.all(promises);
    }
    
    obtenirProgression() {
        const telechargements = Array.from(this.telechargements.values());
        const total = telechargements.length;
        const termines = telechargements.filter(t => t.statut === "termine").length;
        const enCours = telechargements.filter(t => t.statut === "en_cours").length;
        const erreurs = telechargements.filter(t => t.statut === "erreur").length;
        
        const progressionGlobale = total > 0 
            ? Math.floor(telechargements.reduce((sum, t) => sum + t.progression, 0) / total)
            : 0;
        
        return {
            total,
            termines,
            enCours,
            erreurs,
            progressionGlobale,
            details: telechargements
        };
    }
    
    _emettreMiseAJour(id) {
        const etat = this.telechargements.get(id);
        this.evenements.dispatchEvent(new CustomEvent("progression", {
            detail: { id, etat }
        }));
    }
    
    // Ecouter les mises a jour
    onProgression(callback) {
        this.evenements.addEventListener("progression", callback);
    }
}

// Demonstration
async function demonstrationTelechargements() {
    const gestionnaire = new GestionnaireTelechargements();
    
    // Ecouter les mises a jour
    gestionnaire.onProgression((event) => {
        const { id, etat } = event.detail;
        console.log(`${etat.nomFichier}: ${etat.progression}% (${etat.statut})`);
    });
    
    const fichiers = [
        { url: "https://example.com/file1.zip", nomFichier: "file1.zip" },
        { url: "https://example.com/file2.pdf", nomFichier: "file2.pdf" },
        { url: "https://example.com/file3.mp4", nomFichier: "file3.mp4" }
    ];
    
    try {
        console.log("Debut des telechargements...");
        const resultats = await gestionnaire.telechargerMultiples(fichiers);
        console.log("Resultats finaux:", resultats);
        console.log("Progression finale:", gestionnaire.obtenirProgression());
    } catch (error) {
        console.error("Erreur globale:", error);
    }
}
```

**Explication :** Cette solution simule des telechargements reels avec progression, gestion d'erreurs et events pour le suivi en temps reel.

### Solution Exercice 2
```javascript
class QueueAvecPriorite {
    constructor(maxConcurrence = 3) {
        this.queue = [];
        this.enCours = new Set();
        this.maxConcurrence = maxConcurrence;
        this.compteurId = 0;
        this.resultats = new Map();
    }
    
    async ajouter(tache, priorite = 5) {
        const id = ++this.compteurId;
        const promiseResolution = {};
        
        const promise = new Promise((resolve, reject) => {
            promiseResolution.resolve = resolve;
            promiseResolution.reject = reject;
        });
        
        const elementQueue = {
            id,
            tache,
            priorite,
            promise,
            resolve: promiseResolution.resolve,
            reject: promiseResolution.reject,
            ajouteA: Date.now(),
            statut: "en_attente"
        };
        
        // Insertion par priorite (plus bas = plus prioritaire)
        let position = this.queue.findIndex(item => item.priorite > priorite);
        if (position === -1) {
            this.queue.push(elementQueue);
        } else {
            this.queue.splice(position, 0, elementQueue);
        }
        
        this._traiterQueue();
        return { id, promise };
    }
    
    async _traiterQueue() {
        while (this.queue.length > 0 && this.enCours.size < this.maxConcurrence) {
            const element = this.queue.shift();
            element.statut = "en_cours";
            this.enCours.add(element.id);
            
            // Executer la tache de maniere asynchrone
            this._executerTache(element);
        }
    }
    
    async _executerTache(element) {
        try {
            console.log(`Execution tache ${element.id} (priorite: ${element.priorite})`);
            const resultat = await element.tache();
            
            element.statut = "termine";
            this.resultats.set(element.id, {
                id: element.id,
                resultat,
                statut: "succes",
                duree: Date.now() - element.ajouteA
            });
            
            element.resolve(resultat);
            
        } catch (error) {
            element.statut = "erreur";
            this.resultats.set(element.id, {
                id: element.id,
                erreur: error.message,
                statut: "echec",
                duree: Date.now() - element.ajouteA
            });
            
            element.reject(error);
            
        } finally {
            this.enCours.delete(element.id);
            // Continuer le traitement de la queue
            this._traiterQueue();
        }
    }
    
    annuler(id) {
        // Chercher dans la queue
        const index = this.queue.findIndex(item => item.id === id);
        if (index !== -1) {
            const element = this.queue.splice(index, 1)[0];
            element.reject(new Error("Tache annulee"));
            return true;
        }
        return false; // Tache deja en cours ou terminee
    }
    
    obtenirStatut() {
        return {
            enAttente: this.queue.length,
            enCours: this.enCours.size,
            terminees: this.resultats.size,
            queueDetails: this.queue.map(item => ({
                id: item.id,
                priorite: item.priorite,
                statut: item.statut
            }))
        };
    }
}

// Demonstration
async function demonstrationQueue() {
    const queue = new QueueAvecPriorite(2); // Max 2 taches simultanees
    
    // Ajouter des taches avec differentes priorites
    const taches = [
        { priorite: 3, nom: "Tache normale", duree: 2000 },
        { priorite: 1, nom: "Tache prioritaire", duree: 1000 },
        { priorite: 5, nom: "Tache basse priorite", duree: 1500 },
        { priorite: 2, nom: "Tache importante", duree: 3000 },
        { priorite: 1, nom: "Tache urgente", duree: 500 }
    ];
    
    const promises = [];
    
    for (const spec of taches) {
        const { id, promise } = await queue.ajouter(
            () => new Promise(resolve => 
                setTimeout(() => resolve(`${spec.nom} terminee`), spec.duree)
            ),
            spec.priorite
        );
        
        promises.push(
            promise
                .then(resultat => console.log(`Resultat ${id}: ${resultat}`))
                .catch(error => console.log(`Erreur ${id}: ${error.message}`))
        );
    }
    
    console.log("Statut initial:", queue.obtenirStatut());
    
    // Attendre toutes les taches
    await Promise.allSettled(promises);
    console.log("Statut final:", queue.obtenirStatut());
}
```

**Alternatives possibles :**
- Implementer une pause/reprise de la queue
- Ajouter des callbacks de progression pour chaque tache

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts prealables utilises
- **Fonctions** : Callbacks sont des fonctions passees en parametre
- **Objets** : Promises sont des objets avec methodes then/catch
- **Gestion d'erreurs** : try/catch avec async/await

### Prochaines etapes
- **APIs du navigateur** : fetch, WebSocket, Service Workers
- **Node.js** : Programmation asynchrone cote serveur
- **Performance** : Optimisation des operations asynchrones

### Concepts complementaires
- **Event handling** : Gestion d'evenements asynchrones
- **State management** : Gestion d'etat avec operations asynchrones

---

## **PROJET MINI**

### Defi pratique
**Objectif :** Creer une application de chat en temps reel avec gestion de messages hors ligne

**Fonctionnalites requises :**
- [ ] Interface de chat avec messages en temps reel
- [ ] Queue de messages hors ligne avec retry automatique
- [ ] Indicateurs de statut (en ligne, en cours d'envoi, echec)
- [ ] Synchronisation automatique a la reconnexion

**Structure de fichiers suggeree :**
```
📁 chat-app/
├── 📄 index.html
├── 📄 style.css
├── 📄 chat.js
├── 📄 queue-manager.js
└── 📄 websocket-wrapper.js
```

**Temps estime :** 120 minutes

---

## **RESSOURCES COMPLEMENTAIRES**

### Documentation officielle
- [MDN - Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)
- [MDN - Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN - async/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

### Videos recommandees
- "JavaScript Promises In 10 Minutes" - Web Dev Simplified (10 min)
- "The Event Loop" - Philip Roberts at JSConf (26 min)
- Pourquoi ces videos sont utiles : Explications visuelles du fonctionnement interne

### Articles approfondis
- "JavaScript Event Loop Explained" - JavaScript.info
- "Async/Await Best Practices" - JavaScript.info
- Ce que ces articles apportent en plus : Patterns avances et optimisations

### Outils de pratique
- [Event Loop Visualizer](http://latentflip.com/loupe/)
- [Promise Exercises](https://github.com/stevekane/promise-it-wont-hurt)

---

## **AUTO-EVALUATION**

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre Promise.all() et Promise.allSettled() ?
   - Reponse : Promise.all() echoue si une promise echoue, Promise.allSettled() attend toutes les promises

2. **Question pratique :** Que fait ce code ?
   ```javascript
   async function exemple() {
       console.log("1");
       await Promise.resolve();
       console.log("2");
   }
   console.log("3");
   exemple();
   console.log("4");
   ```
   - Reponse : Affiche "3", "1", "4", puis "2" (a cause de l'event loop)

3. **Question d'application :** Quand utiliseriez-vous Promise.race() ?
   - Reponse : Pour implementer des timeouts, choisir la reponse la plus rapide parmi plusieurs APIs

### Checklist de maitrise
- [ ] Je comprends la difference entre synchrone et asynchrone
- [ ] Je sais utiliser async/await correctement
- [ ] Je peux gerer les erreurs dans le code asynchrone
- [ ] Je comprends le fonctionnement de l'Event Loop
- [ ] Je peux optimiser les performances asynchrones
- [ ] Je peux combiner plusieurs operations asynchrones

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur frequente 1
**Code problematique :**
```javascript
async function maFonction() {
    const data = await fetch("/api");
    return data.json(); // Oubli d'await
}
```

**Probleme :** Retourne une Promise au lieu de la valeur

**Solution :**
```javascript
async function maFonction() {
    const data = await fetch("/api");
    return await data.json(); // ou juste data.json() sans await
}
```

**Explication :** Dans une fonction async, return await est optionnel pour la derniere instruction.

### Erreur frequente 2
**Code problematique :**
```javascript
for (let i = 0; i < urls.length; i++) {
    await fetch(urls[i]); // Execution sequentielle non-optimale
}
```

**Probleme :** Les requetes s'executent une par une au lieu d'en parallele

**Solution :**
```javascript
const promises = urls.map(url => fetch(url));
await Promise.all(promises); // Execution parallele
```

**Explication :** Promise.all() permet d'executer les operations en parallele pour de meilleures performances.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Le fonctionnement de l'Event Loop JavaScript
- Les differences entre callbacks, promises et async/await
- Comment optimiser les performances asynchrones

### Questions a approfondir
- Microtasks vs macrotasks dans l'Event Loop
- Patterns avances avec generators et async iterators

### Revision programmee
- [ ] Revision dans 1 jour (rappel rapide)
- [ ] Revision dans 1 semaine (exercices)
- [ ] Revision dans 1 mois (application dans un projet)

---

## **TAGS ET METADONNEES**

**Mots-cles :** #javascript #asynchrone #promises #async-await #event-loop #callbacks #avance

**Temps de completion :** [Temps reellement passe]

**Difficulte ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Pret pour le concept suivant :** ✅ Oui / ❌ Non (revision necessaire)

---

*📅 Fiche creee le : 17/07/2025*
*🔄 Derniere revision : 17/07/2025*
*📍 Position dans la roadmap : 13/126*
