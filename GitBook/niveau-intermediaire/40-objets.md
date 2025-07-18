# Objets JavaScript - Structures de Données Complexes

{% hint style="info" %}
**Niveau :** Intermédiaire  
**Durée estimée :** 130 minutes  
**Prérequis :** Variables, fonctions, arrays, types de données
{% endhint %}

## 🎯 Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

{% tabs %}
{% tab title="Fondamentaux" %}
- Comprendre la structure clé-valeur des objets
- Créer des objets avec propriétés et méthodes
- Accéder aux propriétés avec différentes notations
- Modifier et ajouter des propriétés dynamiquement
{% endtab %}

{% tab title="Méthodes" %}
- Définir des méthodes dans les objets
- Utiliser le mot-clé `this` correctement
- Créer des objets interactifs et fonctionnels
- Organiser le code avec des méthodes logiques
{% endtab %}

{% tab title="Patterns" %}
- Modéliser des entités du monde réel
- Structurer des données complexes
- Implémenter des comportements avec les méthodes
- Créer des systèmes orientés objet simples
{% endtab %}
{% endtabs %}

---

## 📚 Concepts fondamentaux

### 1. Qu'est-ce qu'un objet ?

{% hint style="success" %}
**Un objet est une collection de propriétés**, où chaque propriété est définie par une paire **clé-valeur**.

Imaginez décrire votre meilleur ami avec plusieurs variables :
```javascript
let nomAmi = "Alice";
let ageAmi = 25;
let villeAmi = "Paris";
```

Avec un objet, tout est regroupé logiquement :
```javascript
let ami = {
    nom: "Alice",
    age: 25,
    ville: "Paris"
};
```
{% endhint %}

#### Syntaxe de base

```javascript
// Création d'un objet literal
let personne = {
    nom: "Marie",           // propriété string
    age: 28,                // propriété number
    estEtudiant: true,      // propriété boolean
    hobbies: ["lecture", "sport"], // propriété array
    adresse: {              // propriété objet imbriqué
        rue: "123 rue de la Paix",
        ville: "Lyon",
        codePostal: "69000"
    }
};

console.log(personne);
```

#### Accès aux propriétés

{% tabs %}
{% tab title="Notation point" %}
```javascript
// Lecture avec la notation point (la plus courante)
console.log(personne.nom);        // "Marie"
console.log(personne.age);        // 28
console.log(personne.adresse.ville); // "Lyon"

// Modification
personne.age = 29;
personne.adresse.ville = "Paris";

// Ajout de nouvelles propriétés
personne.email = "marie@example.com";
personne.telephone = "01 23 45 67 89";
```
{% endtab %}

{% tab title="Notation crochet" %}
```javascript
// Lecture avec la notation crochet
console.log(personne["nom"]);     // "Marie"
console.log(personne["age"]);     // 28

// Utile pour les propriétés avec espaces ou caractères spéciaux
let objet = {
    "propriété avec espaces": "valeur",
    "123-numérique": "autre valeur"
};

console.log(objet["propriété avec espaces"]);

// Accès dynamique avec variables
let propriete = "nom";
console.log(personne[propriete]); // "Marie"

// Avec des propriétés calculées
let prefix = "adresse";
console.log(personne[prefix]); // objet adresse
```
{% endtab %}
{% endtabs %}

### 2. Méthodes et `this`

{% hint style="warning" %}
**Les méthodes sont des propriétés qui contiennent des fonctions**. Le mot-clé `this` fait référence à l'objet qui contient la méthode.
{% endhint %}

