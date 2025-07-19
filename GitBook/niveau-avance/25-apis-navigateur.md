# 🌐 APIs du Navigateur

> **Niveau :** 🔴 Avancé | **Durée :** 80 minutes | **Prérequis :** DOM, Async/Await, Gestion d'erreurs

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** Fetch API pour les requêtes HTTP avancées
- [ ] **Utiliser** Web Storage pour la persistance de données
- [ ] **Exploiter** les APIs modernes du navigateur (Geolocation, Notifications, etc.)
- [ ] **Créer** des applications web riches avec APIs natives

---

## 📚 Vue d'ensemble des APIs

{% tabs %}
{% tab title="🌍 Écosystème des APIs" %}
**Classification des APIs navigateur :**

### APIs Réseau
- 🌐 **Fetch API** - Requêtes HTTP modernes
- 🔌 **WebSocket** - Communication bidirectionnelle
- 📡 **Server-Sent Events** - Push notifications

### APIs de Stockage
- 💾 **localStorage** - Persistance locale
- 🗃️ **sessionStorage** - Stockage de session
- 🏪 **IndexedDB** - Base de données client
- 🍪 **Cache API** - Mise en cache des ressources

### APIs Système
- 📍 **Geolocation** - Position géographique
- 🔔 **Notifications** - Alertes système
- 📱 **Device APIs** - Caméra, microphone, capteurs
- ⚡ **Performance** - Monitoring des performances
{% endtab %}

{% tab title="📊 Comparaison technologies" %}
| API | Support | Performance | Sécurité | Cas d'usage |
|-----|---------|-------------|----------|-------------|
| **Fetch** | ✅ Universel | ⚡ Excellente | 🔒 CORS/CSP | HTTP requests |
| **WebSocket** | ✅ Universel | ⚡⚡ Temps réel | 🔒 WSS/Origin | Chat, live data |
| **localStorage** | ✅ Universel | ⚡ Synchrone | ⚠️ XSS | Préférences UI |
| **IndexedDB** | ✅ Universel | ⚡⚡ Asynchrone | 🔒 Same-origin | Données complexes |
| **Geolocation** | ✅ Mobile/Desktop | ⚡ GPS dépendant | 🔒 HTTPS requis | Maps, localisation |
| **Notifications** | ✅ Moderne | ⚡ Native | 🔒 Permissions | Alertes utilisateur |

**Évolution historique :**
```timeline
2005: XMLHttpRequest (AJAX révolution)
2012: localStorage/sessionStorage standardisés
2015: Fetch API introduction
2016: Service Workers et Cache API
2018: Payment Request API
2020: Web Assembly APIs
2024: WebGPU et nouvelles capacités
```
{% endtab %}

{% tab title="🎯 Stratégies utilisation" %}
**Patterns d'implémentation :**

### 1. Progressive Enhancement
```javascript
// Détection de fonctionnalités
const hasGeolocation = 'geolocation' in navigator;
const hasNotifications = 'Notification' in window;
const hasServiceWorker = 'serviceWorker' in navigator;

// Fallbacks gracieux
function getLocation() {
    if (hasGeolocation) {
        return navigator.geolocation.getCurrentPosition();
    } else {
        return Promise.reject('Geolocation not supported');
    }
}
```

### 2. Error Handling Strategy
```javascript
class APIError extends Error {
    constructor(message, status, code) {
        super(message);
        this.name = 'APIError';
        this.status = status;
        this.code = code;
    }
}

async function safeAPICall(url, options) {
    try {
        const response = await fetch(url, options);
        if (!response.ok) {
            throw new APIError(
                `HTTP ${response.status}`,
                response.status,
                'HTTP_ERROR'
            );
        }
        return await response.json();
    } catch (error) {
        if (error instanceof APIError) throw error;
        throw new APIError('Network error', 0, 'NETWORK_ERROR');
    }
}
```

### 3. Performance Optimization
```javascript
// Request batching
class RequestBatcher {
    constructor(delay = 10) {
        this.queue = [];
        this.delay = delay;
        this.timeoutId = null;
    }
    
    add(request) {
        this.queue.push(request);
        this.scheduleFlush();
    }
    
    scheduleFlush() {
        if (this.timeoutId) return;
        this.timeoutId = setTimeout(() => {
            this.flush();
        }, this.delay);
    }
    
    async flush() {
        const batch = this.queue.splice(0);
        this.timeoutId = null;
        
        if (batch.length === 0) return;
        
        // Traitement par lot
        const responses = await Promise.allSettled(
            batch.map(req => fetch(req.url, req.options))
        );
        
        responses.forEach((result, index) => {
            const { resolve, reject } = batch[index];
            if (result.status === 'fulfilled') {
                resolve(result.value);
            } else {
                reject(result.reason);
            }
        });
    }
}
```
{% endtab %}
{% endtabs %}

---

## 🌐 Fetch API Avancée

### Requêtes sophistiquées

