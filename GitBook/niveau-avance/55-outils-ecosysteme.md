# 🛠️ Écosystème et Outils JavaScript

> **Niveau :** 🔴 Avancé | **Durée :** 75 minutes | **Prérequis :** Modules, Build Tools, Performance

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** l'écosystème JavaScript moderne et ses outils
- [ ] **Configurer** des workflows de développement sophistiqués
- [ ] **Optimiser** les pipelines de build et déploiement
- [ ] **Choisir** les bons outils selon le contexte projet

---

## 🌐 Vue d'Ensemble de l'Écosystème

{% tabs %}
{% tab title="🗺️ Cartographie écosystème" %}
**L'écosystème JavaScript en 2025 :**

### Package Managers - Gestionnaires de dépendances
```javascript
// NPM - Le standard historique
npm init -y
npm install lodash
npm install --save-dev typescript

// Yarn - Performance et déterminisme
yarn init -y
yarn add lodash
yarn add --dev typescript

// PNPM - Efficacité disque et monorepos
pnpm init
pnpm add lodash
pnpm add -D typescript

// Bun - Nouvelle génération ultra-rapide
bun init
bun add lodash
bun add -d typescript
```

### Build Tools - Outils de construction
```javascript
// Vite - Modern build tool
// vite.config.js
import { defineConfig } from 'vite';
import { resolve } from 'path';

export default defineConfig({
  root: './src',
  build: {
    outDir: '../dist',
    rollupOptions: {
      input: {
        main: resolve(__dirname, 'src/index.html'),
        admin: resolve(__dirname, 'src/admin.html')
      }
    }
  },
  server: {
    port: 3000,
    open: true,
    cors: true
  },
  preview: {
    port: 8080
  }
});

// Webpack - Configuration complète
// webpack.config.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[contenthash].js',
    clean: true
  },
  
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        type: 'asset/resource'
      }
    ]
  },
  
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css'
    })
  ],
  
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        }
      }
    }
  }
};

// Rollup - Bibliothèques et composants
// rollup.config.js
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';
import { terser } from 'rollup-plugin-terser';

export default {
  input: 'src/index.js',
  output: [
    {
      file: 'dist/bundle.cjs.js',
      format: 'cjs'
    },
    {
      file: 'dist/bundle.esm.js',
      format: 'esm'
    },
    {
      file: 'dist/bundle.umd.js',
      format: 'umd',
      name: 'MyLibrary'
    }
  ],
  plugins: [
    resolve(),
    commonjs(),
    terser()
  ],
  external: ['lodash'] // Exclure les dépendances externes
};
```

### Quality Tools - Outils de qualité
```javascript
// ESLint - Configuration avancée
// .eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2022: true,
    node: true,
    jest: true
  },
  
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'prettier'
  ],
  
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true
    }
  },
  
  plugins: [
    '@typescript-eslint',
    'react',
    'react-hooks',
    'import',
    'security'
  ],
  
  rules: {
    // TypeScript
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/explicit-function-return-type': 'warn',
    
    // React
    'react/prop-types': 'off',
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
    
    // Import/Export
    'import/order': ['error', {
      groups: ['builtin', 'external', 'internal', 'parent', 'sibling'],
      'newlines-between': 'always'
    }],
    
    // Sécurité
    'security/detect-object-injection': 'warn',
    'security/detect-eval-with-expression': 'error',
    
    // Style
    'prefer-const': 'error',
    'no-var': 'error',
    'object-shorthand': 'error'
  },
  
  settings: {
    react: {
      version: 'detect'
    }
  },
  
  overrides: [
    {
      files: ['**/*.test.js', '**/*.test.ts'],
      env: {
        jest: true
      },
      rules: {
        '@typescript-eslint/no-explicit-any': 'off'
      }
    }
  ]
};

// Prettier - Configuration sophistiquée
// .prettierrc.js
module.exports = {
  // Formatage de base
  printWidth: 100,
  tabWidth: 2,
  useTabs: false,
  semi: true,
  singleQuote: true,
  quoteProps: 'as-needed',
  
  // JavaScript
  trailingComma: 'es5',
  bracketSpacing: true,
  bracketSameLine: false,
  arrowParens: 'avoid',
  
  // HTML/JSX
  htmlWhitespaceSensitivity: 'css',
  jsxSingleQuote: true,
  jsxBracketSameLine: false,
  
  // Autres
  endOfLine: 'lf',
  embeddedLanguageFormatting: 'auto',
  
  // Overrides par type de fichier
  overrides: [
    {
      files: '*.json',
      options: {
        printWidth: 80
      }
    },
    {
      files: '*.md',
      options: {
        proseWrap: 'always'
      }
    }
  ]
};

// Husky - Git hooks automatiques
// .husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run lint
npm run type-check
npm run test:changed

// lint-staged - Formatage sélectif
// .lintstagedrc.js
module.exports = {
  '*.{js,jsx,ts,tsx}': [
    'eslint --fix',
    'prettier --write',
    'git add'
  ],
  '*.{css,scss,less}': [
    'stylelint --fix',
    'prettier --write',
    'git add'
  ],
  '*.{json,md,yml,yaml}': [
    'prettier --write',
    'git add'
  ]
};
```
{% endtab %}

