# 🧪 Testing JavaScript Avancé

> **Niveau :** 🔴 Avancé | **Durée :** 120 minutes | **Prérequis :** Modules, Async/Await, Classes

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** Jest et les frameworks de test modernes
- [ ] **Implémenter** TDD/BDD pour un développement robuste
- [ ] **Créer** des mocks et stubs sophistiqués
- [ ] **Analyser** la couverture de code et les métriques
- [ ] **Optimiser** les performances de tests

---

## 🎭 Jest et l'Écosystème de Test

{% tabs %}
{% tab title="⚡ Jest Foundation" %}
**Jest : Le framework de test JavaScript de référence**

### Configuration Jest professionnelle
```javascript
// jest.config.js - Configuration production
module.exports = {
  // Environnement de test
  testEnvironment: 'node', // ou 'jsdom' pour le navigateur
  
  // Patterns de fichiers
  testMatch: [
    '**/__tests__/**/*.(js|jsx|ts|tsx)',
    '**/*.(test|spec).(js|jsx|ts|tsx)'
  ],
  
  // Couverture de code
  collectCoverage: true,
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov', 'html'],
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
    '!src/index.js'
  ],
  
  // Seuils de couverture
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  },
  
  // Setup et teardown
  setupFilesAfterEnv: ['<rootDir>/src/setupTests.js'],
  
  // Transformations
  transform: {
    '^.+\\.(js|jsx|ts|tsx)$': 'babel-jest'
  },
  
  // Mocks automatiques
  clearMocks: true,
  restoreMocks: true,
  
  // Performance
  maxWorkers: '50%',
  cacheDirectory: '.jest-cache'
};
```

### Tests unitaires sophistiqués
```javascript
// src/utils/math.js
export class Calculator {
  constructor(precision = 2) {
    this.precision = precision;
    this.history = [];
  }
  
  add(a, b) {
    const result = this.round(a + b);
    this.history.push({ operation: 'add', a, b, result });
    return result;
  }
  
  divide(a, b) {
    if (b === 0) {
      throw new Error('Division by zero');
    }
    const result = this.round(a / b);
    this.history.push({ operation: 'divide', a, b, result });
    return result;
  }
  
  round(value) {
    return Math.round(value * Math.pow(10, this.precision)) / Math.pow(10, this.precision);
  }
  
  getHistory() {
    return [...this.history];
  }
  
  clear() {
    this.history = [];
  }
}

// src/utils/__tests__/math.test.js
import { Calculator } from '../math';

describe('Calculator', () => {
  let calculator;
  
  beforeEach(() => {
    calculator = new Calculator();
  });
  
  afterEach(() => {
    calculator.clear();
  });
  
  describe('Addition', () => {
    test('should add two positive numbers', () => {
      const result = calculator.add(2, 3);
      expect(result).toBe(5);
    });
    
    test('should handle decimal precision', () => {
      const calc = new Calculator(3);
      const result = calc.add(0.1, 0.2);
      expect(result).toBe(0.3); // Sans Jest, ce serait 0.30000000000000004
    });
    
    test('should record operation in history', () => {
      calculator.add(5, 3);
      const history = calculator.getHistory();
      
      expect(history).toHaveLength(1);
      expect(history[0]).toEqual({
        operation: 'add',
        a: 5,
        b: 3,
        result: 8
      });
    });
  });
  
  describe('Division', () => {
    test('should divide two numbers', () => {
      const result = calculator.divide(10, 2);
      expect(result).toBe(5);
    });
    
    test('should throw error when dividing by zero', () => {
      expect(() => {
        calculator.divide(10, 0);
      }).toThrow('Division by zero');
    });
    
    test('should throw specific error type', () => {
      expect(() => {
        calculator.divide(10, 0);
      }).toThrow(Error);
    });
  });
  
  describe('Parameterized tests', () => {
    test.each([
      [1, 1, 2],
      [2, 2, 4],
      [3, 4, 7],
      [-1, 1, 0],
      [0.1, 0.1, 0.2]
    ])('add(%i, %i) should return %i', (a, b, expected) => {
      expect(calculator.add(a, b)).toBe(expected);
    });
  });
  
  describe('Snapshot testing', () => {
    test('history format should match snapshot', () => {
      calculator.add(1, 2);
      calculator.divide(6, 2);
      
      expect(calculator.getHistory()).toMatchSnapshot();
    });
  });
});
```

