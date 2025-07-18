# 📄 JSON et Manipulation de Données

{% hint style="info" %}
**Niveau :** 🟡 Intermédiaire | **Durée :** 35 min | **Prérequis :** Objets, Arrays, Types de données
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** le format JSON et son importance
- [ ] **Convertir** objets JavaScript ↔ chaînes JSON
- [ ] **Valider** et gérer les erreurs JSON
- [ ] **Manipuler** des structures de données complexes
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**JSON** : JavaScript Object Notation - format d'échange de données  
**Sérialisation** : Conversion objet → chaîne JSON  
**Désérialisation** : Conversion chaîne JSON → objet  
**Parse** : Analyser et convertir du JSON en objet  
**Stringify** : Convertir un objet en chaîne JSON  
{% endtab %}

{% tab title="Importance" %}
🌐 **APIs** : 99% des APIs modernes utilisent JSON  
💾 **Stockage** : localStorage, bases de données NoSQL  
⚙️ **Configuration** : Fichiers de config, package.json  
🔄 **Communication** : Frontend ↔ Backend  
🌍 **Universalité** : Supporté par tous les langages  
{% endtab %}
{% endtabs %}

## 📖 Qu'est-ce que JSON ?

{% hint style="info" %}
**JSON = JavaScript Object Notation**

Format de données textuel basé sur la syntaxe des objets JavaScript, mais **indépendant du langage**.
{% endhint %}

### 🔄 JavaScript vs JSON

{% tabs %}
{% tab title="Objet JavaScript" %}
```javascript
// Objet JavaScript (en mémoire)
const utilisateur = {
  nom: "Alice",
  age: 25,
  actif: true,
  hobbies: ["lecture", "sport"],
  adresse: {
    ville: "Paris",
    codePostal: "75001"
  },
  saluer() {
    return `Bonjour, je suis ${this.nom}`;
  }
};

console.log(utilisateur.saluer()); // "Bonjour, je suis Alice"
```
{% endtab %}

{% tab title="Format JSON" %}
```json
{
  "nom": "Alice",
  "age": 25,
  "actif": true,
  "hobbies": ["lecture", "sport"],
  "adresse": {
    "ville": "Paris",
    "codePostal": "75001"
  }
}
```

**Différences importantes :**
- ✅ Guillemets doubles obligatoires pour les clés
- ❌ Pas de méthodes/fonctions
- ❌ Pas de commentaires
- ❌ Pas de `undefined`, `Symbol`, `BigInt`
{% endtab %}
{% endtabs %}

### 📋 Règles strictes de JSON

{% tabs %}
{% tab title="✅ Autorisé" %}
```json
{
  "string": "Texte avec guillemets doubles",
  "number": 123,
  "decimal": 123.45,
  "boolean": true,
  "null": null,
  "array": ["item1", "item2"],
  "object": {
    "nested": "valeur"
  }
}
```
{% endtab %}

{% tab title="❌ Interdit" %}
```javascript
{
  'singleQuotes': 'Guillemets simples',  // ❌
  unquotedKey: 'Clé sans guillemets',    // ❌
  undefined: undefined,                   // ❌
  infinity: Infinity,                     // ❌
  nan: NaN,                              // ❌
  func: function() { return "test"; },   // ❌
  symbol: Symbol('test'),                // ❌
  // Commentaire                          // ❌
}
```
{% endtab %}
{% endtabs %}

## 🔄 JSON.parse() et JSON.stringify()

### 📥 JSON.parse() - Désérialisation

{% tabs %}
{% tab title="Usage de base" %}
```javascript
// Chaîne JSON → Objet JavaScript
const jsonString = '{"nom": "Bob", "age": 30, "actif": true}';

try {
  const objet = JSON.parse(jsonString);
  console.log(objet.nom); // "Bob"
  console.log(typeof objet); // "object"
  console.log(objet.age + 5); // 35
} catch (error) {
  console.error("JSON invalide:", error.message);
}
```
{% endtab %}