{% tab title="⚡ Modern Toolchains" %}
**Chaînes d'outils modernes pour différents cas d'usage**

### Stack React/Next.js moderne
```javascript
// next.config.js - Configuration Next.js avancée
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Expérimental
  experimental: {
    appDir: true,
    typedRoutes: true,
    serverComponentsExternalPackages: ['mysql2']
  },
  
  // Performance
  swcMinify: true,
  compress: true,
  
  // Images
  images: {
    domains: ['example.com'],
    formats: ['image/webp', 'image/avif'],
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384]
  },
  
  // Headers de sécurité
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'X-Frame-Options',
            value: 'DENY'
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff'
          },
          {
            key: 'Referrer-Policy',
            value: 'origin-when-cross-origin'
          }
        ]
      }
    ];
  },
  
  // Webpack customization
  webpack: (config, { buildId, dev, isServer, defaultLoaders, webpack }) => {
    // Analyse des bundles
    if (!dev && !isServer) {
      config.plugins.push(
        new webpack.optimize.LimitChunkCountPlugin({
          maxChunks: 50
        })
      );
    }
    
    return config;
  },
  
  // Variables d'environnement
  env: {
    CUSTOM_KEY: process.env.CUSTOM_KEY,
  },
  
  // Redirections
  async redirects() {
    return [
      {
        source: '/old-page',
        destination: '/new-page',
        permanent: true
      }
    ];
  }
};

module.exports = nextConfig;

// tailwind.config.js - Configuration Tailwind avancée
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}'
  ],
  
  theme: {
    extend: {
      // Couleurs personnalisées
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          900: '#1e3a8a'
        }
      },
      
      // Animations personnalisées
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.3s ease-out'
      },
      
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' }
        },
        slideUp: {
          '0%': { transform: 'translateY(100%)' },
          '100%': { transform: 'translateY(0)' }
        }
      },
      
      // Typographie
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        mono: ['Fira Code', 'monospace']
      }
    }
  },
  
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio')
  ],
  
  // Dark mode
  darkMode: 'class'
};
```