### Matchers personnalisés
```javascript
// src/setupTests.js
expect.extend({
  toBeWithinRange(received, floor, ceiling) {
    const pass = received >= floor && received <= ceiling;
    if (pass) {
      return {
        message: () =>
          `expected ${received} not to be within range ${floor} - ${ceiling}`,
        pass: true,
      };
    } else {
      return {
        message: () =>
          `expected ${received} to be within range ${floor} - ${ceiling}`,
        pass: false,
      };
    }
  },
  
  toBeValidEmail(received) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    const pass = emailRegex.test(received);
    
    return {
      message: () => `expected ${received} ${pass ? 'not ' : ''}to be a valid email`,
      pass
    };
  },
  
  toHaveBeenCalledWithError(received, errorMessage) {
    const pass = received.mock.calls.some(call => 
      call[0] instanceof Error && call[0].message === errorMessage
    );
    
    return {
      message: () => `expected function ${pass ? 'not ' : ''}to have been called with error: ${errorMessage}`,
      pass
    };
  }
});

// Usage dans les tests
test('custom matchers', () => {
  expect(15).toBeWithinRange(10, 20);
  expect('user@example.com').toBeValidEmail();
  
  const mockFn = jest.fn();
  mockFn(new Error('Something went wrong'));
  expect(mockFn).toHaveBeenCalledWithError('Something went wrong');
});
```
{% endtab %}

{% tab title="🏗️ TDD & BDD Pratiques" %}
**Test-Driven Development et Behavior-Driven Development**

### TDD : Red-Green-Refactor
```javascript
// 1. RED : Écrire un test qui échoue
describe('UserValidator', () => {
  test('should validate email format', () => {
    const validator = new UserValidator();
    expect(validator.isValidEmail('test@example.com')).toBe(true);
    expect(validator.isValidEmail('invalid-email')).toBe(false);
  });
});

// 2. GREEN : Écrire le code minimal pour passer
class UserValidator {
  isValidEmail(email) {
    return email.includes('@') && email.includes('.');
  }
}

// 3. REFACTOR : Améliorer l'implémentation
class UserValidator {
  constructor() {
    this.emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  }
  
  isValidEmail(email) {
    if (typeof email !== 'string') return false;
    return this.emailRegex.test(email.trim());
  }
}

// Cycle TDD complet pour une fonctionnalité
describe('Password Validator TDD', () => {
  let validator;
  
  beforeEach(() => {
    validator = new PasswordValidator();
  });
  
  // Test 1 : Longueur minimale
  test('should reject passwords shorter than 8 characters', () => {
    expect(validator.isValid('1234567')).toBe(false);
    expect(validator.isValid('12345678')).toBe(true);
  });
  
  // Test 2 : Caractères obligatoires
  test('should require at least one uppercase letter', () => {
    expect(validator.isValid('password123')).toBe(false);
    expect(validator.isValid('Password123')).toBe(true);
  });
  
  test('should require at least one number', () => {
    expect(validator.isValid('Password')).toBe(false);
    expect(validator.isValid('Password1')).toBe(true);
  });
  
  // Test 3 : Validation complète
  test('should validate complex password requirements', () => {
    const validPassword = 'StrongPass123!';
    const result = validator.validate(validPassword);
    
    expect(result.isValid).toBe(true);
    expect(result.errors).toHaveLength(0);
  });
  
  test('should return detailed validation errors', () => {
    const weakPassword = 'weak';
    const result = validator.validate(weakPassword);
    
    expect(result.isValid).toBe(false);
    expect(result.errors).toContain('Password must be at least 8 characters');
    expect(result.errors).toContain('Password must contain an uppercase letter');
    expect(result.errors).toContain('Password must contain a number');
  });
});

// Implémentation finale après TDD
class PasswordValidator {
  constructor() {
    this.minLength = 8;
    this.requirements = [
      {
        regex: /[A-Z]/,
        message: 'Password must contain an uppercase letter'
      },
      {
        regex: /[a-z]/,
        message: 'Password must contain a lowercase letter'
      },
      {
        regex: /\d/,
        message: 'Password must contain a number'
      },
      {
        regex: /[!@#$%^&*(),.?":{}|<>]/,
        message: 'Password must contain a special character'
      }
    ];
  }
  
  isValid(password) {
    return this.validate(password).isValid;
  }
  
  validate(password) {
    const errors = [];
    
    if (!password || password.length < this.minLength) {
      errors.push(`Password must be at least ${this.minLength} characters`);
    }
    
    this.requirements.forEach(requirement => {
      if (!requirement.regex.test(password)) {
        errors.push(requirement.message);
      }
    });
    
    return {
      isValid: errors.length === 0,
      errors
    };
  }
}
```

