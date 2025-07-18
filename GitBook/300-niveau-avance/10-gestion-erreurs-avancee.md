# 🚨 Gestion d'Erreurs Avancée en JavaScript

> **Niveau :** 🔴 Avancé | **Durée :** 75 minutes | **Prérequis :** Async/Await, Promises, Classes

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** les patterns avancés de gestion d'erreurs
- [ ] **Créer** des systèmes d'erreurs personnalisées robustes
- [ ] **Implémenter** la surveillance et monitoring des erreurs
- [ ] **Construire** des applications résilientes en production

---

## 📚 Introduction

{% tabs %}
{% tab title="🧠 Importance critique" %}
**Pourquoi la gestion d'erreurs avancée ?**

- **Production-ready** : Applications robustes qui ne crashent pas
- **Expérience utilisateur** : Messages d'erreur clairs et utiles
- **Debugging** : Localisation rapide des problèmes
- **Monitoring** : Surveillance proactive des erreurs
- **Récupération** : Capacité à continuer malgré les erreurs

Une erreur non gérée peut :
- ❌ Crasher votre application
- ❌ Perdre des données utilisateur  
- ❌ Créer une expérience frustrante
- ❌ Impacter votre réputation
{% endtab %}

{% tab title="🔍 Types d'erreurs" %}
```javascript
// 🎯 Erreurs de syntaxe (détectées à l'analyse)
const malformed = { invalid syntax; // SyntaxError

// 🎯 Erreurs de référence (variable inexistante)
console.log(variableInexistante); // ReferenceError

// 🎯 Erreurs de type (méthode inexistante)
const nombre = 42;
nombre.push(1); // TypeError

// 🎯 Erreurs de plage (valeur hors limite)
const arr = new Array(-1); // RangeError

// 🎯 Erreurs logiques (logique incorrecte)
function diviser(a, b) {
    return a / b; // Pas d'erreur technique, mais division par 0
}

// 🎯 Erreurs asynchrones (promises rejetées)
fetch('/api/inexistant'); // Pas d'erreur immédiate
```
{% endtab %}

{% tab title="🏗️ Hiérarchie" %}
```javascript
// Hiérarchie native JavaScript
Error
├── SyntaxError     // Erreur de syntaxe
├── ReferenceError  // Variable non définie  
├── TypeError       // Type incorrect
├── RangeError      // Valeur hors limite
├── URIError        // URI malformée
└── EvalError       // Erreur dans eval()

// Erreurs personnalisées
class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
        this.code = 'VALIDATION_FAILED';
    }
}

class NetworkError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.name = 'NetworkError';
        this.statusCode = statusCode;
        this.isRetryable = statusCode >= 500;
    }
}
```
{% endtab %}
{% endtabs %}

---

## 🛠️ Try/Catch Avancé

### Patterns essentiels

{% tabs %}
{% tab title="🎯 Gestion basique" %}
```javascript
// ✅ Try/catch classique
function divisionSecurisee(a, b) {
    try {
        if (typeof a !== 'number' || typeof b !== 'number') {
            throw new TypeError('Les arguments doivent être des nombres');
        }
        
        if (b === 0) {
            throw new Error('Division par zéro interdite');
        }
        
        return a / b;
    } catch (erreur) {
        console.error('Erreur dans divisionSecurisee:', erreur.message);
        
        // Retourner une valeur par défaut ou re-lancer
        if (erreur instanceof TypeError) {
            return NaN;
        } else {
            return Infinity;
        }
    }
}

// Test
console.log(divisionSecurisee(10, 2));    // 5
console.log(divisionSecurisee(10, 0));    // Infinity
console.log(divisionSecurisee('10', 2));  // NaN
```
{% endtab %}

{% tab title="⚡ Async/Await" %}
```javascript
// ✅ Gestion d'erreurs asynchrones
async function obtenirDonneesUtilisateur(id) {
    try {
        // Validation des entrées
        if (!id || typeof id !== 'string') {
            throw new ValidationError('ID utilisateur invalide', 'id');
        }
        
        // Requête principale
        const response = await fetch(`/api/utilisateur/${id}`);
        
        if (!response.ok) {
            throw new NetworkError(
                `Erreur HTTP: ${response.status}`,
                response.status
            );
        }
        
        const utilisateur = await response.json();
        
        // Post-validation
        if (!utilisateur.email) {
            throw new ValidationError('Email manquant', 'email');
        }
        
        return utilisateur;
        
    } catch (erreur) {
        // Logging spécifique par type d'erreur
        if (erreur instanceof ValidationError) {
            console.warn('Validation échouée:', erreur.message, erreur.field);
        } else if (erreur instanceof NetworkError) {
            console.error('Erreur réseau:', erreur.message, erreur.statusCode);
        } else {
            console.error('Erreur inattendue:', erreur);
        }
        
        // Re-lancer pour traitement par l'appelant
        throw erreur;
    }
}

// Utilisation avec gestion d'erreur
async function chargerProfilUtilisateur(id) {
    try {
        const utilisateur = await obtenirDonneesUtilisateur(id);
        console.log('Profil chargé:', utilisateur);
        return utilisateur;
    } catch (erreur) {
        // Gestion finale avec fallback
        if (erreur instanceof NetworkError && erreur.isRetryable) {
            console.log('Erreur temporaire, tentative ultérieure...');
            return null; // Sera réessayé plus tard
        } else {
            console.log('Affichage du profil par défaut');
            return { nom: 'Utilisateur anonyme', email: 'non-disponible' };
        }
    }
}
```
{% endtab %}

