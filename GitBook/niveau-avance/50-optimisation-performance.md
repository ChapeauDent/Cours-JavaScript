# ⚡ Optimisation et Performance JavaScript

> **Niveau :** 🔴 Avancé | **Durée :** 60 minutes | **Prérequis :** Event Loop, DOM, Async/Await

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Analyser** les goulots d'étranglement et profiler le code
- [ ] **Optimiser** les performances mémoire et CPU
- [ ] **Implémenter** des techniques de lazy loading et caching
- [ ] **Maîtriser** les Web Workers et le code parallèle

---

## 🔍 Profiling et Mesure des Performances

{% tabs %}
{% tab title="📊 Outils de mesure" %}
**Performance API : mesures précises au millième**

### Performance timing avancé
```javascript
// Performance Observer pour monitoring continu
class PerformanceMonitor {
  constructor() {
    this.metrics = new Map();
    this.observers = [];
    this.setupObservers();
  }
  
  setupObservers() {
    // Observer pour les métriques de navigation
    const navObserver = new PerformanceObserver((list) => {
      list.getEntries().forEach(entry => {
        this.collectNavigationMetrics(entry);
      });
    });
    navObserver.observe({ entryTypes: ['navigation'] });
    
    // Observer pour les ressources
    const resourceObserver = new PerformanceObserver((list) => {
      list.getEntries().forEach(entry => {
        this.collectResourceMetrics(entry);
      });
    });
    resourceObserver.observe({ entryTypes: ['resource'] });
    
    // Observer pour les métriques custom
    const measureObserver = new PerformanceObserver((list) => {
      list.getEntries().forEach(entry => {
        this.collectCustomMetrics(entry);
      });
    });
    measureObserver.observe({ entryTypes: ['measure'] });
    
    this.observers.push(navObserver, resourceObserver, measureObserver);
  }
  
  // Mesure custom avec contexte
  time(name, context = {}) {
    const startTime = performance.now();
    const markName = `${name}-start`;
    
    performance.mark(markName, { detail: { ...context, startTime } });
    
    return {
      end: (additionalContext = {}) => {
        const endTime = performance.now();
        const endMarkName = `${name}-end`;
        const measureName = `${name}-duration`;
        
        performance.mark(endMarkName, { 
          detail: { ...context, ...additionalContext, endTime } 
        });
        
        performance.measure(measureName, markName, endMarkName);
        
        const duration = endTime - startTime;
        this.recordMetric(name, duration, { ...context, ...additionalContext });
        
        return duration;
      }
    };
  }
  
  collectNavigationMetrics(entry) {
    const metrics = {
      // Core Web Vitals
      FCP: entry.name === 'first-contentful-paint' ? entry.startTime : null,
      LCP: entry.name === 'largest-contentful-paint' ? entry.startTime : null,
      FID: entry.name === 'first-input-delay' ? entry.duration : null,
      CLS: entry.name === 'cumulative-layout-shift' ? entry.value : null,
      
      // Navigation timing
      domainLookup: entry.domainLookupEnd - entry.domainLookupStart,
      tcpConnect: entry.connectEnd - entry.connectStart,
      tlsConnect: entry.secureConnectionStart ? entry.connectEnd - entry.secureConnectionStart : 0,
      request: entry.responseStart - entry.requestStart,
      response: entry.responseEnd - entry.responseStart,
      domParsing: entry.domContentLoadedEventStart - entry.responseEnd,
      resourceLoading: entry.loadEventStart - entry.domContentLoadedEventEnd
    };
    
    Object.entries(metrics).forEach(([key, value]) => {
      if (value !== null) {
        this.recordMetric(key, value, { type: 'navigation' });
      }
    });
  }
  
  collectResourceMetrics(entry) {
    const resourceType = this.getResourceType(entry.name);
    const metrics = {
      duration: entry.duration,
      transferSize: entry.transferSize,
      encodedBodySize: entry.encodedBodySize,
      decodedBodySize: entry.decodedBodySize
    };
    
    this.recordMetric(`resource-${resourceType}`, metrics.duration, {
      type: 'resource',
      url: entry.name,
      ...metrics
    });
  }
  
  collectCustomMetrics(entry) {
    this.recordMetric(entry.name, entry.duration, {
      type: 'custom',
      detail: entry.detail
    });
  }
  
  recordMetric(name, value, context = {}) {
    if (!this.metrics.has(name)) {
      this.metrics.set(name, []);
    }
    
    this.metrics.get(name).push({
      value,
      timestamp: Date.now(),
      context
    });
    
    // Alertes sur les seuils critiques
    this.checkThresholds(name, value);
  }
  
  checkThresholds(name, value) {
    const thresholds = {
      'LCP': 2500,  // Large Contentful Paint < 2.5s
      'FID': 100,   // First Input Delay < 100ms
      'CLS': 0.1,   // Cumulative Layout Shift < 0.1
      'resource-script': 1000,  // Scripts < 1s
      'resource-stylesheet': 500, // CSS < 500ms
    };
    
    if (thresholds[name] && value > thresholds[name]) {
      console.warn(`⚠️ Performance threshold exceeded: ${name} = ${value}ms (threshold: ${thresholds[name]}ms)`);
      
      // Émettre un événement custom
      window.dispatchEvent(new CustomEvent('performance-warning', {
        detail: { metric: name, value, threshold: thresholds[name] }
      }));
    }
  }
  
  getResourceType(url) {
    if (url.match(/\.(js|mjs)$/)) return 'script';
    if (url.match(/\.(css)$/)) return 'stylesheet';
    if (url.match(/\.(jpg|jpeg|png|gif|webp|svg)$/)) return 'image';
    if (url.match(/\.(woff|woff2|ttf|otf)$/)) return 'font';
    return 'other';
  }
  
  // Statistiques et rapports
  getStats(metricName) {
    const data = this.metrics.get(metricName) || [];
    const values = data.map(d => d.value);
    
    if (values.length === 0) return null;
    
    const sorted = values.sort((a, b) => a - b);
    return {
      count: values.length,
      min: Math.min(...values),
      max: Math.max(...values),
      mean: values.reduce((a, b) => a + b, 0) / values.length,
      median: sorted[Math.floor(sorted.length / 2)],
      p75: sorted[Math.floor(sorted.length * 0.75)],
      p90: sorted[Math.floor(sorted.length * 0.90)],
      p95: sorted[Math.floor(sorted.length * 0.95)]
    };
  }
  
  generateReport() {
    const report = {
      timestamp: new Date().toISOString(),
      metrics: {}
    };
    
    for (const [name] of this.metrics) {
      report.metrics[name] = this.getStats(name);
    }
    
    return report;
  }
  
  cleanup() {
    this.observers.forEach(observer => observer.disconnect());
    this.metrics.clear();
  }
}

// Usage global
const monitor = new PerformanceMonitor();

// Utilisation pour mesurer des fonctions
async function processData(data) {
  const timer = monitor.time('data-processing', { 
    dataSize: data.length,
    operation: 'transformation' 
  });
  
  try {
    // Traitement lourd
    const result = await heavyDataProcessing(data);
    
    timer.end({ success: true, resultSize: result.length });
    return result;
    
  } catch (error) {
    timer.end({ success: false, error: error.message });
    throw error;
  }
}

// Mesure de performance pour les interactions utilisateur
function setupUserInteractionMetrics() {
  // Mesure du temps de réponse des clics
  document.addEventListener('click', (event) => {
    const timer = monitor.time('user-interaction', {
      type: 'click',
      target: event.target.tagName,
      id: event.target.id,
      className: event.target.className
    });
    
    // Simulate async operation
    requestAnimationFrame(() => {
      timer.end();
    });
  });
  
  // Mesure des temps de saisie
  let inputTimer;
  document.addEventListener('input', (event) => {
    if (inputTimer) inputTimer.end({ completed: false });
    
    inputTimer = monitor.time('user-input', {
      type: 'input',
      inputType: event.target.type,
      valueLength: event.target.value.length
    });
  });
  
  document.addEventListener('change', () => {
    if (inputTimer) {
      inputTimer.end({ completed: true });
      inputTimer = null;
    }
  });
}

// Monitoring des erreurs de performance
window.addEventListener('performance-warning', (event) => {
  const { metric, value, threshold } = event.detail;
  
  // Log pour debugging
  console.group(`🚨 Performance Warning: ${metric}`);
  console.log(`Value: ${value}ms`);
  console.log(`Threshold: ${threshold}ms`);
  console.log(`Exceeded by: ${((value / threshold - 1) * 100).toFixed(1)}%`);
  console.groupEnd();
  
  // Envoi optionnel vers un service de monitoring
  // sendToAnalytics('performance-warning', event.detail);
});
```

