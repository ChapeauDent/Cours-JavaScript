# JSON et Manipulation de Données en JavaScript

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** JSON et Manipulation de Données (Sérialisation/Désérialisation)
- **Niveau :** Intermédiaire (concept essentiel)
- **Durée estimée :** 105 minutes
- **Prérequis :** Objets, arrays, types de données, boucles
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est JSON et pourquoi c'est important
- [ ] **Convertir** : Objets JavaScript ↔ chaînes JSON
- [ ] **Valider** : Données JSON et gérer les erreurs
- [ ] **Manipuler** : Structures de données complexes en JSON
- [ ] **Optimiser** : Performance et sécurité des opérations JSON

### Mots-clés à retenir
- **JSON** : JavaScript Object Notation - format d'échange de données
- **Sérialisation** : Conversion objet → chaîne JSON
- **Désérialisation** : Conversion chaîne JSON → objet
- **Parse** : Analyser et convertir du JSON en objet
- **Stringify** : Convertir un objet en chaîne JSON

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
JSON est **LE FORMAT** d'échange de données sur le web moderne. C'est impossible de faire du développement web sans maîtriser JSON.

**Pourquoi c'est crucial :**
- ✅ **APIs** : 99% des APIs modernes utilisent JSON
- ✅ **Stockage** : localStorage, bases de données NoSQL
- ✅ **Configuration** : Fichiers de config, package.json
- ✅ **Communication** : Frontend ↔ Backend
- ✅ **Universalité** : Supporté par tous les langages

**Sans JSON, pas de :**
- Appels API (fetch, axios)
- Sauvegarde de données utilisateur
- Configuration d'applications
- Communication avec bases de données

### Définition et concepts clés

#### **1. Qu'est-ce que JSON ?**

JSON = **J**ava**S**cript **O**bject **N**otation. C'est un format de données textuelles basé sur la syntaxe des objets JavaScript.

```javascript
// Objet JavaScript
let utilisateur = {
    nom: "Alice",
    age: 25,
    actif: true,
    hobbies: ["lecture", "sport"],
    adresse: {
        ville: "Paris",
        codePostal: "75001"
    }
};

// Même données en format JSON (chaîne de caractères)
let utilisateurJSON = `{
    "nom": "Alice",
    "age": 25,
    "actif": true,
    "hobbies": ["lecture", "sport"],
    "adresse": {
        "ville": "Paris",
        "codePostal": "75001"
    }
}`;
```

#### **2. Règles strictes de JSON**

| ✅ Autorisé | ❌ Interdit |
|-------------|-------------|
| `"string"` (guillemets doubles) | `'string'` (guillemets simples) |
| `123`, `123.45` | `undefined`, `NaN`, `Infinity` |
| `true`, `false`, `null` | `Symbol`, `BigInt` |
| `[]`, `{}` | Fonctions, commentaires |
| Propriétés entre guillemets | Propriétés sans guillemets |

```javascript
// ✅ JSON VALIDE
{
    "nom": "Alice",
    "age": 25,
    "actif": true,
    "notes": null,
    "hobbies": ["lecture", "sport"]
}

// ❌ JSON INVALIDE
{
    nom: "Alice",        // Pas de guillemets sur la clé
    'age': 25,          // Guillemets simples
    actif: undefined,   // undefined interdit
    notes: NaN,         // NaN interdit
    func: function() {} // Fonctions interdites
}
```

#### **3. Les deux méthodes essentielles**

**JSON.stringify() - Objet vers JSON**
```javascript
let objet = { nom: "Alice", age: 25 };
let json = JSON.stringify(objet);
console.log(json); // '{"nom":"Alice","age":25}'
console.log(typeof json); // "string"
```

**JSON.parse() - JSON vers Objet**
```javascript
let json = '{"nom":"Alice","age":25}';
let objet = JSON.parse(json);
console.log(objet); // { nom: "Alice", age: 25 }
console.log(typeof objet); // "object"
```

### Syntaxe et utilisation avancée

#### **4. JSON.stringify() avec options**

```javascript
let utilisateur = {
    nom: "Alice",
    motDePasse: "secret123",
    age: 25,
    hobbies: ["lecture", "sport"],
    creeLe: new Date(),
    actif: true
};

// Stringify basique
console.log(JSON.stringify(utilisateur));

// Avec replacer (filtrer/transformer)
let jsonFiltre = JSON.stringify(utilisateur, ["nom", "age", "hobbies"]);
console.log(jsonFiltre); // Ne garde que certaines propriétés

// Avec fonction replacer
let jsonSecurise = JSON.stringify(utilisateur, (key, value) => {
    if (key === "motDePasse") return undefined; // Exclure
    if (value instanceof Date) return value.toISOString(); // Transformer
    return value;
});

// Avec indentation (pretty print)
let jsonFormate = JSON.stringify(utilisateur, null, 2);
console.log(jsonFormate);
/*
{
  "nom": "Alice",
  "motDePasse": "secret123",
  "age": 25,
  "hobbies": [
    "lecture",
    "sport"
  ],
  "creeLe": "2025-07-18T10:30:00.000Z",
  "actif": true
}
*/
```

#### **5. JSON.parse() avec reviver**

```javascript
let jsonAvecDates = '{"nom":"Alice","creeLe":"2025-07-18T10:30:00.000Z","age":25}';

// Parse avec reviver pour restaurer les dates
let objet = JSON.parse(jsonAvecDates, (key, value) => {
    // Détecter les chaînes qui ressemblent à des dates ISO
    if (typeof value === "string" && /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}/.test(value)) {
        return new Date(value); // Convertir en Date
    }
    return value;
});

console.log(objet.creeLe instanceof Date); // true
console.log(objet.creeLe.getFullYear()); // 2025
```