### BDD avec Jest et Cucumber-like syntax
```javascript
// BDD Style Tests
describe('E-commerce Shopping Cart', () => {
  let cart;
  let product;
  
  beforeEach(() => {
    cart = new ShoppingCart();
    product = {
      id: '123',
      name: 'Laptop',
      price: 999.99,
      stock: 5
    };
  });
  
  // Given-When-Then pattern
  describe('Given an empty cart', () => {
    test('When I add a product, Then cart should contain the product', () => {
      // Given - cart is empty (beforeEach)
      expect(cart.getItems()).toHaveLength(0);
      
      // When - add product
      cart.addItem(product, 1);
      
      // Then - cart contains product
      expect(cart.getItems()).toHaveLength(1);
      expect(cart.getItems()[0]).toMatchObject({
        product,
        quantity: 1
      });
    });
    
    test('When I add the same product twice, Then quantity should increase', () => {
      // When
      cart.addItem(product, 1);
      cart.addItem(product, 2);
      
      // Then
      expect(cart.getItems()).toHaveLength(1);
      expect(cart.getItems()[0].quantity).toBe(3);
    });
  });
  
  describe('Given a cart with products', () => {
    beforeEach(() => {
      cart.addItem(product, 2);
    });
    
    test('When I remove a product, Then cart should be updated', () => {
      // When
      cart.removeItem(product.id);
      
      // Then
      expect(cart.getItems()).toHaveLength(0);
    });
    
    test('When I calculate total, Then it should include tax', () => {
      // When
      const total = cart.getTotal();
      
      // Then
      const expectedSubtotal = product.price * 2;
      const expectedTax = expectedSubtotal * 0.08; // 8% tax
      const expectedTotal = expectedSubtotal + expectedTax;
      
      expect(total).toBeCloseTo(expectedTotal, 2);
    });
  });
  
  describe('Given insufficient stock', () => {
    test('When I try to add more than available, Then it should throw error', () => {
      // When & Then
      expect(() => {
        cart.addItem(product, 10); // stock is 5
      }).toThrow('Insufficient stock');
    });
  });
});

// Feature-based organization
describe('Feature: User Authentication', () => {
  let authService;
  let userRepository;
  
  beforeEach(() => {
    userRepository = new MockUserRepository();
    authService = new AuthService(userRepository);
  });
  
  describe('Scenario: Successful login', () => {
    test('Given valid credentials, When user logs in, Then token is returned', async () => {
      // Given
      const credentials = { email: 'user@test.com', password: 'validpass' };
      userRepository.addUser({
        email: 'user@test.com',
        hashedPassword: await hashPassword('validpass')
      });
      
      // When
      const result = await authService.login(credentials);
      
      // Then
      expect(result.success).toBe(true);
      expect(result.token).toBeDefined();
      expect(result.user.email).toBe('user@test.com');
    });
  });
  
  describe('Scenario: Failed login', () => {
    test('Given invalid password, When user logs in, Then error is returned', async () => {
      // Given
      const credentials = { email: 'user@test.com', password: 'wrongpass' };
      userRepository.addUser({
        email: 'user@test.com',
        hashedPassword: await hashPassword('correctpass')
      });
      
      // When
      const result = await authService.login(credentials);
      
      // Then
      expect(result.success).toBe(false);
      expect(result.error).toBe('Invalid credentials');
      expect(result.token).toBeUndefined();
    });
  });
});
```

