# Arrays (Tableaux) en JavaScript

## INFORMATIONS GENERALES

### Metadonnees du cours
- **Titre :** Arrays (Tableaux) - Stockage et manipulation de listes
- **Niveau :** INTERMEDIAIRE (apres fonctions)
- **Duree estimee :** 120 minutes
- **Prerequis :** Variables, types, fonctions, boucles
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## OBJECTIFS PEDAGOGIQUES

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Ce qu'est un array et pourquoi c'est utile
- [ ] **Creer** : Des arrays avec differents types de donnees
- [ ] **Manipuler** : Ajouter, supprimer, modifier des elements
- [ ] **Parcourir** : Utiliser des boucles pour traiter tous les elements

### Mots-cles a retenir
- **Array** : Liste ordonnee d'elements (comme une liste de courses)
- **Index** : Position d'un element dans l'array (commence a 0)
- **Length** : Propriete qui donne le nombre d'elements
- **Push/Pop** : Ajouter/enlever a la fin
- **Shift/Unshift** : Enlever/ajouter au debut

---

## PARTIE THEORIQUE

### Pourquoi ce concept est-il important ?
Imagine que tu veux stocker les prenoms de tes amis. Tu pourrais creer une variable pour chaque ami :
```javascript
let ami1 = "Alice";
let ami2 = "Bob";
let ami3 = "Charlie";
```

Mais si tu as 50 amis ? Tu vas creer 50 variables ? C'est la que les arrays deviennent magiques ! Un seul array peut stocker tous tes amis :
```javascript
let mesAmis = ["Alice", "Bob", "Charlie"];
```

**Les arrays permettent de :**
- **Stocker** plusieurs valeurs dans une seule variable
- **Organiser** des donnees liees (notes, noms, ages)
- **Parcourir** facilement toute la liste
- **Manipuler** les donnees (ajouter, supprimer, trier)

### Definition et concepts cles

**Un array est une liste ordonnee d'elements :**

1. **Syntax** : Entre crochets `[element1, element2, element3]`
2. **Index** : Position de chaque element (commence a 0 !)
3. **Length** : Nombre total d'elements
4. **Types mixtes** : Un array peut contenir differents types

**Methodes principales :**
- **push()** : Ajoute a la fin
- **pop()** : Enleve le dernier
- **unshift()** : Ajoute au debut
- **shift()** : Enleve le premier
- **splice()** : Ajoute/enleve a une position precise

### Syntaxe de base
```javascript
// Creation d'arrays
let fruits = ["pomme", "banane", "orange"];
let nombres = [1, 2, 3, 4, 5];
let melange = ["Alice", 14, true, 3.14];

// Acces aux elements (index commence a 0)
console.log(fruits[0]);    // "pomme" (premier element)
console.log(fruits[1]);    // "banane" (deuxieme element)
console.log(fruits[2]);    // "orange" (troisieme element)

// Proprietes et methodes
console.log(fruits.length);     // 3 (nombre d'elements)
fruits.push("kiwi");           // Ajoute "kiwi" a la fin
let dernierFruit = fruits.pop(); // Enleve et retourne le dernier
```

### Points d'attention
- **Index 0** : Le premier element est a l'index 0, pas 1 !
- **Length** : Toujours 1 de plus que le dernier index
- **Types mixtes** : Possible mais pas toujours recommande
- **Modification** : Les arrays sont mutables (modifiables)

---

## PARTIE PRATIQUE

### Exemple simple (starter)
```javascript
// Ma liste de courses
let listeCourses = ["pain", "lait", "oeufs", "fromage"];

console.log("=== MA LISTE DE COURSES ===");
console.log("Nombre d'articles: " + listeCourses.length);

// Afficher chaque article avec son numero
for (let i = 0; i < listeCourses.length; i++) {
    let numero = i + 1;  // +1 car on compte a partir de 1 pour l'humain
    console.log(numero + ". " + listeCourses[i]);
}

// Ajouter un article
listeCourses.push("chocolat");
console.log("Apres ajout de chocolat: " + listeCourses);

// Enlever le premier article (deja achete)
let premierArticle = listeCourses.shift();
console.log("J'ai achete: " + premierArticle);
console.log("Liste mise a jour: " + listeCourses);
```

**Resultat attendu :**
```
=== MA LISTE DE COURSES ===
Nombre d'articles: 4
1. pain
2. lait
3. oeufs
4. fromage
Apres ajout de chocolat: pain,lait,oeufs,fromage,chocolat
J'ai achete: pain
Liste mise a jour: lait,oeufs,fromage,chocolat
```

