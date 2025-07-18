# 🔐 Closures et Lexical Scope

{% hint style="info" %}
**Niveau :** 🟡 Intermédiaire | **Durée :** 45 min | **Prérequis :** Fonctions, Hoisting, Portée
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** le mécanisme des closures et du lexical scope
- [ ] **Créer** des fonctions avec données privées
- [ ] **Appliquer** les patterns courants avec closures
- [ ] **Déboguer** les problèmes liés aux closures
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Closure** : Fonction qui "capture" des variables de son environnement  
**Lexical Scope** : Portée déterminée par où les variables sont déclarées  
**Factory Function** : Fonction qui crée d'autres fonctions avec closures  
**Encapsulation** : Protection des données internes  
{% endtab %}

{% tab title="Importance" %}
🏗️ **Encapsulation** : Créer des données privées  
🏭 **Factory patterns** : Générer des fonctions spécialisées  
⚡ **Callbacks avancés** : Préserver le contexte  
📦 **Modules** : Organiser le code proprement  
{% endtab %}
{% endtabs %}

## 📖 Lexical Scope expliqué

{% hint style="warning" %}
**Lexical Scope :** La portée est déterminée par **où** les variables sont déclarées dans le code, pas par **comment** les fonctions sont appelées.
{% endhint %}

### 🔍 Visualisation de la chaîne de portées

{% tabs %}
{% tab title="Exemple de base" %}
```javascript
let global = "Je suis globale";

function externe() {
  let secret = "Je suis dans externe()";
  
  function interne() {
    let local = "Je suis locale";
    
    // interne() a accès à TOUTES ces variables :
    console.log(local);  // ✅ Variable locale
    console.log(secret); // ✅ Variable du parent
    console.log(global); // ✅ Variable globale
  }
  
  return interne;
}

const maFonction = externe();
maFonction(); // Fonctionne même après la fin d'externe() !
```
{% endtab %}

{% tab title="Chaîne de recherche" %}
```javascript
// JavaScript cherche les variables dans cet ordre :
// 1. Scope local (interne)
// 2. Scope parent (externe) 
// 3. Scope global
// 4. ReferenceError si introuvable

function a() {
  let x = 1;
  
  function b() {
    let x = 2; // Masque le x du parent
    
    function c() {
      console.log(x); // Affiche 2 (le plus proche)
    }
    
    return c;
  }
  
  return b;
}
```
{% endtab %}
{% endtabs %}

## 🏗️ Qu'est-ce qu'une Closure ?

### 📋 Définition simple

{% hint style="info" %}
**Une closure = fonction + environnement lexical dans lequel elle a été créée**

La fonction "se souvient" des variables de son environnement, même après que cet environnement ait disparu !
{% endhint %}

### 🎯 Exemple fondamental

{% tabs %}
{% tab title="Compteur avec closure" %}
```javascript
function creerCompteur() {
  let compteur = 0; // Variable "capturée"
  
  return function() {
    compteur++; // La fonction "se souvient" de compteur
    return compteur;
  };
}

const monCompteur = creerCompteur();
console.log(monCompteur()); // 1
console.log(monCompteur()); // 2
console.log(monCompteur()); // 3

// La variable 'compteur' est toujours accessible !
// Elle vit dans la closure
```
{% endtab %}

{% tab title="Plusieurs instances" %}
```javascript
function creerCompteur() {
  let compteur = 0;
  
  return function() {
    compteur++;
    return compteur;
  };
}

// Chaque compteur a sa propre closure !
const compteur1 = creerCompteur();
const compteur2 = creerCompteur();

console.log(compteur1()); // 1
console.log(compteur1()); // 2
console.log(compteur2()); // 1 (indépendant !)
console.log(compteur1()); // 3
```
{% endtab %}
{% endtabs %}

## 🏭 Factory Functions et Données Privées

### 🔒 Encapsulation avec closures

