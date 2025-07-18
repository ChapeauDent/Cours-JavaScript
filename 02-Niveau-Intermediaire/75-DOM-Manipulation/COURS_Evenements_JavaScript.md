# Événements JavaScript - Interactivité Web

## **INFORMATIONS GENERALES**

### Metadonnées du cours
- **Titre :** Événements JavaScript - Gestion de l'Interactivité
- **Niveau :** Intermédiaire
- **Durée estimée :** 85 minutes
- **Prérequis :** DOM, fonctions, objets, this, méthodes d'arrays
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Le système d'événements du navigateur et la propagation
- [ ] **Appliquer** : L'ajout et la suppression d'écouteurs d'événements
- [ ] **Analyser** : Les différents types d'événements et leurs propriétés
- [ ] **Créer** : Des interfaces interactives riches et réactives

### Mots-clés à retenir
- **Événement** : Action qui se produit dans le navigateur (clic, saisie, etc.)
- **Event Listener** : Fonction qui écoute et réagit à un événement
- **Propagation** : Mécanisme de remontée/descente des événements dans le DOM
- **Event Object** : Objet contenant les informations sur l'événement
- **preventDefault** : Empêche le comportement par défaut d'un événement
- **Delegation** : Technique d'écoute d'événements sur un parent

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les événements sont le cœur de l'interactivité web. Sans eux, une page web serait statique. Ils permettent de réagir aux actions de l'utilisateur (clics, saisies, mouvements de souris) et aux changements d'état du navigateur (chargement, redimensionnement). Maîtriser les événements est essentiel pour créer des applications web modernes et réactives.

### Définition et concepts clés

#### **1. Types d'événements principaux**
```javascript
// Événements de souris
element.addEventListener('click', handler);      // Clic
element.addEventListener('dblclick', handler);   // Double-clic
element.addEventListener('mousedown', handler);  // Bouton enfoncé
element.addEventListener('mouseup', handler);    // Bouton relâché
element.addEventListener('mouseover', handler);  // Survol
element.addEventListener('mouseout', handler);   // Sortie de survol
element.addEventListener('mousemove', handler);  // Mouvement

// Événements de clavier
element.addEventListener('keydown', handler);    // Touche enfoncée
element.addEventListener('keyup', handler);      // Touche relâchée
element.addEventListener('keypress', handler);   // Caractère saisi

// Événements de formulaire
element.addEventListener('submit', handler);     // Soumission
element.addEventListener('change', handler);     // Changement de valeur
element.addEventListener('input', handler);      // Saisie en temps réel
element.addEventListener('focus', handler);      // Prise de focus
element.addEventListener('blur', handler);       // Perte de focus

// Événements de fenêtre
window.addEventListener('load', handler);        // Chargement complet
window.addEventListener('resize', handler);      // Redimensionnement
window.addEventListener('scroll', handler);      // Défilement
```

#### **2. Propagation des événements**
```javascript
// Phase de capture (descente)
element.addEventListener('click', handler, true);

// Phase de bouillonnement (remontée) - par défaut
element.addEventListener('click', handler, false);

// Contrôle de la propagation
function handler(event) {
    event.stopPropagation();      // Arrête la propagation
    event.stopImmediatePropagation(); // Arrête même les autres listeners
}
```

#### **3. L'objet Event**
```javascript
function gererEvenement(event) {
    console.log('Type:', event.type);           // Type d'événement
    console.log('Cible:', event.target);        // Élément qui a déclenché
    console.log('Élément courant:', event.currentTarget); // Élément avec le listener
    console.log('Position X:', event.clientX);  // Position souris X
    console.log('Position Y:', event.clientY);  // Position souris Y
    console.log('Touche pressée:', event.key);  // Touche clavier
    console.log('Modificateurs:', {
        ctrl: event.ctrlKey,
        shift: event.shiftKey,
        alt: event.altKey
    });
}
```

#### **4. Délégation d'événements**
```javascript
// Au lieu d'écouter chaque élément individuellement
document.getElementById('liste').addEventListener('click', function(event) {
    // Vérifier que c'est bien un élément de liste qui a été cliqué
    if (event.target.matches('li')) {
        console.log('Item cliqué:', event.target.textContent);
    }
});
```

### Syntaxe de base
```javascript
// Méthode moderne (recommandée)
element.addEventListener('eventType', function(event) {
    // Gestion de l'événement
}, options);

// Options avancées
element.addEventListener('click', handler, {
    once: true,        // Exécuter une seule fois
    passive: true,     // Ne pas appeler preventDefault
    capture: true      // Phase de capture
});

// Suppression d'événement
element.removeEventListener('click', handler);

// Ancienne méthode (éviter)
element.onclick = function(event) {
    // Gestion de l'événement
};
```

### Points d'attention
- **Piège this** : Dans les arrow functions, `this` ne pointe pas vers l'élément
- **Piège mémoire** : Toujours supprimer les event listeners non utilisés
- **Bonne pratique** : Utiliser la délégation pour les listes dynamiques

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Système d'interaction basique avec boutons
console.log("=== SYSTÈME D'INTERACTION BASIQUE ===");

// Sélection des éléments
const boutonPrincipal = document.getElementById('bouton-principal');
const compteurDiv = document.getElementById('compteur');
const messageDiv = document.getElementById('message');

// État de l'application
let compteur = 0;

