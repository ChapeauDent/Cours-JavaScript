# 📋 Arrays (Tableaux) en JavaScript

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- ✅ **Comprendre** ce qu'est un array et pourquoi c'est utile
- ✅ **Créer** des arrays avec différents types de données  
- ✅ **Manipuler** des éléments (ajouter, supprimer, modifier)
- ✅ **Parcourir** des arrays avec des boucles

{% hint style="info" %}
**Prérequis :** Variables, types de données, fonctions, boucles
**Durée estimée :** 120 minutes  
**Niveau :** Intermédiaire
{% endhint %}

## 🧠 Comprendre les Arrays

### Pourquoi utiliser des Arrays ?

Imagine que tu veux stocker les prénoms de tes amis :

{% tabs %}
{% tab title="❌ Sans Arrays" %}
```javascript
let ami1 = "Alice";
let ami2 = "Bob"; 
let ami3 = "Charlie";
// Et si tu as 50 amis ? 50 variables ? 😰
```
{% endtab %}

{% tab title="✅ Avec Arrays" %}
```javascript
let mesAmis = ["Alice", "Bob", "Charlie"];
// Une seule variable pour tous tes amis ! 🎉
```
{% endtab %}
{% endtabs %}

### Les concepts clés

{% hint style="success" %}
**Un array est une liste ordonnée d'éléments :**

- **Syntaxe :** Entre crochets `[element1, element2, element3]`
- **Index :** Position de chaque élément (commence à 0 !)
- **Length :** Nombre total d'éléments  
- **Types mixtes :** Un array peut contenir différents types
{% endhint %}

### Mots-clés essentiels

| Terme | Définition | Exemple |
|-------|------------|---------|
| **Array** | Liste ordonnée d'éléments | `["pomme", "banane"]` |
| **Index** | Position (commence à 0) | `fruits[0]` = "pomme" |
| **Length** | Nombre d'éléments | `fruits.length` = 2 |
| **Push** | Ajouter à la fin | `fruits.push("kiwi")` |
| **Pop** | Enlever le dernier | `fruits.pop()` |

## 💻 Syntaxe et utilisation

### Créer des Arrays

{% tabs %}
{% tab title="Arrays simples" %}
```javascript
// Array de fruits
let fruits = ["pomme", "banane", "orange"];

// Array de nombres
let nombres = [1, 2, 3, 4, 5];

// Array vide
let maListe = [];
```
{% endtab %}

{% tab title="Arrays mixtes" %}
```javascript
// Un array peut contenir différents types
let melange = ["Alice", 14, true, 3.14];

// Array d'objets
let personnes = [
    {nom: "Alice", age: 25},
    {nom: "Bob", age: 30}
];
```
{% endtab %}
{% endtabs %}

### Accéder aux éléments

{% hint style="warning" %}
**Important :** L'index commence à 0, pas à 1 !
{% endhint %}

```javascript
let fruits = ["pomme", "banane", "orange"];

console.log(fruits[0]);    // "pomme" (premier élément)
console.log(fruits[1]);    // "banane" (deuxième élément)  
console.log(fruits[2]);    // "orange" (troisième élément)

// Propriétés utiles
console.log(fruits.length);     // 3 (nombre d'éléments)
console.log(fruits[fruits.length - 1]); // "orange" (dernier élément)
```

### Méthodes principales

{% tabs %}
{% tab title="Ajouter des éléments" %}
```javascript
let fruits = ["pomme", "banane"];

// Ajouter à la fin
fruits.push("orange");
console.log(fruits); // ["pomme", "banane", "orange"]

// Ajouter au début  
fruits.unshift("kiwi");
console.log(fruits); // ["kiwi", "pomme", "banane", "orange"]
```
{% endtab %}

{% tab title="Supprimer des éléments" %}
```javascript
let fruits = ["kiwi", "pomme", "banane", "orange"];

// Supprimer le dernier
let dernierFruit = fruits.pop();
console.log(dernierFruit); // "orange"
console.log(fruits); // ["kiwi", "pomme", "banane"]

// Supprimer le premier
let premierFruit = fruits.shift();
console.log(premierFruit); // "kiwi"
console.log(fruits); // ["pomme", "banane"]
```
{% endtab %}

