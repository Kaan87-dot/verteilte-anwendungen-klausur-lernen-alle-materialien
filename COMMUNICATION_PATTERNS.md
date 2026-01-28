# 🔄 COMMUNICATION PATTERNS - Detailliert erklärt

## ⚠️ WICHTIG von deinem Kumpel:

> "Alles was auf der Seite mit den Bildern ist was in Roter Schrift ist ist neu und die Grafik mit den Communication Pattern"
> 
> "Wenn du das mit den Communication Pattern ausdruckst wird den Drucker es schrott aussehen lassen. Nimm dann ein Stift und korrigiere es selbst"

**➡️ Drucke die Patterns aus und korrigiere sie PER HAND!**

---

# Übersicht Communication Patterns

Communication Patterns beschreiben, wie Systeme und Services miteinander kommunizieren.

## Die wichtigsten Patterns:

1. **Request/Response** (Synchron)
2. **Publish/Subscribe** (Asynchron)
3. **Message Queue** (Asynchron)
4. **Event-Driven** (Asynchron)
5. **Request/Acknowledgement** (Asynchron mit Bestätigung)
6. **Fire and Forget** (Asynchron ohne Antwort)
7. **Polling** (Regelmäßiges Abfragen)
8. **Long Polling** (Verlängertes Abfragen)
9. **WebSocket** (Bidirektionale Verbindung)

---

# 1. Request/Response Pattern (Synchron) 🔄

## Beschreibung:
Der **klassische** Kommunikationsweg: Client sendet Request, wartet auf Response.

## Diagramm:
```
Client                          Server
  │                              │
  ├───── REQUEST ───────────────→│
  │      (HTTP GET /users)       │
  │                              │
  │         [Wartet...]          ├─ Verarbeitet
  │                              │
  │←───── RESPONSE ──────────────┤
  │      (200 OK + Daten)        │
  │                              │
```

## Eigenschaften:
- ✅ **Synchron**: Client blockiert und wartet
- ✅ **Sofortige Antwort**: Response kommt direkt zurück
- ✅ **1:1 Kommunikation**: Ein Request → Eine Response
- ❌ **Blockierend**: Client kann währenddessen nichts anderes tun

## Wann verwenden?
- ✅ CRUD Operationen (GET, POST, PUT, DELETE)
- ✅ Wenn sofortige Antwort benötigt wird
- ✅ RESTful APIs
- ✅ Einfache Abfragen

## Technologien:
- HTTP/HTTPS
- REST APIs
- GraphQL
- gRPC

## Code Beispiel:
```java
// Client (Frontend)
const response = await fetch('/api/users/123');
const user = await response.json();
console.log(user);  // Zeigt User direkt an

// Server (Backend)
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);  // Response zurückgeben
}
```

## Vorteile:
- ✅ Einfach zu verstehen
- ✅ Klare Semantik
- ✅ Fehlerbehandlung einfach
- ✅ Debugging einfach

## Nachteile:
- ❌ Client blockiert
- ❌ Nicht für lange Operationen
- ❌ Skalierung schwieriger bei vielen Clients

---

# 2. Publish/Subscribe Pattern (Pub/Sub) 📢

## Beschreibung:
**Publisher** senden Nachrichten an **Topics**, **Subscriber** abonnieren Topics und erhalten alle Nachrichten.

## Diagramm:
```
Publisher A ──┐
Publisher B ──┤
              │
              ▼
        ┌──────────┐
        │  TOPIC   │
        │ "orders" │
        └────┬─────┘
             │
        ┌────┴──────┐
        │           │
        ▼           ▼
  Subscriber 1  Subscriber 2
  (Email)       (Inventory)
```

## Eigenschaften:
- ✅ **Asynchron**: Publisher wartet nicht
- ✅ **Entkoppelt**: Publisher kennt Subscriber nicht
- ✅ **1:N Kommunikation**: Eine Nachricht → Viele Empfänger
- ✅ **Topic-basiert**: Subscriber filtern nach Topics

## Wann verwenden?
- ✅ Ereignis-Benachrichtigungen
- ✅ Wenn mehrere Services auf Events reagieren sollen
- ✅ Broadcasting (alle sollen informiert werden)
- ✅ Lose Kopplung gewünscht

## Technologien:
- RabbitMQ
- Apache Kafka
- Redis Pub/Sub
- AWS SNS
- Google Pub/Sub

