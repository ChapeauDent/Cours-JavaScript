# Testing en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnees du cours
- **Titre :** Testing et Methodologies de Test (Jest, TDD/BDD, Mocking)
- **Niveau :** Avance
- **Duree estimee :** 120 minutes
- **Prerequis :** Modules, async/await, classes, gestion d'erreurs avancee
- **Date de creation :** 18/07/2025
- **Derniere revision :** 18/07/2025

---

## **OBJECTIFS PEDAGOGIQUES**

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Les principes du testing et son importance en developpement professionnel
- [ ] **Appliquer** : Jest pour ecrire des tests unitaires et d'integration
- [ ] **Implementer** : TDD (Test-Driven Development) et BDD (Behavior-Driven Development)
- [ ] **Maitriser** : Les techniques de mocking et stubbing
- [ ] **Analyser** : La couverture de code et les metriques de qualite
- [ ] **Creer** : Une suite de tests complete pour un projet JavaScript

### Mots-cles a retenir
- **Testing** : Verification automatisee du bon fonctionnement du code
- **Jest** : Framework de test populaire pour JavaScript
- **TDD** : Test-Driven Development (ecrire les tests avant le code)
- **BDD** : Behavior-Driven Development (tests bases sur le comportement)
- **Unit Test** : Test d'une fonction ou composant isole
- **Integration Test** : Test de l'interaction entre plusieurs composants
- **Mock** : Simulation d'une dependance externe
- **Stub** : Remplacement temporaire d'une fonction
- **Coverage** : Pourcentage de code couvert par les tests

---

## **PARTIE THEORIQUE**

### Pourquoi ce concept est-il important ?
Le testing est LA competence qui separe un developpeur amateur d'un developpeur professionnel. Dans l'industrie, **tout code sans tests est considere comme non-fini**.

**Impact professionnel :**
- **Confiance** : Deployer sans peur de casser l'application
- **Maintenance** : Refactoriser en toute securite
- **Documentation** : Les tests montrent comment utiliser ton code
- **Collaboration** : Autres developpeurs comprennent tes intentions
- **Qualite** : Detection precoce des bugs (cout 100x moins cher)

**Statistiques industrie :**
- Projets avec >80% coverage : 40% moins de bugs en production
- TDD reduit les bugs de 60-90% selon Microsoft Research
- Temps debugging reduit de 50% avec une bonne suite de tests

### Definition et concepts cles

**1. Types de tests :**

```javascript
// Test Unitaire - teste une fonction isolee
function additionner(a, b) {
    return a + b;
}

test('additionner deux nombres', () => {
    expect(additionner(2, 3)).toBe(5);
});

// Test d'Integration - teste plusieurs composants ensemble
class Calculatrice {
    constructor(logger) {
        this.logger = logger;
    }
    
    calculer(operation, a, b) {
        let resultat;
        switch(operation) {
            case '+': resultat = a + b; break;
            case '-': resultat = a - b; break;
            default: throw new Error('Operation invalide');
        }
        this.logger.log(`${a} ${operation} ${b} = ${resultat}`);
        return resultat;
    }
}

// Test d'integration complet
test('calculatrice avec logger', () => {
    const mockLogger = { log: jest.fn() };
    const calc = new Calculatrice(mockLogger);
    
    const resultat = calc.calculer('+', 5, 3);
    
    expect(resultat).toBe(8);
    expect(mockLogger.log).toHaveBeenCalledWith('5 + 3 = 8');
});
```

**2. TDD (Test-Driven Development) :**
```javascript
// ETAPE 1: Ecrire le test qui echoue (RED)
test('Utilisateur peut se connecter avec email/password valides', () => {
    const auth = new Authentification();
    const resultat = auth.connecter('user@test.com', 'password123');
    expect(resultat.success).toBe(true);
    expect(resultat.token).toBeDefined();
});

// ETAPE 2: Ecrire le code minimal qui passe (GREEN)
class Authentification {
    connecter(email, password) {
        if (email === 'user@test.com' && password === 'password123') {
            return { success: true, token: 'fake-token' };
        }
        return { success: false };
    }
}

// ETAPE 3: Refactoriser (REFACTOR)
class Authentification {
    constructor(userRepository, tokenGenerator) {
        this.users = userRepository;
        this.tokenGen = tokenGenerator;
    }
    
    connecter(email, password) {
        const user = this.users.findByEmail(email);
        if (user && user.checkPassword(password)) {
            return { 
                success: true, 
                token: this.tokenGen.generate(user.id) 
            };
        }
        return { success: false, error: 'Identifiants invalides' };
    }
}
```

