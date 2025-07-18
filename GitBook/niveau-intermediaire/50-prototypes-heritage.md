# Prototypes et Héritage

> **Navigation :** [← Classes et POO](45-classes-poo.md) | [DOM Manipulation →](55-dom-manipulation.md)

## 🎯 Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :
- ✅ Comprendre le système de prototypes de JavaScript
- ✅ Manipuler la chaîne prototypale
- ✅ Créer de l'héritage avec Object.create
- ✅ Différencier héritage prototypal et héritage classique
- ✅ Déboguer les problèmes liés aux prototypes
- ✅ Migrer du code prototype vers les classes ES6

## 📚 Concepts clés

| Concept | Description | Utilisation |
|---------|-------------|-------------|
| **Prototype** | Objet modèle dont héritent d'autres objets | Base de l'héritage JS |
| **Chaîne prototypale** | Succession de prototypes jusqu'à null | Recherche de propriétés |
| **Constructor function** | Fonction créant des objets avec `new` | Pattern avant ES6 |
| **Object.create()** | Création d'objet avec prototype spécifique | Héritage moderne |
| **hasOwnProperty()** | Vérifier si propriété appartient à l'objet | Distinguer propre/hérité |

## 💡 Introduction

{% hint style="info" %}
**🔑 Point crucial :** Les prototypes sont le **CŒUR** de JavaScript. Contrairement aux langages comme Java ou C#, JavaScript utilise l'héritage prototypal, pas l'héritage classique. Les classes ES6 ne sont que du "sucre syntaxique" sur ce système !
{% endhint %}

{% tabs %}
{% tab title="🧬 Qu'est-ce qu'un prototype ?" %}
**Chaque objet JavaScript a un lien vers un autre objet appelé son prototype.**

```javascript
// Exemple simple
let animal = {
    type: "animal",
    respirer() {
        console.log("Je respire 🫁");
    }
};

let chien = {
    race: "Labrador"
};

// Établir l'héritage
Object.setPrototypeOf(chien, animal);

console.log(chien.race);    // "Labrador" (propriété propre)
console.log(chien.type);    // "animal" (trouvé dans le prototype)
chien.respirer();           // "Je respire 🫁" (méthode du prototype)
```

**🔍 Comment ça marche ?**
1. JavaScript cherche `race` sur `chien` → trouvé ✅
2. JavaScript cherche `type` sur `chien` → pas trouvé ❌
3. JavaScript remonte au prototype (`animal`) → trouvé ✅
4. Si pas trouvé, continue jusqu'à `Object.prototype` puis `null`
{% endtab %}

{% tab title="🔗 La chaîne prototypale" %}
**JavaScript remonte la chaîne des prototypes jusqu'à trouver la propriété ou atteindre `null`.**

```javascript
let grandParent = {
    sagesse: "L'expérience compte 🧙‍♂️"
};

let parent = {
    education: "Les études sont importantes 🎓"
};

let enfant = {
    nom: "Alex",
    energie: "Plein d'énergie ! ⚡"
};

// Créer la chaîne : enfant → parent → grandParent → Object.prototype → null
Object.setPrototypeOf(parent, grandParent);
Object.setPrototypeOf(enfant, parent);

console.log(enfant.nom);       // "Alex" (propriété propre)
console.log(enfant.education); // "Les études..." (1er niveau parent)
console.log(enfant.sagesse);   // "L'expérience..." (2ème niveau parent)
console.log(enfant.toString()); // "[object Object]" (Object.prototype)

// Visualiser la chaîne
let current = enfant;
let niveau = 0;
while (current) {
    console.log(`Niveau ${niveau}:`, Object.getOwnPropertyNames(current));
    current = Object.getPrototypeOf(current);
    niveau++;
    if (niveau > 5) break; // Sécurité
}
```
{% endtab %}

{% tab title="⚖️ Classes vs Prototypes" %}
**Les classes ES6 sont du sucre syntaxique sur les prototypes !**

```javascript
// ✨ Classe ES6 (moderne)
class Animal {
    constructor(nom, type) {
        this.nom = nom;
        this.type = type;
    }
    
    respirer() {
        console.log(`${this.nom} respire`);
    }
}

// 🔧 Équivalent en prototypes (traditionnel)
function Animal(nom, type) {
    this.nom = nom;
    this.type = type;
}

Animal.prototype.respirer = function() {
    console.log(`${this.nom} respire`);
};

// Les deux créent EXACTEMENT la même structure !
let chat1 = new Animal("Minou", "chat"); // Classe
let chat2 = new Animal("Felix", "chat");  // Function constructor

console.log(chat1.respirer === chat2.respirer); // true (méthode partagée)
console.log(chat1 instanceof Animal);           // true pour les deux
```
{% endtab %}
{% endtabs %}

## 🛠️ Méthodes de création d'objets

### 1. Constructor Functions (style classique)

