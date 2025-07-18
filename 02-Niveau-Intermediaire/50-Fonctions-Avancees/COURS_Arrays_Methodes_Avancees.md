# Méthodes Avancées des Arrays en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnées du cours
- **Titre :** Méthodes Avancées des Arrays - Programmation Fonctionnelle
- **Niveau :** Intermédiaire
- **Durée estimée :** 75 minutes
- **Prérequis :** Arrays basiques, fonctions, concepts de callback
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Les méthodes fonctionnelles des arrays (map, filter, reduce, etc.)
- [ ] **Appliquer** : La programmation fonctionnelle avec les arrays
- [ ] **Analyser** : Quand utiliser chaque méthode selon le contexte
- [ ] **Créer** : Des chaînes de traitement de données élégantes et efficaces

### Mots-clés à retenir
- **Immutabilité** : Ne pas modifier l'array original
- **Programmation fonctionnelle** : Style de programmation avec des fonctions pures
- **Chaînage** : Enchaîner plusieurs méthodes d'array
- **Callback** : Fonction passée en paramètre à une méthode
- **Prédicat** : Fonction qui retourne true/false

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les méthodes avancées des arrays permettent d'écrire du code plus lisible, plus maintenable et plus expressif. Elles encouragent la programmation fonctionnelle, réduisent les bugs liés aux modifications d'état, et sont devenues essentielles dans le JavaScript moderne.

### Définition et concepts clés

#### **1. Méthodes de transformation**
```javascript
// map() - transforme chaque élément
[1, 2, 3].map(x => x * 2);          // [2, 4, 6]

// filter() - filtre selon une condition
[1, 2, 3, 4].filter(x => x % 2 === 0);  // [2, 4]

// reduce() - réduit à une seule valeur
[1, 2, 3].reduce((acc, x) => acc + x, 0);  // 6
```

#### **2. Méthodes de recherche**
```javascript
// find() - trouve le premier élément
[1, 2, 3].find(x => x > 2);         // 3

// findIndex() - trouve l'index du premier élément
[1, 2, 3].findIndex(x => x > 2);    // 2

// some() - teste si au moins un élément satisfait
[1, 2, 3].some(x => x > 2);         // true

// every() - teste si tous les éléments satisfont
[1, 2, 3].every(x => x > 0);        // true
```

#### **3. Méthodes d'utilitaire**
```javascript
// forEach() - exécute une fonction pour chaque élément
[1, 2, 3].forEach(x => console.log(x));

// includes() - vérifie la présence d'un élément
[1, 2, 3].includes(2);              // true

// sort() - trie les éléments
[3, 1, 2].sort((a, b) => a - b);    // [1, 2, 3]
```

