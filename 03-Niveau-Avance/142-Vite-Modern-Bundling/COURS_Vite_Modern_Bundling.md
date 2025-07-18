# Vite - Bundling Moderne

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** Vite - Build Tool Moderne et Rapide
- **Niveau :** 🔴 Avancé
- **Durée estimée :** 55 minutes
- **Prérequis :** Modules ES6, NPM/Package managers, Node.js
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Pourquoi Vite est plus rapide que Webpack
- [ ] **Appliquer** : Configurer un projet Vite from scratch
- [ ] **Analyser** : Les avantages de Vite vs autres bundlers
- [ ] **Créer** : Applications optimisées avec Hot Module Replacement

### Mots-clés à retenir
- **Vite** : Build tool ultra-rapide basé sur les modules ES
- **ESBuild** : Bundler écrit en Go, extrêmement rapide
- **HMR** : Hot Module Replacement, mise à jour en temps réel
- **Dev Server** : Serveur de développement avec modules natifs
- **Tree-shaking** : Élimination du code mort automatique

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Vite révolutionne le développement frontend en apportant :
- **Vitesse de démarrage** : Serveur dev instantané (<100ms vs 10s+ Webpack)
- **Hot reload ultra-rapide** : Modifications instantanées
- **Build optimisé** : Production rapide avec Rollup
- **Configuration minimale** : Fonctionne out-of-the-box
- **Support moderne** : TypeScript, Vue, React, Svelte natifs

**Problème résolu** : Les gros projets Webpack prennent 30s+ à démarrer, Vite = instantané !

### Définition et concepts clés

**Vite** (français "rapide") exploite les **modules ES natifs** du navigateur pour éliminer le bundling en développement.

**Approche traditionnelle (Webpack) :**
```
Source files → Bundle everything → Serve bundle → Browser
     ↓              ↓                   ↓
   100ms         10-30s              Slow HMR
```

**Approche Vite :**
```
Source files → Serve modules directly → Browser (native ES modules)
     ↓                ↓                      ↓
   100ms           <100ms               Instant HMR
```

### Comparaison des build tools

| Feature | Webpack | Vite | Parcel | Rollup |
|---------|---------|------|--------|--------|
| **Démarrage dev** | 🔴 10-30s | ✅ <100ms | 🟡 2-5s | 🟡 5-10s |
| **HMR** | 🟡 1-3s | ✅ <50ms | 🟡 500ms | ❌ Non |
| **Configuration** | 🔴 Complexe | ✅ Minimal | ✅ Zero-config | 🟡 Moyen |
| **Build production** | ✅ Excellent | ✅ Excellent | 🟡 Bon | ✅ Excellent |
| **Écosystème** | ✅ Énorme | 🟡 Croissant | 🟡 Moyen | 🟡 Spécialisé |

### Architecture de Vite

```
Development:
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   Browser   │◄───│  Vite Server │◄───│ Source Code │
│ (ES modules)│    │   (esbuild)  │    │   (.js/.ts) │
└─────────────┘    └──────────────┘    └─────────────┘

Production:
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   Output    │◄───│   Rollup     │◄───│ Source Code │
│  (bundled)  │    │  (optimized) │    │   (.js/.ts) │
└─────────────┘    └──────────────┘    └─────────────┘
```

### Points d'attention
- **Navigateurs modernes** : Nécessite support ES modules (IE non supporté)
- **Dépendances** : Certaines libs nécessitent configuration spéciale
- **Production** : Utilise Rollup, pas esbuild (pour compatibilité)
- **Migration** : Peut nécessiter adaptations depuis Webpack

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```bash
# Installation et création d'un projet Vite
npm create vite@latest mon-projet -- --template vanilla
cd mon-projet
npm install
npm run dev
```

**Structure générée :**
```
mon-projet/
├── index.html          # Point d'entrée
├── main.js            # JavaScript principal
├── style.css          # Styles
├── package.json       # Dépendances
├── vite.config.js     # Configuration (optionnelle)
└── public/            # Assets statiques
```

