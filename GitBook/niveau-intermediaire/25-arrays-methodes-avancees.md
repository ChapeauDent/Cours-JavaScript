# Arrays et Méthodes Avancées - Programmation Fonctionnelle

{% hint style="info" %}
**Niveau :** Intermédiaire  
**Durée estimée :** 75 minutes  
**Prérequis :** Arrays basiques, fonctions, callbacks
{% endhint %}

## 🎯 Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

{% tabs %}
{% tab title="Transformation" %}
- Utiliser `map()` pour transformer des données
- Appliquer `filter()` pour sélectionner des éléments
- Maîtriser `reduce()` pour agrégations complexes
- Composer des chaînes de transformations
{% endtab %}

{% tab title="Recherche" %}
- Trouver des éléments avec `find()` et `findIndex()`
- Tester des conditions avec `some()` et `every()`
- Optimiser les recherches dans les données
{% endtab %}

{% tab title="Patterns" %}
- Implémenter la programmation fonctionnelle
- Créer des pipelines de traitement de données
- Gérer l'immutabilité et les effets de bord
{% endtab %}
{% endtabs %}

---

## 📚 Méthodes essentielles

### 1. Méthodes de transformation

{% hint style="success" %}
**Principe :** Ces méthodes créent de nouveaux arrays sans modifier l'original (immutabilité).
{% endhint %}

#### map() - Transformer chaque élément

```javascript
// Syntaxe de base
const nouveauArray = array.map((element, index, arrayComplet) => {
    return nouvelleValeur;
});

// Exemples pratiques
const nombres = [1, 2, 3, 4, 5];

// Doubler chaque nombre
const doubles = nombres.map(n => n * 2);
console.log(doubles); // [2, 4, 6, 8, 10]

// Transformer des objets
const utilisateurs = [
    { nom: "Alice", age: 25 },
    { nom: "Bob", age: 30 },
    { nom: "Charlie", age: 35 }
];

const profiles = utilisateurs.map(user => ({
    nom: user.nom.toUpperCase(),
    estAdulte: user.age >= 18,
    generation: user.age < 30 ? "Gen Z" : "Millennial"
}));

console.log(profiles);
// [
//   { nom: "ALICE", estAdulte: true, generation: "Gen Z" },
//   { nom: "BOB", estAdulte: true, generation: "Millennial" },
//   { nom: "CHARLIE", estAdulte: true, generation: "Millennial" }
// ]
```

#### filter() - Sélectionner des éléments

```javascript
// Filtrer selon une condition
const nombres = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const pairs = nombres.filter(n => n % 2 === 0);
console.log(pairs); // [2, 4, 6, 8, 10]

const grandsNombres = nombres.filter(n => n > 5);
console.log(grandsNombres); // [6, 7, 8, 9, 10]

// Filtrage complexe d'objets
const produits = [
    { nom: "Laptop", prix: 999, stock: 5, promotion: true },
    { nom: "Souris", prix: 25, stock: 50, promotion: false },
    { nom: "Clavier", prix: 75, stock: 20, promotion: true },
    { nom: "Écran", prix: 299, stock: 0, promotion: false }
];

// Produits en stock et en promotion
const bonnesAffaires = produits.filter(produit => 
    produit.stock > 0 && produit.promotion && produit.prix < 500
);

console.log(bonnesAffaires);
// [{ nom: "Clavier", prix: 75, stock: 20, promotion: true }]
```

#### reduce() - Réduire à une seule valeur

{% hint style="warning" %}
**Attention :** `reduce()` est très puissant mais peut être complexe. Commencez par des cas simples !
{% endhint %}

```javascript
// Syntaxe complète
const resultat = array.reduce((accumulateur, elementCourant, index, array) => {
    return nouvelAccumulateur;
}, valeurInitiale);

// Somme simple
const nombres = [1, 2, 3, 4, 5];
const somme = nombres.reduce((total, nombre) => total + nombre, 0);
console.log(somme); // 15

// Trouvez le maximum
const maximum = nombres.reduce((max, nombre) => 
    nombre > max ? nombre : max
, nombres[0]);

// Compter les occurrences
const fruits = ["pomme", "banane", "pomme", "orange", "banane", "pomme"];
const comptage = fruits.reduce((acc, fruit) => {
    acc[fruit] = (acc[fruit] || 0) + 1;
    return acc;
}, {});

console.log(comptage);
// { pomme: 3, banane: 2, orange: 1 }

// Grouper par propriété
const personnes = [
    { nom: "Alice", ville: "Paris" },
    { nom: "Bob", ville: "Lyon" },
    { nom: "Charlie", ville: "Paris" },
    { nom: "Diana", ville: "Lyon" }
];

const groupesParVille = personnes.reduce((acc, personne) => {
    const ville = personne.ville;
    if (!acc[ville]) {
        acc[ville] = [];
    }
    acc[ville].push(personne);
    return acc;
}, {});

console.log(groupesParVille);
// {
//   Paris: [{ nom: "Alice", ville: "Paris" }, { nom: "Charlie", ville: "Paris" }],
//   Lyon: [{ nom: "Bob", ville: "Lyon" }, { nom: "Diana", ville: "Lyon" }]
// }
```