### Syntaxe de base
```javascript
// Structure générale d'une méthode d'array
array.methode((element, index, arrayComplet) => {
    // Logique de traitement
    return nouvelleValeur; // Pour map
    return boolean;        // Pour filter, some, every
});

// Exemples avec tous les paramètres
const nombres = [10, 20, 30];

nombres.map((nombre, index, array) => {
    console.log(`Élément ${nombre} à l'index ${index} sur ${array.length}`);
    return nombre * 2;
});
```

### Points d'attention
- **Piège immutabilité** : map, filter, reduce créent de nouveaux arrays
- **Piège performance** : Éviter les chaînes trop longues sur de gros datasets
- **Bonne pratique** : Préférer les méthodes fonctionnelles aux boucles for

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Traitement d'une liste d'étudiants
console.log("=== TRAITEMENT LISTE ÉTUDIANTS ===");

const etudiants = [
    { nom: "Alice", age: 20, notes: [15, 17, 14] },
    { nom: "Bob", age: 19, notes: [12, 16, 18] },
    { nom: "Charlie", age: 21, notes: [18, 19, 17] },
    { nom: "Diana", age: 20, notes: [13, 14, 15] }
];

// 1. map() - Calculer la moyenne pour chaque étudiant
const etudiantsAvecMoyenne = etudiants.map(etudiant => ({
    ...etudiant,  // Copier toutes les propriétés existantes
    moyenne: etudiant.notes.reduce((sum, note) => sum + note, 0) / etudiant.notes.length
}));

console.log("Étudiants avec moyenne:");
etudiantsAvecMoyenne.forEach(etudiant => {
    console.log(`${etudiant.nom}: ${etudiant.moyenne.toFixed(2)}/20`);
});

// 2. filter() - Trouver les étudiants qui ont la moyenne
const etudiantsAdmis = etudiantsAvecMoyenne.filter(etudiant => etudiant.moyenne >= 15);

console.log("\nÉtudiants admis (moyenne ≥ 15):");
etudiantsAdmis.forEach(etudiant => {
    console.log(`✅ ${etudiant.nom}: ${etudiant.moyenne.toFixed(2)}/20`);
});

// 3. find() - Trouver le meilleur étudiant
const meilleurEtudiant = etudiantsAvecMoyenne.find(etudiant => 
    etudiant.moyenne === Math.max(...etudiantsAvecMoyenne.map(e => e.moyenne))
);

console.log(`\nMeilleur étudiant: ${meilleurEtudiant.nom} (${meilleurEtudiant.moyenne.toFixed(2)}/20)`);

// 4. some() et every() - Vérifications globales
const tousAdultes = etudiants.every(etudiant => etudiant.age >= 18);
const certainsExcellents = etudiantsAvecMoyenne.some(etudiant => etudiant.moyenne >= 18);

console.log(`\nTous adultes: ${tousAdultes}`);
console.log(`Certains excellents (≥18): ${certainsExcellents}`);
```

**Résultat attendu :**
```
=== TRAITEMENT LISTE ÉTUDIANTS ===
Étudiants avec moyenne:
Alice: 15.33/20
Bob: 15.33/20
Charlie: 18.00/20
Diana: 14.00/20

Étudiants admis (moyenne ≥ 15):
✅ Alice: 15.33/20
✅ Bob: 15.33/20
✅ Charlie: 18.00/20

Meilleur étudiant: Charlie (18.00/20)

Tous adultes: true
Certains excellents (≥18): true
```

### Exemple intermédiaire
```javascript
// Analyse de données de ventes avec chaînage de méthodes
console.log("=== ANALYSE DONNÉES VENTES ===");

const ventes = [
    { produit: "Laptop", prix: 999, quantite: 5, categorie: "Électronique", vendeur: "Alice" },
    { produit: "Souris", prix: 25, quantite: 20, categorie: "Électronique", vendeur: "Bob" },
    { produit: "Clavier", prix: 75, quantite: 15, categorie: "Électronique", vendeur: "Alice" },
    { produit: "Livre", prix: 15, quantite: 30, categorie: "Éducation", vendeur: "Charlie" },
    { produit: "Cahier", prix: 3, quantite: 50, categorie: "Éducation", vendeur: "Diana" },
    { produit: "Table", prix: 200, quantite: 8, categorie: "Mobilier", vendeur: "Bob" },
    { produit: "Chaise", prix: 80, quantite: 12, categorie: "Mobilier", vendeur: "Alice" }
];

// Chaînage complexe : Analyse des ventes par vendeur
const analyseParVendeur = ventes
    .map(vente => ({
        ...vente,
        chiffreAffaires: vente.prix * vente.quantite
    }))
    .filter(vente => vente.chiffreAffaires > 100) // Exclure les petites ventes
    .reduce((acc, vente) => {
        // Grouper par vendeur
        if (!acc[vente.vendeur]) {
            acc[vente.vendeur] = {
                vendeur: vente.vendeur,
                nombreVentes: 0,
                chiffreAffairesTotal: 0,
                produits: []
            };
        }
        
        acc[vente.vendeur].nombreVentes++;
        acc[vente.vendeur].chiffreAffairesTotal += vente.chiffreAffaires;
        acc[vente.vendeur].produits.push(vente.produit);
        
        return acc;
    }, {});

// Convertir l'objet en array et trier par CA
const classementVendeurs = Object.values(analyseParVendeur)
    .sort((a, b) => b.chiffreAffairesTotal - a.chiffreAffairesTotal);

console.log("Classement des vendeurs (ventes > 100€):");
classementVendeurs.forEach((vendeur, index) => {
    console.log(`${index + 1}. ${vendeur.vendeur}:`);
    console.log(`   CA: ${vendeur.chiffreAffairesTotal}€`);
    console.log(`   Ventes: ${vendeur.nombreVentes}`);
    console.log(`   Produits: ${vendeur.produits.join(', ')}`);
    console.log();
});

// Analyse par catégorie avec reduce avancé
const analyseCategories = ventes.reduce((acc, vente) => {
    const { categorie, prix, quantite } = vente;
    
    if (!acc[categorie]) {
        acc[categorie] = {
            categorie,
            nombreProduits: 0,
            quantiteTotale: 0,
            chiffreAffaires: 0,
            prixMoyen: 0
        };
    }
    
    acc[categorie].nombreProduits++;
    acc[categorie].quantiteTotale += quantite;
    acc[categorie].chiffreAffaires += prix * quantite;
    
    return acc;
}, {});

// Calculer le prix moyen après regroupement
Object.values(analyseCategories).forEach(cat => {
    const produitsCategorie = ventes.filter(v => v.categorie === cat.categorie);
    cat.prixMoyen = produitsCategorie.reduce((sum, v) => sum + v.prix, 0) / produitsCategorie.length;
});

console.log("Analyse par catégorie:");
Object.values(analyseCategories)
    .sort((a, b) => b.chiffreAffaires - a.chiffreAffaires)
    .forEach(cat => {
        console.log(`📂 ${cat.categorie}:`);
        console.log(`   Produits: ${cat.nombreProduits}`);
        console.log(`   Quantité vendue: ${cat.quantiteTotale}`);
        console.log(`   CA: ${cat.chiffreAffaires}€`);
        console.log(`   Prix moyen: ${cat.prixMoyen.toFixed(2)}€`);
        console.log();
    });
```

