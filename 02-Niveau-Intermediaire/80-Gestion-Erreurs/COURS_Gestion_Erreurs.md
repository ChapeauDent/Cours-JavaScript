# Gestion d'erreurs en JavaScript

## **INFORMATIONS GENERALES**

### Metadonnees du cours
- **Titre :** Gestion d'erreurs avec try/catch/finally et throw
- **Niveau :** Intermediaire
- **Duree estimee :** 45 minutes
- **Prerequis :** Variables, fonctions, structures de controle, objets
- **Date de creation :** 17/07/2025
- **Derniere revision :** 17/07/2025

---

## **OBJECTIFS PEDAGOGIQUES**

### A la fin de cette lecon, vous serez capable de :
- [ ] **Comprendre** : Le concept d'erreur et pourquoi les gerer
- [ ] **Appliquer** : Les blocs try/catch/finally pour capturer les erreurs
- [ ] **Analyser** : Les differents types d'erreurs JavaScript
- [ ] **Creer** : Des erreurs personnalisees avec throw

### Mots-cles a retenir
- **Exception** : Une erreur qui interrompt l'execution normale du code
- **try/catch** : Structure pour gerer les erreurs de maniere controlee
- **throw** : Mot-cle pour lancer une erreur personnalisee
- **Error object** : Objet JavaScript contenant les informations d'une erreur

---

## **PARTIE THEORIQUE**

### Pourquoi ce concept est-il important ?
Dans le monde reel, votre code JavaScript peut rencontrer des situations imprevues : un fichier qui n'existe pas, une connexion internet coupee, ou des donnees incorrectes. Sans gestion d'erreurs, ces problemes feraient planter votre application entiere. La gestion d'erreurs vous permet de prevoir ces cas et de reagir de maniere appropriee.

### Definition et concepts cles
Une **erreur** (ou exception) est un evenement qui interrompt le flux normal d'execution du programme. JavaScript fournit plusieurs mecanismes pour gerer ces erreurs :

1. **try/catch** : Pour capturer et gerer les erreurs
2. **finally** : Pour executer du code quoi qu'il arrive
3. **throw** : Pour lancer des erreurs personnalisees
4. **Error objects** : Objets contenant les details de l'erreur

### Syntaxe de base
```javascript
// Structure try/catch basique
try {
    // Code qui peut generer une erreur
} catch (error) {
    // Code execute si une erreur survient
}

// Avec finally
try {
    // Code a risque
} catch (error) {
    // Gestion de l'erreur
} finally {
    // Code execute dans tous les cas
}

// Lancer une erreur
throw new Error("Message d'erreur personnalise");
```

### Points d'attention
- **Piege courant 1** : Utiliser try/catch pour tout - cela peut masquer de vrais problemes
- **Piege courant 2** : Ne pas gerer specifiquement les types d'erreurs differents
- **Bonne pratique** : Toujours fournir des messages d'erreur clairs et utiles

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// Gestion d'une erreur de division par zero
function diviser(a, b) {
    try {
        if (b === 0) {
            throw new Error("Division par zero impossible");
        }
        return a / b;
    } catch (error) {
        console.log("Erreur detectee :", error.message);
        return null;
    }
}

console.log(diviser(10, 2)); // 5
console.log(diviser(10, 0)); // Erreur detectee : Division par zero impossible
```

**Resultat attendu :**
```
5
Erreur detectee : Division par zero impossible
null
```

### Exemple intermediaire
```javascript
// Gestion d'erreurs avec fetch et finally
async function recupererDonnees(url) {
    let reponse = null;
    
    try {
        console.log("Tentative de connexion...");
        reponse = await fetch(url);
        
        if (!reponse.ok) {
            throw new Error(`Erreur HTTP: ${reponse.status}`);
        }
        
        const donnees = await reponse.json();
        console.log("Donnees recuperees avec succes");
        return donnees;
        
    } catch (error) {
        if (error.name === "TypeError") {
            console.log("Erreur de connexion :", error.message);
        } else {
            console.log("Erreur serveur :", error.message);
        }
        return null;
        
    } finally {
        console.log("Operation terminee");
        // Nettoyage si necessaire
    }
}
```

### Exemple avance (optionnel)
```javascript
// Classe d'erreur personnalisee
class ErreurValidation extends Error {
    constructor(message, champ) {
        super(message);
        this.name = "ErreurValidation";
        this.champ = champ;
    }
}

function validerAge(age) {
    try {
        if (typeof age !== "number") {
            throw new ErreurValidation("L'age doit etre un nombre", "age");
        }
        if (age < 0) {
            throw new ErreurValidation("L'age ne peut pas etre negatif", "age");
        }
        if (age > 150) {
            throw new ErreurValidation("L'age semble irrealiste", "age");
        }
        return true;
    } catch (error) {
        if (error instanceof ErreurValidation) {
            console.log(`Erreur de validation sur ${error.champ}: ${error.message}`);
        } else {
            console.log("Erreur inattendue :", error.message);
        }
        return false;
    }
}
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : Calculatrice securisee
**Consigne :** Creez une fonction calculatrice qui gere les erreurs pour les operations mathematiques de base.

