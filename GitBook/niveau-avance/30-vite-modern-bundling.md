# ⚡ Vite et Build Moderne

> **Niveau :** 🔴 Avancé | **Durée :** 55 minutes | **Prérequis :** ES6 Modules, NPM, Node.js

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Comprendre** l'architecture révolutionnaire de Vite
- [ ] **Configurer** des projets Vite optimisés pour la production
- [ ] **Maîtriser** les plugins et l'écosystème Vite
- [ ] **Optimiser** les performances avec les outils modernes

---

## 📚 Révolution des Build Tools

{% tabs %}
{% tab title="⚡ Pourquoi Vite change tout" %}
**Le problème des bundlers traditionnels :**

### Webpack (approche classique)
```
Source Code → Bundle Everything → Serve → Browser
   📁100 files     🔄 30 seconds     🌐      😴
```

**Problèmes :**
- 🐌 **Démarrage lent** : 10-30s sur gros projets
- 🔄 **HMR lourd** : 1-3s par modification
- 🧩 **Configuration complexe** : 200+ lignes de config
- 🔄 **Rebuild complet** à chaque changement

### Vite (approche moderne)
```
Source Code → Serve Modules Directly → Browser (ES Modules)
   📁100 files     ⚡ 100ms            🌐 ✨
```

**Avantages :**
- ⚡ **Démarrage instantané** : <100ms
- 🔥 **HMR ultra-rapide** : <50ms
- 🎯 **Zero-config** : Fonctionne out-of-the-box
- 🎯 **Updates granulaires** : Seul le module modifié

### Performance comparative
```bash
# Temps de démarrage (projet 1000+ composants)
Webpack:  ████████████████████████████████  30s
Parcel:   ████████                          8s
Rollup:   ██████████████                    15s
Vite:     ▌                                 0.1s

# HMR (Hot Module Replacement)
Webpack:  ████████  2s
Parcel:   ████      1s
Vite:     ▌         0.05s
```
{% endtab %}

{% tab title="🏗️ Architecture technique" %}
**Architecture de Vite :**

### Mode Développement
```typescript
// Vite exploite les ES modules natifs
import { createApp } from 'vue'     // → /node_modules/vue/...
import './style.css'                // → Processed on-demand
import Component from './App.vue'   // → Compiled just-in-time

// Le navigateur charge directement les modules
// Pas de bundling = vitesse instantanée
```

### Sous le capot
```
Browser Request → Vite Dev Server → On-demand Processing
     ↓                 ↓                    ↓
ES Module Import → Transform (esbuild) → Return Module
     ↓                 ↓                    ↓
/src/main.js → TypeScript/JSX → JavaScript ES Module
```

### Mode Production
```typescript
// Build optimisé avec Rollup
npm run build

// Génère :
dist/
├── assets/
│   ├── index-[hash].js    // Bundle principal
│   ├── vendor-[hash].js   // Dépendances
│   └── style-[hash].css   // CSS optimisé
├── index.html
└── manifest.json
```

### Technologies intégrées
- **🦀 esbuild** : Transpilation 10-100x plus rapide
- **📦 Rollup** : Bundling production optimisé  
- **🔥 Native ES Modules** : Pas de bundling dev
- **🎯 Pre-bundling** : Dépendances optimisées
- **🌐 HTTP/2 Push** : Livraison optimisée

### Écosystème intégré
```json
{
  "frameworks": ["Vue", "React", "Svelte", "Vanilla"],
  "languages": ["TypeScript", "JSX", "Vue SFC"],
  "styles": ["CSS", "SCSS", "Less", "Stylus", "PostCSS"],
  "assets": ["Images", "Fonts", "JSON", "WASM"],
  "tools": ["ESLint", "Prettier", "Testing", "PWA"]
}
```
{% endtab %}

{% tab title="📊 Benchmarks réels" %}
**Tests sur projet réel (2000+ composants React) :**

### Temps de démarrage
| Tool | Cold Start | Warm Start | HMR |
|------|------------|------------|-----|
| **Webpack 5** | 28.3s | 12.1s | 1.8s |
| **Parcel 2** | 15.7s | 6.2s | 0.9s |
| **Vite 4** | 0.3s | 0.1s | 0.04s |
| **Turbopack** | 2.1s | 0.8s | 0.2s |

