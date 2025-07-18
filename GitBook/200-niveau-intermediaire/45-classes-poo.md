# Classes ES6 et Programmation Orientée Objet

> **Navigation :** [← Objets JavaScript](40-objets.md) | [Prototypes et Héritage →](50-prototypes-heritage.md)

## 🎯 Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :
- ✅ Comprendre les principes fondamentaux de la POO en JavaScript
- ✅ Utiliser la syntaxe moderne des classes ES6
- ✅ Implémenter l'encapsulation avec les champs privés
- ✅ Créer des hiérarchies d'objets avec l'héritage
- ✅ Appliquer le polymorphisme et l'abstraction
- ✅ Convertir du code basé sur les prototypes vers les classes

## 📚 Concepts clés

| Concept | Description | Exemple |
|---------|-------------|---------|
| **Classe** | Syntaxe moderne pour créer des objets | `class Person {}` |
| **Constructor** | Méthode d'initialisation | `constructor(name) {}` |
| **Encapsulation** | Regrouper données et méthodes | Champs privés `#field` |
| **Héritage** | Partager fonctionnalités | `extends`, `super` |
| **Polymorphisme** | Même interface, comportements différents | Surcharge de méthodes |

## 🔍 Introduction

{% tabs %}
{% tab title="💡 Pourquoi la POO ?" %}
La Programmation Orientée Objet offre plusieurs avantages :

**🎯 Organisation du code**
- Structure claire et logique
- Code plus lisible et maintenable
- Séparation des responsabilités

**🔄 Réutilisabilité**
- Éviter la duplication de code
- Héritage et composition
- Modules réutilisables

**🛡️ Encapsulation**
- Protection des données
- Interface publique contrôlée
- Réduction des erreurs

**🌍 Modélisation réelle**
- Représenter des concepts métier
- Relations naturelles entre objets
- Abstraction des complexités
{% endtab %}

{% tab title="📜 Évolution en JavaScript" %}
**Avant ES6 - Fonctions constructeurs**
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};
```

**Avec ES6 - Classes modernes**
```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
}
```

**Avantages des classes ES6**
- Syntaxe plus claire et familière
- Champs privés natifs
- Méthodes statiques simplifiées
- Héritage plus intuitif
{% endtab %}
{% endtabs %}

## 🔧 Les 4 Piliers de la POO

### 1. Encapsulation

{% hint style="info" %}
**L'encapsulation** consiste à regrouper les données et les méthodes qui les manipulent dans une même unité, tout en contrôlant l'accès depuis l'extérieur.
{% endhint %}

{% tabs %}
{% tab title="🔒 Champs privés" %}
```javascript
class BankAccount {
    // Champs privés (ES2022)
    #balance = 0;
    #accountNumber;
    
    constructor(accountNumber, initialBalance = 0) {
        this.#accountNumber = accountNumber;
        this.#balance = initialBalance;
    }
    
    // Méthode privée
    #validateAmount(amount) {
        return amount > 0;
    }
    
    // Interface publique
    deposit(amount) {
        if (this.#validateAmount(amount)) {
            this.#balance += amount;
            return this.#balance;
        }
        throw new Error("Montant invalide");
    }
    
    withdraw(amount) {
        if (this.#validateAmount(amount) && amount <= this.#balance) {
            this.#balance -= amount;
            return this.#balance;
        }
        throw new Error("Retrait impossible");
    }
    
    // Getter pour lecture seule
    get balance() {
        return this.#balance;
    }
    
    get accountInfo() {
        return `Compte ${this.#accountNumber}: ${this.#balance}€`;
    }
}

// Utilisation
const account = new BankAccount("FR123456", 1000);
console.log(account.balance); // 1000
account.deposit(500);
console.log(account.accountInfo); // "Compte FR123456: 1500€"

// Erreur : champ privé inaccessible
// console.log(account.#balance); // SyntaxError
```
{% endtab %}

{% tab title="🎛️ Getters/Setters" %}
```javascript
class Temperature {
    #celsius = 0;
    
    constructor(celsius = 0) {
        this.celsius = celsius; // Utilise le setter
    }
    