```javascript
let calculatrice = {
    marque: "Casio",
    modele: "FX-82",
    
    // Méthodes pour les opérations
    additionner: function(a, b) {
        console.log(`Calcul avec ${this.marque} ${this.modele}`);
        return a + b;
    },
    
    // Syntaxe raccourcie ES6
    multiplier(a, b) {
        return a * b;
    },
    
    // Arrow function (attention : this ne fonctionne pas comme attendu)
    soustraire: (a, b) => {
        // this ne fait PAS référence à calculatrice ici !
        return a - b;
    },
    
    // Méthode complexe utilisant les propriétés de l'objet
    afficherInfo() {
        return `Calculatrice ${this.marque} ${this.modele}`;
    }
};

// Utilisation
console.log(calculatrice.additionner(5, 3)); // 8
console.log(calculatrice.multiplier(4, 7));  // 28
console.log(calculatrice.afficherInfo());    // "Calculatrice Casio FX-82"
```

---

## 🧪 Ateliers pratiques

### Atelier 1 : Profil utilisateur interactif

{% hint style="info" %}
**Objectif :** Créer un objet représentant un profil utilisateur complet avec des méthodes utiles.
{% endhint %}

```javascript
function creerProfil(nom, email, age) {
    return {
        // Propriétés de base
        nom: nom,
        email: email,
        age: age,
        dateInscription: new Date(),
        estActif: true,
        preferences: {
            theme: "clair",
            notifications: true,
            langue: "fr"
        },
        
        // Historique des activités
        activites: [],
        
        // Méthodes du profil
        sePresenter() {
            return `Bonjour, je suis ${this.nom}, j'ai ${this.age} ans.`;
        },
        
        changerEmail(nouvelEmail) {
            if (this.validerEmail(nouvelEmail)) {
                const ancienEmail = this.email;
                this.email = nouvelEmail;
                this.ajouterActivite(`Email changé de ${ancienEmail} vers ${nouvelEmail}`);
                return true;
            }
            return false;
        },
        
        validerEmail(email) {
            const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return regex.test(email);
        },
        
        anniversaire() {
            this.age++;
            this.ajouterActivite(`Anniversaire ! Maintenant ${this.age} ans`);
            console.log(`🎉 Joyeux anniversaire ${this.nom} ! Vous avez maintenant ${this.age} ans.`);
        },
        
        ajouterActivite(description) {
            this.activites.push({
                date: new Date(),
                action: description
            });
        },
        
        obtenirStatistiques() {
            const joursInscrit = Math.floor((new Date() - this.dateInscription) / (1000 * 60 * 60 * 24));
            
            return {
                nom: this.nom,
                age: this.age,
                joursInscrit: joursInscrit,
                nombreActivites: this.activites.length,
                estActif: this.estActif,
                derniereActivite: this.activites.length > 0 ? 
                    this.activites[this.activites.length - 1] : null
            };
        },
        
        changerPreference(cle, valeur) {
            if (this.preferences.hasOwnProperty(cle)) {
                this.preferences[cle] = valeur;
                this.ajouterActivite(`Préférence ${cle} changée vers ${valeur}`);
                console.log(`✅ Préférence ${cle} mise à jour : ${valeur}`);
            } else {
                console.log(`❌ Préférence ${cle} non reconnue`);
            }
        },
        
        exporterDonnees() {
            return {
                nom: this.nom,
                email: this.email,
                age: this.age,
                preferences: { ...this.preferences }, // Copie des préférences
                statistiques: this.obtenirStatistiques()
            };
        }
    };
}

// Tests du profil utilisateur
console.log("👤 CRÉATION DE PROFIL");
const alice = creerProfil("Alice Dupont", "alice@example.com", 28);

console.log(alice.sePresenter());

// Tests des méthodes
alice.changerEmail("alice.dupont@newmail.com");
alice.anniversaire();
alice.changerPreference("theme", "sombre");
alice.changerPreference("notifications", false);

// Affichage des statistiques
console.log("\n📊 Statistiques du profil :");
console.log(alice.obtenirStatistiques());

