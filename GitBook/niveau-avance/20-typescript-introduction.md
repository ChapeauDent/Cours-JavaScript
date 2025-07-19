# 📘 Introduction à TypeScript

> **Niveau :** 🔴 Avancé | **Durée :** 90 minutes | **Prérequis :** ES6+, Modules, Classes

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Comprendre** les avantages du typage statique
- [ ] **Maîtriser** les types TypeScript et interfaces
- [ ] **Configurer** un projet TypeScript professionnel
- [ ] **Créer** des applications robustes et maintenables

---

## 📚 Introduction

{% tabs %}
{% tab title="🤔 Pourquoi TypeScript ?" %}
**Les problèmes de JavaScript à grande échelle :**

- 🚫 **Erreurs silencieuses** : Bugs découverts en production
- 🚫 **Refactoring risqué** : Pas de garantie sur les modifications
- 🚫 **Documentation obsolète** : Code et doc désynchronisés
- 🚫 **IDE limité** : Autocomplétion et navigation difficiles
- 🚫 **Collaboration complexe** : Interfaces implicites

**TypeScript résout ces problèmes :**

- ✅ **Détection précoce** : Erreurs trouvées à la compilation
- ✅ **Refactoring sûr** : Modifications assistées par le compilateur
- ✅ **Documentation vivante** : Types comme documentation
- ✅ **Developer Experience** : IntelliSense et navigation optimales
- ✅ **Collaboration fluide** : Interfaces explicites et partagées
{% endtab %}

{% tab title="🏗️ TypeScript ecosystem" %}
**Adoption massive dans l'industrie :**

```typescript
// React avec TypeScript
interface Props {
    name: string;
    age: number;
    onUpdate: (data: UserData) => void;
}

const UserProfile: React.FC<Props> = ({ name, age, onUpdate }) => {
    // Code typé et sûr
};

// Node.js avec TypeScript  
import express from 'express';
const app: express.Application = express();

// Vue 3 avec TypeScript
import { defineComponent, PropType } from 'vue';

export default defineComponent({
    props: {
        user: {
            type: Object as PropType<User>,
            required: true
        }
    }
});
```

**Projets majeurs utilisant TypeScript :**
- Microsoft (VS Code, Office)
- Google (Angular, Firebase)
- Meta (Jest, Relay)
- Airbnb, Slack, Discord, Shopify...
{% endtab %}

{% tab title="⚡ TypeScript vs JavaScript" %}
| Aspect | JavaScript | TypeScript |
|--------|------------|------------|
| **Types** | Dynamiques (runtime) | Statiques (compile-time) |
| **Erreurs** | Découvertes en exécution | Détectées à la compilation |
| **IDE Support** | Limité | Excellent (IntelliSense) |
| **Refactoring** | Risqué | Sûr et assisté |
| **Documentation** | Externe (peut être obsolète) | Intégrée (types) |
| **Performance** | Runtime checks | Optimisations compile-time |
| **Courbe d'apprentissage** | Faible | Modérée |
| **Écosystème** | Universel | Croissance rapide |

```javascript
// JavaScript - Erreur silencieuse
function calculateTotal(price, tax) {
    return price + tax;
}

calculateTotal("100", 0.2); // "1000.2" 😱

// TypeScript - Erreur détectée
function calculateTotal(price: number, tax: number): number {
    return price + tax;
}

calculateTotal("100", 0.2); // ❌ Compilation error!
```
{% endtab %}
{% endtabs %}

---

## 🔤 Types Fondamentaux

### Types primitifs

{% tabs %}
{% tab title="🎯 Types de base" %}
```typescript
// Types primitifs
let userName: string = "Alice";
let userAge: number = 30;
let isActive: boolean = true;
let data: null = null;
let value: undefined = undefined;

// Type any (à éviter !)
let anything: any = "hello";
anything = 42; // Pas d'erreur, mais on perd le typage

// Type unknown (plus sûr que any)
let userInput: unknown = getUserInput();

// Il faut vérifier le type avant utilisation
if (typeof userInput === "string") {
    console.log(userInput.toUpperCase()); // ✅ Safe
}

// Type never (pour les fonctions qui ne retournent jamais)
function throwError(message: string): never {
    throw new Error(message);
}

// Type void (pour les fonctions sans retour)
function logMessage(message: string): void {
    console.log(message);
    // Pas de return
}

// Inférence de type
let message = "Hello"; // TypeScript infère string
let count = 42;        // TypeScript infère number

// Type assertions (cast)
let someValue: unknown = "this is a string";
let strLength: number = (someValue as string).length;
// Ou avec syntaxe alternative
let strLength2: number = (<string>someValue).length;
```
{% endtab %}

{% tab title="📊 Types composés" %}
```typescript
// Arrays
let numbers: number[] = [1, 2, 3, 4, 5];
let names: Array<string> = ["Alice", "Bob", "Charlie"];
let matrix: number[][] = [[1, 2], [3, 4]];

// Tuples (arrays avec types fixes et longueur définie)
let coordinates: [number, number] = [10, 20];
let userInfo: [string, number, boolean] = ["Alice", 30, true];

// Tuples avec labels (TS 4.0+)
let point: [x: number, y: number] = [10, 20];

// Rest elements dans les tuples
type StringNumberBooleans = [string, number, ...boolean[]];
let data: StringNumberBooleans = ["hello", 42, true, false, true];

// Union Types (OU logique)
let id: string | number = "user123";
id = 456; // ✅ Valide

function printId(id: string | number): void {
    if (typeof id === "string") {
        console.log(`String ID: ${id.toUpperCase()}`);
    } else {
        console.log(`Number ID: ${id.toFixed(2)}`);
    }
}

// Intersection Types (ET logique)
interface Person {
    name: string;
    age: number;
}

interface Employee {
    employeeId: string;
    department: string;
}

type PersonEmployee = Person & Employee;

const worker: PersonEmployee = {
    name: "Alice",
    age: 30,
    employeeId: "EMP001",
    department: "Engineering"
};

// Literal Types
type Status = "pending" | "approved" | "rejected";
type Direction = "up" | "down" | "left" | "right";
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";

let currentStatus: Status = "pending";
// currentStatus = "unknown"; // ❌ Error!

// Template Literal Types (TS 4.1+)
type EventName = "click" | "focus" | "blur";
type EventHandler = `on${Capitalize<EventName>}`;
// Résultat: "onClick" | "onFocus" | "onBlur"
```
{% endtab %}