#### **6. Gestion d'erreurs avec try/catch**

```javascript
function parseJSONSecurise(jsonString) {
    try {
        let objet = JSON.parse(jsonString);
        console.log("✅ JSON valide:", objet);
        return { succes: true, donnees: objet };
    } catch (error) {
        console.log("❌ JSON invalide:", error.message);
        return { succes: false, erreur: error.message };
    }
}

// Tests
parseJSONSecurise('{"nom":"Alice"}'); // ✅ Valide
parseJSONSecurise('{"nom":Alice}'); // ❌ Invalide (pas de guillemets)
parseJSONSecurise('{nom:"Alice"}'); // ❌ Invalide (clé sans guillemets)
parseJSONSecurise('{"nom":"Alice",}'); // ❌ Invalide (virgule finale)
```

### Points d'attention critiques

#### **Piège 1 : Perte de données lors de stringify**
```javascript
let objet = {
    nom: "Alice",
    age: 25,
    // Ces valeurs seront perdues !
    fonction: function() { return "hello"; },
    indefini: undefined,
    symbole: Symbol("test"),
    nombreInfini: Infinity,
    nombreNaN: NaN
};

let json = JSON.stringify(objet);
console.log(json); // '{"nom":"Alice","age":25}' - autres propriétés perdues !

let restaure = JSON.parse(json);
console.log(restaure.fonction); // undefined - fonction perdue
```

#### **Piège 2 : Références circulaires**
```javascript
let objet1 = { nom: "Objet1" };
let objet2 = { nom: "Objet2" };

// Créer une référence circulaire
objet1.ref = objet2;
objet2.ref = objet1;

try {
    JSON.stringify(objet1); // ❌ TypeError: Converting circular structure to JSON
} catch (error) {
    console.log("Erreur référence circulaire:", error.message);
}

// Solution : utiliser WeakSet pour tracker les objets visités
function stringifyAvecCirculaires(obj) {
    let vus = new WeakSet();
    return JSON.stringify(obj, (key, value) => {
        if (typeof value === "object" && value !== null) {
            if (vus.has(value)) {
                return "[Circular Reference]";
            }
            vus.add(value);
        }
        return value;
    });
}
```

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Démonstration basique de JSON
console.log("=== DÉMONSTRATION JSON BASIQUE ===");

// 1. Créer un objet simple
let produit = {
    id: 1,
    nom: "Ordinateur Portable",
    prix: 999.99,
    enStock: true,
    categories: ["electronique", "informatique"],
    specifications: {
        processeur: "Intel i7",
        ram: "16GB",
        stockage: "512GB SSD"
    }
};

console.log("Objet original:", produit);
console.log("Type:", typeof produit);

// 2. Convertir en JSON (sérialisation)
let produitJSON = JSON.stringify(produit);
console.log("\nJSON string:", produitJSON);
console.log("Type:", typeof produitJSON);
console.log("Taille:", produitJSON.length, "caractères");

// 3. JSON formaté (lisible)
let produitJSONFormate = JSON.stringify(produit, null, 2);
console.log("\nJSON formaté:");
console.log(produitJSONFormate);

// 4. Reconvertir en objet (désérialisation)
let produitRestaure = JSON.parse(produitJSON);
console.log("\nObjet restauré:", produitRestaure);
console.log("Type:", typeof produitRestaure);

// 5. Vérifications
console.log("\n=== VÉRIFICATIONS ===");
console.log("Même nom:", produit.nom === produitRestaure.nom);
console.log("Même prix:", produit.prix === produitRestaure.prix);
console.log("Même specifications:", JSON.stringify(produit.specifications) === JSON.stringify(produitRestaure.specifications));

// 6. Mais attention aux références !
console.log("Même objet (référence):", produit === produitRestaure); // false
console.log("Même specifications (référence):", produit.specifications === produitRestaure.specifications); // false
```

### Exemple intermédiaire (API simulation)
```javascript
// Simulation d'API et manipulation de données JSON
console.log("=== SIMULATION API AVEC JSON ===");

// Simuler une base de données d'utilisateurs en JSON
let baseDonneesJSON = `{
    "utilisateurs": [
        {
            "id": 1,
            "nom": "Alice Dupont",
            "email": "alice@example.com",
            "age": 28,
            "actif": true,
            "dernierLogin": "2025-07-18T09:30:00.000Z",
            "roles": ["user", "moderator"],
            "preferences": {
                "theme": "dark",
                "notifications": true,
                "langue": "fr"
            }
        },
        {
            "id": 2,
            "nom": "Bob Martin",
            "email": "bob@example.com",
            "age": 35,
            "actif": false,
            "dernierLogin": "2025-07-15T14:20:00.000Z",
            "roles": ["user"],
            "preferences": {
                "theme": "light",
                "notifications": false,
                "langue": "en"
            }
        },
        {
            "id": 3,
            "nom": "Charlie Brown",
            "email": "charlie@example.com",
            "age": 22,
            "actif": true,
            "dernierLogin": "2025-07-18T11:15:00.000Z",
            "roles": ["user", "admin"],
            "preferences": {
                "theme": "dark",
                "notifications": true,
                "langue": "fr"
            }
        }
    ],
    "metadata": {
        "total": 3,
        "version": "1.0",
        "lastUpdate": "2025-07-18T12:00:00.000Z"
    }
}`;