{% tab title="🔄 Finally et cleanup" %}
```javascript
// ✅ Nettoyage garanti avec finally
class GestionnaireRessources {
    constructor() {
        this.ressourcesOuvertes = new Set();
    }
    
    async traiterFichier(cheminFichier) {
        let fichier = null;
        let verrou = null;
        
        try {
            // Acquisition des ressources
            console.log('🔓 Acquisition du verrou...');
            verrou = await this.acquerirVerrou(cheminFichier);
            this.ressourcesOuvertes.add(verrou);
            
            console.log('📂 Ouverture du fichier...');
            fichier = await this.ouvrirFichier(cheminFichier);
            this.ressourcesOuvertes.add(fichier);
            
            // Traitement principal
            console.log('⚙️ Traitement en cours...');
            const donnees = await fichier.lire();
            const resultat = await this.traiterDonnees(donnees);
            
            console.log('💾 Sauvegarde...');
            await fichier.ecrire(resultat);
            
            return resultat;
            
        } catch (erreur) {
            console.error('❌ Erreur pendant le traitement:', erreur);
            
            // Rollback si nécessaire
            if (fichier && fichier.modifie) {
                await fichier.annulerModifications();
            }
            
            throw erreur;
            
        } finally {
            // Nettoyage TOUJOURS exécuté
            console.log('🧹 Nettoyage des ressources...');
            
            if (fichier) {
                await fichier.fermer();
                this.ressourcesOuvertes.delete(fichier);
                console.log('✅ Fichier fermé');
            }
            
            if (verrou) {
                await this.libererVerrou(verrou);
                this.ressourcesOuvertes.delete(verrou);
                console.log('✅ Verrou libéré');
            }
            
            console.log(`📊 Ressources restantes: ${this.ressourcesOuvertes.size}`);
        }
    }
    
    // Méthodes simulées
    async acquerirVerrou(chemin) {
        return { id: Math.random(), chemin };
    }
    
    async libererVerrou(verrou) {
        console.log(`Verrou ${verrou.id} libéré`);
    }
    
    async ouvrirFichier(chemin) {
        return {
            modifie: false,
            async lire() { return 'données...'; },
            async ecrire(data) { this.modifie = true; },
            async fermer() { console.log('Fichier fermé'); },
            async annulerModifications() { this.modifie = false; }
        };
    }
    
    async traiterDonnees(donnees) {
        // Simulation d'un traitement qui peut échouer
        if (Math.random() < 0.3) {
            throw new Error('Erreur de traitement simulée');
        }
        return donnees.toUpperCase();
    }
}
```
{% endtab %}
{% endtabs %}

### 🏃‍♂️ Exercice Pratique : Gestionnaire de requêtes robuste

{% hint style="warning" %}
**Exercice :** Créez un gestionnaire de requêtes avec retry automatique et circuit breaker
{% endhint %}