**3. Mocking et Stubbing :**
```javascript
// Mock d'une API externe
const fetchMock = jest.fn();
global.fetch = fetchMock;

class WeatherService {
    async getTemperature(city) {
        const response = await fetch(`/weather/${city}`);
        const data = await response.json();
        return data.temperature;
    }
}

test('recupere la temperature', async () => {
    // Arrange - configurer le mock
    fetchMock.mockResolvedValue({
        json: () => Promise.resolve({ temperature: 22 })
    });
    
    // Act - executer la fonction
    const service = new WeatherService();
    const temp = await service.getTemperature('Paris');
    
    // Assert - verifier le resultat
    expect(temp).toBe(22);
    expect(fetchMock).toHaveBeenCalledWith('/weather/Paris');
});
```

### Syntaxe Jest de base
```javascript
// Installation
// npm install --save-dev jest

// Configuration package.json
{
    "scripts": {
        "test": "jest",
        "test:watch": "jest --watch",
        "test:coverage": "jest --coverage"
    }
}

// Structure de test basique
describe('Groupe de tests', () => {
    beforeAll(() => {
        // Setup une fois avant tous les tests
    });
    
    beforeEach(() => {
        // Setup avant chaque test
    });
    
    afterEach(() => {
        // Nettoyage apres chaque test
    });
    
    afterAll(() => {
        // Nettoyage une fois apres tous les tests
    });
    
    test('description du test', () => {
        // Arrange - preparer
        const input = 'test';
        
        // Act - executer
        const result = functionToTest(input);
        
        // Assert - verifier
        expect(result).toBe('expected');
    });
});
```

### Points d'attention
- **Isolation** : Chaque test doit etre independant
- **Rapidite** : Tests unitaires < 1ms, integration < 100ms
- **Lisibilite** : Nom de test = specification fonctionnelle
- **Coverage** : Viser 80%+ mais qualite > quantite

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// math.js - fonctions a tester
function additionner(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw new Error('Les parametres doivent etre des nombres');
    }
    return a + b;
}

function diviser(a, b) {
    if (b === 0) {
        throw new Error('Division par zero impossible');
    }
    return a / b;
}

function calculerMoyenne(nombres) {
    if (!Array.isArray(nombres) || nombres.length === 0) {
        throw new Error('Tableau non vide requis');
    }
    const somme = nombres.reduce((acc, num) => acc + num, 0);
    return somme / nombres.length;
}

module.exports = { additionner, diviser, calculerMoyenne };

// math.test.js - tests unitaires
const { additionner, diviser, calculerMoyenne } = require('./math');

describe('Fonctions mathematiques', () => {
    describe('additionner', () => {
        test('additionne deux nombres positifs', () => {
            expect(additionner(2, 3)).toBe(5);
        });
        
        test('additionne un nombre positif et negatif', () => {
            expect(additionner(5, -2)).toBe(3);
        });
        
        test('lance une erreur avec des non-nombres', () => {
            expect(() => additionner('a', 2)).toThrow('Les parametres doivent etre des nombres');
            expect(() => additionner(2, null)).toThrow();
        });
    });
    
    describe('diviser', () => {
        test('divise deux nombres', () => {
            expect(diviser(10, 2)).toBe(5);
        });
        
        test('lance une erreur pour division par zero', () => {
            expect(() => diviser(10, 0)).toThrow('Division par zero impossible');
        });
    });
    
    describe('calculerMoyenne', () => {
        test('calcule la moyenne d\'un tableau', () => {
            expect(calculerMoyenne([1, 2, 3, 4, 5])).toBe(3);
        });
        
        test('gere un tableau avec un seul element', () => {
            expect(calculerMoyenne([7])).toBe(7);
        });
        
        test('lance une erreur pour tableau vide', () => {
            expect(() => calculerMoyenne([])).toThrow('Tableau non vide requis');
        });
    });
});
```

**Commandes a executer :**
```bash
npm init -y
npm install --save-dev jest
npm test
```

**Resultat attendu :**
```
 PASS  ./math.test.js
  Fonctions mathematiques
    additionner
      ✓ additionne deux nombres positifs
      ✓ additionne un nombre positif et negatif  
      ✓ lance une erreur avec des non-nombres
    diviser
      ✓ divise deux nombres
      ✓ lance une erreur pour division par zero
    calculerMoyenne
      ✓ calcule la moyenne d'un tableau
      ✓ gere un tableau avec un seul element
      ✓ lance une erreur pour tableau vide

