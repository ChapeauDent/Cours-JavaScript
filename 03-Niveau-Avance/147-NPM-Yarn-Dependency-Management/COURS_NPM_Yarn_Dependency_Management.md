# NPM et Yarn - Gestion des Dépendances

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** NPM & Yarn - Gestionnaires de Dépendances JavaScript
- **Niveau :** 🔴 Avancé
- **Durée estimée :** 50 minutes
- **Prérequis :** Node.js, Terminal/CLI, Concepts de modules
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Différences entre NPM, Yarn et pnpm
- [ ] **Appliquer** : Gérer des dépendances de projet professionnel
- [ ] **Analyser** : Résoudre les conflits de versions
- [ ] **Créer** : Scripts personnalisés et workflows automatisés

### Mots-clés à retenir
- **NPM** : Node Package Manager, gestionnaire par défaut
- **Yarn** : Gestionnaire rapide avec lockfile amélioré
- **package.json** : Manifeste du projet avec métadonnées
- **Lockfile** : Fichier de verrouillage des versions exactes
- **Semantic Versioning** : Convention de versioning (semver)

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
La gestion des dépendances est cruciale pour :
- **Reproducibilité** : Mêmes versions partout (dev, staging, prod)
- **Sécurité** : Audit et mise à jour des vulnérabilités
- **Performance** : Optimisation des bundles et cache
- **Collaboration** : Équipes synchronisées sur les versions
- **Maintenance** : Mises à jour contrôlées et compatibles

**Problème résolu** : "Ça marche sur ma machine" → Environnements identiques partout !

### Différences entre gestionnaires

| Feature | NPM | Yarn Classic | Yarn Berry | pnpm |
|---------|-----|-------------|-------------|------|
| **Vitesse install** | 🟡 Moyen | ✅ Rapide | ✅ Très rapide | ✅ Ultra-rapide |
| **Lockfile** | ✅ package-lock.json | ✅ yarn.lock | ✅ yarn.lock | ✅ pnpm-lock.yaml |
| **Cache global** | ❌ Non | ✅ Oui | ✅ Avancé | ✅ Intelligent |
| **Monorepo** | 🟡 Workspaces | ✅ Workspaces | ✅ Natif | ✅ Excellent |
| **Plug'n'Play** | ❌ Non | ❌ Non | ✅ Oui | ❌ Non |
| **Disk usage** | 🔴 Élevé | 🟡 Moyen | 🟡 Moyen | ✅ Minimal |

### Architecture de package.json

```json
{
  "name": "mon-projet",                 // Nom unique
  "version": "1.2.3",                   // Semantic versioning
  "description": "Description courte",   // Métadonnée
  "main": "index.js",                   // Point d'entrée
  "type": "module",                     // ESM vs CommonJS
  "scripts": {                          // Commandes personnalisées
    "start": "node index.js",
    "dev": "nodemon index.js",
    "build": "webpack --mode production",
    "test": "jest",
    "lint": "eslint src/"
  },
  "dependencies": {                     // Production
    "express": "^4.18.0",
    "lodash": "~4.17.21"
  },
  "devDependencies": {                  // Développement uniquement
    "nodemon": "^3.0.0",
    "jest": "^29.0.0"
  },
  "peerDependencies": {                 // Dépendances de l'hôte
    "react": ">=16.8.0"
  },
  "engines": {                          // Versions Node/NPM
    "node": ">=18.0.0",
    "npm": ">=8.0.0"
  },
  "repository": {                       // Git repo
    "type": "git",
    "url": "https://github.com/user/repo.git"
  },
  "keywords": ["javascript", "api"],    // Tags de recherche
  "author": "Votre Nom <email@example.com>",
  "license": "MIT"                      // Licence open source
}
```

### Semantic Versioning (SemVer)

**Format :** `MAJOR.MINOR.PATCH`

```
1.2.3
│ │ │
│ │ └─ PATCH: Bug fixes (compatible)
│ └─── MINOR: Nouvelles features (compatible)
└───── MAJOR: Breaking changes (incompatible)
```

**Préfixes de version :**
- `^1.2.3` : Compatible minor (≥1.2.3 <2.0.0)
- `~1.2.3` : Compatible patch (≥1.2.3 <1.3.0)
- `1.2.3` : Version exacte
- `*` ou `latest` : Dernière version

### Lockfiles et déterminisme