{% tabs %}
{% tab title="📝 Syntaxe de base" %}
```javascript
// Constructor function (convention: première lettre majuscule)
function Personne(nom, age, ville) {
    // Propriétés d'instance (uniques par objet)
    this.nom = nom;
    this.age = age;
    this.ville = ville;
    this.energie = 100;
}

// Méthodes partagées via le prototype
Personne.prototype.sePresenter = function() {
    return `Bonjour, je suis ${this.nom}, ${this.age} ans, de ${this.ville}`;
};

Personne.prototype.vieillir = function() {
    this.age++;
    this.energie = Math.max(0, this.energie - 5);
    console.log(`${this.nom} a maintenant ${this.age} ans (énergie: ${this.energie})`);
};

Personne.prototype.demenager = function(nouvelleVille) {
    console.log(`${this.nom} déménage de ${this.ville} à ${nouvelleVille}`);
    this.ville = nouvelleVille;
};

// Propriété statique
Personne.prototype.espece = "Homo sapiens";

// Création d'instances
let alice = new Personne("Alice", 25, "Paris");
let bob = new Personne("Bob", 30, "Lyon");

console.log(alice.sePresenter());
alice.vieillir();
alice.demenager("Marseille");

// Vérification du partage des méthodes
console.log(alice.sePresenter === bob.sePresenter); // true
```
{% endtab %}

{% tab title="🏗️ Héritage avec Constructor Functions" %}
```javascript
// Parent Constructor
function Vehicule(marque, modele, annee) {
    this.marque = marque;
    this.modele = modele;
    this.annee = annee;
    this.kilometrage = 0;
}

Vehicule.prototype.demarrer = function() {
    console.log(`${this.marque} ${this.modele} démarre 🚗`);
};

Vehicule.prototype.rouler = function(distance) {
    this.kilometrage += distance;
    console.log(`Roulé ${distance}km. Total: ${this.kilometrage}km`);
};

Vehicule.prototype.getDescription = function() {
    return `${this.marque} ${this.modele} (${this.annee})`;
};

// Child Constructor
function Voiture(marque, modele, annee, portes, carburant) {
    // Appeler le constructor parent
    Vehicule.call(this, marque, modele, annee);
    
    // Propriétés spécifiques
    this.portes = portes;
    this.carburant = carburant;
}

// 🔗 Établir l'héritage prototypal
Voiture.prototype = Object.create(Vehicule.prototype);
Voiture.prototype.constructor = Voiture; // Restaurer le constructor

// Ajouter/surcharger des méthodes
Voiture.prototype.demarrer = function() {
    console.log(`${this.getDescription()} démarre avec moteur ${this.carburant} ⛽`);
};

Voiture.prototype.ouvrirCoffre = function() {
    console.log(`Coffre de ${this.getDescription()} ouvert 📦`);
};

// Tests
let maVoiture = new Voiture("Toyota", "Camry", 2020, 4, "essence");
maVoiture.demarrer();     // Méthode surchargée
maVoiture.rouler(100);    // Méthode héritée
maVoiture.ouvrirCoffre(); // Méthode propre

console.log(maVoiture instanceof Voiture);  // true
console.log(maVoiture instanceof Vehicule); // true
```
{% endtab %}
{% endtabs %}

### 2. Object.create() (style moderne)

{% tabs %}
{% tab title="🎨 Création avec prototype" %}
```javascript
// Créer un prototype objet
let AnimalPrototype = {
    // Méthode d'initialisation
    init(nom, espece, habitat) {
        this.nom = nom;
        this.espece = espece;
        this.habitat = habitat;
        this.energie = 100;
        return this; // Pour chaînage
    },
    
    // Méthodes partagées
    manger(nourriture) {
        this.energie = Math.min(100, this.energie + 20);
        console.log(`${this.nom} mange ${nourriture} (énergie: ${this.energie})`);
    },
    
    dormir(heures) {
        this.energie = Math.min(100, this.energie + heures * 10);
        console.log(`${this.nom} dort ${heures}h (énergie: ${this.energie})`);
    },
    
    sePresenter() {
        return `Je suis ${this.nom}, un(e) ${this.espece} vivant en ${this.habitat}`;
    },
    
    depenser(activite, cout = 15) {
        this.energie = Math.max(0, this.energie - cout);
        console.log(`${this.nom} fait: ${activite} (énergie: ${this.energie})`);
    }
};

// Créer des instances
let lion = Object.create(AnimalPrototype)
    .init("Simba", "lion", "savane");

let dauphin = Object.create(AnimalPrototype)
    .init("Flipper", "dauphin", "océan");

// Utilisation
console.log(lion.sePresenter());
lion.manger("gazelle");
lion.depenser("chasser", 25);
lion.dormir(8);

console.log(dauphin.sePresenter());
dauphin.depenser("nager", 10);
dauphin.manger("poisson");
```
{% endtab %}