Test Suites: 1 passed, 1 total
Tests:       8 passed, 8 total
```

### Exemple intermediaire (Mocking et API)
```javascript
// userService.js - service avec dependances externes
class UserService {
    constructor(apiClient, cache) {
        this.api = apiClient;
        this.cache = cache;
    }
    
    async getUser(id) {
        // Verifier le cache d'abord
        const cached = this.cache.get(`user:${id}`);
        if (cached) {
            return cached;
        }
        
        try {
            const user = await this.api.get(`/users/${id}`);
            
            // Mettre en cache pour 5 minutes
            this.cache.set(`user:${id}`, user, 300);
            
            return user;
        } catch (error) {
            if (error.status === 404) {
                return null;
            }
            throw error;
        }
    }
    
    async createUser(userData) {
        // Validation basique
        if (!userData.email || !userData.name) {
            throw new Error('Email et nom requis');
        }
        
        const newUser = await this.api.post('/users', userData);
        
        // Invalider le cache des utilisateurs
        this.cache.deletePattern('user:*');
        
        return newUser;
    }
}

module.exports = UserService;

// userService.test.js - tests avec mocks
const UserService = require('./userService');

describe('UserService', () => {
    let userService;
    let mockApiClient;
    let mockCache;
    
    beforeEach(() => {
        // Creer des mocks pour chaque test
        mockApiClient = {
            get: jest.fn(),
            post: jest.fn()
        };
        
        mockCache = {
            get: jest.fn(),
            set: jest.fn(),
            deletePattern: jest.fn()
        };
        
        userService = new UserService(mockApiClient, mockCache);
    });
    
    describe('getUser', () => {
        test('retourne utilisateur depuis le cache si disponible', async () => {
            // Arrange
            const userId = 123;
            const cachedUser = { id: 123, name: 'John', email: 'john@test.com' };
            mockCache.get.mockReturnValue(cachedUser);
            
            // Act
            const result = await userService.getUser(userId);
            
            // Assert
            expect(result).toBe(cachedUser);
            expect(mockCache.get).toHaveBeenCalledWith('user:123');
            expect(mockApiClient.get).not.toHaveBeenCalled();
        });
        
        test('recupere depuis API et met en cache si pas en cache', async () => {
            // Arrange
            const userId = 123;
            const apiUser = { id: 123, name: 'John', email: 'john@test.com' };
            mockCache.get.mockReturnValue(null);
            mockApiClient.get.mockResolvedValue(apiUser);
            
            // Act
            const result = await userService.getUser(userId);
            
            // Assert
            expect(result).toEqual(apiUser);
            expect(mockApiClient.get).toHaveBeenCalledWith('/users/123');
            expect(mockCache.set).toHaveBeenCalledWith('user:123', apiUser, 300);
        });
        
        test('retourne null si utilisateur non trouve', async () => {
            // Arrange
            mockCache.get.mockReturnValue(null);
            const notFoundError = new Error('Not found');
            notFoundError.status = 404;
            mockApiClient.get.mockRejectedValue(notFoundError);
            
            // Act
            const result = await userService.getUser(123);
            
            // Assert
            expect(result).toBeNull();
        });
        
        test('propage autres erreurs API', async () => {
            // Arrange
            mockCache.get.mockReturnValue(null);
            const serverError = new Error('Server error');
            serverError.status = 500;
            mockApiClient.get.mockRejectedValue(serverError);
            
            // Act & Assert
            await expect(userService.getUser(123)).rejects.toThrow('Server error');
        });
    });
    
    describe('createUser', () => {
        test('cree un nouvel utilisateur avec donnees valides', async () => {
            // Arrange
            const userData = { name: 'John', email: 'john@test.com' };
            const createdUser = { id: 123, ...userData };
            mockApiClient.post.mockResolvedValue(createdUser);
            
            // Act
            const result = await userService.createUser(userData);
            
            // Assert
            expect(result).toEqual(createdUser);
            expect(mockApiClient.post).toHaveBeenCalledWith('/users', userData);
            expect(mockCache.deletePattern).toHaveBeenCalledWith('user:*');
        });
        
        test('lance erreur si email manquant', async () => {
            // Arrange
            const userData = { name: 'John' }; // email manquant
            
            // Act & Assert
            await expect(userService.createUser(userData))
                .rejects.toThrow('Email et nom requis');
            expect(mockApiClient.post).not.toHaveBeenCalled();
        });
    });
});
```

### Exemple avance (TDD complet)
```javascript
// Scenario: Developper un gestionnaire de panier e-commerce avec TDD

