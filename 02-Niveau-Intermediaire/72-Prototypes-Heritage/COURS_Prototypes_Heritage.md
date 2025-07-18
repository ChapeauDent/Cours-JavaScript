# Prototypes et Héritage en JavaScript

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** Prototypes et Héritage Prototypal (Base de JavaScript)
- **Niveau :** Intermédiaire (concept fondamental)
- **Durée estimée :** 135 minutes
- **Prérequis :** Objets, fonctions, classes ES6, this, closures
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est un prototype et la chaîne prototypale
- [ ] **Différencier** : Héritage prototypal vs héritage classique
- [ ] **Manipuler** : Les prototypes avec Object.create, __proto__, prototype
- [ ] **Créer** : Des hiérarchies d'objets avec l'héritage prototypal
- [ ] **Déboguer** : Les problèmes liés aux prototypes et au this

### Mots-clés à retenir
- **Prototype** : Objet modèle dont héritent d'autres objets
- **Chaîne prototypale** : Succession de prototypes jusqu'à null
- **Héritage prototypal** : Mécanisme d'héritage natif de JavaScript
- **Constructor function** : Fonction utilisée avec `new` pour créer des objets
- **Prototype pollution** : Modification dangereuse des prototypes natifs

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les prototypes sont **LE CŒUR** de JavaScript. Contrairement aux langages comme Java/C#, JavaScript utilise l'héritage prototypal, pas l'héritage classique.

**Pourquoi c'est crucial :**
- ✅ **Base de tout** : Tous les objets JavaScript héritent de prototypes
- ✅ **Classes ES6** : Sont du "sucre syntaxique" sur les prototypes
- ✅ **Performance** : Méthodes partagées via prototypes (pas dupliquées)
- ✅ **Flexibilité** : Modification dynamique des "classes"
- ✅ **Frameworks** : React, Vue utilisent massivement les prototypes

**Sans comprendre les prototypes, on ne comprend pas vraiment JavaScript !**

### Définition et concepts clés

#### **1. Qu'est-ce qu'un prototype ?**

Chaque objet JavaScript a un lien vers un autre objet appelé son **prototype**. Si une propriété n'existe pas sur l'objet, JavaScript la cherche dans son prototype, puis dans le prototype du prototype, etc.

```javascript
// Exemple basique
let animal = {
    type: "animal",
    respirer: function() {
        console.log("Je respire");
    }
};

let chien = {
    race: "Labrador"
};

// Faire hériter chien d'animal
Object.setPrototypeOf(chien, animal);

console.log(chien.race); // "Labrador" (propriété propre)
console.log(chien.type); // "animal" (trouvée dans le prototype)
chien.respirer(); // "Je respire" (méthode du prototype)
```

#### **2. La chaîne prototypale**

JavaScript remonte la chaîne des prototypes jusqu'à trouver la propriété ou atteindre `null`.

```javascript
let objetBase = {
    niveau: "base"
};

let objetMilieu = {
    niveau: "milieu", // Cette propriété "masque" celle du prototype
    proprieteUnique: "unique"
};

let objetFinal = {
    nom: "final"
};

// Créer la chaîne : objetFinal → objetMilieu → objetBase → Object.prototype → null
Object.setPrototypeOf(objetMilieu, objetBase);
Object.setPrototypeOf(objetFinal, objetMilieu);

console.log(objetFinal.nom); // "final" (propriété propre)
console.log(objetFinal.proprieteUnique); // "unique" (premier niveau parent)
console.log(objetFinal.niveau); // "milieu" (masque "base")
console.log(objetFinal.toString()); // [object Object] (vient d'Object.prototype)
```

#### **3. Constructor Functions vs Object.create**

**Méthode 1 : Constructor Functions (style ancien)**
```javascript
// Constructor function (convention: première lettre majuscule)
function Animal(nom, type) {
    this.nom = nom;
    this.type = type;
}

// Ajouter des méthodes au prototype
Animal.prototype.respirer = function() {
    console.log(`${this.nom} respire`);
};

Animal.prototype.sePresenter = function() {
    console.log(`Je suis ${this.nom}, un ${this.type}`);
};

// Créer des instances
let chat = new Animal("Minou", "chat");
let chien = new Animal("Rex", "chien");

chat.respirer(); // "Minou respire"
chien.sePresenter(); // "Je suis Rex, un chien"

// Les méthodes sont partagées (même référence)
console.log(chat.respirer === chien.respirer); // true
```

**Méthode 2 : Object.create (plus moderne)**
```javascript
// Créer un prototype
let AnimalPrototype = {
    init: function(nom, type) {
        this.nom = nom;
        this.type = type;
        return this;
    },
    respirer: function() {
        console.log(`${this.nom} respire`);
    },
    sePresenter: function() {
        console.log(`Je suis ${this.nom}, un ${this.type}`);
    }
};

// Créer des objets qui héritent du prototype
let chat = Object.create(AnimalPrototype).init("Minou", "chat");
let chien = Object.create(AnimalPrototype).init("Rex", "chien");

chat.respirer(); // "Minou respire"
chien.sePresenter(); // "Je suis Rex, un chien"
```

#### **4. Classes ES6 vs Prototypes**

```javascript
// Classe ES6 (sucre syntaxique)
class Animal {
    constructor(nom, type) {
        this.nom = nom;
        this.type = type;
    }
    
    respirer() {
        console.log(`${this.nom} respire`);
    }
    
    sePresenter() {
        console.log(`Je suis ${this.nom}, un ${this.type}`);
    }
}

// Équivalent en prototypes
function Animal(nom, type) {
    this.nom = nom;
    this.type = type;
}

Animal.prototype.respirer = function() {
    console.log(`${this.nom} respire`);
};

Animal.prototype.sePresenter = function() {
    console.log(`Je suis ${this.nom}, un ${this.type}`);
};

// Les deux créent exactement la même structure !
```

