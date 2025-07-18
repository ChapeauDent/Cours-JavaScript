# Fonctions Avancées - Closures, Callbacks et Arrow Functions

{% hint style="info" %}
**Niveau :** Intermédiaire  
**Durée estimée :** 120 minutes  
**Prérequis :** Fonctions de base, variables, objets
{% endhint %}

## 🎯 Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

{% tabs %}
{% tab title="Closures" %}
- Comprendre le mécanisme des closures
- Créer des factory functions
- Implémenter l'encapsulation avec les closures
- Utiliser les closures pour gérer l'état privé
{% endtab %}

{% tab title="Callbacks" %}
- Maîtriser les fonctions de callback
- Utiliser les méthodes d'array avec callbacks
- Créer des systèmes d'événements
- Gérer l'asynchrone avec les callbacks
{% endtab %}

{% tab title="Arrow Functions" %}
- Utiliser la syntaxe moderne des arrow functions
- Comprendre les différences avec les fonctions classiques
- Gérer le contexte `this` correctement
- Composer des fonctions avec style fonctionnel
{% endtab %}
{% endtabs %}

---

## 📚 Concepts fondamentaux

### 1. Les Closures

{% hint style="success" %}
**Définition :** Une closure est une fonction qui "capture" et "se souvient" de son environnement lexical, même après que la fonction externe ait terminé son exécution.
{% endhint %}

#### Mécanisme de base

```javascript
// Exemple fondamental de closure
function creerCompteur(valeurInitiale = 0) {
    let compteur = valeurInitiale; // Variable capturée
    
    return function() {
        compteur++; // Accès à la variable externe
        return compteur;
    };
}

const compteur1 = creerCompteur(5);
const compteur2 = creerCompteur(10);

console.log(compteur1()); // 6
console.log(compteur1()); // 7
console.log(compteur2()); // 11
// Chaque compteur garde sa propre "copie" de la variable
```

#### Encapsulation avec closures

{% tabs %}
{% tab title="Module Pattern" %}
```javascript
function creerModule() {
    // Variables privées (non accessibles de l'extérieur)
    let donneesPrivees = [];
    let compteurUtilisation = 0;
    
    // Interface publique
    return {
        ajouter: function(element) {
            donneesPrivees.push(element);
            compteurUtilisation++;
            console.log(`✅ Élément ajouté: ${element}`);
        },
        
        obtenirTous: function() {
            compteurUtilisation++;
            return [...donneesPrivees]; // Copie pour protection
        },
        
        statistiques: function() {
            return {
                nombre: donneesPrivees.length,
                utilisations: compteurUtilisation
            };
        }
    };
}

const monModule = creerModule();
monModule.ajouter("Item 1");
monModule.ajouter("Item 2");
console.log(monModule.statistiques()); // {nombre: 2, utilisations: 3}
// Impossible d'accéder directement à donneesPrivees !
```
{% endtab %}

{% tab title="Cache Pattern" %}
```javascript
function creerCache() {
    const cache = new Map();
    let hits = 0;
    let misses = 0;
    
    return {
        get(cle) {
            if (cache.has(cle)) {
                hits++;
                console.log(`🎯 Cache HIT pour: ${cle}`);
                return cache.get(cle);
            } else {
                misses++;
                console.log(`❌ Cache MISS pour: ${cle}`);
                return null;
            }
        },
        
        set(cle, valeur) {
            cache.set(cle, valeur);
            console.log(`💾 Mise en cache: ${cle}`);
        },
        
        stats() {
            const total = hits + misses;
            return {
                hits,
                misses,
                total,
                hitRate: total > 0 ? (hits / total * 100).toFixed(1) + '%' : '0%'
            };
        }
    };
}
```
{% endtab %}
{% endtabs %}

### 2. Les Callbacks

{% hint style="info" %}
**Définition :** Un callback est une fonction passée en argument à une autre fonction, pour être exécutée à un moment spécifique.
{% endhint %}

#### Callbacks avec les méthodes d'array