{% tab title="Modifier des éléments" %}
```javascript
let fruits = ["pomme", "banane", "orange"];

// Modifier un élément
fruits[1] = "poire";
console.log(fruits); // ["pomme", "poire", "orange"]

// Ajouter/supprimer à une position précise
fruits.splice(1, 1, "kiwi", "mangue");
console.log(fruits); // ["pomme", "kiwi", "mangue", "orange"]
```
{% endtab %}
{% endtabs %}

## 🎮 Exercices interactifs

### 📝 Exercice 1 : Ma liste de courses

Créons un gestionnaire de liste de courses :

{% tabs %}
{% tab title="🎯 Objectif" %}
**Créer un système pour :**
- Afficher tous les articles
- Ajouter un nouvel article
- Supprimer le premier article (acheté)
- Compter les articles restants

**Niveau :** ⭐⭐⭐ (3/5)
{% endtab %}

{% tab title="💡 Code de départ" %}
```javascript
let listeCourses = ["pain", "lait", "oeufs", "fromage"];

console.log("=== MA LISTE DE COURSES ===");
console.log("Nombre d'articles: " + listeCourses.length);

// Afficher chaque article avec son numéro
for (let i = 0; i < listeCourses.length; i++) {
    let numero = i + 1;
    console.log(numero + ". " + listeCourses[i]);
}
```
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
let listeCourses = ["pain", "lait", "oeufs", "fromage"];

// Fonction pour afficher la liste
function afficherListe() {
    console.log("=== MA LISTE DE COURSES ===");
    
    if (listeCourses.length === 0) {
        console.log("Liste vide ! 🎉");
        return;
    }
    
    for (let i = 0; i < listeCourses.length; i++) {
        console.log((i + 1) + ". " + listeCourses[i]);
    }
    console.log("Total: " + listeCourses.length + " article(s)");
}

// Ajouter un article
listeCourses.push("chocolat");
console.log("✅ Chocolat ajouté !");

afficherListe();

// Acheter le premier article
let articleAchete = listeCourses.shift();
console.log("🛒 Acheté: " + articleAchete);

afficherListe();
```
{% endtab %}
{% endtabs %}

### 🎯 Exercice 2 : Gestionnaire de notes

Créons un carnet de notes intelligent :

{% tabs %}
{% tab title="🎯 Objectif" %}
**Fonctionnalités à créer :**
- Ajouter des notes (entre 0 et 20)
- Calculer la moyenne
- Afficher toutes les notes
- Déterminer la mention

**Niveau :** ⭐⭐⭐⭐ (4/5)
{% endtab %}

{% tab title="💡 Structure de base" %}
```javascript
let notesEleve = [];

function ajouterNote(note) {
    if (note >= 0 && note <= 20) {
        notesEleve.push(note);
        console.log("Note " + note + " ajoutée !");
    } else {
        console.log("❌ Note invalide (0-20)");
    }
}