{% tabs %}
{% tab title="🚀 Configuration avancée" %}
```javascript
// Configuration complète de Fetch
class APIClient {
    constructor(baseURL, options = {}) {
        this.baseURL = baseURL;
        this.defaultOptions = {
            timeout: 10000,
            retries: 3,
            retryDelay: 1000,
            ...options
        };
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseURL}${endpoint}`;
        const config = this.buildConfig(options);
        
        return this.executeWithRetry(url, config);
    }
    
    buildConfig(options) {
        return {
            ...this.defaultOptions,
            ...options,
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json',
                ...this.defaultOptions.headers,
                ...options.headers
            }
        };
    }
    
    async executeWithRetry(url, config, attempt = 1) {
        const controller = new AbortController();
        const timeoutId = setTimeout(() => {
            controller.abort();
        }, config.timeout);
        
        try {
            const response = await fetch(url, {
                ...config,
                signal: controller.signal
            });
            
            clearTimeout(timeoutId);
            
            if (!response.ok) {
                throw new APIError(
                    `HTTP ${response.status}: ${response.statusText}`,
                    response.status
                );
            }
            
            return this.parseResponse(response);
            
        } catch (error) {
            clearTimeout(timeoutId);
            
            // Retry logic
            if (attempt < config.retries && this.shouldRetry(error)) {
                await this.delay(config.retryDelay * attempt);
                return this.executeWithRetry(url, config, attempt + 1);
            }
            
            throw error;
        }
    }
    
    shouldRetry(error) {
        // Retry on network errors, timeouts, and 5xx responses
        return error.name === 'AbortError' ||
               error.name === 'TypeError' ||
               (error.status >= 500 && error.status < 600);
    }
    
    async parseResponse(response) {
        const contentType = response.headers.get('content-type');
        
        if (contentType?.includes('application/json')) {
            return await response.json();
        }
        
        if (contentType?.includes('text/')) {
            return await response.text();
        }
        
        if (contentType?.includes('application/octet-stream')) {
            return await response.blob();
        }
        
        return response;
    }
    
    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
    
    // Méthodes de convenance
    get(endpoint, options = {}) {
        return this.request(endpoint, { ...options, method: 'GET' });
    }
    
    post(endpoint, data, options = {}) {
        return this.request(endpoint, {
            ...options,
            method: 'POST',
            body: JSON.stringify(data)
        });
    }
    
    put(endpoint, data, options = {}) {
        return this.request(endpoint, {
            ...options,
            method: 'PUT',
            body: JSON.stringify(data)
        });
    }
    
    delete(endpoint, options = {}) {
        return this.request(endpoint, { ...options, method: 'DELETE' });
    }
    
    // Upload avec progress
    async upload(endpoint, file, onProgress) {
        const formData = new FormData();
        formData.append('file', file);
        
        return new Promise((resolve, reject) => {
            const xhr = new XMLHttpRequest();
            
            xhr.upload.addEventListener('progress', (event) => {
                if (event.lengthComputable) {
                    const progress = (event.loaded / event.total) * 100;
                    onProgress?.(Math.round(progress));
                }
            });
            
            xhr.addEventListener('load', () => {
                if (xhr.status >= 200 && xhr.status < 300) {
                    resolve(JSON.parse(xhr.responseText));
                } else {
                    reject(new APIError(`Upload failed: ${xhr.status}`));
                }
            });
            
            xhr.addEventListener('error', () => {
                reject(new APIError('Upload network error'));
            });
            
            xhr.open('POST', `${this.baseURL}${endpoint}`);
            xhr.send(formData);
        });
    }
}

// Utilisation
const api = new APIClient('https://api.example.com/v1', {
    headers: {
        'Authorization': 'Bearer ' + localStorage.getItem('token')
    },
    timeout: 15000,
    retries: 3
});

// Exemples d'utilisation
try {
    const users = await api.get('/users');
    const newUser = await api.post('/users', { name: 'Alice', email: 'alice@example.com' });
    
    // Upload avec progress
    const fileInput = document.querySelector('#file-input');
    const file = fileInput.files[0];
    
    await api.upload('/upload', file, (progress) => {
        console.log(`Upload progress: ${progress}%`);
    });
    
} catch (error) {
    console.error('API Error:', error.message);
}
```
{% endtab %}

{% tab title="⚡ Optimisations performance" %}
```javascript
// Cache intelligent pour les requêtes
class CachedAPIClient extends APIClient {
    constructor(baseURL, options = {}) {
        super(baseURL, options);
        this.cache = new Map();
        this.maxCacheSize = options.maxCacheSize || 100;
        this.defaultTTL = options.cacheTTL || 300000; // 5 minutes
    }
    
    async get(endpoint, options = {}) {
        const cacheKey = this.getCacheKey(endpoint, options);
        const cached = this.getFromCache(cacheKey);
        
        if (cached && !this.isExpired(cached)) {
            return cached.data;
        }
        
        const data = await super.get(endpoint, options);
        this.setCache(cacheKey, data, options.cacheTTL);
        
        return data;
    }
    
    getCacheKey(endpoint, options) {
        const params = new URLSearchParams(options.params || {});
        return `${endpoint}?${params.toString()}`;
    }
    
    getFromCache(key) {
        return this.cache.get(key);
    }
    
    isExpired(cached) {
        return Date.now() > cached.expiresAt;
    }
    
    setCache(key, data, ttl = this.defaultTTL) {
        // LRU eviction
        if (this.cache.size >= this.maxCacheSize) {
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);
        }
        
        this.cache.set(key, {
            data,
            expiresAt: Date.now() + ttl,
            createdAt: Date.now()
        });
    }
    
    clearCache(pattern) {
        if (pattern) {
            for (const key of this.cache.keys()) {
                if (key.includes(pattern)) {
                    this.cache.delete(key);
                }
            }
        } else {
            this.cache.clear();
        }
    }
    
    // Request deduplication
    pendingRequests = new Map();
    
    async dedupedRequest(endpoint, options) {
        const key = this.getCacheKey(endpoint, options);
        
        if (this.pendingRequests.has(key)) {
            return this.pendingRequests.get(key);
        }
        
        const promise = super.request(endpoint, options);
        this.pendingRequests.set(key, promise);
        
        try {
            const result = await promise;
            return result;
        } finally {
            this.pendingRequests.delete(key);
        }
    }
}

// Intercepteurs pour logging et monitoring
class MonitoredAPIClient extends CachedAPIClient {
    constructor(baseURL, options = {}) {
        super(baseURL, options);
        this.requestInterceptors = [];
        this.responseInterceptors = [];
        this.metrics = {
            requests: 0,
            errors: 0,
            totalTime: 0,
            averageTime: 0
        };
    }
    
    addRequestInterceptor(interceptor) {
        this.requestInterceptors.push(interceptor);
    }
    
    addResponseInterceptor(interceptor) {
        this.responseInterceptors.push(interceptor);
    }
    