    get celsius() {
        return this.#celsius;
    }
    
    set celsius(value) {
        if (value < -273.15) {
            throw new Error("Température en dessous du zéro absolu");
        }
        this.#celsius = value;
    }
    
    get fahrenheit() {
        return (this.#celsius * 9/5) + 32;
    }
    
    set fahrenheit(value) {
        this.celsius = (value - 32) * 5/9;
    }
    
    get kelvin() {
        return this.#celsius + 273.15;
    }
    
    toString() {
        return `${this.#celsius}°C (${this.fahrenheit}°F, ${this.kelvin}K)`;
    }
}

// Utilisation
const temp = new Temperature(25);
console.log(temp.toString()); // "25°C (77°F, 298.15K)"

temp.fahrenheit = 100;
console.log(temp.celsius); // 37.78
```
{% endtab %}
{% endtabs %}

### 2. Héritage

{% hint style="info" %}
**L'héritage** permet de créer de nouvelles classes basées sur des classes existantes, partageant leurs propriétés et méthodes.
{% endhint %}

{% tabs %}
{% tab title="🌳 Extends et Super" %}
```javascript
// Classe parent
class Vehicle {
    static totalVehicles = 0;
    
    constructor(brand, model, year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
        this.speed = 0;
        Vehicle.totalVehicles++;
    }
    
    start() {
        return `${this.getDescription()} starts`;
    }
    
    accelerate(increase) {
        this.speed += increase;
        return `Speed: ${this.speed} km/h`;
    }
    
    getDescription() {
        return `${this.brand} ${this.model} (${this.year})`;
    }
    
    getAge() {
        return new Date().getFullYear() - this.year;
    }
}

// Classe enfant
class Car extends Vehicle {
    constructor(brand, model, year, doors, fuelType) {
        super(brand, model, year); // Appel du constructeur parent
        this.doors = doors;
        this.fuelType = fuelType;
    }
    
    // Surcharge de méthode
    start() {
        const parentStart = super.start(); // Appel méthode parent
        return `${parentStart} with ${this.fuelType}`;
    }
    
    // Nouvelle méthode
    openTrunk() {
        return `${this.getDescription()} trunk opened`;
    }
    
    getType() {
        return `Car with ${this.doors} doors`;
    }
}

class Motorcycle extends Vehicle {
    constructor(brand, model, year, engineSize) {
        super(brand, model, year);
        this.engineSize = engineSize;
    }
    
    start() {
        return `${super.start()} - VROOOM! (${this.engineSize}cc)`;
    }
    
    wheelie() {
        return `${this.getDescription()} does a wheelie!`;
    }
    
    getType() {
        return `Motorcycle ${this.engineSize}cc`;
    }
}

// Tests
const car = new Car("Toyota", "Camry", 2020, 4, "gasoline");
const bike = new Motorcycle("Honda", "CBR", 2021, 1000);

console.log(car.start()); // "Toyota Camry (2020) starts with gasoline"
console.log(bike.start()); // "Honda CBR (2021) starts - VROOOM! (1000cc)"
console.log(car.accelerate(50)); // "Speed: 50 km/h"
console.log(bike.wheelie()); // "Honda CBR (2021) does a wheelie!"

// Vérification héritage
console.log(car instanceof Vehicle); // true
console.log(car instanceof Car); // true
console.log(bike instanceof Vehicle); // true
console.log(bike instanceof Car); // false
```
{% endtab %}

{% tab title="📊 Méthodes statiques" %}
```javascript
class MathHelper {
    static PI = 3.14159;
    static E = 2.71828;
    
    // Méthodes statiques - appartiennent à la classe
    static square(x) {
        return x * x;
    }
    
    static circleArea(radius) {
        return MathHelper.PI * MathHelper.square(radius);
    }
    
    static randomBetween(min, max) {
        return Math.random() * (max - min) + min;
    }
    
    static factorial(n) {
        if (n <= 1) return 1;
        return n * MathHelper.factorial(n - 1);
    }
    
    // Méthode d'instance
    constructor(value) {
        this.value = value;
    }
    
    multiply(factor) {
        this.value *= factor;
        return this;
    }
    
