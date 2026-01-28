# 📚 THEORIE HILFE - Für den Theorieteil der Klausur

## 🎯 Was dich erwartet

**Anfang und Ende der Klausur**: Theoriefragen
- Kommunikationsmodelle skizzieren
- Communication Patterns erklären
- Konzepte beschreiben

---

# 1. Kommunikationsmodelle 📡

## OSI-Modell (7 Schichten)

Das **OSI-Modell** (Open Systems Interconnection) beschreibt, wie Daten über ein Netzwerk übertragen werden.

### Die 7 Schichten (von oben nach unten):

```
┌─────────────────────────────────────┐
│  7. Application Layer (Anwendung)   │  ← HTTP, FTP, SMTP, DNS
├─────────────────────────────────────┤
│  6. Presentation Layer (Darstellung)│  ← Verschlüsselung, Kompression
├─────────────────────────────────────┤
│  5. Session Layer (Sitzung)         │  ← Session-Management
├─────────────────────────────────────┤
│  4. Transport Layer (Transport)     │  ← TCP, UDP
├─────────────────────────────────────┤
│  3. Network Layer (Vermittlung)     │  ← IP, Router
├─────────────────────────────────────┤
│  2. Data Link Layer (Sicherung)     │  ← Ethernet, MAC
├─────────────────────────────────────┤
│  1. Physical Layer (Bitübertragung) │  ← Kabel, Signale
└─────────────────────────────────────┘
```

### Merksatz: **"Alle Priester saufen Tequila nach der Predigt"**
- **A**pplication
- **P**resentation
- **S**ession
- **T**ransport
- **N**etwork
- **D**ata Link
- **P**hysical

### Detaillierte Erklärung:

#### 7. Application Layer (Anwendungsschicht)
- **Was**: Schnittstelle zu Anwendungen
- **Protokolle**: HTTP, HTTPS, FTP, SMTP, DNS, SSH
- **Beispiel**: Web-Browser sendet HTTP-Request

#### 6. Presentation Layer (Darstellungsschicht)
- **Was**: Datenformatierung, Verschlüsselung, Kompression
- **Beispiel**: SSL/TLS Verschlüsselung, JPEG/GIF Kompression

#### 5. Session Layer (Sitzungsschicht)
- **Was**: Aufbau, Verwaltung und Abbau von Sitzungen
- **Beispiel**: Login-Session, Authentifizierung

#### 4. Transport Layer (Transportschicht)
- **Was**: Ende-zu-Ende Kommunikation, Fehlerkorrektur
- **Protokolle**: TCP (zuverlässig), UDP (schnell)
- **Beispiel**: TCP sorgt für zuverlässige Datenübertragung

#### 3. Network Layer (Vermittlungsschicht)
- **Was**: Routing, Adressierung (IP-Adressen)
- **Protokolle**: IP, ICMP, Router
- **Beispiel**: Router leitet Pakete weiter

#### 2. Data Link Layer (Sicherungsschicht)
- **Was**: Fehlererkennung, MAC-Adressen
- **Protokolle**: Ethernet, WiFi, PPP
- **Beispiel**: Switch im lokalen Netzwerk

#### 1. Physical Layer (Bitübertragungsschicht)
- **Was**: Physikalische Übertragung (Bits)
- **Beispiel**: Kabel, Funkwellen, Signale

---

## TCP/IP-Modell (4 Schichten)

Das **TCP/IP-Modell** ist das praktisch verwendete Modell im Internet.

```
┌──────────────────────────────────────┐
│  4. Application Layer                │  ← HTTP, FTP, SMTP, DNS
│     (entspricht OSI 5-7)             │
├──────────────────────────────────────┤
│  3. Transport Layer                  │  ← TCP, UDP
│     (entspricht OSI 4)               │
├──────────────────────────────────────┤
│  2. Internet Layer                   │  ← IP, ICMP, Router
│     (entspricht OSI 3)               │
├──────────────────────────────────────┤
│  1. Network Access Layer             │  ← Ethernet, WiFi
│     (entspricht OSI 1-2)             │
└──────────────────────────────────────┘
```

### Vergleich OSI vs. TCP/IP:

| OSI (7 Schichten) | TCP/IP (4 Schichten) |
|-------------------|----------------------|
| Application + Presentation + Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link + Physical | Network Access |

---

## Client-Server-Modell

Das **Client-Server-Modell** beschreibt die Kommunikation zwischen Clients und Servern.

```
┌─────────┐                      ┌─────────┐
│ Client  │ ──── Request ───────→│ Server  │
│         │                      │         │
│ (z.B.   │ ←─── Response ───────│ (z.B.   │
│ Browser)│                      │ API)    │
└─────────┘                      └─────────┘
```