{% tab title="Avec reviver function" %}
```javascript
const jsonAvecDate = '{"nom": "Alice", "naissance": "1998-05-15T10:30:00Z"}';

// Reviver function pour transformer les dates
const objet = JSON.parse(jsonAvecDate, (key, value) => {
  // Transformer les chaînes qui ressemblent à des dates
  if (typeof value === 'string' && /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}/.test(value)) {
    return new Date(value);
  }
  return value;
});

console.log(objet.naissance instanceof Date); // true
console.log(objet.naissance.getFullYear()); // 1998
```
{% endtab %}
{% endtabs %}

### 📤 JSON.stringify() - Sérialisation

{% tabs %}
{% tab title="Usage de base" %}
```javascript
// Objet JavaScript → Chaîne JSON
const utilisateur = {
  nom: "Charlie",
  age: 28,
  hobbies: ["musique", "voyage"],
  adresse: {
    ville: "Lyon",
    pays: "France"
  }
};

const jsonString = JSON.stringify(utilisateur);
console.log(jsonString);
// '{"nom":"Charlie","age":28,"hobbies":["musique","voyage"],"adresse":{"ville":"Lyon","pays":"France"}}'

// Avec indentation pour la lisibilité
const jsonFormaté = JSON.stringify(utilisateur, null, 2);
console.log(jsonFormaté);
```
{% endtab %}

{% tab title="Avec replacer function" %}
```javascript
const utilisateur = {
  nom: "David",
  motDePasse: "secret123",
  email: "david@example.com",
  age: 35,
  dernièreConnexion: new Date()
};

// Replacer function pour filtrer et transformer
const jsonSécurisé = JSON.stringify(utilisateur, (key, value) => {
  // Exclure les données sensibles
  if (key === 'motDePasse') return undefined;
  
  // Formater les dates
  if (value instanceof Date) {
    return value.toISOString();
  }
  
  return value;
});

console.log(jsonSécurisé);
// {"nom":"David","email":"david@example.com","age":35,"dernièreConnexion":"2025-07-18T..."}
```
{% endtab %}

{% tab title="Avec tableau de propriétés" %}
```javascript
const utilisateur = {
  nom: "Eve",
  motDePasse: "secret",
  email: "eve@example.com",
  age: 29,
  téléphone: "+33123456789"
};

// Sérialiser seulement certaines propriétés
const jsonPublic = JSON.stringify(utilisateur, ['nom', 'email', 'age']);
console.log(jsonPublic);
// {"nom":"Eve","email":"eve@example.com","age":29}
```
{% endtab %}
{% endtabs %}

## 🛡️ Gestion d'erreurs et validation

### ⚠️ Erreurs courantes

{% tabs %}
{% tab title="JSON malformé" %}
```javascript
// Exemples de JSON invalide et leur gestion
const exemples = [
  '{"nom": "Alice"',           // Manque }
  "{'nom': 'Alice'}",          // Guillemets simples
  '{"nom": undefined}',        // undefined
  '{"nom": "Alice",}',         // Virgule en trop
];

exemples.forEach((json, index) => {
  try {
    const résultat = JSON.parse(json);
    console.log(`Exemple ${index + 1}: OK`, résultat);
  } catch (error) {
    console.error(`Exemple ${index + 1}: ${error.message}`);
  }
});
```
{% endtab %}

{% tab title="Validation robuste" %}
```javascript
function parseJSONSécurisé(jsonString, defaultValue = null) {
  try {
    // Vérifier que c'est bien une chaîne
    if (typeof jsonString !== 'string') {
      throw new Error('Input must be a string');
    }
    
    // Nettoyer les espaces
    const cleaned = jsonString.trim();
    
    // Vérifier que ça commence et finit correctement
    if (!cleaned.startsWith('{') && !cleaned.startsWith('[')) {
      throw new Error('Invalid JSON format');
    }
    
    const parsed = JSON.parse(cleaned);
    
    // Validation supplémentaire si nécessaire
    if (parsed === null || (typeof parsed !== 'object')) {
      throw new Error('Parsed value is not an object or array');
    }
    
    return parsed;
    
  } catch (error) {
    console.warn('JSON parsing failed:', error.message);
    return defaultValue;
  }
}

// Tests
console.log(parseJSONSécurisé('{"nom": "Alice"}')); // {nom: "Alice"}
console.log(parseJSONSécurisé('invalid json'));     // null
console.log(parseJSONSécurisé('null'));             // null
console.log(parseJSONSécurisé('123'));              // null
```
{% endtab %}
{% endtabs %}

