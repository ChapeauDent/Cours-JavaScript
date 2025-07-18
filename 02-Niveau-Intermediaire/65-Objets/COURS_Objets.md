# Objets en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** Objets - Structurer et organiser des donnees complexes
- **Niveau :** INTERMEDIAIRE (apres arrays)
- **Duree estimee :** 130 minutes
- **Prerequis :** Variables, types, fonctions, arrays
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est un objet et sa structure cle-valeur
- [ ] **Creer** : Des objets avec proprietes et methodes
- [ ] **Manipuler** : Acceder, modifier, ajouter des proprietes
- [ ] **Organiser** : Structurer des donnees complexes avec des objets

### Mots-cles a retenir
- **Objet** : Structure qui groupe des donnees liees
- **Propriete** : Information stockee dans un objet (nom, age...)
- **Methode** : Fonction qui appartient a un objet
- **Cle** : Nom d'une propriete (comme "nom", "age")
- **Valeur** : Contenu d'une propriete (comme "Alice", 14)

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Imagine que tu veuilles decrire ton meilleur ami. Tu pourrais utiliser plusieurs variables :
```javascript
let nomAmi = "Alice";
let ageAmi = 15;
let couleurYeuxAmi = "bleu";
let hobbiesAmi = ["lecture", "natation", "musique"];
```

Mais si tu as plusieurs amis ? Tu vas creer des dizaines de variables ! C'est la que les objets deviennent puissants :
```javascript
let ami = {
    nom: "Alice",
    age: 15,
    couleurYeux: "bleu",
    hobbies: ["lecture", "natation", "musique"]
};
```

**Les objets permettent de :**
- **Grouper** des donnees qui vont ensemble
- **Structurer** l'information de maniere logique
- **Simplifier** l'acces aux donnees liees
- **Modeliser** des entites du monde reel

### Definition et concepts cles

**Un objet est une collection de proprietes :**

1. **Syntax** : Entre accolades `{ cle: valeur, cle2: valeur2 }`
2. **Proprietes** : Paires cle-valeur qui decrivent l'objet
3. **Methodes** : Proprietes qui contiennent des fonctions
4. **Acces** : Avec point `.` ou crochets `[""]`

**Types de proprietes :**
- **Primitives** : string, number, boolean
- **Arrays** : Listes de valeurs
- **Objets** : Objets imbriques
- **Fonctions** : Methodes de l'objet

### Syntaxe de base
```javascript
// Creation d'un objet
let personne = {
    nom: "Marie",
    age: 14,
    ville: "Paris",
    estEtudiant: true
};

// Acces aux proprietes
console.log(personne.nom);        // "Marie" (notation point)
console.log(personne["age"]);     // 14 (notation crochet)

// Modification de proprietes
personne.age = 15;                // Changer l'age
personne.lycee = "Victor Hugo";   // Ajouter une nouvelle propriete

// Methodes dans un objet
let calculatrice = {
    marque: "Casio",
    additionner: function(a, b) {
        return a + b;
    }
};

let resultat = calculatrice.additionner(5, 3);  // 8
```

### Points d'attention
- **Notation** : Point pour noms simples, crochets pour noms complexes
- **Proprietes dynamiques** : On peut ajouter/supprimer a tout moment
- **References** : Les objets sont passes par reference
- **This** : Dans une methode, `this` refere a l'objet

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```javascript
// Mon profil d'etudiant
let monProfil = {
    prenom: "Alex",
    nom: "Durand",
    age: 14,
    classe: "3eme",
    matierePreferee: "Mathematiques",
    notes: [15, 12, 18, 14],
    
    // Methode pour se presenter
    sePresenter: function() {
        return "Salut ! Je suis " + this.prenom + " " + this.nom + 
               ", j'ai " + this.age + " ans et je suis en " + this.classe + ".";
    },
    
    // Methode pour calculer la moyenne
    calculerMoyenne: function() {
        let total = 0;
        for (let i = 0; i < this.notes.length; i++) {
            total += this.notes[i];
        }
        return total / this.notes.length;
    }
};

// Utilisation de l'objet
console.log("=== MON PROFIL ===");
console.log(monProfil.sePresenter());
console.log("Matiere preferee: " + monProfil.matierePreferee);
console.log("Moyenne actuelle: " + monProfil.calculerMoyenne().toFixed(2) + "/20");

// Ajouter une nouvelle note
monProfil.notes.push(16);
console.log("Nouvelle moyenne: " + monProfil.calculerMoyenne().toFixed(2) + "/20");

// Ajouter une nouvelle propriete
monProfil.email = "alex.durand@lycee.fr";
console.log("Email: " + monProfil.email);
```