// Classe pour gérer les données utilisateurs
class GestionnaireUtilisateurs {
    constructor(jsonData) {
        try {
            // Parser le JSON et restaurer les dates
            this.donnees = JSON.parse(jsonData, this.reviverDates);
            console.log("✅ Données chargées:", this.donnees.metadata.total, "utilisateurs");
        } catch (error) {
            console.error("❌ Erreur lors du parsing JSON:", error.message);
            this.donnees = { utilisateurs: [], metadata: {} };
        }
    }
    
    // Fonction reviver pour restaurer les dates
    reviverDates(key, value) {
        if (typeof value === "string" && /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}/.test(value)) {
            return new Date(value);
        }
        return value;
    }
    
    // Obtenir tous les utilisateurs actifs
    obtenirUtilisateursActifs() {
        return this.donnees.utilisateurs.filter(user => user.actif);
    }
    
    // Chercher un utilisateur par email
    chercherParEmail(email) {
        return this.donnees.utilisateurs.find(user => 
            user.email.toLowerCase() === email.toLowerCase()
        );
    }
    
    // Ajouter un nouvel utilisateur
    ajouterUtilisateur(nouvelUtilisateur) {
        // Générer un ID unique
        let maxId = Math.max(...this.donnees.utilisateurs.map(u => u.id));
        nouvelUtilisateur.id = maxId + 1;
        nouvelUtilisateur.dernierLogin = new Date();
        nouvelUtilisateur.actif = true;
        
        this.donnees.utilisateurs.push(nouvelUtilisateur);
        this.donnees.metadata.total++;
        this.donnees.metadata.lastUpdate = new Date();
        
        console.log("✅ Utilisateur ajouté:", nouvelUtilisateur.nom);
    }
    
    // Exporter les données en JSON
    exporterJSON(formatage = false) {
        return JSON.stringify(this.donnees, (key, value) => {
            // Garder les dates au format ISO
            if (value instanceof Date) {
                return value.toISOString();
            }
            return value;
        }, formatage ? 2 : null);
    }
    
    // Statistiques des utilisateurs
    obtenirStatistiques() {
        let stats = {
            total: this.donnees.utilisateurs.length,
            actifs: this.donnees.utilisateurs.filter(u => u.actif).length,
            inactifs: this.donnees.utilisateurs.filter(u => !u.actif).length,
            agesMoyen: this.donnees.utilisateurs.reduce((sum, u) => sum + u.age, 0) / this.donnees.utilisateurs.length,
            rolesPopulaires: {},
            themesPopulaires: {}
        };
        
        // Compter les rôles
        this.donnees.utilisateurs.forEach(user => {
            user.roles.forEach(role => {
                stats.rolesPopulaires[role] = (stats.rolesPopulaires[role] || 0) + 1;
            });
            
            let theme = user.preferences.theme;
            stats.themesPopulaires[theme] = (stats.themesPopulaires[theme] || 0) + 1;
        });
        
        return stats;
    }
}

// Tests du gestionnaire
let gestionnaire = new GestionnaireUtilisateurs(baseDonneesJSON);

console.log("\n=== TESTS DU GESTIONNAIRE ===");

// Obtenir utilisateurs actifs
let actifs = gestionnaire.obtenirUtilisateursActifs();
console.log("Utilisateurs actifs:", actifs.map(u => u.nom));

// Chercher par email
let alice = gestionnaire.chercherParEmail("alice@example.com");
console.log("Utilisateur trouvé:", alice ? alice.nom : "Non trouvé");
console.log("Dernier login d'Alice:", alice.dernierLogin.toLocaleDateString());

// Ajouter un nouvel utilisateur
gestionnaire.ajouterUtilisateur({
    nom: "Diana Prince",
    email: "diana@example.com",
    age: 30,
    roles: ["user", "admin"],
    preferences: {
        theme: "dark",
        notifications: true,
        langue: "en"
    }
});

// Statistiques
let stats = gestionnaire.obtenirStatistiques();
console.log("\n=== STATISTIQUES ===");
console.log("Total utilisateurs:", stats.total);
console.log("Utilisateurs actifs:", stats.actifs);
console.log("Âge moyen:", Math.round(stats.agesMoyen));
console.log("Rôles populaires:", stats.rolesPopulaires);
console.log("Thèmes populaires:", stats.themesPopulaires);

// Exporter les données modifiées
let donneesModifiees = gestionnaire.exporterJSON(true);
console.log("\n=== DONNÉES EXPORTÉES (aperçu) ===");
console.log(donneesModifiees.substring(0, 300) + "...");
```

### Exemple avancé (validation et transformation)
```javascript
// Système avancé de validation et transformation JSON
console.log("=== SYSTÈME AVANCÉ DE VALIDATION JSON ===");

// Classe pour valider et transformer des données JSON
class ValidateurJSON {
    constructor() {
        this.schemas = new Map();
        this.transformateurs = new Map();
        this.erreurs = [];
    }
    
    // Définir un schéma de validation
    definirSchema(nom, schema) {
        this.schemas.set(nom, schema);
    }
    
    // Définir un transformateur
    definirTransformateur(nom, transformateur) {
        this.transformateurs.set(nom, transformateur);
    }
    
    // Valider des données selon un schéma
    valider(donnees, nomSchema) {
        this.erreurs = [];
        let schema = this.schemas.get(nomSchema);
        
        if (!schema) {
            this.erreurs.push(`Schéma '${nomSchema}' non trouvé`);
            return false;
        }
        
        return this._validerObjet(donnees, schema, "");
    }
    
