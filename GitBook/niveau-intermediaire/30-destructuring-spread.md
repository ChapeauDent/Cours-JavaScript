# Destructuring et Spread/Rest Operator

{% hint style="info" %}
**Niveau :** Intermédiaire  
**Durée estimée :** 85 minutes  
**Prérequis :** Arrays, objets, fonctions
{% endhint %}

## 🎯 Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

{% tabs %}
{% tab title="Destructuring" %}
- Extraire des valeurs d'arrays et d'objets facilement
- Utiliser des valeurs par défaut et renommage
- Destructurer dans les paramètres de fonctions
- Appliquer le destructuring imbriqué
{% endtab %}

{% tab title="Spread Operator" %}
- Étaler des arrays et objets avec `...`
- Copier et fusionner des structures de données
- Passer des arguments de fonction dynamiquement
- Créer des copies immutables
{% endtab %}

{% tab title="Rest Operator" %}
- Collecter des éléments restants avec `...`
- Gérer un nombre variable de paramètres
- Extraire une partie des données
- Combiner avec le destructuring
{% endtab %}
{% endtabs %}

---

## 📚 Le Destructuring

### 1. Destructuring d'arrays

{% hint style="success" %}
**Le destructuring permet d'extraire des valeurs d'arrays ou d'objets** et de les assigner à des variables de manière concise.
{% endhint %}

#### Syntaxe de base

```javascript
// Méthode traditionnelle
const fruits = ["pomme", "banane", "orange"];
const premier = fruits[0];
const deuxieme = fruits[1];
const troisieme = fruits[2];

// Avec destructuring
const [premier, deuxieme, troisieme] = fruits;
console.log(premier);  // "pomme"
console.log(deuxieme); // "banane"
console.log(troisieme); // "orange"

// Extraction partielle
const [premierFruit, , troisiemeFruit] = fruits;
console.log(premierFruit);  // "pomme"
console.log(troisiemeFruit); // "orange" (banane ignorée)
```

#### Techniques avancées

{% tabs %}
{% tab title="Valeurs par défaut" %}
```javascript
const couleurs = ["rouge", "vert"];

// Sans valeurs par défaut
const [primaire, secondaire, tertiaire] = couleurs;
console.log(tertiaire); // undefined

// Avec valeurs par défaut
const [r, v, b = "bleu"] = couleurs;
console.log(r); // "rouge"
console.log(v); // "vert"
console.log(b); // "bleu" (valeur par défaut)

// Valeurs par défaut dynamiques
const getDefaultColor = () => {
    console.log("Génération couleur par défaut");
    return "violet";
};

const [c1, c2, c3 = getDefaultColor()] = ["jaune"];
// getDefaultColor() n'est appelée que si nécessaire
```
{% endtab %}

{% tab title="Échange de variables" %}
```javascript
// Échange traditionnel (avec variable temporaire)
let a = 10, b = 20;
let temp = a;
a = b;
b = temp;

// Échange avec destructuring (élégant !)
let x = 10, y = 20;
[x, y] = [y, x];
console.log(x, y); // 20, 10

// Échange dans un array
const arr = [1, 2, 3, 4, 5];
[arr[0], arr[4]] = [arr[4], arr[0]];
console.log(arr); // [5, 2, 3, 4, 1]

// Rotation de variables
let first = "A", second = "B", third = "C";
[first, second, third] = [second, third, first];
console.log(first, second, third); // "B", "C", "A"
```
{% endtab %}
{% endtabs %}

#### Rest dans le destructuring d'arrays

```javascript
const nombres = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Extraire les premiers et regrouper le reste
const [premier, deuxieme, ...reste] = nombres;
console.log(premier);  // 1
console.log(deuxieme); // 2
console.log(reste);    // [3, 4, 5, 6, 7, 8, 9, 10]

// Extraire début et fin
const [debut, ...milieu] = nombres;
console.log(debut);   // 1
console.log(milieu);  // [2, 3, 4, 5, 6, 7, 8, 9, 10]

// Cas pratique : séparation tête/queue
function traiterDonnees(donnees) {
    if (donnees.length === 0) return "Aucune donnée";
    
    const [tete, ...queue] = donnees;
    return {
        premier: tete,
        reste: queue,
        total: donnees.length
    };
}

console.log(traiterDonnees([10, 20, 30, 40]));
// { premier: 10, reste: [20, 30, 40], total: 4 }
```

