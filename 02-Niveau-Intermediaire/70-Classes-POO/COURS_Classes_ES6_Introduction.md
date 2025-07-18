# Classes ES6 - Introduction à la Syntaxe Moderne

## **INFORMATIONS GENERALES**

### Metadonnées du cours
- **Titre :** Classes ES6 - Transition vers la POO Moderne
- **Niveau :** Intermédiaire
- **Durée estimée :** 80 minutes
- **Prérequis :** Objets, POO traditionnelle, prototypes, fonctions constructeurs
- **Date de création :** 17/07/2025
- **Dernière révision :** 17/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : La syntaxe des classes ES6 et ses avantages
- [ ] **Appliquer** : La conversion de fonctions constructeurs en classes
- [ ] **Analyser** : Les différences entre classes et prototypes
- [ ] **Créer** : Des hiérarchies de classes modernes et maintenables

### Mots-clés à retenir
- **Class** : Syntaxe moderne pour créer des "classes" en JavaScript
- **Constructor** : Méthode spéciale d'initialisation d'une classe
- **Extends** : Mot-clé pour l'héritage entre classes
- **Super** : Référence à la classe parent
- **Static** : Méthodes et propriétés de classe (non d'instance)
- **Private** : Champs privés avec la syntaxe #

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les classes ES6 offrent une syntaxe plus claire et plus familière pour la POO, tout en restant compatibles avec le système de prototypes de JavaScript. Elles facilitent la transition pour les développeurs venant d'autres langages orientés objet et rendent le code plus lisible et maintenable.

### Définition et concepts clés

#### **1. Syntaxe de base des classes**
```javascript
// Avant ES6 - Fonction constructeur
function PersonneAvant(nom, age) {
    this.nom = nom;
    this.age = age;
}

PersonneAvant.prototype.sePresenter = function() {
    return `Je suis ${this.nom}, ${this.age} ans`;
};

// Avec ES6 - Classe
class Personne {
    constructor(nom, age) {
        this.nom = nom;
        this.age = age;
    }
    
    sePresenter() {
        return `Je suis ${this.nom}, ${this.age} ans`;
    }
}
```

#### **2. Héritage avec extends et super**
```javascript
// Classe parent
class Animal {
    constructor(nom, espece) {
        this.nom = nom;
        this.espece = espece;
    }
    
    parler() {
        return `${this.nom} fait du bruit`;
    }
    
    dormir() {
        return `${this.nom} dort paisiblement`;
    }
}

// Classe enfant
class Chien extends Animal {
    constructor(nom, race) {
        super(nom, "Chien"); // Appel du constructeur parent
        this.race = race;
    }
    
    // Surcharge de méthode
    parler() {
        return `${this.nom} aboie: Woof! Woof!`;
    }
    
    // Nouvelle méthode
    rapporter() {
        return `${this.nom} rapporte la balle`;
    }
}
```

#### **3. Méthodes et propriétés statiques**
```javascript
class MathUtils {
    // Propriété statique
    static PI = 3.14159;
    
    // Méthode statique
    static carre(x) {
        return x * x;
    }
    
    static aire(rayon) {
        return MathUtils.PI * MathUtils.carre(rayon);
    }
}

// Utilisation sans instanciation
console.log(MathUtils.carre(5)); // 25
console.log(MathUtils.aire(3));  // 28.27
```

#### **4. Champs privés (syntaxe moderne)**
```javascript
class CompteBancaire {
    // Champs privés
    #solde = 0;
    #numero;
    
    constructor(numero, soldeInitial) {
        this.#numero = numero;
        this.#solde = soldeInitial;
    }
    
    // Méthode privée
    #validerMontant(montant) {
        return montant > 0;
    }
    
    deposer(montant) {
        if (this.#validerMontant(montant)) {
            this.#solde += montant;
            return true;
        }
        return false;
    }
    
    getSolde() {
        return this.#solde;
    }
}
```

### Syntaxe de base
```javascript
class NomClasse {
    // Champs publics (optionnel)
    proprietePublique = "valeur";
    
    // Champs privés
    #proprietePrive = "secret";
    
    // Constructeur
    constructor(parametres) {
        // Initialisation
    }
    
    // Méthodes d'instance
    methodePublique() {
        return this.#proprietePrive;
    }
    
    // Méthodes privées
    #methodePrivee() {
        // Logique interne
    }
    
    // Méthodes statiques
    static methodeStatique() {
        return "Méthode de classe";
    }
    
    // Getters et setters
    get propriete() {
        return this.#proprietePrive;
    }
    
    set propriete(valeur) {
        this.#proprietePrive = valeur;
    }
}
```

### Points d'attention
- **Piège hoisting** : Les classes ne sont pas hissées comme les fonctions
- **Piège this** : Le contexte peut toujours être perdu dans les callbacks
- **Bonne pratique** : Utiliser les champs privés pour l'encapsulation réelle

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Conversion d'un système de véhicules en classes ES6
console.log("=== SYSTÈME VÉHICULES AVEC CLASSES ES6 ===");

// Classe de base Vehicule
class Vehicule {
    // Champs publics
    static compteurVehicules = 0;
    
    constructor(marque, modele, annee) {
        this.marque = marque;
        this.modele = modele;
        this.annee = annee;
        this.id = ++Vehicule.compteurVehicules;
        
        console.log(`🚗 Nouveau véhicule créé: ${this.getDescription()}`);
    }
    
    getDescription() {
        return `${this.marque} ${this.modele} (${this.annee})`;
    }
    
    demarrer() {
        return `${this.getDescription()} démarre`;
    }
    
    arreter() {
        return `${this.getDescription()} s'arrête`;
    }
    
    getAge() {
        return new Date().getFullYear() - this.annee;
    }
    
    // Méthode statique
    static getTotalVehicules() {
        return Vehicule.compteurVehicules;
    }
}

// Classe Voiture héritant de Vehicule
class Voiture extends Vehicule {
    constructor(marque, modele, annee, portes, carburant) {
        super(marque, modele, annee); // Appel du constructeur parent
        this.portes = portes;
        this.carburant = carburant;
        this.vitesse = 0;
    }
    
    // Surcharge de méthode
    demarrer() {
        const messageParent = super.demarrer(); // Appel méthode parent
        return `${messageParent} avec ${this.carburant}`;
    }
    
    accelerer(augmentation) {
        this.vitesse += augmentation;
        return `${this.getDescription()} accélère à ${this.vitesse} km/h`;
    }
    
    freiner() {
        this.vitesse = Math.max(0, this.vitesse - 20);
        return `${this.getDescription()} freine, vitesse: ${this.vitesse} km/h`;
    }
    
    getType() {
        return `Voiture ${this.portes} portes`;
    }
}

// Classe Moto héritant de Vehicule
class Moto extends Vehicule {
    constructor(marque, modele, annee, cylindree) {
        super(marque, modele, annee);
        this.cylindree = cylindree;
        this.casqueObligatoire = true;
    }
    
    demarrer() {
        return `${super.demarrer()} - VROOOOM! (${this.cylindree}cc)`;
    }
    
    faireBruit() {
        return `La ${this.getDescription()} fait VROOM VROOM!`;
    }
    
    getType() {
        return `Moto ${this.cylindree}cc`;
    }
}

// Tests des classes
const voiture1 = new Voiture("Toyota", "Corolla", 2020, 4, "essence");
const voiture2 = new Voiture("Tesla", "Model 3", 2022, 4, "électrique");
const moto1 = new Moto("Yamaha", "R1", 2021, 1000);

console.log("\n=== TESTS MÉTHODES ===");
console.log(voiture1.demarrer());
console.log(voiture1.accelerer(50));
console.log(voiture1.freiner());

console.log(voiture2.demarrer());
console.log(voiture2.accelerer(80));

console.log(moto1.demarrer());
console.log(moto1.faireBruit());

console.log("\n=== INFORMATIONS ===");
console.log(`Âge voiture 1: ${voiture1.getAge()} ans`);
console.log(`Type: ${voiture1.getType()}`);
console.log(`Type moto: ${moto1.getType()}`);
console.log(`Total véhicules créés: ${Vehicule.getTotalVehicules()}`);

// Vérification héritage
console.log("\n=== VÉRIFICATION HÉRITAGE ===");
console.log("voiture1 instanceof Vehicule:", voiture1 instanceof Vehicule);
console.log("voiture1 instanceof Voiture:", voiture1 instanceof Voiture);
console.log("moto1 instanceof Vehicule:", moto1 instanceof Vehicule);
console.log("moto1 instanceof Voiture:", moto1 instanceof Voiture);
```

**Résultat attendu :**
```
=== SYSTÈME VÉHICULES AVEC CLASSES ES6 ===
🚗 Nouveau véhicule créé: Toyota Corolla (2020)
🚗 Nouveau véhicule créé: Tesla Model 3 (2022)
🚗 Nouveau véhicule créé: Yamaha R1 (2021)

=== TESTS MÉTHODES ===
Toyota Corolla (2020) démarre avec essence
Toyota Corolla (2020) accélère à 50 km/h
Toyota Corolla (2020) freine, vitesse: 30 km/h
Tesla Model 3 (2022) démarre avec électrique
Tesla Model 3 (2022) accélère à 80 km/h
Yamaha R1 (2021) démarre - VROOOOM! (1000cc)
La Yamaha R1 (2021) fait VROOM VROOM!

=== INFORMATIONS ===
Âge voiture 1: 5 ans
Type: Voiture 4 portes
Type moto: Moto 1000cc
Total véhicules créés: 3

=== VÉRIFICATION HÉRITAGE ===
voiture1 instanceof Vehicule: true
voiture1 instanceof Voiture: true
moto1 instanceof Vehicule: true
moto1 instanceof Voiture: false
```

### Exemple intermédiaire
```javascript
// Système de gestion d'employés avec classes modernes
console.log("=== SYSTÈME RH AVEC CLASSES ES6 ===");

// Classe de base avec champs privés
class Employe {
    // Champs privés
    #salaire;
    #dateEmbauche;
    
    // Champ statique pour compteur
    static #nombreEmployes = 0;
    static #salaireMinimum = 25000;
    
    constructor(nom, prenom, salaire, poste) {
        this.nom = nom;
        this.prenom = prenom;
        this.poste = poste;
        this.#salaire = Math.max(salaire, Employe.#salaireMinimum);
        this.#dateEmbauche = new Date();
        this.id = ++Employe.#nombreEmployes;
    }
    
    // Getter pour salaire (lecture seule depuis l'extérieur)
    get salaire() {
        return this.#salaire;
    }
    
    // Setter avec validation
    set salaire(nouveauSalaire) {
        if (nouveauSalaire >= Employe.#salaireMinimum) {
            const augmentation = ((nouveauSalaire - this.#salaire) / this.#salaire * 100).toFixed(1);
            this.#salaire = nouveauSalaire;
            console.log(`💰 Salaire mis à jour pour ${this.getNomComplet()}: +${augmentation}%`);
        } else {
            throw new Error(`Salaire trop bas. Minimum: ${Employe.#salaireMinimum}€`);
        }
    }
    
    get dateEmbauche() {
        return new Date(this.#dateEmbauche); // Copie pour éviter modification
    }
    
    getNomComplet() {
        return `${this.prenom} ${this.nom}`;
    }
    
    getAnciennete() {
        const diffMs = Date.now() - this.#dateEmbauche.getTime();
        return Math.floor(diffMs / (1000 * 60 * 60 * 24 * 365));
    }
    
    // Méthode privée pour calculs internes
    #calculerPrime(pourcentage) {
        return this.#salaire * (pourcentage / 100);
    }
    
    obtenirPrimeAnciennete() {
        const anciennete = this.getAnciennete();
        if (anciennete >= 5) return this.#calculerPrime(10);
        if (anciennete >= 2) return this.#calculerPrime(5);
        return 0;
    }
    
    afficherInfos() {
        return {
            id: this.id,
            nom: this.getNomComplet(),
            poste: this.poste,
            salaire: this.#salaire,
            anciennete: this.getAnciennete(),
            prime: this.obtenirPrimeAnciennete()
        };
    }
    
    // Méthodes statiques
    static getNombreEmployes() {
        return Employe.#nombreEmployes;
    }
    
    static getSalaireMinimum() {
        return Employe.#salaireMinimum;
    }
    
    static setSalaireMinimum(nouveauMinimum) {
        Employe.#salaireMinimum = nouveauMinimum;
        console.log(`📋 Nouveau salaire minimum: ${nouveauMinimum}€`);
    }
}

// Classe Manager héritant d'Employe
class Manager extends Employe {
    #equipe = [];
    #budget;
    
    constructor(nom, prenom, salaire, departement, budget) {
        super(nom, prenom, salaire, "Manager");
        this.departement = departement;
        this.#budget = budget;
    }
    
    get budget() {
        return this.#budget;
    }
    
    set budget(nouveauBudget) {
        if (nouveauBudget >= 0) {
            console.log(`💼 Budget ${this.departement} mis à jour: ${nouveauBudget}€`);
            this.#budget = nouveauBudget;
        } else {
            throw new Error("Le budget ne peut pas être négatif");
        }
    }
    
    ajouterEmploye(employe) {
        if (employe instanceof Employe && employe !== this) {
            this.#equipe.push(employe);
            console.log(`👥 ${employe.getNomComplet()} ajouté à l'équipe ${this.departement}`);
            return true;
        }
        return false;
    }
    
    retirerEmploye(employeId) {
        const index = this.#equipe.findIndex(emp => emp.id === employeId);
        if (index !== -1) {
            const employe = this.#equipe.splice(index, 1)[0];
            console.log(`👋 ${employe.getNomComplet()} retiré de l'équipe`);
            return employe;
        }
        return null;
    }
    
    get tailleEquipe() {
        return this.#equipe.length;
    }
    
    getBudgetEquipe() {
        return this.#equipe.reduce((total, emp) => total + emp.salaire, 0);
    }
    
    donnerAugmentationEquipe(pourcentage) {
        console.log(`🎉 Augmentation de ${pourcentage}% pour l'équipe ${this.departement}`);
        this.#equipe.forEach(emp => {
            emp.salaire = emp.salaire * (1 + pourcentage / 100);
        });
    }
    
    // Surcharge de la méthode parent
    afficherInfos() {
        const infosBase = super.afficherInfos();
        return {
            ...infosBase,
            departement: this.departement,
            budget: this.#budget,
            tailleEquipe: this.tailleEquipe,
            budgetEquipe: this.getBudgetEquipe()
        };
    }
}

// Classe Developpeur avec spécialisations
class Developpeur extends Employe {
    #langages = new Set();
    #projets = [];
    
    constructor(nom, prenom, salaire, specialite, langages = []) {
        super(nom, prenom, salaire, `Développeur ${specialite}`);
        this.specialite = specialite;
        langages.forEach(lang => this.#langages.add(lang));
    }
    
    ajouterLangage(langage) {
        if (!this.#langages.has(langage)) {
            this.#langages.add(langage);
            console.log(`💻 ${this.getNomComplet()} maîtrise maintenant ${langage}`);
            return true;
        }
        return false;
    }
    
    get langages() {
        return Array.from(this.#langages);
    }
    
    assignerProjet(nomProjet, complexite = "Moyenne") {
        const projet = {
            nom: nomProjet,
            complexite: complexite,
            dateDebut: new Date(),
            statut: "En cours",
            progression: 0
        };
        
        this.#projets.push(projet);
        console.log(`📋 Projet "${nomProjet}" assigné à ${this.getNomComplet()}`);
        return projet;
    }
    
    mettreAJourProjet(nomProjet, progression) {
        const projet = this.#projets.find(p => p.nom === nomProjet);
        if (projet && projet.statut === "En cours") {
            projet.progression = Math.min(100, Math.max(0, progression));
            if (projet.progression === 100) {
                projet.statut = "Terminé";
                projet.dateFin = new Date();
                console.log(`✅ Projet "${nomProjet}" terminé par ${this.getNomComplet()}`);
            }
            return true;
        }
        return false;
    }
    
    get projetsActifs() {
        return this.#projets.filter(p => p.statut === "En cours");
    }
    
    get projetsTermines() {
        return this.#projets.filter(p => p.statut === "Terminé");
    }
    
    // Calcul de prime basé sur la productivité
    obtenirPrimeProductivite() {
        const projetsTermines = this.projetsTermines.length;
        const projetsActifs = this.projetsActifs.length;
        
        if (projetsTermines >= 3) return this.salaire * 0.15;
        if (projetsTermines >= 1) return this.salaire * 0.08;
        return 0;
    }
    
    afficherInfos() {
        const infosBase = super.afficherInfos();
        return {
            ...infosBase,
            specialite: this.specialite,
            langages: this.langages,
            projetsActifs: this.projetsActifs.length,
            projetsTermines: this.projetsTermines.length,
            primeProductivite: this.obtenirPrimeProductivite()
        };
    }
}

// Tests du système
console.log("Création de l'équipe...\n");

// Créer des employés
const alice = new Manager("Dubois", "Alice", 75000, "IT", 500000);
const bob = new Developpeur("Martin", "Bob", 55000, "Frontend", ["JavaScript", "React"]);
const charlie = new Developpeur("Durand", "Charlie", 60000, "Backend", ["Python", "Node.js"]);
const diana = new Employe("Moreau", "Diana", 45000, "Designer");

// Tests des fonctionnalités
try {
    // Gestion d'équipe
    alice.ajouterEmploye(bob);
    alice.ajouterEmploye(charlie);
    alice.ajouterEmploye(diana);
    
    // Développement
    bob.ajouterLangage("TypeScript");
    bob.assignerProjet("Site E-commerce", "Élevée");
    bob.mettreAJourProjet("Site E-commerce", 75);
    
    charlie.assignerProjet("API REST", "Moyenne");
    charlie.assignerProjet("Base de données", "Élevée");
    charlie.mettreAJourProjet("API REST", 100);
    
    // Augmentations
    alice.donnerAugmentationEquipe(8);
    
    // Modification budget
    alice.budget = 600000;
    
    console.log("\n=== INFORMATIONS ÉQUIPE ===");
    console.log("👨‍💼 Manager:", alice.afficherInfos());
    console.log("👨‍💻 Dev Frontend:", bob.afficherInfos());
    console.log("👨‍💻 Dev Backend:", charlie.afficherInfos());
    console.log("🎨 Designer:", diana.afficherInfos());
    
    console.log(`\n📊 Statistiques globales:`);
    console.log(`Total employés: ${Employe.getNombreEmployes()}`);
    console.log(`Salaire minimum: ${Employe.getSalaireMinimum()}€`);
    console.log(`Budget équipe IT: ${alice.getBudgetEquipe()}€`);
    
} catch (error) {
    console.error("Erreur:", error.message);
}
```

### Exemple avancé (optionnel)
```javascript
// Système de notification avec classes modernes et champs privés
console.log("=== SYSTÈME NOTIFICATIONS MODERNE ===");

// Classe abstraite de base
class Notification {
    // Champs privés
    #id;
    #timestamp;
    #lu = false;
    
    // Compteur statique
    static #compteur = 0;
    
    constructor(titre, message, priorite = "normale") {
        if (this.constructor === Notification) {
            throw new Error("Notification est une classe abstraite");
        }
        
        this.#id = ++Notification.#compteur;
        this.titre = titre;
        this.message = message;
        this.priorite = priorite;
        this.#timestamp = new Date();
    }
    
    get id() {
        return this.#id;
    }
    
    get timestamp() {
        return new Date(this.#timestamp);
    }
    
    get lu() {
        return this.#lu;
    }
    
    marquerCommeLu() {
        if (!this.#lu) {
            this.#lu = true;
            this.onMarquageLu();
        }
    }
    
    // Méthode abstraite - doit être implémentée par les sous-classes
    afficher() {
        throw new Error("La méthode afficher() doit être implémentée");
    }
    
    // Hook pour les sous-classes
    onMarquageLu() {
        console.log(`📖 Notification #${this.#id} marquée comme lue`);
    }
    
    // Méthode template
    traiter() {
        console.log(`🔄 Traitement de la notification #${this.#id}...`);
        this.afficher();
        this.marquerCommeLu();
    }
    
    static getTotalNotifications() {
        return Notification.#compteur;
    }
}

// Notification Email
class NotificationEmail extends Notification {
    #destinataire;
    #expediteur;
    
    constructor(titre, message, destinataire, expediteur, priorite) {
        super(titre, message, priorite);
        this.#destinataire = destinataire;
        this.#expediteur = expediteur;
    }
    
    get destinataire() {
        return this.#destinataire;
    }
    
    afficher() {
        console.log(`📧 EMAIL - De: ${this.#expediteur}`);
        console.log(`   À: ${this.#destinataire}`);
        console.log(`   Titre: ${this.titre}`);
        console.log(`   Message: ${this.message}`);
        console.log(`   Priorité: ${this.priorite}`);
    }
    
    onMarquageLu() {
        super.onMarquageLu();
        this.#envoyerAccuseReception();
    }
    
    #envoyerAccuseReception() {
        console.log(`✅ Accusé de réception envoyé à ${this.#expediteur}`);
    }
}

// Notification Push
class NotificationPush extends Notification {
    #deviceId;
    #icone;
    
    constructor(titre, message, deviceId, icone = "default", priorite) {
        super(titre, message, priorite);
        this.#deviceId = deviceId;
        this.#icone = icone;
    }
    
    afficher() {
        console.log(`📱 PUSH - Device: ${this.#deviceId}`);
        console.log(`   Titre: ${this.titre}`);
        console.log(`   Message: ${this.message}`);
        console.log(`   Icône: ${this.#icone}`);
        
        // Simulation d'affichage graphique
        this.#afficherGraphiquement();
    }
    
    #afficherGraphiquement() {
        console.log("┌─────────────────────────────┐");
        console.log(`│ ${this.#icone} ${this.titre.padEnd(24)} │`);
        console.log(`│ ${this.message.padEnd(27)} │`);
        console.log("└─────────────────────────────┘");
    }
}

// Notification SMS
class NotificationSMS extends Notification {
    #numeroTelephone;
    #operateur;
    
    constructor(titre, message, numeroTelephone, operateur, priorite) {
        super(titre, message, priorite);
        this.#numeroTelephone = numeroTelephone;
        this.#operateur = operateur;
    }
    
    afficher() {
        console.log(`📞 SMS - Numéro: ${this.#numeroTelephone}`);
        console.log(`   Opérateur: ${this.#operateur}`);
        console.log(`   Message: ${this.titre} - ${this.message}`);
        
        if (this.message.length > 160) {
            console.log(`⚠️  Message long (${this.message.length} caractères) - Facturé en plusieurs SMS`);
        }
    }
}

// Gestionnaire de notifications
class GestionnaireNotifications {
    #notifications = [];
    #abonnes = new Set();
    
    constructor() {
        console.log("📋 Gestionnaire de notifications initialisé");
    }
    
    ajouterNotification(notification) {
        if (!(notification instanceof Notification)) {
            throw new Error("Doit être une instance de Notification");
        }
        
        this.#notifications.push(notification);
        this.#notifierAbonnes("nouvelle", notification);
        
        console.log(`➕ Notification #${notification.id} ajoutée`);
    }
    
    traiterNotification(id) {
        const notification = this.#notifications.find(n => n.id === id);
        if (notification) {
            notification.traiter();
            this.#notifierAbonnes("traitee", notification);
        } else {
            console.log(`❌ Notification #${id} non trouvée`);
        }
    }
    
    traiterToutes() {
        console.log("\n🔄 Traitement de toutes les notifications...");
        const nonLues = this.#notifications.filter(n => !n.lu);
        
        nonLues.forEach(notification => {
            console.log(`\n--- Notification #${notification.id} ---`);
            notification.traiter();
        });
        
        console.log(`\n✅ ${nonLues.length} notification(s) traitée(s)`);
    }
    
    obtenirStatistiques() {
        const total = this.#notifications.length;
        const lues = this.#notifications.filter(n => n.lu).length;
        const nonLues = total - lues;
        
        const parType = this.#notifications.reduce((acc, notif) => {
            const type = notif.constructor.name;
            acc[type] = (acc[type] || 0) + 1;
            return acc;
        }, {});
        
        return {
            total,
            lues,
            nonLues,
            parType
        };
    }
    
    // Pattern Observer
    abonner(callback) {
        this.#abonnes.add(callback);
    }
    
    desabonner(callback) {
        this.#abonnes.delete(callback);
    }
    
    #notifierAbonnes(evenement, notification) {
        this.#abonnes.forEach(callback => {
            try {
                callback(evenement, notification);
            } catch (error) {
                console.error("Erreur lors de la notification:", error);
            }
        });
    }
}