```javascript
const nombres = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// map - transformer chaque élément
const doubles = nombres.map(n => n * 2);
console.log("Doubles:", doubles); // [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

// filter - sélectionner selon un critère
const pairs = nombres.filter(n => n % 2 === 0);
console.log("Pairs:", pairs); // [2, 4, 6, 8, 10]

// reduce - réduire à une seule valeur
const somme = nombres.reduce((total, n) => total + n, 0);
console.log("Somme:", somme); // 55

// Combinaison de méthodes (chaining)
const resultat = nombres
    .filter(n => n > 5)        // [6, 7, 8, 9, 10]
    .map(n => n * n)           // [36, 49, 64, 81, 100]
    .reduce((sum, n) => sum + n, 0); // 330
```

#### Système d'événements personnalisé

```javascript
function creerGestionnaireEvenements() {
    const evenements = {};
    
    return {
        on(nom, callback) {
            if (!evenements[nom]) {
                evenements[nom] = [];
            }
            evenements[nom].push(callback);
        },
        
        emit(nom, ...args) {
            if (evenements[nom]) {
                evenements[nom].forEach(callback => {
                    callback(...args);
                });
            }
        },
        
        off(nom, callback) {
            if (evenements[nom]) {
                const index = evenements[nom].indexOf(callback);
                if (index > -1) {
                    evenements[nom].splice(index, 1);
                }
            }
        }
    };
}
```

### 3. Les Arrow Functions

{% hint style="warning" %}
**Attention :** Les arrow functions n'ont pas leur propre `this`, `arguments`, `super`, ou `new.target`. Elles héritent de ces valeurs de leur contexte englobant.
{% endhint %}

#### Syntaxes et équivalences

{% tabs %}
{% tab title="Syntaxes" %}
```javascript
// Fonction classique
function addition(a, b) {
    return a + b;
}

// Arrow function complète
const additionArrow = (a, b) => {
    return a + b;
};

// Arrow function concise (return implicite)
const additionConcise = (a, b) => a + b;

// Un seul paramètre (parenthèses optionnelles)
const carre = x => x * x;

// Aucun paramètre
const obtenirTimestamp = () => Date.now();

// Retour d'objet (attention aux accolades)
const creerPersonne = (nom, age) => ({
    nom,
    age,
    saluer: () => `Bonjour, je suis ${nom}`
});
```
{% endtab %}

{% tab title="Contexte this" %}
```javascript
const objet = {
    nom: "MonObjet",
    
    methodeClassique: function() {
        console.log("Dans méthode classique:", this.nom); // "MonObjet"
        
        // Problème avec setTimeout et function
        setTimeout(function() {
            console.log("Dans setTimeout function:", this.nom); // undefined
        }, 100);
        
        // Solution avec arrow function
        setTimeout(() => {
            console.log("Dans setTimeout arrow:", this.nom); // "MonObjet"
        }, 200);
    },
    
    // ⚠️ Attention : arrow function comme méthode
    methodeArrow: () => {
        console.log("Arrow comme méthode:", this.nom); // undefined
    }
};
```
{% endtab %}
{% endtabs %}

---

## 🧪 Ateliers pratiques

### Atelier 1 : Factory de calculatrices

{% hint style="success" %}
**Objectif :** Créer des calculatrices spécialisées avec les closures
{% endhint %}

```javascript
function creerCalculatrice(type) {
    const operations = {
        'basique': {
            add: (a, b) => a + b,
            sub: (a, b) => a - b,
            mul: (a, b) => a * b,
            div: (a, b) => a / b
        },
        'scientifique': {
            ...this.basique,
            pow: (a, b) => Math.pow(a, b),
            sqrt: (a) => Math.sqrt(a),
            log: (a) => Math.log(a),
            sin: (a) => Math.sin(a)
        }
    };
    
    const historique = [];
    const ops = operations[type] || operations.basique;
    
    return {
        calculer(operation, ...args) {
            if (ops[operation]) {
                const resultat = ops[operation](...args);
                historique.push({
                    operation,
                    arguments: args,
                    resultat,
                    timestamp: new Date()
                });
                return resultat;
            }
            throw new Error(`Opération ${operation} non supportée`);
        },
        
        obtenirHistorique() {
            return [...historique];
        },
        
        operationsDisponibles() {
            return Object.keys(ops);
        }
    };
}

// Tests
const calc = creerCalculatrice('scientifique');
console.log(calc.calculer('add', 5, 3)); // 8
console.log(calc.calculer('pow', 2, 3)); // 8
console.log(calc.obtenirHistorique());
```