**Resultat attendu :**
```
=== MON PROFIL ===
Salut ! Je suis Alex Durand, j'ai 14 ans et je suis en 3eme.
Matiere preferee: Mathematiques
Moyenne actuelle: 14.75/20
Nouvelle moyenne: 15.00/20
Email: alex.durand@lycee.fr
```

### Exemple intermediaire
```javascript
// Systeme de gestion de bibliotheque avec objets
let bibliotheque = {
    nom: "Bibliotheque Municipale",
    adresse: "12 rue des Livres, Paris",
    livres: [],
    
    // Methode pour ajouter un livre
    ajouterLivre: function(titre, auteur, annee, genre) {
        let nouveauLivre = {
            id: this.livres.length + 1,
            titre: titre,
            auteur: auteur,
            anneePublication: annee,
            genre: genre,
            disponible: true,
            emprunteur: null
        };
        
        this.livres.push(nouveauLivre);
        console.log("Livre ajoute: " + titre + " par " + auteur);
    },
    
    // Methode pour rechercher un livre
    rechercherLivre: function(motCle) {
        let livresTrouves = [];
        
        for (let i = 0; i < this.livres.length; i++) {
            let livre = this.livres[i];
            let motCleLower = motCle.toLowerCase();
            
            if (livre.titre.toLowerCase().includes(motCleLower) ||
                livre.auteur.toLowerCase().includes(motCleLower) ||
                livre.genre.toLowerCase().includes(motCleLower)) {
                
                livresTrouves.push(livre);
            }
        }
        
        return livresTrouves;
    },
    
    // Methode pour emprunter un livre
    emprunterLivre: function(idLivre, nomEmprunteur) {
        for (let i = 0; i < this.livres.length; i++) {
            let livre = this.livres[i];
            
            if (livre.id === idLivre) {
                if (livre.disponible) {
                    livre.disponible = false;
                    livre.emprunteur = nomEmprunteur;
                    console.log(nomEmprunteur + " a emprunte: " + livre.titre);
                    return true;
                } else {
                    console.log("Livre deja emprunte par: " + livre.emprunteur);
                    return false;
                }
            }
        }
        
        console.log("Livre non trouve !");
        return false;
    },
    
    // Methode pour rendre un livre
    rendreLivre: function(idLivre) {
        for (let i = 0; i < this.livres.length; i++) {
            let livre = this.livres[i];
            
            if (livre.id === idLivre) {
                if (!livre.disponible) {
                    console.log(livre.emprunteur + " a rendu: " + livre.titre);
                    livre.disponible = true;
                    livre.emprunteur = null;
                    return true;
                } else {
                    console.log("Ce livre n'etait pas emprunte !");
                    return false;
                }
            }
        }
        
        console.log("Livre non trouve !");
        return false;
    },
    
    // Methode pour afficher tous les livres
    afficherCatalogue: function() {
        console.log("\n=== CATALOGUE - " + this.nom + " ===");
        
        if (this.livres.length === 0) {
            console.log("Aucun livre dans la bibliotheque");
            return;
        }
        
        for (let i = 0; i < this.livres.length; i++) {
            let livre = this.livres[i];
            let statut = livre.disponible ? "DISPONIBLE" : "EMPRUNTE par " + livre.emprunteur;
            
            console.log("ID: " + livre.id + " | " + livre.titre + " - " + livre.auteur + 
                       " (" + livre.anneePublication + ") [" + livre.genre + "] - " + statut);
        }
        
        let disponibles = this.compterLivresDisponibles();
        console.log("\nTotal: " + this.livres.length + " livre(s), " + 
                   disponibles + " disponible(s)");
    },
    
    // Methode pour compter les livres disponibles
    compterLivresDisponibles: function() {
        let compteur = 0;
        for (let i = 0; i < this.livres.length; i++) {
            if (this.livres[i].disponible) {
                compteur++;
            }
        }
        return compteur;
    }
};

// Test du systeme de bibliotheque
bibliotheque.ajouterLivre("Le Petit Prince", "Antoine de Saint-Exupery", 1943, "Fiction");
bibliotheque.ajouterLivre("1984", "George Orwell", 1949, "Science-Fiction");
bibliotheque.ajouterLivre("Harry Potter", "J.K. Rowling", 1997, "Fantastique");

bibliotheque.afficherCatalogue();

console.log("\n=== EMPRUNTS ===");
bibliotheque.emprunterLivre(1, "Marie Dupont");
bibliotheque.emprunterLivre(2, "Paul Martin");

bibliotheque.afficherCatalogue();

console.log("\n=== RECHERCHE ===");
let resultats = bibliotheque.rechercherLivre("Harry");
console.log("Resultats pour 'Harry': " + resultats.length + " livre(s) trouve(s)");
for (let i = 0; i < resultats.length; i++) {
    console.log("- " + resultats[i].titre);
}

console.log("\n=== RETOUR ===");
bibliotheque.rendreLivre(1);
bibliotheque.afficherCatalogue();
```

