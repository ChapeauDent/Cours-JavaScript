# Introduction à JavaScript

## **INFORMATIONS GENERALES**

### Metadonnées du cours
- **Titre :** Introduction à JavaScript - Premiers pas dans la programmation web
- **Niveau :** Débutant (niveau zéro)
- **Durée estimée :** 90 minutes
- **Prérequis :** Notions de base HTML/CSS recommandées
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est JavaScript et son rôle dans le développement web
- [ ] **Identifier** : Les différents environnements d'exécution de JavaScript
- [ ] **Configurer** : Un environnement de développement simple
- [ ] **Écrire** : Votre premier programme JavaScript
- [ ] **Utiliser** : La console du navigateur et les outils de développement

### Mots-clés à retenir
- **JavaScript** : Langage de programmation pour le web
- **Console** : Interface de débogage dans le navigateur
- **Script** : Fichier contenant du code JavaScript
- **DOM** : Document Object Model (structure de la page web)
- **Interprété** : Langage exécuté ligne par ligne sans compilation

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
JavaScript est LE langage de programmation du web. Contrairement à HTML (structure) et CSS (style), JavaScript apporte l'**interactivité** et la **logique** aux pages web. C'est le seul langage que comprennent nativement tous les navigateurs, ce qui en fait un passage obligé pour tout développeur web.

### Qu'est-ce que JavaScript ?

#### **1. Définition simple**
JavaScript est un langage de programmation qui permet de :
- Rendre les pages web **interactives** (boutons, formulaires, animations)
- **Communiquer** avec des serveurs (récupérer des données)
- Créer des **applications complètes** (web, mobile, desktop)

#### **2. JavaScript ≠ Java**
```
❌ IDÉE FAUSSE : "JavaScript est une version simplifiée de Java"
✅ RÉALITÉ : Ce sont deux langages complètement différents !

Java        → Langage pour applications d'entreprise
JavaScript  → Langage pour applications web
```

#### **3. Les trois piliers du web**
```html
<!-- HTML : Structure (le squelette) -->
<button id="monBouton">Cliquez-moi</button>

<!-- CSS : Style (l'apparence) -->
<style>
#monBouton {
    background-color: blue;
    color: white;
}
</style>

<!-- JavaScript : Comportement (l'intelligence) -->
<script>
document.getElementById('monBouton').onclick = function() {
    alert('Bonjour le monde !');
};
</script>
```

### Où s'exécute JavaScript ?

#### **1. Dans le navigateur (côté client)**
- Manipulation des pages web
- Interactions utilisateur
- Animations et effets visuels

#### **2. Sur le serveur (côté serveur)**
- Node.js pour créer des serveurs
- APIs et bases de données
- Applications en temps réel

#### **3. Autres environnements**
- Applications mobiles (React Native)
- Applications desktop (Electron)
- IoT et objets connectés

### Comment JavaScript fonctionne-t-il ?

#### **Interprété en temps réel**
```javascript
// JavaScript est lu et exécuté ligne par ligne
console.log("Première ligne");    // ← Exécutée immédiatement
console.log("Deuxième ligne");    // ← Puis celle-ci
console.log("Troisième ligne");   // ← Enfin celle-ci
```

#### **Typé dynamiquement**
```javascript
// Les variables peuvent changer de type
let maVariable = "Bonjour";       // String
maVariable = 42;                  // Maintenant c'est un Number
maVariable = true;                // Maintenant c'est un Boolean
```

### Syntaxe de base
```javascript
// === COMMENTAIRES ===
// Ceci est un commentaire sur une ligne

/* 
   Ceci est un commentaire
   sur plusieurs lignes
*/

// === AFFICHAGE ===
console.log("Hello World!");     // Affiche dans la console
alert("Message à l'utilisateur"); // Popup dans le navigateur

// === VARIABLES ===
let nom = "Alice";               // Variable modifiable
const age = 25;                  // Constante (non modifiable)

// === FONCTIONS SIMPLES ===
function direBonjour() {
    console.log("Bonjour !");
}

direBonjour(); // Appel de la fonction
```

