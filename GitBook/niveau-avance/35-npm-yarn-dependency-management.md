# 📦 NPM & Yarn - Gestion des Dépendances

> **Niveau :** 🔴 Avancé | **Durée :** 50 minutes | **Prérequis :** Node.js, CLI, Modules

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** NPM, Yarn et pnpm pour projets professionnels
- [ ] **Optimiser** la gestion des dépendances et monorepos
- [ ] **Sécuriser** les projets avec audits et policies
- [ ] **Automatiser** les workflows et CI/CD avec package managers

---

## 📚 Écosystème des Package Managers

{% tabs %}
{% tab title="🏆 Comparaison des solutions" %}
**Évolution des package managers JavaScript :**

### Timeline d'innovation
```timeline
2010: NPM 1.0 - Premier package manager Node.js
2016: Yarn 1.0 - Révolution performance + lockfiles
2018: pnpm 2.0 - Optimisation espace disque
2020: Yarn 2.0 (Berry) - Plug'n'Play + workspaces
2024: NPM 10 - Performance améliorée + features modernes
```

### Métriques de performance (projet 500+ dépendances)
| Gestionnaire | Install Time | Disk Usage | Cache Hit | Monorepo |
|--------------|--------------|------------|-----------|----------|
| **NPM 10** | 45s | 180MB | ❌ | 🟡 Workspaces |
| **Yarn 1** | 25s | 150MB | ✅ | ✅ Workspaces |
| **Yarn 3** | 15s | 120MB | ✅ | ✅ Natif |
| **pnpm 8** | 12s | 60MB | ✅ | ✅ Excellent |

### Stratégies d'adoption
```javascript
// Choix par cas d'usage
const packageManagerStrategy = {
  // Projets simples/équipes débutantes
  simple: 'npm',
  
  // Projets moyens/équipes expérimentées  
  medium: 'yarn',
  
  // Monorepos/optimisation espace
  enterprise: 'pnpm',
  
  // Innovation/features bleeding-edge
  cutting_edge: 'yarn berry (PnP)'
};

// Migration progressive
const migrationPath = {
  from_npm: {
    to_yarn: 'rm package-lock.json && yarn install',
    to_pnpm: 'rm package-lock.json && pnpm install'
  },
  
  from_yarn: {
    to_npm: 'rm yarn.lock && npm install',
    to_pnpm: 'rm yarn.lock && pnpm import && pnpm install'
  }
};
```
{% endtab %}

{% tab title="📊 Architecture et concepts" %}
**Structure des projets modernes :**

### Package.json exhaustif
```json
{
  "name": "@company/project-name",
  "version": "2.1.0",
  "description": "Production-ready application",
  "type": "module",
  "main": "./dist/index.js",
  "module": "./dist/index.mjs",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/index.mjs",
      "require": "./dist/index.cjs",
      "types": "./dist/index.d.ts"
    },
    "./utils": {
      "import": "./dist/utils.mjs",
      "require": "./dist/utils.cjs"
    }
  },
  
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "build:lib": "tsup src/index.ts --format cjs,esm --dts",
    "test": "vitest",
    "test:e2e": "playwright test",
    "lint": "eslint . --fix",
    "type-check": "tsc --noEmit",
    "prepare": "husky install",
    "prepack": "npm run build",
    "release": "release-it"
  },
  
  "dependencies": {
    "vue": "^3.3.0",
    "pinia": "^2.1.0"
  },
  
  "devDependencies": {
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0",
    "vite": "^4.0.0",
    "vitest": "^0.34.0"
  },
  
  "peerDependencies": {
    "vue": ">=3.0.0"
  },
  
  "peerDependenciesMeta": {
    "vue": {
      "optional": true
    }
  },
  
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=9.0.0"
  },
  
  "packageManager": "pnpm@8.6.0",
  
  "publishConfig": {
    "access": "public",
    "registry": "https://registry.npmjs.org/"
  }
}
```

