# Event Loop et Asynchrone - Les Bases

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** Event Loop et Programmation Asynchrone - Introduction
- **Niveau :** 🟡 Intermédiaire  
- **Durée estimée :** 40 minutes
- **Prérequis :** DOM, Événements, Fonctions, Callbacks de base
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Comment JavaScript gère les tâches asynchrones
- [ ] **Appliquer** : setTimeout et setInterval correctement
- [ ] **Analyser** : L'ordre d'exécution du code asynchrone
- [ ] **Créer** : Applications avec des délais et animations simples

### Mots-clés à retenir
- **Event Loop** : Boucle qui gère l'exécution du code asynchrone
- **Call Stack** : Pile d'exécution des fonctions
- **Web APIs** : Services du navigateur (setTimeout, DOM, etc.)
- **Task Queue** : File d'attente des tâches à exécuter
- **Callback** : Fonction appelée après une opération asynchrone

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
L'Event Loop est le cœur de JavaScript ! Comprendre ce mécanisme t'aide à :
- Créer des animations et interactions fluides
- Éviter que ton code "bloque" le navigateur
- Comprendre pourquoi certains codes s'exécutent "plus tard"
- Préparer aux concepts avancés (Promises, async/await)
- Déboguer les problèmes de timing dans tes applications

**JavaScript est mono-thread** mais peut faire plusieurs choses "en même temps" grâce à l'Event Loop !

### Définition et concepts clés

JavaScript ne peut exécuter qu'**une seule chose à la fois** (mono-thread), mais le navigateur nous aide avec des **Web APIs** pour les tâches longues.

**Composants de l'Event Loop :**

1. **Call Stack** : Pile des fonctions en cours d'exécution
2. **Web APIs** : Services du navigateur (timers, DOM events, etc.)
3. **Task Queue** : File d'attente des callbacks prêts
4. **Event Loop** : Surveille et transfère les tâches

**Processus simplifié :**
```javascript
// 1. Code synchrone : exécuté immédiatement
console.log("1. Début");

// 2. setTimeout : délégué aux Web APIs
setTimeout(() => {
    console.log("3. Dans setTimeout");
}, 0);

// 3. Code synchrone : exécuté immédiatement  
console.log("2. Fin");

// Résultat : "1. Début", "2. Fin", "3. Dans setTimeout"
```

### Visualisation de l'Event Loop

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│ Call Stack  │    │  Web APIs    │    │ Task Queue  │
│ (Sync code) │    │ (setTimeout, │    │ (Callbacks  │
│             │◄───┤  DOM events) │───►│  ready)     │
└─────────────┘    └──────────────┘    └─────────────┘
       ▲                                       │
       │               Event Loop              │
       └───────────────────────────────────────┘
```

### Points d'attention
- **setTimeout(fn, 0)** : N'est pas instantané ! Minimum ~4ms
- **Blocking code** : Code synchrone long bloque tout
- **Order matters** : L'ordre des callbacks dépend de leur timing
- **DOM updates** : Se font entre les tâches de l'Event Loop

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
console.log("=== Démonstration Event Loop ===");

// Test de l'ordre d'exécution
console.log("1. Première ligne");

setTimeout(() => {
    console.log("4. Premier setTimeout (100ms)");
}, 100);

setTimeout(() => {
    console.log("3. Deuxième setTimeout (0ms)");
}, 0);

console.log("2. Dernière ligne synchrone");

// Résultat :
// 1. Première ligne
// 2. Dernière ligne synchrone  
// 3. Deuxième setTimeout (0ms)
// 4. Premier setTimeout (100ms)
```

**Résultat attendu :**
```
=== Démonstration Event Loop ===
1. Première ligne
2. Dernière ligne synchrone
3. Deuxième setTimeout (0ms)
4. Premier setTimeout (100ms)
```

