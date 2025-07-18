# ⚙️ Opérateurs en JavaScript

{% hint style="info" %}
**Niveau :** 🟢 Débutant | **Durée :** 90 min | **Prérequis :** Variables, Types, Conversion de types
{% endhint %}

## 🎯 Objectifs d'apprentissage

{% hint style="success" %}
À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** les différents types d'opérateurs et leur utilité
- [ ] **Appliquer** les opérateurs pour faire des calculs et comparaisons
- [ ] **Analyser** la priorité des opérateurs dans les expressions
- [ ] **Créer** des expressions complexes avec plusieurs opérateurs
{% endhint %}

## 🔑 Concepts clés

{% tabs %}
{% tab title="Types d'opérateurs" %}
**Arithmétiques** : `+`, `-`, `*`, `/`, `%`, `**` (calculs)  
**Comparaison** : `==`, `===`, `!=`, `>`, `<` (comparer)  
**Logiques** : `&&`, `||`, `!` (combiner conditions)  
**Assignation** : `=`, `+=`, `-=`, `*=` (assigner valeurs)  
**Incrémentation** : `++`, `--` (ajouter/enlever 1)  
{% endtab %}

{% tab title="Concepts importants" %}
**Opérateur** : Symbole qui effectue une opération  
**Opérande** : Valeur sur laquelle agit un opérateur  
**Expression** : Combinaison de valeurs et d'opérateurs  
**Précédence** : Ordre de calcul (comme en maths)  
{% endtab %}
{% endtabs %}

## 💡 Pourquoi ce concept est-il important ?

{% hint style="warning" %}
**Les opérateurs sont comme les outils d'un artisan :**

🔨 **Opérateurs arithmétiques :** Pour calculer des prix, âges, scores  
⚖️ **Opérateurs de comparaison :** Pour vérifier des conditions  
🧠 **Opérateurs logiques :** Pour combiner plusieurs critères  
📝 **Opérateurs d'assignation :** Pour modifier des variables rapidement  

**Sans opérateurs, votre programme ne pourrait ni calculer, ni décider, ni évoluer !**
{% endhint %}

## ➕➖ Opérateurs arithmétiques

### Les opérateurs de base

{% tabs %}
{% tab title="Addition et soustraction" %}
```javascript
// Addition (+)
let a = 10;
let b = 3;
let somme = a + b;        // 13

// Attention : + avec strings = concaténation !
let nom = "Alice";
let age = 25;
let message = nom + " a " + age + " ans";  // "Alice a 25 ans"

// Soustraction (-)
let difference = a - b;   // 7
let temperateure = 20;
temperateure = temperateure - 5;  // 15
```

**💡 Piège courant :** `"5" + 3 = "53"` (pas 8 !)
{% endtab %}

{% tab title="Multiplication et division" %}
```javascript
// Multiplication (*)
let longueur = 8;
let largeur = 6;
let surface = longueur * largeur;    // 48

// Division (/)
let total = 100;
let nombre = 4;
let moyenne = total / nombre;        // 25

// Division avec résultat décimal
let a = 10;
let b = 3;
let resultat = a / b;               // 3.3333...

// Arrondir le résultat
let arrondi = Math.round(resultat);  // 3
let deuxDecimales = resultat.toFixed(2);  // "3.33"
```
{% endtab %}

{% tab title="Modulo et puissance" %}
```javascript
// Modulo (%) - Le reste d'une division
let nombre = 17;
let reste = nombre % 5;             // 2 (17 = 3×5 + 2)

// Utilité du modulo : détecter les nombres pairs/impairs
function estPair(n) {
    return n % 2 === 0;
}

console.log(estPair(4));  // true
console.log(estPair(7));  // false

// Puissance (**)
let base = 2;
let exposant = 3;
let puissance = base ** exposant;   // 8 (2³)

// Racine carrée avec puissance
let nombre = 16;
let racine = nombre ** 0.5;         // 4 (√16)
```

**💡 Applications pratiques :**
- Modulo : Heures (24h), pagination, cycles
- Puissance : Aires, volumes, croissance exponentielle
{% endtab %}
{% endtabs %}

### Exemples pratiques d'arithmétique

{% tabs %}
{% tab title="Calculateur d'âge" %}
```javascript
function calculerAge(ageAnnees) {
    console.log(`=== Calculateur d'âge pour ${ageAnnees} ans ===`);
    
    // Conversions avec opérateurs arithmétiques
    const jours = ageAnnees * 365;
    const heures = jours * 24;
    const minutes = heures * 60;
    const secondes = minutes * 60;
    
    // Affichage des résultats
    console.log(`📅 En jours : ${jours.toLocaleString()}`);
    console.log(`⏰ En heures : ${heures.toLocaleString()}`);
    console.log(`⏱️ En minutes : ${minutes.toLocaleString()}`);
    console.log(`⏲️ En secondes : ${secondes.toLocaleString()}`);
    
    // Calculs intéressants
    const millionSecondes = 1_000_000;
    if (secondes > millionSecondes) {
        console.log("🎉 Vous avez vécu plus d'un million de secondes !");
    } else {
        const reste = millionSecondes - secondes;
        console.log(`⏳ Il vous reste ${reste.toLocaleString()} secondes avant le million !`);
    }
    
    return { jours, heures, minutes, secondes };
}

// Test
calculerAge(16);
```
{% endtab %}

{% tab title="Calculateur de prix" %}
```javascript
function calculerPrixTotal(prixHT, tauxTVA = 20, remise = 0) {
    console.log("=== Calculateur de Prix ===");
    
    // Calcul de la remise
    const montantRemise = prixHT * (remise / 100);
    const prixApresRemise = prixHT - montantRemise;
    
    // Calcul de la TVA
    const montantTVA = prixApresRemise * (tauxTVA / 100);
    const prixTTC = prixApresRemise + montantTVA;
    
    // Affichage détaillé
    console.log(`💰 Prix HT : ${prixHT.toFixed(2)}€`);
    
    if (remise > 0) {
        console.log(`🎫 Remise (${remise}%) : -${montantRemise.toFixed(2)}€`);
        console.log(`💸 Prix après remise : ${prixApresRemise.toFixed(2)}€`);
    }
    
    console.log(`📊 TVA (${tauxTVA}%) : ${montantTVA.toFixed(2)}€`);
    console.log(`🛒 Prix TTC : ${prixTTC.toFixed(2)}€`);
    
    return {
        prixHT,
        remise: montantRemise,
        tva: montantTVA,
        prixTTC
    };
}