### Memory profiling avancé
```javascript
class MemoryProfiler {
  constructor() {
    this.baseline = null;
    this.snapshots = [];
    this.leakDetector = new LeakDetector();
  }
  
  takeSnapshot(label = `snapshot-${Date.now()}`) {
    if (performance.memory) {
      const snapshot = {
        label,
        timestamp: Date.now(),
        memory: {
          used: performance.memory.usedJSHeapSize,
          total: performance.memory.totalJSHeapSize,
          limit: performance.memory.jsHeapSizeLimit
        },
        usage: {
          percentage: (performance.memory.usedJSHeapSize / performance.memory.totalJSHeapSize) * 100,
          available: performance.memory.totalJSHeapSize - performance.memory.usedJSHeapSize
        }
      };
      
      this.snapshots.push(snapshot);
      
      if (!this.baseline) {
        this.baseline = snapshot;
      }
      
      return snapshot;
    }
    
    console.warn('Performance.memory API not available');
    return null;
  }
  
  compareTrend(windowSize = 10) {
    const recent = this.snapshots.slice(-windowSize);
    if (recent.length < 2) return null;
    
    const first = recent[0];
    const last = recent[recent.length - 1];
    
    const memoryGrowth = last.memory.used - first.memory.used;
    const timeElapsed = last.timestamp - first.timestamp;
    const growthRate = memoryGrowth / (timeElapsed / 1000); // bytes per second
    
    return {
      growth: memoryGrowth,
      rate: growthRate,
      trend: growthRate > 1024 * 1024 ? 'increasing' : // >1MB/s
              growthRate < -1024 * 1024 ? 'decreasing' : 'stable',
      recommendation: this.getMemoryRecommendation(growthRate, last.usage.percentage)
    };
  }
  
  getMemoryRecommendation(growthRate, usagePercentage) {
    const growthMBperS = growthRate / (1024 * 1024);
    
    if (usagePercentage > 90) {
      return 'CRITICAL: Memory usage > 90%. Check for memory leaks immediately.';
    } else if (usagePercentage > 70 && growthMBperS > 1) {
      return 'WARNING: High memory usage with significant growth. Monitor closely.';
    } else if (growthMBperS > 5) {
      return 'CAUTION: Rapid memory growth detected. Review recent changes.';
    } else {
      return 'OK: Memory usage within normal parameters.';
    }
  }
  
  detectLeaks() {
    return this.leakDetector.scan();
  }
  
  generateReport() {
    const latest = this.snapshots[this.snapshots.length - 1];
    const trend = this.compareTrend();
    const leaks = this.detectLeaks();
    
    return {
      summary: {
        current: latest,
        baseline: this.baseline,
        growth: latest ? latest.memory.used - this.baseline.memory.used : 0
      },
      trend,
      leaks,
      snapshots: this.snapshots.length,
      recommendations: this.generateRecommendations(trend, leaks)
    };
  }
  
  generateRecommendations(trend, leaks) {
    const recommendations = [];
    
    if (trend?.trend === 'increasing') {
      recommendations.push('Consider implementing object pooling for frequently created objects');
      recommendations.push('Review event listeners for proper cleanup');
      recommendations.push('Check for circular references in complex data structures');
    }
    
    if (leaks?.suspiciousPatterns.length > 0) {
      recommendations.push('Detected potential memory leaks - review identified patterns');
      recommendations.push('Implement WeakMap/WeakSet for temporary object references');
    }
    
    return recommendations;
  }
}

class LeakDetector {
  constructor() {
    this.trackedObjects = new WeakMap();
    this.suspiciousPatterns = [];
  }
  
  scan() {
    // Analyse des patterns suspects
    const domNodes = document.querySelectorAll('*').length;
    const eventListeners = this.countEventListeners();
    const timers = this.getActiveTimers();
    
    const analysis = {
      domNodeCount: domNodes,
      eventListenerCount: eventListeners,
      activeTimers: timers,
      suspiciousPatterns: this.identifySuspiciousPatterns()
    };
    
    return analysis;
  }
  
  countEventListeners() {
    // Approximation - dans un vrai profiler, utiliser DevTools Protocol
    let count = 0;
    const elements = document.querySelectorAll('*');
    
    elements.forEach(element => {
      const listeners = getEventListeners?.(element);
      if (listeners) {
        count += Object.keys(listeners).length;
      }
    });
    
    return count;
  }
  
  getActiveTimers() {
    // Tracking des timers actifs (nécessite instrumentation)
    return {
      intervals: window.__activeIntervals?.size || 0,
      timeouts: window.__activeTimeouts?.size || 0
    };
  }
  
  identifySuspiciousPatterns() {
    const patterns = [];
    
    // Pattern 1: Trop de listeners
    if (this.countEventListeners() > 1000) {
      patterns.push({
        type: 'excessive-listeners',
        severity: 'warning',
        description: 'High number of event listeners detected'
      });
    }
    
    // Pattern 2: DOM bloat
    if (document.querySelectorAll('*').length > 5000) {
      patterns.push({
        type: 'dom-bloat',
        severity: 'warning',
        description: 'DOM contains excessive number of elements'
      });
    }
    
    return patterns;
  }
}

// Usage
const memoryProfiler = new MemoryProfiler();

// Profiling automatique
setInterval(() => {
  memoryProfiler.takeSnapshot();
  
  const trend = memoryProfiler.compareTrend();
  if (trend?.trend === 'increasing') {
    console.warn('Memory trend:', trend);
  }
}, 30000); // Chaque 30 secondes

// Profiling manuel pour des opérations spécifiques
async function profileOperation(operation, name) {
  const beforeSnapshot = memoryProfiler.takeSnapshot(`${name}-before`);
  
  const result = await operation();
  
  const afterSnapshot = memoryProfiler.takeSnapshot(`${name}-after`);
  
  const memoryDelta = afterSnapshot.memory.used - beforeSnapshot.memory.used;
  
  console.log(`Operation ${name}:`);
  console.log(`Memory delta: ${(memoryDelta / 1024 / 1024).toFixed(2)} MB`);
  
  return result;
}
```
{% endtab %}