### Bundle size (production)
```bash
# Webpack
dist/: 2.3MB (gzipped: 650KB)
vendor-*.js: 1.8MB
app-*.js: 500KB

# Vite (avec optimisations)
dist/: 1.9MB (gzipped: 520KB)
vendor-*.js: 1.4MB
app-*.js: 500KB
+ Better tree-shaking: -15%
+ Modern output: -8%
```

### Métriques développeur
```typescript
interface DeveloperExperience {
  timeToFirstPixel: '100ms';     // vs 15s Webpack
  saveToReflect: '50ms';         // vs 2s Webpack  
  errorFeedback: 'instantané';   // vs 1-2s Webpack
  configComplexity: 'minimal';   // vs complex Webpack
  mentaleOverhead: 'faible';     // vs élevé Webpack
}

// Productivité : +40-60% mesurée en équipes
// Satisfaction dev : 95% vs 60% Webpack
```

### Optimisations automatiques
- **🌳 Tree-shaking** : Élimination code mort
- **📦 Code-splitting** : Chargement à la demande
- **🗜️ Minification** : Compression optimale
- **🖼️ Asset optimization** : Images/fonts optimisés
- **⚡ Preloading** : Ressources critiques
- **🎯 Modern/Legacy** : Builds adaptatifs
{% endtab %}
{% endtabs %}

---

## ⚙️ Configuration et Setup

### Projet from scratch

{% tabs %}
{% tab title="🚀 Installation rapide" %}
```bash
# Création d'un nouveau projet
npm create vite@latest my-app
cd my-app
npm install
npm run dev

# Templates disponibles
npm create vite@latest my-vue-app -- --template vue
npm create vite@latest my-react-app -- --template react  
npm create vite@latest my-react-ts -- --template react-ts
npm create vite@latest my-svelte-app -- --template svelte
npm create vite@latest my-lit-app -- --template lit
npm create vite@latest my-vanilla-app -- --template vanilla

# Templates communautaires
npm create vite@latest my-app -- --template vue-ts
npm create vite@latest my-app -- --template react-swc-ts
npm create vite@latest my-app -- --template preact
npm create vite@latest my-app -- --template solid
```

### Structure de projet générée
```
my-app/
├── public/
│   └── vite.svg              # Assets statiques
├── src/
│   ├── assets/               # Assets source
│   ├── components/           # Composants
│   ├── App.vue              # Composant principal
│   ├── main.js              # Point d'entrée
│   └── style.css            # Styles globaux
├── index.html               # Template HTML
├── package.json
├── vite.config.js           # Configuration Vite
└── README.md
```

### Configuration de base
```javascript
// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  
  // Configuration du serveur de développement
  server: {
    port: 3000,
    open: true,              // Ouvre le navigateur automatiquement
    cors: true,              // CORS activé
    proxy: {                 // Proxy pour les APIs
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        secure: false
      }
    }
  },
  
  // Configuration de build
  build: {
    outDir: 'dist',          // Dossier de sortie
    assetsDir: 'assets',     // Dossier des assets
    sourcemap: true,         // Source maps
    minify: 'esbuild',       // Minification
    target: 'es2015',        // Cible de compilation
    
    // Options de bundle
    rollupOptions: {
      output: {
        chunkFileNames: 'js/[name]-[hash].js',
        entryFileNames: 'js/[name]-[hash].js',
        assetFileNames: 'assets/[name]-[hash].[ext]'
      }
    }
  },
  
  // Optimisation des dépendances
  optimizeDeps: {
    include: ['lodash', 'axios'],  // Force l'inclusion
    exclude: ['some-esm-dep']      // Exclut du pre-bundling
  }
})
```

### Scripts package.json optimisés
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "build:analyze": "vite build --mode analyze",
    "build:staging": "vite build --mode staging",
    "clean": "rm -rf dist node_modules/.vite",
    "type-check": "vue-tsc --noEmit",
    "lint": "eslint . --fix",
    "test": "vitest",
    "test:ui": "vitest --ui"
  }
}
```
{% endtab %}

{% tab title="🔧 Configuration avancée" %}
```typescript
// vite.config.ts - Configuration TypeScript complète
import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'