// ETAPE 1: Ecrire les tests d'abord (ils vont echouer)
// shoppingCart.test.js
describe('ShoppingCart - TDD', () => {
    let cart;
    
    beforeEach(() => {
        cart = new ShoppingCart();
    });
    
    // Test 1: Panier vide au depart
    test('nouveau panier est vide', () => {
        expect(cart.getItemCount()).toBe(0);
        expect(cart.getTotal()).toBe(0);
    });
    
    // Test 2: Ajouter un article
    test('peut ajouter un article', () => {
        cart.addItem('pomme', 2.50, 3);
        expect(cart.getItemCount()).toBe(1);
        expect(cart.getTotal()).toBe(7.50);
    });
    
    // Test 3: Ajouter plusieurs articles
    test('peut ajouter plusieurs articles', () => {
        cart.addItem('pomme', 2.50, 3);
        cart.addItem('banane', 1.20, 2);
        expect(cart.getItemCount()).toBe(2);
        expect(cart.getTotal()).toBe(9.90);
    });
    
    // Test 4: Retirer un article
    test('peut retirer un article', () => {
        cart.addItem('pomme', 2.50, 3);
        cart.removeItem('pomme');
        expect(cart.getItemCount()).toBe(0);
        expect(cart.getTotal()).toBe(0);
    });
    
    // Test 5: Appliquer une remise
    test('peut appliquer une remise pourcentage', () => {
        cart.addItem('pomme', 10.00, 1);
        cart.applyDiscount(20); // 20% de remise
        expect(cart.getTotal()).toBe(8.00);
    });
    
    // Test 6: Gestion des erreurs
    test('lance erreur pour prix negatif', () => {
        expect(() => cart.addItem('pomme', -5, 1))
            .toThrow('Prix doit etre positif');
    });
});

// ETAPE 2: Implementer le code minimal pour passer les tests
// shoppingCart.js
class ShoppingCart {
    constructor() {
        this.items = [];
        this.discountPercent = 0;
    }
    
    addItem(name, price, quantity) {
        if (price < 0) {
            throw new Error('Prix doit etre positif');
        }
        
        this.items.push({
            name,
            price,
            quantity
        });
    }
    
    removeItem(name) {
        this.items = this.items.filter(item => item.name !== name);
    }
    
    getItemCount() {
        return this.items.length;
    }
    
    getSubtotal() {
        return this.items.reduce((total, item) => {
            return total + (item.price * item.quantity);
        }, 0);
    }
    
    applyDiscount(percent) {
        this.discountPercent = percent;
    }
    
    getTotal() {
        const subtotal = this.getSubtotal();
        const discountAmount = subtotal * (this.discountPercent / 100);
        return subtotal - discountAmount;
    }
}

module.exports = ShoppingCart;

// ETAPE 3: Refactoriser et ajouter plus de tests
describe('ShoppingCart - Tests avances', () => {
    let cart;
    
    beforeEach(() => {
        cart = new ShoppingCart();
    });
    
    test('peut modifier la quantite d\'un article existant', () => {
        cart.addItem('pomme', 2.50, 3);
        cart.updateQuantity('pomme', 5);
        expect(cart.getTotal()).toBe(12.50);
    });
    
    test('calcule correctement avec plusieurs remises', () => {
        cart.addItem('pomme', 10.00, 2);
        cart.applyDiscount(10); // 10% de remise
        cart.applyVoucher(5);   // 5€ de bon
        expect(cart.getTotal()).toBe(13.00); // 20 - 2 - 5 = 13
    });
    
    test('peut obtenir le detail des articles', () => {
        cart.addItem('pomme', 2.50, 3);
        cart.addItem('banane', 1.20, 2);
        
        const items = cart.getItemsDetail();
        expect(items).toHaveLength(2);
        expect(items[0]).toEqual({
            name: 'pomme',
            price: 2.50,
            quantity: 3,
            subtotal: 7.50
        });
    });
});

// Version refactorisee avec plus de fonctionnalites
class ShoppingCart {
    constructor() {
        this.items = new Map();
        this.discountPercent = 0;
        this.voucherAmount = 0;
    }
    
    addItem(name, price, quantity) {
        this._validatePrice(price);
        this._validateQuantity(quantity);
        
        if (this.items.has(name)) {
            const existing = this.items.get(name);
            existing.quantity += quantity;
        } else {
            this.items.set(name, { name, price, quantity });
        }
    }
    
    updateQuantity(name, newQuantity) {
        this._validateQuantity(newQuantity);
        
        if (this.items.has(name)) {
            this.items.get(name).quantity = newQuantity;
        }
    }
    
    removeItem(name) {
        this.items.delete(name);
    }
    
    getItemCount() {
        return this.items.size;
    }
    
