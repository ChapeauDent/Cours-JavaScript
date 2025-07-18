# Modules et Organisation du Code JavaScript

## **INFORMATIONS GENERALES**

### Metadonnees du cours
- **Titre :** Modules ES6, CommonJS et organisation de projets JavaScript
- **Niveau :** Avance
- **Duree estimee :** 60 minutes
- **Prerequis :** Fonctions, objets, programmation asynchrone, concepts de base Node.js
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## **OBJECTIFS PEDAGOGIQUES**

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : L'importance de la modularite dans les projets JavaScript
- [ ] **Appliquer** : Les modules ES6 (import/export) et CommonJS (require/module.exports)
- [ ] **Analyser** : Les differentes strategies d'organisation de code
- [ ] **Creer** : Une architecture modulaire pour un projet complexe

### Mots-cles a retenir
- **Module** : Fichier JavaScript qui exporte et importe des fonctionnalites
- **Export** : Rendre disponible des variables/fonctions d'un module
- **Import** : Utiliser des fonctionnalites d'autres modules
- **CommonJS** : Systeme de modules utilise par Node.js
- **ES6 Modules** : Standard moderne pour les modules JavaScript

---

## **PARTIE THEORIQUE**

### Pourquoi ce concept est-il important ?
Sans modules, tout votre code JavaScript serait dans le scope global, causant des conflits de noms et rendant la maintenance impossible. Les modules permettent de :
- **Encapsuler** le code en unites logiques
- **Reutiliser** des fonctionnalites dans differents projets
- **Collaborer** efficacement en equipe
- **Maintenir** facilement de gros projets

### Definition et concepts cles

#### 1. Evolution des modules JavaScript
- **Script tags** : Methode primitive avec pollution du scope global
- **IIFE** : Immediately Invoked Function Expression pour l'encapsulation
- **CommonJS** : Standard Node.js (require/module.exports)
- **AMD** : Asynchronous Module Definition (RequireJS)
- **ES6 Modules** : Standard moderne (import/export)

#### 2. ES6 Modules vs CommonJS
```javascript
// ES6 Modules (modern browsers, Node.js 12+)
import { fonction } from './module.js';
export const maVariable = 42;

// CommonJS (Node.js traditionnel)
const { fonction } = require('./module');
module.exports = { maVariable: 42 };
```

#### 3. Types d'exports
- **Named exports** : Exporter plusieurs valeurs nommees
- **Default export** : Exporter une valeur principale
- **Re-exports** : Exporter depuis un autre module

### Syntaxe de base
```javascript
// --- fichier: math-utils.js ---
// Named exports
export const PI = 3.14159;
export function aire(rayon) {
    return PI * rayon * rayon;
}

// Default export
export default class Calculatrice {
    constructor() {
        this.historique = [];
    }
}

// --- fichier: main.js ---
// Named imports
import { PI, aire } from './math-utils.js';

// Default import
import Calculatrice from './math-utils.js';

// Mixed import
import Calculatrice, { PI, aire } from './math-utils.js';

// Import everything
import * as MathUtils from './math-utils.js';
```

### Points d'attention
- **Piege courant 1** : Melanger ES6 modules et CommonJS dans le meme projet
- **Piege courant 2** : Oublier l'extension .js dans les imports ES6
- **Bonne pratique** : Un module = une responsabilite bien definie

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// --- fichier: logger.js ---
// Module utilitaire pour les logs
export const LEVELS = {
    DEBUG: 0,
    INFO: 1,
    WARNING: 2,
    ERROR: 3
};

class Logger {
    constructor(name = 'App') {
        this.name = name;
        this.level = LEVELS.INFO;
    }
    
    setLevel(level) {
        this.level = level;
    }
    
    _log(level, message) {
        if (level >= this.level) {
            const timestamp = new Date().toISOString();
            const levelName = Object.keys(LEVELS)[level];
            console.log(`[${timestamp}] [${this.name}] ${levelName}: ${message}`);
        }
    }
    
    debug(message) { this._log(LEVELS.DEBUG, message); }
    info(message) { this._log(LEVELS.INFO, message); }
    warning(message) { this._log(LEVELS.WARNING, message); }
    error(message) { this._log(LEVELS.ERROR, message); }
}