### Concepts avancés

#### **5. Héritage prototypal avec Object.create**

```javascript
// Parent prototype
let VehiculePrototype = {
    init: function(marque, modele) {
        this.marque = marque;
        this.modele = modele;
        return this;
    },
    
    demarrer: function() {
        console.log(`${this.marque} ${this.modele} démarre`);
    },
    
    arreter: function() {
        console.log(`${this.marque} ${this.modele} s'arrête`);
    }
};

// Child prototype héritant du parent
let VoiturePrototype = Object.create(VehiculePrototype);

VoiturePrototype.initVoiture = function(marque, modele, portes) {
    // Appeler la méthode parent
    this.init(marque, modele);
    this.portes = portes;
    return this;
};

VoiturePrototype.ouvrirPortes = function() {
    console.log(`Ouverture des ${this.portes} portes`);
};

// Utilisation
let maVoiture = Object.create(VoiturePrototype)
    .initVoiture("Toyota", "Camry", 4);

maVoiture.demarrer(); // Méthode héritée
maVoiture.ouvrirPortes(); // Méthode propre
maVoiture.arreter(); // Méthode héritée
```

#### **6. Inspection des prototypes**

```javascript
let animal = { type: "animal" };
let chien = Object.create(animal);
chien.race = "Labrador";

// Vérifications
console.log(chien.hasOwnProperty("race")); // true
console.log(chien.hasOwnProperty("type")); // false (dans le prototype)

console.log("race" in chien); // true
console.log("type" in chien); // true (trouve dans le prototype)

// Obtenir le prototype
console.log(Object.getPrototypeOf(chien) === animal); // true

// Lister les propriétés propres
console.log(Object.getOwnPropertyNames(chien)); // ["race"]

// Vérifier l'héritage
console.log(animal.isPrototypeOf(chien)); // true
```

### Points d'attention et pièges

#### **Piège 1 : Modification de propriétés référence**
```javascript
function Equipe(nom) {
    this.nom = nom;
}

// ❌ ERREUR : Array partagé entre toutes les instances
Equipe.prototype.membres = [];

Equipe.prototype.ajouterMembre = function(membre) {
    this.membres.push(membre);
};

let equipe1 = new Equipe("Alpha");
let equipe2 = new Equipe("Beta");

equipe1.ajouterMembre("Alice");
equipe2.ajouterMembre("Bob");

console.log(equipe1.membres); // ["Alice", "Bob"] ❌ 
console.log(equipe2.membres); // ["Alice", "Bob"] ❌ Partagé !

// ✅ SOLUTION : Initialiser dans le constructor
function EquipeCorrigee(nom) {
    this.nom = nom;
    this.membres = []; // Chaque instance a son propre array
}
```

#### **Piège 2 : Perte du contexte this**
```javascript
function Personne(nom) {
    this.nom = nom;
}

Personne.prototype.saluer = function() {
    console.log(`Bonjour, je suis ${this.nom}`);
};

let alice = new Personne("Alice");
alice.saluer(); // "Bonjour, je suis Alice" ✅

// ❌ Problème avec extraction de méthode
let methode = alice.saluer;
methode(); // "Bonjour, je suis undefined" ❌

// ✅ Solutions
alice.saluer.call(alice); // Spécifier this
alice.saluer.bind(alice)(); // Lier this
```

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Démonstration des prototypes de base
console.log("=== PROTOTYPES DE BASE ===");

// 1. Créer un objet simple avec prototype
let animal = {
    dormir: function() {
        console.log("😴 L'animal dort");
    },
    manger: function(nourriture) {
        console.log(`😋 L'animal mange ${nourriture}`);
    }
};

// 2. Créer un objet qui hérite d'animal
let chien = Object.create(animal);
chien.aboyer = function() {
    console.log("🐕 Wouf wouf!");
};

// 3. Test des méthodes
console.log("Test chien:");
chien.dormir(); // Méthode héritée
chien.aboyer(); // Méthode propre
chien.manger("croquettes"); // Méthode héritée

// 4. Vérifications
console.log("Vérifications:");
console.log("chien a dormir:", "dormir" in chien); // true
console.log("chien possède dormir:", chien.hasOwnProperty("dormir")); // false
console.log("chien possède aboyer:", chien.hasOwnProperty("aboyer")); // true

// 5. Chaîne prototypale
console.log("Chaîne prototypale:");
console.log("Prototype de chien:", Object.getPrototypeOf(chien) === animal); // true
console.log("Prototype d'animal:", Object.getPrototypeOf(animal) === Object.prototype); // true
console.log("Prototype d'Object:", Object.getPrototypeOf(Object.prototype)); // null

// 6. Constructor function basique
function Chat(nom, couleur) {
    this.nom = nom;
    this.couleur = couleur;
}

Chat.prototype.miauler = function() {
    console.log(`🐱 ${this.nom} fait: Miaou!`);
};

Chat.prototype.seDecrire = function() {
    console.log(`🐱 Je suis ${this.nom}, un chat ${this.couleur}`);
};

let felix = new Chat("Félix", "noir");
let minou = new Chat("Minou", "blanc");

felix.miauler();
felix.seDecrire();
minou.miauler();

// Vérification du partage de méthodes
console.log("Méthodes partagées:", felix.miauler === minou.miauler); // true
```

### Exemple intermédiaire (hiérarchie complète)
```javascript
// Système de hiérarchie d'objets avec prototypes
console.log("=== HIÉRARCHIE D'OBJETS ===");

