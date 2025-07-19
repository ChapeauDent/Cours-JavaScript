# 🚀 Introduction à JavaScript

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 90 min | **Prérequis :** Notions HTML/CSS
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** ce qu'est JavaScript et son rôle dans le web
- [ ] **Identifier** les différents environnements d'exécution
- [ ] **Configurer** un environnement de développement simple
- [ ] **Écrire** votre premier programme JavaScript
- [ ] **Utiliser** la console et les outils de développement
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**JavaScript** : Langage de programmation pour rendre les pages web interactives  
**Console** : Interface de débogage dans le navigateur  
**Script** : Fichier contenant du code JavaScript  
**DOM** : Document Object Model - structure de la page web  
**Interprété** : Exécuté ligne par ligne sans compilation préalable  
{% endtab %}

{% tab title="Importance" %}
🌐 **Langage universel du web** : Seul langage compris par tous les navigateurs  
💡 **Interactivité** : Transforme les pages statiques en applications  
🔄 **Polyvalent** : Web, mobile, desktop, serveur  
📈 **Carrière** : Compétence très demandée sur le marché  
🚀 **Évolutif** : Écosystème riche et en constante évolution  
{% endtab %}
{% endtabs %}

## 🌐 Qu'est-ce que JavaScript ?

### 📋 Les trois piliers du web moderne

{% hint style="info" %}
**Analogie simple :** Si une page web était une maison...
- 🏗️ **HTML** = La structure (murs, toit, fondations)
- 🎨 **CSS** = La décoration (couleurs, style, aménagement)  
- ⚡ **JavaScript** = L'électricité (éclairage, appareils, automation)
{% endhint %}

{% tabs %}
{% tab title="HTML : Structure" %}
```html
<!-- HTML : Le squelette de la page -->
<!DOCTYPE html>
<html>
<head>
    <title>Ma page</title>
</head>
<body>
    <h1>Bienvenue</h1>
    <button id="monBouton">Cliquez-moi</button>
    <p id="message">Pas encore cliqué</p>
</body>
</html>
```

**Rôle :** Définit le contenu et la structure de la page
{% endtab %}

{% tab title="CSS : Style" %}
```css
/* CSS : L'apparence de la page */
body {
    font-family: Arial, sans-serif;
    background-color: #f0f8ff;
    padding: 20px;
}

button {
    background-color: #007bff;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}

button:hover {
    background-color: #0056b3;
}
```

**Rôle :** Définit l'apparence et la mise en page
{% endtab %}

{% tab title="JavaScript : Comportement" %}
```javascript
// JavaScript : L'intelligence de la page
document.getElementById('monBouton').addEventListener('click', function() {
    const message = document.getElementById('message');
    message.textContent = "Merci d'avoir cliqué !";
    message.style.color = "green";
    
    // Animation simple
    message.style.fontSize = "24px";
    setTimeout(() => {
        message.style.fontSize = "16px";
    }, 300);
});

// Message de bienvenue
console.log("🎉 Page chargée ! JavaScript est actif !");
```

**Rôle :** Ajoute l'interactivité et la logique
{% endtab %}
{% endtabs %}

### 🆚 JavaScript ≠ Java

{% hint style="warning" %}
**Idée fausse commune :** "JavaScript est une version simplifiée de Java"

**Réalité :** Ce sont deux langages **complètement différents** !
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// JavaScript - Syntaxe simple et flexible
let message = "Bonjour le monde !";
console.log(message);

function calculer(a, b) {
    return a + b; // Pas besoin de spécifier les types
}

// Exécution directe dans le navigateur
```

**Caractéristiques :**
- 🌐 Conçu pour le web
- 📝 Syntaxe simple et flexible
- 🏃‍♂️ Interprété (exécution immédiate)
- 🎯 Typé dynamiquement
{% endtab %}

{% tab title="Java" %}
```java
// Java - Syntaxe plus stricte
public class HelloWorld {
    public static void main(String[] args) {
        String message = "Bonjour le monde !";
        System.out.println(message);
    }
    