### Atelier 2 : Système de filtres composables

```javascript
// Créateur de filtres
const creerFiltre = {
    parAge: (min, max) => personne => 
        personne.age >= min && personne.age <= max,
    
    parSalaire: (min) => personne => 
        personne.salaire >= min,
    
    parVille: (ville) => personne => 
        personne.ville.toLowerCase() === ville.toLowerCase(),
    
    parNom: (terme) => personne => 
        personne.nom.toLowerCase().includes(terme.toLowerCase())
};

// Composition de filtres
const combinerFiltres = (...filtres) => element => 
    filtres.every(filtre => filtre(element));

// Pipeline de traitement
const traiterDonnees = (donnees, ...filtres) => 
    donnees.filter(combinerFiltres(...filtres));

// Données de test
const personnes = [
    {nom: "Alice", age: 25, salaire: 50000, ville: "Paris"},
    {nom: "Bob", age: 30, salaire: 60000, ville: "Lyon"},
    {nom: "Charlie", age: 35, salaire: 70000, ville: "Paris"},
    {nom: "Diana", age: 28, salaire: 55000, ville: "Marseille"}
];

// Utilisation
const resultat = traiterDonnees(
    personnes,
    creerFiltre.parAge(25, 35),
    creerFiltre.parSalaire(55000),
    creerFiltre.parVille("Paris")
);

console.log("Résultat filtré:", resultat);
```

---

## 🎯 Quiz interactif

{% tabs %}
{% tab title="Question 1" %}
**Que va afficher ce code ?**

```javascript
function creerFonction() {
    let x = 10;
    return function() {
        x++;
        return x;
    };
}

const fn1 = creerFonction();
const fn2 = creerFonction();

console.log(fn1()); // ?
console.log(fn1()); // ?
console.log(fn2()); // ?
```

{% hint style="info" %}
**Réponse :** 11, 12, 11

Chaque appel à `creerFonction()` crée une nouvelle closure avec sa propre variable `x`.
{% endhint %}
{% endtab %}

{% tab title="Question 2" %}
**Quelle est la différence principale entre ces deux codes ?**

```javascript
// Code A
const objet = {
    nom: "Test",
    methode: function() {
        return this.nom;
    }
};

// Code B
const objet = {
    nom: "Test",
    methode: () => {
        return this.nom;
    }
};
```

{% hint style="info" %}
**Réponse :** Dans le Code A, `this` fait référence à `objet`, donc `this.nom` retourne "Test". Dans le Code B, l'arrow function n'a pas son propre `this`, donc `this.nom` sera `undefined`.
{% endhint %}
{% endtab %}

{% tab title="Question 3" %}
**Que fait ce code ?**

```javascript
const nombres = [1, 2, 3, 4, 5];
const resultat = nombres
    .map(x => x * 2)
    .filter(x => x > 5)
    .reduce((sum, x) => sum + x, 0);
```

{% hint style="info" %}
**Réponse :** 
1. `map(x => x * 2)` → [2, 4, 6, 8, 10]
2. `filter(x => x > 5)` → [6, 8, 10]
3. `reduce((sum, x) => sum + x, 0)` → 24

Le résultat final est 24.
{% endhint %}
{% endtab %}
{% endtabs %}

---

## 💡 Exercices pratiques

### Exercice 1 : Debounce function

{% hint style="warning" %}
**Difficulté :** ⭐⭐⭐⭐☆
{% endhint %}

**Consigne :** Implémentez une fonction `debounce` qui limite la fréquence d'exécution d'une fonction.

```javascript
function debounce(func, delay) {
    // Votre code ici
    // La fonction ne doit s'exécuter qu'après `delay` ms de "silence"
    // Si appelée avant la fin du délai, remettre le timer à zéro
}

// Test
const sauvegarder = () => console.log("💾 Sauvegarde effectuée");
const sauvegardeDebounced = debounce(sauvegarder, 1000);

// Ces appels ne déclencheront qu'une seule sauvegarde
sauvegardeDebounced();
sauvegardeDebounced();
sauvegardeDebounced();
```