// 1. Prototype de base : Être vivant
let EtreVivantPrototype = {
    init: function(nom, age) {
        this.nom = nom;
        this.age = age;
        this.energie = 100;
        return this;
    },
    
    vieillir: function() {
        this.age++;
        this.energie = Math.max(0, this.energie - 5);
        console.log(`${this.nom} vieillit, maintenant ${this.age} ans (énergie: ${this.energie})`);
    },
    
    seReposer: function() {
        this.energie = Math.min(100, this.energie + 20);
        console.log(`${this.nom} se repose (énergie: ${this.energie})`);
    },
    
    estVivant: function() {
        return this.energie > 0;
    }
};

// 2. Prototype Animal héritant d'ÊtreVivant
let AnimalPrototype = Object.create(EtreVivantPrototype);

AnimalPrototype.initAnimal = function(nom, age, espece) {
    this.init(nom, age);
    this.espece = espece;
    return this;
};

AnimalPrototype.deplacement = function(distance) {
    if (this.energie >= distance) {
        this.energie -= distance;
        console.log(`${this.nom} se déplace de ${distance}m (énergie: ${this.energie})`);
    } else {
        console.log(`${this.nom} est trop fatigué pour se déplacer`);
    }
};

AnimalPrototype.sePresenter = function() {
    console.log(`Je suis ${this.nom}, un ${this.espece} de ${this.age} ans`);
};

// 3. Prototype Mammifère héritant d'Animal
let MammiferePrototype = Object.create(AnimalPrototype);

MammiferePrototype.initMammifere = function(nom, age, espece, fourrure) {
    this.initAnimal(nom, age, espece);
    this.fourrure = fourrure;
    return this;
};

MammiferePrototype.regulerTemperature = function() {
    console.log(`${this.nom} régule sa température grâce à sa fourrure ${this.fourrure}`);
};

// 4. Prototype Chien héritant de Mammifère
let ChienPrototype = Object.create(MammiferePrototype);

ChienPrototype.initChien = function(nom, age, race) {
    this.initMammifere(nom, age, "chien", "dense");
    this.race = race;
    this.fidelite = 100;
    return this;
};

ChienPrototype.aboyer = function() {
    this.energie -= 2;
    console.log(`${this.nom} aboie: Wouf! (énergie: ${this.energie})`);
};

ChienPrototype.faireLeBeau = function() {
    this.energie -= 5;
    this.fidelite += 2;
    console.log(`${this.nom} fait le beau (fidélité: ${this.fidelite})`);
};

ChienPrototype.sePresenter = function() {
    // Appeler la méthode parent et ajouter des infos
    AnimalPrototype.sePresenter.call(this);
    console.log(`Je suis un ${this.race} avec une fidélité de ${this.fidelite}`);
};

// 5. Tests de la hiérarchie
let rex = Object.create(ChienPrototype)
    .initChien("Rex", 3, "Labrador");

console.log("=== Tests Rex ===");
rex.sePresenter(); // Méthode overridée
rex.aboyer(); // Méthode propre
rex.deplacement(10); // Méthode d'Animal
rex.seReposer(); // Méthode d'ÊtreVivant
rex.regulerTemperature(); // Méthode de Mammifère
rex.faireLeBeau();
rex.vieillir(); // Méthode d'ÊtreVivant

// 6. Inspection de la chaîne
console.log("=== Inspection de la chaîne prototypale ===");
console.log("Rex est un chien:", ChienPrototype.isPrototypeOf(rex));
console.log("Rex est un mammifère:", MammiferePrototype.isPrototypeOf(rex));
console.log("Rex est un animal:", AnimalPrototype.isPrototypeOf(rex));
console.log("Rex est un être vivant:", EtreVivantPrototype.isPrototypeOf(rex));

// 7. Propriétés propres vs héritées
console.log("=== Propriétés ===");
console.log("Propriétés propres:", Object.getOwnPropertyNames(rex));
console.log("A 'nom':", rex.hasOwnProperty("nom"));
console.log("A 'aboyer':", rex.hasOwnProperty("aboyer"));
console.log("Peut 'aboyer':", "aboyer" in rex);
```

### Exemple avancé (mixins et compositions)
```javascript
// Système avancé avec mixins et composition
console.log("=== SYSTÈME AVANCÉ AVEC MIXINS ===");

// 1. Mixins (fonctionnalités réutilisables)
let VolantMixin = {
    voler: function(altitude) {
        this.altitude = altitude;
        this.energie -= altitude / 10;
        console.log(`${this.nom} vole à ${altitude}m d'altitude (énergie: ${this.energie})`);
    },
    
    atterrir: function() {
        if (this.altitude > 0) {
            console.log(`${this.nom} atterrit depuis ${this.altitude}m`);
            this.altitude = 0;
        } else {
            console.log(`${this.nom} est déjà au sol`);
        }
    }
};

let NageurMixin = {
    nager: function(distance) {
        this.energie -= distance / 5;
        console.log(`${this.nom} nage ${distance}m (énergie: ${this.energie})`);
    },
    
    plonger: function(profondeur) {
        this.profondeur = profondeur;
        this.energie -= profondeur / 3;
        console.log(`${this.nom} plonge à ${profondeur}m (énergie: ${this.energie})`);
    }
};

let CommunicantMixin = {
    communiquer: function(message) {
        console.log(`${this.nom} communique: "${message}"`);
    },
    
    apprendreLangue: function(langue) {
        if (!this.langues) this.langues = [];
        this.langues.push(langue);
        console.log(`${this.nom} apprend ${langue}. Langues connues: ${this.langues.join(", ")}`);
    }
};

// 2. Fonction utilitaire pour appliquer des mixins
function appliquerMixins(prototype, ...mixins) {
    mixins.forEach(mixin => {
        Object.assign(prototype, mixin);
    });
    return prototype;
}

// 3. Prototype de base Animal (réutilisé)
let BaseAnimalPrototype = {
    init: function(nom, espece) {
        this.nom = nom;
        this.espece = espece;
        this.energie = 100;
        this.altitude = 0;
        this.profondeur = 0;
        return this;
    },
    
    sePresenter: function() {
        let capacites = [];
        if (this.voler) capacites.push("voler");
        if (this.nager) capacites.push("nager");
        if (this.communiquer) capacites.push("communiquer");
        
        console.log(`${this.nom} est un ${this.espece} qui peut: ${capacites.join(", ")}`);
    }
};