### Semantic Versioning avancé
```typescript
interface SemanticVersion {
  major: number;    // Breaking changes
  minor: number;    // New features (backward compatible)
  patch: number;    // Bug fixes
  prerelease?: string; // alpha, beta, rc
  build?: string;   // Build metadata
}

// Exemples de versioning
const versionExamples = {
  exact: "1.2.3",           // Version exacte
  patch: "~1.2.3",          // >=1.2.3 <1.3.0
  minor: "^1.2.3",          // >=1.2.3 <2.0.0
  major: "1.x",             // >=1.0.0 <2.0.0
  range: "1.2.3 - 1.4.0",   // Entre versions
  latest: "*",              // Dernière version
  prerelease: "1.0.0-beta.1" // Pre-release
};

// Stratégies de mise à jour
const updateStrategies = {
  conservative: "~",  // Patches seulement
  balanced: "^",      // Minor + patches
  aggressive: "*",    // Toutes versions
  pinned: "exact"     // Version figée
};
```

### Lockfiles et déterminisme
```yaml
# pnpm-lock.yaml (exemple simplifié)
lockfileVersion: '6.0'

dependencies:
  vue:
    specifier: ^3.3.0
    version: 3.3.4
    
devDependencies:
  typescript:
    specifier: ^5.0.0
    version: 5.1.6

packages:
  /vue@3.3.4:
    resolution: {integrity: sha512-VuE...}
    dependencies:
      '@vue/compiler-dom': 3.3.4
      '@vue/runtime-dom': 3.3.4
    dev: false
    
  /@vue/compiler-dom@3.3.4:
    resolution: {integrity: sha512-wyM...}
    dependencies:
      '@vue/compiler-core': 3.3.4
      '@vue/shared': 3.3.4
    dev: false
```
{% endtab %}

{% tab title="⚡ Optimisations avancées" %}
**Techniques d'optimisation professionnelles :**

### Cache intelligent
```bash
# NPM Cache
npm config set cache ~/.npm-cache
npm cache verify
npm cache clean --force

# Yarn Cache  
yarn config set cache-folder ~/.yarn-cache
yarn cache list
yarn cache clean

# pnpm Store
pnpm config set store-dir ~/.pnpm-store
pnpm store status
pnpm store prune
```

### Installation sélective
```json
{
  "scripts": {
    "install:prod": "npm ci --only=production",
    "install:dev": "npm ci --include=dev",
    "install:fast": "pnpm install --frozen-lockfile --prefer-offline"
  },
  
  "pnpm": {
    "overrides": {
      "lodash": "^4.17.21"
    }
  }
}
```

### Monorepo configuration
```json
// package.json (root)
{
  "name": "monorepo-project",
  "workspaces": [
    "packages/*",
    "apps/*"
  ],
  
  "scripts": {
    "build": "pnpm -r build",
    "test": "pnpm -r test",
    "dev": "pnpm -r --parallel dev"
  }
}

// pnpm-workspace.yaml
packages:
  - 'packages/*'
  - 'apps/*'
  - '!**/test/**'
```

### Performance monitoring
```typescript
// Package analysis
class DependencyAnalyzer {
  async analyzeBundleSize() {
    const bundleInfo = await this.getBundleInfo();
    
    return {
      totalSize: bundleInfo.size,
      gzippedSize: bundleInfo.gzipped,
      chunksCount: bundleInfo.chunks.length,
      
      // Analyse par dépendance
      dependencies: bundleInfo.dependencies.map(dep => ({
        name: dep.name,
        size: dep.size,
        percentage: (dep.size / bundleInfo.size) * 100,
        treeshakable: dep.treeshakable
      }))
    };
  }
  
  async findUnusedDependencies() {
    // Analyse statique du code
    const usedImports = await this.scanImports();
    const declaredDeps = await this.readPackageJson();
    
    return declaredDeps.filter(dep => 
      !usedImports.includes(dep.name)
    );
  }
  
  async checkVulnerabilities() {
    // Audit de sécurité
    const auditResult = await this.runSecurityAudit();
    
    return {
      critical: auditResult.vulnerabilities.critical,
      high: auditResult.vulnerabilities.high,
      fixable: auditResult.fixable,
      recommendations: auditResult.recommendations
    };
  }
}

// Utilisation
const analyzer = new DependencyAnalyzer();

// Scripts d'analyse
{
  "scripts": {
    "analyze:size": "bundlesize",
    "analyze:deps": "depcheck",
    "analyze:duplicates": "yarn-deduplicate --list",
    "analyze:licenses": "license-checker",
    "analyze:outdated": "npm outdated",
    "audit:security": "npm audit --audit-level moderate",
    "audit:fix": "npm audit fix"
  }
}
```