### Exemple intermediaire
```javascript
// Gestion des notes d'un eleve
let notesEleve = [];  // Array vide au depart

// Fonction pour ajouter une note
function ajouterNote(note) {
    if (note >= 0 && note <= 20) {
        notesEleve.push(note);
        console.log("Note " + note + " ajoutee !");
    } else {
        console.log("Erreur: La note doit etre entre 0 et 20");
    }
}

// Fonction pour calculer la moyenne
function calculerMoyenne() {
    if (notesEleve.length === 0) {
        return 0;
    }
    
    let total = 0;
    for (let i = 0; i < notesEleve.length; i++) {
        total += notesEleve[i];
    }
    
    return total / notesEleve.length;
}

// Fonction pour afficher toutes les notes
function afficherNotes() {
    console.log("=== CARNET DE NOTES ===");
    
    if (notesEleve.length === 0) {
        console.log("Aucune note enregistree");
        return;
    }
    
    for (let i = 0; i < notesEleve.length; i++) {
        console.log("Note " + (i + 1) + ": " + notesEleve[i] + "/20");
    }
    
    let moyenne = calculerMoyenne();
    console.log("Moyenne generale: " + moyenne.toFixed(2) + "/20");
    
    // Determination de la mention
    if (moyenne >= 16) {
        console.log("Mention: Tres Bien !");
    } else if (moyenne >= 14) {
        console.log("Mention: Bien");
    } else if (moyenne >= 12) {
        console.log("Mention: Assez Bien");
    } else if (moyenne >= 10) {
        console.log("Mention: Passable");
    } else {
        console.log("Mention: Insuffisant");
    }
}

// Test du systeme
ajouterNote(15);
ajouterNote(12);
ajouterNote(18);
ajouterNote(14);

afficherNotes();

// Modifier une note (remplacer la note a l'index 1)
notesEleve[1] = 16;
console.log("\nApres modification de la 2e note:");
afficherNotes();
```

### Exemple avance (optionnel)
```javascript
// Gestionnaire de playlist musicale
let playlist = [
    { titre: "Bohemian Rhapsody", artiste: "Queen", duree: 355 },
    { titre: "Imagine", artiste: "John Lennon", duree: 183 },
    { titre: "Hotel California", artiste: "Eagles", duree: 391 }
];

// Fonction pour ajouter une chanson
function ajouterChanson(titre, artiste, dureeEnSecondes) {
    let nouvelleChanson = {
        titre: titre,
        artiste: artiste,
        duree: dureeEnSecondes
    };
    
    playlist.push(nouvelleChanson);
    console.log("Chanson ajoutee: " + titre + " par " + artiste);
}

// Fonction pour formater la duree en minutes:secondes
function formaterDuree(secondes) {
    let minutes = Math.floor(secondes / 60);
    let secondesRestantes = secondes % 60;
    
    // Ajouter un 0 devant si necessaire
    if (secondesRestantes < 10) {
        secondesRestantes = "0" + secondesRestantes;
    }
    
    return minutes + ":" + secondesRestantes;
}

// Fonction pour afficher la playlist
function afficherPlaylist() {
    console.log("=== MA PLAYLIST ===");
    
    let dureeTotal = 0;
    
    for (let i = 0; i < playlist.length; i++) {
        let chanson = playlist[i];
        let dureeFormatee = formaterDuree(chanson.duree);
        
        console.log((i + 1) + ". " + chanson.titre + " - " + chanson.artiste + " (" + dureeFormatee + ")");
        
        dureeTotal += chanson.duree;
    }
    
    console.log("\nNombre de chansons: " + playlist.length);
    console.log("Duree totale: " + formaterDuree(dureeTotal));
}

// Fonction pour rechercher par artiste
function rechercherParArtiste(nomArtiste) {
    let chansonsTouvees = [];
    
    for (let i = 0; i < playlist.length; i++) {
        let chanson = playlist[i];
        
        // Recherche insensible a la casse
        if (chanson.artiste.toLowerCase().includes(nomArtiste.toLowerCase())) {
            chansonsTouvees.push(chanson);
        }
    }
    
    return chansonsTouvees;
}

// Fonction pour supprimer une chanson par index
function supprimerChanson(index) {
    if (index >= 0 && index < playlist.length) {
        let chansonSupprimee = playlist.splice(index, 1)[0];  // splice() retourne un array
        console.log("Chanson supprimee: " + chansonSupprimee.titre);
    } else {
        console.log("Index invalide !");
    }
}

// Tests du gestionnaire
afficherPlaylist();

ajouterChanson("Stairway to Heaven", "Led Zeppelin", 482);
console.log("");

afficherPlaylist();

console.log("\n=== RECHERCHE ===");
let chansonsQueen = rechercherParArtiste("Queen");
console.log("Chansons de Queen trouvees: " + chansonsQueen.length);
for (let i = 0; i < chansonsQueen.length; i++) {
    console.log("- " + chansonsQueen[i].titre);
}

console.log("\n=== SUPPRESSION ===");
supprimerChanson(0);  // Supprimer la premiere chanson
afficherPlaylist();
```