{% tab title="🔍 Type Guards" %}
```typescript
// Type Guards - Vérifications de type à l'exécution

// 1. typeof guards
function processValue(value: string | number): string {
    if (typeof value === "string") {
        // TypeScript sait que value est string ici
        return value.toUpperCase();
    } else {
        // TypeScript sait que value est number ici
        return value.toFixed(2);
    }
}

// 2. instanceof guards
class Dog {
    bark(): void {
        console.log("Woof!");
    }
}

class Cat {
    meow(): void {
        console.log("Meow!");
    }
}

function makeSound(animal: Dog | Cat): void {
    if (animal instanceof Dog) {
        animal.bark(); // ✅ TypeScript sait que c'est un Dog
    } else {
        animal.meow(); // ✅ TypeScript sait que c'est un Cat
    }
}

// 3. in operator guards
interface Bird {
    type: "bird";
    fly(): void;
}

interface Fish {
    type: "fish";
    swim(): void;
}

function move(animal: Bird | Fish): void {
    if ("fly" in animal) {
        animal.fly(); // ✅ Bird
    } else {
        animal.swim(); // ✅ Fish
    }
}

// 4. Custom type guards (user-defined)
interface User {
    name: string;
    email: string;
}

function isUser(obj: any): obj is User {
    return obj && 
           typeof obj.name === "string" && 
           typeof obj.email === "string";
}

function processUserData(data: unknown): void {
    if (isUser(data)) {
        // TypeScript sait maintenant que data est User
        console.log(`Hello ${data.name}!`);
        console.log(`Email: ${data.email}`);
    } else {
        console.log("Invalid user data");
    }
}

// 5. Discriminated Unions
interface Square {
    kind: "square";
    size: number;
}

interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}

interface Circle {
    kind: "circle";
    radius: number;
}

type Shape = Square | Rectangle | Circle;

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "square":
            return shape.size * shape.size;
        case "rectangle":
            return shape.width * shape.height;
        case "circle":
            return Math.PI * shape.radius ** 2;
        default:
            // Exhaustiveness check
            const _exhaustiveCheck: never = shape;
            return _exhaustiveCheck;
    }
}
```
{% endtab %}
{% endtabs %}

### 🏃‍♂️ Exercice Pratique : Système de validation typé

{% hint style="warning" %}
**Exercice :** Créez un système de validation avec types TypeScript avancés
{% endhint %}

```typescript
// Complétez ce système de validation
type ValidationResult<T> = {
    success: boolean;
    data?: T;
    errors: string[];
};

// TODO: Définir les types pour les règles de validation
type ValidationRule<T> = {
    // Définir la structure d'une règle
};

// TODO: Créer des types pour différents types de données
interface UserData {
    name: string;
    email: string;
    age: number;
    role: 'admin' | 'user' | 'moderator';
}

interface ProductData {
    id: string;
    name: string;
    price: number;
    category: string;
    inStock: boolean;
}

// TODO: Implémenter le validateur générique
class Validator<T> {
    private rules: ValidationRule<T>[] = [];
    
    // TODO: Ajouter des règles de validation
    addRule(rule: ValidationRule<T>): this {
        this.rules.push(rule);
        return this;
    }
    
    // TODO: Valider les données
    validate(data: unknown): ValidationResult<T> {
        const errors: string[] = [];
        
        // 1. Vérification basique du type
        if (!data || typeof data !== 'object') {
            return {
                success: false,
                errors: ['Les données doivent être un objet valide']
            };
        }
        
        // 2. Appliquer toutes les règles
        for (const rule of this.rules) {
            const value = (data as any)[rule.field];
            
            if (!rule.validate(value, data as T)) {
                errors.push(rule.message);
            }
        }
        
        // 3. Retourner le résultat
        return {
            success: errors.length === 0,
            data: errors.length === 0 ? data as T : undefined,
            errors
        };
    }
    
    // TODO: Méthodes helper pour règles communes
    static required<T>(field: keyof T, message?: string): ValidationRule<T> {
        // Règle pour champ requis
    }
    
    static minLength<T>(field: keyof T, min: number, message?: string): ValidationRule<T> {
        // Règle pour longueur minimale
    }
    
    static email<T>(field: keyof T, message?: string): ValidationRule<T> {
        // Règle pour format email
    }
    
    static range<T>(field: keyof T, min: number, max: number, message?: string): ValidationRule<T> {
        // Règle pour plage numérique
    }
}

// Tests
const userValidator = new Validator<UserData>()
    .addRule(Validator.required('name', 'Le nom est requis'))
    .addRule(Validator.minLength('name', 2, 'Le nom doit faire au moins 2 caractères'))
    .addRule(Validator.email('email', 'Email invalide'))
    .addRule(Validator.range('age', 0, 120, 'Âge invalide'));

const userData = {
    name: "Alice",
    email: "alice@example.com",
    age: 25,
    role: "admin" as const
};

const result = userValidator.validate(userData);
console.log(result);
```

{% tabs %}
{% tab title="💡 Indice" %}
```typescript
type ValidationRule<T> = {
    field: keyof T;
    validate: (value: any, data: T) => boolean;
    message: string;
};

class Validator<T> {
    validate(data: unknown): ValidationResult<T> {
        const errors: string[] = [];
        
        // Type guard pour vérifier la structure de base
        if (!this.isValidType(data)) {
            return {
                success: false,
                errors: ['Données invalides']
            };
        }
        
        // Appliquer les règles
        for (const rule of this.rules) {
            const value = (data as any)[rule.field];
            if (!rule.validate(value, data as T)) {
                errors.push(rule.message);
            }
        }
        
        return {
            success: errors.length === 0,
            data: errors.length === 0 ? data as T : undefined,
            errors
        };
    }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```typescript
