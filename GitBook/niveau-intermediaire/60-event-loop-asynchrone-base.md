# ⏰ Event Loop et Asynchrone - Les Bases

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- ✅ **Comprendre** comment JavaScript gère les tâches asynchrones
- ✅ **Appliquer** setTimeout et setInterval correctement
- ✅ **Analyser** l'ordre d'exécution du code asynchrone
- ✅ **Créer** des applications avec délais et animations simples

{% hint style="info" %}
**Prérequis :** DOM, Événements, Fonctions, Callbacks de base
**Durée estimée :** 40 minutes
**Niveau :** Intermédiaire
{% endhint %}

## 🧠 Comprendre l'Event Loop

### Pourquoi c'est important ?

L'Event Loop est le **cœur de JavaScript** ! Comprendre ce mécanisme t'aide à :

{% hint style="success" %}
- ✨ Créer des animations et interactions fluides
- 🚫 Éviter que ton code "bloque" le navigateur  
- 🔍 Comprendre pourquoi certains codes s'exécutent "plus tard"
- 🚀 Préparer aux concepts avancés (Promises, async/await)
- 🐛 Déboguer les problèmes de timing
{% endhint %}

### Le secret de JavaScript

{% hint style="warning" %}
**JavaScript est mono-thread** mais peut faire plusieurs choses "en même temps" grâce à l'Event Loop !
{% endhint %}

JavaScript ne peut exécuter qu'**une seule chose à la fois**, mais le navigateur nous aide avec des **Web APIs** pour les tâches longues.

### Mots-clés essentiels

| Terme | Définition | Analogie |
|-------|------------|----------|
| **Event Loop** | Boucle qui gère l'exécution asynchrone | Chef d'orchestre |
| **Call Stack** | Pile d'exécution des fonctions | Pile d'assiettes |
| **Web APIs** | Services du navigateur | Assistants |
| **Task Queue** | File d'attente des tâches | File d'attente |
| **Callback** | Fonction appelée après une opération | Rappel téléphonique |

## 🔄 Visualiser l'Event Loop

### Architecture simplifiée

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│ Call Stack  │    │  Web APIs    │    │ Task Queue  │
│ (Sync code) │◄───┤ (setTimeout, │───►│ (Callbacks  │
│             │    │  DOM events) │    │  ready)     │
└─────────────┘    └──────────────┘    └─────────────┘
       ▲                                       │
       │               Event Loop              │
       └───────────────────────────────────────┘
```

### Processus étape par étape

{% tabs %}
{% tab title="1. Code synchrone" %}
```javascript
console.log("1. Début");
// ↓ Exécuté immédiatement
```
{% endtab %}

{% tab title="2. Code asynchrone" %}
```javascript
setTimeout(() => {
    console.log("3. Dans setTimeout");
}, 0);
// ↓ Délégué aux Web APIs
```
{% endtab %}

{% tab title="3. Retour synchrone" %}
```javascript
console.log("2. Fin");
// ↓ Exécuté immédiatement
```
{% endtab %}

{% tab title="4. Résultat" %}
```
1. Début
2. Fin  
3. Dans setTimeout
```
L'ordre peut surprendre ! 🤔
{% endtab %}
{% endtabs %}

## 💻 Syntaxe et utilisation

### setTimeout - Exécuter avec délai

{% tabs %}
{% tab title="Syntaxe de base" %}
```javascript
// Syntax : setTimeout(callback, délaiEnMs)
setTimeout(() => {
    console.log("Exécuté après 1 seconde");
}, 1000);

// Avec paramètres
setTimeout((nom) => {
    console.log("Bonjour " + nom);
}, 500, "Alice");
```
{% endtab %}

{% tab title="Sauvegarder la référence" %}
```javascript
// Garder la référence pour pouvoir annuler
const timerId = setTimeout(() => {
    console.log("Ce message n'apparaîtra pas");
}, 2000);

// Annuler avant exécution
clearTimeout(timerId);
console.log("Timer annulé !");
```
{% endtab %}
{% endtabs %}

### setInterval - Répéter périodiquement

{% tabs %}
{% tab title="Horloge simple" %}
```javascript
// Afficher l'heure toutes les secondes
const intervalId = setInterval(() => {
    const maintenant = new Date();
    console.log(maintenant.toLocaleTimeString());
}, 1000);