```javascript
// Complétez cette classe
class GestionnaireRequetesRobuste {
    constructor(options = {}) {
        this.maxRetries = options.maxRetries || 3;
        this.retryDelay = options.retryDelay || 1000;
        this.circuitBreakerThreshold = options.circuitBreakerThreshold || 5;
        this.circuitBreakerTimeout = options.circuitBreakerTimeout || 30000;
        
        // TODO: Initialiser le state du circuit breaker
        this.circuitState = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
        this.failureCount = 0;
        this.lastFailureTime = null;
    }
    
    // TODO: Méthode principale avec retry et circuit breaker
    async effectuerRequete(url, options = {}) {
        // 1. Vérifier l'état du circuit breaker
        // 2. Effectuer la requête avec retry
        // 3. Gérer les succès/échecs pour le circuit breaker
        // 4. Retourner le résultat ou lancer une erreur appropriée
    }
    
    // TODO: Logique de retry avec backoff exponentiel
    async _effectuerRequeteAvecRetry(url, options, tentative = 1) {
        // Implémenter la logique de retry
    }
    
    // TODO: Gestion du circuit breaker
    _verifierCircuitBreaker() {
        // Vérifier si on peut effectuer la requête
    }
    
    _enregistrerSucces() {
        // Réinitialiser les compteurs d'échec
    }
    
    _enregistrerEchec() {
        // Incrémenter les compteurs et potentiellement ouvrir le circuit
    }
    
    // TODO: Méthodes utilitaires
    obtenirStatistiques() {
        return {
            circuitState: this.circuitState,
            failureCount: this.failureCount,
            lastFailureTime: this.lastFailureTime
        };
    }
}

// Test
async function testerGestionnaireRobuste() {
    const gestionnaire = new GestionnaireRequetesRobuste({
        maxRetries: 3,
        retryDelay: 500,
        circuitBreakerThreshold: 3
    });
    
    // TODO: Tester différents scénarios
    try {
        // URL qui marche
        const resultat1 = await gestionnaire.effectuerRequete('https://httpbin.org/get');
        console.log('Succès:', resultat1.status);
        
        // URL qui échoue (pour tester le retry)
        const resultat2 = await gestionnaire.effectuerRequete('https://httpbin.org/status/500');
        
    } catch (erreur) {
        console.error('Erreur finale:', erreur.message);
        console.log('Stats:', gestionnaire.obtenirStatistiques());
    }
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
async effectuerRequete(url, options = {}) {
    // Vérifier le circuit breaker
    this._verifierCircuitBreaker();
    
    try {
        const resultat = await this._effectuerRequeteAvecRetry(url, options);
        this._enregistrerSucces();
        return resultat;
    } catch (erreur) {
        this._enregistrerEchec();
        throw erreur;
    }
}

async _effectuerRequeteAvecRetry(url, options, tentative = 1) {
    try {
        const response = await fetch(url, options);
        if (!response.ok) {
            throw new NetworkError(`HTTP ${response.status}`, response.status);
        }
        return response;
    } catch (erreur) {
        if (tentative < this.maxRetries && this._estRetryable(erreur)) {
            const delai = this.retryDelay * Math.pow(2, tentative - 1);
            await new Promise(resolve => setTimeout(resolve, delai));
            return this._effectuerRequeteAvecRetry(url, options, tentative + 1);
        }
        throw erreur;
    }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
class GestionnaireRequetesRobuste {
    constructor(options = {}) {
        this.maxRetries = options.maxRetries || 3;
        this.retryDelay = options.retryDelay || 1000;
        this.circuitBreakerThreshold = options.circuitBreakerThreshold || 5;
        this.circuitBreakerTimeout = options.circuitBreakerTimeout || 30000;
        
        this.circuitState = 'CLOSED';
        this.failureCount = 0;
        this.lastFailureTime = null;
        this.successCount = 0;
        this.totalRequests = 0;
    }
    
    async effectuerRequete(url, options = {}) {
        this.totalRequests++;
        
        this._verifierCircuitBreaker();
        
        try {
            const resultat = await this._effectuerRequeteAvecRetry(url, options);
            this._enregistrerSucces();
            return resultat;
        } catch (erreur) {
            this._enregistrerEchec();
            throw new NetworkError(
                `Requête échouée après ${this.maxRetries} tentatives: ${erreur.message}`,
                erreur.status || 0
            );
        }
    }
    
    async _effectuerRequeteAvecRetry(url, options, tentative = 1) {
        try {
            console.log(`🔄 Tentative ${tentative} pour ${url}`);
            
            const controller = new AbortController();
            const timeoutId = setTimeout(() => controller.abort(), 10000);
            
            const response = await fetch(url, {
                ...options,
                signal: controller.signal
            });
            
            clearTimeout(timeoutId);
            
            if (!response.ok) {
                throw new NetworkError(
                    `HTTP ${response.status}: ${response.statusText}`,
                    response.status
                );
            }
            
            console.log(`✅ Succès tentative ${tentative}`);
            return response;
            
        } catch (erreur) {
            console.log(`❌ Échec tentative ${tentative}:`, erreur.message);
            
            if (tentative < this.maxRetries && this._estRetryable(erreur)) {
                const delai = this.retryDelay * Math.pow(2, tentative - 1);
                console.log(`⏳ Attente ${delai}ms avant retry...`);
                
                await new Promise(resolve => setTimeout(resolve, delai));
                return this._effectuerRequeteAvecRetry(url, options, tentative + 1);
            }
            
            throw erreur;
        }
    }
    
    _estRetryable(erreur) {
        if (erreur.name === 'AbortError') return false;
        if (erreur instanceof NetworkError) {
            return erreur.statusCode >= 500 || erreur.statusCode === 0;
        }
        return true;
    }
    
    _verifierCircuitBreaker() {
        const maintenant = Date.now();
        
        switch (this.circuitState) {
            case 'OPEN':
                if (maintenant - this.lastFailureTime > this.circuitBreakerTimeout) {
                    console.log('🔄 Circuit breaker: OPEN → HALF_OPEN');
                    this.circuitState = 'HALF_OPEN';
                } else {
                    throw new Error('Circuit breaker OUVERT - service indisponible');
                }
                break;
                
            case 'HALF_OPEN':
                // Permet une requête de test
                break;
                
            case 'CLOSED':
                // Fonctionnement normal
                break;
        }
    }
    
    _enregistrerSucces() {
        this.successCount++;
        this.failureCount = 0;
        
        if (this.circuitState === 'HALF_OPEN') {
            console.log('✅ Circuit breaker: HALF_OPEN → CLOSED');
            this.circuitState = 'CLOSED';
        }
    }
    
    _enregistrerEchec() {
        this.failureCount++;
        this.lastFailureTime = Date.now();
        
        if (this.failureCount >= this.circuitBreakerThreshold) {
            if (this.circuitState !== 'OPEN') {
                console.log('🚨 Circuit breaker: CLOSED → OPEN');
                this.circuitState = 'OPEN';
            }
        }
    }
    
    obtenirStatistiques() {
        const successRate = this.totalRequests > 0 
            ? (this.successCount / this.totalRequests * 100).toFixed(2)
            : 0;
            
        return {
            circuitState: this.circuitState,
            failureCount: this.failureCount,
            successCount: this.successCount,
            totalRequests: this.totalRequests,
            successRate: `${successRate}%`,
            lastFailureTime: this.lastFailureTime 
                ? new Date(this.lastFailureTime).toISOString()
                : null
        };
    }
    
    reset() {
        this.circuitState = 'CLOSED';
        this.failureCount = 0;
        this.successCount = 0;
        this.totalRequests = 0;
        this.lastFailureTime = null;
    }
}

// Classes d'erreur personnalisées
class NetworkError extends Error {
    constructor(message, statusCode = 0) {
        super(message);
        this.name = 'NetworkError';
        this.statusCode = statusCode;
        this.isRetryable = statusCode >= 500 || statusCode === 0;
    }
}

class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
    }
}
```
{% endtab %}
{% endtabs %}