## Code Beispiel:
```java
// Publisher
@Service
public class OrderPublisher {
    
    @Autowired
    private MessageChannel orderChannel;
    
    public void publishOrder(Order order) {
        // Sendet Nachricht an Topic "orders"
        orderChannel.send(
            MessageBuilder
                .withPayload(order)
                .setHeader("event", "ORDER_CREATED")
                .build()
        );
    }
}

// Subscriber 1: Email Service
@Component
public class EmailSubscriber {
    
    @StreamListener(target = "orders")
    public void handleOrder(Order order) {
        // Sendet Email bei neuer Bestellung
        emailService.sendOrderConfirmation(order);
    }
}

// Subscriber 2: Inventory Service
@Component
public class InventorySubscriber {
    
    @StreamListener(target = "orders")
    public void handleOrder(Order order) {
        // Reduziert Lagerbestand
        inventoryService.reduceStock(order);
    }
}
```

## Vorteile:
- ✅ Hohe Entkoppelung
- ✅ Skalierbar (neue Subscriber jederzeit)
- ✅ Flexibel
- ✅ Broadcasting

## Nachteile:
- ❌ Komplexer als Request/Response
- ❌ Message Delivery Garantien kompliziert
- ❌ Debugging schwieriger
- ❌ Reihenfolge nicht garantiert (meist)

---

# 3. Message Queue Pattern 📬

## Beschreibung:
**Producer** senden Nachrichten in eine **Queue**, **Consumer** holen Nachrichten aus der Queue und verarbeiten sie.

## Diagramm:
```
Producer               Queue               Consumer
  │                     │                    │
  ├─ Message 1 ────────→│                    │
  ├─ Message 2 ────────→│                    │
  ├─ Message 3 ────────→│                    │
  │                     │←─ Get Message ─────┤
  │                     ├─ Message 1 ────────→│
  │                     │                    ├─ Process
  │                     │                    ├─ ACK ──→│
  │                     │←─ Get Message ─────┤
  │                     ├─ Message 2 ────────→│
```

## Eigenschaften:
- ✅ **Asynchron**: Producer und Consumer unabhängig
- ✅ **Persistent**: Nachrichten werden gespeichert
- ✅ **FIFO**: First In, First Out (meist)
- ✅ **Load Balancing**: Mehrere Consumer möglich
- ✅ **At-least-once delivery**: Nachricht wird mindestens einmal zugestellt

## Wann verwenden?
- ✅ Zeitintensive Operationen (Email versenden, Video verarbeiten)
- ✅ Lastspitzen abfangen (Queue als Puffer)
- ✅ Retry-Logik bei Fehlern
- ✅ Entkopplung von Producer und Consumer

## Technologien:
- RabbitMQ
- Amazon SQS
- ActiveMQ
- Azure Service Bus
- Apache Kafka (kann auch als Queue verwendet werden)

## Code Beispiel:
```java
// Producer
@Service
public class EmailProducer {
    
    @Autowired
    private RabbitTemplate rabbitTemplate;
    
    public void sendEmail(EmailTask task) {
        // Nachricht in Queue "email-queue" senden
        rabbitTemplate.convertAndSend("email-queue", task);
    }
}

// Consumer
@Component
public class EmailConsumer {
    
    @RabbitListener(queues = "email-queue")
    public void processEmail(EmailTask task) {
        try {
            // Email versenden
            emailService.send(task.getRecipient(), task.getSubject(), task.getBody());
            // Nachricht wird automatisch aus Queue entfernt (ACK)
        } catch (Exception e) {
            // Bei Fehler: Nachricht zurück in Queue (NACK)
            throw new AmqpRejectAndDontRequeueException("Failed to send email", e);
        }
    }
}
```

## Vorteile:
- ✅ Entkoppelung
- ✅ Resilience (Fehlertoleranz)
- ✅ Load Balancing
- ✅ Skalierbar
- ✅ Garantierte Zustellung

## Nachteile:
- ❌ Zusätzliche Infrastruktur (Queue-System)
- ❌ Komplexität
- ❌ Latenz (nicht real-time)
- ❌ Eventual Consistency

---

# 4. Event-Driven Architecture (EDA) ⚡