    _validerObjet(objet, schema, chemin) {
        let valide = true;
        
        // Vérifier les propriétés requises
        if (schema.required) {
            for (let prop of schema.required) {
                if (!(prop in objet)) {
                    this.erreurs.push(`Propriété requise manquante: ${chemin}.${prop}`);
                    valide = false;
                }
            }
        }
        
        // Vérifier les types et contraintes
        for (let [prop, regles] of Object.entries(schema.properties || {})) {
            if (prop in objet) {
                let valeur = objet[prop];
                let nouveauChemin = chemin ? `${chemin}.${prop}` : prop;
                
                // Vérifier le type
                if (regles.type && typeof valeur !== regles.type) {
                    this.erreurs.push(`Type incorrect pour ${nouveauChemin}: attendu ${regles.type}, reçu ${typeof valeur}`);
                    valide = false;
                    continue;
                }
                
                // Vérifications spécifiques par type
                if (regles.type === "string") {
                    if (regles.minLength && valeur.length < regles.minLength) {
                        this.erreurs.push(`${nouveauChemin} trop court (min: ${regles.minLength})`);
                        valide = false;
                    }
                    if (regles.pattern && !new RegExp(regles.pattern).test(valeur)) {
                        this.erreurs.push(`${nouveauChemin} ne respecte pas le pattern`);
                        valide = false;
                    }
                }
                
                if (regles.type === "number") {
                    if (regles.min !== undefined && valeur < regles.min) {
                        this.erreurs.push(`${nouveauChemin} trop petit (min: ${regles.min})`);
                        valide = false;
                    }
                    if (regles.max !== undefined && valeur > regles.max) {
                        this.erreurs.push(`${nouveauChemin} trop grand (max: ${regles.max})`);
                        valide = false;
                    }
                }
                
                if (regles.type === "array") {
                    if (regles.items) {
                        for (let i = 0; i < valeur.length; i++) {
                            this._validerObjet(valeur[i], { properties: { item: regles.items } }, `${nouveauChemin}[${i}].item`);
                        }
                    }
                }
                
                // Validation récursive pour les objets
                if (regles.type === "object" && regles.properties) {
                    valide = this._validerObjet(valeur, regles, nouveauChemin) && valide;
                }
            }
        }
        
        return valide;
    }
    
    // Transformer des données
    transformer(donnees, nomTransformateur) {
        let transformateur = this.transformateurs.get(nomTransformateur);
        if (!transformateur) {
            throw new Error(`Transformateur '${nomTransformateur}' non trouvé`);
        }
        
        return transformateur(donnees);
    }
    
    // Obtenir les erreurs de validation
    obtenirErreurs() {
        return [...this.erreurs];
    }
}

// Configuration des schémas et transformateurs
let validateur = new ValidateurJSON();

// Schéma pour un utilisateur
validateur.definirSchema("utilisateur", {
    required: ["nom", "email", "age"],
    properties: {
        nom: {
            type: "string",
            minLength: 2
        },
        email: {
            type: "string",
            pattern: "^[^@]+@[^@]+\\.[^@]+$"
        },
        age: {
            type: "number",
            min: 0,
            max: 150
        },
        actif: {
            type: "boolean"
        },
        preferences: {
            type: "object",
            properties: {
                theme: {
                    type: "string"
                },
                notifications: {
                    type: "boolean"
                }
            }
        }
    }
});

// Transformateur pour nettoyer les données utilisateur
validateur.definirTransformateur("nettoyerUtilisateur", (donnees) => {
    return {
        ...donnees,
        nom: donnees.nom?.trim(),
        email: donnees.email?.toLowerCase().trim(),
        age: Number(donnees.age),
        actif: donnees.actif !== false, // Par défaut true
        preferences: {
            theme: donnees.preferences?.theme || "light",
            notifications: donnees.preferences?.notifications !== false
        },
        creeLe: new Date().toISOString(),
        modifieLe: new Date().toISOString()
    };
});

// Tests de validation
console.log("\n=== TESTS DE VALIDATION ===");

// Données valides
let utilisateurValide = {
    nom: "John Doe",
    email: "john@example.com",
    age: 30,
    actif: true,
    preferences: {
        theme: "dark",
        notifications: true
    }
};

console.log("Validation utilisateur valide:");
let estValide = validateur.valider(utilisateurValide, "utilisateur");
console.log("Résultat:", estValide ? "✅ Valide" : "❌ Invalide");
if (!estValide) {
    console.log("Erreurs:", validateur.obtenirErreurs());
}

// Données invalides
let utilisateurInvalide = {
    nom: "A", // Trop court
    email: "pas-un-email", // Format invalide
    age: -5, // Négatif
    preferences: {
        theme: 123 // Mauvais type
    }
    // 'actif' manquant mais pas requis
};

console.log("\nValidation utilisateur invalide:");
estValide = validateur.valider(utilisateurInvalide, "utilisateur");
console.log("Résultat:", estValide ? "✅ Valide" : "❌ Invalide");
if (!estValide) {
    console.log("Erreurs:");
    validateur.obtenirErreurs().forEach(erreur => console.log("  -", erreur));
}

// Test de transformation
console.log("\n=== TESTS DE TRANSFORMATION ===");

let donneesASalesir = {
    nom: "  Jane Doe  ",
    email: "  JANE@EXAMPLE.COM  ",
    age: "25", // String au lieu de number
    preferences: {
        theme: "dark"
        // notifications manquant
    }
};

console.log("Avant transformation:", donneesASalesir);
let donneesNettoyees = validateur.transformer(donneesASalesir, "nettoyerUtilisateur");
console.log("Après transformation:", donneesNettoyees);