// Tests
calculerPrixTotal(100);           // Prix simple
calculerPrixTotal(150, 20, 10);   // Avec remise
```
{% endtab %}
{% endtabs %}

## ⚖️ Opérateurs de comparaison

### Égalité et différence

{% hint style="warning" %}
**Différence cruciale entre == et === :**
- `==` : Égalité avec conversion de type (peut surprendre)
- `===` : Égalité stricte sans conversion (recommandée)
{% endhint %}

{% tabs %}
{% tab title="== vs === en détail" %}
```javascript
// Égalité simple (==) - Avec conversion automatique
console.log("=== Égalité simple (==) ===");
console.log("5" == 5);          // true ⚠️ (string → number)
console.log(true == 1);         // true ⚠️ (boolean → number)
console.log("" == 0);           // true ⚠️ (string vide → 0)
console.log(null == undefined); // true ⚠️ (cas spécial)

// Égalité stricte (===) - Sans conversion
console.log("=== Égalité stricte (===) ===");
console.log("5" === 5);         // false ✅ (types différents)
console.log(true === 1);        // false ✅ (types différents)
console.log("" === 0);          // false ✅ (types différents)
console.log(null === undefined);// false ✅ (types différents)

// Cas où les deux donnent le même résultat
console.log(5 == 5);            // true
console.log(5 === 5);           // true ✅ (même type et valeur)
```

**💡 Recommandation :** Utilisez toujours `===` et `!==` pour éviter les surprises !
{% endtab %}

{% tab title="Inégalité" %}
```javascript
// Différence simple (!=) et stricte (!==)
const age = 18;
const ageTexte = "18";

// Différence simple (avec conversion)
console.log(age != "20");       // true
console.log(age != ageTexte);   // false ⚠️ ("18" converti en 18)

// Différence stricte (sans conversion)
console.log(age !== "20");      // true
console.log(age !== ageTexte);  // true ✅ (number vs string)

// Utilisation pratique
function verifierAge(ageInput) {
    if (ageInput === "") {
        return "Âge requis";
    }
    
    if (ageInput !== String(Number(ageInput))) {
        return "Âge invalide";
    }
    
    const age = Number(ageInput);
    if (age < 0 || age > 120) {
        return "Âge non réaliste";
    }
    
    return "Âge valide";
}

console.log(verifierAge("25"));   // "Âge valide"
console.log(verifierAge("abc"));  // "Âge invalide"
```
{% endtab %}
{% endtabs %}

### Comparaisons numériques

{% tabs %}
{% tab title="Supérieur et inférieur" %}
```javascript
// Opérateurs de comparaison numérique
const noteEtudiant = 15;
const notePassing = 10;
const noteExcellente = 16;

// Supérieur (>)
const aReussi = noteEtudiant > notePassing;       // true
console.log(`A réussi : ${aReussi}`);

// Supérieur ou égal (>=)
const estExcellent = noteEtudiant >= noteExcellente; // false
console.log(`Est excellent : ${estExcellent}`);

// Inférieur (<)
const doitRepasser = noteEtudiant < notePassing;   // false
console.log(`Doit repasser : ${doitRepasser}`);

// Inférieur ou égal (<=)
const peutAmeliorer = noteEtudiant <= noteExcellente; // true
console.log(`Peut améliorer : ${peutAmeliorer}`);

// Fonction de notation complète
function evaluerNote(note) {
    if (note >= 16) return "🏆 Excellent";
    if (note >= 14) return "😊 Bien";
    if (note >= 12) return "🙂 Assez bien";
    if (note >= 10) return "😐 Passable";
    return "😞 Insuffisant";
}

console.log(evaluerNote(15)); // "😊 Bien"
```
{% endtab %}

{% tab title="Comparaisons avec strings" %}
```javascript
// Les strings se comparent par ordre alphabétique
console.log("=== Comparaison de strings ===");
console.log("Alice" < "Bob");         // true (A avant B)
console.log("alice" > "Alice");       // true (minuscule > majuscule)
console.log("10" < "9");              // true ⚠️ (ordre alphabétique !)

// Piège avec les nombres en string
const nums = ["10", "2", "30", "4"];
nums.sort(); // Tri alphabétique par défaut
console.log(nums); // ["10", "2", "30", "4"] ⚠️

// Solution : conversion ou comparateur
nums.sort((a, b) => Number(a) - Number(b));
console.log(nums); // ["2", "4", "10", "30"] ✅

// Comparaison de dates
const date1 = new Date("2023-01-01");
const date2 = new Date("2023-12-31");
console.log(date1 < date2);           // true
```
{% endtab %}
{% endtabs %}

## 🔀 Opérateurs d'assignation

### Assignation simple et composée

{% tabs %}
{% tab title="Assignation de base" %}
```javascript
// Assignation simple (=)
let score = 100;
let nom = "Alice";
let estActif = true;

// Réassignation
score = 150;        // Nouvelle valeur
nom = "Bob";        // Changement de nom

// Assignation multiple (destructuring)
let a, b, c;
a = b = c = 0;      // Tous à zéro

// Échange de variables (moderne)
let x = 10;
let y = 20;
[x, y] = [y, x];    // x=20, y=10
console.log(`x=${x}, y=${y}`);
```
{% endtab %}

{% tab title="Opérateurs d'assignation composée" %}
```javascript
// Avant : notation longue
let points = 100;
points = points + 50;     // 150
points = points - 20;     // 130
points = points * 2;      // 260
points = points / 4;      // 65