// Arrêter après 5 secondes
setTimeout(() => {
    clearInterval(intervalId);
    console.log("Horloge arrêtée");
}, 5000);
```
{% endtab %}

{% tab title="Compteur avec arrêt automatique" %}
```javascript
let compteur = 0;

const intervalId = setInterval(() => {
    compteur++;
    console.log("Compteur: " + compteur);
    
    // Arrêter à 5
    if (compteur >= 5) {
        clearInterval(intervalId);
        console.log("Compteur terminé !");
    }
}, 1000);
```
{% endtab %}
{% endtabs %}

## 🎮 Exercices interactifs

### 🔮 Exercice 1 : Prédire l'ordre d'exécution

Avant de tester, devine l'ordre d'affichage !

{% tabs %}
{% tab title="🎯 Challenge" %}
```javascript
console.log("A");

setTimeout(() => console.log("B"), 0);

console.log("C");

setTimeout(() => console.log("D"), 100);

console.log("E");
```

**Ta prédiction :** Écris l'ordre dans les commentaires puis teste !
{% endtab %}

{% tab title="🤔 Réflexion" %}
**Questions à se poser :**

1. Quel code est synchrone ?
2. Quel code est asynchrone ?
3. Quel setTimeout sera exécuté en premier ?
4. Pourquoi setTimeout(fn, 0) n'est pas instantané ?
{% endtab %}

{% tab title="✅ Solution" %}
**Ordre correct :** A, C, E, B, D

**Explication :**
- A, C, E : Code synchrone, exécuté immédiatement
- B : setTimeout(0ms) mais passe par l'Event Loop
- D : setTimeout(100ms) donc en dernier

{% hint style="info" %}
**Règle d'or :** Le code synchrone s'exécute TOUJOURS avant l'asynchrone, même avec setTimeout(0) !
{% endhint %}
{% endtab %}
{% endtabs %}

### ⏱️ Exercice 2 : Minuteur interactif

Créons un minuteur complet avec interface !

{% tabs %}
{% tab title="🎯 Objectif" %}
**Fonctionnalités à créer :**
- ⏱️ Compte à rebours format MM:SS
- ▶️ Boutons Start/Pause/Reset
- 📊 Barre de progression visuelle
- 🔔 Alerte à la fin

**Niveau :** ⭐⭐⭐ (3/5)
{% endtab %}

{% tab title="🏗️ Structure de base" %}
```javascript
class Minuteur {
    constructor(dureeEnSecondes) {
        this.duree = dureeEnSecondes;
        this.tempsRestant = dureeEnSecondes;
        this.intervalId = null;
        this.enMarche = false;
    }
    
    demarrer() {
        if (this.enMarche) return;
        // À compléter...
    }
    
    pauseer() {
        // À compléter...
    }
    
    reset() {
        // À compléter...
    }
    