// Valider après transformation
estValide = validateur.valider(donneesNettoyees, "utilisateur");
console.log("Validation après transformation:", estValide ? "✅ Valide" : "❌ Invalide");

// Exemple de traitement de fichier JSON
console.log("\n=== TRAITEMENT FICHIER JSON ===");

// Simuler le contenu d'un fichier de configuration
let configJSON = `{
    "application": {
        "nom": "Mon App",
        "version": "1.2.3",
        "debug": true
    },
    "database": {
        "host": "localhost",
        "port": 5432,
        "nom": "ma_base"
    },
    "features": {
        "authentification": true,
        "cache": false,
        "logs": true
    }
}`;

try {
    let config = JSON.parse(configJSON);
    
    // Transformer la configuration
    let configTransformee = {
        ...config,
        application: {
            ...config.application,
            environnement: config.application.debug ? "development" : "production",
            demarrageLe: new Date().toISOString()
        },
        database: {
            ...config.database,
            url: `postgresql://${config.database.host}:${config.database.port}/${config.database.nom}`
        }
    };
    
    console.log("Configuration transformée:");
    console.log(JSON.stringify(configTransformee, null, 2));
    
} catch (error) {
    console.error("Erreur lors du traitement de la configuration:", error.message);
}
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Convertisseur de formats
**Consigne :** Créez une fonction qui convertit des objets en différents formats JSON selon les besoins.

```javascript
function convertirFormat(objet, format) {
    // format peut être: "compact", "readable", "secure"
    // "compact": JSON minifié
    // "readable": JSON indenté 
    // "secure": exclure les propriétés sensibles (password, secret, token)
}

let utilisateur = {
    nom: "Alice",
    password: "secret123",
    age: 25,
    apiToken: "abc123def456"
};

console.log(convertirFormat(utilisateur, "compact"));
console.log(convertirFormat(utilisateur, "readable"));
console.log(convertirFormat(utilisateur, "secure"));
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Validation robuste
**Consigne :** Créez un validateur JSON qui gère les types complexes et les validations personnalisées.

```javascript
function validerProduit(produitJSON) {
    // Valider qu'un produit a:
    // - nom (string, 2-100 caractères)
    // - prix (number, > 0)
    // - categories (array de strings)
    // - enStock (boolean)
    // - specifications (objet avec propriétés optionnelles)
}

let produitValide = '{"nom":"Laptop","prix":999,"categories":["tech"],"enStock":true}';
let produitInvalide = '{"nom":"A","prix":-100,"categories":"tech","enStock":"yes"}';

console.log(validerProduit(produitValide)); // true
console.log(validerProduit(produitInvalide)); // false avec erreurs
```

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Synchronisation de données
**Consigne :** Créez un système qui compare et synchronise des objets JSON.

```javascript
function synchroniser(ancien, nouveau) {
    // Retourner un objet avec:
    // - ajouts: nouvelles propriétés
    // - modifications: propriétés modifiées  
    // - suppressions: propriétés supprimées
    // - conflit: propriétés en conflit
}

let ancien = { nom: "Alice", age: 25, ville: "Paris" };
let nouveau = { nom: "Alice", age: 26, pays: "France" };

console.log(synchroniser(ancien, nouveau));
// { ajouts: ["pays"], modifications: ["age"], suppressions: ["ville"] }
```

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function convertirFormat(objet, format) {
    switch (format) {
        case "compact":
            return JSON.stringify(objet);
            
        case "readable":
            return JSON.stringify(objet, null, 2);
            
        case "secure":
            // Liste des propriétés sensibles à exclure
            let proprieteSensibles = ["password", "motDePasse", "secret", "token", "apiToken", "apiKey"];
            
            return JSON.stringify(objet, (key, value) => {
                // Exclure les propriétés sensibles
                if (proprieteSensibles.some(prop => key.toLowerCase().includes(prop.toLowerCase()))) {
                    return undefined;
                }
                return value;
            }, 2);
            
        default:
            throw new Error(`Format '${format}' non supporté`);
    }
}

// Tests
let utilisateur = {
    nom: "Alice",
    password: "secret123",
    age: 25,
    apiToken: "abc123def456",
    preferences: {
        theme: "dark",
        secretKey: "hidden123"
    }
};

console.log("=== TESTS CONVERTISSEUR ===");

console.log("Format compact:");
console.log(convertirFormat(utilisateur, "compact"));

console.log("\nFormat readable:");
console.log(convertirFormat(utilisateur, "readable"));

console.log("\nFormat secure:");
console.log(convertirFormat(utilisateur, "secure"));

// Version avancée avec options personnalisables
function convertirFormatAvance(objet, options = {}) {
    let {
        format = "readable",
        indentation = 2,
        proprietesExclues = [],
        proprietesSeules = null,
        transformations = {}
    } = options;
    
    return JSON.stringify(objet, (key, value) => {
        // Exclure des propriétés spécifiques
        if (proprietesExclues.includes(key)) {
            return undefined;
        }
        
        // Garder seulement certaines propriétés
        if (proprietesSeules && !proprietesSeules.includes(key) && key !== "") {
            return undefined;
        }
        
        // Appliquer des transformations personnalisées
        if (transformations[key]) {
            return transformations[key](value);
        }
        
        // Transformation automatique des dates
        if (value instanceof Date) {
            return value.toISOString();
        }
        
        return value;
    }, format === "compact" ? null : indentation);
}

// Test avancé
let utilisateurAvecDate = {
    ...utilisateur,
    creeLe: new Date(),
    modifieLe: new Date()
};

console.log("\n=== CONVERTISSEUR AVANCÉ ===");
console.log(convertirFormatAvance(utilisateurAvecDate, {
    format: "readable",
    proprietesExclues: ["password", "apiToken"],
    transformations: {
        age: (age) => `${age} ans`,
        nom: (nom) => nom.toUpperCase()
    }
}));
```