// Export par defaut
export default Logger;

// --- fichier: app.js ---
import Logger, { LEVELS } from './logger.js';

const logger = new Logger('MyApp');
logger.setLevel(LEVELS.DEBUG);

logger.debug('Application demarree');
logger.info('Utilisateur connecte');
logger.warning('Cache expire');
logger.error('Erreur de connexion');
```

**Resultat attendu :**
```
[2025-07-17T10:30:00.000Z] [MyApp] DEBUG: Application demarree
[2025-07-17T10:30:00.001Z] [MyApp] INFO: Utilisateur connecte
[2025-07-17T10:30:00.002Z] [MyApp] WARNING: Cache expire
[2025-07-17T10:30:00.003Z] [MyApp] ERROR: Erreur de connexion
```

### Exemple intermediaire
```javascript
// --- Structure de projet modulaire ---
// src/
//   config/
//     database.js
//     api.js
//   services/
//     user-service.js
//     notification-service.js
//   utils/
//     validation.js
//     date-utils.js
//   models/
//     user.js
//     notification.js
//   app.js

// --- fichier: src/config/database.js ---
export const databaseConfig = {
    host: process.env.DB_HOST || 'localhost',
    port: process.env.DB_PORT || 5432,
    name: process.env.DB_NAME || 'myapp',
    user: process.env.DB_USER || 'postgres',
    password: process.env.DB_PASSWORD || ''
};

export function createConnectionString() {
    return `postgresql://${databaseConfig.user}:${databaseConfig.password}@${databaseConfig.host}:${databaseConfig.port}/${databaseConfig.name}`;
}

// --- fichier: src/utils/validation.js ---
export class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
    }
}

export const validators = {
    email: (value) => {
        const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!regex.test(value)) {
            throw new ValidationError('Email invalide', 'email');
        }
        return true;
    },
    
    password: (value) => {
        if (!value || value.length < 8) {
            throw new ValidationError('Mot de passe trop court (min 8 caracteres)', 'password');
        }
        if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(value)) {
            throw new ValidationError('Le mot de passe doit contenir au moins une majuscule, une minuscule et un chiffre', 'password');
        }
        return true;
    },
    
    required: (value, fieldName) => {
        if (value === null || value === undefined || value === '') {
            throw new ValidationError(`${fieldName} est requis`, fieldName);
        }
        return true;
    }
};

// --- fichier: src/models/user.js ---
import { validators, ValidationError } from '../utils/validation.js';

export class User {
    constructor(data = {}) {
        this.id = data.id || null;
        this.email = data.email || '';
        this.name = data.name || '';
        this.password = data.password || '';
        this.createdAt = data.createdAt || new Date();
        this.updatedAt = data.updatedAt || new Date();
    }
    
    validate() {
        const errors = [];
        
        try {
            validators.required(this.email, 'email');
            validators.email(this.email);
        } catch (error) {
            errors.push(error);
        }
        
        try {
            validators.required(this.name, 'name');
        } catch (error) {
            errors.push(error);
        }
        
        try {
            validators.required(this.password, 'password');
            validators.password(this.password);
        } catch (error) {
            errors.push(error);
        }
        
        if (errors.length > 0) {
            throw new ValidationError('Erreurs de validation', 'multiple', errors);
        }
        
        return true;
    }
    
    toJSON() {
        // Ne pas exposer le mot de passe
        const { password, ...userData } = this;
        return userData;
    }
}

// --- fichier: src/services/user-service.js ---
import { User } from '../models/user.js';
import { databaseConfig } from '../config/database.js';
import Logger from '../utils/logger.js';

const logger = new Logger('UserService');

export class UserService {
    constructor() {
        this.users = new Map(); // Simulation d'une base de donnees
        this.nextId = 1;
    }
    
    async create(userData) {
        logger.info(`Creation d'un nouvel utilisateur: ${userData.email}`);
        
        try {
            const user = new User({
                ...userData,
                id: this.nextId++
            });
            
            // Validation
            user.validate();
            
            // Verifier l'unicite de l'email
            if (this.findByEmail(user.email)) {
                throw new Error('Un utilisateur avec cet email existe deja');
            }
            
            // Sauvegarder
            this.users.set(user.id, user);
            
            logger.info(`Utilisateur cree avec l'ID: ${user.id}`);
            return user.toJSON();
            
        } catch (error) {
            logger.error(`Erreur lors de la creation: ${error.message}`);
            throw error;
        }
    }
    