{% tab title="🚀 Optimisations CPU" %}
**Techniques avancées pour optimiser les performances CPU**

### Optimisation des boucles et algorithmes
```javascript
// Optimisations de boucles critiques
class LoopOptimizer {
  
  // Déroulage de boucles (loop unrolling)
  static processArrayUnrolled(array, processor) {
    const length = array.length;
    const remainder = length % 4;
    const limit = length - remainder;
    
    // Traiter 4 éléments à la fois
    for (let i = 0; i < limit; i += 4) {
      processor(array[i]);
      processor(array[i + 1]);
      processor(array[i + 2]);
      processor(array[i + 3]);
    }
    
    // Traiter les éléments restants
    for (let i = limit; i < length; i++) {
      processor(array[i]);
    }
  }
  
  // Optimisation avec utilisation efficace des caches
  static matrixMultiply(a, b) {
    const rowsA = a.length;
    const colsA = a[0].length;
    const colsB = b[0].length;
    
    // Initialisation optimisée
    const result = new Array(rowsA);
    for (let i = 0; i < rowsA; i++) {
      result[i] = new Array(colsB).fill(0);
    }
    
    // Multiplication avec optimisation cache (ijk -> ikj)
    for (let i = 0; i < rowsA; i++) {
      for (let k = 0; k < colsA; k++) {
        const aik = a[i][k]; // Cache la valeur
        for (let j = 0; j < colsB; j++) {
          result[i][j] += aik * b[k][j];
        }
      }
    }
    
    return result;
  }
  
  // Recherche optimisée avec early exit
  static findFirstMatch(array, predicate, startIndex = 0) {
    const length = array.length;
    
    // Vérification des limites
    if (startIndex >= length) return -1;
    
    // Recherche par blocs avec prefetch
    const blockSize = 8;
    
    for (let block = startIndex; block < length; block += blockSize) {
      const blockEnd = Math.min(block + blockSize, length);
      
      // Prefetch - aide le CPU à charger les données
      if (blockEnd + blockSize < length) {
        // Toucher la mémoire du prochain bloc
        void array[blockEnd + blockSize - 1];
      }
      
      // Traitement du bloc actuel
      for (let i = block; i < blockEnd; i++) {
        if (predicate(array[i], i)) {
          return i;
        }
      }
    }
    
    return -1;
  }
}

// Exemple d'usage
const largeArray = new Array(1000000).fill(0).map((_, i) => i);

// Comparaison de performance
console.time('Standard loop');
largeArray.forEach(x => x * 2);
console.timeEnd('Standard loop');

console.time('Unrolled loop');
LoopOptimizer.processArrayUnrolled(largeArray, x => x * 2);
console.timeEnd('Unrolled loop');
```