### 2. Destructuring d'objets

#### Syntaxe de base

```javascript
const utilisateur = {
    nom: "Alice",
    age: 28,
    email: "alice@example.com",
    ville: "Paris"
};

// Méthode traditionnelle
const nom = utilisateur.nom;
const age = utilisateur.age;
const email = utilisateur.email;

// Avec destructuring
const { nom, age, email } = utilisateur;
console.log(nom);   // "Alice"
console.log(age);   // 28
console.log(email); // "alice@example.com"

// Extraction partielle
const { nom, ville } = utilisateur;
console.log(nom, ville); // "Alice", "Paris"
```

#### Renommage et valeurs par défaut

{% tabs %}
{% tab title="Renommage" %}
```javascript
const produit = {
    nom: "Laptop",
    prix: 999,
    disponible: true
};

// Renommer lors du destructuring
const { 
    nom: nomProduit, 
    prix: prixUnitaire, 
    disponible: enStock 
} = produit;

console.log(nomProduit);   // "Laptop"
console.log(prixUnitaire); // 999
console.log(enStock);      // true

// nom, prix, disponible ne sont pas définis ici
```
{% endtab %}

{% tab title="Valeurs par défaut" %}
```javascript
const configuration = {
    host: "localhost",
    port: 3000
    // ssl manque
};

// Avec valeurs par défaut
const { 
    host, 
    port, 
    ssl = false,
    timeout = 5000 
} = configuration;

console.log(host);    // "localhost"
console.log(port);    // 3000
console.log(ssl);     // false (défaut)
console.log(timeout); // 5000 (défaut)

// Combinaison renommage + défaut
const { 
    host: serveur, 
    ssl: securise = true 
} = configuration;

console.log(serveur);  // "localhost"
console.log(securise); // true (défaut car ssl n'existe pas)
```
{% endtab %}
{% endtabs %}

#### Destructuring imbriqué

```javascript
const entreprise = {
    nom: "TechCorp",
    adresse: {
        rue: "123 Avenue Tech",
        ville: "Paris",
        pays: "France",
        coordonnees: {
            lat: 48.8566,
            lng: 2.3522
        }
    },
    employes: [
        { nom: "Alice", poste: "Dev" },
        { nom: "Bob", poste: "Designer" }
    ]
};

// Destructuring profond
const {
    nom: nomEntreprise,
    adresse: {
        ville,
        pays,
        coordonnees: { lat, lng }
    },
    employes: [premierEmploye, { nom: nomDeuxiemeEmploye }]
} = entreprise;

console.log(nomEntreprise);        // "TechCorp"
console.log(ville);                // "Paris"
console.log(pays);                 // "France"
console.log(lat, lng);             // 48.8566, 2.3522
console.log(premierEmploye);       // { nom: "Alice", poste: "Dev" }
console.log(nomDeuxiemeEmploye);   // "Bob"
```

### 3. Destructuring dans les paramètres de fonctions

{% hint style="info" %}
**Très utile pour les fonctions** qui reçoivent des objets de configuration ou des arrays complexes.
{% endhint %}

```javascript
// Fonction traditionnelle
function creerUtilisateur(donnees) {
    const nom = donnees.nom;
    const age = donnees.age;
    const email = donnees.email || "non-fourni";
    // ...
}

// Avec destructuring dans les paramètres
function creerUtilisateur({ nom, age, email = "non-fourni", actif = true }) {
    console.log(`Création de ${nom}, ${age} ans`);
    console.log(`Email: ${email}, Actif: ${actif}`);
    
    return {
        nom,
        age,
        email,
        actif,
        dateCreation: new Date()
    };
}

// Utilisation
const nouvelUtilisateur = creerUtilisateur({
    nom: "Charlie",
    age: 30,
    email: "charlie@example.com"
});

// Destructuring d'arrays dans les paramètres
function calculerStats([min, max, ...valeurs]) {
    return {
        min,
        max,
        moyenne: valeurs.reduce((a, b) => a + b, 0) / valeurs.length,
        total: valeurs.length + 2
    };
}

const stats = calculerStats([0, 100, 85, 92, 78, 88, 95]);
console.log(stats);
```