**Code de depart :**
```javascript
function calculatrice(operation, a, b) {
    // TODO: Ajouter la gestion d'erreurs
    // Operations supportees: +, -, *, /
    // Gerer: division par zero, operation invalide, parametres non-numeriques
}

// Tests
console.log(calculatrice("+", 5, 3));
console.log(calculatrice("/", 10, 0));
console.log(calculatrice("x", 4, 2));
console.log(calculatrice("+", "a", 3));
```

**Resultat attendu :**
```
8
Erreur: Division par zero
Erreur: Operation invalide
Erreur: Parametres doivent etre des nombres
```

**Niveau de difficulte :** ⭐⭐☆☆☆

### Exercice 2 : Gestionnaire de fichiers
**Consigne :** Simulez la lecture de fichiers avec gestion d'erreurs appropriee.

**Contraintes :**
- Simuler differents types d'erreurs (fichier inexistant, permissions, etc.)
- Utiliser un bloc finally pour nettoyer les ressources
- Fournir des messages d'erreur clairs

**Niveau de difficulte :** ⭐⭐⭐☆☆

### Exercice 3 : Validateur de formulaire (optionnel)
**Consigne :** Creez un systeme de validation de formulaire avec erreurs personnalisees.

**Niveau de difficulte :** ⭐⭐⭐⭐☆

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
function calculatrice(operation, a, b) {
    try {
        // Verification des types
        if (typeof a !== "number" || typeof b !== "number") {
            throw new Error("Parametres doivent etre des nombres");
        }
        
        // Verification de l'operation
        switch (operation) {
            case "+":
                return a + b;
            case "-":
                return a - b;
            case "*":
                return a * b;
            case "/":
                if (b === 0) {
                    throw new Error("Division par zero");
                }
                return a / b;
            default:
                throw new Error("Operation invalide");
        }
    } catch (error) {
        console.log("Erreur:", error.message);
        return null;
    }
}
```

**Explication :** Cette solution verifie systematiquement tous les cas d'erreur possibles et fournit des messages clairs pour chaque situation.

### Solution Exercice 2
```javascript
function lireFichier(nomFichier) {
    // Simulation d'une base de donnees de fichiers
    const fichiers = {
        "document.txt": "Contenu du document",
        "prive.txt": null // Simule un fichier sans permissions
    };
    
    let fichierOuvert = false;
    
    try {
        console.log(`Ouverture du fichier: ${nomFichier}`);
        fichierOuvert = true;
        
        if (!fichiers.hasOwnProperty(nomFichier)) {
            throw new Error("Fichier inexistant");
        }
        
        if (fichiers[nomFichier] === null) {
            throw new Error("Permissions insuffisantes");
        }
        
        console.log("Lecture reussie");
        return fichiers[nomFichier];
        
    } catch (error) {
        console.log(`Erreur lors de la lecture: ${error.message}`);
        return null;
        
    } finally {
        if (fichierOuvert) {
            console.log("Fermeture du fichier");
        }
    }
}
```

**Alternatives possibles :**
- Utiliser des codes d'erreur numeriques
- Implementer un systeme de retry automatique

### Solution Exercice 3
```javascript
class ErreurValidation extends Error {
    constructor(message, champ, valeur) {
        super(message);
        this.name = "ErreurValidation";
        this.champ = champ;
        this.valeur = valeur;
    }
}

