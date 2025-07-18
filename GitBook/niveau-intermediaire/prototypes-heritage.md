# 🧬 Prototypes et Héritage

{% hint style="info" %}
**Niveau :** 🟡 Intermédiaire | **Durée :** 50 min | **Prérequis :** Objets, Fonctions, Classes ES6
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** la chaîne prototypale et son fonctionnement
- [ ] **Différencier** héritage prototypal vs héritage classique
- [ ] **Manipuler** les prototypes et créer des hiérarchies d'objets
- [ ] **Déboguer** les problèmes liés aux prototypes
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Prototype** : Objet modèle dont héritent d'autres objets  
**Chaîne prototypale** : Succession de prototypes jusqu'à `null`  
**Héritage prototypal** : Mécanisme d'héritage natif de JavaScript  
**Constructor function** : Fonction utilisée avec `new`  
**Prototype pollution** : Modification dangereuse des prototypes natifs  
{% endtab %}

{% tab title="Importance" %}
🏗️ **Base de tout** : Tous les objets JavaScript héritent de prototypes  
🎭 **Classes ES6** : Sont du "sucre syntaxique" sur les prototypes  
⚡ **Performance** : Méthodes partagées via prototypes  
🔄 **Flexibilité** : Modification dynamique des "classes"  
📱 **Frameworks** : React, Vue utilisent massivement les prototypes  
{% endtab %}
{% endtabs %}

## 📖 Qu'est-ce qu'un prototype ?

{% hint style="warning" %}
**Concept clé :** Chaque objet JavaScript a un lien vers un autre objet appelé son **prototype**. Si une propriété n'existe pas sur l'objet, JavaScript la cherche dans son prototype.
{% endhint %}

### 🔍 Exemple fondamental

{% tabs %}
{% tab title="Héritage simple" %}
```javascript
// Objet "parent"
const animal = {
  type: "animal",
  respirer() {
    console.log(`${this.nom || 'L\'animal'} respire`);
  },
  dormir() {
    console.log(`${this.nom || 'L\'animal'} dort`);
  }
};

// Objet "enfant"
const chien = {
  nom: "Rex",
  race: "Labrador"
};

// Créer l'héritage
Object.setPrototypeOf(chien, animal);

// Utilisation
console.log(chien.nom);    // "Rex" (propriété propre)
console.log(chien.type);   // "animal" (trouvée dans le prototype)
chien.respirer();          // "Rex respire" (méthode du prototype)
chien.dormir();            // "Rex dort"
```
{% endtab %}

{% tab title="Vérification du prototype" %}
```javascript
// Méthodes pour vérifier les prototypes
console.log(Object.getPrototypeOf(chien) === animal); // true
console.log(chien.__proto__ === animal); // true (déprécié)
console.log(animal.isPrototypeOf(chien)); // true

// Vérifier si une propriété est propre à l'objet
console.log(chien.hasOwnProperty('nom'));    // true
console.log(chien.hasOwnProperty('type'));   // false (dans le prototype)

// Lister les propriétés propres
console.log(Object.getOwnPropertyNames(chien)); // ['nom', 'race']
```
{% endtab %}
{% endtabs %}

## ⛓️ La chaîne prototypale

### 🔗 Comment ça fonctionne

{% hint style="info" %}
JavaScript remonte la chaîne des prototypes dans cet ordre :
1. **Objet lui-même**
2. **Son prototype direct**
3. **Le prototype du prototype**
4. **Jusqu'à `Object.prototype`**
5. **Puis `null`** (fin de chaîne)
{% endhint %}

{% tabs %}
{% tab title="Chaîne complète" %}
```javascript
const grandParent = {
  famille: "Mammifères",
  ancêtre() {
    return "Je suis l'ancêtre";
  }
};

const parent = {
  famille: "Carnivores", // Masque la propriété du grand-parent
  chasseur: true
};

const enfant = {
  nom: "Loup",
  hurler() {
    return "Aouuuu!";
  }
};

// Créer la chaîne : enfant → parent → grandParent → Object.prototype → null
Object.setPrototypeOf(parent, grandParent);
Object.setPrototypeOf(enfant, parent);

// Recherche dans la chaîne
console.log(enfant.nom);        // "Loup" (propriété propre)
console.log(enfant.chasseur);   // true (premier niveau parent)
console.log(enfant.famille);    // "Carnivores" (masque "Mammifères")
console.log(enfant.ancêtre());  // "Je suis l'ancêtre" (grand-parent)
console.log(enfant.toString()); // "[object Object]" (Object.prototype)
```
{% endtab %}

