# 🧩 Premiers Algorithmes Simples en JavaScript

{% hint style="info" %}
**Niveau :** 🟡 Débutant→Intermédiaire | **Durée :** 90 min | **Prérequis :** Variables, Types, Opérateurs, Comparaisons, Structures de contrôle
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** ce qu'est un algorithme et comment l'appliquer
- [ ] **Décomposer** un problème complexe en étapes simples
- [ ] **Appliquer** tous les concepts débutants ensemble dans des mini-programmes
- [ ] **Créer** vos premiers algorithmes pour résoudre des problèmes concrets
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Définitions" %}
**Algorithme** : Suite d'instructions pour résoudre un problème  
**Logique** : Raisonnement structuré pour arriver à une solution  
**Itération** : Répétition d'actions dans une boucle  
**Condition** : Test qui détermine le chemin à suivre  
**Variable de travail** : Variable temporaire pour stocker des calculs  
{% endtab %}

{% tab title="Méthodologie" %}
**Étapes pour créer un algorithme :**
1. **Comprendre** le problème (qu'est-ce qu'on veut ?)
2. **Décomposer** en sous-problèmes plus simples
3. **Identifier** les données nécessaires (variables)
4. **Écrire** les étapes logiques (pseudocode)
5. **Traduire** en code JavaScript
6. **Tester** avec différents cas
{% endtab %}

{% tab title="Types d'algorithmes" %}
🔍 **Recherche** : Trouver un élément dans une liste  
📊 **Tri** : Classer des éléments par ordre  
🧮 **Calcul** : Somme, moyenne, minimum, maximum  
✅ **Validation** : Vérifier des données (âge, email, etc.)  
🔄 **Transformation** : Modifier des données  
{% endtab %}
{% endtabs %}

## 🎨 Analogie : L'algorithme comme une recette

{% hint style="info" %}
**Un algorithme, c'est comme une recette de cuisine :**

🥘 **Ingrédients** = Variables et données  
📋 **Étapes** = Instructions dans l'ordre  
🍳 **Techniques** = Boucles, conditions, calculs  
🍽️ **Résultat** = Solution au problème  

**Exemple concret :** Faire des crêpes
1. Mélanger farine, œufs, lait *(variables)*
2. Pour chaque crêpe *(boucle)*
3. Si la crêpe est dorée *(condition)*
4. Retourner la crêpe *(action)*
{% endhint %}

## 🧠 Pourquoi ce concept est-il crucial ?

{% hint style="warning" %}
**Cette leçon fait le pont entre :**

📚 **"Connaître la syntaxe"** → Savoir écrire `if`, `for`, etc.  
🚀 **"Savoir programmer"** → Résoudre de vrais problèmes  

**Maintenant que vous connaissez les briques de base** (variables, types, opérateurs, conditions, boucles), **il est temps de les assembler** pour créer quelque chose d'utile !
{% endhint %}

## 📝 Template d'algorithme simple

```javascript
// Structure générale d'un algorithme
function resoudreProbleme(donnees) {
    // 1. Initialiser les variables de travail
    let resultat = 0;
    let trouve = false;
    
    // 2. Traiter les données (souvent avec une boucle)
    for (let i = 0; i < donnees.length; i++) {
        // 3. Appliquer la logique selon les conditions
        if (condition) {
            // Action 1
        } else {
            // Action 2
        }
    }
    
    // 4. Retourner le résultat
    return resultat;
}
```

## 🎯 Algorithmes essentiels à maîtriser

### 1. 🔍 Recherche du maximum

{% tabs %}
{% tab title="Problème" %}
**Objectif :** Trouver le plus grand nombre dans une liste

**Approche :**
1. Commencer avec le premier nombre
2. Comparer avec tous les autres
3. Garder le plus grand trouvé
{% endtab %}

{% tab title="Code commenté" %}
```javascript
function trouverPlusGrand(nombres) {
    // Étape 1: Vérifier qu'on a des données
    if (nombres.length === 0) {
        return "Liste vide !";
    }
    
    // Étape 2: Commencer avec le premier nombre
    let plusGrand = nombres[0];
    console.log("🔹 On commence avec:", plusGrand);
    
    // Étape 3: Comparer avec tous les autres
    for (let i = 1; i < nombres.length; i++) {
        console.log(`🔍 Comparaison: ${plusGrand} vs ${nombres[i]}`);
        
        if (nombres[i] > plusGrand) {
            plusGrand = nombres[i];
            console.log(`🏆 Nouveau plus grand: ${plusGrand}`);
        }
    }
    
    // Étape 4: Retourner le résultat
    return plusGrand;
}

// Test de l'algorithme
const mesNombres = [3, 8, 1, 15, 4, 9];
const resultat = trouverPlusGrand(mesNombres);
console.log("✅ Le plus grand nombre est:", resultat);
```
{% endtab %}