function calculerMoyenne() {
    if (notesEleve.length === 0) return 0;
    
    // À compléter...
}
```
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
let notesEleve = [];

function ajouterNote(note) {
    if (note >= 0 && note <= 20) {
        notesEleve.push(note);
        console.log("✅ Note " + note + " ajoutée !");
    } else {
        console.log("❌ La note doit être entre 0 et 20");
    }
}

function calculerMoyenne() {
    if (notesEleve.length === 0) return 0;
    
    let total = 0;
    for (let i = 0; i < notesEleve.length; i++) {
        total += notesEleve[i];
    }
    return total / notesEleve.length;
}

function afficherBulletin() {
    console.log("\n📊 === BULLETIN DE NOTES ===");
    
    if (notesEleve.length === 0) {
        console.log("Aucune note enregistrée");
        return;
    }
    
    // Afficher toutes les notes
    for (let i = 0; i < notesEleve.length; i++) {
        console.log("Note " + (i + 1) + ": " + notesEleve[i] + "/20");
    }
    
    // Calculer et afficher la moyenne
    let moyenne = calculerMoyenne();
    console.log("\n📈 Moyenne: " + moyenne.toFixed(2) + "/20");
    
    // Déterminer la mention
    if (moyenne >= 16) {
        console.log("🏆 Mention: Très Bien !");
    } else if (moyenne >= 14) {
        console.log("🥈 Mention: Bien");
    } else if (moyenne >= 12) {
        console.log("🥉 Mention: Assez Bien");  
    } else if (moyenne >= 10) {
        console.log("✅ Mention: Passable");
    } else {
        console.log("❌ Mention: Insuffisant");
    }
}

// Test du système
ajouterNote(15);
ajouterNote(12);
ajouterNote(18);
ajouterNote(14);

afficherBulletin();
```
{% endtab %}
{% endtabs %}

### 🎵 Exercice 3 : Playlist musicale (Avancé)

{% tabs %}
{% tab title="🎯 Objectif" %}
**Gestionnaire de playlist avec :**
- Array d'objets (titre, artiste, durée)
- Ajouter des chansons
- Rechercher par artiste
- Calculer la durée totale
- Supprimer des chansons

**Niveau :** ⭐⭐⭐⭐⭐ (5/5)
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
let playlist = [
    { titre: "Bohemian Rhapsody", artiste: "Queen", duree: 355 },
    { titre: "Imagine", artiste: "John Lennon", duree: 183 },
    { titre: "Hotel California", artiste: "Eagles", duree: 391 }
];

function ajouterChanson(titre, artiste, dureeEnSecondes) {
    let nouvelleChanson = {
        titre: titre,
        artiste: artiste,
        duree: dureeEnSecondes
    };
    
    playlist.push(nouvelleChanson);
    console.log("🎵 Ajouté: " + titre + " par " + artiste);
}

function formaterDuree(secondes) {
    let minutes = Math.floor(secondes / 60);
    let sec = secondes % 60;
    return minutes + ":" + (sec < 10 ? "0" + sec : sec);
}

function afficherPlaylist() {
    console.log("\n🎵 === MA PLAYLIST ===");
    
    let dureeTotal = 0;
    
    for (let i = 0; i < playlist.length; i++) {
        let chanson = playlist[i];
        let dureeFormatee = formaterDuree(chanson.duree);
        
        console.log((i + 1) + ". " + chanson.titre + 
                   " - " + chanson.artiste + 
                   " (" + dureeFormatee + ")");
        
        dureeTotal += chanson.duree;
    }
    
    console.log("\n📊 Statistiques:");
    console.log("Chansons: " + playlist.length);
    console.log("Durée totale: " + formaterDuree(dureeTotal));
}

function rechercherParArtiste(nomArtiste) {
    let chansonsTouvees = [];
    
    for (let i = 0; i < playlist.length; i++) {
        let chanson = playlist[i];
        if (chanson.artiste.toLowerCase().includes(nomArtiste.toLowerCase())) {
            chansonsTouvees.push(chanson);
        }
    }
    
    return chansonsTouvees;
}

// Test du gestionnaire
afficherPlaylist();

ajouterChanson("Stairway to Heaven", "Led Zeppelin", 482);
afficherPlaylist();

// Recherche
let chansonsQueen = rechercherParArtiste("Queen");
console.log("\n🔍 Chansons de Queen: " + chansonsQueen.length);
for (let i = 0; i < chansonsQueen.length; i++) {
    console.log("- " + chansonsQueen[i].titre);
}
```
{% endtab %}
{% endtabs %}

## 🔗 Parcourir des Arrays

### Méthodes de parcours

{% tabs %}
{% tab title="Boucle for classique" %}
```javascript
let fruits = ["pomme", "banane", "orange"];

