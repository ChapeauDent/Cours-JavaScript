# 🔌 WebSockets et Socket.IO

> **Niveau :** 🔴 Avancé | **Durée :** 60 minutes | **Prérequis :** Event Loop, APIs, Node.js

## 🎯 Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :

- [ ] **Maîtriser** WebSockets natifs et Socket.IO avancé
- [ ] **Créer** des applications temps réel robustes et scalables
- [ ] **Implémenter** des patterns de communication sophistiqués
- [ ] **Optimiser** les performances et la resilience

---

## 📚 Communication Temps Réel

{% tabs %}
{% tab title="⚡ Évolution des communications web" %}
**De HTTP à WebSockets : révolution temps réel**

### Limitations du HTTP classique
```
Client → Request → Serveur
Client ← Response ← Serveur
🔌 Connexion fermée

Problèmes :
- Latence élevée (handshake à chaque requête)
- Impossible pour le serveur d'initier
- Overhead des headers HTTP
- Polling inefficace pour le temps réel
```

### WebSockets : communication bidirectionnelle
```
Client ↔ Connexion persistante ↔ Serveur
       Messages instantanés
       
Avantages :
- Latence ultra-faible (<50ms)
- Communication bidirectionnelle
- Overhead minimal
- Push serveur natif
```

### Comparaison des approches temps réel
| Technique | Latence | Bandwidth | Complexité | Support |
|-----------|---------|-----------|------------|---------|
| **Polling** | 2-5s | 🔴 Élevé | 🟢 Simple | ✅ Universel |
| **Long Polling** | 0.5-2s | 🟡 Moyen | 🟡 Moyen | ✅ Bon |
| **Server-Sent Events** | <200ms | 🟢 Faible | 🟡 Moyen | 🟡 Moderne |
| **WebSockets** | <50ms | ✅ Minimal | 🔴 Complexe | ✅ Moderne |

### Cas d'usage par approche
```javascript
const realTimeUseCases = {
  // WebSockets recommandés
  gaming: 'Jeux multijoueurs temps réel',
  trading: 'Données financières live',
  chat: 'Messagerie instantanée',
  collaboration: 'Édition collaborative',
  
  // SSE suffisants
  notifications: 'Alertes push',
  feeds: 'Flux de données',
  monitoring: 'Dashboards temps réel',
  
  // Polling acceptable
  status: 'Statuts occasionnels',
  weather: 'Données peu fréquentes'
};
```
{% endtab %}

{% tab title="🏗️ Architecture WebSocket" %}
**Anatomie d'une connexion WebSocket :**

### Handshake HTTP → WebSocket
```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

### Lifecycle d'une connexion
```typescript
interface WebSocketLifecycle {
  // États de connexion
  states: {
    CONNECTING: 0;    // En cours de connexion
    OPEN: 1;         // Connexion établie
    CLOSING: 2;      // Fermeture en cours
    CLOSED: 3;       // Connexion fermée
  };
  
  // Événements
  events: {
    onopen: (event: Event) => void;
    onmessage: (event: MessageEvent) => void;
    onerror: (event: Event) => void;
    onclose: (event: CloseEvent) => void;
  };
  
  // Méthodes
  methods: {
    send: (data: string | ArrayBuffer | Blob) => void;
    close: (code?: number, reason?: string) => void;
  };
}
```

### Gestion des états avancée
```typescript
class AdvancedWebSocket {
  private ws: WebSocket | null = null;
  private reconnectAttempts = 0;
  private maxReconnectAttempts = 5;
  private reconnectDelay = 1000;
  
  constructor(private url: string) {
    this.connect();
  }
  
  private connect() {
    try {
      this.ws = new WebSocket(this.url);
      this.setupEventListeners();
    } catch (error) {
      console.error('WebSocket connection failed:', error);
      this.scheduleReconnect();
    }
  }
  
  private setupEventListeners() {
    if (!this.ws) return;
    
    this.ws.onopen = (event) => {
      console.log('WebSocket connected');
      this.reconnectAttempts = 0;
      this.onConnected(event);
    };
    
    this.ws.onmessage = (event) => {
      this.handleMessage(event.data);
    };
    
    this.ws.onerror = (error) => {
      console.error('WebSocket error:', error);
      this.onError(error);
    };
    
    this.ws.onclose = (event) => {
      console.log('WebSocket closed:', event.code, event.reason);
      this.onDisconnected(event);
      
      // Reconnexion automatique
      if (!event.wasClean && this.shouldReconnect(event.code)) {
        this.scheduleReconnect();
      }
    };
  }
  