**Sans lockfile :**
```
Developer A: lodash@^4.17.20 → installe 4.17.21
Developer B: lodash@^4.17.20 → installe 4.17.23 (plus récent)
→ Environnements différents !
```

**Avec lockfile :**
```
package-lock.json/yarn.lock contient:
lodash@4.17.21 + toutes ses sous-dépendances
→ Installation identique partout !
```

### Points d'attention
- **Security audit** : `npm audit` régulièrement
- **Node version** : Garder compatible avec team
- **Lockfile** : Toujours commiter (package-lock.json/yarn.lock)
- **Cache cleaning** : En cas de problèmes étranges

---

## **PARTIE PRATIQUE**

### Exemple simple (NPM de base)
```bash
# Création d'un nouveau projet
mkdir mon-projet
cd mon-projet
npm init -y  # -y pour accepter les valeurs par défaut

# Installation de dépendances
npm install express          # Production dependency
npm install --save-dev jest  # Development dependency
npm install -g nodemon       # Installation globale

# Utilisation des packages
npm start                    # Exécute script "start"
npm run dev                  # Exécute script "dev"
npm test                     # Exécute script "test"
```

**Package.json généré :**
```json
{
  "name": "mon-projet",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest",
    "build": "echo \"No build specified\"",
    "lint": "eslint src/ --fix"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1"
  },
  "devDependencies": {
    "jest": "^29.6.2",
    "nodemon": "^3.0.1",
    "eslint": "^8.45.0"
  },
  "keywords": ["node", "api"],
  "author": "Votre Nom",
  "license": "MIT"
}
```

**Application Express simple :**
```javascript
// index.js
const express = require('express');
const cors = require('cors');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 3000;

// Middlewares
app.use(cors());
app.use(express.json());

// Routes
app.get('/', (req, res) => {
  res.json({
    message: 'API démarrée avec succès !',
    version: process.env.npm_package_version,
    environment: process.env.NODE_ENV || 'development'
  });
});

app.get('/api/health', (req, res) => {
  res.json({
    status: 'OK',
    timestamp: new Date().toISOString(),
    uptime: process.uptime()
  });
});

// Gestion d'erreurs
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({
    error: 'Something broke!',
    message: process.env.NODE_ENV === 'development' ? err.message : undefined
  });
});

app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
  console.log(`📦 NPM version: ${process.env.npm_version}`);
});
```

**Scripts utiles :**
```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "dev:debug": "nodemon --inspect index.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "lint": "eslint src/ --fix",
    "format": "prettier --write src/",
    "build": "webpack --mode production",
    "clean": "rm -rf dist/ node_modules/",
    "fresh-install": "npm run clean && npm install",
    "audit-fix": "npm audit fix",
    "outdated": "npm outdated",
    "size-check": "bundlesize"
  }
}
```

### Exemple intermédiaire (Yarn + Workspaces)
```bash
# Installation de Yarn
npm install -g yarn

# Création d'un monorepo
mkdir mon-monorepo
cd mon-monorepo
yarn init -y
```

**Structure monorepo :**
```
mon-monorepo/
├── package.json           # Root package
├── yarn.lock             # Lockfile partagé
├── packages/
│   ├── frontend/         # App React
│   │   ├── package.json
│   │   └── src/
│   ├── backend/          # API Node
│   │   ├── package.json
│   │   └── src/
│   └── shared/           # Utilities partagées
│       ├── package.json
│       └── src/
└── scripts/              # Scripts de build
```

**package.json root :**
```json
{
  "name": "mon-monorepo",
  "version": "1.0.0",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "dev": "yarn workspaces run dev",
    "build": "yarn workspaces run build",
    "test": "yarn workspaces run test",
    "lint": "yarn workspaces run lint",
    "clean": "yarn workspaces run clean",
    "install-all": "yarn install --frozen-lockfile",
    "frontend": "yarn workspace frontend",
    "backend": "yarn workspace backend",
    "shared": "yarn workspace shared"
  },
  "devDependencies": {
    "lerna": "^7.1.4",
    "concurrently": "^8.2.0",
    "cross-env": "^7.0.3"
  }
}
```

**packages/frontend/package.json :**
```json
{
  "name": "frontend",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "shared": "workspace:*"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.0.3",
    "vite": "^4.4.5",
    "vitest": "^0.34.1"
  }
}
```