**Fichier `vite.config.js` basique :**
```javascript
import { defineConfig } from 'vite'

export default defineConfig({
  // Configuration de base
  server: {
    port: 3000,
    open: true // Ouvre automatiquement le navigateur
  },
  
  // Dossier de build
  build: {
    outDir: 'dist',
    sourcemap: true
  },
  
  // Variables d'environnement
  define: {
    __APP_VERSION__: JSON.stringify('1.0.0')
  }
})
```

**Exemple d'application simple :**
```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>App Vite</title>
</head>
<body>
    <div id="app">
        <h1>🚀 Mon App avec Vite</h1>
        <div id="counter-container"></div>
        <div id="time-container"></div>
    </div>
    <script type="module" src="/main.js"></script>
</body>
</html>
```

```javascript
// main.js
import './style.css'
import { createCounter } from './components/counter.js'
import { createClock } from './components/clock.js'

console.log('🚀 Application Vite démarrée !');

// Initialiser les composants
const app = document.getElementById('app');

// Composant compteur
const counterContainer = document.getElementById('counter-container');
createCounter(counterContainer);

// Composant horloge
const timeContainer = document.getElementById('time-container');
createClock(timeContainer);

// Hot Module Replacement (HMR)
if (import.meta.hot) {
  import.meta.hot.accept('./components/counter.js', (newModule) => {
    console.log('🔥 Counter module updated');
  });
  
  import.meta.hot.accept('./components/clock.js', (newModule) => {
    console.log('🔥 Clock module updated');
  });
}
```

```javascript
// components/counter.js
let count = 0;

export function createCounter(container) {
  container.innerHTML = `
    <div class="counter">
      <h2>Compteur</h2>
      <div class="count-display">${count}</div>
      <button id="increment">+1</button>
      <button id="decrement">-1</button>
      <button id="reset">Reset</button>
    </div>
  `;
  
  // Événements
  container.querySelector('#increment').onclick = () => {
    count++;
    updateDisplay();
  };
  
  container.querySelector('#decrement').onclick = () => {
    count--;
    updateDisplay();
  };
  
  container.querySelector('#reset').onclick = () => {
    count = 0;
    updateDisplay();
  };
  
  function updateDisplay() {
    container.querySelector('.count-display').textContent = count;
  }
}
```

```javascript
// components/clock.js
export function createClock(container) {
  container.innerHTML = `
    <div class="clock">
      <h2>Horloge Temps Réel</h2>
      <div class="time-display"></div>
      <div class="timezone-selector">
        <select id="timezone">
          <option value="Europe/Paris">Paris</option>
          <option value="America/New_York">New York</option>
          <option value="Asia/Tokyo">Tokyo</option>
        </select>
      </div>
    </div>
  `;
  
  let currentTimezone = 'Europe/Paris';
  
  function updateTime() {
    const now = new Date();
    const timeDisplay = container.querySelector('.time-display');
    
    timeDisplay.textContent = now.toLocaleString('fr-FR', {
      timeZone: currentTimezone,
      hour: '2-digit',
      minute: '2-digit',
      second: '2-digit'
    });
  }
  
  // Mise à jour chaque seconde
  setInterval(updateTime, 1000);
  updateTime(); // Affichage initial
  
  // Changement de timezone
  container.querySelector('#timezone').onchange = (e) => {
    currentTimezone = e.target.value;
    updateTime();
  };
}
```

```css
/* style.css */
:root {
  --primary-color: #646cff;
  --secondary-color: #535bf2;
  --background: #ffffff;
  --text-color: #213547;
  --border-color: #e0e0e0;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: var(--background);
  color: var(--text-color);
  line-height: 1.6;
}

#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  text-align: center;
  color: var(--primary-color);
  margin-bottom: 2rem;
}

.counter, .clock {
  background: white;
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 1.5rem;
  margin-bottom: 1rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.count-display, .time-display {
  font-size: 2rem;
  font-weight: bold;
  text-align: center;
  margin: 1rem 0;
  color: var(--secondary-color);
}

button {
  background: var(--primary-color);
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  margin: 0.25rem;
  transition: background-color 0.2s;
}

button:hover {
  background: var(--secondary-color);
}

.timezone-selector {
  text-align: center;
  margin-top: 1rem;
}

select {
  padding: 0.5rem;
  border: 1px solid var(--border-color);
  border-radius: 4px;
  background: white;
}

/* HMR indicator */
.vite-error-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  z-index: 99999;
}
```