    async request(endpoint, options = {}) {
        const startTime = performance.now();
        const requestId = Math.random().toString(36).substr(2, 9);
        
        // Apply request interceptors
        let processedOptions = options;
        for (const interceptor of this.requestInterceptors) {
            processedOptions = await interceptor(endpoint, processedOptions, requestId);
        }
        
        try {
            this.metrics.requests++;
            
            const response = await super.request(endpoint, processedOptions);
            const endTime = performance.now();
            const duration = endTime - startTime;
            
            this.updateMetrics(duration);
            
            // Apply response interceptors
            let processedResponse = response;
            for (const interceptor of this.responseInterceptors) {
                processedResponse = await interceptor(
                    processedResponse, 
                    endpoint, 
                    processedOptions, 
                    requestId,
                    duration
                );
            }
            
            return processedResponse;
            
        } catch (error) {
            this.metrics.errors++;
            const endTime = performance.now();
            const duration = endTime - startTime;
            
            this.updateMetrics(duration);
            
            // Log error
            console.error(`API Request failed [${requestId}]:`, {
                endpoint,
                options: processedOptions,
                error: error.message,
                duration
            });
            
            throw error;
        }
    }
    
    updateMetrics(duration) {
        this.metrics.totalTime += duration;
        this.metrics.averageTime = this.metrics.totalTime / this.metrics.requests;
    }
    
    getMetrics() {
        return { ...this.metrics };
    }
    
    resetMetrics() {
        this.metrics = {
            requests: 0,
            errors: 0,
            totalTime: 0,
            averageTime: 0
        };
    }
}

// Utilisation avec intercepteurs
const api = new MonitoredAPIClient('https://api.example.com');

// Intercepteur de logging
api.addRequestInterceptor(async (endpoint, options, requestId) => {
    console.log(`[${requestId}] Request: ${options.method || 'GET'} ${endpoint}`);
    return options;
});

api.addResponseInterceptor(async (response, endpoint, options, requestId, duration) => {
    console.log(`[${requestId}] Response: ${endpoint} (${duration.toFixed(2)}ms)`);
    return response;
});

// Intercepteur d'authentification
api.addRequestInterceptor(async (endpoint, options, requestId) => {
    const token = localStorage.getItem('authToken');
    if (token) {
        options.headers = {
            ...options.headers,
            'Authorization': `Bearer ${token}`
        };
    }
    return options;
});
```
{% endtab %}

{% tab title="🔄 Streaming et WebStreams" %}
```javascript
// Traitement de flux de données volumineux
class StreamingAPIClient extends APIClient {
    
    // Stream download avec progress
    async streamDownload(endpoint, onProgress, onChunk) {
        const response = await fetch(`${this.baseURL}${endpoint}`);
        
        if (!response.ok) {
            throw new APIError(`Download failed: ${response.status}`);
        }
        
        const contentLength = response.headers.get('content-length');
        const total = contentLength ? parseInt(contentLength) : 0;
        let loaded = 0;
        
        const reader = response.body.getReader();
        const chunks = [];
        
        try {
            while (true) {
                const { done, value } = await reader.read();
                
                if (done) break;
                
                chunks.push(value);
                loaded += value.length;
                
                if (onProgress && total > 0) {
                    onProgress((loaded / total) * 100);
                }
                
                if (onChunk) {
                    onChunk(value, loaded, total);
                }
            }
            
            // Combiner tous les chunks
            const totalLength = chunks.reduce((acc, chunk) => acc + chunk.length, 0);
            const result = new Uint8Array(totalLength);
            let offset = 0;
            
            for (const chunk of chunks) {
                result.set(chunk, offset);
                offset += chunk.length;
            }
            
            return result;
            
        } finally {
            reader.releaseLock();
        }
    }
    
    // Stream JSON processing pour grandes réponses
    async streamJSON(endpoint, onObject) {
        const response = await fetch(`${this.baseURL}${endpoint}`);
        
        if (!response.ok) {
            throw new APIError(`Stream failed: ${response.status}`);
        }
        
        const reader = response.body
            .pipeThrough(new TextDecoderStream())
            .pipeThrough(new JSONParserStream())
            .getReader();
        
        try {
            while (true) {
                const { done, value } = await reader.read();
                if (done) break;
                
                onObject(value);
            }
        } finally {
            reader.releaseLock();
        }
    }
    
    // Server-Sent Events client
    createEventStream(endpoint, options = {}) {
        const url = `${this.baseURL}${endpoint}`;
        const eventSource = new EventSource(url);
        
        const stream = {
            on(event, handler) {
                eventSource.addEventListener(event, handler);
                return this;
            },
            
            close() {
                eventSource.close();
            },
            
            get readyState() {
                return eventSource.readyState;
            }
        };
        
        return stream;
    }
}

// Custom JSON parser stream
class JSONParserStream extends TransformStream {
    constructor() {
        let buffer = '';
        let braceCount = 0;
        let inString = false;
        let escaped = false;
        
        super({
            transform(chunk, controller) {
                buffer += chunk;
                
                for (let i = 0; i < buffer.length; i++) {
                    const char = buffer[i];
                    
                    if (escaped) {
                        escaped = false;
                        continue;
                    }
                    
                    if (char === '\\') {
                        escaped = true;
                        continue;
                    }
                    
                    if (char === '"') {
                        inString = !inString;
                        continue;
                    }
                    
                    if (inString) continue;
                    
                    if (char === '{') {
                        braceCount++;
                    } else if (char === '}') {
                        braceCount--;
                        
                        if (braceCount === 0) {
                            const jsonStr = buffer.substring(0, i + 1);
                            try {
                                const obj = JSON.parse(jsonStr);
                                controller.enqueue(obj);
                            } catch (e) {
                                console.warn('Failed to parse JSON:', jsonStr);
                            }
                            
                            buffer = buffer.substring(i + 1);
                            i = -1; // Reset loop
                        }
                    }
                }
            }
        });
    }
}

// Utilisation des streams
const streamClient = new StreamingAPIClient('https://api.example.com');

