# APIs du Navigateur JavaScript

## **INFORMATIONS GENERALES**

### Metadonnees du cours
- **Titre :** APIs du navigateur - DOM avance, BOM, Fetch, Storage et APIs modernes
- **Niveau :** Avance
- **Duree estimee :** 80 minutes
- **Prerequis :** DOM de base, programmation asynchrone, gestion d'erreurs, objets
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## **OBJECTIFS PEDAGOGIQUES**

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Les principales APIs du navigateur et leurs cas d'usage
- [ ] **Appliquer** : Fetch API pour les requetes HTTP, Web Storage pour la persistance
- [ ] **Analyser** : Le BOM (Browser Object Model) et ses possibilites
- [ ] **Creer** : Des applications utilisant les APIs modernes du navigateur

### Mots-cles a retenir
- **API du navigateur** : Interface fournie par le navigateur pour interagir avec ses fonctionnalites
- **Fetch API** : Interface moderne pour faire des requetes HTTP
- **Web Storage** : localStorage et sessionStorage pour stocker des donnees
- **BOM** : Browser Object Model, objets pour interagir avec le navigateur
- **Geolocation** : API pour obtenir la position geographique

---

## **PARTIE THEORIQUE**

### Pourquoi ce concept est-il important ?
Les APIs du navigateur permettent a vos applications JavaScript d'interagir avec l'environnement du navigateur : faire des requetes reseau, stocker des donnees localement, acceder a la geolocalisation, envoyer des notifications, etc. Maitriser ces APIs est essentiel pour creer des applications web modernes et interactives.

### Definition et concepts cles

#### 1. Categories d'APIs du navigateur
- **DOM APIs** : Manipulation des elements HTML/CSS
- **Network APIs** : Fetch, WebSocket, Server-Sent Events
- **Storage APIs** : localStorage, sessionStorage, IndexedDB
- **Device APIs** : Geolocation, Camera, Microphone
- **Performance APIs** : Performance measurement, timing
- **Security APIs** : Crypto, CSP

#### 2. Fetch API vs XMLHttpRequest
```javascript
// Ancienne methode (XMLHttpRequest)
const xhr = new XMLHttpRequest();
xhr.open('GET', '/api/data');
xhr.onload = function() {
    if (xhr.status === 200) {
        const data = JSON.parse(xhr.responseText);
        console.log(data);
    }
};
xhr.send();

// Methode moderne (Fetch API)
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

#### 3. Web Storage vs Cookies
- **Cookies** : Limites a 4KB, envoyes avec chaque requete
- **localStorage** : Jusqu'a 5-10MB, persiste entre les sessions
- **sessionStorage** : Jusqu'a 5-10MB, supprime a la fermeture de l'onglet

### Syntaxe de base
```javascript
// Fetch API
const response = await fetch('/api/endpoint', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data)
});

// Web Storage
localStorage.setItem('key', 'value');
const value = localStorage.getItem('key');

// Geolocation
navigator.geolocation.getCurrentPosition(position => {
    console.log(position.coords.latitude, position.coords.longitude);
});
```

### Points d'attention
- **Piege courant 1** : Oublier de gerer les erreurs reseau avec fetch
- **Piege courant 2** : Stocker des objets directement sans JSON.stringify
- **Bonne pratique** : Toujours verifier la disponibilite des APIs avant utilisation

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Gestionnaire d'API simple avec fetch
class APIClient {
    constructor(baseURL = '') {
        this.baseURL = baseURL;
        this.defaultHeaders = {
            'Content-Type': 'application/json'
        };
    }
    
    // Methode GET
    async get(endpoint) {
        try {
            const response = await fetch(`${this.baseURL}${endpoint}`, {
                method: 'GET',
                headers: this.defaultHeaders
            });
            
            if (!response.ok) {
                throw new Error(`HTTP Error: ${response.status} ${response.statusText}`);
            }
            
            return await response.json();
        } catch (error) {
            console.error('Erreur GET:', error);
            throw error;
        }
    }
    
    // Methode POST
    async post(endpoint, data) {
        try {
            const response = await fetch(`${this.baseURL}${endpoint}`, {
                method: 'POST',
                headers: this.defaultHeaders,
                body: JSON.stringify(data)
            });
            
            if (!response.ok) {
                throw new Error(`HTTP Error: ${response.status} ${response.statusText}`);
            }
            
            return await response.json();
        } catch (error) {
            console.error('Erreur POST:', error);
            throw error;
        }
    }
}

// Gestionnaire de stockage local
class StorageManager {
    // Sauvegarder un objet
    save(key, data) {
        try {
            const serializedData = JSON.stringify(data);
            localStorage.setItem(key, serializedData);
            return true;
        } catch (error) {
            console.error('Erreur sauvegarde:', error);
            return false;
        }
    }
    
    // Charger un objet
    load(key) {
        try {
            const data = localStorage.getItem(key);
            return data ? JSON.parse(data) : null;
        } catch (error) {
            console.error('Erreur chargement:', error);
            return null;
        }
    }
    
    // Supprimer une cle
    remove(key) {
        localStorage.removeItem(key);
    }
    
    // Vider tout le stockage
    clear() {
        localStorage.clear();
    }
}

// Demonstration
async function demonstrationBase() {
    const api = new APIClient('https://jsonplaceholder.typicode.com');
    const storage = new StorageManager();
    
    try {
        // Charger des donnees depuis l'API
        console.log('Chargement des donnees...');
        const posts = await api.get('/posts?_limit=3');
        console.log('Posts charges:', posts);
        
        // Sauvegarder en local
        storage.save('posts', posts);
        console.log('Donnees sauvegardees localement');
        
        // Recharger depuis le stockage local
        const localPosts = storage.load('posts');
        console.log('Donnees rechargees:', localPosts);
        
    } catch (error) {
        console.error('Erreur demonstration:', error);
    }
}

demonstrationBase();
```