### Exemple intermédiaire
```javascript
// vite.config.js - Configuration avancée
import { defineConfig } from 'vite'
import { resolve } from 'path'

export default defineConfig({
  // Plugins
  plugins: [
    // Plugin personnalisé pour dev
    {
      name: 'dev-logger',
      buildStart() {
        console.log('🚀 Build Vite démarré');
      }
    }
  ],
  
  // Résolution des modules
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src'),
      '@components': resolve(__dirname, 'src/components'),
      '@utils': resolve(__dirname, 'src/utils')
    }
  },
  
  // Configuration du serveur de dev
  server: {
    port: 3000,
    host: true, // Accessible depuis réseau local
    open: '/app', // Page d'ouverture
    proxy: {
      // Proxy API calls
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, '')
      }
    },
    cors: true
  },
  
  // Configuration de build
  build: {
    outDir: 'dist',
    assetsDir: 'assets',
    sourcemap: true,
    
    // Options Rollup
    rollupOptions: {
      input: {
        main: resolve(__dirname, 'index.html'),
        admin: resolve(__dirname, 'admin.html')
      },
      output: {
        // Nommage des chunks
        chunkFileNames: 'js/[name]-[hash].js',
        entryFileNames: 'js/[name]-[hash].js',
        assetFileNames: 'assets/[name]-[hash].[ext]'
      }
    },
    
    // Taille limite avant warning
    chunkSizeWarningLimit: 1000
  },
  
  // Variables d'environnement
  define: {
    __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
    __BUILD_TIME__: JSON.stringify(new Date().toISOString())
  },
  
  // Configuration CSS
  css: {
    devSourcemap: true,
    preprocessorOptions: {
      scss: {
        additionalData: `@import "@/styles/variables.scss";`
      }
    }
  },
  
  // Optimisation des dépendances
  optimizeDeps: {
    include: ['lodash'], // Force l'inclusion
    exclude: ['@my-lib'] // Exclure du pre-bundling
  }
})
```

**Structure projet avancée :**
```
mon-projet-avance/
├── index.html
├── admin.html
├── vite.config.js
├── package.json
├── src/
│   ├── main.js
│   ├── admin.js
│   ├── components/
│   │   ├── Counter.js
│   │   ├── Clock.js
│   │   └── DataTable.js
│   ├── utils/
│   │   ├── api.js
│   │   ├── helpers.js
│   │   └── constants.js
│   ├── styles/
│   │   ├── main.css
│   │   ├── variables.scss
│   │   └── components.scss
│   └── assets/
│       ├── images/
│       └── fonts/
├── public/
│   ├── favicon.ico
│   └── robots.txt
└── dist/ (généré)
```