// Gestionnaire d'événement simple
function gererClicBouton(event) {
    compteur++;
    
    // Mise à jour de l'affichage
    compteurDiv.textContent = `Clics: ${compteur}`;
    messageDiv.textContent = `Dernier clic à ${new Date().toLocaleTimeString()}`;
    
    // Information sur l'événement
    console.log('Événement:', {
        type: event.type,
        target: event.target.tagName,
        position: { x: event.clientX, y: event.clientY },
        timestamp: event.timeStamp
    });
    
    // Effet visuel
    event.target.style.transform = 'scale(0.95)';
    setTimeout(() => {
        event.target.style.transform = 'scale(1)';
    }, 100);
}

// Ajout de l'écouteur d'événement
boutonPrincipal.addEventListener('click', gererClicBouton);

// Gestionnaire pour les événements clavier
function gererClavier(event) {
    console.log(`Touche pressée: ${event.key}`);
    
    switch (event.key) {
        case 'Enter':
        case ' ': // Espace
            gererClicBouton(event);
            break;
        case 'r':
        case 'R':
            compteur = 0;
            compteurDiv.textContent = 'Clics: 0';
            messageDiv.textContent = 'Compteur remis à zéro';
            break;
        case 'Escape':
            messageDiv.textContent = 'Interaction annulée';
            break;
    }
}

// Écoute des événements clavier sur toute la page
document.addEventListener('keydown', gererClavier);

// Gestionnaire pour le survol
function gererSurvol(event) {
    const bouton = event.target;
    bouton.style.backgroundColor = '#007bff';
    bouton.style.color = 'white';
    messageDiv.textContent = 'Bouton survolé - Cliquez ou appuyez sur Entrée';
}

function gererSortieSurvol(event) {
    const bouton = event.target;
    bouton.style.backgroundColor = '';
    bouton.style.color = '';
    messageDiv.textContent = '';
}

// Ajout des événements de survol
boutonPrincipal.addEventListener('mouseenter', gererSurvol);
boutonPrincipal.addEventListener('mouseleave', gererSortieSurvol);

console.log("Interface interactive initialisée!");
console.log("- Cliquez sur le bouton");
console.log("- Appuyez sur Entrée ou Espace pour simuler un clic");
console.log("- Appuyez sur R pour reset");
console.log("- Appuyez sur Échap pour annuler");
```

**Résultat attendu :**
```
=== SYSTÈME D'INTERACTION BASIQUE ===
Interface interactive initialisée!
- Cliquez sur le bouton
- Appuyez sur Entrée ou Espace pour simuler un clic
- Appuyez sur R pour reset
- Appuyez sur Échap pour annuler

Événement: {
  type: "click",
  target: "BUTTON",
  position: { x: 150, y: 200 },
  timestamp: 1234567890
}
```

### Exemple intermédiaire
```javascript
// Système de formulaire interactif avec validation en temps réel
console.log("=== FORMULAIRE INTERACTIF AVANCÉ ===");

class FormulaireInteractif {
    constructor(formulaireSelector) {
        this.formulaire = document.querySelector(formulaireSelector);
        this.champs = {};
        this.erreurs = {};
        this.init();
    }
    
    init() {
        // Initialiser les champs
        this.formulaire.querySelectorAll('input, textarea, select').forEach(champ => {
            this.champs[champ.name] = champ;
            this.ajouterEcouteursChamp(champ);
        });
        
        // Gestionnaire de soumission
        this.formulaire.addEventListener('submit', (event) => {
            this.gererSoumission(event);
        });
        
        console.log(`Formulaire initialisé avec ${Object.keys(this.champs).length} champs`);
    }
    
    ajouterEcouteursChamp(champ) {
        // Validation en temps réel pendant la saisie
        champ.addEventListener('input', (event) => {
            this.validerChamp(event.target);
            this.mettreAJourAffichage(event.target);
        });
        
        // Validation à la perte de focus
        champ.addEventListener('blur', (event) => {
            this.validerChamp(event.target);
            this.afficherErreur(event.target);
        });
        
        // Effacer les erreurs quand l'utilisateur recommence à taper
        champ.addEventListener('focus', (event) => {
            this.effacerErreur(event.target);
        });
        
        // Gestion spéciale pour les mots de passe
        if (champ.type === 'password') {
            this.ajouterToggleMotDePasse(champ);
        }
    }
    
    validerChamp(champ) {
        const valeur = champ.value.trim();
        const nom = champ.name;
        let valide = true;
        let messageErreur = '';
        
        // Validation selon le type de champ
        switch (nom) {
            case 'email':
                const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
                if (!regexEmail.test(valeur)) {
                    valide = false;
                    messageErreur = 'Format d\'email invalide';
                }
                break;
                
            case 'telephone':
                const regexTel = /^(\+33|0)[1-9]([0-9]{8})$/;
                if (!regexTel.test(valeur.replace(/\s/g, ''))) {
                    valide = false;
                    messageErreur = 'Numéro de téléphone invalide';
                }
                break;
                
            case 'motDePasse':
                if (valeur.length < 8) {
                    valide = false;
                    messageErreur = 'Le mot de passe doit contenir au moins 8 caractères';
                } else if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(valeur)) {
                    valide = false;
                    messageErreur = 'Le mot de passe doit contenir une majuscule, une minuscule et un chiffre';
                }
                break;
                
            case 'confirmationMotDePasse':
                const motDePasse = this.champs.motDePasse?.value;
                if (valeur !== motDePasse) {
                    valide = false;
                    messageErreur = 'Les mots de passe ne correspondent pas';
                }
                break;
                
            case 'nom':
            case 'prenom':
                if (valeur.length < 2) {
                    valide = false;
                    messageErreur = 'Ce champ doit contenir au moins 2 caractères';
                }
                break;
        }
        
        // Validation champs requis
        if (champ.required && valeur === '') {
            valide = false;
            messageErreur = 'Ce champ est obligatoire';
        }
        