**Resultat attendu :**
```
Chargement des donnees...
Posts charges: [{ id: 1, title: "...", body: "..." }, ...]
Donnees sauvegardees localement
Donnees rechargees: [{ id: 1, title: "...", body: "..." }, ...]
```

### Exemple intermediaire
```javascript
// Gestionnaire complet d'APIs avec cache et retry
class AdvancedAPIManager {
    constructor(options = {}) {
        this.baseURL = options.baseURL || '';
        this.timeout = options.timeout || 10000;
        this.retryAttempts = options.retryAttempts || 3;
        this.retryDelay = options.retryDelay || 1000;
        this.cache = new Map();
        this.cacheTimeout = options.cacheTimeout || 300000; // 5 minutes
    }
    
    // Requete avec cache et retry
    async request(endpoint, options = {}) {
        const cacheKey = this._getCacheKey(endpoint, options);
        
        // Verifier le cache
        if (options.useCache !== false) {
            const cached = this._getFromCache(cacheKey);
            if (cached) {
                console.log('Donnees trouvees en cache:', endpoint);
                return cached;
            }
        }
        
        // Faire la requete avec retry
        const data = await this._requestWithRetry(endpoint, options);
        
        // Mettre en cache
        if (options.useCache !== false) {
            this._setCache(cacheKey, data);
        }
        
        return data;
    }
    
    async _requestWithRetry(endpoint, options) {
        let lastError;
        
        for (let attempt = 1; attempt <= this.retryAttempts; attempt++) {
            try {
                return await this._makeRequest(endpoint, options);
            } catch (error) {
                lastError = error;
                
                if (attempt < this.retryAttempts) {
                    console.warn(`Tentative ${attempt} echouee, retry dans ${this.retryDelay}ms`);
                    await this._delay(this.retryDelay * attempt); // Backoff exponentiel
                }
            }
        }
        
        throw lastError;
    }
    
    async _makeRequest(endpoint, options) {
        const controller = new AbortController();
        const timeoutId = setTimeout(() => controller.abort(), this.timeout);
        
        try {
            const response = await fetch(`${this.baseURL}${endpoint}`, {
                ...options,
                signal: controller.signal
            });
            
            clearTimeout(timeoutId);
            
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            }
            
            // Detecter le type de contenu
            const contentType = response.headers.get('content-type');
            if (contentType && contentType.includes('application/json')) {
                return await response.json();
            } else {
                return await response.text();
            }
            
        } catch (error) {
            clearTimeout(timeoutId);
            
            if (error.name === 'AbortError') {
                throw new Error('Requete timeout');
            }
            throw error;
        }
    }
    
    _getCacheKey(endpoint, options) {
        return `${endpoint}_${JSON.stringify(options)}`;
    }
    
    _getFromCache(key) {
        const cached = this.cache.get(key);
        if (cached && Date.now() - cached.timestamp < this.cacheTimeout) {
            return cached.data;
        }
        this.cache.delete(key);
        return null;
    }
    
    _setCache(key, data) {
        this.cache.set(key, {
            data,
            timestamp: Date.now()
        });
    }
    
    _delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
    
    // Vider le cache
    clearCache() {
        this.cache.clear();
    }
    
    // Statistiques
    getCacheStats() {
        return {
            size: this.cache.size,
            keys: Array.from(this.cache.keys())
        };
    }
}

// Gestionnaire de notifications
class NotificationManager {
    constructor() {
        this.permission = null;
        this.checkPermission();
    }
    
    async checkPermission() {
        if (!('Notification' in window)) {
            console.warn('Notifications non supportees');
            return false;
        }
        
        this.permission = Notification.permission;
        
        if (this.permission === 'default') {
            this.permission = await Notification.requestPermission();
        }
        
        return this.permission === 'granted';
    }
    
    async show(title, options = {}) {
        const hasPermission = await this.checkPermission();
        
        if (!hasPermission) {
            console.warn('Permission de notification refusee');
            return null;
        }
        
        const notification = new Notification(title, {
            icon: '/favicon.ico',
            badge: '/badge.png',
            ...options
        });
        
        // Auto-fermeture apres 5 secondes
        setTimeout(() => notification.close(), 5000);
        
        return notification;
    }
}

// Gestionnaire de geolocalisation
class LocationManager {
    constructor() {
        this.currentPosition = null;
        this.watchId = null;
    }
    
    async getCurrentPosition(options = {}) {
        return new Promise((resolve, reject) => {
            if (!navigator.geolocation) {
                reject(new Error('Geolocalisation non supportee'));
                return;
            }
            
            navigator.geolocation.getCurrentPosition(
                (position) => {
                    this.currentPosition = position;
                    resolve(position);
                },
                (error) => {
                    reject(new Error(`Erreur geolocalisation: ${error.message}`));
                },
                {
                    enableHighAccuracy: true,
                    timeout: 10000,
                    maximumAge: 60000,
                    ...options
                }
            );
        });
    }
    
    watchPosition(callback, errorCallback) {
        if (!navigator.geolocation) {
            errorCallback(new Error('Geolocalisation non supportee'));
            return;
        }
        
        this.watchId = navigator.geolocation.watchPosition(
            callback,
            errorCallback,
            {
                enableHighAccuracy: true,
                timeout: 5000,
                maximumAge: 30000
            }
        );
        
        return this.watchId;
    }
    
    stopWatching() {
        if (this.watchId) {
            navigator.geolocation.clearWatch(this.watchId);
            this.watchId = null;
        }
    }
    
    // Calculer la distance entre deux points
    calculateDistance(lat1, lon1, lat2, lon2) {
        const R = 6371; // Rayon de la Terre en km
        const dLat = this._toRad(lat2 - lat1);
        const dLon = this._toRad(lon2 - lon1);
        
        const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) +
                  Math.cos(this._toRad(lat1)) * Math.cos(this._toRad(lat2)) *
                  Math.sin(dLon / 2) * Math.sin(dLon / 2);
        
        const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
        return R * c;
    }
    
    _toRad(deg) {
        return deg * (Math.PI / 180);
    }
}

// Demonstration avancee
async function demonstrationAvancee() {
    const api = new AdvancedAPIManager({
        baseURL: 'https://jsonplaceholder.typicode.com',
        timeout: 5000,
        retryAttempts: 2,
        cacheTimeout: 60000
    });
    
    const notifications = new NotificationManager();
    const location = new LocationManager();
    
    try {
        // Test API avec cache
        console.log('Test API avec cache...');
        const data1 = await api.request('/posts/1');
        console.log('Premier appel:', data1.title);
        
        const data2 = await api.request('/posts/1');
        console.log('Deuxieme appel (cache):', data2.title);
        
        console.log('Stats cache:', api.getCacheStats());
        
        // Test notification
        await notifications.show('Test Notification', {
            body: 'Ceci est une notification de test',
            tag: 'test'
        });
        
        // Test geolocalisation
        try {
            const position = await location.getCurrentPosition();
            console.log('Position actuelle:', {
                lat: position.coords.latitude,
                lon: position.coords.longitude,
                precision: position.coords.accuracy
            });
            
            // Calculer distance avec Paris (exemple)
            const distanceParis = location.calculateDistance(
                position.coords.latitude,
                position.coords.longitude,
                48.8566,
                2.3522
            );
            console.log(`Distance avec Paris: ${distanceParis.toFixed(2)} km`);
            
        } catch (error) {
            console.error('Erreur geolocalisation:', error.message);
        }
        
    } catch (error) {
        console.error('Erreur demonstration:', error);
    }
}

demonstrationAvancee();
```