### Solution Exercice 2
```javascript
function validerProduit(produitJSON) {
    let resultat = {
        valide: false,
        erreurs: [],
        donnees: null
    };
    
    try {
        // Parser le JSON
        let produit = JSON.parse(produitJSON);
        resultat.donnees = produit;
        
        // Validation du nom
        if (!produit.nom) {
            resultat.erreurs.push("Le nom est requis");
        } else if (typeof produit.nom !== "string") {
            resultat.erreurs.push("Le nom doit être une chaîne de caractères");
        } else if (produit.nom.length < 2 || produit.nom.length > 100) {
            resultat.erreurs.push("Le nom doit contenir entre 2 et 100 caractères");
        }
        
        // Validation du prix
        if (produit.prix === undefined) {
            resultat.erreurs.push("Le prix est requis");
        } else if (typeof produit.prix !== "number" || isNaN(produit.prix)) {
            resultat.erreurs.push("Le prix doit être un nombre");
        } else if (produit.prix <= 0) {
            resultat.erreurs.push("Le prix doit être positif");
        }
        
        // Validation des catégories
        if (!produit.categories) {
            resultat.erreurs.push("Les catégories sont requises");
        } else if (!Array.isArray(produit.categories)) {
            resultat.erreurs.push("Les catégories doivent être un tableau");
        } else if (produit.categories.length === 0) {
            resultat.erreurs.push("Au moins une catégorie est requise");
        } else {
            // Vérifier que chaque catégorie est une string
            produit.categories.forEach((cat, index) => {
                if (typeof cat !== "string") {
                    resultat.erreurs.push(`La catégorie ${index + 1} doit être une chaîne de caractères`);
                }
            });
        }
        
        // Validation du stock
        if (produit.enStock === undefined) {
            resultat.erreurs.push("Le statut de stock est requis");
        } else if (typeof produit.enStock !== "boolean") {
            resultat.erreurs.push("Le statut de stock doit être un booléen");
        }
        
        // Validation des spécifications (optionnel)
        if (produit.specifications !== undefined) {
            if (typeof produit.specifications !== "object" || Array.isArray(produit.specifications)) {
                resultat.erreurs.push("Les spécifications doivent être un objet");
            }
        }
        
        resultat.valide = resultat.erreurs.length === 0;
        
    } catch (error) {
        resultat.erreurs.push(`JSON invalide: ${error.message}`);
    }
    
    return resultat;
}

// Fonction utilitaire pour valider plusieurs produits
function validerCatalogue(catalogueJSON) {
    let catalogue;
    try {
        catalogue = JSON.parse(catalogueJSON);
    } catch (error) {
        return { valide: false, erreur: `JSON invalide: ${error.message}` };
    }
    
    if (!Array.isArray(catalogue.produits)) {
        return { valide: false, erreur: "Le catalogue doit contenir un tableau 'produits'" };
    }
    
    let resultats = catalogue.produits.map((produit, index) => {
        let validation = validerProduit(JSON.stringify(produit));
        return {
            index,
            produit: produit.nom || `Produit ${index + 1}`,
            ...validation
        };
    });
    
    let produitsValides = resultats.filter(r => r.valide);
    let produitsInvalides = resultats.filter(r => !r.valide);
    
    return {
        valide: produitsInvalides.length === 0,
        total: resultats.length,
        valides: produitsValides.length,
        invalides: produitsInvalides.length,
        details: resultats
    };
}

// Tests
console.log("=== TESTS VALIDATION PRODUIT ===");

let produitValide = '{"nom":"Ordinateur Portable","prix":999.99,"categories":["electronique","informatique"],"enStock":true,"specifications":{"processeur":"Intel i7","ram":"16GB"}}';

let produitInvalide = '{"nom":"A","prix":-100,"categories":"tech","enStock":"yes","specifications":"invalid"}';

console.log("Produit valide:");
let resultValide = validerProduit(produitValide);
console.log("Valide:", resultValide.valide);
if (!resultValide.valide) {
    console.log("Erreurs:", resultValide.erreurs);
}

console.log("\nProduit invalide:");
let resultInvalide = validerProduit(produitInvalide);
console.log("Valide:", resultInvalide.valide);
console.log("Erreurs:", resultInvalide.erreurs);

// Test du catalogue
let catalogueJSON = `{
    "produits": [
        {"nom":"Laptop","prix":999,"categories":["tech"],"enStock":true},
        {"nom":"A","prix":-100,"categories":"tech","enStock":"yes"},
        {"nom":"Smartphone","prix":599,"categories":["tech","mobile"],"enStock":false}
    ]
}`;

console.log("\n=== VALIDATION CATALOGUE ===");
let resultCatalogue = validerCatalogue(catalogueJSON);
console.log(`Catalogue: ${resultCatalogue.valides}/${resultCatalogue.total} produits valides`);
resultCatalogue.details.forEach(detail => {
    console.log(`${detail.produit}: ${detail.valide ? "✅" : "❌"}`);
    if (!detail.valide) {
        detail.erreurs.forEach(err => console.log(`  - ${err}`));
    }
});
```