---

## EXERCICES PRATIQUES

### Exercice 1 : Gestionnaire de taches
**Consigne :** Cree un array pour gerer ta liste de taches quotidiennes

**Fonctionnalites a implementer :**
- Ajouter une tache
- Marquer une tache comme terminee (la supprimer)
- Afficher toutes les taches avec numeros
- Compter les taches restantes

**Niveau de difficulte :** 3 etoiles sur 5

### Exercice 2 : Calculateur de statistiques
**Consigne :** Cree un array de nombres et calcule differentes statistiques

**Statistiques a calculer :**
- Moyenne, minimum, maximum
- Nombre d'elements pairs/impairs
- Somme totale
- Trier du plus petit au plus grand

**Niveau de difficulte :** 4 etoiles sur 5

### Exercice 3 : Carnet d'adresses simple
**Consigne :** Cree un array d'objets pour stocker des contacts

**Informations par contact :**
- Nom, telephone, email
- Fonctions : ajouter, rechercher, supprimer contact
- Affichage trie par nom

**Niveau de difficulte :** 5 etoiles sur 5

---

## SOLUTIONS DES EXERCICES

### Solution Exercice 1
```javascript
// Gestionnaire de taches
let listeTaches = [];

// Fonction pour ajouter une tache
function ajouterTache(description) {
    listeTaches.push(description);
    console.log("Tache ajoutee: " + description);
    afficherTaches();
}

// Fonction pour terminer une tache (la supprimer)
function terminerTache(index) {
    if (index >= 0 && index < listeTaches.length) {
        let tacheTerminee = listeTaches.splice(index, 1)[0];
        console.log("Tache terminee: " + tacheTerminee);
        afficherTaches();
    } else {
        console.log("Numero de tache invalide !");
    }
}

// Fonction pour afficher toutes les taches
function afficherTaches() {
    console.log("\n=== MES TACHES ===");
    
    if (listeTaches.length === 0) {
        console.log("Aucune tache ! Tu es libre !");
        return;
    }
    
    for (let i = 0; i < listeTaches.length; i++) {
        console.log((i + 1) + ". " + listeTaches[i]);
    }
    
    console.log("Total: " + listeTaches.length + " tache(s) restante(s)");
    console.log("");
}

// Fonction pour compter les taches par type (bonus)
function statistiquesTaches() {
    let tachesEcole = 0;
    let tachesMaison = 0;
    let autresTaches = 0;
    
    for (let i = 0; i < listeTaches.length; i++) {
        let tache = listeTaches[i].toLowerCase();
        
        if (tache.includes("devoir") || tache.includes("cours") || tache.includes("etude")) {
            tachesEcole++;
        } else if (tache.includes("menage") || tache.includes("ranger") || tache.includes("cuisine")) {
            tachesMaison++;
        } else {
            autresTaches++;
        }
    }
    
    console.log("=== STATISTIQUES ===");
    console.log("Taches ecole: " + tachesEcole);
    console.log("Taches maison: " + tachesMaison);
    console.log("Autres taches: " + autresTaches);
}

// Tests du gestionnaire
ajouterTache("Faire les devoirs de maths");
ajouterTache("Ranger ma chambre");
ajouterTache("Appeler grand-maman");
ajouterTache("Reviser le cours d'anglais");

statistiquesTaches();

// Terminer la premiere tache
terminerTache(0);

// Ajouter une nouvelle tache
ajouterTache("Preparer le sac pour demain");
```

**Explication :** L'array `listeTaches` stocke toutes les taches. `splice()` permet de supprimer un element a un index precis.