### Exemple avance (optionnel)
```javascript
// Service Worker pour cache hors ligne
class OfflineManager {
    constructor() {
        this.isOnline = navigator.onLine;
        this.offlineQueue = [];
        this.cacheName = 'app-cache-v1';
        
        this.setupEventListeners();
        this.registerServiceWorker();
    }
    
    setupEventListeners() {
        window.addEventListener('online', () => {
            this.isOnline = true;
            console.log('Connexion retablie');
            this.processOfflineQueue();
        });
        
        window.addEventListener('offline', () => {
            this.isOnline = false;
            console.log('Connexion perdue');
        });
    }
    
    async registerServiceWorker() {
        if ('serviceWorker' in navigator) {
            try {
                const registration = await navigator.serviceWorker.register('/sw.js');
                console.log('Service Worker enregistre:', registration);
                
                // Ecouter les mises a jour
                registration.addEventListener('updatefound', () => {
                    console.log('Nouvelle version disponible');
                });
                
            } catch (error) {
                console.error('Erreur Service Worker:', error);
            }
        }
    }
    
    async makeRequest(url, options = {}) {
        if (this.isOnline) {
            try {
                return await fetch(url, options);
            } catch (error) {
                // Ajouter a la queue hors ligne
                this.addToOfflineQueue(url, options);
                throw error;
            }
        } else {
            // Mode hors ligne
            this.addToOfflineQueue(url, options);
            throw new Error('Mode hors ligne - requete mise en queue');
        }
    }
    
    addToOfflineQueue(url, options) {
        this.offlineQueue.push({
            url,
            options,
            timestamp: Date.now()
        });
        
        // Sauvegarder dans localStorage
        localStorage.setItem('offlineQueue', JSON.stringify(this.offlineQueue));
    }
    
    async processOfflineQueue() {
        const savedQueue = localStorage.getItem('offlineQueue');
        if (savedQueue) {
            this.offlineQueue = JSON.parse(savedQueue);
        }
        
        console.log(`Traitement de ${this.offlineQueue.length} requetes en attente`);
        
        const processed = [];
        for (const request of this.offlineQueue) {
            try {
                await fetch(request.url, request.options);
                processed.push(request);
                console.log('Requete synchronisee:', request.url);
            } catch (error) {
                console.error('Echec synchronisation:', error);
            }
        }
        
        // Retirer les requetes traitees
        this.offlineQueue = this.offlineQueue.filter(req => !processed.includes(req));
        localStorage.setItem('offlineQueue', JSON.stringify(this.offlineQueue));
    }
}

// API de Performance pour monitoring
class PerformanceMonitor {
    constructor() {
        this.metrics = new Map();
        this.observers = new Map();
        
        this.setupObservers();
    }
    
    setupObservers() {
        // Observer les ressources
        if ('PerformanceObserver' in window) {
            const resourceObserver = new PerformanceObserver((list) => {
                for (const entry of list.getEntries()) {
                    this.recordResourceTiming(entry);
                }
            });
            resourceObserver.observe({ entryTypes: ['resource'] });
            this.observers.set('resource', resourceObserver);
            
            // Observer les navigations
            const navigationObserver = new PerformanceObserver((list) => {
                for (const entry of list.getEntries()) {
                    this.recordNavigationTiming(entry);
                }
            });
            navigationObserver.observe({ entryTypes: ['navigation'] });
            this.observers.set('navigation', navigationObserver);
        }
    }
    
    recordResourceTiming(entry) {
        const metrics = {
            name: entry.name,
            duration: entry.duration,
            transferSize: entry.transferSize,
            encodedBodySize: entry.encodedBodySize,
            decodedBodySize: entry.decodedBodySize,
            type: entry.initiatorType
        };
        
        this.metrics.set(`resource_${Date.now()}`, metrics);
    }
    
    recordNavigationTiming(entry) {
        const metrics = {
            domContentLoaded: entry.domContentLoadedEventEnd - entry.domContentLoadedEventStart,
            loadComplete: entry.loadEventEnd - entry.loadEventStart,
            firstPaint: this.getFirstPaint(),
            firstContentfulPaint: this.getFirstContentfulPaint()
        };
        
        this.metrics.set('navigation', metrics);
    }
    
    getFirstPaint() {
        const paint = performance.getEntriesByType('paint');
        const fp = paint.find(entry => entry.name === 'first-paint');
        return fp ? fp.startTime : null;
    }
    
    getFirstContentfulPaint() {
        const paint = performance.getEntriesByType('paint');
        const fcp = paint.find(entry => entry.name === 'first-contentful-paint');
        return fcp ? fcp.startTime : null;
    }
    
    // Mesurer une operation custom
    async measureOperation(name, operation) {
        const start = performance.now();
        try {
            const result = await operation();
            const duration = performance.now() - start;
            
            this.metrics.set(`custom_${name}`, {
                name,
                duration,
                status: 'success',
                timestamp: Date.now()
            });
            
            return result;
        } catch (error) {
            const duration = performance.now() - start;
            
            this.metrics.set(`custom_${name}`, {
                name,
                duration,
                status: 'error',
                error: error.message,
                timestamp: Date.now()
            });
            
            throw error;
        }
    }
    
    // Obtenir un rapport de performance
    getReport() {
        const report = {
            navigation: this.metrics.get('navigation'),
            resources: Array.from(this.metrics.entries())
                .filter(([key]) => key.startsWith('resource_'))
                .map(([, value]) => value),
            customOperations: Array.from(this.metrics.entries())
                .filter(([key]) => key.startsWith('custom_'))
                .map(([, value]) => value)
        };
        
        return report;
    }
    
    // Envoyer les metriques a un service d'analytics
    async sendMetrics(endpoint) {
        const report = this.getReport();
        
        try {
            await fetch(endpoint, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(report)
            });
            console.log('Metriques envoyees');
        } catch (error) {
            console.error('Erreur envoi metriques:', error);
        }
    }
}

// Demonstration complete
async function demonstrationComplete() {
    const offlineManager = new OfflineManager();
    const performanceMonitor = new PerformanceMonitor();
    
    // Test mesure de performance
    await performanceMonitor.measureOperation('api-call', async () => {
        const response = await offlineManager.makeRequest('https://jsonplaceholder.typicode.com/posts/1');
        return response.json();
    });
    
    // Afficher le rapport
    setTimeout(() => {
        const report = performanceMonitor.getReport();
        console.log('Rapport de performance:', report);
    }, 2000);
}

demonstrationComplete();
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Gestionnaire de cache intelligent
**Consigne :** Creez un gestionnaire de cache qui utilise localStorage avec expiration automatique et nettoyage periodique.

**Code de depart :**
```javascript
class SmartCache {
    constructor() {
        // TODO: Initialiser le cache avec nettoyage automatique
    }
    
