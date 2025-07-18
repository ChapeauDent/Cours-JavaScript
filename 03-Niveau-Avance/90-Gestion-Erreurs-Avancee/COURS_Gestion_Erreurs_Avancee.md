# Gestion d'Erreurs Avancée en JavaScript

## **INFORMATIONS GENERALES**

### Métadonnées du cours
- **Titre :** Gestion d'erreurs avancée - Try/Catch, Custom Errors, Error Boundaries
- **Niveau :** Avancé
- **Durée estimée :** 75 minutes
- **Prérequis :** Programmation asynchrone, Promises, async/await, objets, classes
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Les types d'erreurs et leur propagation en JavaScript
- [ ] **Appliquer** : Les patterns avancés de gestion d'erreurs avec async/await
- [ ] **Analyser** : Les erreurs en production et implémenter du monitoring
- [ ] **Créer** : Des systèmes robustes avec gestion d'erreurs complète

### Mots-clés à retenir
- **Error Object** : Objet natif JavaScript pour représenter les erreurs
- **Custom Error** : Classe d'erreur personnalisée héritant d'Error
- **Error Boundary** : Mécanisme de capture d'erreurs globales
- **Graceful Degradation** : Dégradation élégante en cas d'erreur
- **Error Monitoring** : Surveillance et tracking des erreurs en production
- **Retry Pattern** : Pattern de nouvelle tentative automatique

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
La gestion d'erreurs avancée est cruciale pour créer des applications robustes en production. Une erreur non gérée peut crasher votre application, perdre des données utilisateur, ou créer une expérience frustrante. Maîtriser ces techniques vous permet de créer des applications fiables qui gèrent élégamment tous les cas d'erreur.

### Définition et concepts clés

#### **1. Hiérarchie des erreurs JavaScript**
```javascript
// Erreurs natives JavaScript
Error              // Erreur générique
├── SyntaxError    // Erreur de syntaxe
├── ReferenceError // Variable non définie
├── TypeError      // Type incorrect
├── RangeError     // Valeur hors limite
├── URIError       // URI malformée
└── EvalError      // Erreur dans eval()

// Erreurs personnalisées
class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
    }
}
```

#### **2. Patterns de gestion d'erreurs modernes**
```javascript
// Pattern Result/Option
class Result {
    constructor(value, error) {
        this.value = value;
        this.error = error;
    }
    
    static ok(value) {
        return new Result(value, null);
    }
    
    static error(error) {
        return new Result(null, error);
    }
    
    isOk() {
        return this.error === null;
    }
    
    isError() {
        return this.error !== null;
    }
}

// Pattern Either
class Either {
    static left(value) {
        return { isLeft: true, value };
    }
    
    static right(value) {
        return { isLeft: false, value };
    }
}
```

#### **3. Gestion d'erreurs asynchrones**
```javascript
// Avec Promises
fetch('/api/data')
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        return response.json();
    })
    .catch(error => {
        if (error instanceof TypeError) {
            console.error('Erreur réseau:', error);
        } else {
            console.error('Erreur API:', error);
        }
    });

// Avec async/await
async function fetchDataSafely() {
    try {
        const response = await fetch('/api/data');
        if (!response.ok) {
            throw new NetworkError(`HTTP ${response.status}`, response.status);
        }
        return await response.json();
    } catch (error) {
        if (error instanceof NetworkError) {
            throw new UserFriendlyError('Problème de connexion. Réessayez plus tard.');
        }
        throw error; // Re-throw unknown errors
    }
}
```

### Syntaxe de base
```javascript
// Try/catch basique
try {
    riskyOperation();
} catch (error) {
    console.error(error.message);
} finally {
    cleanup();
}

// Custom Error
class CustomError extends Error {
    constructor(message, code) {
        super(message);
        this.name = 'CustomError';
        this.code = code;
    }
}

// Async error handling
async function safeAsyncOperation() {
    try {
        const result = await asyncOperation();
        return result;
    } catch (error) {
        logger.error('Operation failed:', error);
        throw new OperationError('Failed to complete operation');
    }
}
```

### Points d'attention
- **Piège courant 1** : Capturer toutes les erreurs sans les re-throw appropriées
- **Piège courant 2** : Oublier de nettoyer les ressources dans finally
- **Bonne pratique** : Créer des erreurs spécifiques avec des messages informatifs

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Système de validation avec erreurs personnalisées
console.log("=== SYSTÈME DE VALIDATION ROBUSTE ===");

// Définition des erreurs personnalisées
class ValidationError extends Error {
    constructor(message, field, value) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
        this.value = value;
        this.timestamp = new Date().toISOString();
    }
}

class NetworkError extends Error {
    constructor(message, status, url) {
        super(message);
        this.name = 'NetworkError';
        this.status = status;
        this.url = url;
        this.timestamp = new Date().toISOString();
    }
}

// Classe de validation
class UserValidator {
    static validateEmail(email) {
        if (!email) {
            throw new ValidationError('Email requis', 'email', email);
        }
        
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(email)) {
            throw new ValidationError('Format email invalide', 'email', email);
        }
        