type ValidationResult<T> = {
    success: boolean;
    data?: T;
    errors: string[];
};

type ValidationRule<T> = {
    field: keyof T;
    validate: (value: any, data: T) => boolean;
    message: string;
};

class Validator<T> {
    private rules: ValidationRule<T>[] = [];
    private typeGuard?: (obj: any) => obj is T;
    
    constructor(typeGuard?: (obj: any) => obj is T) {
        this.typeGuard = typeGuard;
    }
    
    addRule(rule: ValidationRule<T>): this {
        this.rules.push(rule);
        return this;
    }
    
    validate(data: unknown): ValidationResult<T> {
        const errors: string[] = [];
        
        // Vérification du type de base
        if (this.typeGuard && !this.typeGuard(data)) {
            return {
                success: false,
                errors: ['Les données ne correspondent pas au type attendu']
            };
        }
        
        // Vérification basique
        if (!data || typeof data !== 'object') {
            return {
                success: false,
                errors: ['Les données doivent être un objet']
            };
        }
        
        // Appliquer toutes les règles
        for (const rule of this.rules) {
            const value = (data as any)[rule.field];
            
            try {
                if (!rule.validate(value, data as T)) {
                    errors.push(rule.message);
                }
            } catch (error) {
                errors.push(`Erreur de validation pour ${String(rule.field)}: ${error}`);
            }
        }
        
        return {
            success: errors.length === 0,
            data: errors.length === 0 ? data as T : undefined,
            errors
        };
    }
    
    // Méthodes statiques pour créer des règles communes
    static required<T>(field: keyof T, message?: string): ValidationRule<T> {
        return {
            field,
            validate: (value) => value !== null && value !== undefined && value !== '',
            message: message || `Le champ ${String(field)} est requis`
        };
    }
    
    static minLength<T>(field: keyof T, min: number, message?: string): ValidationRule<T> {
        return {
            field,
            validate: (value) => {
                if (typeof value !== 'string') return false;
                return value.length >= min;
            },
            message: message || `${String(field)} doit faire au moins ${min} caractères`
        };
    }
    
    static maxLength<T>(field: keyof T, max: number, message?: string): ValidationRule<T> {
        return {
            field,
            validate: (value) => {
                if (typeof value !== 'string') return false;
                return value.length <= max;
            },
            message: message || `${String(field)} ne peut pas dépasser ${max} caractères`
        };
    }
    
    static email<T>(field: keyof T, message?: string): ValidationRule<T> {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return {
            field,
            validate: (value) => {
                if (typeof value !== 'string') return false;
                return emailRegex.test(value);
            },
            message: message || `${String(field)} doit être un email valide`
        };
    }
    
    static range<T>(field: keyof T, min: number, max: number, message?: string): ValidationRule<T> {
        return {
            field,
            validate: (value) => {
                if (typeof value !== 'number') return false;
                return value >= min && value <= max;
            },
            message: message || `${String(field)} doit être entre ${min} et ${max}`
        };
    }
    
    static oneOf<T, K extends keyof T>(field: K, values: T[K][], message?: string): ValidationRule<T> {
        return {
            field,
            validate: (value) => values.includes(value),
            message: message || `${String(field)} doit être l'une des valeurs: ${values.join(', ')}`
        };
    }
    
    static custom<T>(field: keyof T, validator: (value: any, data: T) => boolean, message: string): ValidationRule<T> {
        return {
            field,
            validate: validator,
            message
        };
    }
}

// Type guards pour vérifier les types
function isUserData(obj: any): obj is UserData {
    return obj &&
           typeof obj.name === 'string' &&
           typeof obj.email === 'string' &&
           typeof obj.age === 'number' &&
           ['admin', 'user', 'moderator'].includes(obj.role);
}

function isProductData(obj: any): obj is ProductData {
    return obj &&
           typeof obj.id === 'string' &&
           typeof obj.name === 'string' &&
           typeof obj.price === 'number' &&
           typeof obj.category === 'string' &&
           typeof obj.inStock === 'boolean';
}

// Définition des types
interface UserData {
    name: string;
    email: string;
    age: number;
    role: 'admin' | 'user' | 'moderator';
}

interface ProductData {
    id: string;
    name: string;
    price: number;
    category: string;
    inStock: boolean;
}

// Exemples d'utilisation
const userValidator = new Validator<UserData>(isUserData)
    .addRule(Validator.required('name'))
    .addRule(Validator.minLength('name', 2))
    .addRule(Validator.maxLength('name', 50))
    .addRule(Validator.required('email'))
    .addRule(Validator.email('email'))
    .addRule(Validator.required('age'))
    .addRule(Validator.range('age', 0, 120))
    .addRule(Validator.oneOf('role', ['admin', 'user', 'moderator']));

const productValidator = new Validator<ProductData>(isProductData)
    .addRule(Validator.required('id'))
    .addRule(Validator.minLength('id', 3))
    .addRule(Validator.required('name'))
    .addRule(Validator.minLength('name', 1))
    .addRule(Validator.range('price', 0, Number.MAX_SAFE_INTEGER))
    .addRule(Validator.custom('inStock', (value, data) => {
        // Règle métier: si le prix est 0, le produit ne peut pas être en stock
        return data.price > 0 || !value;
    }, 'Un produit gratuit ne peut pas être en stock'));

// Tests
console.log('=== Test User Validation ===');

const validUser = {
    name: "Alice",
    email: "alice@example.com",
    age: 25,
    role: "admin" as const
};

const invalidUser = {
    name: "A",
    email: "invalid-email",
    age: 150,
    role: "unknown"
};