---

## 🎯 Erreurs Personnalisées

### Création d'erreurs métier

{% tabs %}
{% tab title="🏗️ Classes d'erreur" %}
```javascript
// Classe de base pour erreurs applicatives
class AppError extends Error {
    constructor(message, code, statusCode = 500, isOperational = true) {
        super(message);
        
        this.name = this.constructor.name;
        this.code = code;
        this.statusCode = statusCode;
        this.isOperational = isOperational;
        this.timestamp = new Date().toISOString();
        
        // Capture la stack trace
        Error.captureStackTrace(this, this.constructor);
    }
    
    toJSON() {
        return {
            name: this.name,
            message: this.message,
            code: this.code,
            statusCode: this.statusCode,
            timestamp: this.timestamp,
            stack: this.stack
        };
    }
}

// Erreurs spécifiques métier
class ValidationError extends AppError {
    constructor(message, field, value) {
        super(message, 'VALIDATION_ERROR', 400);
        this.field = field;
        this.value = value;
    }
}

class AuthenticationError extends AppError {
    constructor(message = 'Non autorisé') {
        super(message, 'AUTH_ERROR', 401);
    }
}

class AuthorizationError extends AppError {
    constructor(message = 'Accès interdit', requiredRole) {
        super(message, 'AUTHZ_ERROR', 403);
        this.requiredRole = requiredRole;
    }
}

class NotFoundError extends AppError {
    constructor(resource, id) {
        super(`${resource} avec l'ID ${id} introuvable`, 'NOT_FOUND', 404);
        this.resource = resource;
        this.resourceId = id;
    }
}

class BusinessError extends AppError {
    constructor(message, businessRule) {
        super(message, 'BUSINESS_RULE_VIOLATION', 422);
        this.businessRule = businessRule;
    }
}

class ExternalServiceError extends AppError {
    constructor(service, originalError) {
        super(`Erreur du service externe: ${service}`, 'EXTERNAL_SERVICE_ERROR', 502);
        this.service = service;
        this.originalError = originalError;
    }
}

// Exemple d'utilisation
class UserService {
    async creerUtilisateur(donnees) {
        // Validation
        if (!donnees.email) {
            throw new ValidationError('Email requis', 'email', donnees.email);
        }
        
        if (!this._estEmailValide(donnees.email)) {
            throw new ValidationError('Format email invalide', 'email', donnees.email);
        }
        
        // Vérification unicité
        const utilisateurExistant = await this.rechercherParEmail(donnees.email);
        if (utilisateurExistant) {
            throw new BusinessError(
                'Un utilisateur avec cet email existe déjà',
                'EMAIL_UNIQUENESS'
            );
        }
        
        // Création
        try {
            return await this.sauvegarder(donnees);
        } catch (erreur) {
            if (erreur.code === 'DB_CONNECTION_ERROR') {
                throw new ExternalServiceError('Base de données', erreur);
            }
            throw erreur;
        }
    }
    
    _estEmailValide(email) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    }
}
```
{% endtab %}

{% tab title="🎛️ Gestionnaire global" %}
```javascript
// Gestionnaire centralisé d'erreurs
class GestionnaireErreurs {
    constructor(options = {}) {
        this.loggers = new Map();
        this.handlers = new Map();
        this.fallbackHandler = options.fallbackHandler || this._defaultHandler;
        this.isProduction = options.isProduction || false;
    }
    
    // Enregistrer des handlers par type d'erreur
    enregistrerHandler(ErrorClass, handler) {
        this.handlers.set(ErrorClass.name, handler);
    }
    
    // Enregistrer des loggers
    enregistrerLogger(nom, logger) {
        this.loggers.set(nom, logger);
    }
    
    // Traitement principal des erreurs
    async traiterErreur(erreur, contexte = {}) {
        const erreurEnrichie = this._enrichirErreur(erreur, contexte);
        
        // Logging
        await this._loggerErreur(erreurEnrichie);
        
        // Handler spécifique
        const handler = this.handlers.get(erreur.constructor.name) || this.fallbackHandler;
        
        try {
            return await handler(erreurEnrichie, contexte);
        } catch (handlerError) {
            console.error('Erreur dans le handler d\'erreur:', handlerError);
            return this._defaultHandler(erreurEnrichie, contexte);
        }
    }
    
    _enrichirErreur(erreur, contexte) {
        const erreurEnrichie = {
            ...erreur,
            id: this._genererIdErreur(),
            contexte: {
                timestamp: new Date().toISOString(),
                userAgent: typeof navigator !== 'undefined' ? navigator.userAgent : 'server',
                url: typeof window !== 'undefined' ? window.location.href : contexte.url,
                userId: contexte.userId,
                sessionId: contexte.sessionId,
                ...contexte
            }
        };
        
        // Masquer les informations sensibles en production
        if (this.isProduction && !erreur.isOperational) {
            erreurEnrichie.message = 'Une erreur interne s\'est produite';
            erreurEnrichie.stack = undefined;
        }
        
        return erreurEnrichie;
    }
    