    afficher() {
        // Convertir en MM:SS
        const minutes = Math.floor(this.tempsRestant / 60);
        const secondes = this.tempsRestant % 60;
        return `${minutes.toString().padStart(2, '0')}:${secondes.toString().padStart(2, '0')}`;
    }
}
```
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
class Minuteur {
    constructor(dureeEnSecondes) {
        this.duree = dureeEnSecondes;
        this.tempsRestant = dureeEnSecondes;
        this.intervalId = null;
        this.enMarche = false;
        this.callbacks = { onTick: null, onFini: null };
    }
    
    demarrer() {
        if (this.enMarche) return;
        
        this.enMarche = true;
        console.log("▶️ Minuteur démarré");
        
        this.intervalId = setInterval(() => {
            this.tempsRestant--;
            
            // Callback à chaque seconde
            if (this.callbacks.onTick) {
                this.callbacks.onTick(this.tempsRestant);
            }
            
            console.log("⏱️ " + this.afficher());
            
            // Fin du minuteur
            if (this.tempsRestant <= 0) {
                this.pauseer();
                if (this.callbacks.onFini) {
                    this.callbacks.onFini();
                }
                console.log("⏰ Temps écoulé !");
            }
        }, 1000);
    }
    
    pauseer() {
        if (!this.enMarche) return;
        
        clearInterval(this.intervalId);
        this.intervalId = null;
        this.enMarche = false;
        console.log("⏸️ Minuteur en pause");
    }
    
    reset() {
        this.pauseer();
        this.tempsRestant = this.duree;
        console.log("🔄 Minuteur remis à zéro");
        console.log("⏱️ " + this.afficher());
    }
    
    afficher() {
        const minutes = Math.floor(this.tempsRestant / 60);
        const secondes = this.tempsRestant % 60;
        return `${minutes.toString().padStart(2, '0')}:${secondes.toString().padStart(2, '0')}`;
    }
    
    onTick(callback) { this.callbacks.onTick = callback; }
    onFini(callback) { this.callbacks.onFini = callback; }
    
    obtenirStatut() {
        return {
            tempsRestant: this.tempsRestant,
            enMarche: this.enMarche,
            progression: ((this.duree - this.tempsRestant) / this.duree) * 100
        };
    }
}

// Interface utilisateur
function creerInterfaceMinuteur() {
    const container = document.createElement('div');
    container.style.cssText = `
        text-align: center; font-family: Arial; background: #f0f0f0;
        padding: 20px; border-radius: 10px; margin: 20px auto; width: 300px;
    `;
    
    container.innerHTML = `
        <h3>⏱️ Minuteur Pomodoro</h3>
        <div id="affichage-minuteur" style="font-size: 2.5em; font-family: monospace; margin: 20px 0; color: #333;">25:00</div>
        <div style="margin: 15px 0;">
            <button id="btn-start" style="margin: 5px; padding: 10px 15px; font-size: 16px;">▶️ Start</button>
            <button id="btn-pause" style="margin: 5px; padding: 10px 15px; font-size: 16px;">⏸️ Pause</button>
            <button id="btn-reset" style="margin: 5px; padding: 10px 15px; font-size: 16px;">🔄 Reset</button>
        </div>
        <div id="barre-progression" style="background: #ddd; height: 15px; margin-top: 20px; border-radius: 10px; overflow: hidden;">
            <div id="progression" style="background: linear-gradient(45deg, #4CAF50, #45a049); height: 100%; width: 0%; transition: width 1s ease;"></div>
        </div>
        <div id="status" style="margin-top: 10px; font-size: 14px; color: #666;">Prêt à commencer</div>
    `;
    
    document.body.appendChild(container);
    
    // Créer le minuteur (25 minutes Pomodoro par défaut)
    const minuteur = new Minuteur(25 * 60);
    
    // Éléments DOM
    const affichage = document.getElementById('affichage-minuteur');
    const progression = document.getElementById('progression');
    const status = document.getElementById('status');
    const btnStart = document.getElementById('btn-start');
    const btnPause = document.getElementById('btn-pause');
    const btnReset = document.getElementById('btn-reset');
    
    // Callbacks
    minuteur.onTick((tempsRestant) => {
        affichage.textContent = minuteur.afficher();
        const statut = minuteur.obtenirStatut();
        progression.style.width = statut.progression + '%';
        status.textContent = `${tempsRestant} secondes restantes`;
        
        // Changement de couleur sur les dernières 60 secondes
        if (tempsRestant <= 60) {
            affichage.style.color = '#ff4444';
            progression.style.background = 'linear-gradient(45deg, #ff4444, #cc0000)';
        } else {
            affichage.style.color = '#333';
            progression.style.background = 'linear-gradient(45deg, #4CAF50, #45a049)';
        }
    });
    
    minuteur.onFini(() => {
        status.textContent = "⏰ Session terminée ! Bravo ! 🎉";
        alert("⏰ Temps écoulé ! Session Pomodoro terminée ! 🍅");
    });
    
    // Événements boutons
    btnStart.onclick = () => {
        minuteur.demarrer();
        status.textContent = "⏱️ Minuteur en cours...";
    };
    btnPause.onclick = () => {
        minuteur.pauseer();
        status.textContent = "⏸️ Minuteur en pause";
    };
    btnReset.onclick = () => {
        minuteur.reset();
        affichage.textContent = minuteur.afficher();
        progression.style.width = '0%';
        status.textContent = "🔄 Remis à zéro - Prêt à redémarrer";
        affichage.style.color = '#333';
        progression.style.background = 'linear-gradient(45deg, #4CAF50, #45a049)';
    };
    
    return minuteur;
}

// Créer l'interface
const minuteur = creerInterfaceMinuteur();
```
{% endtab %}
{% endtabs %}