## 🎮 Manipulation de données complexes

### 🌳 Structures imbriquées

{% tabs %}
{% tab title="Navigation dans les données" %}
```javascript
const apiResponse = {
  status: "success",
  data: {
    users: [
      {
        id: 1,
        profile: {
          name: "Alice",
          settings: {
            theme: "dark",
            notifications: {
              email: true,
              push: false
            }
          }
        }
      },
      {
        id: 2,
        profile: {
          name: "Bob",
          settings: {
            theme: "light",
            notifications: {
              email: false,
              push: true
            }
          }
        }
      }
    ]
  }
};

// Navigation sécurisée avec optional chaining (ES2020)
function obtenirNotificationEmail(userId) {
  const user = apiResponse.data?.users?.find(u => u.id === userId);
  return user?.profile?.settings?.notifications?.email ?? false;
}

console.log(obtenirNotificationEmail(1)); // true
console.log(obtenirNotificationEmail(3)); // false (utilisateur introuvable)
```
{% endtab %}

{% tab title="Transformation de données" %}
```javascript
const commandes = {
  "commandes": [
    {
      "id": "CMD001",
      "client": {"nom": "Alice", "email": "alice@example.com"},
      "items": [
        {"produit": "Livre", "prix": 15.99, "quantité": 2},
        {"produit": "Stylo", "prix": 2.50, "quantité": 5}
      ],
      "date": "2025-07-18"
    },
    {
      "id": "CMD002", 
      "client": {"nom": "Bob", "email": "bob@example.com"},
      "items": [
        {"produit": "Cahier", "prix": 3.99, "quantité": 3}
      ],
      "date": "2025-07-17"
    }
  ]
};

// Transformer en rapport de ventes
function générerRapportVentes(données) {
  return données.commandes.map(commande => {
    const total = commande.items.reduce((sum, item) => {
      return sum + (item.prix * item.quantité);
    }, 0);
    
    return {
      commandeId: commande.id,
      client: commande.client.nom,
      email: commande.client.email,
      nombreItems: commande.items.length,
      total: Math.round(total * 100) / 100, // Arrondir à 2 décimales
      date: commande.date
    };
  });
}

const rapport = générerRapportVentes(commandes);
console.log(JSON.stringify(rapport, null, 2));
```
{% endtab %}
{% endtabs %}

### 🔄 Fusion et filtrage

{% tabs %}
{% tab title="Fusion de sources JSON" %}
```javascript
const source1 = {
  "utilisateurs": [
    {"id": 1, "nom": "Alice", "département": "IT"},
    {"id": 2, "nom": "Bob", "département": "RH"}
  ]
};

const source2 = {
  "utilisateurs": [
    {"id": 1, "email": "alice@company.com", "téléphone": "+33123"},
    {"id": 3, "nom": "Charlie", "département": "Finance", "email": "charlie@company.com"}
  ]
};

function fusionnerUtilisateurs(source1, source2) {
  const utilisateursMap = new Map();
  
  // Ajouter source1
  source1.utilisateurs.forEach(user => {
    utilisateursMap.set(user.id, { ...user });
  });
  
  // Fusionner avec source2
  source2.utilisateurs.forEach(user => {
    if (utilisateursMap.has(user.id)) {
      // Fusionner les propriétés
      const existing = utilisateursMap.get(user.id);
      utilisateursMap.set(user.id, { ...existing, ...user });
    } else {
      // Nouveau utilisateur
      utilisateursMap.set(user.id, { ...user });
    }
  });
  
  return {
    utilisateurs: Array.from(utilisateursMap.values())
  };
}

const résultat = fusionnerUtilisateurs(source1, source2);
console.log(JSON.stringify(résultat, null, 2));
```
{% endtab %}