### Points d'attention pour débutants
- **Sensible à la casse** : `maVariable` ≠ `mavariable`
- **Point-virgule optionnel** : Bonne pratique de les mettre
- **Guillemets flexibles** : `"texte"` ou `'texte'` (mais être cohérent)
- **Console = votre amie** : Utilisez `console.log()` pour tout tester

---

## **PARTIE PRATIQUE**

### Exemple starter : Premier contact
```html
<!DOCTYPE html>
<html>
<head>
    <title>Mon premier JavaScript</title>
</head>
<body>
    <h1>Ma première page interactive</h1>
    <button onclick="direBonjour()">Dire bonjour</button>
    
    <script>
        // JavaScript intégré dans la page
        function direBonjour() {
            alert("Bonjour ! Bienvenue dans le monde de JavaScript !");
        }
        
        // Un message dans la console dès le chargement
        console.log("La page est chargée !");
        console.log("Ouvrez les outils de développement pour voir ce message");
    </script>
</body>
</html>
```

**Résultat attendu :**
- Une page avec un bouton
- Clic sur le bouton → popup "Bonjour ! Bienvenue..."
- Dans la console → "La page est chargée !"

### Exemple intermédiaire : Calculatrice simple
```html
<!DOCTYPE html>
<html>
<head>
    <title>Calculatrice JavaScript</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        .calculatrice { background: #f0f0f0; padding: 20px; border-radius: 10px; width: 300px; }
        input { margin: 5px; padding: 10px; font-size: 16px; }
        button { padding: 10px 20px; font-size: 16px; background: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer; }
        button:hover { background: #0056b3; }
    </style>
</head>
<body>
    <div class="calculatrice">
        <h2>🧮 Ma première calculatrice</h2>
        
        <input type="number" id="nombre1" placeholder="Premier nombre">
        <input type="number" id="nombre2" placeholder="Deuxième nombre">
        
        <br><br>
        
        <button onclick="additionner()">➕ Addition</button>
        <button onclick="soustraire()">➖ Soustraction</button>
        <button onclick="multiplier()">✖️ Multiplication</button>
        <button onclick="diviser()">➗ Division</button>
        
        <br><br>
        
        <div id="resultat" style="font-size: 18px; font-weight: bold; color: #007bff;">
            Résultat apparaîtra ici
        </div>
    </div>

    <script>
        // === FONCTIONS DE CALCUL ===
        
        function obtenirNombres() {
            // Récupérer les valeurs des champs
            let nombre1 = document.getElementById('nombre1').value;
            let nombre2 = document.getElementById('nombre2').value;
            
            // Les convertir en nombres (ils arrivent en texte)
            nombre1 = Number(nombre1);
            nombre2 = Number(nombre2);
            
            console.log("Nombre 1:", nombre1);
            console.log("Nombre 2:", nombre2);
            
            return [nombre1, nombre2];
        }
        
        function afficherResultat(resultat) {
            // Trouver l'élément pour afficher le résultat
            let elementResultat = document.getElementById('resultat');
            elementResultat.textContent = "Résultat : " + resultat;
            
            console.log("Résultat affiché:", resultat);
        }
        
        function additionner() {
            let [a, b] = obtenirNombres();
            let resultat = a + b;
            afficherResultat(resultat);
        }
        
        function soustraire() {
            let [a, b] = obtenirNombres();
            let resultat = a - b;
            afficherResultat(resultat);
        }
        
        function multiplier() {
            let [a, b] = obtenirNombres();
            let resultat = a * b;
            afficherResultat(resultat);
        }
        
        function diviser() {
            let [a, b] = obtenirNombres();
            
            // Vérification division par zéro
            if (b === 0) {
                afficherResultat("Erreur : Division par zéro impossible !");
                return;
            }
            
            let resultat = a / b;
            afficherResultat(resultat);
        }
        
        // === MESSAGE DE BIENVENUE ===
        console.log("🚀 Calculatrice JavaScript chargée !");
        console.log("💡 Astuce : Ouvrez la console pour voir les calculs détaillés");
    </script>
</body>
</html>
```

