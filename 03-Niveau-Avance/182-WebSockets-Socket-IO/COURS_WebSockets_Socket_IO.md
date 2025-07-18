# WebSockets et Socket.IO

## **INFORMATIONS GÉNÉRALES**

### Métadonnées du cours
- **Titre :** WebSockets et Communication Temps Réel avec Socket.IO
- **Niveau :** 🔴 Avancé
- **Durée estimée :** 60 minutes
- **Prérequis :** Event Loop, Fetch API, Gestion d'erreurs, Node.js de base
- **Date de création :** 18/07/2025
- **Dernière révision :** 18/07/2025

---

## **OBJECTIFS PÉDAGOGIQUES**

### À la fin de cette leçon, vous serez capable de :
- [ ] **Comprendre** : La différence entre HTTP et WebSockets
- [ ] **Appliquer** : Créer des connexions WebSocket bidirectionnelles
- [ ] **Analyser** : Quand utiliser WebSockets vs autres technologies
- [ ] **Créer** : Applications temps réel (chat, notifications, jeux)

### Mots-clés à retenir
- **WebSocket** : Protocole de communication bidirectionnelle en temps réel
- **Socket.IO** : Bibliothèque qui facilite les WebSockets avec fallbacks
- **Temps réel** : Communication instantanée entre client et serveur
- **Bidirectionnel** : Communication dans les deux sens simultanément
- **Room/Channel** : Groupes de connexions pour diffusion ciblée

---

## **PARTIE THÉORIQUE**

### Pourquoi ce concept est-il important ?
Les WebSockets révolutionnent les applications web en permettant :
- **Chat en temps réel** : Messages instantanés
- **Notifications push** : Alertes immédiates
- **Jeux multijoueurs** : Synchronisation des joueurs
- **Collaboration** : Édition collaborative (Google Docs)
- **Trading/Finance** : Cours en temps réel
- **IoT/Monitoring** : Données de capteurs live

**Sans WebSockets** : il faut faire du polling (demander des nouvelles toutes les X secondes) = inefficace !

### Définition et concepts clés

**HTTP classique** : Request/Response, le client demande, le serveur répond, puis se déconnecte.
```
Client → [Request] → Serveur
Client ← [Response] ← Serveur
🔌 Connexion fermée
```

**WebSocket** : Connexion persistante, communication bidirectionnelle.
```
Client ↔ [Messages permanents] ↔ Serveur
🔗 Connexion ouverte en permanence
```

### Comparaison des approches

| Approche | Latence | Efficacité | Complexité | Cas d'usage |
|----------|---------|------------|------------|-------------|
| **Polling** | 1-5s | ❌ Faible | 🟢 Simple | Notifications rares |
| **Long Polling** | 0.5-2s | 🟡 Moyenne | 🟡 Moyenne | Updates occasionnels |
| **Server-Sent Events** | <100ms | 🟢 Bonne | 🟡 Moyenne | Flux unidirectionnel |
| **WebSockets** | <50ms | ✅ Excellente | 🔴 Complexe | Temps réel bidirectionnel |

### Points d'attention
- **Gestion des déconnexions** : Internet mobile, mise en veille
- **Authentification** : Sécuriser les connexions WebSocket
- **Scaling** : Gérer des milliers de connexions simultanées
- **Fallbacks** : Support des vieux navigateurs

---

## **PARTIE PRATIQUE**

### Exemple simple (starter)
```javascript
// WebSocket natif côté client
console.log("=== WebSocket Natif ===");

// 1. Créer la connexion
const ws = new WebSocket('wss://echo.websocket.org/');

// 2. Événements de connexion
ws.onopen = function(event) {
    console.log('✅ Connexion WebSocket ouverte');
    
    // Envoyer un message de test
    ws.send('Hello WebSocket!');
};

// 3. Recevoir des messages
ws.onmessage = function(event) {
    console.log('📨 Message reçu:', event.data);
};

// 4. Gestion des erreurs
ws.onerror = function(error) {
    console.error('❌ Erreur WebSocket:', error);
};

// 5. Fermeture de connexion
ws.onclose = function(event) {
    console.log('🔌 Connexion fermée', event.code, event.reason);
};

// Exemple d'envoi de messages après connexion
setTimeout(() => {
    if (ws.readyState === WebSocket.OPEN) {
        ws.send(JSON.stringify({
            type: 'test',
            message: 'Message JSON',
            timestamp: new Date().toISOString()
        }));
    }
}, 2000);

// Fermer proprement après 10 secondes
setTimeout(() => {
    ws.close(1000, 'Test terminé');
}, 10000);
```

