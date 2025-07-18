# Introduction à TypeScript

## **INFORMATIONS GENERALES**

### Métadonnées du cours
- **Titre :** Introduction à TypeScript - JavaScript avec typage statique
- **Niveau :** Avancé
- **Durée estimée :** 90 minutes
- **Prérequis :** JavaScript ES6+, modules, classes, interfaces conceptuelles
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Les avantages du typage statique et l'écosystème TypeScript
- [ ] **Appliquer** : Les types de base, interfaces et classes TypeScript
- [ ] **Analyser** : Les erreurs de type et les patterns TypeScript courants
- [ ] **Créer** : Des projets TypeScript avec configuration et tooling moderne

### Mots-clés à retenir
- **TypeScript** : Superset de JavaScript avec typage statique
- **Type Annotation** : Spécification explicite du type d'une variable
- **Interface** : Définition de la structure d'un objet
- **Generic** : Type paramétré pour la réutilisabilité
- **Union Type** : Type pouvant être plusieurs types différents
- **Type Guard** : Mécanisme de vérification de type à l'exécution

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
TypeScript résout les principaux problèmes de JavaScript à grande échelle : absence de typage, erreurs difficiles à détecter, refactoring risqué, et documentation implicite. Il est devenu standard dans l'industrie (React, Angular, Vue 3, Node.js) et est essentiel pour comprendre et contribuer aux projets modernes.

### Définition et concepts clés

#### **1. TypeScript vs JavaScript**
```typescript
// JavaScript - Types implicites, erreurs à l'exécution
function calculateTotal(price, tax) {
    return price + tax; // Que se passe-t-il si price est une string ?
}

calculateTotal("100", 0.2); // "1000.2" - Bug silencieux !

// TypeScript - Types explicites, erreurs à la compilation
function calculateTotal(price: number, tax: number): number {
    return price + tax;
}

calculateTotal("100", 0.2); // ❌ Erreur de compilation détectée !
```

#### **2. Types primitifs et composés**
```typescript
// Types primitifs
let name: string = "Alice";
let age: number = 30;
let isActive: boolean = true;
let value: null = null;
let data: undefined = undefined;

// Types composés
let numbers: number[] = [1, 2, 3];
let colors: Array<string> = ["red", "green", "blue"];
let coordinates: [number, number] = [10, 20]; // Tuple

// Union types
let id: string | number = "user123";
id = 456; // Valide

// Type literal
let status: "pending" | "approved" | "rejected" = "pending";
```

#### **3. Interfaces et types personnalisés**
```typescript
// Interface pour définir la structure d'un objet
interface User {
    id: number;
    name: string;
    email: string;
    age?: number; // Propriété optionnelle
    readonly createdAt: Date; // Propriété en lecture seule
}

// Type alias
type UserRole = "admin" | "user" | "moderator";

// Interface avec méthodes
interface Calculator {
    add(a: number, b: number): number;
    subtract(a: number, b: number): number;
}

// Extension d'interface
interface AdminUser extends User {
    role: UserRole;
    permissions: string[];
}
```

#### **4. Génériques (Generics)**
```typescript
// Fonction générique
function identity<T>(arg: T): T {
    return arg;
}

let result1 = identity<string>("hello"); // result1 est de type string
let result2 = identity<number>(42); // result2 est de type number

// Interface générique
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

type UserResponse = ApiResponse<User>;
type ProductResponse = ApiResponse<Product[]>;
```

### Syntaxe de base
```typescript
// Variables avec types
const message: string = "Hello TypeScript";
const count: number = 42;
const isValid: boolean = true;

// Fonctions avec types
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Arrow function avec types
const multiply = (a: number, b: number): number => a * b;

// Classe avec types
class Person {
    private _name: string;
    public age: number;
    
    constructor(name: string, age: number) {
        this._name = name;
        this.age = age;
    }
    
    get name(): string {
        return this._name;
    }
}
```