    async _loggerErreur(erreur) {
        // Logger console (toujours)
        const logLevel = this._determinerNiveauLog(erreur);
        console[logLevel](`[${erreur.name}] ${erreur.message}`, {
            id: erreur.id,
            code: erreur.code,
            contexte: erreur.contexte
        });
        
        // Loggers externes
        const promises = Array.from(this.loggers.values()).map(logger => {
            return logger.log(erreur).catch(err => {
                console.warn('Erreur de logging externe:', err);
            });
        });
        
        await Promise.allSettled(promises);
    }
    
    _determinerNiveauLog(erreur) {
        if (erreur.statusCode >= 500) return 'error';
        if (erreur.statusCode >= 400) return 'warn';
        return 'info';
    }
    
    _genererIdErreur() {
        return Math.random().toString(36).substr(2, 9) + Date.now().toString(36);
    }
    
    _defaultHandler(erreur, contexte) {
        return {
            success: false,
            error: {
                message: erreur.message,
                code: erreur.code,
                id: erreur.id
            }
        };
    }
}

// Configuration globale
const gestionnaireErreurs = new GestionnaireErreurs({
    isProduction: process.env.NODE_ENV === 'production'
});

// Handlers spécifiques
gestionnaireErreurs.enregistrerHandler(ValidationError, (erreur) => ({
    success: false,
    error: {
        message: erreur.message,
        field: erreur.field,
        code: erreur.code
    }
}));

gestionnaireErreurs.enregistrerHandler(NotFoundError, (erreur) => ({
    success: false,
    error: {
        message: 'Ressource introuvable',
        resource: erreur.resource,
        id: erreur.resourceId
    }
}));

// Logger externe (exemple)
gestionnaireErreurs.enregistrerLogger('sentry', {
    async log(erreur) {
        // Envoyer à Sentry, Bugsnag, etc.
        console.log('📡 Envoyé à Sentry:', erreur.id);
    }
});
```
{% endtab %}

{% tab title="🔧 Middleware Express" %}
```javascript
// Middleware Express pour gestion d'erreurs
function middlewareGestionErreurs(gestionnaireErreurs) {
    return async (erreur, req, res, next) => {
        const contexte = {
            method: req.method,
            url: req.originalUrl,
            userAgent: req.get('User-Agent'),
            ip: req.ip,
            userId: req.user?.id,
            sessionId: req.sessionID
        };
        
        const resultat = await gestionnaireErreurs.traiterErreur(erreur, contexte);
        
        const statusCode = erreur.statusCode || 500;
        res.status(statusCode).json(resultat);
    };
}

// Wrapper pour routes async
function wrapAsync(fn) {
    return (req, res, next) => {
        Promise.resolve(fn(req, res, next)).catch(next);
    };
}

// Exemple d'utilisation dans Express
const express = require('express');
const app = express();

// Routes
app.get('/api/users/:id', wrapAsync(async (req, res) => {
    const utilisateur = await userService.obtenirUtilisateur(req.params.id);
    if (!utilisateur) {
        throw new NotFoundError('Utilisateur', req.params.id);
    }
    res.json(utilisateur);
}));

app.post('/api/users', wrapAsync(async (req, res) => {
    const nouvelUtilisateur = await userService.creerUtilisateur(req.body);
    res.status(201).json(nouvelUtilisateur);
}));

// Middleware de gestion d'erreurs (doit être en dernier)
app.use(middlewareGestionErreurs(gestionnaireErreurs));
```
{% endtab %}
{% endtabs %}

---

## 📊 Monitoring et Observabilité

### Surveillance proactive

{% tabs %}
{% tab title="📈 Métriques d'erreurs" %}
```javascript
// Collecteur de métriques d'erreurs
class MetriquesErreurs {
    constructor() {
        this.metriques = {
            totalErreurs: 0,
            erreursParType: new Map(),
            erreursParCode: new Map(),
            erreursParHeure: new Map(),
            tempsReponse: [],
            disponibilite: {
                uptime: Date.now(),
                totalRequetes: 0,
                requetesReussies: 0
            }
        };
        
        this.historique = [];
        this.maxHistorique = 1000;
        
        // Nettoyage périodique
        setInterval(() => this.nettoyerHistorique(), 60000);
    }
    
    enregistrerErreur(erreur, tempsReponse = 0) {
        this.metriques.totalErreurs++;
        
        // Par type
        const type = erreur.constructor.name;
        this.metriques.erreursParType.set(
            type,
            (this.metriques.erreursParType.get(type) || 0) + 1
        );
        
        // Par code
        if (erreur.code) {
            this.metriques.erreursParCode.set(
                erreur.code,
                (this.metriques.erreursParCode.get(erreur.code) || 0) + 1
            );
        }
        
        // Par heure
        const heure = new Date().getHours();
        this.metriques.erreursParHeure.set(
            heure,
            (this.metriques.erreursParHeure.get(heure) || 0) + 1
        );
        
        // Temps de réponse
        this.metriques.tempsReponse.push(tempsReponse);
        if (this.metriques.tempsReponse.length > 100) {
            this.metriques.tempsReponse.shift();
        }
        
        // Historique
        this.historique.push({
            timestamp: Date.now(),
            type,
            code: erreur.code,
            message: erreur.message,
            tempsReponse
        });
        
        // Vérifier les seuils d'alerte
        this.verifierAlertes();
    }
    
    enregistrerSucces(tempsReponse = 0) {
        this.metriques.disponibilite.totalRequetes++;
        this.metriques.disponibilite.requetesReussies++;
        
        this.metriques.tempsReponse.push(tempsReponse);
        if (this.metriques.tempsReponse.length > 100) {
            this.metriques.tempsReponse.shift();
        }
    }
    