### Memoization et caching avancés
```javascript
// Cache intelligent avec LRU et TTL
class SmartCache {
  constructor(maxSize = 1000, ttl = 300000) { // 5 minutes par défaut
    this.maxSize = maxSize;
    this.ttl = ttl;
    this.cache = new Map();
    this.accessOrder = new Map();
    this.timers = new Map();
  }
  
  get(key) {
    const item = this.cache.get(key);
    if (!item) return undefined;
    
    // Vérifier l'expiration
    if (Date.now() > item.expires) {
      this.delete(key);
      return undefined;
    }
    
    // Mettre à jour l'ordre d'accès (LRU)
    this.accessOrder.delete(key);
    this.accessOrder.set(key, Date.now());
    
    return item.value;
  }
  
  set(key, value) {
    // Supprimer l'ancienne entrée si elle existe
    if (this.cache.has(key)) {
      this.delete(key);
    }
    
    // Vérifier la taille du cache
    if (this.cache.size >= this.maxSize) {
      this.evictLRU();
    }
    
    const expires = Date.now() + this.ttl;
    const item = { value, expires };
    
    this.cache.set(key, item);
    this.accessOrder.set(key, Date.now());
    
    // Timer pour nettoyage automatique
    const timer = setTimeout(() => {
      this.delete(key);
    }, this.ttl);
    
    this.timers.set(key, timer);
  }
  
  delete(key) {
    this.cache.delete(key);
    this.accessOrder.delete(key);
    
    const timer = this.timers.get(key);
    if (timer) {
      clearTimeout(timer);
      this.timers.delete(key);
    }
  }
  
  evictLRU() {
    if (this.accessOrder.size === 0) return;
    
    // Trouver la clé la moins récemment utilisée
    const [lruKey] = this.accessOrder;
    this.delete(lruKey);
  }
  
  clear() {
    this.cache.clear();
    this.accessOrder.clear();
    this.timers.forEach(timer => clearTimeout(timer));
    this.timers.clear();
  }
  
  size() {
    return this.cache.size;
  }
  
  // Statistiques du cache
  getStats() {
    const now = Date.now();
    let expiredCount = 0;
    
    for (const [key, item] of this.cache) {
      if (now > item.expires) {
        expiredCount++;
      }
    }
    
    return {
      size: this.cache.size,
      maxSize: this.maxSize,
      expired: expiredCount,
      utilizationRate: (this.cache.size / this.maxSize) * 100
    };
  }
}

// Memoization avec cache intelligent
function memoizeAdvanced(fn, options = {}) {
  const {
    maxSize = 100,
    ttl = 300000,
    keyGenerator = (...args) => JSON.stringify(args),
    onCacheHit = () => {},
    onCacheMiss = () => {}
  } = options;
  
  const cache = new SmartCache(maxSize, ttl);
  
  const memoized = function(...args) {
    const key = keyGenerator(...args);
    
    // Vérifier le cache
    let result = cache.get(key);
    if (result !== undefined) {
      onCacheHit(key);
      return result;
    }
    
    // Calculer et mettre en cache
    result = fn.apply(this, args);
    cache.set(key, result);
    onCacheMiss(key);
    
    return result;
  };
  
  // Méthodes utilitaires
  memoized.cache = cache;
  memoized.clear = () => cache.clear();
  memoized.delete = (key) => cache.delete(key);
  memoized.getStats = () => cache.getStats();
  
  return memoized;
}

// Exemples d'usage
const expensiveFunction = memoizeAdvanced((n) => {
  // Simulation d'un calcul coûteux
  let result = 0;
  for (let i = 0; i < n * 1000000; i++) {
    result += Math.sqrt(i);
  }
  return result;
}, {
  maxSize: 50,
  ttl: 60000, // 1 minute
  onCacheHit: (key) => console.log(`Cache hit: ${key}`),
  onCacheMiss: (key) => console.log(`Cache miss: ${key}`)
});

// Fibonacci avec memoization
const fibonacci = memoizeAdvanced((n) => {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.time('Fibonacci 40 (first call)');
console.log(fibonacci(40));
console.timeEnd('Fibonacci 40 (first call)');

console.time('Fibonacci 40 (cached)');
console.log(fibonacci(40));
console.timeEnd('Fibonacci 40 (cached)');
```