### Exemple avancé : Générateur de citation
```html
<!DOCTYPE html>
<html>
<head>
    <title>Générateur de Citations</title>
    <style>
        body { 
            font-family: Georgia, serif; 
            max-width: 600px; 
            margin: 50px auto; 
            padding: 20px; 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            min-height: 100vh;
        }
        .container { 
            background: rgba(255,255,255,0.1); 
            padding: 30px; 
            border-radius: 15px; 
            backdrop-filter: blur(10px);
        }
        .citation { 
            font-size: 24px; 
            font-style: italic; 
            margin: 30px 0; 
            padding: 20px;
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
            border-left: 5px solid #ffd700;
        }
        .auteur { 
            text-align: right; 
            font-size: 18px; 
            color: #ffd700; 
        }
        button { 
            padding: 15px 30px; 
            font-size: 18px; 
            background: #ffd700; 
            color: #333; 
            border: none; 
            border-radius: 25px; 
            cursor: pointer; 
            transition: all 0.3s;
        }
        button:hover { 
            background: #ffed4e; 
            transform: translateY(-2px);
        }
        .stats {
            margin-top: 20px;
            font-size: 14px;
            opacity: 0.8;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>✨ Générateur de Citations Inspirantes</h1>
        
        <div class="citation" id="citationTexte">
            "Cliquez sur le bouton pour découvrir une citation inspirante !"
        </div>
        
        <div class="auteur" id="citationAuteur">
            — Votre motivation quotidienne
        </div>
        
        <br>
        
        <button onclick="genererCitation()">🎲 Nouvelle Citation</button>
        
        <div class="stats" id="statistiques">
            Citations générées : 0
        </div>
    </div>

    <script>
        // === BASE DE DONNÉES DE CITATIONS ===
        let citations = [
            {
                texte: "La seule façon de faire du bon travail est d'aimer ce que vous faites.",
                auteur: "Steve Jobs"
            },
            {
                texte: "L'innovation distingue un leader d'un suiveur.",
                auteur: "Steve Jobs"
            },
            {
                texte: "Le code est comme l'humour. Quand vous devez l'expliquer, c'est mauvais.",
                auteur: "Cory House"
            },
            {
                texte: "D'abord, résolvez le problème. Ensuite, écrivez le code.",
                auteur: "John Johnson"
            },
            {
                texte: "Programmer, c'est l'art de dire à un autre humain ce que vous voulez que l'ordinateur fasse.",
                auteur: "Donald Knuth"
            },
            {
                texte: "La perfection est atteinte non pas lorsqu'il n'y a plus rien à ajouter, mais lorsqu'il n'y a plus rien à retirer.",
                auteur: "Antoine de Saint-Exupéry"
            },
            {
                texte: "Un expert est quelqu'un qui a fait toutes les erreurs possibles dans un domaine très restreint.",
                auteur: "Niels Bohr"
            },
            {
                texte: "L'expérience est le nom que chacun donne à ses erreurs.",
                auteur: "Oscar Wilde"
            }
        ];
        
        // === VARIABLES GLOBALES ===
        let compteurCitations = 0;
        let derniereIndexCitation = -1;
        
        // === FONCTION PRINCIPALE ===
        function genererCitation() {
            console.log("🎲 Génération d'une nouvelle citation...");
            
            // Éviter la même citation deux fois de suite
            let indexAleatoire;
            do {
                indexAleatoire = Math.floor(Math.random() * citations.length);
            } while (indexAleatoire === derniereIndexCitation && citations.length > 1);
            
            derniereIndexCitation = indexAleatoire;
            
            // Récupérer la citation choisie
            let citationChoisie = citations[indexAleatoire];
            
            console.log("📖 Citation choisie:", citationChoisie.texte);
            console.log("👤 Auteur:", citationChoisie.auteur);
            
            // Afficher dans la page
            document.getElementById('citationTexte').textContent = '"' + citationChoisie.texte + '"';
            document.getElementById('citationAuteur').textContent = '— ' + citationChoisie.auteur;
            
            // Mettre à jour les statistiques
            compteurCitations++;
            mettreAJourStatistiques();
        }
        
        function mettreAJourStatistiques() {
            let messageStats = `Citations générées : ${compteurCitations}`;
            
            if (compteurCitations >= 5) {
                messageStats += " 🔥 Vous êtes motivé(e) !";
            }
            
            document.getElementById('statistiques').textContent = messageStats;
        }
        
        // === INITIALISATION ===
        console.log("✨ Générateur de citations initialisé !");
        console.log(`📚 ${citations.length} citations disponibles`);
        console.log("💡 Astuce : Utilisez la console pour voir les détails des citations générées");
        
        // Générer une première citation automatiquement
        genererCitation();
    </script>
</body>
</html>
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Mon premier script
**Consigne :** Créez une page qui affiche votre nom et votre âge, calculé automatiquement selon votre année de naissance.

**Code de départ :**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Qui suis-je ?</title>
</head>
<body>
    <h1>Présentation</h1>
    <div id="presentation"></div>
    
    <script>
        // TODO: Créer des variables pour votre nom et année de naissance
        // TODO: Calculer votre âge
        // TODO: Afficher le tout dans la div "presentation"
    </script>
</body>
</html>
```

