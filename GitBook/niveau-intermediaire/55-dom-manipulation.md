# DOM - Manipulation du Document Object Model

## 🎯 Objectifs du chapitre

À la fin de ce chapitre, vous serez capable de :
- Comprendre la structure et le fonctionnement du DOM
- Sélectionner et manipuler des éléments HTML
- Modifier le contenu, les attributs et les styles
- Créer et supprimer des éléments dynamiquement
- Naviguer dans l'arbre DOM efficacement

## 📋 Prérequis

- Maîtrise des bases de JavaScript (variables, fonctions, objets)
- Connaissance de base en HTML et CSS
- Compréhension des événements JavaScript

---

## 1. Introduction au DOM

### Qu'est-ce que le DOM ?

Le **Document Object Model (DOM)** est une interface de programmation qui représente la structure d'un document HTML ou XML sous forme d'arbre d'objets. Il permet à JavaScript d'interagir avec et de modifier le contenu, la structure et le style d'une page web.

{% tabs %}
{% tab title="Structure DOM" %}
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Ma Page</title>
  </head>
  <body>
    <div id="container">
      <h1 class="title">Bonjour</h1>
      <p>Contenu de la page</p>
    </div>
  </body>
</html>
```

**Représentation en arbre DOM :**
```
document
└── html
    ├── head
    │   └── title
    │       └── "Ma Page"
    └── body
        └── div#container
            ├── h1.title
            │   └── "Bonjour"
            └── p
                └── "Contenu de la page"
```
{% endtab %}

{% tab title="Types de nœuds" %}
Le DOM contient différents types de nœuds :

- **Element Node** (1) : Éléments HTML (`<div>`, `<p>`, etc.)
- **Text Node** (3) : Contenu textuel
- **Comment Node** (8) : Commentaires HTML
- **Document Node** (9) : Le document racine

```javascript
// Vérifier le type de nœud
console.log(document.nodeType); // 9 (Document)
console.log(document.body.nodeType); // 1 (Element)
console.log(document.body.firstChild.nodeType); // 3 (Text, si c'est du texte)
```
{% endtab %}
{% endtabs %}

---

## 2. Sélection d'éléments

### Méthodes de sélection modernes

{% tabs %}
{% tab title="querySelector & querySelectorAll" %}
```javascript
// Sélection par sélecteur CSS (un seul élément)
const titre = document.querySelector('h1');
const premierDiv = document.querySelector('div');
const elementParId = document.querySelector('#monId');
const elementParClasse = document.querySelector('.maClasse');

// Sélection multiple (NodeList)
const tousLesParagraphes = document.querySelectorAll('p');
const elementsParClasse = document.querySelectorAll('.maClasse');

// Sélecteurs complexes
const liensDansNavigation = document.querySelectorAll('nav a');
const boutonsPrimaires = document.querySelectorAll('button.primary');
const elementsActifs = document.querySelectorAll('[data-active="true"]');
```
{% endtab %}

{% tab title="Méthodes spécifiques" %}
```javascript
// Par ID (retourne un élément ou null)
const element = document.getElementById('monId');

// Par classe (retourne une HTMLCollection)
const elements = document.getElementsByClassName('maClasse');

// Par nom de balise
const paragraphes = document.getElementsByTagName('p');

// Par attribut name
const inputsName = document.getElementsByName('username');

// Différence entre NodeList et HTMLCollection
const nodeList = document.querySelectorAll('p'); // Statique
const htmlCollection = document.getElementsByTagName('p'); // Dynamique
```
{% endtab %}

{% tab title="Sélection relative" %}
```javascript
const element = document.querySelector('#container');

// Navigation vers les parents
const parent = element.parentElement;
const parentNode = element.parentNode;
const ancetre = element.closest('.wrapper'); // Premier ancêtre correspondant

// Navigation vers les enfants
const premiers = element.firstElementChild;
const derniers = element.lastElementChild;
const enfants = element.children; // HTMLCollection
const tousEnfants = element.childNodes; // NodeList (inclut texte)