### Eigenschaften:
- **Client**: Stellt Anfragen, wartet auf Antworten
- **Server**: Beantwortet Anfragen, stellt Dienste bereit
- **Asymmetrisch**: Server hat mehr Verantwortung
- **Zentral**: Server ist zentrale Instanz

### Beispiele:
- **Web**: Browser (Client) ↔ Webserver (Server)
- **E-Mail**: E-Mail-Client ↔ Mail-Server
- **Datenbank**: Anwendung (Client) ↔ Datenbank (Server)

---

## Peer-to-Peer-Modell (P2P)

Im **P2P-Modell** sind alle Teilnehmer gleichberechtigt.

```
     ┌──────┐
     │ Peer │
     │  A   │
     └───┬──┘
         │
    ┌────┼────┐
    │    │    │
┌───▼──┐ │ ┌──▼───┐
│ Peer │ │ │ Peer │
│  B   │ │ │  C   │
└───┬──┘ │ └──┬───┘
    │    │    │
    └────┴────┘
```

### Eigenschaften:
- **Gleichberechtigt**: Jeder Peer kann Client und Server sein
- **Dezentral**: Keine zentrale Instanz
- **Skalierbar**: Mehr Peers = mehr Ressourcen

### Beispiele:
- **BitTorrent**: File-Sharing
- **Blockchain**: Verteiltes Ledger
- **Skype**: Peer-to-Peer Calls (früher)

---

# 2. Communication Patterns 🔄

## Request/Response Pattern (Synchron)

Das **klassische Pattern** für Client-Server-Kommunikation.

```
Client                          Server
  │                              │
  ├───── Request ───────────────→│
  │         (HTTP GET)           │
  │                              ├─ Process
  │                              │
  │←───── Response ──────────────┤
  │         (200 OK + Data)      │
  │                              │
```

### Eigenschaften:
- **Synchron**: Client wartet auf Antwort
- **Blockierend**: Client kann währenddessen nichts anderes tun
- **1:1 Kommunikation**: Ein Request, eine Response

### Wann verwenden?
- ✅ Wenn sofortige Antwort benötigt wird
- ✅ Bei einfachen CRUD-Operationen
- ✅ Bei RESTful APIs

### Beispiele:
```java
// Client sendet Request
GET /api/users/123

// Server sendet Response
200 OK
{ "id": 123, "name": "Max" }
```

### Vor- und Nachteile:
**Vorteile:**
- ✅ Einfach zu verstehen
- ✅ Klare Semantik
- ✅ Fehlerbehandlung einfach

**Nachteile:**
- ❌ Client blockiert
- ❌ Nicht für lange Operationen geeignet
- ❌ Keine Ereignisse möglich

---

## Publish/Subscribe Pattern (Asynchron)

Im **Pub/Sub Pattern** senden Publisher Nachrichten an Topics, Subscriber abonnieren Topics.

```
Publisher A ─┐
Publisher B ─┤
             │
             ▼
        ┌─────────┐
        │  Topic  │
        │ "orders"│
        └────┬────┘
             │
        ┌────┴────┐
        │         │
        ▼         ▼
  Subscriber 1  Subscriber 2
```

### Eigenschaften:
- **Asynchron**: Publisher wartet nicht auf Subscriber
- **Entkoppelt**: Publisher kennt Subscriber nicht
- **1:N Kommunikation**: Eine Nachricht, viele Empfänger

### Wann verwenden?
- ✅ Ereignis-getriebene Systeme
- ✅ Wenn mehrere Komponenten auf Ereignisse reagieren sollen
- ✅ Bei lose gekoppelten Systemen

### Beispiele:
```java
// Publisher sendet Nachricht
publish("orders", new Order(123, "Product A"));

// Subscriber 1 (Email Service)
subscribe("orders", (order) -> sendEmail(order));

// Subscriber 2 (Inventory Service)
subscribe("orders", (order) -> updateStock(order));
```

### Technologien:
- **RabbitMQ**: Message Broker
- **Apache Kafka**: Event Streaming
- **Redis Pub/Sub**: In-Memory Messaging

### Vor- und Nachteile:
**Vorteile:**
- ✅ Entkoppelung
- ✅ Skalierbar
- ✅ Flexibel (neue Subscriber jederzeit)

**Nachteile:**
- ❌ Komplexer
- ❌ Garantien schwieriger (Message Delivery)
- ❌ Debugging schwieriger

---

## Message Queue Pattern

**Message Queues** speichern Nachrichten zwischen, bis sie verarbeitet werden.

```
Producer              Queue              Consumer
  │                    │                   │
  ├─ Message 1 ───────→│                   │
  ├─ Message 2 ───────→│                   │
  ├─ Message 3 ───────→│                   │
  │                    │←─ Get Message ────┤
  │                    ├─ Message 1 ───────→│
  │                    │                   ├─ Process
  │                    │                   ├─ Acknowledge ─→│
  │                    │←─ Get Message ────┤
  │                    ├─ Message 2 ───────→│
```