### Solution Exercice 2
```javascript
// Calculateur de statistiques pour un array de nombres
let nombres = [12, 7, 23, 4, 18, 9, 31, 15, 6, 22];

console.log("Nombres a analyser: " + nombres);

// Fonction pour calculer la moyenne
function calculerMoyenne(arrayNombres) {
    let total = 0;
    for (let i = 0; i < arrayNombres.length; i++) {
        total += arrayNombres[i];
    }
    return total / arrayNombres.length;
}

// Fonction pour trouver le minimum
function trouverMinimum(arrayNombres) {
    let minimum = arrayNombres[0];  // Commencer par le premier
    
    for (let i = 1; i < arrayNombres.length; i++) {
        if (arrayNombres[i] < minimum) {
            minimum = arrayNombres[i];
        }
    }
    
    return minimum;
}

// Fonction pour trouver le maximum
function trouverMaximum(arrayNombres) {
    let maximum = arrayNombres[0];  // Commencer par le premier
    
    for (let i = 1; i < arrayNombres.length; i++) {
        if (arrayNombres[i] > maximum) {
            maximum = arrayNombres[i];
        }
    }
    
    return maximum;
}

// Fonction pour compter pairs et impairs
function compterPairsImpairs(arrayNombres) {
    let pairs = 0;
    let impairs = 0;
    
    for (let i = 0; i < arrayNombres.length; i++) {
        if (arrayNombres[i] % 2 === 0) {
            pairs++;
        } else {
            impairs++;
        }
    }
    
    return { pairs: pairs, impairs: impairs };
}

// Fonction pour trier (algorithme simple - bubble sort)
function trierNombres(arrayNombres) {
    let copie = [...arrayNombres];  // Copie pour ne pas modifier l'original
    
    // Bubble sort simple
    for (let i = 0; i < copie.length - 1; i++) {
        for (let j = 0; j < copie.length - i - 1; j++) {
            if (copie[j] > copie[j + 1]) {
                // Echanger les elements
                let temp = copie[j];
                copie[j] = copie[j + 1];
                copie[j + 1] = temp;
            }
        }
    }
    
    return copie;
}

// Calculs et affichage des statistiques
console.log("\n=== STATISTIQUES ===");

let moyenne = calculerMoyenne(nombres);
console.log("Moyenne: " + moyenne.toFixed(2));

let minimum = trouverMinimum(nombres);
console.log("Minimum: " + minimum);

let maximum = trouverMaximum(nombres);
console.log("Maximum: " + maximum);

let somme = 0;
for (let i = 0; i < nombres.length; i++) {
    somme += nombres[i];
}
console.log("Somme totale: " + somme);

let pairsImpairs = compterPairsImpairs(nombres);
console.log("Nombres pairs: " + pairsImpairs.pairs);
console.log("Nombres impairs: " + pairsImpairs.impairs);

let nombresTries = trierNombres(nombres);
console.log("Trie croissant: " + nombresTries);

// Verification avec methodes JavaScript natives (pour comparaison)
console.log("\n=== VERIFICATION AVEC METHODES NATIVES ===");
console.log("Math.min(): " + Math.min(...nombres));
console.log("Math.max(): " + Math.max(...nombres));
console.log("Array.sort(): " + [...nombres].sort((a, b) => a - b));
```