### Exemple avancé (optionnel)
```javascript
// Configuration Vite pour application multi-framework
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import vue from '@vitejs/plugin-vue'
import { svelte } from '@sveltejs/vite-plugin-svelte'

export default defineConfig({
  plugins: [
    // Support multi-framework
    react({
      include: "**/*.{jsx,tsx}"
    }),
    vue({
      include: [/\.vue$/]
    }),
    svelte({
      include: "**/*.svelte"
    }),
    
    // Plugin personnalisé pour analytics
    {
      name: 'build-analytics',
      generateBundle(options, bundle) {
        const stats = {
          chunks: Object.keys(bundle).length,
          totalSize: Object.values(bundle)
            .reduce((acc, chunk) => acc + (chunk.code?.length || 0), 0),
          timestamp: new Date().toISOString()
        };
        
        console.log('📊 Build Stats:', stats);
        
        // Sauvegarder les stats
        this.emitFile({
          type: 'asset',
          fileName: 'build-stats.json',
          source: JSON.stringify(stats, null, 2)
        });
      }
    }
  ],
  
  // Build pour différents environnements
  build: {
    rollupOptions: {
      output: {
        // Code splitting intelligent
        manualChunks: {
          'vendor': ['react', 'react-dom'],
          'utils': ['lodash', 'date-fns'],
          'ui': ['@material-ui/core']
        }
      }
    },
    
    // Build conditionnelle selon environnement
    ...(process.env.NODE_ENV === 'production' && {
      minify: 'terser',
      terserOptions: {
        compress: {
          drop_console: true,
          drop_debugger: true
        }
      }
    })
  },
  
  // Configuration par environnement
  ...(process.env.NODE_ENV === 'development' && {
    server: {
      hmr: {
        overlay: true
      }
    }
  })
})
```

**Scripts package.json optimisés :**
```json
{
  "name": "mon-app-vite",
  "private": true,
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "dev:host": "vite --host",
    "dev:debug": "vite --debug",
    "build": "vite build",
    "build:analyze": "vite build --mode analyze",
    "preview": "vite preview",
    "preview:host": "vite preview --host",
    "clean": "rm -rf dist",
    "type-check": "tsc --noEmit",
    "lint": "eslint src --ext .js,.ts,.jsx,.tsx",
    "test": "vitest",
    "test:ui": "vitest --ui"
  },
  "dependencies": {
    "lodash": "^4.17.21",
    "date-fns": "^2.29.3"
  },
  "devDependencies": {
    "vite": "^4.4.0",
    "@vitejs/plugin-react": "^4.0.0",
    "typescript": "^5.0.0",
    "vitest": "^0.34.0",
    "eslint": "^8.45.0"
  }
}
```

**Environnements multiples :**
```bash
# .env.development
VITE_API_URL=http://localhost:8080
VITE_APP_MODE=development
VITE_ENABLE_LOGGING=true

# .env.production
VITE_API_URL=https://api.monapp.com
VITE_APP_MODE=production
VITE_ENABLE_LOGGING=false

# .env.staging
VITE_API_URL=https://staging-api.monapp.com
VITE_APP_MODE=staging
VITE_ENABLE_LOGGING=true
```

```javascript
// utils/config.js - Utilisation des variables d'environnement
export const config = {
  apiUrl: import.meta.env.VITE_API_URL,
  mode: import.meta.env.VITE_APP_MODE,
  logging: import.meta.env.VITE_ENABLE_LOGGING === 'true',
  version: import.meta.env.VITE_APP_VERSION || '1.0.0'
}

// Logger conditionnel
export const logger = {
  log: (...args) => {
    if (config.logging) {
      console.log('[APP]', ...args)
    }
  },
  error: (...args) => {
    if (config.logging) {
      console.error('[ERROR]', ...args)
    }
  }
}
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Migration depuis Create React App
**Consigne :** Migre un projet Create React App vers Vite

**Code de départ CRA :**
```javascript
// src/App.js (CRA)
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>Hello React!</p>
      </header>
    </div>
  );
}