    add(number) {
        this.value += number;
        return this;
    }
    
    result() {
        return this.value;
    }
}

// Utilisation des méthodes statiques
console.log(MathHelper.square(5)); // 25
console.log(MathHelper.circleArea(3)); // 28.27
console.log(MathHelper.factorial(5)); // 120

// Utilisation des méthodes d'instance
const calc = new MathHelper(10);
const result = calc.multiply(2).add(5).result(); // 25
```
{% endtab %}
{% endtabs %}

### 3. Polymorphisme

{% hint style="info" %}
**Le polymorphisme** permet à des objets de différentes classes de répondre à la même interface de manière appropriée à leur type.
{% endhint %}

{% tabs %}
{% tab title="🎭 Interface commune" %}
```javascript
// Classe abstraite de base
class Shape {
    constructor(color) {
        if (this.constructor === Shape) {
            throw new Error("Shape est une classe abstraite");
        }
        this.color = color;
    }
    
    // Méthodes abstraites - doivent être implémentées
    area() {
        throw new Error("La méthode area() doit être implémentée");
    }
    
    perimeter() {
        throw new Error("La méthode perimeter() doit être implémentée");
    }
    
    // Méthode concrète commune
    getInfo() {
        return `${this.constructor.name} ${this.color}: 
                Surface = ${this.area()}, 
                Périmètre = ${this.perimeter()}`;
    }
}

class Circle extends Shape {
    constructor(color, radius) {
        super(color);
        this.radius = radius;
    }
    
    area() {
        return Math.PI * this.radius ** 2;
    }
    
    perimeter() {
        return 2 * Math.PI * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(color, width, height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    area() {
        return this.width * this.height;
    }
    
    perimeter() {
        return 2 * (this.width + this.height);
    }
}

class Triangle extends Shape {
    constructor(color, base, height, side1, side2) {
        super(color);
        this.base = base;
        this.height = height;
        this.side1 = side1;
        this.side2 = side2;
    }
    
    area() {
        return (this.base * this.height) / 2;
    }
    
    perimeter() {
        return this.base + this.side1 + this.side2;
    }
}

// Polymorphisme en action
function calculateTotalArea(shapes) {
    return shapes.reduce((total, shape) => total + shape.area(), 0);
}

function displayShapeInfo(shapes) {
    shapes.forEach(shape => {
        console.log(shape.getInfo());
        console.log("---");
    });
}

// Tests
const shapes = [
    new Circle("rouge", 5),
    new Rectangle("bleu", 4, 6),
    new Triangle("vert", 8, 6, 5, 7)
];

displayShapeInfo(shapes);
console.log(`Surface totale: ${calculateTotalArea(shapes).toFixed(2)}`);
```
{% endtab %}

{% tab title="🔄 Duck Typing" %}
```javascript
// Duck Typing : "Si ça marche comme un canard..."
class Duck {
    quack() {
        return "Quack quack!";
    }
    
    fly() {
        return "🦆 Duck flies away";
    }
    
    swim() {
        return "🏊 Duck swims gracefully";
    }
}

class Robot {
    quack() {
        return "BEEP BEEP (simulating quack)";
    }
    
    fly() {
        return "🤖 Robot activates jetpack";
    }
    
    swim() {
        return "⚡ Robot activates underwater mode";
    }
}

class Person {
    quack() {
        return "I'm pretending to be a duck!";
    }
    
    fly() {
        return "👤 Person jumps (but can't really fly)";
    }
    
    swim() {
        return "🏊‍♂️ Person swims with effort";
    }
}

// Fonction polymorphe - accepte tout objet avec les bonnes méthodes
function makeDuckActions(duckLike) {
    console.log(`Quack: ${duckLike.quack()}`);
    console.log(`Fly: ${duckLike.fly()}`);
    console.log(`Swim: ${duckLike.swim()}`);
    console.log("---");
}

// Tests - tous se comportent comme des "canards"
const creatures = [
    new Duck(),
    new Robot(),
    new Person()
];

creatures.forEach(creature => {
    console.log(`${creature.constructor.name}:`);
    makeDuckActions(creature);
});
```
{% endtab %}
{% endtabs %}

### 4. Abstraction

{% hint style="info" %}
**L'abstraction** cache la complexité interne et expose seulement les fonctionnalités nécessaires à travers une interface simple.
{% endhint %}

{% tabs %}
{% tab title="🎮 Interface simplifiée" %}
```javascript
class GameEngine {
    #renderer;
    #physics;
    #audio;
    #entities = [];
    #running = false;
    
    constructor(canvasId) {
        this.#renderer = this.#initRenderer(canvasId);
        this.#physics = this.#initPhysics();
        this.#audio = this.#initAudio();
        console.log("🎮 Game Engine initialized");
    }
    
    // Méthodes privées - complexité cachée
    #initRenderer(canvasId) {
        // Configuration complexe du rendu
        return {
            canvas: document.getElementById(canvasId),
            render: (entities) => {
                // Algorithmes de rendu complexes
                console.log(`🎨 Rendering ${entities.length} entities`);
            }
        };
    }
    
    #initPhysics() {
        return {
            update: (entities, deltaTime) => {
                // Calculs physiques complexes
                entities.forEach(entity => {
                    if (entity.velocity) {
                        entity.x += entity.velocity.x * deltaTime;
                        entity.y += entity.velocity.y * deltaTime;
                    }
                });
            }
        };
    }
    