{% tab title="Visualisation de la chaîne" %}
```javascript
function afficherChaînePrototypale(obj, nom = "objet") {
  console.log(`\n🔍 Chaîne prototypale de ${nom}:`);
  
  let current = obj;
  let niveau = 0;
  
  while (current !== null) {
    const constructorName = current.constructor?.name || "Unknown";
    const isBuiltIn = current === Object.prototype || 
                     current === Function.prototype ||
                     current === Array.prototype;
    
    console.log(`  ${"  ".repeat(niveau)}↓ ${isBuiltIn ? '🔧' : '📦'} ${constructorName}`);
    
    // Afficher les propriétés propres
    const props = Object.getOwnPropertyNames(current);
    const visibleProps = props.filter(p => 
      p !== 'constructor' && 
      typeof current[p] !== 'function'
    ).slice(0, 3); // Limiter l'affichage
    
    if (visibleProps.length > 0) {
      console.log(`  ${"  ".repeat(niveau + 1)}   ${visibleProps.join(', ')}`);
    }
    
    current = Object.getPrototypeOf(current);
    niveau++;
    
    if (niveau > 10) break; // Sécurité
  }
  
  console.log(`  ${"  ".repeat(niveau)}↓ null`);
}

// Test avec notre exemple
afficherChaînePrototypale(enfant, "enfant");
```
{% endtab %}
{% endtabs %}

## 🏗️ Constructor Functions et Prototypes

### 🎭 L'approche "classique" de JavaScript

{% tabs %}
{% tab title="Constructor Function basique" %}
```javascript
// Constructor function (convention : PascalCase)
function Animal(nom, type) {
  // Propriétés d'instance
  this.nom = nom;
  this.type = type;
  this.énergie = 100;
}

// Méthodes partagées via prototype
Animal.prototype.manger = function() {
  this.énergie += 10;
  console.log(`${this.nom} mange et a maintenant ${this.énergie} d'énergie`);
};

Animal.prototype.dormir = function() {
  this.énergie += 20;
  console.log(`${this.nom} dort et récupère de l'énergie`);
};

Animal.prototype.info = function() {
  return `${this.nom} est un ${this.type} avec ${this.énergie} d'énergie`;
};

// Création d'instances
const chat = new Animal("Miaou", "chat");
const chien = new Animal("Rex", "chien");

chat.manger(); // "Miaou mange et a maintenant 110 d'énergie"
chien.dormir(); // "Rex dort et récupère de l'énergie"

console.log(chat.info()); // "Miaou est un chat avec 110 d'énergie"

// Vérifications
console.log(chat instanceof Animal); // true
console.log(chat.constructor === Animal); // true
console.log(Object.getPrototypeOf(chat) === Animal.prototype); // true
```
{% endtab %}

{% tab title="Héritage avec constructor functions" %}
```javascript
// Constructor parent
function Animal(nom, type) {
  this.nom = nom;
  this.type = type;
  this.énergie = 100;
}

Animal.prototype.manger = function() {
  this.énergie += 10;
  return `${this.nom} mange`;
};

Animal.prototype.dormir = function() {
  this.énergie += 20;
  return `${this.nom} dort`;
};

// Constructor enfant
function Chien(nom, race) {
  // Appeler le constructor parent
  Animal.call(this, nom, "chien");
  this.race = race;
}

// Créer l'héritage prototypal
Chien.prototype = Object.create(Animal.prototype);
Chien.prototype.constructor = Chien; // Restaurer le constructor

// Ajouter des méthodes spécifiques
Chien.prototype.aboyer = function() {
  console.log(`${this.nom} aboie: Woof! Woof!`);
  this.énergie -= 5;
};

Chien.prototype.manger = function() {
  // Surcharger la méthode parent
  const result = Animal.prototype.manger.call(this);
  console.log(`${result} (comme un bon chien)`);
  return result;
};