**Résultat attendu :**
"Je m'appelle [Votre nom] et j'ai [votre âge] ans."

**Niveau de difficulté :** ⭐☆☆☆☆

### Exercice 2 : Compteur interactif
**Consigne :** Créez un compteur avec des boutons + et - et un bouton reset.

**Contraintes :**
- Le compteur ne peut pas descendre en dessous de 0
- Afficher une couleur différente selon la valeur (rouge si 0, vert si positif)

**Niveau de difficulté :** ⭐⭐☆☆☆

### Exercice 3 : Devinette de nombre
**Consigne :** L'ordinateur choisit un nombre entre 1 et 10, l'utilisateur doit deviner.

**Contraintes :**
- Indiquer si le nombre est trop grand ou trop petit
- Compter le nombre d'essais
- Proposer de rejouer

**Niveau de difficulté :** ⭐⭐⭐☆☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```html
<!DOCTYPE html>
<html>
<head>
    <title>Qui suis-je ?</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        .presentation { 
            background: #e3f2fd; 
            padding: 20px; 
            border-radius: 10px; 
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>✨ Présentation</h1>
    <div id="presentation" class="presentation"></div>
    
    <script>
        // Variables personnelles
        let monNom = "Alice Dupont";  // ← Changez par votre nom
        let anneeNaissance = 1995;    // ← Changez par votre année de naissance
        
        // Calcul de l'âge
        let anneeActuelle = new Date().getFullYear(); // 2025
        let monAge = anneeActuelle - anneeNaissance;
        
        console.log("Année actuelle:", anneeActuelle);
        console.log("Année de naissance:", anneeNaissance);
        console.log("Âge calculé:", monAge);
        
        // Création du message
        let message = `Je m'appelle ${monNom} et j'ai ${monAge} ans.`;
        
        // Affichage dans la page
        document.getElementById('presentation').textContent = message;
        
        // Message bonus dans la console
        console.log("👋 " + message);
    </script>
</body>
</html>
```