### Points d'attention
- **Piège courant 1** : Over-typing - typer chaque variable alors que l'inférence suffit
- **Piège courant 2** : Utiliser `any` partout, perdant les bénéfices du typage
- **Bonne pratique** : Laisser TypeScript inférer quand c'est évident, typer explicitement quand nécessaire

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```typescript
// Système de gestion d'utilisateurs avec TypeScript
console.log("=== SYSTÈME UTILISATEURS TYPESCRIPT ===");

// Types et interfaces de base
type UserRole = "admin" | "user" | "moderator";

interface User {
    id: number;
    name: string;
    email: string;
    role: UserRole;
    isActive: boolean;
    createdAt: Date;
    lastLogin?: Date; // Optionnel
}

// Classe UserManager avec typage complet
class UserManager {
    private users: User[] = [];
    private nextId: number = 1;
    
    // Méthode pour créer un utilisateur
    createUser(userData: Omit<User, 'id' | 'createdAt'>): User {
        const newUser: User = {
            id: this.nextId++,
            createdAt: new Date(),
            ...userData
        };
        
        this.users.push(newUser);
        console.log(`✅ Utilisateur créé: ${newUser.name} (${newUser.role})`);
        return newUser;
    }
    
    // Méthode pour trouver un utilisateur
    findUser(criteria: { id?: number; email?: string; role?: UserRole }): User | undefined {
        return this.users.find(user => {
            if (criteria.id && user.id !== criteria.id) return false;
            if (criteria.email && user.email !== criteria.email) return false;
            if (criteria.role && user.role !== criteria.role) return false;
            return true;
        });
    }
    
    // Méthode pour filtrer les utilisateurs
    getUsers(filter?: { role?: UserRole; isActive?: boolean }): User[] {
        if (!filter) return this.users;
        
        return this.users.filter(user => {
            if (filter.role && user.role !== filter.role) return false;
            if (filter.isActive !== undefined && user.isActive !== filter.isActive) return false;
            return true;
        });
    }
    
    // Méthode pour mettre à jour un utilisateur
    updateUser(id: number, updates: Partial<Omit<User, 'id' | 'createdAt'>>): User | null {
        const userIndex = this.users.findIndex(user => user.id === id);
        
        if (userIndex === -1) {
            console.log(`❌ Utilisateur avec l'ID ${id} non trouvé`);
            return null;
        }
        
        this.users[userIndex] = { ...this.users[userIndex], ...updates };
        console.log(`🔄 Utilisateur ${id} mis à jour`);
        return this.users[userIndex];
    }
    
    // Méthode pour obtenir des statistiques
    getStats(): { total: number; byRole: Record<UserRole, number>; active: number } {
        const stats = {
            total: this.users.length,
            byRole: { admin: 0, user: 0, moderator: 0 } as Record<UserRole, number>,
            active: 0
        };
        
        this.users.forEach(user => {
            stats.byRole[user.role]++;
            if (user.isActive) stats.active++;
        });
        
        return stats;
    }
}

// Utilisation du système
const userManager = new UserManager();

// Création d'utilisateurs avec typage strict
const alice = userManager.createUser({
    name: "Alice Dupont",
    email: "alice@example.com",
    role: "admin",
    isActive: true
});

const bob = userManager.createUser({
    name: "Bob Martin",
    email: "bob@example.com",
    role: "user",
    isActive: true,
    lastLogin: new Date("2025-01-15")
});

const charlie = userManager.createUser({
    name: "Charlie Brown",
    email: "charlie@example.com",
    role: "moderator",
    isActive: false
});

console.log("\n--- Recherche d'utilisateurs ---");

// Recherche par email
const foundUser = userManager.findUser({ email: "alice@example.com" });
if (foundUser) {
    console.log(`👤 Trouvé: ${foundUser.name} (${foundUser.role})`);
}

// Filtrage par rôle
const admins = userManager.getUsers({ role: "admin" });
console.log(`👑 Administrateurs: ${admins.length}`);

// Mise à jour d'utilisateur
userManager.updateUser(bob.id, { 
    lastLogin: new Date(),
    isActive: true 
});

// Statistiques
console.log("\n--- Statistiques ---");
const stats = userManager.getStats();
console.log(`📊 Total: ${stats.total} utilisateurs`);
console.log(`📊 Actifs: ${stats.active} utilisateurs`);
console.log(`📊 Par rôle:`, stats.byRole);

// Démonstration des avantages du typage
console.log("\n--- Avantages du typage TypeScript ---");

// ✅ TypeScript détecterait ces erreurs à la compilation:
/*
userManager.createUser({
    name: "Invalid User",
    email: "invalid@email.com",
    role: "invalid_role", // ❌ Erreur: role doit être "admin" | "user" | "moderator"
    isActive: "yes" // ❌ Erreur: isActive doit être boolean
});

const user = userManager.findUser({ email: "test@test.com" });
console.log(user.name); // ❌ Erreur: user peut être undefined
*/

// ✅ Version correcte avec type guards
const user = userManager.findUser({ email: "alice@example.com" });
if (user) { // Type guard
    console.log(`✅ Utilisateur trouvé: ${user.name}`);
    console.log(`📧 Email: ${user.email}`);
    console.log(`🔐 Rôle: ${user.role}`);
}
```

**Résultat attendu :**
```
=== SYSTÈME UTILISATEURS TYPESCRIPT ===
✅ Utilisateur créé: Alice Dupont (admin)
✅ Utilisateur créé: Bob Martin (user)
✅ Utilisateur créé: Charlie Brown (moderator)

--- Recherche d'utilisateurs ---
👤 Trouvé: Alice Dupont (admin)
👑 Administrateurs: 1
🔄 Utilisateur 2 mis à jour

--- Statistiques ---
📊 Total: 3 utilisateurs
📊 Actifs: 2 utilisateurs
📊 Par rôle: { admin: 1, user: 1, moderator: 1 }

--- Avantages du typage TypeScript ---
✅ Utilisateur trouvé: Alice Dupont
📧 Email: alice@example.com
🔐 Rôle: admin
```

### Exemple intermédiaire
```typescript
// API Client générique avec TypeScript avancé
console.log("=== API CLIENT TYPESCRIPT AVANCÉ ===");

// Types pour les réponses API
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
    timestamp: string;
}

interface ApiError {
    error: string;
    code: number;
    details?: Record<string, any>;
}

// Union type pour les résultats
type ApiResult<T> = ApiResponse<T> | ApiError;