// Après : notation raccourcie
let pointsRaccourci = 100;
pointsRaccourci += 50;    // Équivaut à : pointsRaccourci = pointsRaccourci + 50
pointsRaccourci -= 20;    // Équivaut à : pointsRaccourci = pointsRaccourci - 20
pointsRaccourci *= 2;     // Équivaut à : pointsRaccourci = pointsRaccourci * 2
pointsRaccourci /= 4;     // Équivaut à : pointsRaccourci = pointsRaccourci / 4

console.log(pointsRaccourci); // 65

// Autres opérateurs d'assignation
let nombre = 17;
nombre %= 5;              // nombre = nombre % 5 → 2
nombre **= 3;             // nombre = nombre ** 3 → 8

// Avec des strings
let message = "Bonjour";
message += " le monde";   // "Bonjour le monde"
message += "!";           // "Bonjour le monde!"
```
{% endtab %}

{% tab title="Incrémentation et décrémentation" %}
```javascript
// Incrémentation (++)
let compteur = 5;

// Post-incrémentation (utilise la valeur, puis incrémente)
console.log(compteur++);  // Affiche 5, puis compteur devient 6
console.log(compteur);    // 6

// Pré-incrémentation (incrémente, puis utilise la valeur)
console.log(++compteur);  // compteur devient 7, puis affiche 7

// Décrémentation (--)
let vies = 3;
console.log(vies--);      // Affiche 3, puis vies devient 2
console.log(--vies);      // vies devient 1, puis affiche 1

// Utilisation pratique dans les boucles
for (let i = 0; i < 5; i++) {
    console.log(`Tour ${i + 1}`);
}

// Gestion d'un compteur de vies dans un jeu
function utiliserVie() {
    if (vies > 0) {
        vies--;
        console.log(`Vie perdue ! Il reste ${vies} vies`);
        return true;
    } else {
        console.log("Game Over !");
        return false;
    }
}
```
{% endtab %}
{% endtabs %}

## 📊 Priorité des opérateurs

{% hint style="warning" %}
**Ordre de priorité (du plus prioritaire au moins prioritaire) :**
1. `()` Parenthèses
2. `**` Puissance  
3. `*`, `/`, `%` Multiplication, division, modulo
4. `+`, `-` Addition, soustraction
5. `<`, `>`, `<=`, `>=` Comparaisons
6. `==`, `===`, `!=`, `!==` Égalité
7. `&&` ET logique
8. `||` OU logique
9. `=`, `+=`, `-=`, etc. Assignation
{% endhint %}

{% tabs %}
{% tab title="Exemples de priorité" %}
```javascript
// Sans parenthèses - priorité naturelle
let resultat1 = 10 + 5 * 2;     // 20 (multiplication avant addition)
let resultat2 = 20 / 4 + 3;     // 8 (division avant addition)
let resultat3 = 2 ** 3 * 4;     // 32 (puissance avant multiplication)

console.log("Sans parenthèses :");
console.log(`10 + 5 * 2 = ${resultat1}`);      // 20
console.log(`20 / 4 + 3 = ${resultat2}`);      // 8
console.log(`2 ** 3 * 4 = ${resultat3}`);      // 32

// Avec parenthèses - force l'ordre
let resultat4 = (10 + 5) * 2;   // 30 (addition forcée en premier)
let resultat5 = 20 / (4 + 3);   // 2.857... (addition forcée en premier)
let resultat6 = (2 ** 3) * 4;   // 32 (même résultat, mais plus clair)

console.log("\nAvec parenthèses :");
console.log(`(10 + 5) * 2 = ${resultat4}`);    // 30
console.log(`20 / (4 + 3) = ${resultat5.toFixed(2)}`); // 2.86
```
{% endtab %}

{% tab title="Expressions complexes" %}
```javascript
// Expression complexe avec plusieurs opérateurs
const a = 5;
const b = 10;
const c = 2;

// Sans parenthèses - difficile à lire
const resultat1 = a + b * c ** 2 > a * b && a < b || c === 2;

// Avec parenthèses - beaucoup plus clair
const resultat2 = (a + (b * (c ** 2))) > (a * b) && (a < b) || (c === 2);

// Découpé en étapes - le plus lisible
const puissance = c ** 2;           // 4
const multiplication = b * puissance; // 40
const addition = a + multiplication;  // 45
const produit = a * b;              // 50

const condition1 = addition > produit;  // 45 > 50 = false
const condition2 = a < b;               // 5 < 10 = true
const condition3 = c === 2;             // 2 === 2 = true

const resultatFinal = (condition1 && condition2) || condition3;
// false && true = false
// false || true = true

console.log("Résultat final :", resultatFinal); // true
```
{% endtab %}

{% tab title="Calculateur avec priorité" %}
```javascript
function calculerFormule(x, y, z) {
    console.log(`=== Calcul pour x=${x}, y=${y}, z=${z} ===`);
    
    // Formule complexe : (x + y) * z - x ** 2 / y
    const etape1 = x + y;           // Parenthèses d'abord
    const etape2 = x ** 2;          // Puissance
    const etape3 = etape2 / y;      // Division
    const etape4 = etape1 * z;      // Multiplication
    const resultat = etape4 - etape3; // Soustraction
    
    console.log(`1. (${x} + ${y}) = ${etape1}`);
    console.log(`2. ${x} ** 2 = ${etape2}`);
    console.log(`3. ${etape2} / ${y} = ${etape3}`);
    console.log(`4. ${etape1} * ${z} = ${etape4}`);
    console.log(`5. ${etape4} - ${etape3} = ${resultat}`);
    
    // Version directe (même résultat)
    const direct = (x + y) * z - x ** 2 / y;
    console.log(`Direct: (${x} + ${y}) * ${z} - ${x} ** 2 / ${y} = ${direct}`);
    
    return resultat;
}

calculerFormule(3, 4, 2); // Test avec des valeurs
```
{% endtab %}
{% endtabs %}

## 💻 Exemples pratiques avancés

### Système de scoring pour un jeu

{% tabs %}
{% tab title="Code complet" %}
```javascript
class SystemeScore {
    constructor(joueur) {
        this.joueur = joueur;
        this.score = 0;
        this.niveau = 1;
        this.vies = 3;
        this.multiplicateur = 1;
        this.combos = 0;
    }
    