### Debouncing et Throttling sophistiqués
```javascript
// Debouncing avancé avec différentes stratégies
class AdvancedDebouncer {
  static leading(func, delay, options = {}) {
    let timeoutId;
    let lastCallTime;
    const { maxWait = Infinity } = options;
    
    return function debounced(...args) {
      const now = Date.now();
      const isInvoking = !timeoutId;
      
      lastCallTime = now;
      
      if (isInvoking) {
        func.apply(this, args);
      }
      
      clearTimeout(timeoutId);
      
      timeoutId = setTimeout(() => {
        const timeSinceLastCall = Date.now() - lastCallTime;
        
        if (timeSinceLastCall < delay && timeSinceLastCall >= 0) {
          timeoutId = setTimeout(arguments.callee, delay - timeSinceLastCall);
        } else {
          timeoutId = null;
        }
      }, delay);
    };
  }
  
  static trailing(func, delay, options = {}) {
    let timeoutId;
    let lastArgs;
    
    return function debounced(...args) {
      lastArgs = args;
      
      clearTimeout(timeoutId);
      
      timeoutId = setTimeout(() => {
        func.apply(this, lastArgs);
        timeoutId = null;
      }, delay);
    };
  }
  
  // Debouncing avec limite maximale d'attente
  static withMaxWait(func, delay, maxWait) {
    let timeoutId;
    let maxTimeoutId;
    let lastArgs;
    
    return function debounced(...args) {
      lastArgs = args;
      
      const invokeFunc = () => {
        clearTimeout(timeoutId);
        clearTimeout(maxTimeoutId);
        func.apply(this, lastArgs);
        timeoutId = null;
        maxTimeoutId = null;
      };
      
      clearTimeout(timeoutId);
      
      if (!maxTimeoutId) {
        maxTimeoutId = setTimeout(invokeFunc, maxWait);
      }
      
      timeoutId = setTimeout(invokeFunc, delay);
    };
  }
}

// Throttling avec contrôle précis
class AdvancedThrottler {
  static requestAnimationFrame(func) {
    let rafId;
    let lastArgs;
    
    return function throttled(...args) {
      lastArgs = args;
      
      if (!rafId) {
        rafId = requestAnimationFrame(() => {
          func.apply(this, lastArgs);
          rafId = null;
        });
      }
    };
  }
  
  static adaptive(func, baseDelay = 16) {
    let lastExecTime = 0;
    let timeoutId;
    let execCount = 0;
    let avgExecutionTime = 0;
    
    return function adaptive(...args) {
      const now = performance.now();
      
      clearTimeout(timeoutId);
      
      const executeFunction = () => {
        const execStart = performance.now();
        
        func.apply(this, args);
        
        const execTime = performance.now() - execStart;
        
        // Mise à jour de la moyenne d'exécution
        execCount++;
        avgExecutionTime = (avgExecutionTime * (execCount - 1) + execTime) / execCount;
        
        lastExecTime = now;
        
        // Ajuster le délai basé sur la performance
        const adaptiveDelay = Math.max(baseDelay, avgExecutionTime * 2);
        
        return adaptiveDelay;
      };
      
      const timeSinceLastExec = now - lastExecTime;
      const currentDelay = execCount === 0 ? baseDelay : Math.max(baseDelay, avgExecutionTime * 2);
      
      if (timeSinceLastExec >= currentDelay) {
        executeFunction();
      } else {
        timeoutId = setTimeout(executeFunction, currentDelay - timeSinceLastExec);
      }
    };
  }
}

// Exemples d'usage avancé
class PerformanceOptimizedComponent {
  constructor() {
    this.setupEventHandlers();
  }
  
  setupEventHandlers() {
    // Recherche avec debouncing intelligent
    this.handleSearch = AdvancedDebouncer.withMaxWait(
      this.performSearch.bind(this),
      300,  // délai normal
      2000  // délai maximum
    );
    
    // Scroll avec throttling adaptatif
    this.handleScroll = AdvancedThrottler.adaptive(
      this.updateScrollPosition.bind(this),
      16 // 60fps de base
    );
    
    // Resize avec requestAnimationFrame
    this.handleResize = AdvancedThrottler.requestAnimationFrame(
      this.updateLayout.bind(this)
    );
  }
  
  performSearch(query) {
    console.log('Searching for:', query);
    // Logique de recherche
  }
  
  updateScrollPosition(event) {
    console.log('Scroll position updated');
    // Mise à jour de l'interface
  }
  
  updateLayout() {
    console.log('Layout updated');
    // Recalcul du layout
  }
}

// Usage
const component = new PerformanceOptimizedComponent();

document.getElementById('search').addEventListener('input', (e) => {
  component.handleSearch(e.target.value);
});

window.addEventListener('scroll', component.handleScroll);
window.addEventListener('resize', component.handleResize);
```
{% endtab %}