### Optimisation CI/CD
```yaml
# .github/workflows/dependencies.yml
name: Dependencies Management

on:
  schedule:
    - cron: '0 0 * * 1'  # Weekly
  pull_request:
    paths: ['package.json', 'pnpm-lock.yaml']

jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'pnpm'
      
      - name: Install dependencies
        run: pnpm install --frozen-lockfile
      
      - name: Security audit
        run: pnpm audit --audit-level high
      
      - name: Check outdated packages
        run: pnpm outdated
      
      - name: Bundle size analysis
        run: pnpm run analyze:size
        
      - name: License compliance
        run: pnpm run analyze:licenses

  update:
    runs-on: ubuntu-latest
    if: github.event_name == 'schedule'
    steps:
      - name: Update dependencies
        run: |
          pnpm update --interactive
          pnpm dedupe
          
      - name: Create PR
        uses: peter-evans/create-pull-request@v5
        with:
          title: 'chore: update dependencies'
          body: 'Automated dependency updates'
```
{% endtab %}
{% endtabs %}

---

## 🔧 Configuration Avancée

### Scripts et automation

{% tabs %}
{% tab title="🚀 Scripts professionnels" %}
```json
{
  "scripts": {
    // Development
    "dev": "concurrently \"npm:dev:*\"",
    "dev:client": "vite",
    "dev:server": "nodemon server.js",
    "dev:types": "tsc --watch --noEmit",
    
    // Build pipeline
    "build": "npm-run-all clean build:*",
    "build:client": "vite build",
    "build:server": "tsc --project tsconfig.server.json",
    "build:types": "tsc --emitDeclarationOnly",
    
    // Testing
    "test": "npm-run-all test:*",
    "test:unit": "vitest run",
    "test:e2e": "playwright test",
    "test:visual": "chromatic --build-script-name build",
    "test:coverage": "vitest run --coverage",
    
    // Quality assurance
    "lint": "npm-run-all lint:*",
    "lint:eslint": "eslint . --fix",
    "lint:prettier": "prettier . --write",
    "lint:stylelint": "stylelint \"**/*.{css,scss,vue}\" --fix",
    "type-check": "tsc --noEmit",
    
    // Maintenance
    "clean": "rimraf dist coverage .nyc_output",
    "reset": "npm run clean && rimraf node_modules && npm install",
    "update": "npm-check-updates -ui",
    "audit": "npm audit --audit-level moderate",
    "audit:fix": "npm audit fix",
    
    // Release workflow
    "prerelease": "npm run build && npm run test",
    "release": "release-it",
    "release:beta": "release-it --preRelease=beta",
    "postrelease": "npm run deploy",
    
    // Git hooks
    "prepare": "husky install",
    "pre-commit": "lint-staged",
    "pre-push": "npm run type-check && npm run test:unit",
    
    // Documentation
    "docs:dev": "vitepress dev docs",
    "docs:build": "vitepress build docs",
    "docs:serve": "vitepress serve docs",
    
    // Utilities
    "size": "bundlesize",
    "analyze": "webpack-bundle-analyzer dist/stats.json",
    "lighthouse": "lhci autorun"
  }
}
```

### Configuration avancée
```json
{
  "config": {
    "port": 3000,
    "api_url": "https://api.example.com"
  },
  
  "lint-staged": {
    "*.{js,ts,vue}": ["eslint --fix", "prettier --write"],
    "*.{css,scss}": ["stylelint --fix", "prettier --write"],
    "*.{md,json}": ["prettier --write"]
  },
  
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
      "pre-push": "npm run type-check"
    }
  },
  
  "release-it": {
    "git": {
      "commitMessage": "chore: release v${version}",
      "tagName": "v${version}"
    },
    "github": {
      "release": true
    },
    "npm": {
      "publish": true
    }
  },
  
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead",
    "not ie 11"
  ]
}
```

### Variables d'environnement
```bash
# .npmrc
registry=https://registry.npmjs.org/
@company:registry=https://npm.company.com/
//npm.company.com/:_authToken=${NPM_TOKEN}

# Cache et performance
cache=/path/to/cache
prefer-offline=true
progress=false
audit-level=moderate

# Configuration stricte
engine-strict=true
save-exact=true
fund=false
```
{% endtab %}