    getSubtotal() {
        let total = 0;
        for (const item of this.items.values()) {
            total += item.price * item.quantity;
        }
        return total;
    }
    
    applyDiscount(percent) {
        if (percent < 0 || percent > 100) {
            throw new Error('Remise doit etre entre 0 et 100%');
        }
        this.discountPercent = percent;
    }
    
    applyVoucher(amount) {
        if (amount < 0) {
            throw new Error('Bon doit etre positif');
        }
        this.voucherAmount = amount;
    }
    
    getTotal() {
        const subtotal = this.getSubtotal();
        const discountAmount = subtotal * (this.discountPercent / 100);
        const afterDiscount = subtotal - discountAmount;
        return Math.max(0, afterDiscount - this.voucherAmount);
    }
    
    getItemsDetail() {
        const details = [];
        for (const item of this.items.values()) {
            details.push({
                name: item.name,
                price: item.price,
                quantity: item.quantity,
                subtotal: item.price * item.quantity
            });
        }
        return details;
    }
    
    _validatePrice(price) {
        if (typeof price !== 'number' || price < 0) {
            throw new Error('Prix doit etre positif');
        }
    }
    
    _validateQuantity(quantity) {
        if (typeof quantity !== 'number' || quantity <= 0 || !Number.isInteger(quantity)) {
            throw new Error('Quantite doit etre un entier positif');
        }
    }
}
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Tests pour calculatrice
**Consigne :** Cree une suite de tests complete pour une calculatrice qui gere les 4 operations de base.

**Contraintes :**
- Tests unitaires pour chaque operation
- Gestion des cas d'erreur (division par 0, parametres invalides)
- Tests de cas limites (nombres tres grands, decimaux)
- Coverage minimum 90%

**Code de depart :**
```javascript
class Calculatrice {
    additionner(a, b) {
        // A implementer
    }
    
    soustraire(a, b) {
        // A implementer  
    }
    
    multiplier(a, b) {
        // A implementer
    }
    
    diviser(a, b) {
        // A implementer
    }
}
```

**Niveau de difficulte :** 3/5

### Exercice 2 : API Mock et tests asynchrones
**Consigne :** Cree un service de gestion de taches avec tests mocks pour une API REST.

**Fonctionnalites requises :**
- CRUD complet (Create, Read, Update, Delete)
- Gestion des erreurs reseau
- Cache local avec TTL
- Tests avec mocks d'API

**Niveau de difficulte :** 4/5

### Exercice 3 : TDD pour authentification (optionnel)
**Consigne :** Developpe un systeme d'authentification complet en suivant strictement TDD.

**Fonctionnalites :**
- Inscription/connexion
- Validation email/password
- Gestion des tokens JWT
- Rate limiting
- Tests de securite

**Niveau de difficulte :** 5/5

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// calculatrice.js
class Calculatrice {
    additionner(a, b) {
        this._validerNombres(a, b);
        return a + b;
    }
    
    soustraire(a, b) {
        this._validerNombres(a, b);
        return a - b;
    }
    
    multiplier(a, b) {
        this._validerNombres(a, b);
        return a * b;
    }
    
    diviser(a, b) {
        this._validerNombres(a, b);
        if (b === 0) {
            throw new Error('Division par zero impossible');
        }
        return a / b;
    }
    
    _validerNombres(a, b) {
        if (typeof a !== 'number' || typeof b !== 'number') {
            throw new Error('Les parametres doivent etre des nombres');
        }
        if (!Number.isFinite(a) || !Number.isFinite(b)) {
            throw new Error('Les nombres doivent etre finis');
        }
    }
}

module.exports = Calculatrice;

// calculatrice.test.js
const Calculatrice = require('./calculatrice');