// Type guard pour vérifier les erreurs
function isApiError(result: ApiResult<any>): result is ApiError {
    return 'error' in result;
}

// Generic constraint pour les entités avec ID
interface HasId {
    id: number | string;
}

// Types pour différentes entités
interface Product extends HasId {
    id: number;
    name: string;
    price: number;
    category: string;
    inStock: boolean;
}

interface Order extends HasId {
    id: string;
    userId: number;
    products: { productId: number; quantity: number }[];
    total: number;
    status: "pending" | "processing" | "shipped" | "delivered";
    createdAt: string;
}

// Configuration du client API
interface ApiClientConfig {
    baseUrl: string;
    timeout: number;
    retries: number;
    headers?: Record<string, string>;
}

// Client API générique avec types avancés
class TypedApiClient {
    private config: ApiClientConfig;
    private cache: Map<string, { data: any; expiry: number }> = new Map();
    
    constructor(config: ApiClientConfig) {
        this.config = config;
        console.log(`🔧 API Client configuré: ${config.baseUrl}`);
    }
    
    // Méthode générique pour GET avec cache
    async get<T>(endpoint: string, useCache: boolean = true): Promise<ApiResult<T>> {
        const cacheKey = `GET:${endpoint}`;
        
        // Vérifier le cache
        if (useCache && this.cache.has(cacheKey)) {
            const cached = this.cache.get(cacheKey)!;
            if (Date.now() < cached.expiry) {
                console.log(`📦 Cache hit pour ${endpoint}`);
                return cached.data;
            }
            this.cache.delete(cacheKey);
        }
        
        try {
            console.log(`🌐 GET ${endpoint}`);
            
            // Simulation d'appel API
            const result = await this.simulateApiCall<T>(endpoint, 'GET');
            
            // Mettre en cache si succès
            if (!isApiError(result) && useCache) {
                this.cache.set(cacheKey, {
                    data: result,
                    expiry: Date.now() + 60000 // 1 minute
                });
            }
            
            return result;
        } catch (error) {
            return this.handleError(error);
        }
    }
    
    // Méthode générique pour POST
    async post<TRequest, TResponse>(
        endpoint: string, 
        data: TRequest
    ): Promise<ApiResult<TResponse>> {
        try {
            console.log(`🌐 POST ${endpoint}`, data);
            return await this.simulateApiCall<TResponse>(endpoint, 'POST', data);
        } catch (error) {
            return this.handleError(error);
        }
    }
    
    // Méthode générique pour PUT
    async put<TRequest, TResponse>(
        endpoint: string, 
        data: TRequest
    ): Promise<ApiResult<TResponse>> {
        try {
            console.log(`🌐 PUT ${endpoint}`, data);
            return await this.simulateApiCall<TResponse>(endpoint, 'PUT', data);
        } catch (error) {
            return this.handleError(error);
        }
    }
    
    // Méthode générique pour DELETE
    async delete<T extends HasId>(endpoint: string): Promise<ApiResult<{ deleted: boolean }>> {
        try {
            console.log(`🌐 DELETE ${endpoint}`);
            return await this.simulateApiCall<{ deleted: boolean }>(endpoint, 'DELETE');
        } catch (error) {
            return this.handleError(error);
        }
    }
    
    // Simulation d'appel API (remplace fetch en réalité)
    private async simulateApiCall<T>(
        endpoint: string, 
        method: string, 
        data?: any
    ): Promise<ApiResult<T>> {
        // Simuler un délai réseau
        await new Promise(resolve => setTimeout(resolve, 100 + Math.random() * 200));
        
        // Simuler des erreurs occasionnelles
        if (Math.random() < 0.1) { // 10% d'erreurs
            throw new Error(`Erreur réseau pour ${method} ${endpoint}`);
        }
        
        // Simuler des réponses selon l'endpoint
        if (endpoint.includes('/products')) {
            const mockProducts: Product[] = [
                { id: 1, name: "Laptop", price: 999, category: "Electronics", inStock: true },
                { id: 2, name: "Mouse", price: 25, category: "Electronics", inStock: true },
                { id: 3, name: "Book", price: 15, category: "Education", inStock: false }
            ];
            
            if (method === 'GET') {
                return {
                    data: endpoint.includes('/products/') 
                        ? mockProducts[0] as T // Produit unique
                        : mockProducts as T, // Liste de produits
                    status: 200,
                    message: "Success",
                    timestamp: new Date().toISOString()
                };
            }
        }
        
        if (endpoint.includes('/orders')) {
            const mockOrder: Order = {
                id: "ORD-" + Date.now(),
                userId: 1,
                products: [{ productId: 1, quantity: 2 }],
                total: 1998,
                status: "pending",
                createdAt: new Date().toISOString()
            };
            
            return {
                data: mockOrder as T,
                status: method === 'POST' ? 201 : 200,
                message: method === 'POST' ? "Created" : "Success",
                timestamp: new Date().toISOString()
            };
        }
        
        // Réponse par défaut
        return {
            data: { success: true } as T,
            status: 200,
            message: "Success",
            timestamp: new Date().toISOString()
        };
    }
    
    private handleError(error: any): ApiError {
        console.error(`❌ Erreur API:`, error.message);
        return {
            error: error.message || "Erreur inconnue",
            code: 500,
            details: { originalError: error }
        };
    }
    
