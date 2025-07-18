# Programmation Orientée Objet en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnées du cours
- **Titre :** Programmation Orientée Objet - Concepts Fondamentaux
- **Niveau :** Intermédiaire
- **Durée estimée :** 90 minutes
- **Prérequis :** Objets, fonctions, prototypes, this, méthodes d'arrays
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : Les principes fondamentaux de la POO en JavaScript
- [ ] **Appliquer** : L'encapsulation, l'héritage et le polymorphisme
- [ ] **Analyser** : Quand utiliser la POO vs programmation fonctionnelle
- [ ] **Créer** : Des systèmes d'objets cohérents et maintenables

### Mots-clés à retenir
- **Encapsulation** : Regrouper données et méthodes dans un objet
- **Héritage** : Permettre à un objet d'hériter des propriétés d'un autre
- **Polymorphisme** : Différents objets répondent à la même interface
- **Abstraction** : Cacher la complexité interne derrière une interface simple
- **Prototype** : Mécanisme d'héritage de JavaScript

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
La POO permet d'organiser le code de façon plus structurée et réutilisable. Elle aide à modéliser des concepts du monde réel, facilite la collaboration en équipe, et rend le code plus maintenable à long terme. JavaScript utilise un système de prototypes unique qui diffère des langages de classe traditionnels.

### Définition et concepts clés

#### **1. Les 4 Piliers de la POO**
```javascript
// 1. ENCAPSULATION - Regrouper données et comportements
function Voiture(marque, modele) {
    // Propriétés privées (convention avec _)
    this._vitesse = 0;
    this._carburant = 100;
    
    // Propriétés publiques
    this.marque = marque;
    this.modele = modele;
    
    // Méthodes publiques
    this.accelerer = function(augmentation) {
        if (this._carburant > 0) {
            this._vitesse += augmentation;
            this._carburant -= 2;
        }
    };
    
    this.getVitesse = function() {
        return this._vitesse;
    };
}

// 2. HÉRITAGE - Partager fonctionnalités entre objets
function VoitureElectrique(marque, modele, autonomie) {
    // Hériter des propriétés de Voiture
    Voiture.call(this, marque, modele);
    this.autonomie = autonomie;
    this._batterie = 100;
}

// Hériter des méthodes
VoitureElectrique.prototype = Object.create(Voiture.prototype);
VoitureElectrique.prototype.constructor = VoitureElectrique;

// 3. POLYMORPHISME - Même interface, comportements différents
VoitureElectrique.prototype.accelerer = function(augmentation) {
    if (this._batterie > 0) {
        this._vitesse += augmentation * 1.2; // Plus efficace
        this._batterie -= 1;
    }
};

// 4. ABSTRACTION - Interface simple pour complexité cachée
function GestionnaireFlotte() {
    const vehicules = [];
    
    return {
        ajouterVehicule(vehicule) {
            vehicules.push(vehicule);
        },
        
        demarrerTous() {
            vehicules.forEach(v => v.accelerer(10));
        },
        
        rapport() {
            return vehicules.map(v => ({
                info: `${v.marque} ${v.modele}`,
                vitesse: v.getVitesse()
            }));
        }
    };
}
```

#### **2. Patterns de création d'objets**
```javascript
// Factory Function
function creerPersonne(nom, age) {
    return {
        nom: nom,
        age: age,
        sePresenter() {
            return `Je suis ${this.nom}, ${this.age} ans`;
        }
    };
}

// Constructor Function
function Personne(nom, age) {
    this.nom = nom;
    this.age = age;
}

Personne.prototype.sePresenter = function() {
    return `Je suis ${this.nom}, ${this.age} ans`;
};

// Object.create()
const personnePrototype = {
    init(nom, age) {
        this.nom = nom;
        this.age = age;
        return this;
    },
    
    sePresenter() {
        return `Je suis ${this.nom}, ${this.age} ans`;
    }
};

const nouvellePersonne = Object.create(personnePrototype).init("Alice", 25);
```

### Syntaxe de base
```javascript
// Constructeur avec prototype
function Animal(nom, espece) {
    this.nom = nom;
    this.espece = espece;
}

Animal.prototype.parler = function() {
    return `${this.nom} fait du bruit`;
};

Animal.prototype.dormir = function() {
    return `${this.nom} dort`;
};

// Création d'instance
const monChien = new Animal("Rex", "Chien");
console.log(monChien.parler()); // "Rex fait du bruit"

// Héritage
function Chien(nom, race) {
    Animal.call(this, nom, "Chien");
    this.race = race;
}

Chien.prototype = Object.create(Animal.prototype);
Chien.prototype.constructor = Chien;

// Surcharge de méthode
Chien.prototype.parler = function() {
    return `${this.nom} aboie: Woof!`;
};
```