    obtenirStatistiques() {
        const maintenant = Date.now();
        const uptime = maintenant - this.metriques.disponibilite.uptime;
        
        const tempsReponse = this.metriques.tempsReponse;
        const tempsReponseMoyen = tempsReponse.length > 0
            ? tempsReponse.reduce((a, b) => a + b, 0) / tempsReponse.length
            : 0;
            
        const tauxDisponibilite = this.metriques.disponibilite.totalRequetes > 0
            ? (this.metriques.disponibilite.requetesReussies / this.metriques.disponibilite.totalRequetes) * 100
            : 100;
        
        return {
            totalErreurs: this.metriques.totalErreurs,
            erreursParType: Object.fromEntries(this.metriques.erreursParType),
            erreursParCode: Object.fromEntries(this.metriques.erreursParCode),
            erreursParHeure: Object.fromEntries(this.metriques.erreursParHeure),
            tempsReponseMoyen: Math.round(tempsReponseMoyen),
            tauxDisponibilite: Math.round(tauxDisponibilite * 100) / 100,
            uptime: Math.round(uptime / 1000),
            dernieresErreurs: this.historique.slice(-10)
        };
    }
    
    verifierAlertes() {
        const stats = this.obtenirStatistiques();
        
        // Taux d'erreur élevé (> 5% sur les 10 dernières minutes)
        const erreursRecentes = this.historique.filter(
            e => Date.now() - e.timestamp < 600000 // 10 minutes
        ).length;
        
        if (erreursRecentes > 10) {
            this.declencherAlerte('TAUX_ERREUR_ELEVE', {
                erreursRecentes,
                seuil: 10
            });
        }
        
        // Temps de réponse élevé
        if (stats.tempsReponseMoyen > 5000) {
            this.declencherAlerte('TEMPS_REPONSE_ELEVE', {
                tempsReponse: stats.tempsReponseMoyen,
                seuil: 5000
            });
        }
        
        // Disponibilité faible
        if (stats.tauxDisponibilite < 95) {
            this.declencherAlerte('DISPONIBILITE_FAIBLE', {
                tauxActuel: stats.tauxDisponibilite,
                seuil: 95
            });
        }
    }
    
    declencherAlerte(type, donnees) {
        console.warn(`🚨 ALERTE ${type}:`, donnees);
        
        // Envoyer notification (Slack, email, etc.)
        this.envoyerNotification(type, donnees);
    }
    
    async envoyerNotification(type, donnees) {
        // Simulation d'envoi de notification
        const message = {
            alert: type,
            data: donnees,
            timestamp: new Date().toISOString(),
            severity: this.determinerSeverite(type)
        };
        
        console.log('📧 Notification envoyée:', message);
    }
    
    determinerSeverite(type) {
        const severites = {
            'TAUX_ERREUR_ELEVE': 'HIGH',
            'TEMPS_REPONSE_ELEVE': 'MEDIUM',
            'DISPONIBILITE_FAIBLE': 'CRITICAL'
        };
        
        return severites[type] || 'LOW';
    }
    
    nettoyerHistorique() {
        if (this.historique.length > this.maxHistorique) {
            this.historique = this.historique.slice(-this.maxHistorique);
        }
        
        // Nettoyer les anciennes métriques par heure
        const heureActuelle = new Date().getHours();
        // Garder seulement les 24 dernières heures
        for (const [heure] of this.metriques.erreursParHeure) {
            if (Math.abs(heure - heureActuelle) > 12) {
                this.metriques.erreursParHeure.delete(heure);
            }
        }
    }
    
    reset() {
        this.metriques = {
            totalErreurs: 0,
            erreursParType: new Map(),
            erreursParCode: new Map(),
            erreursParHeure: new Map(),
            tempsReponse: [],
            disponibilite: {
                uptime: Date.now(),
                totalRequetes: 0,
                requetesReussies: 0
            }
        };
        this.historique = [];
    }
}

// Instance globale
const metriquesErreurs = new MetriquesErreurs();
```
{% endtab %}

{% tab title="🔍 Health Check" %}
```javascript
// Système de health check complet
class HealthChecker {
    constructor() {
        this.checks = new Map();
        this.status = 'UNKNOWN';
        this.derniereVerification = null;
        this.intervalleVerification = 30000; // 30 secondes
        this.historique = [];
        
        this.demarrerVerificationsPeriodiques();
    }
    
    enregistrerCheck(nom, checkFunction, options = {}) {
        this.checks.set(nom, {
            fonction: checkFunction,
            timeout: options.timeout || 5000,
            critique: options.critique || false,
            description: options.description || nom
        });
    }
    