{% tab title="Résultat" %}
```
🔹 On commence avec: 3
🔍 Comparaison: 3 vs 8
🏆 Nouveau plus grand: 8
🔍 Comparaison: 8 vs 1
🔍 Comparaison: 8 vs 15
🏆 Nouveau plus grand: 15
🔍 Comparaison: 15 vs 4
🔍 Comparaison: 15 vs 9
✅ Le plus grand nombre est: 15
```
{% endtab %}
{% endtabs %}

### 2. 📊 Analyse statistique

{% tabs %}
{% tab title="Problème" %}
**Objectif :** Calculer des statistiques sur des notes d'élève

**Ce qu'on veut :**
- Moyenne des notes
- Note minimale et maximale
- Mention selon la moyenne
- Nombre total de notes
{% endtab %}

{% tab title="Code complet" %}
```javascript
function analyserNotes(notes) {
    console.log("📊 === ANALYSE DES NOTES ===");
    console.log("📝 Notes à analyser:", notes);
    
    // Variables de travail pour nos calculs
    let somme = 0;
    let minimum = notes[0];
    let maximum = notes[0];
    let nombreNotes = notes.length;
    
    // Parcourir toutes les notes une seule fois (efficace !)
    for (let i = 0; i < notes.length; i++) {
        let noteActuelle = notes[i];
        
        // Additionner pour la moyenne
        somme = somme + noteActuelle;
        
        // Chercher le minimum
        if (noteActuelle < minimum) {
            minimum = noteActuelle;
        }
        
        // Chercher le maximum  
        if (noteActuelle > maximum) {
            maximum = noteActuelle;
        }
        
        console.log(`📋 Note ${i+1}: ${noteActuelle} | Somme actuelle: ${somme}`);
    }
    
    // Calculer la moyenne
    let moyenne = somme / nombreNotes;
    
    // Déterminer la mention (logique de décision)
    let mention;
    if (moyenne >= 16) {
        mention = "Très bien 🏆";
    } else if (moyenne >= 14) {
        mention = "Bien 👍";  
    } else if (moyenne >= 12) {
        mention = "Assez bien 👌";
    } else if (moyenne >= 10) {
        mention = "Passable 😐";
    } else {
        mention = "Insuffisant 😞";
    }
    
    // Afficher les résultats
    console.log("\n🎯 === RÉSULTATS ===");
    console.log("📊 Nombre de notes:", nombreNotes);
    console.log("➕ Somme totale:", somme);
    console.log("📉 Note minimum:", minimum);
    console.log("📈 Note maximum:", maximum);
    console.log("📊 Moyenne:", moyenne.toFixed(2));
    console.log("🏅 Mention:", mention);
    
    return {
        moyenne: moyenne,
        minimum: minimum,
        maximum: maximum,
        mention: mention
    };
}

// Test avec des vraies notes
const notesEleve = [14, 16, 12, 18, 13, 15, 17];
analyserNotes(notesEleve);
```
{% endtab %}

{% tab title="Résultat" %}
```
📊 === ANALYSE DES NOTES ===
📝 Notes à analyser: [14, 16, 12, 18, 13, 15, 17]
📋 Note 1: 14 | Somme actuelle: 14
📋 Note 2: 16 | Somme actuelle: 30
📋 Note 3: 12 | Somme actuelle: 42
📋 Note 4: 18 | Somme actuelle: 60
📋 Note 5: 13 | Somme actuelle: 73
📋 Note 6: 15 | Somme actuelle: 88
📋 Note 7: 17 | Somme actuelle: 105

🎯 === RÉSULTATS ===
📊 Nombre de notes: 7
➕ Somme totale: 105
📉 Note minimum: 12
📈 Note maximum: 18
📊 Moyenne: 15.00
🏅 Mention: Bien 👍
```
{% endtab %}
{% endtabs %}