### Solution Exercice 2
```html
<!DOCTYPE html>
<html>
<head>
    <title>Compteur Interactif</title>
    <style>
        body { 
            font-family: Arial; 
            text-align: center; 
            padding: 50px; 
            background: #f5f5f5;
        }
        .compteur-container { 
            background: white; 
            padding: 30px; 
            border-radius: 15px; 
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            display: inline-block;
        }
        .compteur { 
            font-size: 72px; 
            font-weight: bold; 
            margin: 20px 0; 
            transition: color 0.3s;
        }
        .rouge { color: #dc3545; }
        .vert { color: #28a745; }
        button { 
            font-size: 24px; 
            padding: 15px 25px; 
            margin: 0 10px; 
            border: none; 
            border-radius: 50%; 
            cursor: pointer; 
            transition: transform 0.2s;
        }
        button:hover { transform: scale(1.1); }
        .plus { background: #28a745; color: white; }
        .moins { background: #dc3545; color: white; }
        .reset { background: #ffc107; color: black; padding: 10px 20px; border-radius: 25px; }
    </style>
</head>
<body>
    <div class="compteur-container">
        <h1>🔢 Compteur Interactif</h1>
        
        <div class="compteur" id="affichageCompteur">0</div>
        
        <button class="moins" onclick="decrementer()">−</button>
        <button class="plus" onclick="incrementer()">+</button>
        
        <br><br>
        
        <button class="reset" onclick="remettrAZero()">🔄 Reset</button>
    </div>

    <script>
        // === VARIABLE GLOBALE ===
        let compteur = 0;
        
        // === FONCTION D'AFFICHAGE ===
        function mettreAJourAffichage() {
            let elementCompteur = document.getElementById('affichageCompteur');
            elementCompteur.textContent = compteur;
            
            // Changer la couleur selon la valeur
            elementCompteur.className = 'compteur ' + (compteur === 0 ? 'rouge' : 'vert');
            
            console.log("Compteur mis à jour:", compteur);
        }
        
        // === FONCTIONS DE CONTRÔLE ===
        function incrementer() {
            compteur++;
            mettreAJourAffichage();
            console.log("➕ Incrémentation:", compteur);
        }
        
        function decrementer() {
            // Ne pas descendre en dessous de 0
            if (compteur > 0) {
                compteur--;
                mettreAJourAffichage();
                console.log("➖ Décrémentation:", compteur);
            } else {
                console.log("⚠️ Impossible de descendre en dessous de 0");
            }
        }
        
        function remettrAZero() {
            compteur = 0;
            mettreAJourAffichage();
            console.log("🔄 Remise à zéro");
        }
        
        // === INITIALISATION ===
        console.log("🔢 Compteur interactif initialisé !");
        mettreAJourAffichage(); // Affichage initial
    </script>
</body>
</html>
```