### Solution Exercice 3
```javascript
function synchroniser(ancien, nouveau) {
    let ajouts = [];
    let modifications = [];
    let suppressions = [];
    let conflits = [];
    
    // Identifier toutes les clés uniques
    let toutesLesCles = new Set([
        ...Object.keys(ancien || {}),
        ...Object.keys(nouveau || {})
    ]);
    
    for (let cle of toutesLesCles) {
        let existeAncien = cle in ancien;
        let existeNouveau = cle in nouveau;
        
        if (!existeAncien && existeNouveau) {
            // Nouvelle propriété ajoutée
            ajouts.push({
                cle,
                valeur: nouveau[cle]
            });
        } else if (existeAncien && !existeNouveau) {
            // Propriété supprimée
            suppressions.push({
                cle,
                ancienneValeur: ancien[cle]
            });
        } else if (existeAncien && existeNouveau) {
            // Propriété existante, vérifier si modifiée
            if (!sontEgales(ancien[cle], nouveau[cle])) {
                modifications.push({
                    cle,
                    ancienneValeur: ancien[cle],
                    nouvelleValeur: nouveau[cle]
                });
            }
        }
    }
    
    return {
        ajouts,
        modifications,
        suppressions,
        conflits, // Pour usage futur
        resume: {
            totalChangements: ajouts.length + modifications.length + suppressions.length,
            ajouts: ajouts.length,
            modifications: modifications.length,
            suppressions: suppressions.length
        }
    };
}

// Fonction utilitaire pour comparer des valeurs (deep equality)
function sontEgales(valeur1, valeur2) {
    // Comparaison stricte pour les primitifs
    if (valeur1 === valeur2) return true;
    
    // null et undefined
    if (valeur1 == null || valeur2 == null) return valeur1 === valeur2;
    
    // Types différents
    if (typeof valeur1 !== typeof valeur2) return false;
    
    // Dates
    if (valeur1 instanceof Date && valeur2 instanceof Date) {
        return valeur1.getTime() === valeur2.getTime();
    }
    
    // Arrays
    if (Array.isArray(valeur1) && Array.isArray(valeur2)) {
        if (valeur1.length !== valeur2.length) return false;
        return valeur1.every((item, index) => sontEgales(item, valeur2[index]));
    }
    
    // Objets
    if (typeof valeur1 === "object" && typeof valeur2 === "object") {
        let cles1 = Object.keys(valeur1);
        let cles2 = Object.keys(valeur2);
        
        if (cles1.length !== cles2.length) return false;
        
        return cles1.every(cle => 
            cles2.includes(cle) && sontEgales(valeur1[cle], valeur2[cle])
        );
    }
    
    return false;
}

// Fonction avancée pour synchronisation avec conflits
function synchroniserAvecConflits(ancien, nouveau, source = "local") {
    let result = synchroniser(ancien, nouveau);
    
    // Simuler des conflits (par exemple, modifications concurrentes)
    if (source === "remote") {
        result.modifications.forEach(mod => {
            // Vérifier si c'est un conflit potentiel
            if (typeof mod.ancienneValeur === "object" && typeof mod.nouvelleValeur === "object") {
                result.conflits.push({
                    cle: mod.cle,
                    raisonConflit: "Modification concurrente d'objet complexe",
                    valeurLocale: mod.ancienneValeur,
                    valeurDistante: mod.nouvelleValeur,
                    suggestions: [
                        "Fusionner les objets",
                        "Garder la version locale",
                        "Garder la version distante"
                    ]
                });
            }
        });
    }
    
    return result;
}

// Fonction pour appliquer les changements
function appliquerChangements(objet, changements) {
    let resultat = { ...objet };
    
    // Appliquer les suppressions
    changements.suppressions.forEach(suppression => {
        delete resultat[suppression.cle];
    });
    
    // Appliquer les modifications
    changements.modifications.forEach(modification => {
        resultat[modification.cle] = modification.nouvelleValeur;
    });
    
    // Appliquer les ajouts
    changements.ajouts.forEach(ajout => {
        resultat[ajout.cle] = ajout.valeur;
    });
    
    return resultat;
}

// Tests
console.log("=== TESTS SYNCHRONISATION ===");

let ancien = {
    nom: "Alice",
    age: 25,
    ville: "Paris",
    hobbies: ["lecture", "sport"],
    preferences: {
        theme: "dark",
        notifications: true
    }
};

let nouveau = {
    nom: "Alice",
    age: 26, // Modifié
    pays: "France", // Ajouté
    hobbies: ["lecture", "sport", "cuisine"], // Modifié
    preferences: {
        theme: "light", // Modifié
        notifications: true,
        langue: "fr" // Ajouté dans sous-objet
    }
    // ville supprimée
};

console.log("Objet ancien:", ancien);
console.log("Objet nouveau:", nouveau);

let changements = synchroniser(ancien, nouveau);
console.log("\n=== CHANGEMENTS DÉTECTÉS ===");
console.log("Résumé:", changements.resume);

console.log("\nAjouts:");
changements.ajouts.forEach(ajout => {
    console.log(`  + ${ajout.cle}:`, ajout.valeur);
});

console.log("\nModifications:");
changements.modifications.forEach(mod => {
    console.log(`  ~ ${mod.cle}:`);
    console.log(`    Avant:`, mod.ancienneValeur);
    console.log(`    Après:`, mod.nouvelleValeur);
});

console.log("\nSuppressions:");
changements.suppressions.forEach(supp => {
    console.log(`  - ${supp.cle}:`, supp.ancienneValeur);
});

// Test d'application des changements
let objetFinal = appliquerChangements(ancien, changements);
console.log("\n=== OBJET APRÈS APPLICATION ===");
console.log("Objet final:", objetFinal);
console.log("Égal au nouveau:", sontEgales(objetFinal, nouveau));

// Test avec conflits
console.log("\n=== TEST AVEC CONFLITS ===");
let changementsAvecConflits = synchroniserAvecConflits(ancien, nouveau, "remote");
if (changementsAvecConflits.conflits.length > 0) {
    console.log("Conflits détectés:");
    changementsAvecConflits.conflits.forEach(conflit => {
        console.log(`  ⚠️ ${conflit.cle}: ${conflit.raisonConflit}`);
        console.log(`    Suggestions:`, conflit.suggestions);
    });
}
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Objets** : Structure de données à sérialiser
- **Arrays** : Collections dans JSON
- **Types** : Limitations des types JSON
- **Boucles** : Parcours des structures JSON

### Prochaines étapes importantes
- **APIs REST** : Communication avec serveurs
- **Fetch/AJAX** : Récupération de données JSON
- **LocalStorage** : Stockage local avec JSON
- **Modules** : Import/export de configurations JSON

### Concepts complémentaires
- **Schema Validation** : JSON Schema, Joi, Yup
- **JSONP** : Contournement CORS (legacy)
- **MessagePack** : Alternative binaire à JSON
- **YAML** : Format plus lisible pour configuration

---

## **PROJET MINI**

### Défi pratique : Gestionnaire de contacts avec JSON
**Objectif :** Créer un gestionnaire de contacts qui utilise JSON pour toutes les opérations.

**Fonctionnalités requises :**
- [ ] Import/export de contacts en JSON
- [ ] Validation des données de contact
- [ ] Recherche et filtrage
- [ ] Synchronisation avec stockage local
- [ ] Gestion des erreurs et données corrompues
- [ ] Backup et restauration

**Structure suggérée :**
```
📁 gestionnaire-contacts/
├── 📄 index.html
├── 📄 contacts.js (gestion des contacts)
├── 📄 validation.js (schémas de validation)
├── 📄 stockage.js (localStorage avec JSON)
└── 📄 import-export.js (gestion fichiers)
```

**Temps estimé :** 120 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - JSON](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [JSON.org](https://www.json.org/json-fr.html) - Spécification complète
- [JSON Schema](https://json-schema.org/) - Validation avancée

### Articles approfondis
- "Working with JSON Data" - JavaScript.info
- "JSON.parse vs eval: Security and Performance" - Guide sécurité
- "Best Practices for JSON APIs" - Design d'APIs

### Outils pratiques
- [JSONLint](https://jsonlint.com/) - Validation JSON en ligne
- [JSON Formatter](https://jsonformatter.org/) - Formatage et validation
- [JSON Schema Validator](https://www.jsonschemavalidator.net/) - Test de schémas

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Quelle est la différence entre JSON.parse() et eval() ?**
   - Réponse : JSON.parse() est sécurisé et valide uniquement JSON, eval() exécute du code JavaScript

2. **Pourquoi les fonctions ne sont-elles pas supportées en JSON ?**
   - Réponse : JSON est un format d'échange de données, pas de code exécutable

3. **Comment gérer les dates en JSON ?**
   - Réponse : Les convertir en chaînes ISO avec toISOString() et les reconvertir avec new Date()

### Checklist de maîtrise
- [ ] Je comprends les règles strictes de JSON
- [ ] Je peux convertir objets ↔ JSON avec options
- [ ] Je sais gérer les erreurs de parsing
- [ ] Je peux valider des structures JSON complexes
- [ ] Je maîtrise les transformations avec replacer/reviver
- [ ] Je connais les pièges (circular references, types perdus)

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 : JSON malformé
**Code problématique :**
```javascript
let json = `{
    nom: "Alice",        // ❌ Clé sans guillemets
    'age': 25,          // ❌ Guillemets simples
    actif: undefined    // ❌ undefined interdit
}`;
JSON.parse(json); // SyntaxError
```

**Solution :**
```javascript
let json = `{
    "nom": "Alice",
    "age": 25,
    "actif": null
}`;
```

### Erreur fréquente 2 : Références circulaires
**Code problématique :**
```javascript
let obj = { nom: "test" };
obj.self = obj;
JSON.stringify(obj); // TypeError: Converting circular structure
```

**Solution :**
```javascript
JSON.stringify(obj, (key, value) => {
    if (key === "self") return "[Circular]";
    return value;
});
```

### Erreur fréquente 3 : Perte de types
**Code problématique :**
```javascript
let obj = {
    date: new Date(),
    func: () => "hello"
};
let restored = JSON.parse(JSON.stringify(obj));
console.log(restored.date instanceof Date); // false
console.log(typeof restored.func); // undefined
```

**Solution :**
```javascript
// Avec reviver
let restored = JSON.parse(JSON.stringify(obj), (key, value) => {
    if (key === "date") return new Date(value);
    return value;
});
```

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- JSON est le format standard d'échange de données web
- Les règles strictes de JSON garantissent l'interopérabilité
- Les transformations permettent de gérer les limitations

### Questions à approfondir
- Performance de JSON.parse vs alternatives
- Sécurité : validation et sanitization
- JSON Schema pour validation robuste

### Révision programmée
- [ ] Révision dans 1 jour (refaire les exercices complexes)
- [ ] Révision dans 1 semaine (créer un projet avec APIs)
- [ ] Révision dans 1 mois (lien avec stockage et bases de données)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #json #api #serialization #data #parse #stringify #validation

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 8/126*