// Navigation entre frères et sœurs
const suivant = element.nextElementSibling;
const precedent = element.previousElementSibling;

// Vérification
const contient = element.contains(autreElement);
const correspond = element.matches('.classe');
```
{% endtab %}
{% endtabs %}

### 💡 Astuce
> Utilisez `querySelector` et `querySelectorAll` pour la plupart des cas. Ils sont plus flexibles et permettent d'utiliser tous les sélecteurs CSS.

---

## 3. Manipulation du contenu

### Modification du contenu textuel et HTML

{% tabs %}
{% tab title="Propriétés de contenu" %}
```javascript
const element = document.querySelector('#monElement');

// Contenu textuel uniquement
console.log(element.textContent); // Récupère le texte
element.textContent = 'Nouveau texte'; // Modifie le texte

// Contenu HTML
console.log(element.innerHTML); // Récupère le HTML
element.innerHTML = '<strong>Texte en gras</strong>'; // Modifie le HTML

// Contenu HTML externe (y compris l'élément lui-même)
console.log(element.outerHTML);
element.outerHTML = '<div class="nouveau">Remplace complètement</div>';

// Texte visible uniquement (respecte les styles CSS)
console.log(element.innerText);
```
{% endtab %}

{% tab title="Insertion de contenu" %}
```javascript
const container = document.querySelector('#container');

// Insertion relative
container.insertAdjacentHTML('beforebegin', '<p>Avant l\'élément</p>');
container.insertAdjacentHTML('afterbegin', '<p>Début du contenu</p>');
container.insertAdjacentHTML('beforeend', '<p>Fin du contenu</p>');
container.insertAdjacentHTML('afterend', '<p>Après l\'élément</p>');

// Insertion de texte
container.insertAdjacentText('beforeend', 'Texte simple');

// Insertion d'éléments
const nouveauElement = document.createElement('div');
container.insertAdjacentElement('beforeend', nouveauElement);
```
{% endtab %}

{% tab title="Exemple pratique" %}
```javascript
// Mise à jour d'une liste de tâches
function ajouterTache(texte) {
  const liste = document.querySelector('#listeTaches');
  const nouvelleTache = `
    <li class="tache">
      <span class="texte">${texte}</span>
      <button class="supprimer">×</button>
    </li>
  `;
  liste.insertAdjacentHTML('beforeend', nouvelleTache);
}

// Utilisation sécurisée avec textContent
function ajouterTacheSecurise(texte) {
  const liste = document.querySelector('#listeTaches');
  const li = document.createElement('li');
  const span = document.createElement('span');
  const button = document.createElement('button');
  
  li.className = 'tache';
  span.className = 'texte';
  span.textContent = texte; // Sécurisé contre XSS
  button.className = 'supprimer';
  button.textContent = '×';
  
  li.appendChild(span);
  li.appendChild(button);
  liste.appendChild(li);
}
```
{% endtab %}
{% endtabs %}

---

## 4. Manipulation des attributs

### Gestion des attributs HTML

{% tabs %}
{% tab title="Attributs standards" %}
```javascript
const lien = document.querySelector('a');

// Lecture d'attributs
const href = lien.getAttribute('href');
const target = lien.getAttribute('target');

// Modification d'attributs
lien.setAttribute('href', 'https://example.com');
lien.setAttribute('target', '_blank');
lien.setAttribute('rel', 'noopener');

// Suppression d'attributs
lien.removeAttribute('target');

// Vérification d'existence
const aHref = lien.hasAttribute('href');

// Accès direct aux propriétés
lien.href = 'https://nouveau-lien.com';
lien.title = 'Nouveau titre';
```
{% endtab %}

{% tab title="Attributs de données" %}
```javascript
const element = document.querySelector('#monElement');

// HTML: <div id="monElement" data-user-id="123" data-role="admin">

// Lecture via dataset
const userId = element.dataset.userId; // "123"
const role = element.dataset.role; // "admin"