export default App;
```

**Tâches :**
- [ ] Créer la configuration Vite équivalente
- [ ] Adapter l'index.html
- [ ] Configurer les alias de paths
- [ ] Tester le HMR

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Application multi-pages
**Consigne :** Crée une app avec plusieurs pages et navigation

**Spécifications :**
- Page d'accueil, À propos, Contact
- Routing côté client sans framework
- Composants réutilisables
- Build optimisé avec code splitting

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Plugin Vite personnalisé
**Consigne :** Développe un plugin qui génère automatiquement un sitemap

**Contraintes :**
- Scanner tous les fichiers HTML de build
- Générer sitemap.xml valide
- Hook sur le processus de build
- Configuration paramétrable

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// vite.config.js - Migration CRA vers Vite
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { resolve } from 'path'

export default defineConfig({
  plugins: [react()],
  
  resolve: {
    alias: {
      // Alias CRA standard
      '@': resolve(__dirname, 'src'),
      '@components': resolve(__dirname, 'src/components'),
      '@utils': resolve(__dirname, 'src/utils'),
      '@assets': resolve(__dirname, 'src/assets')
    }
  },
  
  server: {
    port: 3000, // Port par défaut CRA
    open: true
  },
  
  build: {
    outDir: 'build', // Garder le nom CRA
    rollupOptions: {
      output: {
        assetFileNames: 'static/media/[name].[hash].[ext]',
        chunkFileNames: 'static/js/[name].[hash].js',
        entryFileNames: 'static/js/[name].[hash].js'
      }
    }
  },
  
  // Support des variables d'environnement CRA
  define: {
    'process.env.PUBLIC_URL': JSON.stringify(''),
    'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
  }
})
```

```html
<!-- index.html adapté -->
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App - Vite</title>
</head>
<body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
</body>
</html>
```

```javascript
// src/main.jsx - Point d'entrée Vite
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

**Explication :** Configuration minimale pour garder la compatibilité CRA tout en bénéficiant de Vite.

### Solution Exercice 2
```javascript
// vite.config.js - Multi-pages
import { defineConfig } from 'vite'
import { resolve } from 'path'

export default defineConfig({
  build: {
    rollupOptions: {
      input: {
        main: resolve(__dirname, 'index.html'),
        about: resolve(__dirname, 'about.html'),
        contact: resolve(__dirname, 'contact.html')
      }
    }
  },
  
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src')
    }
  }
})
```

```javascript
// src/router.js - Router simple
class SimpleRouter {
  constructor() {
    this.routes = new Map();
    this.currentPath = window.location.pathname;
    
    window.addEventListener('popstate', () => {
      this.navigate(window.location.pathname, false);
    });
  }
  
  addRoute(path, handler) {
    this.routes.set(path, handler);
  }
  
  navigate(path, pushState = true) {
    const handler = this.routes.get(path);
    
    if (handler) {
      this.currentPath = path;
      if (pushState) {
        window.history.pushState({}, '', path);
      }
      handler();
    } else {
      console.warn(`Route not found: ${path}`);
    }
  }
  
  init() {
    // Route initiale
    this.navigate(this.currentPath, false);
  }
}

// Instance globale
export const router = new SimpleRouter();

// Helper pour créer des liens
export function createLink(text, path, className = '') {
  const link = document.createElement('a');
  link.href = path;
  link.textContent = text;
  link.className = className;
  
  link.onclick = (e) => {
    e.preventDefault();
    router.navigate(path);
  };
  
  return link;
}
```

```javascript
// src/components/Navigation.js
import { createLink } from '@/router.js';

export function createNavigation(container) {
  const nav = document.createElement('nav');
  nav.className = 'navigation';
  
  const links = [
    { text: '🏠 Accueil', path: '/' },
    { text: '📖 À propos', path: '/about' },
    { text: '📧 Contact', path: '/contact' }
  ];
  
  links.forEach(({ text, path }) => {
    const link = createLink(text, path, 'nav-link');
    nav.appendChild(link);
  });
  
  container.appendChild(nav);
}
```

```javascript
// src/pages/HomePage.js
import { createNavigation } from '@/components/Navigation.js';

export function HomePage() {
  const container = document.getElementById('app');
  
  container.innerHTML = `
    <div class="page home-page">
      <div id="nav-container"></div>
      <main>
        <h1>🏠 Bienvenue sur notre site</h1>
        <p>Ceci est la page d'accueil construite avec Vite !</p>
        <div class="features">
          <div class="feature">
            <h3>⚡ Rapide</h3>
            <p>Développement ultra-rapide avec Vite</p>
          </div>
          <div class="feature">
            <h3>🔥 HMR</h3>
            <p>Hot Module Replacement instantané</p>
          </div>
          <div class="feature">
            <h3>📦 Optimisé</h3>
            <p>Build de production optimisé</p>
          </div>
        </div>
      </main>
    </div>
  `;
  
  createNavigation(document.getElementById('nav-container'));
}
```

```javascript
// main.js - Point d'entrée principal
import { router } from '@/router.js';
import { HomePage } from '@/pages/HomePage.js';
import { AboutPage } from '@/pages/AboutPage.js';
import { ContactPage } from '@/pages/ContactPage.js';
import '@/styles/main.css';