---

## 📚 Le Spread Operator (...)

### 1. Spread avec les arrays

{% hint style="success" %}
**Le spread operator `...` permet d'étaler les éléments** d'un array ou d'un objet.
{% endhint %}

```javascript
const fruits = ["pomme", "banane"];
const legumes = ["carotte", "brocoli"];

// Combinaison d'arrays
const aliments = [...fruits, ...legumes];
console.log(aliments); // ["pomme", "banane", "carotte", "brocoli"]

// Insertion au milieu
const nouveauxFruits = ["orange", ...fruits, "kiwi"];
console.log(nouveauxFruits); // ["orange", "pomme", "banane", "kiwi"]

// Copie d'array (shallow copy)
const copie = [...fruits];
copie.push("orange");
console.log(fruits); // ["pomme", "banane"] (inchangé)
console.log(copie);  // ["pomme", "banane", "orange"]

// Conversion NodeList vers Array
const divs = document.querySelectorAll('div');
const divsArray = [...divs]; // Maintenant on peut utiliser map, filter, etc.
```

#### Fonctions utilitaires avec spread

{% tabs %}
{% tab title="Math et agregations" %}
```javascript
const notes = [15, 18, 12, 16, 19, 14];

// Trouver min/max
const maximum = Math.max(...notes);
const minimum = Math.min(...notes);
console.log(`Min: ${minimum}, Max: ${maximum}`); // Min: 12, Max: 19

// Somme avec reduce
const somme = notes.reduce((a, b) => a + b, 0);
const moyenne = somme / notes.length;

// Fonction générique pour statistiques
function calculerStatistiques(...nombres) {
    return {
        min: Math.min(...nombres),
        max: Math.max(...nombres),
        moyenne: nombres.reduce((a, b) => a + b, 0) / nombres.length,
        count: nombres.length
    };
}

console.log(calculerStatistiques(10, 20, 30, 40, 50));
// { min: 10, max: 50, moyenne: 30, count: 5 }
```
{% endtab %}

{% tab title="Manipulation d'arrays" %}
```javascript
// Suppression d'éléments par index
function supprimerParIndex(array, index) {
    return [
        ...array.slice(0, index),
        ...array.slice(index + 1)
    ];
}

const nombres = [1, 2, 3, 4, 5];
const sansTroisieme = supprimerParIndex(nombres, 2);
console.log(sansTroisieme); // [1, 2, 4, 5]

// Insertion à un index spécifique
function insererA(array, index, element) {
    return [
        ...array.slice(0, index),
        element,
        ...array.slice(index)
    ];
}

const avecNouvel = insererA(nombres, 2, "NOUVEAU");
console.log(avecNouvel); // [1, 2, "NOUVEAU", 3, 4, 5]

// Mélange d'array (Fisher-Yates avec spread)
function melangerArray(array) {
    const melange = [...array];
    for (let i = melange.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [melange[i], melange[j]] = [melange[j], melange[i]];
    }
    return melange;
}

console.log(melangerArray([1, 2, 3, 4, 5]));
```
{% endtab %}
{% endtabs %}

### 2. Spread avec les objets

```javascript
const utilisateur = {
    nom: "Alice",
    age: 28,
    email: "alice@example.com"
};

const preferences = {
    theme: "sombre",
    langue: "fr",
    notifications: true
};

// Fusion d'objets
const profilComplet = {
    ...utilisateur,
    ...preferences,
    dateInscription: new Date()
};

console.log(profilComplet);
// {
//   nom: "Alice",
//   age: 28,
//   email: "alice@example.com",
//   theme: "sombre",
//   langue: "fr",
//   notifications: true,
//   dateInscription: 2024-01-15T10:30:00.000Z
// }

// Mise à jour immutable
const utilisateurMisAJour = {
    ...utilisateur,
    age: 29,
    ville: "Paris" // nouvelle propriété
};

console.log(utilisateur);        // Original inchangé
console.log(utilisateurMisAJour); // Nouvelle version avec modifications
```

#### Patterns utiles avec objets