// Download avec progress
await streamClient.streamDownload('/large-file.zip', 
    (progress) => {
        console.log(`Download: ${progress.toFixed(2)}%`);
        updateProgressBar(progress);
    },
    (chunk, loaded, total) => {
        console.log(`Received ${chunk.length} bytes (${loaded}/${total})`);
    }
);

// Stream JSON pour traiter de gros datasets
await streamClient.streamJSON('/big-data', (object) => {
    processDataItem(object);
});

// Server-Sent Events
const eventStream = streamClient.createEventStream('/events')
    .on('message', (event) => {
        console.log('New message:', JSON.parse(event.data));
    })
    .on('notification', (event) => {
        showNotification(JSON.parse(event.data));
    })
    .on('error', (event) => {
        console.error('Stream error:', event);
    });

// Fermer le stream plus tard
setTimeout(() => {
    eventStream.close();
}, 60000);
```
{% endtab %}
{% endtabs %}

---

## 💾 Web Storage Avancé

### Stratégies de persistence

{% tabs %}
{% tab title="🗄️ Storage Management" %}
```javascript
// Gestionnaire de stockage unifié
class StorageManager {
    constructor() {
        this.adapters = {
            local: new LocalStorageAdapter(),
            session: new SessionStorageAdapter(),
            indexed: new IndexedDBAdapter(),
            memory: new MemoryAdapter()
        };
        
        this.defaultAdapter = 'local';
        this.compressionThreshold = 1024; // 1KB
    }
    
    // Interface unifiée
    async set(key, value, options = {}) {
        const adapter = this.getAdapter(options.storage);
        const serialized = this.serialize(value, options);
        
        // Compression si nécessaire
        const finalValue = serialized.length > this.compressionThreshold
            ? await this.compress(serialized)
            : serialized;
        
        return adapter.set(key, finalValue, options);
    }
    
    async get(key, options = {}) {
        const adapter = this.getAdapter(options.storage);
        const value = await adapter.get(key);
        
        if (value === null) return null;
        
        // Décompression si nécessaire
        const decompressed = await this.tryDecompress(value);
        return this.deserialize(decompressed, options);
    }
    
    async remove(key, options = {}) {
        const adapter = this.getAdapter(options.storage);
        return adapter.remove(key);
    }
    
    async clear(options = {}) {
        const adapter = this.getAdapter(options.storage);
        return adapter.clear();
    }
    
    async keys(options = {}) {
        const adapter = this.getAdapter(options.storage);
        return adapter.keys();
    }
    
    getAdapter(storage = this.defaultAdapter) {
        return this.adapters[storage];
    }
    
    serialize(value, options) {
        if (options.raw) return value;
        
        return JSON.stringify({
            data: value,
            timestamp: Date.now(),
            version: options.version || 1,
            compressed: false
        });
    }
    
    deserialize(value, options) {
        if (options.raw) return value;
        
        try {
            const parsed = JSON.parse(value);
            
            // Vérification d'expiration
            if (options.ttl && Date.now() - parsed.timestamp > options.ttl) {
                return null;
            }
            
            // Migration de version si nécessaire
            if (options.migrate && parsed.version !== options.version) {
                return options.migrate(parsed.data, parsed.version, options.version);
            }
            
            return parsed.data;
        } catch (e) {
            return value; // Fallback pour les valeurs non-JSON
        }
    }
    
    async compress(data) {
        if (!('CompressionStream' in window)) {
            return data; // Pas de compression supportée
        }
        
        const stream = new CompressionStream('gzip');
        const writer = stream.writable.getWriter();
        const reader = stream.readable.getReader();
        
        writer.write(new TextEncoder().encode(data));
        writer.close();
        
        const chunks = [];
        let done = false;
        
        while (!done) {
            const { value, done: readerDone } = await reader.read();
            done = readerDone;
            if (value) chunks.push(value);
        }
        
        const compressed = new Uint8Array(
            chunks.reduce((acc, chunk) => acc + chunk.length, 0)
        );
        
        let offset = 0;
        for (const chunk of chunks) {
            compressed.set(chunk, offset);
            offset += chunk.length;
        }
        
        return btoa(String.fromCharCode(...compressed));
    }
    
    async tryDecompress(data) {
        // Détection simple de données compressées
        if (typeof data === 'string' && data.startsWith('H4sI')) {
            return this.decompress(data);
        }
        return data;
    }
    
    async decompress(compressedData) {
        if (!('DecompressionStream' in window)) {
            return compressedData;
        }
        
        const bytes = Uint8Array.from(atob(compressedData), c => c.charCodeAt(0));
        const stream = new DecompressionStream('gzip');
        const writer = stream.writable.getWriter();
        const reader = stream.readable.getReader();
        
        writer.write(bytes);
        writer.close();
        
        const chunks = [];
        let done = false;
        
        while (!done) {
            const { value, done: readerDone } = await reader.read();
            done = readerDone;
            if (value) chunks.push(value);
        }
        
        return new TextDecoder().decode(
            new Uint8Array(chunks.reduce((acc, chunk) => [...acc, ...chunk], []))
        );
    }
    
    // Monitoring de l'espace disponible
    async getStorageQuota() {
        if ('storage' in navigator && 'estimate' in navigator.storage) {
            return await navigator.storage.estimate();
        }
        return null;
    }
    
    // Nettoyage automatique
    async cleanup(maxAge = 7 * 24 * 60 * 60 * 1000) { // 7 jours
        const keys = await this.keys();
        const now = Date.now();
        
        for (const key of keys) {
            try {
                const raw = await this.adapters.local.get(key);
                const parsed = JSON.parse(raw);
                
                if (parsed.timestamp && now - parsed.timestamp > maxAge) {
                    await this.remove(key);
                }
            } catch (e) {
                // Ignorer les erreurs de parsing
            }
        }
    }
}

// Adapters pour différents types de stockage
class LocalStorageAdapter {
    async set(key, value, options) {
        try {
            localStorage.setItem(key, value);
            return true;
        } catch (e) {
            if (e.name === 'QuotaExceededError') {
                throw new StorageError('Local storage quota exceeded');
            }
            throw e;
        }
    }
    