// Export des données
console.log("\n💾 Données exportées :");
console.log(alice.exporterDonnees());
```

### Atelier 2 : Système de gestion de bibliothèque

```javascript
const bibliotheque = {
    nom: "Bibliothèque Centrale",
    adresse: "12 rue des Livres, Paris",
    livres: [],
    membres: [],
    emprunts: [],
    
    // Gestion des livres
    ajouterLivre(titre, auteur, isbn, genre) {
        const nouveauLivre = {
            id: this.livres.length + 1,
            titre,
            auteur,
            isbn,
            genre,
            disponible: true,
            dateAjout: new Date()
        };
        
        this.livres.push(nouveauLivre);
        console.log(`📚 Livre ajouté : "${titre}" par ${auteur}`);
        return nouveauLivre.id;
    },
    
    rechercherLivres(terme) {
        const termeLower = terme.toLowerCase();
        return this.livres.filter(livre => 
            livre.titre.toLowerCase().includes(termeLower) ||
            livre.auteur.toLowerCase().includes(termeLower) ||
            livre.genre.toLowerCase().includes(termeLower)
        );
    },
    
    // Gestion des membres
    ajouterMembre(nom, email) {
        const nouveauMembre = {
            id: this.membres.length + 1,
            nom,
            email,
            dateInscription: new Date(),
            empruntsActuels: [],
            historiqueEmprunts: []
        };
        
        this.membres.push(nouveauMembre);
        console.log(`👤 Membre ajouté : ${nom}`);
        return nouveauMembre.id;
    },
    
    trouverMembre(id) {
        return this.membres.find(membre => membre.id === id);
    },
    
    // Gestion des emprunts
    emprunterLivre(idMembre, idLivre) {
        const membre = this.trouverMembre(idMembre);
        const livre = this.livres.find(l => l.id === idLivre);
        
        if (!membre) {
            console.log("❌ Membre non trouvé");
            return false;
        }
        
        if (!livre) {
            console.log("❌ Livre non trouvé");
            return false;
        }
        
        if (!livre.disponible) {
            console.log("❌ Livre déjà emprunté");
            return false;
        }
        
        if (membre.empruntsActuels.length >= 3) {
            console.log("❌ Limite d'emprunt atteinte (3 livres max)");
            return false;
        }
        
        // Effectuer l'emprunt
        const emprunt = {
            id: this.emprunts.length + 1,
            idMembre,
            idLivre,
            dateEmprunt: new Date(),
            dateRetourPrevue: new Date(Date.now() + 14 * 24 * 60 * 60 * 1000), // 14 jours
            dateRetourEffectif: null
        };
        
        this.emprunts.push(emprunt);
        livre.disponible = false;
        membre.empruntsActuels.push(emprunt.id);
        
        console.log(`✅ ${membre.nom} a emprunté "${livre.titre}"`);
        console.log(`📅 À retourner avant le ${emprunt.dateRetourPrevue.toLocaleDateString()}`);
        
        return emprunt.id;
    },
    
    retournerLivre(idEmprunt) {
        const emprunt = this.emprunts.find(e => e.id === idEmprunt);
        
        if (!emprunt || emprunt.dateRetourEffectif) {
            console.log("❌ Emprunt non trouvé ou livre déjà retourné");
            return false;
        }
        
        const membre = this.trouverMembre(emprunt.idMembre);
        const livre = this.livres.find(l => l.id === emprunt.idLivre);
        
        // Effectuer le retour
        emprunt.dateRetourEffectif = new Date();
        livre.disponible = true;
        
        // Retirer de la liste des emprunts actuels
        const index = membre.empruntsActuels.indexOf(idEmprunt);
        if (index > -1) {
            membre.empruntsActuels.splice(index, 1);
        }
        
        // Ajouter à l'historique
        membre.historiqueEmprunts.push(idEmprunt);
        
        // Vérifier si en retard
        const enRetard = emprunt.dateRetourEffectif > emprunt.dateRetourPrevue;
        const joursRetard = enRetard ? 
            Math.ceil((emprunt.dateRetourEffectif - emprunt.dateRetourPrevue) / (24 * 60 * 60 * 1000)) : 0;
        
        console.log(`✅ Livre "${livre.titre}" retourné par ${membre.nom}`);
        if (enRetard) {
            console.log(`⚠️ Retour en retard de ${joursRetard} jour(s)`);
        }
        
        return true;
    },
    
    // Statistiques et rapports
    obtenirStatistiques() {
        const totalLivres = this.livres.length;
        const livresDisponibles = this.livres.filter(l => l.disponible).length;
        const totalMembres = this.membres.length;
        const empruntsActifs = this.emprunts.filter(e => !e.dateRetourEffectif).length;
        
        return {
            totalLivres,
            livresDisponibles,
            livresEmpruntes: totalLivres - livresDisponibles,
            totalMembres,
            empruntsActifs,
            tauxOccupation: ((totalLivres - livresDisponibles) / totalLivres * 100).toFixed(1) + '%'
        };
    },
    
    afficherCatalogue() {
        console.log(`\n📖 CATALOGUE - ${this.nom}`);
        console.log("=".repeat(50));
        
        if (this.livres.length === 0) {
            console.log("Aucun livre dans la bibliothèque");
            return;
        }
        
        this.livres.forEach((livre, index) => {
            const statut = livre.disponible ? "🟢 DISPONIBLE" : "🔴 EMPRUNTÉ";
            console.log(`${index + 1}. "${livre.titre}" - ${livre.auteur}`);
            console.log(`   Genre: ${livre.genre} | ${statut}`);
        });
        
        const stats = this.obtenirStatistiques();
        console.log(`\n📊 ${stats.totalLivres} livre(s), ${stats.livresDisponibles} disponible(s)`);
    }
};