### Exemple avance (optionnel)
```javascript
// Jeu de role simple avec objets complexes
function creerPersonnage(nom, classe) {
    return {
        nom: nom,
        classe: classe,
        niveau: 1,
        pointsDeVie: 100,
        pointsDeVieMax: 100,
        force: 10,
        defense: 5,
        experience: 0,
        experienceNecessaire: 100,
        inventaire: [],
        
        // Methode pour attaquer un autre personnage
        attaquer: function(cible) {
            let degats = Math.floor(Math.random() * this.force) + 1;
            cible.recevoirDegats(degats);
            
            console.log(this.nom + " attaque " + cible.nom + " et inflige " + degats + " degats !");
            
            return degats;
        },
        
        // Methode pour recevoir des degats
        recevoirDegats: function(degats) {
            let degatsReels = Math.max(0, degats - this.defense);
            this.pointsDeVie -= degatsReels;
            
            if (this.pointsDeVie < 0) {
                this.pointsDeVie = 0;
            }
            
            console.log(this.nom + " recoit " + degatsReels + " degats (defense: " + this.defense + ")");
            console.log("PV restants: " + this.pointsDeVie + "/" + this.pointsDeVieMax);
            
            if (this.pointsDeVie === 0) {
                console.log(this.nom + " est vaincu !");
            }
        },
        
        // Methode pour se soigner
        soigner: function(montant) {
            let anciensPV = this.pointsDeVie;
            this.pointsDeVie = Math.min(this.pointsDeVieMax, this.pointsDeVie + montant);
            let pvGagnes = this.pointsDeVie - anciensPV;
            
            console.log(this.nom + " recupere " + pvGagnes + " PV !");
            console.log("PV actuels: " + this.pointsDeVie + "/" + this.pointsDeVieMax);
        },
        
        // Methode pour gagner de l'experience
        gagnerExperience: function(exp) {
            this.experience += exp;
            console.log(this.nom + " gagne " + exp + " points d'experience !");
            
            // Verification montee de niveau
            while (this.experience >= this.experienceNecessaire) {
                this.monterNiveau();
            }
        },
        
        // Methode pour monter de niveau
        monterNiveau: function() {
            this.experience -= this.experienceNecessaire;
            this.niveau++;
            this.experienceNecessaire = Math.floor(this.experienceNecessaire * 1.5);
            
            // Amelioration des statistiques
            this.pointsDeVieMax += 20;
            this.pointsDeVie = this.pointsDeVieMax;  // Soigne completement
            this.force += 2;
            this.defense += 1;
            
            console.log("*** " + this.nom + " monte au niveau " + this.niveau + " ! ***");
            console.log("Nouvelles stats - PV: " + this.pointsDeVieMax + 
                       ", Force: " + this.force + ", Defense: " + this.defense);
        },
        
        // Methode pour ajouter un objet a l'inventaire
        ajouterObjet: function(objet) {
            this.inventaire.push(objet);
            console.log(this.nom + " obtient: " + objet.nom);
        },
        
        // Methode pour utiliser un objet
        utiliserObjet: function(nomObjet) {
            for (let i = 0; i < this.inventaire.length; i++) {
                let objet = this.inventaire[i];
                
                if (objet.nom.toLowerCase() === nomObjet.toLowerCase()) {
                    objet.utiliser(this);
                    this.inventaire.splice(i, 1);  // Supprimer l'objet de l'inventaire
                    return;
                }
            }
            
            console.log(this.nom + " n'a pas d'objet nomme: " + nomObjet);
        },
        
        // Methode pour afficher les statistiques
        afficherStats: function() {
            console.log("\n=== " + this.nom + " (" + this.classe + ") ===");
            console.log("Niveau: " + this.niveau);
            console.log("PV: " + this.pointsDeVie + "/" + this.pointsDeVieMax);
            console.log("Force: " + this.force);
            console.log("Defense: " + this.defense);
            console.log("Experience: " + this.experience + "/" + this.experienceNecessaire);
            
            if (this.inventaire.length > 0) {
                console.log("Inventaire:");
                for (let i = 0; i < this.inventaire.length; i++) {
                    console.log("- " + this.inventaire[i].nom);
                }
            } else {
                console.log("Inventaire vide");
            }
        }
    };
}

// Creation d'objets pour les objets du jeu
let potionSoin = {
    nom: "Potion de Soin",
    description: "Restaure 30 PV",
    utiliser: function(personnage) {
        console.log(personnage.nom + " utilise une " + this.nom);
        personnage.soigner(30);
    }
};

let epeeEnchantee = {
    nom: "Epee Enchantee",
    description: "Augmente la force de 5 points",
    utiliser: function(personnage) {
        console.log(personnage.nom + " utilise une " + this.nom);
        personnage.force += 5;
        console.log("Force augmentee ! Nouvelle force: " + personnage.force);
    }
};

// Test du jeu de role
let hero = creerPersonnage("Aragorn", "Guerrier");
let monstre = creerPersonnage("Gobelin", "Monstre");

// Affichage initial
hero.afficherStats();
monstre.afficherStats();

// Combat
console.log("\n=== DEBUT DU COMBAT ===");
hero.attaquer(monstre);
monstre.attaquer(hero);

// Utilisation d'objets
hero.ajouterObjet(potionSoin);
hero.ajouterObjet(epeeEnchantee);

hero.utiliserObjet("Potion de Soin");
hero.utiliserObjet("Epee Enchantee");

// Gain d'experience
hero.gagnerExperience(120);

// Stats finales
hero.afficherStats();
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Carnet de contacts enrichi
**Consigne :** Cree un objet contact avec toutes les informations et methodes

**Proprietes a inclure :**
- Nom, prenom, telephone, email, adresse
- Methodes : afficher(), modifier(), estValide()

**Niveau de difficulte :** 3 etoiles sur 5

### Exercice 2 : Compte bancaire simple
**Consigne :** Simule un compte bancaire avec toutes les operations

**Fonctionnalites :**
- Solde, proprietaire, numero de compte
- Methodes : deposer(), retirer(), consulterSolde(), transferer()
- Historique des operations

**Niveau de difficulte :** 4 etoiles sur 5

### Exercice 3 : Jeu de quiz interactif
**Consigne :** Cree un systeme de quiz avec objets pour questions et joueur

**Structures :**
- Objet Question (texte, reponses, bonne reponse)
- Objet Joueur (nom, score, niveau)
- Objet Quiz (questions, jouer(), calculerScore())

**Niveau de difficulte :** 5 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```javascript
// Carnet de contacts enrichi avec objet
function creerContact(nom, prenom, telephone, email, adresse) {
    return {
        nom: nom,
        prenom: prenom,
        telephone: telephone,
        email: email,
        adresse: adresse,
        dateCreation: new Date(),
        
        // Methode pour afficher le contact
        afficher: function() {
            console.log("=== CONTACT ===");
            console.log("Nom complet: " + this.prenom + " " + this.nom);
            console.log("Telephone: " + this.telephone);
            console.log("Email: " + this.email);
            console.log("Adresse: " + this.adresse);
            console.log("Cree le: " + this.dateCreation.toLocaleDateString());
        },
        
        // Methode pour modifier une propriete
        modifier: function(propriete, nouvelleValeur) {
            if (this.hasOwnProperty(propriete) && propriete !== "dateCreation") {
                let ancienneValeur = this[propriete];
                this[propriete] = nouvelleValeur;
                console.log("Modification: " + propriete + " change de '" + 
                           ancienneValeur + "' vers '" + nouvelleValeur + "'");
            } else {
                console.log("Propriete '" + propriete + "' non modifiable ou inexistante");
            }
        },
        
        // Methode pour verifier si le contact est valide
        estValide: function() {
            let erreurs = [];
            
            // Verification nom et prenom
            if (!this.nom || this.nom.trim().length < 2) {
                erreurs.push("Nom invalide (minimum 2 caracteres)");
            }
            
            if (!this.prenom || this.prenom.trim().length < 2) {
                erreurs.push("Prenom invalide (minimum 2 caracteres)");
            }
            
            // Verification telephone (format simple)
            if (!this.telephone || !/^[0-9\-\.\s\+]{10,}$/.test(this.telephone)) {
                erreurs.push("Telephone invalide");
            }
            
            // Verification email (format simple)
            if (!this.email || !this.email.includes("@") || !this.email.includes(".")) {
                erreurs.push("Email invalide");
            }
            
            // Verification adresse
            if (!this.adresse || this.adresse.trim().length < 5) {
                erreurs.push("Adresse trop courte (minimum 5 caracteres)");
            }
            
            // Affichage des resultats
            if (erreurs.length === 0) {
                console.log("Contact valide !");
                return true;
            } else {
                console.log("Contact invalide. Erreurs:");
                for (let i = 0; i < erreurs.length; i++) {
                    console.log("- " + erreurs[i]);
                }
                return false;
            }
        },
        
        // Methode pour obtenir le nom complet
        nomComplet: function() {
            return this.prenom + " " + this.nom;
        },
        
        // Methode pour exporter en format texte
        exporter: function() {
            return this.nomComplet() + ";" + this.telephone + ";" + 
                   this.email + ";" + this.adresse;
        }
    };
}