**Résultat attendu :**
```
✅ Connexion WebSocket ouverte
📨 Message reçu: Hello WebSocket!
📨 Message reçu: {"type":"test","message":"Message JSON","timestamp":"2025-07-18T..."}
🔌 Connexion fermée 1000 Test terminé
```

### Exemple intermédiaire
```javascript
// Gestionnaire WebSocket avancé avec reconnexion automatique
class WebSocketManager {
    constructor(url, options = {}) {
        this.url = url;
        this.options = {
            reconnectInterval: 1000,
            maxReconnectAttempts: 5,
            heartbeatInterval: 30000,
            ...options
        };
        
        this.ws = null;
        this.reconnectAttempts = 0;
        this.isConnected = false;
        this.messageQueue = [];
        this.heartbeatTimer = null;
        
        // Callbacks personnalisés
        this.onOpenCallback = null;
        this.onMessageCallback = null;
        this.onCloseCallback = null;
        this.onErrorCallback = null;
        
        this.connect();
    }
    
    connect() {
        try {
            console.log(`🔄 Tentative de connexion à ${this.url}`);
            this.ws = new WebSocket(this.url);
            
            this.ws.onopen = (event) => {
                console.log('✅ WebSocket connecté');
                this.isConnected = true;
                this.reconnectAttempts = 0;
                
                // Envoyer les messages en attente
                this.flushMessageQueue();
                
                // Démarrer le heartbeat
                this.startHeartbeat();
                
                if (this.onOpenCallback) {
                    this.onOpenCallback(event);
                }
            };
            
            this.ws.onmessage = (event) => {
                const data = this.parseMessage(event.data);
                
                // Gérer les messages système
                if (data.type === 'pong') {
                    console.log('💓 Pong reçu');
                    return;
                }
                
                console.log('📨 Message reçu:', data);
                
                if (this.onMessageCallback) {
                    this.onMessageCallback(data);
                }
            };
            
            this.ws.onclose = (event) => {
                console.log('🔌 WebSocket fermé:', event.code, event.reason);
                this.isConnected = false;
                this.stopHeartbeat();
                
                if (this.onCloseCallback) {
                    this.onCloseCallback(event);
                }
                
                // Reconnexion automatique
                this.handleReconnect();
            };
            
            this.ws.onerror = (error) => {
                console.error('❌ Erreur WebSocket:', error);
                
                if (this.onErrorCallback) {
                    this.onErrorCallback(error);
                }
            };
            
        } catch (error) {
            console.error('💥 Erreur création WebSocket:', error);
            this.handleReconnect();
        }
    }
    
    send(data) {
        const message = typeof data === 'string' ? data : JSON.stringify(data);
        
        if (this.isConnected && this.ws.readyState === WebSocket.OPEN) {
            this.ws.send(message);
            console.log('📤 Message envoyé:', data);
        } else {
            console.log('📋 Message mis en queue:', data);
            this.messageQueue.push(message);
        }
    }
    
    flushMessageQueue() {
        while (this.messageQueue.length > 0) {
            const message = this.messageQueue.shift();
            this.ws.send(message);
            console.log('📤 Message de la queue envoyé');
        }
    }
    
    parseMessage(rawData) {
        try {
            return JSON.parse(rawData);
        } catch (error) {
            return { type: 'text', data: rawData };
        }
    }
    
    startHeartbeat() {
        this.heartbeatTimer = setInterval(() => {
            if (this.isConnected) {
                this.send({ type: 'ping', timestamp: Date.now() });
            }
        }, this.options.heartbeatInterval);
    }
    
    stopHeartbeat() {
        if (this.heartbeatTimer) {
            clearInterval(this.heartbeatTimer);
            this.heartbeatTimer = null;
        }
    }
    
    handleReconnect() {
        if (this.reconnectAttempts < this.options.maxReconnectAttempts) {
            this.reconnectAttempts++;
            const delay = this.options.reconnectInterval * this.reconnectAttempts;
            
            console.log(`🔄 Reconnexion dans ${delay}ms (tentative ${this.reconnectAttempts}/${this.options.maxReconnectAttempts})`);
            
            setTimeout(() => {
                this.connect();
            }, delay);
        } else {
            console.error('💀 Nombre maximum de tentatives de reconnexion atteint');
        }
    }
    
    close() {
        this.stopHeartbeat();
        if (this.ws) {
            this.ws.close(1000, 'Fermeture manuelle');
        }
    }
    
    // Méthodes pour définir les callbacks
    onOpen(callback) { this.onOpenCallback = callback; }
    onMessage(callback) { this.onMessageCallback = callback; }
    onClose(callback) { this.onCloseCallback = callback; }
    onError(callback) { this.onErrorCallback = callback; }
}

// Utilisation du gestionnaire
const wsManager = new WebSocketManager('wss://echo.websocket.org/', {
    reconnectInterval: 2000,
    maxReconnectAttempts: 3,
    heartbeatInterval: 15000
});

// Définir les callbacks
wsManager.onOpen(() => {
    console.log('🎉 Application connectée !');
    wsManager.send({
        type: 'join',
        user: 'Alice',
        room: 'general'
    });
});

wsManager.onMessage((data) => {
    console.log('🎯 Traitement du message:', data);
    // Traiter selon le type de message
    switch (data.type) {
        case 'chat':
            afficherMessageChat(data);
            break;
        case 'notification':
            afficherNotification(data);
            break;
        default:
            console.log('📄 Message non géré:', data);
    }
});

// Fonctions utilitaires
function afficherMessageChat(data) {
    console.log(`💬 ${data.user}: ${data.message}`);
}

function afficherNotification(data) {
    console.log(`🔔 Notification: ${data.message}`);
}

// Test d'envoi de messages
setTimeout(() => {
    wsManager.send({
        type: 'chat',
        user: 'Alice',
        message: 'Hello tout le monde !',
        room: 'general'
    });
}, 3000);
```