### Stack Vue/Nuxt moderne
```javascript
// nuxt.config.ts - Configuration Nuxt 3
export default defineNuxtConfig({
  // Modules
  modules: [
    '@pinia/nuxt',
    '@nuxtjs/tailwindcss',
    '@vueuse/nuxt',
    '@nuxt/image'
  ],
  
  // TypeScript
  typescript: {
    strict: true,
    typeCheck: true
  },
  
  // CSS Framework
  css: ['~/assets/css/main.css'],
  
  // Build configuration
  build: {
    transpile: ['some-library']
  },
  
  // Nitro server
  nitro: {
    preset: 'vercel-edge',
    storage: {
      redis: {
        driver: 'redis',
        host: process.env.REDIS_HOST,
        port: process.env.REDIS_PORT
      }
    }
  },
  
  // Runtime config
  runtimeConfig: {
    apiSecret: process.env.API_SECRET,
    public: {
      apiBase: process.env.API_BASE_URL
    }
  },
  
  // Performance
  experimental: {
    payloadExtraction: false,
    renderJsonPayloads: true
  },
  
  // SEO
  app: {
    head: {
      title: 'Mon App',
      meta: [
        { charset: 'utf-8' },
        { name: 'viewport', content: 'width=device-width, initial-scale=1' }
      ]
    }
  }
});

// vite.config.ts pour projets Vue purs
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import { resolve } from 'path';

export default defineConfig({
  plugins: [
    vue({
      script: {
        defineModel: true,
        propsDestructure: true
      }
    })
  ],
  
  resolve: {
    alias: {
      '@': resolve(__dirname, './src'),
      '~': resolve(__dirname, './src')
    }
  },
  
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['vue', 'vue-router'],
          ui: ['@headlessui/vue', '@heroicons/vue']
        }
      }
    }
  },
  
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:3001',
        changeOrigin: true,
        rewrite: path => path.replace(/^\/api/, '')
      }
    }
  }
});
```

### Stack Node.js/Express sophistiqué
```javascript
// Configuration complète pour API Node.js

// tsconfig.json - Configuration TypeScript serveur
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "allowJs": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "moduleDetection": "force",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "verbatimModuleSyntax": false,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "paths": {
      "@/*": ["./src/*"],
      "@controllers/*": ["./src/controllers/*"],
      "@services/*": ["./src/services/*"],
      "@models/*": ["./src/models/*"],
      "@utils/*": ["./src/utils/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}

// nodemon.json - Configuration de développement
{
  "watch": ["src"],
  "ext": "ts,json",
  "ignore": ["src/**/*.test.ts"],
  "exec": "tsx src/index.ts",
  "env": {
    "NODE_ENV": "development"
  }
}

// docker-compose.yml - Environnement de développement
version: '3.8'

services:
  app:
    build: 
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://user:pass@postgres:5432/mydb
    depends_on:
      - postgres
      - redis
    
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
      
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### Monitoring et Observabilité
```javascript
// Configuration monitoring avec observabilité complète

// New Relic - APM
// newrelic.js
'use strict';

exports.config = {
  app_name: ['My Node.js App'],
  license_key: process.env.NEW_RELIC_LICENSE_KEY,
  logging: {
    level: 'info'
  },
  
  // Distributed tracing
  distributed_tracing: {
    enabled: true
  },
  
  // Custom attributes
  attributes: {
    exclude: [
      'request.headers.cookie',
      'request.headers.authorization'
    ]
  }
};

// Sentry - Error tracking
// sentry.config.js
import * as Sentry from '@sentry/node';
import { ProfilingIntegration } from '@sentry/profiling-node';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  
  integrations: [
    new ProfilingIntegration(),
    new Sentry.Integrations.Http({ tracing: true }),
    new Sentry.Integrations.Express({ app })
  ],
  
  tracesSampleRate: 1.0,
  profilesSampleRate: 1.0,
  
  beforeSend(event) {
    // Filtrer les données sensibles
    if (event.request?.headers) {
      delete event.request.headers.authorization;
      delete event.request.headers.cookie;
    }
    return event;
  }
});

// Prometheus - Métriques custom
// metrics.js
import client from 'prom-client';

// Métriques par défaut
client.collectDefaultMetrics();

// Métriques custom
export const httpRequestDuration = new client.Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status_code']
});

export const activeConnections = new client.Gauge({
  name: 'active_connections',
  help: 'Number of active connections'
});

export const businessMetrics = new client.Counter({
  name: 'business_operations_total',
  help: 'Total number of business operations',
  labelNames: ['operation_type', 'status']
});