console.log('Valid user:', userValidator.validate(validUser));
console.log('Invalid user:', userValidator.validate(invalidUser));

console.log('\n=== Test Product Validation ===');

const validProduct = {
    id: "PRD001",
    name: "Laptop",
    price: 999.99,
    category: "Electronics",
    inStock: true
};

const invalidProduct = {
    id: "P1",
    name: "",
    price: 0,
    category: "Electronics",
    inStock: true // Invalide car prix = 0
};

console.log('Valid product:', productValidator.validate(validProduct));
console.log('Invalid product:', productValidator.validate(invalidProduct));
```
{% endtab %}
{% endtabs %}

---

## 🏗️ Interfaces et Classes

### Interfaces avancées

{% tabs %}
{% tab title="📋 Définition interfaces" %}
```typescript
// Interface de base
interface User {
    readonly id: number;        // Propriété en lecture seule
    name: string;
    email: string;
    age?: number;               // Propriété optionnelle
    preferences: {
        theme: 'light' | 'dark';
        language: string;
    };
}

// Extension d'interface
interface AdminUser extends User {
    permissions: string[];
    lastLogin: Date;
}

// Interface avec signatures d'index
interface StringDictionary {
    [key: string]: string;
}

interface NumberDictionary {
    [key: string]: number;
    length: number;             // OK, length est un number
    // name: string;            // ❌ Error! 'name' n'est pas un number
}

// Interface hybride (objet + fonction)
interface Counter {
    (start: number): string;    // Signature d'appel
    interval: number;           // Propriété
    reset(): void;              // Méthode
}

function createCounter(): Counter {
    let counter = function(start: number) {
        return `Count: ${start}`;
    } as Counter;
    
    counter.interval = 1000;
    counter.reset = function() {
        console.log('Counter reset');
    };
    
    return counter;
}

// Interface avec méthodes
interface Repository<T> {
    create(item: T): Promise<T>;
    findById(id: string): Promise<T | null>;
    findAll(): Promise<T[]>;
    update(id: string, item: Partial<T>): Promise<T>;
    delete(id: string): Promise<boolean>;
}

// Interface pour classes (contrat)
interface Drivable {
    speed: number;
    start(): void;
    stop(): void;
    accelerate(amount: number): void;
}

class Car implements Drivable {
    speed: number = 0;
    
    start(): void {
        console.log('Car started');
    }
    
    stop(): void {
        this.speed = 0;
        console.log('Car stopped');
    }
    
    accelerate(amount: number): void {
        this.speed += amount;
        console.log(`Speed: ${this.speed} km/h`);
    }
}

// Interface avec génériques
interface ApiResponse<T> {
    success: boolean;
    data: T;
    message?: string;
    errors?: string[];
}

// Utilisation
type UserResponse = ApiResponse<User>;
type UsersResponse = ApiResponse<User[]>;

// Interface conditionnelle
interface BaseConfig {
    debug: boolean;
}

interface DevConfig extends BaseConfig {
    debug: true;
    hotReload: boolean;
    sourceMaps: boolean;
}

interface ProdConfig extends BaseConfig {
    debug: false;
    minify: boolean;
    optimize: boolean;
}

type Config = DevConfig | ProdConfig;
```
{% endtab %}

{% tab title="🎭 Classes TypeScript" %}
```typescript
// Classe de base avec modificateurs d'accès
abstract class Animal {
    protected name: string;                    // Accessible dans les sous-classes
    private _age: number;                      // Privé à cette classe
    public readonly species: string;           // Public et lecture seule
    
    constructor(name: string, age: number, species: string) {
        this.name = name;
        this._age = age;
        this.species = species;
    }
    
    // Getter/Setter
    get age(): number {
        return this._age;
    }
    
    set age(value: number) {
        if (value >= 0) {
            this._age = value;
        } else {
            throw new Error('Age cannot be negative');
        }
    }
    
    // Méthode abstraite (doit être implémentée dans les sous-classes)
    abstract makeSound(): void;
    
    // Méthode concrète
    protected move(): void {
        console.log(`${this.name} is moving`);
    }
    
    // Méthode statique
    static createFromData(data: any): Animal {
        // Factory method
        if (data.type === 'dog') {
            return new Dog(data.name, data.age);
        } else if (data.type === 'cat') {
            return new Cat(data.name, data.age);
        }
        throw new Error('Unknown animal type');
    }
}

// Classes dérivées
class Dog extends Animal {
    private breed: string;
    
    constructor(name: string, age: number, breed: string = 'Mixed') {
        super(name, age, 'Canis lupus');
        this.breed = breed;
    }
    
    makeSound(): void {
        console.log(`${this.name} barks: Woof!`);
    }
    
    // Override avec super
    protected move(): void {
        super.move();
        console.log('Running on four legs');
    }
    
    // Méthode spécifique à Dog
    fetch(): void {
        console.log(`${this.name} fetches the ball`);
    }
    
    getBreed(): string {
        return this.breed;
    }
}

class Cat extends Animal {
    constructor(name: string, age: number) {
        super(name, age, 'Felis catus');
    }
    
    makeSound(): void {
        console.log(`${this.name} meows: Meow!`);
    }
    
    climb(): void {
        console.log(`${this.name} climbs the tree`);
    }
}

// Classe avec génériques
class GenericRepository<T extends { id: string }> {
    private items: Map<string, T> = new Map();
    
    async create(item: T): Promise<T> {
        this.items.set(item.id, item);
        return item;
    }
    
    async findById(id: string): Promise<T | null> {
        return this.items.get(id) || null;
    }
    
    async findAll(): Promise<T[]> {
        return Array.from(this.items.values());
    }
    
    async update(id: string, updates: Partial<T>): Promise<T | null> {
        const item = this.items.get(id);
        if (item) {
            const updated = { ...item, ...updates };
            this.items.set(id, updated);
            return updated;
        }
        return null;
    }
    