// 4. Créer différents types d'animaux avec mixins

// Oiseau (vole + communique)
let OiseauPrototype = Object.create(BaseAnimalPrototype);
appliquerMixins(OiseauPrototype, VolantMixin, CommunicantMixin);

OiseauPrototype.initOiseau = function(nom, espece) {
    this.init(nom, espece);
    this.langues = ["chant d'oiseau"];
    return this;
};

// Poisson (nage seulement)
let PoissonPrototype = Object.create(BaseAnimalPrototype);
appliquerMixins(PoissonPrototype, NageurMixin);

PoissonPrototype.initPoisson = function(nom, espece) {
    this.init(nom, espece);
    return this;
};

// Dauphin (nage + communique)
let DauphinPrototype = Object.create(BaseAnimalPrototype);
appliquerMixins(DauphinPrototype, NageurMixin, CommunicantMixin);

DauphinPrototype.initDauphin = function(nom) {
    this.init(nom, "dauphin");
    this.langues = ["sonar"];
    return this;
};

DauphinPrototype.echolocation = function() {
    console.log(`${this.nom} utilise l'écholocation`);
};

// Canard (vole + nage + communique) - Animal polyvalent!
let CanardPrototype = Object.create(BaseAnimalPrototype);
appliquerMixins(CanardPrototype, VolantMixin, NageurMixin, CommunicantMixin);

CanardPrototype.initCanard = function(nom) {
    this.init(nom, "canard");
    this.langues = ["coin-coin"];
    return this;
};

// 5. Tests des animaux avec mixins
console.log("=== Tests des animaux ===");

let aigle = Object.create(OiseauPrototype).initOiseau("Aigle", "rapace");
aigle.sePresenter();
aigle.voler(100);
aigle.communiquer("Cri perçant");
aigle.apprendreLangue("signal d'alarme");
aigle.atterrir();

console.log();

let nemo = Object.create(PoissonPrototype).initPoisson("Nemo", "poisson-clown");
nemo.sePresenter();
nemo.nager(50);
nemo.plonger(20);

console.log();

let flipper = Object.create(DauphinPrototype).initDauphin("Flipper");
flipper.sePresenter();
flipper.nager(100);
flipper.communiquer("Click click click");
flipper.echolocation();
flipper.plonger(50);

console.log();

let donald = Object.create(CanardPrototype).initCanard("Donald");
donald.sePresenter();
donald.nager(20);
donald.voler(30);
donald.communiquer("Coin coin!");
donald.atterrir();
donald.nager(10);

// 6. Analyse de la composition
console.log("=== Analyse des capacités ===");
function analyserCapacites(animal) {
    let capacites = {
        peutVoler: typeof animal.voler === "function",
        peutNager: typeof animal.nager === "function",
        peutCommuniquer: typeof animal.communiquer === "function"
    };
    
    console.log(`${animal.nom} - Capacités:`, capacites);
    return capacites;
}

analyserCapacites(aigle);
analyserCapacites(nemo);
analyserCapacites(flipper);
analyserCapacites(donald);

// 7. Performance et partage de méthodes
console.log("=== Partage de méthodes ===");
let aigle2 = Object.create(OiseauPrototype).initOiseau("Aigle2", "rapace");
console.log("Méthodes partagées entre aigles:", aigle.voler === aigle2.voler);
console.log("Méthodes partagées canard/aigle:", donald.voler === aigle.voler);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Système de formes géométriques
**Consigne :** Créez une hiérarchie de formes avec des prototypes.

```javascript
// Créer FormePrototype avec aire() et perimetre()
// Créer RectanglePrototype qui hérite de FormePrototype  
// Créer CerclePrototype qui hérite de FormePrototype
// Chaque forme doit implémenter ses propres calculs

let FormePrototype = {
    // Votre code ici
};

let RectanglePrototype = {
    // Votre code ici
};

let CerclePrototype = {
    // Votre code ici
};

// Tests
let rect = Object.create(RectanglePrototype).init(5, 3);
let cercle = Object.create(CerclePrototype).init(4);

console.log("Rectangle aire:", rect.aire()); // 15
console.log("Cercle aire:", cercle.aire()); // ~50.27
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Système de véhicules avec mixins
**Consigne :** Créez différents véhicules en composant des capacités.

```javascript
// Créer des mixins : Roulant, Volant, Flottant
// Créer VehiculePrototype de base
// Composer : Voiture (roule), Avion (roule + vole), Bateau (flotte)

let RoulantMixin = {
    // Méthodes liées au roulement
};

let VolantMixin = {
    // Méthodes liées au vol
};

// Tester avec différents véhicules
```

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Factory avec prototype dynamique
**Consigne :** Créez une factory qui génère des objets avec des prototypes personnalisés.

```javascript
function creerObjetAvecCapacites(nom, capacites) {
    // Créer dynamiquement un prototype basé sur les capacités demandées
    // Retourner un objet qui hérite de ce prototype
}