    ajouterPoints(points, typeAction = "normal") {
        console.log(`\n🎮 ${this.joueur} - Action: ${typeAction}`);
        
        // Calculs avec différents opérateurs
        let pointsBase = points;
        let bonus = 0;
        
        // Bonus selon le type d'action
        switch (typeAction) {
            case "parfait":
                bonus = pointsBase * 0.5;  // 50% de bonus
                this.combos++;
                break;
            case "rapide":
                bonus = pointsBase * 0.2;  // 20% de bonus
                break;
            case "critique":
                bonus = pointsBase * 1.0;  // 100% de bonus
                this.combos += 2;
                break;
        }
        
        // Multiplicateur de combo
        if (this.combos >= 5) {
            this.multiplicateur = 2.0;
            console.log("🔥 MULTIPLICATEUR x2 ACTIVÉ !");
        } else if (this.combos >= 3) {
            this.multiplicateur = 1.5;
        }
        
        // Calcul final
        const pointsAvecBonus = pointsBase + bonus;
        const pointsFinaux = Math.round(pointsAvecBonus * this.multiplicateur);
        
        // Assignation composée
        this.score += pointsFinaux;
        
        console.log(`📊 Points de base: ${pointsBase}`);
        if (bonus > 0) {
            console.log(`⭐ Bonus ${typeAction}: +${bonus.toFixed(1)}`);
        }
        if (this.multiplicateur > 1) {
            console.log(`🔥 Multiplicateur: x${this.multiplicateur}`);
        }
        console.log(`✅ Points gagnés: ${pointsFinaux}`);
        console.log(`🏆 Score total: ${this.score}`);
        
        // Vérifier montée de niveau
        this.verifierNiveau();
    }
    
    perdreVie() {
        this.vies--;
        this.combos = 0;  // Reset combo
        this.multiplicateur = 1;  // Reset multiplicateur
        
        console.log(`💔 Vie perdue ! Vies restantes: ${this.vies}`);
        
        if (this.vies <= 0) {
            console.log("💀 GAME OVER !");
            return false;
        }
        return true;
    }
    
    verifierNiveau() {
        // Formule de niveau : score / (niveau * 1000)
        const seuilNiveau = this.niveau * 1000;
        
        if (this.score >= seuilNiveau) {
            this.niveau++;
            this.vies++;  // Vie bonus
            console.log(`🎊 NIVEAU ${this.niveau} ATTEINT !`);
            console.log(`❤️ Vie bonus ! Total: ${this.vies} vies`);
        }
    }
    
    obtenirStatistiques() {
        // Calculs statistiques avec opérateurs
        const moyenneParNiveau = Math.round(this.score / this.niveau);
        const efficacite = this.vies > 0 ? 
            Math.round((this.score / (4 - this.vies)) * 100) / 100 : 0;
        
        return {
            joueur: this.joueur,
            score: this.score,
            niveau: this.niveau,
            vies: this.vies,
            combos: this.combos,
            multiplicateur: this.multiplicateur,
            moyenneParNiveau,
            efficacite
        };
    }
}

// Simulation d'une partie
const jeu = new SystemeScore("Alice");

jeu.ajouterPoints(100, "normal");
jeu.ajouterPoints(150, "parfait");
jeu.ajouterPoints(200, "critique");
jeu.ajouterPoints(80, "rapide");
jeu.perdreVie();
jeu.ajouterPoints(300, "parfait");
jeu.ajouterPoints(500, "critique");

console.log("\n📈 STATISTIQUES FINALES:");
console.table(jeu.obtenirStatistiques());
```
{% endtab %}

{% tab title="Résultat de simulation" %}
```
🎮 Alice - Action: normal
📊 Points de base: 100
✅ Points gagnés: 100
🏆 Score total: 100

🎮 Alice - Action: parfait
📊 Points de base: 150
⭐ Bonus parfait: +75.0
✅ Points gagnés: 225
🏆 Score total: 325

🎮 Alice - Action: critique
📊 Points de base: 200
⭐ Bonus critique: +200.0
🔥 MULTIPLICATEUR x1.5 ACTIVÉ !
🔥 Multiplicateur: x1.5
✅ Points gagnés: 600
🏆 Score total: 925

🎮 Alice - Action: rapide
📊 Points de base: 80
⭐ Bonus rapide: +16.0
🔥 MULTIPLICATEUR x2 ACTIVÉ !
🔥 Multiplicateur: x2
✅ Points gagnés: 192
🏆 Score total: 1117
🎊 NIVEAU 2 ATTEINT !
❤️ Vie bonus ! Total: 4 vies

💔 Vie perdue ! Vies restantes: 3

🎮 Alice - Action: parfait
📊 Points de base: 300
⭐ Bonus parfait: +150.0
✅ Points gagnés: 450
🏆 Score total: 1567