  private shouldReconnect(code: number): boolean {
    // Codes qui ne nécessitent pas de reconnexion
    const noReconnectCodes = [1000, 1001, 1005, 4000];
    return !noReconnectCodes.includes(code) && 
           this.reconnectAttempts < this.maxReconnectAttempts;
  }
  
  private scheduleReconnect() {
    if (this.reconnectAttempts >= this.maxReconnectAttempts) {
      console.error('Max reconnection attempts reached');
      return;
    }
    
    const delay = this.reconnectDelay * Math.pow(2, this.reconnectAttempts);
    this.reconnectAttempts++;
    
    setTimeout(() => {
      console.log(`Reconnection attempt ${this.reconnectAttempts}`);
      this.connect();
    }, delay);
  }
  
  send(data: any) {
    if (this.ws?.readyState === WebSocket.OPEN) {
      this.ws.send(JSON.stringify(data));
    } else {
      console.warn('WebSocket not open, message queued');
      this.queueMessage(data);
    }
  }
  
  // Hooks à implémenter
  protected onConnected(event: Event) {}
  protected onDisconnected(event: CloseEvent) {}
  protected onError(error: Event) {}
  protected handleMessage(data: string) {}
  protected queueMessage(data: any) {}
}
```

### Sécurité WebSocket
```typescript
class SecureWebSocket extends AdvancedWebSocket {
  private authToken: string | null = null;
  private heartbeatInterval: number | null = null;
  
  constructor(url: string, authToken?: string) {
    super(url);
    this.authToken = authToken;
  }
  
  protected onConnected(event: Event) {
    // Authentification au niveau application
    if (this.authToken) {
      this.send({
        type: 'auth',
        token: this.authToken
      });
    }
    
    // Heartbeat pour maintenir la connexion
    this.startHeartbeat();
  }
  
  private startHeartbeat() {
    this.heartbeatInterval = window.setInterval(() => {
      this.send({ type: 'ping', timestamp: Date.now() });
    }, 30000); // Ping toutes les 30 secondes
  }
  
  protected handleMessage(data: string) {
    try {
      const message = JSON.parse(data);
      
      switch (message.type) {
        case 'auth_success':
          console.log('Authentication successful');
          break;
          
        case 'auth_failed':
          console.error('Authentication failed');
          this.ws?.close(4001, 'Authentication failed');
          break;
          
        case 'pong':
          // Réponse au ping
          break;
          
        default:
          this.onApplicationMessage(message);
      }
    } catch (error) {
      console.error('Failed to parse message:', error);
    }
  }
  
  protected onApplicationMessage(message: any) {
    // À implémenter par les classes filles
  }
  
  protected onDisconnected(event: CloseEvent) {
    if (this.heartbeatInterval) {
      clearInterval(this.heartbeatInterval);
      this.heartbeatInterval = null;
    }
  }
}
```
{% endtab %}

{% tab title="🌟 Socket.IO avancé" %}
**Socket.IO : WebSockets avec superpowers**

### Avantages de Socket.IO
```typescript
const socketIOAdvantages = {
  // Fallbacks automatiques
  transports: ['websocket', 'polling'],
  
  // Fonctionnalités avancées
  features: {
    rooms: 'Groupes de connexions',
    namespaces: 'Séparation logique',
    events: 'Système d événements typés',
    acknowledgments: 'Confirmation de réception',
    broadcasting: 'Diffusion ciblée',
    middleware: 'Pipeline de traitement'
  },
  
  // Resilience
  resilience: {
    reconnection: 'Automatique avec backoff',
    buffering: 'Messages en attente',
    heartbeat: 'Détection de déconnexion'
  }
};
```

### Client Socket.IO sophistiqué
```typescript
import { io, Socket } from 'socket.io-client';

interface ServerToClientEvents {
  message: (data: ChatMessage) => void;
  userJoined: (user: User) => void;
  userLeft: (userId: string) => void;
  typing: (data: TypingData) => void;
  error: (error: ErrorData) => void;
}

interface ClientToServerEvents {
  sendMessage: (message: string, callback: (id: string) => void) => void;
  joinRoom: (roomId: string) => void;
  startTyping: () => void;
  stopTyping: () => void;
}

class ChatClient {
  private socket: Socket<ServerToClientEvents, ClientToServerEvents>;
  private currentRoom: string | null = null;
  private typingTimeout: number | null = null;
  