// Tests du système
const gestionnaire = new GestionnaireNotifications();

// Abonner un observateur
gestionnaire.abonner((evenement, notification) => {
    console.log(`🔔 Observateur: ${evenement} - Notification #${notification.id}`);
});

// Créer différents types de notifications
const email = new NotificationEmail(
    "Bienvenue!",
    "Merci de vous être inscrit à notre service",
    "user@example.com",
    "noreply@service.com",
    "normale"
);

const push = new NotificationPush(
    "Nouvelle mise à jour",
    "Version 2.0 disponible",
    "device123",
    "🚀",
    "élevée"
);

const sms = new NotificationSMS(
    "Code de vérification",
    "Votre code: 123456. Valide 5 minutes.",
    "+33123456789",
    "Orange",
    "urgente"
);

// Ajouter au gestionnaire
gestionnaire.ajouterNotification(email);
gestionnaire.ajouterNotification(push);
gestionnaire.ajouterNotification(sms);

// Traiter toutes les notifications
gestionnaire.traiterToutes();

// Afficher les statistiques
const stats = gestionnaire.obtenirStatistiques();
console.log("\n📊 STATISTIQUES FINALES:");
console.log(`Total: ${stats.total}`);
console.log(`Lues: ${stats.lues}`);
console.log(`Non lues: ${stats.nonLues}`);
console.log("Par type:", stats.parType);
console.log(`Total créées: ${Notification.getTotalNotifications()}`);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Conversion fonction constructeur vers classe
**Consigne :** Convertissez ce système de tâches de fonction constructeur vers les classes ES6.