### Eigenschaften:
- **Asynchron**: Producer und Consumer unabhängig
- **Persistent**: Nachrichten werden gespeichert
- **FIFO**: First In, First Out (meist)
- **Load Balancing**: Mehrere Consumer möglich

### Wann verwenden?
- ✅ Bei zeitintensiven Operationen
- ✅ Bei unzuverlässigen Systemen (Retry-Logik)
- ✅ Bei Lastspitzen (Queue als Puffer)

### Beispiele:
```java
// Producer sendet Nachricht in Queue
queue.send("email-queue", new EmailTask("max@example.com", "Hello"));

// Consumer verarbeitet Nachrichten
queue.consume("email-queue", (task) -> {
    sendEmail(task);
    // Nachricht wird automatisch aus Queue entfernt
});
```

### Technologien:
- **RabbitMQ**: AMQP Message Broker
- **Amazon SQS**: Cloud-basierte Queue
- **ActiveMQ**: JMS Message Broker

### Vor- und Nachteile:
**Vorteile:**
- ✅ Entkoppelung
- ✅ Resilience (Fehlertoleranz)
- ✅ Load Balancing

**Nachteile:**
- ❌ Zusätzliche Infrastruktur
- ❌ Komplexität
- ❌ Latenz

---

## Event-Driven Architecture

In einer **Event-Driven Architecture** kommunizieren Komponenten über Events.

```
┌─────────┐         Event         ┌─────────┐
│ Service │ ─────── "Order" ─────→│ Event   │
│    A    │        Created         │  Bus    │
└─────────┘                        └────┬────┘
                                        │
                               ┌────────┴────────┐
                               │                 │
                               ▼                 ▼
                        ┌──────────┐      ┌──────────┐
                        │ Service  │      │ Service  │
                        │    B     │      │    C     │
                        └──────────┘      └──────────┘
```

### Eigenschaften:
- **Event-basiert**: Alles sind Events
- **Asynchron**: Services reagieren auf Events
- **Entkoppelt**: Services kennen sich nicht

### Wann verwenden?
- ✅ Microservices-Architektur
- ✅ Komplexe Geschäftsprozesse
- ✅ Wenn viele Services zusammenarbeiten

### Beispiele:
```java
// Service A erstellt Event
eventBus.publish(new OrderCreatedEvent(order));

// Service B reagiert auf Event (Email)
@EventListener
public void onOrderCreated(OrderCreatedEvent event) {
    emailService.sendOrderConfirmation(event.getOrder());
}

// Service C reagiert auf Event (Inventory)
@EventListener
public void onOrderCreated(OrderCreatedEvent event) {
    inventoryService.reserveStock(event.getOrder());
}
```

### Vor- und Nachteile:
**Vorteile:**
- ✅ Hohe Entkoppelung
- ✅ Skalierbar
- ✅ Flexibel

**Nachteile:**
- ❌ Komplex
- ❌ Debugging schwierig
- ❌ Eventual Consistency

---

## Synchron vs. Asynchron

### Synchrone Kommunikation:
```
Client ────→ Server
       ←────
      (wartet)
```

**Eigenschaften:**
- Client blockiert und wartet
- Sofortige Antwort
- Einfach zu verstehen

**Beispiele:**
- HTTP Request/Response
- Remote Procedure Call (RPC)
- Database Query

**Wann verwenden?**
- ✅ Wenn sofortige Antwort benötigt
- ✅ Bei einfachen Operationen
- ✅ Bei UI-Interaktionen

---

### Asynchrone Kommunikation:
```
Client ────→ Queue
       ←────
    (sofort zurück)

Queue ────→ Worker
             (später)
```

**Eigenschaften:**
- Client wartet nicht
- Verzögerte Verarbeitung
- Entkoppelt

**Beispiele:**
- Message Queues
- Event Streaming
- Email Versand

**Wann verwenden?**
- ✅ Bei zeitintensiven Operationen
- ✅ Bei unabhängigen Services
- ✅ Bei Lastspitzen

---

# 3. Wichtige Konzepte 🔑

## RESTful API Principles

**REST** = Representational State Transfer

### Prinzipien:
1. **Stateless**: Jeder Request ist unabhängig
2. **Client-Server**: Trennung von Concerns
3. **Cacheable**: Responses können gecacht werden
4. **Uniform Interface**: Einheitliche Schnittstelle
5. **Layered System**: Schichtenarchitektur
6. **Code on Demand** (optional): Code kann geladen werden