### 2. Méthodes de recherche

{% tabs %}
{% tab title="find() et findIndex()" %}
```javascript
const utilisateurs = [
    { id: 1, nom: "Alice", actif: true },
    { id: 2, nom: "Bob", actif: false },
    { id: 3, nom: "Charlie", actif: true }
];

// Trouver le premier élément correspondant
const utilisateurActif = utilisateurs.find(user => user.actif);
console.log(utilisateurActif); // { id: 1, nom: "Alice", actif: true }

// Trouver l'index du premier élément correspondant
const indexBob = utilisateurs.findIndex(user => user.nom === "Bob");
console.log(indexBob); // 1

// Si aucun élément trouvé
const inexistant = utilisateurs.find(user => user.nom === "Inexistant");
console.log(inexistant); // undefined

const indexInexistant = utilisateurs.findIndex(user => user.nom === "Inexistant");
console.log(indexInexistant); // -1
```
{% endtab %}

{% tab title="some() et every()" %}
```javascript
const notes = [12, 15, 18, 14, 16];

// some() - au moins un élément satisfait la condition
const auMoinsUneBonneNote = notes.some(note => note >= 15);
console.log(auMoinsUneBonneNote); // true

const auMoinsUneNoteParfaite = notes.some(note => note === 20);
console.log(auMoinsUneNoteParfaite); // false

// every() - tous les éléments satisfont la condition
const toutesNotesPositives = notes.every(note => note >= 10);
console.log(toutesNotesPositives); // true

const toutesNotesExcellentes = notes.every(note => note >= 18);
console.log(toutesNotesExcellentes); // false

// Cas pratiques
const produits = [
    { nom: "A", enStock: true, prix: 50 },
    { nom: "B", enStock: true, prix: 30 },
    { nom: "C", enStock: false, prix: 80 }
];

const tousEnStock = produits.every(p => p.enStock);
console.log(tousEnStock); // false

const certainsAbordables = produits.some(p => p.prix < 40);
console.log(certainsAbordables); // true
```
{% endtab %}
{% endtabs %}

### 3. Chaînage de méthodes

{% hint style="success" %}
**Pattern puissant :** Le chaînage permet de créer des pipelines de transformation élégants.
{% endhint %}

```javascript
const ventes = [
    { produit: "Laptop", prix: 999, quantite: 2, vendeur: "Alice" },
    { produit: "Souris", prix: 25, quantite: 10, vendeur: "Bob" },
    { produit: "Clavier", prix: 75, quantite: 5, vendeur: "Alice" },
    { produit: "Écran", prix: 299, quantite: 1, vendeur: "Charlie" },
    { produit: "Webcam", prix: 89, quantite: 3, vendeur: "Bob" }
];

// Pipeline complexe : analyser les ventes par vendeur
const analyseVendeurs = ventes
    .map(vente => ({
        ...vente,
        chiffreAffaires: vente.prix * vente.quantite
    }))
    .filter(vente => vente.chiffreAffaires > 100)
    .reduce((acc, vente) => {
        const vendeur = vente.vendeur;
        if (!acc[vendeur]) {
            acc[vendeur] = {
                vendeur,
                nombreVentes: 0,
                chiffreAffairesTotal: 0,
                produits: []
            };
        }
        acc[vendeur].nombreVentes++;
        acc[vendeur].chiffreAffairesTotal += vente.chiffreAffaires;
        acc[vendeur].produits.push(vente.produit);
        return acc;
    }, {});

console.log(analyseVendeurs);

// Convertir en array et trier
const classement = Object.values(analyseVendeurs)
    .sort((a, b) => b.chiffreAffairesTotal - a.chiffreAffairesTotal);

console.log("🏆 Classement des vendeurs:");
classement.forEach((vendeur, index) => {
    console.log(`${index + 1}. ${vendeur.vendeur}: ${vendeur.chiffreAffairesTotal}€`);
});
```