### Points d'attention
- **Piège this** : Le contexte de `this` peut changer selon l'appel
- **Piège prototype** : Modifications du prototype affectent toutes les instances
- **Bonne pratique** : Utiliser la convention `_` pour les propriétés privées

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Système de gestion de bibliothèque simple
console.log("=== SYSTÈME BIBLIOTHÈQUE ===");

// Constructeur Livre
function Livre(titre, auteur, isbn, pages) {
    this.titre = titre;
    this.auteur = auteur;
    this.isbn = isbn;
    this.pages = pages;
    this._emprunte = false;
    this._dateEmprunt = null;
}

// Méthodes dans le prototype
Livre.prototype.emprunter = function() {
    if (!this._emprunte) {
        this._emprunte = true;
        this._dateEmprunt = new Date();
        return `"${this.titre}" emprunté avec succès`;
    }
    return `"${this.titre}" est déjà emprunté`;
};

Livre.prototype.rendre = function() {
    if (this._emprunte) {
        this._emprunte = false;
        const duree = Math.ceil((new Date() - this._dateEmprunt) / (1000 * 60 * 60 * 24));
        this._dateEmprunt = null;
        return `"${this.titre}" rendu après ${duree} jour(s)`;
    }
    return `"${this.titre}" n'était pas emprunté`;
};

Livre.prototype.getInfo = function() {
    const statut = this._emprunte ? "Emprunté" : "Disponible";
    return `📖 "${this.titre}" par ${this.auteur} - ${statut}`;
};

// Constructeur Bibliothèque
function Bibliotheque(nom) {
    this.nom = nom;
    this._livres = [];
    this._membres = [];
}

Bibliotheque.prototype.ajouterLivre = function(livre) {
    this._livres.push(livre);
    console.log(`➕ Livre ajouté: ${livre.titre}`);
};

Bibliotheque.prototype.chercherLivre = function(critere, valeur) {
    return this._livres.filter(livre => 
        livre[critere] && livre[critere].toLowerCase().includes(valeur.toLowerCase())
    );
};

Bibliotheque.prototype.listerLivres = function() {
    console.log(`\n📚 Catalogue de ${this.nom}:`);
    this._livres.forEach((livre, index) => {
        console.log(`${index + 1}. ${livre.getInfo()}`);
    });
};

Bibliotheque.prototype.statistiques = function() {
    const total = this._livres.length;
    const empruntes = this._livres.filter(l => l._emprunte).length;
    const disponibles = total - empruntes;
    
    return {
        total: total,
        empruntes: empruntes,
        disponibles: disponibles,
        tauxOccupation: ((empruntes / total) * 100).toFixed(1) + '%'
    };
};

// Tests
const bibliothequeVille = new Bibliotheque("Bibliothèque Municipale");

// Créer des livres
const livre1 = new Livre("1984", "George Orwell", "978-0-452-28423-4", 328);
const livre2 = new Livre("Le Petit Prince", "Antoine de Saint-Exupéry", "978-0-15-602600-5", 96);
const livre3 = new Livre("Dune", "Frank Herbert", "978-0-441-17271-9", 688);

// Ajouter à la bibliothèque
bibliothequeVille.ajouterLivre(livre1);
bibliothequeVille.ajouterLivre(livre2);
bibliothequeVille.ajouterLivre(livre3);

// Lister tous les livres
bibliothequeVille.listerLivres();

// Emprunter quelques livres
console.log("\n" + livre1.emprunter());
console.log(livre2.emprunter());
console.log(livre1.emprunter()); // Déjà emprunté

// Rechercher des livres
console.log("\n🔍 Recherche 'prince':");
const resultats = bibliothequeVille.chercherLivre('titre', 'prince');
resultats.forEach(livre => console.log(`  ${livre.getInfo()}`));

// Statistiques
console.log("\n📊 Statistiques:");
const stats = bibliothequeVille.statistiques();
console.log(`Total: ${stats.total} livres`);
console.log(`Empruntes: ${stats.empruntes}`);
console.log(`Disponibles: ${stats.disponibles}`);
console.log(`Taux d'occupation: ${stats.tauxOccupation}`);
```

**Résultat attendu :**
```
=== SYSTÈME BIBLIOTHÈQUE ===
➕ Livre ajouté: 1984
➕ Livre ajouté: Le Petit Prince
➕ Livre ajouté: Dune

📚 Catalogue de Bibliothèque Municipale:
1. 📖 "1984" par George Orwell - Disponible
2. 📖 "Le Petit Prince" par Antoine de Saint-Exupéry - Disponible
3. 📖 "Dune" par Frank Herbert - Disponible

"1984" emprunté avec succès
"Le Petit Prince" emprunté avec succès
"1984" est déjà emprunté

🔍 Recherche 'prince':
  📖 "Le Petit Prince" par Antoine de Saint-Exupéry - Emprunté