### Solution Exercice 3
```html
<!DOCTYPE html>
<html>
<head>
    <title>Devinette de Nombre</title>
    <style>
        body { 
            font-family: Arial; 
            max-width: 500px; 
            margin: 50px auto; 
            padding: 20px; 
            background: linear-gradient(135deg, #74b9ff 0%, #0984e3 100%);
            color: white;
            min-height: 100vh;
        }
        .jeu-container { 
            background: rgba(255,255,255,0.1); 
            padding: 30px; 
            border-radius: 15px; 
            backdrop-filter: blur(10px);
        }
        input { 
            padding: 10px; 
            font-size: 18px; 
            border: none; 
            border-radius: 5px; 
            margin: 10px 5px;
            width: 100px;
            text-align: center;
        }
        button { 
            padding: 10px 20px; 
            font-size: 16px; 
            background: #00b894; 
            color: white; 
            border: none; 
            border-radius: 5px; 
            cursor: pointer; 
            margin: 5px;
        }
        button:hover { background: #00a085; }
        .message { 
            margin: 20px 0; 
            padding: 15px; 
            border-radius: 10px; 
            font-size: 18px;
        }
        .victoire { background: rgba(0, 184, 148, 0.3); }
        .indice { background: rgba(255, 193, 7, 0.3); }
        .erreur { background: rgba(220, 53, 69, 0.3); }
        .stats { 
            margin-top: 20px; 
            font-size: 14px; 
            opacity: 0.8; 
        }
    </style>
</head>
<body>
    <div class="jeu-container">
        <h1>🎯 Devinette de Nombre</h1>
        <p>J'ai choisi un nombre entre 1 et 10. À vous de le deviner !</p>
        
        <input type="number" id="propositionJoueur" min="1" max="10" placeholder="?">
        <button onclick="verifierProposition()">Deviner</button>
        
        <div class="message" id="zoneMessage">
            Entrez votre première proposition !
        </div>
        
        <button onclick="nouveauJeu()" id="boutonNouveauJeu" style="display: none;">
            🎲 Nouveau Jeu
        </button>
        
        <div class="stats" id="statistiques">
            Essais : 0 | Parties gagnées : 0
        </div>
    </div>

    <script>
        // === VARIABLES DU JEU ===
        let nombreMystere;
        let nombreEssais;
        let partiesGagnees = 0;
        let jeuEnCours;
        
        // === INITIALISATION DU JEU ===
        function nouveauJeu() {
            // Générer un nombre aléatoire entre 1 et 10
            nombreMystere = Math.floor(Math.random() * 10) + 1;
            nombreEssais = 0;
            jeuEnCours = true;
            
            console.log("🎲 Nouveau jeu commencé");
            console.log("🤫 Nombre mystère:", nombreMystere, "(chut, c'est secret !)");
            
            // Réinitialiser l'interface
            document.getElementById('zoneMessage').textContent = "Entrez votre première proposition !";
            document.getElementById('zoneMessage').className = "message";
            document.getElementById('propositionJoueur').value = "";
            document.getElementById('propositionJoueur').disabled = false;
            document.getElementById('boutonNouveauJeu').style.display = "none";
            
            mettreAJourStats();
        }
        
        // === FONCTION PRINCIPALE ===
        function verifierProposition() {
            if (!jeuEnCours) return;
            
            let proposition = document.getElementById('propositionJoueur').value;
            let messageElement = document.getElementById('zoneMessage');
            
            // Vérifications de base
            if (!proposition) {
                messageElement.textContent = "⚠️ Veuillez entrer un nombre !";
                messageElement.className = "message erreur";
                return;
            }
            
            proposition = Number(proposition);
            
            if (proposition < 1 || proposition > 10) {
                messageElement.textContent = "⚠️ Le nombre doit être entre 1 et 10 !";
                messageElement.className = "message erreur";
                return;
            }
            
            // Compter l'essai
            nombreEssais++;
            console.log(`Essai ${nombreEssais}: ${proposition} (mystère: ${nombreMystere})`);
            
            // Vérifier la proposition
            if (proposition === nombreMystere) {
                // VICTOIRE !
                partiesGagnees++;
                jeuEnCours = false;
                
                let messageFelicitations;
                if (nombreEssais === 1) {
                    messageFelicitations = "🎉 INCROYABLE ! Du premier coup !";
                } else if (nombreEssais <= 3) {
                    messageFelicitations = `🎉 BRAVO ! Trouvé en ${nombreEssais} essais !`;
                } else {
                    messageFelicitations = `🎉 Félicitations ! Trouvé en ${nombreEssais} essais !`;
                }
                
                messageElement.textContent = messageFelicitations;
                messageElement.className = "message victoire";
                
                document.getElementById('propositionJoueur').disabled = true;
                document.getElementById('boutonNouveauJeu').style.display = "inline-block";
                
                console.log("🎉 Victoire en", nombreEssais, "essais !");
                
            } else if (proposition < nombreMystere) {
                // Trop petit
                messageElement.textContent = `📈 ${proposition} est trop petit ! Essayez plus grand.`;
                messageElement.className = "message indice";
                
            } else {
                // Trop grand
                messageElement.textContent = `📉 ${proposition} est trop grand ! Essayez plus petit.`;
                messageElement.className = "message indice";
            }
            
            // Vider le champ pour la prochaine tentative
            document.getElementById('propositionJoueur').value = "";
            mettreAJourStats();
        }
        
        // === MISE À JOUR DES STATISTIQUES ===
        function mettreAJourStats() {
            document.getElementById('statistiques').textContent = 
                `Essais : ${nombreEssais} | Parties gagnées : ${partiesGagnees}`;
        }
        
        // === GESTION CLAVIER ===
        document.getElementById('propositionJoueur').addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                verifierProposition();
            }
        });
        
        // === DÉMARRAGE AUTOMATIQUE ===
        console.log("🎯 Jeu de devinette initialisé !");
        nouveauJeu(); // Commencer automatiquement
    </script>
</body>
</html>
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Prochaines étapes recommandées
1. **Variables et déclarations** → Maîtriser let, const, var
2. **Types de données** → Numbers, strings, booleans
3. **Fonctions** → Créer du code réutilisable
4. **DOM** → Manipuler les éléments de la page

### Concepts complémentaires
- **HTML/CSS** : Base indispensable pour le développement web
- **Outils de développement** : Console, débogueur, sources
- **Éditeurs de code** : VS Code, Sublime Text, etc.

---

## **CONFIGURATION ENVIRONNEMENT**

### Outils recommandés pour débuter
1. **Navigateur moderne** (Chrome, Firefox, Edge)
2. **Éditeur de texte** (Notepad++, VS Code, Sublime Text)
3. **Console du navigateur** (F12)

### Comment tester vos scripts
```javascript
// 1. Dans la console du navigateur (F12)
console.log("Hello World!");