// Modification via dataset
element.dataset.userId = "456";
element.dataset.newProperty = "valeur";

// Lecture/modification traditionnelle
const userIdAttr = element.getAttribute('data-user-id');
element.setAttribute('data-status', 'active');

// Conversion automatique kebab-case ↔ camelCase
element.dataset.userPreference = "dark"; // devient data-user-preference
```
{% endtab %}

{% tab title="Classes CSS" %}
```javascript
const element = document.querySelector('.monElement');

// Propriété className (string)
element.className = 'nouvelle-classe autre-classe';
console.log(element.className); // "nouvelle-classe autre-classe"

// API classList (plus pratique)
element.classList.add('active', 'visible');
element.classList.remove('hidden');
element.classList.toggle('collapsed'); // Ajoute/supprime selon l'état
element.classList.replace('old-class', 'new-class');

// Vérifications
const aLaClasse = element.classList.contains('active');
const nombreClasses = element.classList.length;

// Itération
element.classList.forEach(classe => console.log(classe));
```
{% endtab %}
{% endtabs %}

---

## 5. Manipulation des styles

### Modification des styles CSS

{% tabs %}
{% tab title="Styles inline" %}
```javascript
const element = document.querySelector('#monElement');

// Modification directe
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.fontSize = '16px';
element.style.marginTop = '10px';

// Propriétés avec tirets (utilisez camelCase)
element.style.borderRadius = '5px'; // border-radius
element.style.textAlign = 'center'; // text-align

// Propriétés CSS personnalisées
element.style.setProperty('--couleur-principale', '#3498db');

// Suppression de styles
element.style.removeProperty('color');
element.style.color = ''; // Alternative

// Lecture de styles inline
console.log(element.style.color);
console.log(element.style.getPropertyValue('background-color'));
```
{% endtab %}

{% tab title="Styles calculés" %}
```javascript
const element = document.querySelector('#monElement');

// Récupération des styles calculés (finaux)
const styles = window.getComputedStyle(element);

console.log(styles.color); // Couleur finale
console.log(styles.fontSize); // Taille finale
console.log(styles.marginTop); // Marge finale

// Avec pseudo-éléments
const pseudoStyles = window.getComputedStyle(element, '::before');
console.log(pseudoStyles.content);

// Fonction utilitaire
function getStyle(element, property) {
  return window.getComputedStyle(element).getPropertyValue(property);
}

console.log(getStyle(element, 'background-color'));
```
{% endtab %}

{% tab title="Gestion responsive" %}
```javascript
// Modification conditionnelle selon la taille d'écran
function ajusterStyles() {
  const element = document.querySelector('#responsive-element');
  
  if (window.innerWidth < 768) {
    element.style.fontSize = '14px';
    element.style.padding = '5px';
  } else {
    element.style.fontSize = '16px';
    element.style.padding = '10px';
  }
}

// Écouter les changements de taille
window.addEventListener('resize', ajusterStyles);

// Utilisation de CSS personnalisées pour la responsivité
function setTheme(theme) {
  const root = document.documentElement;
  
  if (theme === 'dark') {
    root.style.setProperty('--bg-color', '#333');
    root.style.setProperty('--text-color', '#fff');
  } else {
    root.style.setProperty('--bg-color', '#fff');
    root.style.setProperty('--text-color', '#333');
  }
}
```
{% endtab %}
{% endtabs %}

---

## 6. Création et suppression d'éléments

### Création dynamique d'éléments

{% tabs %}
{% tab title="Création basique" %}
```javascript
// Création d'éléments
const div = document.createElement('div');
const paragraph = document.createElement('p');
const lien = document.createElement('a');

// Configuration des attributs
div.id = 'nouveau-container';
div.className = 'container active';
paragraph.textContent = 'Nouveau paragraphe';
lien.href = 'https://example.com';
lien.textContent = 'Lien externe';
lien.target = '_blank';

// Assemblage
div.appendChild(paragraph);
div.appendChild(lien);