// Test
const labrador = new Chien("Buddy", "Labrador");
console.log(labrador.info()); // Hérité d'Animal
labrador.aboyer(); // Méthode spécifique
labrador.manger(); // Méthode surchargée

// Vérifications d'héritage
console.log(labrador instanceof Chien); // true
console.log(labrador instanceof Animal); // true
console.log(labrador instanceof Object); // true
```
{% endtab %}
{% endtabs %}

## 🎨 Classes ES6 vs Prototypes

### 🆚 Comparaison syntaxique

{% tabs %}
{% tab title="Avec Classes ES6" %}
```javascript
class Animal {
  constructor(nom, type) {
    this.nom = nom;
    this.type = type;
    this.énergie = 100;
  }
  
  manger() {
    this.énergie += 10;
    return `${this.nom} mange`;
  }
  
  dormir() {
    this.énergie += 20;
    return `${this.nom} dort`;
  }
  
  get info() {
    return `${this.nom} est un ${this.type} avec ${this.énergie} d'énergie`;
  }
  
  static espèce() {
    return "Être vivant";
  }
}

class Chien extends Animal {
  constructor(nom, race) {
    super(nom, "chien");
    this.race = race;
  }
  
  aboyer() {
    console.log(`${this.nom} aboie: Woof!`);
    this.énergie -= 5;
  }
  
  manger() {
    const result = super.manger();
    console.log(`${result} (comme un bon chien)`);
    return result;
  }
}

const rex = new Chien("Rex", "Berger");
console.log(rex.info); // getter
rex.aboyer();
```
{% endtab %}

{% tab title="Équivalent prototypal" %}
```javascript
// Exactement le même comportement avec les prototypes
function Animal(nom, type) {
  this.nom = nom;
  this.type = type;
  this.énergie = 100;
}

Animal.prototype.manger = function() {
  this.énergie += 10;
  return `${this.nom} mange`;
};

Animal.prototype.dormir = function() {
  this.énergie += 20;
  return `${this.nom} dort`;
};

Object.defineProperty(Animal.prototype, 'info', {
  get: function() {
    return `${this.nom} est un ${this.type} avec ${this.énergie} d'énergie`;
  }
});

Animal.espèce = function() {
  return "Être vivant";
};

function Chien(nom, race) {
  Animal.call(this, nom, "chien");
  this.race = race;
}

Chien.prototype = Object.create(Animal.prototype);
Chien.prototype.constructor = Chien;

Chien.prototype.aboyer = function() {
  console.log(`${this.nom} aboie: Woof!`);
  this.énergie -= 5;
};

Chien.prototype.manger = function() {
  const result = Animal.prototype.manger.call(this);
  console.log(`${result} (comme un bon chien)`);
  return result;
};

// Résultat identique !
const rex = new Chien("Rex", "Berger");
console.log(rex.info);
rex.aboyer();
```
{% endtab %}
{% endtabs %}

### 🎯 Sous le capot

{% hint style="success" %}
**Les classes ES6 sont du "sucre syntaxique" !**

Elles génèrent exactement le même code prototypal que l'approche traditionnelle, mais avec une syntaxe plus familière pour les développeurs venant d'autres langages.
{% endhint %}

```javascript
// Preuve que les classes utilisent les prototypes
class MaClasse {
  méthode() {
    return "test";
  }
}

const instance = new MaClasse();

console.log(typeof MaClasse); // "function" (pas "class"!)
console.log(MaClasse.prototype.méthode); // function méthode() { return "test"; }
console.log(instance.__proto__ === MaClasse.prototype); // true
```

## 🔧 Méthodes avancées de manipulation

### 🛠️ Object.create() et alternatives

{% tabs %}
{% tab title="Object.create()" %}
```javascript
// Object.create() : la méthode moderne recommandée
const animalPrototype = {
  init(nom, type) {
    this.nom = nom;
    this.type = type;
    this.énergie = 100;
    return this;
  },
  
  manger() {
    this.énergie += 10;
    console.log(`${this.nom} mange`);
  },
  
  info() {
    return `${this.nom} (${this.type}): ${this.énergie} énergie`;
  }
};

// Créer des objets avec ce prototype
const chat = Object.create(animalPrototype).init("Miaou", "chat");
const chien = Object.create(animalPrototype).init("Rex", "chien");

chat.manger();
console.log(chat.info());