{% tab title="Filtrage et recherche" %}
```javascript
const bibliothèque = {
  "livres": [
    {"id": 1, "titre": "JavaScript Guide", "auteur": "John Doe", "année": 2023, "genre": "Technique", "disponible": true},
    {"id": 2, "titre": "Histoire de France", "auteur": "Marie Dupont", "année": 2020, "genre": "Histoire", "disponible": false},
    {"id": 3, "titre": "Cuisine italienne", "auteur": "Giuseppe Romano", "année": 2022, "genre": "Cuisine", "disponible": true},
    {"id": 4, "titre": "Python avancé", "auteur": "Alice Smith", "année": 2023, "genre": "Technique", "disponible": true}
  ]
};

// Système de recherche flexible
function rechercherLivres(données, critères) {
  return données.livres.filter(livre => {
    // Recherche par texte (titre, auteur)
    if (critères.texte) {
      const texte = critères.texte.toLowerCase();
      const correspond = 
        livre.titre.toLowerCase().includes(texte) ||
        livre.auteur.toLowerCase().includes(texte);
      if (!correspond) return false;
    }
    
    // Filtrage par genre
    if (critères.genre && livre.genre !== critères.genre) {
      return false;
    }
    
    // Filtrage par disponibilité
    if (critères.disponible !== undefined && livre.disponible !== critères.disponible) {
      return false;
    }
    
    // Filtrage par année
    if (critères.annéeMin && livre.année < critères.annéeMin) {
      return false;
    }
    
    return true;
  });
}

// Tests de recherche
console.log("Livres techniques disponibles:");
console.log(rechercherLivres(bibliothèque, {
  genre: "Technique",
  disponible: true
}));

console.log("Livres récents (2022+):");
console.log(rechercherLivres(bibliothèque, {
  annéeMin: 2022
}));

console.log("Recherche 'python':");
console.log(rechercherLivres(bibliothèque, {
  texte: "python"
}));
```
{% endtab %}
{% endtabs %}

## 🚀 Cas d'usage pratiques

### 💾 Stockage local

{% tabs %}
{% tab title="localStorage avec JSON" %}
```javascript
// Système de préférences utilisateur
class PreférencesUtilisateur {
  constructor() {
    this.clé = 'préférences_utilisateur';
    this.défauts = {
      thème: 'light',
      langue: 'fr',
      notifications: true,
      autoSave: true
    };
  }
  
  charger() {
    try {
      const stored = localStorage.getItem(this.clé);
      if (stored) {
        const parsed = JSON.parse(stored);
        return { ...this.défauts, ...parsed };
      }
    } catch (error) {
      console.warn('Erreur lors du chargement des préférences:', error);
    }
    return this.défauts;
  }
  
  sauvegarder(préférences) {
    try {
      const àSauvegarder = { ...this.charger(), ...préférences };
      localStorage.setItem(this.clé, JSON.stringify(àSauvegarder));
      return true;
    } catch (error) {
      console.error('Erreur lors de la sauvegarde:', error);
      return false;
    }
  }
  
  obtenirPréférence(clé) {
    return this.charger()[clé];
  }
  
  définirPréférence(clé, valeur) {
    const préférences = this.charger();
    préférences[clé] = valeur;
    return this.sauvegarder(préférences);
  }
  
  réinitialiser() {
    localStorage.removeItem(this.clé);
    return this.défauts;
  }
}

// Utilisation
const préfs = new PreférencesUtilisateur();

console.log(préfs.charger()); // Préférences actuelles
préfs.définirPréférence('thème', 'dark');
préfs.définirPréférence('langue', 'en');
console.log(préfs.obtenirPréférence('thème')); // 'dark'
```
{% endtab %}