**packages/backend/package.json :**
```json
{
  "name": "backend",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "nodemon src/index.js",
    "start": "node src/index.js",
    "test": "jest",
    "build": "echo 'No build for backend'"
  },
  "dependencies": {
    "express": "^4.18.2",
    "shared": "workspace:*"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.6.2"
  }
}
```

**Commandes Yarn Workspaces :**
```bash
# Installation des dépendances pour tous les packages
yarn install

# Exécuter une commande dans un workspace spécifique
yarn workspace frontend add axios
yarn workspace backend add mongoose

# Exécuter une commande dans tous les workspaces
yarn workspaces run test
yarn workspaces run build

# Ajouter une dépendance au root
yarn add -W husky lint-staged

# Information sur les workspaces
yarn workspaces info
```

### Exemple avancé (Gestion avancée)
```javascript
// scripts/dependency-manager.js
const fs = require('fs');
const { execSync } = require('child_process');
const path = require('path');

class DependencyManager {
  constructor(projectRoot = process.cwd()) {
    this.projectRoot = projectRoot;
    this.packageJsonPath = path.join(projectRoot, 'package.json');
    this.packageJson = require(this.packageJsonPath);
  }

  // Analyser les dépendances obsolètes
  async checkOutdated() {
    console.log('🔍 Vérification des dépendances obsolètes...');
    
    try {
      const result = execSync('npm outdated --json', { encoding: 'utf8' });
      const outdated = JSON.parse(result);
      
      Object.entries(outdated).forEach(([pkg, info]) => {
        console.log(`📦 ${pkg}: ${info.current} → ${info.latest}`);
      });
      
      return outdated;
    } catch (error) {
      console.log('✅ Toutes les dépendances sont à jour');
      return {};
    }
  }

  // Audit de sécurité
  async auditSecurity() {
    console.log('🔒 Audit de sécurité...');
    
    try {
      const result = execSync('npm audit --json', { encoding: 'utf8' });
      const audit = JSON.parse(result);
      
      if (audit.vulnerabilities && Object.keys(audit.vulnerabilities).length > 0) {
        console.log(`⚠️ ${Object.keys(audit.vulnerabilities).length} vulnérabilités trouvées`);
        
        Object.entries(audit.vulnerabilities).forEach(([pkg, vuln]) => {
          console.log(`- ${pkg}: ${vuln.severity} (${vuln.via.join(', ')})`);
        });
        
        return audit;
      } else {
        console.log('✅ Aucune vulnérabilité détectée');
        return null;
      }
    } catch (error) {
      console.error('❌ Erreur lors de l\'audit:', error.message);
    }
  }

  // Analyser la taille des dépendances
  analyzeBundleSize() {
    console.log('📊 Analyse de la taille des dépendances...');
    
    const dependencies = this.packageJson.dependencies || {};
    const sizes = {};
    
    Object.keys(dependencies).forEach(dep => {
      try {
        const depPath = path.join(this.projectRoot, 'node_modules', dep);
        const stats = this.getDirSize(depPath);
        sizes[dep] = {
          size: stats,
          sizeFormatted: this.formatBytes(stats)
        };
      } catch (error) {
        sizes[dep] = { size: 0, sizeFormatted: 'N/A' };
      }
    });
    
    // Trier par taille
    const sorted = Object.entries(sizes)
      .sort(([,a], [,b]) => b.size - a.size)
      .slice(0, 10);
    
    console.log('\n📦 Top 10 des plus grosses dépendances:');
    sorted.forEach(([pkg, info]) => {
      console.log(`  ${pkg}: ${info.sizeFormatted}`);
    });
    
    return sizes;
  }

  // Nettoyer les dépendances inutilisées
  async findUnusedDependencies() {
    console.log('🧹 Recherche des dépendances inutilisées...');
    
    try {
      // Utilise depcheck pour trouver les dépendances inutilisées
      const depcheck = require('depcheck');
      
      const result = await depcheck(this.projectRoot, {
        ignoreBinPackage: true,
        skipMissing: false
      });
      
      if (result.dependencies.length > 0) {
        console.log('🗑️ Dépendances probablement inutilisées:');
        result.dependencies.forEach(dep => {
          console.log(`  - ${dep}`);
        });
      } else {
        console.log('✅ Aucune dépendance inutilisée détectée');
      }
      
      return result;
    } catch (error) {
      console.log('⚠️ Installation de depcheck nécessaire: npm install -g depcheck');
    }
  }

  // Mettre à jour de façon sécurisée
  async safeUpdate(packages = []) {
    console.log('🔄 Mise à jour sécurisée...');
    
    // Backup du package.json
    const backup = JSON.stringify(this.packageJson, null, 2);
    fs.writeFileSync(this.packageJsonPath + '.backup', backup);
    
    try {
      if (packages.length === 0) {
        // Mise à jour patch uniquement (safe)
        execSync('npm update --save', { stdio: 'inherit' });
      } else {
        // Mise à jour de packages spécifiques
        packages.forEach(pkg => {
          console.log(`🔄 Updating ${pkg}...`);
          execSync(`npm install ${pkg}@latest`, { stdio: 'inherit' });
        });
      }
      
      // Test après mise à jour
      console.log('🧪 Test après mise à jour...');
      execSync('npm test', { stdio: 'inherit' });
      
      // Supprimer le backup si succès
      fs.unlinkSync(this.packageJsonPath + '.backup');
      console.log('✅ Mise à jour réussie');
      
    } catch (error) {
      console.error('❌ Erreur pendant la mise à jour');
      
      // Restaurer le backup
      fs.writeFileSync(this.packageJsonPath, backup);
      console.log('🔄 Package.json restauré depuis le backup');
      
      throw error;
    }
  }

  // Utilitaires
  getDirSize(dirPath) {
    let size = 0;
    
    function calculateSize(itemPath) {
      const stats = fs.statSync(itemPath);
      
      if (stats.isFile()) {
        size += stats.size;
      } else if (stats.isDirectory()) {
        const items = fs.readdirSync(itemPath);
        items.forEach(item => {
          calculateSize(path.join(itemPath, item));
        });
      }
    }
    
    if (fs.existsSync(dirPath)) {
      calculateSize(dirPath);
    }
    
    return size;
  }

  formatBytes(bytes) {
    if (bytes === 0) return '0 Bytes';
    const k = 1024;
    const sizes = ['Bytes', 'KB', 'MB', 'GB'];
    const i = Math.floor(Math.log(bytes) / Math.log(k));
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
  }
}

// Utilisation
const manager = new DependencyManager();

async function runAnalysis() {
  await manager.checkOutdated();
  await manager.auditSecurity();
  manager.analyzeBundleSize();
  await manager.findUnusedDependencies();
}

// Export pour utilisation en tant que module
module.exports = { DependencyManager };

// Exécution directe
if (require.main === module) {
  runAnalysis().catch(console.error);
}
```