### Exemple avancé (optionnel)
```javascript
// Application de chat complète avec Socket.IO (côté client)
// Note: Nécessite l'inclusion de la librairie Socket.IO

class ChatApplication {
    constructor(serverUrl) {
        this.serverUrl = serverUrl;
        this.socket = null;
        this.currentUser = null;
        this.currentRoom = 'general';
        this.users = new Map();
        this.messages = [];
        
        this.initializeUI();
    }
    
    initializeUI() {
        // Créer l'interface utilisateur
        const chatContainer = document.createElement('div');
        chatContainer.innerHTML = `
            <div id="chat-app" style="max-width: 600px; margin: 20px auto; font-family: Arial;">
                <div id="login-section">
                    <h3>🚀 Connexion au Chat</h3>
                    <input type="text" id="username-input" placeholder="Votre nom d'utilisateur" />
                    <button id="connect-btn">Se connecter</button>
                </div>
                
                <div id="chat-section" style="display: none;">
                    <div style="display: flex; gap: 20px;">
                        <div style="flex: 1;">
                            <div id="chat-header">
                                <h3>💬 Chat - Salon: <span id="current-room">general</span></h3>
                                <div>Connecté en tant que: <strong id="current-user"></strong></div>
                            </div>
                            
                            <div id="messages-container" style="
                                height: 300px; 
                                border: 1px solid #ccc; 
                                padding: 10px; 
                                overflow-y: auto; 
                                background: #f9f9f9;
                                margin: 10px 0;
                            "></div>
                            
                            <div id="message-input-container">
                                <input type="text" id="message-input" placeholder="Tapez votre message..." style="width: 70%;" />
                                <button id="send-btn" style="width: 25%;">Envoyer</button>
                            </div>
                        </div>
                        
                        <div style="width: 200px;">
                            <h4>👥 Utilisateurs connectés</h4>
                            <div id="users-list" style="
                                border: 1px solid #ccc;
                                padding: 10px;
                                height: 200px;
                                overflow-y: auto;
                                background: #f0f0f0;
                            "></div>
                            
                            <h4>🏠 Salons</h4>
                            <div id="rooms-list">
                                <div class="room-item" data-room="general">📢 Général</div>
                                <div class="room-item" data-room="tech">💻 Tech</div>
                                <div class="room-item" data-room="random">🎲 Random</div>
                            </div>
                        </div>
                    </div>
                    
                    <div id="status-bar" style="
                        margin-top: 10px; 
                        padding: 5px; 
                        background: #e0e0e0; 
                        border-radius: 3px;
                        font-size: 12px;
                    ">
                        Status: <span id="connection-status">Déconnecté</span>
                    </div>
                </div>
            </div>
        `;
        
        document.body.appendChild(chatContainer);
        this.bindEvents();
    }
    
    bindEvents() {
        // Connexion
        document.getElementById('connect-btn').onclick = () => {
            const username = document.getElementById('username-input').value.trim();
            if (username) {
                this.connect(username);
            }
        };
        
        // Envoi de message
        document.getElementById('send-btn').onclick = () => {
            this.sendMessage();
        };
        
        document.getElementById('message-input').onkeypress = (e) => {
            if (e.key === 'Enter') {
                this.sendMessage();
            }
        };
        
        // Changement de salon
        document.querySelectorAll('.room-item').forEach(room => {
            room.onclick = () => {
                const roomName = room.dataset.room;
                this.joinRoom(roomName);
            };
            room.style.cssText = `
                padding: 5px; 
                cursor: pointer; 
                margin: 2px 0;
                background: #fff;
                border-radius: 3px;
            `;
            room.onmouseover = () => room.style.background = '#e0e0e0';
            room.onmouseout = () => room.style.background = '#fff';
        });
    }
    
    connect(username) {
        this.currentUser = username;
        
        // Simulation d'une connexion Socket.IO
        // En pratique: this.socket = io(this.serverUrl);
        this.socket = this.createMockSocket();
        
        this.socket.on('connect', () => {
            console.log('✅ Connecté au serveur de chat');
            this.updateStatus('Connecté');
            this.showChatInterface();
            
            // S'identifier auprès du serveur
            this.socket.emit('user:join', {
                username: this.currentUser,
                room: this.currentRoom
            });
        });
        
        this.socket.on('disconnect', () => {
            console.log('🔌 Déconnecté du serveur');
            this.updateStatus('Déconnecté');
        });
        
        this.socket.on('message', (data) => {
            this.displayMessage(data);
        });
        
        this.socket.on('user:joined', (data) => {
            this.addUser(data.user);
            this.displaySystemMessage(`${data.user.username} a rejoint le salon`);
        });
        
        this.socket.on('user:left', (data) => {
            this.removeUser(data.user.id);
            this.displaySystemMessage(`${data.user.username} a quitté le salon`);
        });
        
        this.socket.on('users:list', (users) => {
            this.updateUsersList(users);
        });
        
        this.socket.on('room:joined', (data) => {
            this.currentRoom = data.room;
            this.messages = [];
            this.clearMessages();
            this.updateCurrentRoom();
            this.displaySystemMessage(`Vous avez rejoint le salon ${data.room}`);
        });
        
        // Démarrer la connexion
        this.socket.connect();
    }
    
    createMockSocket() {
        // Mock Socket.IO pour la démonstration
        const mockSocket = {
            events: {},
            connected: false,
            
            on(event, callback) {
                if (!this.events[event]) this.events[event] = [];
                this.events[event].push(callback);
            },
            
            emit(event, data) {
                console.log(`📤 Émission:`, event, data);
                
                // Simuler des réponses du serveur
                setTimeout(() => {
                    switch (event) {
                        case 'user:join':
                            this.trigger('users:list', [
                                { id: 1, username: 'Alice' },
                                { id: 2, username: 'Bob' },
                                { id: 3, username: data.username }
                            ]);
                            break;
                        case 'message:send':
                            this.trigger('message', {
                                id: Date.now(),
                                user: { username: data.username },
                                content: data.content,
                                timestamp: new Date(),
                                room: data.room
                            });
                            break;
                        case 'room:join':
                            this.trigger('room:joined', { room: data.room });
                            break;
                    }
                }, 100);
            },
            
            trigger(event, data) {
                if (this.events[event]) {
                    this.events[event].forEach(callback => callback(data));
                }
            },
            
            connect() {
                setTimeout(() => {
                    this.connected = true;
                    this.trigger('connect');
                }, 500);
            },
            
            disconnect() {
                this.connected = false;
                this.trigger('disconnect');
            }
        };
        
        return mockSocket;
    }
    
    sendMessage() {
        const input = document.getElementById('message-input');
        const content = input.value.trim();
        
        if (content && this.socket) {
            this.socket.emit('message:send', {
                username: this.currentUser,
                content: content,
                room: this.currentRoom
            });
            
            input.value = '';
        }
    }
    
    joinRoom(roomName) {
        if (this.socket && roomName !== this.currentRoom) {
            this.socket.emit('room:join', { room: roomName });
        }
    }
    
    displayMessage(messageData) {
        const container = document.getElementById('messages-container');
        const messageElement = document.createElement('div');
        
        const time = new Date(messageData.timestamp).toLocaleTimeString();
        const isOwnMessage = messageData.user.username === this.currentUser;
        
        messageElement.style.cssText = `
            margin: 5px 0;
            padding: 5px 10px;
            border-radius: 10px;
            max-width: 80%;
            ${isOwnMessage ? 
                'background: #007bff; color: white; margin-left: auto; text-align: right;' : 
                'background: #e0e0e0; margin-right: auto;'
            }
        `;
        
        messageElement.innerHTML = `
            <div style="font-size: 12px; opacity: 0.7;">
                ${isOwnMessage ? 'Vous' : messageData.user.username} • ${time}
            </div>
            <div>${messageData.content}</div>
        `;
        
        container.appendChild(messageElement);
        container.scrollTop = container.scrollHeight;
        
        this.messages.push(messageData);
    }
    
    displaySystemMessage(content) {
        const container = document.getElementById('messages-container');
        const systemElement = document.createElement('div');
        
        systemElement.style.cssText = `
            margin: 5px 0;
            padding: 5px;
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            border-radius: 5px;
            text-align: center;
            font-style: italic;
            color: #856404;
        `;
        
        systemElement.textContent = content;
        container.appendChild(systemElement);
        container.scrollTop = container.scrollHeight;
    }
    
    updateUsersList(users) {
        const container = document.getElementById('users-list');
        container.innerHTML = '';
        
        users.forEach(user => {
            const userElement = document.createElement('div');
            userElement.style.cssText = `
                padding: 3px 5px;
                margin: 2px 0;
                background: ${user.username === this.currentUser ? '#d4edda' : '#f8f9fa'};
                border-radius: 3px;
                font-size: 14px;
            `;
            
            userElement.innerHTML = `
                ${user.username === this.currentUser ? '👤' : '👥'} ${user.username}
                ${user.username === this.currentUser ? ' (vous)' : ''}
            `;
            
            container.appendChild(userElement);
            this.users.set(user.id, user);
        });
    }
    
    addUser(user) {
        this.users.set(user.id, user);
        this.updateUsersList(Array.from(this.users.values()));
    }
    
    removeUser(userId) {
        this.users.delete(userId);
        this.updateUsersList(Array.from(this.users.values()));
    }
    
    updateCurrentRoom() {
        document.getElementById('current-room').textContent = this.currentRoom;
        document.getElementById('current-user').textContent = this.currentUser;
        
        // Mettre à jour la sélection visuelle des salons
        document.querySelectorAll('.room-item').forEach(room => {
            if (room.dataset.room === this.currentRoom) {
                room.style.background = '#007bff';
                room.style.color = 'white';
            } else {
                room.style.background = '#fff';
                room.style.color = 'black';
            }
        });
    }
    
    clearMessages() {
        document.getElementById('messages-container').innerHTML = '';
    }
    
    showChatInterface() {
        document.getElementById('login-section').style.display = 'none';
        document.getElementById('chat-section').style.display = 'block';
        this.updateCurrentRoom();
    }
    
    updateStatus(status) {
        document.getElementById('connection-status').textContent = status;
    }
}

// Initialiser l'application de chat
console.log("=== Initialisation de l'application de chat ===");
const chatApp = new ChatApplication('ws://localhost:3000');

// Ajouter quelques messages de démonstration après 2 secondes
setTimeout(() => {
    if (chatApp.socket && chatApp.socket.connected) {
        chatApp.displayMessage({
            id: 1,
            user: { username: 'System' },
            content: 'Bienvenue dans le chat ! Tapez votre message ci-dessous.',
            timestamp: new Date(),
            room: 'general'
        });
    }
}, 2000);
```