### 3. 🎲 Jeu de devinette (logique complète)

{% tabs %}
{% tab title="Principe" %}
**Objectif :** Créer un jeu où l'ordinateur devine un nombre

**Logique du jeu :**
1. Générer un nombre secret aléatoire
2. Permettre des tentatives limitées
3. Donner des indices (trop grand/petit)
4. Déclarer victoire ou défaite
{% endtab %}

{% tab title="Code du jeu" %}
```javascript
function jeuDevinette() {
    console.log("🎲 === JEU DE DEVINETTE ===");
    console.log("🤔 Je pense à un nombre entre 1 et 100...");
    
    // Générer le nombre secret
    const nombreSecret = Math.floor(Math.random() * 100) + 1;
    console.log("🤫 (Psst... le nombre secret est:", nombreSecret, "mais chut !)");
    
    // Variables du jeu
    let tentatives = 0;
    const maxTentatives = 7;
    let trouve = false;
    
    // Simulation de tentatives (normalement de l'utilisateur)
    const tentativesSimulees = [50, 75, 63, 69, 66, 68, 67]; // Exemple pour 67
    
    console.log("\n🎯 Commence à deviner !");
    
    // Boucle principale du jeu
    while (tentatives < maxTentatives && !trouve) {
        tentatives++;
        const proposition = tentativesSimulees[tentatives - 1]; // Simulation
        
        console.log(`\n🎯 Tentative ${tentatives}: ${proposition}`);
        
        // Logique de comparaison
        if (proposition === nombreSecret) {
            trouve = true;
            console.log(`🎉 BRAVO ! Tu as trouvé en ${tentatives} tentatives !`);
        } else if (proposition < nombreSecret) {
            console.log("📈 Trop petit ! Essaie plus grand.");
        } else {
            console.log("📉 Trop grand ! Essaie plus petit.");
        }
        
        // Afficher les tentatives restantes
        if (!trouve && tentatives < maxTentatives) {
            const restantes = maxTentatives - tentatives;
            console.log(`⏰ Il te reste ${restantes} tentative(s).`);
        }
    }
    
    // Fin de jeu
    if (!trouve) {
        console.log(`\n💔 Dommage ! Le nombre était ${nombreSecret}. Ressaie !`);
    }
    
    return {
        gagne: trouve,
        tentatives: tentatives,
        nombreSecret: nombreSecret
    };
}

// Lancer le jeu
jeuDevinette();
```
{% endtab %}
{% endtabs %}

## 💻 Exercices pratiques

### Exercice 1 : Compteur de voyelles

{% hint style="info" %}
**Créez un algorithme qui compte les voyelles dans un mot.**

**Étapes suggérées :**
1. Définir les voyelles (a, e, i, o, u)
2. Parcourir chaque lettre du mot
3. Compter si c'est une voyelle
4. Retourner le nombre total
{% endhint %}

```javascript
function compterVoyelles(mot) {
    // TODO: À vous de jouer !
}

// Tests
console.log(compterVoyelles("bonjour"));    // Doit donner 3 (o, o, u)
console.log(compterVoyelles("javascript")); // Doit donner 3 (a, a, i)
console.log(compterVoyelles("xyz"));        // Doit donner 0
```

<details>
<summary>💡 Solution détaillée</summary>

```javascript
function compterVoyelles(mot) {
    // Étape 1: Définir les voyelles (minuscules et majuscules)
    const voyelles = "aeiouAEIOU";
    let compteur = 0;
    
    console.log(`🔍 Analyse du mot: "${mot}"`);
    
    // Étape 2: Parcourir chaque caractère du mot
    for (let i = 0; i < mot.length; i++) {
        const lettre = mot[i];
        
        // Étape 3: Vérifier si c'est une voyelle
        if (voyelles.includes(lettre)) {
            compteur++;
            console.log(`✅ Position ${i}: "${lettre}" est une voyelle. Total: ${compteur}`);
        } else {
            console.log(`➖ Position ${i}: "${lettre}" n'est pas une voyelle.`);
        }
    }
    
    console.log(`🎯 Résultat final: ${compteur} voyelles dans "${mot}"\n`);
    return compteur;
}

// Version plus concise
function compterVoyellesSimple(mot) {
    const voyelles = "aeiouAEIOU";
    let compteur = 0;
    
    for (let i = 0; i < mot.length; i++) {
        if (voyelles.includes(mot[i])) {
            compteur++;
        }
    }
    
    return compteur;
}