    async get(key) {
        return localStorage.getItem(key);
    }
    
    async remove(key) {
        localStorage.removeItem(key);
        return true;
    }
    
    async clear() {
        localStorage.clear();
        return true;
    }
    
    async keys() {
        return Object.keys(localStorage);
    }
}

class SessionStorageAdapter {
    async set(key, value, options) {
        sessionStorage.setItem(key, value);
        return true;
    }
    
    async get(key) {
        return sessionStorage.getItem(key);
    }
    
    async remove(key) {
        sessionStorage.removeItem(key);
        return true;
    }
    
    async clear() {
        sessionStorage.clear();
        return true;
    }
    
    async keys() {
        return Object.keys(sessionStorage);
    }
}

class IndexedDBAdapter {
    constructor(dbName = 'AppStorage', version = 1) {
        this.dbName = dbName;
        this.version = version;
        this.storeName = 'keyvalue';
    }
    
    async getDB() {
        if (this.db) return this.db;
        
        return new Promise((resolve, reject) => {
            const request = indexedDB.open(this.dbName, this.version);
            
            request.onerror = () => reject(request.error);
            request.onsuccess = () => {
                this.db = request.result;
                resolve(this.db);
            };
            
            request.onupgradeneeded = (event) => {
                const db = event.target.result;
                if (!db.objectStoreNames.contains(this.storeName)) {
                    db.createObjectStore(this.storeName);
                }
            };
        });
    }
    
    async set(key, value, options) {
        const db = await this.getDB();
        const transaction = db.transaction([this.storeName], 'readwrite');
        const store = transaction.objectStore(this.storeName);
        
        return new Promise((resolve, reject) => {
            const request = store.put(value, key);
            request.onerror = () => reject(request.error);
            request.onsuccess = () => resolve(true);
        });
    }
    
    async get(key) {
        const db = await this.getDB();
        const transaction = db.transaction([this.storeName], 'readonly');
        const store = transaction.objectStore(this.storeName);
        
        return new Promise((resolve, reject) => {
            const request = store.get(key);
            request.onerror = () => reject(request.error);
            request.onsuccess = () => resolve(request.result || null);
        });
    }
    
    async remove(key) {
        const db = await this.getDB();
        const transaction = db.transaction([this.storeName], 'readwrite');
        const store = transaction.objectStore(this.storeName);
        
        return new Promise((resolve, reject) => {
            const request = store.delete(key);
            request.onerror = () => reject(request.error);
            request.onsuccess = () => resolve(true);
        });
    }
    
    async clear() {
        const db = await this.getDB();
        const transaction = db.transaction([this.storeName], 'readwrite');
        const store = transaction.objectStore(this.storeName);
        
        return new Promise((resolve, reject) => {
            const request = store.clear();
            request.onerror = () => reject(request.error);
            request.onsuccess = () => resolve(true);
        });
    }
    
    async keys() {
        const db = await this.getDB();
        const transaction = db.transaction([this.storeName], 'readonly');
        const store = transaction.objectStore(this.storeName);
        
        return new Promise((resolve, reject) => {
            const request = store.getAllKeys();
            request.onerror = () => reject(request.error);
            request.onsuccess = () => resolve(request.result);
        });
    }
}

class MemoryAdapter {
    constructor() {
        this.data = new Map();
    }
    
    async set(key, value, options) {
        this.data.set(key, value);
        return true;
    }
    
    async get(key) {
        return this.data.get(key) || null;
    }
    
    async remove(key) {
        return this.data.delete(key);
    }
    
    async clear() {
        this.data.clear();
        return true;
    }
    
    async keys() {
        return Array.from(this.data.keys());
    }
}

class StorageError extends Error {
    constructor(message) {
        super(message);
        this.name = 'StorageError';
    }
}

// Utilisation
const storage = new StorageManager();

// Stockage simple
await storage.set('user-preferences', {
    theme: 'dark',
    language: 'fr',
    notifications: true
});

// Stockage avec expiration
await storage.set('api-cache', data, {
    ttl: 60000, // 1 minute
    storage: 'session'
});

// Stockage avec compression automatique
await storage.set('large-dataset', bigData, {
    storage: 'indexed'
});

// Récupération avec migration
const settings = await storage.get('app-settings', {
    version: 2,
    migrate: (data, fromVersion, toVersion) => {
        if (fromVersion === 1 && toVersion === 2) {
            return {
                ...data,
                newFeature: true
            };
        }
        return data;
    }
});
```
{% endtab %}

{% tab title="📊 Cache stratégies" %}
```javascript
// Cache intelligent avec différentes stratégies
class CacheManager {
    constructor(options = {}) {
        this.strategies = {
            lru: new LRUStrategy(options.maxSize || 100),
            lfu: new LFUStrategy(options.maxSize || 100),
            ttl: new TTLStrategy(options.defaultTTL || 300000),
            fifo: new FIFOStrategy(options.maxSize || 100)
        };
        
        this.defaultStrategy = options.strategy || 'lru';
        this.storage = new StorageManager();
    }
    
    async get(key, options = {}) {
        const strategy = this.getStrategy(options.strategy);
        
        // Vérifier le cache en mémoire d'abord
        const memoryValue = strategy.get(key);
        if (memoryValue !== null) {
            return memoryValue;
        }
        
        // Puis le stockage persistant
        const persistentValue = await this.storage.get(key, options);
        if (persistentValue !== null) {
            strategy.set(key, persistentValue, options);
            return persistentValue;
        }
        
        return null;
    }
    
    async set(key, value, options = {}) {
        const strategy = this.getStrategy(options.strategy);
        
        // Stocker en mémoire
        strategy.set(key, value, options);
        
        // Stocker de manière persistante si demandé
        if (options.persist) {
            await this.storage.set(key, value, options);
        }
        
        return true;
    }
    
