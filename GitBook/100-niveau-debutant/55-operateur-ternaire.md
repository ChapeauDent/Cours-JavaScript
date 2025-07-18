# ❓ Opérateur Ternaire et Expressions Conditionnelles

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 20 min | **Prérequis :** Structures de contrôle
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Maîtriser** la syntaxe de l'opérateur ternaire
- [ ] **Remplacer** les if/else simples par des ternaires
- [ ] **Utiliser** les ternaires pour l'affectation conditionnelle
- [ ] **Éviter** les pièges et anti-patterns des ternaires
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définition" %}
**Opérateur ternaire** : Le seul opérateur JavaScript qui prend 3 opérandes  
**Syntaxe** : `condition ? valeurSiVrai : valeurSiFaux`  
**Alternative** : Version condensée de `if/else`  
**Expression** : Retourne toujours une valeur  
{% endtab %}

{% tab title="Avantages" %}
✅ **Concis** : Une ligne au lieu de plusieurs  
✅ **Expression** : Utilisable dans les assignations  
✅ **Lisible** : Pour les conditions simples  
✅ **Fonctionnel** : Parfait pour le style functional programming  
{% endtab %}
{% endtabs %}

## 📖 Syntaxe de base

### 🔄 Transformation if/else → ternaire

{% tabs %}
{% tab title="if/else classique" %}
```javascript
// Structure if/else traditionnelle
let message;
if (age >= 18) {
  message = "Vous êtes majeur";
} else {
  message = "Vous êtes mineur";
}
console.log(message);
```
{% endtab %}

{% tab title="Opérateur ternaire" %}
```javascript
// Même logique avec l'opérateur ternaire
const message = age >= 18 ? "Vous êtes majeur" : "Vous êtes mineur";
console.log(message);

// Ou directement dans l'utilisation
console.log(age >= 18 ? "Vous êtes majeur" : "Vous êtes mineur");
```
{% endtab %}
{% endtabs %}

### 🎨 Anatomie de l'opérateur ternaire

```javascript
condition ? valeurSiVrai : valeurSiFaux
    ↑           ↑             ↑
  test      résultat si    résultat si
           condition       condition
           vraie          fausse
```

{% hint style="info" %}
**Points importants :**
- 🔍 La **condition** est évaluée en boolean
- ✅ Si `true` → retourne la **première valeur**
- ❌ Si `false` → retourne la **seconde valeur**
- 🎯 C'est une **expression**, pas une instruction
{% endhint %}

## 💡 Exemples pratiques

### 1. 🏷️ Affectation conditionnelle

{% tabs %}
{% tab title="Statut utilisateur" %}
```javascript
const user = { 
  name: "Alice", 
  isAdmin: true,
  credits: 150
};

// Affichage du statut
const status = user.isAdmin ? "Administrateur" : "Utilisateur";
console.log(`Statut: ${status}`);

// Calcul de réduction
const discount = user.credits > 100 ? 0.15 : 0.05;
console.log(`Réduction: ${discount * 100}%`);

// Accès autorisé
const canAccess = user.isAdmin ? "all-pages" : "public-pages";
```
{% endtab %}

{% tab title="Validation de données" %}
```javascript
const email = "user@example.com";
const password = "12345";

// Validation simple
const isValidEmail = email.includes("@") ? true : false;
// Mieux : email.includes("@") suffit (déjà un boolean)

const passwordStrength = password.length >= 8 ? "Fort" : "Faible";

const canLogin = (isValidEmail && password) ? "Connexion autorisée" : "Données invalides";
```
{% endtab %}
{% endtabs %}

### 2. 🎨 Affichage conditionnel

{% tabs %}
{% tab title="Interface utilisateur" %}
```javascript
const items = ["pomme", "banane", "orange"];
const user = { name: "Bob", isLoggedIn: false };

// Affichage de liste
const listDisplay = items.length > 0 
  ? `${items.length} articles disponibles`
  : "Aucun article";

// Message de bienvenue
const greeting = user.isLoggedIn 
  ? `Bonjour ${user.name}!`
  : "Veuillez vous connecter";

// Bouton dynamique
const buttonText = user.isLoggedIn ? "Se déconnecter" : "Se connecter";
const buttonClass = user.isLoggedIn ? "btn-danger" : "btn-primary";
```
{% endtab %}

