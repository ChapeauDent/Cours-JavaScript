# 📦 Modules et Organisation du Code JavaScript

> **Niveau :** 🔴 Avancé | **Durée :** 60 minutes | **Prérequis :** Fonctions, Classes, Async/Await

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** les modules ES6 et CommonJS
- [ ] **Organiser** efficacement des projets complexes  
- [ ] **Concevoir** des architectures modulaires scalables
- [ ] **Optimiser** les performances de chargement des modules

---

## 📚 Introduction

{% tabs %}
{% tab title="🧠 Pourquoi les modules ?" %}
**Sans modules, votre code devient :**
- 🚫 **Ingérable** : Tout dans le scope global
- 🚫 **Fragile** : Conflits de noms et dépendances
- 🚫 **Non réutilisable** : Code dupliqué partout
- 🚫 **Difficile à tester** : Pas d'isolation
- 🚫 **Impossible à maintenir** : Effet boule de neige

**Avec les modules :**
- ✅ **Encapsulation** : Code organisé en unités logiques
- ✅ **Réutilisabilité** : Composants réutilisables
- ✅ **Maintenabilité** : Structure claire et évolutive
- ✅ **Collaboration** : Travail en équipe facilité
- ✅ **Performance** : Chargement optimisé
{% endtab %}

{% tab title="🔄 Évolution historique" %}
```javascript
// 1️⃣ Script tags (années 2000) - CHAOS GLOBAL
<script src="jquery.js"></script>
<script src="app.js"></script>
// Tout dans window.* = conflits garantis

// 2️⃣ IIFE (2010s) - ISOLATION MANUELLE  
(function() {
    var maVariable = 'privée';
    window.MonModule = { /* API publique */ };
})();

// 3️⃣ CommonJS (Node.js) - SERVEUR
const fs = require('fs');
module.exports = { maFonction };

// 4️⃣ AMD/RequireJS (2011) - CLIENT
define(['dependency'], function(dep) {
    return { myModule: dep };
});

// 5️⃣ ES6 Modules (2015+) - STANDARD MODERNE
import { maFonction } from './module.js';
export const maVariable = 42;
```
{% endtab %}

{% tab title="🆚 ES6 vs CommonJS" %}
| Aspect | ES6 Modules | CommonJS |
|--------|-------------|----------|
| **Syntaxe** | `import/export` | `require/module.exports` |
| **Chargement** | Statique (compile-time) | Dynamique (runtime) |
| **Performance** | Tree-shaking possible | Pas d'optimisation |
| **Hoisting** | Oui, imports hissés | Non |
| **Scope** | Module strict mode | Contexte Node.js |
| **Compatibilité** | Moderne (2015+) | Node.js historique |
| **Asynchrone** | `import()` dynamique | Synchrone uniquement |

```javascript
// ES6 - Statique et optimisable
import { fonction } from './module.js'; // Analysé à la compilation

// CommonJS - Dynamique et flexible  
const module = require('./module'); // Résolu à l'exécution
```
{% endtab %}
{% endtabs %}

---

## 🚀 ES6 Modules

### Syntaxes d'export

{% tabs %}
{% tab title="📤 Named Exports" %}
```javascript
// 📁 math-utils.js
// Export inline
export const PI = 3.14159;
export const E = 2.71828;

export function aire(rayon) {
    return PI * rayon * rayon;
}

export function perimetre(rayon) {
    return 2 * PI * rayon;
}

export class Calculatrice {
    constructor() {
        this.historique = [];
    }
    
    calculer(expression) {
        const resultat = eval(expression); // ⚠️ Attention en production !
        this.historique.push({ expression, resultat });
        return resultat;
    }
    
    obtenirHistorique() {
        return [...this.historique];
    }
}

// Export en fin de fichier
function puissance(base, exposant) {
    return Math.pow(base, exposant);
}

function racine(nombre) {
    return Math.sqrt(nombre);
}

export { puissance, racine };

// Export avec alias
function calculComplexe(x, y) {
    return x * y + Math.random();
}

export { calculComplexe as calcul };
```

```javascript
// 📁 app.js - Utilisation des named exports
import { PI, aire, Calculatrice } from './math-utils.js';

console.log('PI =', PI); // 3.14159
console.log('Aire =', aire(5)); // 78.54

const calc = new Calculatrice();
console.log(calc.calculer('2 + 3')); // 5

// Import avec alias
import { calcul as calculAvance } from './math-utils.js';

// Import sélectif
import { puissance, racine } from './math-utils.js';

// Import everything
import * as Math from './math-utils.js';
console.log(Math.PI, Math.aire(3));
```
{% endtab %}

{% tab title="🎯 Default Export" %}
```javascript
// 📁 user-service.js
class UserService {
    constructor(apiUrl) {
        this.apiUrl = apiUrl;
        this.cache = new Map();
    }
    
    async obtenirUtilisateur(id) {
        if (this.cache.has(id)) {
            return this.cache.get(id);
        }
        
        const response = await fetch(`${this.apiUrl}/users/${id}`);
        const utilisateur = await response.json();
        
        this.cache.set(id, utilisateur);
        return utilisateur;
    }
    
    async creerUtilisateur(donnees) {
        const response = await fetch(`${this.apiUrl}/users`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(donnees)
        });
        
        return response.json();
    }
    
    viderCache() {
        this.cache.clear();
    }
}

// Default export
export default UserService;

// Peut aussi s'écrire :
// export { UserService as default };
```