// Insertion dans le DOM
document.body.appendChild(div);
```
{% endtab %}

{% tab title="Méthodes insertion" %}
```javascript
const container = document.querySelector('#container');
const newElement = document.createElement('div');

// Insertion à la fin
container.appendChild(newElement);

// Insertion avant un élément spécifique
const referenceElement = document.querySelector('#reference');
container.insertBefore(newElement, referenceElement);

// Méthodes modernes (plus flexibles)
container.prepend(newElement); // Au début
container.append(newElement); // À la fin
referenceElement.before(newElement); // Avant l'élément
referenceElement.after(newElement); // Après l'élément

// Remplacement
const oldElement = document.querySelector('#old');
oldElement.replaceWith(newElement);
```
{% endtab %}

{% tab title="Clonage éléments" %}
```javascript
const original = document.querySelector('#template');

// Clonage superficiel (sans enfants)
const copieSuperficielle = original.cloneNode(false);

// Clonage profond (avec tous les enfants)
const copieProfonde = original.cloneNode(true);

// Exemple pratique : template de carte
function creerCarte(titre, contenu) {
  const template = document.querySelector('#template-carte');
  const carte = template.cloneNode(true);
  
  carte.id = ''; // Retirer l'ID du template
  carte.querySelector('.titre').textContent = titre;
  carte.querySelector('.contenu').textContent = contenu;
  
  return carte;
}

// Utilisation
const nouvelleCarte = creerCarte('Mon Titre', 'Mon contenu');
document.querySelector('#conteneur-cartes').appendChild(nouvelleCarte);
```
{% endtab %}
{% endtabs %}

### Suppression d'éléments

{% tabs %}
{% tab title="Méthodes de suppression" %}
```javascript
const element = document.querySelector('#aSupprimer');

// Suppression moderne
element.remove(); // Supprime l'élément du DOM

// Suppression traditionnelle
element.parentNode.removeChild(element);

// Suppression de tous les enfants
const container = document.querySelector('#container');
container.innerHTML = ''; // Rapide mais perd les événements

// Suppression propre des enfants (préserve les événements)
while (container.firstChild) {
  container.removeChild(container.firstChild);
}

// Ou avec une boucle moderne
[...container.children].forEach(child => child.remove());
```
{% endtab %}

{% tab title="Suppression conditionnelle" %}
```javascript
// Supprimer tous les éléments d'une classe
document.querySelectorAll('.a-supprimer').forEach(el => el.remove());

// Supprimer selon un critère
function supprimerTachesCompletes() {
  const taches = document.querySelectorAll('.tache');
  taches.forEach(tache => {
    if (tache.classList.contains('complete')) {
      tache.remove();
    }
  });
}

// Suppression avec confirmation
function supprimerAvecConfirmation(element) {
  if (confirm('Êtes-vous sûr de vouloir supprimer cet élément ?')) {
    element.remove();
  }
}
```
{% endtab %}
{% endtabs %}

---

## 7. Formulaires et interactions

### Manipulation des formulaires

{% tabs %}
{% tab title="Accès aux éléments de formulaire" %}
```javascript
const form = document.querySelector('#monFormulaire');

// Accès par nom ou index
const inputNom = form.elements.nom; // <input name="nom">
const premierInput = form.elements[0];

// Accès direct aux valeurs
const valeurs = {
  nom: form.nom.value,
  email: form.email.value,
  age: parseInt(form.age.value),
  accepte: form.accepte.checked
};

// Itération sur tous les éléments
for (let element of form.elements) {
  if (element.type !== 'submit') {
    console.log(`${element.name}: ${element.value}`);
  }
}
```
{% endtab %}

{% tab title="Validation et gestion" %}
```javascript
function validerFormulaire(form) {
  const erreurs = [];
  
  // Validation du nom
  const nom = form.nom.value.trim();
  if (!nom) {
    erreurs.push('Le nom est requis');
    form.nom.classList.add('erreur');
  } else {
    form.nom.classList.remove('erreur');
  }
  
  // Validation de l'email
  const email = form.email.value.trim();
  const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!regexEmail.test(email)) {
    erreurs.push('Email invalide');
    form.email.classList.add('erreur');
  } else {
    form.email.classList.remove('erreur');
  }
  
  return erreurs;
}