    set(key, value, ttl = 3600000) {
        // TODO: Stocker avec timestamp d'expiration
    }
    
    get(key) {
        // TODO: Recuperer en verifiant l'expiration
    }
    
    cleanup() {
        // TODO: Nettoyer les entrees expirees
    }
    
    getStats() {
        // TODO: Retourner statistiques du cache
    }
}

// Tests
const cache = new SmartCache();
// Implementer les tests
```

**Niveau de difficulte :** ⭐⭐⭐☆☆

### Exercice 2 : Application meteo avec geolocalisation
**Consigne :** Creez une application qui affiche la meteo basee sur la position de l'utilisateur avec cache et mode hors ligne.

**Contraintes :**
- Utiliser l'API de geolocalisation
- Implementer un cache pour les donnees meteo
- Gerer le mode hors ligne
- Interface utilisateur responsive

**Niveau de difficulte :** ⭐⭐⭐⭐☆

### Exercice 3 : Moniteur de performance temps reel (optionnel)
**Consigne :** Creez un outil de monitoring qui affiche les performances en temps reel avec graphiques.

**Niveau de difficulte :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
class SmartCache {
    constructor(options = {}) {
        this.prefix = options.prefix || 'cache_';
        this.maxSize = options.maxSize || 100;
        this.cleanupInterval = options.cleanupInterval || 300000; // 5 minutes
        
        // Demarrer le nettoyage automatique
        this.startAutoCleanup();
        
        // Nettoyer au demarrage
        this.cleanup();
    }
    
    set(key, value, ttl = 3600000) {
        try {
            const item = {
                value,
                timestamp: Date.now(),
                ttl,
                expires: Date.now() + ttl
            };
            
            const storageKey = this.prefix + key;
            localStorage.setItem(storageKey, JSON.stringify(item));
            
            // Verifier la taille et nettoyer si necessaire
            this.enforceMaxSize();
            
            return true;
        } catch (error) {
            console.error('Erreur cache set:', error);
            return false;
        }
    }
    
    get(key) {
        try {
            const storageKey = this.prefix + key;
            const item = localStorage.getItem(storageKey);
            
            if (!item) {
                return null;
            }
            
            const parsed = JSON.parse(item);
            
            // Verifier l'expiration
            if (Date.now() > parsed.expires) {
                localStorage.removeItem(storageKey);
                return null;
            }
            
            return parsed.value;
        } catch (error) {
            console.error('Erreur cache get:', error);
            return null;
        }
    }
    
    has(key) {
        return this.get(key) !== null;
    }
    
    delete(key) {
        const storageKey = this.prefix + key;
        localStorage.removeItem(storageKey);
    }
    
    cleanup() {
        const now = Date.now();
        let removed = 0;
        
        // Parcourir tous les items du localStorage
        for (let i = localStorage.length - 1; i >= 0; i--) {
            const key = localStorage.key(i);
            
            if (key && key.startsWith(this.prefix)) {
                try {
                    const item = JSON.parse(localStorage.getItem(key));
                    
                    if (now > item.expires) {
                        localStorage.removeItem(key);
                        removed++;
                    }
                } catch (error) {
                    // Item corrompu, le supprimer
                    localStorage.removeItem(key);
                    removed++;
                }
            }
        }
        
        console.log(`Cache cleanup: ${removed} entrees supprimees`);
        return removed;
    }
    
    enforceMaxSize() {
        const items = this.getAllItems()
            .sort((a, b) => a.timestamp - b.timestamp); // Plus ancien en premier
        
        while (items.length > this.maxSize) {
            const oldest = items.shift();
            localStorage.removeItem(oldest.key);
        }
    }
    
    getAllItems() {
        const items = [];
        
        for (let i = 0; i < localStorage.length; i++) {
            const key = localStorage.key(i);
            
            if (key && key.startsWith(this.prefix)) {
                try {
                    const item = JSON.parse(localStorage.getItem(key));
                    items.push({ key, ...item });
                } catch (error) {
                    // Ignorer les items corrompus
                }
            }
        }
        
        return items;
    }
    
    getStats() {
        const items = this.getAllItems();
        const now = Date.now();
        
        const valid = items.filter(item => now <= item.expires);
        const expired = items.filter(item => now > item.expires);
        
        const totalSize = items.reduce((size, item) => {
            return size + JSON.stringify(item).length;
        }, 0);
        
        return {
            total: items.length,
            valid: valid.length,
            expired: expired.length,
            sizeBytes: totalSize,
            sizeKB: Math.round(totalSize / 1024),
            oldestTimestamp: items.length > 0 ? Math.min(...items.map(i => i.timestamp)) : null,
            newestTimestamp: items.length > 0 ? Math.max(...items.map(i => i.timestamp)) : null
        };
    }
    
    clear() {
        const items = this.getAllItems();
        items.forEach(item => localStorage.removeItem(item.key));
        console.log(`Cache vide: ${items.length} entrees supprimees`);
    }
    
    startAutoCleanup() {
        setInterval(() => {
            this.cleanup();
        }, this.cleanupInterval);
    }
    
    // Methodes utilitaires
    keys() {
        return this.getAllItems()
            .filter(item => Date.now() <= item.expires)
            .map(item => item.key.replace(this.prefix, ''));
    }
    
    size() {
        return this.getAllItems()
            .filter(item => Date.now() <= item.expires)
            .length;
    }
}

// Demonstration
function demonstrationSmartCache() {
    const cache = new SmartCache({
        prefix: 'app_',
        maxSize: 5,
        cleanupInterval: 10000
    });
    
    // Tester le cache
    console.log('Test du cache...');
    
    // Ajouter des items
    cache.set('user1', { name: 'Alice', age: 25 }, 5000);
    cache.set('user2', { name: 'Bob', age: 30 }, 10000);
    cache.set('config', { theme: 'dark', lang: 'fr' }, 60000);
    
    // Lire les items
    console.log('User1:', cache.get('user1'));
    console.log('Config:', cache.get('config'));
    
    // Stats initiales
    console.log('Stats initiales:', cache.getStats());
    
    // Tester l'expiration
    setTimeout(() => {
        console.log('Apres 6 secondes...');
        console.log('User1 (expire):', cache.get('user1'));
        console.log('User2 (valide):', cache.get('user2'));
        console.log('Stats apres expiration:', cache.getStats());
    }, 6000);
}

demonstrationSmartCache();
```