let ninja = creerObjetAvecCapacites("Naruto", ["courir", "sauter", "cloner"]);
let magicien = creerObjetAvecCapacites("Gandalf", ["voler", "lancer_sort", "teleporter"]);
```

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// Système de formes géométriques avec prototypes

// 1. Prototype de base pour toutes les formes
let FormePrototype = {
    init: function(nom) {
        this.nom = nom;
        this.couleur = "blanc";
        return this;
    },
    
    // Méthodes abstraites (à implémenter par les enfants)
    aire: function() {
        throw new Error("La méthode aire() doit être implémentée");
    },
    
    perimetre: function() {
        throw new Error("La méthode perimetre() doit être implémentée");
    },
    
    // Méthodes communes
    setCouleur: function(couleur) {
        this.couleur = couleur;
        return this;
    },
    
    decrire: function() {
        console.log(`${this.nom} ${this.couleur} - Aire: ${this.aire().toFixed(2)}, Périmètre: ${this.perimetre().toFixed(2)}`);
    },
    
    comparer: function(autreForme) {
        let monAire = this.aire();
        let autreAire = autreForme.aire();
        
        if (monAire > autreAire) {
            console.log(`${this.nom} est plus grand que ${autreForme.nom}`);
        } else if (monAire < autreAire) {
            console.log(`${this.nom} est plus petit que ${autreForme.nom}`);
        } else {
            console.log(`${this.nom} et ${autreForme.nom} ont la même aire`);
        }
    }
};

// 2. Prototype Rectangle
let RectanglePrototype = Object.create(FormePrototype);

RectanglePrototype.init = function(largeur, hauteur) {
    FormePrototype.init.call(this, "Rectangle");
    this.largeur = largeur;
    this.hauteur = hauteur;
    return this;
};

RectanglePrototype.aire = function() {
    return this.largeur * this.hauteur;
};

RectanglePrototype.perimetre = function() {
    return 2 * (this.largeur + this.hauteur);
};

RectanglePrototype.estCarre = function() {
    return this.largeur === this.hauteur;
};

// 3. Prototype Cercle
let CerclePrototype = Object.create(FormePrototype);

CerclePrototype.init = function(rayon) {
    FormePrototype.init.call(this, "Cercle");
    this.rayon = rayon;
    return this;
};

CerclePrototype.aire = function() {
    return Math.PI * this.rayon * this.rayon;
};

CerclePrototype.perimetre = function() {
    return 2 * Math.PI * this.rayon;
};

CerclePrototype.diametre = function() {
    return 2 * this.rayon;
};

// 4. Prototype Triangle
let TrianglePrototype = Object.create(FormePrototype);

TrianglePrototype.init = function(base, hauteur, cote1, cote2) {
    FormePrototype.init.call(this, "Triangle");
    this.base = base;
    this.hauteur = hauteur;
    this.cote1 = cote1 || base;
    this.cote2 = cote2 || base;
    return this;
};

TrianglePrototype.aire = function() {
    return (this.base * this.hauteur) / 2;
};

TrianglePrototype.perimetre = function() {
    return this.base + this.cote1 + this.cote2;
};

TrianglePrototype.estEquilateral = function() {
    return this.base === this.cote1 && this.cote1 === this.cote2;
};

// 5. Tests
console.log("=== Tests formes géométriques ===");

let rectangle = Object.create(RectanglePrototype)
    .init(5, 3)
    .setCouleur("rouge");

let cercle = Object.create(CerclePrototype)
    .init(4)
    .setCouleur("bleu");

let triangle = Object.create(TrianglePrototype)
    .init(6, 4, 5, 5)
    .setCouleur("vert");

// Descriptions
rectangle.decrire();
cercle.decrire(); 
triangle.decrire();

// Comparaisons
rectangle.comparer(cercle);
cercle.comparer(triangle);

// Méthodes spécifiques
console.log("Rectangle est carré:", rectangle.estCarre());
console.log("Diamètre du cercle:", cercle.diametre());
console.log("Triangle équilatéral:", triangle.estEquilateral());

// Vérification héritage
console.log("Rectangle hérite de Forme:", FormePrototype.isPrototypeOf(rectangle));
console.log("Cercle hérite de Forme:", FormePrototype.isPrototypeOf(cercle));
```

### Solution Exercice 2
```javascript
// Système de véhicules avec mixins

// 1. Mixins pour différentes capacités
let RoulantMixin = {
    accelerer: function(vitesse) {
        this.vitesse = Math.min(this.vitesseMax, (this.vitesse || 0) + vitesse);
        console.log(`${this.nom} accélère à ${this.vitesse} km/h`);
    },
    
    freiner: function() {
        this.vitesse = Math.max(0, (this.vitesse || 0) - 20);
        console.log(`${this.nom} freine, vitesse: ${this.vitesse} km/h`);
    },
    
    rouler: function(distance) {
        if (this.vitesse > 0) {
            console.log(`${this.nom} roule ${distance} km à ${this.vitesse} km/h`);
        } else {
            console.log(`${this.nom} doit d'abord accélérer`);
        }
    }
};

let VolantMixin = {
    decoller: function() {
        if (this.vitesse >= this.vitesseDecollage) {
            this.altitude = 100;
            console.log(`${this.nom} décolle!`);
        } else {
            console.log(`${this.nom} n'a pas assez de vitesse pour décoller`);
        }
    },
    
    voler: function(altitude) {
        if (this.altitude > 0) {
            this.altitude = altitude;
            console.log(`${this.nom} vole à ${altitude}m d'altitude`);
        } else {
            console.log(`${this.nom} doit d'abord décoller`);
        }
    },
    
    atterrir: function() {
        this.altitude = 0;
        this.vitesse = 50; // Vitesse d'atterrissage
        console.log(`${this.nom} atterrit`);
    }
};

let FlottantMixin = {
    naviguer: function(distance) {
        console.log(`${this.nom} navigue ${distance} km sur l'eau`);
    },
    
    ancrer: function() {
        this.vitesse = 0;
        console.log(`${this.nom} jette l'ancre`);
    },
    
    accoster: function() {
        this.vitesse = 0;
        console.log(`${this.nom} accoste au port`);
    }
};