// Avec des propriétés supplémentaires
const oiseau = Object.create(animalPrototype, {
  peutVoler: {
    value: true,
    writable: false,
    enumerable: true
  },
  altitude: {
    value: 0,
    writable: true
  }
});

oiseau.init("Tweety", "canari");
console.log(oiseau.peutVoler); // true
```
{% endtab %}

{% tab title="Factory Functions" %}
```javascript
// Pattern Factory avec prototypes
function créerAnimal(nom, type, spécialités = {}) {
  // Prototype de base
  const animalProto = {
    manger() {
      this.énergie += 10;
      return this;
    },
    
    dormir() {
      this.énergie += 20;
      return this;
    },
    
    info() {
      return `${this.nom} (${this.type}): ${this.énergie} énergie`;
    }
  };
  
  // Créer l'objet avec le prototype
  const animal = Object.create(animalProto);
  
  // Propriétés d'instance
  animal.nom = nom;
  animal.type = type;
  animal.énergie = 100;
  
  // Ajouter les spécialités
  Object.assign(animal, spécialités);
  
  return animal;
}

// Utilisation
const dauphin = créerAnimal("Flipper", "dauphin", {
  nager() {
    console.log(`${this.nom} nage gracieusement`);
    this.énergie -= 5;
    return this;
  },
  
  saut() {
    console.log(`${this.nom} fait un saut spectaculaire!`);
    this.énergie -= 15;
    return this;
  }
});

// Chaînage de méthodes
dauphin.nager().saut().manger().dormir();
console.log(dauphin.info());
```
{% endtab %}
{% endtabs %}

### 🎛️ Mixins et composition

{% tabs %}
{% tab title="Système de Mixins" %}
```javascript
// Mixins : ajouter des comportements à des objets
const Volant = {
  voler() {
    console.log(`${this.nom} vole dans le ciel`);
    this.énergie -= 10;
    return this;
  },
  
  atterrir() {
    console.log(`${this.nom} atterrit`);
    this.énergie -= 2;
    return this;
  }
};

const Nageur = {
  nager() {
    console.log(`${this.nom} nage`);
    this.énergie -= 5;
    return this;
  },
  
  plonger() {
    console.log(`${this.nom} plonge`);
    this.énergie -= 8;
    return this;
  }
};

const Terrestre = {
  courir() {
    console.log(`${this.nom} court`);
    this.énergie -= 7;
    return this;
  },
  
  marcher() {
    console.log(`${this.nom} marche`);
    this.énergie -= 3;
    return this;
  }
};

// Fonction pour appliquer des mixins
function appliquerMixins(target, ...mixins) {
  mixins.forEach(mixin => {
    Object.assign(target.prototype || target, mixin);
  });
  return target;
}

// Créer des animaux spécialisés
function Canard(nom) {
  this.nom = nom;
  this.type = "canard";
  this.énergie = 100;
}

// Le canard peut voler, nager ET marcher
appliquerMixins(Canard, Volant, Nageur, Terrestre);

const donald = new Canard("Donald");
donald.nager().voler().atterrir().marcher();
console.log(`Énergie restante: ${donald.énergie}`);
```
{% endtab %}

{% tab title="Composition avancée" %}
```javascript
// Système de composition plus sophistiqué
class CompositeBuilder {
  constructor() {
    this.behaviors = new Map();
  }
  
  addBehavior(name, behavior) {
    this.behaviors.set(name, behavior);
    return this;
  }
  
  compose(target) {
    for (const [name, behavior] of this.behaviors) {
      if (typeof behavior === 'function') {
        target[name] = behavior.bind(target);
      } else if (typeof behavior === 'object') {
        Object.assign(target, behavior);
      }
    }
    return target;
  }
  
  createFactory(baseProps = {}) {
    return (props = {}) => {
      const instance = Object.assign({}, baseProps, props);
      return this.compose(instance);
    };
  }
}

// Définir des comportements
const builder = new CompositeBuilder()
  .addBehavior('parler', function(message) {
    console.log(`${this.nom} dit: "${message}"`);
    return this;
  })
  .addBehavior('manger', function() {
    this.faim = Math.max(0, this.faim - 20);
    console.log(`${this.nom} mange. Faim: ${this.faim}`);
    return this;
  })
  .addBehavior('repos', function() {
    this.fatigue = Math.max(0, this.fatigue - 30);
    console.log(`${this.nom} se repose. Fatigue: ${this.fatigue}`);
    return this;
  });