// Tests du système de bibliothèque
console.log("\n📚 SYSTÈME DE BIBLIOTHÈQUE");

// Ajouter des livres
bibliotheque.ajouterLivre("Le Petit Prince", "Antoine de Saint-Exupéry", "978-2-07-040845-4", "Fiction");
bibliotheque.ajouterLivre("1984", "George Orwell", "978-0-452-28423-4", "Science-Fiction");
bibliotheque.ajouterLivre("Harry Potter", "J.K. Rowling", "978-0-439-70818-8", "Fantastique");

// Ajouter des membres
const idAlice = bibliotheque.ajouterMembre("Alice Martin", "alice@email.com");
const idBob = bibliotheque.ajouterMembre("Bob Dupont", "bob@email.com");

// Afficher le catalogue
bibliotheque.afficherCatalogue();

// Tests d'emprunt
console.log("\n📋 EMPRUNTS");
bibliotheque.emprunterLivre(idAlice, 1); // Alice emprunte Le Petit Prince
bibliotheque.emprunterLivre(idBob, 2);   // Bob emprunte 1984

// Statistiques
console.log("\n📊 STATISTIQUES");
console.log(bibliotheque.obtenirStatistiques());
```

---

## 🎯 Quiz interactif

{% tabs %}
{% tab title="Question 1" %}
**Quelle est la différence entre ces deux notations ?**

```javascript
const obj = { nom: "Alice" };
console.log(obj.nom);      // A
console.log(obj["nom"]);   // B
```

{% hint style="info" %}
**Réponse :** Les deux produisent le même résultat ("Alice").

**Différences :**
- **Notation point** : Plus lisible, propriétés fixes
- **Notation crochet** : Propriétés dynamiques, avec espaces ou caractères spéciaux

```javascript
const prop = "nom";
console.log(obj[prop]); // Utilisation dynamique
```
{% endhint %}
{% endtab %}

{% tab title="Question 2" %}
**Que va afficher ce code ?**

```javascript
const personne = {
    nom: "Alice",
    saluer: function() {
        return "Bonjour, je suis " + this.nom;
    }
};