// Gestion de la soumission
form.addEventListener('submit', function(e) {
  e.preventDefault();
  
  const erreurs = validerFormulaire(this);
  
  if (erreurs.length === 0) {
    // Formulaire valide
    console.log('Formulaire soumis !');
  } else {
    // Afficher les erreurs
    afficherErreurs(erreurs);
  }
});
```
{% endtab %}

{% tab title="Manipulation des champs" %}
```javascript
// Sélection multiple
const select = document.querySelector('#pays');
const options = [...select.options];
const valeursSelectionnees = options
  .filter(option => option.selected)
  .map(option => option.value);

// Cases à cocher groupées
const checkboxes = document.querySelectorAll('input[name="hobbies"]');
const hobbiesSelectionnes = [...checkboxes]
  .filter(cb => cb.checked)
  .map(cb => cb.value);

// Boutons radio
const genreSelectionne = document.querySelector('input[name="genre"]:checked')?.value;

// Manipulation dynamique
function toggleChamp(condition, champ) {
  champ.disabled = !condition;
  champ.style.opacity = condition ? '1' : '0.5';
}

// Auto-complétion simple
function setupAutoComplete(input, suggestions) {
  input.addEventListener('input', function() {
    const value = this.value.toLowerCase();
    const matches = suggestions.filter(s => 
      s.toLowerCase().includes(value)
    );
    afficherSuggestions(matches);
  });
}
```
{% endtab %}
{% endtabs %}

---

## 8. Performance et bonnes pratiques

### Optimisation des performances

{% hint style="warning" %}
**Attention aux performances !**
Les manipulations DOM peuvent être coûteuses. Voici les bonnes pratiques à suivre.
{% endhint %}

{% tabs %}
{% tab title="Minimiser les reflows" %}
```javascript
// ❌ Mauvais : Multiple accès au DOM
function mauvaiseMethode() {
  const element = document.querySelector('#element');
  element.style.width = '100px';
  element.style.height = '100px';
  element.style.backgroundColor = 'red';
  // 3 reflows !
}

// ✅ Bon : Regrouper les modifications
function bonneMethode() {
  const element = document.querySelector('#element');
  element.style.cssText = 'width: 100px; height: 100px; background-color: red;';
  // 1 seul reflow !
}

// ✅ Encore mieux : Utiliser des classes CSS
function meilleureMethode() {
  const element = document.querySelector('#element');
  element.className = 'nouvelle-classe';
  // Styles définis en CSS
}
```
{% endtab %}

{% tab title="Batch DOM operations" %}
```javascript
// ❌ Mauvais : Insertions multiples
function ajouterElementsMauvais(items) {
  const container = document.querySelector('#container');
  items.forEach(item => {
    const div = document.createElement('div');
    div.textContent = item;
    container.appendChild(div); // Reflow à chaque insertion !
  });
}

// ✅ Bon : DocumentFragment
function ajouterElementsBon(items) {
  const container = document.querySelector('#container');
  const fragment = document.createDocumentFragment();
  
  items.forEach(item => {
    const div = document.createElement('div');
    div.textContent = item;
    fragment.appendChild(div);
  });
  
  container.appendChild(fragment); // Un seul reflow !
}

// ✅ Alternative : innerHTML groupé
function ajouterElementsAlternatif(items) {
  const container = document.querySelector('#container');
  const html = items.map(item => `<div>${item}</div>`).join('');
  container.insertAdjacentHTML('beforeend', html);
}
```
{% endtab %}

{% tab title="Cache des sélections" %}
```javascript
// ❌ Mauvais : Sélections répétées
function mauvaiseGestion() {
  document.querySelector('#button').addEventListener('click', () => {
    document.querySelector('#element').style.color = 'red';
    document.querySelector('#element').style.fontSize = '16px';
    document.querySelector('#element').textContent = 'Modifié';
  });
}