    async delete(id: string): Promise<boolean> {
        return this.items.delete(id);
    }
    
    get count(): number {
        return this.items.size;
    }
}

// Utilisation
const userRepo = new GenericRepository<User>();
const productRepo = new GenericRepository<ProductData>();

// Classe utilitaire avec méthodes statiques
class DateUtils {
    static format(date: Date, format: string): string {
        // Implementation simple
        return date.toISOString().split('T')[0];
    }
    
    static addDays(date: Date, days: number): Date {
        const result = new Date(date);
        result.setDate(result.getDate() + days);
        return result;
    }
    
    static isWeekend(date: Date): boolean {
        const day = date.getDay();
        return day === 0 || day === 6;
    }
    
    // Propriété statique
    static readonly DAYS_IN_WEEK = 7;
    static readonly MONTHS = [
        'January', 'February', 'March', 'April',
        'May', 'June', 'July', 'August',
        'September', 'October', 'November', 'December'
    ] as const;
}

// Classe avec décorateurs (expérimental)
class Component {
    @readonly
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
    
    @logExecution
    process(): void {
        console.log(`Processing ${this.name}`);
    }
}

// Décorateur simple
function readonly(target: any, propertyKey: string) {
    Object.defineProperty(target, propertyKey, {
        writable: false
    });
}

function logExecution(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    
    descriptor.value = function(...args: any[]) {
        console.log(`Executing ${propertyKey}`);
        const result = originalMethod.apply(this, args);
        console.log(`Finished ${propertyKey}`);
        return result;
    };
}
```
{% endtab %}

{% tab title="🔗 Mixins et composition" %}
```typescript
// Pattern Mixin pour composition de comportements

// Classes de base pour mixins
class Timestamped {
    timestamp: Date = new Date();
    
    touch(): void {
        this.timestamp = new Date();
    }
}

class Activatable {
    isActive: boolean = false;
    
    activate(): void {
        this.isActive = true;
    }
    
    deactivate(): void {
        this.isActive = false;
    }
}

class Labeled {
    label: string = '';
    
    setLabel(label: string): void {
        this.label = label;
    }
}

// Type pour constructor
type Constructor<T = {}> = new (...args: any[]) => T;

// Mixin functions
function WithTimestamp<TBase extends Constructor>(Base: TBase) {
    return class extends Base {
        timestamp: Date = new Date();
        
        touch(): void {
            this.timestamp = new Date();
        }
    };
}

function WithActivation<TBase extends Constructor>(Base: TBase) {
    return class extends Base {
        isActive: boolean = false;
        
        activate(): void {
            this.isActive = true;
        }
        
        deactivate(): void {
            this.isActive = false;
        }
    };
}

function WithLabel<TBase extends Constructor>(Base: TBase) {
    return class extends Base {
        label: string = '';
        
        setLabel(label: string): void {
            this.label = label;
        }
    };
}

// Classe de base
class BaseEntity {
    id: string;
    
    constructor(id: string) {
        this.id = id;
    }
}

// Composition avec mixins
const TimestampedEntity = WithTimestamp(BaseEntity);
const ActivatableEntity = WithActivation(BaseEntity);
const FullEntity = WithLabel(WithActivation(WithTimestamp(BaseEntity)));

// Utilisation
const entity = new FullEntity('entity-1');
entity.touch();
entity.activate();
entity.setLabel('My Entity');

console.log(entity.id);          // entity-1
console.log(entity.timestamp);   // Date actuelle
console.log(entity.isActive);    // true
console.log(entity.label);       // My Entity

// Interface pour typer les mixins
interface ITimestamped {
    timestamp: Date;
    touch(): void;
}

interface IActivatable {
    isActive: boolean;
    activate(): void;
    deactivate(): void;
}

interface ILabeled {
    label: string;
    setLabel(label: string): void;
}

// Type intersection pour les entités composées
type TimestampedActivatableEntity = BaseEntity & ITimestamped & IActivatable;
type FullEntityType = BaseEntity & ITimestamped & IActivatable & ILabeled;

// Factory function avec types stricts
function createFullEntity(id: string): FullEntityType {
    return new FullEntity(id);
}

// Alternative avec composition d'objets
class EntityBuilder {
    private entity: any = {};
    
    withId(id: string): this {
        this.entity.id = id;
        return this;
    }
    
    withTimestamp(): this {
        this.entity.timestamp = new Date();
        this.entity.touch = () => {
            this.entity.timestamp = new Date();
        };
        return this;
    }
    
    withActivation(): this {
        this.entity.isActive = false;
        this.entity.activate = () => {
            this.entity.isActive = true;
        };
        this.entity.deactivate = () => {
            this.entity.isActive = false;
        };
        return this;
    }
    
    withLabel(): this {
        this.entity.label = '';
        this.entity.setLabel = (label: string) => {
            this.entity.label = label;
        };
        return this;
    }
    
    build<T>(): T {
        return this.entity as T;
    }
}

// Utilisation du builder
const builtEntity = new EntityBuilder()
    .withId('built-1')
    .withTimestamp()
    .withActivation()
    .withLabel()
    .build<FullEntityType>();
```
{% endtab %}
{% endtabs %}

---

## 🧬 Génériques (Generics)

### Concepts avancés

{% tabs %}
{% tab title="🎯 Génériques de base" %}
```typescript
// Fonction générique simple
function identity<T>(arg: T): T {
    return arg;
}

// Utilisation avec inférence
let result1 = identity("hello");        // Type inféré: string
let result2 = identity(42);             // Type inféré: number
let result3 = identity<boolean>(true);  // Type explicite: boolean

// Générique avec contraintes
interface Lengthwise {
    length: number;
}

function logLength<T extends Lengthwise>(arg: T): T {
    console.log(`Length: ${arg.length}`);
    return arg;
}

logLength("hello");           // ✅ string a une propriété length
logLength([1, 2, 3]);         // ✅ array a une propriété length
// logLength(42);             // ❌ number n'a pas de propriété length