---

## **EXERCICES PRATIQUES**

### Exercice 1 : WebSocket de base
**Consigne :** Crée un système de notifications en temps réel

**Code de départ :**
```javascript
class NotificationSystem {
    constructor(wsUrl) {
        this.wsUrl = wsUrl;
        this.ws = null;
        this.notifications = [];
    }
    
    connect() {
        // Implémenter la connexion WebSocket
        // Gérer les événements onopen, onmessage, onclose, onerror
    }
    
    sendNotification(notification) {
        // Envoyer une notification via WebSocket
    }
    
    displayNotification(notification) {
        // Afficher la notification dans l'interface
    }
}

// Teste avec wss://echo.websocket.org/
```

**Niveau de difficulté :** ⭐⭐⭐☆☆

### Exercice 2 : Jeu multijoueur simple
**Consigne :** Crée un jeu de clics en temps réel (qui clique le plus vite)

**Spécifications :**
- Affichage du score de tous les joueurs
- Mise à jour en temps réel des scores
- Classement des joueurs
- Gestion des connexions/déconnexions

**Niveau de difficulté :** ⭐⭐⭐⭐☆

### Exercice 3 : Système de monitoring
**Consigne :** Crée un dashboard qui affiche des métriques en temps réel

**Contraintes :**
- Graphiques mis à jour en temps réel
- Alertes si métriques critiques
- Historique des données
- Interface responsive

