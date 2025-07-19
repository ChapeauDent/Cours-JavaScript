# 📖 Glossaire JavaScript

## A

### API (Application Programming Interface)
Interface qui permet à différentes applications ou composants de communiquer entre eux. En JavaScript, on utilise souvent des APIs web comme l'API DOM ou Fetch API.

### Arguments
Liste des valeurs passées à une fonction lors de son appel. Les arguments sont accessibles via l'objet `arguments` dans les fonctions classiques.

### Array (Tableau)
Structure de données qui stocke une collection ordonnée d'éléments. Les arrays en JavaScript sont dynamiques et peuvent contenir des éléments de différents types.

### Arrow Function (Fonction fléchée)
Syntaxe moderne (`=>`) pour déclarer des fonctions. Les arrow functions ont un comportement différent pour le contexte `this` et ne possèdent pas leur propre objet `arguments`.

### Asynchrone
Mode d'exécution qui permet au code de continuer à s'exécuter sans attendre qu'une opération soit terminée. Géré avec des callbacks, Promises ou async/await.

## B

### Binding
Processus qui détermine la valeur de `this` dans une fonction. Peut être explicite (`bind()`, `call()`, `apply()`) ou implicite selon le contexte d'appel.

### Block Scope
Portée délimitée par des accolades `{}`. Les variables déclarées avec `let` et `const` ont une portée de bloc, contrairement à `var`.

### Boolean
Type de données primitif qui ne peut avoir que deux valeurs : `true` ou `false`. Utilisé pour les conditions et la logique.

### BOM (Browser Object Model)
Interface qui permet à JavaScript d'interagir avec le navigateur (window, location, navigator, etc.).

## C

### Callback
Fonction passée en argument à une autre fonction et exécutée à un moment donné. Fondamental pour la programmation asynchrone et événementielle.

### Class
Modèle pour créer des objets avec des propriétés et méthodes communes. Introduction d'ES6 pour la programmation orientée objet.

### Closure
Mécanisme qui permet à une fonction d'accéder aux variables de sa portée lexicale même après que cette portée ait été fermée. Fondamental pour l'encapsulation.

### Const
Mot-clé pour déclarer des constantes. La valeur ne peut pas être réassignée, mais les objets et arrays peuvent être mutés.

### Constructor
Méthode spéciale d'une classe qui s'exécute lors de la création d'une instance. Utilisé pour initialiser les propriétés de l'objet.

## D

### Debouncing
Technique d'optimisation qui retarde l'exécution d'une fonction jusqu'à ce qu'un certain délai se soit écoulé sans nouvel appel.

### Destructuring
Syntaxe permettant d'extraire des valeurs d'arrays ou d'objets et de les assigner à des variables distinctes.

### DOM (Document Object Model)
Représentation en arbre de la structure HTML d'une page web. Permet à JavaScript de manipuler dynamiquement le contenu et la structure.

## E

### Event (Événement)
Action ou occurrence détectée par le navigateur (click, scroll, keypress, etc.). JavaScript peut écouter et réagir aux événements.

### Event Loop
Mécanisme qui gère l'exécution du code asynchrone en JavaScript. Coordonne la stack d'exécution avec les files de tâches.

### Expression
Combinaison de valeurs, variables et opérateurs qui produit une valeur. Contrairement aux statements, les expressions retournent une valeur.

## F

### Factory Function
Fonction qui retourne un nouvel objet. Alternative aux constructeurs et classes pour créer des objets.

### Falsy
Valeurs qui sont considérées comme `false` dans un contexte booléen : `false`, `0`, `""`, `null`, `undefined`, `NaN`.

### Function
Bloc de code réutilisable qui peut être appelé avec des arguments. Peut être déclarée, exprimée ou fléchée.

## G

### Generator
Fonction spéciale qui peut être mise en pause et reprise. Utilise la syntaxe `function*` et le mot-clé `yield`.

### Global Scope
Portée globale où les variables sont accessibles partout dans le programme. Variables déclarées hors de toute fonction.

## H

### Hoisting
Mécanisme JavaScript qui "remonte" les déclarations de variables et fonctions au début de leur portée lors de la compilation.

### HTML Collection
Collection dynamique d'éléments DOM retournée par des méthodes comme `getElementsByClassName()`. Se met à jour automatiquement.

## I

### IIFE (Immediately Invoked Function Expression)
Fonction qui s'exécute immédiatement après sa définition. Pattern utilisé pour créer une portée isolée.

### Immutability (Immutabilité)
Principe où les données ne peuvent pas être modifiées après leur création. Encourage la création de nouvelles versions plutôt que la mutation.

### Inheritance (Héritage)
Mécanisme permettant à une classe de hériter des propriétés et méthodes d'une autre classe. Implementé avec `extends` en ES6.

### Iterator
Objet qui implémente le protocole d'itération avec une méthode `next()`. Permet de parcourir des collections de manière contrôlée.

## J

### JSON (JavaScript Object Notation)
Format d'échange de données basé sur la syntaxe des objets JavaScript. Méthodes `JSON.parse()` et `JSON.stringify()`.

## L

### Let
Mot-clé pour déclarer des variables avec portée de bloc. Introduit en ES6, alternative recommandée à `var`.