    async getOrSet(key, factory, options = {}) {
        let value = await this.get(key, options);
        
        if (value === null) {
            value = await factory();
            await this.set(key, value, options);
        }
        
        return value;
    }
    
    getStrategy(name = this.defaultStrategy) {
        return this.strategies[name];
    }
    
    // Cache warming
    async warm(entries) {
        const promises = entries.map(({ key, factory, options }) =>
            this.getOrSet(key, factory, options)
        );
        
        await Promise.allSettled(promises);
    }
    
    // Cache invalidation
    async invalidate(pattern) {
        for (const strategy of Object.values(this.strategies)) {
            strategy.invalidate(pattern);
        }
        
        // Invalider aussi le stockage persistant
        const keys = await this.storage.keys();
        for (const key of keys) {
            if (typeof pattern === 'string' && key.includes(pattern)) {
                await this.storage.remove(key);
            } else if (pattern instanceof RegExp && pattern.test(key)) {
                await this.storage.remove(key);
            }
        }
    }
    
    // Statistiques
    getStats() {
        const stats = {};
        for (const [name, strategy] of Object.entries(this.strategies)) {
            stats[name] = strategy.getStats();
        }
        return stats;
    }
}

// Stratégie LRU (Least Recently Used)
class LRUStrategy {
    constructor(maxSize) {
        this.maxSize = maxSize;
        this.cache = new Map();
        this.stats = { hits: 0, misses: 0, evictions: 0 };
    }
    
    get(key) {
        if (this.cache.has(key)) {
            // Déplacer à la fin (plus récent)
            const value = this.cache.get(key);
            this.cache.delete(key);
            this.cache.set(key, value);
            this.stats.hits++;
            return value;
        }
        
        this.stats.misses++;
        return null;
    }
    
    set(key, value, options = {}) {
        if (this.cache.has(key)) {
            this.cache.delete(key);
        } else if (this.cache.size >= this.maxSize) {
            // Supprimer le plus ancien (premier)
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);
            this.stats.evictions++;
        }
        
        this.cache.set(key, value);
    }
    
    invalidate(pattern) {
        const keysToDelete = [];
        for (const key of this.cache.keys()) {
            if (typeof pattern === 'string' && key.includes(pattern)) {
                keysToDelete.push(key);
            } else if (pattern instanceof RegExp && pattern.test(key)) {
                keysToDelete.push(key);
            }
        }
        
        keysToDelete.forEach(key => this.cache.delete(key));
    }
    
    getStats() {
        return {
            ...this.stats,
            size: this.cache.size,
            hitRate: this.stats.hits / (this.stats.hits + this.stats.misses) || 0
        };
    }
}

// Stratégie TTL (Time To Live)
class TTLStrategy {
    constructor(defaultTTL) {
        this.defaultTTL = defaultTTL;
        this.cache = new Map();
        this.timers = new Map();
        this.stats = { hits: 0, misses: 0, expirations: 0 };
    }
    
    get(key) {
        if (this.cache.has(key)) {
            this.stats.hits++;
            return this.cache.get(key);
        }
        
        this.stats.misses++;
        return null;
    }
    
    set(key, value, options = {}) {
        const ttl = options.ttl || this.defaultTTL;
        
        // Nettoyer l'ancien timer si existant
        if (this.timers.has(key)) {
            clearTimeout(this.timers.get(key));
        }
        
        this.cache.set(key, value);
        
        // Définir expiration
        const timer = setTimeout(() => {
            this.cache.delete(key);
            this.timers.delete(key);
            this.stats.expirations++;
        }, ttl);
        
        this.timers.set(key, timer);
    }
    
    invalidate(pattern) {
        const keysToDelete = [];
        for (const key of this.cache.keys()) {
            if (typeof pattern === 'string' && key.includes(pattern)) {
                keysToDelete.push(key);
            } else if (pattern instanceof RegExp && pattern.test(key)) {
                keysToDelete.push(key);
            }
        }
        
        keysToDelete.forEach(key => {
            this.cache.delete(key);
            if (this.timers.has(key)) {
                clearTimeout(this.timers.get(key));
                this.timers.delete(key);
            }
        });
    }
    
    getStats() {
        return {
            ...this.stats,
            size: this.cache.size,
            hitRate: this.stats.hits / (this.stats.hits + this.stats.misses) || 0
        };
    }
}

// Utilisation du cache manager
const cache = new CacheManager({
    strategy: 'lru',
    maxSize: 200,
    defaultTTL: 300000 // 5 minutes
});

// Cache simple
await cache.set('user:123', userData, { persist: true });
const user = await cache.get('user:123');

// Cache avec factory function
const expensiveData = await cache.getOrSet(
    'expensive-computation',
    async () => {
        // Calcul coûteux
        return await performExpensiveComputation();
    },
    { 
        strategy: 'ttl', 
        ttl: 600000, // 10 minutes
        persist: true 
    }
);

// Cache warming pour les données critiques
await cache.warm([
    {
        key: 'navigation-menu',
        factory: () => fetchNavigationMenu(),
        options: { strategy: 'lru', persist: true }
    },
    {
        key: 'user-permissions',
        factory: () => fetchUserPermissions(),
        options: { strategy: 'ttl', ttl: 900000 }
    }
]);

// Invalidation sélective
await cache.invalidate('user:'); // Invalide tous les caches d'utilisateurs
await cache.invalidate(/^api-/); // Invalide tous les caches API

// Monitoring des performances
console.log('Cache stats:', cache.getStats());
```
{% endtab %}

{% tab title="🔄 Synchronization patterns" %}
```javascript
// Synchronisation de données entre onglets
class TabSynchronizer {
    constructor(channel = 'app-sync') {
        this.channel = channel;
        this.bc = new BroadcastChannel(channel);
        this.listeners = new Map();
        this.setupListeners();
    }
    