// Tests
compterVoyelles("bonjour");    // 3
compterVoyelles("javascript"); // 3
compterVoyelles("xyz");        // 0
```

**Concepts utilisés :**
- ✅ Boucle `for` pour parcourir la string
- ✅ Condition `if` pour tester chaque caractère
- ✅ Méthode `includes()` pour vérifier l'appartenance
- ✅ Variable compteur pour accumuler le résultat

</details>

### Exercice 2 : Calculateur de prix avec remise

{% hint style="info" %}
**Créez un algorithme qui calcule le prix final avec remises selon le montant.**

**Règles de remise :**
- 0-50€ : pas de remise
- 50-100€ : 5% de remise
- 100-200€ : 10% de remise
- Plus de 200€ : 15% de remise
- **Bonus :** Ajouter la TVA (20%)
{% endhint %}

<details>
<summary>🚀 Code de départ</summary>

```javascript
function calculerPrixFinal(prixInitial, inclureTVA = true) {
    // TODO: 
    // 1. Déterminer le pourcentage de remise selon le prix
    // 2. Calculer le montant de la remise
    // 3. Appliquer la remise
    // 4. Ajouter la TVA si demandée
    // 5. Retourner le résultat détaillé
}

// Tests
calculerPrixFinal(30);     // Pas de remise
calculerPrixFinal(75);     // 5% de remise
calculerPrixFinal(150);    // 10% de remise
calculerPrixFinal(250);    // 15% de remise
```

</details>

<details>
<summary>💡 Solution complète</summary>

```javascript
function calculerPrixFinal(prixInitial, inclureTVA = true) {
    console.log(`💰 === CALCUL PRIX FINAL ===`);
    console.log(`💵 Prix initial: ${prixInitial}€`);
    
    // Étape 1: Déterminer le pourcentage de remise
    let remisePourcentage = 0;
    
    if (prixInitial >= 200) {
        remisePourcentage = 15;
    } else if (prixInitial >= 100) {
        remisePourcentage = 10;
    } else if (prixInitial >= 50) {
        remisePourcentage = 5;
    } else {
        remisePourcentage = 0;
    }
    
    console.log(`🎫 Remise applicable: ${remisePourcentage}%`);
    
    // Étape 2: Calculer le montant de la remise
    const montantRemise = (prixInitial * remisePourcentage) / 100;
    console.log(`💸 Montant de la remise: ${montantRemise.toFixed(2)}€`);
    
    // Étape 3: Prix après remise
    const prixApresRemise = prixInitial - montantRemise;
    console.log(`💰 Prix après remise: ${prixApresRemise.toFixed(2)}€`);
    
    // Étape 4: Ajouter la TVA si demandée
    let prixFinal = prixApresRemise;
    let montantTVA = 0;
    
    if (inclureTVA) {
        montantTVA = (prixApresRemise * 20) / 100;
        prixFinal = prixApresRemise + montantTVA;
        console.log(`📊 TVA (20%): ${montantTVA.toFixed(2)}€`);
    }
    
    console.log(`🏆 Prix final: ${prixFinal.toFixed(2)}€`);
    console.log("");
    
    return {
        prixInitial: prixInitial,
        remisePourcentage: remisePourcentage,
        montantRemise: montantRemise,
        prixApresRemise: prixApresRemise,
        montantTVA: montantTVA,
        prixFinal: prixFinal
    };
}