  constructor(private serverUrl: string, private authToken: string) {
    this.socket = io(serverUrl, {
      auth: { token: authToken },
      transports: ['websocket', 'polling'],
      timeout: 5000,
      retries: 3,
      
      // Configuration de reconnexion
      reconnection: true,
      reconnectionAttempts: 5,
      reconnectionDelay: 1000,
      reconnectionDelayMax: 5000,
      
      // Configuration avancée
      forceNew: false,
      multiplex: true
    });
    
    this.setupEventListeners();
  }
  
  private setupEventListeners() {
    // Événements de connexion
    this.socket.on('connect', () => {
      console.log('Connected to server:', this.socket.id);
    });
    
    this.socket.on('disconnect', (reason) => {
      console.log('Disconnected:', reason);
      
      if (reason === 'io server disconnect') {
        // Déconnexion serveur, reconnexion manuelle nécessaire
        this.socket.connect();
      }
    });
    
    this.socket.on('connect_error', (error) => {
      console.error('Connection error:', error);
    });
    
    // Événements métier
    this.socket.on('message', (data) => {
      this.handleNewMessage(data);
    });
    
    this.socket.on('userJoined', (user) => {
      this.handleUserJoined(user);
    });
    
    this.socket.on('userLeft', (userId) => {
      this.handleUserLeft(userId);
    });
    
    this.socket.on('typing', (data) => {
      this.handleTyping(data);
    });
    
    this.socket.on('error', (error) => {
      this.handleError(error);
    });
  }
  
  // Actions client
  joinRoom(roomId: string) {
    this.currentRoom = roomId;
    this.socket.emit('joinRoom', roomId);
  }
  
  sendMessage(message: string): Promise<string> {
    return new Promise((resolve, reject) => {
      if (!this.socket.connected) {
        reject(new Error('Not connected'));
        return;
      }
      
      this.socket.emit('sendMessage', message, (messageId) => {
        resolve(messageId);
      });
      
      // Timeout si pas de réponse
      setTimeout(() => {
        reject(new Error('Message timeout'));
      }, 5000);
    });
  }
  
  startTyping() {
    this.socket.emit('startTyping');
    
    // Auto-stop après inactivité
    if (this.typingTimeout) {
      clearTimeout(this.typingTimeout);
    }
    
    this.typingTimeout = window.setTimeout(() => {
      this.stopTyping();
    }, 3000);
  }
  
  stopTyping() {
    if (this.typingTimeout) {
      clearTimeout(this.typingTimeout);
      this.typingTimeout = null;
    }
    this.socket.emit('stopTyping');
  }
  
  // Gestionnaires d'événements
  private handleNewMessage(data: ChatMessage) {
    console.log('New message:', data);
    // Mettre à jour l'UI
  }
  
  private handleUserJoined(user: User) {
    console.log('User joined:', user.name);
    // Notification d'arrivée
  }
  
  private handleUserLeft(userId: string) {
    console.log('User left:', userId);
    // Notification de départ
  }
  
  private handleTyping(data: TypingData) {
    console.log(`${data.userName} is typing...`);
    // Indicateur de frappe
  }
  
  private handleError(error: ErrorData) {
    console.error('Server error:', error);
    // Gestion d'erreur
  }
  
  disconnect() {
    this.socket.disconnect();
  }
}

// Types
interface ChatMessage {
  id: string;
  userId: string;
  userName: string;
  content: string;
  timestamp: number;
  roomId: string;
}

interface User {
  id: string;
  name: string;
  avatar?: string;
}

interface TypingData {
  userId: string;
  userName: string;
  roomId: string;
}

interface ErrorData {
  code: string;
  message: string;
}
```

### Serveur Socket.IO avancé
```typescript
import { Server } from 'socket.io';
import { createServer } from 'http';
import jwt from 'jsonwebtoken';

interface SocketData {
  userId: string;
  userName: string;
}

const httpServer = createServer();
const io = new Server<
  ClientToServerEvents,
  ServerToClientEvents,
  {},
  SocketData
>(httpServer, {
  cors: {
    origin: process.env.CLIENT_URL,
    credentials: true
  },
  
  // Configuration des transports
  transports: ['websocket', 'polling'],
  
  // Configuration avancée
  pingTimeout: 60000,
  pingInterval: 25000,
  
  // Gestion des connexions
  maxHttpBufferSize: 1e6, // 1MB
  allowEIO3: true
});

// Middleware d'authentification
io.use(async (socket, next) => {
  try {
    const token = socket.handshake.auth.token;
    const decoded = jwt.verify(token, process.env.JWT_SECRET!) as any;
    
    // Récupérer les infos utilisateur
    const user = await getUserById(decoded.userId);
    if (!user) {
      return next(new Error('User not found'));
    }
    
    // Stocker dans socket.data
    socket.data.userId = user.id;
    socket.data.userName = user.name;
    
    next();
  } catch (error) {
    next(new Error('Authentication failed'));
  }
});