### Exemple intermédiaire
```javascript
// Horloge numérique en temps réel
function creerHorloge() {
    const elementHorloge = document.createElement('div');
    elementHorloge.id = 'horloge';
    elementHorloge.style.cssText = `
        font-family: monospace;
        font-size: 2em;
        text-align: center;
        background: #333;
        color: #0ff;
        padding: 20px;
        border-radius: 10px;
        margin: 20px auto;
        width: 300px;
    `;
    document.body.appendChild(elementHorloge);

    function mettreAJourHeure() {
        const maintenant = new Date();
        const heureActuelle = maintenant.toLocaleTimeString();
        elementHorloge.textContent = heureActuelle;
    }

    // Mise à jour immédiate
    mettreAJourHeure();
    
    // Mise à jour chaque seconde
    const intervalId = setInterval(mettreAJourHeure, 1000);
    
    console.log("Horloge démarrée ! ID interval:", intervalId);
    
    // Retourner une fonction pour arrêter l'horloge
    return function arreterHorloge() {
        clearInterval(intervalId);
        elementHorloge.remove();
        console.log("Horloge arrêtée !");
    };
}

// Animation de compteur avec callbacks
function animerCompteur(elementId, valeurFinale, duree = 2000) {
    const element = document.getElementById(elementId);
    if (!element) {
        console.error("Élément non trouvé:", elementId);
        return;
    }
    
    const valeurDepart = 0;
    const increment = valeurFinale / (duree / 50); // 50ms par frame
    let valeurActuelle = valeurDepart;
    
    const timer = setInterval(() => {
        valeurActuelle += increment;
        
        if (valeurActuelle >= valeurFinale) {
            valeurActuelle = valeurFinale;
            clearInterval(timer);
            console.log("Animation terminée !");
        }
        
        element.textContent = Math.floor(valeurActuelle);
    }, 50);
}

// Test de performance avec timing
function testerPerformance() {
    console.log("=== Test de performance ===");
    
    const debut = performance.now();
    
    // Simulation d'une tâche lourde (à éviter !)
    function tacheLourde() {
        let resultat = 0;
        for (let i = 0; i < 1000000; i++) {
            resultat += Math.random();
        }
        return resultat;
    }
    
    // Version bloquante (mauvaise)
    console.log("Début tâche bloquante...");
    const resultatBloquant = tacheLourde();
    const finBloquant = performance.now();
    console.log(`Tâche bloquante terminée en ${finBloquant - debut}ms`);
    
    // Version non-bloquante (meilleure)
    console.log("Début tâche non-bloquante...");
    setTimeout(() => {
        const resultatNonBloquant = tacheLourde();
        const finNonBloquant = performance.now();
        console.log(`Tâche non-bloquante terminée en ${finNonBloquant - finBloquant}ms`);
    }, 0);
    
    console.log("Cette ligne s'affiche avant la tâche non-bloquante !");
}

// Démonstration complète
console.log("=== Démarrage des démos ===");

// Créer éléments de test
document.body.innerHTML += `
    <div style="text-align: center; font-family: Arial;">
        <h2>Démo Event Loop</h2>
        <div id="compteur" style="font-size: 3em; color: blue;">0</div>
        <button onclick="animerCompteur('compteur', 100)">Animer Compteur</button>
        <button onclick="testerPerformance()">Test Performance</button>
    </div>
`;

// Démarrer l'horloge
const arreterHorloge = creerHorloge();

// Arrêter l'horloge après 10 secondes
setTimeout(() => {
    arreterHorloge();
    console.log("Horloge automatiquement arrêtée après 10 secondes");
}, 10000);
```