// Test du contact enrichi
let contact1 = creerContact("Dupont", "Marie", "06.12.34.56.78", 
                           "marie.dupont@email.com", "123 Rue de la Paix, Paris");

contact1.afficher();
console.log("");

contact1.estValide();
console.log("");

contact1.modifier("telephone", "06.99.88.77.66");
contact1.modifier("dateCreation", "impossible");  // Test propriete protegee
console.log("");

console.log("Nom complet: " + contact1.nomComplet());
console.log("Export: " + contact1.exporter());
```

### Solution Exercice 2
```javascript
// Compte bancaire simple avec historique
function creerCompteBancaire(proprietaire, numeroCompte, soldeInitial = 0) {
    return {
        proprietaire: proprietaire,
        numeroCompte: numeroCompte,
        solde: soldeInitial,
        historique: [],
        dateCreation: new Date(),
        
        // Methode privee pour ajouter a l'historique
        ajouterHistorique: function(type, montant, description) {
            let operation = {
                date: new Date(),
                type: type,
                montant: montant,
                description: description,
                soldeApres: this.solde
            };
            
            this.historique.push(operation);
        },
        
        // Methode pour deposer de l'argent
        deposer: function(montant) {
            if (montant <= 0) {
                console.log("Erreur: Le montant doit etre positif");
                return false;
            }
            
            this.solde += montant;
            this.ajouterHistorique("DEPOT", montant, "Depot d'argent");
            
            console.log("Depot de " + montant + " euros effectue");
            console.log("Nouveau solde: " + this.solde + " euros");
            
            return true;
        },
        
        // Methode pour retirer de l'argent
        retirer: function(montant) {
            if (montant <= 0) {
                console.log("Erreur: Le montant doit etre positif");
                return false;
            }
            
            if (montant > this.solde) {
                console.log("Erreur: Solde insuffisant");
                console.log("Solde actuel: " + this.solde + " euros");
                console.log("Montant demande: " + montant + " euros");
                return false;
            }
            
            this.solde -= montant;
            this.ajouterHistorique("RETRAIT", -montant, "Retrait d'argent");
            
            console.log("Retrait de " + montant + " euros effectue");
            console.log("Nouveau solde: " + this.solde + " euros");
            
            return true;
        },
        
        // Methode pour consulter le solde
        consulterSolde: function() {
            console.log("=== CONSULTATION DE COMPTE ===");
            console.log("Proprietaire: " + this.proprietaire);
            console.log("Numero de compte: " + this.numeroCompte);
            console.log("Solde actuel: " + this.solde + " euros");
            console.log("Compte cree le: " + this.dateCreation.toLocaleDateString());
            
            return this.solde;
        },
        
        // Methode pour transferer vers un autre compte
        transferer: function(montant, compteDestination) {
            if (this.retirer(montant)) {
                compteDestination.deposer(montant);
                
                // Mise a jour de l'historique pour plus de precision
                this.historique[this.historique.length - 1].description = 
                    "Transfert vers " + compteDestination.proprietaire;
                
                compteDestination.historique[compteDestination.historique.length - 1].description = 
                    "Transfert recu de " + this.proprietaire;
                
                console.log("Transfert de " + montant + " euros vers " + 
                           compteDestination.proprietaire + " effectue");
                
                return true;
            } else {
                console.log("Transfert annule");
                return false;
            }
        },
        
        // Methode pour afficher l'historique
        afficherHistorique: function(nombreOperations = 10) {
            console.log("=== HISTORIQUE DU COMPTE ===");
            console.log("Proprietaire: " + this.proprietaire);
            
            if (this.historique.length === 0) {
                console.log("Aucune operation effectuee");
                return;
            }
            
            let debut = Math.max(0, this.historique.length - nombreOperations);
            
            for (let i = debut; i < this.historique.length; i++) {
                let op = this.historique[i];
                let date = op.date.toLocaleDateString() + " " + 
                          op.date.toLocaleTimeString();
                
                console.log(date + " | " + op.type + " | " + 
                           op.montant + "€ | " + op.description + 
                           " | Solde: " + op.soldeApres + "€");
            }
            
            console.log("Total operations: " + this.historique.length);
        },
        
        // Methode pour calculer les statistiques
        calculerStatistiques: function() {
            let totalDepots = 0;
            let totalRetraits = 0;
            let nombreDepots = 0;
            let nombreRetraits = 0;
            
            for (let i = 0; i < this.historique.length; i++) {
                let op = this.historique[i];
                
                if (op.type === "DEPOT") {
                    totalDepots += op.montant;
                    nombreDepots++;
                } else if (op.type === "RETRAIT") {
                    totalRetraits += Math.abs(op.montant);
                    nombreRetraits++;
                }
            }
            
            console.log("=== STATISTIQUES ===");
            console.log("Total des depots: " + totalDepots + "€ (" + nombreDepots + " operations)");
            console.log("Total des retraits: " + totalRetraits + "€ (" + nombreRetraits + " operations)");
            console.log("Solde actuel: " + this.solde + "€");
        }
    };
}