🎮 Alice - Action: critique
📊 Points de base: 500
⭐ Bonus critique: +500.0
✅ Points gagnés: 1000
🏆 Score total: 2567
🎊 NIVEAU 3 ATTEINT !
❤️ Vie bonus ! Total: 4 vies
```
{% endtab %}
{% endtabs %}

### Calculateur financier

{% tabs %}
{% tab title="Prêt immobilier" %}
```javascript
function calculerPret(capital, tauxAnnuel, dureeAnnees) {
    console.log("🏠 === CALCULATEUR DE PRÊT IMMOBILIER ===");
    console.log(`💰 Capital emprunté: ${capital.toLocaleString()}€`);
    console.log(`📊 Taux annuel: ${tauxAnnuel}%`);
    console.log(`📅 Durée: ${dureeAnnees} ans`);
    
    // Conversion des paramètres
    const tauxMensuel = tauxAnnuel / 100 / 12;
    const nombreMois = dureeAnnees * 12;
    
    // Formule de calcul de mensualité
    // M = C × (t × (1 + t)^n) / ((1 + t)^n - 1)
    const puissance = (1 + tauxMensuel) ** nombreMois;
    const mensualite = capital * (tauxMensuel * puissance) / (puissance - 1);
    
    // Calculs dérivés
    const coutTotal = mensualite * nombreMois;
    const interetsTotal = coutTotal - capital;
    const tauxEndettement = (mensualite / 3000) * 100; // Supposons salaire 3000€
    
    console.log("\n💡 RÉSULTATS:");
    console.log(`💳 Mensualité: ${mensualite.toFixed(2)}€`);
    console.log(`💸 Coût total: ${coutTotal.toFixed(2)}€`);
    console.log(`📈 Intérêts totaux: ${interetsTotal.toFixed(2)}€`);
    console.log(`⚖️ Taux d'endettement: ${tauxEndettement.toFixed(1)}%`);
    
    // Conseils automatiques avec opérateurs de comparaison
    if (tauxEndettement > 33) {
        console.log("⚠️ Taux d'endettement trop élevé (>33%)");
    } else if (tauxEndettement > 25) {
        console.log("⚡ Taux d'endettement élevé mais acceptable");
    } else {
        console.log("✅ Taux d'endettement confortable");
    }
    
    return {
        mensualite: Math.round(mensualite * 100) / 100,
        coutTotal: Math.round(coutTotal * 100) / 100,
        interetsTotal: Math.round(interetsTotal * 100) / 100,
        tauxEndettement: Math.round(tauxEndettement * 10) / 10
    };
}

// Tests avec différents scénarios
calculerPret(200000, 2.5, 20);   // Prêt classique
calculerPret(300000, 1.8, 25);   // Prêt long terme
```
{% endtab %}

{% tab title="Épargne et intérêts composés" %}
```javascript
function calculerEpargne(capitalInitial, versementMensuel, tauxAnnuel, dureeAnnees) {
    console.log("💰 === SIMULATEUR D'ÉPARGNE ===");
    
    const tauxMensuel = tauxAnnuel / 100 / 12;
    const nombreMois = dureeAnnees * 12;
    
    let capital = capitalInitial;
    let totalVerse = capitalInitial;
    let historique = [];
    
    console.log(`🏦 Capital initial: ${capitalInitial.toLocaleString()}€`);
    console.log(`📊 Versement mensuel: ${versementMensuel.toLocaleString()}€`);
    console.log(`📈 Taux annuel: ${tauxAnnuel}%`);
    console.log(`⏰ Durée: ${dureeAnnees} ans\n`);
    
    // Simulation mois par mois
    for (let mois = 1; mois <= nombreMois; mois++) {
        // Intérêts sur le capital existant
        const interetsMois = capital * tauxMensuel;
        
        // Mise à jour du capital avec opérateurs d'assignation
        capital += interetsMois;        // Ajout des intérêts
        capital += versementMensuel;    // Ajout du versement
        totalVerse += versementMensuel; // Cumul des versements
        
        // Historique annuel
        if (mois % 12 === 0) {
            const annee = mois / 12;
            const gainTotal = capital - totalVerse;
            const rendement = (gainTotal / totalVerse) * 100;
            
            historique.push({
                annee,
                capital: Math.round(capital),
                totalVerse: Math.round(totalVerse),
                gains: Math.round(gainTotal),
                rendement: Math.round(rendement * 100) / 100
            });
        }
    }
    
    // Résultats finaux
    const gainFinal = capital - totalVerse;
    const rendementGlobal = (gainFinal / totalVerse) * 100;
    
    console.log("📊 ÉVOLUTION ANNUELLE:");
    console.table(historique);
    
    console.log("\n🎯 BILAN FINAL:");
    console.log(`💎 Capital final: ${Math.round(capital).toLocaleString()}€`);
    console.log(`💸 Total versé: ${Math.round(totalVerse).toLocaleString()}€`);
    console.log(`📈 Gains totaux: ${Math.round(gainFinal).toLocaleString()}€`);
    console.log(`⚡ Rendement: ${rendementGlobal.toFixed(2)}%`);
    
    return {
        capitalFinal: Math.round(capital),
        totalVerse: Math.round(totalVerse),
        gains: Math.round(gainFinal),
        rendement: Math.round(rendementGlobal * 100) / 100,
        historique
    };
}

// Simulation d'épargne
calculerEpargne(10000, 300, 3, 10);
```
{% endtab %}
{% endtabs %}

## 🎮 Quiz interactif

{% hint style="info" %}
**Question 1 :** Que donne `10 + 5 * 2` ?
A) 30  B) 20  C) 25  D) 40
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Réponse : B) 20**

La multiplication a priorité sur l'addition :
`10 + 5 * 2 = 10 + (5 * 2) = 10 + 10 = 20`

Pour obtenir 30, il faudrait : `(10 + 5) * 2`

</details>

{% hint style="info" %}
**Question 2 :** Quelle est la différence entre `x++` et `++x` ?
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Différence dans l'ordre d'exécution :**

- `x++` (post-incrémentation) : Utilise la valeur actuelle, PUIS incrémente
- `++x` (pré-incrémentation) : Incrémente D'ABORD, puis utilise la nouvelle valeur

```javascript
let a = 5;
console.log(a++); // Affiche 5, puis a devient 6
console.log(a);   // Affiche 6

let b = 5;
console.log(++b); // b devient 6, puis affiche 6
console.log(b);   // Affiche 6
```

</details>

{% hint style="info" %}
**Question 3 :** Pourquoi utiliser `===` plutôt que `==` ?
{% endhint %}

<details>
<summary>💡 Voir la réponse</summary>

**Pour éviter les conversions automatiques surprenantes :**

```javascript
// Problèmes avec ==
console.log("5" == 5);        // true (surprise !)
console.log("" == 0);         // true (surprise !)
console.log(true == 1);       // true (surprise !)