{% tab title="🏢 Monorepo et workspaces" %}
```typescript
// Configuration monorepo professionnel
interface MonorepoStructure {
  root: {
    'package.json': PackageJson;
    'pnpm-workspace.yaml': WorkspaceConfig;
  };
  
  packages: {
    [name: string]: {
      'package.json': PackageJson;
      src: string[];
    };
  };
  
  apps: {
    [name: string]: {
      'package.json': PackageJson;
      src: string[];
    };
  };
}

// Workspace configuration
const workspaceConfig = {
  // pnpm-workspace.yaml
  packages: [
    'packages/*',
    'apps/*',
    'tools/*',
    '!**/test/**',
    '!**/*.test.*'
  ]
};

// Root package.json
const rootPackage = {
  "name": "@company/monorepo",
  "private": true,
  "workspaces": [
    "packages/*",
    "apps/*"
  ],
  
  "scripts": {
    // Parallel execution
    "dev": "pnpm -r --parallel dev",
    "build": "pnpm -r build",
    "test": "pnpm -r test",
    
    // Filtered execution
    "build:apps": "pnpm --filter './apps/*' build",
    "test:packages": "pnpm --filter './packages/*' test",
    
    // Specific workspace
    "dev:web": "pnpm --filter @company/web-app dev",
    "build:ui": "pnpm --filter @company/ui-lib build",
    
    // Dependencies management
    "update:all": "pnpm -r update",
    "install:workspace": "pnpm install",
    "clean:all": "pnpm -r clean && rimraf node_modules"
  },
  
  "devDependencies": {
    // Shared tooling
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "prettier": "^3.0.0",
    "husky": "^8.0.0",
    "lint-staged": "^13.0.0"
  }
};

// Package workspace example
const packageWorkspace = {
  "name": "@company/ui-components",
  "version": "1.0.0",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts",
  
  "dependencies": {
    // Local workspace dependency
    "@company/design-tokens": "workspace:*",
    "@company/utils": "workspace:^1.0.0"
  },
  
  "scripts": {
    "build": "tsup src/index.ts --dts",
    "dev": "tsup src/index.ts --watch",
    "test": "vitest"
  }
};

// Cross-workspace dependencies
const dependencyGraph = {
  '@company/web-app': [
    '@company/ui-components',
    '@company/api-client',
    '@company/utils'
  ],
  
  '@company/ui-components': [
    '@company/design-tokens',
    '@company/utils'
  ],
  
  '@company/api-client': [
    '@company/utils'
  ]
};
```

### Gestion des versions en monorepo
```typescript
// Versioning strategy
class MonorepoVersioning {
  // Independent versioning
  async updateIndependent() {
    await this.runCommand('pnpm changeset');
    await this.runCommand('pnpm changeset version');
    await this.runCommand('pnpm changeset publish');
  }
  
  // Fixed versioning (lerna-style)
  async updateFixed() {
    await this.runCommand('lerna version --conventional-commits');
    await this.runCommand('lerna publish from-git');
  }
  
  // Selective versioning
  async updateSelective(packages: string[]) {
    for (const pkg of packages) {
      await this.runCommand(`pnpm --filter ${pkg} version patch`);
    }
  }
}

// Changesets configuration
const changesetsConfig = {
  // .changeset/config.json
  "$schema": "https://unpkg.com/@changesets/config@2.3.0/schema.json",
  "changelog": "@changesets/cli/changelog",
  "commit": false,
  "fixed": [],
  "linked": [
    ["@company/ui-*"]  // Linked versioning for UI packages
  ],
  "access": "public",
  "baseBranch": "main",
  "updateInternalDependencies": "patch",
  "ignore": ["@company/docs"]
};
```

### Build orchestration
```yaml
# GitHub Actions - Monorepo CI
name: Monorepo CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  changes:
    runs-on: ubuntu-latest
    outputs:
      packages: ${{ steps.changes.outputs.packages }}
      apps: ${{ steps.changes.outputs.apps }}
    steps:
      - uses: actions/checkout@v3
      - uses: dorny/paths-filter@v2
        id: changes
        with:
          filters: |
            packages:
              - 'packages/**'
            apps:
              - 'apps/**'

  test-packages:
    needs: changes
    if: ${{ needs.changes.outputs.packages == 'true' }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 8
      - name: Test packages
        run: pnpm --filter './packages/*' test

  build-apps:
    needs: changes
    if: ${{ needs.changes.outputs.apps == 'true' }}
    runs-on: ubuntu-latest
    strategy:
      matrix:
        app: [web-app, admin-app, mobile-app]
    steps:
      - uses: actions/checkout@v3
      - name: Build ${{ matrix.app }}
        run: pnpm --filter @company/${{ matrix.app }} build
```
{% endtab %}