// Créer une factory
const créerPersonnage = builder.createFactory({
  faim: 50,
  fatigue: 30,
  humeur: "neutre"
});

// Utilisation
const héros = créerPersonnage({ nom: "Gandalf", type: "magicien" });
héros.parler("Tu ne passeras pas!")
     .manger()
     .repos()
     .parler("Maintenant je me sens mieux");

console.log(héros);
```
{% endtab %}
{% endtabs %}

## ⚠️ Pièges et bonnes pratiques

### 🚨 Erreurs communes

{% tabs %}
{% tab title="Modification des prototypes natifs" %}
```javascript
// ❌ DANGEREUX : Ne jamais modifier les prototypes natifs
Array.prototype.dernierÉlément = function() {
  return this[this.length - 1];
};

// ❌ Cela peut casser d'autres librairies
String.prototype.inverser = function() {
  return this.split('').reverse().join('');
};

// ✅ Bonnes alternatives :
function obtenirDernierÉlément(arr) {
  return arr[arr.length - 1];
}

function inverserChaîne(str) {
  return str.split('').reverse().join('');
}

// ✅ Ou créer vos propres classes
class MonArray extends Array {
  get dernierÉlément() {
    return this[this.length - 1];
  }
}

const arr = new MonArray(1, 2, 3);
console.log(arr.dernierÉlément); // 3
```
{% endtab %}

{% tab title="Problèmes avec 'this'" %}
```javascript
function Animal(nom) {
  this.nom = nom;
}

Animal.prototype.présenter = function() {
  return `Je suis ${this.nom}`;
};

const chat = new Animal("Miaou");

// ❌ Problème : perte du contexte 'this'
const présentation = chat.présenter;
console.log(présentation()); // "Je suis undefined"

// ✅ Solutions :
// 1. bind()
const présentationLiée = chat.présenter.bind(chat);
console.log(présentationLiée()); // "Je suis Miaou"

// 2. Arrow function dans un wrapper
const wrapper = () => chat.présenter();
console.log(wrapper()); // "Je suis Miaou"

// 3. call() ou apply()
console.log(chat.présenter.call(chat)); // "Je suis Miaou"

// ✅ Pattern moderne : méthodes avec arrow functions
function ModernAnimal(nom) {
  this.nom = nom;
  
  // Arrow function lie automatiquement 'this'
  this.présenter = () => {
    return `Je suis ${this.nom}`;
  };
}

const chienModerne = new ModernAnimal("Rex");
const prés = chienModerne.présenter;
console.log(prés()); // "Je suis Rex" ✅
```
{% endtab %}

{% tab title="Performance et mémoire" %}
```javascript
// ❌ Méthodes dans le constructor (dupliquées)
function AnimalInefficient(nom) {
  this.nom = nom;
  
  // Chaque instance a sa propre copie de ces méthodes !
  this.manger = function() {
    console.log(`${this.nom} mange`);
  };
  
  this.dormir = function() {
    console.log(`${this.nom} dort`);
  };
}

// ✅ Méthodes dans le prototype (partagées)
function AnimalEfficient(nom) {
  this.nom = nom;
}

AnimalEfficient.prototype.manger = function() {
  console.log(`${this.nom} mange`);
};

AnimalEfficient.prototype.dormir = function() {
  console.log(`${this.nom} dort`);
};

// Test de performance
console.time("Inefficient");
const animauxInefficients = [];
for (let i = 0; i < 10000; i++) {
  animauxInefficients.push(new AnimalInefficient(`Animal${i}`));
}
console.timeEnd("Inefficient");

console.time("Efficient");
const animauxEfficients = [];
for (let i = 0; i < 10000; i++) {
  animauxEfficients.push(new AnimalEfficient(`Animal${i}`));
}
console.timeEnd("Efficient");

// Vérification mémoire
console.log("Première méthode même référence:", 
  animauxEfficients[0].manger === animauxEfficients[1].manger); // true

console.log("Deuxième méthode même référence:", 
  animauxInefficients[0].manger === animauxInefficients[1].manger); // false