📊 Statistiques:
Total: 3 livres
Empruntes: 2
Disponibles: 1
Taux d'occupation: 66.7%
```

### Exemple intermédiaire
```javascript
// Système de gestion d'employés avec héritage
console.log("=== SYSTÈME GESTION EMPLOYÉS ===");

// Classe de base Employe
function Employe(nom, prenom, salaire, departement) {
    this.nom = nom;
    this.prenom = prenom;
    this._salaire = salaire; // Propriété "privée"
    this.departement = departement;
    this._dateEmbauche = new Date();
    this._id = Employe._prochainId++;
}

// Variable statique pour IDs
Employe._prochainId = 1;

// Méthodes communes
Employe.prototype.getNomComplet = function() {
    return `${this.prenom} ${this.nom}`;
};

Employe.prototype.getSalaire = function() {
    return this._salaire;
};

Employe.prototype.augmenterSalaire = function(pourcentage) {
    this._salaire *= (1 + pourcentage / 100);
    console.log(`💰 Augmentation de ${pourcentage}% pour ${this.getNomComplet()}`);
};

Employe.prototype.getAnciennete = function() {
    const maintenant = new Date();
    const diffMs = maintenant - this._dateEmbauche;
    return Math.floor(diffMs / (1000 * 60 * 60 * 24 * 365));
};

Employe.prototype.toString = function() {
    return `👤 ${this.getNomComplet()} (ID: ${this._id}) - ${this.departement}`;
};

// Classe héritée Manager
function Manager(nom, prenom, salaire, departement, equipe) {
    // Appeler le constructeur parent
    Employe.call(this, nom, prenom, salaire, departement);
    this._equipe = equipe || [];
    this._budget = 0;
}

// Héritage du prototype
Manager.prototype = Object.create(Employe.prototype);
Manager.prototype.constructor = Manager;

// Méthodes spécifiques aux managers
Manager.prototype.ajouterEmploye = function(employe) {
    this._equipe.push(employe);
    console.log(`👥 ${employe.getNomComplet()} ajouté à l'équipe de ${this.getNomComplet()}`);
};

Manager.prototype.retirerEmploye = function(employeId) {
    const index = this._equipe.findIndex(emp => emp._id === employeId);
    if (index !== -1) {
        const employe = this._equipe.splice(index, 1)[0];
        console.log(`👥 ${employe.getNomComplet()} retiré de l'équipe`);
        return employe;
    }
    return null;
};

Manager.prototype.getTailleEquipe = function() {
    return this._equipe.length;
};

Manager.prototype.getBudgetEquipe = function() {
    return this._equipe.reduce((total, emp) => total + emp.getSalaire(), 0);
};

Manager.prototype.donnerAugmentationEquipe = function(pourcentage) {
    this._equipe.forEach(emp => emp.augmenterSalaire(pourcentage));
    console.log(`🎉 Augmentation de ${pourcentage}% pour toute l'équipe de ${this.getNomComplet()}`);
};

// Surcharge de la méthode toString
Manager.prototype.toString = function() {
    return `👨‍💼 Manager: ${this.getNomComplet()} (ID: ${this._id}) - ${this.departement} (Équipe: ${this.getTailleEquipe()})`;
};

// Classe héritée Developpeur
function Developpeur(nom, prenom, salaire, langages) {
    Employe.call(this, nom, prenom, salaire, "IT");
    this._langages = langages || [];
    this._projets = [];
}

Developpeur.prototype = Object.create(Employe.prototype);
Developpeur.prototype.constructor = Developpeur;

Developpeur.prototype.ajouterLangage = function(langage) {
    if (!this._langages.includes(langage)) {
        this._langages.push(langage);
        console.log(`💻 ${this.getNomComplet()} maîtrise maintenant ${langage}`);
    }
};

Developpeur.prototype.assignerProjet = function(projet) {
    this._projets.push({
        nom: projet,
        dateDebut: new Date(),
        statut: "En cours"
    });
    console.log(`📋 Projet "${projet}" assigné à ${this.getNomComplet()}`);
};

Developpeur.prototype.terminerProjet = function(nomProjet) {
    const projet = this._projets.find(p => p.nom === nomProjet);
    if (projet) {
        projet.statut = "Terminé";
        projet.dateFin = new Date();
        console.log(`✅ Projet "${nomProjet}" terminé par ${this.getNomComplet()}`);
    }
};

Developpeur.prototype.getLangages = function() {
    return [...this._langages]; // Copie pour éviter modifications externes
};

Developpeur.prototype.toString = function() {
    return `👨‍💻 Dev: ${this.getNomComplet()} (ID: ${this._id}) - Langages: ${this._langages.join(', ')}`;
};

// Classe Entreprise pour gérer tous les employés
function Entreprise(nom) {
    this.nom = nom;
    this._employes = [];
    this._managers = [];
    this._developpeurs = [];
}