**Niveau de difficulté :** ⭐⭐⭐⭐⭐

---

## **SOLUTIONS DES EXERCICES**

### Solution Exercice 1
```javascript
class NotificationSystem {
    constructor(wsUrl) {
        this.wsUrl = wsUrl;
        this.ws = null;
        this.notifications = [];
        this.container = this.createNotificationContainer();
    }
    
    createNotificationContainer() {
        const container = document.createElement('div');
        container.id = 'notifications-container';
        container.style.cssText = `
            position: fixed;
            top: 20px;
            right: 20px;
            width: 300px;
            z-index: 1000;
        `;
        document.body.appendChild(container);
        return container;
    }
    
    connect() {
        try {
            this.ws = new WebSocket(this.wsUrl);
            
            this.ws.onopen = (event) => {
                console.log('✅ Système de notifications connecté');
                this.showSystemNotification('Connecté au système de notifications', 'success');
                
                // Envoyer un message de test
                this.sendNotification({
                    type: 'system',
                    title: 'Test de connexion',
                    message: 'Le système de notifications fonctionne !',
                    priority: 'info'
                });
            };
            
            this.ws.onmessage = (event) => {
                try {
                    const notification = JSON.parse(event.data);
                    this.displayNotification(notification);
                } catch (error) {
                    // Message texte simple
                    this.displayNotification({
                        type: 'text',
                        title: 'Message reçu',
                        message: event.data,
                        priority: 'info'
                    });
                }
            };
            
            this.ws.onclose = (event) => {
                console.log('🔌 Connexion notifications fermée');
                this.showSystemNotification('Connexion fermée', 'warning');
            };
            
            this.ws.onerror = (error) => {
                console.error('❌ Erreur notifications:', error);
                this.showSystemNotification('Erreur de connexion', 'error');
            };
            
        } catch (error) {
            console.error('💥 Erreur création WebSocket:', error);
            this.showSystemNotification('Impossible de se connecter', 'error');
        }
    }
    
    sendNotification(notification) {
        if (this.ws && this.ws.readyState === WebSocket.OPEN) {
            const message = {
                ...notification,
                id: Date.now(),
                timestamp: new Date().toISOString()
            };
            
            this.ws.send(JSON.stringify(message));
            console.log('📤 Notification envoyée:', message);
        } else {
            console.warn('⚠️ WebSocket non connecté, notification non envoyée');
        }
    }
    
    displayNotification(notification) {
        const notifElement = document.createElement('div');
        
        const priorityColors = {
            'info': '#17a2b8',
            'success': '#28a745',
            'warning': '#ffc107',
            'error': '#dc3545'
        };
        
        const color = priorityColors[notification.priority] || '#6c757d';
        
        notifElement.style.cssText = `
            background: white;
            border-left: 4px solid ${color};
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            border-radius: 4px;
            padding: 15px;
            margin-bottom: 10px;
            animation: slideIn 0.3s ease-out;
            position: relative;
            cursor: pointer;
        `;
        
        notifElement.innerHTML = `
            <div style="font-weight: bold; color: ${color}; margin-bottom: 5px;">
                ${this.getPriorityIcon(notification.priority)} ${notification.title || 'Notification'}
            </div>
            <div style="color: #333; font-size: 14px;">
                ${notification.message}
            </div>
            <div style="font-size: 11px; color: #888; margin-top: 5px;">
                ${new Date().toLocaleTimeString()}
            </div>
            <div style="position: absolute; top: 5px; right: 5px; cursor: pointer; color: #999;">
                ✕
            </div>
        `;
        
        // Ajouter la CSS d'animation si elle n'existe pas
        if (!document.getElementById('notification-styles')) {
            const style = document.createElement('style');
            style.id = 'notification-styles';
            style.textContent = `
                @keyframes slideIn {
                    from { transform: translateX(100%); opacity: 0; }
                    to { transform: translateX(0); opacity: 1; }
                }
                @keyframes slideOut {
                    from { transform: translateX(0); opacity: 1; }
                    to { transform: translateX(100%); opacity: 0; }
                }
            `;
            document.head.appendChild(style);
        }
        
        // Événement de fermeture
        const closeBtn = notifElement.querySelector('div:last-child');
        closeBtn.onclick = (e) => {
            e.stopPropagation();
            this.removeNotification(notifElement);
        };
        
        // Auto-fermeture après 5 secondes
        setTimeout(() => {
            if (notifElement.parentNode) {
                this.removeNotification(notifElement);
            }
        }, 5000);
        
        this.container.appendChild(notifElement);
        this.notifications.push(notification);
        
        // Limiter le nombre de notifications affichées
        if (this.container.children.length > 5) {
            this.removeNotification(this.container.firstChild);
        }
    }
    
    removeNotification(element) {
        element.style.animation = 'slideOut 0.3s ease-in';
        setTimeout(() => {
            if (element.parentNode) {
                element.parentNode.removeChild(element);
            }
        }, 300);
    }
    
    getPriorityIcon(priority) {
        const icons = {
            'info': 'ℹ️',
            'success': '✅',
            'warning': '⚠️',
            'error': '❌'
        };
        return icons[priority] || '📢';
    }
    
    showSystemNotification(message, priority = 'info') {
        this.displayNotification({
            type: 'system',
            title: 'Système',
            message: message,
            priority: priority
        });
    }
    
    disconnect() {
        if (this.ws) {
            this.ws.close();
        }
    }
}

// Interface de test
function createNotificationTestInterface() {
    const testContainer = document.createElement('div');
    testContainer.style.cssText = `
        position: fixed;
        bottom: 20px;
        left: 20px;
        background: white;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        font-family: Arial;
    `;
    
    testContainer.innerHTML = `
        <h4>🔔 Test des Notifications</h4>
        <div>
            <select id="notif-priority">
                <option value="info">Info</option>
                <option value="success">Succès</option>
                <option value="warning">Attention</option>
                <option value="error">Erreur</option>
            </select>
        </div>
        <div style="margin: 10px 0;">
            <input type="text" id="notif-title" placeholder="Titre" style="width: 100%; margin-bottom: 5px;">
            <textarea id="notif-message" placeholder="Message" style="width: 100%; height: 60px;"></textarea>
        </div>
        <button id="send-notif">Envoyer Notification</button>
        <button id="disconnect-notif">Déconnecter</button>
    `;
    
    document.body.appendChild(testContainer);
    return testContainer;
}

// Initialisation
const notifSystem = new NotificationSystem('wss://echo.websocket.org/');
notifSystem.connect();

const testInterface = createNotificationTestInterface();

// Événements de l'interface de test
document.getElementById('send-notif').onclick = () => {
    const priority = document.getElementById('notif-priority').value;
    const title = document.getElementById('notif-title').value || 'Test';
    const message = document.getElementById('notif-message').value || 'Message de test';
    
    notifSystem.sendNotification({
        type: 'test',
        title: title,
        message: message,
        priority: priority
    });
};

document.getElementById('disconnect-notif').onclick = () => {
    notifSystem.disconnect();
};

// Notifications de démonstration automatiques
setTimeout(() => {
    notifSystem.displayNotification({
        type: 'welcome',
        title: 'Bienvenue !',
        message: 'Le système de notifications est actif.',
        priority: 'success'
    });
}, 2000);

setTimeout(() => {
    notifSystem.displayNotification({
        type: 'tip',
        title: 'Astuce',
        message: 'Cliquez sur une notification pour la fermer.',
        priority: 'info'
    });
}, 4000);
```