describe('Calculatrice', () => {
    let calc;
    
    beforeEach(() => {
        calc = new Calculatrice();
    });
    
    describe('additionner', () => {
        test('additionne deux entiers positifs', () => {
            expect(calc.additionner(2, 3)).toBe(5);
        });
        
        test('additionne nombres decimaux', () => {
            expect(calc.additionner(1.5, 2.3)).toBeCloseTo(3.8);
        });
        
        test('additionne nombres negatifs', () => {
            expect(calc.additionner(-5, -3)).toBe(-8);
            expect(calc.additionner(-5, 3)).toBe(-2);
        });
        
        test('additionne zero', () => {
            expect(calc.additionner(5, 0)).toBe(5);
            expect(calc.additionner(0, 0)).toBe(0);
        });
        
        test('gere les grands nombres', () => {
            expect(calc.additionner(Number.MAX_SAFE_INTEGER, 1))
                .toBe(Number.MAX_SAFE_INTEGER + 1);
        });
        
        test('lance erreur avec non-nombres', () => {
            expect(() => calc.additionner('5', 3)).toThrow('Les parametres doivent etre des nombres');
            expect(() => calc.additionner(5, null)).toThrow();
            expect(() => calc.additionner(undefined, 5)).toThrow();
        });
        
        test('lance erreur avec nombres non-finis', () => {
            expect(() => calc.additionner(Infinity, 5)).toThrow('Les nombres doivent etre finis');
            expect(() => calc.additionner(5, NaN)).toThrow();
        });
    });
    
    describe('diviser', () => {
        test('divise deux nombres', () => {
            expect(calc.diviser(10, 2)).toBe(5);
            expect(calc.diviser(7, 2)).toBe(3.5);
        });
        
        test('lance erreur pour division par zero', () => {
            expect(() => calc.diviser(10, 0)).toThrow('Division par zero impossible');
            expect(() => calc.diviser(-10, 0)).toThrow();
        });
        
        test('divise par nombre negatif', () => {
            expect(calc.diviser(10, -2)).toBe(-5);
        });
        
        test('divise zero par nombre', () => {
            expect(calc.diviser(0, 5)).toBe(0);
        });
    });
    
    // Tests similaires pour soustraire et multiplier...
});

// Pour verifier la coverage:
// npm test -- --coverage
```

**Points cles :**
- Tests exhaustifs des cas normaux et limites
- Validation stricte des entrees
- Messages d'erreur clairs
- Utilisation de `toBeCloseTo()` pour les decimaux

### Solution Exercice 2
```javascript
// taskService.js
class TaskService {
    constructor(apiClient, cache) {
        this.api = apiClient;
        this.cache = cache;
        this.CACHE_TTL = 300; // 5 minutes
    }
    
    async getAllTasks() {
        const cacheKey = 'tasks:all';
        const cached = this.cache.get(cacheKey);
        
        if (cached) {
            return cached;
        }
        
        try {
            const tasks = await this.api.get('/tasks');
            this.cache.set(cacheKey, tasks, this.CACHE_TTL);
            return tasks;
        } catch (error) {
            throw this._handleApiError(error);
        }
    }
    
    async getTask(id) {
        const cacheKey = `task:${id}`;
        const cached = this.cache.get(cacheKey);
        
        if (cached) {
            return cached;
        }
        
        try {
            const task = await this.api.get(`/tasks/${id}`);
            this.cache.set(cacheKey, task, this.CACHE_TTL);
            return task;
        } catch (error) {
            if (error.status === 404) {
                return null;
            }
            throw this._handleApiError(error);
        }
    }
    
    async createTask(taskData) {
        this._validateTaskData(taskData);
        
        try {
            const newTask = await this.api.post('/tasks', taskData);
            this._invalidateListCache();
            return newTask;
        } catch (error) {
            throw this._handleApiError(error);
        }
    }
    
    async updateTask(id, updates) {
        this._validateTaskData(updates, false);
        
        try {
            const updatedTask = await this.api.put(`/tasks/${id}`, updates);
            this.cache.delete(`task:${id}`);
            this._invalidateListCache();
            return updatedTask;
        } catch (error) {
            throw this._handleApiError(error);
        }
    }
    
    async deleteTask(id) {
        try {
            await this.api.delete(`/tasks/${id}`);
            this.cache.delete(`task:${id}`);
            this._invalidateListCache();
            return true;
        } catch (error) {
            if (error.status === 404) {
                return false;
            }
            throw this._handleApiError(error);
        }
    }
    
    _validateTaskData(data, requireAll = true) {
        if (requireAll && !data.title) {
            throw new Error('Titre requis');
        }
        if (data.title && typeof data.title !== 'string') {
            throw new Error('Titre doit etre une chaine');
        }
        if (data.completed !== undefined && typeof data.completed !== 'boolean') {
            throw new Error('Completed doit etre un boolean');
        }
    }
    
    _handleApiError(error) {
        if (error.status >= 500) {
            return new Error('Erreur serveur, reessayez plus tard');
        }
        if (error.status === 400) {
            return new Error('Donnees invalides');
        }
        return error;
    }
    
    _invalidateListCache() {
        this.cache.delete('tasks:all');
    }
}

module.exports = TaskService;

// taskService.test.js
const TaskService = require('./taskService');