// Middleware de métriques
export const metricsMiddleware = (req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    const duration = (Date.now() - start) / 1000;
    httpRequestDuration
      .labels(req.method, req.route?.path || req.path, res.statusCode)
      .observe(duration);
  });
  
  next();
};
```
{% endtab %}

{% tab title="🚀 DevOps et CI/CD" %}
**Pipelines modernes et déploiement automatisé**

### GitHub Actions - CI/CD moderne
```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '18'
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  # Phase de test et qualité
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16, 18, 20]
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        
      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Run lint
        run: npm run lint
        
      - name: Run type check
        run: npm run type-check
        
      - name: Run tests
        run: npm run test:coverage
        
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage/coverage-final.json
          
  # Analyse de sécurité
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Snyk to check for vulnerabilities
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
          
      - name: Run CodeQL Analysis
        uses: github/codeql-action/analyze@v2
        with:
          languages: javascript
          
  # Build et déploiement
  build-and-deploy:
    needs: [test, security]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build application
        run: npm run build
        env:
          NODE_ENV: production
          
      - name: Generate deployment package
        run: |
          zip -r deploy.zip dist/ package.json package-lock.json
          
      - name: Deploy to staging
        if: github.ref == 'refs/heads/develop'
        uses: some-deploy-action@v1
        with:
          environment: staging
          package: deploy.zip
          
      - name: Deploy to production
        if: github.ref == 'refs/heads/main'
        uses: some-deploy-action@v1
        with:
          environment: production
          package: deploy.zip
          
      - name: Notify deployment
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}

  # Performance testing
  performance:
    needs: build-and-deploy
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Lighthouse CI
        uses: treosh/lighthouse-ci-action@v9
        with:
          configPath: './lighthouserc.json'
          uploadArtifacts: true
          temporaryPublicStorage: true
```

### Docker - Containerisation moderne
```dockerfile
# Dockerfile.prod - Production optimisé
# Stage 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

# Copier les fichiers de dépendances
COPY package*.json ./
COPY tsconfig.json ./

# Installer les dépendances
RUN npm ci --only=production && npm cache clean --force

# Copier le code source
COPY src/ ./src/

# Build de l'application
RUN npm run build

# Stage 2: Runtime
FROM node:18-alpine AS runtime

# Créer un utilisateur non-root
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

WORKDIR /app

# Copier les fichiers nécessaires
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package.json ./

# Changer vers l'utilisateur non-root
USER nextjs

# Exposer le port
EXPOSE 3000

# Variables d'environnement par défaut
ENV NODE_ENV=production
ENV PORT=3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node healthcheck.js

# Commande de démarrage
CMD ["node", "dist/index.js"]

# .dockerignore
node_modules
npm-debug.log
.git
.gitignore
README.md
.env
.nyc_output
coverage
.coverage
.cache
dist
```

### Kubernetes - Déploiement cloud-native
```yaml
# k8s/deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
  labels:
    app: nodejs-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nodejs-app
  template:
    metadata:
      labels:
        app: nodejs-app
    spec:
      containers:
      - name: app
        image: myregistry/nodejs-app:latest
        ports:
        - containerPort: 3000
        env:
        - name: NODE_ENV
          value: "production"
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: app-secrets
              key: database-url
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: app-service
spec:
  selector:
    app: nodejs-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
spec:
  tls:
  - hosts:
    - myapp.example.com
    secretName: app-tls
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