### Test doubles (Mocks, Stubs, Spies)
```javascript
// Service avec dépendances externes
class NotificationService {
  constructor(emailProvider, smsProvider, database) {
    this.emailProvider = emailProvider;
    this.smsProvider = smsProvider;
    this.database = database;
  }
  
  async sendWelcomeNotification(user) {
    try {
      // Save notification record
      const notification = await this.database.saveNotification({
        userId: user.id,
        type: 'welcome',
        timestamp: new Date()
      });
      
      // Send email
      await this.emailProvider.send({
        to: user.email,
        subject: 'Welcome!',
        template: 'welcome',
        data: { name: user.name }
      });
      
      // Send SMS if phone provided
      if (user.phone) {
        await this.smsProvider.send({
          to: user.phone,
          message: `Welcome ${user.name}!`
        });
      }
      
      // Update notification status
      await this.database.updateNotification(notification.id, {
        status: 'sent'
      });
      
      return { success: true, notificationId: notification.id };
      
    } catch (error) {
      console.error('Notification failed:', error);
      return { success: false, error: error.message };
    }
  }
}

// Tests avec différents types de test doubles
describe('NotificationService', () => {
  let notificationService;
  let mockEmailProvider;
  let mockSmsProvider;
  let mockDatabase;
  
  beforeEach(() => {
    // Mocks complets avec Jest
    mockEmailProvider = {
      send: jest.fn().mockResolvedValue({ messageId: 'email123' })
    };
    
    mockSmsProvider = {
      send: jest.fn().mockResolvedValue({ messageId: 'sms456' })
    };
    
    mockDatabase = {
      saveNotification: jest.fn().mockResolvedValue({ id: 'notif789' }),
      updateNotification: jest.fn().mockResolvedValue(true)
    };
    
    notificationService = new NotificationService(
      mockEmailProvider,
      mockSmsProvider,
      mockDatabase
    );
  });
  
  test('should send complete welcome notification', async () => {
    // Arrange
    const user = {
      id: 'user123',
      name: 'John Doe',
      email: 'john@example.com',
      phone: '+1234567890'
    };
    
    // Act
    const result = await notificationService.sendWelcomeNotification(user);
    
    // Assert
    expect(result.success).toBe(true);
    expect(result.notificationId).toBe('notif789');
    
    // Verify email was sent
    expect(mockEmailProvider.send).toHaveBeenCalledWith({
      to: 'john@example.com',
      subject: 'Welcome!',
      template: 'welcome',
      data: { name: 'John Doe' }
    });
    
    // Verify SMS was sent
    expect(mockSmsProvider.send).toHaveBeenCalledWith({
      to: '+1234567890',
      message: 'Welcome John Doe!'
    });
    
    // Verify database operations
    expect(mockDatabase.saveNotification).toHaveBeenCalledWith({
      userId: 'user123',
      type: 'welcome',
      timestamp: expect.any(Date)
    });
    
    expect(mockDatabase.updateNotification).toHaveBeenCalledWith('notif789', {
      status: 'sent'
    });
  });
  
  test('should handle email failure gracefully', async () => {
    // Arrange
    const user = { id: 'user123', name: 'John', email: 'john@example.com' };
    mockEmailProvider.send.mockRejectedValue(new Error('Email service down'));
    
    // Act
    const result = await notificationService.sendWelcomeNotification(user);
    
    // Assert
    expect(result.success).toBe(false);
    expect(result.error).toBe('Email service down');
    
    // Verify database was still called
    expect(mockDatabase.saveNotification).toHaveBeenCalled();
    
    // Verify update was not called due to error
    expect(mockDatabase.updateNotification).not.toHaveBeenCalled();
  });
  
  test('should skip SMS for users without phone', async () => {
    // Arrange
    const user = {
      id: 'user123',
      name: 'John Doe',
      email: 'john@example.com'
      // No phone number
    };
    
    // Act
    await notificationService.sendWelcomeNotification(user);
    
    // Assert
    expect(mockEmailProvider.send).toHaveBeenCalled();
    expect(mockSmsProvider.send).not.toHaveBeenCalled();
  });
});
```
{% endtab %}

{% tab title="🎮 Tests Asynchrones Avancés" %}
**Testing du code asynchrone avec Jest**