{% tabs %}
{% tab title="Configurations" %}
```javascript
// Configuration par défaut
const configDefaut = {
    host: "localhost",
    port: 3000,
    ssl: false,
    timeout: 5000,
    retries: 3
};

// Fonction avec configuration personnalisable
function creerServeur(configPersonnalisee = {}) {
    const config = {
        ...configDefaut,
        ...configPersonnalisee
    };
    
    console.log("Configuration serveur:", config);
    return config;
}

// Utilisation
const serveurDev = creerServeur({
    port: 8080,
    ssl: true
});

const serveurProd = creerServeur({
    host: "api.monsite.com",
    port: 443,
    ssl: true,
    timeout: 10000
});
```
{% endtab %}

{% tab title="Transformation de données" %}
```javascript
// Transformer tout en gardant certaines propriétés
function transformerUtilisateur(utilisateur, transformations) {
    return {
        ...utilisateur,
        ...transformations,
        lastUpdated: new Date()
    };
}

const alice = { nom: "Alice", age: 28, ville: "Paris" };

const aliceModifiee = transformerUtilisateur(alice, {
    age: 29,
    email: "alice@newmail.com"
});

// Extraction de propriétés avec rest
function extraireInfo({ motDePasse, ...infoPublique }) {
    return infoPublique; // Toutes les propriétés sauf motDePasse
}

const utilisateurComplet = {
    nom: "Bob",
    email: "bob@example.com",
    motDePasse: "secret123",
    age: 32
};

const infoAffichable = extraireInfo(utilisateurComplet);
console.log(infoAffichable); // { nom: "Bob", email: "bob@example.com", age: 32 }
```
{% endtab %}
{% endtabs %}

---

## 🧪 Ateliers pratiques

### Atelier 1 : Gestionnaire de configuration

{% hint style="info" %}
**Objectif :** Créer un système de configuration flexible avec destructuring et spread
{% endhint %}

```javascript
class ConfigManager {
    constructor(configBase = {}) {
        this.configDefaut = {
            // Configuration serveur
            serveur: {
                host: "localhost",
                port: 3000,
                ssl: false
            },
            // Configuration base de données
            database: {
                type: "sqlite",
                host: "localhost",
                port: 5432,
                nom: "app_db"
            },
            // Configuration application
            app: {
                nom: "MonApp",
                version: "1.0.0",
                debug: false,
                maxConnexions: 100
            }
        };
        
        this.config = this.fusionnerConfig(configBase);
    }
    
    fusionnerConfig(configPersonnalisee) {
        return {
            serveur: {
                ...this.configDefaut.serveur,
                ...configPersonnalisee.serveur
            },
            database: {
                ...this.configDefaut.database,
                ...configPersonnalisee.database
            },
            app: {
                ...this.configDefaut.app,
                ...configPersonnalisee.app
            }
        };
    }
    
    // Mise à jour avec destructuring
    mettreAJour(section, nouvellesValeurs) {
        if (!this.config[section]) {
            throw new Error(`Section ${section} n'existe pas`);
        }
        
        this.config = {
            ...this.config,
            [section]: {
                ...this.config[section],
                ...nouvellesValeurs
            }
        };
        
        return this.config[section];
    }
    
    // Extraction de configuration pour une section
    obtenirConfig(section, ...proprietes) {
        if (!this.config[section]) {
            return null;
        }
        
        if (proprietes.length === 0) {
            return { ...this.config[section] };
        }
        
        // Destructuring dynamique
        const config = this.config[section];
        const result = {};
        
        proprietes.forEach(prop => {
            if (config.hasOwnProperty(prop)) {
                result[prop] = config[prop];
            }
        });
        
        return result;
    }
    
    // Validation avec destructuring
    valider() {
        const erreurs = [];
        
        // Validation serveur
        const { serveur: { host, port, ssl } } = this.config;
        if (!host || typeof host !== 'string') {
            erreurs.push("Host serveur invalide");
        }
        if (!port || port < 1 || port > 65535) {
            erreurs.push("Port serveur invalide");
        }
        
        // Validation database
        const { database: { type, nom } } = this.config;
        if (!["sqlite", "postgresql", "mysql"].includes(type)) {
            erreurs.push("Type de base de données non supporté");
        }
        if (!nom || nom.length < 3) {
            erreurs.push("Nom de base de données trop court");
        }
        
        return {
            valide: erreurs.length === 0,
            erreurs
        };
    }
    
    // Export pour environnement
    exporterPourEnvironnement(env) {
        const { serveur, database, app } = this.config;
        
        const baseConfig = {
            ...serveur,
            ...database,
            ...app
        };
        
        switch (env) {
            case 'development':
                return {
                    ...baseConfig,
                    debug: true,
                    ssl: false
                };
            
            case 'production':
                return {
                    ...baseConfig,
                    debug: false,
                    ssl: true,
                    // Supprimer les infos sensibles
                    ...this.supprimerInfosSensibles(baseConfig)
                };
            
            default:
                return baseConfig;
        }
    }
    
    supprimerInfosSensibles(config) {
        const { password, secret, ...configNettoye } = config;
        return configNettoye;
    }
}