// 2. Prototype de base pour tous les véhicules
let VehiculePrototype = {
    init: function(nom, vitesseMax) {
        this.nom = nom;
        this.vitesseMax = vitesseMax;
        this.vitesse = 0;
        this.altitude = 0;
        return this;
    },
    
    arreter: function() {
        this.vitesse = 0;
        console.log(`${this.nom} s'arrête`);
    },
    
    etat: function() {
        let status = [`Vitesse: ${this.vitesse} km/h`];
        if (this.altitude > 0) status.push(`Altitude: ${this.altitude}m`);
        console.log(`${this.nom} - ${status.join(", ")}`);
    }
};

// 3. Fonction utilitaire pour appliquer mixins
function creerVehiculeAvecMixins(prototype, ...mixins) {
    mixins.forEach(mixin => {
        Object.assign(prototype, mixin);
    });
    return prototype;
}

// 4. Types de véhicules spécifiques

// Voiture (roule uniquement)
let VoiturePrototype = Object.create(VehiculePrototype);
creerVehiculeAvecMixins(VoiturePrototype, RoulantMixin);

VoiturePrototype.initVoiture = function(nom, vitesseMax, carburant) {
    this.init(nom, vitesseMax);
    this.carburant = carburant;
    return this;
};

// Avion (roule + vole)
let AvionPrototype = Object.create(VehiculePrototype);
creerVehiculeAvecMixins(AvionPrototype, RoulantMixin, VolantMixin);

AvionPrototype.initAvion = function(nom, vitesseMax, vitesseDecollage) {
    this.init(nom, vitesseMax);
    this.vitesseDecollage = vitesseDecollage;
    return this;
};

// Bateau (flotte uniquement)
let BateauPrototype = Object.create(VehiculePrototype);
creerVehiculeAvecMixins(BateauPrototype, FlottantMixin);

BateauPrototype.initBateau = function(nom, vitesseMax, tonnage) {
    this.init(nom, vitesseMax);
    this.tonnage = tonnage;
    return this;
};

// Hydravion (roule + vole + flotte)
let HydravionPrototype = Object.create(VehiculePrototype);
creerVehiculeAvecMixins(HydravionPrototype, RoulantMixin, VolantMixin, FlottantMixin);

HydravionPrototype.initHydravion = function(nom, vitesseMax, vitesseDecollage) {
    this.init(nom, vitesseMax);
    this.vitesseDecollage = vitesseDecollage;
    return this;
};

// 5. Tests des véhicules
console.log("=== Tests des véhicules ===");

let ferrari = Object.create(VoiturePrototype)
    .initVoiture("Ferrari", 300, "essence");

console.log("Test Ferrari:");
ferrari.accelerer(50);
ferrari.rouler(100);
ferrari.accelerer(100);
ferrari.etat();
ferrari.freiner();
ferrari.arreter();

console.log("\nTest Avion:");
let boeing = Object.create(AvionPrototype)
    .initAvion("Boeing 747", 900, 250);

boeing.accelerer(100);
boeing.accelerer(100);
boeing.accelerer(100); // 300 km/h, peut décoller
boeing.decoller();
boeing.voler(10000);
boeing.etat();
boeing.atterrir();

console.log("\nTest Bateau:");
let titanic = Object.create(BateauPrototype)
    .initBateau("Titanic", 45, 52000);

titanic.naviguer(500);
titanic.ancrer();
titanic.accoster();

console.log("\nTest Hydravion:");
let hydravion = Object.create(HydravionPrototype)
    .initHydravion("Canadair", 400, 150);

hydravion.naviguer(50); // Sur l'eau
hydravion.accelerer(80);
hydravion.accelerer(80); // 160 km/h, peut décoller
hydravion.decoller();
hydravion.voler(3000);
hydravion.atterrir();
hydravion.accoster();

// 6. Analyse des capacités
console.log("\n=== Analyse des capacités ===");
function analyserCapacites(vehicule) {
    let capacites = [];
    if (vehicule.rouler) capacites.push("rouler");
    if (vehicule.voler) capacites.push("voler");
    if (vehicule.naviguer) capacites.push("naviguer");
    
    console.log(`${vehicule.nom} peut: ${capacites.join(", ")}`);
}

analyserCapacites(ferrari);
analyserCapacites(boeing);
analyserCapacites(titanic);
analyserCapacites(hydravion);
```

### Solution Exercice 3
```javascript
// Factory avec prototype dynamique

// 1. Bibliothèque de capacités disponibles
let BibliothequeCapacites = {
    // Capacités de mouvement
    courir: {
        courir: function(distance) {
            this.energie -= distance / 10;
            console.log(`${this.nom} court ${distance}m (énergie: ${this.energie})`);
        }
    },
    
    sauter: {
        sauter: function(hauteur) {
            this.energie -= hauteur / 5;
            console.log(`${this.nom} saute ${hauteur}m de haut (énergie: ${this.energie})`);
        }
    },
    
    voler: {
        voler: function(altitude) {
            this.altitude = altitude;
            this.energie -= altitude / 20;
            console.log(`${this.nom} vole à ${altitude}m (énergie: ${this.energie})`);
        },
        
        atterrir: function() {
            console.log(`${this.nom} atterrit depuis ${this.altitude || 0}m`);
            this.altitude = 0;
        }
    },
    
    nager: {
        nager: function(distance) {
            this.energie -= distance / 8;
            console.log(`${this.nom} nage ${distance}m (énergie: ${this.energie})`);
        }
    },
    
    // Capacités de combat
    attaquer: {
        attaquer: function(cible) {
            let degats = this.force || 10;
            console.log(`${this.nom} attaque ${cible} pour ${degats} dégâts`);
            return degats;
        }
    },
    
    defendre: {
        defendre: function() {
            let defense = this.defense || 5;
            console.log(`${this.nom} se défend (+${defense} défense)`);
            return defense;
        }
    },
    
    // Capacités magiques
    lancer_sort: {
        lancerSort: function(sort, cible) {
            if (this.mana >= 10) {
                this.mana -= 10;
                console.log(`${this.nom} lance ${sort} sur ${cible} (mana: ${this.mana})`);
            } else {
                console.log(`${this.nom} n'a pas assez de mana`);
            }
        }
    },
    
    guerir: {
        guerir: function(cible) {
            if (this.mana >= 15) {
                this.mana -= 15;
                let soin = 20;
                console.log(`${this.nom} guérit ${cible} de ${soin} PV (mana: ${this.mana})`);
                return soin;
            } else {
                console.log(`${this.nom} n'a pas assez de mana pour guérir`);
                return 0;
            }
        }
    },
    
    teleporter: {
        teleporter: function(destination) {
            if (this.mana >= 25) {
                this.mana -= 25;
                console.log(`${this.nom} se téléporte à ${destination} (mana: ${this.mana})`);
            } else {
                console.log(`${this.nom} n'a pas assez de mana pour se téléporter`);
            }
        }
    },
    
    // Capacités spéciales
    cloner: {
        cloner: function() {
            if (this.energie >= 30) {
                this.energie -= 30;
                console.log(`${this.nom} crée un clone (énergie: ${this.energie})`);
                return `Clone de ${this.nom}`;
            } else {
                console.log(`${this.nom} n'a pas assez d'énergie pour se cloner`);
                return null;
            }
        }
    },
    
    invisibilite: {
        deviendrInvisible: function() {
            this.invisible = true;
            this.energie -= 20;
            console.log(`${this.nom} devient invisible (énergie: ${this.energie})`);
        },
        
        devenirVisible: function() {
            this.invisible = false;
            console.log(`${this.nom} redevient visible`);
        }
    }
};