### Promises et Async/Await
```javascript
// Service API avec retry logic
class ApiService {
  constructor(baseURL, retryCount = 3) {
    this.baseURL = baseURL;
    this.retryCount = retryCount;
  }
  
  async fetchWithRetry(endpoint, options = {}) {
    let lastError;
    
    for (let attempt = 1; attempt <= this.retryCount; attempt++) {
      try {
        const response = await fetch(`${this.baseURL}${endpoint}`, {
          ...options,
          headers: {
            'Content-Type': 'application/json',
            ...options.headers
          }
        });
        
        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        return await response.json();
        
      } catch (error) {
        lastError = error;
        
        if (attempt < this.retryCount) {
          await this.delay(1000 * attempt); // Exponential backoff
        }
      }
    }
    
    throw new Error(`Failed after ${this.retryCount} attempts: ${lastError.message}`);
  }
  
  delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
  
  async createUser(userData) {
    return this.fetchWithRetry('/users', {
      method: 'POST',
      body: JSON.stringify(userData)
    });
  }
  
  async getUser(id) {
    return this.fetchWithRetry(`/users/${id}`);
  }
}

// Tests avec mocks sophistiqués
describe('ApiService', () => {
  let apiService;
  let fetchMock;
  
  beforeEach(() => {
    // Mock global fetch
    global.fetch = jest.fn();
    fetchMock = global.fetch;
    
    apiService = new ApiService('https://api.example.com');
    
    // Mock console pour éviter les logs pendant les tests
    jest.spyOn(console, 'error').mockImplementation(() => {});
  });
  
  afterEach(() => {
    jest.restoreAllMocks();
  });
  
  describe('Successful requests', () => {
    test('should fetch user data successfully', async () => {
      // Arrange
      const mockUser = { id: 1, name: 'John Doe' };
      fetchMock.mockResolvedValueOnce({
        ok: true,
        json: async () => mockUser
      });
      
      // Act
      const result = await apiService.getUser(1);
      
      // Assert
      expect(result).toEqual(mockUser);
      expect(fetchMock).toHaveBeenCalledWith(
        'https://api.example.com/users/1',
        expect.objectContaining({
          headers: { 'Content-Type': 'application/json' }
        })
      );
    });
    
    test('should create user with correct payload', async () => {
      // Arrange
      const userData = { name: 'Jane Doe', email: 'jane@example.com' };
      const mockResponse = { id: 2, ...userData };
      
      fetchMock.mockResolvedValueOnce({
        ok: true,
        json: async () => mockResponse
      });
      
      // Act
      const result = await apiService.createUser(userData);
      
      // Assert
      expect(result).toEqual(mockResponse);
      expect(fetchMock).toHaveBeenCalledWith(
        'https://api.example.com/users',
        expect.objectContaining({
          method: 'POST',
          body: JSON.stringify(userData)
        })
      );
    });
  });
  
  describe('Retry mechanism', () => {
    test('should retry on failure and eventually succeed', async () => {
      // Arrange
      const mockUser = { id: 1, name: 'John Doe' };
      
      fetchMock
        .mockRejectedValueOnce(new Error('Network error'))
        .mockRejectedValueOnce(new Error('Timeout'))
        .mockResolvedValueOnce({
          ok: true,
          json: async () => mockUser
        });
      
      // Mock delay to speed up tests
      jest.spyOn(apiService, 'delay').mockResolvedValue();
      
      // Act
      const result = await apiService.getUser(1);
      
      // Assert
      expect(result).toEqual(mockUser);
      expect(fetchMock).toHaveBeenCalledTimes(3);
      expect(apiService.delay).toHaveBeenCalledTimes(2);
    });
    
    test('should fail after max retry attempts', async () => {
      // Arrange
      fetchMock.mockRejectedValue(new Error('Persistent error'));
      jest.spyOn(apiService, 'delay').mockResolvedValue();
      
      // Act & Assert
      await expect(apiService.getUser(1)).rejects.toThrow(
        'Failed after 3 attempts: Persistent error'
      );
      
      expect(fetchMock).toHaveBeenCalledTimes(3);
      expect(apiService.delay).toHaveBeenCalledTimes(2);
    });
    
    test('should use exponential backoff', async () => {
      // Arrange
      fetchMock.mockRejectedValue(new Error('Error'));
      const delaySpy = jest.spyOn(apiService, 'delay').mockResolvedValue();
      
      // Act
      try {
        await apiService.getUser(1);
      } catch (error) {
        // Expected to fail
      }
      
      // Assert backoff timing
      expect(delaySpy).toHaveBeenNthCalledWith(1, 1000); // 1st retry: 1s
      expect(delaySpy).toHaveBeenNthCalledWith(2, 2000); // 2nd retry: 2s
    });
  });
  
  describe('HTTP error handling', () => {
    test('should handle 404 errors', async () => {
      // Arrange
      fetchMock.mockResolvedValueOnce({
        ok: false,
        status: 404,
        statusText: 'Not Found'
      });
      
      jest.spyOn(apiService, 'delay').mockResolvedValue();
      
      // Act & Assert
      await expect(apiService.getUser(999)).rejects.toThrow(
        'Failed after 3 attempts: HTTP 404: Not Found'
      );
    });
    
    test('should handle 500 errors with retry', async () => {
      // Arrange
      fetchMock
        .mockResolvedValueOnce({
          ok: false,
          status: 500,
          statusText: 'Internal Server Error'
        })
        .mockResolvedValueOnce({
          ok: true,
          json: async () => ({ id: 1, name: 'John' })
        });
      
      jest.spyOn(apiService, 'delay').mockResolvedValue();
      
      // Act
      const result = await apiService.getUser(1);
      
      // Assert
      expect(result).toEqual({ id: 1, name: 'John' });
      expect(fetchMock).toHaveBeenCalledTimes(2);
    });
  });
});

// Tests avec fake timers pour contrôler le temps
describe('Delayed operations', () => {
  beforeEach(() => {
    jest.useFakeTimers();
  });
  
  afterEach(() => {
    jest.useRealTimers();
  });
  
  test('should handle timeouts correctly', async () => {
    // Arrange
    const timeoutPromise = new Promise((resolve, reject) => {
      setTimeout(() => reject(new Error('Timeout')), 5000);
    });
    
    // Act
    const promise = expect(timeoutPromise).rejects.toThrow('Timeout');
    
    // Fast-forward time
    jest.advanceTimersByTime(5000);
    
    // Assert
    await promise;
  });
  
  test('should test debounced function', () => {
    // Arrange
    const callback = jest.fn();
    const debouncedFn = debounce(callback, 1000);
    
    // Act
    debouncedFn('call 1');
    debouncedFn('call 2');
    debouncedFn('call 3');
    
    // Fast-forward time
    jest.advanceTimersByTime(1000);
    
    // Assert
    expect(callback).toHaveBeenCalledTimes(1);
    expect(callback).toHaveBeenCalledWith('call 3');
  });
});

// Fonction utilitaire debounce
function debounce(func, delay) {
  let timeoutId;
  return function (...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}
```