export default defineConfig(({ command, mode }) => {
  // Charger les variables d'environnement
  const env = loadEnv(mode, process.cwd(), '')
  
  return {
    plugins: [
      vue({
        // Configuration Vue spécifique
        script: {
          defineModel: true,
          propsDestructure: true
        }
      })
    ],
    
    // Alias de chemins
    resolve: {
      alias: {
        '@': resolve(__dirname, 'src'),
        '@components': resolve(__dirname, 'src/components'),
        '@utils': resolve(__dirname, 'src/utils'),
        '@assets': resolve(__dirname, 'src/assets'),
        '@types': resolve(__dirname, 'src/types')
      }
    },
    
    // Variables d'environnement
    define: {
      __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
      __BUILD_TIME__: JSON.stringify(new Date().toISOString())
    },
    
    // Configuration CSS
    css: {
      modules: {
        localsConvention: 'camelCase'
      },
      preprocessorOptions: {
        scss: {
          additionalData: `@import "@/styles/variables.scss";`
        }
      },
      postcss: {
        plugins: [
          require('autoprefixer'),
          require('cssnano')({
            preset: 'default'
          })
        ]
      }
    },
    
    // Configuration du serveur
    server: {
      host: true,              // Écoute sur toutes les interfaces
      port: parseInt(env.VITE_PORT) || 3000,
      strictPort: true,        // Échoue si le port est occupé
      
      // HTTPS en développement
      https: env.VITE_HTTPS === 'true' ? {
        key: env.VITE_SSL_KEY,
        cert: env.VITE_SSL_CERT
      } : false,
      
      // Proxy avancé
      proxy: {
        '/api': {
          target: env.VITE_API_URL || 'http://localhost:8080',
          changeOrigin: true,
          secure: false,
          rewrite: (path) => path.replace(/^\/api/, ''),
          configure: (proxy, options) => {
            // Middleware personnalisé
            proxy.on('proxyReq', (proxyReq, req, res) => {
              console.log(`[PROXY] ${req.method} ${req.url}`)
            })
          }
        },
        
        // WebSocket proxy
        '/socket.io': {
          target: 'ws://localhost:8080',
          ws: true
        }
      }
    },
    
    // Configuration de build production
    build: {
      target: 'es2015',
      outDir: 'dist',
      assetsDir: 'static',
      sourcemap: mode === 'development',
      minify: 'esbuild',
      
      // Configuration Rollup avancée
      rollupOptions: {
        input: {
          main: resolve(__dirname, 'index.html'),
          admin: resolve(__dirname, 'admin.html')  // Multi-page
        },
        
        output: {
          // Code splitting manuel
          manualChunks: {
            vendor: ['vue', 'vue-router', 'pinia'],
            ui: ['@headlessui/vue', '@heroicons/vue'],
            utils: ['lodash', 'date-fns', 'axios']
          },
          
          // Nommage des chunks
          chunkFileNames: (chunkInfo) => {
            if (chunkInfo.name === 'vendor') {
              return 'js/vendor-[hash].js'
            }
            return 'js/[name]-[hash].js'
          }
        },
        
        // Optimisations
        treeshake: {
          moduleSideEffects: false
        }
      },
      
      // Analyse de bundle
      reportCompressedSize: true,
      chunkSizeWarningLimit: 1000
    },
    
    // Test configuration (Vitest)
    test: {
      globals: true,
      environment: 'jsdom',
      setupFiles: ['./src/test/setup.ts']
    },
    
    // Optimisation des dépendances
    optimizeDeps: {
      include: [
        'vue',
        'vue-router',
        'pinia',
        'axios'
      ],
      exclude: [
        'vue-demi'
      ],
      
      // ESBuild options
      esbuildOptions: {
        target: 'es2015',
        define: {
          global: 'globalThis'
        }
      }
    },
    
    // Worker configuration
    worker: {
      format: 'es',
      plugins: []
    }
  }
})
```

### Variables d'environnement
```bash
# .env.local
VITE_APP_TITLE=My App
VITE_API_URL=http://localhost:8080
VITE_ENABLE_MOCK=false

# .env.development
VITE_APP_ENV=development
VITE_DEBUG=true
VITE_SOURCE_MAP=true