{% tab title="💾 Optimisations Mémoire" %}
**Gestion avancée de la mémoire et prévention des fuites**

### Object Pooling pour les performances
```javascript
// Pool d'objets générique pour réduire le garbage collection
class ObjectPool {
  constructor(createFn, resetFn, initialSize = 10) {
    this.createFn = createFn;
    this.resetFn = resetFn;
    this.pool = [];
    this.created = 0;
    this.reused = 0;
    
    // Pré-créer les objets
    for (let i = 0; i < initialSize; i++) {
      this.pool.push(this.createFn());
    }
  }
  
  acquire() {
    if (this.pool.length > 0) {
      this.reused++;
      return this.pool.pop();
    }
    
    this.created++;
    return this.createFn();
  }
  
  release(obj) {
    if (this.resetFn) {
      this.resetFn(obj);
    }
    this.pool.push(obj);
  }
  
  getStats() {
    return {
      poolSize: this.pool.length,
      created: this.created,
      reused: this.reused,
      reuseRate: this.reused / (this.created + this.reused) * 100
    };
  }
}

// Exemple avec des objets géométriques fréquemment créés
class Vector2D {
  constructor(x = 0, y = 0) {
    this.x = x;
    this.y = y;
  }
  
  set(x, y) {
    this.x = x;
    this.y = y;
    return this;
  }
  
  add(other) {
    this.x += other.x;
    this.y += other.y;
    return this;
  }
  
  magnitude() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
  }
}

// Pool pour les vecteurs
const vectorPool = new ObjectPool(
  () => new Vector2D(),           // Création
  (vector) => vector.set(0, 0),   // Reset
  50                              // Taille initiale
);

// Pool pour les particules de jeu
class Particle {
  constructor() {
    this.x = 0;
    this.y = 0;
    this.vx = 0;
    this.vy = 0;
    this.life = 1.0;
    this.active = false;
  }
  
  init(x, y, vx, vy, life = 1.0) {
    this.x = x;
    this.y = y;
    this.vx = vx;
    this.vy = vy;
    this.life = life;
    this.active = true;
    return this;
  }
  
  update(deltaTime) {
    if (!this.active) return;
    
    this.x += this.vx * deltaTime;
    this.y += this.vy * deltaTime;
    this.life -= deltaTime;
    
    if (this.life <= 0) {
      this.active = false;
    }
  }
  
  reset() {
    this.x = 0;
    this.y = 0;
    this.vx = 0;
    this.vy = 0;
    this.life = 1.0;
    this.active = false;
  }
}

const particlePool = new ObjectPool(
  () => new Particle(),
  (particle) => particle.reset(),
  100
);

// Système de particules optimisé
class ParticleSystem {
  constructor() {
    this.particles = [];
    this.maxParticles = 1000;
  }
  
  emit(x, y, count = 1) {
    for (let i = 0; i < count && this.particles.length < this.maxParticles; i++) {
      const particle = particlePool.acquire();
      particle.init(
        x,
        y,
        (Math.random() - 0.5) * 200, // vx
        (Math.random() - 0.5) * 200, // vy
        Math.random() * 2 + 1         // life
      );
      
      this.particles.push(particle);
    }
  }
  
  update(deltaTime) {
    // Mise à jour en place pour éviter les allocations
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.update(deltaTime);
      
      if (!particle.active) {
        // Retourner au pool
        particlePool.release(particle);
        
        // Suppression efficace (swap avec le dernier)
        this.particles[i] = this.particles[this.particles.length - 1];
        this.particles.pop();
      }
    }
  }
  
  getStats() {
    return {
      activeParticles: this.particles.length,
      poolStats: particlePool.getStats()
    };
  }
}
```

### WeakMap et WeakSet pour éviter les fuites
```javascript
// Gestion des métadonnées sans fuites mémoire
class MetadataManager {
  constructor() {
    // WeakMap permet de stocker des données liées à des objets
    // sans empêcher leur garbage collection
    this.metadata = new WeakMap();
    this.observers = new WeakSet();
    this.timers = new WeakMap();
  }
  
  attachMetadata(object, data) {
    this.metadata.set(object, {
      ...data,
      attachedAt: Date.now(),
      id: Math.random().toString(36).substr(2, 9)
    });
  }
  
  getMetadata(object) {
    return this.metadata.get(object);
  }
  
  addObserver(object) {
    if (this.observers.has(object)) return;
    
    this.observers.add(object);
    
    // Auto-cleanup après 30 secondes
    const timerId = setTimeout(() => {
      this.removeObserver(object);
    }, 30000);
    
    this.timers.set(object, timerId);
  }
  
  removeObserver(object) {
    this.observers.delete(object);
    
    const timerId = this.timers.get(object);
    if (timerId) {
      clearTimeout(timerId);
      this.timers.delete(object);
    }
  }
  
  isObserver(object) {
    return this.observers.has(object);
  }
  
  // Nettoyage explicite (optionnel car WeakMap/WeakSet se nettoient automatiquement)
  cleanup() {
    // Nettoyer les timers actifs
    // Note: On ne peut pas itérer sur WeakMap, donc on garde une liste séparée si nécessaire
  }
}

// Usage sans risque de fuite mémoire
const metadataManager = new MetadataManager();

function createGameObjects() {
  const objects = [];
  
  for (let i = 0; i < 1000; i++) {
    const obj = {
      id: i,
      x: Math.random() * 800,
      y: Math.random() * 600
    };
    
    // Attachement de métadonnées - sera nettoyé automatiquement
    metadataManager.attachMetadata(obj, {
      type: 'gameObject',
      level: Math.floor(i / 100),
      powerUps: []
    });
    
    // Ajout comme observateur
    metadataManager.addObserver(obj);
    
    objects.push(obj);
  }
  
  return objects;
}

// Même si les objets sortent de la portée, pas de fuite mémoire
let gameObjects = createGameObjects();
console.log('Objects created:', gameObjects.length);

// Simulation de suppression d'objets
gameObjects = null; // Les métadonnées seront automatiquement nettoyées
```