### Tests d'intégration avec base de données
```javascript
// Tests d'intégration avec base de données de test
describe('UserRepository Integration', () => {
  let db;
  let userRepository;
  
  beforeAll(async () => {
    // Setup test database
    db = await setupTestDatabase();
    userRepository = new UserRepository(db);
  });
  
  afterAll(async () => {
    await db.close();
  });
  
  beforeEach(async () => {
    await db.collection('users').deleteMany({});
  });
  
  test('should create and retrieve user', async () => {
    // Arrange
    const userData = {
      name: 'John Doe',
      email: 'john@example.com',
      age: 30
    };
    
    // Act
    const createdUser = await userRepository.create(userData);
    const retrievedUser = await userRepository.findById(createdUser.id);
    
    // Assert
    expect(retrievedUser).toMatchObject(userData);
    expect(retrievedUser.id).toBeDefined();
    expect(retrievedUser.createdAt).toBeInstanceOf(Date);
  });
  
  test('should handle concurrent operations', async () => {
    // Arrange
    const userPromises = Array.from({ length: 10 }, (_, i) =>
      userRepository.create({
        name: `User ${i}`,
        email: `user${i}@example.com`
      })
    );
    
    // Act
    const users = await Promise.all(userPromises);
    
    // Assert
    expect(users).toHaveLength(10);
    expect(new Set(users.map(u => u.id))).toHaveSize(10); // All IDs unique
  });
});

async function setupTestDatabase() {
  // Configuration pour base de test
  const { MongoMemoryServer } = require('mongodb-memory-server');
  const { MongoClient } = require('mongodb');
  
  const mongod = new MongoMemoryServer();
  const uri = await mongod.getUri();
  const client = new MongoClient(uri);
  await client.connect();
  
  return client.db();
}
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Suite de Tests Complète

{% hint style="warning" %}
**Défi :** Créez une suite de tests professionnelle pour une application e-commerce
{% endhint %}

```typescript
// Créez une suite de tests complète pour ce système
interface EcommerceSystem {
    // TODO: Testing strategy
    strategy: {
        unitTests: boolean;
        integrationTests: boolean;
        e2eTests: boolean;
        performanceTests: boolean;
    };
    