Entreprise.prototype.embaucher = function(employe) {
    this._employes.push(employe);
    
    // Ajouter dans la liste appropriée selon le type
    if (employe instanceof Manager) {
        this._managers.push(employe);
    } else if (employe instanceof Developpeur) {
        this._developpeurs.push(employe);
    }
    
    console.log(`🏢 ${employe.getNomComplet()} embauché chez ${this.nom}`);
};

Entreprise.prototype.licencier = function(employeId) {
    const index = this._employes.findIndex(emp => emp._id === employeId);
    if (index !== -1) {
        const employe = this._employes.splice(index, 1)[0];
        
        // Retirer des listes spécialisées
        if (employe instanceof Manager) {
            const mgIndex = this._managers.findIndex(mg => mg._id === employeId);
            if (mgIndex !== -1) this._managers.splice(mgIndex, 1);
        } else if (employe instanceof Developpeur) {
            const devIndex = this._developpeurs.findIndex(dev => dev._id === employeId);
            if (devIndex !== -1) this._developpeurs.splice(devIndex, 1);
        }
        
        console.log(`🚪 ${employe.getNomComplet()} a quitté l'entreprise`);
        return employe;
    }
    return null;
};

Entreprise.prototype.listerEmployes = function() {
    console.log(`\n👥 Employés de ${this.nom}:`);
    this._employes.forEach(emp => console.log(`  ${emp.toString()}`));
};

Entreprise.prototype.calculerMasseSalariale = function() {
    return this._employes.reduce((total, emp) => total + emp.getSalaire(), 0);
};

Entreprise.prototype.statistiques = function() {
    const stats = {
        totalEmployes: this._employes.length,
        managers: this._managers.length,
        developpeurs: this._developpeurs.length,
        autres: this._employes.length - this._managers.length - this._developpeurs.length,
        masseSalariale: this.calculerMasseSalariale(),
        salaireMoyen: this.calculerMasseSalariale() / this._employes.length
    };
    
    console.log(`\n📊 Statistiques ${this.nom}:`);
    console.log(`  Total employés: ${stats.totalEmployes}`);
    console.log(`  Managers: ${stats.managers}`);
    console.log(`  Développeurs: ${stats.developpeurs}`);
    console.log(`  Autres: ${stats.autres}`);
    console.log(`  Masse salariale: ${stats.masseSalariale.toLocaleString()}€`);
    console.log(`  Salaire moyen: ${stats.salaireMoyen.toLocaleString()}€`);
    
    return stats;
};

// Tests du système
const techCorp = new Entreprise("TechCorp");

// Créer des employés
const alice = new Manager("Dubois", "Alice", 75000, "IT", []);
const bob = new Developpeur("Martin", "Bob", 55000, ["JavaScript", "Python"]);
const charlie = new Developpeur("Durand", "Charlie", 52000, ["Java", "C++"]);
const diana = new Employe("Moreau", "Diana", 45000, "RH");

// Embaucher
techCorp.embaucher(alice);
techCorp.embaucher(bob);
techCorp.embaucher(charlie);
techCorp.embaucher(diana);

// Gérer l'équipe
alice.ajouterEmploye(bob);
alice.ajouterEmploye(charlie);

// Développeur activities
bob.ajouterLangage("TypeScript");
bob.assignerProjet("Site E-commerce");
charlie.assignerProjet("API REST");

// Augmentations
alice.donnerAugmentationEquipe(5);
diana.augmenterSalaire(8);

// Affichage final
techCorp.listerEmployes();
techCorp.statistiques();

console.log(`\n👨‍💼 Équipe d'Alice: ${alice.getTailleEquipe()} personnes`);
console.log(`💰 Budget équipe Alice: ${alice.getBudgetEquipe().toLocaleString()}€`);
console.log(`💻 Langages de Bob: ${bob.getLangages().join(', ')}`);
```

### Exemple avancé (optionnel)
```javascript
// Système de notification avec observers et polymorphisme
console.log("=== SYSTÈME DE NOTIFICATIONS ===");

// Interface commune pour tous les observateurs
function Observateur() {
    if (this.constructor === Observateur) {
        throw new Error("Observateur est une classe abstraite");
    }
}

Observateur.prototype.notifier = function(evenement) {
    throw new Error("La méthode notifier doit être implémentée");
};

// Observable (sujet observé)
function Observable() {
    this._observateurs = [];
}

Observable.prototype.ajouterObservateur = function(observateur) {
    if (!(observateur instanceof Observateur)) {
        throw new Error("L'observateur doit hériter d'Observateur");
    }
    this._observateurs.push(observateur);
};

Observable.prototype.retirerObservateur = function(observateur) {
    const index = this._observateurs.indexOf(observateur);
    if (index !== -1) {
        this._observateurs.splice(index, 1);
    }
};