// Tests du gestionnaire de configuration
console.log("⚙️ GESTIONNAIRE DE CONFIGURATION");

const manager = new ConfigManager({
    serveur: {
        port: 8080,
        ssl: true
    },
    database: {
        type: "postgresql",
        nom: "prod_db"
    }
});

console.log("Configuration initiale:", manager.config);

// Mise à jour
manager.mettreAJour("app", {
    nom: "SuperApp",
    version: "2.0.0"
});

// Extraction de config spécifique
const configServeur = manager.obtenirConfig("serveur", "host", "port");
console.log("Config serveur (partielle):", configServeur);

// Validation
const validation = manager.valider();
console.log("Validation:", validation);

// Export pour différents environnements
console.log("Config dev:", manager.exporterPourEnvironnement('development'));
console.log("Config prod:", manager.exporterPourEnvironnement('production'));
```

### Atelier 2 : Système de transformation de données

```javascript
// Utilitaires de transformation avec destructuring/spread
class DataTransformer {
    
    // Normaliser un utilisateur
    static normaliserUtilisateur(donneesBrutes) {
        // Destructuring avec renommage et valeurs par défaut
        const {
            firstName: prenom = "Inconnu",
            lastName: nom = "Inconnu",
            email = null,
            age = 0,
            address: {
                street: rue = "",
                city: ville = "",
                country: pays = "France"
            } = {},
            preferences: {
                theme = "clair",
                language: langue = "fr",
                notifications = true
            } = {},
            ...autresInfos
        } = donneesBrutes;
        
        return {
            nom: `${prenom} ${nom}`,
            email,
            age,
            adresse: {
                rue,
                ville,
                pays
            },
            preferences: {
                theme,
                langue,
                notifications
            },
            metadonnees: {
                ...autresInfos,
                dateNormalisation: new Date()
            }
        };
    }
    
    // Fusionner plusieurs objets utilisateur
    static fusionnerUtilisateurs(...utilisateurs) {
        return utilisateurs.reduce((fusion, utilisateur) => {
            const { nom, email, ...autresProps } = utilisateur;
            
            return {
                ...fusion,
                nom: nom || fusion.nom,
                email: email || fusion.email,
                ...autresProps
            };
        }, {});
    }
    
    // Transformer array d'objets
    static transformerListe(liste, transformations) {
        return liste.map(item => {
            const transforme = { ...item };
            
            // Appliquer chaque transformation
            for (const [champ, transformation] of Object.entries(transformations)) {
                if (typeof transformation === 'function') {
                    transforme[champ] = transformation(item[champ], item);
                } else {
                    transforme[champ] = transformation;
                }
            }
            
            return transforme;
        });
    }
    
    // Grouper par propriété
    static grouperPar(liste, propriete) {
        return liste.reduce((groupes, item) => {
            const cle = typeof propriete === 'function' 
                ? propriete(item) 
                : item[propriete];
            
            return {
                ...groupes,
                [cle]: [...(groupes[cle] || []), item]
            };
        }, {});
    }
    
    // Extraire et transformer propriétés spécifiques
    static extraire(objet, ...proprietes) {
        const extrait = {};
        
        proprietes.forEach(prop => {
            if (typeof prop === 'string') {
                extrait[prop] = objet[prop];
            } else if (typeof prop === 'object') {
                // Format { propriete: "nouveauNom" }
                const [ancienNom, nouveauNom] = Object.entries(prop)[0];
                extrait[nouveauNom] = objet[ancienNom];
            }
        });
        
        return extrait;
    }
}