{% tab title="Formatage de données" %}
```javascript
const price = 1299;
const stock = 0;
const rating = 4.7;

// Prix avec devise
const formattedPrice = price > 0 ? `${price}€` : "Gratuit";

// Disponibilité
const availability = stock > 0 ? `${stock} en stock` : "Rupture de stock";

// Note avec étoiles
const stars = rating >= 4 ? "⭐⭐⭐⭐⭐" : "⭐⭐⭐";

// Couleur du statut
const statusColor = stock > 0 ? "green" : "red";
```
{% endtab %}
{% endtabs %}

### 3. 🔢 Calculs conditionnels

```javascript
const age = 25;
const isStudent = true;
const isMember = false;

// Prix du billet
const ticketPrice = age < 18 ? 10 : (isStudent ? 15 : 20);

// Frais de port
const shippingCost = isMember ? 0 : (price > 50 ? 0 : 5);

// TVA applicable
const taxRate = age < 18 ? 0 : 0.20;
const finalPrice = price * (1 + taxRate);
```

## 🔗 Ternaires imbriqués

{% hint style="warning" %}
**Attention :** Les ternaires imbriqués peuvent devenir illisibles !
{% endhint %}

### ✅ Cas acceptable

{% tabs %}
{% tab title="Imbrication simple" %}
```javascript
const score = 85;

// Lisible avec parenthèses
const grade = score >= 90 ? "A" : (score >= 80 ? "B" : "C");

// Plus clair sur plusieurs lignes
const grade = score >= 90 ? "A" 
            : score >= 80 ? "B" 
            : score >= 70 ? "C"
            : "D";
```
{% endtab %}

{% tab title="Alternative avec switch" %}
```javascript
// Pour les cas complexes, préférez switch
function getGrade(score) {
  switch (true) {
    case score >= 90: return "A";
    case score >= 80: return "B";
    case score >= 70: return "C";
    default: return "D";
  }
}

// Ou une fonction avec if/else
function getGrade(score) {
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  return "D";
}
```
{% endtab %}
{% endtabs %}

### ❌ À éviter : trop complexe

```javascript
// ❌ Difficile à lire et maintenir
const result = condition1 ? 
  (condition2 ? value1 : (condition3 ? value2 : value3)) :
  (condition4 ? value4 : (condition5 ? value5 : value6));

// ✅ Mieux : fonction claire
function getResult() {
  if (condition1) {
    if (condition2) return value1;
    return condition3 ? value2 : value3;
  }
  return condition4 ? value4 : (condition5 ? value5 : value6);
}
```

## 🎮 Cas d'usage modernes

### 🔄 Avec destructuring

```javascript
const user = { name: "Alice", theme: "dark", notifications: true };

// Valeurs par défaut avec ternaire
const { theme = "light", notifications = false } = user;
const displayTheme = theme === "dark" ? "🌙 Mode sombre" : "☀️ Mode clair";

// Configuration d'interface
const config = {
  theme: user.theme || "light",
  showNotifications: user.notifications ? "Activées" : "Désactivées",
  welcomeMessage: user.name ? `Bonjour ${user.name}!` : "Bonjour visiteur!"
};
```

### 🎨 Avec template literals

```javascript
const product = { name: "iPhone", price: 999, inStock: true };

// HTML conditionnel
const productHTML = `
  <div class="product ${product.inStock ? 'available' : 'sold-out'}">
    <h3>${product.name}</h3>
    <p class="price">${product.price}€</p>
    <button ${product.inStock ? '' : 'disabled'}>
      ${product.inStock ? 'Acheter' : 'Rupture de stock'}
    </button>
  </div>
`;
```

### 📱 Avec les arrays

```javascript
const tasks = ["Faire les courses", "Coder", "Sport"];
const users = [];

// Affichage de listes
const taskList = tasks.length > 0 
  ? tasks.map(task => `✓ ${task}`).join('\n')
  : "Aucune tâche";

const userCount = users.length === 0 
  ? "Aucun utilisateur"
  : users.length === 1 
  ? "1 utilisateur"
  : `${users.length} utilisateurs`;
```

## 🔧 Bonnes pratiques

### ✅ Quand utiliser l'opérateur ternaire