### Lazy Loading et Chunking
```javascript
// Chargement paresseux de ressources lourdes
class LazyResourceManager {
  constructor() {
    this.resources = new Map();
    this.loadingPromises = new Map();
    this.loadStats = {
      requested: 0,
      loaded: 0,
      cached: 0,
      errors: 0
    };
  }
  
  async loadResource(url, options = {}) {
    this.loadStats.requested++;
    
    // Vérifier le cache
    if (this.resources.has(url)) {
      this.loadStats.cached++;
      return this.resources.get(url);
    }
    
    // Vérifier si déjà en cours de chargement
    if (this.loadingPromises.has(url)) {
      return this.loadingPromises.get(url);
    }
    
    // Démarrer le chargement
    const loadPromise = this.performLoad(url, options);
    this.loadingPromises.set(url, loadPromise);
    
    try {
      const resource = await loadPromise;
      this.resources.set(url, resource);
      this.loadStats.loaded++;
      return resource;
    } catch (error) {
      this.loadStats.errors++;
      throw error;
    } finally {
      this.loadingPromises.delete(url);
    }
  }
  
  async performLoad(url, options) {
    const { type = 'auto', timeout = 30000 } = options;
    
    // Timeout pour éviter les blocages
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), timeout);
    
    try {
      const response = await fetch(url, {
        signal: controller.signal,
        ...options.fetchOptions
      });
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }
      
      // Détection automatique du type
      const contentType = response.headers.get('content-type');
      const finalType = type === 'auto' ? this.detectType(url, contentType) : type;
      
      switch (finalType) {
        case 'json':
          return await response.json();
        case 'text':
          return await response.text();
        case 'blob':
          return await response.blob();
        case 'image':
          return await this.loadImage(response);
        default:
          return await response.arrayBuffer();
      }
    } finally {
      clearTimeout(timeoutId);
    }
  }
  
  detectType(url, contentType) {
    if (contentType?.includes('application/json')) return 'json';
    if (contentType?.includes('text/')) return 'text';
    if (contentType?.includes('image/')) return 'image';
    if (url.match(/\.(jpg|jpeg|png|gif|webp)$/i)) return 'image';
    if (url.match(/\.(json)$/i)) return 'json';
    return 'blob';
  }
  
  async loadImage(response) {
    const blob = await response.blob();
    const url = URL.createObjectURL(blob);
    
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.onload = () => {
        URL.revokeObjectURL(url);
        resolve(img);
      };
      img.onerror = () => {
        URL.revokeObjectURL(url);
        reject(new Error('Failed to load image'));
      };
      img.src = url;
    });
  }
  
  // Chunked loading pour gros datasets
  async loadChunked(url, chunkSize = 1000, processor) {
    const response = await fetch(url);
    const data = await response.json();
    
    if (!Array.isArray(data)) {
      throw new Error('Chunked loading requires array data');
    }
    
    const results = [];
    
    for (let i = 0; i < data.length; i += chunkSize) {
      const chunk = data.slice(i, i + chunkSize);
      
      // Traitement asynchrone du chunk
      const processedChunk = processor ? await processor(chunk) : chunk;
      results.push(...processedChunk);
      
      // Yield control pour éviter de bloquer l'UI
      await new Promise(resolve => setTimeout(resolve, 0));
    }
    
    return results;
  }
  
  // Préchargement intelligent
  preload(urls, priority = 'low') {
    return Promise.allSettled(
      urls.map(url => {
        return this.loadResource(url, { priority });
      })
    );
  }
  
  unload(url) {
    const resource = this.resources.get(url);
    if (resource) {
      // Cleanup spécifique par type
      if (resource instanceof Image) {
        resource.src = '';
      }
      if (resource instanceof Blob) {
        URL.revokeObjectURL(resource);
      }
      
      this.resources.delete(url);
    }
  }
  
  getStats() {
    return {
      ...this.loadStats,
      cacheSize: this.resources.size,
      pendingLoads: this.loadingPromises.size,
      hitRate: this.loadStats.cached / this.loadStats.requested * 100
    };
  }
}

// Usage pour une galerie d'images
class LazyImageGallery {
  constructor() {
    this.resourceManager = new LazyResourceManager();
    this.intersectionObserver = this.createIntersectionObserver();
    this.loadedImages = new Set();
  }
  
  createIntersectionObserver() {
    return new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.loadImage(entry.target);
        }
      });
    }, {
      root: null,
      rootMargin: '100px', // Charger 100px avant que l'image soit visible
      threshold: 0.1
    });
  }
  
  async loadImage(imgElement) {
    const src = imgElement.dataset.src;
    if (!src || this.loadedImages.has(src)) return;
    
    this.loadedImages.add(src);
    
    try {
      // Placeholder pendant le chargement
      imgElement.style.filter = 'blur(5px)';
      
      const image = await this.resourceManager.loadResource(src, { type: 'image' });
      
      // Transition fluide
      imgElement.src = image.src;
      imgElement.style.filter = '';
      imgElement.classList.add('loaded');
      
      // Arrêter d'observer cet élément
      this.intersectionObserver.unobserve(imgElement);
      
    } catch (error) {
      console.error('Failed to load image:', src, error);
      imgElement.classList.add('error');
    }
  }
  
  observe(imgElement) {
    this.intersectionObserver.observe(imgElement);
  }
  
  getStats() {
    return this.resourceManager.getStats();
  }
}

// Usage
const gallery = new LazyImageGallery();

// Observer toutes les images lazy
document.querySelectorAll('img[data-src]').forEach(img => {
  gallery.observe(img);
});

// Stats périodiques
setInterval(() => {
  console.log('Gallery stats:', gallery.getStats());
}, 10000);
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Optimisation Complète d'Application

{% hint style="warning" %}
**Défi :** Optimisez une application complète avec toutes les techniques avancées
{% endhint %}

```typescript
// Créez un système d'optimisation complet
interface PerformanceOptimizedApp {
    // TODO: Profiling et monitoring
    monitoring: {
        performance: 'Real-time metrics';
        memory: 'Leak detection et usage tracking';
        user: 'Core Web Vitals et UX metrics';
        errors: 'Performance bottlenecks';
    };
    