// Solutions avec ===
console.log("5" === 5);       // false (logique)
console.log("" === 0);        // false (logique)
console.log(true === 1);      // false (logique)
```

`===` compare type ET valeur, `==` convertit automatiquement les types.

</details>

## 💻 Exercices pratiques

### Exercice 1 : Calculateur d'IMC avancé

{% tabs %}
{% tab title="Consigne" %}
Créez un calculateur d'IMC (Indice de Masse Corporelle) qui :

**Fonctionnalités :**
- Calcule l'IMC : poids / (taille²)
- Détermine la catégorie (sous-poids, normal, surpoids, obésité)
- Calcule le poids idéal
- Donne des conseils personnalisés

**Formules :**
- IMC = poids (kg) / taille² (m)
- Poids idéal ≈ 22 × taille²
{% endtab %}

{% tab title="Solution" %}
```javascript
function calculerIMC(poids, tailleCm, age = null, sexe = null) {
    console.log("⚖️ === CALCULATEUR D'IMC AVANCÉ ===");
    
    // Conversion et validation
    const taille = tailleCm / 100; // cm → m
    
    if (poids <= 0 || taille <= 0) {
        return { erreur: "Poids et taille doivent être positifs" };
    }
    
    // Calcul de l'IMC
    const imc = poids / (taille ** 2);
    
    // Détermination de la catégorie
    let categorie, conseils;
    
    if (imc < 16.5) {
        categorie = "Dénutrition";
        conseils = "⚠️ Consultez rapidement un médecin";
    } else if (imc < 18.5) {
        categorie = "Sous-poids";
        conseils = "💪 Augmentez votre apport calorique sainement";
    } else if (imc < 25) {
        categorie = "Poids normal";
        conseils = "✅ Maintenez vos bonnes habitudes !";
    } else if (imc < 30) {
        categorie = "Surpoids";
        conseils = "🏃 Activité physique et alimentation équilibrée";
    } else if (imc < 35) {
        categorie = "Obésité modérée";
        conseils = "🏥 Consultez un professionnel de santé";
    } else {
        categorie = "Obésité sévère";
        conseils = "🚨 Suivi médical urgent recommandé";
    }
    
    // Calculs supplémentaires
    const poidsIdeal = 22 * (taille ** 2);
    const differencePoidsIdeal = poids - poidsIdeal;
    
    // Ajustements selon l'âge et le sexe
    let imcAjuste = imc;
    if (age !== null && age > 65) {
        imcAjuste = imc * 0.95; // Tolérance pour les seniors
    }
    if (sexe === "homme") {
        imcAjuste = imc * 1.02; // Légère tolérance pour les hommes
    }
    
    // Calcul des calories pour atteindre le poids idéal
    const deficitCaloriqueJour = Math.abs(differencePoidsIdeal) * 7700 / 365;
    
    console.log(`👤 Profil: ${poids}kg, ${tailleCm}cm`);
    if (age) console.log(`🎂 Âge: ${age} ans`);
    if (sexe) console.log(`⚧ Sexe: ${sexe}`);
    
    console.log(`\n📊 IMC: ${imc.toFixed(1)}`);
    console.log(`📋 Catégorie: ${categorie}`);
    console.log(`💡 ${conseils}`);
    
    console.log(`\n🎯 Poids idéal: ${poidsIdeal.toFixed(1)}kg`);
    
    if (Math.abs(differencePoidsIdeal) > 1) {
        const action = differencePoidsIdeal > 0 ? "perdre" : "prendre";
        console.log(`⚖️ ${action} ${Math.abs(differencePoidsIdeal).toFixed(1)}kg`);
        console.log(`🔥 Déficit/surplus: ${Math.round(deficitCaloriqueJour)} cal/jour`);
    } else {
        console.log("✅ Vous êtes très proche du poids idéal !");
    }
    
    return {
        imc: Math.round(imc * 10) / 10,
        categorie,
        poidsIdeal: Math.round(poidsIdeal * 10) / 10,
        difference: Math.round(differencePoidsIdeal * 10) / 10,
        caloriesJour: Math.round(deficitCaloriqueJour),
        conseils
    };
}