    async effectuerVerification() {
        const debut = Date.now();
        const resultats = new Map();
        
        // Exécuter tous les checks en parallèle
        const promises = Array.from(this.checks.entries()).map(async ([nom, check]) => {
            try {
                const timeoutPromise = new Promise((_, reject) =>
                    setTimeout(() => reject(new Error('Health check timeout')), check.timeout)
                );
                
                const checkPromise = Promise.resolve(check.fonction());
                const resultat = await Promise.race([checkPromise, timeoutPromise]);
                
                resultats.set(nom, {
                    status: 'UP',
                    message: 'OK',
                    details: resultat,
                    critique: check.critique,
                    description: check.description
                });
                
            } catch (erreur) {
                resultats.set(nom, {
                    status: 'DOWN',
                    message: erreur.message,
                    details: null,
                    critique: check.critique,
                    description: check.description
                });
            }
        });
        
        await Promise.allSettled(promises);
        
        // Déterminer le status global
        let statusGlobal = 'UP';
        let checksEchoues = [];
        
        for (const [nom, resultat] of resultats) {
            if (resultat.status === 'DOWN') {
                if (resultat.critique) {
                    statusGlobal = 'DOWN';
                } else if (statusGlobal === 'UP') {
                    statusGlobal = 'DEGRADED';
                }
                checksEchoues.push(nom);
            }
        }
        
        const duree = Date.now() - debut;
        const rapport = {
            status: statusGlobal,
            timestamp: new Date().toISOString(),
            duree,
            checks: Object.fromEntries(resultats),
            checksEchoues,
            metriques: metriquesErreurs.obtenirStatistiques()
        };
        
        this.status = statusGlobal;
        this.derniereVerification = rapport;
        
        // Historique
        this.historique.push(rapport);
        if (this.historique.length > 100) {
            this.historique.shift();
        }
        
        // Alertes si changement d'état
        if (checksEchoues.length > 0) {
            console.warn('⚠️ Health check issues:', checksEchoues);
        }
        
        return rapport;
    }
    
    demarrerVerificationsPeriodiques() {
        setInterval(async () => {
            try {
                await this.effectuerVerification();
            } catch (erreur) {
                console.error('Erreur lors de la vérification de santé:', erreur);
            }
        }, this.intervalleVerification);
        
        // Première vérification immédiate
        this.effectuerVerification();
    }
    
    obtenirRapportComplet() {
        return {
            statusGlobal: this.status,
            derniereVerification: this.derniereVerification,
            historique: this.historique.slice(-10),
            tendances: this.analyserTendances()
        };
    }
    
    analyserTendances() {
        if (this.historique.length < 5) {
            return { message: 'Pas assez de données pour analyser les tendances' };
        }
        
        const recent = this.historique.slice(-10);
        const problemesRecents = recent.filter(r => r.status !== 'UP').length;
        const tempsMoyenReponse = recent.reduce((acc, r) => acc + r.duree, 0) / recent.length;
        
        return {
            problemesRecents: `${problemesRecents}/10`,
            tempsMoyenReponse: Math.round(tempsMoyenReponse),
            stabilite: problemesRecents === 0 ? 'STABLE' : problemesRecents > 3 ? 'INSTABLE' : 'MONITORER'
        };
    }
}

// Configuration des health checks
const healthChecker = new HealthChecker();

// Check base de données
healthChecker.enregistrerCheck('database', async () => {
    // Simulation d'un check DB
    const debut = Date.now();
    await new Promise(resolve => setTimeout(resolve, Math.random() * 100));
    
    if (Math.random() < 0.05) { // 5% de chance d'échec
        throw new Error('Connexion DB timeout');
    }
    
    return {
        latence: Date.now() - debut,
        connectionsActives: Math.floor(Math.random() * 20)
    };
}, { critique: true, description: 'Connexion base de données principale' });

// Check service externe
healthChecker.enregistrerCheck('external-api', async () => {
    const response = await fetch('https://httpbin.org/status/200', { 
        timeout: 3000 
    });
    
    if (!response.ok) {
        throw new Error(`API externe indisponible: ${response.status}`);
    }
    
    return { status: response.status };
}, { critique: false, description: 'Service externe requis' });

// Check mémoire
healthChecker.enregistrerCheck('memory', async () => {
    if (typeof process !== 'undefined') {
        const memoire = process.memoryUsage();
        const usage = (memoire.heapUsed / memoire.heapTotal) * 100;
        
        if (usage > 90) {
            throw new Error(`Usage mémoire critique: ${usage.toFixed(2)}%`);
        }
        
        return {
            heapUsed: Math.round(memoire.heapUsed / 1024 / 1024),
            heapTotal: Math.round(memoire.heapTotal / 1024 / 1024),
            usage: Math.round(usage)
        };
    }
    
    return { message: 'Check mémoire non disponible côté client' };
}, { critique: false, description: 'Utilisation mémoire du processus' });
```
{% endtab %}
{% endtabs %}

---

## 🎯 Projet Final : Système de Monitoring Complet

{% hint style="danger" %}
**Projet Capstone :** Créez un système complet de gestion et monitoring d'erreurs
{% endhint %}

### Spécifications du projet

Créez un système qui intègre :

1. **Gestionnaire d'erreurs centralisé** avec types personnalisés
2. **Monitoring en temps réel** avec métriques et alertes
3. **Health checks automatiques** de tous les services
4. **Dashboard web** pour visualiser les erreurs
5. **Notifications** automatiques selon la criticité
6. **Recovery automatique** quand possible

```javascript
// Votre mission : compléter ce système
class SystemeMonitoringComplet {
    constructor(config = {}) {
        this.config = {
            alertes: {
                email: config.email || [],
                slack: config.slack || null,
                sms: config.sms || []
            },
            seuils: {
                tauxErreur: config.tauxErreur || 5,
                tempsReponse: config.tempsReponse || 5000,
                disponibilite: config.disponibilite || 95
            },
            recovery: {
                autoRestart: config.autoRestart || false,
                maxRestarts: config.maxRestarts || 3
            }
        };
        
        // TODO: Initialiser tous les composants
        this.gestionnaireErreurs = new GestionnaireErreurs();
        this.metriques = new MetriquesErreurs();
        this.healthChecker = new HealthChecker();
        this.dashboard = new Dashboard();
        this.notificateur = new Notificateur(this.config.alertes);
        
        this.demarrer();
    }
    