// Tests avec différents montants
calculerPrixFinal(30);     // Pas de remise
calculerPrixFinal(75);     // 5% de remise
calculerPrixFinal(150);    // 10% de remise
calculerPrixFinal(250);    // 15% de remise
calculerPrixFinal(100, false); // Sans TVA
```

**Concepts utilisés :**
- ✅ Conditions en cascade avec `if/else if`
- ✅ Calculs de pourcentages
- ✅ Paramètre par défaut (`inclureTVA = true`)
- ✅ Objet de retour avec tous les détails
- ✅ Méthode `toFixed()` pour formater les nombres

</details>

### Exercice 3 : Validateur de mot de passe

{% hint style="info" %}
**Créez un algorithme qui vérifie qu'un mot de passe respecte les règles de sécurité.**

**Critères de validation :**
- Minimum 8 caractères
- Au moins une majuscule
- Au moins une minuscule
- Au moins un chiffre
- Au moins un caractère spécial
{% endhint %}

<details>
<summary>💡 Solution avancée</summary>

```javascript
function validerMotDePasse(motDePasse) {
    console.log(`🔐 === VALIDATION MOT DE PASSE ===`);
    console.log(`🔍 Mot de passe à tester: "${motDePasse}"`);
    
    // Variables pour tracker les critères
    const criteres = {
        longueur: false,
        majuscule: false,
        minuscule: false,
        chiffre: false,
        caractereSpecial: false
    };
    
    const erreurs = [];
    
    // Critère 1: Longueur minimum 8 caractères
    if (motDePasse.length >= 8) {
        criteres.longueur = true;
        console.log("✅ Longueur OK (8+ caractères)");
    } else {
        erreurs.push("Doit contenir au moins 8 caractères");
        console.log("❌ Trop court (minimum 8 caractères)");
    }
    
    // Caractères spéciaux autorisés
    const caracteresSpeciaux = "!@#$%^&*()_+-=[]{}|;:,.<>?";
    
    // Parcourir chaque caractère pour vérifier les critères
    for (let i = 0; i < motDePasse.length; i++) {
        const caractere = motDePasse[i];
        
        // Vérifier majuscule (A-Z)
        if (caractere >= 'A' && caractere <= 'Z') {
            criteres.majuscule = true;
        }
        
        // Vérifier minuscule (a-z)
        if (caractere >= 'a' && caractere <= 'z') {
            criteres.minuscule = true;
        }
        
        // Vérifier chiffre (0-9)
        if (caractere >= '0' && caractere <= '9') {
            criteres.chiffre = true;
        }
        
        // Vérifier caractère spécial
        if (caracteresSpeciaux.includes(caractere)) {
            criteres.caractereSpecial = true;
        }
    }
    
    // Vérifier chaque critère et ajouter les erreurs
    if (criteres.majuscule) {
        console.log("✅ Contient au moins une majuscule");
    } else {
        erreurs.push("Doit contenir au moins une majuscule");
        console.log("❌ Manque une majuscule");
    }
    
    if (criteres.minuscule) {
        console.log("✅ Contient au moins une minuscule");
    } else {
        erreurs.push("Doit contenir au moins une minuscule");
        console.log("❌ Manque une minuscule");
    }
    
    if (criteres.chiffre) {
        console.log("✅ Contient au moins un chiffre");
    } else {
        erreurs.push("Doit contenir au moins un chiffre");
        console.log("❌ Manque un chiffre");
    }
    
    if (criteres.caractereSpecial) {
        console.log("✅ Contient au moins un caractère spécial");
    } else {
        erreurs.push("Doit contenir au moins un caractère spécial (!@#$%^&*...)");
        console.log("❌ Manque un caractère spécial");
    }
    
    // Résultat final
    const estValide = erreurs.length === 0;
    
    console.log(`\n🎯 === RÉSULTAT ===`);
    if (estValide) {
        console.log("🎉 Mot de passe VALIDE !");
    } else {
        console.log("❌ Mot de passe INVALIDE");
        console.log("🔧 Problèmes à corriger:");
        erreurs.forEach((erreur, index) => {
            console.log(`   ${index + 1}. ${erreur}`);
        });
    }
    console.log("");
    
    return {
        valide: estValide,
        criteres: criteres,
        erreurs: erreurs
    };
}