// Tests des transformations
console.log("\n🔄 SYSTÈME DE TRANSFORMATION");

const donneesUtilisateurBrutes = {
    firstName: "Alice",
    lastName: "Dupont",
    email: "alice@example.com",
    age: 28,
    address: {
        street: "123 rue de la Paix",
        city: "Paris"
    },
    preferences: {
        theme: "sombre"
    },
    createdAt: "2024-01-01",
    isActive: true
};

// Normalisation
const utilisateurNormalise = DataTransformer.normaliserUtilisateur(donneesUtilisateurBrutes);
console.log("Utilisateur normalisé:", utilisateurNormalise);

// Transformation d'une liste
const listeUtilisateurs = [
    { nom: "Alice", age: 28, salaire: 50000 },
    { nom: "Bob", age: 32, salaire: 60000 },
    { nom: "Charlie", age: 25, salaire: 45000 }
];

const utilisateursTransformes = DataTransformer.transformerListe(listeUtilisateurs, {
    age: age => `${age} ans`,
    salaire: (salaire, utilisateur) => `${salaire.toLocaleString()}€`,
    niveau: (_, utilisateur) => utilisateur.age > 30 ? "Senior" : "Junior"
});

console.log("Utilisateurs transformés:", utilisateursTransformes);

// Groupement
const groupesParNiveau = DataTransformer.grouperPar(utilisateursTransformes, 'niveau');
console.log("Groupés par niveau:", groupesParNiveau);

// Extraction
const infosSimplesAlice = DataTransformer.extraire(
    utilisateurNormalise,
    "nom",
    "email",
    { age: "anciennete" },
    "preferences"
);
console.log("Infos extraites:", infosSimplesAlice);
```

---

## 🎯 Quiz interactif

{% tabs %}
{% tab title="Question 1" %}
**Que va afficher ce code ?**

```javascript
const arr = [1, 2, 3, 4, 5];
const [a, , c, ...rest] = arr;
console.log(a, c, rest);
```

{% hint style="info" %}
**Réponse :** `1 3 [4, 5]`

**Explication :**
- `a` prend la première valeur (1)
- La deuxième valeur est ignorée (`,` sans variable)
- `c` prend la troisième valeur (3)
- `...rest` collecte le reste dans un array
{% endhint %}
{% endtab %}

{% tab title="Question 2" %}
**Comment créer une copie d'objet en modifiant une propriété ?**

```javascript
const user = { nom: "Alice", age: 25, ville: "Paris" };
// Créer une copie avec age: 26
```

{% hint style="info" %}
**Réponse :**

```javascript
const userModifie = {
    ...user,
    age: 26
};
```

**Important :** Cela crée une nouvelle copie (shallow copy) sans modifier l'original.
{% endhint %}
{% endtab %}

{% tab title="Question 3" %}
**Quelle est la différence entre ces deux codes ?**

```javascript
// Code A
function func(...args) {
    console.log(args);
}

// Code B
function func(args) {
    console.log(args);
}
```

{% hint style="info" %}
**Réponse :**
- **Code A :** `...args` est le rest operator, `args` sera toujours un array
- **Code B :** `args` est un paramètre normal, peut être n'importe quel type

```javascript
func(1, 2, 3);
// Code A: [1, 2, 3]
// Code B: 1 (seul le premier argument)
```
{% endhint %}
{% endtab %}
{% endtabs %}

---

## 💡 Exercices pratiques

### Exercice 1 : Fonction de fusion d'objets

{% hint style="warning" %}
**Difficulté :** ⭐⭐⭐☆☆
{% endhint %}

**Consigne :** Créez une fonction qui fusionne plusieurs objets avec gestion de conflits.

```javascript
function fusionnerObjets(strategie, ...objets) {
    // Stratégies : 'remplacer', 'conserver', 'combiner'
    // Implémenter la logique de fusion
}

// Tests
const obj1 = { a: 1, b: 2, c: [1, 2] };
const obj2 = { b: 3, d: 4, c: [3, 4] };
const obj3 = { a: 5, e: 6 };