{% tabs %}
{% tab title="Compte bancaire sécurisé" %}
```javascript
function creerCompteBancaire(soldeInitial) {
  let solde = soldeInitial; // Variable privée !
  
  return {
    // Méthodes publiques
    deposer(montant) {
      if (montant > 0) {
        solde += montant;
        return `Dépôt de ${montant}€. Solde: ${solde}€`;
      }
      return "Montant invalide";
    },
    
    retirer(montant) {
      if (montant > 0 && montant <= solde) {
        solde -= montant;
        return `Retrait de ${montant}€. Solde: ${solde}€`;
      }
      return "Retrait impossible";
    },
    
    consulterSolde() {
      return `Solde actuel: ${solde}€`;
    }
    
    // Pas d'accès direct à 'solde' depuis l'extérieur !
  };
}

const compte = creerCompteBancaire(100);
console.log(compte.consulterSolde()); // "Solde actuel: 100€"
console.log(compte.deposer(50));      // "Dépôt de 50€. Solde: 150€"
console.log(compte.retirer(30));      // "Retrait de 30€. Solde: 120€"

// ❌ Impossible d'accéder directement au solde
console.log(compte.solde); // undefined
```
{% endtab %}

{% tab title="Configuration d'API" %}
```javascript
function creerApiClient(baseUrl, apiKey) {
  // Données privées dans la closure
  const config = {
    baseUrl,
    apiKey,
    timeout: 5000
  };
  
  return {
    get(endpoint) {
      return fetch(`${config.baseUrl}${endpoint}`, {
        headers: {
          'Authorization': `Bearer ${config.apiKey}`,
          'Content-Type': 'application/json'
        },
        timeout: config.timeout
      });
    },
    
    post(endpoint, data) {
      return fetch(`${config.baseUrl}${endpoint}`, {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${config.apiKey}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data),
        timeout: config.timeout
      });
    },
    
    // Configuration protégée
    setTimeout(newTimeout) {
      config.timeout = newTimeout;
    }
  };
}

const api = creerApiClient('https://api.example.com', 'secret-key');
api.get('/users').then(response => response.json());
```
{% endtab %}
{% endtabs %}

## 🎮 Patterns courants avec closures

### 1. 🎛️ Module Pattern

{% tabs %}
{% tab title="Module simple" %}
```javascript
const MonModule = (function() {
  // Variables privées
  let compteurPrivé = 0;
  const donnéesPrivées = [];
  
  // Fonctions privées
  function fonctionPrivée() {
    return "Accessible seulement dans le module";
  }
  
  // API publique
  return {
    incrementer() {
      compteurPrivé++;
      return compteurPrivé;
    },
    
    ajouter(item) {
      donnéesPrivées.push(item);
      return donnéesPrivées.length;
    },
    
    obtenirTotal() {
      return compteurPrivé;
    },
    
    obtenirDonnées() {
      return [...donnéesPrivées]; // Copie pour protection
    }
  };
})();

// Utilisation
console.log(MonModule.incrementer()); // 1
console.log(MonModule.ajouter("test")); // 1
console.log(MonModule.obtenirTotal()); // 1

// ❌ Accès impossible aux données privées
console.log(MonModule.compteurPrivé); // undefined
```
{% endtab %}

{% tab title="Module avec paramètres" %}
```javascript
const creerCalculatrice = (function() {
  return function(nom) {
    let historique = [];
    
    return {
      nom,
      
      additionner(a, b) {
        const résultat = a + b;
        historique.push(`${a} + ${b} = ${résultat}`);
        return résultat;
      },
      
      multiplier(a, b) {
        const résultat = a * b;
        historique.push(`${a} × ${b} = ${résultat}`);
        return résultat;
      },
      
      obtenirHistorique() {
        return [...historique];
      },
      
      effacerHistorique() {
        historique = [];
        return "Historique effacé";
      }
    };
  };
})();

const calc1 = creerCalculatrice("Calculatrice Pro");
const calc2 = creerCalculatrice("Calculatrice Basic");

calc1.additionner(5, 3); // 8
calc2.multiplier(4, 7);  // 28

console.log(calc1.obtenirHistorique()); // ["5 + 3 = 8"]
console.log(calc2.obtenirHistorique()); // ["4 × 7 = 28"]
```
{% endtab %}
{% endtabs %}