// Méthode la plus contrôlée
for (let i = 0; i < fruits.length; i++) {
    console.log(i + ": " + fruits[i]);
}
// 0: pomme
// 1: banane  
// 2: orange
```
{% endtab %}

{% tab title="Boucle for...of" %}
```javascript
let fruits = ["pomme", "banane", "orange"];

// Plus simple, pas d'index
for (let fruit of fruits) {
    console.log(fruit);
}
// pomme
// banane
// orange
```
{% endtab %}

{% tab title="Avec index et valeur" %}
```javascript
let fruits = ["pomme", "banane", "orange"];

// Quand on a besoin de l'index ET de la valeur
for (let i = 0; i < fruits.length; i++) {
    let fruit = fruits[i];
    console.log("Position " + i + ": " + fruit);
    
    // Modifier conditionnellement
    if (fruit === "banane") {
        fruits[i] = "poire";
    }
}
```
{% endtab %}
{% endtabs %}

## ⚠️ Erreurs courantes et solutions

### Erreur 1 : Index hors limites

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
let fruits = ["pomme", "banane", "orange"];
console.log(fruits[3]); // undefined - Index n'existe pas !
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
let fruits = ["pomme", "banane", "orange"];

// Vérifier avant d'accéder
if (fruits.length > 3) {
    console.log(fruits[3]);
} else {
    console.log("Index trop grand !");
}

// Ou accéder au dernier élément
console.log(fruits[fruits.length - 1]); // "orange"
```
{% endtab %}
{% endtabs %}