{% tabs %}
{% tab title="Cas appropriés" %}
```javascript
// ✅ Affectation simple
const status = isOnline ? "En ligne" : "Hors ligne";

// ✅ Valeur par défaut
const name = user.name ? user.name : "Anonyme";
// Ou mieux avec nullish coalescing (ES2020)
const name = user.name ?? "Anonyme";

// ✅ Propriété conditionnelle
const button = {
  text: "Valider",
  disabled: !isValid ? true : false // ou simplement !isValid
};

// ✅ Return conditionnel
const getDiscount = (isMember) => isMember ? 0.1 : 0;
```
{% endtab %}

{% tab title="Dans les frameworks" %}
```javascript
// ✅ React/Vue - affichage conditionnel
const Component = ({ user }) => (
  <div>
    {user.isAdmin ? <AdminPanel /> : <UserPanel />}
    <p>Bienvenue {user.name ? user.name : 'visiteur'}</p>
  </div>
);

// ✅ Configuration d'API
const apiCall = {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    ...(token ? { 'Authorization': `Bearer ${token}` } : {})
  }
};
```
{% endtab %}
{% endtabs %}

### ❌ Quand l'éviter

```javascript
// ❌ Logique complexe
const result = user && user.isActive && user.permissions 
  ? user.permissions.includes('admin') 
    ? 'full-access' 
    : user.permissions.includes('read') 
      ? 'read-only' 
      : 'no-access'
  : 'not-authenticated';

// ✅ Mieux avec une fonction
function getUserAccess(user) {
  if (!user?.isActive || !user?.permissions) return 'not-authenticated';
  if (user.permissions.includes('admin')) return 'full-access';
  if (user.permissions.includes('read')) return 'read-only';
  return 'no-access';
}

// ❌ Effets de bord
condition ? console.log("debug") : null; // Utilise un if classique

// ❌ Mutations
condition ? array.push(item) : array.pop(); // Pas clair
```

## 🧪 Exercices pratiques

### 🎯 Exercice 1 : Conversion if/else

{% hint style="info" %}
**Convertissez ces structures if/else en opérateurs ternaires :**
{% endhint %}

```javascript
// Code à convertir
let weatherMessage;
if (temperature > 25) {
  weatherMessage = "Il fait chaud !";
} else {
  weatherMessage = "Il fait bon.";
}

let accessLevel;
if (user.isAdmin) {
  accessLevel = "Accès complet";
} else {
  accessLevel = "Accès limité";
}
```

<details>
<summary>💡 Solution</summary>

```javascript
// Version ternaire
const weatherMessage = temperature > 25 ? "Il fait chaud !" : "Il fait bon.";

const accessLevel = user.isAdmin ? "Accès complet" : "Accès limité";

// Bonus : utilisation directe
console.log(temperature > 25 ? "Il fait chaud !" : "Il fait bon.");
```

</details>

### 🎯 Exercice 2 : Interface dynamique

{% hint style="info" %}
**Créez une interface qui s'adapte selon l'état utilisateur :**
{% endhint %}

```javascript
const user = {
  name: "Marie",
  isLoggedIn: true,
  isVIP: false,
  cart: ["livre", "stylo"],
  balance: 50
};

// Créez ces éléments avec des ternaires :
// 1. Message de bienvenue (nom si connecté, "Visiteur" sinon)
// 2. Classe CSS pour le statut VIP
// 3. Texte du panier (nombre d'articles ou "vide")
// 4. Bouton de paiement (enabled si balance suffisante)
```

<details>
<summary>💡 Solution</summary>

```javascript
const user = {
  name: "Marie",
  isLoggedIn: true,
  isVIP: false,
  cart: ["livre", "stylo"],
  balance: 50
};

// 1. Message de bienvenue
const welcomeMessage = user.isLoggedIn 
  ? `Bonjour ${user.name}!` 
  : "Bonjour visiteur!";

// 2. Classe CSS pour VIP
const userClass = user.isVIP ? "user-vip" : "user-standard";

// 3. Texte du panier
const cartText = user.cart.length > 0 
  ? `${user.cart.length} article${user.cart.length > 1 ? 's' : ''}`
  : "Panier vide";

// 4. Bouton de paiement (supposons total = 30€)
const totalPrice = 30;
const paymentButton = user.balance >= totalPrice 
  ? { text: "Payer", disabled: false, class: "btn-success" }
  : { text: "Solde insuffisant", disabled: true, class: "btn-disabled" };

console.log(welcomeMessage);
console.log(`Classe: ${userClass}`);
console.log(`Panier: ${cartText}`);
console.log(`Bouton: ${paymentButton.text}`);
```