{% tab title="🌳 Héritage avec Object.create" %}
```javascript
// Prototype parent
let VehiculePrototype = {
    init(marque, modele, annee) {
        this.marque = marque;
        this.modele = modele;
        this.annee = annee;
        this.kilometrage = 0;
        return this;
    },
    
    demarrer() {
        console.log(`${this.getDescription()} démarre`);
    },
    
    rouler(distance) {
        this.kilometrage += distance;
        console.log(`+${distance}km. Total: ${this.kilometrage}km`);
    },
    
    getDescription() {
        return `${this.marque} ${this.modele} (${this.annee})`;
    },
    
    getAge() {
        return new Date().getFullYear() - this.annee;
    }
};

// Prototype enfant héritant du parent
let VoiturePrototype = Object.create(VehiculePrototype);

// Méthode d'initialisation spécialisée
VoiturePrototype.initVoiture = function(marque, modele, annee, portes, carburant) {
    // Appeler la méthode parent
    this.init(marque, modele, annee);
    
    // Propriétés spécifiques
    this.portes = portes;
    this.carburant = carburant;
    return this;
};

// Surcharger une méthode
VoiturePrototype.demarrer = function() {
    console.log(`${this.getDescription()} démarre avec moteur ${this.carburant} 🚗`);
};

// Ajouter des méthodes spécifiques
VoiturePrototype.ouvrirPortes = function() {
    console.log(`Ouverture des ${this.portes} portes`);
};

VoiturePrototype.fairePleinEssence = function() {
    console.log(`Plein d'essence pour ${this.getDescription()} ⛽`);
};

// Prototype petit-enfant
let VoitureElectriquePrototype = Object.create(VoiturePrototype);

VoitureElectriquePrototype.initElectrique = function(marque, modele, annee, autonomie) {
    this.initVoiture(marque, modele, annee, 4, "électrique");
    this.autonomie = autonomie;
    this.batterie = 100;
    return this;
};

VoitureElectriquePrototype.demarrer = function() {
    console.log(`${this.getDescription()} démarre silencieusement ⚡`);
};

VoitureElectriquePrototype.recharger = function(temps) {
    this.batterie = Math.min(100, this.batterie + temps * 10);
    console.log(`Recharge ${temps}h. Batterie: ${this.batterie}%`);
};

// Tests de la hiérarchie
let voitureClassique = Object.create(VoiturePrototype)
    .initVoiture("Honda", "Civic", 2019, 4, "essence");

let voitureElectrique = Object.create(VoitureElectriquePrototype)
    .initElectrique("Tesla", "Model 3", 2021, 400);

console.log("=== Voiture classique ===");
voitureClassique.demarrer();
voitureClassique.rouler(50);
voitureClassique.fairePleinEssence();

console.log("\n=== Voiture électrique ===");
voitureElectrique.demarrer();
voitureElectrique.rouler(80);
voitureElectrique.recharger(2);

// Vérification de l'héritage
console.log("\n=== Vérifications ===");
console.log("Voiture classique hérite de Vehicule:", 
    VehiculePrototype.isPrototypeOf(voitureClassique)); // true
console.log("Voiture électrique hérite de Voiture:", 
    VoiturePrototype.isPrototypeOf(voitureElectrique)); // true
console.log("Voiture électrique hérite de Vehicule:", 
    VehiculePrototype.isPrototypeOf(voitureElectrique)); // true
```
{% endtab %}
{% endtabs %}

## 🔍 Inspection et débogage

{% tabs %}
{% tab title="🕵️ Méthodes d'inspection" %}
```javascript
let animal = { 
    type: "animal",
    respirer() { return "Je respire"; }
};

let chien = Object.create(animal);
chien.race = "Labrador";
chien.nom = "Rex";

let labrador = Object.create(chien);
labrador.nom = "Buddy"; // Masque la propriété parent

console.log("=== INSPECTION DES PROTOTYPES ===");

// 1. Vérifier les propriétés propres vs héritées
console.log("Propriétés propres de labrador:", Object.getOwnPropertyNames(labrador));
console.log("labrador.hasOwnProperty('nom'):", labrador.hasOwnProperty('nom'));     // true
console.log("labrador.hasOwnProperty('race'):", labrador.hasOwnProperty('race'));   // false
console.log("labrador.hasOwnProperty('type'):", labrador.hasOwnProperty('type'));   // false

// 2. Vérifier la présence (propre + hérité)
console.log("'nom' in labrador:", 'nom' in labrador);   // true
console.log("'race' in labrador:", 'race' in labrador); // true
console.log("'type' in labrador:", 'type' in labrador); // true

// 3. Obtenir les prototypes
console.log("Prototype direct:", Object.getPrototypeOf(labrador) === chien);   // true
console.log("Grand-parent:", Object.getPrototypeOf(chien) === animal);        // true

// 4. Vérifier l'héritage
console.log("chien.isPrototypeOf(labrador):", chien.isPrototypeOf(labrador));   // true
console.log("animal.isPrototypeOf(labrador):", animal.isPrototypeOf(labrador)); // true