### Erreur 2 : Boucle infinie

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
let nombres = [1, 2, 3];
for (let i = 0; i <= nombres.length; i++) { // <= au lieu de <
    console.log(nombres[i]); // undefined à la fin
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
let nombres = [1, 2, 3];
for (let i = 0; i < nombres.length; i++) { // < pas <=
    console.log(nombres[i]);
}
```
{% endtab %}
{% endtabs %}

### Erreur 3 : Copie vs Référence

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
let array1 = [1, 2, 3];
let array2 = array1; // Ce n'est PAS une copie !
array2.push(4);
console.log(array1); // [1, 2, 3, 4] - modifié aussi !
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
let array1 = [1, 2, 3];

// Vraie copie avec spread operator
let array2 = [...array1];

// Ou avec slice()
let array3 = array1.slice();

array2.push(4);
console.log(array1); // [1, 2, 3] - inchangé
console.log(array2); // [1, 2, 3, 4]
```
{% endtab %}
{% endtabs %}

## 🚀 Projet : Gestionnaire de tâches

### Objectif du projet

Créer un gestionnaire de tâches complet avec les fonctionnalités suivantes :

{% hint style="success" %}
**Fonctionnalités à implémenter :**
- ✅ Ajouter une tâche
- ✅ Marquer comme terminée (supprimer)
- ✅ Afficher toutes les tâches 
- ✅ Compter les tâches par catégorie
- ✅ Statistiques globales
{% endhint %}

{% tabs %}
{% tab title="🏗️ Structure de base" %}
```javascript
let listeTaches = [];

function ajouterTache(description) {
    // À implémenter
}

function terminerTache(index) {
    // À implémenter  
}

function afficherTaches() {
    // À implémenter
}
```
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
let listeTaches = [];

function ajouterTache(description) {
    listeTaches.push(description);
    console.log("✅ Tâche ajoutée: " + description);
    afficherTaches();
}

function terminerTache(index) {
    if (index >= 0 && index < listeTaches.length) {
        let tacheTerminee = listeTaches.splice(index, 1)[0];
        console.log("🎉 Tâche terminée: " + tacheTerminee);
        afficherTaches();
    } else {
        console.log("❌ Numéro de tâche invalide !");
    }
}

function afficherTaches() {
    console.log("\n📋 === MES TÂCHES ===");
    
    if (listeTaches.length === 0) {
        console.log("🎉 Aucune tâche ! Tu es libre !");
        return;
    }
    
    for (let i = 0; i < listeTaches.length; i++) {
        console.log((i + 1) + ". " + listeTaches[i]);
    }
    
    console.log("📊 Total: " + listeTaches.length + " tâche(s)");
}

function statistiquesTaches() {
    let tachesEcole = 0;
    let tachesMaison = 0;
    let autresTaches = 0;
    
    for (let i = 0; i < listeTaches.length; i++) {
        let tache = listeTaches[i].toLowerCase();
        
        if (tache.includes("devoir") || tache.includes("cours")) {
            tachesEcole++;
        } else if (tache.includes("menage") || tache.includes("ranger")) {
            tachesMaison++;
        } else {
            autresTaches++;
        }
    }
    
    console.log("\n📊 === STATISTIQUES ===");
    console.log("🎓 École: " + tachesEcole);
    console.log("🏠 Maison: " + tachesMaison);  
    console.log("📝 Autres: " + autresTaches);
}

// Tests
ajouterTache("Faire les devoirs de maths");
ajouterTache("Ranger ma chambre");
ajouterTache("Appeler grand-maman");

statistiquesTaches();
terminerTache(0); // Terminer la première tâche
```
{% endtab %}
{% endtabs %}

## 🎯 Points clés à retenir

{% hint style="success" %}
**Les arrays permettent de :**

✅ **Stocker** plusieurs valeurs dans une variable  
✅ **Organiser** des données liées (notes, noms, âges)  
✅ **Parcourir** facilement toute la liste  
✅ **Manipuler** les données (ajouter, supprimer, trier)  
{% endhint %}

### Méthodes essentielles à maîtriser

| Méthode | Action | Exemple |
|---------|--------|---------|
| `push()` | Ajouter à la fin | `fruits.push("kiwi")` |
| `pop()` | Supprimer le dernier | `fruits.pop()` |
| `unshift()` | Ajouter au début | `fruits.unshift("mangue")` |
| `shift()` | Supprimer le premier | `fruits.shift()` |
| `splice()` | Ajouter/supprimer à une position | `fruits.splice(1, 1, "poire")` |
| `length` | Nombre d'éléments | `fruits.length` |

## 🔄 Liens avec d'autres concepts

### Concepts utilisés
- **Variables** : Pour stocker les arrays
- **Fonctions** : Pour manipuler proprement  
- **Boucles** : Pour parcourir les éléments
- **Conditions** : Pour filtrer et valider

### Prochaines étapes
- **Méthodes avancées** : map(), filter(), reduce()
- **Objets** : Structurer des données complexes
- **JSON** : Échanger des arrays de données

## ✅ Auto-évaluation

### Questions de compréhension

{% tabs %}
{% tab title="Question 1" %}
**Quel est l'index du premier élément d'un array ?**

<details>
<summary>Voir la réponse</summary>
**Réponse :** 0 (les arrays commencent à l'index 0)
</details>
{% endtab %}

{% tab title="Question 2" %}
**Comment ajouter un élément à la fin d'un array ?**

<details>
<summary>Voir la réponse</summary>
**Réponse :** Utiliser `push()` : `monArray.push(nouvelElement)`
</details>
{% endtab %}

{% tab title="Question 3" %}
**Si un array a 5 éléments, quel est l'index du dernier ?**

<details>
<summary>Voir la réponse</summary>
**Réponse :** 4 (car on commence à 0 : 0,1,2,3,4)
</details>
{% endtab %}
{% endtabs %}

### Checklist de maîtrise

- [ ] Je sais créer un array avec `[]`
- [ ] Je comprends que l'index commence à 0
- [ ] Je peux accéder à un élément avec `array[index]`
- [ ] Je connais `push()`, `pop()`, `shift()`, `unshift()`
- [ ] Je peux parcourir un array avec une boucle `for`
- [ ] Je comprends la propriété `length`

---

{% hint style="info" %}
**Prochaine étape :** [Méthodes d'itération des Arrays](25-arrays-methodes-avancees.md)

Maintenant que vous maîtrisez les bases des arrays, découvrons les méthodes puissantes comme `forEach()`, `map()`, et `filter()` !
{% endhint %}