// Test du compte bancaire
let compteAlice = creerCompteBancaire("Alice Dupont", "FR76-1234-5678-9012", 1000);
let compteBob = creerCompteBancaire("Bob Martin", "FR76-9876-5432-1098", 500);

console.log("=== CREATION DES COMPTES ===");
compteAlice.consulterSolde();
console.log("");
compteBob.consulterSolde();

console.log("\n=== OPERATIONS ===");
compteAlice.deposer(200);
compteAlice.retirer(150);
compteAlice.retirer(2000);  // Test solde insuffisant

console.log("\n=== TRANSFERT ===");
compteAlice.transferer(300, compteBob);

console.log("\n=== HISTORIQUES ===");
compteAlice.afficherHistorique();
console.log("");
compteBob.afficherHistorique();

console.log("\n=== STATISTIQUES ===");
compteAlice.calculerStatistiques();
```

### Solution Exercice 3
```javascript
// Systeme de quiz interactif avec objets
function creerQuestion(texte, reponses, bonneReponse, niveau = 1) {
    return {
        texte: texte,
        reponses: reponses,
        bonneReponse: bonneReponse,
        niveau: niveau,
        
        // Methode pour afficher la question
        afficher: function() {
            console.log("Question: " + this.texte);
            for (let i = 0; i < this.reponses.length; i++) {
                console.log((i + 1) + ". " + this.reponses[i]);
            }
        },
        
        // Methode pour verifier une reponse
        verifierReponse: function(numeroReponse) {
            return numeroReponse === this.bonneReponse;
        }
    };
}