**Scripts package.json avancés :**
```json
{
  "scripts": {
    "preinstall": "node scripts/check-node-version.js",
    "postinstall": "node scripts/post-install.js",
    "precommit": "lint-staged",
    "prepush": "npm run test",
    "release": "npm run test && npm run build && npm version patch && npm publish",
    "analyze-deps": "node scripts/dependency-manager.js",
    "security-check": "npm audit && npm run analyze-deps",
    "clean-install": "rm -rf node_modules package-lock.json && npm install",
    "dep-check": "depcheck",
    "bundle-analyze": "npm run build && npx webpack-bundle-analyzer build/static/js/*.js"
  }
}
```

**Configuration .npmrc avancée :**
```ini
# .npmrc - Configuration NPM
registry=https://registry.npmjs.org/
cache=/tmp/npm-cache
init-author-name=Votre Nom
init-author-email=email@example.com
init-license=MIT
save-exact=true
fund=false
audit-level=moderate
progress=true
timing=true

# Pour entreprise avec registry privé
# registry=https://npm.company.com/
# //npm.company.com/:_authToken=${NPM_TOKEN}
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Migration NPM vers Yarn
**Consigne :** Migre un projet existant NPM vers Yarn en gardant les mêmes versions

**Projet de départ :**
```json
{
  "name": "migration-test",
  "dependencies": {
    "lodash": "^4.17.21",
    "axios": "~1.4.0",
    "moment": "2.29.4"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "webpack": "^5.80.0"
  }
}
```

**Tâches :**
- [ ] Installer Yarn
- [ ] Migrer le lockfile
- [ ] Vérifier l'équivalence des versions
- [ ] Adapter les scripts CI/CD

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Monorepo avec Workspaces
**Consigne :** Crée un monorepo avec packages partagés

**Structure cible :**
- Frontend React
- Backend Express
- Package partagé avec utilitaires communs
- Scripts de build centralisés

**Contraintes :**
- Hoisting des dépendances communes
- Scripts cross-platform (Windows/Mac/Linux)
- Gestion des versions synchronisées

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Audit automatisé
**Consigne :** Développe un système d'audit automatisé des dépendances

**Fonctionnalités requises :**
- Détection vulnérabilités + solutions
- Rapport de dépendances obsolètes
- Notification Slack/email des résultats
- Intégration CI pour bloquer déploiements à risque

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```bash
# Étapes de migration NPM → Yarn