{% tab title="Cache avec expiration" %}
```javascript
class CacheJSON {
  constructor(nomCache = 'cache_json') {
    this.nom = nomCache;
  }
  
  set(clé, données, duréeVie = 3600000) { // 1h par défaut
    const item = {
      données,
      expiration: Date.now() + duréeVie
    };
    
    try {
      localStorage.setItem(`${this.nom}_${clé}`, JSON.stringify(item));
      return true;
    } catch (error) {
      console.error('Erreur cache set:', error);
      return false;
    }
  }
  
  get(clé) {
    try {
      const stored = localStorage.getItem(`${this.nom}_${clé}`);
      if (!stored) return null;
      
      const item = JSON.parse(stored);
      
      // Vérifier l'expiration
      if (Date.now() > item.expiration) {
        this.delete(clé);
        return null;
      }
      
      return item.données;
    } catch (error) {
      console.error('Erreur cache get:', error);
      return null;
    }
  }
  
  delete(clé) {
    localStorage.removeItem(`${this.nom}_${clé}`);
  }
  
  clear() {
    const keys = Object.keys(localStorage);
    keys.forEach(key => {
      if (key.startsWith(`${this.nom}_`)) {
        localStorage.removeItem(key);
      }
    });
  }
  
  nettoyer() {
    const keys = Object.keys(localStorage);
    const now = Date.now();
    
    keys.forEach(key => {
      if (key.startsWith(`${this.nom}_`)) {
        try {
          const item = JSON.parse(localStorage.getItem(key));
          if (now > item.expiration) {
            localStorage.removeItem(key);
          }
        } catch (error) {
          // Supprimer les items corrompus
          localStorage.removeItem(key);
        }
      }
    });
  }
}

// Utilisation
const cache = new CacheJSON('api_cache');

// Stocker des données d'API
cache.set('user_profile', {
  nom: 'Alice',
  email: 'alice@example.com'
}, 300000); // 5 minutes

// Récupérer
const profile = cache.get('user_profile');
console.log(profile); // {nom: 'Alice', email: 'alice@example.com'} ou null si expiré
```
{% endtab %}
{% endtabs %}

### 🌐 Communication API

{% tabs %}
{% tab title="Wrapper API avec JSON" %}
```javascript
class ApiClient {
  constructor(baseUrl, options = {}) {
    this.baseUrl = baseUrl.replace(/\/$/, ''); // Supprimer / final
    this.défauts = {
      headers: {
        'Content-Type': 'application/json',
        ...options.headers
      },
      timeout: options.timeout || 10000
    };
  }
  
  async requête(endpoint, options = {}) {
    const url = `${this.baseUrl}${endpoint}`;
    const config = {
      ...this.défauts,
      ...options,
      headers: {
        ...this.défauts.headers,
        ...options.headers
      }
    };
    
    // Convertir body en JSON si c'est un objet
    if (config.body && typeof config.body === 'object') {
      config.body = JSON.stringify(config.body);
    }
    
    try {
      const response = await fetch(url, config);
      
      // Vérifier le content-type
      const contentType = response.headers.get('content-type');
      if (!contentType?.includes('application/json')) {
        throw new Error(`Réponse non-JSON: ${contentType}`);
      }
      
      const data = await response.json();
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${data.message || 'Erreur inconnue'}`);
      }
      
      return data;
    } catch (error) {
      if (error.name === 'SyntaxError') {
        throw new Error('Réponse JSON malformée');
      }
      throw error;
    }
  }
  
  async get(endpoint) {
    return this.requête(endpoint, { method: 'GET' });
  }
  
  async post(endpoint, data) {
    return this.requête(endpoint, {
      method: 'POST',
      body: data
    });
  }
  
  async put(endpoint, data) {
    return this.requête(endpoint, {
      method: 'PUT',
      body: data
    });
  }
  
  async delete(endpoint) {
    return this.requête(endpoint, { method: 'DELETE' });
  }
}

// Utilisation
const api = new ApiClient('https://jsonplaceholder.typicode.com');

// GET
api.get('/users/1')
  .then(user => console.log('Utilisateur:', user))
  .catch(error => console.error('Erreur:', error.message));

// POST
api.post('/users', {
    nom: 'Nouvel Utilisateur',
    email: 'nouveau@example.com'
  })
  .then(result => console.log('Créé:', result))
  .catch(error => console.error('Erreur:', error.message));
```
{% endtab %}
{% endtabs %}

## 🧪 Exercices pratiques

### 🎯 Exercice 1 : Analyseur de logs JSON

{% hint style="info" %}
**Créez un analyseur de logs qui traite des entrées JSON :**
{% endhint %}

```javascript
// Données de test
const logs = [
  '{"timestamp": "2025-07-18T10:30:00Z", "level": "INFO", "message": "Application started", "user": "system"}',
  '{"timestamp": "2025-07-18T10:31:00Z", "level": "ERROR", "message": "Database connection failed", "user": "admin", "details": {"error": "ECONNREFUSED", "host": "localhost"}}',
  '{"timestamp": "2025-07-18T10:32:00Z", "level": "WARN", "message": "High memory usage", "user": "monitor"}',
  'invalid json line',
  '{"timestamp": "2025-07-18T10:33:00Z", "level": "INFO", "message": "User login", "user": "alice", "metadata": {"ip": "192.168.1.100"}}'
];