### Solution Exercice 3
```javascript
// Carnet d'adresses simple
let contacts = [];

// Fonction pour ajouter un contact
function ajouterContact(nom, telephone, email) {
    // Verification que le contact n'existe pas deja
    for (let i = 0; i < contacts.length; i++) {
        if (contacts[i].nom.toLowerCase() === nom.toLowerCase()) {
            console.log("Contact deja existant !");
            return;
        }
    }
    
    let nouveauContact = {
        nom: nom,
        telephone: telephone,
        email: email
    };
    
    contacts.push(nouveauContact);
    console.log("Contact ajoute: " + nom);
}

// Fonction pour rechercher un contact
function rechercherContact(motCle) {
    let contactsTrouves = [];
    
    for (let i = 0; i < contacts.length; i++) {
        let contact = contacts[i];
        let motCleLower = motCle.toLowerCase();
        
        // Recherche dans nom, telephone, ou email
        if (contact.nom.toLowerCase().includes(motCleLower) ||
            contact.telephone.includes(motCle) ||
            contact.email.toLowerCase().includes(motCleLower)) {
            
            contactsTrouves.push(contact);
        }
    }
    
    return contactsTrouves;
}

// Fonction pour supprimer un contact
function supprimerContact(nom) {
    for (let i = 0; i < contacts.length; i++) {
        if (contacts[i].nom.toLowerCase() === nom.toLowerCase()) {
            let contactSupprime = contacts.splice(i, 1)[0];
            console.log("Contact supprime: " + contactSupprime.nom);
            return;
        }
    }
    
    console.log("Contact non trouve !");
}

// Fonction pour trier les contacts par nom
function trierContacts() {
    contacts.sort(function(a, b) {
        if (a.nom.toLowerCase() < b.nom.toLowerCase()) {
            return -1;
        }
        if (a.nom.toLowerCase() > b.nom.toLowerCase()) {
            return 1;
        }
        return 0;
    });
}

// Fonction pour afficher tous les contacts
function afficherContacts() {
    console.log("\n=== CARNET D'ADRESSES ===");
    
    if (contacts.length === 0) {
        console.log("Aucun contact enregistre");
        return;
    }
    
    trierContacts();  // Trier avant affichage
    
    for (let i = 0; i < contacts.length; i++) {
        let contact = contacts[i];
        console.log((i + 1) + ". " + contact.nom);
        console.log("   Tel: " + contact.telephone);
        console.log("   Email: " + contact.email);
        console.log("");
    }
    
    console.log("Total: " + contacts.length + " contact(s)");
}

// Fonction pour exporter les contacts (bonus)
function exporterContacts() {
    console.log("=== EXPORT DES CONTACTS ===");
    
    let exportTexte = "Nom;Telephone;Email\n";
    
    for (let i = 0; i < contacts.length; i++) {
        let contact = contacts[i];
        exportTexte += contact.nom + ";" + contact.telephone + ";" + contact.email + "\n";
    }
    
    console.log(exportTexte);
}

// Tests du carnet d'adresses
ajouterContact("Alice Dupont", "06.12.34.56.78", "alice@email.com");
ajouterContact("Bob Martin", "06.87.65.43.21", "bob.martin@email.com");
ajouterContact("Charlie Dubois", "06.11.22.33.44", "charlie@email.com");
ajouterContact("Diana Prince", "06.99.88.77.66", "diana.prince@email.com");

afficherContacts();

console.log("=== RECHERCHE ===");
let resultats = rechercherContact("alice");
console.log("Resultats pour 'alice': " + resultats.length + " trouve(s)");
for (let i = 0; i < resultats.length; i++) {
    console.log("- " + resultats[i].nom + " (" + resultats[i].email + ")");
}

console.log("\n=== SUPPRESSION ===");
supprimerContact("Bob Martin");

afficherContacts();

exporterContacts();
```

---

## LIENS AVEC D'AUTRES CONCEPTS

### Concepts prealables utilises
- **Variables** : Pour stocker les arrays
- **Types** : Arrays peuvent contenir tous types
- **Fonctions** : Pour manipuler les arrays proprement
- **Boucles** : Pour parcourir tous les elements
- **Conditions** : Pour filtrer et valider

### Prochaines etapes  
- **Methodes d'arrays avancees** : map(), filter(), reduce()
- **Objets** : Structurer des donnees plus complexes
- **JSON** : Format pour echanger des arrays
- **API** : Recevoir et envoyer des arrays de donnees

### Concepts complementaires
- **Destructuring** : Extraire des elements facilement
- **Spread operator** : Copier et fusionner des arrays
- **Array multidimensionnels** : Arrays dans des arrays

---

## PROJET MINI

### Defi pratique
**Objectif :** Creer un gestionnaire de notes d'examen complet

**Fonctionnalites requises :**
- [ ] Ajouter des matieres avec plusieurs notes
- [ ] Calculer moyenne par matiere et generale
- [ ] Trouver meilleure/pire note
- [ ] Afficher bulletin de notes formate
- [ ] Sauvegarder en format texte
- [ ] Simuler les points necessaires pour atteindre une moyenne

**Arrays a utiliser :**
- Array des matieres
- Array des notes par matiere
- Array des coefficients par matiere

**Structure de fichiers suggeree :**
```
gestionnaire-notes/
├── index.html
├── style.css  
└── script.js
```

**Temps estime :** 2 heures

---

## RESSOURCES COMPLEMENTAIRES