```
{% endtab %}
{% endtabs %}

## 🧪 Exercices pratiques

### 🎯 Exercice 1 : Système de véhicules

{% hint style="info" %}
**Créez une hiérarchie de véhicules avec prototypes :**
{% endhint %}

```javascript
// Créez :
// - Vehicule (classe de base)
// - Voiture, Moto (héritent de Vehicule)
// - VoitureElectrique (hérite de Voiture)
// 
// Propriétés : marque, modèle, année, carburant, autonomie
// Méthodes : démarrer(), arrêter(), rouler(distance), faireLePlein()

// Utilisation attendue :
const tesla = new VoitureElectrique("Tesla", "Model 3", 2023);
tesla.démarrer();
tesla.rouler(100);
tesla.recharger(); // Méthode spécifique aux électriques
```

<details>
<summary>💡 Solution</summary>

```javascript
// Classe de base Vehicule
function Vehicule(marque, modèle, année) {
  this.marque = marque;
  this.modèle = modèle;
  this.année = année;
  this.enMarche = false;
  this.kilométrage = 0;
}

Vehicule.prototype.démarrer = function() {
  if (!this.enMarche) {
    this.enMarche = true;
    console.log(`${this.marque} ${this.modèle} a démarré`);
  } else {
    console.log("Le véhicule est déjà en marche");
  }
  return this;
};

Vehicule.prototype.arrêter = function() {
  if (this.enMarche) {
    this.enMarche = false;
    console.log(`${this.marque} ${this.modèle} s'est arrêté`);
  } else {
    console.log("Le véhicule est déjà arrêté");
  }
  return this;
};

Vehicule.prototype.rouler = function(distance) {
  if (!this.enMarche) {
    console.log("Démarrez d'abord le véhicule!");
    return this;
  }
  
  this.kilométrage += distance;
  console.log(`A roulé ${distance}km. Kilométrage total: ${this.kilométrage}km`);
  return this;
};

Vehicule.prototype.info = function() {
  return `${this.marque} ${this.modèle} (${this.année}) - ${this.kilométrage}km`;
};

// Voiture
function Voiture(marque, modèle, année, typeCarburant = "essence") {
  Vehicule.call(this, marque, modèle, année);
  this.typeCarburant = typeCarburant;
  this.réservoir = 50; // litres
  this.consommation = 7; // L/100km
}

Voiture.prototype = Object.create(Vehicule.prototype);
Voiture.prototype.constructor = Voiture;

Voiture.prototype.faireLePlein = function() {
  this.réservoir = 50;
  console.log(`${this.marque} ${this.modèle} a fait le plein`);
  return this;
};

Voiture.prototype.rouler = function(distance) {
  if (!this.enMarche) {
    console.log("Démarrez d'abord le véhicule!");
    return this;
  }
  
  const carburantNécessaire = (distance * this.consommation) / 100;
  
  if (carburantNécessaire > this.réservoir) {
    console.log("Pas assez de carburant!");
    return this;
  }
  
  this.réservoir -= carburantNécessaire;
  this.kilométrage += distance;
  console.log(`A roulé ${distance}km. Carburant restant: ${this.réservoir.toFixed(1)}L`);
  return this;
};

// Voiture Électrique
function VoitureElectrique(marque, modèle, année) {
  Voiture.call(this, marque, modèle, année, "électrique");
  this.batterie = 100; // %
  this.autonomie = 400; // km
  this.consommationElectrique = 15; // kWh/100km
}

VoitureElectrique.prototype = Object.create(Voiture.prototype);
VoitureElectrique.prototype.constructor = VoitureElectrique;

VoitureElectrique.prototype.recharger = function() {
  this.batterie = 100;
  console.log(`${this.marque} ${this.modèle} est rechargée à 100%`);
  return this;
};

VoitureElectrique.prototype.rouler = function(distance) {
  if (!this.enMarche) {
    console.log("Démarrez d'abord le véhicule!");
    return this;
  }
  
  const batterieNécessaire = (distance / this.autonomie) * 100;
  
  if (batterieNécessaire > this.batterie) {
    console.log("Batterie insuffisante!");
    return this;
  }
  
  this.batterie -= batterieNécessaire;
  this.kilométrage += distance;
  console.log(`A roulé ${distance}km. Batterie: ${this.batterie.toFixed(1)}%`);
  return this;
};