### 🎮 Exercice 3 : Animation de balle rebondissante

{% tabs %}
{% tab title="🎯 Objectif" %}
**Animation fluide avec :**
- 🎾 Balle qui rebondit dans un canvas
- 🎨 Changement de couleur à chaque collision
- ⚡ Contrôle de vitesse en temps réel
- 🎛️ Boutons Start/Stop/Reset

**Niveau :** ⭐⭐⭐⭐ (4/5)

{% hint style="info" %}
**Bonus :** Utiliser `requestAnimationFrame` pour une animation à 60fps !
{% endhint %}
{% endtab %}

{% tab title="✅ Solution complète" %}
```javascript
class AnimationBalle {
    constructor(containerId) {
        this.container = document.getElementById(containerId) || document.body;
        this.setupCanvas();
        this.setupBalle();
        this.setupControles();
        
        this.animationId = null;
        this.enMarche = false;
    }
    
    setupCanvas() {
        this.canvas = document.createElement('canvas');
        this.ctx = this.canvas.getContext('2d');
        this.canvas.width = 600;
        this.canvas.height = 400;
        this.canvas.style.cssText = `
            border: 3px solid #333;
            border-radius: 10px;
            background: linear-gradient(45deg, #e3f2fd, #f3e5f5);
            display: block;
            margin: 0 auto;
        `;
        this.container.appendChild(this.canvas);
    }
    
    setupBalle() {
        this.balle = {
            x: 50,
            y: 50,
            rayon: 20,
            vitesseX: 4,
            vitesseY: 3,
            couleur: '#ff4444',
            trail: [] // Traînée pour effet visuel
        };
    }
    
    setupControles() {
        const controles = document.createElement('div');
        controles.style.cssText = `
            text-align: center; margin: 15px 0; font-family: Arial;
        `;
        
        controles.innerHTML = `
            <div style="margin: 10px 0;">
                <button id="btn-start-anim" style="margin: 5px; padding: 8px 15px; font-size: 14px; background: #4CAF50; color: white; border: none; border-radius: 5px; cursor: pointer;">▶️ Start</button>
                <button id="btn-stop-anim" style="margin: 5px; padding: 8px 15px; font-size: 14px; background: #f44336; color: white; border: none; border-radius: 5px; cursor: pointer;">⏹️ Stop</button>
                <button id="btn-reset-anim" style="margin: 5px; padding: 8px 15px; font-size: 14px; background: #ff9800; color: white; border: none; border-radius: 5px; cursor: pointer;">🔄 Reset</button>
            </div>
            <div style="margin: 10px 0;">
                <label style="font-weight: bold;">⚡ Vitesse: </label>
                <input type="range" id="vitesse-slider" min="1" max="15" value="4" style="width: 200px; margin: 0 10px;">
                <span id="vitesse-value">4</span>
            </div>
            <div id="stats" style="margin: 10px 0; font-size: 14px; color: #666;">
                Collisions: 0 | FPS: -- | Status: Arrêté
            </div>
        `;
        
        this.container.appendChild(controles);
        this.setupEventListeners();
    }
    
    setupEventListeners() {
        document.getElementById('btn-start-anim').onclick = () => this.demarrer();
        document.getElementById('btn-stop-anim').onclick = () => this.arreter();
        document.getElementById('btn-reset-anim').onclick = () => this.reset();
        
        const slider = document.getElementById('vitesse-slider');
        const valueDisplay = document.getElementById('vitesse-value');
        
        slider.oninput = (e) => {
            const vitesse = parseInt(e.target.value);
            valueDisplay.textContent = vitesse;
            
            // Ajuster la vitesse en gardant la direction
            const ratioX = this.balle.vitesseX / Math.abs(this.balle.vitesseX);
            const ratioY = this.balle.vitesseY / Math.abs(this.balle.vitesseY);
            
            this.balle.vitesseX = vitesse * ratioX;
            this.balle.vitesseY = (vitesse * 0.8) * ratioY;
        };
    }
    
    demarrer() {
        if (this.enMarche) return;
        
        this.enMarche = true;
        this.collisions = 0;
        this.startTime = performance.now();
        this.frameCount = 0;
        
        this.animer();
        this.updateStats();
        console.log("🎮 Animation démarrée");
    }
    
    arreter() {
        if (!this.enMarche) return;
        
        this.enMarche = false;
        cancelAnimationFrame(this.animationId);
        this.updateStats();
        console.log("⏹️ Animation arrêtée");
    }
    
    reset() {
        this.arreter();
        this.balle.x = 50;
        this.balle.y = 50;
        this.balle.vitesseX = 4;
        this.balle.vitesseY = 3;
        this.balle.couleur = '#ff4444';
        this.balle.trail = [];
        this.collisions = 0;
        this.dessiner();
        this.updateStats();
        console.log("🔄 Animation remise à zéro");
    }
    
    animer() {
        if (!this.enMarche) return;
        
        this.mettreAJour();
        this.dessiner();
        this.frameCount++;
        
        // FPS calculation every 60 frames
        if (this.frameCount % 60 === 0) {
            this.updateStats();
        }
        
        this.animationId = requestAnimationFrame(() => this.animer());
    }
    
    mettreAJour() {
        // Ajouter position actuelle à la traînée
        this.balle.trail.push({x: this.balle.x, y: this.balle.y});
        if (this.balle.trail.length > 10) {
            this.balle.trail.shift();
        }
        
        // Déplacer la balle
        this.balle.x += this.balle.vitesseX;
        this.balle.y += this.balle.vitesseY;
        
        // Collision avec les bords
        let collision = false;
        
        if (this.balle.x + this.balle.rayon >= this.canvas.width || 
            this.balle.x - this.balle.rayon <= 0) {
            this.balle.vitesseX = -this.balle.vitesseX;
            collision = true;
        }
        
        if (this.balle.y + this.balle.rayon >= this.canvas.height || 
            this.balle.y - this.balle.rayon <= 0) {
            this.balle.vitesseY = -this.balle.vitesseY;
            collision = true;
        }
        
        if (collision) {
            this.collisions++;
            this.balle.couleur = this.genererCouleurAleatoire();
        }
        
        // Garder dans les limites (sécurité)
        this.balle.x = Math.max(this.balle.rayon, 
                      Math.min(this.canvas.width - this.balle.rayon, this.balle.x));
        this.balle.y = Math.max(this.balle.rayon, 
                      Math.min(this.canvas.height - this.balle.rayon, this.balle.y));
    }
    
    dessiner() {
        // Effacer avec effet de fade
        this.ctx.fillStyle = 'rgba(227, 242, 253, 0.3)';
        this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);
        
        // Dessiner la traînée
        this.balle.trail.forEach((point, index) => {
            const alpha = index / this.balle.trail.length;
            this.ctx.beginPath();
            this.ctx.arc(point.x, point.y, this.balle.rayon * alpha, 0, Math.PI * 2);
            this.ctx.fillStyle = this.balle.couleur.replace(')', `, ${alpha * 0.3})`).replace('#', 'rgba(').replace(/(.{2})(.{2})(.{2})/, (match, r, g, b) => 
                `rgba(${parseInt(r, 16)}, ${parseInt(g, 16)}, ${parseInt(b, 16)}, ${alpha * 0.3})`);
            this.ctx.fill();
        });
        
        // Dessiner la balle principale
        this.ctx.beginPath();
        this.ctx.arc(this.balle.x, this.balle.y, this.balle.rayon, 0, Math.PI * 2);
        this.ctx.fillStyle = this.balle.couleur;
        this.ctx.fill();
        
        // Bordure
        this.ctx.strokeStyle = '#333';
        this.ctx.lineWidth = 2;
        this.ctx.stroke();
        
        // Effet de brillance
        this.ctx.beginPath();
        this.ctx.arc(this.balle.x - 7, this.balle.y - 7, 5, 0, Math.PI * 2);
        this.ctx.fillStyle = 'rgba(255, 255, 255, 0.7)';
        this.ctx.fill();
    }
    
    genererCouleurAleatoire() {
        const couleurs = ['#ff4444', '#44ff44', '#4444ff', '#ffff44', '#ff44ff', '#44ffff', '#ff8800', '#8800ff'];
        return couleurs[Math.floor(Math.random() * couleurs.length)];
    }
    
    updateStats() {
        const currentTime = performance.now();
        const elapsedTime = (currentTime - this.startTime) / 1000;
        const fps = this.frameCount / elapsedTime;
        
        const statsElement = document.getElementById('stats');
        if (statsElement) {
            const status = this.enMarche ? 'En cours' : 'Arrêté';
            const fpsDisplay = this.enMarche ? Math.round(fps) : '--';
            statsElement.textContent = `Collisions: ${this.collisions} | FPS: ${fpsDisplay} | Status: ${status}`;
        }
    }
}

// Créer l'interface
function creerInterfaceAnimation() {
    const container = document.createElement('div');
    container.id = 'animation-container';
    container.style.cssText = `
        text-align: center; margin: 20px auto; padding: 20px;
        background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
        border-radius: 15px; width: fit-content; box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    `;
    
    container.innerHTML = '<h3 style="margin-top: 0; color: #333;">🎮 Animation Balle Rebondissante</h3>';
    document.body.appendChild(container);
    
    return new AnimationBalle('animation-container');
}

// Lancer la démo
const animation = creerInterfaceAnimation();

// Auto-start après 2 secondes
setTimeout(() => {
    animation.demarrer();
    console.log("🚀 Animation démarrée automatiquement !");
}, 2000);
```
{% endtab %}
{% endtabs %}