// Créez une fonction analyzerLogs(logs) qui retourne :
// - Nombre total de logs valides
// - Nombre de logs par niveau (INFO, WARN, ERROR)
// - Liste des erreurs avec détails
// - Utilisateurs les plus actifs
```

<details>
<summary>💡 Solution</summary>

```javascript
function analyzerLogs(logs) {
  const stats = {
    totalValides: 0,
    niveaux: { INFO: 0, WARN: 0, ERROR: 0 },
    erreurs: [],
    utilisateurs: {},
    logsInvalides: 0
  };
  
  logs.forEach((logLine, index) => {
    try {
      const log = JSON.parse(logLine);
      
      // Validation basique
      if (!log.timestamp || !log.level || !log.message) {
        throw new Error('Champs obligatoires manquants');
      }
      
      stats.totalValides++;
      
      // Compter par niveau
      if (stats.niveaux.hasOwnProperty(log.level)) {
        stats.niveaux[log.level]++;
      }
      
      // Collecter les erreurs
      if (log.level === 'ERROR') {
        stats.erreurs.push({
          timestamp: log.timestamp,
          message: log.message,
          user: log.user,
          details: log.details || null,
          ligne: index + 1
        });
      }
      
      // Compter les utilisateurs
      if (log.user) {
        stats.utilisateurs[log.user] = (stats.utilisateurs[log.user] || 0) + 1;
      }
      
    } catch (error) {
      stats.logsInvalides++;
      console.warn(`Log invalide ligne ${index + 1}: ${error.message}`);
    }
  });
  
  // Trier les utilisateurs par activité
  const utilisateursTriés = Object.entries(stats.utilisateurs)
    .sort(([,a], [,b]) => b - a)
    .slice(0, 5); // Top 5
  
  return {
    ...stats,
    utilisateursActifs: utilisateursTriés
  };
}

const résultat = analyzerLogs(logs);
console.log(JSON.stringify(résultat, null, 2));
```

</details>

### 🎯 Exercice 2 : Configurateur d'application

{% hint style="info" %}
**Créez un système de configuration avec validation et migration :**
{% endhint %}

```javascript
// Créez une classe ConfigManager qui :
// - Charge la config depuis localStorage
// - Valide la structure
// - Migre automatiquement les anciennes versions
// - Fournit des valeurs par défaut

const schemaV1 = {
  version: 1,
  app: {
    name: "Mon App",
    theme: "light"
  }
};

const schemaV2 = {
  version: 2,
  app: {
    name: "Mon App",
    theme: "light",
    language: "fr"
  },
  user: {
    preferences: {}
  }
};

// Utilisation attendue :
const config = new ConfigManager('app_config', schemaV2);
config.get('app.theme'); // 'light'
config.set('app.theme', 'dark');
config.reset();
```

<details>
<summary>💡 Solution</summary>

```javascript
class ConfigManager {
  constructor(storageKey, schemaActuel) {
    this.storageKey = storageKey;
    this.schemaActuel = schemaActuel;
    this.config = this.charger();
  }
  
  charger() {
    try {
      const stored = localStorage.getItem(this.storageKey);
      if (!stored) {
        return this.cloner(this.schemaActuel);
      }
      
      const config = JSON.parse(stored);
      return this.migrer(config);
      
    } catch (error) {
      console.warn('Erreur lors du chargement de la config:', error);
      return this.cloner(this.schemaActuel);
    }
  }
  
  migrer(config) {
    const versionActuelle = config.version || 1;
    const versionCible = this.schemaActuel.version;
    
    if (versionActuelle < versionCible) {
      console.log(`Migration config v${versionActuelle} → v${versionCible}`);
      
      // Migrations spécifiques
      if (versionActuelle === 1 && versionCible >= 2) {
        config = this.migrerV1VersV2(config);
      }
      
      // Ajouter d'autres migrations ici
      
      config.version = versionCible;
      this.sauvegarder(config);
    }
    
    return config;
  }
  
  migrerV1VersV2(configV1) {
    return {
      version: 2,
      app: {
        ...configV1.app,
        language: 'fr' // Nouvelle propriété
      },
      user: {
        preferences: {} // Nouvelle section
      }
    };
  }
  