Observable.prototype.notifierObservateurs = function(evenement) {
    this._observateurs.forEach(obs => {
        try {
            obs.notifier(evenement);
        } catch (error) {
            console.error(`Erreur notification: ${error.message}`);
        }
    });
};

// Compte bancaire observable
function CompteBancaire(numero, soldeInitial) {
    Observable.call(this);
    this._numero = numero;
    this._solde = soldeInitial;
    this._historique = [];
}

CompteBancaire.prototype = Object.create(Observable.prototype);
CompteBancaire.prototype.constructor = CompteBancaire;

CompteBancaire.prototype.deposer = function(montant) {
    this._solde += montant;
    const transaction = {
        type: "DEPOT",
        montant: montant,
        solde: this._solde,
        date: new Date()
    };
    this._historique.push(transaction);
    
    this.notifierObservateurs({
        type: "TRANSACTION",
        compte: this._numero,
        transaction: transaction
    });
};

CompteBancaire.prototype.retirer = function(montant) {
    if (montant > this._solde) {
        const evenement = {
            type: "ERREUR",
            compte: this._numero,
            message: "Solde insuffisant",
            montantDemande: montant,
            soldeActuel: this._solde
        };
        this.notifierObservateurs(evenement);
        return false;
    }
    
    this._solde -= montant;
    const transaction = {
        type: "RETRAIT",
        montant: montant,
        solde: this._solde,
        date: new Date()
    };
    this._historique.push(transaction);
    
    this.notifierObservateurs({
        type: "TRANSACTION",
        compte: this._numero,
        transaction: transaction
    });
    
    // Vérifier les seuils
    if (this._solde < 100) {
        this.notifierObservateurs({
            type: "ALERTE",
            compte: this._numero,
            message: "Solde faible",
            solde: this._solde
        });
    }
    
    return true;
};

CompteBancaire.prototype.getSolde = function() {
    return this._solde;
};

// Différents types d'observateurs
function NotificateurEmail(email) {
    Observateur.call(this);
    this.email = email;
}

NotificateurEmail.prototype = Object.create(Observateur.prototype);
NotificateurEmail.prototype.constructor = NotificateurEmail;

NotificateurEmail.prototype.notifier = function(evenement) {
    switch (evenement.type) {
        case "TRANSACTION":
            console.log(`📧 Email à ${this.email}: Transaction ${evenement.transaction.type} de ${evenement.transaction.montant}€`);
            break;
        case "ALERTE":
            console.log(`📧 Email URGENT à ${this.email}: ${evenement.message} (${evenement.solde}€)`);
            break;
        case "ERREUR":
            console.log(`📧 Email ERREUR à ${this.email}: ${evenement.message}`);
            break;
    }
};

function NotificateurSMS(telephone) {
    Observateur.call(this);
    this.telephone = telephone;
}

NotificateurSMS.prototype = Object.create(Observateur.prototype);
NotificateurSMS.prototype.constructor = NotificateurSMS;

NotificateurSMS.prototype.notifier = function(evenement) {
    if (evenement.type === "ALERTE" || evenement.type === "ERREUR") {
        console.log(`📱 SMS à ${this.telephone}: ${evenement.message}`);
    }
};

function NotificateurLogging() {
    Observateur.call(this);
    this._logs = [];
}

NotificateurLogging.prototype = Object.create(Observateur.prototype);
NotificateurLogging.prototype.constructor = NotificateurLogging;

NotificateurLogging.prototype.notifier = function(evenement) {
    const logEntry = {
        timestamp: new Date().toISOString(),
        compte: evenement.compte,
        type: evenement.type,
        details: evenement
    };
    this._logs.push(logEntry);
    console.log(`📝 LOG: ${evenement.type} sur compte ${evenement.compte}`);
};

NotificateurLogging.prototype.getLogs = function() {
    return [...this._logs];
};

// Tests du système
const compte = new CompteBancaire("FR123456789", 1000);

// Créer les observateurs
const emailNotif = new NotificateurEmail("client@example.com");
const smsNotif = new NotificateurSMS("+33123456789");
const logger = new NotificateurLogging();

// Abonner les observateurs
compte.ajouterObservateur(emailNotif);
compte.ajouterObservateur(smsNotif);
compte.ajouterObservateur(logger);

console.log("=== TESTS POLYMORPHISME ===");

// Effectuer des opérations
compte.deposer(500);
console.log(`Solde: ${compte.getSolde()}€\n`);

compte.retirer(200);
console.log(`Solde: ${compte.getSolde()}€\n`);

compte.retirer(1250); // Solde faible
console.log(`Solde: ${compte.getSolde()}€\n`);

compte.retirer(100); // Erreur
console.log(`Solde: ${compte.getSolde()}€\n`);

// Afficher les logs
console.log("📋 HISTORIQUE DES LOGS:");
logger.getLogs().forEach((log, index) => {
    console.log(`${index + 1}. [${log.timestamp}] ${log.type} - Compte ${log.compte}`);
});
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Système de commandes e-commerce
**Consigne :** Créez un système de commandes avec Produit, Commande et Client en utilisant l'héritage et l'encapsulation.