    public static int calculer(int a, int b) {
        return a + b; // Types obligatoires
    }
}

// Compilation nécessaire avant exécution
```

**Caractéristiques :**
- 🏢 Conçu pour les applications d'entreprise
- 📋 Syntaxe stricte et verbale
- ⚙️ Compilé (création d'un fichier exécutable)
- 🎯 Typé statiquement
{% endtab %}
{% endtabs %}

## 🌍 Où s'exécute JavaScript ?

### 💻 Côté Client (Navigateur)

{% tabs %}
{% tab title="Dans le navigateur" %}
```javascript
// Manipulation de la page web
document.title = "Nouvelle page";
document.body.style.backgroundColor = "lightblue";

// Interactions utilisateur
button.onclick = function() {
    alert("Bouton cliqué !");
};

// Animations et effets
element.animate([
    { transform: 'scale(1)' },
    { transform: 'scale(1.1)' },
    { transform: 'scale(1)' }
], 500);

// Communication avec des serveurs
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data));
```

**Utilisations courantes :**
- 🖱️ Gestion des clics et événements
- 🎨 Animations et transitions
- 📡 Récupération de données
- ✅ Validation de formulaires
{% endtab %}

{% tab title="Exemples concrets" %}
**Sites populaires utilisant JavaScript :**

🔍 **Google** : Recherche instantanée, Google Maps  
📘 **Facebook** : Timeline infinie, chat en temps réel  
🛒 **Amazon** : Recommandations, panier dynamique  
🎵 **Spotify** : Lecteur web, playlists interactives  
📧 **Gmail** : Interface email riche, recherche  

**Ce que JavaScript permet :**
- ✨ **Interactivité** : Boutons, menus, formulaires
- 🔄 **Temps réel** : Chat, notifications, mises à jour
- 🎬 **Multimédia** : Lecteurs vidéo, galeries photos
- 📊 **Visualisation** : Graphiques, cartes interactives
{% endtab %}
{% endtabs %}

### 🖥️ Côté Serveur (Node.js)

{% tabs %}
{% tab title="Applications serveur" %}
```javascript
// Serveur web avec Node.js
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>Serveur JavaScript !</h1>');
});

server.listen(3000, () => {
    console.log('Serveur démarré sur http://localhost:3000');
});

// API REST
const express = require('express');
const app = express();

app.get('/api/users', (req, res) => {
    res.json({ users: ['Alice', 'Bob', 'Charlie'] });
});

// Base de données
const users = await database.query('SELECT * FROM users');
```

**Avantages :**
- 🌐 Même langage côté client et serveur
- ⚡ Performance élevée
- 📦 Écosystème npm riche
- 🔄 Temps réel facilité
{% endtab %}

{% tab title="Autres environnements" %}
**JavaScript partout :**

📱 **Applications mobiles**
```javascript
// React Native
import React from 'react';
import { Text, View } from 'react-native';

export default function App() {
    return (
        <View>
            <Text>Hello Mobile World!</Text>
        </View>
    );
}
```

🖥️ **Applications desktop**
```javascript
// Electron
const { app, BrowserWindow } = require('electron');

function createWindow() {
    const win = new BrowserWindow({
        width: 800,
        height: 600
    });
    win.loadFile('index.html');
}

app.whenReady().then(() => createWindow());
```

🤖 **IoT et objets connectés**
```javascript
// Johnny-Five (Arduino)
const five = require('johnny-five');
const board = new five.Board();

board.on('ready', function() {
    const led = new five.Led(13);
    led.blink();
});
```
{% endtab %}
{% endtabs %}

## ⚙️ Comment JavaScript fonctionne

### 🔄 Langage interprété

{% hint style="info" %}
**JavaScript est interprété en temps réel :** Le code est lu et exécuté ligne par ligne, sans compilation préalable.
{% endhint %}

{% tabs %}
{% tab title="Exécution ligne par ligne" %}
```javascript
// Le navigateur lit et exécute chaque ligne immédiatement
console.log("1. Première instruction");     // ✅ Exécutée
console.log("2. Deuxième instruction");     // ✅ Exécutée
console.log("3. Troisième instruction");    // ✅ Exécutée