    async findById(id) {
        const user = this.users.get(id);
        return user ? user.toJSON() : null;
    }
    
    findByEmail(email) {
        for (const user of this.users.values()) {
            if (user.email === email) {
                return user.toJSON();
            }
        }
        return null;
    }
    
    async update(id, updateData) {
        const user = this.users.get(id);
        if (!user) {
            throw new Error('Utilisateur non trouve');
        }
        
        // Mettre a jour les proprietes
        Object.assign(user, updateData, { updatedAt: new Date() });
        user.validate();
        
        logger.info(`Utilisateur ${id} mis a jour`);
        return user.toJSON();
    }
    
    async delete(id) {
        if (this.users.delete(id)) {
            logger.info(`Utilisateur ${id} supprime`);
            return true;
        }
        throw new Error('Utilisateur non trouve');
    }
    
    async list() {
        return Array.from(this.users.values()).map(user => user.toJSON());
    }
}

// Export d'une instance singleton
export default new UserService();

// --- fichier: src/app.js ---
import userService from './services/user-service.js';
import Logger from './utils/logger.js';

const logger = new Logger('App');

async function demonstrationApp() {
    try {
        logger.info('Demarrage de l\'application');
        
        // Creer des utilisateurs
        const user1 = await userService.create({
            email: 'alice@example.com',
            name: 'Alice Dupont',
            password: 'Password123'
        });
        
        const user2 = await userService.create({
            email: 'bob@example.com',
            name: 'Bob Martin',
            password: 'SecurePass456'
        });
        
        console.log('Utilisateurs crees:', { user1, user2 });
        
        // Lister les utilisateurs
        const users = await userService.list();
        console.log('Liste des utilisateurs:', users);
        
        // Mettre a jour un utilisateur
        const updatedUser = await userService.update(user1.id, {
            name: 'Alice Dupont-Smith'
        });
        console.log('Utilisateur mis a jour:', updatedUser);
        
    } catch (error) {
        logger.error(`Erreur application: ${error.message}`);
    }
}

demonstrationApp();
```

### Exemple avance (optionnel)
```javascript
// --- Systeme de plugins modulaire ---

// --- fichier: src/core/plugin-system.js ---
export class PluginSystem {
    constructor() {
        this.plugins = new Map();
        this.hooks = new Map();
        this.middleware = [];
    }
    
    // Enregistrer un plugin
    register(name, plugin) {
        if (this.plugins.has(name)) {
            throw new Error(`Plugin ${name} deja enregistre`);
        }
        
        // Valider l'interface du plugin
        this.validatePlugin(plugin);
        
        this.plugins.set(name, plugin);
        
        // Initialiser le plugin
        if (plugin.init) {
            plugin.init(this);
        }
        
        console.log(`Plugin ${name} enregistre`);
    }
    
    validatePlugin(plugin) {
        if (typeof plugin !== 'object') {
            throw new Error('Le plugin doit etre un objet');
        }
        if (typeof plugin.name !== 'string') {
            throw new Error('Le plugin doit avoir une propriete name');
        }
        if (typeof plugin.version !== 'string') {
            throw new Error('Le plugin doit avoir une propriete version');
        }
    }
    
    // Systeme de hooks pour extensibilite
    addHook(eventName, callback) {
        if (!this.hooks.has(eventName)) {
            this.hooks.set(eventName, []);
        }
        this.hooks.get(eventName).push(callback);
    }
    
    async executeHook(eventName, ...args) {
        const callbacks = this.hooks.get(eventName) || [];
        const results = [];
        
        for (const callback of callbacks) {
            try {
                const result = await callback(...args);
                results.push(result);
            } catch (error) {
                console.error(`Erreur dans hook ${eventName}:`, error);
            }
        }
        
        return results;
    }
    
    // Middleware pour pipeline de traitement
    use(middleware) {
        if (typeof middleware !== 'function') {
            throw new Error('Middleware doit etre une fonction');
        }
        this.middleware.push(middleware);
    }
    