**Explication :** Système complet avec interface de test, gestion des priorités et animations.

---

## **LIENS AVEC D'AUTRES CONCEPTS**

### Concepts préalables utilisés
- **Event Loop** : Gestion des événements asynchrones
- **JSON** : Sérialisation des messages
- **DOM** : Manipulation d'interface pour les notifications

### Prochaines étapes
- **Node.js + Express** : Serveur WebSocket backend
- **Socket.IO** : Bibliothèque complète avec fallbacks
- **Redis** : Persistence et scaling des connexions

### Concepts complémentaires
- **Server-Sent Events** : Alternative unidirectionnelle
- **WebRTC** : Communication peer-to-peer
- **Service Workers** : Notifications même app fermée

---

## **PROJET MINI**

### Défi pratique
**Objectif :** Créer une application de "Tableau blanc collaboratif" en temps réel

**Fonctionnalités requises :**
- [ ] Dessin synchronisé entre tous les utilisateurs
- [ ] Curseurs des autres utilisateurs visibles
- [ ] Chat intégré pour communication
- [ ] Sauvegarde automatique des dessins
- [ ] Gestion des utilisateurs connectés

**Structure de fichiers suggérée :**
```
📁 tableau-collaboratif/
├── 📄 index.html
├── 📄 style.css
├── 📄 client.js
└── 📄 server.js (Node.js + Socket.IO)
```