// Gestion des connexions
io.on('connection', (socket) => {
  console.log(`User ${socket.data.userName} connected`);
  
  // Rejoindre une room
  socket.on('joinRoom', async (roomId) => {
    try {
      // Vérifier les permissions
      const canJoin = await checkRoomPermissions(socket.data.userId, roomId);
      if (!canJoin) {
        socket.emit('error', { code: 'PERMISSION_DENIED', message: 'Cannot join room' });
        return;
      }
      
      // Quitter les anciennes rooms
      socket.rooms.forEach(room => {
        if (room !== socket.id) {
          socket.leave(room);
        }
      });
      
      // Rejoindre la nouvelle room
      socket.join(roomId);
      
      // Notifier les autres
      socket.to(roomId).emit('userJoined', {
        id: socket.data.userId,
        name: socket.data.userName
      });
      
    } catch (error) {
      socket.emit('error', { code: 'JOIN_ERROR', message: error.message });
    }
  });
  
  // Envoyer un message
  socket.on('sendMessage', async (message, callback) => {
    try {
      // Validation et nettoyage
      const cleanMessage = sanitizeMessage(message);
      if (!cleanMessage.trim()) {
        callback(''); // Message vide
        return;
      }
      
      // Récupérer la room actuelle
      const currentRoom = getCurrentRoom(socket);
      if (!currentRoom) {
        socket.emit('error', { code: 'NO_ROOM', message: 'Not in a room' });
        return;
      }
      
      // Sauvegarder en base
      const messageData: ChatMessage = {
        id: generateId(),
        userId: socket.data.userId,
        userName: socket.data.userName,
        content: cleanMessage,
        timestamp: Date.now(),
        roomId: currentRoom
      };
      
      await saveMessage(messageData);
      
      // Diffuser à tous dans la room
      io.to(currentRoom).emit('message', messageData);
      
      // Confirmation au sender
      callback(messageData.id);
      
    } catch (error) {
      socket.emit('error', { code: 'SEND_ERROR', message: error.message });
      callback('');
    }
  });
  
  // Indicateurs de frappe
  socket.on('startTyping', () => {
    const currentRoom = getCurrentRoom(socket);
    if (currentRoom) {
      socket.to(currentRoom).emit('typing', {
        userId: socket.data.userId,
        userName: socket.data.userName,
        roomId: currentRoom
      });
    }
  });
  
  socket.on('stopTyping', () => {
    const currentRoom = getCurrentRoom(socket);
    if (currentRoom) {
      socket.to(currentRoom).emit('typing', {
        userId: socket.data.userId,
        userName: socket.data.userName,
        roomId: currentRoom
      });
    }
  });
  
  // Déconnexion
  socket.on('disconnect', (reason) => {
    console.log(`User ${socket.data.userName} disconnected: ${reason}`);
    
    // Notifier les rooms
    socket.rooms.forEach(room => {
      if (room !== socket.id) {
        socket.to(room).emit('userLeft', socket.data.userId);
      }
    });
  });
});

// Fonctions utilitaires
function getCurrentRoom(socket: Socket): string | null {
  const rooms = Array.from(socket.rooms);
  return rooms.find(room => room !== socket.id) || null;
}

function sanitizeMessage(message: string): string {
  return message.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '');
}

async function getUserById(userId: string): Promise<User | null> {
  // Implémentation base de données
  return null;
}

async function checkRoomPermissions(userId: string, roomId: string): Promise<boolean> {
  // Vérification des permissions
  return true;
}

async function saveMessage(message: ChatMessage): Promise<void> {
  // Sauvegarde en base
}

function generateId(): string {
  return Math.random().toString(36).substr(2, 9);
}

httpServer.listen(3000, () => {
  console.log('Socket.IO server listening on port 3000');
});
```
{% endtab %}
{% endtabs %}

---

## 🎯 Exercice Pratique : Application Temps Réel Complète

{% hint style="warning" %}
**Exercice :** Créez une application temps réel complète avec WebSockets/Socket.IO
{% endhint %}

```typescript
// Créez une application temps réel sophistiquée
interface RealTimeApplication {
    // TODO: Architecture client/serveur
    architecture: {
        client: 'WebSocket natif' | 'Socket.IO';
        server: 'Node.js' | 'Deno' | 'Python';
        database: 'Redis' | 'MongoDB' | 'PostgreSQL';
        scaling: boolean;
    };
    