    setupListeners() {
        this.bc.addEventListener('message', (event) => {
            const { type, data, sender } = event.data;
            
            // Ignorer ses propres messages
            if (sender === this.getTabId()) return;
            
            const handler = this.listeners.get(type);
            if (handler) {
                handler(data, sender);
            }
        });
        
        // Synchronisation au focus
        window.addEventListener('focus', () => {
            this.requestSync();
        });
        
        // Nettoyage à la fermeture
        window.addEventListener('beforeunload', () => {
            this.bc.close();
        });
    }
    
    getTabId() {
        if (!this.tabId) {
            this.tabId = `tab-${Date.now()}-${Math.random().toString(36).substr(2, 9)}`;
        }
        return this.tabId;
    }
    
    broadcast(type, data) {
        this.bc.postMessage({
            type,
            data,
            sender: this.getTabId(),
            timestamp: Date.now()
        });
    }
    
    on(type, handler) {
        this.listeners.set(type, handler);
    }
    
    off(type) {
        this.listeners.delete(type);
    }
    
    requestSync() {
        this.broadcast('sync-request', { tabId: this.getTabId() });
    }
    
    sendSync(data) {
        this.broadcast('sync-data', data);
    }
}

// Gestionnaire de synchronisation d'état
class StateSynchronizer {
    constructor(initialState = {}) {
        this.state = { ...initialState };
        this.subscribers = new Set();
        this.tabSync = new TabSynchronizer('state-sync');
        this.storage = new StorageManager();
        
        this.setupSynchronization();
    }
    
    setupSynchronization() {
        // Écouter les changements d'autres onglets
        this.tabSync.on('state-change', (change) => {
            this.applyChange(change, false); // false = ne pas propager
        });
        
        // Répondre aux demandes de synchronisation
        this.tabSync.on('sync-request', async () => {
            const currentState = await this.getPersistedState();
            this.tabSync.sendSync(currentState);
        });
        
        // Recevoir l'état synchronisé
        this.tabSync.on('sync-data', (state) => {
            this.state = { ...state };
            this.notifySubscribers();
        });
        
        // Charger l'état persisté au démarrage
        this.loadPersistedState();
    }
    
    async loadPersistedState() {
        const persistedState = await this.storage.get('app-state');
        if (persistedState) {
            this.state = { ...this.state, ...persistedState };
            this.notifySubscribers();
        }
    }
    
    async getPersistedState() {
        return await this.storage.get('app-state') || {};
    }
    
    async persistState() {
        await this.storage.set('app-state', this.state, { persist: true });
    }
    
    setState(newState, persist = true) {
        const oldState = { ...this.state };
        this.state = { ...this.state, ...newState };
        
        const change = {
            type: 'update',
            payload: newState,
            timestamp: Date.now()
        };
        
        this.applyChange(change, true, persist);
    }
    
    applyChange(change, propagate = true, persist = true) {
        if (change.type === 'update') {
            this.state = { ...this.state, ...change.payload };
        }
        
        // Notifier les subscribers locaux
        this.notifySubscribers();
        
        // Propager aux autres onglets
        if (propagate) {
            this.tabSync.broadcast('state-change', change);
        }
        
        // Persister si demandé
        if (persist) {
            this.persistState();
        }
    }
    
    getState() {
        return { ...this.state };
    }
    
    subscribe(callback) {
        this.subscribers.add(callback);
        
        // Retourner fonction de désabonnement
        return () => {
            this.subscribers.delete(callback);
        };
    }
    
    notifySubscribers() {
        this.subscribers.forEach(callback => {
            try {
                callback(this.getState());
            } catch (error) {
                console.error('Error in state subscriber:', error);
            }
        });
    }
}

// Gestionnaire de conflits de synchronisation
class ConflictResolver {
    static strategies = {
        // Le dernier change gagne
        lastWins: (local, remote) => remote,
        
        // Le premier change gagne
        firstWins: (local, remote) => local,
        
        // Fusion profonde des objets
        deepMerge: (local, remote) => {
            return ConflictResolver.deepMerge(local, remote);
        },
        
        // Résolution basée sur timestamp
        timestampBased: (local, remote) => {
            const localTime = local.timestamp || 0;
            const remoteTime = remote.timestamp || 0;
            return remoteTime > localTime ? remote : local;
        },
        
        // Résolution personnalisée
        custom: (local, remote, resolver) => {
            return resolver(local, remote);
        }
    };
    
    static deepMerge(obj1, obj2) {
        const result = { ...obj1 };
        
        for (const key in obj2) {
            if (obj2.hasOwnProperty(key)) {
                if (typeof obj2[key] === 'object' && 
                    obj2[key] !== null && 
                    !Array.isArray(obj2[key]) &&
                    typeof obj1[key] === 'object' && 
                    obj1[key] !== null && 
                    !Array.isArray(obj1[key])) {
                    result[key] = ConflictResolver.deepMerge(obj1[key], obj2[key]);
                } else {
                    result[key] = obj2[key];
                }
            }
        }
        
        return result;
    }
    
    static resolve(local, remote, strategy = 'lastWins', customResolver = null) {
        const resolver = ConflictResolver.strategies[strategy];
        if (!resolver) {
            throw new Error(`Unknown conflict resolution strategy: ${strategy}`);
        }
        
        return strategy === 'custom' 
            ? resolver(local, remote, customResolver)
            : resolver(local, remote);
    }
}

// Synchroniseur offline-first
class OfflineFirstSynchronizer extends StateSynchronizer {
    constructor(initialState = {}) {
        super(initialState);
        this.pendingChanges = [];
        this.isOnline = navigator.onLine;
        this.setupOfflineHandling();
    }
    
    setupOfflineHandling() {
        window.addEventListener('online', () => {
            this.isOnline = true;
            this.syncPendingChanges();
        });
        
        window.addEventListener('offline', () => {
            this.isOnline = false;
        });
    }
    
    async setState(newState, persist = true, sync = true) {
        super.setState(newState, persist);
        
        if (sync && !this.isOnline) {
            // Ajouter aux changements en attente
            this.pendingChanges.push({
                type: 'update',
                payload: newState,
                timestamp: Date.now()
            });
            
            await this.storage.set('pending-changes', this.pendingChanges);
        }
    }
    