## Beschreibung:
Services kommunizieren über **Events**. Wenn etwas passiert, wird ein Event veröffentlicht, andere Services reagieren darauf.

## Diagramm:
```
┌──────────┐       Event:        ┌──────────┐
│ Order    │    "OrderCreated"   │  Event   │
│ Service  ├─────────────────────→│   Bus    │
└──────────┘                      └────┬─────┘
                                       │
                          ┌────────────┴───────────┐
                          │                        │
                          ▼                        ▼
                    ┌──────────┐            ┌──────────┐
                    │  Email   │            │ Inventory│
                    │ Service  │            │ Service  │
                    └──────────┘            └──────────┘
                    Sendet Email         Reduziert Stock
```

## Eigenschaften:
- ✅ **Event-basiert**: Alles sind Events
- ✅ **Asynchron**: Services warten nicht aufeinander
- ✅ **Lose gekoppelt**: Services kennen sich nicht
- ✅ **Reaktiv**: Services reagieren auf Events

## Wann verwenden?
- ✅ Microservices-Architektur
- ✅ Komplexe Geschäftsprozesse
- ✅ Wenn viele Services zusammenarbeiten
- ✅ Event Sourcing

## Technologien:
- Apache Kafka
- Amazon EventBridge
- Azure Event Grid
- RabbitMQ
- NATS

## Code Beispiel:
```java
// Event Definition
public class OrderCreatedEvent {
    private Long orderId;
    private Long userId;
    private BigDecimal totalPrice;
    private LocalDateTime timestamp;
    // Getters, Setters
}

// Event Publisher (Order Service)
@Service
public class OrderService {
    
    @Autowired
    private ApplicationEventPublisher eventPublisher;
    
    public Order createOrder(OrderRequest request) {
        // Bestellung erstellen
        Order order = orderRepository.save(new Order(request));
        
        // Event veröffentlichen
        eventPublisher.publishEvent(
            new OrderCreatedEvent(order.getId(), order.getUserId(), order.getTotal())
        );
        
        return order;
    }
}

// Event Listener 1 (Email Service)
@Component
public class EmailEventListener {
    
    @EventListener
    public void handleOrderCreated(OrderCreatedEvent event) {
        // Email versenden
        emailService.sendOrderConfirmation(event.getOrderId());
    }
}

// Event Listener 2 (Inventory Service)
@Component
public class InventoryEventListener {
    
    @EventListener
    public void handleOrderCreated(OrderCreatedEvent event) {
        // Lagerbestand reduzieren
        inventoryService.reduceStock(event.getOrderId());
    }
}

// Event Listener 3 (Analytics Service)
@Component
public class AnalyticsEventListener {
    
    @EventListener
    public void handleOrderCreated(OrderCreatedEvent event) {
        // Analytics tracken
        analyticsService.trackOrder(event);
    }
}
```

## Vorteile:
- ✅ Hohe Entkoppelung
- ✅ Skalierbar
- ✅ Flexibel (neue Services einfach hinzufügen)
- ✅ Resilient

## Nachteile:
- ❌ Sehr komplex
- ❌ Debugging sehr schwierig
- ❌ Eventual Consistency
- ❌ Event-Versionierung kann problematisch sein

---

# 5. Fire and Forget Pattern 🚀

## Beschreibung:
Client sendet Request, wartet NICHT auf Response, macht sofort weiter.

## Diagramm:
```
Client                          Server
  │                              │
  ├───── REQUEST ───────────────→│
  │    (Asynchrone Operation)    │
  │                              │
  │ (macht sofort weiter)        ├─ Verarbeitet
  ▼                              │  (später)
Nächste Operation                │
```

## Eigenschaften:
- ✅ **Asynchron**: Client wartet nicht
- ✅ **Keine Response**: Client bekommt keine Antwort
- ✅ **Schnell**: Client blockiert nicht

## Wann verwenden?
- ✅ Logging
- ✅ Analytics Tracking
- ✅ Notifications (nicht kritisch)
- ✅ Wenn Response nicht benötigt wird

## Code Beispiel:
```java
// Client
@Async
public void logActivity(UserActivity activity) {
    // Sendet asynchron, wartet nicht auf Antwort
    loggingService.log(activity);
    // Code geht sofort weiter
}

// Server
@Service
public class LoggingService {
    
    public void log(UserActivity activity) {
        // Speichert Log in Datenbank (dauert lange)
        logRepository.save(activity);
        // Client hat nicht gewartet!
    }
}
```