// 5. Parcourir la chaîne prototypale
function explorerChaine(obj, nom = "objet") {
    console.log(`\n📋 Exploration de ${nom}:`);
    let current = obj;
    let niveau = 0;
    
    while (current) {
        const proprietes = Object.getOwnPropertyNames(current);
        console.log(`  Niveau ${niveau}: ${proprietes.join(', ') || '(vide)'}`);
        
        current = Object.getPrototypeOf(current);
        niveau++;
        
        if (niveau > 10) { // Sécurité
            console.log("  ... (trop profond)");
            break;
        }
    }
}

explorerChaine(labrador, "labrador");

// 6. Comparaison avec instanceof (fonctionne avec constructor functions)
function Animal(type) { this.type = type; }
function Chien(nom) { 
    Animal.call(this, "chien"); 
    this.nom = nom; 
}
Chien.prototype = Object.create(Animal.prototype);

let rex = new Chien("Rex");
console.log("rex instanceof Chien:", rex instanceof Chien);   // true
console.log("rex instanceof Animal:", rex instanceof Animal); // true
```
{% endtab %}

{% tab title="🔧 Outils de débogage" %}
```javascript
// Utilitaires pour comprendre les prototypes
const PrototypeUtils = {
    // Afficher la chaîne prototypale complète
    afficherChaine(obj, nom = "objet") {
        console.log(`\n🔗 Chaîne prototypale de ${nom}:`);
        
        let current = obj;
        let niveau = 0;
        
        while (current) {
            const constructor = current.constructor?.name || "Inconnu";
            const proprietes = Object.getOwnPropertyNames(current);
            const methodes = proprietes.filter(prop => 
                typeof current[prop] === 'function'
            );
            
            console.log(`  ${niveau}: ${constructor}`);
            console.log(`     Propriétés: ${proprietes.filter(p => !methodes.includes(p)).join(', ') || 'aucune'}`);
            console.log(`     Méthodes: ${methodes.join(', ') || 'aucune'}`);
            
            current = Object.getPrototypeOf(current);
            niveau++;
            
            if (niveau > 8) break;
        }
    },
    
    // Chercher où est définie une propriété
    chercherPropriete(obj, prop) {
        console.log(`\n🔍 Recherche de "${prop}":`);
        
        let current = obj;
        let niveau = 0;
        
        while (current) {
            if (current.hasOwnProperty(prop)) {
                const type = typeof current[prop];
                console.log(`  ✅ Trouvée au niveau ${niveau} (${type})`);
                console.log(`     Constructeur: ${current.constructor?.name || 'Inconnu'}`);
                return { niveau, objet: current, type };
            }
            
            current = Object.getPrototypeOf(current);
            niveau++;
            
            if (niveau > 10) break;
        }
        
        console.log(`  ❌ "${prop}" non trouvée dans la chaîne`);
        return null;
    },
    
    // Comparer deux objets et leurs prototypes
    comparer(obj1, obj2, nom1 = "objet1", nom2 = "objet2") {
        console.log(`\n⚖️ Comparaison ${nom1} vs ${nom2}:`);
        
        const proto1 = Object.getPrototypeOf(obj1);
        const proto2 = Object.getPrototypeOf(obj2);
        
        console.log(`  Même prototype: ${proto1 === proto2}`);
        console.log(`  ${nom1} hérite de ${nom2}: ${proto2?.isPrototypeOf?.(obj1) || false}`);
        console.log(`  ${nom2} hérite de ${nom1}: ${proto1?.isPrototypeOf?.(obj2) || false}`);
        
        // Propriétés communes
        const props1 = new Set(Object.getOwnPropertyNames(obj1));
        const props2 = new Set(Object.getOwnPropertyNames(obj2));
        const communes = [...props1].filter(p => props2.has(p));
        
        console.log(`  Propriétés communes: ${communes.join(', ') || 'aucune'}`);
    },
    
    // Analyser les performances (méthodes partagées vs dupliquées)
    analyserPerformances(objets, nomMethode) {
        console.log(`\n⚡ Analyse de performance pour "${nomMethode}":`);
        
        if (objets.length < 2) {
            console.log("  ⚠️ Au moins 2 objets nécessaires");
            return;
        }
        
        const premiereMethode = objets[0][nomMethode];
        let partagee = true;
        
        for (let i = 1; i < objets.length; i++) {
            if (objets[i][nomMethode] !== premiereMethode) {
                partagee = false;
                break;
            }
        }
        
        console.log(`  Méthode partagée: ${partagee ? '✅' : '❌'}`);
        console.log(`  ${partagee ? 'Optimisé' : 'Chaque objet a sa copie'} (${objets.length} objets)`);
        
        if (!partagee) {
            console.log("  💡 Conseil: Déplacer vers le prototype pour optimiser");
        }
    }
};

// Tests des outils
function TestClass(nom) {
    this.nom = nom;
}
TestClass.prototype.dire = function() { return "Hello"; };

let obj1 = new TestClass("obj1");
let obj2 = new TestClass("obj2");