**Explication :** Cette solution implemente un cache intelligent avec expiration automatique, nettoyage periodique et gestion de la taille maximale.

### Solution Exercice 2
```javascript
// Application meteo complete
class WeatherApp {
    constructor() {
        this.cache = new SmartCache({ prefix: 'weather_' });
        this.apiKey = 'YOUR_API_KEY'; // Remplacer par une vraie cle
        this.baseURL = 'https://api.openweathermap.org/data/2.5';
        this.isOnline = navigator.onLine;
        
        this.setupUI();
        this.setupEventListeners();
        this.init();
    }
    
    setupUI() {
        document.body.innerHTML = `
            <div id="weather-app">
                <header>
                    <h1>Meteo App</h1>
                    <button id="refresh-btn">Actualiser</button>
                    <span id="status" class="status online">En ligne</span>
                </header>
                
                <main id="weather-content">
                    <div id="loading">Chargement...</div>
                </main>
                
                <footer>
                    <div id="cache-info"></div>
                </footer>
            </div>
            
            <style>
                #weather-app {
                    max-width: 600px;
                    margin: 0 auto;
                    padding: 20px;
                    font-family: Arial, sans-serif;
                }
                
                header {
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    margin-bottom: 20px;
                }
                
                .status {
                    padding: 5px 10px;
                    border-radius: 5px;
                    color: white;
                }
                
                .status.online { background: green; }
                .status.offline { background: red; }
                
                .weather-card {
                    background: #f0f0f0;
                    padding: 20px;
                    border-radius: 10px;
                    margin: 10px 0;
                }
                
                .temperature {
                    font-size: 2em;
                    font-weight: bold;
                    color: #333;
                }
                
                .description {
                    text-transform: capitalize;
                    color: #666;
                }
                
                .error {
                    background: #ffebee;
                    color: #c62828;
                    padding: 15px;
                    border-radius: 5px;
                    margin: 10px 0;
                }
                
                .cache-indicator {
                    background: #e3f2fd;
                    color: #1976d2;
                    padding: 10px;
                    border-radius: 5px;
                    margin: 10px 0;
                    font-size: 0.9em;
                }
            </style>
        `;
    }
    
    setupEventListeners() {
        // Bouton refresh
        document.getElementById('refresh-btn').addEventListener('click', () => {
            this.loadWeather(true); // Force refresh
        });
        
        // Status de connexion
        window.addEventListener('online', () => {
            this.isOnline = true;
            this.updateConnectionStatus();
            this.loadWeather(); // Recharger automatiquement
        });
        
        window.addEventListener('offline', () => {
            this.isOnline = false;
            this.updateConnectionStatus();
        });
    }
    
    updateConnectionStatus() {
        const statusEl = document.getElementById('status');
        if (this.isOnline) {
            statusEl.textContent = 'En ligne';
            statusEl.className = 'status online';
        } else {
            statusEl.textContent = 'Hors ligne';
            statusEl.className = 'status offline';
        }
    }
    
    async init() {
        this.updateConnectionStatus();
        await this.loadWeather();
    }
    
    async loadWeather(forceRefresh = false) {
        try {
            this.showLoading();
            
            // Obtenir la position
            const position = await this.getCurrentPosition();
            const { latitude, longitude } = position.coords;
            
            // Cle de cache basee sur la position
            const cacheKey = `weather_${latitude.toFixed(2)}_${longitude.toFixed(2)}`;
            
            let weatherData = null;
            let fromCache = false;
            
            // Verifier le cache si pas de refresh force
            if (!forceRefresh) {
                weatherData = this.cache.get(cacheKey);
                if (weatherData) {
                    fromCache = true;
                }
            }
            
            // Charger depuis l'API si necessaire
            if (!weatherData && this.isOnline) {
                weatherData = await this.fetchWeatherData(latitude, longitude);
                
                // Mettre en cache pour 10 minutes
                this.cache.set(cacheKey, weatherData, 600000);
            }
            
            if (weatherData) {
                this.displayWeather(weatherData, fromCache);
            } else {
                this.showError('Donnees meteo indisponibles (mode hors ligne et pas de cache)');
            }
            
        } catch (error) {
            this.showError(error.message);
        }
    }
    
    async getCurrentPosition() {
        return new Promise((resolve, reject) => {
            if (!navigator.geolocation) {
                reject(new Error('Geolocalisation non supportee'));
                return;
            }
            
            navigator.geolocation.getCurrentPosition(
                resolve,
                (error) => reject(new Error(`Erreur geolocalisation: ${error.message}`)),
                {
                    enableHighAccuracy: true,
                    timeout: 10000,
                    maximumAge: 300000 // 5 minutes
                }
            );
        });
    }
    
    async fetchWeatherData(lat, lon) {
        const url = `${this.baseURL}/weather?lat=${lat}&lon=${lon}&appid=${this.apiKey}&units=metric&lang=fr`;
        
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`Erreur API meteo: ${response.status}`);
        }
        
        return await response.json();
    }
    
    displayWeather(data, fromCache = false) {
        const content = document.getElementById('weather-content');
        
        content.innerHTML = `
            ${fromCache ? '<div class="cache-indicator">Donnees du cache local</div>' : ''}
            
            <div class="weather-card">
                <h2>${data.name}, ${data.sys.country}</h2>
                <div class="temperature">${Math.round(data.main.temp)}°C</div>
                <div class="description">${data.weather[0].description}</div>
                
                <div style="margin-top: 15px;">
                    <div>Ressenti: ${Math.round(data.main.feels_like)}°C</div>
                    <div>Humidite: ${data.main.humidity}%</div>
                    <div>Vent: ${data.wind.speed} m/s</div>
                    <div>Pression: ${data.main.pressure} hPa</div>
                </div>
                
                <div style="margin-top: 10px; font-size: 0.9em; color: #666;">
                    Derniere mise a jour: ${new Date().toLocaleTimeString()}
                </div>
            </div>
        `;
        
        this.updateCacheInfo();
    }
    
    showLoading() {
        const content = document.getElementById('weather-content');
        content.innerHTML = '<div id="loading">Chargement des donnees meteo...</div>';
    }
    
    showError(message) {
        const content = document.getElementById('weather-content');
        content.innerHTML = `
            <div class="error">
                Erreur: ${message}
            </div>
        `;
        this.updateCacheInfo();
    }
    
    updateCacheInfo() {
        const stats = this.cache.getStats();
        const cacheInfo = document.getElementById('cache-info');
        
        cacheInfo.innerHTML = `
            Cache: ${stats.valid} entrees valides, ${stats.sizeKB} KB utilises
        `;
    }
}

// Lancer l'application
document.addEventListener('DOMContentLoaded', () => {
    new WeatherApp();
});
```