{% hint style="success" %}
**Solution cachée - Cliquez pour révéler**

```javascript
function debounce(func, delay) {
    let timeoutId;
    
    return function(...args) {
        clearTimeout(timeoutId);
        
        timeoutId = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}
```
{% endhint %}

### Exercice 2 : Pipeline de validation

**Consigne :** Créez un système de validation avec composition de fonctions.

```javascript
// Créez des fonctions de validation qui retournent true/false
const validations = {
    estNombre: (valeur) => !isNaN(valeur),
    estPositif: (valeur) => valeur > 0,
    estEntier: (valeur) => Number.isInteger(Number(valeur))
};

// Créez une fonction qui compose plusieurs validations
function valider(valeur, ...validations) {
    // Votre code ici
}

// Test
console.log(valider("5", validations.estNombre, validations.estPositif)); // true
console.log(valider("-3", validations.estNombre, validations.estPositif)); // false
```

---

## 🔗 Liens avec d'autres concepts

### Concepts précédents
- [Variables et portée](15-variables-declarations.md) - Comprendre le scope lexical
- [Fonctions de base](../niveau-debutant/50-structures-controle.md) - Foundation des fonctions
- [Objets](70-objets.md) - Contexte `this` et méthodes

### Concepts suivants
- [Programmation asynchrone](85-programmation-asynchrone.md) - Callbacks et Promises
- [Classes ES6](70-classes-poo.md) - Méthodes et this
- [Modules](95-modules-organisation.md) - Encapsulation avancée

---

## 🎓 Projet pratique

### Système de notifications intelligent

{% hint style="info" %}
**Objectif :** Créer un système complet utilisant tous les concepts appris
{% endhint %}

**Spécifications :**
- Factory pour créer différents types de notifications
- Système d'abonnement avec callbacks
- Debounce pour éviter le spam
- Filtres composables par priorité/type
- Interface fluide (method chaining)

```javascript
// Usage attendu :
const notifier = creerSystemeNotification()
    .avecDebounce(500)
    .avecFiltre(notif => notif.priorite >= 2)
    .surEvenement('nouvelle', (notif) => {
        console.log(`📱 ${notif.message}`);
    });

notifier.envoyer('info', 'Message de test', {priorite: 3});
```

**Temps estimé :** 90 minutes

---

## 🔍 Débogage et erreurs courantes

### Erreur 1 : Confusion avec `this` dans les arrow functions

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
const objet = {
    nom: "Test",
    methode: () => {
        console.log(this.nom); // undefined
    }
};
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
const objet = {
    nom: "Test",
    methode: function() {
        console.log(this.nom); // "Test"
        
        // Ou utiliser arrow function dans un callback
        setTimeout(() => {
            console.log(this.nom); // "Test"
        }, 100);
    }
};
```
{% endtab %}
{% endtabs %}

### Erreur 2 : Closure dans une boucle

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // Affiche 3, 3, 3
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Avec let (block scope)
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // Affiche 0, 1, 2
}

// Ou avec closure explicite
for (var i = 0; i < 3; i++) {
    ((j) => {
        setTimeout(() => console.log(j), 100);
    })(i);
}
```
{% endtab %}
{% endtabs %}

---

## 📖 Résumé et points clés

{% hint style="success" %}
**Les fonctions avancées sont essentielles en JavaScript moderne :**

- **Closures** : Permettent l'encapsulation et la création de factory functions
- **Callbacks** : Fondement de la programmation asynchrone et fonctionnelle
- **Arrow functions** : Syntaxe moderne avec gestion différente du contexte
- **Composition** : Technique pour créer des fonctions complexes à partir de fonctions simples

Ces concepts sont la base de frameworks comme React, Vue.js et des APIs modernes.
{% endhint %}

---

**Navigation :**
- ← Précédent : [Fonctions de base](45-fonctions.md)
- → Suivant : [Closures et Lexical Scope](47-closures-lexical-scope.md)

---

*💡 **Astuce :** Pratiquez en refactorisant du code existant pour utiliser ces techniques. Commencez par remplacer des boucles par des méthodes d'array, puis explorez les closures pour l'encapsulation.*