// 2. Factory function principale
function creerObjetAvecCapacites(nom, capacites, stats = {}) {
    // Créer le prototype de base
    let PrototypeBase = {
        init: function(nom, stats) {
            this.nom = nom;
            this.energie = stats.energie || 100;
            this.mana = stats.mana || 50;
            this.force = stats.force || 10;
            this.defense = stats.defense || 5;
            this.niveau = stats.niveau || 1;
            this.altitude = 0;
            this.invisible = false;
            return this;
        },
        
        sePresenter: function() {
            let capacitesList = capacites.join(", ");
            console.log(`Je suis ${this.nom}, niveau ${this.niveau}`);
            console.log(`Capacités: ${capacitesList}`);
            console.log(`Stats: Énergie ${this.energie}, Mana ${this.mana}, Force ${this.force}, Défense ${this.defense}`);
        },
        
        seReposer: function() {
            this.energie = Math.min(100, this.energie + 30);
            this.mana = Math.min(100, this.mana + 20);
            console.log(`${this.nom} se repose (Énergie: ${this.energie}, Mana: ${this.mana})`);
        },
        
        monterNiveau: function() {
            this.niveau++;
            this.energie += 10;
            this.mana += 10;
            this.force += 2;
            this.defense += 1;
            console.log(`${this.nom} monte au niveau ${this.niveau}! Stats améliorées.`);
        },
        
        estVivant: function() {
            return this.energie > 0;
        }
    };
    
    // Créer un prototype personnalisé en copiant la base
    let PrototypePersonnalise = Object.create(PrototypeBase);
    
    // Ajouter les capacités demandées
    capacites.forEach(capacite => {
        if (BibliothequeCapacites[capacite]) {
            Object.assign(PrototypePersonnalise, BibliothequeCapacites[capacite]);
        } else {
            console.warn(`Capacité '${capacite}' non trouvée`);
        }
    });
    
    // Créer et initialiser l'objet
    return Object.create(PrototypePersonnalise).init(nom, stats);
}

// 3. Fonction utilitaire pour lister les capacités disponibles
function listerCapacitesDisponibles() {
    console.log("Capacités disponibles:");
    Object.keys(BibliothequeCapacites).forEach(capacite => {
        let methodes = Object.keys(BibliothequeCapacites[capacite]);
        console.log(`- ${capacite}: ${methodes.join(", ")}`);
    });
}

// 4. Tests de la factory
console.log("=== Tests Factory avec capacités ===");

// Créer différents personnages
let ninja = creerObjetAvecCapacites("Naruto", ["courir", "sauter", "cloner"], {
    energie: 120,
    force: 15,
    niveau: 5
});

let magicien = creerObjetAvecCapacites("Gandalf", ["voler", "lancer_sort", "teleporter", "guerir"], {
    mana: 100,
    force: 8,
    defense: 12,
    niveau: 10
});

let guerrier = creerObjetAvecCapacites("Conan", ["attaquer", "defendre", "courir"], {
    energie: 150,
    force: 20,
    defense: 15,
    niveau: 8
});

let espion = creerObjetAvecCapacites("James Bond", ["courir", "nager", "invisibilite"], {
    energie: 110,
    force: 12,
    defense: 8,
    niveau: 7
});

// Tests des personnages
console.log("=== Ninja ===");
ninja.sePresenter();
ninja.courir(50);
ninja.sauter(10);
ninja.cloner();

console.log("\n=== Magicien ===");
magicien.sePresenter();
magicien.voler(100);
magicien.lancerSort("Boule de feu", "Dragon");
magicien.guerir("Ally");
magicien.teleporter("Tour de la Sagesse");
magicien.atterrir();

console.log("\n=== Guerrier ===");
guerrier.sePresenter();
guerrier.attaquer("Orc");
guerrier.defendre();
guerrier.courir(30);

console.log("\n=== Espion ===");
espion.sePresenter();
espion.deviendrInvisible();
espion.nager(100);
espion.devenirVisible();
espion.courir(80);

// Tests de progression
console.log("\n=== Progression ===");
ninja.seReposer();
ninja.monterNiveau();
ninja.sePresenter();