### Exemple avancé (optionnel)
```javascript
// Gestionnaire de tâches asynchrones avec priorités
class GestionnaireAsync {
    constructor() {
        this.taches = [];
        this.enCours = false;
    }
    
    ajouterTache(nom, callback, delai = 0, priorite = 1) {
        const tache = {
            nom,
            callback,
            delai,
            priorite,
            ajoutee: Date.now()
        };
        
        this.taches.push(tache);
        this.taches.sort((a, b) => b.priorite - a.priorite); // Tri par priorité
        
        console.log(`Tâche "${nom}" ajoutée avec priorité ${priorite}`);
        
        if (!this.enCours) {
            this.executerProchaineeTache();
        }
    }
    
    executerProchaineeTache() {
        if (this.taches.length === 0) {
            this.enCours = false;
            console.log("Toutes les tâches terminées !");
            return;
        }
        
        this.enCours = true;
        const tache = this.taches.shift();
        
        console.log(`Exécution de "${tache.nom}" dans ${tache.delai}ms...`);
        
        setTimeout(() => {
            const debut = performance.now();
            
            try {
                tache.callback();
                const fin = performance.now();
                console.log(`✅ "${tache.nom}" terminée en ${(fin - debut).toFixed(2)}ms`);
            } catch (error) {
                console.error(`❌ Erreur dans "${tache.nom}":`, error.message);
            }
            
            // Exécuter la prochaine tâche
            this.executerProchaineeTache();
            
        }, tache.delai);
    }
    
    viderTaches() {
        this.taches = [];
        console.log("Toutes les tâches en attente supprimées");
    }
    
    obtenirStatut() {
        return {
            tachesEnAttente: this.taches.length,
            enCours: this.enCours,
            prochaineeTache: this.taches[0]?.nom || "Aucune"
        };
    }
}

// Démo du gestionnaire
const gestionnaire = new GestionnaireAsync();

// Ajouter différentes tâches
gestionnaire.ajouterTache("Tâche rapide", () => {
    console.log("💨 Tâche rapide exécutée !");
}, 100, 3); // Haute priorité

gestionnaire.ajouterTache("Tâche lente", () => {
    console.log("🐌 Début tâche lente...");
    // Simulation tâche lourde
    let result = 0;
    for (let i = 0; i < 500000; i++) {
        result += Math.sqrt(i);
    }
    console.log("🐌 Tâche lente terminée !");
}, 200, 1); // Basse priorité

gestionnaire.ajouterTache("Tâche critique", () => {
    console.log("🚨 TÂCHE CRITIQUE exécutée !");
}, 50, 5); // Très haute priorité

gestionnaire.ajouterTache("Animation", () => {
    console.log("🎨 Animation démarrée");
    animerCompteur('compteur', 50);
}, 300, 2); // Priorité moyenne

// Afficher le statut toutes les 500ms
const statusInterval = setInterval(() => {
    const statut = gestionnaire.obtenirStatut();
    console.log("📊 Statut:", statut);
    
    if (!statut.enCours && statut.tachesEnAttente === 0) {
        clearInterval(statusInterval);
        console.log("🎉 Gestionnaire de tâches terminé !");
    }
}, 500);

// Démonstration de callback hell (problème à éviter)
function exempleCallbackHell() {
    console.log("=== Exemple Callback Hell (à éviter) ===");
    
    setTimeout(() => {
        console.log("Étape 1 terminée");
        setTimeout(() => {
            console.log("Étape 2 terminée");
            setTimeout(() => {
                console.log("Étape 3 terminée");
                setTimeout(() => {
                    console.log("Étape 4 terminée - CALLBACK HELL ! 😵");
                }, 100);
            }, 100);
        }, 100);
    }, 100);
}

// Lancer l'exemple après 5 secondes
setTimeout(exempleCallbackHell, 5000);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Prédire l'ordre d'exécution
**Consigne :** Sans exécuter le code, prévoir l'ordre d'affichage des messages

**Code à analyser :**
```javascript
console.log("A");

setTimeout(() => console.log("B"), 0);

console.log("C");

setTimeout(() => console.log("D"), 100);

console.log("E");
```

**Quel sera l'ordre d'affichage ?** Écris tes prédictions puis teste !

**Niveau de difficulté :** ⭐⭐☆☆☆

### Exercice 2 : Minuteur personnalisé
**Consigne :** Crée un minuteur avec affichage en temps réel

**Spécifications :**
- Compte à rebours depuis une durée donnée
- Affichage format MM:SS
- Alerte sonore à la fin (bonus)
- Boutons Start/Pause/Reset

**Code de départ :**
```javascript
class Minuteur {
    constructor(dureeEnSecondes) {
        this.duree = dureeEnSecondes;
        this.tempsRestant = dureeEnSecondes;
        this.intervalId = null;
        this.enMarche = false;
    }
    
    demarrer() {
        // Ton code ici
    }
    
    pauseer() {
        // Ton code ici
    }
    
    reset() {
        // Ton code ici
    }
    
    afficher() {
        // Convertir en MM:SS et afficher
    }
}
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 3 : Animation fluide
**Consigne :** Crée une animation d'une balle qui rebondit