// Générique avec plusieurs types
function merge<T, U>(obj1: T, obj2: U): T & U {
    return { ...obj1, ...obj2 };
}

const merged = merge(
    { name: "Alice", age: 30 },
    { email: "alice@example.com", active: true }
);
// Type: { name: string; age: number; email: string; active: boolean; }

// Générique avec valeurs par défaut
interface ApiCall<TRequest = any, TResponse = any> {
    request: TRequest;
    response: TResponse;
    timestamp: Date;
}

// Utilisation avec defaults
type SimpleCall = ApiCall;  // ApiCall<any, any>
type UserCall = ApiCall<{ userId: string }, User>;

// Contraintes avec keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const person = { name: "Alice", age: 30, email: "alice@example.com" };

const name = getProperty(person, "name");     // Type: string
const age = getProperty(person, "age");       // Type: number
// const invalid = getProperty(person, "foo"); // ❌ 'foo' n'existe pas

// Type mapping avec génériques
type Partial<T> = {
    [P in keyof T]?: T[P];
};

type Required<T> = {
    [P in keyof T]-?: T[P];
};

type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

// Utilisation
type PartialUser = Partial<User>;     // Toutes les propriétés optionnelles
type RequiredUser = Required<User>;   // Toutes les propriétés requises
type ReadonlyUser = Readonly<User>;   // Toutes les propriétés en lecture seule
```
{% endtab %}

{% tab title="🏭 Factory Patterns" %}
```typescript
// Factory générique
abstract class Creator<T> {
    abstract create(...args: any[]): T;
    
    // Template method
    process(...args: any[]): T {
        const product = this.create(...args);
        this.configure(product);
        return product;
    }
    
    protected configure(product: T): void {
        // Configuration par défaut
    }
}

// Factory concrète
class UserCreator extends Creator<User> {
    create(name: string, email: string): User {
        return {
            id: Math.random(),
            name,
            email,
            preferences: {
                theme: 'light',
                language: 'en'
            }
        };
    }
    
    protected configure(user: User): void {
        // Configuration spécifique pour User
        user.preferences.theme = 'dark';
    }
}

// Factory registry
class FactoryRegistry {
    private static factories = new Map<string, Creator<any>>();
    
    static register<T>(type: string, factory: Creator<T>): void {
        this.factories.set(type, factory);
    }
    
    static create<T>(type: string, ...args: any[]): T {
        const factory = this.factories.get(type);
        if (!factory) {
            throw new Error(`No factory registered for type: ${type}`);
        }
        return factory.process(...args);
    }
}

// Repository pattern avec génériques
interface IRepository<T, ID = string> {
    findById(id: ID): Promise<T | null>;
    findAll(): Promise<T[]>;
    save(entity: T): Promise<T>;
    delete(id: ID): Promise<boolean>;
}

class InMemoryRepository<T extends { id: ID }, ID = string> implements IRepository<T, ID> {
    private data = new Map<ID, T>();
    
    async findById(id: ID): Promise<T | null> {
        return this.data.get(id) || null;
    }
    
    async findAll(): Promise<T[]> {
        return Array.from(this.data.values());
    }
    
    async save(entity: T): Promise<T> {
        this.data.set(entity.id, entity);
        return entity;
    }
    
    async delete(id: ID): Promise<boolean> {
        return this.data.delete(id);
    }
    
    async findByPredicate(predicate: (item: T) => boolean): Promise<T[]> {
        return Array.from(this.data.values()).filter(predicate);
    }
}

// Service layer avec injection de dépendance
class Service<T, ID = string> {
    constructor(private repository: IRepository<T, ID>) {}
    
    async getById(id: ID): Promise<T> {
        const entity = await this.repository.findById(id);
        if (!entity) {
            throw new Error(`Entity with id ${id} not found`);
        }
        return entity;
    }
    
    async getAll(): Promise<T[]> {
        return this.repository.findAll();
    }
    
    async create(entity: T): Promise<T> {
        return this.repository.save(entity);
    }
    
    async update(entity: T): Promise<T> {
        return this.repository.save(entity);
    }
    
    async remove(id: ID): Promise<void> {
        const success = await this.repository.delete(id);
        if (!success) {
            throw new Error(`Failed to delete entity with id ${id}`);
        }
    }
}

// Utilisation
const userRepo = new InMemoryRepository<User>();
const userService = new Service(userRepo);

// Event system générique
interface IEvent {
    type: string;
    timestamp: Date;
}

class EventBus<TEventMap extends Record<string, IEvent>> {
    private listeners = new Map<keyof TEventMap, Array<(event: any) => void>>();
    
    on<K extends keyof TEventMap>(
        eventType: K,
        listener: (event: TEventMap[K]) => void
    ): void {
        if (!this.listeners.has(eventType)) {
            this.listeners.set(eventType, []);
        }
        this.listeners.get(eventType)!.push(listener);
    }
    
    emit<K extends keyof TEventMap>(
        eventType: K,
        event: TEventMap[K]
    ): void {
        const listeners = this.listeners.get(eventType);
        if (listeners) {
            listeners.forEach(listener => listener(event));
        }
    }
    
    off<K extends keyof TEventMap>(
        eventType: K,
        listener: (event: TEventMap[K]) => void
    ): void {
        const listeners = this.listeners.get(eventType);
        if (listeners) {
            const index = listeners.indexOf(listener);
            if (index > -1) {
                listeners.splice(index, 1);
            }
        }
    }
}

// Définition des événements
interface AppEvents {
    'user-created': {
        type: 'user-created';
        timestamp: Date;
        user: User;
    };
    'user-updated': {
        type: 'user-updated';
        timestamp: Date;
        user: User;
        changes: Partial<User>;
    };
    'user-deleted': {
        type: 'user-deleted';
        timestamp: Date;
        userId: string;
    };
}

// Utilisation typée
const eventBus = new EventBus<AppEvents>();