    #initAudio() {
        return {
            play: (sound) => console.log(`🔊 Playing: ${sound}`)
        };
    }
    
    #gameLoop = () => {
        if (!this.#running) return;
        
        const deltaTime = 0.016; // 60 FPS
        
        // Mise à jour complexe
        this.#physics.update(this.#entities, deltaTime);
        this.#renderer.render(this.#entities);
        
        requestAnimationFrame(this.#gameLoop);
    }
    
    // Interface publique simple
    start() {
        this.#running = true;
        this.#gameLoop();
        console.log("▶️ Game started");
    }
    
    stop() {
        this.#running = false;
        console.log("⏹️ Game stopped");
    }
    
    addEntity(entity) {
        this.#entities.push(entity);
        console.log(`➕ Added entity: ${entity.name}`);
    }
    
    removeEntity(entityName) {
        const index = this.#entities.findIndex(e => e.name === entityName);
        if (index !== -1) {
            this.#entities.splice(index, 1);
            console.log(`➖ Removed entity: ${entityName}`);
        }
    }
    
    playSound(soundName) {
        this.#audio.play(soundName);
    }
    
    getEntityCount() {
        return this.#entities.length;
    }
}

// Utilisation simple de l'interface complexe
const game = new GameEngine("gameCanvas");

// Ajouter des entités
game.addEntity({ name: "Player", x: 100, y: 100, velocity: { x: 1, y: 0 } });
game.addEntity({ name: "Enemy", x: 200, y: 150, velocity: { x: -0.5, y: 0 } });

// Contrôles simples
game.start();
game.playSound("background_music");

setTimeout(() => {
    game.stop();
    console.log(`Final entity count: ${game.getEntityCount()}`);
}, 2000);
```
{% endtab %}
{% endtabs %}

## 💡 Quiz de compréhension

{% tabs %}
{% tab title="❓ Questions" %}
**Question 1 :** Quelle est la différence principale entre une classe et une fonction constructeur ?

**Question 2 :** Que se passe-t-il si vous oubliez d'appeler `super()` dans un constructeur de classe héritée ?

**Question 3 :** Comment créer un champ vraiment privé dans une classe ES6 moderne ?

**Question 4 :** Quelle est la différence entre une méthode statique et une méthode d'instance ?

**Question 5 :** Qu'est-ce que le polymorphisme et comment l'implémenter en JavaScript ?
{% endtab %}

{% tab title="✅ Réponses" %}
**Réponse 1 :** Les classes offrent une syntaxe plus claire, des champs privés natifs, un héritage simplifié avec `extends/super`, et ne sont pas hissées comme les fonctions.

**Réponse 2 :** JavaScript lèvera une `ReferenceError` car `this` n'est pas initialisé avant l'appel à `super()`.

**Réponse 3 :** Utiliser la syntaxe `#` : `#privateField = value;` pour les champs et `#privateMethod() {}` pour les méthodes.

**Réponse 4 :** Les méthodes statiques appartiennent à la classe (appelées avec `Class.method()`), les méthodes d'instance appartiennent aux objets créés.

**Réponse 5 :** Le polymorphisme permet à différents objets de répondre à la même interface. En JavaScript, on l'implémente via l'héritage et la surcharge de méthodes.
{% endtab %}
{% endtabs %}

## 🛠️ Exercices pratiques

### Exercice 1 : Système de véhicules
{% tabs %}
{% tab title="📋 Énoncé" %}
Créez un système de gestion de véhicules avec les spécifications suivantes :

**Classes à implémenter :**
1. `Vehicle` (classe de base)
   - Propriétés : brand, model, year, mileage
   - Méthodes : start(), stop(), drive(distance), getInfo()

2. `Car` extends Vehicle
   - Propriétés supplémentaires : doors, fuelType
   - Redéfinir start() pour inclure le type de carburant
   - Méthode : openTrunk()

3. `Truck` extends Vehicle
   - Propriétés supplémentaires : loadCapacity, currentLoad
   - Méthodes : loadCargo(weight), unloadCargo()
   - Vérifier que le poids ne dépasse pas la capacité

**Contraintes :**
- Utiliser des champs privés pour le kilométrage
- Implémenter des getters/setters appropriés
- Ajouter une méthode statique pour compter les véhicules
```javascript
// Votre code ici...
```
{% endtab %}

{% tab title="💡 Hints" %}
{% hint style="info" %}
**Astuce 1 :** Utilisez `#mileage` comme champ privé et créez un getter public.

**Astuce 2 :** Dans les classes dérivées, appelez `super()` en premier dans le constructeur.

**Astuce 3 :** Pour les méthodes statiques, utilisez `static` et accédez avec `Vehicle.count`.

**Astuce 4 :** Validez les données dans les setters avant de les assigner.
{% endhint %}
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
class Vehicle {
    static #totalVehicles = 0;
    #mileage = 0;
    