function validerFormulaire(donnees) {
    const erreurs = [];
    
    try {
        // Validation email
        if (!donnees.email || !donnees.email.includes("@")) {
            throw new ErreurValidation("Email invalide", "email", donnees.email);
        }
        
        // Validation mot de passe
        if (!donnees.motDePasse || donnees.motDePasse.length < 8) {
            throw new ErreurValidation("Mot de passe trop court", "motDePasse");
        }
        
        // Validation age
        if (donnees.age < 13) {
            throw new ErreurValidation("Age minimum requis: 13 ans", "age", donnees.age);
        }
        
        return { valide: true, erreurs: [] };
        
    } catch (error) {
        if (error instanceof ErreurValidation) {
            erreurs.push({
                champ: error.champ,
                message: error.message,
                valeur: error.valeur
            });
        }
        return { valide: false, erreurs };
    }
}
```

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts prealables utilises
- **Fonctions** : Les erreurs surviennent souvent dans les fonctions
- **Structures de controle** : if/else pour verifier les conditions d'erreur
- **Objets** : Les objets Error contiennent les informations d'erreur

### Prochaines etapes
- **Programmation asynchrone** : Gestion d'erreurs avec async/await
- **APIs du navigateur** : Gestion d'erreurs avec fetch et localStorage
- **Debugging** : Utiliser les erreurs pour debugger efficacement

### Concepts complementaires
- **Testing** : Tester les cas d'erreur dans votre code
- **Logging** : Enregistrer les erreurs pour le debugging

---

## **PROJET MINI**

### Defi pratique
**Objectif :** Creer un convertisseur d'unites robuste avec gestion d'erreurs complete

**Fonctionnalites requises :**
- [ ] Conversion temperature (Celsius, Fahrenheit, Kelvin)
- [ ] Validation des entrees avec erreurs personnalisees
- [ ] Interface utilisateur qui affiche les erreurs clairement
- [ ] Gestion des cas limites (temperatures impossibles)

**Structure de fichiers suggeree :**
```
📁 convertisseur-unites/
├── 📄 index.html
├── 📄 style.css
└── 📄 convertisseur.js
```

**Temps estime :** 60 minutes

---

## **RESSOURCES COMPLEMENTAIRES**

### Documentation officielle
- [MDN - Error handling](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#error_handling_statements)
- [MDN - Error object](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Error)

### Videos recommandees
- "JavaScript Error Handling" - The Net Ninja (15 min)
- Pourquoi cette video est utile : Explications visuelles claires

### Articles approfondis
- "Error Handling in JavaScript" - JavaScript.info
- Ce que cet article apporte en plus : Exemples avances et patterns

### Outils de pratique
- [CodePen - Error Handling Examples](https://codepen.io/tag/error-handling)
- [Exercices interactifs MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)

---

## **AUTO-EVALUATION**

### Questions de comprehension
1. **Question conceptuelle :** Quelle est la difference entre try/catch et if/else ?
   - Reponse : try/catch gere les erreurs qui interrompent l'execution, if/else gere les conditions logiques normales

2. **Question pratique :** Que fait ce code ?
   ```javascript
   try {
       console.log("Debut");
       throw new Error("Probleme");
       console.log("Fin"); // Cette ligne ne s'execute jamais
   } catch (error) {
       console.log("Erreur:", error.message);
   } finally {
       console.log("Nettoyage");
   }
   ```
   - Reponse : Affiche "Debut", puis "Erreur: Probleme", puis "Nettoyage". La ligne "Fin" n'est jamais executee.

3. **Question d'application :** Dans quel cas utiliseriez-vous un bloc finally ?
   - Reponse : Pour nettoyer des ressources (fermer des fichiers, connexions) qui doivent etre liberees quoi qu'il arrive

### Checklist de maitrise
- [ ] Je comprends la syntaxe try/catch/finally
- [ ] Je sais identifier les cas ou gerer les erreurs
- [ ] Je peux creer des erreurs personnalisees avec throw
- [ ] Je peux expliquer les differents types d'erreurs
- [ ] Je peux debugger les erreurs courantes
- [ ] Je peux combiner la gestion d'erreurs avec d'autres concepts

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur frequente 1
**Code problematique :**
```javascript
try {
    let result = riskyFunction();
    console.log(result);
} catch {
    console.log("Une erreur s'est produite");
}
```

**Message d'erreur :** `SyntaxError: Missing catch or finally after try`

**Solution :**
```javascript
try {
    let result = riskyFunction();
    console.log(result);
} catch (error) {
    console.log("Une erreur s'est produite:", error.message);
}
```

**Explication :** Le bloc catch doit toujours avoir un parametre pour recevoir l'objet erreur, meme si vous ne l'utilisez pas.

### Erreur frequente 2
**Code problematique :**
```javascript
function diviser(a, b) {
    if (b === 0) {
        return "Erreur: division par zero";
    }
    return a / b;
}
```

**Probleme :** Retourner une string au lieu de gerer l'erreur proprement

**Solution :**
```javascript
function diviser(a, b) {
    if (b === 0) {
        throw new Error("Division par zero impossible");
    }
    return a / b;
}
```

**Explication :** Il vaut mieux lancer une erreur que retourner un type different, cela permet à l'appelant de gerer l'erreur appropriement.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- La difference entre erreurs et conditions logiques
- L'importance du bloc finally pour le nettoyage
- Comment creer des erreurs personnalisees

### Questions a approfondir
- Comment gerer les erreurs dans le code asynchrone
- Quand utiliser different types d'objets Error

### Revision programmee
- [ ] Revision dans 1 jour (rappel rapide)
- [ ] Revision dans 1 semaine (exercices)
- [ ] Revision dans 1 mois (application dans un projet)

---

## **TAGS ET METADONNEES**

**Mots-cles :** #javascript #gestion-erreurs #try-catch #throw #debugging #intermediaire

**Temps de completion :** [Temps reellement passe]

**Difficulte ressentie :** ⭐⭐⭐☆☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Pret pour le concept suivant :** ✅ Oui / ❌ Non (revision necessaire)

---

*📅 Fiche creee le : 17/07/2025*
*🔄 Derniere revision : 17/07/2025*
*📍 Position dans la roadmap : 11/126*