## ⚠️ Erreurs courantes et solutions

### 1. Variable dans une boucle avec setTimeout

{% tabs %}
{% tab title="❌ Problème classique" %}
```javascript
// Problème : Affiche "3" trois fois !
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Résultat : 3, 3, 3 (pas 0, 1, 2)
```

**Pourquoi ?** `var` a un function scope, pas un block scope !
{% endtab %}

{% tab title="✅ Solution 1 : let" %}
```javascript
// Solution simple : utiliser let
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Résultat : 0, 1, 2 ✨
```

`let` crée un nouveau scope à chaque itération !
{% endtab %}

{% tab title="✅ Solution 2 : Closure" %}
```javascript
// Solution avancée : closure manuelle
for (var i = 0; i < 3; i++) {
    (function(j) {
        setTimeout(() => console.log(j), 100);
    })(i);
}
// Résultat : 0, 1, 2 ✨
```

La fonction immédiate capture la valeur de `i` !
{% endtab %}
{% endtabs %}

### 2. Oublier de nettoyer les timers

{% tabs %}
{% tab title="❌ Fuite de mémoire" %}
```javascript
function demarrerTimer() {
    // Problème : pas de nettoyage !
    setInterval(() => {
        console.log("Timer en cours...");
    }, 1000);
}

// Chaque appel crée un nouveau timer
demarrerTimer(); // Timer 1
demarrerTimer(); // Timer 2 + Timer 1
demarrerTimer(); // Timer 3 + Timer 2 + Timer 1
```
{% endtab %}