        return true;
    }
    
    static validateAge(age) {
        if (age === undefined || age === null) {
            throw new ValidationError('Âge requis', 'age', age);
        }
        
        const numAge = Number(age);
        if (isNaN(numAge)) {
            throw new ValidationError('Âge doit être un nombre', 'age', age);
        }
        
        if (numAge < 0 || numAge > 150) {
            throw new ValidationError('Âge doit être entre 0 et 150', 'age', age);
        }
        
        return true;
    }
    
    static validatePassword(password) {
        if (!password) {
            throw new ValidationError('Mot de passe requis', 'password', '[hidden]');
        }
        
        if (password.length < 8) {
            throw new ValidationError('Mot de passe trop court (min 8 caractères)', 'password', '[hidden]');
        }
        
        if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(password)) {
            throw new ValidationError(
                'Mot de passe doit contenir minuscule, majuscule et chiffre',
                'password',
                '[hidden]'
            );
        }
        
        return true;
    }
    
    static validateUser(userData) {
        const errors = [];
        
        // Validation de chaque champ
        ['email', 'age', 'password'].forEach(field => {
            try {
                switch (field) {
                    case 'email':
                        this.validateEmail(userData.email);
                        break;
                    case 'age':
                        this.validateAge(userData.age);
                        break;
                    case 'password':
                        this.validatePassword(userData.password);
                        break;
                }
            } catch (error) {
                if (error instanceof ValidationError) {
                    errors.push(error);
                } else {
                    // Erreur inattendue
                    errors.push(new ValidationError(
                        `Erreur de validation inattendue pour ${field}`,
                        field,
                        userData[field]
                    ));
                }
            }
        });
        
        if (errors.length > 0) {
            const aggregateError = new Error(`Validation échouée: ${errors.length} erreur(s)`);
            aggregateError.name = 'AggregateValidationError';
            aggregateError.errors = errors;
            throw aggregateError;
        }
        
        return true;
    }
}

// Tests de validation
function testerValidation() {
    const testsData = [
        // Cas valide
        { email: 'user@example.com', age: 25, password: 'MonMotDePasse123' },
        // Cas invalides
        { email: 'invalid-email', age: 25, password: 'MonMotDePasse123' },
        { email: 'user@example.com', age: -5, password: 'MonMotDePasse123' },
        { email: 'user@example.com', age: 25, password: 'trop_court' },
        { email: '', age: 'abc', password: '' }
    ];
    
    testsData.forEach((userData, index) => {
        console.log(`\n--- Test ${index + 1} ---`);
        console.log('Données:', { 
            email: userData.email, 
            age: userData.age, 
            password: userData.password ? '[hidden]' : userData.password 
        });
        
        try {
            UserValidator.validateUser(userData);
            console.log('✅ Validation réussie');
        } catch (error) {
            if (error.name === 'AggregateValidationError') {
                console.log(`❌ Validation échouée: ${error.errors.length} erreur(s)`);
                error.errors.forEach((validationError, i) => {
                    console.log(`   ${i + 1}. ${validationError.field}: ${validationError.message}`);
                });
            } else {
                console.error('❌ Erreur inattendue:', error.message);
            }
        }
    });
}

testerValidation();

console.log("\n=== GESTION D'ERREURS AVEC RETRY ===");

// Pattern Retry avec backoff exponentiel
class RetryManager {
    static async withRetry(fn, maxRetries = 3, baseDelay = 1000) {
        let lastError;
        
        for (let attempt = 1; attempt <= maxRetries; attempt++) {
            try {
                console.log(`Tentative ${attempt}/${maxRetries}`);
                const result = await fn();
                console.log('✅ Opération réussie');
                return result;
            } catch (error) {
                lastError = error;
                console.log(`❌ Tentative ${attempt} échouée:`, error.message);
                
                if (attempt === maxRetries) {
                    console.log('🚫 Toutes les tentatives épuisées');
                    break;
                }
                
                // Backoff exponentiel: 1s, 2s, 4s, 8s...
                const delay = baseDelay * Math.pow(2, attempt - 1);
                console.log(`⏱️ Attente de ${delay}ms avant nouvelle tentative`);
                await new Promise(resolve => setTimeout(resolve, delay));
            }
        }
        
        throw new Error(`Opération échouée après ${maxRetries} tentatives: ${lastError.message}`);
    }
}

// Simulation d'une API instable
let apiCallCount = 0;
async function unreliableApiCall() {
    apiCallCount++;
    
    // Simule un taux d'échec de 70%
    if (Math.random() < 0.7) {
        throw new NetworkError('Timeout de connexion', 408, '/api/data');
    }
    
    return { data: 'Données récupérées avec succès', attempt: apiCallCount };
}

// Test du retry pattern
RetryManager.withRetry(unreliableApiCall, 5, 500)
    .then(result => {
        console.log('🎉 Résultat final:', result);
    })
    .catch(error => {
        console.error('💥 Échec définitif:', error.message);
    });
```

**Résultat attendu :**
```
=== SYSTÈME DE VALIDATION ROBUSTE ===