    // TODO: Fonctionnalités temps réel
    features: {
        chat: boolean;
        notifications: boolean;
        collaboration: boolean;
        gaming: boolean;
        monitoring: boolean;
    };
    
    // TODO: Patterns avancés
    patterns: {
        rooms: boolean;
        namespaces: boolean;
        authentication: boolean;
        rateLimit: boolean;
        compression: boolean;
    };
    
    // TODO: Performance et scaling
    scaling: {
        clustering: boolean;
        loadBalancing: boolean;
        horizontalScale: boolean;
        caching: boolean;
    };
}

// TODO: Implémentez l'application complète
class AdvancedRealTimeApp {
    constructor(config: RealTimeApplication) {}
    
    // TODO: Client sophistiqué
    setupClient() {
        // Connexion resiliente
        // Gestion des événements
        // Authentification
        // UI temps réel
    }
    
    // TODO: Serveur scalable
    setupServer() {
        // Socket.IO server
        // Middleware pipeline
        // Room management
        // Rate limiting
        // Horizontal scaling
    }
    
    // TODO: Persistence et cache
    setupDataLayer() {
        // Redis pour sessions
        // MongoDB pour messages
        // Cache strategy
        // Real-time sync
    }
    
    // TODO: Monitoring et analytics
    setupMonitoring() {
        // Connection metrics
        // Message throughput
        // Error tracking
        // Performance monitoring
    }
}

// Cas d'usage spécifiques
const applicationTypes = {
    // Chat en temps réel
    chat: {
        features: ['rooms', 'private messages', 'file sharing', 'typing indicators'],
        scaling: 'horizontal with Redis adapter'
    },
    
    // Collaboration en temps réel
    collaboration: {
        features: ['operational transform', 'conflict resolution', 'presence'],
        scaling: 'clustered with CRDT'
    },
    
    // Jeu multijoueur
    gaming: {
        features: ['game state sync', 'player matching', 'spectators'],
        scaling: 'game room isolation'
    },
    
    // Dashboard de monitoring
    monitoring: {
        features: ['real-time metrics', 'alerts', 'historical data'],
        scaling: 'time-series database'
    }
};

// Tests et validation
async function testRealTimeApplication() {
    // TODO: Tests de performance
    // TODO: Tests de resilience  
    // TODO: Tests de scaling
    // TODO: Tests de sécurité
}
```

{% tabs %}
{% tab title="💡 Indice" %}
```typescript
// Architecture de base
class RealTimeChat {
  constructor() {
    this.setupSocketIO();
    this.setupRedis();
    this.setupAuthentication();
  }
  
  setupSocketIO() {
    this.io = new Server({
      cors: { origin: "*" },
      adapter: createAdapter() // Redis adapter
    });
  }
  
  setupAuthentication() {
    this.io.use(async (socket, next) => {
      const token = socket.handshake.auth.token;
      // Verify JWT token
      next();
    });
  }
}
```
{% endtab %}

{% tab title="✅ Solution" %}
Solution complète avec architecture scalable, patterns avancés et monitoring dans l'onglet suivant.
{% endtab %}
{% endtabs %}

---

## 🎓 Récapitulatif

{% hint style="success" %}
**Félicitations !** Vous maîtrisez maintenant WebSockets et Socket.IO pour des applications temps réel !
{% endhint %}

### Compétences acquises

{% tabs %}
{% tab title="🧠 Concepts clés" %}
- **WebSockets natifs** et leur cycle de vie
- **Socket.IO** avec fonctionnalités avancées
- **Patterns temps réel** (rooms, namespaces, events)
- **Authentification** et sécurité WebSocket
- **Resilience** et reconnexion automatique
- **Scaling** horizontal et clustering
{% endtab %}

{% tab title="🛠️ Compétences pratiques" %}
- Créer des connexions WebSocket robustes
- Implémenter des applications chat temps réel
- Gérer l'authentification et la sécurité
- Optimiser les performances et le scaling
- Intégrer dans des architectures complexes
- Monitorer et déboguer les connexions
{% endtab %}

{% tab title="🚀 Applications réelles" %}
- **Chat applications** avec rooms et permissions
- **Collaboration tools** (Google Docs-like)
- **Gaming platforms** multijoueurs
- **Trading platforms** avec données live
- **IoT dashboards** temps réel
- **Notification systems** push
{% endtab %}
{% endtabs %}

---

**Vous avez maintenant une base solide en JavaScript avancé ! Prêt pour les projets pratiques ?** 🏗️