---

## 🧪 Ateliers pratiques

### Atelier 1 : Analyse de données e-commerce

{% hint style="info" %}
**Objectif :** Créer un système d'analyse de catalogue complet
{% endhint %}

```javascript
const produits = [
    { nom: "iPhone 15", prix: 999, stock: 50, categorie: "Téléphone", note: 4.8, promotion: true },
    { nom: "Samsung Galaxy", prix: 799, stock: 30, categorie: "Téléphone", note: 4.6, promotion: false },
    { nom: "MacBook Pro", prix: 1999, stock: 20, categorie: "Ordinateur", note: 4.9, promotion: true },
    { nom: "Dell XPS", prix: 1299, stock: 25, categorie: "Ordinateur", note: 4.5, promotion: false },
    { nom: "iPad Air", prix: 599, stock: 40, categorie: "Tablette", note: 4.7, promotion: true },
    { nom: "Surface Pro", prix: 899, stock: 15, categorie: "Tablette", note: 4.3, promotion: false }
];

// Fonction 1: Produits en promotion avec prix réduit
function produitsEnPromotion(produits, tauxReduction = 0.2) {
    return produits
        .filter(p => p.promotion)
        .map(p => ({
            ...p,
            prixOriginal: p.prix,
            prixPromo: Math.round(p.prix * (1 - tauxReduction)),
            economie: Math.round(p.prix * tauxReduction)
        }));
}

// Fonction 2: Top produits par note
function topProduits(produits, limite = 3) {
    return produits
        .slice() // Copie pour éviter mutation
        .sort((a, b) => b.note - a.note)
        .slice(0, limite)
        .map((p, index) => ({
            rang: index + 1,
            nom: p.nom,
            note: p.note,
            badge: index === 0 ? '🥇' : index === 1 ? '🥈' : '🥉'
        }));
}

// Fonction 3: Analyse par catégorie
function analyseParCategorie(produits) {
    return Object.values(
        produits.reduce((acc, produit) => {
            const cat = produit.categorie;
            
            if (!acc[cat]) {
                acc[cat] = {
                    categorie: cat,
                    produits: [],
                    nombreProduits: 0,
                    stockTotal: 0,
                    valeurStock: 0,
                    noteMoyenne: 0,
                    prixMoyen: 0
                };
            }
            
            acc[cat].produits.push(produit);
            acc[cat].nombreProduits++;
            acc[cat].stockTotal += produit.stock;
            acc[cat].valeurStock += produit.prix * produit.stock;
            
            return acc;
        }, {})
    ).map(cat => {
        // Calculs finaux
        cat.noteMoyenne = (cat.produits.reduce((sum, p) => sum + p.note, 0) / cat.nombreProduits).toFixed(2);
        cat.prixMoyen = Math.round(cat.produits.reduce((sum, p) => sum + p.prix, 0) / cat.nombreProduits);
        
        // Nettoyer l'objet
        delete cat.produits;
        
        return cat;
    })
    .sort((a, b) => b.valeurStock - a.valeurStock);
}

// Tests
console.log("🛍️ ANALYSE E-COMMERCE");

console.log("\n1. Produits en promotion:");
produitsEnPromotion(produits).forEach(p => {
    console.log(`💰 ${p.nom}: ${p.prixOriginal}€ → ${p.prixPromo}€ (économie: ${p.economie}€)`);
});

console.log("\n2. Top 3 produits:");
topProduits(produits).forEach(p => {
    console.log(`${p.badge} ${p.rang}. ${p.nom} - Note: ${p.note}/5`);
});

console.log("\n3. Analyse par catégorie:");
analyseParCategorie(produits).forEach(cat => {
    console.log(`📂 ${cat.categorie}:`);
    console.log(`   • ${cat.nombreProduits} produits`);
    console.log(`   • Stock: ${cat.stockTotal} unités (${cat.valeurStock.toLocaleString()}€)`);
    console.log(`   • Prix moyen: ${cat.prixMoyen}€`);
    console.log(`   • Note moyenne: ${cat.noteMoyenne}/5`);
});
```

### Atelier 2 : Système de recommandation