const salutation = personne.saluer;
console.log(salutation());
```

{% hint style="info" %}
**Réponse :** "Bonjour, je suis undefined"

**Explication :** Quand on assigne la méthode à une variable, elle perd le contexte de `this`. Pour conserver le contexte :

```javascript
const salutation = personne.saluer.bind(personne);
// ou
console.log(personne.saluer()); // Appel direct
```
{% endhint %}
{% endtab %}

{% tab title="Question 3" %}
**Comment ajouter une nouvelle propriété dynamiquement ?**

```javascript
const obj = { nom: "Alice" };
// Ajouter 'age: 25'
```

{% hint style="info" %}
**Réponse :** Plusieurs façons :

```javascript
// Notation point
obj.age = 25;

// Notation crochet
obj["age"] = 25;

// Avec variable
const propriete = "age";
obj[propriete] = 25;

// Object.assign
Object.assign(obj, { age: 25 });
```
{% endhint %}
{% endtab %}
{% endtabs %}

---

## 💡 Exercices pratiques

### Exercice 1 : Compte bancaire

{% hint style="warning" %}
**Difficulté :** ⭐⭐⭐☆☆
{% endhint %}

**Consigne :** Créez un objet représentant un compte bancaire avec toutes les opérations.

```javascript
function creerCompteBancaire(numeroCompte, proprietaire, soldeInitial = 0) {
    return {
        // Propriétés
        numeroCompte: numeroCompte,
        proprietaire: proprietaire,
        solde: soldeInitial,
        historique: [],
        
        // Méthodes à implémenter :
        
        deposer(montant) {
            // Ajouter de l'argent au compte
            // Vérifier que le montant est positif
            // Enregistrer dans l'historique
        },
        
        retirer(montant) {
            // Retirer de l'argent
            // Vérifier le solde suffisant
            // Enregistrer dans l'historique
        },
        
        consulterSolde() {
            // Afficher le solde actuel
        },
        
        obtenirHistorique() {
            // Retourner l'historique des opérations
        },
        
        transferer(montant, compteDestination) {
            // Transférer vers un autre compte
        }
    };
}

// Tests attendus
const compte1 = creerCompteBancaire("FR123456", "Alice Dupont", 1000);
const compte2 = creerCompteBancaire("FR789012", "Bob Martin", 500);