```javascript
// 📁 app.js - Import du default export
// Import sans accolades pour le default
import UserService from './user-service.js';

// Peut être renommé librement
import Service from './user-service.js';
import MonService from './user-service.js';

// Utilisation
const userService = new UserService('https://api.example.com');
const utilisateur = await userService.obtenirUtilisateur('123');

// Mix default + named exports
// 📁 module-mixte.js
export default class MaClasse {}
export const CONSTANTE = 42;
export function utilitaire() {}

// 📁 import-mixte.js
import MaClasse, { CONSTANTE, utilitaire } from './module-mixte.js';
```
{% endtab %}

{% tab title="🔄 Re-exports" %}
```javascript
// 📁 services/user-service.js
export class UserService {}

// 📁 services/product-service.js  
export class ProductService {}

// 📁 services/order-service.js
export class OrderService {}

// 📁 services/index.js - Point d'entrée centralisé
// Re-export everything
export { UserService } from './user-service.js';
export { ProductService } from './product-service.js';
export { OrderService } from './order-service.js';

// Re-export avec alias
export { UserService as Users } from './user-service.js';

// Re-export default as named
export { default as EmailService } from './email-service.js';

// Re-export tout d'un module
export * from './auth-service.js';

// Default re-export
export { UserService as default } from './user-service.js';
```

```javascript
// 📁 app.js - Import simplifié
// Avant (sans re-exports)
import { UserService } from './services/user-service.js';
import { ProductService } from './services/product-service.js';
import { OrderService } from './services/order-service.js';

// Après (avec re-exports depuis index.js)
import { 
    UserService, 
    ProductService, 
    OrderService 
} from './services/index.js';

// Ou plus court
import { UserService, ProductService, OrderService } from './services';
```
{% endtab %}
{% endtabs %}

### Imports dynamiques

{% tabs %}
{% tab title="⚡ Import() dynamique" %}
```javascript
// Import conditionnel
async function chargerModule(condition) {
    if (condition) {
        // Import uniquement si nécessaire
        const { ModuleLourd } = await import('./module-lourd.js');
        return new ModuleLourd();
    }
    return null;
}

// Lazy loading pour optimiser les performances
class Application {
    constructor() {
        this.modules = new Map();
    }
    
    async chargerModuleSiNecessaire(nom) {
        if (!this.modules.has(nom)) {
            console.log(`🔄 Chargement du module ${nom}...`);
            
            let module;
            switch (nom) {
                case 'charts':
                    module = await import('./modules/charts.js');
                    break;
                case 'dashboard':
                    module = await import('./modules/dashboard.js');
                    break;
                case 'reports':
                    module = await import('./modules/reports.js');
                    break;
                default:
                    throw new Error(`Module ${nom} inconnu`);
            }
            
            this.modules.set(nom, module);
            console.log(`✅ Module ${nom} chargé !`);
        }
        
        return this.modules.get(nom);
    }
    
    async afficherGraphiques(donnees) {
        const { CreateurGraphiques } = await this.chargerModuleSiNecessaire('charts');
        const createur = new CreateurGraphiques();
        return createur.creer(donnees);
    }
}

// Chargement en fonction de la route
class Router {
    async naviguer(route) {
        try {
            console.log(`🧭 Navigation vers ${route}`);
            
            // Import dynamique basé sur la route
            const modulePath = `./pages/${route}.js`;
            const module = await import(modulePath);
            
            // Le module doit exposer une fonction render
            if (typeof module.render === 'function') {
                module.render();
            } else {
                console.error(`Module ${route} n'expose pas de fonction render`);
            }
            
        } catch (erreur) {
            console.error(`Erreur de navigation vers ${route}:`, erreur);
            
            // Fallback vers page d'erreur
            const { render } = await import('./pages/404.js');
            render();
        }
    }
}

// Utilisation avec gestion d'erreur
async function exempleImportDynamique() {
    try {
        // Import conditionnel avec détection de fonctionnalité
        if ('IntersectionObserver' in window) {
            const { LazyLoader } = await import('./lazy-loader.js');
            new LazyLoader().init();
        }
        
        // Import avec timeout
        const timeoutPromise = new Promise((_, reject) =>
            setTimeout(() => reject(new Error('Timeout')), 5000)
        );
        
        const modulePromise = import('./module-externe.js');
        const module = await Promise.race([modulePromise, timeoutPromise]);
        
        console.log('Module chargé avec succès');
        
    } catch (erreur) {
        console.error('Échec du chargement:', erreur);
        // Utiliser un module de fallback
        const { FallbackModule } = await import('./fallback.js');
        new FallbackModule().init();
    }
}
```
{% endtab %}

{% tab title="📊 Monitoring des imports" %}
```javascript
// Gestionnaire avancé d'imports dynamiques
class ModuleManager {
    constructor() {
        this.modules = new Map();
        this.loading = new Map(); // Pour éviter les imports multiples
        this.stats = {
            totalImports: 0,
            tempsChargement: new Map(),
            erreurs: []
        };
    }
    
    async importer(nom, chemin, options = {}) {
        // Éviter les imports simultanés du même module
        if (this.loading.has(nom)) {
            return this.loading.get(nom);
        }
        
        // Module déjà chargé
        if (this.modules.has(nom)) {
            return this.modules.get(nom);
        }
        
        const debut = performance.now();
        
        const promiseImport = this._effectuerImport(nom, chemin, options)
            .finally(() => {
                this.loading.delete(nom);
                const duree = performance.now() - debut;
                this.stats.tempsChargement.set(nom, duree);
                console.log(`📊 Module ${nom} chargé en ${duree.toFixed(2)}ms`);
            });
        
        this.loading.set(nom, promiseImport);
        return promiseImport;
    }
    