**Alternatives possibles :**
- Ajouter des previsions sur plusieurs jours
- Implementer un systeme de favoris de villes
- Ajouter des graphiques de tendance

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts prealables utilises
- **Programmation asynchrone** : Fetch API, geolocalisation
- **Gestion d'erreurs** : try/catch avec APIs asynchrones
- **Objets** : Manipulation des reponses API

### Prochaines etapes
- **Service Workers** : Cache avance et notifications push
- **WebAssembly** : Performance native dans le navigateur
- **Progressive Web Apps** : Applications web natives

### Concepts complementaires
- **Securite** : CORS, CSP, HTTPS pour les APIs
- **Performance** : Lazy loading, compression, CDN

---

## **PROJET MINI**

### Defi pratique
**Objectif :** Creer une application de suivi de fitness avec geolocalisation, notifications et stockage local

**Fonctionnalites requises :**
- [ ] Tracking de course avec geolocalisation en temps reel
- [ ] Stockage des sessions d'entrainement en local
- [ ] Notifications de performance et objectifs
- [ ] Interface de statistiques avec graphiques
- [ ] Mode hors ligne avec synchronisation

**Structure de fichiers suggeree :**
```
📁 fitness-tracker/
├── 📄 index.html
├── 📄 style.css
├── 📄 app.js
├── 📄 fitness-tracker.js
├── 📄 storage-manager.js
└── 📄 sw.js (Service Worker)
```