PrototypeUtils.afficherChaine(obj1, "obj1");
PrototypeUtils.chercherPropriete(obj1, "dire");
PrototypeUtils.comparer(obj1, obj2);
PrototypeUtils.analyserPerformances([obj1, obj2], "dire");
```
{% endtab %}
{% endtabs %}

## ⚠️ Pièges courants et solutions

{% tabs %}
{% tab title="🪤 Piège 1: Propriétés référence partagées" %}
```javascript
// ❌ PROBLÈME: Propriétés référence dans le prototype
function Equipe(nom) {
    this.nom = nom;
}

// ERREUR: Array partagé entre toutes les instances !
Equipe.prototype.membres = [];
Equipe.prototype.scores = {};

Equipe.prototype.ajouterMembre = function(membre) {
    this.membres.push(membre);
};

Equipe.prototype.ajouterScore = function(joueur, score) {
    this.scores[joueur] = score;
};

// Test du problème
let equipeA = new Equipe("Alpha");
let equipeB = new Equipe("Beta");

equipeA.ajouterMembre("Alice");
equipeB.ajouterMembre("Bob");

console.log("❌ Équipe A:", equipeA.membres); // ["Alice", "Bob"] - INCORRECT !
console.log("❌ Équipe B:", equipeB.membres); // ["Alice", "Bob"] - INCORRECT !

equipeA.ajouterScore("Alice", 100);
equipeB.ajouterScore("Bob", 200);

console.log("❌ Scores A:", equipeA.scores); // {Alice: 100, Bob: 200} - INCORRECT !
console.log("❌ Scores B:", equipeB.scores); // {Alice: 100, Bob: 200} - INCORRECT !

// ✅ SOLUTION 1: Initialiser dans le constructor
function EquipeCorrigee(nom) {
    this.nom = nom;
    this.membres = [];  // Chaque instance a son propre array
    this.scores = {};   // Chaque instance a son propre objet
}

EquipeCorrigee.prototype.ajouterMembre = function(membre) {
    this.membres.push(membre);
};

EquipeCorrigee.prototype.ajouterScore = function(joueur, score) {
    this.scores[joueur] = score;
};

// Test de la solution
let equipeC = new EquipeCorrigee("Charlie");
let equipeD = new EquipeCorrigee("Delta");

equipeC.ajouterMembre("Charlie");
equipeD.ajouterMembre("David");

console.log("✅ Équipe C:", equipeC.membres); // ["Charlie"] - CORRECT !
console.log("✅ Équipe D:", equipeD.membres); // ["David"] - CORRECT !

// ✅ SOLUTION 2: Avec Object.create
let EquipePrototype = {
    init(nom) {
        this.nom = nom;
        this.membres = [];
        this.scores = {};
        return this;
    },
    
    ajouterMembre(membre) {
        this.membres.push(membre);
    },
    
    ajouterScore(joueur, score) {
        this.scores[joueur] = score;
    }
};

let equipeE = Object.create(EquipePrototype).init("Echo");
let equipeF = Object.create(EquipePrototype).init("Foxtrot");

equipeE.ajouterMembre("Echo1");
equipeF.ajouterMembre("Foxtrot1");

console.log("✅ Équipe E:", equipeE.membres); // ["Echo1"] - CORRECT !
console.log("✅ Équipe F:", equipeF.membres); // ["Foxtrot1"] - CORRECT !
```
{% endtab %}

{% tab title="🎯 Piège 2: Perte du contexte this" %}
```javascript
// ❌ PROBLÈME: Perte du contexte this
function Personne(nom) {
    this.nom = nom;
}

Personne.prototype.saluer = function() {
    console.log(`Bonjour, je suis ${this.nom}`);
};

Personne.prototype.saluerAvecDelai = function(delai = 1000) {
    setTimeout(function() {
        // ❌ 'this' ne pointe plus vers l'instance !
        console.log(`Salut, je suis ${this.nom}`); // undefined
    }, delai);
};

let alice = new Personne("Alice");
alice.saluer(); // "Bonjour, je suis Alice" ✅

// Problèmes courants
let methode = alice.saluer;
methode(); // "Bonjour, je suis undefined" ❌

alice.saluerAvecDelai(); // "Salut, je suis undefined" ❌

// ✅ SOLUTIONS:

// Solution 1: Arrow function (conserve this lexical)
Personne.prototype.saluerAvecDelaiCorrige1 = function(delai = 1000) {
    setTimeout(() => {
        console.log(`✅ Arrow: Salut, je suis ${this.nom}`);
    }, delai);
};

// Solution 2: bind()
Personne.prototype.saluerAvecDelaiCorrige2 = function(delai = 1000) {
    setTimeout(function() {
        console.log(`✅ Bind: Salut, je suis ${this.nom}`);
    }.bind(this), delai);
};

// Solution 3: Variable de capture
Personne.prototype.saluerAvecDelaiCorrige3 = function(delai = 1000) {
    const self = this;
    setTimeout(function() {
        console.log(`✅ Self: Salut, je suis ${self.nom}`);
    }, delai);
};