console.log(fusionnerObjets('remplacer', obj1, obj2, obj3));
// { a: 5, b: 3, c: [3, 4], d: 4, e: 6 }

console.log(fusionnerObjets('combiner', obj1, obj2, obj3));
// { a: [1, 5], b: [2, 3], c: [1, 2, 3, 4], d: 4, e: 6 }
```

{% hint style="success" %}
**Solution - Cliquez pour révéler**

```javascript
function fusionnerObjets(strategie, ...objets) {
    return objets.reduce((resultat, objetCourant) => {
        const nouvelleFusion = { ...resultat };
        
        for (const [cle, valeur] of Object.entries(objetCourant)) {
            if (!(cle in nouvelleFusion)) {
                // Nouvelle propriété
                nouvelleFusion[cle] = valeur;
            } else {
                // Conflit : appliquer la stratégie
                switch (strategie) {
                    case 'remplacer':
                        nouvelleFusion[cle] = valeur;
                        break;
                    
                    case 'conserver':
                        // Garder la valeur existante
                        break;
                    
                    case 'combiner':
                        const valeurExistante = nouvelleFusion[cle];
                        if (Array.isArray(valeurExistante) && Array.isArray(valeur)) {
                            nouvelleFusion[cle] = [...valeurExistante, ...valeur];
                        } else {
                            nouvelleFusion[cle] = [
                                ...(Array.isArray(valeurExistante) ? valeurExistante : [valeurExistante]),
                                ...(Array.isArray(valeur) ? valeur : [valeur])
                            ];
                        }
                        break;
                }
            }
        }
        
        return nouvelleFusion;
    }, {});
}
```
{% endhint %}

### Exercice 2 : Analyseur de structures

**Consigne :** Créez un analyseur qui extrait des informations de structures complexes.

```javascript
// Analysez cette structure avec destructuring
const donnees = {
    api: {
        version: "2.0",
        endpoints: [
            { path: "/users", methods: ["GET", "POST"] },
            { path: "/products", methods: ["GET", "PUT", "DELETE"] }
        ]
    },
    users: [
        { id: 1, profile: { name: "Alice", settings: { theme: "dark" } } },
        { id: 2, profile: { name: "Bob", settings: { theme: "light" } } }
    ]
};

// Extraire :
// - Version de l'API
// - Tous les chemins d'endpoints
// - Tous les noms d'utilisateurs
// - Tous les thèmes utilisés
```

---

## 🔗 Liens avec d'autres concepts

### Concepts précédents
- [Arrays](25-arrays-methodes-avancees.md) - Base pour le destructuring d'arrays
- [Objets](75-objets.md) - Foundation pour le destructuring d'objets
- [Fonctions](45-fonctions.md) - Paramètres et arguments

### Concepts suivants
- [Modules](../niveau-avance/95-modules-organisation.md) - Import/export avec destructuring
- [React](../niveau-specialise/react-hooks.md) - useState, useEffect avec destructuring
- [API](../niveau-avance/fetch-api.md) - Destructuring des réponses

---

## 📖 Résumé et bonnes pratiques

{% hint style="success" %}
**Le destructuring et spread/rest permettent :**

- **Code plus concis** : Moins de répétition, plus lisible
- **Immutabilité** : Copies sans modification des originaux
- **Flexibilité** : Gestion dynamique des données
- **Élégance** : Solutions JavaScript modernes

**Bonnes pratiques :**
- Utilisez des valeurs par défaut pour éviter `undefined`
- Attention aux copies superficielles (shallow copy)
- Préférez le destructuring dans les paramètres de fonctions
- Combinez avec les méthodes d'array pour plus de puissance
- Pensez performance sur de gros objets (pas toujours nécessaire de tout copier)
{% endhint %}

---

**Navigation :**
- ← Précédent : [Arrays et Méthodes Avancées](70-arrays-methodes-avancees.md)
- → Suivant : [JSON et Manipulation](53-json-manipulation.md)

---

*💡 **Astuce :** Le destructuring et spread sont essentiels en JavaScript moderne ! Ils sont omniprésents dans React, Vue.js, Node.js et toutes les APIs modernes. Maîtrisez-les pour un code plus élégant et maintenable.*