    constructor(brand, model, year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
        Vehicle.#totalVehicles++;
    }
    
    get mileage() {
        return this.#mileage;
    }
    
    start() {
        return `${this.getDescription()} is starting`;
    }
    
    stop() {
        return `${this.getDescription()} has stopped`;
    }
    
    drive(distance) {
        if (distance > 0) {
            this.#mileage += distance;
            return `Drove ${distance}km. Total mileage: ${this.#mileage}km`;
        }
        return "Distance must be positive";
    }
    
    getDescription() {
        return `${this.brand} ${this.model} (${this.year})`;
    }
    
    getInfo() {
        return {
            description: this.getDescription(),
            mileage: this.#mileage,
            age: new Date().getFullYear() - this.year
        };
    }
    
    static getTotalVehicles() {
        return Vehicle.#totalVehicles;
    }
}

class Car extends Vehicle {
    constructor(brand, model, year, doors, fuelType) {
        super(brand, model, year);
        this.doors = doors;
        this.fuelType = fuelType;
    }
    
    start() {
        return `${super.start()} with ${this.fuelType} engine`;
    }
    
    openTrunk() {
        return `${this.getDescription()} trunk is open`;
    }
    
    getInfo() {
        return {
            ...super.getInfo(),
            doors: this.doors,
            fuelType: this.fuelType,
            type: "Car"
        };
    }
}

class Truck extends Vehicle {
    #currentLoad = 0;
    
    constructor(brand, model, year, loadCapacity) {
        super(brand, model, year);
        this.loadCapacity = loadCapacity;
    }
    
    get currentLoad() {
        return this.#currentLoad;
    }
    
    get availableCapacity() {
        return this.loadCapacity - this.#currentLoad;
    }
    
    loadCargo(weight) {
        if (weight <= 0) {
            return "Weight must be positive";
        }
        
        if (this.#currentLoad + weight > this.loadCapacity) {
            return `Cannot load ${weight}kg. Exceeds capacity by ${
                (this.#currentLoad + weight) - this.loadCapacity
            }kg`;
        }
        
        this.#currentLoad += weight;
        return `Loaded ${weight}kg. Current load: ${this.#currentLoad}kg`;
    }
    