### 2. 🔄 Callbacks et Event Handlers

{% tabs %}
{% tab title="Gestionnaire d'événements" %}
```javascript
function creerGestionnaireClics(message) {
  let nombreClics = 0;
  
  return function(event) {
    nombreClics++;
    console.log(`${message} - Clic n°${nombreClics}`);
    
    // La closure "se souvient" de message et nombreClics
    if (nombreClics >= 5) {
      console.log("Limite de clics atteinte !");
      event.target.removeEventListener('click', arguments.callee);
    }
  };
}

// Utilisation avec des éléments DOM
const bouton1 = document.getElementById('btn1');
const bouton2 = document.getElementById('btn2');

bouton1.addEventListener('click', creerGestionnaireClics('Bouton 1 cliqué'));
bouton2.addEventListener('click', creerGestionnaireClics('Bouton 2 cliqué'));

// Chaque bouton a son propre compteur !
```
{% endtab %}

{% tab title="Timeout avec contexte" %}
```javascript
function creerProgrammateur(nom) {
  let tâches = [];
  
  return {
    programmer(callback, délai, ...args) {
      const id = Date.now();
      
      const timeoutId = setTimeout(() => {
        console.log(`[${nom}] Exécution de la tâche ${id}`);
        callback(...args);
        
        // Supprimer de la liste des tâches
        tâches = tâches.filter(t => t.id !== id);
      }, délai);
      
      tâches.push({ id, timeoutId, délai });
      return id;
    },
    
    annuler(id) {
      const tâche = tâches.find(t => t.id === id);
      if (tâche) {
        clearTimeout(tâche.timeoutId);
        tâches = tâches.filter(t => t.id !== id);
        return `Tâche ${id} annulée`;
      }
      return "Tâche introuvable";
    },
    
    obtenirTâchesEnCours() {
      return tâches.map(t => ({ id: t.id, délai: t.délai }));
    }
  };
}

const programmateur = creerProgrammateur("Mon Programmateur");

const id1 = programmateur.programmer(() => {
  console.log("Tâche 1 exécutée !");
}, 2000);

const id2 = programmateur.programmer((message) => {
  console.log(`Tâche 2: ${message}`);
}, 1000, "Hello World!");

console.log(programmateur.obtenirTâchesEnCours());
```
{% endtab %}
{% endtabs %}

## 🐛 Pièges courants et solutions

### ⚠️ Piège classique : Boucles et closures

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
// ❌ Piège classique avec var
function creerFonctions() {
  const fonctions = [];
  
  for (var i = 0; i < 3; i++) {
    fonctions.push(function() {
      console.log(i); // Affiche toujours 3 !
    });
  }
  
  return fonctions;
}

const mesFonctions = creerFonctions();
mesFonctions[0](); // 3 (pas 0 !)
mesFonctions[1](); // 3 (pas 1 !)
mesFonctions[2](); // 3 (pas 2 !)

// Problème : toutes les fonctions partagent la même variable i
```
{% endtab %}

{% tab title="✅ Solutions" %}
```javascript
// ✅ Solution 1 : let (scope de bloc)
function creerFonctions() {
  const fonctions = [];
  
  for (let i = 0; i < 3; i++) { // let au lieu de var
    fonctions.push(function() {
      console.log(i); // Chaque i est dans son propre scope
    });
  }
  
  return fonctions;
}

// ✅ Solution 2 : IIFE (Immediately Invoked Function Expression)
function creerFonctions() {
  const fonctions = [];
  
  for (var i = 0; i < 3; i++) {
    fonctions.push((function(index) {
      return function() {
        console.log(index); // Capture index dans la closure
      };
    })(i));
  }
  
  return fonctions;
}