        // Stocker le résultat
        this.erreurs[nom] = valide ? null : messageErreur;
        
        return valide;
    }
    
    mettreAJourAffichage(champ) {
        const nom = champ.name;
        const valide = !this.erreurs[nom];
        
        // Mise à jour visuelle
        champ.classList.toggle('valide', valide);
        champ.classList.toggle('invalide', !valide);
        
        // Mise à jour du compteur de caractères (si applicable)
        const compteur = document.querySelector(`[data-compteur="${nom}"]`);
        if (compteur) {
            const longueur = champ.value.length;
            const max = champ.maxLength;
            compteur.textContent = `${longueur}${max ? `/${max}` : ''} caractères`;
            compteur.style.color = longueur > max * 0.9 ? 'orange' : '';
        }
    }
    
    afficherErreur(champ) {
        const nom = champ.name;
        const erreur = this.erreurs[nom];
        
        let divErreur = document.querySelector(`[data-erreur="${nom}"]`);
        
        if (!divErreur) {
            divErreur = document.createElement('div');
            divErreur.setAttribute('data-erreur', nom);
            divErreur.className = 'message-erreur';
            champ.parentNode.appendChild(divErreur);
        }
        
        if (erreur) {
            divErreur.textContent = erreur;
            divErreur.style.display = 'block';
            divErreur.style.color = 'red';
            divErreur.style.fontSize = '0.8em';
            divErreur.style.marginTop = '5px';
        } else {
            divErreur.style.display = 'none';
        }
    }
    
    effacerErreur(champ) {
        const nom = champ.name;
        const divErreur = document.querySelector(`[data-erreur="${nom}"]`);
        if (divErreur) {
            divErreur.style.display = 'none';
        }
        champ.classList.remove('invalide');
    }
    
    ajouterToggleMotDePasse(champMotDePasse) {
        const conteneur = champMotDePasse.parentNode;
        const boutonToggle = document.createElement('button');
        boutonToggle.type = 'button';
        boutonToggle.textContent = '👁️';
        boutonToggle.style.cssText = `
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            cursor: pointer;
        `;
        
        conteneur.style.position = 'relative';
        conteneur.appendChild(boutonToggle);
        
        boutonToggle.addEventListener('click', (event) => {
            event.preventDefault();
            const estVisible = champMotDePasse.type === 'text';
            champMotDePasse.type = estVisible ? 'password' : 'text';
            boutonToggle.textContent = estVisible ? '👁️' : '🙈';
        });
    }
    
    gererSoumission(event) {
        event.preventDefault(); // Empêcher la soumission par défaut
        
        console.log('🔄 Validation du formulaire...');
        
        // Valider tous les champs
        let formulaireValide = true;
        Object.values(this.champs).forEach(champ => {
            if (!this.validerChamp(champ)) {
                formulaireValide = false;
                this.afficherErreur(champ);
            }
        });
        
        if (formulaireValide) {
            this.soumettreFormulaire();
        } else {
            console.log('❌ Formulaire invalide');
            this.afficherMessageGlobal('Veuillez corriger les erreurs avant de continuer', 'erreur');
        }
    }
    
    soumettreFormulaire() {
        const donnees = new FormData(this.formulaire);
        const objet = {};
        
        for (let [cle, valeur] of donnees.entries()) {
            objet[cle] = valeur;
        }
        
        console.log('✅ Formulaire valide, données:', objet);
        this.afficherMessageGlobal('Formulaire soumis avec succès!', 'succes');
        
        // Simulation d'envoi
        setTimeout(() => {
            this.afficherMessageGlobal('Données enregistrées!', 'info');
        }, 1000);
    }
    
    afficherMessageGlobal(message, type) {
        let divMessage = document.querySelector('[data-message-global]');
        
        if (!divMessage) {
            divMessage = document.createElement('div');
            divMessage.setAttribute('data-message-global', '');
            this.formulaire.insertBefore(divMessage, this.formulaire.firstChild);
        }
        
        const couleurs = {
            erreur: '#ff4444',
            succes: '#44ff44',
            info: '#4444ff'
        };
        
        divMessage.textContent = message;
        divMessage.style.cssText = `
            padding: 10px;
            margin: 10px 0;
            border-radius: 4px;
            background-color: ${couleurs[type]}20;
            color: ${couleurs[type]};
            border: 1px solid ${couleurs[type]};
        `;
        
        // Masquer après 3 secondes
        setTimeout(() => {
            divMessage.style.display = 'none';
        }, 3000);
    }
}

// Initialisation du formulaire
// const formulaire = new FormulaireInteractif('#formulaire-inscription');

// Code HTML suggéré pour tester:
const htmlSuggere = `
<form id="formulaire-inscription">
    <input name="prenom" placeholder="Prénom" required>
    <input name="nom" placeholder="Nom" required>
    <input name="email" type="email" placeholder="Email" required>
    <input name="telephone" placeholder="Téléphone">
    <input name="motDePasse" type="password" placeholder="Mot de passe" required>
    <input name="confirmationMotDePasse" type="password" placeholder="Confirmer le mot de passe" required>
    <button type="submit">S'inscrire</button>
</form>
`;

console.log("Code HTML suggéré pour tester le formulaire:");
console.log(htmlSuggere);
```

### Exemple avancé (optionnel)
```javascript
// Système de drag and drop avec événements personnalisés
console.log("=== SYSTÈME DRAG & DROP AVANCÉ ===");

class DragDropManager {
    constructor() {
        this.elementEnCoursDeDrag = null;
        this.zonesDepot = new Set();
        this.elementsTrainables = new Set();
        this.callbacks = {
            onDragStart: [],
            onDragEnd: [],
            onDrop: []
        };
        
        this.init();
    }
    