### Exemple avancé (optionnel)
```javascript
// Système de recommandation avec méthodes avancées
console.log("=== SYSTÈME DE RECOMMANDATION ===");

const utilisateurs = [
    { id: 1, nom: "Alice", preferences: ["action", "sci-fi", "thriller"] },
    { id: 2, nom: "Bob", preferences: ["comédie", "romance", "drame"] },
    { id: 3, nom: "Charlie", preferences: ["action", "thriller", "aventure"] },
    { id: 4, nom: "Diana", preferences: ["sci-fi", "drame", "thriller"] }
];

const films = [
    { id: 1, titre: "Matrix", genres: ["action", "sci-fi"], note: 8.7, annee: 1999 },
    { id: 2, titre: "Inception", genres: ["action", "sci-fi", "thriller"], note: 8.8, annee: 2010 },
    { id: 3, titre: "Amélie", genres: ["comédie", "romance"], note: 8.3, annee: 2001 },
    { id: 4, titre: "Titanic", genres: ["romance", "drame"], note: 7.8, annee: 1997 },
    { id: 5, titre: "Mad Max", genres: ["action", "aventure"], note: 8.1, annee: 2015 },
    { id: 6, titre: "Interstellar", genres: ["sci-fi", "drame"], note: 8.6, annee: 2014 }
];

// Fonction de recommandation avancée
function recommanderFilms(utilisateurId, nombreRecommandations = 3) {
    const utilisateur = utilisateurs.find(u => u.id === utilisateurId);
    
    if (!utilisateur) {
        return { erreur: "Utilisateur non trouvé" };
    }
    
    // Calculer le score de compatibilité pour chaque film
    const filmsAvecScore = films
        .map(film => {
            // Calculer le nombre de genres en commun
            const genresCommuns = film.genres.filter(genre => 
                utilisateur.preferences.includes(genre)
            ).length;
            
            // Score basé sur genres communs et note du film
            const scoreGenres = genresCommuns / utilisateur.preferences.length;
            const scoreNote = film.note / 10;
            const scoreAnnee = film.annee >= 2000 ? 1 : 0.8; // Bonus films récents
            
            const scoreFinal = (scoreGenres * 0.5) + (scoreNote * 0.4) + (scoreAnnee * 0.1);
            
            return {
                ...film,
                scoreCompatibilite: scoreFinal,
                genresCommuns: genresCommuns,
                raisonRecommandation: genresCommuns > 0 ? 
                    `${genresCommuns} genre(s) en commun: ${film.genres.filter(g => utilisateur.preferences.includes(g)).join(', ')}` :
                    "Film populaire"
            };
        })
        .filter(film => film.genresCommuns > 0) // Garder seulement films avec au moins 1 genre commun
        .sort((a, b) => b.scoreCompatibilite - a.scoreCompatibilite) // Trier par score
        .slice(0, nombreRecommandations); // Prendre le top N
    
    return {
        utilisateur: utilisateur.nom,
        preferences: utilisateur.preferences,
        recommandations: filmsAvecScore
    };
}

// Tests du système de recommandation
[1, 2, 3, 4].forEach(userId => {
    const recommandations = recommanderFilms(userId, 2);
    
    console.log(`🎬 Recommandations pour ${recommandations.utilisateur}:`);
    console.log(`Préférences: ${recommandations.preferences.join(', ')}`);
    console.log();
    
    recommandations.recommandations.forEach((film, index) => {
        console.log(`${index + 1}. ${film.titre} (${film.annee})`);
        console.log(`   Note: ${film.note}/10`);
        console.log(`   Genres: ${film.genres.join(', ')}`);
        console.log(`   Score: ${(film.scoreCompatibilite * 100).toFixed(1)}%`);
        console.log(`   Raison: ${film.raisonRecommandation}`);
        console.log();
    });
    
    console.log("---".repeat(20));
});

// Analyse globale avec reduce complexe
const statistiquesGlobales = films.reduce((stats, film) => {
    // Mise à jour des statistiques générales
    stats.nombreFilms++;
    stats.noteTotal += film.note;
    stats.anneeMoyenne += film.annee;
    
    // Analyse par genre
    film.genres.forEach(genre => {
        if (!stats.genresPopulaires[genre]) {
            stats.genresPopulaires[genre] = { count: 0, notesMoyenne: [] };
        }
        stats.genresPopulaires[genre].count++;
        stats.genresPopulaires[genre].notesMoyenne.push(film.note);
    });
    
    // Analyse par décennie
    const decennie = Math.floor(film.annee / 10) * 10;
    const decennieLabel = `${decennie}s`;
    if (!stats.parDecennie[decennieLabel]) {
        stats.parDecennie[decennieLabel] = { films: 0, noteMoyenne: [] };
    }
    stats.parDecennie[decennieLabel].films++;
    stats.parDecennie[decennieLabel].noteMoyenne.push(film.note);
    
    return stats;
}, {
    nombreFilms: 0,
    noteTotal: 0,
    anneeMoyenne: 0,
    genresPopulaires: {},
    parDecennie: {}
});

// Finaliser les calculs
statistiquesGlobales.noteMoyenne = statistiquesGlobales.noteTotal / statistiquesGlobales.nombreFilms;
statistiquesGlobales.anneeMoyenne = Math.round(statistiquesGlobales.anneeMoyenne / statistiquesGlobales.nombreFilms);

// Calculer moyennes par genre
Object.values(statistiquesGlobales.genresPopulaires).forEach(genreData => {
    genreData.noteMoyenne = genreData.notesMoyenne.reduce((a, b) => a + b, 0) / genreData.notesMoyenne.length;
    delete genreData.notesMoyenne; // Nettoyer
});

// Calculer moyennes par décennie
Object.values(statistiquesGlobales.parDecennie).forEach(decennieData => {
    decennieData.noteMoyenne = decennieData.noteMoyenne.reduce((a, b) => a + b, 0) / decennieData.noteMoyenne.length;
});

console.log("📊 STATISTIQUES GLOBALES:");
console.log(`Films total: ${statistiquesGlobales.nombreFilms}`);
console.log(`Note moyenne: ${statistiquesGlobales.noteMoyenne.toFixed(2)}/10`);
console.log(`Année moyenne: ${statistiquesGlobales.anneeMoyenne}`);

console.log("\nGenres les plus populaires:");
Object.entries(statistiquesGlobales.genresPopulaires)
    .sort(([,a], [,b]) => b.count - a.count)
    .forEach(([genre, data]) => {
        console.log(`${genre}: ${data.count} films (note moy: ${data.noteMoyenne.toFixed(2)})`);
    });
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Analyse d'un catalogue de produits
**Consigne :** Créez des fonctions d'analyse pour un catalogue e-commerce en utilisant les méthodes d'arrays.

**Code de départ :**
```javascript
const produits = [
    { nom: "iPhone", prix: 999, stock: 50, categorie: "Téléphone", promotion: true },
    { nom: "Samsung Galaxy", prix: 799, stock: 30, categorie: "Téléphone", promotion: false },
    { nom: "MacBook", prix: 1299, stock: 20, categorie: "Ordinateur", promotion: true },
    { nom: "Dell XPS", prix: 899, stock: 25, categorie: "Ordinateur", promotion: false },
    { nom: "iPad", prix: 599, stock: 40, categorie: "Tablette", promotion: true },
    { nom: "Sony Headphones", prix: 199, stock: 60, categorie: "Audio", promotion: false }
];