// Si erreur à la ligne 4...
console.log("4. Erreur syntaxe);           // ❌ Erreur !
console.log("5. Cette ligne ne s'exécute jamais");
```

**Avantages :**
- 🚀 **Développement rapide** : Pas de compilation
- 🔧 **Débogage facile** : Erreurs pointées précisément
- 💡 **Tests immédiats** : Résultats instantanés

**Inconvénients :**
- 🐌 **Parfois plus lent** que les langages compilés
- 🔍 **Erreurs à l'exécution** : Pas de vérification préalable
{% endtab %}

{% tab title="Typé dynamiquement" %}
```javascript
// Les variables peuvent changer de type librement
let maVariable = "Bonjour";        // String
console.log(typeof maVariable);    // "string"

maVariable = 42;                   // Maintenant c'est un Number
console.log(typeof maVariable);    // "number"

maVariable = true;                 // Maintenant c'est un Boolean
console.log(typeof maVariable);    // "boolean"

maVariable = { nom: "Alice" };     // Maintenant c'est un Object
console.log(typeof maVariable);    // "object"

// JavaScript s'adapte automatiquement !
```

**Flexibilité :**
- ✅ **Facile pour débuter** : Moins de règles strictes
- ✅ **Prototypage rapide** : Tests d'idées facilitées
- ⚠️ **Attention aux erreurs** : Plus de vigilance nécessaire
{% endtab %}
{% endtabs %}

## 🛠️ Premier contact avec JavaScript

### 🎯 Votre premier programme

{% hint style="success" %}
**Objectif :** Créer une page web interactive simple pour comprendre les bases de JavaScript.
{% endhint %}

{% tabs %}
{% tab title="Code complet" %}
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🚀 Mon premier JavaScript</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background-color: #f8f9fa;
        }
        
        .container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        button {
            background-color: #007bff;
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            margin: 10px 5px;
            transition: background-color 0.3s;
        }
        
        button:hover {
            background-color: #0056b3;
        }
        
        .result {
            margin-top: 20px;
            padding: 15px;
            background-color: #e7f3ff;
            border-left: 4px solid #007bff;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎉 Bienvenue dans JavaScript !</h1>
        <p>Cliquez sur les boutons pour voir JavaScript en action :</p>
        
        <button onclick="direBonjour()">👋 Dire bonjour</button>
        <button onclick="changerCouleur()">🎨 Changer la couleur</button>
        <button onclick="afficherHeure()">⏰ Afficher l'heure</button>
        <button onclick="demanderNom()">📝 Demander votre nom</button>
        
        <div id="resultat" class="result" style="display: none;">
            <h3>Résultat :</h3>
            <p id="message">Aucun message pour le moment...</p>
        </div>
    </div>

    <script>
        // 🎯 Fonctions JavaScript
        
        // Message de bienvenue dans la console
        console.log("🚀 JavaScript est chargé et prêt !");
        console.log("💡 Conseil : Ouvrez les outils de développement (F12) pour voir plus de messages !");
        
        // Fonction 1 : Dire bonjour
        function direBonjour() {
            afficherMessage("👋 Bonjour ! Bienvenue dans le monde merveilleux de JavaScript !");
            console.log("✅ Fonction direBonjour() appelée");
        }
        
        // Fonction 2 : Changer la couleur de fond
        function changerCouleur() {
            const couleurs = ['#ffebee', '#e8f5e8', '#fff3e0', '#f3e5f5', '#e0f2f1'];
            const couleurAleatoire = couleurs[Math.floor(Math.random() * couleurs.length)];
            
            document.body.style.backgroundColor = couleurAleatoire;
            afficherMessage(`🎨 Couleur changée ! Nouvelle couleur : ${couleurAleatoire}`);
            console.log("✅ Couleur de fond changée vers :", couleurAleatoire);
        }
        
        // Fonction 3 : Afficher l'heure actuelle
        function afficherHeure() {
            const maintenant = new Date();
            const heureFormatee = maintenant.toLocaleString('fr-FR');
            
            afficherMessage(`⏰ Il est actuellement : ${heureFormatee}`);
            console.log("✅ Heure affichée :", heureFormatee);
        }
        
        // Fonction 4 : Demander le nom de l'utilisateur
        function demanderNom() {
            const nom = prompt("Comment vous appelez-vous ?");
            
            if (nom && nom.trim() !== "") {
                afficherMessage(`📝 Enchanté de vous rencontrer, ${nom} ! 🤝`);
                console.log("✅ Nom saisi :", nom);
            } else {
                afficherMessage("😅 Vous n'avez pas saisi de nom...");
                console.log("⚠️ Aucun nom saisi");
            }
        }
        
        // Fonction utilitaire : Afficher un message dans la page
        function afficherMessage(texte) {
            const resultat = document.getElementById('resultat');
            const message = document.getElementById('message');
            
            message.textContent = texte;
            resultat.style.display = 'block';
            
            // Petite animation
            resultat.style.opacity = '0';
            setTimeout(() => {
                resultat.style.opacity = '1';
            }, 50);
        }
        
        // Message de fin de chargement
        console.log("✨ Toutes les fonctions sont prêtes à être utilisées !");
    </script>
</body>
</html>
```
{% endtab %}

{% tab title="Étapes d'apprentissage" %}
**Ce que fait ce code :**

1. **Structure HTML** : Page avec boutons et zone d'affichage
2. **Style CSS** : Apparence moderne et responsive
3. **JavaScript** : 4 fonctions interactives

**Concepts illustrés :**

🎯 **Variables et fonctions**
```javascript
const couleurs = ['#ffebee', '#e8f5e8'];  // Array
const maintenant = new Date();             // Object
```

🎯 **Manipulation du DOM**
```javascript
document.getElementById('resultat');       // Sélection
element.style.backgroundColor = couleur;   // Modification
```

🎯 **Interaction utilisateur**
```javascript
onclick="direBonjour()";                   // Événement
const nom = prompt("Comment vous appelez-vous ?"); // Input
```

🎯 **Console et débogage**
```javascript
console.log("Message de débogage");       // Affichage console
```
{% endtab %}

{% tab title="Comment tester" %}
**Étapes pour tester votre premier programme :**

1. **Créer le fichier**
   - Ouvrez un éditeur de texte (Notepad++, VS Code...)
   - Copiez le code complet
   - Sauvegardez sous `mon-premier-javascript.html`

2. **Ouvrir dans le navigateur**
   - Double-cliquez sur le fichier
   - Ou faites `Ctrl+O` dans votre navigateur

3. **Tester les fonctionnalités**
   - Cliquez sur chaque bouton
   - Observez les changements sur la page

4. **Explorer la console**
   - Appuyez sur `F12` (outils développeur)
   - Allez dans l'onglet "Console"
   - Observez les messages JavaScript

**Résultats attendus :**
- ✅ Boutons fonctionnels
- ✅ Messages affichés
- ✅ Couleur de fond qui change
- ✅ Messages dans la console
{% endtab %}
{% endtabs %}

## 🧪 Exercices pratiques

### 🎯 Exercice 1 : Vos premiers messages

{% hint style="info" %}
**Objectif :** Apprendre à afficher des messages dans la console
{% endhint %}

```javascript
// Dans la console du navigateur, tapez ces commandes une par une :

// 1. Afficher un message simple
console.log("Hello JavaScript !");

// 2. Afficher votre nom
console.log("Je m'appelle [VOTRE NOM]");

// 3. Faire un calcul simple
console.log(5 + 3);

// 4. Afficher plusieurs choses à la fois
console.log("Résultat :", 10 - 4);
```

<details>
<summary>💡 À essayer</summary>

```javascript
// Testez ces exemples dans la console :

console.log("🎉 Bienvenue en JavaScript !");
console.log("2 + 2 =", 2 + 2);
console.log("Mon âge :", 25);
console.log("JavaScript", "est", "génial", "!");

// Essayez aussi :
console.log("Ma première" + " " + "phrase concaténée");
console.log(365 * 24); // Nombre d'heures dans une année
```

</details>

### 🎯 Exercice 2 : Jouer avec les calculs

{% hint style="info" %}
**Objectif :** Utiliser JavaScript comme une calculatrice
{% endhint %}

```javascript
// Tapez ces calculs dans la console :

// Additions
console.log("10 + 5 =", 10 + 5);

// Soustractions  
console.log("20 - 8 =", 20 - 8);

// Multiplications
console.log("6 × 7 =", 6 * 7);

// Divisions
console.log("15 ÷ 3 =", 15 / 3);
```

<details>
<summary>💡 Exercices bonus</summary>

```javascript
// Calculez votre âge en jours (approximatif) :
console.log("Mon âge en jours :", 25 * 365);

// Calculez le périmètre d'un carré de côté 5 :
console.log("Périmètre du carré :", 4 * 5);

// Calculez l'aire d'un rectangle 8×6 :
console.log("Aire du rectangle :", 8 * 6);

// Combien font 2 puissance 8 ?
console.log("2 puissance 8 :", 2 * 2 * 2 * 2 * 2 * 2 * 2 * 2);

// Messages colorés (bonus fun) :
console.log("%cMessage en rouge !", "color: red; font-size: 20px;");
console.log("%cMessage en bleu !", "color: blue; font-weight: bold;");
```

</details>

### 🎯 Exercice 3 : Explorer la console

{% hint style="info" %}
**Objectif :** Découvrir les différents types de messages
{% endhint %}

```javascript
// Testez ces différentes commandes :

// Message normal
console.log("ℹ️ Ceci est une information");

// Avertissement
console.warn("⚠️ Ceci est un avertissement");

// Erreur (ne casse rien, c'est juste pour voir)
console.error("❌ Ceci est une erreur (factice)");

// Information
console.info("📋 Information détaillée");
```

<details>
<summary>💡 Plus d'explorations</summary>

```javascript
// Grouper des messages :
console.group("🔵 Mes calculs");
console.log("2 + 2 =", 4);
console.log("3 × 3 =", 9);
console.log("10 / 2 =", 5);
console.groupEnd();

// Messages avec émojis :
console.log("🚀 Décollage imminent !");
console.log("🎯 Mission accomplie !");
console.log("🎉 Bravo, vous maîtrisez la console !");

// Tester le titre de la page :
console.log("Le titre de cette page est :", document.title);
```

</details>

## 🔧 Outils de développement

### 🛠️ Console du navigateur

{% hint style="success" %}
**La console est votre meilleur ami pour apprendre JavaScript !**

**Accès rapide :** `F12` → Onglet "Console"
{% endhint %}

{% tabs %}
{% tab title="Commandes de base" %}
```javascript
// Affichage simple
console.log("Hello World!");

// Avec variables
let nom = "Alice";
let age = 25;
console.log("Nom:", nom, "Age:", age);

// Messages colorés
console.log("%cMessage en rouge", "color: red; font-size: 20px;");
console.log("%cMessage en bleu", "color: blue; font-weight: bold;");

// Types de messages
console.info("ℹ️ Information");
console.warn("⚠️ Avertissement");
console.error("❌ Erreur");

// Groupes
console.group("Mes tests");
console.log("Test 1");
console.log("Test 2");
console.groupEnd();

// Tables
console.table([
    { nom: "Alice", age: 25 },
    { nom: "Bob", age: 30 },
    { nom: "Charlie", age: 35 }
]);
```
{% endtab %}

{% tab title="Débogage" %}
```javascript
// Variables et objets
let utilisateur = { nom: "Alice", age: 25, ville: "Paris" };
console.log("Utilisateur:", utilisateur);

// Inspection d'éléments DOM
console.log("Body:", document.body);
console.log("Titre:", document.title);

// Temps d'exécution
console.time("Mon timer");
// ... code à mesurer ...
console.timeEnd("Mon timer");

// Pile d'exécution
function fonction1() {
    fonction2();
}
function fonction2() {
    console.trace("Où suis-je ?");
}
fonction1();

// Conditions
let x = 5;
console.assert(x === 10, "x devrait être égal à 10");
```
{% endtab %}

{% tab title="Tests interactifs" %}
```javascript
// Tester directement dans la console

// Variables
let message = "Bonjour JavaScript !";
console.log(message);

// Fonctions
function calculer(a, b) {
    return a + b;
}
calculer(5, 3); // Affiche 8

// DOM
document.title = "Nouveau titre";
document.body.style.backgroundColor = "lightblue";

// Boucles
for (let i = 1; i <= 5; i++) {
    console.log("Compteur:", i);
}

// Objets
let personne = {
    nom: "Alice",
    saluer() {
        return `Bonjour, je suis ${this.nom}`;
    }
};
personne.saluer();
```
{% endtab %}
{% endtabs %}

## 🎯 Quiz de validation

{% hint style="info" %}
**Question 1 :** Quelle est la différence entre JavaScript et Java ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**Ce sont deux langages complètement différents :**

**JavaScript :**
- 🌐 Conçu pour le web
- 🏃‍♂️ Interprété (exécution directe)
- 📝 Syntaxe flexible, typé dynamiquement
- 🎯 Spécialisé dans l'interactivité web

**Java :**
- 🏢 Conçu pour les applications d'entreprise
- ⚙️ Compilé (nécessite compilation)
- 📋 Syntaxe stricte, typé statiquement
- 🎯 Spécialisé dans les applications robustes

</details>

{% hint style="info" %}
**Question 2 :** Où peut s'exécuter JavaScript ?
{% endhint %}

<details>
<summary>💡 Réponse</summary>

**JavaScript peut s'exécuter dans plusieurs environnements :**

- 💻 **Navigateurs** : Chrome, Firefox, Safari, Edge
- 🖥️ **Serveurs** : Node.js
- 📱 **Applications mobiles** : React Native, Ionic
- 🖥️ **Applications desktop** : Electron
- 🤖 **IoT** : Johnny-Five, Espruino
- ⚡ **Microcontrôleurs** : Arduino, Raspberry Pi

</details>

## 🔗 Liens avec d'autres concepts

{% content-ref url="variables-declarations.md" %}
[variables-declarations.md](variables-declarations.md)
{% endcontent-ref %}

{% content-ref url="types-donnees.md" %}
[types-donnees.md](types-donnees.md)
{% endcontent-ref %}

{% content-ref url="../niveau-intermediaire/dom-manipulation.md" %}
[dom-manipulation.md](../niveau-intermediaire/dom-manipulation.md)
{% endcontent-ref %}

## 🚀 Prochaines étapes

{% hint style="success" %}
**Félicitations ! Vous avez fait vos premiers pas en JavaScript.**

👉 **Continuez avec** : [Variables et Déclarations](variables-declarations.md)  
👉 **Puis explorez** : [Types de Données](types-donnees.md)  
👉 **Et découvrez** : [Structures de Contrôle](structures-controle.md)  
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🚀 **JavaScript = interactivité** sur le web  
🌐 **Universel** : navigateurs, serveurs, mobile, desktop  
🔄 **Interprété** : exécution immédiate, pas de compilation  
🎯 **Typé dynamiquement** : flexibilité maximale  
🛠️ **Console** : votre outil de débogage principal  
💡 **Premier langage idéal** pour débuter la programmation  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Découvrons maintenant les [Variables et Déclarations](variables-declarations.md) !
{% endhint %}