{% tab title="✅ Gestion propre" %}
```javascript
let timerId = null;

function demarrerTimer() {
    // Éviter les doublons
    if (timerId) return;
    
    timerId = setInterval(() => {
        console.log("Timer en cours...");
    }, 1000);
    
    console.log("Timer démarré:", timerId);
}

function arreterTimer() {
    if (timerId) {
        clearInterval(timerId);
        timerId = null;
        console.log("Timer arrêté et nettoyé");
    }
}

// Usage sécurisé
demarrerTimer(); // OK
demarrerTimer(); // Ignoré (évite doublon)
arreterTimer();  // Nettoyage propre
```
{% endtab %}
{% endtabs %}

### 3. Code bloquant l'interface

{% tabs %}
{% tab title="❌ Interface bloquée" %}
```javascript
function tacheLourde() {
    // Bloque complètement l'interface !
    for (let i = 0; i < 10000000; i++) {
        Math.sqrt(i); // Calcul intensif
    }
    console.log("Terminé !");
}

// L'interface freeze pendant l'exécution 😱
tacheLourde();
```
{% endtab %}

{% tab title="✅ Version non-bloquante" %}
```javascript
function tacheLourdeNonBloquante(callback) {
    let i = 0;
    const batchSize = 10000; // Traiter par petits paquets
    
    function traiterBatch() {
        const fin = Math.min(i + batchSize, 10000000);
        
        // Traiter un petit paquet
        for (; i < fin; i++) {
            Math.sqrt(i);
        }
        
        // Progression
        const pourcentage = Math.round((i / 10000000) * 100);
        console.log(`Progression: ${pourcentage}%`);
        
        if (i < 10000000) {
            // Laisser respirer l'interface
            setTimeout(traiterBatch, 0);
        } else {
            console.log("Terminé sans bloquer !");
            if (callback) callback();
        }
    }
    
    traiterBatch();
}

// Usage non-bloquant
tacheLourdeNonBloquante(() => {
    console.log("Callback de fin exécuté !");
});

console.log("Cette ligne s'affiche immédiatement !");
```
{% endtab %}
{% endtabs %}

