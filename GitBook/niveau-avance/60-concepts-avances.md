# 🧠 Concepts JavaScript Avancés

> **Niveau :** 🔴 Avancé | **Durée :** 90 minutes | **Prérequis :** Objets, Fonctions, Async/Await

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** les expressions régulières complexes
- [ ] **Créer** des itérateurs et générateurs sophistiqués
- [ ] **Implémenter** la métaprogrammation avec Proxy et Reflect
- [ ] **Optimiser** la mémoire avec WeakMap et WeakSet

---

## 🔍 Expressions Régulières Avancées

{% tabs %}
{% tab title="🎯 Patterns sophistiqués" %}
**Maîtrisez les regex pour le parsing et la validation avancée**

### Construction de regex complexes
```javascript
// Validateur email robuste (RFC 5322 simplifié)
class EmailValidator {
  static pattern = new RegExp([
    '^[a-zA-Z0-9]',                    // Premier caractère alphanumérique
    '(?:[a-zA-Z0-9._-]*[a-zA-Z0-9])?', // Corps optionnel
    '@',                               // @
    '[a-zA-Z0-9]',                     // Domaine commence par alphanumérique
    '(?:[a-zA-Z0-9.-]*[a-zA-Z0-9])?',  // Corps du domaine
    '\\.',                             // Point obligatoire
    '[a-zA-Z]{2,}$'                    // TLD minimum 2 caractères
  ].join(''));
  
  static validate(email) {
    return this.pattern.test(email);
  }
  
  static extractDomain(email) {
    const match = email.match(/@(.+)$/);
    return match ? match[1] : null;
  }
}

// Parsing d'URLs sophistiqué
class URLParser {
  static pattern = /^(?:(?<protocol>[a-z][a-z0-9+.-]*):)?(?:\/\/(?:(?<username>[^\s:\/]+)(?::(?<password>[^\s@\/]+))?@)?(?<hostname>[^\s:\/?#]+)(?::(?<port>\d+))?)?(?<pathname>\/[^\s?#]*)?(?:\?(?<search>[^\s#]*))?(?:#(?<hash>.*))?$/i;
  
  static parse(url) {
    const match = url.match(this.pattern);
    if (!match) return null;
    
    const { groups } = match;
    return {
      protocol: groups.protocol || 'http',
      username: groups.username || null,
      password: groups.password || null,
      hostname: groups.hostname || 'localhost',
      port: groups.port ? parseInt(groups.port, 10) : null,
      pathname: groups.pathname || '/',
      search: groups.search || '',
      hash: groups.hash || '',
      
      // Propriétés calculées
      get host() {
        return this.port ? `${this.hostname}:${this.port}` : this.hostname;
      },
      
      get origin() {
        return `${this.protocol}://${this.host}`;
      },
      
      get href() {
        let url = `${this.protocol}://${this.host}${this.pathname}`;
        if (this.search) url += `?${this.search}`;
        if (this.hash) url += `#${this.hash}`;
        return url;
      }
    };
  }
  
  static isValidUrl(url) {
    return this.pattern.test(url);
  }
}