--- Test 1 ---
Données: { email: 'user@example.com', age: 25, password: '[hidden]' }
✅ Validation réussie

--- Test 2 ---
Données: { email: 'invalid-email', age: 25, password: '[hidden]' }
❌ Validation échouée: 1 erreur(s)
   1. email: Format email invalide

=== GESTION D'ERREURS AVEC RETRY ===
Tentative 1/5
❌ Tentative 1 échouée: Timeout de connexion
⏱️ Attente de 500ms avant nouvelle tentative
Tentative 2/5
✅ Opération réussie
🎉 Résultat final: { data: 'Données récupérées avec succès', attempt: 2 }
```

### Exemple intermédiaire
```javascript
// Système de monitoring d'erreurs complet
console.log("=== SYSTÈME DE MONITORING D'ERREURS ===");

class ErrorLogger {
    constructor() {
        this.logs = [];
        this.errorCounts = new Map();
        this.criticalErrors = [];
    }
    
    log(error, context = {}) {
        const logEntry = {
            timestamp: new Date().toISOString(),
            error: {
                name: error.name,
                message: error.message,
                stack: error.stack
            },
            context,
            severity: this.getSeverity(error),
            id: this.generateId()
        };
        
        this.logs.push(logEntry);
        this.updateErrorCounts(error.name);
        
        if (logEntry.severity === 'critical') {
            this.criticalErrors.push(logEntry);
            this.notifyCriticalError(logEntry);
        }
        
        return logEntry.id;
    }
    
    getSeverity(error) {
        if (error instanceof ValidationError) return 'low';
        if (error instanceof NetworkError) {
            return error.status >= 500 ? 'high' : 'medium';
        }
        if (error.name === 'TypeError' || error.name === 'ReferenceError') {
            return 'critical';
        }
        return 'medium';
    }
    
    generateId() {
        return Date.now().toString(36) + Math.random().toString(36).substr(2);
    }
    
    updateErrorCounts(errorType) {
        const current = this.errorCounts.get(errorType) || 0;
        this.errorCounts.set(errorType, current + 1);
    }
    
    notifyCriticalError(logEntry) {
        console.warn('🚨 ERREUR CRITIQUE DÉTECTÉE:', {
            id: logEntry.id,
            message: logEntry.error.message,
            timestamp: logEntry.timestamp
        });
    }
    
    getStats() {
        return {
            totalErrors: this.logs.length,
            criticalErrors: this.criticalErrors.length,
            errorsByType: Object.fromEntries(this.errorCounts),
            recentErrors: this.logs.slice(-5)
        };
    }
    
    exportLogs(format = 'json') {
        switch (format) {
            case 'json':
                return JSON.stringify(this.logs, null, 2);
            case 'csv':
                const headers = 'timestamp,error_type,message,severity\n';
                const rows = this.logs.map(log => 
                    `${log.timestamp},${log.error.name},"${log.error.message}",${log.severity}`
                ).join('\n');
                return headers + rows;
            default:
                return this.logs;
        }
    }
}

// Circuit Breaker Pattern
class CircuitBreaker {
    constructor(threshold = 5, timeout = 60000) {
        this.failureThreshold = threshold;
        this.timeout = timeout;
        this.failureCount = 0;
        this.lastFailureTime = null;
        this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    }
    
    async call(fn) {
        if (this.state === 'OPEN') {
            if (Date.now() - this.lastFailureTime > this.timeout) {
                this.state = 'HALF_OPEN';
                console.log('🔄 Circuit Breaker: Passage en HALF_OPEN');
            } else {
                throw new Error('Circuit Breaker OPEN - Service indisponible');
            }
        }
        
        try {
            const result = await fn();
            this.onSuccess();
            return result;
        } catch (error) {
            this.onFailure();
            throw error;
        }
    }
    
    onSuccess() {
        this.failureCount = 0;
        if (this.state === 'HALF_OPEN') {
            this.state = 'CLOSED';
            console.log('✅ Circuit Breaker: Retour à CLOSED');
        }
    }
    
    onFailure() {
        this.failureCount++;
        this.lastFailureTime = Date.now();
        
        if (this.failureCount >= this.failureThreshold) {
            this.state = 'OPEN';
            console.log('🔴 Circuit Breaker: Passage en OPEN');
        }
    }
    
    getState() {
        return {
            state: this.state,
            failureCount: this.failureCount,
            lastFailureTime: this.lastFailureTime
        };
    }
}

// Service avec gestion d'erreurs complète
class UserService {
    constructor() {
        this.logger = new ErrorLogger();
        this.circuitBreaker = new CircuitBreaker(3, 30000);
        this.cache = new Map();
    }
    