    init() {
        // Désactiver le drag and drop par défaut du navigateur sur certains éléments
        document.addEventListener('dragover', (event) => {
            event.preventDefault();
        });
        
        document.addEventListener('drop', (event) => {
            event.preventDefault();
        });
        
        console.log('🎯 Gestionnaire Drag & Drop initialisé');
    }
    
    // Rendre un élément draggable
    rendreTrainable(element, donnees = {}) {
        element.draggable = true;
        this.elementsTrainables.add(element);
        
        // Données à transporter
        element.dataset.dragData = JSON.stringify(donnees);
        
        // Événements de drag
        element.addEventListener('dragstart', (event) => {
            this.gererDebutDrag(event);
        });
        
        element.addEventListener('dragend', (event) => {
            this.gererFinDrag(event);
        });
        
        // Effet visuel
        element.style.cursor = 'grab';
        element.classList.add('element-trainable');
        
        console.log(`✅ Élément rendu traînable:`, element);
    }
    
    // Définir une zone de dépôt
    definirZoneDepot(element, options = {}) {
        this.zonesDepot.add(element);
        element.classList.add('zone-depot');
        
        // Configuration
        const config = {
            accepte: options.accepte || ['*'], // Types acceptés
            multiple: options.multiple || true,
            callback: options.callback || null,
            ...options
        };
        
        element.dataset.dropConfig = JSON.stringify(config);
        
        // Événements de drop
        element.addEventListener('dragover', (event) => {
            this.gererSurvol(event);
        });
        
        element.addEventListener('dragenter', (event) => {
            this.gererEntree(event);
        });
        
        element.addEventListener('dragleave', (event) => {
            this.gererSortie(event);
        });
        
        element.addEventListener('drop', (event) => {
            this.gererDepot(event);
        });
        
        console.log(`📦 Zone de dépôt définie:`, element);
    }
    
    gererDebutDrag(event) {
        this.elementEnCoursDeDrag = event.target;
        
        // Données à transférer
        const donnees = event.target.dataset.dragData || '{}';
        event.dataTransfer.setData('application/json', donnees);
        event.dataTransfer.setData('text/plain', event.target.textContent);
        
        // Effet visuel
        event.target.style.opacity = '0.5';
        event.target.style.cursor = 'grabbing';
        
        // Déclencher les callbacks
        this.declencherCallbacks('onDragStart', {
            element: event.target,
            donnees: JSON.parse(donnees)
        });
        
        console.log('🎬 Début du drag:', event.target);
    }
    
    gererFinDrag(event) {
        // Restaurer l'apparence
        event.target.style.opacity = '';
        event.target.style.cursor = 'grab';
        
        // Nettoyer les effets visuels des zones
        this.zonesDepot.forEach(zone => {
            zone.classList.remove('zone-depot-survol', 'zone-depot-valide');
        });
        
        // Déclencher les callbacks
        this.declencherCallbacks('onDragEnd', {
            element: event.target
        });
        
        this.elementEnCoursDeDrag = null;
        console.log('🏁 Fin du drag');
    }
    
    gererSurvol(event) {
        if (!this.elementEnCoursDeDrag) return;
        
        event.preventDefault();
        
        const zone = event.currentTarget;
        const config = JSON.parse(zone.dataset.dropConfig || '{}');
        
        // Vérifier si l'élément peut être déposé ici
        if (this.peutEtreDepose(this.elementEnCoursDeDrag, zone, config)) {
            event.dataTransfer.dropEffect = 'move';
            zone.classList.add('zone-depot-valide');
        } else {
            event.dataTransfer.dropEffect = 'none';
            zone.classList.remove('zone-depot-valide');
        }
    }
    
    gererEntree(event) {
        if (!this.elementEnCoursDeDrag) return;
        
        event.currentTarget.classList.add('zone-depot-survol');
    }
    
    gererSortie(event) {
        // Vérifier si on sort vraiment de la zone (pas d'un enfant)
        if (!event.currentTarget.contains(event.relatedTarget)) {
            event.currentTarget.classList.remove('zone-depot-survol', 'zone-depot-valide');
        }
    }
    
    gererDepot(event) {
        event.preventDefault();
        
        const zone = event.currentTarget;
        const config = JSON.parse(zone.dataset.dropConfig || '{}');
        
        if (!this.peutEtreDepose(this.elementEnCoursDeDrag, zone, config)) {
            console.log('❌ Dépôt refusé');
            return;
        }
        
        // Récupérer les données
        const donneesJSON = event.dataTransfer.getData('application/json');
        const donnees = donneesJSON ? JSON.parse(donneesJSON) : {};
        
        // Créer l'événement de dépôt
        const evenementDepot = {
            element: this.elementEnCoursDeDrag,
            zone: zone,
            donnees: donnees,
            position: {
                x: event.clientX,
                y: event.clientY
            }
        };
        
        // Callback spécifique à la zone
        if (config.callback) {
            config.callback(evenementDepot);
        }
        
        // Callbacks globaux
        this.declencherCallbacks('onDrop', evenementDepot);
        
        // Nettoyer l'apparence
        zone.classList.remove('zone-depot-survol', 'zone-depot-valide');
        
        console.log('📦 Dépôt effectué:', evenementDepot);
    }
    
    peutEtreDepose(element, zone, config) {
        // Vérifier les types acceptés
        const typeElement = element.dataset.type || 'default';
        const typesAcceptes = config.accepte || ['*'];
        
        if (!typesAcceptes.includes('*') && !typesAcceptes.includes(typeElement)) {
            return false;
        }
        
        // Vérifier le nombre d'éléments si multiple = false
        if (!config.multiple && zone.children.length > 0) {
            return false;
        }
        
        return true;
    }
    