    // Méthodes typées spécifiques aux entités
    async getProducts(): Promise<ApiResult<Product[]>> {
        return this.get<Product[]>('/products');
    }
    
    async getProduct(id: number): Promise<ApiResult<Product>> {
        return this.get<Product>(`/products/${id}`);
    }
    
    async createOrder(orderData: Omit<Order, 'id' | 'createdAt'>): Promise<ApiResult<Order>> {
        return this.post<Omit<Order, 'id' | 'createdAt'>, Order>('/orders', orderData);
    }
    
    async updateOrderStatus(
        orderId: string, 
        status: Order['status']
    ): Promise<ApiResult<Order>> {
        return this.put<{ status: Order['status'] }, Order>(
            `/orders/${orderId}`, 
            { status }
        );
    }
}

// Service layer avec gestion d'erreurs typée
class EcommerceService {
    constructor(private apiClient: TypedApiClient) {}
    
    async getAvailableProducts(): Promise<Product[]> {
        const result = await this.apiClient.getProducts();
        
        if (isApiError(result)) {
            console.error(`❌ Erreur lors du chargement des produits: ${result.error}`);
            return [];
        }
        
        // Filtrer les produits en stock
        return result.data.filter(product => product.inStock);
    }
    
    async placeOrder(
        userId: number, 
        items: { productId: number; quantity: number }[]
    ): Promise<Order | null> {
        // Calculer le total (simplifié)
        const total = items.reduce((sum, item) => sum + (item.quantity * 100), 0); // Prix fictif
        
        const orderData: Omit<Order, 'id' | 'createdAt'> = {
            userId,
            products: items,
            total,
            status: "pending"
        };
        
        const result = await this.apiClient.createOrder(orderData);
        
        if (isApiError(result)) {
            console.error(`❌ Erreur lors de la création de commande: ${result.error}`);
            return null;
        }
        
        console.log(`✅ Commande créée: ${result.data.id}`);
        return result.data;
    }
    
    async processOrder(orderId: string): Promise<boolean> {
        const result = await this.apiClient.updateOrderStatus(orderId, "processing");
        
        if (isApiError(result)) {
            console.error(`❌ Erreur lors du traitement de commande: ${result.error}`);
            return false;
        }
        
        console.log(`🔄 Commande ${orderId} en cours de traitement`);
        return true;
    }
}

// Test du système complet
async function testerEcommerceSystem() {
    console.log("🚀 Test du système e-commerce TypeScript\n");
    
    // Configuration du client API
    const apiConfig: ApiClientConfig = {
        baseUrl: "https://api.example.com",
        timeout: 5000,
        retries: 3,
        headers: {
            "Authorization": "Bearer token123",
            "Content-Type": "application/json"
        }
    };
    
    const apiClient = new TypedApiClient(apiConfig);
    const ecommerceService = new EcommerceService(apiClient);
    
    // Test 1: Charger les produits disponibles
    console.log("--- Test 1: Produits disponibles ---");
    const products = await ecommerceService.getAvailableProducts();
    console.log(`📦 ${products.length} produits disponibles`);
    products.forEach(p => {
        console.log(`  - ${p.name}: ${p.price}€ (${p.category})`);
    });
    
    // Test 2: Créer une commande
    console.log("\n--- Test 2: Création de commande ---");
    const order = await ecommerceService.placeOrder(123, [
        { productId: 1, quantity: 2 },
        { productId: 2, quantity: 1 }
    ]);
    
    if (order) {
        console.log(`💰 Total de la commande: ${order.total}€`);
        
        // Test 3: Traiter la commande
        console.log("\n--- Test 3: Traitement de commande ---");
        await ecommerceService.processOrder(order.id);
    }
    
    // Test 4: Test du cache
    console.log("\n--- Test 4: Cache API ---");
    console.log("Premier appel (pas de cache):");
    await apiClient.getProducts();
    console.log("Deuxième appel (avec cache):");
    await apiClient.getProducts();
}

testerEcommerceSystem();

// Démonstration des avantages TypeScript
console.log("\n=== AVANTAGES TYPESCRIPT DÉMONTRÉS ===");
console.log("✅ Typage strict des paramètres et retours");
console.log("✅ Auto-complétion intelligente dans l'IDE");
console.log("✅ Détection d'erreurs à la compilation");
console.log("✅ Refactoring sûr et automatisé");
console.log("✅ Documentation vivante via les types");
console.log("✅ Génériques pour la réutilisabilité");
console.log("✅ Type guards pour la sécurité runtime");
```

### Exemple avancé (optionnel)
```typescript
// Système de validation avec décorateurs et types avancés
console.log("=== SYSTÈME DE VALIDATION TYPESCRIPT AVANCÉ ===");

// Types utilitaires avancés
type RequiredFields<T, K extends keyof T> = T & Required<Pick<T, K>>;
type OptionalFields<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>;

// Conditional types
type NonNullable<T> = T extends null | undefined ? never : T;
type ApiEndpoint<T> = T extends string ? `/api/${T}` : never;

// Mapped types pour la validation
type ValidationRules<T> = {
    [K in keyof T]?: {
        required?: boolean;
        minLength?: number;
        maxLength?: number;
        pattern?: RegExp;
        custom?: (value: T[K]) => boolean | string;
    };
};