// 2. Dans un fichier HTML
/*
<!DOCTYPE html>
<html>
<head><title>Test</title></head>
<body>
    <script>
        console.log("Hello World!");
    </script>
</body>
</html>
*/

// 3. Dans un fichier .js séparé
// Créer script.js, puis l'inclure avec :
// <script src="script.js"></script>
```

---

## **PROJET MINI**

### Défi pratique : Portfolio interactif simple
**Objectif :** Créer une page de présentation personnelle avec JavaScript

**Fonctionnalités requises :**
- [ ] Affichage de votre nom et âge calculé automatiquement
- [ ] Bouton pour changer la couleur de fond aléatoirement
- [ ] Compteur de visiteurs (simulé)
- [ ] Horloge en temps réel
- [ ] Message personnalisé selon l'heure de la journée

**Temps estimé :** 60 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Sites d'apprentissage
- [MDN JavaScript Guide](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide) (référence officielle)
- [JavaScript.info](https://javascript.info/) (tutoriel complet)
- [freeCodeCamp](https://www.freecodecamp.org/) (exercices pratiques)

### Outils en ligne
- [CodePen](https://codepen.io/) (éditeur en ligne)
- [JSFiddle](https://jsfiddle.net/) (test rapide)
- [Repl.it](https://replit.com/) (environnement complet)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Quelle est la différence entre HTML, CSS et JavaScript ?**
   - Réponse : HTML = structure, CSS = style, JavaScript = comportement/logique

2. **Comment afficher un message dans la console ?**
   - Réponse : `console.log("mon message")`

3. **JavaScript et Java sont-ils la même chose ?**
   - Réponse : Non, ce sont deux langages complètement différents

### Checklist de maîtrise
- [ ] Je sais ce qu'est JavaScript et son rôle
- [ ] Je peux ouvrir la console du navigateur
- [ ] Je sais écrire du JavaScript simple dans une page HTML
- [ ] Je comprends les bases de la syntaxe (variables, fonctions)
- [ ] Je peux déboguer avec console.log()

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 : Script ne s'exécute pas
**Symptôme :** Rien ne se passe, pas de message d'erreur
```html
<!-- ❌ Script mal placé -->
<script>
    document.getElementById('monId').textContent = "Hello";
</script>
<div id="monId">Texte</div>

<!-- ✅ Solution : placer le script après l'élément -->
<div id="monId">Texte</div>
<script>
    document.getElementById('monId').textContent = "Hello";
</script>
```

### Erreur fréquente 2 : Syntaxe incorrecte
**Code problématique :**
```javascript
consol.log("Hello"); // ❌ Faute de frappe
Console.log("Hello"); // ❌ Mauvaise casse
```

**Solution :**
```javascript
console.log("Hello"); // ✅ Correct
```

---

## **NOTES PERSONNELLES**

### Ce que j'ai découvert aujourd'hui
- JavaScript rend les pages web interactives
- La console est un outil précieux pour déboguer
- Importance de l'ordre d'exécution du code

### Questions à approfondir
- Comment organiser le code JavaScript dans de gros projets ?
- Quelles sont les bonnes pratiques de nommage ?

### Révision programmée
- [ ] Révision dans 1 jour (refaire les exemples)
- [ ] Révision dans 1 semaine (créer un petit projet)
- [ ] Révision dans 1 mois (approfondir avec les concepts suivants)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #introduction #debutant #console #premier-programme

**Temps de completion :** [Votre temps réel]

**Difficulté ressentie :** ⭐☆☆☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 1/126*