    // Système de callbacks
    on(evenement, callback) {
        if (this.callbacks[evenement]) {
            this.callbacks[evenement].push(callback);
        }
    }
    
    off(evenement, callback) {
        if (this.callbacks[evenement]) {
            const index = this.callbacks[evenement].indexOf(callback);
            if (index > -1) {
                this.callbacks[evenement].splice(index, 1);
            }
        }
    }
    
    declencherCallbacks(evenement, donnees) {
        if (this.callbacks[evenement]) {
            this.callbacks[evenement].forEach(callback => {
                try {
                    callback(donnees);
                } catch (error) {
                    console.error(`Erreur dans callback ${evenement}:`, error);
                }
            });
        }
    }
    
    // Créer des éléments traînables dynamiquement
    creerElementTrainable(texte, type = 'default', donnees = {}) {
        const element = document.createElement('div');
        element.textContent = texte;
        element.dataset.type = type;
        element.style.cssText = `
            padding: 10px;
            margin: 5px;
            background: #f0f0f0;
            border: 1px solid #ccc;
            border-radius: 4px;
            display: inline-block;
            user-select: none;
        `;
        
        this.rendreTrainable(element, { type, ...donnees });
        
        return element;
    }
    
    // Créer une zone de dépôt
    creerZoneDepot(titre, options = {}) {
        const zone = document.createElement('div');
        zone.innerHTML = `
            <h3>${titre}</h3>
            <div class="contenu-zone" style="min-height: 100px; border: 2px dashed #ccc; padding: 10px;"></div>
        `;
        
        zone.style.cssText = `
            margin: 10px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        `;
        
        const zoneDepot = zone.querySelector('.contenu-zone');
        this.definirZoneDepot(zoneDepot, options);
        
        return zone;
    }
}

// Utilisation et démonstration
const ddManager = new DragDropManager();

// Créer une interface de test
function creerInterfaceTest() {
    const conteneur = document.createElement('div');
    conteneur.innerHTML = `
        <h2>Test Drag & Drop</h2>
        <div id="elements-source" style="display: flex; flex-wrap: wrap;">
            <h3 style="width: 100%;">Éléments à traîner:</h3>
        </div>
        <div id="zones-depot" style="display: flex; gap: 20px;">
        </div>
    `;
    
    document.body.appendChild(conteneur);
    
    // Créer des éléments traînables
    const sourceContainer = document.getElementById('elements-source');
    
    ['Tâche 1', 'Tâche 2', 'Tâche 3'].forEach((texte, index) => {
        const element = ddManager.creerElementTrainable(texte, 'tache', { id: index });
        sourceContainer.appendChild(element);
    });
    
    ['Fichier A', 'Fichier B'].forEach((texte, index) => {
        const element = ddManager.creerElementTrainable(texte, 'fichier', { id: index });
        sourceContainer.appendChild(element);
    });
    
    // Créer des zones de dépôt
    const zonesContainer = document.getElementById('zones-depot');
    
    const zoneTaches = ddManager.creerZoneDepot('À faire', {
        accepte: ['tache'],
        callback: (event) => {
            console.log('✅ Tâche déposée dans "À faire":', event.element.textContent);
        }
    });
    
    const zoneFichiers = ddManager.creerZoneDepot('Fichiers', {
        accepte: ['fichier'],
        callback: (event) => {
            console.log('📁 Fichier déposé:', event.element.textContent);
        }
    });
    
    const zonePoubelle = ddManager.creerZoneDepot('🗑️ Poubelle', {
        accepte: ['*'],
        callback: (event) => {
            console.log('🗑️ Élément supprimé:', event.element.textContent);
            event.element.remove();
        }
    });
    
    zonesContainer.appendChild(zoneTaches);
    zonesContainer.appendChild(zoneFichiers);
    zonesContainer.appendChild(zonePoubelle);
}

// Ajouter des callbacks globaux
ddManager.on('onDragStart', (data) => {
    console.log('🎬 Drag commencé globalement:', data.element.textContent);
});

ddManager.on('onDrop', (data) => {
    console.log('📦 Drop global:', {
        element: data.element.textContent,
        zone: data.zone.parentElement.querySelector('h3').textContent
    });
});

// Créer l'interface si on est dans un navigateur
if (typeof document !== 'undefined') {
    // creerInterfaceTest();
}

console.log("Système Drag & Drop prêt!");
console.log("Utilisez creerInterfaceTest() pour créer une interface de test");
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Calculatrice interactive
**Consigne :** Créez une calculatrice fonctionnelle avec gestion complète des événements.

**Code de départ :**
```javascript
// TODO: Implémenter une calculatrice avec :
// - Boutons cliquables ET saisie clavier
// - Gestion des erreurs (division par zéro)
// - Historique des calculs
// - Raccourcis clavier (Entrée = calcul, Échap = reset)
// - Feedback visuel sur les actions

const calculatrice = {
    valeurCourante: '0',
    operateur: null,
    valeurPrecedente: null,
    
    init() {
        // Initialiser les événements
    },
    
    ajouterChiffre(chiffre) {
        // Ajouter un chiffre
    },
    
    effectuerOperation(op) {
        // Effectuer l'opération
    }
};
```

**Résultat attendu :**
- Interface réactive aux clics et au clavier
- Gestion complète des erreurs
- Historique persistant

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Galerie d'images avec navigation
**Consigne :** Créez une galerie d'images avec navigation au clavier et à la souris.