**Temps estime :** 180 minutes

---

## **RESSOURCES COMPLEMENTAIRES**

### Documentation officielle
- [MDN - Web APIs](https://developer.mozilla.org/en-US/docs/Web/API)
- [MDN - Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN - Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- [MDN - Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)

### Videos recommandees
- "Fetch API Crash Course" - Traversy Media (30 min)
- "JavaScript Web APIs" - The Net Ninja (serie)
- Pourquoi ces videos sont utiles : Demonstrations pratiques et cas d'usage

### Articles approfondis
- "Modern Browser APIs" - Web.dev
- "Offline First Strategies" - Google Developers
- Ce que ces articles apportent en plus : Strategies avancees et optimisations

### Outils de pratique
- [Can I Use](https://caniuse.com/) - Compatibilite des APIs
- [Web API Examples](https://github.com/mdn/dom-examples)

---

## **AUTO-EVALUATION**

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre localStorage et sessionStorage ?
   - Reponse : localStorage persiste entre les sessions, sessionStorage est supprime a la fermeture de l'onglet

2. **Question pratique :** Pourquoi fetch() ne rejette pas automatiquement les erreurs HTTP 4xx/5xx ?
   - Reponse : fetch() ne rejette que les erreurs reseau, il faut verifier response.ok pour les erreurs HTTP

3. **Question d'application :** Quand utiliseriez-vous l'API Notification vs un simple alert() ?
   - Reponse : Notifications pour les evenements asynchrones, alert() pour les interactions immediates

### Checklist de maitrise
- [ ] Je sais utiliser Fetch API pour les requetes HTTP
- [ ] Je peux gerer le stockage local avec Web Storage
- [ ] Je comprends les APIs de geolocalisation et notifications
- [ ] Je peux gerer le mode hors ligne efficacement
- [ ] Je sais optimiser les performances avec le cache
- [ ] Je peux integrer plusieurs APIs dans une application

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur frequente 1
**Code problematique :**
```javascript
localStorage.setItem('user', user); // Object directement
```

**Probleme :** Les objets ne sont pas automatiquement serialises

**Solution :**
```javascript
localStorage.setItem('user', JSON.stringify(user));
const user = JSON.parse(localStorage.getItem('user'));
```

**Explication :** Web Storage ne stocke que des strings, il faut serialiser les objets.

### Erreur frequente 2
**Code problematique :**
```javascript
const response = await fetch('/api/data');
const data = response.json(); // Oubli d'await
```

**Probleme :** response.json() retourne une Promise

**Solution :**
```javascript
const response = await fetch('/api/data');
const data = await response.json(); // Avec await
```

**Explication :** Les methodes de reponse fetch sont asynchrones et requirent await.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- L'utilisation moderne des APIs du navigateur
- La gestion du mode hors ligne et du cache
- L'integration de plusieurs APIs dans une application

### Questions a approfondir
- Service Workers pour le cache avance
- WebRTC pour la communication peer-to-peer

### Revision programmee
- [ ] Revision dans 1 jour (rappel rapide)
- [ ] Revision dans 1 semaine (exercices)
- [ ] Revision dans 1 mois (application dans un projet)

---

## **TAGS ET METADONNEES**

**Mots-cles :** #javascript #apis-navigateur #fetch #storage #geolocation #notifications #hors-ligne #avance

**Temps de completion :** [Temps reellement passe]

**Difficulte ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Pret pour le concept suivant :** ✅ Oui / ❌ Non (revision necessaire)

---

*📅 Fiche creee le : 17/07/2025*
*🔄 Derniere revision : 17/07/2025*
*📍 Position dans la roadmap : 15/126*