// Décorateur de validation (simulation)
function validate<T>(rules: ValidationRules<T>) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        const originalMethod = descriptor.value;
        
        descriptor.value = function (...args: any[]) {
            const data = args[0];
            const errors = validateData(data, rules);
            
            if (errors.length > 0) {
                throw new ValidationError(`Validation failed: ${errors.join(', ')}`);
            }
            
            return originalMethod.apply(this, args);
        };
    };
}

function validateData<T>(data: T, rules: ValidationRules<T>): string[] {
    const errors: string[] = [];
    
    for (const [field, rule] of Object.entries(rules) as [keyof T, any][]) {
        const value = data[field];
        
        if (rule.required && (value === null || value === undefined || value === '')) {
            errors.push(`${String(field)} is required`);
            continue;
        }
        
        if (value && typeof value === 'string') {
            if (rule.minLength && value.length < rule.minLength) {
                errors.push(`${String(field)} must be at least ${rule.minLength} characters`);
            }
            
            if (rule.maxLength && value.length > rule.maxLength) {
                errors.push(`${String(field)} must be at most ${rule.maxLength} characters`);
            }
            
            if (rule.pattern && !rule.pattern.test(value)) {
                errors.push(`${String(field)} has invalid format`);
            }
        }
        
        if (rule.custom && value !== undefined) {
            const customResult = rule.custom(value);
            if (typeof customResult === 'string') {
                errors.push(customResult);
            } else if (!customResult) {
                errors.push(`${String(field)} failed custom validation`);
            }
        }
    }
    
    return errors;
}

// Types pour un système de blog
interface BlogPost {
    id: string;
    title: string;
    content: string;
    authorId: string;
    tags: string[];
    publishedAt?: Date;
    updatedAt: Date;
    status: 'draft' | 'published' | 'archived';
    metadata: Record<string, any>;
}

interface Author {
    id: string;
    name: string;
    email: string;
    bio?: string;
    avatar?: string;
    socialLinks: {
        twitter?: string;
        linkedin?: string;
        website?: string;
    };
}

// Types pour les DTOs (Data Transfer Objects)
type CreatePostDTO = OptionalFields<
    Omit<BlogPost, 'id' | 'updatedAt'>, 
    'publishedAt' | 'metadata'
>;

type UpdatePostDTO = Partial<Omit<BlogPost, 'id' | 'authorId' | 'updatedAt'>>;

type PostSummary = Pick<BlogPost, 'id' | 'title' | 'authorId' | 'publishedAt' | 'status'>;

// Service de blog avec validation et types avancés
class BlogService {
    private posts: Map<string, BlogPost> = new Map();
    private authors: Map<string, Author> = new Map();
    
    // Validation rules
    private postValidationRules: ValidationRules<CreatePostDTO> = {
        title: {
            required: true,
            minLength: 5,
            maxLength: 100
        },
        content: {
            required: true,
            minLength: 50,
            custom: (content: string) => {
                if (content.includes('<script>')) {
                    return 'Content cannot contain script tags';
                }
                return true;
            }
        },
        tags: {
            custom: (tags: string[]) => {
                if (tags.length > 10) {
                    return 'Maximum 10 tags allowed';
                }
                return true;
            }
        }
    };
    
    private authorValidationRules: ValidationRules<Author> = {
        name: {
            required: true,
            minLength: 2,
            maxLength: 50
        },
        email: {
            required: true,
            pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
        }
    };
    
    // Décorateur appliqué à la méthode
    @validate<CreatePostDTO>(this.postValidationRules)
    async createPost(postData: CreatePostDTO): Promise<BlogPost> {
        const post: BlogPost = {
            id: this.generateId(),
            updatedAt: new Date(),
            metadata: {},
            ...postData
        };
        
        this.posts.set(post.id, post);
        console.log(`📝 Article créé: "${post.title}" par ${post.authorId}`);
        return post;
    }
    
    @validate<Author>(this.authorValidationRules)
    async createAuthor(authorData: Author): Promise<Author> {
        this.authors.set(authorData.id, authorData);
        console.log(`👤 Auteur créé: ${authorData.name}`);
        return authorData;
    }
    
    // Méthode avec types conditionnels
    async getPost<T extends boolean>(
        id: string, 
        includeContent: T
    ): Promise<T extends true ? BlogPost : PostSummary | null> {
        const post = this.posts.get(id);
        
        if (!post) {
            return null as any;
        }
        
        if (includeContent) {
            return post as any;
        }
        
        // Retourner seulement le résumé
        const summary: PostSummary = {
            id: post.id,
            title: post.title,
            authorId: post.authorId,
            publishedAt: post.publishedAt,
            status: post.status
        };
        
        return summary as any;
    }
    