**Contraintes :**
- Navigation avec flèches du clavier
- Zoom avec molette de souris
- Diaporama automatique
- Gestion des gestes tactiles (bonus)

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Éditeur de texte basique (optionnel)
**Consigne :** Implémentez un éditeur de texte avec raccourcis clavier.

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
class CalculatriceInteractive {
    constructor(conteneurSelector) {
        this.conteneur = document.querySelector(conteneurSelector);
        this.valeurCourante = '0';
        this.valeurPrecedente = null;
        this.operateur = null;
        this.dernierResultat = null;
        this.historique = [];
        this.nouveauNombre = true;
        
        this.init();
    }
    
    init() {
        this.creerInterface();
        this.ajouterEvenements();
        this.mettreAJourAffichage();
        
        console.log('🔢 Calculatrice initialisée');
    }
    
    creerInterface() {
        this.conteneur.innerHTML = `
            <div class="calculatrice">
                <div class="ecran">
                    <div class="affichage-principal">0</div>
                    <div class="affichage-operation"></div>
                </div>
                <div class="boutons">
                    <button data-action="clear" class="btn-fonction">C</button>
                    <button data-action="delete" class="btn-fonction">⌫</button>
                    <button data-action="operation" data-op="/" class="btn-operateur">÷</button>
                    <button data-action="operation" data-op="*" class="btn-operateur">×</button>
                    
                    <button data-action="chiffre" data-chiffre="7" class="btn-chiffre">7</button>
                    <button data-action="chiffre" data-chiffre="8" class="btn-chiffre">8</button>
                    <button data-action="chiffre" data-chiffre="9" class="btn-chiffre">9</button>
                    <button data-action="operation" data-op="-" class="btn-operateur">-</button>
                    
                    <button data-action="chiffre" data-chiffre="4" class="btn-chiffre">4</button>
                    <button data-action="chiffre" data-chiffre="5" class="btn-chiffre">5</button>
                    <button data-action="chiffre" data-chiffre="6" class="btn-chiffre">6</button>
                    <button data-action="operation" data-op="+" class="btn-operateur">+</button>
                    
                    <button data-action="chiffre" data-chiffre="1" class="btn-chiffre">1</button>
                    <button data-action="chiffre" data-chiffre="2" class="btn-chiffre">2</button>
                    <button data-action="chiffre" data-chiffre="3" class="btn-chiffre">3</button>
                    <button data-action="equals" class="btn-equals" rowspan="2">=</button>
                    
                    <button data-action="chiffre" data-chiffre="0" class="btn-zero">0</button>
                    <button data-action="decimal" class="btn-chiffre">.</button>
                </div>
                <div class="historique">
                    <h4>Historique</h4>
                    <div class="liste-historique"></div>
                </div>
            </div>
        `;
        
        // Styles CSS
        const style = document.createElement('style');
        style.textContent = `
            .calculatrice {
                max-width: 300px;
                margin: 20px auto;
                border: 1px solid #ccc;
                border-radius: 10px;
                padding: 20px;
                background: #f9f9f9;
            }
            .ecran {
                background: #000;
                color: #fff;
                padding: 15px;
                margin-bottom: 15px;
                border-radius: 5px;
                text-align: right;
            }
            .affichage-principal {
                font-size: 2em;
                font-weight: bold;
            }
            .affichage-operation {
                font-size: 0.8em;
                color: #ccc;
                height: 1em;
            }
            .boutons {
                display: grid;
                grid-template-columns: repeat(4, 1fr);
                gap: 10px;
                margin-bottom: 20px;
            }
            .boutons button {
                padding: 15px;
                font-size: 1.1em;
                border: none;
                border-radius: 5px;
                cursor: pointer;
                transition: all 0.2s;
            }
            .btn-chiffre {
                background: #e0e0e0;
            }
            .btn-operateur {
                background: #ff9500;
                color: white;
            }
            .btn-fonction {
                background: #a6a6a6;
                color: white;
            }
            .btn-equals {
                background: #ff9500;
                color: white;
                grid-row: span 2;
            }
            .btn-zero {
                grid-column: span 2;
            }
            .boutons button:hover {
                transform: scale(1.05);
                box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            }
            .boutons button:active {
                transform: scale(0.95);
            }
            .historique {
                max-height: 150px;
                overflow-y: auto;
                border-top: 1px solid #ccc;
                padding-top: 10px;
            }
            .historique h4 {
                margin-top: 0;
            }
            .entree-historique {
                padding: 5px;
                margin: 2px 0;
                background: #f0f0f0;
                border-radius: 3px;
                font-family: monospace;
                cursor: pointer;
            }
            .entree-historique:hover {
                background: #e0e0e0;
            }
        `;
        document.head.appendChild(style);
    }
    
    ajouterEvenements() {
        // Événements de clic sur les boutons
        this.conteneur.addEventListener('click', (event) => {
            if (event.target.matches('button')) {
                this.gererClicBouton(event.target);
                this.animerBouton(event.target);
            }
        });
        
        // Événements clavier
        document.addEventListener('keydown', (event) => {
            this.gererClavierCalculatrice(event);
        });
        
        // Clic sur l'historique
        this.conteneur.addEventListener('click', (event) => {
            if (event.target.matches('.entree-historique')) {
                this.chargerDepuisHistorique(event.target.textContent);
            }
        });
    }
    