    async getUser(id) {
        try {
            // Vérifier le cache d'abord
            if (this.cache.has(id)) {
                console.log(`📦 Cache hit pour l'utilisateur ${id}`);
                return this.cache.get(id);
            }
            
            // Utiliser le circuit breaker
            const user = await this.circuitBreaker.call(async () => {
                return await this.fetchUserFromAPI(id);
            });
            
            // Mettre en cache
            this.cache.set(id, user);
            return user;
            
        } catch (error) {
            const logId = this.logger.log(error, { userId: id, operation: 'getUser' });
            
            // Stratégie de fallback
            const fallbackUser = await this.getFallbackUser(id);
            if (fallbackUser) {
                console.log(`🔄 Utilisation du fallback pour l'utilisateur ${id}`);
                return fallbackUser;
            }
            
            // Si aucun fallback, transformer en erreur user-friendly
            throw new Error(`Impossible de récupérer l'utilisateur ${id}. Veuillez réessayer plus tard. (Ref: ${logId})`);
        }
    }
    
    async fetchUserFromAPI(id) {
        // Simulation d'une API avec taux d'échec
        console.log(`🌐 Appel API pour l'utilisateur ${id}`);
        
        if (Math.random() < 0.6) { // 60% d'échec
            throw new NetworkError('Service temporairement indisponible', 503, `/api/users/${id}`);
        }
        
        return {
            id,
            name: `Utilisateur ${id}`,
            email: `user${id}@example.com`,
            lastUpdated: new Date().toISOString()
        };
    }
    
    async getFallbackUser(id) {
        // Simulation de données en cache local ou données par défaut
        console.log(`💾 Recherche de données de fallback pour ${id}`);
        
        // Simuler des données de fallback partielles
        if (id <= 3) {
            return {
                id,
                name: `Utilisateur ${id} (données de fallback)`,
                email: 'non-disponible',
                lastUpdated: 'cache-local'
            };
        }
        
        return null;
    }
    
    getServiceStats() {
        return {
            errorStats: this.logger.getStats(),
            circuitBreakerState: this.circuitBreaker.getState(),
            cacheSize: this.cache.size
        };
    }
}

// Test du service complet
async function testerServiceComplet() {
    const userService = new UserService();
    const userIds = [1, 2, 3, 4, 5, 6, 7, 8];
    
    console.log('🚀 Test du service avec gestion d\'erreurs complète\n');
    
    for (const id of userIds) {
        try {
            console.log(`--- Récupération utilisateur ${id} ---`);
            const user = await userService.getUser(id);
            console.log('✅ Utilisateur récupéré:', user.name);
        } catch (error) {
            console.error(`❌ Erreur pour utilisateur ${id}:`, error.message);
        }
        
        // Petit délai entre les requêtes
        await new Promise(resolve => setTimeout(resolve, 200));
    }
    
    // Afficher les statistiques finales
    console.log('\n=== STATISTIQUES DU SERVICE ===');
    const stats = userService.getServiceStats();
    console.log('Erreurs par type:', stats.errorStats.errorsByType);
    console.log('Circuit Breaker:', stats.circuitBreakerState.state);
    console.log('Erreurs critiques:', stats.errorStats.criticalErrors);
    console.log('Taille du cache:', stats.cacheSize);
}

testerServiceComplet();
```

### Exemple avancé (optionnel)
```javascript
// Système de Error Boundary pour applications complexes
console.log("=== ERROR BOUNDARY SYSTÈME ===");

class ErrorBoundary {
    constructor() {
        this.errorHandlers = new Map();
        this.globalErrorHandler = null;
        this.isInitialized = false;
        this.errorQueue = [];
        this.maxQueueSize = 100;
        
        this.init();
    }
    
    init() {
        if (this.isInitialized) return;
        
        // Capture des erreurs globales
        window.addEventListener('error', (event) => {
            this.handleError(event.error, {
                type: 'javascript',
                filename: event.filename,
                lineno: event.lineno,
                colno: event.colno
            });
        });
        
        // Capture des rejections de Promise non gérées
        window.addEventListener('unhandledrejection', (event) => {
            this.handleError(event.reason, {
                type: 'unhandled-promise-rejection',
                promise: event.promise
            });
        });
        
        this.isInitialized = true;
        console.log('🛡️ Error Boundary initialisé');
    }
    
    // Enregistrer un handler pour un type d'erreur spécifique
    registerHandler(errorType, handler) {
        if (!this.errorHandlers.has(errorType)) {
            this.errorHandlers.set(errorType, []);
        }
        this.errorHandlers.get(errorType).push(handler);
    }
    
    // Handler global pour toutes les erreurs
    setGlobalHandler(handler) {
        this.globalErrorHandler = handler;
    }
    
    // Gestion centralisée des erreurs
    handleError(error, context = {}) {
        const errorInfo = {
            error,
            context,
            timestamp: new Date().toISOString(),
            url: window.location.href,
            userAgent: navigator.userAgent,
            id: this.generateErrorId()
        };
        
        // Ajouter à la queue avec limitation de taille
        this.addToQueue(errorInfo);
        
        // Appeler les handlers spécifiques
        const errorType = error.name || 'UnknownError';
        const handlers = this.errorHandlers.get(errorType) || [];
        
        handlers.forEach(handler => {
            try {
                handler(errorInfo);
            } catch (handlerError) {
                console.error('Erreur dans error handler:', handlerError);
            }
        });
        
        // Appeler le handler global
        if (this.globalErrorHandler) {
            try {
                this.globalErrorHandler(errorInfo);
            } catch (handlerError) {
                console.error('Erreur dans global error handler:', handlerError);
            }
        }
        
        // Log par défaut si aucun handler
        if (handlers.length === 0 && !this.globalErrorHandler) {
            this.defaultErrorHandler(errorInfo);
        }
    }
    