    // TODO: Optimisations CPU
    cpu: {
        algorithms: 'Loop unrolling et cache optimization';
        memoization: 'Smart caching avec LRU/TTL';
        debouncing: 'Event handling optimization';
        lazy: 'Lazy evaluation et chunking';
    };
    
    // TODO: Optimisations mémoire
    memory: {
        pooling: 'Object pools pour hot paths';
        weakRefs: 'WeakMap/WeakSet pour metadata';
        cleanup: 'Automatic garbage collection';
        streaming: 'Large data processing';
    };
    
    // TODO: Optimisations réseau
    network: {
        caching: 'Intelligent resource caching';
        compression: 'Data compression strategies';
        bundling: 'Code splitting et lazy loading';
        prefetching: 'Predictive loading';
    };
}

// TODO: Implémentez le système complet
class HighPerformanceApplication {
    constructor() {
        this.setupMonitoring();
        this.setupOptimizations();
        this.setupNetworkOptimizations();
    }
    
    // TODO: Monitoring en temps réel
    setupMonitoring() {
        // Core Web Vitals
        // Performance Observer
        // Memory tracking
        // Error boundaries
        // User interaction metrics
    }
    
    // TODO: Optimisations CPU/Memory
    setupOptimizations() {
        // Object pooling
        // Smart caching
        // Event optimization
        // Algorithm improvements
    }
    
    // TODO: Optimisations réseau
    setupNetworkOptimizations() {
        // Resource management
        // Lazy loading
        // Compression
        // Caching strategies
    }
    
    // TODO: Web Workers pour calculs lourds
    setupWebWorkers() {
        // Background processing
        // Main thread liberation
        // Parallel computations
    }
    
    // TODO: Adaptive performance
    setupAdaptivePerformance() {
        // Device capability detection
        // Dynamic quality adjustment
        // Progressive enhancement
    }
}

// Exemple d'optimisation pour une SPA complexe
class OptimizedSPA {
    // TODO: Router avec code splitting
    // TODO: Component lazy loading
    // TODO: State management optimization
    // TODO: Virtual scrolling
    // TODO: Image optimization
    // TODO: Bundle optimization
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```typescript
// Structure de base
class PerformanceOptimizer {
  constructor() {
    this.monitor = new PerformanceMonitor();
    this.cache = new SmartCache();
    this.pools = new Map();
  }
  
  optimize(target) {
    // Analyse des performances
    const metrics = this.monitor.analyze(target);
    
    // Application des optimisations
    return this.applyOptimizations(target, metrics);
  }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
Solution complète avec monitoring temps réel, optimisations automatiques et analytics détaillées dans l'onglet suivant.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Excellent !** Vous maîtrisez maintenant l'optimisation JavaScript au niveau expert !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Performance profiling** avec outils natifs
- **Memory management** avancé et leak detection
- **Algorithm optimization** et cache strategies
- **Event optimization** avec debouncing/throttling
- **Resource management** intelligent
- **Web Workers** et parallelization
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Analyser et mesurer les performances
- Implémenter des systèmes de cache sophistiqués
- Optimiser les algorithmes critiques
- Gérer la mémoire efficacement
- Créer des applications hautement performantes
- Monitorer les performances en production
{% endtab %}

{% tab title="🚀 Applications professionnelles" %}
- **Applications haute performance** (trading, gaming)
- **Progressive Web Apps** optimisées
- **Data visualization** fluide
- **Real-time applications** scalables
- **Mobile-first** experiences
- **Enterprise dashboards** performants
{% endtab %}
{% endtabs %}

---

**Prochaine étape : Outils et Écosystème !** 🛠️