    // TODO: Orchestrer tous les composants
    demarrer() {
        // Connecter les événements
        // Démarrer les vérifications périodiques
        // Lancer le dashboard web
    }
    
    // TODO: API publique
    async traiterErreur(erreur, contexte) {}
    async obtenirStatistiques() {}
    async exporterRapport(format = 'json') {}
    async configurerAlerte(type, seuil, actions) {}
    async testerRecovery() {}
}

// Dashboard web simple
class Dashboard {
    constructor() {
        this.serveur = null;
        this.donnees = {
            erreurs: [],
            metriques: {},
            health: {},
            alertes: []
        };
    }
    
    // TODO: Créer un serveur web simple pour afficher les données
    demarrer(port = 3000) {}
    
    // TODO: API REST pour les données
    creerAPI() {}
    
    // TODO: Interface HTML simple
    genererHTML() {}
}

// Système de notifications
class Notificateur {
    constructor(config) {
        this.config = config;
        this.canaux = new Map();
        this.historique = [];
        
        this.initialiserCanaux();
    }
    
    // TODO: Différents canaux de notification
    initialiserCanaux() {}
    async envoyerNotification(niveau, message, donnees) {}
    async testerNotifications() {}
}

// Tests et démonstration
async function demonstrationComplete() {
    const systeme = new SystemeMonitoringComplet({
        email: ['admin@example.com'],
        tauxErreur: 3,
        autoRestart: true
    });
    
    // Simuler différents types d'erreurs
    try {
        throw new ValidationError('Test validation', 'email');
    } catch (e) {
        await systeme.traiterErreur(e, { userId: '123' });
    }
    
    // Générer du trafic pour tester les seuils
    for (let i = 0; i < 100; i++) {
        if (Math.random() < 0.1) {
            try {
                throw new NetworkError('Test network', 500);
            } catch (e) {
                await systeme.traiterErreur(e);
            }
        }
    }
    
    // Afficher le rapport final
    const rapport = await systeme.exporterRapport('html');
    console.log('Rapport généré:', rapport);
}
```

### Critères d'évaluation

{% tabs %}
{% tab title="🎯 Fonctionnalités de base" %}
- [ ] **Gestion d'erreurs** complète avec types personnalisés
- [ ] **Métriques** en temps réel (taux, latence, disponibilité)
- [ ] **Health checks** automatiques
- [ ] **Alertes** configurables par seuils
- [ ] **Dashboard** web fonctionnel
- [ ] **Notifications** multi-canaux
- [ ] **Export** de rapports
{% endtab %}

{% tab title="⭐ Fonctionnalités avancées" %}
- [ ] **Recovery automatique** des services
- [ ] **Prédiction** des pannes par ML
- [ ] **Intégration** avec services externes (Slack, PagerDuty)
- [ ] **Géolocalisation** des erreurs
- [ ] **A/B testing** pour résolution d'erreurs
- [ ] **Circuit breaker** intelligent
- [ ] **Replay** des erreurs pour debugging
{% endtab %}

{% tab title="🏆 Excellence technique" %}
- [ ] **Architecture** scalable et modulaire
- [ ] **Performance** optimisée (< 10ms overhead)
- [ ] **Tests** automatisés complets
- [ ] **Documentation** API complète
- [ ] **Sécurité** (authentification, chiffrement)
- [ ] **Observabilité** de l'observabilité (meta-monitoring)
- [ ] **Déploiement** containerisé
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Bravo !** Vous maîtrisez maintenant la gestion d'erreurs avancée en JavaScript !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts maîtrisés" %}
- **Try/catch avancé** avec gestion async/await
- **Erreurs personnalisées** avec hiérarchie métier
- **Patterns de résilience** (retry, circuit breaker)
- **Monitoring proactif** avec métriques
- **Health checks** automatisés
- **Système d'alertes** intelligent
- **Recovery automatique** des services
{% endtab %}

{% tab title="🛠️ Compétences techniques" %}
- Créer des applications production-ready
- Implémenter la surveillance d'erreurs
- Gérer la résilience des systèmes
- Optimiser l'observabilité
- Automatiser la résolution de problèmes
- Concevoir des dashboards de monitoring
{% endtab %}

{% tab title="🚀 Applications pratiques" %}
- **Applications critiques** avec haute disponibilité
- **Microservices** résilients
- **APIs robustes** avec gestion d'erreurs complète  
- **Systèmes distribués** avec monitoring centralisé
- **DevOps** avec observabilité automatisée
- **Production** avec alertes proactives
{% endtab %}
{% endtabs %}

### Ressources pour approfondir

- 📚 [Error Handling Best Practices](https://www.joyent.com/node-js/production/design/errors)
- 🎥 [Building Resilient Node.js Applications](https://nodejs.org/en/docs/guides/error-handling/)
- 📖 [Site Reliability Engineering Book](https://sre.google/books/)
- 🛠️ [Monitoring and Observability](https://www.honeycomb.io/blog/monitoring-vs-observability/)

---

**Prêt pour construire des systèmes ultra-robustes ? Continuez avec les modules de production !** 🏗️