    unloadCargo(weight = this.#currentLoad) {
        if (weight <= 0) {
            return "Weight must be positive";
        }
        
        if (weight > this.#currentLoad) {
            weight = this.#currentLoad;
        }
        
        this.#currentLoad -= weight;
        return `Unloaded ${weight}kg. Current load: ${this.#currentLoad}kg`;
    }
    
    getInfo() {
        return {
            ...super.getInfo(),
            loadCapacity: this.loadCapacity,
            currentLoad: this.#currentLoad,
            availableCapacity: this.availableCapacity,
            type: "Truck"
        };
    }
}

// Tests
const car = new Car("Toyota", "Camry", 2020, 4, "gasoline");
const truck = new Truck("Volvo", "FH16", 2019, 25000);

console.log(car.start());
console.log(car.drive(150));
console.log(car.openTrunk());

console.log(truck.loadCargo(15000));
console.log(truck.loadCargo(12000)); // Devrait échouer
console.log(truck.unloadCargo(5000));

console.log("=== Vehicle Info ===");
console.log(car.getInfo());
console.log(truck.getInfo());
console.log(`Total vehicles: ${Vehicle.getTotalVehicles()}`);
```
{% endtab %}
{% endtabs %}

### Exercice 2 : Système de gestion d'employés
{% tabs %}
{% tab title="📋 Énoncé" %}
Créez un système RH complet avec polymorphisme :

**Classes requises :**
1. `Employee` (classe abstraite)
   - Champs privés : salary, hireDate
   - Méthodes abstraites : calculateBonus(), getRole()
   - Méthodes concrètes : getInfo(), getYearsOfService()

2. `Developer` extends Employee
   - Propriétés : programmingLanguages[], projects[]
   - Bonus basé sur le nombre de projets

3. `Manager` extends Employee
   - Propriétés : team[], department, budget
   - Bonus basé sur la taille de l'équipe

4. `Designer` extends Employee
   - Propriétés : designTools[], portfolio[]
   - Bonus basé sur le portfolio

**Fonctionnalités avancées :**
- Méthodes statiques pour les statistiques
- Validation des données
- Pattern Observer pour les changements de salaire
```javascript
// À vous de jouer !
```
{% endtab %}

{% tab title="💡 Hints" %}
{% hint style="info" %}
**Astuce 1 :** Pour une classe abstraite, vérifiez `this.constructor === Employee` dans le constructeur.

**Astuce 2 :** Utilisez `throw new Error()` pour les méthodes abstraites non implémentées.

**Astuce 3 :** Pour le pattern Observer, maintenez une liste de callbacks à notifier.

**Astuce 4 :** Calculez l'ancienneté avec `Date.now() - this.#hireDate.getTime()`.
{% endhint %}
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Solution complète disponible dans le code source
// Cette solution montre l'utilisation avancée des classes ES6
```
{% endtab %}
{% endtabs %}

## 🎯 Projet pratique : Système de jeu RPG

{% tabs %}
{% tab title="🎮 Description" %}
**Objectif :** Créer un système de jeu RPG avec classes et POO

**Fonctionnalités :**
- Système de personnages avec classes (Guerrier, Mage, Archer)
- Système de combat polymorphe
- Inventaire et équipements
- Système de niveaux et d'expérience
- Sauvegarde/chargement

**Architecture :**
```
Character (classe abstraite)
├── Warrior
├── Mage
└── Archer

Item (classe abstraite)
├── Weapon
├── Armor
└── Consumable

Game (gestionnaire principal)
```
{% endtab %}

{% tab title="🚀 Implémentation" %}
```javascript
// Exemple de structure de base
class Character {
    #health;
    #mana;
    #experience = 0;
    #level = 1;
    
    constructor(name, baseStats) {
        if (this.constructor === Character) {
            throw new Error("Character is abstract");
        }
        
        this.name = name;
        this.#health = baseStats.health;
        this.#mana = baseStats.mana;
        this.inventory = new Inventory();
    }
    