    async execute(data) {
        let result = data;
        
        for (const middleware of this.middleware) {
            try {
                result = await middleware(result, this);
            } catch (error) {
                console.error('Erreur middleware:', error);
                throw error;
            }
        }
        
        return result;
    }
    
    // Obtenir informations sur les plugins
    getPluginInfo() {
        return Array.from(this.plugins.entries()).map(([name, plugin]) => ({
            name,
            version: plugin.version,
            description: plugin.description || 'Aucune description'
        }));
    }
}

// --- fichier: src/plugins/logger-plugin.js ---
export default {
    name: 'logger',
    version: '1.0.0',
    description: 'Plugin de logging avance',
    
    init(pluginSystem) {
        // Ajouter middleware de logging
        pluginSystem.use(async (data, system) => {
            console.log('Middleware Logger - Donnees recues:', data);
            
            // Executer hook de pre-traitement
            await system.executeHook('beforeProcess', data);
            
            return data;
        });
        
        // Ajouter hook pour logging des erreurs
        pluginSystem.addHook('error', (error) => {
            console.error('Erreur capturee par le plugin logger:', error);
        });
    }
};

// --- fichier: src/plugins/validation-plugin.js ---
export default {
    name: 'validation',
    version: '1.0.0',
    description: 'Plugin de validation des donnees',
    
    init(pluginSystem) {
        pluginSystem.use(async (data) => {
            if (!data || typeof data !== 'object') {
                throw new Error('Donnees invalides');
            }
            
            // Validation basique
            if (data.email && !data.email.includes('@')) {
                throw new Error('Email invalide');
            }
            
            console.log('Validation reussie');
            return data;
        });
    }
};

// --- fichier: src/plugins/cache-plugin.js ---
export default {
    name: 'cache',
    version: '1.0.0',
    description: 'Plugin de mise en cache',
    
    init(pluginSystem) {
        const cache = new Map();
        
        pluginSystem.use(async (data) => {
            const key = JSON.stringify(data);
            
            // Verifier le cache
            if (cache.has(key)) {
                console.log('Donnees trouvees en cache');
                return cache.get(key);
            }
            
            // Mettre en cache pour la prochaine fois
            cache.set(key, data);
            console.log('Donnees mises en cache');
            
            return data;
        });
    }
};

// --- fichier: src/app-with-plugins.js ---
import { PluginSystem } from './core/plugin-system.js';
import loggerPlugin from './plugins/logger-plugin.js';
import validationPlugin from './plugins/validation-plugin.js';
import cachePlugin from './plugins/cache-plugin.js';

async function demonstrationPlugins() {
    const system = new PluginSystem();
    
    // Enregistrer les plugins
    system.register('logger', loggerPlugin);
    system.register('validation', validationPlugin);
    system.register('cache', cachePlugin);
    
    console.log('Plugins enregistres:', system.getPluginInfo());
    
    try {
        // Traiter des donnees a travers le pipeline
        const data1 = await system.execute({
            email: 'test@example.com',
            name: 'Test User'
        });
        
        // Deuxieme appel - devrait utiliser le cache
        const data2 = await system.execute({
            email: 'test@example.com',
            name: 'Test User'
        });
        
        console.log('Traitement termine:', { data1, data2 });
        
    } catch (error) {
        await system.executeHook('error', error);
    }
}