</details>

### 🎯 Exercice 3 : Validation de formulaire

```javascript
const formData = {
  email: "test@example.com",
  password: "12345",
  confirmPassword: "12345",
  age: 17
};

// Utilisez des ternaires pour créer ces validations :
// 1. Email valide (contient @ et .)
// 2. Mot de passe suffisant (8+ caractères)
// 3. Confirmation identique
// 4. Âge autorisé (18+)
// 5. Formulaire valide (toutes conditions OK)
```

<details>
<summary>💡 Solution</summary>

```javascript
const formData = {
  email: "test@example.com",
  password: "12345",
  confirmPassword: "12345",
  age: 17
};

// Validations avec ternaires
const isEmailValid = formData.email.includes("@") && formData.email.includes(".") 
  ? true : false;
// Ou plus simplement : formData.email.includes("@") && formData.email.includes(".")

const isPasswordValid = formData.password.length >= 8 
  ? true : false;

const isPasswordConfirmed = formData.password === formData.confirmPassword 
  ? true : false;

const isAgeValid = formData.age >= 18 
  ? true : false;

const isFormValid = isEmailValid && isPasswordValid && isPasswordConfirmed && isAgeValid 
  ? true : false;

// Messages d'erreur
const emailMessage = isEmailValid ? "✅ Email valide" : "❌ Email invalide";
const passwordMessage = isPasswordValid ? "✅ Mot de passe OK" : "❌ Minimum 8 caractères";
const confirmMessage = isPasswordConfirmed ? "✅ Confirmation OK" : "❌ Mots de passe différents";
const ageMessage = isAgeValid ? "✅ Âge OK" : "❌ Vous devez être majeur";
const submitMessage = isFormValid ? "✅ Formulaire prêt" : "❌ Corrigez les erreurs";

console.log(emailMessage);
console.log(passwordMessage);
console.log(confirmMessage);
console.log(ageMessage);
console.log(submitMessage);
```

</details>

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Que retourne cette expression ?
```javascript
const result = false ? "A" : true ? "B" : "C";
```
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**"B"**

L'associativité de droite à gauche donne :
`false ? "A" : (true ? "B" : "C")`
- `false` → va à la partie `else`
- `true ? "B" : "C"` → retourne "B"

</details>

{% hint style="info" %}
**Question 2 :** Pourquoi éviter ce code ?
```javascript
const x = condition ? doSomething() : doSomethingElse();
```
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Si `doSomething()` et `doSomethingElse()` ont des effets de bord !**

L'opérateur ternaire est fait pour retourner des **valeurs**, pas pour exécuter des **actions**.

**Mieux :**
```javascript
if (condition) {
  doSomething();
} else {
  doSomethingElse();
}
```

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="../structures-controle.md" %}
[structures-controle.md](../structures-controle.md)
{% endcontent-ref %}

{% content-ref url="../operateurs-logiques.md" %}
[operateurs-logiques.md](../operateurs-logiques.md)
{% endcontent-ref %}

{% content-ref url="../../niveau-intermediaire/destructuring-spread.md" %}
[destructuring-spread.md](../../niveau-intermediaire/destructuring-spread.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Maintenant que vous maîtrisez l'opérateur ternaire :**

👉 **Combinez** avec les [Opérateurs Logiques](../operateurs-logiques.md)  
👉 **Explorez** le [Destructuring et Spread](../../niveau-intermediaire/destructuring-spread.md)  
👉 **Approfondissez** les [Fonctions Avancées](../../niveau-intermediaire/fonctions-avancees.md)  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

❓ **Syntaxe :** `condition ? valeurSiVrai : valeurSiFaux`  
✨ **Usage :** Parfait pour les affectations conditionnelles simples  
🎯 **Lisibilité :** Idéal pour remplacer les if/else courts  
⚠️ **Limitation :** Éviter les imbrications complexes  
🔄 **Expression :** Retourne toujours une valeur (utilisable partout)  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Découvrez les [Opérateurs Logiques](../operateurs-logiques.md) pour des conditions plus sophistiquées !
{% endhint %}