    async _effectuerImport(nom, chemin, options) {
        try {
            this.stats.totalImports++;
            
            // Préchargement optionnel
            if (options.preload) {
                await this._precharger(chemin);
            }
            
            console.log(`🔄 Import de ${nom} depuis ${chemin}...`);
            
            const module = await import(chemin);
            
            // Validation du module
            if (options.validator && !options.validator(module)) {
                throw new Error(`Module ${nom} ne respecte pas le contrat attendu`);
            }
            
            this.modules.set(nom, module);
            
            console.log(`✅ Module ${nom} importé avec succès`);
            return module;
            
        } catch (erreur) {
            console.error(`❌ Échec import ${nom}:`, erreur);
            this.stats.erreurs.push({ nom, erreur, timestamp: Date.now() });
            
            // Module de fallback
            if (options.fallback) {
                console.log(`🔄 Tentative de fallback pour ${nom}...`);
                return this.importer(`${nom}-fallback`, options.fallback);
            }
            
            throw erreur;
        }
    }
    
    async _precharger(chemin) {
        // Utilise <link rel="modulepreload"> si disponible
        if (typeof document !== 'undefined') {
            const link = document.createElement('link');
            link.rel = 'modulepreload';
            link.href = chemin;
            document.head.appendChild(link);
            
            // Attendre un peu pour que le preload soit effectif
            await new Promise(resolve => setTimeout(resolve, 100));
        }
    }
    
    obtenirStatistiques() {
        const tempsTotal = Array.from(this.stats.tempsChargement.values())
            .reduce((acc, temps) => acc + temps, 0);
        
        const tempsMoyen = this.stats.totalImports > 0 
            ? tempsTotal / this.stats.totalImports 
            : 0;
        
        return {
            modulesCharges: this.modules.size,
            totalImports: this.stats.totalImports,
            tempsMoyenChargement: Math.round(tempsMoyen * 100) / 100,
            tempsTotal: Math.round(tempsTotal * 100) / 100,
            erreursImport: this.stats.erreurs.length,
            modulesEnCache: Array.from(this.modules.keys())
        };
    }
    
    // Nettoyage et optimisation
    viderCache(pattern = null) {
        if (pattern) {
            const regex = new RegExp(pattern);
            for (const [nom] of this.modules) {
                if (regex.test(nom)) {
                    this.modules.delete(nom);
                }
            }
        } else {
            this.modules.clear();
        }
    }
    
    // Preload intelligent basé sur les patterns d'usage
    async prechargerModulesFrequents() {
        const modulesFrequents = [
            { nom: 'utils', chemin: './utils/index.js' },
            { nom: 'components', chemin: './components/index.js' }
        ];
        
        const promises = modulesFrequents.map(({ nom, chemin }) =>
            this.importer(nom, chemin, { preload: true })
                .catch(err => console.warn(`Préchargement échoué pour ${nom}:`, err))
        );
        
        await Promise.allSettled(promises);
    }
}

// Instance globale
const moduleManager = new ModuleManager();

// Export pour utilisation dans d'autres modules
export default moduleManager;
```
{% endtab %}
{% endtabs %}

### 🏃‍♂️ Exercice Pratique : Système de plugins dynamiques

{% hint style="warning" %}
**Exercice :** Créez un système de plugins avec chargement dynamique et hot-reload
{% endhint %}

```javascript
// Complétez ce système de plugins
class SystemePlugins {
    constructor() {
        this.plugins = new Map();
        this.hooks = new Map();
        this.config = {
            autoReload: true,
            watchFiles: true,
            maxConcurrentLoads: 3
        };
        
        // TODO: Initialiser le système
    }
    
    // TODO: Charger un plugin dynamiquement
    async chargerPlugin(nom, chemin, options = {}) {
        // 1. Vérifier si le plugin existe déjà
        // 2. Importer dynamiquement le module
        // 3. Valider l'interface du plugin
        // 4. Initialiser le plugin
        // 5. Enregistrer les hooks
        // 6. Gérer les dépendances
    }
    
    // TODO: Décharger un plugin
    async dechargerPlugin(nom) {
        // 1. Appeler la méthode cleanup du plugin
        // 2. Désenregistrer les hooks
        // 3. Supprimer des caches
    }
    
    // TODO: Hot reload d'un plugin
    async rechargerPlugin(nom) {
        // Décharger puis recharger avec préservation d'état
    }
    
    // TODO: Système de hooks
    enregistrerHook(evenement, callback, priorite = 10) {}
    async executerHook(evenement, donnees) {}
    
    // TODO: Gestion des dépendances entre plugins
    resoudreDependances(plugin) {}
    
    // TODO: Interface de plugin standardisée
    validerPlugin(plugin) {
        // Le plugin doit implémenter :
        // - name: string
        // - version: string  
        // - init(): Promise<void>
        // - cleanup(): Promise<void>
        // - hooks?: { [event: string]: Function }
        // - dependencies?: string[]
    }
    
    // TODO: Monitoring et debugging
    obtenirStatistiques() {}
    listerPlugins() {}
    obtenirInfoPlugin(nom) {}
}