// ✅ Bon : Cache des références
class UIManager {
  constructor() {
    this.elements = {
      button: document.querySelector('#button'),
      element: document.querySelector('#element'),
      container: document.querySelector('#container')
    };
    
    this.bindEvents();
  }
  
  bindEvents() {
    this.elements.button.addEventListener('click', () => {
      this.modifierElement();
    });
  }
  
  modifierElement() {
    const el = this.elements.element;
    el.style.cssText = 'color: red; font-size: 16px;';
    el.textContent = 'Modifié';
  }
}
```
{% endtab %}
{% endtabs %}

---

## 🧪 Exercices pratiques

### Exercice 1 : Galerie d'images interactive

{% hint style="info" %}
**Objectif :** Créer une galerie d'images avec aperçu et navigation
{% endhint %}

```javascript
class GalerieImages {
  constructor(containerId, images) {
    this.container = document.querySelector(containerId);
    this.images = images;
    this.currentIndex = 0;
    
    this.render();
    this.bindEvents();
  }
  
  render() {
    // À compléter : créer la structure HTML
    // - Conteneur principal avec aperçu
    // - Miniatures cliquables
    // - Boutons navigation (précédent/suivant)
  }
  
  bindEvents() {
    // À compléter : gérer les clics et la navigation
  }
  
  showImage(index) {
    // À compléter : afficher l'image à l'index donné
  }
}

// Test
const images = [
  { src: 'image1.jpg', alt: 'Image 1' },
  { src: 'image2.jpg', alt: 'Image 2' },
  { src: 'image3.jpg', alt: 'Image 3' }
];

const galerie = new GalerieImages('#galerie', images);
```

{% hint style="success" %}
**Solution cachée :** [Cliquez pour révéler](75-dom-manipulation.md#solution-exercice-1)
{% endhint %}

### Exercice 2 : Liste de tâches avancée

{% hint style="info" %}
**Objectif :** Créer une application de gestion de tâches complète
{% endhint %}

```javascript
class TodoList {
  constructor(containerId) {
    this.container = document.querySelector(containerId);
    this.tasks = [];
    this.nextId = 1;
    
    this.render();
  }
  
  addTask(text) {
    // À compléter : ajouter une nouvelle tâche
  }
  
  removeTask(id) {
    // À compléter : supprimer une tâche
  }
  
  toggleTask(id) {
    // À compléter : marquer comme complète/incomplète
  }
  
  editTask(id, newText) {
    // À compléter : éditer le texte d'une tâche
  }
  
  filterTasks(filter) {
    // À compléter : filtrer par 'all', 'active', 'completed'
  }
  