function creerJoueur(nom) {
    return {
        nom: nom,
        score: 0,
        niveau: 1,
        bonnesReponses: 0,
        totalQuestions: 0,
        
        // Methode pour repondre a une question
        repondre: function(question, numeroReponse) {
            this.totalQuestions++;
            
            if (question.verifierReponse(numeroReponse)) {
                this.bonnesReponses++;
                let points = question.niveau * 10;
                this.score += points;
                
                console.log("Bonne reponse ! +" + points + " points");
                
                // Montee de niveau tous les 5 bonnes reponses
                if (this.bonnesReponses % 5 === 0) {
                    this.niveau++;
                    console.log("Niveau monte ! Nouveau niveau: " + this.niveau);
                }
                
                return true;
            } else {
                console.log("Mauvaise reponse ! La bonne reponse etait: " + 
                           question.reponses[question.bonneReponse - 1]);
                return false;
            }
        },
        
        // Methode pour afficher les statistiques
        afficherStats: function() {
            console.log("=== STATISTIQUES DE " + this.nom.toUpperCase() + " ===");
            console.log("Score: " + this.score + " points");
            console.log("Niveau: " + this.niveau);
            console.log("Bonnes reponses: " + this.bonnesReponses + "/" + this.totalQuestions);
            
            if (this.totalQuestions > 0) {
                let pourcentage = (this.bonnesReponses / this.totalQuestions * 100).toFixed(1);
                console.log("Taux de reussite: " + pourcentage + "%");
            }
        }
    };
}