// Tests avec différents mots de passe
validerMotDePasse("123");              // Trop court, manque tout
validerMotDePasse("motdepasse");       // Manque maj/chiffre/spécial
validerMotDePasse("MotDePasse");       // Manque chiffre/spécial
validerMotDePasse("MotDePasse123");    // Manque spécial
validerMotDePasse("MotDePasse123!");   // VALIDE
```

**Concepts avancés utilisés :**
- ✅ Objet pour tracker plusieurs états
- ✅ Array pour collecter les erreurs
- ✅ Comparaison de caractères avec ASCII
- ✅ Logique complexe avec multiple conditions
- ✅ Retour d'objet structuré avec détails

</details>

## 🎮 Quiz interactif

{% hint style="info" %}
**Question 1 :** Quelle est la première étape pour créer un algorithme ?
A) Écrire le code  B) Comprendre le problème  C) Tester  D) Optimiser
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) Comprendre le problème**

Il faut d'abord bien comprendre ce qu'on veut résoudre avant de commencer à coder !

</details>

{% hint style="info" %}
**Question 2 :** Pour trouver le minimum dans une liste, avec quelle valeur faut-il initialiser la variable ?
A) 0  B) -1  C) Le premier élément  D) Infinity
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : C) Le premier élément**

Initialiser avec 0 ou -1 pourrait donner un mauvais résultat si tous les nombres sont plus grands !

</details>

## ⚠️ Pièges courants à éviter

### 1. 🪤 Boucle infinie

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
// DANGER: Boucle infinie !
let i = 0;
while (i < 10) {
    console.log(i);
    // Oubli de i++ -> boucle infinie !
}
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Toujours s'assurer que la condition change
let i = 0;
while (i < 10) {
    console.log(i);
    i++; // Incrémenter pour éviter la boucle infinie
}

// Ou utiliser une boucle for plus sûre
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```
{% endtab %}
{% endtabs %}

### 2. 🪤 Mauvaise initialisation

{% tabs %}
{% tab title="❌ Problème" %}
```javascript
// ERREUR: et si tous les nombres sont > 0 ?
function trouverMinimum(nombres) {
    let minimum = 0; // Mauvaise initialisation !
    for (const nombre of nombres) {
        if (nombre < minimum) {
            minimum = nombre;
        }
    }
    return minimum;
}

trouverMinimum([5, 3, 8, 1]); // Retourne 0 au lieu de 1 !
```
{% endtab %}

{% tab title="✅ Solution" %}
```javascript
// Initialiser avec une valeur du problème
function trouverMinimum(nombres) {
    let minimum = nombres[0]; // Commencer avec le premier élément
    for (let i = 1; i < nombres.length; i++) {
        if (nombres[i] < minimum) {
            minimum = nombres[i];
        }
    }
    return minimum;
}

trouverMinimum([5, 3, 8, 1]); // Retourne 1 ✅
```
{% endtab %}
{% endtabs %}

## 🔗 Liens avec d'autres concepts

### Concepts utilisés
{% content-ref url="variables-declarations.md" %}
[variables-declarations.md](variables-declarations.md)
{% endcontent-ref %}

{% content-ref url="types-donnees.md" %}
[types-donnees.md](types-donnees.md)
{% endcontent-ref %}

{% content-ref url="structures-controle.md" %}
[structures-controle.md](structures-controle.md)
{% endcontent-ref %}

{% content-ref url="operateurs.md" %}
[operateurs.md](operateurs.md)
{% endcontent-ref %}

### Prochaines étapes
{% content-ref url="../../niveau-intermediaire/fonctions-avancees.md" %}
[fonctions-avancees.md](../../niveau-intermediaire/fonctions-avancees.md)
{% endcontent-ref %}

{% content-ref url="../../niveau-intermediaire/arrays.md" %}
[arrays.md](../../niveau-intermediaire/arrays.md)
{% endcontent-ref %}

## 🎯 Résumé

{% hint style="success" %}
**Points clés à retenir :**

🧩 **Un algorithme = une recette** pour résoudre un problème  
📋 **Méthodologie** : Comprendre → Décomposer → Coder → Tester  
🔄 **Types courants** : Recherche, calcul, validation, transformation  
⚠️ **Attention aux pièges** : Boucles infinies, mauvaise initialisation  
🎯 **Combiner tous les concepts** : Variables, conditions, boucles ensemble  
{% endhint %}

## 🚀 Prochaines étapes

{% hint style="info" %}
**Félicitations ! Vous maîtrisez maintenant les bases de JavaScript.**

👉 **Passez au niveau intermédiaire** avec les [Fonctions Avancées](../../niveau-intermediaire/fonctions-avancees.md)  
👉 **Découvrez** les [Arrays et leurs méthodes](../../niveau-intermediaire/arrays.md)  
👉 **Apprenez** la [Programmation Orientée Objet](../../niveau-intermediaire/classes-poo.md)  
{% endhint %}

---

{% hint style="warning" %}
**Vous avez terminé le niveau débutant !** 🎉 Bravo ! Vous êtes prêt(e) pour des défis plus complexes au [niveau intermédiaire](../../niveau-intermediaire/).
{% endhint %}