    async syncPendingChanges() {
        if (this.pendingChanges.length === 0) {
            // Charger depuis le stockage
            this.pendingChanges = await this.storage.get('pending-changes') || [];
        }
        
        if (this.pendingChanges.length === 0) return;
        
        try {
            // Synchroniser avec le serveur
            for (const change of this.pendingChanges) {
                await this.syncChangeWithServer(change);
            }
            
            // Nettoyer les changements en attente
            this.pendingChanges = [];
            await this.storage.remove('pending-changes');
            
        } catch (error) {
            console.error('Failed to sync pending changes:', error);
        }
    }
    
    async syncChangeWithServer(change) {
        // Simulation d'une synchronisation serveur
        const response = await fetch('/api/sync', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(change)
        });
        
        if (!response.ok) {
            throw new Error(`Sync failed: ${response.status}`);
        }
        
        const serverResponse = await response.json();
        
        // Résoudre les conflits si nécessaire
        if (serverResponse.conflict) {
            const resolved = ConflictResolver.resolve(
                change.payload,
                serverResponse.serverState,
                'timestampBased'
            );
            
            this.setState(resolved, true, false);
        }
    }
}

// Utilisation
const synchronizer = new OfflineFirstSynchronizer({
    user: null,
    preferences: { theme: 'light' },
    data: {}
});

// S'abonner aux changements d'état
const unsubscribe = synchronizer.subscribe((state) => {
    console.log('State updated:', state);
    updateUI(state);
});

// Modifier l'état (synchronisé automatiquement)
synchronizer.setState({
    user: { id: 1, name: 'Alice' },
    preferences: { theme: 'dark', language: 'fr' }
});

// L'état sera automatiquement synchronisé entre les onglets
// et persisté même en mode offline
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Dashboard temps réel

{% hint style="warning" %}
**Exercice :** Créez un dashboard avec APIs multiples et synchronisation avancée
{% endhint %}

```javascript
// Créez un dashboard intégrant plusieurs APIs du navigateur
class RealTimeDashboard {
    constructor() {
        this.apis = {
            // TODO: Intégrer les APIs suivantes
            location: null,    // Geolocation
            storage: null,     // Storage unifié  
            network: null,     // Fetch avancé
            sync: null,        // Synchronisation multi-onglets
            notifications: null // Notifications système
        };
        
        this.widgets = new Map();
        this.eventBus = new EventTarget();
    }
    
    // TODO: Initialiser le dashboard
    async init() {
        // 1. Configurer les APIs
        // 2. Charger la configuration sauvegardée
        // 3. Initialiser les widgets
        // 4. Démarrer la synchronisation temps réel
    }
    
    // TODO: Créer des widgets interactifs
    createWidget(type, config) {
        // Types: 'weather', 'location', 'metrics', 'notifications', 'storage-monitor'
    }
    
    // TODO: Gérer les notifications intelligentes
    async setupSmartNotifications() {
        // Permission, filtrage, groupement, persistance
    }
    
    // TODO: Monitoring des performances
    setupPerformanceMonitoring() {
        // Navigation timing, resource timing, user timing
    }
    
    // TODO: Gestion offline/online
    setupOfflineHandling() {
        // Cache stratégies, synchronisation différée, UI offline
    }
}

// Tests et démonstration
async function demonstrationAPIsNavigateur() {
    const dashboard = new RealTimeDashboard();
    
    // TODO: Tester toutes les fonctionnalités
    await dashboard.init();
    
    // TODO: Créer des widgets interactifs
    dashboard.createWidget('weather', {
        updateInterval: 300000, // 5 minutes
        location: 'auto',
        units: 'metric'
    });
    
    dashboard.createWidget('storage-monitor', {
        showQuota: true,
        autoCleanup: true,
        threshold: 0.8
    });
    
    // TODO: Tester la synchronisation multi-onglets
    // TODO: Tester le mode offline
    // TODO: Tester les notifications
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
class RealTimeDashboard {
    async init() {
        // Geolocation
        this.apis.location = new GeolocationAPI();
        
        // Storage unifié
        this.apis.storage = new StorageManager();
        
        // Network client
        this.apis.network = new MonitoredAPIClient('https://api.dashboard.com');
        
        // Synchronisation
        this.apis.sync = new StateSynchronizer();
        
        // Notifications
        this.apis.notifications = new SmartNotifications();
        
        // Charger la configuration
        const config = await this.apis.storage.get('dashboard-config') || {};
        this.applyConfig(config);
    }
    
    createWidget(type, config) {
        const widget = WidgetFactory.create(type, config, this.apis);
        this.widgets.set(widget.id, widget);
        return widget;
    }
}
```
{% endtab %}

{% tab title="✅ Solution partielle" %}
Voir la solution complète dans l'onglet suivant avec toutes les implémentations détaillées des APIs et widgets.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant les APIs avancées du navigateur !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Fetch API** avancée avec retry, cache, streaming
- **Web Storage** intelligent avec compression et TTL
- **Synchronisation** multi-onglets et offline-first
- **APIs système** (Geolocation, Notifications, Performance)
- **Gestion d'erreurs** robuste et resilience
- **Optimisations** de performance et monitoring
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Créer des clients HTTP sophistiqués
- Implémenter des stratégies de cache avancées
- Gérer la persistence de données complexes
- Synchroniser l'état entre onglets/devices
- Optimiser les performances réseau
- Monitorer et déboguer les APIs
{% endtab %}

{% tab title="🚀 Applications réelles" %}
- **Dashboards** temps réel avec données live
- **Applications offline-first** avec synchronisation
- **Systèmes de cache** intelligents et performants
- **Notifications** contextuelle et personnalisées
- **APIs robustes** avec gestion d'erreurs complète
{% endtab %}
{% endtabs %}

---

**Prêt pour les outils modernes de build ? Continuez avec Vite et l'écosystème moderne !** ⚡