    gererClicBouton(bouton) {
        const action = bouton.dataset.action;
        
        switch (action) {
            case 'chiffre':
                this.ajouterChiffre(bouton.dataset.chiffre);
                break;
            case 'operation':
                this.definirOperation(bouton.dataset.op);
                break;
            case 'equals':
                this.calculer();
                break;
            case 'decimal':
                this.ajouterDecimal();
                break;
            case 'clear':
                this.effacer();
                break;
            case 'delete':
                this.supprimerDernier();
                break;
        }
        
        this.mettreAJourAffichage();
    }
    
    gererClavierCalculatrice(event) {
        // Empêcher les raccourcis par défaut pour certaines touches
        if ('0123456789+-*/.='.includes(event.key) || 
            ['Enter', 'Escape', 'Backspace'].includes(event.key)) {
            event.preventDefault();
        }
        
        if ('0123456789'.includes(event.key)) {
            this.ajouterChiffre(event.key);
        } else if ('+-'.includes(event.key)) {
            this.definirOperation(event.key);
        } else if (event.key === '*') {
            this.definirOperation('*');
        } else if (event.key === '/') {
            this.definirOperation('/');
        } else if (event.key === '.' || event.key === ',') {
            this.ajouterDecimal();
        } else if (event.key === 'Enter' || event.key === '=') {
            this.calculer();
        } else if (event.key === 'Escape') {
            this.effacer();
        } else if (event.key === 'Backspace') {
            this.supprimerDernier();
        }
        
        this.mettreAJourAffichage();
        
        // Feedback visuel pour les touches
        this.animerToucheClavier(event.key);
    }
    
    ajouterChiffre(chiffre) {
        if (this.nouveauNombre) {
            this.valeurCourante = chiffre;
            this.nouveauNombre = false;
        } else {
            this.valeurCourante = this.valeurCourante === '0' ? chiffre : this.valeurCourante + chiffre;
        }
    }
    
    ajouterDecimal() {
        if (this.nouveauNombre) {
            this.valeurCourante = '0.';
            this.nouveauNombre = false;
        } else if (!this.valeurCourante.includes('.')) {
            this.valeurCourante += '.';
        }
    }
    
    definirOperation(op) {
        if (this.operateur && !this.nouveauNombre) {
            this.calculer();
        }
        
        this.valeurPrecedente = this.valeurCourante;
        this.operateur = op;
        this.nouveauNombre = true;
    }
    
    calculer() {
        if (!this.operateur || this.valeurPrecedente === null) return;
        
        const prev = parseFloat(this.valeurPrecedente);
        const current = parseFloat(this.valeurCourante);
        let resultat;
        
        try {
            switch (this.operateur) {
                case '+':
                    resultat = prev + current;
                    break;
                case '-':
                    resultat = prev - current;
                    break;
                case '*':
                    resultat = prev * current;
                    break;
                case '/':
                    if (current === 0) {
                        throw new Error('Division par zéro');
                    }
                    resultat = prev / current;
                    break;
                default:
                    return;
            }
            
            const expression = `${this.valeurPrecedente} ${this.operateur} ${this.valeurCourante} = ${resultat}`;
            this.ajouterAHistorique(expression);
            
            this.valeurCourante = resultat.toString();
            this.dernierResultat = resultat;
            
        } catch (error) {
            this.valeurCourante = 'Erreur';
            console.error('Erreur de calcul:', error.message);
        }
        
        this.operateur = null;
        this.valeurPrecedente = null;
        this.nouveauNombre = true;
    }
    
    effacer() {
        this.valeurCourante = '0';
        this.valeurPrecedente = null;
        this.operateur = null;
        this.nouveauNombre = true;
    }
    
    supprimerDernier() {
        if (this.valeurCourante.length > 1) {
            this.valeurCourante = this.valeurCourante.slice(0, -1);
        } else {
            this.valeurCourante = '0';
        }
    }
    
    mettreAJourAffichage() {
        const affichagePrincipal = this.conteneur.querySelector('.affichage-principal');
        const affichageOperation = this.conteneur.querySelector('.affichage-operation');
        
        affichagePrincipal.textContent = this.valeurCourante;
        
        if (this.operateur && this.valeurPrecedente) {
            affichageOperation.textContent = `${this.valeurPrecedente} ${this.operateur}`;
        } else {
            affichageOperation.textContent = '';
        }
    }
    
    ajouterAHistorique(expression) {
        this.historique.unshift(expression);
        if (this.historique.length > 10) {
            this.historique = this.historique.slice(0, 10);
        }
        
        this.mettreAJourAffichageHistorique();
    }
    
    mettreAJourAffichageHistorique() {
        const listeHistorique = this.conteneur.querySelector('.liste-historique');
        listeHistorique.innerHTML = this.historique
            .map(entree => `<div class="entree-historique">${entree}</div>`)
            .join('');
    }
    
    chargerDepuisHistorique(expression) {
        const resultat = expression.split(' = ')[1];
        if (resultat && !isNaN(resultat)) {
            this.valeurCourante = resultat;
            this.nouveauNombre = true;
            this.mettreAJourAffichage();
        }
    }
    
    animerBouton(bouton) {
        bouton.style.transform = 'scale(0.95)';
        setTimeout(() => {
            bouton.style.transform = '';
        }, 150);
    }
    
    animerToucheClavier(touche) {
        // Trouver le bouton correspondant et l'animer
        let selecteur = '';
        
        if ('0123456789'.includes(touche)) {
            selecteur = `[data-chiffre="${touche}"]`;
        } else if (touche === '+') {
            selecteur = '[data-op="+"]';
        } else if (touche === '-') {
            selecteur = '[data-op="-"]';
        } else if (touche === '*') {
            selecteur = '[data-op="*"]';
        } else if (touche === '/') {
            selecteur = '[data-op="/"]';
        } else if (touche === 'Enter' || touche === '=') {
            selecteur = '[data-action="equals"]';
        } else if (touche === 'Escape') {
            selecteur = '[data-action="clear"]';
        } else if (touche === 'Backspace') {
            selecteur = '[data-action="delete"]';
        }
        
        if (selecteur) {
            const bouton = this.conteneur.querySelector(selecteur);
            if (bouton) {
                this.animerBouton(bouton);
            }
        }
    }
}