```javascript
// Système de recommandation basé sur les préférences
const utilisateurs = [
    { id: 1, nom: "Alice", genres: ["action", "sci-fi"], ageMin: 12, noteMin: 4.0 },
    { id: 2, nom: "Bob", genres: ["comédie", "romance"], ageMin: 16, noteMin: 3.5 },
    { id: 3, nom: "Charlie", genres: ["thriller", "drame"], ageMin: 18, noteMin: 4.5 }
];

const films = [
    { titre: "Inception", genres: ["action", "sci-fi"], note: 4.8, age: 12, annee: 2010 },
    { titre: "The Matrix", genres: ["action", "sci-fi"], note: 4.6, age: 15, annee: 1999 },
    { titre: "Amélie", genres: ["comédie", "romance"], note: 4.2, age: 6, annee: 2001 },
    { titre: "Pulp Fiction", genres: ["thriller", "drame"], note: 4.9, age: 18, annee: 1994 },
    { titre: "Titanic", genres: ["romance", "drame"], note: 3.8, age: 12, annee: 1997 }
];

function recommanderFilms(utilisateurId, nombreMax = 3) {
    const utilisateur = utilisateurs.find(u => u.id === utilisateurId);
    
    if (!utilisateur) {
        return { erreur: "Utilisateur non trouvé" };
    }
    
    const recommandations = films
        .filter(film => 
            film.age <= utilisateur.ageMin && 
            film.note >= utilisateur.noteMin
        )
        .map(film => {
            // Score de compatibilité
            const genresCommuns = film.genres.filter(genre => 
                utilisateur.genres.includes(genre)
            ).length;
            
            const scoreGenres = genresCommuns / Math.max(utilisateur.genres.length, film.genres.length);
            const scoreNote = film.note / 5;
            const scoreRecent = film.annee >= 2000 ? 1 : 0.8;
            
            return {
                ...film,
                score: (scoreGenres * 0.5 + scoreNote * 0.3 + scoreRecent * 0.2).toFixed(3),
                genresCommuns,
                raison: genresCommuns > 0 ? 
                    `Genres en commun: ${film.genres.filter(g => utilisateur.genres.includes(g)).join(', ')}` :
                    'Film populaire'
            };
        })
        .filter(film => film.genresCommuns > 0)
        .sort((a, b) => b.score - a.score)
        .slice(0, nombreMax);
    
    return {
        utilisateur: utilisateur.nom,
        preferences: utilisateur.genres,
        recommandations
    };
}

// Test du système
console.log("\n🎬 SYSTÈME DE RECOMMANDATION");

[1, 2, 3].forEach(userId => {
    const result = recommanderFilms(userId, 2);
    console.log(`\n👤 ${result.utilisateur} (genres: ${result.preferences.join(', ')}):`);
    
    if (result.recommandations.length === 0) {
        console.log("   Aucune recommandation trouvée");
    } else {
        result.recommandations.forEach((film, index) => {
            console.log(`   ${index + 1}. ${film.titre} (${film.annee})`);
            console.log(`      Score: ${film.score} | Note: ${film.note}/5`);
            console.log(`      ${film.raison}`);
        });
    }
});
```

---

## 🎯 Quiz interactif

{% tabs %}
{% tab title="Question 1" %}
**Que retourne ce code ?**

```javascript
const nombres = [1, 2, 3, 4, 5];
const resultat = nombres
    .filter(n => n % 2 === 0)
    .map(n => n * 3)
    .reduce((sum, n) => sum + n, 0);

console.log(resultat);
```

{% hint style="info" %}
**Réponse :** `18`

**Explication :**
1. `filter(n => n % 2 === 0)` → `[2, 4]` (nombres pairs)
2. `map(n => n * 3)` → `[6, 12]` (multiplier par 3)
3. `reduce((sum, n) => sum + n, 0)` → `18` (6 + 12)
{% endhint %}
{% endtab %}

{% tab title="Question 2" %}
**Laquelle de ces méthodes modifie l'array original ?**

A) `map()`  
B) `filter()`  
C) `reduce()`  
D) `sort()`

{% hint style="info" %}
**Réponse :** D) `sort()`

**Explication :** Les méthodes `map()`, `filter()` et `reduce()` créent de nouveaux arrays/valeurs. `sort()` modifie l'array original. Pour éviter cela, utilisez `array.slice().sort()`.
{% endhint %}
{% endtab %}

{% tab title="Question 3" %}
**Comment grouper un array d'objets par propriété ?**

```javascript
const personnes = [
    { nom: "Alice", ville: "Paris" },
    { nom: "Bob", ville: "Lyon" },
    { nom: "Charlie", ville: "Paris" }
];

// Grouper par ville
```