### REST Constraints:
- **Resource-based**: URLs repräsentieren Ressourcen
- **HTTP Methods**: GET, POST, PUT, DELETE
- **Stateless**: Keine Session am Server
- **HATEOAS**: Links zu verwandten Ressourcen

---

## Idempotenz

**Idempotent** = Mehrfaches Ausführen hat gleiches Ergebnis

### Idempotente HTTP Methoden:
- ✅ **GET**: Mehrmals lesen = gleich
- ✅ **PUT**: Mehrmals ersetzen = gleich
- ✅ **DELETE**: Mehrmals löschen = gleich (404 nach erstem Mal)

### Nicht idempotent:
- ❌ **POST**: Mehrmals erstellen = mehrere Ressourcen

**Beispiel:**
```java
// Idempotent (PUT)
PUT /api/users/123
{ "name": "Max" }
// Mehrmals ausführen = gleicher Zustand

// Nicht idempotent (POST)
POST /api/users
{ "name": "Max" }
// Mehrmals ausführen = mehrere User "Max"
```

---

## Microservices vs. Monolith

### Monolith:
```
┌─────────────────────────┐
│   Monolithic App        │
│  ┌────────────────────┐ │
│  │ User Management    │ │
│  ├────────────────────┤ │
│  │ Order Processing   │ │
│  ├────────────────────┤ │
│  │ Payment            │ │
│  └────────────────────┘ │
└─────────────────────────┘
```

**Vorteile:**
- ✅ Einfach zu entwickeln
- ✅ Einfach zu deployen
- ✅ Einfach zu testen

**Nachteile:**
- ❌ Schwer skalierbar
- ❌ Technologie-Lock-in
- ❌ Große Codebase

---

### Microservices:
```
┌──────────┐  ┌──────────┐  ┌──────────┐
│  User    │  │  Order   │  │ Payment  │
│ Service  │  │ Service  │  │ Service  │
└──────────┘  └──────────┘  └──────────┘
```

**Vorteile:**
- ✅ Unabhängig skalierbar
- ✅ Technologie-Flexibilität
- ✅ Kleine Teams

**Nachteile:**
- ❌ Komplex
- ❌ Verteiltes System
- ❌ DevOps Overhead

---

## Load Balancing

**Load Balancer** verteilt Last auf mehrere Server.

```
        Client Requests
              │
              ▼
        ┌───────────┐
        │   Load    │
        │  Balancer │
        └─────┬─────┘
              │
     ┌────────┼────────┐
     │        │        │
     ▼        ▼        ▼
┌────────┐ ┌────────┐ ┌────────┐
│Server 1│ │Server 2│ │Server 3│
└────────┘ └────────┘ └────────┘
```

### Load Balancing Strategien:
- **Round Robin**: Reihum verteilen
- **Least Connections**: Server mit wenigsten Verbindungen
- **IP Hash**: Basierend auf Client-IP
- **Weighted**: Basierend auf Server-Kapazität

---

## Caching

**Cache** speichert häufig verwendete Daten für schnellen Zugriff.

```
Client → Cache → Database
         ↓ Hit
       Return
         
         ↓ Miss
       Database → Cache
```

### Cache-Strategien:
1. **Cache Aside**: App verwaltet Cache
2. **Read Through**: Cache lädt automatisch
3. **Write Through**: Schreiben in Cache + DB
4. **Write Behind**: Schreiben in Cache, später in DB

### Technologien:
- **Redis**: In-Memory Cache
- **Memcached**: Distributed Cache
- **CDN**: Content Delivery Network

---

# 📝 Für die Klausur merken!

## Must-Know für Zeichnungen:

### OSI-Modell (7 Schichten):
```
7. Application  ← HTTP
4. Transport    ← TCP/UDP
3. Network      ← IP
1. Physical     ← Kabel
```

### Client-Server-Modell:
```
Client → Request → Server
Client ← Response ← Server
```

### Request/Response Pattern:
```
Client ──Request──→ Server
Client ←─Response─── Server
```

### Pub/Sub Pattern:
```
Publisher → Topic → Subscribers (mehrere)
```

---

## Quick Reference:

**Synchron:**
- Request/Response
- RPC
- REST API

**Asynchron:**
- Message Queue
- Pub/Sub
- Events

**HTTP Idempotent:**
- GET, PUT, DELETE ✅
- POST ❌

**REST Principles:**
- Stateless
- Resource-based
- HTTP Methods
- Cacheable

---

# 🎯 Nächste Schritte

1. ✅ OSI-Modell auswendig lernen (7 Schichten)
2. ✅ Communication Patterns verstehen
3. ✅ Modelle zeichnen üben
4. ✅ Zu [THREE_TIER_ARCHITECTURE.md](THREE_TIER_ARCHITECTURE.md)

**Du bist bereit für die Theoriefragen! 💪**