describe('TaskService', () => {
    let service;
    let mockApi;
    let mockCache;
    
    beforeEach(() => {
        mockApi = {
            get: jest.fn(),
            post: jest.fn(),
            put: jest.fn(),
            delete: jest.fn()
        };
        
        mockCache = {
            get: jest.fn(),
            set: jest.fn(),
            delete: jest.fn()
        };
        
        service = new TaskService(mockApi, mockCache);
    });
    
    describe('getAllTasks', () => {
        test('retourne tasks depuis cache si disponible', async () => {
            const cachedTasks = [{ id: 1, title: 'Task 1' }];
            mockCache.get.mockReturnValue(cachedTasks);
            
            const result = await service.getAllTasks();
            
            expect(result).toBe(cachedTasks);
            expect(mockCache.get).toHaveBeenCalledWith('tasks:all');
            expect(mockApi.get).not.toHaveBeenCalled();
        });
        
        test('recupere depuis API et met en cache', async () => {
            const apiTasks = [{ id: 1, title: 'Task 1' }];
            mockCache.get.mockReturnValue(null);
            mockApi.get.mockResolvedValue(apiTasks);
            
            const result = await service.getAllTasks();
            
            expect(result).toBe(apiTasks);
            expect(mockApi.get).toHaveBeenCalledWith('/tasks');
            expect(mockCache.set).toHaveBeenCalledWith('tasks:all', apiTasks, 300);
        });
        
        test('gere erreurs API correctement', async () => {
            mockCache.get.mockReturnValue(null);
            const serverError = new Error('Server Error');
            serverError.status = 500;
            mockApi.get.mockRejectedValue(serverError);
            
            await expect(service.getAllTasks())
                .rejects.toThrow('Erreur serveur, reessayez plus tard');
        });
    });
    
    describe('createTask', () => {
        test('cree une nouvelle tache', async () => {
            const taskData = { title: 'New Task', completed: false };
            const createdTask = { id: 1, ...taskData };
            mockApi.post.mockResolvedValue(createdTask);
            
            const result = await service.createTask(taskData);
            
            expect(result).toBe(createdTask);
            expect(mockApi.post).toHaveBeenCalledWith('/tasks', taskData);
            expect(mockCache.delete).toHaveBeenCalledWith('tasks:all');
        });
        
        test('valide les donnees requises', async () => {
            await expect(service.createTask({}))
                .rejects.toThrow('Titre requis');
            
            await expect(service.createTask({ title: 123 }))
                .rejects.toThrow('Titre doit etre une chaine');
        });
    });
    
    describe('deleteTask', () => {
        test('supprime une tache existante', async () => {
            mockApi.delete.mockResolvedValue();
            
            const result = await service.deleteTask(1);
            
            expect(result).toBe(true);
            expect(mockApi.delete).toHaveBeenCalledWith('/tasks/1');
            expect(mockCache.delete).toHaveBeenCalledWith('task:1');
            expect(mockCache.delete).toHaveBeenCalledWith('tasks:all');
        });
        
        test('retourne false si tache non trouvee', async () => {
            const notFoundError = new Error('Not found');
            notFoundError.status = 404;
            mockApi.delete.mockRejectedValue(notFoundError);
            
            const result = await service.deleteTask(999);
            
            expect(result).toBe(false);
        });
    });
});
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts prealables utilises
- **Modules ES6** : Import/export des tests et code
- **Async/await** : Tests de fonctions asynchrones
- **Classes** : Structure orientee objet testable
- **Gestion d'erreurs** : Verification des exceptions
- **Promises** : Tests d'operations asynchrones

### Prochaines etapes
- **CI/CD** : Integration des tests dans le pipeline
- **E2E Testing** : Tests bout-en-bout avec Cypress/Playwright
- **Performance Testing** : Tests de charge et performance
- **Visual Testing** : Tests de regression visuelle

### Concepts complementaires
- **Code Coverage** : Mesurer la qualite des tests
- **Mutation Testing** : Tester la qualite des tests eux-memes
- **Property-based Testing** : Tests avec donnees generees aleatoirement

---

## **PROJET MINI**

### Defi pratique
**Objectif :** Creer une API de blog avec suite de tests complete

**Fonctionnalites requises :**
- [ ] CRUD articles (titre, contenu, auteur, date)
- [ ] Systeme de commentaires
- [ ] Authentification basique  
- [ ] Tests unitaires (>90% coverage)
- [ ] Tests d'integration API
- [ ] Mocks pour base de donnees
- [ ] Tests de securite (injection, validation)
- [ ] Documentation des tests

**Structure suggeree :**
```
📁 blog-api-tested/
├── 📁 src/
│   ├── 📄 article.js
│   ├── 📄 comment.js
│   ├── 📄 auth.js
│   └── 📄 database.js
├── 📁 tests/
│   ├── 📁 unit/
│   ├── 📁 integration/
│   └── 📁 mocks/
├── 📄 package.json
├── 📄 jest.config.js
└── 📄 README.md
```