// Tests
calculerIMC(70, 175, 30, "homme");
calculerIMC(55, 160, 25, "femme");
calculerIMC(90, 180, 40);
```
{% endtab %}
{% endtabs %}

### Exercice 2 : Système de notation d'examen

{% tabs %}
{% tab title="Consigne" %}
Créez un système qui :

**Traite une liste de notes :**
- Calcule moyenne, médiane, écart-type
- Détermine les mentions
- Calcule le taux de réussite
- Identifie les notes aberrantes

**Utilisez tous les types d'opérateurs vus dans le cours**
{% endtab %}

{% tab title="Solution" %}
```javascript
function analyserNotes(notes, matieres = null) {
    console.log("📊 === ANALYSE STATISTIQUE DES NOTES ===");
    
    // Validation et nettoyage
    const notesValides = notes.filter(note => 
        typeof note === 'number' && note >= 0 && note <= 20
    );
    
    if (notesValides.length === 0) {
        return { erreur: "Aucune note valide à analyser" };
    }
    
    const nbNotes = notesValides.length;
    console.log(`📝 ${nbNotes} notes analysées`);
    
    // Calculs de base avec opérateurs arithmétiques
    let somme = 0;
    let noteMax = -Infinity;
    let noteMin = Infinity;
    
    for (let i = 0; i < notesValides.length; i++) {
        const note = notesValides[i];
        somme += note;              // Opérateur d'assignation composée
        
        if (note > noteMax) noteMax = note;  // Opérateur de comparaison
        if (note < noteMin) noteMin = note;
    }
    
    const moyenne = somme / nbNotes;
    
    // Calcul de la médiane
    const notesTriees = [...notesValides].sort((a, b) => a - b);
    let mediane;
    
    if (nbNotes % 2 === 0) {  // Nombre pair
        const mid1 = notesTriees[nbNotes / 2 - 1];
        const mid2 = notesTriees[nbNotes / 2];
        mediane = (mid1 + mid2) / 2;
    } else {  // Nombre impair
        mediane = notesTriees[Math.floor(nbNotes / 2)];
    }
    
    // Calcul de l'écart-type
    let sommeCarre = 0;
    for (const note of notesValides) {
        const ecart = note - moyenne;
        sommeCarre += ecart ** 2;   // Opérateur puissance
    }
    const ecartType = Math.sqrt(sommeCarre / nbNotes);
    
    // Analyse des mentions
    let nbTresBien = 0, nbBien = 0, nbAssezBien = 0, nbPassable = 0, nbInsuffisant = 0;
    
    notesValides.forEach(note => {
        if (note >= 16) {
            nbTresBien++;
        } else if (note >= 14) {
            nbBien++;
        } else if (note >= 12) {
            nbAssezBien++;
        } else if (note >= 10) {
            nbPassable++;
        } else {
            nbInsuffisant++;
        }
    });
    
    // Calculs de pourcentages avec opérateurs arithmétiques
    const tauxReussite = ((nbNotes - nbInsuffisant) / nbNotes) * 100;
    const tauxMention = ((nbTresBien + nbBien + nbAssezBien) / nbNotes) * 100;
    
    // Détection des notes aberrantes (> 2 écarts-types)
    const notesAberrantes = notesValides.filter(note => 
        Math.abs(note - moyenne) > 2 * ecartType
    );
    
    // Affichage des résultats
    console.log("\n📈 STATISTIQUES CENTRALES:");
    console.log(`➡️ Moyenne: ${moyenne.toFixed(2)}/20`);
    console.log(`📊 Médiane: ${mediane.toFixed(2)}/20`);
    console.log(`📏 Écart-type: ${ecartType.toFixed(2)}`);
    console.log(`⬆️ Note max: ${noteMax}/20`);
    console.log(`⬇️ Note min: ${noteMin}/20`);
    console.log(`📐 Amplitude: ${(noteMax - noteMin).toFixed(1)}`);
    
    console.log("\n🏆 RÉPARTITION DES MENTIONS:");
    console.log(`🥇 Très bien (≥16): ${nbTresBien} (${(nbTresBien/nbNotes*100).toFixed(1)}%)`);
    console.log(`🥈 Bien (14-15): ${nbBien} (${(nbBien/nbNotes*100).toFixed(1)}%)`);
    console.log(`🥉 Assez bien (12-13): ${nbAssezBien} (${(nbAssezBien/nbNotes*100).toFixed(1)}%)`);
    console.log(`✅ Passable (10-11): ${nbPassable} (${(nbPassable/nbNotes*100).toFixed(1)}%)`);
    console.log(`❌ Insuffisant (<10): ${nbInsuffisant} (${(nbInsuffisant/nbNotes*100).toFixed(1)}%)`);
    
    console.log("\n📊 INDICATEURS:");
    console.log(`✅ Taux de réussite: ${tauxReussite.toFixed(1)}%`);
    console.log(`🏆 Taux de mention: ${tauxMention.toFixed(1)}%`);
    
    if (notesAberrantes.length > 0) {
        console.log(`⚠️ Notes aberrantes: ${notesAberrantes.join(', ')}`);
    }
    
    // Appréciation globale avec opérateurs logiques
    let appreciation;
    if (moyenne >= 15 && tauxReussite >= 90) {
        appreciation = "🏆 Excellente classe !";
    } else if (moyenne >= 12 && tauxReussite >= 75) {
        appreciation = "😊 Bonne classe !";
    } else if (moyenne >= 10 && tauxReussite >= 60) {
        appreciation = "🙂 Classe correcte";
    } else if (tauxReussite >= 50) {
        appreciation = "😐 Classe en difficulté";
    } else {
        appreciation = "😟 Classe en grande difficulté";
    }
    
    console.log(`\n${appreciation}`);
    
    // Analyse par matière si fournie
    if (matieres && matieres.length === notesValides.length) {
        console.log("\n📚 ANALYSE PAR MATIÈRE:");
        
        const analyseMatiere = {};
        for (let i = 0; i < notesValides.length; i++) {
            const matiere = matieres[i];
            if (!analyseMatiere[matiere]) {
                analyseMatiere[matiere] = [];
            }
            analyseMatiere[matiere].push(notesValides[i]);
        }
        
        Object.entries(analyseMatiere).forEach(([matiere, notesMatiere]) => {
            const moyMatiere = notesMatiere.reduce((sum, note) => sum + note, 0) / notesMatiere.length;
            console.log(`${matiere}: ${moyMatiere.toFixed(1)}/20 (${notesMatiere.length} notes)`);
        });
    }
    
    return {
        moyenne: Math.round(moyenne * 100) / 100,
        mediane: Math.round(mediane * 100) / 100,
        ecartType: Math.round(ecartType * 100) / 100,
        noteMax,
        noteMin,
        tauxReussite: Math.round(tauxReussite * 10) / 10,
        tauxMention: Math.round(tauxMention * 10) / 10,
        repartition: { nbTresBien, nbBien, nbAssezBien, nbPassable, nbInsuffisant },
        notesAberrantes,
        appreciation
    };
}

// Test avec des données
const notesClasse = [15, 12, 18, 8, 16, 14, 6, 17, 11, 13, 19, 9, 15, 12, 20];
const matieres = ["Math", "Fr", "Hist", "Math", "Angl", "SVT", "Fr", "Phys", "Math", "Angl", "Hist", "SVT", "Phys", "Fr", "Math"];

analyserNotes(notesClasse, matieres);
```
{% endtab %}
{% endtabs %}

## ⚠️ Pièges courants et solutions

### Piège 1 : Conversion automatique avec +

{% tabs %}
{% tab title="❌ Problème fréquent" %}
```javascript
// Entrées utilisateur (toujours des strings)
const age = "25";
const bonus = "5";

// Addition qui devient concaténation !
const total = age + bonus;        // "255" ⚠️
console.log("Total:", total);

// Dans une boucle
for (let i = 0; i < 3; i++) {
    console.log("Numéro " + i + 1);  // "Numéro 01", "Numéro 11", "Numéro 21" ⚠️
}
```
{% endtab %}

{% tab title="✅ Solutions" %}
```javascript
// Solution 1 : Conversion explicite
const age = "25";
const bonus = "5";
const total = Number(age) + Number(bonus);  // 30 ✅
console.log("Total:", total);