// Utilisation
// const calc = new CalculatriceInteractive('#calculatrice-conteneur');

console.log("Calculatrice interactive prête!");
console.log("- Utilisez la souris ou le clavier");
console.log("- Touches: 0-9, +, -, *, /, Entrée, Échap, Retour arrière");
console.log("- Cliquez sur l'historique pour réutiliser un résultat");
```

**Explication :** La calculatrice gère tous les événements (clics, clavier) avec feedback visuel, validation d'erreurs et historique persistant.

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **DOM** : Sélection et manipulation d'éléments
- **Fonctions** : Event handlers et callbacks
- **Objets** : Propriétés de l'objet Event
- **This** : Contexte dans les gestionnaires d'événements

### Prochaines étapes
- **Programmation asynchrone** : Événements avec Promise et async/await
- **Frameworks** : Systèmes d'événements dans React, Vue, etc.
- **Web APIs** : Intersection Observer, Resize Observer

### Concepts complémentaires
- **Accessibilité** : Événements clavier pour navigation accessible
- **Performance** : Optimisation des événements (debouncing, throttling)
- **Testing** : Tests d'interaction utilisateur

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un jeu de memory interactif avec gestion complète des événements

**Fonctionnalités requises :**
- [ ] Grille de cartes cliquables
- [ ] Animation de retournement
- [ ] Détection des paires
- [ ] Système de score et timer
- [ ] Navigation au clavier (accessibilité)
- [ ] Niveaux de difficulté
- [ ] Son et effets visuels

**Structure suggérée :**
```
📁 memory-game/
├── 📄 index.html
├── 📄 style.css
├── 📄 game.js
├── 📄 events.js
└── 📄 sounds/
```

**Temps estimé :** 90 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Event reference](https://developer.mozilla.org/fr/docs/Web/Events)
- [MDN - EventTarget.addEventListener()](https://developer.mozilla.org/fr/docs/Web/API/EventTarget/addEventListener)
- [MDN - Event.preventDefault()](https://developer.mozilla.org/fr/docs/Web/API/Event/preventDefault)

### Vidéos recommandées
- "JavaScript Events Explained" - Web Dev Simplified (20 min)
- Pourquoi cette vidéo est utile : Explications claires avec exemples pratiques

### Articles approfondis
- "Event Delegation in JavaScript" - JavaScript.info
- Ce que cet article apporte en plus : Techniques avancées de délégation

### Outils de pratique
- [JavaScript Event Playground](https://codepen.io/collection/XNdVra)
- [Event Listener Exercises](https://github.com/javascript-tutorial/event-basic)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre event.target et event.currentTarget ?
   - Réponse : target = élément qui a déclenché, currentTarget = élément avec le listener

2. **Question pratique :** Comment empêcher la propagation d'un événement ?
   - Réponse : event.stopPropagation() ou event.stopImmediatePropagation()

3. **Question d'application :** Qu'est-ce que la délégation d'événements ?
   - Réponse : Écouter sur un parent pour gérer les événements des enfants dynamiques

### Checklist de maîtrise
- [ ] Je comprends les différents types d'événements
- [ ] Je sais utiliser addEventListener et removeEventListener
- [ ] Je maîtrise l'objet Event et ses propriétés
- [ ] Je comprends la propagation et le bubbling
- [ ] Je peux implémenter la délégation d'événements
- [ ] Je gère correctement les événements clavier pour l'accessibilité

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
// Fuite mémoire avec addEventListener
function ajouterEvenements() {
    document.addEventListener('click', function() {
        console.log('Clic détecté');
    });
}

// Appelé plusieurs fois
ajouterEvenements();
ajouterEvenements();
ajouterEvenements();
```

**Solution :**
```javascript
let clickHandler = function() {
    console.log('Clic détecté');
};

function ajouterEvenements() {
    // Supprimer avant d'ajouter
    document.removeEventListener('click', clickHandler);
    document.addEventListener('click', clickHandler);
}
```

**Explication :** Toujours supprimer les anciens listeners avant d'en ajouter de nouveaux.

### Erreur fréquente 2
**Code problématique :**
```javascript
// Contexte this perdu
class MonComposant {
    constructor() {
        this.valeur = 42;
        document.addEventListener('click', this.gererClic); // ❌ this sera undefined
    }
    
    gererClic() {
        console.log(this.valeur); // undefined
    }
}
```

**Solution :**
```javascript
class MonComposant {
    constructor() {
        this.valeur = 42;
        document.addEventListener('click', this.gererClic.bind(this)); // ✅
        // ou
        document.addEventListener('click', (event) => this.gererClic(event)); // ✅
    }
    
    gererClic() {
        console.log(this.valeur); // 42
    }
}
```

**Explication :** Bind ou arrow functions pour préserver le contexte this.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Le système complet d'événements JavaScript
- L'importance de la délégation pour les performances
- Les techniques d'optimisation des événements

### Questions à approfondir
- Événements touch pour les mobiles
- Web Workers et événements

### Révision programmée
- [ ] Révision dans 1 jour (rappel rapide)
- [ ] Révision dans 1 semaine (exercices)
- [ ] Révision dans 1 mois (application dans un projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #events #dom #interactivite #listeners #intermediaire

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 10.5/126*