  render() {
    // À compléter : afficher l'interface complète
  }
}
```

---

## 🔗 Liens avec d'autres concepts

### Connexions importantes

{% tabs %}
{% tab title="Événements" %}
- **Délégation d'événements** : Utiliser la propagation pour gérer les événements dynamiques
- **Performance** : Éviter trop d'écouteurs d'événements
- **Nettoyage** : Supprimer les événements lors de la suppression d'éléments

```javascript
// Délégation d'événements
document.addEventListener('click', function(e) {
  if (e.target.matches('.btn-delete')) {
    e.target.closest('.item').remove();
  }
});
```
{% endtab %}

{% tab title="Asynchrone" %}
- **API Fetch** : Récupérer des données pour alimenter le DOM
- **Promesses** : Gérer les opérations asynchrones avant manipulation
- **Async/Await** : Syntaxe moderne pour les opérations DOM asynchrones

```javascript
async function chargerContenu() {
  const container = document.querySelector('#content');
  container.innerHTML = '<div class="loading">Chargement...</div>';
  
  try {
    const data = await fetch('/api/content');
    const html = await data.text();
    container.innerHTML = html;
  } catch (error) {
    container.innerHTML = '<div class="error">Erreur de chargement</div>';
  }
}
```
{% endtab %}

{% tab title="Frameworks" %}
- **Concepts similaires** : Virtual DOM (React), Templates (Vue.js)
- **Migration** : Comprendre les différences d'approche
- **Performance** : Comparaison avec les solutions framework

La manipulation DOM native reste importante pour :
- Comprendre les frameworks
- Optimiser les performances
- Créer des solutions légères
{% endtab %}
{% endtabs %}

---

## 📝 Résumé

### Points clés à retenir

1. **Sélection** : `querySelector` et `querySelectorAll` pour la flexibilité
2. **Contenu** : `textContent` pour le texte, `innerHTML` pour le HTML
3. **Attributs** : `dataset` pour les données, `classList` pour les classes
4. **Styles** : Préférer les classes CSS aux styles inline
5. **Performance** : Regrouper les modifications, utiliser DocumentFragment
6. **Sécurité** : Attention aux injections XSS avec `innerHTML`

### Checklist des bonnes pratiques

- [ ] Cache des références DOM fréquemment utilisées
- [ ] Utilise DocumentFragment pour les insertions multiples
- [ ] Évite les modifications de style répétées
- [ ] Nettoie les écouteurs d'événements lors de la suppression
- [ ] Utilise la délégation d'événements pour les éléments dynamiques
- [ ] Préfère `textContent` à `innerHTML` pour le contenu simple
- [ ] Valide les entrées utilisateur avant insertion dans le DOM

### Prochaines étapes

Après avoir maîtrisé la manipulation DOM :
- **[Event Loop et Asynchrone](60-event-loop-asynchrone-base.md)** : Comprendre l'exécution JavaScript
- **[Gestion d'erreurs](65-gestion-erreurs.md)** : Gérer les erreurs robustement
- **[API Fetch](../niveau-avance/110-fetch-api-avance.md)** : Récupérer des données externes
- **[WebComponents](../niveau-avance/115-concepts-avances.md)** : Créer des composants réutilisables

---

### Solutions des exercices

#### Solution exercice 1

{% hint style="success" %}
```javascript
class GalerieImages {
  constructor(containerId, images) {
    this.container = document.querySelector(containerId);
    this.images = images;
    this.currentIndex = 0;
    
    this.render();
    this.bindEvents();
  }
  
  render() {
    this.container.innerHTML = `
      <div class="galerie">
        <div class="apercu">
          <img id="image-principale" src="${this.images[0].src}" alt="${this.images[0].alt}">
          <div class="navigation">
            <button id="prev">❮</button>
            <button id="next">❯</button>
          </div>
        </div>
        <div class="miniatures">
          ${this.images.map((img, index) => `
            <img class="miniature ${index === 0 ? 'active' : ''}" 
                 data-index="${index}" 
                 src="${img.src}" 
                 alt="${img.alt}">
          `).join('')}
        </div>
      </div>
    `;
  }
  
  bindEvents() {
    this.container.addEventListener('click', (e) => {
      if (e.target.matches('.miniature')) {
        this.showImage(parseInt(e.target.dataset.index));
      } else if (e.target.id === 'prev') {
        this.previousImage();
      } else if (e.target.id === 'next') {
        this.nextImage();
      }
    });
  }
  
  showImage(index) {
    this.currentIndex = index;
    const img = this.container.querySelector('#image-principale');
    img.src = this.images[index].src;
    img.alt = this.images[index].alt;
    
    // Mise à jour des miniatures
    this.container.querySelectorAll('.miniature').forEach((mini, i) => {
      mini.classList.toggle('active', i === index);
    });
  }
  
  previousImage() {
    const newIndex = this.currentIndex > 0 ? this.currentIndex - 1 : this.images.length - 1;
    this.showImage(newIndex);
  }
  
  nextImage() {
    const newIndex = this.currentIndex < this.images.length - 1 ? this.currentIndex + 1 : 0;
    this.showImage(newIndex);
  }
}
```
{% endhint %}