    // Méthode avec génériques contraints
    async findPosts<T extends keyof BlogPost>(
        filters: Partial<Pick<BlogPost, T>>,
        sortBy?: T
    ): Promise<BlogPost[]> {
        let results = Array.from(this.posts.values());
        
        // Filtrage
        for (const [key, value] of Object.entries(filters) as [T, any][]) {
            results = results.filter(post => {
                if (Array.isArray(post[key]) && Array.isArray(value)) {
                    return value.some(v => (post[key] as any[]).includes(v));
                }
                return post[key] === value;
            });
        }
        
        // Tri
        if (sortBy) {
            results.sort((a, b) => {
                const aVal = a[sortBy];
                const bVal = b[sortBy];
                
                if (typeof aVal === 'string' && typeof bVal === 'string') {
                    return aVal.localeCompare(bVal);
                }
                
                if (aVal instanceof Date && bVal instanceof Date) {
                    return bVal.getTime() - aVal.getTime();
                }
                
                return 0;
            });
        }
        
        return results;
    }
    
    // Utility method
    private generateId(): string {
        return `post_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
    }
    
    // Méthode avec type guards avancés
    async publishPost(id: string): Promise<boolean> {
        const post = this.posts.get(id);
        
        if (!post) {
            console.log(`❌ Article ${id} non trouvé`);
            return false;
        }
        
        if (this.isPublishable(post)) {
            post.status = 'published';
            post.publishedAt = new Date();
            post.updatedAt = new Date();
            
            console.log(`🚀 Article "${post.title}" publié`);
            return true;
        }
        
        console.log(`❌ Article "${post.title}" ne peut pas être publié`);
        return false;
    }
    
    // Type guard personnalisé
    private isPublishable(post: BlogPost): post is BlogPost & { status: 'draft' } {
        return post.status === 'draft' && 
               post.title.length > 0 && 
               post.content.length > 50;
    }
    
    // Méthode avec retour conditionnel complexe
    async getAuthorPosts<T extends 'summary' | 'full'>(
        authorId: string,
        format: T
    ): Promise<T extends 'full' ? BlogPost[] : PostSummary[]> {
        const posts = Array.from(this.posts.values())
            .filter(post => post.authorId === authorId);
        
        if (format === 'full') {
            return posts as any;
        }
        
        const summaries: PostSummary[] = posts.map(post => ({
            id: post.id,
            title: post.title,
            authorId: post.authorId,
            publishedAt: post.publishedAt,
            status: post.status
        }));
        
        return summaries as any;
    }
    
    // Statistiques avec types calculés
    getStats(): {
        totalPosts: number;
        postsByStatus: Record<BlogPost['status'], number>;
        totalAuthors: number;
        averagePostsPerAuthor: number;
    } {
        const postsByStatus = {
            draft: 0,
            published: 0,
            archived: 0
        } as Record<BlogPost['status'], number>;
        
        Array.from(this.posts.values()).forEach(post => {
            postsByStatus[post.status]++;
        });
        
        return {
            totalPosts: this.posts.size,
            postsByStatus,
            totalAuthors: this.authors.size,
            averagePostsPerAuthor: this.authors.size > 0 ? this.posts.size / this.authors.size : 0
        };
    }
}

// Test du système de blog avancé
async function testerBlogSystem() {
    console.log("🚀 Test du système de blog TypeScript avancé\n");
    
    const blogService = new BlogService();
    
    try {
        // Créer des auteurs
        const author1 = await blogService.createAuthor({
            id: "author1",
            name: "Alice Developer",
            email: "alice@dev.com",
            bio: "Développeuse passionnée",
            socialLinks: {
                twitter: "@alice_dev",
                website: "https://alice.dev"
            }
        });
        
        const author2 = await blogService.createAuthor({
            id: "author2",
            name: "Bob Writer",
            email: "bob@writer.com",
            bio: "Écrivain technique",
            socialLinks: {
                linkedin: "bob-writer"
            }
        });
        
        // Créer des articles
        const post1 = await blogService.createPost({
            title: "Introduction à TypeScript",
            content: "TypeScript est un langage de programmation développé par Microsoft qui ajoute le typage statique à JavaScript. Cela permet de détecter les erreurs plus tôt dans le processus de développement...",
            authorId: author1.id,
            tags: ["typescript", "javascript", "programming"],
            status: "draft"
        });
        
        const post2 = await blogService.createPost({
            title: "Les avantages du typage statique",
            content: "Le typage statique offre de nombreux avantages pour le développement d'applications complexes. Il améliore la maintenabilité, réduit les bugs et facilite la collaboration en équipe...",
            authorId: author2.id,
            tags: ["typescript", "best-practices"],
            status: "draft"
        });
        
        // Publier un article
        console.log("\n--- Publication d'articles ---");
        await blogService.publishPost(post1.id);
        
        // Recherche avec types conditionnels
        console.log("\n--- Recherche d'articles ---");
        const fullPost = await blogService.getPost(post1.id, true);
        if (fullPost) {
            console.log(`📖 Article complet: "${fullPost.title}" (${fullPost.content.length} caractères)`);
        }
        
        const summary = await blogService.getPost(post2.id, false);
        if (summary) {
            console.log(`📄 Résumé: "${summary.title}" (status: ${summary.status})`);
        }
        
        // Filtrage avancé
        console.log("\n--- Filtrage et tri ---");
        const publishedPosts = await blogService.findPosts(
            { status: "published" },
            "updatedAt"
        );
        console.log(`📚 Articles publiés: ${publishedPosts.length}`);
        
        const typescriptPosts = await blogService.findPosts(
            { tags: ["typescript"] }
        );
        console.log(`🏷️ Articles sur TypeScript: ${typescriptPosts.length}`);
        
        // Articles par auteur
        console.log("\n--- Articles par auteur ---");
        const alicePosts = await blogService.getAuthorPosts(author1.id, "summary");
        console.log(`👤 Articles d'Alice (résumé): ${alicePosts.length}`);
        
        const bobPosts = await blogService.getAuthorPosts(author2.id, "full");
        console.log(`👤 Articles de Bob (complet): ${bobPosts.length}`);
        
        // Statistiques
        console.log("\n--- Statistiques ---");
        const stats = blogService.getStats();
        console.log(`📊 Total: ${stats.totalPosts} articles`);
        console.log(`📊 Par statut:`, stats.postsByStatus);
        console.log(`📊 Auteurs: ${stats.totalAuthors}`);
        console.log(`📊 Moyenne: ${stats.averagePostsPerAuthor.toFixed(1)} articles/auteur`);
        
    } catch (error) {
        if (error instanceof ValidationError) {
            console.error(`❌ Erreur de validation: ${error.message}`);
        } else {
            console.error(`❌ Erreur inattendue:`, error);
        }
    }
}