    addToQueue(errorInfo) {
        if (this.errorQueue.length >= this.maxQueueSize) {
            this.errorQueue.shift(); // Supprimer le plus ancien
        }
        this.errorQueue.push(errorInfo);
    }
    
    generateErrorId() {
        return `err_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
    }
    
    defaultErrorHandler(errorInfo) {
        console.group(`🚨 Erreur non gérée [${errorInfo.id}]`);
        console.error('Type:', errorInfo.error.name);
        console.error('Message:', errorInfo.error.message);
        console.error('Context:', errorInfo.context);
        console.error('Stack:', errorInfo.error.stack);
        console.groupEnd();
    }
    
    // Méthodes pour wrapper des fonctions avec gestion d'erreurs
    wrapFunction(fn, errorHandler) {
        return (...args) => {
            try {
                const result = fn.apply(this, args);
                
                // Si c'est une Promise, capturer les rejections
                if (result && typeof result.then === 'function') {
                    return result.catch(error => {
                        this.handleError(error, { 
                            type: 'wrapped-async-function',
                            functionName: fn.name || 'anonymous'
                        });
                        if (errorHandler) {
                            return errorHandler(error);
                        }
                        throw error;
                    });
                }
                
                return result;
            } catch (error) {
                this.handleError(error, {
                    type: 'wrapped-function',
                    functionName: fn.name || 'anonymous'
                });
                if (errorHandler) {
                    return errorHandler(error);
                }
                throw error;
            }
        };
    }
    
    // Wrapper pour les classes
    wrapClass(ClassConstructor) {
        return class extends ClassConstructor {
            constructor(...args) {
                super(...args);
                
                // Wrapper toutes les méthodes de la classe
                Object.getOwnPropertyNames(Object.getPrototypeOf(this))
                    .filter(name => name !== 'constructor' && typeof this[name] === 'function')
                    .forEach(methodName => {
                        const originalMethod = this[methodName];
                        this[methodName] = errorBoundary.wrapFunction(originalMethod.bind(this));
                    });
            }
        };
    }
    
    // Obtenir les statistiques d'erreurs
    getErrorStats() {
        const errorsByType = {};
        const recentErrors = this.errorQueue.slice(-10);
        
        this.errorQueue.forEach(errorInfo => {
            const type = errorInfo.error.name || 'Unknown';
            errorsByType[type] = (errorsByType[type] || 0) + 1;
        });
        
        return {
            totalErrors: this.errorQueue.length,
            errorsByType,
            recentErrors: recentErrors.map(info => ({
                id: info.id,
                type: info.error.name,
                message: info.error.message,
                timestamp: info.timestamp
            }))
        };
    }
    
    // Exporter les erreurs pour analyse
    exportErrors(format = 'json') {
        const data = this.errorQueue.map(info => ({
            id: info.id,
            timestamp: info.timestamp,
            type: info.error.name,
            message: info.error.message,
            stack: info.error.stack,
            context: info.context,
            url: info.url,
            userAgent: info.userAgent
        }));
        
        if (format === 'csv') {
            const headers = 'id,timestamp,type,message,url\n';
            const rows = data.map(item => 
                `${item.id},${item.timestamp},${item.type},"${item.message}","${item.url}"`
            ).join('\n');
            return headers + rows;
        }
        
        return JSON.stringify(data, null, 2);
    }
}

// Initialiser l'Error Boundary global
const errorBoundary = new ErrorBoundary();

// Handlers spécifiques par type d'erreur
errorBoundary.registerHandler('ValidationError', (errorInfo) => {
    console.log('📝 Erreur de validation capturée:', errorInfo.error.message);
    // Envoyer à un service de monitoring spécialisé pour les validations
});

errorBoundary.registerHandler('NetworkError', (errorInfo) => {
    console.log('🌐 Erreur réseau capturée:', errorInfo.error.message);
    // Envoyer à un service de monitoring réseau
});

errorBoundary.registerHandler('TypeError', (errorInfo) => {
    console.log('🔧 Erreur de type capturée - potentiel bug:', errorInfo.error.message);
    // Envoyer immédiatement aux développeurs
});

// Handler global pour toutes les autres erreurs
errorBoundary.setGlobalHandler((errorInfo) => {
    console.log('🌍 Handler global:', errorInfo.error.name, '-', errorInfo.error.message);
    
    // Simuler l'envoi à un service de monitoring externe
    if (Math.random() < 0.1) { // 10% du temps
        console.log('📡 Envoi vers service de monitoring externe...');
    }
});

// Test du système Error Boundary
function testerErrorBoundary() {
    console.log('🧪 Test du système Error Boundary\n');
    
    // Déclencher différents types d'erreurs
    setTimeout(() => {
        console.log('--- Erreur de validation ---');
        const validationError = new ValidationError('Email invalide', 'email', 'invalid-email');
        errorBoundary.handleError(validationError, { component: 'UserForm' });
    }, 100);
    
    setTimeout(() => {
        console.log('--- Erreur réseau ---');
        const networkError = new NetworkError('Timeout', 408, '/api/users');
        errorBoundary.handleError(networkError, { component: 'UserService' });
    }, 200);
    
    setTimeout(() => {
        console.log('--- Erreur de type (simulation) ---');
        try {
            const obj = null;
            obj.someMethod(); // TypeError
        } catch (error) {
            // Cette erreur sera automatiquement capturée par les listeners globaux
        }
    }, 300);
    
    setTimeout(() => {
        console.log('--- Promise rejection non gérée ---');
        Promise.reject(new Error('Promise rejetée non gérée'));
    }, 400);
    
    // Afficher les stats après un délai
    setTimeout(() => {
        console.log('\n=== STATISTIQUES ERROR BOUNDARY ===');
        const stats = errorBoundary.getErrorStats();
        console.log('Total erreurs:', stats.totalErrors);
        console.log('Erreurs par type:', stats.errorsByType);
        console.log('Erreurs récentes:', stats.recentErrors.length);
    }, 1000);
}

// Exemple de classe wrappée avec gestion d'erreurs
@errorBoundary.wrapClass
class UserManager {
    constructor() {
        this.users = new Map();
    }
    
    addUser(userData) {
        // Cette méthode sera automatiquement wrappée
        if (!userData.email) {
            throw new ValidationError('Email requis', 'email');
        }
        
        this.users.set(userData.id, userData);
        return userData;
    }
    
    async fetchUser(id) {
        // Cette méthode async sera aussi wrappée
        if (Math.random() < 0.5) {
            throw new NetworkError('Service indisponible', 503, `/api/users/${id}`);
        }
        
        return { id, name: `User ${id}` };
    }
}

testerErrorBoundary();

// Test de la classe wrappée
const userManager = new UserManager();

setTimeout(async () => {
    console.log('\n--- Test classe wrappée ---');
    
    try {
        userManager.addUser({ id: 1 }); // ValidationError
    } catch (error) {
        console.log('Erreur capturée dans addUser');
    }
    
    try {
        await userManager.fetchUser(1); // Potential NetworkError
        console.log('fetchUser réussi');
    } catch (error) {
        console.log('Erreur capturée dans fetchUser');
    }
}, 1500);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Système de retry intelligent
**Consigne :** Créez un système de retry qui s'adapte au type d'erreur.

**Code de départ :**
```javascript
// TODO: Implémenter un RetryStrategy qui :
// - Retry immédiatement pour les erreurs réseau temporaires
// - Retry avec backoff exponentiel pour les erreurs de surcharge
// - N'essaie pas de retry pour les erreurs de validation
// - Limite le nombre total de retries sur une période donnée

class SmartRetryManager {
    constructor() {
        // Votre implémentation
    }
    
    async executeWithRetry(operation, context = {}) {
        // Votre logique de retry intelligent
    }
}
```

**Résultat attendu :**
- Différentes stratégies selon le type d'erreur
- Limitation intelligente des retries
- Métriques de performance

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 2 : Error Budget et SLA monitoring
**Consigne :** Implémentez un système de budget d'erreur pour surveiller la qualité du service.

**Contraintes :**
- Calculer le taux d'erreur par fenêtre de temps
- Alerter quand le budget d'erreur est dépassé
- Fournir des métriques SLA

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

### Exercice 3 : Error correlation et root cause analysis
**Consigne :** Créez un système qui corrèle les erreurs pour identifier les causes racines.

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
class SmartRetryManager {
    constructor() {
        this.retryHistory = new Map(); // operationId -> retry info
        this.globalRetryCount = 0;
        this.maxGlobalRetries = 100; // per hour
        this.resetTime = 60 * 60 * 1000; // 1 hour
        this.lastReset = Date.now();
    }
    
    async executeWithRetry(operation, context = {}) {
        const operationId = context.operationId || this.generateOperationId();
        const strategy = this.getRetryStrategy(context.errorType);
        
        // Vérifier les limites globales
        this.checkGlobalLimits();
        
        let attempt = 1;
        let lastError;
        
        while (attempt <= strategy.maxRetries) {
            try {
                console.log(`🔄 Tentative ${attempt}/${strategy.maxRetries} pour ${operationId}`);
                const result = await operation();
                
                // Succès - nettoyer l'historique
                this.retryHistory.delete(operationId);
                console.log(`✅ Opération ${operationId} réussie au bout de ${attempt} tentative(s)`);
                return result;
                
            } catch (error) {
                lastError = error;
                console.log(`❌ Tentative ${attempt} échouée:`, error.message);
                
                // Vérifier si on doit retry pour ce type d'erreur
                if (!this.shouldRetry(error, attempt, strategy)) {
                    break;
                }
                
                // Enregistrer la tentative
                this.recordRetryAttempt(operationId, attempt, error);
                
                // Attendre selon la stratégie
                if (attempt < strategy.maxRetries) {
                    const delay = this.calculateDelay(strategy, attempt, error);
                    console.log(`⏱️ Attente de ${delay}ms avant nouvelle tentative`);
                    await this.sleep(delay);
                }
                
                attempt++;
            }
        }
        
        // Toutes les tentatives épuisées
        const finalError = new Error(
            `Opération ${operationId} échouée après ${attempt - 1} tentatives: ${lastError.message}`
        );
        finalError.originalError = lastError;
        finalError.retryInfo = this.retryHistory.get(operationId);
        
        throw finalError;
    }
    
    getRetryStrategy(errorType) {
        const strategies = {
            'NetworkError': {
                maxRetries: 5,
                baseDelay: 1000,
                backoffType: 'exponential',
                retryable: true,
                maxDelay: 30000
            },
            'RateLimitError': {
                maxRetries: 3,
                baseDelay: 5000,
                backoffType: 'linear',
                retryable: true,
                maxDelay: 60000
            },
            'ValidationError': {
                maxRetries: 0, // Pas de retry pour les erreurs de validation
                retryable: false
            },
            'AuthenticationError': {
                maxRetries: 1, // Un seul retry pour l'auth
                baseDelay: 0,
                retryable: true
            },
            'default': {
                maxRetries: 3,
                baseDelay: 1000,
                backoffType: 'exponential',
                retryable: true,
                maxDelay: 10000
            }
        };
        
        return strategies[errorType] || strategies.default;
    }
    
    shouldRetry(error, attempt, strategy) {
        if (!strategy.retryable) {
            console.log(`🚫 Type d'erreur ${error.name} non retryable`);
            return false;
        }
        