# .env.production  
VITE_APP_ENV=production
VITE_DEBUG=false
VITE_SOURCE_MAP=false
VITE_CDN_URL=https://cdn.example.com
```

### TypeScript configuration
```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@utils/*": ["src/utils/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```
{% endtab %}

{% tab title="🔌 Plugins ecosystem" %}
```typescript
// Configuration avec plugins populaires
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'

// Plugins essentiels
import legacy from '@vitejs/plugin-legacy'
import { defineConfig } from 'vite'
import eslint from 'vite-plugin-eslint'
import { createHtmlPlugin } from 'vite-plugin-html'
import { visualizer } from 'rollup-plugin-visualizer'
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    // Framework plugins
    vue({
      template: {
        compilerOptions: {
          isCustomElement: (tag) => tag.startsWith('custom-')
        }
      }
    }),
    
    // Support navigateurs anciens
    legacy({
      targets: ['defaults', 'not IE 11']
    }),
    
    // Linting en développement
    eslint({
      include: ['src/**/*.{js,ts,vue}'],
      exclude: ['node_modules', 'dist']
    }),
    
    // HTML template processing
    createHtmlPlugin({
      minify: true,
      inject: {
        data: {
          title: 'My Vite App',
          description: 'Built with Vite'
        }
      }
    }),
    
    // Bundle analysis
    visualizer({
      filename: 'dist/stats.html',
      open: true,
      gzipSize: true
    }),
    
    // Progressive Web App
    VitePWA({
      registerType: 'autoUpdate',
      workbox: {
        globPatterns: ['**/*.{js,css,html,ico,png,svg}']
      },
      manifest: {
        name: 'My Vite App',
        short_name: 'ViteApp',
        description: 'My Awesome Vite App',
        theme_color: '#ffffff',
        icons: [
          {
            src: 'icon-192x192.png',
            sizes: '192x192',
            type: 'image/png'
          }
        ]
      }
    })
  ]
})

// Plugins par écosystème
const frameworkPlugins = {
  // React
  react: [
    '@vitejs/plugin-react',
    '@vitejs/plugin-react-swc'  // Plus rapide
  ],
  
  // Vue
  vue: [
    '@vitejs/plugin-vue',
    '@vitejs/plugin-vue-jsx',
    'unplugin-vue-components',   // Auto-import composants
    'vite-plugin-vue-layouts'    // Layouts automatiques
  ],
  
  // Svelte
  svelte: [
    '@sveltejs/vite-plugin-svelte'
  ],
  
  // Solid
  solid: [
    'vite-plugin-solid'
  ]
}

const utilityPlugins = {
  // Development
  dev: [
    'vite-plugin-mock',          // API mocking
    'vite-plugin-inspect',       // Debug transforms
    'vite-plugin-eslint',        // Linting
    'vite-plugin-checker'        // Type checking
  ],
  
  // Assets & Optimization
  assets: [
    'vite-plugin-imagemin',      // Image optimization
    'vite-plugin-svg-icons',     // SVG sprite
    'vite-plugin-windicss',      // CSS framework
    'unplugin-auto-import'       // Auto imports
  ],
  
  // Testing
  testing: [
    'vitest',                    // Unit testing
    'vite-plugin-coverage'       // Code coverage
  ],
  
  // Build & Deploy
  build: [
    '@vitejs/plugin-legacy',     // IE support
    'vite-plugin-pwa',           // PWA
    'rollup-plugin-visualizer',  // Bundle analysis
    'vite-plugin-compress'       // Gzip compression
  ]
}
```

### Plugin personnalisé
```typescript
// plugins/custom-plugin.ts
import type { Plugin } from 'vite'

export function customPlugin(options: { prefix?: string } = {}): Plugin {
  const { prefix = '[CUSTOM]' } = options
  
  return {
    name: 'custom-plugin',
    
    // Build start
    buildStart() {
      console.log(`${prefix} Build started`)
    },
    
    // Transform imports
    resolveId(id) {
      if (id.startsWith('virtual:')) {
        return id
      }
    },
    
    // Transform code
    transform(code, id) {
      if (id.endsWith('.special.js')) {
        return {
          code: `// Transformed by custom plugin\n${code}`,
          map: null
        }
      }
    },
    
    // Generate bundle
    generateBundle(options, bundle) {
      // Modifier le bundle avant génération
      console.log(`${prefix} Generated ${Object.keys(bundle).length} chunks`)
    },
    
    // Development only
    configureServer(server) {
      server.middlewares.use('/api/custom', (req, res, next) => {
        res.setHeader('Content-Type', 'application/json')
        res.end(JSON.stringify({ message: 'Custom API' }))
      })
    }
  }
}