**Contraintes :**
- Utiliser requestAnimationFrame (plus fluide que setTimeout)
- Gestion des collisions avec les bords
- Contrôle de la vitesse
- Boutons pour démarrer/arrêter l'animation

**Niveau de difficulté :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// Ordre prédit : A, C, E, B, D

console.log("A");           // 1. Synchrone - immédiat

setTimeout(() => console.log("B"), 0);  // 3. Asynchrone - 0ms mais après sync

console.log("C");           // 2. Synchrone - immédiat

setTimeout(() => console.log("D"), 100); // 4. Asynchrone - 100ms

console.log("E");           // 3. Synchrone - immédiat

// Résultat : A, C, E, B, D
```

**Explication :** Le code synchrone s'exécute d'abord, puis les callbacks dans l'ordre de leur délai.

### Solution Exercice 2
```javascript
class Minuteur {
    constructor(dureeEnSecondes) {
        this.duree = dureeEnSecondes;
        this.tempsRestant = dureeEnSecondes;
        this.intervalId = null;
        this.enMarche = false;
        this.callbacks = {
            onTick: null,
            onFini: null
        };
    }
    
    demarrer() {
        if (this.enMarche) return;
        
        this.enMarche = true;
        
        this.intervalId = setInterval(() => {
            this.tempsRestant--;
            
            // Callback à chaque seconde
            if (this.callbacks.onTick) {
                this.callbacks.onTick(this.tempsRestant);
            }
            
            this.afficher();
            
            // Fin du minuteur
            if (this.tempsRestant <= 0) {
                this.pauseer();
                if (this.callbacks.onFini) {
                    this.callbacks.onFini();
                }
                console.log("⏰ Temps écoulé !");
            }
        }, 1000);
        
        console.log("▶️ Minuteur démarré");
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
        this.afficher();
        
        console.log("🔄 Minuteur remis à zéro");
    }
    
    afficher() {
        const minutes = Math.floor(this.tempsRestant / 60);
        const secondes = this.tempsRestant % 60;
        const affichage = `${minutes.toString().padStart(2, '0')}:${secondes.toString().padStart(2, '0')}`;
        
        console.log(`⏱️ ${affichage}`);
        return affichage;
    }
    
    onTick(callback) {
        this.callbacks.onTick = callback;
    }
    