{% hint style="info" %}
**Réponse :**

```javascript
const groupes = personnes.reduce((acc, personne) => {
    const ville = personne.ville;
    if (!acc[ville]) {
        acc[ville] = [];
    }
    acc[ville].push(personne);
    return acc;
}, {});
```

Cette technique est très courante pour organiser des données.
{% endhint %}
{% endtab %}
{% endtabs %}

---

## 💡 Exercices pratiques

### Exercice 1 : Gestionnaire de playlist

{% hint style="warning" %}
**Difficulté :** ⭐⭐⭐☆☆
{% endhint %}

**Consigne :** Créez un système de gestion de playlist avec toutes les fonctionnalités.

```javascript
const chansons = [
    { titre: "Bohemian Rhapsody", artiste: "Queen", duree: 355, genre: "Rock", note: 4.9 },
    { titre: "Hotel California", artiste: "Eagles", duree: 391, genre: "Rock", note: 4.7 },
    { titre: "Imagine", artiste: "John Lennon", duree: 183, genre: "Pop", note: 4.8 },
    { titre: "Billie Jean", artiste: "Michael Jackson", duree: 294, genre: "Pop", note: 4.6 },
    { titre: "Smells Like Teen Spirit", artiste: "Nirvana", duree: 301, genre: "Grunge", note: 4.5 }
];

// À implémenter :
function creerPlaylist(chansons) {
    return {
        // 1. Filtrer par genre
        parGenre: (genre) => { /* votre code */ },
        
        // 2. Trier par note décroissante
        meilleuresChansons: () => { /* votre code */ },
        
        // 3. Calculer durée totale
        dureeTotale: () => { /* votre code */ },
        
        // 4. Statistiques par artiste
        statistiquesArtistes: () => { /* votre code */ },
        
        // 5. Recherche multicritère
        rechercher: (criteres) => { /* votre code */ }
    };
}

// Tests attendus
const playlist = creerPlaylist(chansons);
console.log(playlist.parGenre("Rock")); // 2 chansons
console.log(playlist.dureeTotale()); // "25:44" (format mm:ss)
```

{% hint style="success" %}
**Solution - Cliquez pour révéler**

```javascript
function creerPlaylist(chansons) {
    return {
        parGenre: (genre) => 
            chansons.filter(c => c.genre.toLowerCase() === genre.toLowerCase()),
        
        meilleuresChansons: () => 
            chansons.slice().sort((a, b) => b.note - a.note),
        
        dureeTotale: () => {
            const totalSecondes = chansons.reduce((total, c) => total + c.duree, 0);
            const minutes = Math.floor(totalSecondes / 60);
            const secondes = totalSecondes % 60;
            return `${minutes}:${secondes.toString().padStart(2, '0')}`;
        },
        
        statistiquesArtistes: () => 
            Object.values(chansons.reduce((acc, chanson) => {
                const artiste = chanson.artiste;
                if (!acc[artiste]) {
                    acc[artiste] = { artiste, chansons: 0, dureeTotale: 0, noteMoyenne: 0, notes: [] };
                }
                acc[artiste].chansons++;
                acc[artiste].dureeTotale += chanson.duree;
                acc[artiste].notes.push(chanson.note);
                return acc;
            }, {})).map(stats => ({
                ...stats,
                noteMoyenne: (stats.notes.reduce((a, b) => a + b, 0) / stats.notes.length).toFixed(2),
                notes: undefined
            })),
        
        rechercher: (criteres) => 
            chansons.filter(chanson => 
                Object.entries(criteres).every(([cle, valeur]) => {
                    if (typeof valeur === 'string') {
                        return chanson[cle].toLowerCase().includes(valeur.toLowerCase());
                    }
                    return chanson[cle] === valeur;
                })
            )
    };
}
```
{% endhint %}

### Exercice 2 : Analyseur de logs

**Consigne :** Analysez des logs de serveur web pour extraire des insights.

```javascript
const logs = [
    { timestamp: "2024-01-15 10:30:15", ip: "192.168.1.1", method: "GET", url: "/api/users", status: 200, size: 1024 },
    { timestamp: "2024-01-15 10:31:20", ip: "192.168.1.2", method: "POST", url: "/api/login", status: 401, size: 256 },
    { timestamp: "2024-01-15 10:32:10", ip: "192.168.1.1", method: "GET", url: "/api/products", status: 200, size: 2048 },
    // ... plus de logs
];

// Créez des fonctions pour :
// 1. Compter les erreurs par code de statut
// 2. Identifier les IPs les plus actives
// 3. Analyser la bande passante par heure
// 4. Détecter les tentatives d'intrusion (trop d'erreurs 401)
```