// ✅ Solution 3 : bind()
function creerFonctions() {
  const fonctions = [];
  
  for (var i = 0; i < 3; i++) {
    fonctions.push(function(index) {
      console.log(index);
    }.bind(null, i));
  }
  
  return fonctions;
}
```
{% endtab %}
{% endtabs %}

### 🧠 Fuites mémoire et performance

{% hint style="warning" %}
**Attention :** Les closures gardent des références vers leur environnement lexical, ce qui peut causer des fuites mémoire !
{% endhint %}

{% tabs %}
{% tab title="⚠️ Problème de mémoire" %}
```javascript
// ❌ Fuite mémoire potentielle
function creerGestionnaireAvecFuite() {
  const grosseseDonnées = new Array(1000000).fill('data'); // 1M d'éléments
  
  return function() {
    // Cette fonction garde toute grosseseDonnées en mémoire
    // même si elle n'utilise qu'un élément !
    return grosseseDonnées[0];
  };
}

// grosseseDonnées ne sera jamais libérée !
```
{% endtab %}

{% tab title="✅ Solution optimisée" %}
```javascript
// ✅ Optimisation
function creerGestionnaireOptimisé() {
  const grosseseDonnées = new Array(1000000).fill('data');
  const premierElement = grosseseDonnées[0]; // Extraire seulement ce qui est nécessaire
  
  // Libérer la référence
  grosseseDonnées = null;
  
  return function() {
    return premierElement; // Seul premierElement est gardé en mémoire
  };
}

// Ou mieux : ne capturer que ce qui est nécessaire
function creerGestionnaireMinimal() {
  const grosseseDonnées = new Array(1000000).fill('data');
  
  return (function(element) {
    // IIFE qui capture seulement l'élément nécessaire
    return function() {
      return element;
    };
  })(grosseseDonnées[0]);
}
```
{% endtab %}
{% endtabs %}

## 🧪 Exercices pratiques

### 🎯 Exercice 1 : Cache avec TTL

{% hint style="info" %}
**Créez un système de cache avec expiration automatique :**
{% endhint %}

```javascript
// Créez une fonction creerCache() qui retourne un objet avec :
// - set(clé, valeur, ttl) : stocke une valeur avec time-to-live
// - get(clé) : récupère une valeur si elle n'a pas expiré
// - clear() : vide le cache
// - size() : retourne le nombre d'éléments

// Utilisation attendue :
const cache = creerCache();
cache.set('user:1', { nom: 'Alice' }, 5000); // Expire dans 5s
cache.set('user:2', { nom: 'Bob' }, 10000);  // Expire dans 10s

console.log(cache.get('user:1')); // { nom: 'Alice' }
// Après 6s :
console.log(cache.get('user:1')); // null (expiré)
console.log(cache.size()); // 1 (user:2 encore valide)
```

<details>
<summary>💡 Solution</summary>

```javascript
function creerCache() {
  let cache = new Map();
  
  return {
    set(clé, valeur, ttl = Infinity) {
      const expiration = Date.now() + ttl;
      cache.set(clé, { valeur, expiration });
      
      // Nettoyage automatique après expiration
      if (ttl !== Infinity) {
        setTimeout(() => {
          if (cache.has(clé)) {
            const item = cache.get(clé);
            if (item && Date.now() >= item.expiration) {
              cache.delete(clé);
            }
          }
        }, ttl);
      }
    },
    
    get(clé) {
      if (!cache.has(clé)) return null;
      
      const item = cache.get(clé);
      if (Date.now() >= item.expiration) {
        cache.delete(clé);
        return null;
      }
      
      return item.valeur;
    },
    
    clear() {
      cache.clear();
    },
    
    size() {
      // Nettoyer les éléments expirés avant de compter
      const now = Date.now();
      for (const [clé, item] of cache.entries()) {
        if (now >= item.expiration) {
          cache.delete(clé);
        }
      }
      return cache.size;
    }
  };
}
```

</details>

### 🎯 Exercice 2 : Rate Limiter

{% hint style="info" %}
**Créez un limiteur de taux d'appels :**
{% endhint %}

```javascript
// Créez creerRateLimiter(maxAppels, fenêtreTempsMsMs) qui retourne une fonction
// qui limite le nombre d'appels dans une fenêtre de temps

