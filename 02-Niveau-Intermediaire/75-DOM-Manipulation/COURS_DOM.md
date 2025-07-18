# DOM (Document Object Model) en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** DOM - Manipuler les pages web avec JavaScript
- **Niveau :** INTERMEDIAIRE (transition vers l'interactif)
- **Duree estimee :** 140 minutes
- **Prerequis :** Variables, fonctions, objets, events de base
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est le DOM et comment JavaScript interagit avec HTML
- [ ] **Selectionner** : Trouver des elements HTML depuis JavaScript
- [ ] **Modifier** : Changer le contenu, les styles, et les attributs
- [ ] **Creer** : Rendre tes pages web interactives et dynamiques

### Mots-cles a retenir
- **DOM** : Representation de la page web que JavaScript peut manipuler
- **Element** : Une balise HTML (div, p, button, etc.)
- **Selector** : Moyen de trouver un element (id, class, tag)
- **innerHTML** : Contenu HTML d'un element
- **textContent** : Texte pur d'un element (sans HTML)
- **addEventListener** : Ecouter les clics, survols, etc.

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Imagine que ta page web est comme une maison, et JavaScript est comme toi qui peux :
- **Allumer/eteindre** les lumieres (changer les couleurs)
- **Deplacer** les meubles (modifier la position des elements)
- **Changer** les tableaux sur les murs (modifier le texte et les images)
- **Reagir** quand quelqu'un sonne (ecouter les clics)

Sans le DOM, JavaScript ne pourrait que faire des calculs dans la console. Avec le DOM, JavaScript peut transformer n'importe quelle page web en application interactive !

**Le DOM permet de :**
- **Modifier** le contenu en temps reel
- **Reagir** aux actions de l'utilisateur
- **Creer** des interfaces dynamiques
- **Valider** des formulaires instantanement

### Definition et concepts cles

**Le DOM est l'interface entre JavaScript et HTML :**

1. **Document** : Represente toute la page web
2. **Elements** : Chaque balise HTML devient un objet JavaScript
3. **Proprietes** : innerHTML, textContent, style, className
4. **Methodes** : getElementById(), querySelector(), addEventListener()

**Principales methodes de selection :**
- **getElementById()** : Trouve par ID unique
- **querySelector()** : Trouve par selecteur CSS
- **querySelectorAll()** : Trouve tous les elements correspondants
- **getElementsByClassName()** : Trouve par nom de classe

### Syntaxe de base
```javascript
// Trouver des elements
let titre = document.getElementById("monTitre");
let bouton = document.querySelector(".mon-bouton");
let tousLesParagraphes = document.querySelectorAll("p");

// Modifier le contenu
titre.textContent = "Nouveau titre !";
titre.innerHTML = "<strong>Titre en gras</strong>";

// Modifier le style
titre.style.color = "red";
titre.style.fontSize = "24px";

// Ecouter les evenements
bouton.addEventListener("click", function() {
    alert("Bouton clique !");
});
```

### Points d'attention
- **DOM ready** : Attendre que la page soit chargee avant de manipuler
- **Null check** : Verifier qu'un element existe avant de le modifier
- **Performance** : Eviter de chercher le meme element plusieurs fois
- **Separation** : Garder JavaScript, HTML et CSS bien separes

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```html
<!DOCTYPE html>
<html>
<head>
    <title>Mon Premier DOM</title>
    <style>
        .highlight { background-color: yellow; }
        .big-text { font-size: 20px; }
    </style>
</head>
<body>
    <h1 id="titre-principal">Bonjour le monde !</h1>
    <p id="message">Cliquez sur le bouton ci-dessous</p>
    <button id="mon-bouton">Cliquez-moi !</button>
    <p id="compteur">Clics: 0</p>

    <script>
        // Attendre que la page soit chargee
        document.addEventListener("DOMContentLoaded", function() {
            
            // Trouver les elements
            let titre = document.getElementById("titre-principal");
            let message = document.getElementById("message");
            let bouton = document.getElementById("mon-bouton");
            let compteur = document.getElementById("compteur");
            
            let nombreClics = 0;
            
            // Modifier le titre au debut
            titre.textContent = "Bienvenue sur ma page interactive !";
            titre.style.color = "blue";
            
            // Ajouter un evenement au bouton
            bouton.addEventListener("click", function() {
                nombreClics++;
                
                // Mettre a jour le compteur
                compteur.textContent = "Clics: " + nombreClics;
                
                // Changer le message selon le nombre de clics
                if (nombreClics === 1) {
                    message.textContent = "Premier clic ! Continuez...";
                    message.className = "highlight";
                } else if (nombreClics === 5) {
                    message.textContent = "Wow ! Vous aimez cliquer !";
                    message.className = "big-text highlight";
                } else if (nombreClics >= 10) {
                    message.textContent = "Vous etes un champion du clic !";
                    message.style.color = "green";
                    bouton.textContent = "Arretez de me cliquer !";
                }
            });
            
            console.log("Page chargee et JavaScript pret !");
        });
    </script>
</body>
</html>
```

### Exemple intermediaire
```html
<!DOCTYPE html>
<html>
<head>
    <title>Calculatrice Interactive</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 400px; margin: 50px auto; }
        .calculatrice { border: 2px solid #333; padding: 20px; border-radius: 10px; }
        .affichage { font-size: 24px; padding: 10px; background: #f0f0f0; margin-bottom: 10px; }
        .boutons { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
        .bouton { padding: 20px; font-size: 18px; border: none; border-radius: 5px; cursor: pointer; }
        .nombre { background-color: #e0e0e0; }
        .operateur { background-color: #ffab40; color: white; }
        .egal { background-color: #4caf50; color: white; }
        .effacer { background-color: #f44336; color: white; }
    </style>
</head>
<body>
    <div class="calculatrice">
        <h2>Ma Calculatrice</h2>
        <div id="affichage" class="affichage">0</div>
        
        <div class="boutons">
            <button class="bouton effacer" onclick="effacer()">C</button>
            <button class="bouton operateur" onclick="ajouterOperateur('/')">/</button>
            <button class="bouton operateur" onclick="ajouterOperateur('*')">*</button>
            <button class="bouton operateur" onclick="ajouterOperateur('-')">-</button>
            
            <button class="bouton nombre" onclick="ajouterNombre('7')">7</button>
            <button class="bouton nombre" onclick="ajouterNombre('8')">8</button>
            <button class="bouton nombre" onclick="ajouterNombre('9')">9</button>
            <button class="bouton operateur" onclick="ajouterOperateur('+')">+</button>
            
            <button class="bouton nombre" onclick="ajouterNombre('4')">4</button>
            <button class="bouton nombre" onclick="ajouterNombre('5')">5</button>
            <button class="bouton nombre" onclick="ajouterNombre('6')">6</button>
            <button class="bouton egal" onclick="calculer()" rowspan="2">=</button>
            
            <button class="bouton nombre" onclick="ajouterNombre('1')">1</button>
            <button class="bouton nombre" onclick="ajouterNombre('2')">2</button>
            <button class="bouton nombre" onclick="ajouterNombre('3')">3</button>
            
            <button class="bouton nombre" onclick="ajouterNombre('0')" style="grid-column: span 2;">0</button>
            <button class="bouton nombre" onclick="ajouterNombre('.')">.</button>
        </div>
        
        <div id="historique" style="margin-top: 20px;">
            <h3>Historique</h3>
            <ul id="liste-historique"></ul>
        </div>
    </div>

    <script>
        // Variables globales pour la calculatrice
        let affichage = document.getElementById("affichage");
        let listeHistorique = document.getElementById("liste-historique");
        let operationCourante = "";
        let dernierResultat = 0;
        
        // Fonction pour ajouter un nombre
        function ajouterNombre(nombre) {
            if (affichage.textContent === "0" || affichage.textContent === "Erreur") {
                affichage.textContent = nombre;
            } else {
                affichage.textContent += nombre;
            }
            operationCourante += nombre;
        }
        
        // Fonction pour ajouter un operateur
        function ajouterOperateur(operateur) {
            if (affichage.textContent !== "Erreur") {
                affichage.textContent += " " + operateur + " ";
                operationCourante += operateur;
            }
        }
        
        // Fonction pour calculer le resultat
        function calculer() {
            try {
                // ATTENTION: eval() est dangereux en production !
                // Ici c'est pour l'apprentissage seulement
                let resultat = eval(operationCourante);
                
                // Ajouter a l'historique
                ajouterHistorique(operationCourante + " = " + resultat);
                
                // Afficher le resultat
                affichage.textContent = resultat;
                dernierResultat = resultat;
                operationCourante = resultat.toString();
                
            } catch (erreur) {
                affichage.textContent = "Erreur";
                operationCourante = "";
                console.log("Erreur de calcul:", erreur);
            }
        }
        
        // Fonction pour effacer
        function effacer() {
            affichage.textContent = "0";
            operationCourante = "";
        }
        
        // Fonction pour ajouter a l'historique
        function ajouterHistorique(operation) {
            let nouveauElement = document.createElement("li");
            nouveauElement.textContent = operation;
            
            // Ajouter au debut de la liste
            if (listeHistorique.firstChild) {
                listeHistorique.insertBefore(nouveauElement, listeHistorique.firstChild);
            } else {
                listeHistorique.appendChild(nouveauElement);
            }
            
            // Limiter a 5 elements dans l'historique
            if (listeHistorique.children.length > 5) {
                listeHistorique.removeChild(listeHistorique.lastChild);
            }
        }
        
        // Ecouter les touches du clavier (bonus)
        document.addEventListener("keydown", function(event) {
            let touche = event.key;
            
            if (touche >= "0" && touche <= "9") {
                ajouterNombre(touche);
            } else if (touche === "+" || touche === "-" || touche === "*" || touche === "/") {
                ajouterOperateur(touche);
            } else if (touche === "Enter" || touche === "=") {
                calculer();
            } else if (touche === "Escape" || touche === "c" || touche === "C") {
                effacer();
            }
        });
        
        console.log("Calculatrice prete ! Vous pouvez aussi utiliser le clavier.");
    </script>
</body>
</html>
```

### Exemple avance (optionnel)
```html
<!DOCTYPE html>
<html>
<head>
    <title>Gestionnaire de Taches Dynamique</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
        .conteneur { border: 1px solid #ddd; padding: 20px; border-radius: 8px; }
        .formulaire { display: flex; gap: 10px; margin-bottom: 20px; }
        .input-tache { flex: 1; padding: 10px; border: 1px solid #ccc; border-radius: 4px; }
        .btn { padding: 10px 15px; border: none; border-radius: 4px; cursor: pointer; }
        .btn-ajouter { background-color: #4caf50; color: white; }
        .btn-supprimer { background-color: #f44336; color: white; font-size: 12px; }
        .tache { display: flex; align-items: center; padding: 10px; margin: 5px 0; border-radius: 4px; }
        .tache.terminee { background-color: #e8f5e8; text-decoration: line-through; opacity: 0.7; }
        .tache.en-cours { background-color: #fff3cd; }
        .checkbox { margin-right: 10px; }
        .texte-tache { flex: 1; }
        .statistiques { background-color: #f8f9fa; padding: 15px; margin-top: 20px; border-radius: 4px; }
        .filtres { margin: 10px 0; }
        .filtre { margin-right: 10px; padding: 5px 10px; border: 1px solid #ccc; background: white; }
        .filtre.actif { background-color: #007bff; color: white; }
    </style>
</head>
<body>
    <div class="conteneur">
        <h1>Gestionnaire de Taches</h1>
        
        <div class="formulaire">
            <input type="text" id="input-nouvelle-tache" class="input-tache" 
                   placeholder="Tapez votre nouvelle tache..." maxlength="100">
            <button id="btn-ajouter" class="btn btn-ajouter">Ajouter</button>
        </div>
        
        <div class="filtres">
            <span>Afficher: </span>
            <button class="filtre actif" data-filtre="toutes">Toutes</button>
            <button class="filtre" data-filtre="en-cours">En cours</button>
            <button class="filtre" data-filtre="terminees">Terminees</button>
        </div>
        
        <div id="liste-taches"></div>
        
        <div class="statistiques">
            <h3>Statistiques</h3>
            <p>Total: <span id="stat-total">0</span> tache(s)</p>
            <p>En cours: <span id="stat-en-cours">0</span> tache(s)</p>
            <p>Terminees: <span id="stat-terminees">0</span> tache(s)</p>
            <p>Pourcentage complete: <span id="stat-pourcentage">0</span>%</p>
        </div>
    </div>

    <script>
        // Variables globales
        let taches = [];
        let prochainId = 1;
        let filtreActuel = "toutes";
        
        // Elements du DOM
        let inputNouvelleTache = document.getElementById("input-nouvelle-tache");
        let btnAjouter = document.getElementById("btn-ajouter");
        let listeTaches = document.getElementById("liste-taches");
        let boutonsFiltre = document.querySelectorAll(".filtre");
        
        // Elements des statistiques
        let statTotal = document.getElementById("stat-total");
        let statEnCours = document.getElementById("stat-en-cours");
        let statTerminees = document.getElementById("stat-terminees");
        let statPourcentage = document.getElementById("stat-pourcentage");
        
        // Fonction pour creer une nouvelle tache
        function creerTache(texte) {
            return {
                id: prochainId++,
                texte: texte,
                terminee: false,
                dateCreation: new Date()
            };
        }
        
        // Fonction pour ajouter une tache
        function ajouterTache() {
            let texte = inputNouvelleTache.value.trim();
            
            if (texte === "") {
                alert("Veuillez saisir une tache !");
                return;
            }
            
            if (texte.length > 100) {
                alert("La tache est trop longue (max 100 caracteres) !");
                return;
            }
            
            let nouvelleTache = creerTache(texte);
            taches.push(nouvelleTache);
            
            inputNouvelleTache.value = "";
            
            afficherTaches();
            mettreAJourStatistiques();
            
            console.log("Tache ajoutee:", nouvelleTache);
        }
        
        // Fonction pour supprimer une tache
        function supprimerTache(id) {
            let index = taches.findIndex(tache => tache.id === id);
            
            if (index !== -1) {
                let tacheSupprimee = taches.splice(index, 1)[0];
                afficherTaches();
                mettreAJourStatistiques();
                console.log("Tache supprimee:", tacheSupprimee);
            }
        }
        
        // Fonction pour basculer l'etat d'une tache
        function basculerTache(id) {
            let tache = taches.find(t => t.id === id);
            
            if (tache) {
                tache.terminee = !tache.terminee;
                afficherTaches();
                mettreAJourStatistiques();
                console.log("Tache basculee:", tache);
            }
        }
        
        // Fonction pour filtrer les taches
        function filtrerTaches(filtre) {
            if (filtre === "toutes") {
                return taches;
            } else if (filtre === "en-cours") {
                return taches.filter(tache => !tache.terminee);
            } else if (filtre === "terminees") {
                return taches.filter(tache => tache.terminee);
            }
            return taches;
        }
        
        // Fonction pour afficher les taches
        function afficherTaches() {
            let tachesFiltrees = filtrerTaches(filtreActuel);
            
            // Vider la liste
            listeTaches.innerHTML = "";
            
            if (tachesFiltrees.length === 0) {
                let messageVide = document.createElement("p");
                messageVide.textContent = "Aucune tache a afficher.";
                messageVide.style.textAlign = "center";
                messageVide.style.color = "#666";
                listeTaches.appendChild(messageVide);
                return;
            }
            
            // Creer les elements pour chaque tache
            tachesFiltrees.forEach(tache => {
                let divTache = document.createElement("div");
                divTache.className = "tache " + (tache.terminee ? "terminee" : "en-cours");
                
                // Checkbox pour marquer comme terminee
                let checkbox = document.createElement("input");
                checkbox.type = "checkbox";
                checkbox.className = "checkbox";
                checkbox.checked = tache.terminee;
                checkbox.addEventListener("change", function() {
                    basculerTache(tache.id);
                });
                
                // Texte de la tache
                let texte = document.createElement("span");
                texte.className = "texte-tache";
                texte.textContent = tache.texte;
                
                // Bouton de suppression
                let btnSupprimer = document.createElement("button");
                btnSupprimer.className = "btn btn-supprimer";
                btnSupprimer.textContent = "Supprimer";
                btnSupprimer.addEventListener("click", function() {
                    if (confirm("Etes-vous sur de vouloir supprimer cette tache ?")) {
                        supprimerTache(tache.id);
                    }
                });
                
                // Assembler l'element
                divTache.appendChild(checkbox);
                divTache.appendChild(texte);
                divTache.appendChild(btnSupprimer);
                
                listeTaches.appendChild(divTache);
            });
        }
        
        // Fonction pour mettre a jour les statistiques
        function mettreAJourStatistiques() {
            let total = taches.length;
            let terminees = taches.filter(t => t.terminee).length;
            let enCours = total - terminees;
            let pourcentage = total > 0 ? Math.round((terminees / total) * 100) : 0;
            
            statTotal.textContent = total;
            statEnCours.textContent = enCours;
            statTerminees.textContent = terminees;
            statPourcentage.textContent = pourcentage;
        }
        
        // Fonction pour changer le filtre
        function changerFiltre(nouveauFiltre) {
            filtreActuel = nouveauFiltre;
            
            // Mettre a jour l'apparence des boutons
            boutonsFiltre.forEach(btn => {
                if (btn.dataset.filtre === nouveauFiltre) {
                    btn.classList.add("actif");
                } else {
                    btn.classList.remove("actif");
                }
            });
            
            afficherTaches();
        }
        
        // Evenements
        btnAjouter.addEventListener("click", ajouterTache);
        
        inputNouvelleTache.addEventListener("keydown", function(event) {
            if (event.key === "Enter") {
                ajouterTache();
            }
        });
        
        boutonsFiltre.forEach(btn => {
            btn.addEventListener("click", function() {
                changerFiltre(this.dataset.filtre);
            });
        });
        
        // Sauvegarder dans localStorage (bonus)
        function sauvegarder() {
            localStorage.setItem("mes-taches", JSON.stringify(taches));
            localStorage.setItem("prochain-id", prochainId.toString());
        }
        
        function charger() {
            let tachesSauvees = localStorage.getItem("mes-taches");
            let idSauve = localStorage.getItem("prochain-id");
            
            if (tachesSauvees) {
                taches = JSON.parse(tachesSauvees);
                afficherTaches();
                mettreAJourStatistiques();
            }
            
            if (idSauve) {
                prochainId = parseInt(idSauve);
            }
        }
        
        // Auto-sauvegarde a chaque modification
        let sauvegardeOriginal = ajouterTache;
        ajouterTache = function() {
            sauvegardeOriginal();
            sauvegarder();
        };
        
        // Charger au demarrage
        document.addEventListener("DOMContentLoaded", function() {
            charger();
            console.log("Gestionnaire de taches charge !");
        });
        
        // Sauvegarder avant de quitter
        window.addEventListener("beforeunload", sauvegarder);
    </script>
</body>
</html>
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Changeur de couleurs
**Consigne :** Cree une page avec des boutons qui changent la couleur de fond

**Elements a inclure :**
- 5 boutons de couleurs differentes
- Zone d'affichage de la couleur actuelle
- Compteur de changements de couleur

**Niveau de difficulte :** 2 etoiles sur 5

### Exercice 2 : Formulaire de contact dynamique
**Consigne :** Cree un formulaire qui valide en temps reel

**Fonctionnalites :**
- Validation nom, email, message
- Messages d'erreur personnalises
- Compteur de caracteres pour le message
- Apercu du message formate

**Niveau de difficulte :** 4 etoiles sur 5

### Exercice 3 : Mini-jeu de memoire
**Consigne :** Cree un jeu de memoire avec des cartes retournables

**Specifications :**
- Grille de cartes cachees
- Retourner les cartes au clic
- Detecter les paires
- Score et temps de jeu

**Niveau de difficulte :** 5 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```html
<!DOCTYPE html>
<html>
<head>
    <title>Changeur de Couleurs</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            text-align: center; 
            padding: 50px;
            transition: background-color 0.3s ease;
        }
        .conteneur { 
            max-width: 500px; 
            margin: 0 auto; 
            background: rgba(255,255,255,0.9);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .boutons { margin: 20px 0; }
        .bouton-couleur {
            padding: 15px 25px;
            margin: 10px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            color: white;
            font-weight: bold;
            transition: transform 0.2s;
        }
        .bouton-couleur:hover { transform: scale(1.1); }
        .rouge { background-color: #e74c3c; }
        .bleu { background-color: #3498db; }
        .vert { background-color: #2ecc71; }
        .jaune { background-color: #f1c40f; color: black; }
        .violet { background-color: #9b59b6; }
        .affichage {
            font-size: 20px;
            margin: 20px 0;
            padding: 15px;
            border: 2px solid #333;
            border-radius: 8px;
            background: white;
        }
        .statistiques {
            margin-top: 20px;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div class="conteneur">
        <h1>Changeur de Couleurs Magique</h1>
        <p>Cliquez sur un bouton pour changer la couleur de fond !</p>
        
        <div class="boutons">
            <button class="bouton-couleur rouge" onclick="changerCouleur('#e74c3c', 'Rouge')">Rouge</button>
            <button class="bouton-couleur bleu" onclick="changerCouleur('#3498db', 'Bleu')">Bleu</button>
            <button class="bouton-couleur vert" onclick="changerCouleur('#2ecc71', 'Vert')">Vert</button>
            <button class="bouton-couleur jaune" onclick="changerCouleur('#f1c40f', 'Jaune')">Jaune</button>
            <button class="bouton-couleur violet" onclick="changerCouleur('#9b59b6', 'Violet')">Violet</button>
        </div>
        
        <div class="affichage">
            <div id="couleur-actuelle">Couleur actuelle: Blanche</div>
            <div id="code-couleur">Code: #ffffff</div>
        </div>
        
        <div class="statistiques">
            <div>Changements: <span id="compteur">0</span></div>
            <div>Couleur preferee: <span id="couleur-preferee">Aucune</span></div>
        </div>
        
        <button onclick="reinitialiser()" style="margin-top: 20px; padding: 10px 20px;">Reinitialiser</button>
    </div>

    <script>
        // Variables globales
        let compteurChangements = 0;
        let statistiquesCouleurs = {
            'Rouge': 0,
            'Bleu': 0,
            'Vert': 0,
            'Jaune': 0,
            'Violet': 0
        };
        
        // Elements DOM
        let body = document.body;
        let couleurActuelle = document.getElementById("couleur-actuelle");
        let codeCouleur = document.getElementById("code-couleur");
        let compteur = document.getElementById("compteur");
        let couleurPreferee = document.getElementById("couleur-preferee");
        
        // Fonction pour changer la couleur
        function changerCouleur(code, nom) {
            // Changer la couleur de fond
            body.style.backgroundColor = code;
            
            // Mettre a jour l'affichage
            couleurActuelle.textContent = "Couleur actuelle: " + nom;
            codeCouleur.textContent = "Code: " + code;
            
            // Mettre a jour les statistiques
            compteurChangements++;
            statistiquesCouleurs[nom]++;
            
            compteur.textContent = compteurChangements;
            
            // Determiner la couleur preferee
            let maxUtilisation = 0;
            let couleurMax = "Aucune";
            
            for (let couleur in statistiquesCouleurs) {
                if (statistiquesCouleurs[couleur] > maxUtilisation) {
                    maxUtilisation = statistiquesCouleurs[couleur];
                    couleurMax = couleur;
                }
            }
            
            couleurPreferee.textContent = couleurMax + " (" + maxUtilisation + " fois)";
            
            // Effet sonore (optionnel - necessite des fichiers audio)
            console.log("Couleur changee vers: " + nom + " (" + code + ")");
            
            // Animation bonus
            document.querySelector('.conteneur').style.transform = 'scale(1.05)';
            setTimeout(() => {
                document.querySelector('.conteneur').style.transform = 'scale(1)';
            }, 200);
        }
        
        // Fonction pour reinitialiser
        function reinitialiser() {
            body.style.backgroundColor = "#ffffff";
            couleurActuelle.textContent = "Couleur actuelle: Blanche";
            codeCouleur.textContent = "Code: #ffffff";
            
            compteurChangements = 0;
            compteur.textContent = "0";
            
            // Reinitialiser les statistiques
            for (let couleur in statistiquesCouleurs) {
                statistiquesCouleurs[couleur] = 0;
            }
            
            couleurPreferee.textContent = "Aucune";
            
            console.log("Application reinitialisee");
        }
        
        // Evenement au chargement
        document.addEventListener("DOMContentLoaded", function() {
            console.log("Changeur de couleurs pret !");
            
            // Couleur aleatoire au demarrage (bonus)
            let couleurs = [
                {code: '#e74c3c', nom: 'Rouge'},
                {code: '#3498db', nom: 'Bleu'},
                {code: '#2ecc71', nom: 'Vert'},
                {code: '#f1c40f', nom: 'Jaune'},
                {code: '#9b59b6', nom: 'Violet'}
            ];
            
            // Decommenter pour couleur aleatoire au demarrage
            // let couleurAleatoire = couleurs[Math.floor(Math.random() * couleurs.length)];
            // changerCouleur(couleurAleatoire.code, couleurAleatoire.nom);
        });
        
        // Raccourcis clavier (bonus)
        document.addEventListener("keydown", function(event) {
            switch(event.key.toLowerCase()) {
                case 'r':
                    changerCouleur('#e74c3c', 'Rouge');
                    break;
                case 'b':
                    changerCouleur('#3498db', 'Bleu');
                    break;
                case 'v':
                    changerCouleur('#2ecc71', 'Vert');
                    break;
                case 'j':
                    changerCouleur('#f1c40f', 'Jaune');
                    break;
                case 'p':
                    changerCouleur('#9b59b6', 'Violet');
                    break;
                case 'escape':
                    reinitialiser();
                    break;
            }
        });
    </script>
</body>
</html>
```

### Solution Exercice 2
```html
<!DOCTYPE html>
<html>
<head>
    <title>Formulaire de Contact Dynamique</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
        .formulaire { border: 1px solid #ddd; padding: 20px; border-radius: 8px; }
        .champ { margin-bottom: 15px; }
        .label { display: block; margin-bottom: 5px; font-weight: bold; }
        .input, .textarea { 
            width: 100%; 
            padding: 10px; 
            border: 1px solid #ccc; 
            border-radius: 4px; 
            font-size: 16px;
        }
        .input.valide { border-color: #28a745; background-color: #f8fff8; }
        .input.invalide { border-color: #dc3545; background-color: #fff8f8; }
        .erreur { color: #dc3545; font-size: 14px; margin-top: 5px; }
        .succes { color: #28a745; font-size: 14px; margin-top: 5px; }
        .compteur { font-size: 12px; color: #666; text-align: right; }
        .apercu { 
            background: #f8f9fa; 
            padding: 15px; 
            border-radius: 4px; 
            margin-top: 20px;
            border-left: 4px solid #007bff;
        }
        .bouton { 
            background: #007bff; 
            color: white; 
            padding: 12px 25px; 
            border: none; 
            border-radius: 4px; 
            cursor: pointer; 
            font-size: 16px;
        }
        .bouton:disabled { background: #6c757d; cursor: not-allowed; }
    </style>
</head>
<body>
    <div class="formulaire">
        <h1>Formulaire de Contact</h1>
        <p>Tous les champs sont valides en temps reel !</p>
        
        <form id="contact-form">
            <div class="champ">
                <label class="label" for="nom">Nom complet *</label>
                <input type="text" id="nom" class="input" placeholder="Votre nom et prenom">
                <div id="erreur-nom" class="erreur"></div>
                <div id="succes-nom" class="succes"></div>
            </div>
            
            <div class="champ">
                <label class="label" for="email">Email *</label>
                <input type="email" id="email" class="input" placeholder="votre@email.com">
                <div id="erreur-email" class="erreur"></div>
                <div id="succes-email" class="succes"></div>
            </div>
            
            <div class="champ">
                <label class="label" for="telephone">Telephone (optionnel)</label>
                <input type="tel" id="telephone" class="input" placeholder="06.12.34.56.78">
                <div id="erreur-telephone" class="erreur"></div>
                <div id="succes-telephone" class="succes"></div>
            </div>
            
            <div class="champ">
                <label class="label" for="sujet">Sujet *</label>
                <input type="text" id="sujet" class="input" placeholder="Sujet de votre message">
                <div id="erreur-sujet" class="erreur"></div>
                <div id="succes-sujet" class="succes"></div>
            </div>
            
            <div class="champ">
                <label class="label" for="message">Message *</label>
                <textarea id="message" class="textarea input" rows="5" 
                         placeholder="Votre message (minimum 20 caracteres)"></textarea>
                <div class="compteur">
                    <span id="compteur-message">0</span> / 500 caracteres
                </div>
                <div id="erreur-message" class="erreur"></div>
                <div id="succes-message" class="succes"></div>
            </div>
            
            <button type="submit" id="bouton-envoyer" class="bouton" disabled>
                Envoyer le message
            </button>
        </form>
        
        <div id="apercu" class="apercu" style="display: none;">
            <h3>Apercu du message</h3>
            <p><strong>De:</strong> <span id="apercu-nom"></span> (<span id="apercu-email"></span>)</p>
            <p><strong>Telephone:</strong> <span id="apercu-telephone"></span></p>
            <p><strong>Sujet:</strong> <span id="apercu-sujet"></span></p>
            <p><strong>Message:</strong></p>
            <div id="apercu-message"></div>
        </div>
    </div>

    <script>
        // Elements du formulaire
        let champNom = document.getElementById("nom");
        let champEmail = document.getElementById("email");
        let champTelephone = document.getElementById("telephone");
        let champSujet = document.getElementById("sujet");
        let champMessage = document.getElementById("message");
        let boutonEnvoyer = document.getElementById("bouton-envoyer");
        let formulaire = document.getElementById("contact-form");
        
        // Elements d'erreur et succes
        let erreurNom = document.getElementById("erreur-nom");
        let succesNom = document.getElementById("succes-nom");
        let erreurEmail = document.getElementById("erreur-email");
        let succesEmail = document.getElementById("succes-email");
        let erreurTelephone = document.getElementById("erreur-telephone");
        let succesTelephone = document.getElementById("succes-telephone");
        let erreurSujet = document.getElementById("erreur-sujet");
        let succesSujet = document.getElementById("succes-sujet");
        let erreurMessage = document.getElementById("erreur-message");
        let succesMessage = document.getElementById("succes-message");
        
        // Elements de l'apercu
        let apercu = document.getElementById("apercu");
        let apercuNom = document.getElementById("apercu-nom");
        let apercuEmail = document.getElementById("apercu-email");
        let apercuTelephone = document.getElementById("apercu-telephone");
        let apercuSujet = document.getElementById("apercu-sujet");
        let apercuMessage = document.getElementById("apercu-message");
        
        // Compteur de caracteres
        let compteurMessage = document.getElementById("compteur-message");
        
        // Etat de validation
        let validationEtat = {
            nom: false,
            email: false,
            telephone: true,  // Optionnel donc valide par defaut
            sujet: false,
            message: false
        };
        
        // Fonction pour valider le nom
        function validerNom() {
            let nom = champNom.value.trim();
            
            if (nom.length === 0) {
                afficherErreur(champNom, erreurNom, succesNom, "Le nom est obligatoire");
                return false;
            }
            
            if (nom.length < 2) {
                afficherErreur(champNom, erreurNom, succesNom, "Le nom doit avoir au moins 2 caracteres");
                return false;
            }
            
            if (!/^[a-zA-Z\s\-']+$/.test(nom)) {
                afficherErreur(champNom, erreurNom, succesNom, "Le nom ne peut contenir que des lettres");
                return false;
            }
            
            afficherSucces(champNom, erreurNom, succesNom, "Nom valide");
            return true;
        }
        
        // Fonction pour valider l'email
        function validerEmail() {
            let email = champEmail.value.trim();
            
            if (email.length === 0) {
                afficherErreur(champEmail, erreurEmail, succesEmail, "L'email est obligatoire");
                return false;
            }
            
            let regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!regexEmail.test(email)) {
                afficherErreur(champEmail, erreurEmail, succesEmail, "Format d'email invalide");
                return false;
            }
            
            afficherSucces(champEmail, erreurEmail, succesEmail, "Email valide");
            return true;
        }
        
        // Fonction pour valider le telephone
        function validerTelephone() {
            let telephone = champTelephone.value.trim();
            
            if (telephone.length === 0) {
                // Telephone optionnel
                champTelephone.className = "input";
                erreurTelephone.textContent = "";
                succesTelephone.textContent = "";
                return true;
            }
            
            let regexTel = /^[0-9\-\.\s\+]{10,}$/;
            if (!regexTel.test(telephone)) {
                afficherErreur(champTelephone, erreurTelephone, succesTelephone, "Format de telephone invalide");
                return false;
            }
            
            afficherSucces(champTelephone, erreurTelephone, succesTelephone, "Telephone valide");
            return true;
        }
        
        // Fonction pour valider le sujet
        function validerSujet() {
            let sujet = champSujet.value.trim();
            
            if (sujet.length === 0) {
                afficherErreur(champSujet, erreurSujet, succesSujet, "Le sujet est obligatoire");
                return false;
            }
            
            if (sujet.length < 5) {
                afficherErreur(champSujet, erreurSujet, succesSujet, "Le sujet doit avoir au moins 5 caracteres");
                return false;
            }
            
            afficherSucces(champSujet, erreurSujet, succesSujet, "Sujet valide");
            return true;
        }
        
        // Fonction pour valider le message
        function validerMessage() {
            let message = champMessage.value.trim();
            
            // Mettre a jour le compteur
            compteurMessage.textContent = message.length;
            
            if (message.length === 0) {
                afficherErreur(champMessage, erreurMessage, succesMessage, "Le message est obligatoire");
                return false;
            }
            
            if (message.length < 20) {
                afficherErreur(champMessage, erreurMessage, succesMessage, 
                             "Le message doit avoir au moins 20 caracteres (" + (20 - message.length) + " restants)");
                return false;
            }
            
            if (message.length > 500) {
                afficherErreur(champMessage, erreurMessage, succesMessage, "Le message ne peut pas depasser 500 caracteres");
                return false;
            }
            
            afficherSucces(champMessage, erreurMessage, succesMessage, "Message valide");
            return true;
        }
        
        // Fonction pour afficher une erreur
        function afficherErreur(champ, elementErreur, elementSucces, message) {
            champ.className = "input invalide";
            elementErreur.textContent = message;
            elementSucces.textContent = "";
        }
        
        // Fonction pour afficher un succes
        function afficherSucces(champ, elementErreur, elementSucces, message) {
            champ.className = "input valide";
            elementErreur.textContent = "";
            elementSucces.textContent = message;
        }
        
        // Fonction pour mettre a jour l'etat du bouton
        function mettreAJourBouton() {
            let toutValide = validationEtat.nom && validationEtat.email && 
                           validationEtat.telephone && validationEtat.sujet && 
                           validationEtat.message;
            
            boutonEnvoyer.disabled = !toutValide;
            
            if (toutValide) {
                boutonEnvoyer.textContent = "Envoyer le message";
                afficherApercu();
            } else {
                boutonEnvoyer.textContent = "Completez le formulaire";
                apercu.style.display = "none";
            }
        }
        
        // Fonction pour afficher l'apercu
        function afficherApercu() {
            apercuNom.textContent = champNom.value;
            apercuEmail.textContent = champEmail.value;
            apercuTelephone.textContent = champTelephone.value || "Non renseigne";
            apercuSujet.textContent = champSujet.value;
            apercuMessage.textContent = champMessage.value;
            
            apercu.style.display = "block";
        }
        
        // Evenements de validation en temps reel
        champNom.addEventListener("input", function() {
            validationEtat.nom = validerNom();
            mettreAJourBouton();
        });
        
        champEmail.addEventListener("input", function() {
            validationEtat.email = validerEmail();
            mettreAJourBouton();
        });
        
        champTelephone.addEventListener("input", function() {
            validationEtat.telephone = validerTelephone();
            mettreAJourBouton();
        });
        
        champSujet.addEventListener("input", function() {
            validationEtat.sujet = validerSujet();
            mettreAJourBouton();
        });
        
        champMessage.addEventListener("input", function() {
            validationEtat.message = validerMessage();
            mettreAJourBouton();
        });
        
        // Soumission du formulaire
        formulaire.addEventListener("submit", function(event) {
            event.preventDefault();
            
            alert("Message envoye avec succes !\n\n" +
                  "Dans une vraie application, ce message serait envoye au serveur.");
            
            // Reinitialiser le formulaire
            formulaire.reset();
            
            // Reinitialiser l'etat
            for (let champ in validationEtat) {
                if (champ === "telephone") {
                    validationEtat[champ] = true;
                } else {
                    validationEtat[champ] = false;
                }
            }
            
            // Nettoyer l'affichage
            document.querySelectorAll(".input").forEach(el => el.className = "input");
            document.querySelectorAll(".erreur, .succes").forEach(el => el.textContent = "");
            compteurMessage.textContent = "0";
            apercu.style.display = "none";
            
            mettreAJourBouton();
        });
        
        console.log("Formulaire de contact dynamique pret !");
    </script>
</body>
</html>
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Variables** : Pour stocker les references aux elements
- **Fonctions** : Pour organiser les interactions
- **Objets** : Elements DOM sont des objets JavaScript
- **Arrays** : Pour manipuler plusieurs elements
- **Events** : Pour reagir aux actions utilisateur

### Prochaines etapes  
- **Events avances** : Delegation, propagation, custom events
- **APIs du navigateur** : Local Storage, Geolocation, etc.
- **AJAX/Fetch** : Communiquer avec des serveurs
- **Frameworks** : React, Vue.js pour du DOM plus complexe

### Concepts complementaires
- **CSS dynamique** : Modifier les styles avec JavaScript
- **Animations** : Creer des transitions et effets
- **Responsive** : Adapter l'interface selon l'ecran

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer une application de gestion de budget personnel

**Fonctionnalites requises :**
- [ ] Ajouter des revenus et depenses
- [ ] Categoriser les transactions (nourriture, transport, loisirs)
- [ ] Calculer le solde en temps reel
- [ ] Graphique simple (barres CSS) des categories
- [ ] Filtrer par periode et categorie
- [ ] Sauvegarder dans localStorage
- [ ] Export des donnees en format texte

**Elements DOM a manipuler :**
- Formulaires dynamiques
- Listes de transactions
- Indicateurs visuels (couleurs selon le solde)
- Graphiques CSS
- Modales de confirmation

**Structure de fichiers suggeree :**
```
budget-app/
├── index.html
├── style.css  
├── script.js
└── README.md
```

**Temps estime :** 4 heures

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - DOM](https://developer.mozilla.org/fr/docs/Web/API/Document_Object_Model)
- [MDN - Document methods](https://developer.mozilla.org/fr/docs/Web/API/Document)
- [MDN - Element methods](https://developer.mozilla.org/fr/docs/Web/API/Element)
- [MDN - Events](https://developer.mozilla.org/fr/docs/Web/Events)

### Videos recommandees
- "DOM JavaScript pour debutants" - Grafikart (90min)
- "Manipuler le DOM comme un pro" - techniques avancees

### Articles approfondis
- "Performance DOM : bonnes pratiques" - optimisation des manipulations
- "Securite DOM : eviter les failles XSS" - securite front-end

### Outils de pratique
- [CodePen - DOM playground](https://codepen.io) - tester les manipulations DOM
- [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools) - debugger le DOM

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre innerHTML et textContent ?
   - Reponse : innerHTML inclut le HTML, textContent ne prend que le texte

2. **Question pratique :** Comment trouver un element par son ID ?
   - Reponse : document.getElementById("monId")

3. **Question d'application :** Comment ecouter un clic sur un bouton ?
   - Reponse : bouton.addEventListener("click", function() { ... })

### Checklist de maitrise
- [ ] Je sais trouver des elements avec getElementById et querySelector
- [ ] Je peux modifier le contenu avec innerHTML et textContent
- [ ] Je sais changer les styles avec element.style
- [ ] Je comprends comment ecouter les evenements
- [ ] Je peux creer des elements dynamiquement
- [ ] Je sais organiser mon code DOM proprement

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
let bouton = document.getElementById("monBouton");
bouton.addEventListener("click", function() {  // Erreur si bouton = null
    alert("Clic !");
});
```

**Solution :**
```javascript
let bouton = document.getElementById("monBouton");
if (bouton) {  // Verifier que l'element existe
    bouton.addEventListener("click", function() {
        alert("Clic !");
    });
} else {
    console.log("Element 'monBouton' non trouve !");
}
```

**Explication :** Toujours verifier qu'un element existe avant de l'utiliser.

### Erreur frequente 2
**Code problematique :**
```javascript
// Script execute avant le chargement de la page
let titre = document.getElementById("titre");  // null !
titre.textContent = "Nouveau titre";  // Erreur !
```

**Solution :**
```javascript
// Attendre que la page soit chargee
document.addEventListener("DOMContentLoaded", function() {
    let titre = document.getElementById("titre");
    if (titre) {
        titre.textContent = "Nouveau titre";
    }
});
```

**Explication :** Le DOM doit etre charge avant qu'on puisse le manipuler.

### Erreur frequente 3
**Code problematique :**
```javascript
// Chercher le meme element plusieurs fois (inefficace)
function mettreAJour() {
    document.getElementById("compteur").textContent = compteur;
    document.getElementById("compteur").style.color = "red";
    document.getElementById("compteur").style.fontSize = "20px";
}
```

**Solution :**
```javascript
// Stocker la reference une seule fois
let elementCompteur = document.getElementById("compteur");

function mettreAJour() {
    if (elementCompteur) {
        elementCompteur.textContent = compteur;
        elementCompteur.style.color = "red";
        elementCompteur.style.fontSize = "20px";
    }
}
```

**Explication :** Stocker les references aux elements pour eviter les recherches repetees.

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Le DOM permet de faire le lien entre JavaScript et HTML]
- [getElementById et querySelector sont essentiels]  
- [addEventListener rend les pages interactives]

### Questions a approfondir
- [Comment optimiser les performances DOM ?]
- [Quelles sont les meilleures pratiques pour les gros projets ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices DOM)
- [ ] Revision dans 1 semaine (creer une app interactive complete)
- [ ] Revision dans 1 mois (expliquer le DOM a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript dom manipulation html elements events interactif

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 10/126*