### Terraform - Infrastructure as Code
```hcl
# infrastructure/main.tf
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "nodejs-app/terraform.tfstate"
    region = "us-west-2"
  }
}

provider "aws" {
  region = var.aws_region
}

# ECS Cluster
resource "aws_ecs_cluster" "main" {
  name = "${var.app_name}-cluster"
  
  setting {
    name  = "containerInsights"
    value = "enabled"
  }
}

# Application Load Balancer
resource "aws_lb" "main" {
  name               = "${var.app_name}-lb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.lb.id]
  subnets           = var.public_subnets
  
  enable_deletion_protection = false
}

# ECS Service
resource "aws_ecs_service" "main" {
  name            = "${var.app_name}-service"
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.app.arn
  desired_count   = var.app_count
  
  network_configuration {
    security_groups  = [aws_security_group.ecs_tasks.id]
    subnets         = var.private_subnets
    assign_public_ip = true
  }
  
  load_balancer {
    target_group_arn = aws_lb_target_group.app.arn
    container_name   = var.app_name
    container_port   = var.app_port
  }
  
  depends_on = [aws_lb_listener.front_end]
}

# Auto Scaling
resource "aws_appautoscaling_target" "target" {
  service_namespace  = "ecs"
  resource_id        = "service/${aws_ecs_cluster.main.name}/${aws_ecs_service.main.name}"
  scalable_dimension = "ecs:service:DesiredCount"
  min_capacity       = 2
  max_capacity       = 10
}

resource "aws_appautoscaling_policy" "up" {
  name               = "${var.app_name}-scale-up"
  service_namespace  = "ecs"
  resource_id        = aws_appautoscaling_target.target.resource_id
  scalable_dimension = aws_appautoscaling_target.target.scalable_dimension
  
  step_scaling_policy_configuration {
    adjustment_type         = "ChangeInCapacity"
    cooldown               = 60
    metric_aggregation_type = "Maximum"
    
    step_adjustment {
      metric_interval_lower_bound = 0
      scaling_adjustment          = 1
    }
  }
}

# CloudWatch monitoring
resource "aws_cloudwatch_metric_alarm" "cpu_high" {
  alarm_name          = "${var.app_name}-cpu-utilization-high"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = "2"
  metric_name         = "CPUUtilization"
  namespace           = "AWS/ECS"
  period              = "60"
  statistic           = "Average"
  threshold           = "85"
  alarm_description   = "This metric monitors ecs cpu utilization"
  
  dimensions = {
    ServiceName = aws_ecs_service.main.name
    ClusterName = aws_ecs_cluster.main.name
  }
  
  alarm_actions = [aws_appautoscaling_policy.up.arn]
}
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Workflow Complet de Développement

{% hint style="warning" %}
**Défi :** Créez un workflow de développement moderne avec tous les outils essentiels
{% endhint %}

```javascript
// Créez un environnement de développement complet
interface ModernDevelopmentWorkflow {
    // TODO: Project structure
    structure: {
        monorepo: boolean;
        microfrontends: boolean;
        microservices: boolean;
        packages: string[];
    };
    
    // TODO: Development tools
    devTools: {
        bundler: 'vite' | 'webpack' | 'turbopack';
        packageManager: 'npm' | 'yarn' | 'pnpm' | 'bun';
        runtime: 'node' | 'deno' | 'bun';
        testing: 'jest' | 'vitest' | 'playwright';
    };
    
    // TODO: Quality gates
    qualityGates: {
        linting: 'eslint' | 'biome';
        formatting: 'prettier' | 'biome';
        typeChecking: 'typescript' | 'flow';
        testing: 'unit' | 'integration' | 'e2e';
        coverage: number; // percentage
    };
    
    // TODO: CI/CD pipeline
    cicd: {
        platform: 'github' | 'gitlab' | 'jenkins';
        stages: string[];
        deploymentStrategy: 'blue-green' | 'canary' | 'rolling';
        environments: string[];
    };
    
    // TODO: Infrastructure
    infrastructure: {
        containerization: 'docker' | 'podman';
        orchestration: 'kubernetes' | 'docker-swarm';
        iac: 'terraform' | 'pulumi' | 'cdk';
        monitoring: 'prometheus' | 'datadog' | 'newrelic';
    };
}

// TODO: Implémentez le workflow complet
class ModernWorkflowManager {
    constructor(config: ModernDevelopmentWorkflow) {}
    
    // TODO: Setup du projet
    setupProject() {
        // Initialisation du projet
        // Configuration des outils
        // Structure des dossiers
        // Scripts de développement
    }
    
    // TODO: Configuration des outils
    configureTools() {
        // Package.json scripts
        // Configuration ESLint/Prettier
        // Configuration TypeScript
        // Configuration des tests
        // Configuration du bundler
    }
    
    // TODO: Pipeline CI/CD
    setupCICD() {
        // GitHub Actions / GitLab CI
        // Docker configuration
        // Kubernetes manifests
        // Terraform scripts
        // Monitoring setup
    }
    
    // TODO: Quality gates
    setupQualityGates() {
        // Pre-commit hooks
        // Code coverage
        // Security scanning
        // Performance budgets
        // Accessibility testing
    }
    