// Test
const tesla = new VoitureElectrique("Tesla", "Model 3", 2023);
tesla.démarrer()
     .rouler(100)
     .rouler(200)
     .recharger()
     .rouler(150);

console.log(tesla.info());
```

</details>

### 🎯 Exercice 2 : Plugin System

{% hint style="info" %}
**Créez un système de plugins extensible :**
{% endhint %}

```javascript
// Créez un système où :
// - Une classe de base App peut être étendue avec des plugins
// - Les plugins ajoutent des méthodes et propriétés
// - Les plugins peuvent dépendre d'autres plugins
// - On peut activer/désactiver des plugins dynamiquement

// Utilisation attendue :
const app = new App();
app.use(Logger);
app.use(Database, { host: 'localhost' });
app.use(Router);

app.log('Application démarrée');
app.route('/api', handler);
```

<details>
<summary>💡 Solution</summary>

```javascript
// Système de plugins avancé
class App {
  constructor() {
    this.plugins = new Map();
    this.pluginInstances = new Map();
    this.hooks = new Map();
    this.config = {};
  }
  
  use(PluginClass, options = {}) {
    const pluginName = PluginClass.pluginName || PluginClass.name;
    
    // Vérifier les dépendances
    if (PluginClass.dependencies) {
      for (const dep of PluginClass.dependencies) {
        if (!this.plugins.has(dep)) {
          throw new Error(`Plugin ${pluginName} requires ${dep}`);
        }
      }
    }
    
    // Créer l'instance du plugin
    const plugin = new PluginClass(this, options);
    
    // Enregistrer
    this.plugins.set(pluginName, PluginClass);
    this.pluginInstances.set(pluginName, plugin);
    
    // Installer les méthodes du plugin
    if (plugin.install) {
      plugin.install();
    }
    
    // Déclencher le hook d'installation
    this.triggerHook('plugin:installed', pluginName);
    
    console.log(`Plugin ${pluginName} installé`);
    return this;
  }
  
  hasPlugin(name) {
    return this.plugins.has(name);
  }
  
  getPlugin(name) {
    return this.pluginInstances.get(name);
  }
  
  removePlugin(name) {
    const plugin = this.pluginInstances.get(name);
    if (plugin && plugin.uninstall) {
      plugin.uninstall();
    }
    
    this.plugins.delete(name);
    this.pluginInstances.delete(name);
    this.triggerHook('plugin:removed', name);
  }
  
  // Système de hooks
  addHook(event, callback) {
    if (!this.hooks.has(event)) {
      this.hooks.set(event, []);
    }
    this.hooks.get(event).push(callback);
  }
  
  triggerHook(event, ...args) {
    const callbacks = this.hooks.get(event) || [];
    callbacks.forEach(callback => callback(...args));
  }
}

// Plugin Logger
class Logger {
  static pluginName = 'Logger';
  
  constructor(app, options = {}) {
    this.app = app;
    this.level = options.level || 'info';
    this.prefix = options.prefix || '[LOG]';
  }
  
  install() {
    // Ajouter les méthodes à l'app
    this.app.log = (message, level = 'info') => {
      if (this.shouldLog(level)) {
        console.log(`${this.prefix} [${level.toUpperCase()}] ${message}`);
      }
    };
    
    this.app.error = (message) => this.app.log(message, 'error');
    this.app.warn = (message) => this.app.log(message, 'warn');
    this.app.info = (message) => this.app.log(message, 'info');
    this.app.debug = (message) => this.app.log(message, 'debug');
  }
  
  shouldLog(level) {
    const levels = { debug: 0, info: 1, warn: 2, error: 3 };
    return levels[level] >= levels[this.level];
  }
  
  uninstall() {
    delete this.app.log;
    delete this.app.error;
    delete this.app.warn;
    delete this.app.info;
    delete this.app.debug;
  }
}

// Plugin Database
class Database {
  static pluginName = 'Database';
  static dependencies = ['Logger'];
  
  constructor(app, options = {}) {
    this.app = app;
    this.host = options.host || 'localhost';
    this.port = options.port || 5432;
    this.connected = false;
  }
  