---

# 6. Request/Acknowledgement Pattern ✅

## Beschreibung:
Client sendet Request, bekommt sofort **Acknowledgement** (Bestätigung), dass Request empfangen wurde. Verarbeitung erfolgt asynchron.

## Diagramm:
```
Client                          Server
  │                              │
  ├───── REQUEST ───────────────→│
  │                              ├─ Empfangen!
  │←───── ACK (202) ─────────────┤
  │   "Request empfangen"        │
  │                              │
  │ (macht weiter)               ├─ Verarbeitet
  │                              │  (später)
  │                              │
  │    (Optional: Callback)      │
  │←───── NOTIFICATION ──────────┤
  │    "Fertig verarbeitet"      │
```

## Eigenschaften:
- ✅ **Asynchron**: Client wartet nicht auf Verarbeitung
- ✅ **Sofortiges Feedback**: Client weiß, dass Request angekommen ist
- ✅ **Status Code 202**: "Accepted" - wird verarbeitet

## Wann verwenden?
- ✅ Lange Operationen (Video-Verarbeitung, Report-Generierung)
- ✅ Batch-Prozesse
- ✅ Wenn Client nicht warten soll

## Code Beispiel:
```java
// Server
@PostMapping("/reports")
public ResponseEntity<ReportResponse> generateReport(@RequestBody ReportRequest request) {
    // Erstelle Job ID
    String jobId = UUID.randomUUID().toString();
    
    // Starte asynchrone Verarbeitung
    reportService.generateReportAsync(jobId, request);
    
    // Sende sofort ACK zurück
    return ResponseEntity
        .status(HttpStatus.ACCEPTED)  // 202 Accepted
        .body(new ReportResponse(jobId, "Processing"));
}

// Asynchrone Verarbeitung
@Async
public void generateReportAsync(String jobId, ReportRequest request) {
    // Lange Operation
    Report report = generateReport(request);
    
    // Optional: Benachrichtige Client
    webhookService.notify(jobId, "Completed");
}

// Client kann Status abfragen
@GetMapping("/reports/{jobId}/status")
public ReportStatus getStatus(@PathVariable String jobId) {
    return reportService.getStatus(jobId);
}
```

---

# 7. Polling Pattern 🔄

## Beschreibung:
Client fragt **regelmäßig** beim Server nach, ob neue Daten vorhanden sind.

## Diagramm:
```
Client                          Server
  │                              │
  ├───── REQUEST ───────────────→│
  │←───── "Keine neuen Daten" ───┤
  │                              │
  │ (wartet 5 Sekunden)          │
  │                              │
  ├───── REQUEST ───────────────→│
  │←───── "Keine neuen Daten" ───┤
  │                              │
  │ (wartet 5 Sekunden)          │
  │                              │
  ├───── REQUEST ───────────────→│
  │←───── "NEUE DATEN!" ─────────┤
  │                              │
```

## Eigenschaften:
- ✅ **Regelmäßige Abfrage**: z.B. alle 5 Sekunden
- ❌ **Ineffizient**: Viele unnötige Requests

## Wann verwenden?
- ✅ Wenn Server keine Push-Notifications unterstützt
- ❌ Besser: Long Polling oder WebSocket verwenden

## Code Beispiel:
```javascript
// Client (JavaScript)
setInterval(async () => {
    const response = await fetch('/api/notifications');
    const notifications = await response.json();
    
    if (notifications.length > 0) {
        showNotifications(notifications);
    }
}, 5000);  // Alle 5 Sekunden
```

---

# 8. Long Polling Pattern ⏱️

## Beschreibung:
Wie Polling, aber Server hält Request offen, bis neue Daten verfügbar sind.

## Diagramm:
```
Client                          Server
  │                              │
  ├───── REQUEST ───────────────→│
  │                              ├─ Wartet...
  │         (hält offen)         │
  │                              │ (neue Daten!)
  │←───── RESPONSE ──────────────┤
  │        (mit Daten)           │
  │                              │
  ├───── REQUEST ───────────────→│
  │                              ├─ Wartet...
```

## Eigenschaften:
- ✅ **Effizienter** als normales Polling
- ✅ Server sendet nur bei neuen Daten
- ❌ Server muss Verbindung lange offen halten