// TODO: Implémenter ces fonctions avec les méthodes d'arrays
function produitsPrixDecroissant(produits) {
    // Trier par prix décroissant
}

function produitsEnPromotion(produits) {
    // Filtrer les produits en promotion et calculer le prix après remise de 20%
}

function analyseParCategorie(produits) {
    // Grouper par catégorie avec statistiques
}

function stockFaible(produits, seuil = 30) {
    // Trouver les produits avec stock en dessous du seuil
}
```

**Résultat attendu :**
- Fonctions utilisant map, filter, reduce, sort
- Données transformées et analysées correctement

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Gestionnaire de playlist musicale
**Consigne :** Créez un système de gestion de playlist avec recherche et tri avancés.

**Contraintes :**
- Utiliser find, findIndex, some, every
- Implémenter recherche multicritères
- Calculer statistiques (durée totale, genre majoritaire, etc.)

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Analyse de logs de serveur (optionnel)
**Consigne :** Analysez des logs de serveur web pour extraire des insights.

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function produitsPrixDecroissant(produits) {
    return produits
        .slice() // Créer une copie pour éviter la mutation
        .sort((a, b) => b.prix - a.prix);
}

function produitsEnPromotion(produits) {
    return produits
        .filter(produit => produit.promotion)
        .map(produit => ({
            ...produit,
            prixOriginal: produit.prix,
            prixPromo: Math.round(produit.prix * 0.8),
            economie: Math.round(produit.prix * 0.2)
        }));
}

function analyseParCategorie(produits) {
    return produits.reduce((acc, produit) => {
        const cat = produit.categorie;
        
        if (!acc[cat]) {
            acc[cat] = {
                categorie: cat,
                nombreProduits: 0,
                stockTotal: 0,
                prixMoyen: 0,
                prixMin: Infinity,
                prixMax: 0,
                sommePrix: 0
            };
        }
        
        acc[cat].nombreProduits++;
        acc[cat].stockTotal += produit.stock;
        acc[cat].sommePrix += produit.prix;
        acc[cat].prixMin = Math.min(acc[cat].prixMin, produit.prix);
        acc[cat].prixMax = Math.max(acc[cat].prixMax, produit.prix);
        acc[cat].prixMoyen = acc[cat].sommePrix / acc[cat].nombreProduits;
        
        return acc;
    }, {});
}

function stockFaible(produits, seuil = 30) {
    return produits
        .filter(produit => produit.stock < seuil)
        .sort((a, b) => a.stock - b.stock) // Trier par stock croissant
        .map(produit => ({
            ...produit,
            alerteNiveau: produit.stock < seuil * 0.5 ? 'CRITIQUE' : 'ATTENTION'
        }));
}

// Tests
console.log("=== TESTS FONCTIONS PRODUITS ===");

console.log("1. Produits par prix décroissant:");
produitsPrixDecroissant(produits).forEach(p => 
    console.log(`${p.nom}: ${p.prix}€`)
);

console.log("\n2. Produits en promotion:");
produitsEnPromotion(produits).forEach(p => 
    console.log(`${p.nom}: ${p.prixOriginal}€ → ${p.prixPromo}€ (économie: ${p.economie}€)`)
);

console.log("\n3. Analyse par catégorie:");
Object.values(analyseParCategorie(produits)).forEach(cat => {
    console.log(`${cat.categorie}:`);
    console.log(`  - ${cat.nombreProduits} produits`);
    console.log(`  - Stock total: ${cat.stockTotal}`);
    console.log(`  - Prix: ${cat.prixMin}€ - ${cat.prixMax}€ (moy: ${cat.prixMoyen.toFixed(2)}€)`);
});

console.log("\n4. Stock faible:");
stockFaible(produits).forEach(p => 
    console.log(`⚠️ ${p.nom}: ${p.stock} unités (${p.alerteNiveau})`)
);
```