// Solution 4: call/apply pour extraction de méthode
let methodeCorrigee = alice.saluer.bind(alice);
methodeCorrigee(); // "Bonjour, je suis Alice" ✅

// Tests des solutions
alice.saluerAvecDelaiCorrige1(500);
alice.saluerAvecDelaiCorrige2(1000);
alice.saluerAvecDelaiCorrige3(1500);

// Solution pour callbacks
function executerCallback(callback, contexte) {
    if (contexte) {
        callback.call(contexte);
    } else {
        callback();
    }
}

executerCallback(alice.saluer, alice); // ✅ Contexte préservé
```
{% endtab %}

{% tab title="🔒 Piège 3: Pollution de prototype" %}
```javascript
// ⚠️ ATTENTION: Modification des prototypes natifs

// ❌ DANGEREUX: Modifier Object.prototype
Object.prototype.monMethode = function() {
    return "Ajouté à tous les objets !";
};

// Impact sur TOUS les objets
let obj = {};
console.log(obj.monMethode()); // "Ajouté à tous les objets !"

// Problème avec for...in
for (let prop in obj) {
    console.log(prop); // "monMethode" apparaît !
}

// ❌ DANGEREUX: Modifier Array.prototype
Array.prototype.dernierElement = function() {
    return this[this.length - 1];
};

let arr = [1, 2, 3];
console.log(arr.dernierElement()); // 3

// Mais pollue tous les arrays
for (let index in arr) {
    console.log(index); // "0", "1", "2", "dernierElement" !
}

// ✅ SOLUTIONS SÛRES:

// Solution 1: Utiliser des classes ou prototypes personnalisés
class MonArray extends Array {
    dernierElement() {
        return this[this.length - 1];
    }
    
    premierElement() {
        return this[0];
    }
}

let monArr = new MonArray(1, 2, 3);
console.log(monArr.dernierElement()); // 3

// Solution 2: Fonctions utilitaires
const ArrayUtils = {
    dernierElement(arr) {
        return arr[arr.length - 1];
    },
    
    premierElement(arr) {
        return arr[0];
    },
    
    estVide(arr) {
        return arr.length === 0;
    }
};

console.log(ArrayUtils.dernierElement([1, 2, 3])); // 3

// Solution 3: Créer des prototypes spécialisés
let MonObjetPrototype = {
    init(data) {
        Object.assign(this, data);
        return this;
    },
    
    clone() {
        return Object.create(Object.getPrototypeOf(this)).init(this);
    },
    
    equals(other) {
        return JSON.stringify(this) === JSON.stringify(other);
    }
};

let obj1 = Object.create(MonObjetPrototype).init({ nom: "test", valeur: 42 });
let obj2 = obj1.clone();

console.log(obj1.equals(obj2)); // true

// ✅ Méthode sûre pour étendre si vraiment nécessaire
if (!Array.prototype.includes) { // Polyfill conditionnel
    Array.prototype.includes = function(searchElement) {
        return this.indexOf(searchElement) !== -1;
    };
}

// Nettoyage (pour les tests)
delete Object.prototype.monMethode;
delete Array.prototype.dernierElement;
```
{% endtab %}
{% endtabs %}

## 💡 Quiz de compréhension

{% tabs %}
{% tab title="❓ Questions" %}
**Question 1 :** Quelle est la différence fondamentale entre l'héritage prototypal et l'héritage classique ?

**Question 2 :** Que se passe-t-il quand JavaScript cherche une propriété sur un objet ?

**Question 3 :** Pourquoi `obj.hasOwnProperty('prop')` et `'prop' in obj` peuvent-ils donner des résultats différents ?

**Question 4 :** Comment créer un objet qui hérite d'un autre avec `Object.create()` ?

**Question 5 :** Pourquoi mettre un array dans `Constructor.prototype.items = []` est-il problématique ?

**Question 6 :** Quelle est la relation entre les classes ES6 et les prototypes ?
{% endtab %}

{% tab title="✅ Réponses" %}
**Réponse 1 :** L'héritage prototypal utilise des objets comme modèles (délégation), tandis que l'héritage classique copie les propriétés. JavaScript utilise un système de liens entre objets, pas de classes "réelles".

**Réponse 2 :** JavaScript cherche d'abord sur l'objet lui-même, puis remonte la chaîne prototypale jusqu'à trouver la propriété ou atteindre `null`.

**Réponse 3 :** `hasOwnProperty()` vérifie seulement les propriétés directes de l'objet, tandis que `in` vérifie aussi la chaîne prototypale.

**Réponse 4 :** `let enfant = Object.create(parent)` crée un objet dont le prototype est `parent`.

**Réponse 5 :** Toutes les instances partageront le même array, car il est sur le prototype. Chaque modification affectera toutes les instances.

**Réponse 6 :** Les classes ES6 sont du "sucre syntaxique" : elles créent en réalité des constructor functions et utilisent le système de prototypes sous-jacent.
{% endtab %}
{% endtabs %}

## 🛠️ Exercices pratiques

### Exercice 1 : Migration prototype vers classe
{% tabs %}
{% tab title="📋 Énoncé" %}
Convertissez ce système basé sur les prototypes vers les classes ES6 modernes :

```javascript
// Code existant à convertir
function Livre(titre, auteur, pages) {
    this.titre = titre;
    this.auteur = auteur;
    this.pages = pages;
    this.lu = false;
}