**Temps estimé :** 90 minutes

---

## **RESSOURCES COMPLÉMENTAIRES**

### Documentation officielle
- [MDN - WebSocket API](https://developer.mozilla.org/fr/docs/Web/API/WebSocket)
- [Socket.IO Documentation](https://socket.io/docs/v4/)

### Vidéos recommandées
- "WebSockets in 100 Seconds" - Fireship (2 min)
- "Real-time Chat App with Socket.IO" - Traversy Media (45 min)

### Articles approfondis
- "When to use WebSockets vs Server-Sent Events" - Stack Overflow Blog
- "WebSocket Security Best Practices" - OWASP

### Outils de pratique
- [Websocket.org Echo Test](https://www.websocket.org/echo.html)
- [Socket.IO Playground](https://socket.io/)

---

## **AUTO-ÉVALUATION**

### Questions de compréhension
1. **Question conceptuelle :** Quelle est la différence entre WebSocket et HTTP ?
   - **Réponse :** WebSocket maintient une connexion persistante bidirectionnelle, HTTP est request/response

2. **Question pratique :** Que fait ce code ?
   ```javascript
   ws.send(JSON.stringify({type: 'chat', message: 'Hello'}));
   ```
   - **Réponse :** Envoie un message JSON structuré via WebSocket

3. **Question d'application :** Quand utiliser WebSockets vs Server-Sent Events ?
   - **Réponse :** WebSockets pour bidirectionnel, SSE pour flux unidirectionnel du serveur

### Checklist de maîtrise
- [ ] Je comprends les WebSockets vs HTTP
- [ ] Je gère les connexions et déconnexions
- [ ] Je structure les messages JSON
- [ ] J'implémente la reconnexion automatique
- [ ] Je gère les erreurs et timeouts
- [ ] Je peux créer des apps temps réel

**Score de confiance :** ___/10

---

## **DEBUGGING ET ERREURS COURANTES**

### Erreur fréquente 1 - Connexion prématurée
**Code problématique :**
```javascript
const ws = new WebSocket('ws://server.com');
ws.send('Hello'); // Erreur ! Connexion pas encore ouverte
```

**Solution :**
```javascript
const ws = new WebSocket('ws://server.com');
ws.onopen = () => {
    ws.send('Hello'); // Correct !
};
```

**Explication :** Attendre l'événement 'open' avant d'envoyer des messages.

### Erreur fréquente 2 - Oublier de nettoyer
**Code problématique :**
```javascript
function setupWebSocket() {
    const ws = new WebSocket('ws://server.com');
    // Pas de nettoyage en cas de changement de page
}
```

**Solution :**
```javascript
let ws = null;

function setupWebSocket() {
    ws = new WebSocket('ws://server.com');
}

window.onbeforeunload = () => {
    if (ws) ws.close();
};
```

**Explication :** Fermer proprement les connexions pour éviter les fuites.

### Erreur fréquente 3 - Pas de gestion de reconnexion
**Code problématique :**
```javascript
ws.onclose = () => {
    console.log('Connexion fermée'); // Et après ?
};
```

**Solution :**
```javascript
ws.onclose = (event) => {
    if (event.code !== 1000) { // Pas une fermeture normale
        setTimeout(() => {
            setupWebSocket(); // Reconnecter
        }, 1000);
    }
};
```

**Explication :** Implémenter une logique de reconnexion pour la robustesse.

---

## **NOTES PERSONNELLES**

### Ce que j'ai appris aujourd'hui
- WebSockets permettent la communication bidirectionnelle en temps réel
- Socket.IO simplifie l'implémentation avec des fallbacks
- Important de gérer les déconnexions et reconnexions

### Questions à approfondir
- Comment scaler les WebSockets sur plusieurs serveurs ?
- Sécurisation des connexions WebSocket (authentification) ?

### Révision programmée
- [ ] Révision dans 1 jour (concepts de base)
- [ ] Révision dans 1 semaine (implémentation complète)
- [ ] Révision dans 1 mois (application en production)

---

## **TAGS ET MÉTADONNÉES**

**Mots-clés :** #javascript #websockets #socket-io #temps-reel #communication #avance

**Temps de completion :** ___

**Difficulté ressentie :** ⭐⭐⭐⭐☆

**Satisfaction du cours :** ⭐⭐⭐⭐⭐

**Prêt pour le concept suivant :** ✅ Oui / ❌ Non (révision nécessaire)

---

*📅 Fiche créée le : 18/07/2025*
*🔄 Dernière révision : 18/07/2025*
*📍 Position dans la roadmap : 182/126*