**Code de départ :**
```javascript
// TODO: Implémenter les classes suivantes

function Produit(nom, prix, stock) {
    // Propriétés et méthodes pour gérer un produit
}

function Commande(client) {
    // Gérer une commande avec produits, quantités, total
}

function Client(nom, email) {
    // Gérer un client avec son historique de commandes
}

// Bonus: Implémenter différents types de produits (physique, numérique)
// avec des comportements différents pour la livraison
```

**Résultat attendu :**
- Encapsulation des données sensibles
- Méthodes pour ajouter/retirer produits
- Calcul automatique des totaux
- Historique des commandes par client

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Jeu avec personnages et héritage
**Consigne :** Créez un système de jeu avec différents types de personnages héritant d'une classe de base.

**Contraintes :**
- Classe Personnage de base avec vie, attaque, défense
- Classes héritées: Guerrier, Mage, Archer
- Chaque classe a des capacités spéciales différentes
- Système de combat polymorphe

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Framework MVC simple (optionnel)
**Consigne :** Implémentez un mini-framework MVC avec observateurs.

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// Classe de base Produit
function Produit(nom, prix, stock, categorie) {
    this._id = Produit._prochainId++;
    this.nom = nom;
    this._prix = prix;
    this._stock = stock;
    this.categorie = categorie;
    this._dateCreation = new Date();
}

Produit._prochainId = 1;

Produit.prototype.getPrix = function() {
    return this._prix;
};

Produit.prototype.getStock = function() {
    return this._stock;
};

Produit.prototype.retirerStock = function(quantite) {
    if (this._stock >= quantite) {
        this._stock -= quantite;
        return true;
    }
    return false;
};

Produit.prototype.ajouterStock = function(quantite) {
    this._stock += quantite;
};

Produit.prototype.toString = function() {
    return `${this.nom} - ${this._prix}€ (Stock: ${this._stock})`;
};

// Produit physique (hérite de Produit)
function ProduitPhysique(nom, prix, stock, poids, dimensions) {
    Produit.call(this, nom, prix, stock, "Physique");
    this.poids = poids;
    this.dimensions = dimensions;
    this._fraisLivraison = this.calculerFraisLivraison();
}

ProduitPhysique.prototype = Object.create(Produit.prototype);
ProduitPhysique.prototype.constructor = ProduitPhysique;

ProduitPhysique.prototype.calculerFraisLivraison = function() {
    // Frais basés sur le poids
    return Math.max(5, this.poids * 0.5);
};

ProduitPhysique.prototype.getFraisLivraison = function() {
    return this._fraisLivraison;
};

// Produit numérique (hérite de Produit)
function ProduitNumerique(nom, prix, lienTelechargement, tailleFichier) {
    Produit.call(this, nom, prix, Infinity, "Numérique"); // Stock infini
    this._lienTelechargement = lienTelechargement;
    this.tailleFichier = tailleFichier;
}

ProduitNumerique.prototype = Object.create(Produit.prototype);
ProduitNumerique.prototype.constructor = ProduitNumerique;

ProduitNumerique.prototype.getLienTelechargement = function() {
    return this._lienTelechargement;
};

ProduitNumerique.prototype.getFraisLivraison = function() {
    return 0; // Pas de frais pour produits numériques
};

// Classe Client
function Client(nom, email, adresse) {
    this._id = Client._prochainId++;
    this.nom = nom;
    this.email = email;
    this.adresse = adresse;
    this._commandes = [];
    this._dateInscription = new Date();
}

Client._prochainId = 1;

Client.prototype.ajouterCommande = function(commande) {
    this._commandes.push(commande);
};

Client.prototype.getHistoriqueCommandes = function() {
    return [...this._commandes];
};

Client.prototype.getTotalDepense = function() {
    return this._commandes.reduce((total, commande) => 
        total + commande.getTotal(), 0);
};

Client.prototype.getNombreCommandes = function() {
    return this._commandes.length;
};

// Classe Commande
function Commande(client) {
    this._id = Commande._prochainId++;
    this.client = client;
    this._lignesCommande = [];
    this._dateCommande = new Date();
    this._statut = "En cours";
    this._sousTotal = 0;
    this._fraisLivraison = 0;
    this._total = 0;
}

Commande._prochainId = 1;