**Explication :** Les fonctions utilisent différentes méthodes d'arrays pour transformer et analyser les données de façon immutable.

### Solution Exercice 2
```javascript
class GestionnairePlaylist {
    constructor(chansons = []) {
        this.chansons = chansons;
    }
    
    // Ajouter une chanson
    ajouterChanson(chanson) {
        this.chansons.push({
            ...chanson,
            id: Date.now() + Math.random(),
            dateAjout: new Date()
        });
    }
    
    // Recherche multicritères
    rechercher(criteres) {
        return this.chansons.filter(chanson => {
            return Object.entries(criteres).every(([cle, valeur]) => {
                if (typeof valeur === 'string') {
                    return chanson[cle]?.toLowerCase().includes(valeur.toLowerCase());
                }
                return chanson[cle] === valeur;
            });
        });
    }
    
    // Trouver une chanson spécifique
    trouverChanson(predicate) {
        return this.chansons.find(predicate);
    }
    
    // Vérifications
    contientArtiste(nomArtiste) {
        return this.chansons.some(chanson => 
            chanson.artiste.toLowerCase().includes(nomArtiste.toLowerCase())
        );
    }
    
    toutesDeGenre(genre) {
        return this.chansons.every(chanson => chanson.genre === genre);
    }
    
    // Statistiques complètes
    obtenirStatistiques() {
        if (this.chansons.length === 0) {
            return { message: "Playlist vide" };
        }
        
        // Durée totale
        const dureeTotale = this.chansons.reduce((total, chanson) => 
            total + chanson.duree, 0);
        
        // Genre majoritaire
        const genresCount = this.chansons.reduce((acc, chanson) => {
            acc[chanson.genre] = (acc[chanson.genre] || 0) + 1;
            return acc;
        }, {});
        
        const genreMajoritaire = Object.entries(genresCount)
            .sort(([,a], [,b]) => b - a)[0];
        
        // Artiste le plus présent
        const artistesCount = this.chansons.reduce((acc, chanson) => {
            acc[chanson.artiste] = (acc[chanson.artiste] || 0) + 1;
            return acc;
        }, {});
        
        const artistePrincipal = Object.entries(artistesCount)
            .sort(([,a], [,b]) => b - a)[0];
        
        // Durée moyenne
        const dureeMoyenne = dureeTotale / this.chansons.length;
        
        // Chanson la plus longue/courte
        const chansons = this.chansons.slice().sort((a, b) => a.duree - b.duree);
        const plusCourte = chansons[0];
        const plusLongue = chansons[chansons.length - 1];
        
        return {
            nombreChansons: this.chansons.length,
            dureeTotale: Math.round(dureeTotale),
            dureeMoyenne: Math.round(dureeMoyenne),
            genreMajoritaire: {
                genre: genreMajoritaire[0],
                count: genreMajoritaire[1]
            },
            artistePrincipal: {
                artiste: artistePrincipal[0],
                count: artistePrincipal[1]
            },
            plusCourte: {
                titre: plusCourte.titre,
                duree: plusCourte.duree
            },
            plusLongue: {
                titre: plusLongue.titre,
                duree: plusLongue.duree
            },
            genres: Object.keys(genresCount).sort(),
            artistes: Object.keys(artistesCount).sort()
        };
    }
    
    // Tri avancé
    trierPar(critere, ordre = 'asc') {
        const copie = this.chansons.slice();
        
        return copie.sort((a, b) => {
            let valeurA = a[critere];
            let valeurB = b[critere];
            
            // Gestion des strings (tri alphabétique)
            if (typeof valeurA === 'string') {
                valeurA = valeurA.toLowerCase();
                valeurB = valeurB.toLowerCase();
            }
            
            let comparaison;
            if (valeurA < valeurB) {
                comparaison = -1;
            } else if (valeurA > valeurB) {
                comparaison = 1;
            } else {
                comparaison = 0;
            }
            
            return ordre === 'desc' ? -comparaison : comparaison;
        });
    }
    
    // Créer des playlists dérivées
    creerPlaylistParGenre(genre) {
        const chansonsGenre = this.chansons.filter(chanson => 
            chanson.genre.toLowerCase() === genre.toLowerCase()
        );
        return new GestionnairePlaylist(chansonsGenre);
    }
    
    creerPlaylistAleatoire(nombreChansons) {
        const chansonsAleatoires = this.chansons
            .slice()
            .sort(() => Math.random() - 0.5)
            .slice(0, nombreChansons);
        return new GestionnairePlaylist(chansonsAleatoires);
    }
}

// Données de test
const playlist = new GestionnairePlaylist();

// Ajouter des chansons
[
    { titre: "Bohemian Rhapsody", artiste: "Queen", genre: "Rock", duree: 355 },
    { titre: "Hotel California", artiste: "Eagles", genre: "Rock", duree: 390 },
    { titre: "Imagine", artiste: "John Lennon", genre: "Pop", duree: 183 },
    { titre: "Billie Jean", artiste: "Michael Jackson", genre: "Pop", duree: 294 },
    { titre: "Smells Like Teen Spirit", artiste: "Nirvana", genre: "Grunge", duree: 301 },
    { titre: "Another One Bites the Dust", artiste: "Queen", genre: "Rock", duree: 215 }
].forEach(chanson => playlist.ajouterChanson(chanson));

// Tests
console.log("=== TESTS GESTIONNAIRE PLAYLIST ===");

console.log("1. Recherche 'Queen':");
playlist.rechercher({ artiste: "Queen" }).forEach(c => 
    console.log(`♪ ${c.titre} - ${c.artiste}`)
);

console.log("\n2. Recherche genre 'Rock':");
playlist.rechercher({ genre: "Rock" }).forEach(c => 
    console.log(`♪ ${c.titre} - ${c.artiste}`)
);

console.log("\n3. Contient Queen?", playlist.contientArtiste("Queen"));
console.log("4. Toutes Rock?", playlist.toutesDeGenre("Rock"));

console.log("\n5. Tri par durée (décroissant):");
playlist.trierPar('duree', 'desc').slice(0, 3).forEach(c => 
    console.log(`♪ ${c.titre}: ${c.duree}s`)
);

console.log("\n6. Statistiques:");
const stats = playlist.obtenirStatistiques();
console.log(`Chansons: ${stats.nombreChansons}`);
console.log(`Durée totale: ${Math.floor(stats.dureeTotale/60)}:${(stats.dureeTotale%60).toString().padStart(2,'0')}`);
console.log(`Genre majoritaire: ${stats.genreMajoritaire.genre} (${stats.genreMajoritaire.count} chansons)`);
console.log(`Artiste principal: ${stats.artistePrincipal.artiste} (${stats.artistePrincipal.count} chansons)`);

console.log("\n7. Playlist Rock uniquement:");
const playlistRock = playlist.creerPlaylistParGenre("Rock");
playlistRock.chansons.forEach(c => console.log(`🎸 ${c.titre} - ${c.artiste}`));
```