// Classe d'erreur personnalisée
class ValidationError extends Error {
    constructor(message: string) {
        super(message);
        this.name = 'ValidationError';
    }
}

testerBlogSystem();

console.log("\n=== FONCTIONNALITÉS TYPESCRIPT AVANCÉES UTILISÉES ===");
console.log("✅ Types conditionnels (Conditional Types)");
console.log("✅ Types mappés (Mapped Types)");
console.log("✅ Types utilitaires (Utility Types)");
console.log("✅ Génériques avec contraintes");
console.log("✅ Type Guards personnalisés");
console.log("✅ Décorateurs (simulation)");
console.log("✅ Union et Intersection Types");
console.log("✅ Template Literal Types");
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Système de permissions typé
**Consigne :** Créez un système de gestion de permissions avec TypeScript avancé.

**Code de départ :**
```typescript
// TODO: Implémenter un système qui :
// - Définit des rôles et permissions avec des types stricts
// - Utilise des types conditionnels pour les vérifications
// - Implemente des decorateurs pour la vérification automatique
// - Gère les permissions hiérarchiques

type Permission = "read" | "write" | "delete" | "admin";
type Role = "user" | "moderator" | "admin";

// Votre implémentation ici
```

**Résultat attendu :**
- Système de types qui empêche les erreurs de permission
- Décorateurs pour la vérification automatique
- Hiérarchie de permissions typée

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 2 : Builder Pattern typé
**Consigne :** Implémentez le pattern Builder avec TypeScript pour la création d'objets complexes.

**Contraintes :**
- Types qui garantissent que tous les champs requis sont définis
- Méthodes chaînées avec types appropriés
- Validation à la compilation

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

### Exercice 3 : ORM simple avec TypeScript
**Consigne :** Créez un mini-ORM avec des requêtes typées.

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```typescript
// Système de permissions typé complet
type Permission = "read" | "write" | "delete" | "admin";
type Role = "user" | "moderator" | "admin";

// Mapping des permissions par rôle
type RolePermissions = {
    user: "read";
    moderator: "read" | "write";
    admin: "read" | "write" | "delete" | "admin";
};

// Type conditionnel pour vérifier les permissions
type HasPermission<R extends Role, P extends Permission> = 
    P extends RolePermissions[R] ? true : false;

// Interface pour les objets avec permissions
interface SecureResource {
    id: string;
    requiredPermission: Permission;
}

// Décorateur de vérification de permission
function requiresPermission<P extends Permission>(permission: P) {
    return function <T extends { userRole: Role }>(
        target: any,
        propertyKey: string,
        descriptor: PropertyDescriptor
    ) {
        const originalMethod = descriptor.value;
        
        descriptor.value = function (this: T, ...args: any[]) {
            if (!hasPermission(this.userRole, permission)) {
                throw new Error(`Permission denied. Required: ${permission}, User role: ${this.userRole}`);
            }
            return originalMethod.apply(this, args);
        };
    };
}

// Function pour vérifier les permissions
function hasPermission<R extends Role, P extends Permission>(
    role: R, 
    permission: P
): HasPermission<R, P> {
    const permissions: Record<Role, Permission[]> = {
        user: ["read"],
        moderator: ["read", "write"],
        admin: ["read", "write", "delete", "admin"]
    };
    
    return permissions[role].includes(permission) as HasPermission<R, P>;
}

// Service avec permissions typées
class SecureDocumentService {
    constructor(public userRole: Role) {}
    
    @requiresPermission("read")
    readDocument(id: string): string {
        return `Document ${id} content`;
    }
    
    @requiresPermission("write")
    updateDocument(id: string, content: string): boolean {
        console.log(`Updating document ${id}`);
        return true;
    }
    
    @requiresPermission("delete")
    deleteDocument(id: string): boolean {
        console.log(`Deleting document ${id}`);
        return true;
    }
    
    @requiresPermission("admin")
    managePermissions(userId: string, newRole: Role): boolean {
        console.log(`Changing user ${userId} role to ${newRole}`);
        return true;
    }
}

// Test du système
function testPermissionSystem() {
    const userService = new SecureDocumentService("user");
    const adminService = new SecureDocumentService("admin");
    
    try {
        // OK - user peut lire
        userService.readDocument("doc1");
        console.log("✅ User read: OK");
        
        // KO - user ne peut pas supprimer
        userService.deleteDocument("doc1");
    } catch (error) {
        console.log("❌ User delete: " + error.message);
    }
    
    try {
        // OK - admin peut tout faire
        adminService.deleteDocument("doc1");
        console.log("✅ Admin delete: OK");
    } catch (error) {
        console.log("❌ Admin delete: " + error.message);
    }
}

testPermissionSystem();
```