  install() {
    this.app.db = {
      connect: () => this.connect(),
      query: (sql) => this.query(sql),
      disconnect: () => this.disconnect()
    };
  }
  
  connect() {
    this.connected = true;
    this.app.log(`Connected to database at ${this.host}:${this.port}`);
    return Promise.resolve();
  }
  
  query(sql) {
    if (!this.connected) {
      this.app.error('Database not connected');
      return Promise.reject(new Error('Not connected'));
    }
    this.app.debug(`Executing query: ${sql}`);
    return Promise.resolve({ rows: [], affected: 0 });
  }
  
  disconnect() {
    this.connected = false;
    this.app.log('Disconnected from database');
  }
  
  uninstall() {
    this.disconnect();
    delete this.app.db;
  }
}

// Plugin Router
class Router {
  static pluginName = 'Router';
  static dependencies = ['Logger'];
  
  constructor(app, options = {}) {
    this.app = app;
    this.routes = new Map();
  }
  
  install() {
    this.app.route = (path, handler) => this.addRoute(path, handler);
    this.app.handleRequest = (path) => this.handleRequest(path);
  }
  
  addRoute(path, handler) {
    this.routes.set(path, handler);
    this.app.log(`Route registered: ${path}`);
  }
  
  handleRequest(path) {
    const handler = this.routes.get(path);
    if (handler) {
      this.app.log(`Handling request: ${path}`);
      return handler();
    } else {
      this.app.warn(`Route not found: ${path}`);
      return null;
    }
  }
  
  uninstall() {
    delete this.app.route;
    delete this.app.handleRequest;
  }
}

// Test du système
const app = new App();

try {
  app.use(Logger, { level: 'debug' })
     .use(Database, { host: 'localhost', port: 5432 })
     .use(Router);
  
  app.log('Application started');
  app.route('/api/users', () => 'Users data');
  app.route('/api/posts', () => 'Posts data');
  
  app.db.connect().then(() => {
    app.handleRequest('/api/users');
    app.handleRequest('/api/unknown');
  });
  
} catch (error) {
  console.error('Plugin error:', error.message);
}
```

</details>

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Quelle est la différence entre `__proto__` et `prototype` ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**`__proto__`** : Propriété de **chaque objet** qui pointe vers son prototype  
**`prototype`** : Propriété des **fonctions** utilisée pour créer le prototype des instances

```javascript
function Personne() {}
const alice = new Personne();

// alice.__proto__ === Personne.prototype (true)
// alice.prototype === undefined (les objets n'ont pas 'prototype')
// Personne.__proto__ === Function.prototype (true)
```

</details>

{% hint style="info" %}
**Question 2 :** Pourquoi éviter de modifier les prototypes natifs ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Risques :**
- 🔥 **Conflits** avec d'autres librairies
- 🐛 **Comportement imprévisible** du code existant
- 🚫 **Standards** : violation des bonnes pratiques
- 🔄 **Évolution** : les futures versions JS peuvent ajouter ces méthodes

**Alternative :** Créer vos propres classes ou fonctions utilitaires.

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="../closures-lexical-scope.md" %}
[closures-lexical-scope.md](../closures-lexical-scope.md)
{% endcontent-ref %}

{% content-ref url="../classes-poo.md" %}
[classes-poo.md](../classes-poo.md)
{% endcontent-ref %}

{% content-ref url="../../niveau-avance/design-patterns.md" %}
[design-patterns.md](../../niveau-avance/design-patterns.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Maintenant que vous maîtrisez les prototypes :**

👉 **Approfondissez** les [Classes et POO](../classes-poo.md)  
👉 **Explorez** les [Design Patterns](../../niveau-avance/design-patterns.md)  
👉 **Découvrez** les [Modules et Organisation](../../niveau-avance/modules-organisation.md)  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🧬 **Prototypes = base de JavaScript** - tout objet hérite d'un prototype  
⛓️ **Chaîne prototypale** - recherche des propriétés en remontant  
🎭 **Classes ES6 = sucre syntaxique** sur les prototypes  
🏗️ **Constructor functions** - approche classique pour créer des "classes"  
⚠️ **Ne jamais modifier** les prototypes natifs  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Les prototypes sont la fondation des [Classes et de la POO](../classes-poo.md) !
{% endhint %}