Commande.prototype.ajouterProduit = function(produit, quantite) {
    // Vérifier le stock disponible
    if (produit.getStock() < quantite) {
        throw new Error(`Stock insuffisant pour ${produit.nom}. Disponible: ${produit.getStock()}`);
    }
    
    // Chercher si le produit est déjà dans la commande
    const ligneExistante = this._lignesCommande.find(ligne => 
        ligne.produit._id === produit._id);
    
    if (ligneExistante) {
        // Augmenter la quantité
        const nouvelleQuantite = ligneExistante.quantite + quantite;
        if (produit.getStock() < nouvelleQuantite) {
            throw new Error(`Stock insuffisant pour ${produit.nom}`);
        }
        ligneExistante.quantite = nouvelleQuantite;
        ligneExistante.sousTotal = ligneExistante.quantite * produit.getPrix();
    } else {
        // Ajouter nouvelle ligne
        this._lignesCommande.push({
            produit: produit,
            quantite: quantite,
            prixUnitaire: produit.getPrix(),
            sousTotal: quantite * produit.getPrix()
        });
    }
    
    this.calculerTotaux();
    console.log(`✅ ${quantite}x ${produit.nom} ajouté à la commande`);
};

Commande.prototype.retirerProduit = function(produitId) {
    const index = this._lignesCommande.findIndex(ligne => 
        ligne.produit._id === produitId);
    
    if (index !== -1) {
        const ligne = this._lignesCommande.splice(index, 1)[0];
        this.calculerTotaux();
        console.log(`❌ ${ligne.produit.nom} retiré de la commande`);
        return true;
    }
    return false;
};

Commande.prototype.calculerTotaux = function() {
    this._sousTotal = this._lignesCommande.reduce((total, ligne) => 
        total + ligne.sousTotal, 0);
    
    this._fraisLivraison = this._lignesCommande.reduce((total, ligne) => 
        total + ligne.produit.getFraisLivraison(), 0);
    
    this._total = this._sousTotal + this._fraisLivraison;
};

Commande.prototype.valider = function() {
    if (this._lignesCommande.length === 0) {
        throw new Error("Impossible de valider une commande vide");
    }
    
    // Retirer le stock pour chaque produit
    this._lignesCommande.forEach(ligne => {
        if (!ligne.produit.retirerStock(ligne.quantite)) {
            throw new Error(`Stock insuffisant pour ${ligne.produit.nom}`);
        }
    });
    
    this._statut = "Validée";
    this.client.ajouterCommande(this);
    
    console.log(`🎉 Commande #${this._id} validée pour ${this.client.nom}`);
    return this._id;
};

Commande.prototype.afficherRecapitulatif = function() {
    console.log(`\n📋 Commande #${this._id} - ${this.client.nom}`);
    console.log(`📅 Date: ${this._dateCommande.toLocaleDateString()}`);
    console.log(`🎯 Statut: ${this._statut}\n`);
    
    this._lignesCommande.forEach(ligne => {
        console.log(`  ${ligne.quantite}x ${ligne.produit.nom}`);
        console.log(`      ${ligne.prixUnitaire}€ x ${ligne.quantite} = ${ligne.sousTotal}€`);
    });
    
    console.log(`\n💰 Sous-total: ${this._sousTotal}€`);
    console.log(`🚚 Frais de livraison: ${this._fraisLivraison}€`);
    console.log(`💳 TOTAL: ${this._total}€`);
};

Commande.prototype.getTotal = function() {
    return this._total;
};

// Tests du système
console.log("=== SYSTÈME E-COMMERCE ===");

// Créer des produits
const laptop = new ProduitPhysique("MacBook Pro", 2499, 10, 2.0, "35x24x1.5cm");
const souris = new ProduitPhysique("Souris Gaming", 79, 50, 0.15, "12x7x4cm");
const logiciel = new ProduitNumerique("Photoshop", 239, "https://download.adobe.com/ps", "2.5GB");

// Créer des clients
const alice = new Client("Alice Dubois", "alice@example.com", "123 Rue de la Paix, Paris");
const bob = new Client("Bob Martin", "bob@example.com", "456 Avenue des Champs, Lyon");

// Créer une commande pour Alice
const commandeAlice = new Commande(alice);

try {
    commandeAlice.ajouterProduit(laptop, 1);
    commandeAlice.ajouterProduit(souris, 2);
    commandeAlice.ajouterProduit(logiciel, 1);
    
    commandeAlice.afficherRecapitulatif();
    commandeAlice.valider();
    
} catch (error) {
    console.error("Erreur:", error.message);
}

// Créer une commande pour Bob
const commandeBob = new Commande(bob);

try {
    commandeBob.ajouterProduit(souris, 1);
    commandeBob.ajouterProduit(logiciel, 1);
    commandeBob.afficherRecapitulatif();
    commandeBob.valider();
    
} catch (error) {
    console.error("Erreur:", error.message);
}

// Statistiques clients
console.log(`\n📊 Statistiques Alice:`);
console.log(`  Commandes: ${alice.getNombreCommandes()}`);
console.log(`  Total dépensé: ${alice.getTotalDepense()}€`);

console.log(`\n📊 Statistiques Bob:`);
console.log(`  Commandes: ${bob.getNombreCommandes()}`);
console.log(`  Total dépensé: ${bob.getTotalDepense()}€`);