# 1. Installation de Yarn
npm install -g yarn

# 2. Supprimer le lockfile NPM
rm package-lock.json

# 3. Import des dépendances
yarn import  # Génère yarn.lock à partir de package-lock.json

# 4. Vérification des versions
yarn list --pattern lodash
yarn list --pattern axios

# 5. Installation propre
yarn install --frozen-lockfile

# 6. Test de fonctionnement
yarn test
yarn build
```

**Adaptation des scripts CI :**
```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      # Cache Yarn
      - name: Get yarn cache directory path
        id: yarn-cache-dir-path
        run: echo "dir=$(yarn cache dir)" >> $GITHUB_OUTPUT
      
      - uses: actions/cache@v3
        with:
          path: ${{ steps.yarn-cache-dir-path.outputs.dir }}
          key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
          restore-keys: |
            ${{ runner.os }}-yarn-
      
      # Installation
      - name: Install dependencies
        run: yarn install --frozen-lockfile
      
      # Tests
      - name: Run tests
        run: yarn test
```

**Scripts package.json adaptés :**
```json
{
  "scripts": {
    "install": "yarn install --frozen-lockfile",
    "ci": "yarn install --frozen-lockfile --production=false",
    "upgrade-interactive": "yarn upgrade-interactive --latest"
  }
}
```

### Solution Exercice 2
```json
// package.json root - Configuration monorepo
{
  "name": "monorepo-example",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "dev": "concurrently \"yarn frontend dev\" \"yarn backend dev\"",
    "build": "yarn shared build && yarn frontend build && yarn backend build",
    "test": "yarn workspaces run test",
    "lint": "yarn workspaces run lint",
    "clean": "yarn workspaces run clean && rm -rf node_modules",
    "fresh": "yarn clean && yarn install",
    "release": "lerna version --conventional-commits && lerna publish from-git",
    "frontend": "yarn workspace @monorepo/frontend",
    "backend": "yarn workspace @monorepo/backend",
    "shared": "yarn workspace @monorepo/shared"
  },
  "devDependencies": {
    "concurrently": "^8.2.0",
    "lerna": "^7.1.4",
    "cross-env": "^7.0.3"
  }
}
```

```json
// packages/shared/package.json
{
  "name": "@monorepo/shared",
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch",
    "clean": "rm -rf dist",
    "test": "jest"
  },
  "dependencies": {
    "lodash": "^4.17.21",
    "date-fns": "^2.30.0"
  },
  "devDependencies": {
    "typescript": "^5.1.6",
    "@types/lodash": "^4.14.195",
    "jest": "^29.6.2"
  }
}
```

```javascript
// packages/shared/src/index.ts
import { format } from 'date-fns';
import { camelCase } from 'lodash';

export interface ApiResponse<T = any> {
  data: T;
  success: boolean;
  timestamp: string;
  message?: string;
}

export function createApiResponse<T>(
  data: T,
  success: boolean = true,
  message?: string
): ApiResponse<T> {
  return {
    data,
    success,
    timestamp: format(new Date(), 'yyyy-MM-dd HH:mm:ss'),
    message
  };
}

export function normalizeKey(key: string): string {
  return camelCase(key);
}

export function validateEmail(email: string): boolean {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
}