Livre.prototype.lire = function() {
    this.lu = true;
    console.log(`📖 "${this.titre}" terminé !`);
};

Livre.prototype.getInfo = function() {
    return `"${this.titre}" par ${this.auteur} (${this.pages} pages)`;
};

function LivreNumerique(titre, auteur, pages, format, taille) {
    Livre.call(this, titre, auteur, pages);
    this.format = format;
    this.taille = taille;
}

LivreNumerique.prototype = Object.create(Livre.prototype);
LivreNumerique.prototype.constructor = LivreNumerique;

LivreNumerique.prototype.telecharger = function() {
    console.log(`📥 Téléchargement de "${this.titre}" (${this.taille})`);
};

// TODO: Convertir en classes ES6 avec:
// - Syntaxe class/extends
// - Champs privés pour les métadonnées
// - Getters/setters appropriés
// - Méthodes statiques
```
{% endtab %}

{% tab title="💡 Hints" %}
{% hint style="info" %}
**Astuce 1 :** Utilisez `class` et `extends` pour remplacer les constructor functions.

**Astuce 2 :** Les champs privés commencent par `#` en ES2022.

**Astuce 3 :** `super()` remplace `Parent.call(this, ...)`.

**Astuce 4 :** Ajoutez des méthodes statiques pour les statistiques globales.
{% endhint %}
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
class Livre {
    // Champs privés
    #dateCreation = new Date();
    #lu = false;
    
    // Compteur statique
    static #totalLivres = 0;
    
    constructor(titre, auteur, pages) {
        this.titre = titre;
        this.auteur = auteur;
        this.pages = pages;
        Livre.#totalLivres++;
    }
    
    // Getter/setter pour l'état de lecture
    get lu() {
        return this.#lu;
    }
    
    lire() {
        if (!this.#lu) {
            this.#lu = true;
            console.log(`📖 "${this.titre}" terminé !`);
            return true;
        }
        console.log(`📚 "${this.titre}" déjà lu`);
        return false;
    }
    
    getInfo() {
        return `"${this.titre}" par ${this.auteur} (${this.pages} pages)`;
    }
    
    getStatut() {
        return {
            ...this.getInfo(),
            lu: this.#lu,
            dateCreation: this.#dateCreation.toLocaleDateString()
        };
    }
    
    // Méthodes statiques
    static getTotalLivres() {
        return Livre.#totalLivres;
    }
    
    static creerCollection(...livres) {
        return livres.map(data => new Livre(...data));
    }
}

class LivreNumerique extends Livre {
    #telecharge = false;
    #cheminFichier = null;
    
    constructor(titre, auteur, pages, format, taille) {
        super(titre, auteur, pages);
        this.format = format;
        this.taille = taille;
    }
    
    get telecharge() {
        return this.#telecharge;
    }
    
    get cheminFichier() {
        return this.#cheminFichier;
    }
    
    async telecharger(chemin = `/downloads/${this.titre}.${this.format}`) {
        if (this.#telecharge) {
            console.log(`📁 "${this.titre}" déjà téléchargé`);
            return this.#cheminFichier;
        }
        
        console.log(`📥 Téléchargement de "${this.titre}" (${this.taille})...`);
        
        // Simulation du téléchargement
        await new Promise(resolve => setTimeout(resolve, 1000));
        
        this.#telecharge = true;
        this.#cheminFichier = chemin;
        
        console.log(`✅ Téléchargement terminé: ${chemin}`);
        return chemin;
    }
    
    supprimer() {
        if (this.#telecharge) {
            console.log(`🗑️ Suppression de "${this.titre}"`);
            this.#telecharge = false;
            this.#cheminFichier = null;
        }
    }
    
    getInfo() {
        const infoBase = super.getInfo();
        return `${infoBase} [${this.format.toUpperCase()}, ${this.taille}]`;
    }
    
    getStatut() {
        return {
            ...super.getStatut(),
            format: this.format,
            taille: this.taille,
            telecharge: this.#telecharge,
            chemin: this.#cheminFichier
        };
    }
}

// Tests
const livre1 = new Livre("1984", "George Orwell", 328);
const livre2 = new LivreNumerique("Dune", "Frank Herbert", 688, "epub", "2.1MB");

console.log(livre1.getInfo());
livre1.lire();

console.log(livre2.getInfo());
livre2.telecharger().then(() => {
    livre2.lire();
    console.log("Statut:", livre2.getStatut());
});