demonstrationPlugins();
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Systeme de configuration modulaire
**Consigne :** Creez un systeme de configuration qui peut charger des parametres depuis differentes sources (fichiers JSON, variables d'environnement, arguments CLI).

**Code de depart :**
```javascript
// Structure attendue:
// config/
//   base-config.js
//   env-config.js
//   file-config.js
//   config-manager.js

class ConfigManager {
    constructor() {
        // TODO: Implementer le gestionnaire de configuration
    }
    
    async load() {
        // TODO: Charger configuration depuis toutes les sources
    }
    
    get(key) {
        // TODO: Obtenir une valeur de configuration
    }
    
    set(key, value) {
        // TODO: Definir une valeur de configuration
    }
}

export default ConfigManager;
```

**Niveau de difficulte :** ⭐⭐⭐☆☆

### Exercice 2 : Systeme de modules avec lazy loading
**Consigne :** Implementez un systeme qui charge les modules seulement quand ils sont necessaires (lazy loading).

**Contraintes :**
- Cache des modules charges
- Gestion des dependances circulaires
- Interface Promise-based

**Niveau de difficulte :** ⭐⭐⭐⭐☆

### Exercice 3 : Micro-framework modulaire (optionnel)
**Consigne :** Creez un mini-framework web modulaire avec systeme de routage, middleware et plugins.

**Niveau de difficulte :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// --- fichier: config/base-config.js ---
export const baseConfig = {
    app: {
        name: 'MonApp',
        version: '1.0.0',
        env: 'development'
    },
    server: {
        port: 3000,
        host: 'localhost'
    },
    database: {
        type: 'postgresql',
        host: 'localhost',
        port: 5432
    },
    features: {
        enableCache: true,
        enableLogs: true,
        maxConnections: 100
    }
};

// --- fichier: config/env-config.js ---
export function loadEnvConfig() {
    const envConfig = {};
    
    // Mapper les variables d'environnement
    const envMappings = {
        'APP_NAME': 'app.name',
        'APP_ENV': 'app.env',
        'SERVER_PORT': 'server.port',
        'SERVER_HOST': 'server.host',
        'DB_HOST': 'database.host',
        'DB_PORT': 'database.port',
        'ENABLE_CACHE': 'features.enableCache',
        'MAX_CONNECTIONS': 'features.maxConnections'
    };
    
    for (const [envVar, configPath] of Object.entries(envMappings)) {
        const value = process.env[envVar];
        if (value !== undefined) {
            setNestedValue(envConfig, configPath, parseValue(value));
        }
    }
    
    return envConfig;
}

function setNestedValue(obj, path, value) {
    const keys = path.split('.');
    let current = obj;
    
    for (let i = 0; i < keys.length - 1; i++) {
        const key = keys[i];
        if (!(key in current)) {
            current[key] = {};
        }
        current = current[key];
    }
    
    current[keys[keys.length - 1]] = value;
}

function parseValue(value) {
    // Tenter de parser comme JSON pour les booleans/numbers
    if (value === 'true') return true;
    if (value === 'false') return false;
    if (/^\d+$/.test(value)) return parseInt(value, 10);
    if (/^\d+\.\d+$/.test(value)) return parseFloat(value);
    return value;
}

// --- fichier: config/file-config.js ---
import { promises as fs } from 'fs';
import path from 'path';

export class FileConfigLoader {
    constructor(configDir = './config') {
        this.configDir = configDir;
    }
    
    async loadJSON(filename) {
        try {
            const filePath = path.join(this.configDir, filename);
            const content = await fs.readFile(filePath, 'utf8');
            return JSON.parse(content);
        } catch (error) {
            if (error.code === 'ENOENT') {
                console.warn(`Fichier de configuration ${filename} non trouve`);
                return {};
            }
            throw error;
        }
    }
    
    async loadAllJSON() {
        const configs = {};
        const configFiles = ['app.json', 'database.json', 'features.json'];
        
        for (const file of configFiles) {
            const name = path.basename(file, '.json');
            configs[name] = await this.loadJSON(file);
        }
        
        return configs;
    }
}

// --- fichier: config/config-manager.js ---
import { baseConfig } from './base-config.js';
import { loadEnvConfig } from './env-config.js';
import { FileConfigLoader } from './file-config.js';

export class ConfigManager {
    constructor(configDir) {
        this.config = {};
        this.fileLoader = new FileConfigLoader(configDir);
        this.loaded = false;
    }
    
    async load() {
        if (this.loaded) {
            return this.config;
        }
        
        try {
            // 1. Charger la configuration de base
            this.config = this.deepClone(baseConfig);
            console.log('Configuration de base chargee');
            
            // 2. Charger les fichiers de configuration
            const fileConfigs = await this.fileLoader.loadAllJSON();
            for (const [section, config] of Object.entries(fileConfigs)) {
                this.mergeConfig(this.config, { [section]: config });
            }
            console.log('Fichiers de configuration charges');
            
            // 3. Charger les variables d'environnement (priorite la plus haute)
            const envConfig = loadEnvConfig();
            this.mergeConfig(this.config, envConfig);
            console.log('Variables d\'environnement chargees');
            
            this.loaded = true;
            console.log('Configuration finale:', this.config);
            
        } catch (error) {
            console.error('Erreur lors du chargement de la configuration:', error);
            throw error;
        }
        
        return this.config;
    }
    
    get(key) {
        if (!this.loaded) {
            throw new Error('Configuration non chargee. Appelez load() d\'abord.');
        }
        
        return this.getNestedValue(this.config, key);
    }
    
    set(key, value) {
        if (!this.loaded) {
            throw new Error('Configuration non chargee. Appelez load() d\'abord.');
        }
        
        this.setNestedValue(this.config, key, value);
    }
    
    getNestedValue(obj, path) {
        return path.split('.').reduce((current, key) => {
            return current && current[key] !== undefined ? current[key] : undefined;
        }, obj);
    }
    
    setNestedValue(obj, path, value) {
        const keys = path.split('.');
        let current = obj;
        
        for (let i = 0; i < keys.length - 1; i++) {
            const key = keys[i];
            if (!(key in current) || typeof current[key] !== 'object') {
                current[key] = {};
            }
            current = current[key];
        }
        
        current[keys[keys.length - 1]] = value;
    }
    
    mergeConfig(target, source) {
        for (const key in source) {
            if (source[key] && typeof source[key] === 'object' && !Array.isArray(source[key])) {
                if (!target[key]) target[key] = {};
                this.mergeConfig(target[key], source[key]);
            } else {
                target[key] = source[key];
            }
        }
    }
    
    deepClone(obj) {
        return JSON.parse(JSON.stringify(obj));
    }
    
    // Obtenir toute la configuration
    getAll() {
        return this.deepClone(this.config);
    }
    
    // Valider la configuration
    validate() {
        const required = [
            'app.name',
            'server.port',
            'database.host'
        ];
        
        const missing = required.filter(key => this.get(key) === undefined);
        
        if (missing.length > 0) {
            throw new Error(`Configuration manquante: ${missing.join(', ')}`);
        }
        
        return true;
    }
}

// Export d'une instance singleton
export default new ConfigManager();

// --- fichier: exemple-utilisation.js ---
import configManager from './config/config-manager.js';

async function demonstrationConfig() {
    try {
        // Charger la configuration
        await configManager.load();
        
        // Valider
        configManager.validate();
        
        // Utiliser la configuration
        console.log('Nom de l\'app:', configManager.get('app.name'));
        console.log('Port serveur:', configManager.get('server.port'));
        console.log('Cache active:', configManager.get('features.enableCache'));
        
        // Modifier une valeur
        configManager.set('server.port', 8080);
        console.log('Nouveau port:', configManager.get('server.port'));
        
        // Obtenir toute la configuration
        const fullConfig = configManager.getAll();
        console.log('Configuration complete:', fullConfig);
        
    } catch (error) {
        console.error('Erreur configuration:', error.message);
    }
}

demonstrationConfig();
```

**Explication :** Cette solution implemente un systeme de configuration flexible avec priorites (base < fichiers < environnement) et gestion des valeurs imbriquees.

### Solution Exercice 2
```javascript
// --- fichier: lazy-module-loader.js ---
export class LazyModuleLoader {
    constructor() {
        this.moduleCache = new Map();
        this.loadingPromises = new Map();
        this.dependencyGraph = new Map();
    }
    
    // Enregistrer un module avec ses dependances
    register(name, moduleFactory, dependencies = []) {
        this.dependencyGraph.set(name, {
            factory: moduleFactory,
            dependencies,
            loaded: false
        });
    }
    
    // Charger un module avec ses dependances
    async load(moduleName) {
        // Verifier si deja en cache
        if (this.moduleCache.has(moduleName)) {
            return this.moduleCache.get(moduleName);
        }
        
        // Verifier si en cours de chargement
        if (this.loadingPromises.has(moduleName)) {
            return await this.loadingPromises.get(moduleName);
        }
        
        // Demarrer le chargement
        const loadingPromise = this._loadModule(moduleName);
        this.loadingPromises.set(moduleName, loadingPromise);
        
        try {
            const module = await loadingPromise;
            this.moduleCache.set(moduleName, module);
            return module;
        } finally {
            this.loadingPromises.delete(moduleName);
        }
    }
    
    async _loadModule(moduleName) {
        const moduleInfo = this.dependencyGraph.get(moduleName);
        if (!moduleInfo) {
            throw new Error(`Module ${moduleName} non enregistre`);
        }
        
        // Detecter les dependances circulaires
        this._checkCircularDependency(moduleName, new Set());
        
        // Charger les dependances d'abord
        const dependencies = {};
        for (const depName of moduleInfo.dependencies) {
            dependencies[depName] = await this.load(depName);
        }
        
        // Executer la factory function
        console.log(`Chargement du module: ${moduleName}`);
        const module = await moduleInfo.factory(dependencies);
        
        moduleInfo.loaded = true;
        return module;
    }
    
    _checkCircularDependency(moduleName, visiting, visited = new Set()) {
        if (visiting.has(moduleName)) {
            throw new Error(`Dependance circulaire detectee: ${Array.from(visiting).join(' -> ')} -> ${moduleName}`);
        }
        
        if (visited.has(moduleName)) {
            return;
        }
        
        const moduleInfo = this.dependencyGraph.get(moduleName);
        if (!moduleInfo) return;
        
        visiting.add(moduleName);
        
        for (const dependency of moduleInfo.dependencies) {
            this._checkCircularDependency(dependency, visiting, visited);
        }
        
        visiting.delete(moduleName);
        visited.add(moduleName);
    }
    
    // Obtenir les statistiques
    getStats() {
        const totalModules = this.dependencyGraph.size;
        const loadedModules = this.moduleCache.size;
        const loadingModules = this.loadingPromises.size;
        
        return {
            total: totalModules,
            loaded: loadedModules,
            loading: loadingModules,
            cached: Array.from(this.moduleCache.keys()),
            registered: Array.from(this.dependencyGraph.keys())
        };
    }
    
    // Vider le cache
    clearCache() {
        this.moduleCache.clear();
        console.log('Cache des modules vide');
    }
}

// --- Demonstration d'utilisation ---
async function demonstrationLazyLoading() {
    const loader = new LazyModuleLoader();
    
    // Enregistrer des modules avec dependances
    loader.register('logger', async () => {
        console.log('Factory: Creation du logger');
        return {
            log: (message) => console.log(`[LOG] ${message}`)
        };
    });
    
    loader.register('database', async (deps) => {
        console.log('Factory: Creation de la base de donnees');
        deps.logger.log('Connexion a la base de donnees');
        return {
            query: (sql) => deps.logger.log(`SQL: ${sql}`)
        };
    }, ['logger']);
    
    loader.register('userService', async (deps) => {
        console.log('Factory: Creation du service utilisateur');
        deps.logger.log('Service utilisateur initialise');
        return {
            createUser: (name) => {
                deps.logger.log(`Creation utilisateur: ${name}`);
                deps.database.query(`INSERT INTO users (name) VALUES ('${name}')`);
            }
        };
    }, ['logger', 'database']);
    
    try {
        console.log('Stats initiales:', loader.getStats());
        
        // Charger seulement le service utilisateur
        // Les dependances seront chargees automatiquement
        const userService = await loader.load('userService');
        
        console.log('Stats apres chargement:', loader.getStats());
        
        // Utiliser le service
        userService.createUser('Alice');
        
        // Deuxieme chargement - utilise le cache
        const userService2 = await loader.load('userService');
        console.log('Meme instance?', userService === userService2);
        
    } catch (error) {
        console.error('Erreur:', error.message);
    }
}

demonstrationLazyLoading();
```

**Alternatives possibles :**
- Implementer un timeout pour les modules qui ne se chargent pas
- Ajouter un systeme de versions pour les modules

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts prealables utilises
- **Fonctions** : Les modules exportent principalement des fonctions
- **Objets** : Structure des exports et imports
- **Programmation asynchrone** : Chargement dynamique de modules

### Prochaines etapes
- **Bundlers** : Webpack, Vite pour optimiser les modules
- **Package managers** : npm, yarn pour gerer les dependances
- **Testing** : Tests unitaires pour modules isoles

### Concepts complementaires
- **Design patterns** : Module pattern, Singleton pattern
- **Architecture** : Separation of concerns, DRY principle

---

## **PROJET MINI**

### Defi pratique
**Objectif :** Creer un systeme de gestion de bibliotheque modulaire avec interface en ligne de commande

**Fonctionnalites requises :**
- [ ] Modules separes pour livres, auteurs, emprunts
- [ ] Systeme de configuration multi-sources
- [ ] Interface CLI avec commandes modulaires
- [ ] Persistance des donnees avec modules de stockage interchangeables

**Structure de fichiers suggeree :**
```
📁 bibliotheque-app/
├── 📁 src/
│   ├── 📁 models/
│   ├── 📁 services/
│   ├── 📁 cli/
│   ├── 📁 storage/
│   └── 📁 config/
├── 📄 package.json
└── 📄 main.js
```

**Temps estime :** 150 minutes

---

## **RESSOURCES COMPLEMENTAIRES**

### Documentation officielle
- [MDN - JavaScript Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [Node.js - Modules](https://nodejs.org/api/modules.html)
- [ES6 Modules Specification](https://tc39.es/ecma262/#sec-modules)

### Videos recommandees
- "JavaScript Modules - ES6 Import Export" - The Net Ninja (15 min)
- "Node.js Module System" - Academind (20 min)
- Pourquoi ces videos sont utiles : Demonstrations pratiques et comparaisons

### Articles approfondis
- "ES6 Modules vs CommonJS" - JavaScript.info
- "Module Bundling Explained" - Webpack docs
- Ce que ces articles apportent en plus : Strategies d'optimisation et best practices

### Outils de pratique
- [ES6 Module Examples](https://github.com/mdn/js-examples/tree/master/modules)
- [Node.js Module Patterns](https://nodejs.org/api/modules.html#modules_the_module_wrapper)

---

## **AUTO-EVALUATION**

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference principale entre require() et import ?
   - Reponse : require() est synchrone et dynamique, import est statique et permet l'analyse statique

2. **Question pratique :** Que fait ce code ?
   ```javascript
   export { default as UserService } from './user-service.js';
   export * from './validation.js';
   ```
   - Reponse : Re-exporte le default export de user-service.js et tous les exports de validation.js

3. **Question d'application :** Quand utiliseriez-vous un default export vs named exports ?
   - Reponse : Default export pour l'export principal du module, named exports pour plusieurs fonctionnalites

### Checklist de maitrise
- [ ] Je comprends les differents systemes de modules JavaScript
- [ ] Je sais organiser un projet en modules logiques
- [ ] Je peux gerer les dependances entre modules
- [ ] Je comprends l'importance de l'encapsulation
- [ ] Je peux creer des modules reutilisables
- [ ] Je sais optimiser l'organisation du code

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur frequente 1
**Code problematique :**
```javascript
// main.js
import { myFunction } from './module'; // Oubli de l'extension
```

**Message d'erreur :** `Error: Cannot resolve module './module'`

**Solution :**
```javascript
// main.js
import { myFunction } from './module.js'; // Avec extension
```

**Explication :** Les modules ES6 requirent l'extension de fichier explicite dans les navigateurs et Node.js recent.

### Erreur frequente 2
**Code problematique :**
```javascript
// module.js
const data = require('./data.json'); // CommonJS
export default data; // ES6
```

**Probleme :** Melange de CommonJS et ES6 modules

**Solution :**
```javascript
// module.js
import data from './data.json' assert { type: 'json' }; // ES6
export default data;
```

**Explication :** Ne pas melanger les syntaxes de modules dans le meme fichier.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- L'importance de la modularite pour la maintenance du code
- Les differences entre CommonJS et ES6 modules
- Comment organiser efficacement un projet JavaScript

### Questions a approfondir
- Module bundling et tree shaking
- Dynamic imports pour le code splitting

### Revision programmee
- [ ] Revision dans 1 jour (rappel rapide)
- [ ] Revision dans 1 semaine (exercices)
- [ ] Revision dans 1 mois (application dans un projet)

---

## **TAGS ET METADONNEES**

**Mots-cles :** #javascript #modules #es6 #commonjs #organisation #architecture #avance

**Temps de completion :** [Temps reellement passe]

**Difficulte ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Pret pour le concept suivant :** ✅ Oui / ❌ Non (revision necessaire)

---

*📅 Fiche creee le : 17/07/2025*
*🔄 Derniere revision : 17/07/2025*
*📍 Position dans la roadmap : 14/126*