    // Méthodes abstraites
    attack(target) {
        throw new Error("Must implement attack method");
    }
    
    useSpecialAbility(target) {
        throw new Error("Must implement special ability");
    }
    
    // Méthodes communes
    takeDamage(amount) {
        this.#health = Math.max(0, this.#health - amount);
        return this.#health === 0;
    }
    
    heal(amount) {
        this.#health = Math.min(this.maxHealth, this.#health + amount);
    }
    
    gainExperience(amount) {
        this.#experience += amount;
        while (this.#experience >= this.expToNextLevel) {
            this.levelUp();
        }
    }
    
    levelUp() {
        this.#level++;
        this.#experience -= this.expToNextLevel;
        this.onLevelUp();
    }
    
    // Hook pour les sous-classes
    onLevelUp() {
        console.log(`${this.name} reached level ${this.#level}!`);
    }
    
    get health() { return this.#health; }
    get mana() { return this.#mana; }
    get level() { return this.#level; }
    get experience() { return this.#experience; }
    
    get expToNextLevel() {
        return this.#level * 100;
    }
    
    get maxHealth() {
        return 100 + (this.#level - 1) * 20;
    }
}

class Warrior extends Character {
    constructor(name) {
        super(name, { health: 120, mana: 30 });
        this.strength = 15;
        this.defense = 12;
    }
    
    attack(target) {
        const damage = this.strength + Math.floor(Math.random() * 10);
        console.log(`${this.name} attacks ${target.name} for ${damage} damage!`);
        return target.takeDamage(damage);
    }
    
    useSpecialAbility(target) {
        if (this.mana >= 20) {
            const damage = this.strength * 2;
            this.mana -= 20;
            console.log(`${this.name} uses Berserker Rage! Massive damage!`);
            return target.takeDamage(damage);
        }
        console.log("Not enough mana!");
        return false;
    }
    
    onLevelUp() {
        super.onLevelUp();
        this.strength += 3;
        this.defense += 2;
    }
}

// Continuez avec Mage, Archer, système d'items, etc.
```
{% endtab %}
{% endtabs %}

## 🔗 Liens et références

{% tabs %}
{% tab title="📚 Approfondissement" %}
**Concepts liés :**
- [Prototypes et Héritage](50-prototypes-heritage.md) - Mécanisme sous-jacent
- [Modules et Organisation](95-modules-organisation.md) - Structure du code
- [Design Patterns](120-design-patterns.md) - Patterns avec classes

**Documentation officielle :**
- [MDN Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [MDN Private fields](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)
{% endtab %}

{% tab title="🔧 Outils utiles" %}
**Linters et formateurs :**
- ESLint rules pour les classes
- Prettier pour le formatage
- TypeScript pour le typage

**Debugging :**
- DevTools pour inspecter les instances
- `instanceof` pour vérifier l'héritage
- `Object.getPrototypeOf()` pour explorer la chaîne
{% endtab %}
{% endtabs %}

## 🎯 Points clés à retenir

{% hint style="success" %}
**✅ Ce qu'il faut maîtriser :**
- Syntaxe des classes ES6 et champs privés
- Héritage avec `extends` et `super`
- Polymorphisme et méthodes abstraites
- Encapsulation et contrôle d'accès
- Différence entre méthodes statiques et d'instance

**⚡ Bonnes pratiques :**
- Utilisez les champs privés pour l'encapsulation réelle
- Préférez la composition à l'héritage multiple
- Documentez les interfaces publiques
- Validez les données dans les setters
- Utilisez des méthodes abstraites pour définir des contrats
{% endhint %}

{% hint style="warning" %}
**⚠️ Pièges courants :**
- Oublier `super()` dans les constructeurs héritées
- Confondre méthodes statiques et d'instance
- Mauvaise gestion du contexte `this`
- Accès direct aux propriétés au lieu des getters/setters
- Héritage trop profond (préférer la composition)
{% endhint %}

---

> **Navigation :** [← Objets JavaScript](40-objets.md) | [Prototypes et Héritage →](50-prototypes-heritage.md)