let quiz = {
    questions: [],
    joueurs: [],
    questionActuelle: 0,
    
    // Methode pour ajouter une question
    ajouterQuestion: function(texte, reponses, bonneReponse, niveau = 1) {
        let question = creerQuestion(texte, reponses, bonneReponse, niveau);
        this.questions.push(question);
        console.log("Question ajoutee ! Total: " + this.questions.length + " questions");
    },
    
    // Methode pour ajouter un joueur
    ajouterJoueur: function(nom) {
        let joueur = creerJoueur(nom);
        this.joueurs.push(joueur);
        console.log("Joueur ajoute: " + nom);
    },
    
    // Methode pour jouer une partie complete
    jouerPartie: function(nomJoueur) {
        // Trouver le joueur
        let joueur = null;
        for (let i = 0; i < this.joueurs.length; i++) {
            if (this.joueurs[i].nom === nomJoueur) {
                joueur = this.joueurs[i];
                break;
            }
        }
        
        if (!joueur) {
            console.log("Joueur non trouve !");
            return;
        }
        
        console.log("\n=== DEBUT DU QUIZ POUR " + nomJoueur.toUpperCase() + " ===");
        
        // Jouer toutes les questions
        for (let i = 0; i < this.questions.length; i++) {
            console.log("\n--- Question " + (i + 1) + "/" + this.questions.length + " ---");
            let question = this.questions[i];
            
            question.afficher();
            
            // Simulation d'une reponse aleatoire (en vrai, ce serait une saisie utilisateur)
            let reponseAleatoire = Math.floor(Math.random() * question.reponses.length) + 1;
            console.log("Reponse choisie: " + reponseAleatoire);
            
            joueur.repondre(question, reponseAleatoire);
        }
        
        console.log("\n=== FIN DU QUIZ ===");
        joueur.afficherStats();
    },
    
    // Methode pour afficher le classement
    afficherClassement: function() {
        console.log("\n=== CLASSEMENT GENERAL ===");
        
        if (this.joueurs.length === 0) {
            console.log("Aucun joueur inscrit");
            return;
        }
        
        // Trier les joueurs par score decroissant
        let joueursClasses = [...this.joueurs].sort(function(a, b) {
            return b.score - a.score;
        });
        
        for (let i = 0; i < joueursClasses.length; i++) {
            let joueur = joueursClasses[i];
            let position = i + 1;
            let medaille = "";
            
            if (position === 1) medaille = " 🥇";
            else if (position === 2) medaille = " 🥈";
            else if (position === 3) medaille = " 🥉";
            
            console.log(position + ". " + joueur.nom + " - " + 
                       joueur.score + " points (Niveau " + joueur.niveau + ")" + medaille);
        }
    },
    
    // Methode pour reinitialiser le quiz
    reinitialiser: function() {
        this.questionActuelle = 0;
        for (let i = 0; i < this.joueurs.length; i++) {
            this.joueurs[i].score = 0;
            this.joueurs[i].niveau = 1;
            this.joueurs[i].bonnesReponses = 0;
            this.joueurs[i].totalQuestions = 0;
        }
        console.log("Quiz reinitialise !");
    }
};

// Creation du quiz de test
console.log("=== CREATION DU QUIZ ===");

quiz.ajouterQuestion("Quelle est la capitale de la France ?", 
                    ["Lyon", "Marseille", "Paris", "Toulouse"], 3, 1);

quiz.ajouterQuestion("Combien font 7 x 8 ?", 
                    ["54", "56", "58", "52"], 2, 1);

quiz.ajouterQuestion("Qui a ecrit 'Les Miserables' ?", 
                    ["Emile Zola", "Victor Hugo", "Gustave Flaubert", "Alexandre Dumas"], 2, 2);

quiz.ajouterQuestion("Quelle est la formule chimique de l'eau ?", 
                    ["H2O", "CO2", "NaCl", "CH4"], 1, 2);

quiz.ajouterQuestion("En quelle annee a eu lieu la Revolution francaise ?", 
                    ["1789", "1792", "1799", "1804"], 1, 3);

// Ajout des joueurs
quiz.ajouterJoueur("Alice");
quiz.ajouterJoueur("Bob");
quiz.ajouterJoueur("Charlie");

// Test des parties
quiz.jouerPartie("Alice");
quiz.jouerPartie("Bob");
quiz.jouerPartie("Charlie");

// Affichage du classement
quiz.afficherClassement();
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Variables** : Pour stocker les objets
- **Fonctions** : Pour creer les methodes des objets
- **Arrays** : Pour stocker des listes dans les objets
- **Types** : Pour les valeurs des proprietes
- **Boucles** : Pour parcourir les proprietes