**Code de départ :**
```javascript
// Système de tâches avec fonctions constructeurs
function Tache(titre, description, priorite) {
    this.titre = titre;
    this.description = description;
    this.priorite = priorite || "normale";
    this.terminee = false;
    this.dateCreation = new Date();
}

Tache.prototype.marquerTerminee = function() {
    this.terminee = true;
    this.dateTerminee = new Date();
};

Tache.prototype.getDuree = function() {
    if (this.terminee) {
        return this.dateTerminee - this.dateCreation;
    }
    return Date.now() - this.dateCreation;
};

// TODO: Convertir en classes ES6 avec:
// - Champs privés pour les dates
// - Getters/setters appropriés
// - Méthodes statiques pour statistiques
// - Classe héritée TacheUrgente
```

**Résultat attendu :**
- Syntaxe classe moderne
- Encapsulation avec champs privés
- Héritage avec extends/super

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Système de jeu avec classes
**Consigne :** Créez un système de personnages de jeu avec classes ES6 complètes.

**Contraintes :**
- Classe abstraite Personnage de base
- Classes héritées: Guerrier, Mage, Archer
- Champs privés pour les stats
- Méthodes statiques pour la gestion globale
- Système de combat polymorphe

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Framework MVC avec classes (optionnel)
**Consigne :** Implémentez un mini-framework MVC avec classes ES6 modernes.

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
// Conversion vers classes ES6 modernes
class Tache {
    // Champs privés
    #dateCreation;
    #dateTerminee = null;
    #terminee = false;
    