**Explication :** Le système utilise des types conditionnels pour vérifier les permissions à la compilation et des décorateurs pour la vérification runtime.

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **JavaScript ES6+** : Classes, modules, destructuring
- **POO** : Héritage, encapsulation, polymorphisme
- **Patterns** : Factory, Builder, Observer
- **APIs** : Fetch, Promise, async/await

### Prochaines étapes
- **Frameworks TypeScript** : Angular, NestJS, TypeORM
- **Outils avancés** : ESLint + TypeScript, Prettier
- **Testing** : Jest avec TypeScript, type mocking

### Concepts complémentaires
- **Tooling** : TSConfig, compilation, source maps
- **Performance** : Compilation incrémentale, project references
- **Integration** : Babel, Webpack, Vite avec TypeScript

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un système de gestion de tâches typé avec TypeScript

**Fonctionnalités requises :**
- [ ] Types stricts pour tâches, projets, utilisateurs
- [ ] API client générique avec validation
- [ ] Système de permissions par rôle
- [ ] Builder pattern pour la création de tâches
- [ ] Validation avec décorateurs
- [ ] Export des données avec types préservés

**Structure suggérée :**
```
📁 task-manager-ts/
├── 📄 types/
│   ├── user.ts
│   ├── task.ts
│   └── project.ts
├── 📄 services/
│   ├── api-client.ts
│   ├── task-service.ts
│   └── permission-service.ts
├── 📄 builders/
│   └── task-builder.ts
├── 📄 decorators/
│   └── validation.ts
└── 📄 main.ts
```

**Temps estimé :** 2.5 heures

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [DefinitelyTyped](https://definitelytyped.org/) - Types pour les librairies JS

### Vidéos recommandées
- "TypeScript Course for Beginners" - freeCodeCamp (5h)
- Pourquoi cette vidéo est utile : Cours complet avec projets pratiques

### Articles approfondis
- "Advanced TypeScript Types" - Marius Schulz
- Ce que cet article apporte en plus : Types avancés avec exemples concrets

### Outils de pratique
- [TypeScript Playground](https://www.typescriptlang.org/play) - Test en ligne
- [Type Challenges](https://github.com/type-challenges/type-challenges) - Exercices de types

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre `interface` et `type` en TypeScript ?
   - Réponse : interface peut être étendue et mergée, type est plus flexible pour les unions et conditions

2. **Question pratique :** Comment créer un type qui prend seulement certaines clés d'un objet ?
   - Réponse : Utiliser `Pick<T, K>` ou `Omit<T, K>` selon le besoin

3. **Question d'application :** Quand utiliser des génériques vs des types union ?
   - Réponse : Génériques pour la réutilisabilité et la préservation de type, unions pour des valeurs fixes

### Checklist de maîtrise
- [ ] Je comprends les types de base et leur utilisation
- [ ] Je sais créer et utiliser des interfaces
- [ ] Je maîtrise les génériques et leurs contraintes
- [ ] Je comprends les types utilitaires (Pick, Omit, etc.)
- [ ] Je peux utiliser les types conditionnels et mappés
- [ ] Je sais configurer un projet TypeScript

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```typescript
// Utilisation excessive d'any
function processData(data: any): any {
    return data.someProperty.transform();
}
```

**Solution :**
```typescript
// Types stricts avec génériques
function processData<T extends { someProperty: { transform(): U } }, U>(data: T): U {
    return data.someProperty.transform();
}
```

**Explication :** Éviter `any` preserve les bénéfices du typage statique.

### Erreur fréquente 2
**Code problématique :**
```typescript
// Oublier les type guards
function processUser(user: User | null) {
    console.log(user.name); // ❌ Erreur: user peut être null
}
```

**Solution :**
```typescript
function processUser(user: User | null) {
    if (user) { // Type guard
        console.log(user.name); // ✅ OK: user est maintenant de type User
    }
}
```

**Explication :** Toujours vérifier les types nullable avant utilisation.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les bases du système de types TypeScript
- L'importance du typage statique pour la maintenabilité
- Les patterns avancés avec génériques et types conditionnels

### Questions à approfondir
- Décorateurs officiels et métaprogrammation
- Performance de compilation sur gros projets
- Integration avec les frameworks populaires

### Révision programmée
- [ ] Révision dans 1 jour (rappel rapide)
- [ ] Révision dans 1 semaine (exercices)
- [ ] Révision dans 1 mois (application dans un projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #typescript #types #interfaces #generics #static-typing #avance

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 13.5/126*