        // Vérifier si c'est une erreur spécifique non retryable
        if (error.status >= 400 && error.status < 500 && error.status !== 429) {
            console.log(`🚫 Erreur client ${error.status} non retryable`);
            return false;
        }
        
        return attempt < strategy.maxRetries;
    }
    
    calculateDelay(strategy, attempt, error) {
        let delay = strategy.baseDelay;
        
        switch (strategy.backoffType) {
            case 'exponential':
                delay = strategy.baseDelay * Math.pow(2, attempt - 1);
                break;
            case 'linear':
                delay = strategy.baseDelay * attempt;
                break;
            case 'fixed':
                delay = strategy.baseDelay;
                break;
        }
        
        // Ajouter du jitter pour éviter le thundering herd
        const jitter = Math.random() * 0.3 * delay; // 30% de jitter
        delay += jitter;
        
        // Respecter le délai maximum
        return Math.min(delay, strategy.maxDelay || 60000);
    }
    
    recordRetryAttempt(operationId, attempt, error) {
        if (!this.retryHistory.has(operationId)) {
            this.retryHistory.set(operationId, {
                attempts: [],
                startTime: Date.now()
            });
        }
        
        const info = this.retryHistory.get(operationId);
        info.attempts.push({
            attempt,
            timestamp: Date.now(),
            error: {
                name: error.name,
                message: error.message,
                status: error.status
            }
        });
        
        this.globalRetryCount++;
    }
    
    checkGlobalLimits() {
        const now = Date.now();
        
        // Reset compteur si nécessaire
        if (now - this.lastReset > this.resetTime) {
            this.globalRetryCount = 0;
            this.lastReset = now;
            console.log('🔄 Reset du compteur global de retries');
        }
        
        if (this.globalRetryCount >= this.maxGlobalRetries) {
            throw new Error(
                `Limite globale de retries atteinte (${this.maxGlobalRetries}/heure). ` +
                'Système en mode protection.'
            );
        }
    }
    
    generateOperationId() {
        return `op_${Date.now()}_${Math.random().toString(36).substr(2, 6)}`;
    }
    
    sleep(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
    
    getStats() {
        return {
            globalRetryCount: this.globalRetryCount,
            activeOperations: this.retryHistory.size,
            resetTime: new Date(this.lastReset + this.resetTime).toISOString()
        };
    }
}