eventBus.on('user-created', (event) => {
    // event est automatiquement typé
    console.log(`User ${event.user.name} was created`);
});

eventBus.emit('user-created', {
    type: 'user-created',
    timestamp: new Date(),
    user: { id: 1, name: "Alice", email: "alice@example.com" }
});
```
{% endtab %}

{% tab title="🔄 Utility Types" %}
```typescript
// Types utilitaires personnalisés

// Deep Partial - Rendre toutes les propriétés optionnelles récursivement
type DeepPartial<T> = {
    [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

interface NestedConfig {
    database: {
        host: string;
        port: number;
        credentials: {
            username: string;
            password: string;
        };
    };
    cache: {
        enabled: boolean;
        ttl: number;
    };
}

type PartialConfig = DeepPartial<NestedConfig>;
// Toutes les propriétés et sous-propriétés sont optionnelles

// Deep Readonly - Rendre toutes les propriétés readonly récursivement
type DeepReadonly<T> = {
    readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};

type ImmutableConfig = DeepReadonly<NestedConfig>;

// NonNullable - Exclure null et undefined
type NonNullable<T> = T extends null | undefined ? never : T;

type SafeString = NonNullable<string | null | undefined>; // string

// Extract specific types
type ExtractArrayType<T> = T extends (infer U)[] ? U : never;
type ExtractPromiseType<T> = T extends Promise<infer U> ? U : never;
type ExtractFunctionReturn<T> = T extends (...args: any[]) => infer R ? R : never;

// Tests
type NumberArray = ExtractArrayType<number[]>;        // number
type StringPromise = ExtractPromiseType<Promise<string>>; // string
type FunctionReturn = ExtractFunctionReturn<() => boolean>; // boolean

// Conditional types with inference
type ApiResult<T> = T extends string 
    ? { message: T } 
    : T extends number 
    ? { count: T }
    : T extends boolean 
    ? { success: T }
    : { data: T };

type StringResult = ApiResult<string>;   // { message: string }
type NumberResult = ApiResult<number>;   // { count: number }
type BooleanResult = ApiResult<boolean>; // { success: boolean }
type ObjectResult = ApiResult<User>;     // { data: User }

// Mapped types avancés
type Getters<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

type Setters<T> = {
    [K in keyof T as `set${Capitalize<string & K>}`]: (value: T[K]) => void;
};

type UserGetters = Getters<User>;
// {
//     getName: () => string;
//     getEmail: () => string;
//     getAge: () => number | undefined;
// }

type UserSetters = Setters<User>;
// {
//     setName: (value: string) => void;
//     setEmail: (value: string) => void;
//     setAge: (value: number | undefined) => void;
// }

// Function overloading avec génériques
class QueryBuilder<T> {
    where<K extends keyof T>(field: K, value: T[K]): this;
    where<K extends keyof T>(field: K, operator: 'gt' | 'lt' | 'eq', value: T[K]): this;
    where(field: string, ...args: any[]): this {
        // Implementation
        return this;
    }
    
    select<K extends keyof T>(...fields: K[]): Pick<T, K>[] {
        // Implementation
        return [] as Pick<T, K>[];
    }
    
    orderBy<K extends keyof T>(field: K, direction?: 'asc' | 'desc'): this {
        // Implementation
        return this;
    }
}

// Utilisation
const userQuery = new QueryBuilder<User>()
    .where('age', 'gt', 18)
    .where('name', 'Alice')
    .orderBy('email', 'asc')
    .select('name', 'email');

// Type result: Pick<User, 'name' | 'email'>[]

// Template literal types avancés
type HttpMethod = 'GET' | 'POST' | 'PUT' | 'DELETE';
type ApiEndpoint = 'users' | 'products' | 'orders';

type ApiRoute = `/${ApiEndpoint}`;
type ApiRouteWithId = `/${ApiEndpoint}/${string}`;
type ApiMethodRoute = `${HttpMethod} ${ApiRoute | ApiRouteWithId}`;

// Résultat: "GET /users" | "POST /users" | "PUT /users/123" etc.

// Recursive types
type JSONValue = 
    | string 
    | number 
    | boolean 
    | null 
    | JSONValue[] 
    | { [key: string]: JSONValue };

interface JSONObject {
    [key: string]: JSONValue;
}

// Path type pour navigation dans les objets
type PathImpl<T, K extends keyof T> = K extends string
    ? T[K] extends Record<string, any>
        ? T[K] extends ArrayLike<any>
            ? K | `${K}.${PathImpl<T[K], Exclude<keyof T[K], keyof any[]>>}`
            : K | `${K}.${PathImpl<T[K], keyof T[K]>}`
        : K
    : never;

type Path<T> = PathImpl<T, keyof T> | keyof T;

// Utilisation pour obtenir une valeur par path
function getByPath<T, P extends Path<T>>(obj: T, path: P): any {
    // Implementation simplifiée
    return path.split('.').reduce((current: any, key: string) => current?.[key], obj);
}

// Test
const config = {
    database: {
        host: 'localhost',
        port: 5432,
        credentials: {
            username: 'admin',
            password: 'secret'
        }
    }
};

const host = getByPath(config, 'database.host');           // ✅ Valid
const username = getByPath(config, 'database.credentials.username'); // ✅ Valid
// const invalid = getByPath(config, 'database.invalid'); // ❌ Type error
```
{% endtab %}
{% endtabs %}

---

## 🎯 Projet Final : Système de Gestion d'État Typé

{% hint style="danger" %}
**Projet Capstone :** Créez un système de gestion d'état (comme Redux) entièrement typé avec TypeScript
{% endhint %}

### Spécifications

Créez un state manager avec :

1. **Store typé** avec state management immutable
2. **Actions typées** avec payload validation
3. **Reducers typés** avec type safety complet
4. **Selectors** avec memoization et types inférés
5. **Middleware system** extensible
6. **DevTools integration** pour debugging
7. **React hooks** optionnels pour intégration

```typescript
// Votre mission : implémenter ce système complet
interface AppState {
    users: {
        items: User[];
        loading: boolean;
        error: string | null;
    };
    products: {
        items: ProductData[];
        categories: string[];
        filters: {
            category: string | null;
            priceRange: [number, number];
        };
    };
    ui: {
        theme: 'light' | 'dark';
        sidebarOpen: boolean;
        notifications: Notification[];
    };
}

// TODO: Définir le système d'actions typées
type AppActions = 
    | { type: 'users/load/start' }
    | { type: 'users/load/success'; payload: User[] }
    | { type: 'users/load/error'; payload: string }
    | { type: 'products/filter'; payload: { category?: string; priceRange?: [number, number] } }
    | { type: 'ui/toggleSidebar' }
    | { type: 'ui/setTheme'; payload: 'light' | 'dark' }
    | { type: 'ui/addNotification'; payload: Notification };

// TODO: Implémenter le store typé
class TypedStore<TState, TAction> {
    constructor(
        private reducer: (state: TState, action: TAction) => TState,
        private initialState: TState,
        private middleware: Middleware<TState, TAction>[] = []
    ) {}
    
    // TODO: Méthodes du store
    getState(): TState {}
    dispatch(action: TAction): TAction {}
    subscribe(listener: () => void): () => void {}
    
    // TODO: Méthodes avancées
    select<TResult>(selector: (state: TState) => TResult): TResult {}
    createSelector<TResult>(selector: (state: TState) => TResult): () => TResult {}
}

// TODO: Système de middleware
interface Middleware<TState, TAction> {
    (store: { getState: () => TState; dispatch: (action: TAction) => TAction }): 
        (next: (action: TAction) => TAction) => 
        (action: TAction) => TAction;
}

// TODO: Reducers typés
type Reducer<TState, TAction> = (state: TState, action: TAction) => TState;

// TODO: Action creators typés
interface ActionCreator<TPayload = void> {
    type: string;
    (payload: TPayload): { type: string; payload: TPayload };
}

// TODO: Selectors avec memoization
class SelectorFactory<TState> {
    createSelector<TResult>(
        selector: (state: TState) => TResult,
        equalityFn?: (a: TResult, b: TResult) => boolean
    ): (state: TState) => TResult {}
    
    createParametricSelector<TParams, TResult>(
        selector: (state: TState, params: TParams) => TResult
    ): (params: TParams) => (state: TState) => TResult {}
}

// Tests et démonstration
async function demonstrationCompleteTypeScript() {
    // TODO: Créer le store avec reducers
    const store = new TypedStore(rootReducer, initialState, [
        loggingMiddleware,
        thunkMiddleware,
        devToolsMiddleware
    ]);
    
    // TODO: Tester les actions typées
    store.dispatch({ type: 'users/load/start' });
    
    // TODO: Tester les selectors
    const users = store.select(state => state.users.items);
    const filteredProducts = store.select(state => 
        state.products.items.filter(p => 
            !state.products.filters.category || 
            p.category === state.products.filters.category
        )
    );
    
    // TODO: Tester le middleware
    store.dispatch(async (dispatch, getState) => {
        dispatch({ type: 'users/load/start' });
        try {
            const users = await fetchUsers();
            dispatch({ type: 'users/load/success', payload: users });
        } catch (error) {
            dispatch({ type: 'users/load/error', payload: error.message });
        }
    });
}
```

### Critères d'évaluation

{% tabs %}
{% tab title="🎯 Fonctionnalités de base" %}
- [ ] **Store typé** avec state immutable
- [ ] **Actions typées** avec validation de payload
- [ ] **Reducers** avec type safety complet
- [ ] **Dispatch** avec types inférés
- [ ] **Selectors** avec memoization
- [ ] **Middleware** system extensible
- [ ] **DevTools** integration
{% endtab %}

{% tab title="⭐ Fonctionnalités avancées" %}
- [ ] **Async actions** avec thunks typés
- [ ] **Optimistic updates** avec rollback
- [ ] **Time travel** debugging
- [ ] **State persistence** avec sérialisation
- [ ] **Performance monitoring** intégré
- [ ] **React hooks** pour intégration
- [ ] **Hot module replacement** support
{% endtab %}

{% tab title="🏆 Excellence TypeScript" %}
- [ ] **Types avancés** (conditional, mapped, template literal)
- [ ] **Génériques** sophistiqués avec contraintes
- [ ] **Type inference** maximisée
- [ ] **Brand types** pour type safety
- [ ] **Documentation** avec TSDoc complet
- [ ] **Tests de types** avec expectType
- [ ] **Performance** des types optimisée
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant TypeScript pour des applications avancées !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Types fondamentaux** et système de types
- **Interfaces et classes** avec héritage et composition
- **Génériques avancés** avec contraintes et inférence
- **Utility types** et type mapping
- **Type guards** et narrowing
- **Conditional types** et template literals
- **Configuration** et tooling TypeScript
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Migrer des projets JavaScript vers TypeScript
- Créer des APIs type-safe et robustes
- Concevoir des architectures typées scalables
- Optimiser la developer experience avec types
- Intégrer TypeScript dans l'écosystème moderne
- Déboguer et maintenir du code typé
{% endtab %}

{% tab title="🚀 Applications réelles" %}
- **Frameworks modernes** : React, Angular, Vue, Node.js
- **APIs robustes** avec validation automatique
- **Bibliothèques** type-safe et réutilisables
- **Applications d'entreprise** maintenables
- **Outils de développement** avec IntelliSense
- **Architectures** microservices typées
{% endtab %}
{% endtabs %}

### Ressources pour aller plus loin

- 📚 [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- 🎥 [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- 📖 [Effective TypeScript](https://effectivetypescript.com/)
- 🛠️ [Type Challenges](https://github.com/type-challenges/type-challenges)

---

**Prêt à créer des applications enterprise avec TypeScript ? Continuez avec les modules de production !** 🏗️