// Parser de logs avec extraction de métadonnées
class LogParser {
  // Pattern pour logs Apache/Nginx
  static accessLogPattern = /^(?<ip>\d+\.\d+\.\d+\.\d+) - (?<user>\S+) \[(?<timestamp>[^\]]+)\] "(?<method>\w+) (?<path>\S+) (?<protocol>[^"]+)" (?<status>\d+) (?<size>\d+|-) "(?<referer>[^"]*)" "(?<userAgent>[^"]*)"/;
  
  // Pattern pour logs d'application personnalisés
  static appLogPattern = /^\[(?<level>\w+)\] (?<timestamp>\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}\.\d{3}Z) (?<component>\w+): (?<message>.+)/;
  
  static parseAccessLog(line) {
    const match = line.match(this.accessLogPattern);
    if (!match) return null;
    
    const { groups } = match;
    return {
      ip: groups.ip,
      user: groups.user === '-' ? null : groups.user,
      timestamp: new Date(groups.timestamp.replace(/\//g, '-')),
      method: groups.method,
      path: groups.path,
      protocol: groups.protocol,
      status: parseInt(groups.status, 10),
      size: groups.size === '-' ? null : parseInt(groups.size, 10),
      referer: groups.referer || null,
      userAgent: groups.userAgent,
      
      // Analyse dérivée
      isError: function() {
        return this.status >= 400;
      },
      
      isBot: function() {
        return /bot|crawler|spider/i.test(this.userAgent);
      },
      
      getCountry: function() {
        // Simulation - en production, utiliser une base GeoIP
        const countryMap = {
          '192.168.': 'Local',
          '10.': 'Internal',
          '172.16.': 'Internal'
        };
        
        for (const [prefix, country] of Object.entries(countryMap)) {
          if (this.ip.startsWith(prefix)) {
            return country;
          }
        }
        return 'Unknown';
      }
    };
  }
  
  static parseAppLog(line) {
    const match = line.match(this.appLogPattern);
    if (!match) return null;
    
    const { groups } = match;
    return {
      level: groups.level.toUpperCase(),
      timestamp: new Date(groups.timestamp),
      component: groups.component,
      message: groups.message,
      
      isError: function() {
        return ['ERROR', 'FATAL'].includes(this.level);
      },
      
      isWarning: function() {
        return this.level === 'WARN';
      }
    };
  }
}

// Template engine simple avec regex
class SimpleTemplate {
  constructor(template) {
    this.template = template;
    this.compiled = null;
  }
  
  compile() {
    // Pattern pour {{variable}} et {{#if condition}}...{{/if}}
    const patterns = {
      variables: /\{\{(\w+)\}\}/g,
      conditionals: /\{\{#if\s+(\w+)\}\}(.*?)\{\{\/if\}\}/gs,
      loops: /\{\{#each\s+(\w+)\}\}(.*?)\{\{\/each\}\}/gs
    };
    
    let compiled = this.template;
    
    // Traiter les conditions
    compiled = compiled.replace(patterns.conditionals, (match, condition, content) => {
      return `\${${condition} ? \`${content}\` : ''}`;
    });
    
    // Traiter les boucles
    compiled = compiled.replace(patterns.loops, (match, array, content) => {
      return `\${${array}.map(item => \`${content.replace(/\{\{item\.(\w+)\}\}/g, '${item.$1}')}\`).join('')}`;
    });
    
    // Traiter les variables
    compiled = compiled.replace(patterns.variables, (match, variable) => {
      return `\${${variable}}`;
    });
    
    this.compiled = new Function('data', `
      const { ${Object.keys(this.extractVariables()).join(', ')} } = data;
      return \`${compiled}\`;
    `);
    
    return this;
  }
  
  render(data) {
    if (!this.compiled) {
      this.compile();
    }
    return this.compiled(data);
  }
  
  extractVariables() {
    const variables = new Set();
    const patterns = [
      /\{\{(\w+)\}\}/g,
      /\{\{#if\s+(\w+)\}\}/g,
      /\{\{#each\s+(\w+)\}\}/g
    ];
    
    patterns.forEach(pattern => {
      let match;
      while ((match = pattern.exec(this.template)) !== null) {
        variables.add(match[1]);
      }
    });
    
    return Object.fromEntries([...variables].map(v => [v, undefined]));
  }
}

// Usage des regex avancées
console.log('=== Email Validation ===');
console.log(EmailValidator.validate('user@example.com')); // true
console.log(EmailValidator.validate('invalid.email'));    // false
console.log(EmailValidator.extractDomain('user@example.com')); // example.com

console.log('\n=== URL Parsing ===');
const parsed = URLParser.parse('https://user:pass@example.com:8080/path?q=1#section');
console.log(parsed.hostname); // example.com
console.log(parsed.origin);   // https://example.com:8080

console.log('\n=== Log Parsing ===');
const logLine = '192.168.1.1 - user [25/Dec/2023:10:00:00 +0000] "GET /api/users HTTP/1.1" 200 1234 "https://example.com" "Mozilla/5.0"';
const logEntry = LogParser.parseAccessLog(logLine);
console.log(logEntry.method, logEntry.status, logEntry.isError());

console.log('\n=== Template Engine ===');
const template = new SimpleTemplate(`
  <h1>{{title}}</h1>
  {{#if users}}
    <ul>
      {{#each users}}
        <li>{{item.name}} - {{item.email}}</li>
      {{/each}}
    </ul>
  {{/if}}
`);

const html = template.render({
  title: 'User List',
  users: [
    { name: 'Alice', email: 'alice@example.com' },
    { name: 'Bob', email: 'bob@example.com' }
  ]
});
console.log(html);
```

### Regex performance et debugging
```javascript
// Regex performance analyzer
class RegexAnalyzer {
  static benchmark(pattern, text, iterations = 10000) {
    const regex = new RegExp(pattern);
    
    // Warm-up
    for (let i = 0; i < 100; i++) {
      regex.test(text);
    }
    
    // Mesure
    const start = performance.now();
    for (let i = 0; i < iterations; i++) {
      regex.test(text);
    }
    const end = performance.now();
    
    return {
      pattern: pattern.toString(),
      iterations,
      totalTime: end - start,
      avgTime: (end - start) / iterations,
      opsPerSecond: iterations / ((end - start) / 1000)
    };
  }
  
  static analyzeComplexity(pattern) {
    const complexity = {
      quantifiers: (pattern.match(/[*+?{}]/g) || []).length,
      groups: (pattern.match(/[\(\)]/g) || []).length / 2,
      alternations: (pattern.match(/\|/g) || []).length,
      lookarounds: (pattern.match(/\(\?[=!<]/g) || []).length,
      backtracking: /\*|\+|\?|{.*}/.test(pattern)
    };
    
    const score = complexity.quantifiers * 2 + 
                  complexity.groups + 
                  complexity.alternations * 3 + 
                  complexity.lookarounds * 5 +
                  (complexity.backtracking ? 10 : 0);
    
    return {
      ...complexity,
      complexityScore: score,
      risk: score > 20 ? 'HIGH' : score > 10 ? 'MEDIUM' : 'LOW',
      suggestions: this.getSuggestions(complexity, pattern)
    };
  }
  
  static getSuggestions(complexity, pattern) {
    const suggestions = [];
    
    if (complexity.backtracking && complexity.quantifiers > 3) {
      suggestions.push('Consider using possessive quantifiers to prevent excessive backtracking');
    }
    
    if (complexity.alternations > 2) {
      suggestions.push('Multiple alternations can be slow - consider character classes or separate regexes');
    }
    
    if (complexity.lookarounds > 1) {
      suggestions.push('Multiple lookarounds increase complexity - consider preprocessing');
    }
    
    if (pattern.includes('.*') || pattern.includes('.+')) {
      suggestions.push('Dot-star patterns can be slow - be more specific if possible');
    }
    
    return suggestions;
  }
  
  static explainPattern(pattern) {
    const explanations = {
      '^': 'Start of string/line',
      '$': 'End of string/line',
      '.': 'Any character except newline',
      '*': 'Zero or more of preceding',
      '+': 'One or more of preceding',
      '?': 'Zero or one of preceding',
      '\\d': 'Any digit (0-9)',
      '\\w': 'Word character (a-z, A-Z, 0-9, _)',
      '\\s': 'Whitespace character',
      '\\b': 'Word boundary',
      '(?:': 'Non-capturing group',
      '(?=': 'Positive lookahead',
      '(?!': 'Negative lookahead',
      '(?<=': 'Positive lookbehind',
      '(?<!': 'Negative lookbehind'
    };
    
    const parts = [];
    let i = 0;
    
    while (i < pattern.length) {
      let found = false;
      
      // Check for multi-character tokens
      for (const [token, explanation] of Object.entries(explanations)) {
        if (pattern.substr(i, token.length) === token) {
          parts.push({ token, explanation, position: i });
          i += token.length;
          found = true;
          break;
        }
      }
      
      if (!found) {
        parts.push({ 
          token: pattern[i], 
          explanation: `Literal character: ${pattern[i]}`, 
          position: i 
        });
        i++;
      }
    }
    
    return parts;
  }
}

// Usage de l'analyzer
const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
const complexPattern = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;

console.log('=== Regex Performance ===');
console.log(RegexAnalyzer.benchmark(emailPattern, 'user@example.com'));

console.log('\n=== Complexity Analysis ===');
console.log(RegexAnalyzer.analyzeComplexity(complexPattern.source));

console.log('\n=== Pattern Explanation ===');
console.log(RegexAnalyzer.explainPattern('^\\d+$'));
```
{% endtab %}

{% tab title="🔧 Regex utilitaires" %}
**Bibliothèque d'utilitaires regex pour cas courants**

```javascript
// Collection de regex utilitaires
const RegexUtils = {
  // Validation
  patterns: {
    email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
    phone: /^[\+]?[1-9][\d]{0,15}$/,
    url: /^https?:\/\/(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&=]*)$/,
    ipv4: /^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/,
    ipv6: /^(?:[0-9a-fA-F]{1,4}:){7}[0-9a-fA-F]{1,4}$/,
    creditCard: /^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|3[0-9]{13}|6(?:011|5[0-9]{2})[0-9]{12})$/,
    ssn: /^\d{3}-?\d{2}-?\d{4}$/,
    uuid: /^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i,
    hexColor: /^#(?:[0-9a-fA-F]{3}){1,2}$/,
    strongPassword: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/
  },
  
  // Extracteurs
  extractors: {
    urls: /https?:\/\/(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&=]*)/g,
    emails: /[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g,
    hashtags: /#[a-zA-Z0-9_]+/g,
    mentions: /@[a-zA-Z0-9_]+/g,
    numbers: /-?\d+(?:\.\d+)?/g,
    dates: /\b(?:\d{1,2}[\/\-]\d{1,2}[\/\-]\d{2,4}|\d{4}[\/\-]\d{1,2}[\/\-]\d{1,2})\b/g,
    phoneNumbers: /(?:\+?1[-.\s]?)?\(?[0-9]{3}\)?[-.\s]?[0-9]{3}[-.\s]?[0-9]{4}/g
  },
  
  // Nettoyage
  cleaners: {
    whitespace: /\s+/g,
    htmlTags: /<[^>]*>/g,
    specialChars: /[^\w\s]/g,
    multipleSpaces: /\s{2,}/g,
    leadingTrailingSpaces: /^\s+|\s+$/g,
    nonAlphanumeric: /[^a-zA-Z0-9]/g,
    accents: /[\u0300-\u036f]/g
  },
  
  // Méthodes utilitaires
  validate(text, type) {
    const pattern = this.patterns[type];
    if (!pattern) {
      throw new Error(`Unknown validation type: ${type}`);
    }
    return pattern.test(text);
  },
  
  extract(text, type) {
    const pattern = this.extractors[type];
    if (!pattern) {
      throw new Error(`Unknown extraction type: ${type}`);
    }
    return text.match(pattern) || [];
  },
  
  clean(text, type, replacement = '') {
    const pattern = this.cleaners[type];
    if (!pattern) {
      throw new Error(`Unknown cleaning type: ${type}`);
    }
    return text.replace(pattern, replacement);
  },
  
  // Validation avancée avec détails
  validateWithDetails(text, type) {
    const isValid = this.validate(text, type);
    
    if (!isValid) {
      const suggestions = this.getValidationSuggestions(text, type);
      return {
        valid: false,
        suggestions
      };
    }
    
    return { valid: true };
  },
  
  getValidationSuggestions(text, type) {
    const suggestions = [];
    
    switch (type) {
      case 'email':
        if (!text.includes('@')) {
          suggestions.push('Email must contain @ symbol');
        }
        if (!text.includes('.')) {
          suggestions.push('Email must contain domain extension');
        }
        if (text.includes(' ')) {
          suggestions.push('Email cannot contain spaces');
        }
        break;
        
      case 'strongPassword':
        if (text.length < 8) {
          suggestions.push('Password must be at least 8 characters');
        }
        if (!/[a-z]/.test(text)) {
          suggestions.push('Password must contain lowercase letter');
        }
        if (!/[A-Z]/.test(text)) {
          suggestions.push('Password must contain uppercase letter');
        }
        if (!/\d/.test(text)) {
          suggestions.push('Password must contain number');
        }
        if (!/[@$!%*?&]/.test(text)) {
          suggestions.push('Password must contain special character');
        }
        break;
        
      case 'url':
        if (!text.startsWith('http')) {
          suggestions.push('URL must start with http:// or https://');
        }
        if (!text.includes('.')) {
          suggestions.push('URL must contain domain extension');
        }
        break;
    }
    
    return suggestions;
  },
  
  // Génération de patterns dynamiques
  generatePattern(config) {
    const { type, minLength, maxLength, allowSpecial, requireUpper, requireNumber } = config;
    
    let pattern = '';
    const components = [];
    
    if (requireUpper) {
      components.push('(?=.*[A-Z])');
    }
    
    if (requireNumber) {
      components.push('(?=.*\\d)');
    }
    
    if (allowSpecial) {
      components.push('[A-Za-z\\d@$!%*?&]');
    } else {
      components.push('[A-Za-z\\d]');
    }
    
    if (minLength || maxLength) {
      const min = minLength || 0;
      const max = maxLength || '';
      components[components.length - 1] += `{${min},${max}}`;
    } else {
      components[components.length - 1] += '+';
    }
    
    pattern = '^' + components.join('') + '$';
    
    return new RegExp(pattern);
  },
  
  // Fuzzy matching
  fuzzyMatch(pattern, text, tolerance = 0.8) {
    // Simple implementation - en production, utiliser une lib comme fuse.js
    const normalizedPattern = pattern.toLowerCase();
    const normalizedText = text.toLowerCase();
    
    let matches = 0;
    let i = 0, j = 0;
    
    while (i < normalizedPattern.length && j < normalizedText.length) {
      if (normalizedPattern[i] === normalizedText[j]) {
        matches++;
        i++;
      }
      j++;
    }
    
    const similarity = matches / normalizedPattern.length;
    return similarity >= tolerance;
  }
};

// Tests des utilitaires
console.log('=== Validation Tests ===');
console.log(RegexUtils.validate('user@example.com', 'email')); // true
console.log(RegexUtils.validateWithDetails('user', 'email')); // suggestions

console.log('\n=== Extraction Tests ===');
const text = "Contact us at info@company.com or visit https://company.com #javascript @johndoe";
console.log(RegexUtils.extract(text, 'emails'));    // ['info@company.com']
console.log(RegexUtils.extract(text, 'urls'));      // ['https://company.com']
console.log(RegexUtils.extract(text, 'hashtags'));  // ['#javascript']
console.log(RegexUtils.extract(text, 'mentions'));  // ['@johndoe']

console.log('\n=== Cleaning Tests ===');
const dirtyText = "  Hello   <b>World</b>!  @#$  ";
console.log(RegexUtils.clean(dirtyText, 'htmlTags'));         // "  Hello   World!  @#$  "
console.log(RegexUtils.clean(dirtyText, 'specialChars', '')); // "  Hello   World    "
console.log(RegexUtils.clean(dirtyText, 'multipleSpaces', ' ')); // " Hello <b>World</b>! @#$ "

console.log('\n=== Dynamic Pattern ===');
const customPattern = RegexUtils.generatePattern({
  minLength: 6,
  maxLength: 20,
  requireUpper: true,
  requireNumber: true,
  allowSpecial: true
});
console.log(customPattern.test('MyPass123!')); // true

console.log('\n=== Fuzzy Matching ===');
console.log(RegexUtils.fuzzyMatch('javascript', 'java script', 0.7)); // true
console.log(RegexUtils.fuzzyMatch('javascript', 'python', 0.7));      // false
```
{% endtab %}
{% endtabs %}

---

## ⚙️ Itérateurs et Générateurs Avancés

{% tabs %}
{% tab title="🔄 Itérateurs personnalisés" %}
**Créez des protocoles d'itération sophistiqués**

### Itérateurs avec état complexe
```javascript
// Itérateur pour navigation dans une structure d'arbre
class TreeIterator {
  constructor(root, traversalType = 'depth-first') {
    this.root = root;
    this.traversalType = traversalType;
    this.reset();
  }
  
  reset() {
    this.stack = [this.root];
    this.visited = new Set();
    this.current = null;
    this.done = false;
  }
  
  [Symbol.iterator]() {
    return this;
  }
  
  next() {
    if (this.done) {
      return { value: undefined, done: true };
    }
    
    if (this.traversalType === 'depth-first') {
      return this.depthFirstNext();
    } else if (this.traversalType === 'breadth-first') {
      return this.breadthFirstNext();
    }
  }
  
  depthFirstNext() {
    while (this.stack.length > 0) {
      const node = this.stack.pop();
      
      if (!this.visited.has(node)) {
        this.visited.add(node);
        this.current = node;
        
        // Ajouter les enfants à la pile (ordre inverse pour DFS)
        if (node.children) {
          for (let i = node.children.length - 1; i >= 0; i--) {
            this.stack.push(node.children[i]);
          }
        }
        
        return { value: node, done: false };
      }
    }
    
    this.done = true;
    return { value: undefined, done: true };
  }
  
  breadthFirstNext() {
    while (this.stack.length > 0) {
      const node = this.stack.shift(); // Queue behavior for BFS
      
      if (!this.visited.has(node)) {
        this.visited.add(node);
        this.current = node;
        
        // Ajouter les enfants à la fin
        if (node.children) {
          this.stack.push(...node.children);
        }
        
        return { value: node, done: false };
      }
    }
    
    this.done = true;
    return { value: undefined, done: true };
  }
  
  // Méthodes utilitaires
  find(predicate) {
    for (const node of this) {
      if (predicate(node)) {
        return node;
      }
    }
    return null;
  }
  
  filter(predicate) {
    const results = [];
    for (const node of this) {
      if (predicate(node)) {
        results.push(node);
      }
    }
    return results;
  }
  
  map(mapper) {
    const results = [];
    for (const node of this) {
      results.push(mapper(node));
    }
    return results;
  }
  
  toArray() {
    return [...this];
  }
  
  count() {
    let count = 0;
    for (const node of this) {
      count++;
    }
    return count;
  }
}

// Structure d'arbre pour tests
class TreeNode {
  constructor(value, children = []) {
    this.value = value;
    this.children = children;
    this.id = Math.random().toString(36).substr(2, 9);
  }
  
  addChild(child) {
    this.children.push(child);
    return this;
  }
  
  toString() {
    return `Node(${this.value})`;
  }
}

// Itérateur pour chunking de données
class ChunkIterator {
  constructor(iterable, chunkSize) {
    this.iterable = iterable;
    this.chunkSize = chunkSize;
    this.iterator = iterable[Symbol.iterator]();
  }
  
  [Symbol.iterator]() {
    return this;
  }
  
  next() {
    const chunk = [];
    
    for (let i = 0; i < this.chunkSize; i++) {
      const result = this.iterator.next();
      
      if (result.done) {
        if (chunk.length > 0) {
          return { value: chunk, done: false };
        }
        return { value: undefined, done: true };
      }
      
      chunk.push(result.value);
    }
    
    return { value: chunk, done: false };
  }
}

// Itérateur pour window/sliding window
class WindowIterator {
  constructor(iterable, windowSize, step = 1) {
    this.array = Array.from(iterable);
    this.windowSize = windowSize;
    this.step = step;
    this.index = 0;
  }
  
  [Symbol.iterator]() {
    return this;
  }
  
  next() {
    if (this.index + this.windowSize > this.array.length) {
      return { value: undefined, done: true };
    }
    
    const window = this.array.slice(this.index, this.index + this.windowSize);
    this.index += this.step;
    
    return { value: window, done: false };
  }
}

// Itérateur pour combinaisons
class CombinationIterator {
  constructor(array, r) {
    this.array = array;
    this.r = r;
    this.n = array.length;
    this.indices = Array.from({ length: r }, (_, i) => i);
    this.done = false;
  }
  
  [Symbol.iterator]() {
    return this;
  }
  
  next() {
    if (this.done) {
      return { value: undefined, done: true };
    }
    
    // Retourner la combinaison actuelle
    const combination = this.indices.map(i => this.array[i]);
    
    // Calculer la prochaine combinaison
    this.nextCombination();
    
    return { value: combination, done: false };
  }
  
  nextCombination() {
    // Algorithme pour générer la prochaine combinaison lexicographique
    let i = this.r - 1;
    
    while (i >= 0 && this.indices[i] === this.n - this.r + i) {
      i--;
    }
    
    if (i < 0) {
      this.done = true;
      return;
    }
    
    this.indices[i]++;
    
    for (let j = i + 1; j < this.r; j++) {
      this.indices[j] = this.indices[j - 1] + 1;
    }
  }
  
  static count(n, r) {
    // Calculer C(n,r) = n! / (r! * (n-r)!)
    if (r > n) return 0;
    if (r === 0 || r === n) return 1;
    
    let result = 1;
    for (let i = 0; i < r; i++) {
      result = result * (n - i) / (i + 1);
    }
    return Math.round(result);
  }
}

// Tests des itérateurs
console.log('=== Tree Iterator ===');
const tree = new TreeNode('root')
  .addChild(new TreeNode('child1')
    .addChild(new TreeNode('grandchild1'))
    .addChild(new TreeNode('grandchild2')))
  .addChild(new TreeNode('child2')
    .addChild(new TreeNode('grandchild3')));

const dfsIterator = new TreeIterator(tree, 'depth-first');
console.log('DFS:', dfsIterator.map(node => node.value));

const bfsIterator = new TreeIterator(tree, 'breadth-first');
console.log('BFS:', bfsIterator.map(node => node.value));

console.log('\n=== Chunk Iterator ===');
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const chunks = new ChunkIterator(numbers, 3);
for (const chunk of chunks) {
  console.log('Chunk:', chunk);
}

console.log('\n=== Window Iterator ===');
const windows = new WindowIterator([1, 2, 3, 4, 5], 3, 1);
for (const window of windows) {
  console.log('Window:', window);
}

console.log('\n=== Combination Iterator ===');
const combinations = new CombinationIterator(['A', 'B', 'C', 'D'], 2);
for (const combination of combinations) {
  console.log('Combination:', combination);
}
console.log('Total combinations:', CombinationIterator.count(4, 2));
```

### Async iterators
```javascript
// Itérateur asynchrone pour streaming de données
class AsyncDataStream {
  constructor(dataSource, options = {}) {
    this.dataSource = dataSource;
    this.batchSize = options.batchSize || 10;
    this.delay = options.delay || 100;
    this.currentIndex = 0;
    this.buffer = [];
    this.done = false;
  }
  
  [Symbol.asyncIterator]() {
    return this;
  }
  
  async next() {
    if (this.done && this.buffer.length === 0) {
      return { value: undefined, done: true };
    }
    
    // Remplir le buffer si nécessaire
    if (this.buffer.length === 0) {
      await this.fillBuffer();
    }
    
    if (this.buffer.length === 0) {
      return { value: undefined, done: true };
    }
    
    const value = this.buffer.shift();
    return { value, done: false };
  }
  
  async fillBuffer() {
    const endIndex = Math.min(
      this.currentIndex + this.batchSize,
      this.dataSource.length
    );
    
    if (this.currentIndex >= this.dataSource.length) {
      this.done = true;
      return;
    }
    
    // Simuler une opération asynchrone
    await new Promise(resolve => setTimeout(resolve, this.delay));
    
    for (let i = this.currentIndex; i < endIndex; i++) {
      this.buffer.push(await this.processItem(this.dataSource[i]));
    }
    
    this.currentIndex = endIndex;
    
    if (this.currentIndex >= this.dataSource.length) {
      this.done = true;
    }
  }
  
  async processItem(item) {
    // Simuler un traitement asynchrone
    await new Promise(resolve => setTimeout(resolve, 10));
    return `processed_${item}`;
  }
  
  // Méthodes utilitaires async
  async toArray() {
    const results = [];
    for await (const item of this) {
      results.push(item);
    }
    return results;
  }
  
  async forEach(callback) {
    for await (const item of this) {
      await callback(item);
    }
  }
  
  async map(mapper) {
    const results = [];
    for await (const item of this) {
      results.push(await mapper(item));
    }
    return results;
  }
  
  async filter(predicate) {
    const results = [];
    for await (const item of this) {
      if (await predicate(item)) {
        results.push(item);
      }
    }
    return results;
  }
  
  async find(predicate) {
    for await (const item of this) {
      if (await predicate(item)) {
        return item;
      }
    }
    return undefined;
  }
}

// Itérateur async pour polling d'API
class ApiPollingIterator {
  constructor(apiCall, options = {}) {
    this.apiCall = apiCall;
    this.interval = options.interval || 1000;
    this.maxRetries = options.maxRetries || 3;
    this.timeout = options.timeout || 30000;
    this.shouldStop = false;
    this.retryCount = 0;
  }
  
  [Symbol.asyncIterator]() {
    return this;
  }
  
  async next() {
    if (this.shouldStop) {
      return { value: undefined, done: true };
    }
    
    try {
      const startTime = Date.now();
      const result = await this.apiCall();
      
      // Reset retry count on success
      this.retryCount = 0;
      
      // Wait for interval (minus execution time)
      const executionTime = Date.now() - startTime;
      const waitTime = Math.max(0, this.interval - executionTime);
      
      if (waitTime > 0) {
        await new Promise(resolve => setTimeout(resolve, waitTime));
      }
      
      return { value: result, done: false };
      
    } catch (error) {
      this.retryCount++;
      
      if (this.retryCount >= this.maxRetries) {
        this.shouldStop = true;
        throw new Error(`API polling failed after ${this.maxRetries} retries: ${error.message}`);
      }
      
      // Exponential backoff
      const backoffTime = Math.min(1000 * Math.pow(2, this.retryCount - 1), 10000);
      await new Promise(resolve => setTimeout(resolve, backoffTime));
      
      // Retry
      return this.next();
    }
  }
  
  stop() {
    this.shouldStop = true;
  }
  
  async takeUntil(predicate) {
    const results = [];
    
    for await (const item of this) {
      results.push(item);
      
      if (await predicate(item)) {
        this.stop();
        break;
      }
    }
    
    return results;
  }
  
  async takeFor(duration) {
    const results = [];
    const startTime = Date.now();
    
    for await (const item of this) {
      results.push(item);
      
      if (Date.now() - startTime >= duration) {
        this.stop();
        break;
      }
    }
    
    return results;
  }
}

// Tests des async iterators
async function testAsyncIterators() {
  console.log('=== Async Data Stream ===');
  
  const dataSource = Array.from({ length: 25 }, (_, i) => `item_${i}`);
  const stream = new AsyncDataStream(dataSource, { batchSize: 5, delay: 50 });
  
  console.time('Stream processing');
  let count = 0;
  for await (const item of stream) {
    console.log(`Received: ${item}`);
    count++;
    if (count >= 10) break; // Arrêter après 10 items
  }
  console.timeEnd('Stream processing');
  
  console.log('\n=== API Polling ===');
  
  // Simuler un appel API
  let apiCallCount = 0;
  const mockApiCall = async () => {
    apiCallCount++;
    
    // Simuler une erreur occasionnelle
    if (apiCallCount === 3) {
      throw new Error('Temporary API error');
    }
    
    return {
      timestamp: new Date().toISOString(),
      data: `Response ${apiCallCount}`,
      status: 'success'
    };
  };
  
  const poller = new ApiPollingIterator(mockApiCall, {
    interval: 200,
    maxRetries: 3
  });
  
  try {
    const results = await poller.takeFor(1000); // Poll for 1 second
    console.log(`Received ${results.length} responses:`);
    results.forEach((result, index) => {
      console.log(`${index + 1}: ${result.data} at ${result.timestamp}`);
    });
  } catch (error) {
    console.error('Polling error:', error.message);
  }
}

// testAsyncIterators();
```
{% endtab %}

{% tab title="🎰 Générateurs sophistiqués" %}
**Générateurs pour control flow et lazy evaluation**

```javascript
// Générateur pour fibonacci avec memoization
function* fibonacciGenerator() {
  const cache = new Map([[0, 0], [1, 1]]);
  let a = 0, b = 1, index = 0;
  
  while (true) {
    yield cache.get(index) ?? (cache.set(index, a), a);
    [a, b] = [b, a + b];
    index++;
  }
}

// Générateur pour nombres premiers avec optimisations
function* primeGenerator() {
  const primes = [];
  let candidate = 2;
  
  while (true) {
    if (isPrimeOptimized(candidate, primes)) {
      primes.push(candidate);
      yield candidate;
    }
    candidate += candidate === 2 ? 1 : 2; // Skip even numbers after 2
  }
}

function isPrimeOptimized(n, knownPrimes) {
  if (n < 2) return false;
  
  const sqrt = Math.sqrt(n);
  for (const prime of knownPrimes) {
    if (prime > sqrt) break;
    if (n % prime === 0) return false;
  }
  
  return true;
}

// Générateur pour permutations
function* permutations(array) {
  if (array.length <= 1) {
    yield array;
    return;
  }
  
  for (let i = 0; i < array.length; i++) {
    const rest = [...array.slice(0, i), ...array.slice(i + 1)];
    
    for (const perm of permutations(rest)) {
      yield [array[i], ...perm];
    }
  }
}

// Générateur pour combinaisons avec lazy evaluation
function* combinations(array, r) {
  if (r === 0) {
    yield [];
    return;
  }
  
  if (r > array.length) {
    return;
  }
  
  const [first, ...rest] = array;
  
  // Combinaisons incluant le premier élément
  for (const combo of combinations(rest, r - 1)) {
    yield [first, ...combo];
  }
  
  // Combinaisons excluant le premier élément
  yield* combinations(rest, r);
}

// Générateur pour backtracking (résolution de sudoku)
function* sudokuSolver(grid) {
  const size = 9;
  const boxSize = 3;
  
  function isValid(grid, row, col, num) {
    // Vérifier la ligne
    for (let x = 0; x < size; x++) {
      if (grid[row][x] === num) return false;
    }
    
    // Vérifier la colonne
    for (let x = 0; x < size; x++) {
      if (grid[x][col] === num) return false;
    }
    
    // Vérifier le carré 3x3
    const startRow = row - row % boxSize;
    const startCol = col - col % boxSize;
    
    for (let i = 0; i < boxSize; i++) {
      for (let j = 0; j < boxSize; j++) {
        if (grid[i + startRow][j + startCol] === num) {
          return false;
        }
      }
    }
    
    return true;
  }
  
  function findEmpty(grid) {
    for (let i = 0; i < size; i++) {
      for (let j = 0; j < size; j++) {
        if (grid[i][j] === 0) {
          return [i, j];
        }
      }
    }
    return null;
  }
  
  function* solve(grid) {
    const emptyCell = findEmpty(grid);
    
    if (!emptyCell) {
      yield grid.map(row => [...row]); // Solution trouvée
      return;
    }
    
    const [row, col] = emptyCell;
    
    for (let num = 1; num <= 9; num++) {
      if (isValid(grid, row, col, num)) {
        grid[row][col] = num;
        
        yield* solve(grid); // Récursion avec générateur
        
        grid[row][col] = 0; // Backtrack
      }
    }
  }
  
  yield* solve(grid);
}

// Générateur pour streams de données transformées
function* transformStream(source, ...transformers) {
  for (const item of source) {
    let result = item;
    
    for (const transformer of transformers) {
      result = transformer(result);
      
      // Si un transformer retourne undefined, skip cet item
      if (result === undefined) {
        break;
      }
    }
    
    if (result !== undefined) {
      yield result;
    }
  }
}

// Pipeline de générateurs
class GeneratorPipeline {
  constructor(source) {
    this.source = source;
    this.operations = [];
  }
  
  map(mapper) {
    this.operations.push(function*(source) {
      for (const item of source) {
        yield mapper(item);
      }
    });
    return this;
  }
  
  filter(predicate) {
    this.operations.push(function*(source) {
      for (const item of source) {
        if (predicate(item)) {
          yield item;
        }
      }
    });
    return this;
  }
  
  take(count) {
    this.operations.push(function*(source) {
      let taken = 0;
      for (const item of source) {
        if (taken >= count) break;
        yield item;
        taken++;
      }
    });
    return this;
  }
  
  skip(count) {
    this.operations.push(function*(source) {
      let skipped = 0;
      for (const item of source) {
        if (skipped < count) {
          skipped++;
          continue;
        }
        yield item;
      }
    });
    return this;
  }
  
  chunk(size) {
    this.operations.push(function*(source) {
      let chunk = [];
      for (const item of source) {
        chunk.push(item);
        if (chunk.length === size) {
          yield chunk;
          chunk = [];
        }
      }
      if (chunk.length > 0) {
        yield chunk;
      }
    });
    return this;
  }
  
  *[Symbol.iterator]() {
    let current = this.source;
    
    for (const operation of this.operations) {
      current = operation(current);
    }
    
    yield* current;
  }
  
  toArray() {
    return [...this];
  }
  
  reduce(reducer, initialValue) {
    let accumulator = initialValue;
    let hasInitial = arguments.length > 1;
    
    for (const item of this) {
      if (!hasInitial) {
        accumulator = item;
        hasInitial = true;
      } else {
        accumulator = reducer(accumulator, item);
      }
    }
    
    return accumulator;
  }
  
  find(predicate) {
    for (const item of this) {
      if (predicate(item)) {
        return item;
      }
    }
    return undefined;
  }
  
  count() {
    let count = 0;
    for (const item of this) {
      count++;
    }
    return count;
  }
}

// Tests des générateurs
console.log('=== Fibonacci Generator ===');
const fib = fibonacciGenerator();
const first10Fib = [];
for (let i = 0; i < 10; i++) {
  first10Fib.push(fib.next().value);
}
console.log('First 10 Fibonacci:', first10Fib);

console.log('\n=== Prime Generator ===');
const primes = primeGenerator();
const first10Primes = [];
for (let i = 0; i < 10; i++) {
  first10Primes.push(primes.next().value);
}
console.log('First 10 primes:', first10Primes);

console.log('\n=== Permutations ===');
const perms = [...permutations(['A', 'B', 'C'])];
console.log('Permutations of [A,B,C]:', perms);

console.log('\n=== Combinations ===');
const combos = [...combinations([1, 2, 3, 4], 2)];
console.log('Combinations of [1,2,3,4] choose 2:', combos);

console.log('\n=== Generator Pipeline ===');
const pipeline = new GeneratorPipeline(function*() {
  for (let i = 1; i <= 100; i++) {
    yield i;
  }
}())
  .filter(x => x % 2 === 0)    // Nombres pairs
  .map(x => x * x)             // Carré
  .skip(2)                     // Skip les 2 premiers
  .take(5)                     // Prendre 5
  .chunk(2);                   // Grouper par 2

console.log('Pipeline result:', pipeline.toArray());

console.log('\n=== Performance Comparison ===');
// Comparaison performance: générateur vs array
console.time('Array approach');
const arrayResult = Array.from({ length: 1000000 }, (_, i) => i + 1)
  .filter(x => x % 2 === 0)
  .map(x => x * x)
  .slice(2, 7);
console.timeEnd('Array approach');

console.time('Generator approach');
const generatorResult = new GeneratorPipeline(function*() {
  for (let i = 1; i <= 1000000; i++) {
    yield i;
  }
}())
  .filter(x => x % 2 === 0)
  .map(x => x * x)
  .skip(2)
  .take(5)
  .toArray();
console.timeEnd('Generator approach');

console.log('Results equal:', JSON.stringify(arrayResult) === JSON.stringify(generatorResult));
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Système Avancé de Concepts

{% hint style="warning" %}
**Défi :** Créez un système complet utilisant regex, itérateurs, générateurs, Proxy et WeakMap
{% endhint %}

```javascript
// Créez un système de parsing et traitement de logs sophistiqué
interface LogProcessingSystem {
    // TODO: Regex patterns pour différents formats
    patterns: {
        apache: RegExp;
        nginx: RegExp;
        application: RegExp;
        error: RegExp;
        security: RegExp;
    };
    
    // TODO: Itérateurs pour streaming
    iterators: {
        fileStream: AsyncIterator<string>;
        batchProcessor: Iterator<LogEntry[]>;
        filteredStream: Iterator<LogEntry>;
    };
    
    // TODO: Générateurs pour analysis
    generators: {
        anomalyDetector: Generator<AnomalyReport>;
        trendAnalyzer: Generator<TrendData>;
        alertGenerator: Generator<Alert>;
    };
    
    // TODO: Proxy pour monitoring
    monitoring: {
        performanceProxy: ProxyHandler<LogProcessor>;
        cacheProxy: ProxyHandler<LogCache>;
        securityProxy: ProxyHandler<SecurityAnalyzer>;
    };
    
    // TODO: WeakMap pour metadata
    metadata: {
        processingStats: WeakMap<LogEntry, ProcessingMetadata>;
        userSessions: WeakMap<User, SessionData>;
        alertHistory: WeakMap<Alert, AlertMetadata>;
    };
}

// TODO: Implémentez le système complet
class AdvancedLogSystem {
    constructor() {
        this.setupPatterns();
        this.setupIterators();
        this.setupGenerators();
        this.setupProxies();
        this.setupWeakMaps();
    }
    
    // TODO: Configuration regex sophistiquée
    setupPatterns() {
        // Patterns pour différents formats de logs
        // Extraction de métadonnées complexes
        // Validation et parsing robuste
    }
    
    // TODO: Itérateurs pour streaming de fichiers
    setupIterators() {
        // Lecture de gros fichiers par chunks
        // Traitement en batch pour performance
        // Filtrage et transformation en pipeline
    }
    
    // TODO: Générateurs pour analyse temps réel
    setupGenerators() {
        // Détection d'anomalies en streaming
        // Analyse de tendances avec fenêtres glissantes
        // Génération d'alertes basées sur patterns
    }
    
    // TODO: Proxies pour monitoring et cache
    setupProxies() {
        // Interception des accès pour métriques
        // Cache intelligent avec invalidation
        // Sécurité et audit trail automatique
    }
    
    // TODO: WeakMaps pour métadonnées
    setupWeakMaps() {
        // Association de données temporaires
        // Gestion automatique de la mémoire
        // Sessions et contexte utilisateur
    }
    
    // TODO: API principale
    async processLogs(source) {
        // Pipeline complet de traitement
        // Intégration de tous les concepts
        // Retour de résultats et métriques
    }
}

// Cas d'usage complexe
const logSystem = new AdvancedLogSystem();

// TODO: Traitez différents types de logs
const apacheLogs = `
127.0.0.1 - user1 [25/Dec/2023:10:00:00 +0000] "GET /api/users HTTP/1.1" 200 1234
192.168.1.100 - - [25/Dec/2023:10:01:00 +0000] "POST /api/login HTTP/1.1" 401 567
10.0.0.50 - admin [25/Dec/2023:10:02:00 +0000] "DELETE /api/users/123 HTTP/1.1" 200 89
`;

const applicationLogs = `
[INFO] 2023-12-25T10:00:00.123Z UserService: User login successful for user123
[ERROR] 2023-12-25T10:01:00.456Z AuthService: Invalid credentials for user456
[WARN] 2023-12-25T10:02:00.789Z RateLimit: Rate limit exceeded for IP 192.168.1.100
`;

// TODO: Analysez et générez des insights
async function demonstrateLogSystem() {
    const results = await logSystem.processLogs([apacheLogs, applicationLogs]);
    
    console.log('Processing Results:', results);
    // Anomalies détectées
    // Tendances identifiées  
    // Alertes générées
    // Métriques de performance
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```javascript
// Structure de base
class LogProcessor {
  constructor() {
    // Regex pour parsing
    this.patterns = new Map();
    
    // Metadata storage
    this.metadata = new WeakMap();
    
    // Monitoring proxy
    this.proxy = new Proxy(this, {
      get(target, prop) {
        // Log access
        return target[prop];
      }
    });
  }
  
  *processStream(logs) {
    for (const log of logs) {
      const parsed = this.parseLog(log);
      if (parsed) {
        yield parsed;
      }
    }
  }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
Solution complète avec tous les concepts avancés intégrés dans un système de traitement de logs sophistiqué.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Bravo !** Vous maîtrisez maintenant les concepts JavaScript les plus avancés !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **Expressions régulières** complexes et performantes
- **Itérateurs personnalisés** avec logiques sophistiquées  
- **Générateurs avancés** pour lazy evaluation
- **Async iterators** pour streaming de données
- **Proxy et Reflect** pour métaprogrammation
- **WeakMap/WeakSet** pour optimisation mémoire
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Parser des formats de données complexes avec regex
- Créer des pipelines de traitement efficaces
- Implémenter des algorithmes avec générateurs
- Gérer des flux de données asynchrones
- Intercepter et modifier le comportement d'objets
- Optimiser l'utilisation mémoire des applications
{% endtab %}

{% tab title="🚀 Applications réelles" %}
- **Parsers et compilateurs** avec regex sophistiquées
- **Streaming de données** massives avec itérateurs
- **Frameworks et ORMs** avec Proxy pour APIs fluides
- **Algorithmes avancés** avec générateurs optimisés
- **Système de cache** intelligent avec WeakMap
- **Outils de développement** avec métaprogrammation
{% endtab %}
{% endtabs %}

---

**Félicitations ! Vous avez terminé le niveau avancé JavaScript ! 🎉**