compte1.deposer(200);           // Solde: 1200€
compte1.retirer(150);           // Solde: 1050€
compte1.transferer(100, compte2); // Alice: 950€, Bob: 600€
```

{% hint style="success" %}
**Solution - Cliquez pour révéler**

```javascript
function creerCompteBancaire(numeroCompte, proprietaire, soldeInitial = 0) {
    return {
        numeroCompte,
        proprietaire,
        solde: soldeInitial,
        historique: [],
        
        ajouterTransaction(type, montant, description = "") {
            this.historique.push({
                date: new Date(),
                type,
                montant,
                soldeApres: this.solde,
                description
            });
        },
        
        deposer(montant) {
            if (montant <= 0) {
                console.log("❌ Le montant doit être positif");
                return false;
            }
            
            this.solde += montant;
            this.ajouterTransaction("DEPOT", montant);
            console.log(`✅ Dépôt de ${montant}€. Nouveau solde: ${this.solde}€`);
            return true;
        },
        
        retirer(montant) {
            if (montant <= 0) {
                console.log("❌ Le montant doit être positif");
                return false;
            }
            
            if (montant > this.solde) {
                console.log("❌ Solde insuffisant");
                return false;
            }
            
            this.solde -= montant;
            this.ajouterTransaction("RETRAIT", montant);
            console.log(`✅ Retrait de ${montant}€. Nouveau solde: ${this.solde}€`);
            return true;
        },
        
        consulterSolde() {
            console.log(`💰 Solde actuel: ${this.solde}€`);
            return this.solde;
        },
        
        transferer(montant, compteDestination) {
            if (this.retirer(montant)) {
                compteDestination.deposer(montant);
                
                this.ajouterTransaction("TRANSFERT_SORTANT", montant, 
                    `Vers ${compteDestination.proprietaire}`);
                compteDestination.ajouterTransaction("TRANSFERT_ENTRANT", montant, 
                    `De ${this.proprietaire}`);
                
                console.log(`💸 Transfert de ${montant}€ vers ${compteDestination.proprietaire}`);
                return true;
            }
            return false;
        },
        
        obtenirHistorique() {
            console.log(`\n📋 Historique - ${this.proprietaire}`);
            this.historique.forEach((transaction, index) => {
                console.log(`${index + 1}. ${transaction.date.toLocaleDateString()} - ${transaction.type}: ${transaction.montant}€ (Solde: ${transaction.soldeApres}€)`);
            });
            return this.historique;
        }
    };
}
```
{% endhint %}

### Exercice 2 : Jeu de cartes

**Consigne :** Créez un système de jeu de cartes avec deck, mélange et distribution.

```javascript
// Structure suggérée
const jeuDeCartes = {
    cartes: [], // Array de toutes les cartes
    
    initialiser() {
        // Créer un deck de 52 cartes
    },
    
    melanger() {
        // Mélanger les cartes aléatoirement
    },
    
    distribuer(nombreCartes) {
        // Donner un nombre de cartes
    },
    
    afficherCartes() {
        // Afficher toutes les cartes restantes
    }
};
```

---

## 🔗 Liens avec d'autres concepts

### Concepts précédents
- [Variables et types](../niveau-debutant/15-variables-declarations.md) - Foundation des propriétés
- [Fonctions](05-fonctions.md) - Base des méthodes
- [Arrays](25-arrays-methodes-avancees.md) - Structures de données

### Concepts suivants
- [Classes](45-classes-poo.md) - Évolution moderne des objets
- [Prototypes](50-prototypes-heritage.md) - Mécanisme d'héritage
- [Destructuring](30-destructuring-spread.md) - Extraction de propriétés

---

## 🎓 Projet pratique : Gestionnaire de tâches

### Créez un système complet de gestion de tâches

{% hint style="info" %}
**Objectif :** Développer une application de gestion de tâches avec toutes les fonctionnalités essentielles
{% endhint %}

**Spécifications :**
- Créer, modifier, supprimer des tâches
- Organiser par catégories et priorités
- Système de dates d'échéance et rappels
- Statistiques et rapports de productivité
- Export/import des données

**Structure suggérée :**
```javascript
const gestionnaireDesTaches = {
    taches: [],
    categories: [],
    utilisateur: {},
    
    // Méthodes à implémenter
    ajouterTache(titre, description, priorite, echeance) {},
    modifierTache(id, modifications) {},
    supprimerTache(id) {},
    marquerTerminee(id) {},
    filtrerTaches(criteres) {},
    obtenirStatistiques() {},
    exporterDonnees() {}
};
```

**Temps estimé :** 2-3 heures

---

## 📖 Résumé et bonnes pratiques

{% hint style="success" %}
**Les objets JavaScript permettent de :**

- **Structurer** des données complexes de manière logique
- **Grouper** propriétés et comportements ensemble
- **Modéliser** des entités du monde réel
- **Organiser** le code de façon claire et maintenable

**Bonnes pratiques :**
- Utilisez des noms de propriétés descriptifs
- Groupez les données et méthodes logiquement liées
- Préférez les méthodes aux fonctions externes quand possible
- Attention au contexte de `this` dans les méthodes
- Documentez les propriétés importantes de vos objets
{% endhint %}

---

**Navigation :**
- ← Précédent : [JSON et Manipulation](35-json-manipulation.md)
- → Suivant : [Classes et POO](45-classes-poo.md)

---

*💡 **Astuce :** Les objets sont omniprésents en JavaScript ! Maîtrisez-les bien car ils sont la base de concepts plus avancés comme les classes, le JSON, et les APIs.*