    onFini(callback) {
        this.callbacks.onFini = callback;
    }
    
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
        text-align: center;
        font-family: Arial;
        background: #f0f0f0;
        padding: 20px;
        border-radius: 10px;
        margin: 20px auto;
        width: 300px;
    `;
    
    container.innerHTML = `
        <h3>⏱️ Minuteur</h3>
        <div id="affichage-minuteur" style="font-size: 2em; font-family: monospace; margin: 20px 0;">05:00</div>
        <div>
            <button id="btn-start">▶️ Start</button>
            <button id="btn-pause">⏸️ Pause</button>
            <button id="btn-reset">🔄 Reset</button>
        </div>
        <div id="barre-progression" style="background: #ddd; height: 20px; margin-top: 20px; border-radius: 10px;">
            <div id="progression" style="background: #4CAF50; height: 100%; width: 0%; border-radius: 10px; transition: width 1s;"></div>
        </div>
    `;
    
    document.body.appendChild(container);
    
    // Créer le minuteur (5 minutes par défaut)
    const minuteur = new Minuteur(300);
    
    // Éléments DOM
    const affichage = document.getElementById('affichage-minuteur');
    const progression = document.getElementById('progression');
    const btnStart = document.getElementById('btn-start');
    const btnPause = document.getElementById('btn-pause');
    const btnReset = document.getElementById('btn-reset');
    
    // Callbacks
    minuteur.onTick((tempsRestant) => {
        affichage.textContent = minuteur.afficher();
        const statut = minuteur.obtenirStatut();
        progression.style.width = statut.progression + '%';
    });
    
    minuteur.onFini(() => {
        alert("⏰ Temps écoulé !");
        // Bonus : son (si supporté)
        try {
            const audio = new Audio('data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBj+a2/LDciUFLIHO8tiJNwgZaLvt559NEAxQp+PwtmMcBjiR1/LMeSwFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhCSqO1fLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhCSqO1fLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhCSqO1fLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwiCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhCSuQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhCSuQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhCSuQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmshCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmshCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmshCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+DyvmshCCqQ1vLNeSsFJHfH8N2QQAoUXrTp66hVFApGn+Dy');
            audio.play();
        } catch (e) {
            console.log("Son non disponible");
        }
    });
    
    // Événements boutons
    btnStart.onclick = () => minuteur.demarrer();
    btnPause.onclick = () => minuteur.pauseer();
    btnReset.onclick = () => {
        minuteur.reset();
        affichage.textContent = minuteur.afficher();
        progression.style.width = '0%';
    };
    
    return minuteur;
}

// Créer l'interface
const minuteur = creerInterfaceMinuteur();
```

**Alternatives possibles :**
- Ajouter paramètres personnalisables (durée, alarme)
- Sauvegarder l'état dans localStorage
- Ajouter des presets (25min Pomodoro, etc.)

### Solution Exercice 3
```javascript
class AnimationBalle {
    constructor(containerId) {
        this.container = document.getElementById(containerId);
        this.canvas = document.createElement('canvas');
        this.ctx = this.canvas.getContext('2d');
        
        // Configuration canvas
        this.canvas.width = 600;
        this.canvas.height = 400;
        this.canvas.style.border = '2px solid #333';
        this.canvas.style.borderRadius = '10px';
        
        this.container.appendChild(this.canvas);
        
        // État de la balle
        this.balle = {
            x: 50,
            y: 50,
            rayon: 20,
            vitesseX: 3,
            vitesseY: 2,
            couleur: '#ff4444'
        };
        
        this.animationId = null;
        this.enMarche = false;
        
        this.creerControles();
    }
    
    creerControles() {
        const controles = document.createElement('div');
        controles.style.cssText = `
            text-align: center;
            margin: 10px 0;
        `;
        
        controles.innerHTML = `
            <button id="btn-start-anim">▶️ Start</button>
            <button id="btn-stop-anim">⏹️ Stop</button>
            <button id="btn-reset-anim">🔄 Reset</button>
            <label>Vitesse: <input type="range" id="vitesse-slider" min="1" max="10" value="3"></label>
        `;
        
        this.container.appendChild(controles);
        
        // Événements
        document.getElementById('btn-start-anim').onclick = () => this.demarrer();
        document.getElementById('btn-stop-anim').onclick = () => this.arreter();
        document.getElementById('btn-reset-anim').onclick = () => this.reset();
        
        document.getElementById('vitesse-slider').oninput = (e) => {
            const vitesse = parseInt(e.target.value);
            this.balle.vitesseX = this.balle.vitesseX > 0 ? vitesse : -vitesse;
            this.balle.vitesseY = this.balle.vitesseY > 0 ? vitesse - 1 : -(vitesse - 1);
        };
    }
    
    demarrer() {
        if (this.enMarche) return;
        
        this.enMarche = true;
        this.animer();
        console.log("🎮 Animation démarrée");
    }
    
    arreter() {
        if (!this.enMarche) return;
        
        this.enMarche = false;
        cancelAnimationFrame(this.animationId);
        console.log("⏹️ Animation arrêtée");
    }
    
    reset() {
        this.arreter();
        this.balle.x = 50;
        this.balle.y = 50;
        this.balle.vitesseX = 3;
        this.balle.vitesseY = 2;
        this.dessiner();
        console.log("🔄 Animation remise à zéro");
    }
    
    animer() {
        if (!this.enMarche) return;
        
        this.mettreAJour();
        this.dessiner();
        
        // Demander la prochaine frame
        this.animationId = requestAnimationFrame(() => this.animer());
    }
    
    mettreAJour() {
        // Déplacer la balle
        this.balle.x += this.balle.vitesseX;
        this.balle.y += this.balle.vitesseY;
        
        // Collision avec les bords
        if (this.balle.x + this.balle.rayon >= this.canvas.width || 
            this.balle.x - this.balle.rayon <= 0) {
            this.balle.vitesseX = -this.balle.vitesseX;
            this.balle.couleur = this.genererCouleurAleatoire();
        }
        
        if (this.balle.y + this.balle.rayon >= this.canvas.height || 
            this.balle.y - this.balle.rayon <= 0) {
            this.balle.vitesseY = -this.balle.vitesseY;
            this.balle.couleur = this.genererCouleurAleatoire();
        }
        
        // Garder la balle dans les limites (sécurité)
        this.balle.x = Math.max(this.balle.rayon, 
                      Math.min(this.canvas.width - this.balle.rayon, this.balle.x));
        this.balle.y = Math.max(this.balle.rayon, 
                      Math.min(this.canvas.height - this.balle.rayon, this.balle.y));
    }
    
    dessiner() {
        // Effacer le canvas
        this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
        
        // Dessiner la balle
        this.ctx.beginPath();
        this.ctx.arc(this.balle.x, this.balle.y, this.balle.rayon, 0, Math.PI * 2);
        this.ctx.fillStyle = this.balle.couleur;
        this.ctx.fill();
        
        // Ombre pour l'effet 3D
        this.ctx.beginPath();
        this.ctx.arc(this.balle.x + 2, this.balle.y + 2, this.balle.rayon, 0, Math.PI * 2);
        this.ctx.fillStyle = 'rgba(0, 0, 0, 0.3)';
        this.ctx.fill();
        
        // Redessiner la balle par-dessus l'ombre
        this.ctx.beginPath();
        this.ctx.arc(this.balle.x, this.balle.y, this.balle.rayon, 0, Math.PI * 2);
        this.ctx.fillStyle = this.balle.couleur;
        this.ctx.fill();
        
        // Effet de brillance
        this.ctx.beginPath();
        this.ctx.arc(this.balle.x - 5, this.balle.y - 5, 5, 0, Math.PI * 2);
        this.ctx.fillStyle = 'rgba(255, 255, 255, 0.6)';
        this.ctx.fill();
    }
    
    genererCouleurAleatoire() {
        const couleurs = ['#ff4444', '#44ff44', '#4444ff', '#ffff44', '#ff44ff', '#44ffff'];
        return couleurs[Math.floor(Math.random() * couleurs.length)];
    }
}

// Créer l'interface pour l'animation
function creerInterfaceAnimation() {
    const container = document.createElement('div');
    container.id = 'animation-container';
    container.style.cssText = `
        text-align: center;
        margin: 20px auto;
        padding: 20px;
        background: #f9f9f9;
        border-radius: 15px;
        width: fit-content;
    `;
    
    container.innerHTML = '<h3>🎮 Animation Balle Rebondissante</h3>';
    document.body.appendChild(container);
    
    return new AnimationBalle('animation-container');
}

// Lancer la démo
const animation = creerInterfaceAnimation();

// Démarrage automatique après 1 seconde
setTimeout(() => {
    animation.demarrer();
    console.log("🚀 Animation démarrée automatiquement");
}, 1000);
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Fonctions** : Callbacks et fonctions de rappel
- **DOM** : Manipulation d'éléments pour les animations
- **Événements** : Gestion des interactions utilisateur

### Prochaines étapes
- **Promises** : Alternative moderne aux callbacks
- **Async/await** : Syntaxe encore plus claire pour l'asynchrone
- **Fetch API** : Requêtes réseau asynchrones

### Concepts complémentaires
- **Web Workers** : Calculs lourds sans bloquer l'UI
- **Service Workers** : Cache et fonctionnalités offline
- **requestAnimationFrame** : Animations fluides 60fps

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un "Gestionnaire de notifications" avec Event Loop

**Fonctionnalités requises :**
- [ ] File d'attente de notifications avec priorités
- [ ] Affichage temporisé des notifications
- [ ] Animation d'apparition/disparition
- [ ] Gestion de plusieurs notifications simultanées
- [ ] Interface pour créer des notifications de test

**Structure de fichiers suggérée :**
```
📁 gestionnaire-notifications/
├── 📄 index.html
├── 📄 style.css
└── 📄 script.js
```

**Temps estimé :** 45 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Event Loop](https://developer.mozilla.org/fr/docs/Web/JavaScript/EventLoop)
- [MDN - setTimeout](https://developer.mozilla.org/fr/docs/Web/API/setTimeout)

### Vidéos recommandées
- "What the heck is the event loop anyway?" - Philip Roberts (26 min)
- Présentation légendaire avec visualisations excellentes

### Articles approfondis
- "Understanding the JavaScript Event Loop" - Lydia Hallie
- Explications visuelles très claires

### Outils de pratique
- [Loupe](http://latentflip.com/loupe/) : Visualiseur interactif de l'Event Loop
- [Event Loop Visualizer](https://www.jsv9000.app/) : Autre outil de visualisation

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Pourquoi setTimeout(fn, 0) ne s'exécute pas immédiatement ?
   - **Réponse :** Car il passe par l'Event Loop et attend que la call stack soit vide

2. **Question pratique :** Que fait ce code ?
   ```javascript
   for (var i = 0; i < 3; i++) {
       setTimeout(() => console.log(i), 0);
   }
   ```
   - **Réponse :** Affiche "3" trois fois à cause du closure et de var

3. **Question d'application :** Comment éviter de bloquer l'interface utilisateur ?
   - **Réponse :** Utiliser setTimeout/requestAnimationFrame pour découper les tâches lourdes

### Checklist de maîtrise
- [ ] Je comprends le fonctionnement de l'Event Loop
- [ ] Je différencie code synchrone et asynchrone
- [ ] Je maîtrise setTimeout et setInterval
- [ ] Je prédis l'ordre d'exécution du code
- [ ] J'évite de bloquer l'interface utilisateur
- [ ] Je gère correctement les animations

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 - Callback avec mauvaise variable
**Code problématique :**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // Toujours 3 !
}
```

**Solution :**
```javascript
// Solution 1 : let au lieu de var
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // 0, 1, 2
}

// Solution 2 : closure
for (var i = 0; i < 3; i++) {
    (function(j) {
        setTimeout(() => console.log(j), 100); // 0, 1, 2
    })(i);
}
```

**Explication :** var a un function scope, let crée un nouveau scope à chaque itération.

### Erreur fréquente 2 - Oublier de nettoyer les timers
**Code problématique :**
```javascript
function demarrerTimer() {
    setInterval(() => {
        console.log("Timer en cours...");
    }, 1000); // Pas de nettoyage !
}
```

**Solution :**
```javascript
let timerId = null;

function demarrerTimer() {
    if (timerId) return; // Éviter les doublons
    
    timerId = setInterval(() => {
        console.log("Timer en cours...");
    }, 1000);
}

function arreterTimer() {
    if (timerId) {
        clearInterval(timerId);
        timerId = null;
    }
}
```

**Explication :** Sans clearInterval, les timers continuent indéfiniment.

### Erreur fréquente 3 - Code bloquant l'interface
**Code problématique :**
```javascript
function tacheLourde() {
    for (let i = 0; i < 10000000; i++) {
        // Calcul lourd bloque tout !
        Math.sqrt(i);
    }
}
```

**Solution :**
```javascript
function tacheLourdeNonBloquante(callback) {
    let i = 0;
    const batchSize = 10000;
    
    function traiterBatch() {
        const fin = Math.min(i + batchSize, 10000000);
        
        for (; i < fin; i++) {
            Math.sqrt(i);
        }
        
        if (i < 10000000) {
            setTimeout(traiterBatch, 0); // Laisser respirer l'UI
        } else {
            callback(); // Terminé
        }
    }
    
    traiterBatch();
}
```

**Explication :** Découper les tâches lourdes avec setTimeout permet à l'UI de rester responsive.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- L'Event Loop gère l'asynchrone en JavaScript
- setTimeout n'est jamais instantané, même avec 0ms
- Il faut toujours nettoyer les timers pour éviter les fuites

### Questions à approfondir
- Comment fonctionne requestAnimationFrame exactement ?
- Différences entre microtasks et macrotasks ?

### Révision programmée
- [ ] Révision dans 1 jour (ordre d'exécution)
- [ ] Révision dans 1 semaine (création d'animations)
- [ ] Révision dans 1 mois (application complexe)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #event-loop #asynchrone #settimeout #animations #intermediaire

**Temps de completion :** ___

**Difficulté ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 77/126*