// Utilisation :
const limiter = creerRateLimiter(5, 60000); // 5 appels max par minute

for (let i = 0; i < 8; i++) {
  const result = limiter(() => console.log(`Appel ${i + 1}`));
  if (!result) {
    console.log(`Appel ${i + 1} bloqué - limite atteinte`);
  }
}
```

<details>
<summary>💡 Solution</summary>

```javascript
function creerRateLimiter(maxAppels, fenêtreTemps) {
  let appels = [];
  
  return function(callback) {
    const maintenant = Date.now();
    
    // Nettoyer les appels expirés
    appels = appels.filter(temps => maintenant - temps < fenêtreTemps);
    
    if (appels.length >= maxAppels) {
      return false; // Limite atteinte
    }
    
    // Enregistrer cet appel
    appels.push(maintenant);
    
    // Exécuter le callback
    if (typeof callback === 'function') {
      return callback();
    }
    
    return true;
  };
}

// Version avancée avec multiple keys
function creerRateLimiterAvancé(maxAppels, fenêtreTemps) {
  const limites = new Map();
  
  return function(clé, callback) {
    if (!limites.has(clé)) {
      limites.set(clé, []);
    }
    
    const appels = limites.get(clé);
    const maintenant = Date.now();
    
    // Nettoyer les appels expirés pour cette clé
    const appelsValides = appels.filter(temps => maintenant - temps < fenêtreTemps);
    limites.set(clé, appelsValides);
    
    if (appelsValides.length >= maxAppels) {
      return false;
    }
    
    appelsValides.push(maintenant);
    limites.set(clé, appelsValides);
    
    if (typeof callback === 'function') {
      return callback();
    }
    
    return true;
  };
}
```

</details>

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Que va afficher ce code ?
```javascript
function créerFonctions() {
  const fns = [];
  for (let i = 0; i < 3; i++) {
    fns.push(() => i);
  }
  return fns;
}

const fonctions = créerFonctions();
console.log(fonctions[0](), fonctions[1](), fonctions[2]());
```
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**0 1 2**

Avec `let`, chaque itération de la boucle crée un nouveau scope de bloc. Chaque fonction arrow capture sa propre variable `i`.

Si c'était `var`, le résultat serait `3 3 3` car toutes les fonctions partageraient la même variable `i`.

</details>

{% hint style="info" %}
**Question 2 :** Les closures peuvent-elles causer des fuites mémoire ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Oui !** Les closures gardent des références vers leur environnement lexical entier. Si une closure référence une grosse structure de données mais n'utilise qu'une petite partie, toute la structure reste en mémoire.

**Solution :** Extraire seulement les données nécessaires avant de créer la closure.

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="../fonctions-avancees.md" %}
[fonctions-avancees.md](../fonctions-avancees.md)
{% endcontent-ref %}

{% content-ref url="../prototypes-heritage.md" %}
[prototypes-heritage.md](../prototypes-heritage.md)
{% endcontent-ref %}

{% content-ref url="../../niveau-avance/modules-organisation.md" %}
[modules-organisation.md](../../niveau-avance/modules-organisation.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Maintenant que vous maîtrisez les closures :**

👉 **Explorez** les [Fonctions Avancées](../fonctions-avancees.md)  
👉 **Découvrez** les [Prototypes et Héritage](../prototypes-heritage.md)  
👉 **Approfondissez** l'[Organisation en Modules](../../niveau-avance/modules-organisation.md)  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🔐 **Closure = fonction + environnement lexical** capturé  
🏗️ **Encapsulation** : Créer des données privées facilement  
🏭 **Factory functions** : Générer des fonctions spécialisées  
⚠️ **Attention mémoire** : Les closures gardent tout l'environnement  
🎯 **Lexical scope** : Portée déterminée par l'emplacement dans le code  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Les closures sont la fondation des [Fonctions Avancées](../fonctions-avancees.md) !
{% endhint %}