## Wann verwenden?
- ✅ Chat-Anwendungen (alt)
- ✅ Notifications
- ✅ Wenn WebSocket nicht verfügbar

---

# 9. WebSocket Pattern 🔌

## Beschreibung:
**Bidirektionale**, persistente Verbindung zwischen Client und Server.

## Diagramm:
```
Client                          Server
  │                              │
  ├───── WebSocket Handshake ───→│
  │←───── Connection OK ──────────┤
  │                              │
  │═══════ Verbindung offen ════════
  │                              │
  ├───── Nachricht ─────────────→│
  │←───── Nachricht ──────────────┤
  ├───── Nachricht ─────────────→│
  │←───── Nachricht ──────────────┤
  │                              │
```

## Eigenschaften:
- ✅ **Bidirektional**: Beide Seiten können senden
- ✅ **Persistent**: Verbindung bleibt offen
- ✅ **Real-time**: Keine Latenz
- ✅ **Effizient**: Kein HTTP-Overhead

## Wann verwenden?
- ✅ Chat-Anwendungen
- ✅ Live-Updates (Börsenkurse, Spiele)
- ✅ Kollaborative Tools (Google Docs)
- ✅ Real-time Dashboards

## Code Beispiel:
```java
// Server (Spring WebSocket)
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/topic");
        registry.setApplicationDestinationPrefixes("/app");
    }
    
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws").withSockJS();
    }
}

@Controller
public class ChatController {
    
    @MessageMapping("/chat")
    @SendTo("/topic/messages")
    public ChatMessage sendMessage(ChatMessage message) {
        return message;
    }
}

// Client (JavaScript)
const socket = new WebSocket('ws://localhost:8080/ws');

socket.onmessage = (event) => {
    const message = JSON.parse(event.data);
    displayMessage(message);
};

socket.send(JSON.stringify({ text: 'Hello!' }));
```

---

# Vergleich der Patterns

| Pattern | Synchron? | Use Case | Komplexität |
|---------|-----------|----------|-------------|
| Request/Response | ✅ | CRUD, APIs | ⭐ Niedrig |
| Pub/Sub | ❌ | Broadcasting, Events | ⭐⭐ Mittel |
| Message Queue | ❌ | Async Tasks, Load Balancing | ⭐⭐ Mittel |
| Event-Driven | ❌ | Microservices, Complex Flows | ⭐⭐⭐ Hoch |
| Fire & Forget | ❌ | Logging, Analytics | ⭐ Niedrig |
| Polling | Hybrid | Status-Updates | ⭐ Niedrig |
| Long Polling | Hybrid | Notifications | ⭐⭐ Mittel |
| WebSocket | Hybrid | Real-time, Chat | ⭐⭐ Mittel |

---

# 📝 Für den Spickzettel

## Quick Reference:

**Synchron:**
- Request/Response (HTTP REST)

**Asynchron - Broadcasting:**
- Pub/Sub (1 → N)

**Asynchron - Queue:**
- Message Queue (1 → 1, Load Balanced)

**Asynchron - Events:**
- Event-Driven (Events → Listeners)

**Real-time:**
- WebSocket (Bidirektional)

---

# 🎯 Wichtig für die Klausur!

## Musst du zeichnen können:

1. **Request/Response**
2. **Pub/Sub**
3. **Message Queue**
4. **Event-Driven**

## Musst du erklären können:

- Synchron vs. Asynchron
- Wann welches Pattern verwenden
- Vor- und Nachteile

## Auf Spickzettel schreiben:

```
Request/Response: Client → Server → Response (Synchron)
Pub/Sub: Publisher → Topic → Subscribers (Asynchron, 1:N)
Queue: Producer → Queue → Consumer (Asynchron, Load Balance)
Event-Driven: Event → Event Bus → Listeners (Asynchron)
WebSocket: Client ⇄ Server (Real-time, Bidirektional)
```

---

# ✏️ DENK DRAN: Per Hand korrigieren!

Wenn du die Grafiken ausdruckst:
1. ✅ Drucke Communication Patterns aus
2. ✅ Korrigiere die Grafik PER HAND (Druckqualität ist schlecht)
3. ✅ Verwende Farben für bessere Übersicht
4. ✅ Schreibe Beispiele dazu

**Du schaffst das! 🚀**