// Enregistrer les routes
router.addRoute('/', HomePage);
router.addRoute('/about', AboutPage);
router.addRoute('/contact', ContactPage);

// Démarrer l'application
router.init();

// HMR
if (import.meta.hot) {
  import.meta.hot.accept();
}
```

**Alternatives possibles :**
- Utiliser un vrai router comme Navigo
- Intégrer un framework comme Vue ou React
- Ajouter la génération automatique de routes

### Solution Exercice 3
```javascript
// plugins/sitemap-generator.js
import { writeFileSync } from 'fs';
import { resolve } from 'path';

export function sitemapGenerator(options = {}) {
  const {
    hostname = 'https://example.com',
    filename = 'sitemap.xml',
    exclude = [],
    priority = 0.8,
    changefreq = 'weekly'
  } = options;
  
  return {
    name: 'sitemap-generator',
    
    generateBundle(opts, bundle) {
      console.log('🗺️ Génération du sitemap...');
      
      // Trouver tous les fichiers HTML
      const htmlFiles = Object.keys(bundle)
        .filter(fileName => fileName.endsWith('.html'))
        .filter(fileName => !exclude.some(pattern => fileName.includes(pattern)));
      
      // Générer les URLs
      const urls = htmlFiles.map(fileName => {
        // Convertir le nom de fichier en URL
        let url = fileName.replace(/\.html$/, '');
        if (url === 'index') url = '';
        if (url && !url.startsWith('/')) url = '/' + url;
        
        return {
          loc: hostname + url,
          lastmod: new Date().toISOString().split('T')[0],
          changefreq,
          priority: url === '' ? '1.0' : priority
        };
      });
      
      // Générer le XML
      const sitemap = generateSitemapXML(urls);
      
      // Émettre le fichier
      this.emitFile({
        type: 'asset',
        fileName: filename,
        source: sitemap
      });
      
      console.log(`✅ Sitemap généré avec ${urls.length} URLs`);
    }
  };
}

function generateSitemapXML(urls) {
  const urlEntries = urls.map(url => `
  <url>
    <loc>${url.loc}</loc>
    <lastmod>${url.lastmod}</lastmod>
    <changefreq>${url.changefreq}</changefreq>
    <priority>${url.priority}</priority>
  </url>`).join('');
  
  return `<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
${urlEntries}
</urlset>`;
}
```

```javascript
// vite.config.js - Utilisation du plugin
import { defineConfig } from 'vite';
import { sitemapGenerator } from './plugins/sitemap-generator.js';