// Solution 2 : Opérateur unaire +
const total2 = +age + +bonus;               // 30 ✅

// Solution 3 : parseInt/parseFloat
const total3 = parseInt(age) + parseInt(bonus); // 30 ✅

// Dans une boucle - parenthèses pour forcer l'ordre
for (let i = 0; i < 3; i++) {
    console.log("Numéro " + (i + 1));       // "Numéro 1", "Numéro 2", "Numéro 3" ✅
    // ou template literals (moderne)
    console.log(`Numéro ${i + 1}`);         // Plus propre ✅
}
```
{% endtab %}
{% endtabs %}

### Piège 2 : Précédence des opérateurs

{% tabs %}
{% tab title="❌ Expressions ambiguës" %}
```javascript
// Code confus - priorité pas claire
const resultat1 = 10 + 5 * 2 - 3 / 1;     // Que fait ce code ?
const resultat2 = true || false && true;   // Et celui-ci ?

// Variables mal nommées rendent tout confus
const a = 10;
const b = 5;
const c = 2;
const d = a + b * c;                       // 20, pas 30 !
```
{% endtab %}

{% tab title="✅ Code lisible" %}
```javascript
// Parenthèses pour clarifier l'intention
const prix = 10;
const quantite = 5;
const supplement = 2;

const totalSansParentheses = prix + quantite * supplement;  // 20
const totalAvecParentheses = (prix + quantite) * supplement; // 30

// Variables explicites et étapes intermédiaires
const prixUnitaire = 10;
const nombreArticles = 5;
const fraisLivraison = 2;

const sousTotal = prixUnitaire + (nombreArticles * fraisLivraison);
// ou, plus clair :
const fraisTotal = nombreArticles * fraisLivraison;
const prixFinal = prixUnitaire + fraisTotal;

// Conditions complexes avec parenthèses
const age = 25;
const permis = true;
const assurance = true;

// Au lieu de : age >= 18 && permis || assurance
const majeur = age >= 18;
const peutConduire = (majeur && permis) || assurance;
```
{% endtab %}
{% endtabs %}

### Piège 3 : Égalité et types

{% tabs %}
{% tab title="❌ Comparaisons surprenantes" %}
```javascript
// Comparaisons avec == qui surprennent
console.log("" == 0);           // true ⚠️
console.log(" " == 0);          // true ⚠️
console.log("0" == false);      // true ⚠️
console.log(null == undefined); // true ⚠️

// Dans des conditions
const input = "0";
if (input == false) {           // true ⚠️
    console.log("Condition vraie !");
}

// Avec des arrays
const arr1 = [1, 2, 3];
const arr2 = [1, 2, 3];
console.log(arr1 == arr2);      // false ⚠️ (références différentes)
```
{% endtab %}

{% tab title="✅ Comparaisons sûres" %}
```javascript
// Utilisez === pour des comparaisons strictes
console.log("" === 0);          // false ✅
console.log(" " === 0);         // false ✅
console.log("0" === false);     // false ✅
console.log(null === undefined);// false ✅

// Validation explicite des types
function validerAge(input) {
    // Vérifier que c'est bien un nombre
    if (typeof input !== 'number') {
        return false;
    }
    
    // Vérifier la plage
    return input >= 0 && input <= 120;
}

// Pour comparer des arrays/objets, comparez le contenu
function arraysEgaux(arr1, arr2) {
    if (arr1.length !== arr2.length) return false;
    
    for (let i = 0; i < arr1.length; i++) {
        if (arr1[i] !== arr2[i]) return false;
    }
    
    return true;
}

// Utilisation moderne avec JSON (pour objets simples)
const obj1 = { a: 1, b: 2 };
const obj2 = { a: 1, b: 2 };
const sontEgaux = JSON.stringify(obj1) === JSON.stringify(obj2); // true ✅
```
{% endtab %}
{% endtabs %}

## 🔗 Liens avec d'autres concepts

{% content-ref url="conversion-types.md" %}
[conversion-types.md](conversion-types.md)
{% endcontent-ref %}

{% content-ref url="operateurs-logiques.md" %}
[operateurs-logiques.md](operateurs-logiques.md)
{% endcontent-ref %}

{% content-ref url="../niveau-intermediaire/fonctions.md" %}
[fonctions.md](../niveau-intermediaire/fonctions.md)
{% endcontent-ref %}

## 🚀 Projet pratique

{% hint style="success" %}
**Défi : Calculatrice scientifique complète**

**Fonctionnalités à implémenter :**
- [ ] Opérations arithmétiques de base (+, -, *, /, %)
- [ ] Opérations avancées (puissance, racine, factorielle)
- [ ] Fonctions trigonométriques (sin, cos, tan)
- [ ] Historique des calculs avec évaluation d'expressions
- [ ] Interface utilisateur intuitive
- [ ] Gestion d'erreurs complète

**Technologies :** HTML, CSS, JavaScript  
**Temps estimé :** 3-4 heures
{% endhint %}

## 🎯 Résumé

{% hint style="info" %}
**Points clés à retenir :**

🧮 **Arithmétiques :** `+`, `-`, `*`, `/`, `%`, `**` pour tous les calculs  
⚖️ **Comparaison :** Utilisez `===` et `!==` pour éviter les surprises  
🔗 **Logiques :** `&&`, `||`, `!` pour combiner des conditions  
📝 **Assignation :** `+=`, `-=`, `*=` pour du code plus concis  
📊 **Priorité :** Parenthèses pour clarifier, multiplication avant addition  
🎯 **Bonnes pratiques :** Code lisible, variables explicites, gestion d'erreurs  
{% endhint %}

---

{% hint style="warning" %}
**Prêt pour la suite ?** Passez aux [Fonctions](../niveau-intermediaire/fonctions.md) pour organiser votre code avec des blocs réutilisables !
{% endhint %}