// Test du système intelligent
async function testerSmartRetry() {
    const retryManager = new SmartRetryManager();
    
    // Simuler différents types d'erreurs
    const operations = [
        {
            name: 'Network timeout',
            errorType: 'NetworkError',
            operation: () => {
                if (Math.random() < 0.8) {
                    throw new NetworkError('Timeout', 408, '/api/data');
                }
                return { data: 'success' };
            }
        },
        {
            name: 'Rate limit',
            errorType: 'RateLimitError',
            operation: () => {
                if (Math.random() < 0.7) {
                    const error = new Error('Too many requests');
                    error.name = 'RateLimitError';
                    error.status = 429;
                    throw error;
                }
                return { data: 'success' };
            }
        },
        {
            name: 'Validation error',
            errorType: 'ValidationError',
            operation: () => {
                throw new ValidationError('Email invalid', 'email');
            }
        }
    ];
    
    for (const test of operations) {
        console.log(`\n--- Test: ${test.name} ---`);
        try {
            const result = await retryManager.executeWithRetry(
                test.operation,
                { errorType: test.errorType, operationId: `test_${test.name}` }
            );
            console.log('🎉 Résultat:', result);
        } catch (error) {
            console.error('💥 Échec final:', error.message);
        }
    }
    
    console.log('\n--- Stats du retry manager ---');
    console.log(retryManager.getStats());
}