export default defineConfig({
  plugins: [
    sitemapGenerator({
      hostname: 'https://monsite.com',
      exclude: ['404', 'admin'],
      priority: 0.8,
      changefreq: 'weekly'
    })
  ],
  
  build: {
    rollupOptions: {
      input: {
        main: 'index.html',
        about: 'about.html',
        contact: 'contact.html',
        blog: 'blog.html'
      }
    }
  }
});
```

**Exemple de sitemap généré :**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://monsite.com</loc>
    <lastmod>2025-07-18</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://monsite.com/about</loc>
    <lastmod>2025-07-18</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
</urlset>
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Modules ES6** : Base de l'architecture Vite
- **NPM** : Gestion des dépendances et scripts
- **Node.js** : Environnement d'exécution pour les outils

### Prochaines étapes
- **Déploiement** : CI/CD avec Vite builds
- **Performance** : Optimisation avancée
- **Testing** : Vitest pour les tests unitaires

### Concepts complémentaires
- **Rollup** : Bundler utilisé en production
- **ESBuild** : Transpileur ultra-rapide
- **SWC** : Alternative à Babel pour la transformation

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un "Dashboard d'analytics" avec Vite

**Fonctionnalités requises :**
- [ ] Interface multi-onglets (Overview, Traffic, Revenue)
- [ ] Graphiques en temps réel avec Chart.js
- [ ] Mode sombre/clair avec persistence
- [ ] Export des données en CSV/JSON
- [ ] Build optimisé avec code splitting

**Structure de fichiers suggérée :**
```
📁 dashboard-analytics/
├── 📄 index.html
├── 📄 vite.config.js
├── 📂 src/
│   ├── 📄 main.js
│   ├── 📂 components/
│   ├── 📂 utils/
│   └── 📂 styles/
└── 📂 public/
```

**Temps estimé :** 75 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [Vite.js Documentation](https://vitejs.dev/)
- [Vite Plugin Development](https://vitejs.dev/guide/api-plugin.html)

### Vidéos recommandées
- "Vite in 100 Seconds" - Fireship (2 min)
- "Why Vite is Better than Webpack" - Traversy Media (20 min)

### Articles approfondis
- "Migrating from Webpack to Vite" - Vue.js Blog
- "Vite vs Webpack: Bundle Size Comparison" - Performance Studies

### Outils de pratique
- [Vite Templates](https://github.com/vitejs/awesome-vite#templates)
- [Vite Plugin Hub](https://github.com/vitejs/awesome-vite#plugins)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Pourquoi Vite est plus rapide que Webpack en développement ?
   - **Réponse :** Utilise les modules ES natifs, pas de bundling en dev

2. **Question pratique :** Que fait cette configuration ?
   ```javascript
   server: { proxy: { '/api': 'http://localhost:8080' } }
   ```
   - **Réponse :** Redirige les appels /api vers un serveur backend

3. **Question d'application :** Quand utiliser Vite vs Webpack ?
   - **Réponse :** Vite pour nouveaux projets/rapidité, Webpack pour legacy/config complexe

### Checklist de maîtrise
- [ ] Je comprends l'architecture de Vite
- [ ] Je configure un projet from scratch
- [ ] Je gère les assets et imports
- [ ] J'utilise le HMR efficacement
- [ ] J'optimise le build de production
- [ ] Je développe des plugins personnalisés

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 - Import paths incorrects
**Code problématique :**
```javascript
import Component from './component'; // Sans extension
```

**Message d'erreur :** `Failed to resolve import`

**Solution :**
```javascript
import Component from './component.js'; // Avec extension
// Ou configurer les alias dans vite.config.js
```

**Explication :** Vite respecte les standards ES modules qui nécessitent les extensions.

### Erreur fréquente 2 - Variables d'environnement
**Code problématique :**
```javascript
const apiUrl = process.env.REACT_APP_API_URL; // Undefined
```

**Solution :**
```javascript
const apiUrl = import.meta.env.VITE_API_URL; // Format Vite
```

**Explication :** Vite utilise `import.meta.env` avec préfixe `VITE_`.

### Erreur fréquente 3 - Assets non trouvés
**Code problématique :**
```javascript
import logo from '../assets/logo.png'; // En production peut échouer
```

**Solution :**
```javascript
import logo from '../assets/logo.png?url'; // Forcer URL
// Ou utiliser le dossier public/
```

**Explication :** Configurer la gestion des assets selon le besoin (inline vs URL).

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Vite révolutionne le développement avec sa rapidité
- ESBuild et modules ES natifs sont les clés de performance
- Configuration simple mais puissante avec plugins

### Questions à approfondir
- Comment créer des plugins Vite complexes ?
- Optimisations avancées pour très gros projets ?

### Révision programmée
- [ ] Révision dans 1 jour (configuration de base)
- [ ] Révision dans 1 semaine (projet complet)
- [ ] Révision dans 1 mois (optimisations avancées)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #vite #bundler #build-tool #hmr #esbuild #performance #avance

**Temps de completion :** ___

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 142/126*