    // TODO: Monitoring et observabilité
    setupObservability() {
        // Application monitoring
        // Error tracking
        // Performance monitoring
        // Business metrics
        // Alerting
    }
}

// Configuration exemple pour une startup
const startupWorkflow: ModernDevelopmentWorkflow = {
    structure: {
        monorepo: false,
        microfrontends: false,
        microservices: false,
        packages: ['frontend', 'backend', 'shared']
    },
    
    devTools: {
        bundler: 'vite',
        packageManager: 'pnpm',
        runtime: 'node',
        testing: 'vitest'
    },
    
    qualityGates: {
        linting: 'eslint',
        formatting: 'prettier',
        typeChecking: 'typescript',
        testing: 'unit',
        coverage: 80
    },
    
    cicd: {
        platform: 'github',
        stages: ['test', 'build', 'deploy'],
        deploymentStrategy: 'rolling',
        environments: ['staging', 'production']
    },
    
    infrastructure: {
        containerization: 'docker',
        orchestration: 'kubernetes',
        iac: 'terraform',
        monitoring: 'prometheus'
    }
};

// Configuration pour une enterprise
const enterpriseWorkflow: ModernDevelopmentWorkflow = {
    structure: {
        monorepo: true,
        microfrontends: true,
        microservices: true,
        packages: ['apps', 'libs', 'tools', 'infrastructure']
    },
    
    devTools: {
        bundler: 'turbopack',
        packageManager: 'pnpm',
        runtime: 'node',
        testing: 'jest'
    },
    
    qualityGates: {
        linting: 'eslint',
        formatting: 'prettier',
        typeChecking: 'typescript',
        testing: 'e2e',
        coverage: 95
    },
    
    cicd: {
        platform: 'gitlab',
        stages: ['security', 'test', 'build', 'staging', 'production'],
        deploymentStrategy: 'blue-green',
        environments: ['dev', 'staging', 'preprod', 'production']
    },
    
    infrastructure: {
        containerization: 'docker',
        orchestration: 'kubernetes',
        iac: 'terraform',
        monitoring: 'datadog'
    }
};

// TODO: Implémentez et testez les deux configurations
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
// Structure de base du workflow
class WorkflowManager {
  setupBasicProject() {
    // 1. Initialiser package.json avec scripts
    // 2. Configurer TypeScript
    // 3. Setup ESLint et Prettier
    // 4. Configurer le bundler (Vite/Webpack)
    // 5. Setup des tests
    // 6. Configuration Docker
    // 7. GitHub Actions basic
  }
  
  configureQuality() {
    // Husky pour git hooks
    // lint-staged pour formatting
    // Coverage thresholds
    // Security scanning
  }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
Solution complète avec configuration de tous les outils, pipeline CI/CD et infrastructure as code dans l'onglet suivant.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Bravo !** Vous maîtrisez maintenant l'écosystème JavaScript moderne !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Package managers** modernes (npm, yarn, pnpm, bun)
- **Build tools** avancés (Vite, Webpack, Rollup)
- **Quality tools** (ESLint, Prettier, TypeScript)
- **Testing frameworks** (Jest, Vitest, Playwright)
- **CI/CD pipelines** automatisés
- **Infrastructure moderne** (Docker, Kubernetes, Terraform)
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Configurer des environnements de développement modernes
- Créer des pipelines CI/CD robustes
- Optimiser les performances de build
- Mettre en place la qualité de code automatisée
- Déployer sur des infrastructures cloud-native
- Monitorer et observer les applications en production
{% endtab %}

{% tab title="🚀 Applications professionnelles" %}
- **Startups** : Setup rapide et scalable
- **Entreprises** : Workflows enterprise-grade
- **Open Source** : Contributions et maintenance
- **Consulting** : Modernisation d'applications legacy
- **DevOps** : Infrastructure et automatisation
- **Architecture** : Décisions techniques éclairées
{% endtab %}
{% endtabs %}

---

**Félicitations ! Vous avez maintenant toutes les compétences pour maîtriser l'écosystème JavaScript moderne !** 🎉