{% tab title="🔒 Sécurité et conformité" %}
```typescript
// Security configuration avancée
interface SecurityPolicy {
  audit: {
    level: 'low' | 'moderate' | 'high' | 'critical';
    autoFix: boolean;
    exclude: string[];
  };
  
  registry: {
    allowed: string[];
    blocked: string[];
    proxy?: string;
  };
  
  licenses: {
    allowed: string[];
    forbidden: string[];
    review: string[];
  };
}

const securityConfig: SecurityPolicy = {
  audit: {
    level: 'moderate',
    autoFix: true,
    exclude: ['GHSA-xyz'] // Advisory à ignorer
  },
  
  registry: {
    allowed: [
      'https://registry.npmjs.org/',
      'https://npm.company.com/'
    ],
    blocked: [
      'https://sketchy-registry.com/'
    ]
  },
  
  licenses: {
    allowed: ['MIT', 'Apache-2.0', 'BSD-3-Clause'],
    forbidden: ['GPL-3.0', 'AGPL-3.0'],
    review: ['LGPL-2.1', 'MPL-2.0']
  }
};

// Audit automatisé
class SecurityAuditor {
  async runCompleteAudit() {
    const results = {
      vulnerabilities: await this.checkVulnerabilities(),
      licenses: await this.checkLicenses(),
      policies: await this.checkPolicies(),
      dependencies: await this.analyzeDependencies()
    };
    
    return this.generateReport(results);
  }
  
  async checkVulnerabilities() {
    // NPM Audit
    const npmAudit = await this.execCommand('npm audit --json');
    
    // Snyk integration
    const snykAudit = await this.execCommand('snyk test --json');
    
    // OSV Scanner
    const osvAudit = await this.execCommand('osv-scanner --json .');
    
    return this.mergeAuditResults([npmAudit, snykAudit, osvAudit]);
  }
  
  async checkLicenses() {
    const licenses = await this.execCommand('license-checker --json');
    
    return Object.entries(licenses).map(([pkg, info]) => ({
      package: pkg,
      license: info.licenses,
      compliant: this.isLicenseCompliant(info.licenses),
      action: this.getLicenseAction(info.licenses)
    }));
  }
  
  async setupAutomaticUpdates() {
    // Dependabot configuration
    const dependabotConfig = {
      version: 2,
      updates: [
        {
          'package-ecosystem': 'npm',
          directory: '/',
          schedule: { interval: 'weekly' },
          'open-pull-requests-limit': 10,
          'target-branch': 'develop',
          'reviewers': ['security-team'],
          'commit-message': {
            'prefix': 'security',
            'include': 'scope'
          }
        }
      ]
    };
    
    // Renovate configuration
    const renovateConfig = {
      extends: ['config:base'],
      dependencyDashboard: true,
      schedule: ['before 6am on Monday'],
      vulnerabilityAlerts: {
        enabled: true,
        schedule: ['at any time']
      },
      packageRules: [
        {
          matchUpdateTypes: ['patch'],
          automerge: true
        },
        {
          matchPackagePatterns: ['@types/*'],
          automerge: true
        }
      ]
    };
    
    return { dependabotConfig, renovateConfig };
  }
}

// Policy enforcement
const packagePolicies = {
  // .npmrc enterprise
  registry: 'https://npm.company.com/',
  '@company:registry': 'https://npm.company.com/',
  
  // Audit configuration
  'audit-level': 'moderate',
  'fund': false,
  'optional': false,
  
  // Security policies
  'before-security-audit': 'echo "Running security checks..."',
  'after-security-audit': 'npm run security:report'
};

// Scripts de sécurité
const securityScripts = {
  "security:audit": "npm audit --audit-level moderate",
  "security:fix": "npm audit fix",
  "security:licenses": "license-checker --onlyAllow 'MIT;Apache-2.0;BSD-3-Clause'",
  "security:outdated": "npm outdated",
  "security:report": "npm-audit-html --output security-report.html",
  "security:ci": "npm-run-all security:audit security:licenses",
  
  // SAST integration
  "security:sast": "semgrep --config auto .",
  "security:secrets": "truffleHog --regex --entropy=False .",
  "security:sbom": "cyclonedx-npm --output-file sbom.json"
};
```