    // Compteur statique
    static #nombreTaches = 0;
    static #tachesTerminees = 0;
    
    constructor(titre, description, priorite = "normale") {
        this.titre = titre;
        this.description = description;
        this.priorite = priorite;
        this.#dateCreation = new Date();
        this.id = ++Tache.#nombreTaches;
        
        console.log(`📝 Nouvelle tâche créée: "${titre}"`);
    }
    
    // Getter pour statut (lecture seule)
    get terminee() {
        return this.#terminee;
    }
    
    get dateCreation() {
        return new Date(this.#dateCreation); // Copie pour protection
    }
    
    get dateTerminee() {
        return this.#dateTerminee ? new Date(this.#dateTerminee) : null;
    }
    
    marquerTerminee() {
        if (!this.#terminee) {
            this.#terminee = true;
            this.#dateTerminee = new Date();
            Tache.#tachesTerminees++;
            console.log(`✅ Tâche "${this.titre}" terminée`);
            return true;
        }
        return false;
    }
    
    marquerEnCours() {
        if (this.#terminee) {
            this.#terminee = false;
            this.#dateTerminee = null;
            Tache.#tachesTerminees--;
            console.log(`🔄 Tâche "${this.titre}" remise en cours`);
            return true;
        }
        return false;
    }
    
    getDuree() {
        const fin = this.#terminee ? this.#dateTerminee : new Date();
        return fin - this.#dateCreation;
    }
    
    getDureeFormatee() {
        const dureeMs = this.getDuree();
        const jours = Math.floor(dureeMs / (1000 * 60 * 60 * 24));
        const heures = Math.floor((dureeMs % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        const minutes = Math.floor((dureeMs % (1000 * 60 * 60)) / (1000 * 60));
        
        if (jours > 0) return `${jours}j ${heures}h ${minutes}m`;
        if (heures > 0) return `${heures}h ${minutes}m`;
        return `${minutes}m`;
    }
    
    // Méthodes statiques
    static getNombreTaches() {
        return Tache.#nombreTaches;
    }
    
    static getTachesTerminees() {
        return Tache.#tachesTerminees;
    }
    
    static getTachesEnCours() {
        return Tache.#nombreTaches - Tache.#tachesTerminees;
    }
    
    static getStatistiques() {
        const total = Tache.#nombreTaches;
        const terminees = Tache.#tachesTerminees;
        const enCours = total - terminees;
        const pourcentageTerminee = total > 0 ? (terminees / total * 100).toFixed(1) : 0;
        
        return {
            total,
            terminees,
            enCours,
            pourcentageTerminee: `${pourcentageTerminee}%`
        };
    }
    
    toString() {
        const statut = this.#terminee ? "✅" : "⏳";
        return `${statut} [${this.priorite.toUpperCase()}] ${this.titre} - ${this.getDureeFormatee()}`;
    }
}

// Classe héritée pour tâches urgentes
class TacheUrgente extends Tache {
    #dateEcheance;
    
    constructor(titre, description, dateEcheance) {
        super(titre, description, "urgente");
        this.#dateEcheance = new Date(dateEcheance);
        
        if (this.#dateEcheance <= new Date()) {
            console.log(`⚠️ ATTENTION: Tâche urgente "${titre}" a une échéance dépassée!`);
        }
    }
    
    get dateEcheance() {
        return new Date(this.#dateEcheance);
    }
    
    set dateEcheance(nouvelleDate) {
        this.#dateEcheance = new Date(nouvelleDate);
        console.log(`📅 Échéance mise à jour pour "${this.titre}"`);
    }
    
    getTempsRestant() {
        return this.#dateEcheance - new Date();
    }
    
    estEnRetard() {
        return new Date() > this.#dateEcheance && !this.terminee;
    }
    
    getStatutEcheance() {
        if (this.terminee) return "Terminée";
        if (this.estEnRetard()) return "En retard";
        
        const tempsRestant = this.getTempsRestant();
        const heuresRestantes = Math.floor(tempsRestant / (1000 * 60 * 60));
        
        if (heuresRestantes < 24) return "Critique";
        if (heuresRestantes < 72) return "Urgent";
        return "Normal";
    }
    
    toString() {
        const statut = this.estEnRetard() ? "🔴" : "🟡";
        const tempsRestant = this.getTempsRestant();
        const heures = Math.floor(Math.abs(tempsRestant) / (1000 * 60 * 60));
        const texteTemps = tempsRestant < 0 ? `${heures}h de retard` : `${heures}h restantes`;
        
        return `${statut} [URGENTE] ${this.titre} (${texteTemps}) - ${this.getDureeFormatee()}`;
    }
}

// Gestionnaire de tâches
class GestionnaireTaches {
    #taches = [];
    
    ajouterTache(tache) {
        if (!(tache instanceof Tache)) {
            throw new Error("Doit être une instance de Tache");
        }
        this.#taches.push(tache);
        return tache.id;
    }
    
    obtenirTache(id) {
        return this.#taches.find(t => t.id === id);
    }
    
    listerTaches(filtre = "toutes") {
        let tachesFiltrees;
        
        switch (filtre) {
            case "terminees":
                tachesFiltrees = this.#taches.filter(t => t.terminee);
                break;
            case "enCours":
                tachesFiltrees = this.#taches.filter(t => !t.terminee);
                break;
            case "urgentes":
                tachesFiltrees = this.#taches.filter(t => t.priorite === "urgente");
                break;
            default:
                tachesFiltrees = this.#taches;
        }
        
        return tachesFiltrees.sort((a, b) => {
            // Trier par priorité puis par date
            const priorites = { "urgente": 3, "haute": 2, "normale": 1, "basse": 0 };
            if (priorites[a.priorite] !== priorites[b.priorite]) {
                return priorites[b.priorite] - priorites[a.priorite];
            }
            return a.dateCreation - b.dateCreation;
        });
    }
    
    afficherTaches(filtre = "toutes") {
        const taches = this.listerTaches(filtre);
        console.log(`\n📋 Tâches (${filtre}): ${taches.length}`);
        taches.forEach((tache, index) => {
            console.log(`${index + 1}. ${tache.toString()}`);
        });
    }
    
    getStatistiquesGlobales() {
        const statsGenerales = Tache.getStatistiques();
        const tachesUrgentes = this.#taches.filter(t => t instanceof TacheUrgente);
        const urgentesEnRetard = tachesUrgentes.filter(t => t.estEnRetard()).length;
        
        return {
            ...statsGenerales,
            tachesUrgentes: tachesUrgentes.length,
            urgentesEnRetard
        };
    }
}

// Tests
console.log("=== TESTS SYSTÈME DE TÂCHES ES6 ===");

const gestionnaire = new GestionnaireTaches();

// Créer des tâches normales
const tache1 = new Tache("Apprendre JavaScript", "Finir le cours ES6", "haute");
const tache2 = new Tache("Faire les courses", "Acheter fruits et légumes", "normale");

// Créer des tâches urgentes
const demain = new Date();
demain.setDate(demain.getDate() + 1);
const tacheUrgente1 = new TacheUrgente("Rendre le rapport", "Rapport mensuel à finaliser", demain);

const hier = new Date();
hier.setDate(hier.getDate() - 1);
const tacheUrgente2 = new TacheUrgente("Bug critique", "Corriger le bug de sécurité", hier);

// Ajouter au gestionnaire
gestionnaire.ajouterTache(tache1);
gestionnaire.ajouterTache(tache2);
gestionnaire.ajouterTache(tacheUrgente1);
gestionnaire.ajouterTache(tacheUrgente2);

// Terminer quelques tâches
setTimeout(() => {
    tache1.marquerTerminee();
    tacheUrgente2.marquerTerminee();
    
    // Afficher les résultats
    gestionnaire.afficherTaches();
    gestionnaire.afficherTaches("urgentes");
    
    console.log("\n📊 Statistiques globales:");
    console.log(gestionnaire.getStatistiquesGlobales());
    
}, 100);
```

**Explication :** La conversion utilise toutes les fonctionnalités modernes ES6 : champs privés, getters/setters, méthodes statiques, et héritage avec extends/super.

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **POO traditionnelle** : Prototypes et fonctions constructeurs
- **Objets** : Manipulation d'objets et propriétés
- **Fonctions** : Méthodes et contexte d'exécution
- **This** : Contexte dans les classes

### Prochaines étapes
- **Modules ES6** : Organisation du code avec import/export
- **Programmation asynchrone** : Classes avec async/await
- **TypeScript** : Typage fort pour les classes

### Concepts complémentaires
- **Design Patterns** : Patterns avec classes modernes
- **Testing** : Tests unitaires pour les classes
- **Documentation** : JSDoc pour les classes

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer un système de gestion de bibliothèque avec classes ES6 complètes

**Fonctionnalités requises :**
- [ ] Classes Livre, Auteur, Membre avec champs privés
- [ ] Classe Bibliotheque pour gérer le tout
- [ ] Système d'emprunt/retour avec dates
- [ ] Recherche multicritères
- [ ] Statistiques et rapports automatiques
- [ ] Interface utilisateur simple

**Structure suggérée :**
```
📁 bibliotheque-es6/
├── 📄 index.html
├── 📄 style.css
├── 📄 classes/
│   ├── 📄 Livre.js
│   ├── 📄 Auteur.js
│   ├── 📄 Membre.js
│   └── 📄 Bibliotheque.js
└── 📄 main.js
```

**Temps estimé :** 100 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - Classes](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Classes)
- [MDN - Private class features](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)
- [MDN - Static methods and properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)

### Vidéos recommandées
- "ES6 Classes" - Wes Bos (25 min)
- Pourquoi cette vidéo est utile : Exemples pratiques et comparaisons avec l'ancien système

### Articles approfondis
- "Understanding ES6 Classes" - JavaScript.info
- Ce que cet article apporte en plus : Détails sur l'implémentation interne

### Outils de pratique
- [ES6 Classes Exercises](https://github.com/wesbos/es6-articles)
- [JavaScript Classes Workshop](https://github.com/kentcdodds/es6-workshop)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre une classe ES6 et une fonction constructeur ?
   - Réponse : Syntaxe plus claire, support natif des champs privés, pas de hoisting

2. **Question pratique :** Que fait le mot-clé `super` ?
   - Réponse : Appelle le constructeur ou les méthodes de la classe parent

3. **Question d'application :** Comment créer une méthode privée en ES6 ?
   - Réponse : Utiliser la syntaxe `#nomMethode()` avec le préfixe #

### Checklist de maîtrise
- [ ] Je comprends la syntaxe des classes ES6
- [ ] Je sais utiliser constructor, extends, et super
- [ ] Je maîtrise les champs et méthodes privés
- [ ] Je comprends les méthodes statiques
- [ ] Je peux convertir du code legacy vers ES6
- [ ] Je reconnais les avantages des classes modernes

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1
**Code problématique :**
```javascript
class Exemple {
    constructor() {
        this.#proprietePrivee = "test";  // ❌ Champ privé non déclaré
    }
}
```

**Solution :**
```javascript
class Exemple {
    #proprietePrivee;  // ✅ Déclaration du champ privé
    
    constructor() {
        this.#proprietePrivee = "test";
    }
}
```

**Explication :** Les champs privés doivent être déclarés au niveau de la classe.

### Erreur fréquente 2
**Code problématique :**
```javascript
class Enfant extends Parent {
    constructor(valeur) {
        this.valeur = valeur;  // ❌ Pas d'appel à super()
    }
}
```

**Solution :**
```javascript
class Enfant extends Parent {
    constructor(valeur) {
        super();  // ✅ Appel obligatoire à super()
        this.valeur = valeur;
    }
}
```

**Explication :** Dans une classe héritée, super() doit être appelé avant d'utiliser this.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- La syntaxe moderne des classes ES6
- L'importance des champs privés pour l'encapsulation
- La différence entre classes et prototypes

### Questions à approfondir
- Performance des classes vs fonctions constructeurs
- Utilisation avancée des décorateurs

### Révision programmée
- [ ] Révision dans 1 jour (rappel rapide)
- [ ] Révision dans 1 semaine (exercices)
- [ ] Révision dans 1 mois (application dans un projet)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #classes #es6 #poo #heritage #encapsulation #intermediaire

**Temps de completion :** [Temps réellement passé]

**Difficulté ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 17/07/2025*
*🔄 Dernière révision : 17/07/2025*
*📍 Position dans la roadmap : 9.5/126*