// Exemple de plugin
const pluginExample = {
    name: 'mon-plugin',
    version: '1.0.0',
    description: 'Plugin d\'exemple',
    dependencies: ['plugin-base'],
    
    async init() {
        console.log('🔌 Plugin initialisé');
        // Logique d'initialisation
    },
    
    async cleanup() {
        console.log('🧹 Plugin nettoyé');
        // Logique de nettoyage
    },
    
    hooks: {
        'user-login': async (userData) => {
            console.log('🔐 Hook user-login', userData);
        },
        
        'page-load': async (pageData) => {
            console.log('📄 Hook page-load', pageData);
        }
    }
};

// Tests
async function testerSystemePlugins() {
    const systeme = new SystemePlugins();
    
    // TODO: Tester le chargement de plugins
    await systeme.chargerPlugin('exemple', './plugins/exemple.js');
    
    // TODO: Tester les hooks
    await systeme.executerHook('user-login', { userId: '123' });
    
    // TODO: Tester le hot reload
    await systeme.rechargerPlugin('exemple');
    
    console.log('Stats:', systeme.obtenirStatistiques());
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
async chargerPlugin(nom, chemin, options = {}) {
    try {
        // Éviter les doublons
        if (this.plugins.has(nom)) {
            console.warn(`Plugin ${nom} déjà chargé`);
            return this.plugins.get(nom);
        }
        
        // Import dynamique
        const module = await import(chemin);
        const plugin = module.default || module;
        
        // Validation
        if (!this.validerPlugin(plugin)) {
            throw new Error(`Plugin ${nom} invalide`);
        }
        
        // Initialisation
        await plugin.init();
        
        // Enregistrement
        this.plugins.set(nom, plugin);
        
        return plugin;
    } catch (erreur) {
        console.error(`Erreur chargement plugin ${nom}:`, erreur);
        throw erreur;
    }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
class SystemePlugins {
    constructor() {
        this.plugins = new Map();
        this.hooks = new Map();
        this.loadingQueue = [];
        this.config = {
            autoReload: true,
            watchFiles: true,
            maxConcurrentLoads: 3
        };
        
        this.stats = {
            totalChargements: 0,
            pluginsActifs: 0,
            erreurs: [],
            tempsChargement: new Map()
        };
        
        this.dependencyGraph = new Map();
    }
    
    async chargerPlugin(nom, chemin, options = {}) {
        const debut = performance.now();
        
        try {
            if (this.plugins.has(nom)) {
                console.warn(`🔄 Plugin ${nom} déjà chargé`);
                return this.plugins.get(nom);
            }
            
            console.log(`🔌 Chargement du plugin ${nom}...`);
            this.stats.totalChargements++;
            
            // Import avec timeout
            const timeoutPromise = new Promise((_, reject) =>
                setTimeout(() => reject(new Error('Timeout de chargement')), 10000)
            );
            
            const importPromise = import(chemin + '?t=' + Date.now()); // Cache busting
            const module = await Promise.race([importPromise, timeoutPromise]);
            
            const plugin = module.default || module;
            
            if (!this.validerPlugin(plugin)) {
                throw new Error(`Plugin ${nom} ne respecte pas l'interface requise`);
            }
            
            // Résoudre les dépendances
            await this.resoudreDependances(plugin);
            
            // Initialiser le plugin
            console.log(`⚙️ Initialisation du plugin ${plugin.name}...`);
            await plugin.init();
            
            // Enregistrer les hooks
            if (plugin.hooks) {
                for (const [evenement, callback] of Object.entries(plugin.hooks)) {
                    this.enregistrerHook(evenement, callback, plugin.priority || 10);
                }
            }
            
            // Sauvegarder
            this.plugins.set(nom, {
                ...plugin,
                cheminFichier: chemin,
                dateChargement: new Date(),
                actif: true
            });
            
            this.stats.pluginsActifs++;
            const duree = performance.now() - debut;
            this.stats.tempsChargement.set(nom, duree);
            
            console.log(`✅ Plugin ${nom} chargé en ${duree.toFixed(2)}ms`);
            
            // Déclencher événement de chargement
            await this.executerHook('plugin-loaded', { nom, plugin });
            
            return plugin;
            
        } catch (erreur) {
            const duree = performance.now() - debut;
            console.error(`❌ Erreur chargement plugin ${nom} (${duree.toFixed(2)}ms):`, erreur);
            
            this.stats.erreurs.push({
                plugin: nom,
                erreur: erreur.message,
                timestamp: Date.now()
            });
            
            throw erreur;
        }
    }
    
    async dechargerPlugin(nom) {
        const plugin = this.plugins.get(nom);
        if (!plugin) {
            console.warn(`Plugin ${nom} non trouvé`);
            return;
        }
        
        try {
            console.log(`🔌 Déchargement du plugin ${nom}...`);
            
            // Cleanup du plugin
            if (plugin.cleanup) {
                await plugin.cleanup();
            }
            
            // Désenregistrer les hooks
            if (plugin.hooks) {
                for (const evenement of Object.keys(plugin.hooks)) {
                    this.desenregistrerHook(evenement, plugin.hooks[evenement]);
                }
            }
            
            // Supprimer du cache
            this.plugins.delete(nom);
            this.stats.pluginsActifs--;
            
            console.log(`✅ Plugin ${nom} déchargé`);
            
            // Déclencher événement
            await this.executerHook('plugin-unloaded', { nom });
            
        } catch (erreur) {
            console.error(`Erreur déchargement plugin ${nom}:`, erreur);
            throw erreur;
        }
    }
    
    async rechargerPlugin(nom) {
        const plugin = this.plugins.get(nom);
        if (!plugin) {
            throw new Error(`Plugin ${nom} non trouvé`);
        }
        
        const chemin = plugin.cheminFichier;
        console.log(`🔄 Rechargement du plugin ${nom}...`);
        
        // Sauvegarder l'état si le plugin l'expose
        let etatSauvegarde = null;
        if (plugin.sauvegarderEtat) {
            etatSauvegarde = await plugin.sauvegarderEtat();
        }
        
        // Décharger
        await this.dechargerPlugin(nom);
        
        // Recharger
        const nouveauPlugin = await this.chargerPlugin(nom, chemin);
        
        // Restaurer l'état
        if (etatSauvegarde && nouveauPlugin.restaurerEtat) {
            await nouveauPlugin.restaurerEtat(etatSauvegarde);
        }
        
        console.log(`✅ Plugin ${nom} rechargé avec succès`);
        return nouveauPlugin;
    }
    
    enregistrerHook(evenement, callback, priorite = 10) {
        if (!this.hooks.has(evenement)) {
            this.hooks.set(evenement, []);
        }
        
        this.hooks.get(evenement).push({ callback, priorite });
        
        // Trier par priorité (plus haute = exécutée en premier)
        this.hooks.get(evenement).sort((a, b) => b.priorite - a.priorite);
    }
    
    desenregistrerHook(evenement, callback) {
        if (!this.hooks.has(evenement)) return;
        
        const hooks = this.hooks.get(evenement);
        const index = hooks.findIndex(h => h.callback === callback);
        
        if (index !== -1) {
            hooks.splice(index, 1);
        }
    }
    
    async executerHook(evenement, donnees = {}) {
        if (!this.hooks.has(evenement)) {
            return [];
        }
        
        const hooks = this.hooks.get(evenement);
        const resultats = [];
        
        console.log(`🪝 Exécution de ${hooks.length} hooks pour '${evenement}'`);
        
        for (const { callback } of hooks) {
            try {
                const resultat = await callback(donnees);
                resultats.push(resultat);
            } catch (erreur) {
                console.error(`Erreur dans hook ${evenement}:`, erreur);
                resultats.push({ erreur: erreur.message });
            }
        }
        
        return resultats;
    }
    
    async resoudreDependances(plugin) {
        if (!plugin.dependencies || plugin.dependencies.length === 0) {
            return;
        }
        
        console.log(`🔗 Résolution des dépendances pour ${plugin.name}:`, plugin.dependencies);
        
        for (const dep of plugin.dependencies) {
            if (!this.plugins.has(dep)) {
                throw new Error(`Dépendance manquante: ${dep} requis par ${plugin.name}`);
            }
        }
        
        console.log(`✅ Toutes les dépendances résolues pour ${plugin.name}`);
    }
    
    validerPlugin(plugin) {
        const requis = ['name', 'version', 'init'];
        
        for (const champ of requis) {
            if (!(champ in plugin)) {
                console.error(`Champ requis manquant: ${champ}`);
                return false;
            }
        }
        
        if (typeof plugin.init !== 'function') {
            console.error('init doit être une fonction');
            return false;
        }
        
        if (plugin.cleanup && typeof plugin.cleanup !== 'function') {
            console.error('cleanup doit être une fonction');
            return false;
        }
        
        return true;
    }
    
    obtenirStatistiques() {
        const tempsTotal = Array.from(this.stats.tempsChargement.values())
            .reduce((acc, temps) => acc + temps, 0);
        
        return {
            pluginsCharges: this.plugins.size,
            pluginsActifs: this.stats.pluginsActifs,
            totalChargements: this.stats.totalChargements,
            tempsMoyenChargement: this.stats.totalChargements > 0 
                ? Math.round(tempsTotal / this.stats.totalChargements * 100) / 100
                : 0,
            erreursRecentes: this.stats.erreurs.slice(-5),
            hooksEnregistres: this.hooks.size,
            evenementsDisponibles: Array.from(this.hooks.keys())
        };
    }
    
    listerPlugins() {
        return Array.from(this.plugins.entries()).map(([nom, plugin]) => ({
            nom,
            version: plugin.version,
            description: plugin.description,
            actif: plugin.actif,
            dateChargement: plugin.dateChargement,
            dependances: plugin.dependencies || []
        }));
    }
    
    obtenirInfoPlugin(nom) {
        const plugin = this.plugins.get(nom);
        if (!plugin) return null;
        
        return {
            ...plugin,
            tempsChargement: this.stats.tempsChargement.get(nom),
            hooksEnregistres: plugin.hooks ? Object.keys(plugin.hooks) : []
        };
    }
}
```
{% endtab %}
{% endtabs %}

---

## 🏗️ Architecture Modulaire

### Patterns d'organisation

{% tabs %}
{% tab title="📁 Structure de projet" %}
```
📦 mon-projet/
├── 📁 src/
│   ├── 📁 components/          # Composants réutilisables
│   │   ├── 📁 ui/
│   │   │   ├── Button.js
│   │   │   ├── Modal.js
│   │   │   └── index.js        # Point d'entrée
│   │   ├── 📁 forms/
│   │   │   ├── LoginForm.js
│   │   │   ├── ContactForm.js
│   │   │   └── index.js
│   │   └── index.js            # Réexporte tout
│   ├── 📁 services/            # Logique métier
│   │   ├── 📁 api/
│   │   │   ├── UserAPI.js
│   │   │   ├── ProductAPI.js
│   │   │   └── index.js
│   │   ├── 📁 auth/
│   │   │   ├── AuthService.js
│   │   │   ├── TokenManager.js
│   │   │   └── index.js
│   │   └── index.js
│   ├── 📁 utils/               # Utilitaires
│   │   ├── 📁 validation/
│   │   ├── 📁 formatting/
│   │   ├── 📁 helpers/
│   │   └── index.js
│   ├── 📁 store/               # Gestion d'état
│   │   ├── 📁 modules/
│   │   ├── actions.js
│   │   ├── mutations.js
│   │   └── index.js
│   ├── 📁 assets/              # Ressources statiques
│   ├── 📁 config/              # Configuration
│   │   ├── constants.js
│   │   ├── env.js
│   │   └── index.js
│   └── main.js                 # Point d'entrée principal
├── 📁 tests/                   # Tests
├── 📁 docs/                    # Documentation
├── package.json
└── README.md
```

```javascript
// 📁 src/components/index.js - Barrel exports
// Réexporte organisé par catégorie
export * from './ui';
export * from './forms';
export * from './navigation';
export * from './layout';

// Exports par défaut pour les composants principaux
export { default as App } from './App.js';
export { default as Router } from './Router.js';

// 📁 src/services/index.js
// Créer une façade unifiée pour tous les services
export { default as UserService } from './api/UserService.js';
export { default as AuthService } from './auth/AuthService.js';
export { default as NotificationService } from './NotificationService.js';

// Configuration centralisée des services
import { config } from '../config';

export const serviceFactory = {
    user: new UserService(config.api.baseUrl),
    auth: new AuthService(config.auth),
    notification: new NotificationService(config.notifications)
};

// 📁 src/utils/index.js
// Utilitaires organisés par domaine
export * as validation from './validation';
export * as format from './formatting';
export * as date from './date';
export * as string from './string';
export * as array from './array';
export * as object from './object';

// Helpers les plus utilisés directement accessibles
export { debounce, throttle } from './helpers/timing.js';
export { generateId, randomString } from './helpers/generators.js';
export { isEmail, isPhone, isUrl } from './validation/common.js';
```
{% endtab %}

{% tab title="🎯 Separation of Concerns" %}
```javascript
// 📁 src/services/api/BaseAPI.js
// Classe de base pour toutes les APIs
export default class BaseAPI {
    constructor(baseUrl, options = {}) {
        this.baseUrl = baseUrl;
        this.defaultHeaders = {
            'Content-Type': 'application/json',
            ...options.headers
        };
        this.timeout = options.timeout || 10000;
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseUrl}${endpoint}`;
        const config = {
            timeout: this.timeout,
            headers: { ...this.defaultHeaders, ...options.headers },
            ...options
        };
        
        const response = await fetch(url, config);
        
        if (!response.ok) {
            throw new APIError(response.status, response.statusText);
        }
        
        return response.json();
    }
    
    get(endpoint, params = {}) {
        const searchParams = new URLSearchParams(params);
        const url = searchParams.toString() 
            ? `${endpoint}?${searchParams}` 
            : endpoint;
        
        return this.request(url, { method: 'GET' });
    }
    
    post(endpoint, data) {
        return this.request(endpoint, {
            method: 'POST',
            body: JSON.stringify(data)
        });
    }
    
    put(endpoint, data) {
        return this.request(endpoint, {
            method: 'PUT',
            body: JSON.stringify(data)
        });
    }
    
    delete(endpoint) {
        return this.request(endpoint, { method: 'DELETE' });
    }
}

// 📁 src/services/api/UserAPI.js
// API spécialisée héritant de BaseAPI
import BaseAPI from './BaseAPI.js';

export default class UserAPI extends BaseAPI {
    constructor(baseUrl) {
        super(`${baseUrl}/users`);
    }
    
    async obtenirTous(pagination = {}) {
        return this.get('', pagination);
    }
    
    async obtenirParId(id) {
        return this.get(`/${id}`);
    }
    
    async creer(userData) {
        return this.post('', userData);
    }
    
    async modifier(id, userData) {
        return this.put(`/${id}`, userData);
    }
    
    async supprimer(id) {
        return this.delete(`/${id}`);
    }
    
    async rechercherParEmail(email) {
        return this.get('/search', { email });
    }
    
    async obtenirProfil(id) {
        return this.get(`/${id}/profile`);
    }
}

// 📁 src/services/UserService.js
// Service métier utilisant l'API
import UserAPI from './api/UserAPI.js';
import { ValidationService } from './ValidationService.js';
import { CacheService } from './CacheService.js';

export default class UserService {
    constructor(apiUrl) {
        this.api = new UserAPI(apiUrl);
        this.validator = new ValidationService();
        this.cache = new CacheService('users', 300000); // 5 min cache
    }
    
    async obtenirUtilisateur(id) {
        // Vérifier le cache d'abord
        const cached = this.cache.get(id);
        if (cached) return cached;
        
        // Valider l'ID
        if (!this.validator.isValidId(id)) {
            throw new ValidationError('ID utilisateur invalide');
        }
        
        // Appel API
        const utilisateur = await this.api.obtenirParId(id);
        
        // Transformer et cacher
        const transformed = this.transformerUtilisateur(utilisateur);
        this.cache.set(id, transformed);
        
        return transformed;
    }
    
    async creerUtilisateur(donnees) {
        // Validation complète
        const errors = this.validator.validerNouvelUtilisateur(donnees);
        if (errors.length > 0) {
            throw new ValidationError('Données invalides', errors);
        }
        
        // Vérifier unicité email
        const existant = await this.api.rechercherParEmail(donnees.email);
        if (existant.length > 0) {
            throw new BusinessError('Email déjà utilisé');
        }
        
        // Créer
        const nouvelUtilisateur = await this.api.creer(donnees);
        
        // Invalider cache et déclencher événements
        this.cache.invalidatePattern('users.*');
        this.emit('user-created', nouvelUtilisateur);
        
        return this.transformerUtilisateur(nouvelUtilisateur);
    }
    
    transformerUtilisateur(userData) {
        return {
            id: userData.id,
            nom: userData.firstName + ' ' + userData.lastName,
            email: userData.email,
            avatar: userData.avatar || '/default-avatar.png',
            dateInscription: new Date(userData.createdAt),
            permissions: userData.roles?.map(r => r.permissions).flat() || []
        };
    }
    
    // Méthodes d'événements pour découplage
    on(event, callback) {
        // Implémentation EventEmitter
    }
    
    emit(event, data) {
        // Implémentation EventEmitter
    }
}
```
{% endtab %}

{% tab title="🔌 Dependency Injection" %}
```javascript
// 📁 src/core/Container.js
// Container IoC simple mais puissant
export default class Container {
    constructor() {
        this.services = new Map();
        this.factories = new Map();
        this.singletons = new Map();
    }
    
    // Enregistrer un service simple
    register(name, implementation) {
        this.services.set(name, implementation);
        return this;
    }
    
    // Enregistrer une factory
    factory(name, factoryFunction) {
        this.factories.set(name, factoryFunction);
        return this;
    }
    
    // Enregistrer un singleton
    singleton(name, implementation) {
        this.singletons.set(name, implementation);
        return this;
    }
    
    // Résoudre un service
    resolve(name) {
        // Singleton (une seule instance)
        if (this.singletons.has(name)) {
            return this.singletons.get(name);
        }
        
        // Factory (nouvelle instance à chaque fois)
        if (this.factories.has(name)) {
            const factory = this.factories.get(name);
            return factory(this);
        }
        
        // Service simple
        if (this.services.has(name)) {
            const ServiceClass = this.services.get(name);
            
            // Auto-injection des dépendances
            if (ServiceClass.dependencies) {
                const dependencies = ServiceClass.dependencies.map(dep => this.resolve(dep));
                return new ServiceClass(...dependencies);
            }
            
            return new ServiceClass();
        }
        
        throw new Error(`Service '${name}' non trouvé`);
    }
    
    // Vérifier si un service existe
    has(name) {
        return this.services.has(name) || 
               this.factories.has(name) || 
               this.singletons.has(name);
    }
    
    // Auto-wire d'une classe
    wire(TargetClass) {
        if (TargetClass.dependencies) {
            const dependencies = TargetClass.dependencies.map(dep => this.resolve(dep));
            return new TargetClass(...dependencies);
        }
        return new TargetClass();
    }
}

// 📁 src/config/services.js
// Configuration centralisée des services
import Container from '../core/Container.js';
import UserService from '../services/UserService.js';
import AuthService from '../services/AuthService.js';
import NotificationService from '../services/NotificationService.js';
import CacheService from '../services/CacheService.js';
import ValidationService from '../services/ValidationService.js';

// Créer le container
const container = new Container();

// Configuration des services avec leurs dépendances
container
    // Services de base
    .singleton('cache', new CacheService())
    .singleton('validator', new ValidationService())
    .singleton('notification', new NotificationService())
    
    // Services métier avec injection automatique
    .factory('userService', (container) => {
        return new UserService(
            container.resolve('cache'),
            container.resolve('validator')
        );
    })
    
    .factory('authService', (container) => {
        return new AuthService(
            container.resolve('userService'),
            container.resolve('notification')
        );
    });

// Export du container configuré
export default container;

// 📁 src/services/UserService.js - Avec injection de dépendances
export default class UserService {
    // Déclaration des dépendances (convention)
    static dependencies = ['cache', 'validator'];
    
    constructor(cache, validator) {
        this.cache = cache;
        this.validator = validator;
        this.api = new UserAPI(process.env.API_URL);
    }
    
    async obtenirUtilisateur(id) {
        // Le cache et validator sont automatiquement injectés
        const cached = this.cache.get(`user:${id}`);
        if (cached) return cached;
        
        if (!this.validator.isValidId(id)) {
            throw new ValidationError('ID invalide');
        }
        
        const user = await this.api.obtenirParId(id);
        this.cache.set(`user:${id}`, user);
        
        return user;
    }
}

// 📁 src/main.js - Utilisation du container
import container from './config/services.js';

// Bootstrap de l'application
async function bootstrap() {
    try {
        // Résolution automatique des services
        const userService = container.resolve('userService');
        const authService = container.resolve('authService');
        
        // Initialisation
        await authService.init();
        
        // L'application peut maintenant utiliser les services
        window.services = {
            user: userService,
            auth: authService
        };
        
        console.log('✅ Application initialisée avec succès');
        
    } catch (erreur) {
        console.error('❌ Erreur d\'initialisation:', erreur);
    }
}

bootstrap();
```
{% endtab %}
{% endtabs %}

---

## 🎯 Projet Final : Architecture Modulaire Complète

{% hint style="danger" %}
**Projet Capstone :** Concevez une architecture modulaire complète pour une application e-commerce
{% endhint %}

### Spécifications

Créez une architecture modulaire pour une plateforme e-commerce avec :

1. **Modules métier** : Users, Products, Orders, Payments
2. **Services transversaux** : Auth, Cache, Validation, Logging
3. **Système de plugins** pour extensions tierces
4. **API Gateway** pour centraliser les appels
5. **State Management** modulaire
6. **Micro-frontends** avec lazy loading

```javascript
// Votre mission : concevoir cette architecture
const architectureSpecs = {
    modules: [
        {
            name: 'users',
            services: ['UserService', 'ProfileService', 'PreferencesService'],
            api: ['UserAPI', 'ProfileAPI'],
            components: ['UserProfile', 'UserSettings', 'UserList'],
            store: ['userModule', 'profileModule']
        },
        {
            name: 'products',
            services: ['ProductService', 'CategoryService', 'SearchService'],
            api: ['ProductAPI', 'CategoryAPI', 'SearchAPI'],
            components: ['ProductCard', 'ProductList', 'ProductDetails'],
            store: ['productModule', 'categoryModule']
        },
        {
            name: 'orders',
            services: ['OrderService', 'CartService', 'ShippingService'],
            api: ['OrderAPI', 'CartAPI'],
            components: ['Cart', 'Checkout', 'OrderHistory'],
            store: ['orderModule', 'cartModule']
        },
        {
            name: 'payments',
            services: ['PaymentService', 'BillingService'],
            api: ['PaymentAPI', 'BillingAPI'],
            components: ['PaymentForm', 'BillingInfo'],
            store: ['paymentModule']
        }
    ],
    
    crossCutting: [
        'AuthService',
        'CacheService', 
        'ValidationService',
        'LoggingService',
        'NotificationService',
        'AnalyticsService'
    ],
    
    infrastructure: [
        'APIGateway',
        'ModuleLoader',
        'EventBus',
        'StateManager',
        'Router',
        'ErrorHandler'
    ]
};

// TODO: Implémenter cette architecture
class ECommerceArchitecture {
    constructor() {
        this.container = new Container();
        this.moduleLoader = new ModuleLoader();
        this.eventBus = new EventBus();
        this.router = new ModularRouter();
        
        this.modules = new Map();
        this.loadedModules = new Set();
    }
    
    // TODO: Système de chargement modulaire
    async chargerModule(nomModule) {}
    
    // TODO: Configuration des services transversaux  
    configurerServicesTransversaux() {}
    
    // TODO: API Gateway modulaire
    configurerAPIGateway() {}
    
    // TODO: State management modulaire
    configurerStateManagement() {}
    
    // TODO: Routing modulaire avec lazy loading
    configurerRouting() {}
    
    // TODO: Système d'événements inter-modules
    configurerEventBus() {}
    
    // TODO: Hot module replacement
    activerHMR() {}
    
    async init() {
        // TODO: Initialiser l'architecture complète
    }
}
```

### Critères d'évaluation

{% tabs %}
{% tab title="🎯 Architecture" %}
- [ ] **Modularité** : Modules indépendants et cohérents
- [ ] **Scalabilité** : Architecture extensible
- [ ] **Maintenabilité** : Code organisé et documenté
- [ ] **Performance** : Lazy loading et optimisations
- [ ] **Testabilité** : Modules facilement testables
- [ ] **Réutilisabilité** : Composants réutilisables
{% endtab %}

{% tab title="⭐ Fonctionnalités avancées" %}
- [ ] **Micro-frontends** : Modules isolés
- [ ] **Plugin system** : Extensions tierces
- [ ] **Hot module replacement** : Développement optimisé
- [ ] **Federation** : Partage entre applications
- [ ] **Progressive loading** : Chargement intelligent
- [ ] **Error boundaries** : Isolation des erreurs
{% endtab %}

{% tab title="🏆 Excellence" %}
- [ ] **TypeScript** : Typage complet
- [ ] **Documentation** : Architecture documentée
- [ ] **Tests** : Couverture complète
- [ ] **CI/CD** : Pipeline automatisé
- [ ] **Monitoring** : Observabilité intégrée
- [ ] **Security** : Sécurité par design
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant l'organisation modulaire en JavaScript !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **ES6 Modules** : Import/export modernes
- **CommonJS** : Modules Node.js traditionnels
- **Architecture modulaire** : Organisation scalable
- **Dependency injection** : Découplage des services
- **Lazy loading** : Optimisation des performances
- **Plugin systems** : Extensibilité
- **Micro-frontends** : Architecture distribuée
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Concevoir des architectures modulaires
- Optimiser les performances de chargement
- Créer des systèmes extensibles
- Gérer des dépendances complexes
- Implémenter des patterns avancés
- Organiser des projets de grande taille
{% endtab %}

{% tab title="🚀 Applications" %}
- **Grandes applications** web et mobile
- **Plateformes** e-commerce et SaaS
- **Systèmes distribués** et microservices
- **Bibliothèques** et frameworks
- **Applications d'entreprise** complexes
- **Architectures** cloud-native
{% endtab %}
{% endtabs %}

### Ressources pour aller plus loin

- 📚 [ES6 Modules Deep Dive](https://exploringjs.com/es6/ch_modules.html)
- 🎥 [JavaScript Module Systems](https://www.youtube.com/watch?v=qJWALEoGge4)
- 📖 [Clean Architecture JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)
- 🛠️ [Module Federation](https://webpack.js.org/concepts/module-federation/)

---

**Prêt à construire des architectures d'enterprise ? Continuez avec les modules de production !** 🏗️