**Temps estime :** 3-4 heures

---

## **RESSOURCES COMPLEMENTAIRES**

### Documentation officielle
- [Jest Documentation](https://jestjs.io/docs/getting-started) - Framework de test complet
- [Testing Library](https://testing-library.com/) - Outils pour tester l'UI
- [Node.js Testing Guide](https://nodejs.org/en/docs/guides/testing/) - Bonnes pratiques Node

### Videos recommandees  
- "JavaScript Testing Introduction" - Academind (45 min)
- "TDD in JavaScript" - Fun Fun Function (30 min)
- Pourquoi utiles : Approche visuelle du TDD et bonnes pratiques

### Articles approfondis
- "Testing Best Practices" - JavaScript Testing Best Practices (GitHub)
- "Mocking Strategies" - Martin Fowler
- Ce qu'ils apportent : Strategies avancees et patterns professionnels

### Outils de pratique
- [Jest Online IDE](https://repl.it/@amasad/jest-test-runner) - Tester Jest en ligne
- [Testing Playground](https://testing-playground.com/) - Pratique des selecteurs
- [Kata Testing](http://codingdojo.org/kata/) - Exercices TDD

---

## **AUTO-EVALUATION**

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre un test unitaire et un test d'integration ?
   - Reponse : Test unitaire = fonction isolee, test integration = interaction entre composants

2. **Question pratique :** Comment mocker une dependance externe dans Jest ?
   ```javascript
   const mockFn = jest.fn();
   // ou 
   jest.mock('./module', () => ({ function: jest.fn() }));
   ```

3. **Question TDD :** Quelles sont les 3 etapes du cycle TDD ?
   - Reponse : Red (test qui echoue), Green (code minimal), Refactor (ameliorer)

### Checklist de maitrise
- [ ] Je peux ecrire des tests unitaires avec Jest
- [ ] Je comprends les techniques de mocking
- [ ] Je sais appliquer le cycle TDD
- [ ] Je peux tester du code asynchrone
- [ ] Je gere les assertions et matchers Jest
- [ ] Je configure et organise une suite de tests
- [ ] Je comprends l'importance de la coverage
- [ ] Je peux debugger des tests qui echouent

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur frequente 1
**Code problematique :**
```javascript
test('fonction asynchrone', () => {
    asyncFunction().then(result => {
        expect(result).toBe('expected');
    });
});
```

**Message d'erreur :** Test passe meme si l'assertion echoue

**Solution :**
```javascript
test('fonction asynchrone', async () => {
    const result = await asyncFunction();
    expect(result).toBe('expected');
});

// ou avec return
test('fonction asynchrone', () => {
    return asyncFunction().then(result => {
        expect(result).toBe('expected');
    });
});
```

**Explication :** Jest doit attendre la fin de la promise pour evaluer les assertions.

### Erreur frequente 2
**Code problematique :**
```javascript
const mockFn = jest.fn();
// ... tests
expect(mockFn).toHaveBeenCalledWith('param');
```

**Message d'erreur :** Mock function expected to have been called with...

**Solution :**
```javascript
beforeEach(() => {
    jest.clearAllMocks();
    // ou mockFn.mockClear();
});

test('verification appel mock', () => {
    mockFn('param');
    expect(mockFn).toHaveBeenCalledWith('param');
    expect(mockFn).toHaveBeenCalledTimes(1);
});
```

**Explication :** Les mocks gardent leur historique entre les tests, il faut les nettoyer.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Importance cruciale du testing en developpement professionnel
- Techniques de mocking pour isoler les dependances
- Cycle TDD pour un code plus robuste

### Questions a approfondir
- Testing de performance et benchmarks
- Tests E2E avec Cypress
- Testing de securite automatise

### Revision programmee
- [ ] Revision dans 1 jour (concepts cles)
- [ ] Revision dans 1 semaine (pratique TDD)
- [ ] Revision dans 1 mois (projet complet avec tests)

---

## **TAGS ET METADONNEES**

**Mots-cles :** #javascript #testing #jest #tdd #bdd #mocking #avance #professionnel

**Temps de completion :** [Temps reellement passe]

**Difficulte ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Pret pour le concept suivant :** ✅ Oui / ❌ Non (plus de pratique necessaire)

---

*📅 Fiche creee le : 18/07/2025*
*🔄 Derniere revision : 18/07/2025*  
*📍 Position dans la roadmap : 16.5/126*