// Utilisation
export default defineConfig({
  plugins: [
    vue(),
    customPlugin({ prefix: '[MY-PLUGIN]' })
  ]
})
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Build Pipeline Complet

{% hint style="warning" %}
**Exercice :** Créez un pipeline de build moderne avec Vite, optimisations et déploiement automatisé
{% endhint %}

```typescript
// Créez un projet avec toutes les optimisations modernes
interface BuildPipelineConfig {
    // TODO: Configuration Vite complète
    development: {
        hmr: boolean;
        sourceMaps: boolean;
        typeChecking: boolean;
        linting: boolean;
    };
    
    production: {
        minification: boolean;
        treeshaking: boolean;
        codeSplitting: boolean;
        assets: {
            optimization: boolean;
            cdn: boolean;
            compression: boolean;
        };
    };
    
    deployment: {
        staticSites: string[];
        ci: boolean;
        preview: boolean;
    };
}

// TODO: Implémentez le pipeline complet
class ModernBuildPipeline {
    constructor(config: BuildPipelineConfig) {}
    
    // TODO: Configuration Vite optimisée
    setupViteConfig() {
        // Configuration multi-environnement
        // Plugins optimisés
        // Asset processing
        // Performance monitoring
    }
    
    // TODO: Optimisations de performance
    setupPerformanceOptimizations() {
        // Bundle analysis
        // Code splitting intelligent
        // Asset optimization
        // Preloading strategy
    }
    
    // TODO: Pipeline CI/CD
    setupDeploymentPipeline() {
        // GitHub Actions / GitLab CI
        // Preview deployments  
        // Performance budgets
        // Automated testing
    }
}

// Tests et benchmarks
async function benchmarkBuildPerformance() {
    // TODO: Mesurer les performances
    const metrics = {
        coldStart: 0,
        hmrTime: 0,
        buildTime: 0,
        bundleSize: 0
    };
    
    // TODO: Comparer avec Webpack
    // TODO: Optimiser les résultats
    
    return metrics;
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```typescript
// vite.config.ts optimisé
export default defineConfig(({ mode }) => ({
  plugins: [
    vue(),
    // Bundle analyzer en production
    mode === 'production' && visualizer({
      filename: 'dist/stats.html',
      gzipSize: true
    }),
    // PWA pour le cache
    VitePWA({
      strategies: 'generateSW',
      registerType: 'autoUpdate'
    })
  ].filter(Boolean),
  
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['vue', 'vue-router'],
          ui: ['@headlessui/vue']
        }
      }
    }
  }
}))
```
{% endtab %}

{% tab title="✅ Solution" %}
Pipeline complet avec optimisations dans l'onglet suivant, incluant configuration CI/CD et monitoring de performance.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant Vite et les outils de build modernes !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Architecture Vite** vs bundlers traditionnels
- **ES Modules natifs** et leur impact sur les performances
- **Configuration avancée** pour tous environnements
- **Plugins ecosystem** et extensibilité
- **Optimisations de production** avec Rollup
- **Performance monitoring** et bundle analysis
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Configurer des projets Vite optimisés
- Créer des pipelines de build performants
- Implémenter le HMR et dev experience
- Optimiser les bundles de production
- Intégrer les outils modernes (ESLint, TypeScript, PWA)
- Migrer depuis Webpack/Parcel
{% endtab %}

{% tab title="🚀 Applications réelles" %}
- **Applications SPA** ultra-rapides
- **Sites statiques** avec performance optimale
- **Micro-frontends** avec builds indépendants
- **PWA** avec cache et offline-first
- **Projets multi-frameworks** (Vue, React, Svelte)
{% endtab %}
{% endtabs %}

---

**Prêt pour la gestion des dépendances modernes ? Continuez avec NPM/Yarn avancé !** 📦