console.log(`\n📦 Stock restant:`);
console.log(`  ${laptop.toString()}`);
console.log(`  ${souris.toString()}`);
console.log(`  ${logiciel.toString()}`);
```

**Explication :** Le système utilise l'héritage pour différencier les produits physiques et numériques, l'encapsulation pour protéger les données sensibles, et le polymorphisme pour les frais de livraison.

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Objets** : Création et manipulation d'objets complexes
- **Fonctions** : Constructeurs et méthodes
- **Prototypes** : Mécanisme d'héritage de JavaScript
- **This** : Contexte d'exécution dans les méthodes

### Prochaines étapes
- **Classes ES6** : Syntaxe moderne pour la POO
- **Modules** : Organisation du code orienté objet
- **Design Patterns** : Patterns de conception avancés

### Concepts complémentaires
- **Testing** : Tests unitaires pour les classes
- **Documentation** : Documenter les APIs d'objets
- **Performance** : Optimisation des systèmes orientés objet

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un simulateur de zoo virtuel avec différents animaux

**Fonctionnalités requises :**
- [ ] Classe Animal de base avec propriétés communes
- [ ] Classes spécialisées (Mammifère, Oiseau, Reptile)
- [ ] Système d'alimentation et de soins
- [ ] Observateurs pour les événements (maladie, naissance, etc.)
- [ ] Interface de gestion du zoo avec statistiques

**Structure suggérée :**
```
📁 zoo-virtuel/
├── 📄 index.html
├── 📄 style.css
├── 📄 animal.js
├── 📄 zoo.js
└── 📄 interface.js
```

**Temps estimé :** 120 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [MDN - Object-oriented JavaScript introduction](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS)

### Vidéos recommandées
- "Object Oriented Programming in JavaScript" - Programming with Mosh (30 min)
- Pourquoi cette vidéo est utile : Explications claires des concepts POO

### Articles approfondis
- "You Don't Know JS: this & Object Prototypes" - Kyle Simpson
- Ce que cet article apporte en plus : Compréhension profonde des prototypes

### Outils de pratique
- [JavaScript OOP Exercises](https://github.com/getify/You-Dont-Know-JS)
- [Object-Oriented Programming Challenges](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre héritage par prototype et héritage par classe ?
   - Réponse : JavaScript utilise les prototypes (délégation), pas de vraies classes comme Java/C#

2. **Question pratique :** Comment créer une propriété "privée" en JavaScript ?
   - Réponse : Utiliser la convention _ ou les closures/WeakMap pour une vraie privacité

3. **Question d'application :** Que fait `Object.create()` ?
   - Réponse : Crée un nouvel objet avec le prototype spécifié

### Checklist de maîtrise
- [ ] Je comprends les 4 piliers de la POO
- [ ] Je sais implémenter l'héritage avec prototypes
- [ ] Je maîtrise l'encapsulation et les conventions de privacité
- [ ] Je comprends le polymorphisme en JavaScript
- [ ] Je peux créer des hiérarchies d'objets complexes
- [ ] Je reconnais quand utiliser la POO vs autres paradigmes

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
function Animal(nom) {
    this.nom = nom;
}

function Chien(nom, race) {
    this.nom = nom;  // ❌ N'appelle pas le constructeur parent
    this.race = race;
}

Chien.prototype = Animal.prototype;  // ❌ Référence directe
```

**Solution :**
```javascript
function Chien(nom, race) {
    Animal.call(this, nom);  // ✅ Appel du constructeur parent
    this.race = race;
}

Chien.prototype = Object.create(Animal.prototype);  // ✅ Vraie copie
Chien.prototype.constructor = Chien;
```

**Explication :** Il faut appeler le constructeur parent et créer une vraie chaîne de prototypes.

### Erreur fréquente 2
**Code problématique :**
```javascript
function Classe() {
    this.methode = function() {  // ❌ Méthode dans le constructeur
        console.log("Hello");
    };
}
```

**Solution :**
```javascript
function Classe() {
    // Constructeur vide ou initialisation
}

Classe.prototype.methode = function() {  // ✅ Méthode dans le prototype
    console.log("Hello");
};
```

**Explication :** Les méthodes dans le prototype sont partagées entre instances, plus efficace.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- Les principes fondamentaux de la POO en JavaScript
- L'importance de bien structurer les hiérarchies d'objets
- Les patterns de création et d'héritage

### Questions à approfondir
- Classes ES6 vs fonctions constructeurs
- Mixins et composition d'objets

### Révision programmée
- [ ] Révision dans 1 jour (rappel rapide)
- [ ] Révision dans 1 semaine (exercices)
- [ ] Révision dans 1 mois (application dans un projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #poo #heritage #encapsulation #polymorphisme #prototypes #intermediaire

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 9/126*