## 🎯 Points clés à retenir

{% hint style="success" %}
**L'Event Loop en résumé :**

✅ **JavaScript est mono-thread** mais non-bloquant grâce à l'Event Loop  
✅ **Code synchrone** s'exécute toujours avant l'asynchrone  
✅ **setTimeout(fn, 0)** n'est pas instantané (minimum ~4ms)  
✅ **Toujours nettoyer** les timers pour éviter les fuites  
✅ **Découper les tâches lourdes** pour garder l'interface fluide  
{% endhint %}

### Ordre d'exécution simplifié

| Priorité | Type | Exemple |
|----------|------|---------|
| **1** | Code synchrone | `console.log()` |
| **2** | Microtasks | `Promise.then()` |
| **3** | Macrotasks | `setTimeout()` |
| **4** | Render | Mise à jour DOM |

## 🔄 Liens avec d'autres concepts

### Concepts utilisés
- **Fonctions** : Callbacks et fonctions de rappel
- **DOM** : Manipulation pour les animations
- **Événements** : Gestion des interactions utilisateur

### Prochaines étapes
- **Promises** : Alternative moderne aux callbacks
- **Async/await** : Syntaxe encore plus claire
- **Fetch API** : Requêtes réseau asynchrones

## ✅ Auto-évaluation

### Quiz express

{% tabs %}
{% tab title="Question 1" %}
**Pourquoi setTimeout(fn, 0) ne s'exécute pas immédiatement ?**

<details>
<summary>Voir la réponse</summary>

**Réponse :** Car il passe par l'Event Loop et attend que la call stack soit vide. Le code synchrone a toujours la priorité !
</details>
{% endtab %}

{% tab title="Question 2" %}
**Que affiche ce code ?**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 0);
}
```

<details>
<summary>Voir la réponse</summary>

**Réponse :** "3" trois fois, car `var` a un function scope et la boucle se termine avant l'exécution des setTimeout.
</details>
{% endtab %}

{% tab title="Question 3" %}
**Comment éviter de bloquer l'interface ?**

<details>
<summary>Voir la réponse</summary>

**Réponse :** Découper les tâches lourdes avec setTimeout/requestAnimationFrame pour laisser respirer l'Event Loop.
</details>
{% endtab %}
{% endtabs %}

### Checklist de maîtrise

- [ ] Je comprends le fonctionnement de l'Event Loop
- [ ] Je différencie code synchrone et asynchrone  
- [ ] Je maîtrise setTimeout et setInterval
- [ ] Je prédis l'ordre d'exécution du code
- [ ] J'évite de bloquer l'interface utilisateur
- [ ] Je nettoie correctement mes timers

---

{% hint style="info" %}
**Prochaine étape :** [Gestion d'Erreurs](65-gestion-erreurs.md)

Maintenant que vous maîtrisez l'asynchrone, découvrons comment gérer les erreurs qui peuvent survenir !
{% endhint %}