**Alternatives possibles :**
- Gestion des favoris avec Set
- Historique d'écoute avec tracking
- Export/Import de playlists

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Arrays basiques** : Manipulation d'éléments et propriétés length
- **Fonctions** : Callbacks et fonctions fléchées
- **Objets** : Destructuring et spread operator
- **Conditions** : Prédicats et logique booléenne

### Prochaines étapes
- **Objets avancés** : Combinaison avec les méthodes d'objets
- **Programmation asynchrone** : Array.map avec Promise.all
- **Structures de données** : Map et Set pour des cas d'usage spécialisés

### Concepts complémentaires
- **Performance** : Optimisation des chaînes de méthodes
- **Typage** : TypeScript pour typer les callbacks
- **Tests** : Testing des transformations de données

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un analyseur de données de réseaux sociaux

**Fonctionnalités requises :**
- [ ] Analyser les posts par utilisateur (engagement, hashtags, etc.)
- [ ] Détecter les tendances (hashtags populaires, pics d'activité)
- [ ] Recommander du contenu basé sur les interactions
- [ ] Générer des statistiques détaillées
- [ ] Filtrer et trier selon différents critères

**Structure suggérée :**
```
📁 analyseur-social/
├── 📄 index.html
├── 📄 style.css
├── 📄 donnees-posts.js
├── 📄 analyseur.js
└── 📄 interface.js
```

**Temps estimé :** 100 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Array](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN - Array.prototype.map](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [MDN - Array.prototype.filter](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [MDN - Array.prototype.reduce](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

### Vidéos recommandées
- "Functional Programming with Arrays" - Fun Fun Function (25 min)
- Pourquoi cette vidéo est utile : Approche pratique de la programmation fonctionnelle

### Articles approfondis
- "Understanding Array Reduce" - freeCodeCamp
- Ce que cet article apporte en plus : Explications détaillées de reduce avec exemples

### Outils de pratique
- [Array Method Exercises](https://github.com/wesbos/beginner-javascript-exercises)
- [Functional Programming Exercises](https://repl.it/@functional-programming)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre map() et forEach() ?
   - Réponse : map() retourne un nouvel array transformé, forEach() exécute une fonction sans retour

2. **Question pratique :** Comment grouper un array d'objets par propriété ?
   - Réponse : Utiliser reduce() pour créer un objet avec les propriétés comme clés

3. **Question d'application :** Que fait ce code ?
   ```javascript
   array.filter(x => x > 5).map(x => x * 2).reduce((a, b) => a + b, 0)
   ```
   - Réponse : Filtre les nombres > 5, les double, puis fait la somme

### Checklist de maîtrise
- [ ] Je comprends l'immutabilité des méthodes d'arrays
- [ ] Je sais choisir entre map, filter, reduce selon le besoin
- [ ] Je maîtrise le chaînage de méthodes
- [ ] Je comprends les callbacks et leurs paramètres
- [ ] Je peux créer des transformations complexes
- [ ] Je reconnais les cas d'usage de chaque méthode

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
const names = users.map(user => {
    console.log(user.name);  // ❌ Pas de return
});
console.log(names);  // [undefined, undefined, ...]
```

**Solution :**
```javascript
const names = users.map(user => {
    console.log(user.name);
    return user.name;  // ✅ Return explicite
});
// Ou plus concis:
const names = users.map(user => user.name);
```

**Explication :** map() doit retourner une valeur pour chaque élément.

### Erreur fréquente 2
**Code problématique :**
```javascript
const result = array.filter().map().reduce();  // ❌ Chaînage sans paramètres
```

**Solution :**
```javascript
const result = array
    .filter(item => condition)
    .map(item => transformation)
    .reduce((acc, item) => acc + item, 0);
```

**Explication :** Chaque méthode nécessite ses paramètres appropriés.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les méthodes fonctionnelles permettent un code plus expressif
- L'importance de l'immutabilité pour éviter les bugs
- Les patterns de chaînage pour des transformations complexes

### Questions à approfondir
- Performance des chaînes de méthodes sur de gros datasets
- Utilisation avec TypeScript pour un typage strict

### Révision programmée
- [ ] Révision dans 1 jour (rappel rapide)
- [ ] Révision dans 1 semaine (exercices)
- [ ] Révision dans 1 mois (application dans un projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #arrays #programmation-fonctionnelle #map #filter #reduce #intermediaire

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 7.5/126*