    // TODO: Code coverage
    coverage: {
        target: number; // 80%+
        branches: boolean;
        functions: boolean;
        lines: boolean;
        statements: boolean;
    };
    
    // TODO: Test types
    tests: {
        models: 'Product, Cart, Order, User';
        services: 'PaymentService, InventoryService, NotificationService';
        api: 'REST endpoints, GraphQL resolvers';
        integration: 'Database, External APIs, File system';
    };
    
    // TODO: Mocking strategy
    mocks: {
        external: 'Payment gateways, Email service, SMS service';
        database: 'In-memory or test database';
        filesystem: 'Virtual filesystem';
        time: 'Controlled time for date operations';
    };
}

// TODO: Implémentez la suite de tests
class EcommerceTestSuite {
    constructor() {}
    
    // TODO: Tests unitaires des modèles
    setupUnitTests() {
        // Product model tests
        // Cart logic tests
        // Order validation tests
        // User authentication tests
    }
    
    // TODO: Tests des services
    setupServiceTests() {
        // Payment processing
        // Inventory management
        // Notification system
        // Email/SMS services
    }
    
    // TODO: Tests d'intégration
    setupIntegrationTests() {
        // Database operations
        // API endpoints
        // External service integration
        // File operations
    }
    
    // TODO: Tests de performance
    setupPerformanceTests() {
        // Load testing
        // Memory usage
        // Database query performance
        // API response times
    }
    
    // TODO: Test utilities
    setupTestUtils() {
        // Test data factories
        // Mock generators
        // Custom matchers
        // Test helpers
    }
}

// Exemple de cas de test complexe
describe('E-commerce Order Processing', () => {
    // TODO: Setup complet avec mocks
    // TODO: Tests de workflow complet
    // TODO: Tests d'erreurs et edge cases
    // TODO: Tests de performance
    // TODO: Tests de sécurité
});
```

{% tabs %}
{% tab title="💡 Indice" %}
```typescript
// Structure de base
describe('OrderService', () => {
  let orderService;
  let mockPaymentService;
  let mockInventoryService;
  
  beforeEach(() => {
    mockPaymentService = createMockPaymentService();
    mockInventoryService = createMockInventoryService();
    orderService = new OrderService(mockPaymentService, mockInventoryService);
  });
  
  test('should process order successfully', async () => {
    // Arrange
    const orderData = createTestOrder();
    
    // Act
    const result = await orderService.processOrder(orderData);
    
    // Assert
    expect(result.success).toBe(true);
  });
});
```
{% endtab %}

{% tab title="✅ Solution" %}
Solution complète avec tous types de tests, mocks sophistiqués et couverture de code dans l'onglet suivant.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant le testing JavaScript au niveau professionnel !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Jest** configuration et utilisation avancée
- **TDD/BDD** méthodologies et pratiques
- **Mocking** sophistiqué avec test doubles
- **Tests asynchrones** avec Promises et timers
- **Couverture de code** et métriques qualité
- **Performance testing** et optimisation
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Écrire des tests unitaires robustes
- Implémenter TDD/BDD efficacement
- Créer des mocks et stubs sophistiqués
- Tester du code asynchrone complexe
- Organiser des suites de tests scalables
- Analyser et améliorer la couverture
{% endtab %}

{% tab title="🚀 Applications professionnelles" %}
- **CI/CD pipelines** avec tests automatisés
- **Code review** basé sur les tests
- **Refactoring** sécurisé avec tests
- **Documentation** vivante via tests
- **Quality gates** avec métriques
- **Debugging** assisté par tests
{% endtab %}
{% endtabs %}

---

**Prochaine étape : Optimisation et Performance !** ⚡