export * from './constants';
export * from './helpers';
```

```json
// packages/frontend/package.json
{
  "name": "@monorepo/frontend",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "@monorepo/shared": "workspace:*"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.0.3",
    "vite": "^4.4.5"
  }
}
```

```json
// packages/backend/package.json  
{
  "name": "@monorepo/backend",
  "private": true,
  "scripts": {
    "dev": "nodemon src/index.js",
    "start": "node src/index.js",
    "build": "echo 'No build needed for backend'"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "@monorepo/shared": "workspace:*"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

**Utilisation du package partagé :**
```javascript
// packages/backend/src/index.js
const express = require('express');
const { createApiResponse, validateEmail } = require('@monorepo/shared');

const app = express();

app.get('/api/users', (req, res) => {
  const users = [{ id: 1, name: 'John' }];
  res.json(createApiResponse(users));
});

app.post('/api/validate-email', (req, res) => {
  const { email } = req.body;
  const isValid = validateEmail(email);
  res.json(createApiResponse({ isValid }));
});
```

### Solution Exercice 3
```javascript
// scripts/audit-system.js
const fs = require('fs');
const { execSync } = require('child_process');
const axios = require('axios');

class SecurityAuditor {
  constructor(config = {}) {
    this.config = {
      slackWebhook: process.env.SLACK_WEBHOOK_URL,
      emailConfig: {
        smtp: process.env.SMTP_SERVER,
        user: process.env.EMAIL_USER,
        password: process.env.EMAIL_PASSWORD
      },
      thresholds: {
        high: 0,     // Bloquant
        moderate: 5, // Warning
        low: 10      // Info
      },
      ...config
    };
  }

  async runFullAudit() {
    console.log('🔍 Démarrage de l\'audit complet...');
    
    const results = {
      timestamp: new Date().toISOString(),
      vulnerabilities: await this.auditVulnerabilities(),
      outdated: await this.checkOutdated(),
      unused: await this.findUnused(),
      summary: {}
    };

    results.summary = this.generateSummary(results);
    
    // Sauvegarder le rapport
    await this.saveReport(results);
    
    // Notifications
    await this.notify(results);
    
    // Décision CI/CD
    const shouldBlock = this.shouldBlockDeployment(results);
    
    if (shouldBlock) {
      console.error('❌ Déploiement bloqué à cause de vulnérabilités critiques');
      process.exit(1);
    }

    return results;
  }

  async auditVulnerabilities() {
    try {
      const result = execSync('npm audit --json', { encoding: 'utf8' });
      const audit = JSON.parse(result);
      
      return {
        total: audit.metadata?.vulnerabilities?.total || 0,
        high: audit.metadata?.vulnerabilities?.high || 0,
        moderate: audit.metadata?.vulnerabilities?.moderate || 0,
        low: audit.metadata?.vulnerabilities?.low || 0,
        details: audit.vulnerabilities || {}
      };
    } catch (error) {
      // npm audit retourne exit code 1 si vulnérabilités trouvées
      if (error.stdout) {
        const audit = JSON.parse(error.stdout);
        return {
          total: audit.metadata?.vulnerabilities?.total || 0,
          high: audit.metadata?.vulnerabilities?.high || 0,
          moderate: audit.metadata?.vulnerabilities?.moderate || 0,
          low: audit.metadata?.vulnerabilities?.low || 0,
          details: audit.vulnerabilities || {}
        };
      }
      return { total: 0, high: 0, moderate: 0, low: 0, details: {} };
    }
  }

  async checkOutdated() {
    try {
      const result = execSync('npm outdated --json', { encoding: 'utf8' });
      const outdated = JSON.parse(result);
      
      return {
        count: Object.keys(outdated).length,
        packages: outdated
      };
    } catch (error) {
      return { count: 0, packages: {} };
    }
  }

  async findUnused() {
    try {
      const depcheck = require('depcheck');
      const result = await depcheck(process.cwd(), {
        ignoreBinPackage: true,
        skipMissing: false
      });
      
      return {
        count: result.dependencies.length,
        packages: result.dependencies
      };
    } catch (error) {
      return { count: 0, packages: [] };
    }
  }

  generateSummary(results) {
    const { vulnerabilities, outdated, unused } = results;
    
    const riskLevel = this.calculateRiskLevel(vulnerabilities);
    const recommendations = this.generateRecommendations(results);
    
    return {
      riskLevel,
      totalIssues: vulnerabilities.total + outdated.count + unused.count,
      criticalIssues: vulnerabilities.high,
      recommendations,
      blocksDeployment: this.shouldBlockDeployment(results)
    };
  }

  calculateRiskLevel(vulnerabilities) {
    if (vulnerabilities.high > 0) return 'CRITICAL';
    if (vulnerabilities.moderate > this.config.thresholds.moderate) return 'HIGH';
    if (vulnerabilities.low > this.config.thresholds.low) return 'MEDIUM';
    return 'LOW';
  }

  generateRecommendations(results) {
    const recommendations = [];
    
    if (results.vulnerabilities.high > 0) {
      recommendations.push('🚨 URGENT: Corriger les vulnérabilités critiques avec npm audit fix');
    }
    
    if (results.outdated.count > 5) {
      recommendations.push('📦 Mettre à jour les dépendances obsolètes');
    }
    
    if (results.unused.count > 0) {
      recommendations.push('🧹 Supprimer les dépendances inutilisées pour réduire la surface d\'attaque');
    }
    
    return recommendations;
  }

  shouldBlockDeployment(results) {
    return results.vulnerabilities.high > this.config.thresholds.high;
  }

  async saveReport(results) {
    const reportPath = `audit-reports/audit-${Date.now()}.json`;
    
    // Créer le dossier s'il n'existe pas
    if (!fs.existsSync('audit-reports')) {
      fs.mkdirSync('audit-reports');
    }
    
    fs.writeFileSync(reportPath, JSON.stringify(results, null, 2));
    console.log(`📊 Rapport sauvegardé: ${reportPath}`);
  }

  async notify(results) {
    const message = this.formatNotification(results);
    
    // Notification Slack
    if (this.config.slackWebhook) {
      await this.sendSlackNotification(message, results);
    }
    
    // Notification email
    if (this.config.emailConfig.smtp) {
      await this.sendEmailNotification(message, results);
    }
  }

  formatNotification(results) {
    const { summary, vulnerabilities, outdated, unused } = results;
    
    return {
      title: `🔍 Audit de Sécurité - ${summary.riskLevel}`,
      summary: [
        `Vulnérabilités: ${vulnerabilities.total} (${vulnerabilities.high} critiques)`,
        `Dépendances obsolètes: ${outdated.count}`,
        `Dépendances inutilisées: ${unused.count}`,
        `Niveau de risque: ${summary.riskLevel}`
      ].join('\n'),
      recommendations: summary.recommendations.join('\n'),
      blocksDeployment: summary.blocksDeployment
    };
  }

  async sendSlackNotification(message, results) {
    try {
      const color = {
        'CRITICAL': '#ff0000',
        'HIGH': '#ff8800',
        'MEDIUM': '#ffaa00',
        'LOW': '#00ff00'
      }[results.summary.riskLevel];

      const payload = {
        text: message.title,
        attachments: [{
          color,
          fields: [
            {
              title: 'Résumé',
              value: message.summary,
              short: false
            },
            {
              title: 'Recommandations',
              value: message.recommendations || 'Aucune action requise',
              short: false
            }
          ],
          footer: 'Security Audit System',
          ts: Math.floor(Date.now() / 1000)
        }]
      };

      await axios.post(this.config.slackWebhook, payload);
      console.log('✅ Notification Slack envoyée');
    } catch (error) {
      console.error('❌ Erreur notification Slack:', error.message);
    }
  }

  async sendEmailNotification(message, results) {
    // Implémentation avec nodemailer ou service email
    console.log('📧 Notification email simulée');
  }
}

// Utilisation
const auditor = new SecurityAuditor({
  thresholds: {
    high: 0,      // Aucune vulnérabilité critique tolérée
    moderate: 3,  // Max 3 vulnérabilités modérées
    low: 10       // Max 10 vulnérabilités mineures
  }
});

// Export pour utilisation dans CI
module.exports = { SecurityAuditor };

// Exécution directe
if (require.main === module) {
  auditor.runFullAudit()
    .then(results => {
      console.log('✅ Audit terminé');
      console.log(`📊 Résultats: ${results.summary.totalIssues} problèmes trouvés`);
    })
    .catch(error => {
      console.error('❌ Erreur audit:', error);
      process.exit(1);
    });
}
```

**Integration CI/CD GitHub Actions :**
```yaml
# .github/workflows/security-audit.yml
name: Security Audit

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * *'  # Tous les jours à 2h

jobs:
  security-audit:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Install audit dependencies
        run: npm install -g depcheck
      
      - name: Run security audit
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
        run: node scripts/audit-system.js
      
      - name: Upload audit report
        uses: actions/upload-artifact@v3
        if: always()
        with:
          name: security-audit-report
          path: audit-reports/
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Node.js** : Environnement d'exécution pour NPM
- **JSON** : Format de package.json et lockfiles
- **Semantic Versioning** : Convention de gestion des versions

### Prochaines étapes
- **Build Tools** : Webpack, Vite, Rollup avec optimisation deps
- **CI/CD** : Intégration dans pipelines de déploiement
- **Monitoring** : Surveillance continue des vulnérabilités

### Concepts complémentaires
- **Docker** : Conteneurisation avec gestion des dépendances
- **Lerna** : Gestion de monorepos complexes
- **Renovate/Dependabot** : Mise à jour automatisée

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un "Dashboard de gestion des dépendances"

**Fonctionnalités requises :**
- [ ] Interface web pour visualiser l'état des dépendances
- [ ] Graphiques de répartition des vulnérabilités
- [ ] Historique des audits avec tendances
- [ ] Actions de mise à jour en un clic
- [ ] Notification automatique des équipes

**Structure suggérée :**
```
📁 dependency-dashboard/
├── 📂 frontend/          # Interface React
├── 📂 backend/           # API Express
├── 📂 audit-engine/      # Moteur d'audit
├── 📂 shared/            # Types et utilitaires
└── 📄 package.json      # Monorepo config
```

**Temps estimé :** 90 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [NPM Documentation](https://docs.npmjs.com/)
- [Yarn Documentation](https://yarnpkg.com/getting-started)
- [Semantic Versioning](https://semver.org/)

### Vidéos recommandées
- "NPM vs Yarn vs pnpm" - Fireship (8 min)
- "Monorepos with Yarn Workspaces" - Ben Awad (25 min)

### Articles approfondis
- "Understanding package-lock.json" - NPM Blog
- "Security Best Practices for NPM" - Snyk Security

### Outils recommandés
- [npm-check-updates](https://www.npmjs.com/package/npm-check-updates) : Mise à jour interactive
- [bundlesize](https://www.npmjs.com/package/bundlesize) : Contrôle de taille
- [depcheck](https://www.npmjs.com/package/depcheck) : Détection dépendances inutilisées

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre dependencies et devDependencies ?
   - **Réponse :** dependencies = runtime production, devDependencies = développement uniquement

2. **Question pratique :** Que fait `^1.2.3` vs `~1.2.3` ?
   - **Réponse :** ^ permet minor updates (1.x.x), ~ permet patch updates (1.2.x)

3. **Question sécurité :** Pourquoi utiliser un lockfile ?
   - **Réponse :** Garantir versions exactes identiques sur tous les environnements

### Checklist de maîtrise
- [ ] Je comprends semver et les préfixes de version
- [ ] Je gère les dépendances de développement vs production
- [ ] J'utilise les lockfiles correctement
- [ ] Je configure des monorepos avec workspaces
- [ ] J'audite régulièrement la sécurité
- [ ] Je résous les conflits de dépendances

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 - Conflits de versions
**Code problématique :**
```bash
npm ERR! peer dep missing: react@>=16.8.0
```

**Solution :**
```bash
# Installer les peer dependencies manuellement
npm install react@^18.0.0
# Ou utiliser install-peerdeps
npx install-peerdeps package-name
```

**Explication :** Les peer dependencies ne sont pas installées automatiquement.

### Erreur fréquente 2 - Cache corrompu
**Symptôme :** Installation qui échoue mysterieusement

**Solution :**
```bash
# NPM
npm cache clean --force
rm -rf node_modules package-lock.json
npm install

# Yarn
yarn cache clean
rm -rf node_modules yarn.lock
yarn install
```

**Explication :** Le cache peut parfois se corrompre et causer des problèmes.

### Erreur fréquente 3 - Permissions globales
**Code problématique :**
```bash
npm install -g package-name  # EACCES: permission denied
```

**Solution :**
```bash
# Option 1: Changer le dossier global NPM
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH

# Option 2: Utiliser npx
npx package-name

# Option 3: Node Version Manager
nvm install node
```

**Explication :** Éviter sudo avec NPM, utiliser des solutions alternatives.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- La gestion des dépendances va bien au-delà d'un simple `npm install`
- Les lockfiles sont cruciaux pour la reproductibilité
- L'audit de sécurité doit être automatisé et intégré au CI/CD

### Questions à approfondir
- Comment optimiser les performances d'installation en équipe ?
- Stratégies avancées pour les monorepos très complexes ?

### Révision programmée
- [ ] Révision dans 1 jour (commandes de base)
- [ ] Révision dans 1 semaine (configuration avancée)
- [ ] Révision dans 1 mois (audit et sécurité)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #npm #yarn #dependencies #package-manager #security #monorepo #lockfile #avance

**Temps de completion :** ___

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 147/126*