// Vérification des prototypes
console.log("\n=== Vérification prototypes ===");
console.log("Ninja peut cloner:", typeof ninja.cloner === "function");
console.log("Magicien peut cloner:", typeof magicien.cloner === "function");
console.log("Guerrier peut voler:", typeof guerrier.voler === "function");
console.log("Espion peut attaquer:", typeof espion.attaquer === "function");

// Liste des capacités disponibles
console.log("\n=== Capacités disponibles ===");
listerCapacitesDisponibles();
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Objets** : Propriétés, méthodes, this
- **Fonctions** : Constructor functions, call, apply, bind
- **Classes ES6** : Syntaxe moderne pour les prototypes
- **Closures** : Encapsulation et méthodes privées

### Prochaines étapes importantes
- **Modules ES6** : Encapsulation au niveau module
- **Design Patterns** : Factory, Observer, Strategy avec prototypes
- **Frameworks** : React/Vue utilisent les prototypes massivement
- **Performance** : Optimisation mémoire avec prototypes

### Concepts complémentaires
- **Mixins** : Composition de comportements
- **Symbols** : Propriétés privées avec Symbol
- **WeakMap** : Données privées associées aux objets

---

## **PROJET MINI**

### Défi pratique : Système de jeu RPG avec prototypes
**Objectif :** Créer un mini-jeu RPG utilisant tous les concepts de prototypes.

**Fonctionnalités requises :**
- [ ] Hiérarchie de personnages (Guerrier, Mage, Voleur)
- [ ] Système d'équipement modifiant les stats
- [ ] Compétences spéciales par classe
- [ ] Système de progression (niveaux, évolution)
- [ ] Combat avec calculs de dégâts
- [ ] Inventaire et objets avec effets

**Structure suggérée :**
```
📁 rpg-prototype/
├── 📄 index.html
├── 📄 personnages.js (hiérarchie prototypes)
├── 📄 competences.js (mixins capacités)
├── 📄 equipement.js (objets et stats)
├── 📄 combat.js (système de combat)
└── 📄 jeu.js (logique principale)
```

**Temps estimé :** 180 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Héritage et chaîne de prototypes](https://developer.mozilla.org/fr/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [MDN - Object.create()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
- [MDN - Object.setPrototypeOf()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf)

### Articles de référence
- "Understanding Prototypes in JavaScript" - Dmitri Pavlutin
- "JavaScript Prototypes in Plain Language" - JavaScript Is Sexy
- "Prototypal vs Classical Inheritance" - Eric Elliott

### Outils de pratique
- [Prototype Chain Visualizer](https://www.objectplayground.com/) - Visualiser la chaîne prototypale
- [JavaScript Prototype Exercises](https://github.com/getify/You-Dont-Know-JS) - Exercices pratiques

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Quelle est la différence entre héritage prototypal et classique ?**
   - Réponse : Prototypal hérite d'objets directement, classique hérite de classes/blueprints

2. **Que se passe-t-il quand on accède à une propriété inexistante ?**
   - Réponse : JavaScript remonte la chaîne prototypale jusqu'à trouver la propriété ou atteindre null

3. **Pourquoi les classes ES6 sont-elles du "sucre syntaxique" ?**
   - Réponse : Elles utilisent le même mécanisme prototypal en interne, juste avec une syntaxe plus familière

### Checklist de maîtrise
- [ ] Je comprends la chaîne prototypale et son fonctionnement
- [ ] Je peux créer des hiérarchies avec Object.create
- [ ] Je maîtrise les constructor functions et leur prototype
- [ ] Je sais quand utiliser hasOwnProperty vs in
- [ ] Je peux implémenter des mixins pour la composition
- [ ] Je comprends la relation entre classes ES6 et prototypes
- [ ] Je reconnais et évite les pièges des prototypes partagés

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 : Propriétés partagées par référence
**Code problématique :**
```javascript
function Equipe(nom) {
    this.nom = nom;
}
Equipe.prototype.membres = []; // ❌ Partagé!

let equipe1 = new Equipe("A");
let equipe2 = new Equipe("B");
equipe1.membres.push("Alice");
console.log(equipe2.membres); // ["Alice"] ❌
```

**Solution :**
```javascript
function Equipe(nom) {
    this.nom = nom;
    this.membres = []; // ✅ Chaque instance a son propre array
}
```

### Erreur fréquente 2 : Confusion avec this
**Code problématique :**
```javascript
let obj = {
    nom: "Test",
    saluer: function() {
        console.log(`Bonjour ${this.nom}`);
    }
};

let methode = obj.saluer;
methode(); // "Bonjour undefined" ❌
```

**Solution :**
```javascript
let methode = obj.saluer.bind(obj);
methode(); // "Bonjour Test" ✅

// Ou
obj.saluer.call(obj); // ✅
```

### Erreur fréquente 3 : Modification des prototypes natifs
**Code problématique :**
```javascript
Array.prototype.dernierElement = function() {
    return this[this.length - 1];
}; // ❌ Pollution globale
```

**Solution :**
```javascript
// Créer ses propres prototypes
let MonArrayPrototype = Object.create(Array.prototype);
MonArrayPrototype.dernierElement = function() {
    return this[this.length - 1];
}; // ✅
```

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les prototypes sont la base de tout en JavaScript
- L'héritage prototypal est plus flexible que l'héritage classique
- Les classes ES6 sont juste du sucre syntaxique sur les prototypes

### Questions à approfondir
- Performance des lookups dans la chaîne prototypale
- Patterns avancés avec prototypes (mixins, compositions)
- Evolution vers les propriétés privées avec #

### Révision programmée
- [ ] Révision dans 1 jour (refaire les exercices complexes)
- [ ] Révision dans 1 semaine (créer un mini-projet)
- [ ] Révision dans 1 mois (lien avec les design patterns)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #prototypes #heritage #chaine-prototypale #constructor-functions #object-create #mixins

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐⭐⭐

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 9/126*