  get(chemin) {
    return this.obtenirValeurParChemin(this.config, chemin);
  }
  
  set(chemin, valeur) {
    this.définirValeurParChemin(this.config, chemin, valeur);
    this.sauvegarder(this.config);
  }
  
  obtenirValeurParChemin(obj, chemin) {
    return chemin.split('.').reduce((current, key) => {
      return current?.[key];
    }, obj);
  }
  
  définirValeurParChemin(obj, chemin, valeur) {
    const keys = chemin.split('.');
    const dernièreClé = keys.pop();
    
    const target = keys.reduce((current, key) => {
      if (!(key in current)) {
        current[key] = {};
      }
      return current[key];
    }, obj);
    
    target[dernièreClé] = valeur;
  }
  
  sauvegarder(config = this.config) {
    try {
      localStorage.setItem(this.storageKey, JSON.stringify(config, null, 2));
      return true;
    } catch (error) {
      console.error('Erreur sauvegarde config:', error);
      return false;
    }
  }
  
  reset() {
    this.config = this.cloner(this.schemaActuel);
    this.sauvegarder();
    return this.config;
  }
  
  cloner(obj) {
    return JSON.parse(JSON.stringify(obj));
  }
  
  exporter() {
    return JSON.stringify(this.config, null, 2);
  }
  
  importer(jsonString) {
    try {
      const imported = JSON.parse(jsonString);
      this.config = this.migrer(imported);
      this.sauvegarder();
      return true;
    } catch (error) {
      console.error('Erreur import config:', error);
      return false;
    }
  }
}

// Test
const schema = {
  version: 2,
  app: {
    name: "Mon App",
    theme: "light",
    language: "fr"
  },
  user: {
    preferences: {
      notifications: true
    }
  }
};

const config = new ConfigManager('test_config', schema);
console.log(config.get('app.theme')); // 'light'
config.set('app.theme', 'dark');
config.set('user.preferences.notifications', false);
console.log(config.exporter());
```

</details>

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Quelle est la différence principale entre un objet JavaScript et JSON ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Principales différences :**

- **JSON** est un format **textuel** (chaîne de caractères)
- **Objet JS** est une structure **en mémoire**
- **JSON** utilise des guillemets doubles obligatoires pour les clés
- **JSON** ne supporte pas les fonctions, `undefined`, `Symbol`, etc.
- **JSON** ne peut pas contenir de commentaires

</details>

{% hint style="info" %}
**Question 2 :** Comment gérer en sécurité un JSON potentiellement malformé ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

```javascript
function parseJSONSécurisé(json, defaultValue = null) {
  try {
    if (typeof json !== 'string') return defaultValue;
    return JSON.parse(json);
  } catch (error) {
    console.warn('JSON invalide:', error.message);
    return defaultValue;
  }
}
```

**Toujours utiliser try/catch** avec `JSON.parse()` et fournir une valeur par défaut !

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="../destructuring-spread.md" %}
[destructuring-spread.md](../destructuring-spread.md)
{% endcontent-ref %}

{% content-ref url="../../niveau-avance/fetch-api-avance.md" %}
[fetch-api-avance.md](../../niveau-avance/fetch-api-avance.md)
{% endcontent-ref %}

{% content-ref url="../arrays-methodes-iteration.md" %}
[arrays-methodes-iteration.md](../arrays-methodes-iteration.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Maintenant que vous maîtrisez JSON :**

👉 **Explorez** le [Destructuring et Spread](../destructuring-spread.md)  
👉 **Approfondissez** les [Fetch API Avancée](../../niveau-avance/fetch-api-avance.md)  
👉 **Maîtrisez** les [Méthodes d'itération d'Arrays](../arrays-methodes-iteration.md)  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

📄 **JSON = format textuel** pour l'échange de données  
🔄 **JSON.parse()** : chaîne → objet  
📤 **JSON.stringify()** : objet → chaîne  
🛡️ **Toujours utiliser try/catch** avec JSON.parse()  
🌐 **Universel** : APIs, stockage, configuration  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** JSON est la base de la communication avec les [APIs modernes](../../niveau-avance/fetch-api-avance.md) !
{% endhint %}