### Prochaines etapes  
- **Classes** : Syntaxe moderne pour creer des objets
- **JSON** : Format pour echanger des objets
- **Destructuring** : Extraire des proprietes facilement
- **Programmation orientee objet** : Concepts avances

### Concepts complementaires
- **This** : Reference a l'objet courant
- **Prototype** : Heritage entre objets
- **Getters/Setters** : Controle d'acces aux proprietes

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer un systeme de gestion d'ecole complet

**Objets a modeliser :**
- [ ] Eleve (nom, notes, classe, moyenne)
- [ ] Professeur (nom, matiere, classes)
- [ ] Classe (nom, eleves, professeur principal)
- [ ] Ecole (classes, statistiques generales)

**Fonctionnalites :**
- [ ] Ajouter eleves/professeurs/classes
- [ ] Calculer moyennes individuelles et de classe
- [ ] Generer bulletins de notes
- [ ] Statistiques par matiere et par classe
- [ ] Recherche multicriteres

**Structure de fichiers suggeree :**
```
gestion-ecole/
├── index.html
├── style.css  
└── script.js
```

**Temps estime :** 3 heures

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - Objets](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Working_with_Objects)
- [MDN - Object methods](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [MDN - This](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/this)

### Videos recommandees
- "Objets JavaScript expliques simplement" - Grafikart (50min)
- "This en JavaScript" - comprendre le contexte

### Articles approfondis
- "Programmation orientee objet en JavaScript" - concepts avances
- "Bonnes pratiques pour structurer des objets" - architecture

### Outils de pratique
- [CodePen - Objects playground](https://codepen.io) - tester des objets complexes
- [Object visualizer](https://objectplayground.com/) - visualiser la structure

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre un objet et un array ?
   - Reponse : Array = liste ordonnee avec index numeriques, Objet = collection de proprietes avec cles nommees

2. **Question pratique :** Comment acceder a une propriete d'objet ?
   - Reponse : Avec notation point (objet.propriete) ou crochets (objet["propriete"])

3. **Question d'application :** Que fait `this` dans une methode d'objet ?
   - Reponse : `this` refere a l'objet qui contient la methode

### Checklist de maitrise
- [ ] Je sais creer un objet avec {}
- [ ] Je comprends les paires cle-valeur
- [ ] Je peux acceder aux proprietes avec . et []
- [ ] Je sais ajouter des methodes aux objets
- [ ] Je comprends le role de `this`
- [ ] Je peux structurer des donnees complexes avec des objets

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
let personne = {
    nom: "Alice",
    age: 14
};

console.log(personne.Age);  // undefined - mauvaise casse !
```

**Solution :**
```javascript
let personne = {
    nom: "Alice",
    age: 14
};

console.log(personne.age);  // 14 - bonne casse
```

**Explication :** JavaScript est sensible a la casse. `age` ≠ `Age`.

### Erreur frequente 2
**Code problematique :**
```javascript
let objet = {
    valeur: 10,
    doubler: function() {
        return valeur * 2;  // Erreur ! valeur n'est pas defini
    }
};
```

**Solution :**
```javascript
let objet = {
    valeur: 10,
    doubler: function() {
        return this.valeur * 2;  // Utilise this pour acceder a la propriete
    }
};
```

**Explication :** Dans une methode, utilise `this.propriete` pour acceder aux proprietes de l'objet.

### Erreur frequente 3
**Code problematique :**
```javascript
let obj1 = { nom: "Alice" };
let obj2 = obj1;  // Ce n'est PAS une copie !
obj2.nom = "Bob";
console.log(obj1.nom);  // "Bob" - obj1 est modifie aussi !
```

**Solution :**
```javascript
let obj1 = { nom: "Alice" };
let obj2 = { ...obj1 };  // Vraie copie avec spread operator
// ou
let obj2 = Object.assign({}, obj1);  // Copie avec Object.assign()
obj2.nom = "Bob";
console.log(obj1.nom);  // "Alice" - inchange
console.log(obj2.nom);  // "Bob"
```

**Explication :** Les objets sont des references. Pour copier, utilise spread `...` ou `Object.assign()`.

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Les objets groupent des donnees liees avec des cles nommees]
- [Les methodes sont des fonctions dans des objets]  
- [This permet d'acceder aux proprietes de l'objet courant]

### Questions a approfondir
- [Comment marche l'heritage entre objets ?]
- [Quand utiliser des classes au lieu d'objets ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices d'objets)
- [ ] Revision dans 1 semaine (creer un projet complexe avec objets)
- [ ] Revision dans 1 mois (expliquer les objets a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript objets proprietes methodes this cle-valeur structure donnees

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 9/126*