---

## 🔗 Liens avec d'autres concepts

### Concepts précédents
- [Arrays de base](../niveau-debutant/55-arrays.md) - Foundation des arrays
- [Fonctions](45-fonctions.md) - Callbacks et paramètres
- [Objets](65-objets.md) - Manipulation de propriétés

### Concepts suivants
- [Programmation asynchrone](85-programmation-asynchrone.md) - `Array.map()` avec `Promise.all()`
- [Destructuring](52-destructuring-spread.md) - Simplifier les transformations
- [Classes](70-classes-poo.md) - Méthodes sur les collections

---

## 🎓 Projet pratique : Dashboard analytique

### Créez un tableau de bord de données

{% hint style="info" %}
**Objectif :** Développer un système complet d'analyse de données avec visualisation
{% endhint %}

**Spécifications :**
- Analyser des données de ventes multi-dimensionnelles
- Créer des filtres interactifs et dynamiques
- Implémenter des calculs de KPI en temps réel
- Générer des graphiques de tendances
- Exporter les analyses en différents formats

**Structure suggérée :**
```
📁 dashboard-analytics/
├── 📄 index.html
├── 📄 style.css
├── 📄 donnees.js (dataset volumineux)
├── 📄 analyseur.js (toute la logique arrays)
├── 📄 interface.js (DOM manipulation)
└── 📄 utils.js (fonctions utilitaires)
```

**Fonctionnalités requises :**
- [ ] Filtrage multi-critères avec chaînage
- [ ] Agrégations complexes (sommes, moyennes, groupements)
- [ ] Tri dynamique sur toutes les colonnes
- [ ] Recherche textuelle avancée
- [ ] Calculs de tendances et comparaisons
- [ ] Export des résultats

**Temps estimé :** 2 heures

---

## 🔍 Débogage et optimisation

### Performance des chaînes de méthodes

{% tabs %}
{% tab title="⚠️ Attention" %}
```javascript
// Peut être lent sur de gros datasets
const result = hugeMassiveArray
    .map(expensiveTransformation)
    .filter(complexCondition)
    .map(anotherTransformation)
    .filter(anotherCondition);
```
{% endtab %}

{% tab title="✅ Optimisé" %}
```javascript
// Plus efficace : combiner les opérations
const result = hugeMassiveArray.reduce((acc, item) => {
    const transformed = expensiveTransformation(item);
    if (complexCondition(transformed)) {
        const finalItem = anotherTransformation(transformed);
        if (anotherCondition(finalItem)) {
            acc.push(finalItem);
        }
    }
    return acc;
}, []);
```
{% endtab %}
{% endtabs %}

### Erreurs courantes et solutions

{% hint style="danger" %}
**Erreur :** Modifier l'array pendant l'itération
```javascript
// ❌ Problématique
array.forEach((item, index) => {
    if (condition) {
        array.splice(index, 1); // Modifie pendant iteration
    }
});

// ✅ Correct
const newArray = array.filter(item => !condition);
```
{% endhint %}

---

## 📖 Résumé et bonnes pratiques

{% hint style="success" %}
**Méthodes essentielles maîtrisées :**

- **`map()`** : Transformation 1:1, retourne toujours un array de même taille
- **`filter()`** : Sélection conditionnelle, retourne un sous-ensemble
- **`reduce()`** : Agrégation vers une valeur unique ou structure complexe
- **`find()`/`findIndex()`** : Recherche du premier élément correspondant
- **`some()`/`every()`** : Tests de conditions sur la collection

**Bonnes pratiques :**
- Privilégier l'immutabilité (ne pas modifier les arrays originaux)
- Chaîner les méthodes pour des transformations lisibles
- Utiliser des noms de variables expressifs dans les callbacks
- Considérer les performances sur de gros volumes
{% endhint %}

---

**Navigation :**
- ← Précédent : [Fonctions Avancées](65-fonctions-avancees.md)
- → Suivant : [Destructuring et Spread](52-destructuring-spread.md)

---

*💡 **Astuce :** Ces méthodes sont la base du JavaScript moderne. Maîtrisez-les bien car elles sont utilisées partout : React, Vue.js, Node.js, et dans toutes les applications modernes !*