### Documentation officielle
- [MDN - Array](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN - Methodes d'arrays](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array#m%C3%A9thodes)
- [MDN - Boucles et iteration](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Loops_and_iteration)

### Videos recommandees
- "Arrays JavaScript pour debutants" - Grafikart (60min)
- "Methodes d'arrays essentielles" - manipulation avancee

### Articles approfondis
- "10 methodes d'arrays a maitriser" - guide pratique complet
- "Performance des arrays" - optimisation pour gros volumes

### Outils de pratique
- [CodePen - Arrays playground](https://codepen.io) - experimenter avec arrays
- [Array Explorer](https://arrayexplorer.netlify.app/) - outil interactif pour apprendre

---

## AUTO-EVALUATION

### Questions de comprehension
1. **Question conceptuelle :** Quel est l'index du premier element d'un array ?
   - Reponse : 0 (les arrays commencent a l'index 0)

2. **Question pratique :** Comment ajouter un element a la fin d'un array ?
   - Reponse : Utiliser la methode push() : `monArray.push(nouvelElement)`

3. **Question d'application :** Si un array a 5 elements, quel est l'index du dernier ?
   - Reponse : 4 (car on commence a 0 : 0,1,2,3,4)

### Checklist de maitrise
- [ ] Je sais creer un array avec []
- [ ] Je comprends que l'index commence a 0
- [ ] Je peux acceder a un element avec array[index]
- [ ] Je connais push(), pop(), shift(), unshift()
- [ ] Je peux parcourir un array avec une boucle for
- [ ] Je comprends la propriete length

**Score de confiance :** ___/10

---

## DEBUGGING ET ERREURS COURANTES

### Erreur frequente 1
**Code problematique :**
```javascript
let fruits = ["pomme", "banane", "orange"];
console.log(fruits[3]);  // undefined
```

**Solution :**
```javascript
let fruits = ["pomme", "banane", "orange"];
console.log(fruits[2]);  // "orange" (dernier element)
// ou verifier avant d'acceder
if (fruits.length > 3) {
    console.log(fruits[3]);
} else {
    console.log("Index trop grand !");
}
```

**Explication :** Array de 3 elements va de l'index 0 a 2. Index 3 n'existe pas.

### Erreur frequente 2
**Code problematique :**
```javascript
let nombres = [1, 2, 3];
for (let i = 0; i <= nombres.length; i++) {  // <= au lieu de <
    console.log(nombres[i]);
}
// Affiche undefined a la fin
```

**Solution :**
```javascript
let nombres = [1, 2, 3];
for (let i = 0; i < nombres.length; i++) {  // < pas <=
    console.log(nombres[i]);
}
```

**Explication :** Utilise `i < array.length` et non `i <= array.length` dans les boucles.

### Erreur frequente 3
**Code problematique :**
```javascript
let array1 = [1, 2, 3];
let array2 = array1;  // Ce n'est PAS une copie !
array2.push(4);
console.log(array1);  // [1, 2, 3, 4] - modifie aussi !
```

**Solution :**
```javascript
let array1 = [1, 2, 3];
let array2 = [...array1];  // Vraie copie avec spread operator
// ou
let array2 = array1.slice();  // Copie avec slice()
array2.push(4);
console.log(array1);  // [1, 2, 3] - inchange
console.log(array2);  // [1, 2, 3, 4]
```

**Explication :** Les arrays sont des references. Pour copier, utilise spread `...` ou `slice()`.

---

## NOTES PERSONNELLES

### Ce que j'ai appris aujourd'hui
- [Les arrays stockent plusieurs valeurs dans une variable]
- [L'index commence toujours a 0]  
- [Il y a plein de methodes pour manipuler les arrays]

### Questions a approfondir
- [Comment utiliser map() et filter() ?]
- [Comment trier des arrays d'objets complexes ?]

### Revision programmee
- [ ] Revision dans 1 jour (refaire les exercices d'arrays)
- [ ] Revision dans 1 semaine (creer un projet complet avec arrays)
- [ ] Revision dans 1 mois (expliquer les arrays a quelqu'un)

---

## TAGS ET METADONNEES

**Mots-cles :** javascript arrays tableaux listes index length push pop shift unshift

**Temps de completion :** [A remplir]

**Difficulte ressentie :** 1 2 3 4 5 (entoure le chiffre)

**Satisfaction du cours :** 1 2 3 4 5 (entoure le chiffre)

**Pret pour le concept suivant :** OUI / NON

---

*Fiche creee le : 17/07/2025*
*Derniere revision : 17/07/2025*
*Position dans la roadmap : 8/126*