testerSmartRetry();
```

**Explication :** Le système adapte sa stratégie de retry selon le type d'erreur, limite les retries globaux pour protéger le système, et ajoute du jitter pour éviter les pics de charge.

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Programmation asynchrone** : Try/catch avec async/await
- **Classes** : Création d'erreurs personnalisées
- **Promises** : Gestion des rejections
- **Objects** : Structuration des données d'erreur

### Prochaines étapes
- **Testing** : Tests d'erreurs et mocking de failures
- **Monitoring** : Intégration avec des outils de monitoring
- **Performance** : Impact de la gestion d'erreurs sur les performances

### Concepts complémentaires
- **Design Patterns** : Circuit Breaker, Retry, Bulkhead
- **Observabilité** : Logs, métriques, traces
- **Reliability Engineering** : SLA, SLO, Error Budget

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un système de gestion d'erreurs pour une application e-commerce

**Fonctionnalités requises :**
- [ ] Gestion d'erreurs pour panier, paiement, inventaire
- [ ] Retry intelligent selon le contexte métier
- [ ] Error Budget et alerting
- [ ] Dashboard de monitoring des erreurs
- [ ] Recovery strategies pour chaque type d'erreur
- [ ] Export des erreurs pour analyse

**Structure suggérée :**
```
📁 ecommerce-error-system/
├── 📄 error-types.js
├── 📄 retry-strategies.js
├── 📄 error-boundary.js
├── 📄 monitoring.js
├── 📄 recovery.js
└── 📄 dashboard.html
```

**Temps estimé :** 2 heures

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Error](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Error)
- [MDN - try...catch](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/try...catch)
- [Node.js Error Handling](https://nodejs.org/api/errors.html)

### Vidéos recommandées
- "Advanced Error Handling in JavaScript" - Programming with Mosh (25 min)
- Pourquoi cette vidéo est utile : Patterns avancés avec exemples concrets

### Articles approfondis
- "Error Handling in Node.js" - Joyent
- Ce que cet article apporte en plus : Patterns spécifiques au backend

### Outils de pratique
- [Sentry](https://sentry.io/) - Monitoring d'erreurs en production
- [Rollbar](https://rollbar.com/) - Tracking d'erreurs temps réel

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre Error Boundary et try/catch ?
   - Réponse : Error Boundary capture les erreurs globales non gérées, try/catch gère les erreurs locales

2. **Question pratique :** Quand utiliser un Circuit Breaker vs un Retry simple ?
   - Réponse : Circuit Breaker pour protéger contre les cascading failures, Retry pour les erreurs temporaires

3. **Question d'application :** Comment gérer les erreurs dans une chaîne de Promises ?
   - Réponse : Un seul .catch() à la fin capture toutes les erreurs de la chaîne

### Checklist de maîtrise
- [ ] Je comprends les différents types d'erreurs JavaScript
- [ ] Je sais créer des erreurs personnalisées appropriées
- [ ] Je maîtrise la gestion d'erreurs asynchrone avec async/await
- [ ] Je peux implémenter des patterns de resilience (Retry, Circuit Breaker)
- [ ] Je comprends l'importance du monitoring d'erreurs
- [ ] Je sais créer des systèmes de recovery graceful

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
// Avaler les erreurs silencieusement
try {
    await riskyOperation();
} catch (error) {
    // Erreur ignorée - très dangereux!
}
```

**Solution :**
```javascript
try {
    await riskyOperation();
} catch (error) {
    logger.error('Operation failed:', error);
    // Décider explicitement quoi faire
    throw new UserFriendlyError('Operation failed. Please try again.');
}
```

**Explication :** Toujours traiter les erreurs explicitement, ne jamais les ignorer silencieusement.

### Erreur fréquente 2
**Code problématique :**
```javascript
// Ne pas re-throw après logging
async function processData() {
    try {
        return await fetchData();
    } catch (error) {
        console.error('Error:', error);
        return null; // Perte d'information d'erreur
    }
}
```

**Solution :**
```javascript
async function processData() {
    try {
        return await fetchData();
    } catch (error) {
        logger.error('Failed to process data:', error);
        throw new ProcessingError('Data processing failed', error);
    }
}
```

**Explication :** Logger ET re-throw pour permettre la gestion d'erreur en amont.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- La hiérarchie des erreurs JavaScript et leur création
- Les patterns avancés de resilience et recovery
- L'importance du monitoring d'erreurs en production

### Questions à approfondir
- Integration avec des outils de monitoring externes
- Error correlation et machine learning pour la détection

### Révision programmée
- [ ] Révision dans 1 jour (rappel rapide)
- [ ] Révision dans 1 semaine (exercices)
- [ ] Révision dans 1 mois (application dans un projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #errors #exception-handling #resilience #monitoring #avance

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐⭐⭐

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 12.5/126*