### Lexical Scope
Portée déterminée par l'endroit où les variables sont déclarées dans le code source. Basis des closures.

## M

### Map
Collection qui stocke des paires clé-valeur. Contrairement aux objets, les clés peuvent être de n'importe quel type.

### Method
Fonction qui appartient à un objet. Peut accéder aux propriétés de l'objet via `this`.

### Module
Système pour organiser le code en fichiers séparés avec import/export. Standard ES6 pour la modularisation.

### Mutation
Modification directe d'un objet ou array existant. Opposé à la création d'une nouvelle version.

## N

### NaN (Not a Number)
Valeur spéciale représentant un résultat de calcul mathématique impossible. Type `number` mais égal à aucune valeur.

### Node
Élément individuel dans l'arbre DOM. Peut être un élément, du texte, un commentaire, etc.

### NodeList
Collection statique d'éléments DOM retournée par `querySelectorAll()`. Ne se met pas à jour automatiquement.

### Null
Valeur primitive représentant l'absence intentionnelle de valeur. Type `object` (quirk historique).

## O

### Object
Structure de données composée de paires clé-valeur. Type de base pour les structures complexes en JavaScript.

### Operator
Symbole qui effectue une opération sur une ou plusieurs valeurs (opérandes). Arithmétiques, logiques, de comparaison, etc.

## P

### Parameter
Variable dans la déclaration d'une fonction qui reçoit une valeur (argument) lors de l'appel.

### Polymorphism
Capacité d'objets de différents types à répondre à la même interface. Un des piliers de la POO.

### Promise
Objet représentant l'achèvement ou l'échec d'une opération asynchrone. States : pending, fulfilled, rejected.

### Prototype
Mécanisme d'héritage natif de JavaScript. Chaque objet a un lien vers un prototype dont il peut hériter des propriétés.

### Proxy
Objet qui intercepte et personnalise les opérations effectuées sur un autre objet (get, set, etc.). Métaprogrammation avancée.

## Q

### Query Selector
Méthode pour sélectionner des éléments DOM en utilisant la syntaxe des sélecteurs CSS. `querySelector()` et `querySelectorAll()`.

## R

### Recursion
Technique où une fonction s'appelle elle-même. Utile pour résoudre des problèmes divisibles en sous-problèmes similaires.

### Reference
Pointeur vers un emplacement mémoire contenant une valeur. Les objets et arrays sont passés par référence.

### RegExp (Regular Expression)
Pattern utilisé pour faire correspondre des combinaisons de caractères dans des chaînes. Outil puissant pour la manipulation de texte.

## S

### Scope
Portée ou contexte dans lequel une variable est accessible. Global, function, ou block scope.

### Set
Collection de valeurs uniques. Contrairement aux arrays, ne permet pas les doublons.

### Spread Operator
Syntaxe `...` qui permet d'étendre un itérable en éléments individuels. Utile pour les arrays, objets et paramètres de fonction.

### String
Type de données primitif pour représenter du texte. Immutable en JavaScript.

### Symbol
Type de données primitif unique introduit en ES6. Utilisé pour créer des identifiants de propriétés uniques.

## T

### Template Literals
Syntaxe avec backticks `` ` `` permettant l'interpolation de variables et les chaînes multi-lignes. Introduit en ES6.

### This
Mot-clé qui fait référence au contexte d'exécution d'une fonction. Sa valeur dépend de comment la fonction est appelée.

### Throttling
Technique d'optimisation qui limite le nombre d'exécutions d'une fonction dans un intervalle de temps donné.

### Truthy
Valeurs considérées comme `true` dans un contexte booléen. Toutes les valeurs sauf les falsy values.

### Type Coercion
Conversion automatique entre différents types de données. Peut être implicite ou explicite.

## U

### Undefined
Valeur primitive assignée automatiquement aux variables déclarées mais non initialisées.

## V

### Var
Ancien mot-clé pour déclarer des variables. Portée de fonction et hoisting peuvent causer des problèmes.

### Variable
Conteneur nommé pour stocker des données. Peut être déclarée avec `var`, `let`, ou `const`.

### Virtual DOM
Représentation virtuelle du DOM réel utilisée par certains frameworks (React) pour optimiser les performances.

## W

### WeakMap
Collection de paires clé-valeur où les clés sont des objets et peuvent être garbage collected. Ne permet pas l'énumération.

### WeakSet
Collection d'objets uniques qui permet le garbage collection des objets qu'elle contient.

### Web API
Interfaces fournies par le navigateur pour interagir avec diverses fonctionnalités (DOM, Fetch, Geolocation, etc.).

## Y

### Yield
Mot-clé utilisé dans les générateurs pour mettre en pause l'exécution et retourner une valeur.

---

## 📚 Voir aussi

- **[Liens Utiles](liens-utiles.md)** - Ressources externes pour approfondir
- **[Outils Recommandés](outils.md)** - Environnements et extensions
- **[FAQ](faq.md)** - Questions fréquemment posées

---

> **💡 Conseil :** Ce glossaire est un référentiel vivant. N'hésitez pas à y revenir régulièrement lors de votre apprentissage pour clarifier les concepts JavaScript.