console.log(`Total livres créés: ${Livre.getTotalLivres()}`);
```
{% endtab %}
{% endtabs %}

### Exercice 2 : Système de cache avec prototypes
{% tabs %}
{% tab title="📋 Énoncé" %}
Créez un système de cache hiérarchique utilisant les prototypes :

**Spécifications :**
1. `CacheBase` : Prototype avec les opérations de base (get, set, has, delete)
2. `CacheMemoire` : Cache en mémoire avec limite de taille
3. `CachePersistant` : Cache qui simule la persistance
4. `CacheMultiNiveau` : Combine mémoire + persistant

**Fonctionnalités requises :**
- Gestion des TTL (Time To Live)
- Statistiques d'utilisation
- Événements (hit, miss, éviction)
- Stratégies d'éviction (LRU, FIFO)

```javascript
// Structure de base à implémenter
let CacheBase = {
    // Méthodes de base à implémenter
};

// Votre implémentation ici...
```
{% endtab %}

{% tab title="💡 Hints" %}
{% hint style="info" %}
**Astuce 1 :** Utilisez `Object.create()` pour créer la hiérarchie de prototypes.

**Astuce 2 :** Stockez les métadonnées (TTL, timestamp) avec les valeurs.

**Astuce 3 :** Implémentez un pattern Observer pour les événements.

**Astuce 4 :** Utilisez `Map` pour préserver l'ordre d'insertion (utile pour LRU).
{% endhint %}
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Solution complète du système de cache hiérarchique
// (Code détaillé disponible dans les ressources)
```
{% endtab %}
{% endtabs %}

## 🎯 Projet pratique : Framework MVC

{% tabs %}
{% tab title="🏗️ Architecture" %}
**Objectif :** Créer un mini-framework MVC basé sur les prototypes

**Structure :**
```
BaseController (prototype)
├── UserController
├── ProductController
└── OrderController

BaseModel (prototype)
├── User
├── Product
└── Order

BaseView (prototype)
├── ListView
├── DetailView
└── FormView
```

**Fonctionnalités :**
- Système d'événements
- Data binding
- Routing simple
- Validation
{% endtab %}

{% tab title="🚀 Implémentation" %}
```javascript
// Framework de base avec prototypes
let BaseController = {
    init(model, view) {
        this.model = model;
        this.view = view;
        this.setupEventListeners();
        return this;
    },
    
    setupEventListeners() {
        // À implémenter par les sous-contrôleurs
    },
    
    render() {
        this.view.render(this.model.getData());
    }
};

// Exemple d'utilisation
let UserController = Object.create(BaseController);
UserController.setupEventListeners = function() {
    this.view.on('userSelect', (userId) => {
        this.model.loadUser(userId);
        this.render();
    });
};

// Continuez l'implémentation...
```
{% endtab %}
{% endtabs %}

## 🔗 Liens et références

{% tabs %}
{% tab title="📚 Approfondissement" %}
**Concepts liés :**
- [Classes et POO](45-classes-poo.md) - Syntaxe moderne
- [Fonctions Avancées](15-fonctions-avancees.md) - Constructors et this
- [Modules et Organisation](95-modules-organisation.md) - Structurer le code

**Documentation officielle :**
- [MDN Prototype](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)
- [MDN Object.create](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
- [MDN Inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
{% endtab %}

{% tab title="🛠️ Outils de débogage" %}
**Console du navigateur :**
- `obj.__proto__` - Accès au prototype (deprecated)
- `Object.getPrototypeOf(obj)` - Méthode recommandée
- `Object.setPrototypeOf(obj, proto)` - Modification

**DevTools :**
- Onglet "Prototype" dans l'inspecteur
- `console.dir(obj)` pour explorer la structure
- Breakpoints sur l'accès aux propriétés
{% endtab %}
{% endtabs %}

## 🎯 Points clés à retenir

{% hint style="success" %}
**✅ Concepts essentiels :**
- JavaScript utilise l'héritage prototypal, pas classique
- Chaque objet a un lien vers son prototype
- Les classes ES6 sont du sucre syntaxique sur les prototypes
- `Object.create()` est la méthode moderne pour l'héritage
- Les méthodes dans le prototype sont partagées (optimisation)

**⚡ Bonnes pratiques :**
- Préférer les classes ES6 pour le nouveau code
- Comprendre les prototypes pour déboguer et optimiser
- Ne jamais modifier les prototypes natifs
- Initialiser les propriétés référence dans le constructor
- Utiliser `hasOwnProperty()` pour distinguer propre/hérité
{% endhint %}

{% hint style="warning" %}
**⚠️ Pièges à éviter :**
- Propriétés référence dans le prototype (partagées !)
- Perte du contexte `this` dans les callbacks
- Modification des prototypes natifs (pollution)
- Confusion entre `hasOwnProperty()` et `in`
- Oublier de restaurer `constructor` après héritage manuel
{% endhint %}

---

> **Navigation :** [← Classes et POO](45-classes-poo.md) | [DOM Manipulation →](55-dom-manipulation.md)