### Compliance et governance
```json
{
  "eslintConfig": {
    "extends": ["@company/eslint-config"],
    "rules": {
      "import/no-extraneous-dependencies": "error",
      "security/detect-non-literal-require": "error"
    }
  },
  
  "renovate": {
    "extends": ["@company/renovate-config"],
    "vulnerabilityAlerts": {
      "enabled": true
    },
    "securityUpdates": {
      "enabled": true
    }
  },
  
  "snyk": {
    "language-settings": {
      "javascript": {
        "packageManager": "pnpm"
      }
    },
    "exclude": {
      "global": ["test/**", "**/*.test.js"]
    }
  }
}
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Enterprise Package Management

{% hint style="warning" %}
**Exercice :** Créez un système complet de gestion des dépendances pour une entreprise
{% endhint %}

```typescript
// Créez un système de gestion enterprise
interface EnterprisePackageManager {
    // TODO: Configuration monorepo
    monorepo: {
        structure: string[];
        versioning: 'independent' | 'fixed';
        buildOrchestration: boolean;
    };
    
    // TODO: Policies de sécurité
    security: {
        auditLevel: 'low' | 'moderate' | 'high' | 'critical';
        allowedLicenses: string[];
        vulnerabilityMonitoring: boolean;
        automaticUpdates: boolean;
    };
    
    // TODO: Optimisations performance
    performance: {
        caching: boolean;
        parallelBuilds: boolean;
        incrementalBuilds: boolean;
        bundleAnalysis: boolean;
    };
    
    // TODO: CI/CD integration
    cicd: {
        provider: 'github' | 'gitlab' | 'azure';
        testingStrategy: string[];
        deploymentPipeline: boolean;
    };
}

// TODO: Implémentez le système complet
class EnterprisePackageSystem {
    constructor(config: EnterprisePackageManager) {}
    
    // TODO: Setup monorepo
    async setupMonorepo() {
        // Configuration workspace
        // Dependency graph
        // Build orchestration
        // Version management
    }
    
    // TODO: Security pipeline
    async setupSecurityPipeline() {
        // Audit automation
        // Vulnerability scanning
        // License compliance
        // Policy enforcement
    }
    
    // TODO: Performance optimization
    async optimizePerformance() {
        // Cache strategy
        // Bundle analysis
        // Dependency optimization
        // Build parallelization
    }
    
    // TODO: Monitoring et reporting
    async setupMonitoring() {
        // Dependency tracking
        // Security metrics
        // Performance metrics
        // Compliance reporting
    }
}

// Tests et validation
async function validateEnterpriseSetup() {
    // TODO: Tests de performance
    // TODO: Tests de sécurité
    // TODO: Tests de conformité
    // TODO: Tests d'intégration CI/CD
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```typescript
// Structure monorepo enterprise
const monorepoStructure = {
  packages: ['@company/ui', '@company/utils'],
  apps: ['web-app', 'admin-app'],
  tools: ['build-tools', 'eslint-config'],
  
  scripts: {
    "build:all": "pnpm -r build",
    "test:all": "pnpm -r test",
    "security:audit": "pnpm audit --audit-level high"
  }
};

// Configuration de sécurité
const securityPipeline = {
  audit: {
    schedule: 'weekly',
    autoFix: true,
    reports: true
  }
};
```
{% endtab %}

{% tab title="✅ Solution" %}
Solution complète avec configuration enterprise, monitoring et CI/CD dans l'onglet suivant.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant la gestion avancée des dépendances JavaScript !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Package managers** modernes (NPM, Yarn, pnpm)
- **Monorepos** et workspaces avancés
- **Sécurité** et audit des dépendances
- **Performance** et optimisations
- **Semantic versioning** et release management
- **CI/CD** integration et automation
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Configurer des projets enterprise-grade
- Optimiser les performances d'installation
- Sécuriser les dépendances et audits
- Gérer des monorepos complexes
- Automatiser les workflows de release
- Intégrer dans les pipelines CI/CD
{% endtab %}

{% tab title="🚀 Applications réelles" %}
- **Projets enterprise** avec governance
- **Monorepos** multi-équipes scalables
- **Pipelines CI/CD** optimisés
- **Sécurité** et compliance automatisées
- **Performance** et monitoring continus
{% endtab %}
{% endtabs %}

---

**Prêt pour les WebSockets et communications temps réel ? Continuez avec Socket.IO avancé !** 🔌
