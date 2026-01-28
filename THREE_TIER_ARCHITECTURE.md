# 🏗️ THREE-TIER ARCHITECTURE - Für die Klausur!

## ⚠️ WICHTIG: Dein Kumpel sagt, du musst das zeichnen können!

> "Schreibe dir am Rand auch Beispiele für eine three Tier Architecture damit du nicht unnötige Punkte verlierst falls diese Aufgabe wieder kommt. Wir mussten die zeichnen."

---

# Was ist Three-Tier Architecture?

Die **Three-Tier Architecture** (Drei-Schichten-Architektur) teilt eine Anwendung in drei logische Schichten:

1. **Presentation Tier** (Präsentationsschicht) - UI/Frontend
2. **Business Logic Tier** (Geschäftslogikschicht) - Backend/API
3. **Data Tier** (Datenschicht) - Datenbank

---

# Die Drei Schichten im Detail

```
┌─────────────────────────────────────────────┐
│     PRESENTATION TIER (Schicht 1)           │
│                                             │
│  - User Interface                           │
│  - Präsentation der Daten                   │
│  - Benutzerinteraktion                      │
│                                             │
│  Beispiele: Web-Browser, Mobile App, GUI    │
└──────────────────┬──────────────────────────┘
                   │
            HTTP / REST API
                   │
┌──────────────────▼──────────────────────────┐
│     BUSINESS LOGIC TIER (Schicht 2)         │
│                                             │
│  - Geschäftslogik                           │
│  - Datenverarbeitung                        │
│  - Business Rules                           │
│  - API Endpoints                            │
│                                             │
│  Beispiele: Spring Boot, Node.js, Django    │
└──────────────────┬──────────────────────────┘
                   │
            JDBC / SQL
                   │
┌──────────────────▼──────────────────────────┐
│     DATA TIER (Schicht 3)                   │
│                                             │
│  - Datenspeicherung                         │
│  - Datenbank                                │
│  - Persistenz                               │
│                                             │
│  Beispiele: PostgreSQL, MySQL, MongoDB      │
└─────────────────────────────────────────────┘
```

---

# Schicht 1: Presentation Tier (Frontend) 🖥️

## Was macht diese Schicht?
- Zeigt Daten dem Benutzer an
- Nimmt Benutzereingaben entgegen
- Sendet Requests an die Business Logic Tier
- Empfängt und zeigt Responses an

## Technologien:
- **Web**: HTML, CSS, JavaScript, React, Vue.js, Angular
- **Mobile**: iOS (Swift), Android (Kotlin), React Native
- **Desktop**: JavaFX, Electron, WPF

## Beispiel Code (React):
```javascript
// Frontend sendet Request an Backend
function UserList() {
  const [users, setUsers] = useState([]);
  
  useEffect(() => {
    // API Call zur Business Logic Tier
    fetch('http://api.example.com/users')
      .then(response => response.json())
      .then(data => setUsers(data));
  }, []);
  
  return (
    <div>
      <h1>Benutzer Liste</h1>
      {users.map(user => (
        <div key={user.id}>{user.name}</div>
      ))}
    </div>
  );
}
```

## Verantwortlichkeiten:
- ✅ UI/UX
- ✅ Validation (Client-seitig)
- ✅ Routing
- ❌ KEINE Geschäftslogik!
- ❌ KEINE direkten Datenbankzugriffe!

---

# Schicht 2: Business Logic Tier (Backend/API) ⚙️

## Was macht diese Schicht?
- Verarbeitet Geschäftslogik
- Validiert Daten
- Koordiniert zwischen Frontend und Datenbank
- Stellt API Endpoints bereit
- Implementiert Business Rules

## Technologien:
- **Java**: Spring Boot, Java EE
- **JavaScript/TypeScript**: Node.js, Express, NestJS
- **Python**: Django, Flask, FastAPI
- **.NET**: ASP.NET Core
- **Ruby**: Ruby on Rails

## Beispiel Code (Spring Boot):
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    // API Endpoint für Presentation Tier
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        // Geschäftslogik: Validierung
        if (user.getEmail() == null || !isValidEmail(user.getEmail())) {
            throw new IllegalArgumentException("Invalid email");
        }
        
        // Speichern über Data Tier
        return userService.save(user);
    }
}

@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    public List<User> findAll() {
        // Ruft Data Tier auf
        return userRepository.findAll();
    }
    
    public User save(User user) {
        // Business Logic: Passwort hashen
        user.setPassword(hashPassword(user.getPassword()));
        // Speichern in Data Tier
        return userRepository.save(user);
    }
}
```

## Verantwortlichkeiten:
- ✅ Business Logic
- ✅ Validation (Server-seitig)
- ✅ Authentication/Authorization
- ✅ Data Transformation
- ✅ Error Handling
- ❌ KEINE UI-Logik!
- ❌ KEINE direkten SQL Queries (nutzt Data Layer)!

---

# Schicht 3: Data Tier (Datenbank) 🗄️

## Was macht diese Schicht?
- Speichert Daten persistent
- Verwaltet Datenintegrität
- Stellt Daten für Business Logic Tier bereit

## Technologien:
- **Relational**: PostgreSQL, MySQL, Oracle, SQL Server
- **NoSQL**: MongoDB, Redis, Cassandra
- **ORM**: Hibernate, JPA, Entity Framework

## Beispiel Code (JPA Repository):
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @Column(nullable = false)
    private String password;
    
    // Getters and Setters
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Data Tier Operationen
    User findByEmail(String email);
    List<User> findByActiveTrue();
}
```

## Verantwortlichkeiten:
- ✅ Datenspeicherung
- ✅ Datenintegrität (Constraints)
- ✅ Transactions
- ✅ Backup/Recovery
- ❌ KEINE Geschäftslogik!
- ❌ KEINE UI-Logik!

---

# Vollständiges Beispiel: E-Commerce System

## Szenario: Benutzer bestellt ein Produkt

```
┌────────────────────────────────────────────────┐
│  PRESENTATION TIER - React Web App            │
│                                                │
│  - Benutzer klickt "Produkt kaufen"           │
│  - Form validieren (Client-side)              │
│  - POST /api/orders mit Produktdaten          │
└───────────────────┬────────────────────────────┘
                    │
           HTTP POST Request
         { "productId": 123, "quantity": 2 }
                    │
┌───────────────────▼────────────────────────────┐
│  BUSINESS LOGIC TIER - Spring Boot API        │
│                                                │
│  1. Request empfangen                          │
│  2. Authentifizierung prüfen                   │
│  3. Produktverfügbarkeit prüfen               │
│  4. Preis berechnen (Business Logic)          │
│  5. Bestellung erstellen                       │
│  6. Lagerbestand reduzieren                    │
│  7. Email senden (optional)                    │
└───────────────────┬────────────────────────────┘
                    │
        SQL: INSERT INTO orders...
        SQL: UPDATE products SET stock...
                    │
┌───────────────────▼────────────────────────────┐
│  DATA TIER - PostgreSQL Database              │
│                                                │
│  Tables:                                       │
│  - users                                       │
│  - products                                    │
│  - orders                                      │
│  - order_items                                 │
└────────────────────────────────────────────────┘
```

## Code für jede Schicht:

### Presentation Tier (React):
```javascript
function ProductPage() {
  const handleBuy = async (productId) => {
    // 1. Validierung
    if (quantity < 1) {
      alert("Ungültige Menge");
      return;
    }
    
    // 2. API Call
    const response = await fetch('/api/orders', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ 
        productId: productId,
        quantity: quantity 
      })
    });
    
    // 3. Response verarbeiten
    if (response.ok) {
      alert("Bestellung erfolgreich!");
    }
  };
}
```

### Business Logic Tier (Spring Boot):
```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    
    @Autowired
    private OrderService orderService;
    
    @PostMapping
    public ResponseEntity<Order> createOrder(@RequestBody OrderRequest request) {
        // Business Logic aufrufen
        Order order = orderService.createOrder(
            request.getProductId(), 
            request.getQuantity()
        );
        return ResponseEntity.status(HttpStatus.CREATED).body(order);
    }
}

@Service
public class OrderService {
    
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private ProductRepository productRepository;
    
    @Transactional
    public Order createOrder(Long productId, int quantity) {
        // 1. Produkt abrufen (Data Tier)
        Product product = productRepository.findById(productId)
            .orElseThrow(() -> new NotFoundException("Product not found"));
        
        // 2. Business Logic: Verfügbarkeit prüfen
        if (product.getStock() < quantity) {
            throw new InsufficientStockException("Not enough stock");
        }
        
        // 3. Business Logic: Preis berechnen
        BigDecimal totalPrice = product.getPrice()
            .multiply(BigDecimal.valueOf(quantity));
        
        // 4. Bestellung erstellen
        Order order = new Order();
        order.setProductId(productId);
        order.setQuantity(quantity);
        order.setTotalPrice(totalPrice);
        order.setStatus("PENDING");
        
        // 5. Speichern (Data Tier)
        order = orderRepository.save(order);
        
        // 6. Lagerbestand reduzieren
        product.setStock(product.getStock() - quantity);
        productRepository.save(product);
        
        return order;
    }
}
```

### Data Tier (JPA Entities):
```java
@Entity
@Table(name = "orders")
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "product_id")
    private Long productId;
    
    @Column(nullable = false)
    private Integer quantity;
    
    @Column(precision = 10, scale = 2)
    private BigDecimal totalPrice;
    
    @Column(length = 20)
    private String status;
}

@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {
    List<Order> findByStatus(String status);
}
```

---

# Weitere Beispiele für Three-Tier Architecture

## Beispiel 1: Banking System 🏦

```
┌─────────────────────────────────────┐
│  PRESENTATION TIER                  │
│  - Online Banking Website           │
│  - Mobile Banking App               │
│  - ATM Interface                    │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│  BUSINESS LOGIC TIER                │
│  - Account Management               │
│  - Transaction Processing           │
│  - Interest Calculation             │
│  - Fraud Detection                  │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│  DATA TIER                          │
│  - Customer Data (accounts table)   │
│  - Transaction History              │
│  - Audit Logs                       │
└─────────────────────────────────────┘
```

## Beispiel 2: Social Media Platform 📱

```
┌─────────────────────────────────────┐
│  PRESENTATION TIER                  │
│  - Web App (React)                  │
│  - iOS App (Swift)                  │
│  - Android App (Kotlin)             │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│  BUSINESS LOGIC TIER                │
│  - User Authentication              │
│  - Post Creation/Deletion           │
│  - Comment Management               │
│  - Like/Follow Logic                │
│  - Feed Algorithm                   │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│  DATA TIER                          │
│  - Users Table                      │
│  - Posts Table                      │
│  - Comments Table                   │
│  - Likes/Followers Tables           │
└─────────────────────────────────────┘
```

## Beispiel 3: Online Shop 🛒

```
┌─────────────────────────────────────┐
│  PRESENTATION TIER                  │
│  - Product Catalog (Angular)        │
│  - Shopping Cart                    │
│  - Checkout Page                    │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│  BUSINESS LOGIC TIER                │
│  - Product Search                   │
│  - Price Calculation                │
│  - Inventory Management             │
│  - Order Processing                 │
│  - Payment Integration              │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│  DATA TIER                          │
│  - Products (PostgreSQL)            │
│  - Orders (PostgreSQL)              │
│  - Customers (PostgreSQL)           │
│  - Session Data (Redis)             │
└─────────────────────────────────────┘
```

---

# Vorteile der Three-Tier Architecture

## ✅ Vorteile:

1. **Separation of Concerns**
   - Jede Schicht hat klare Verantwortlichkeit
   - Einfacher zu verstehen und zu warten

2. **Skalierbarkeit**
   - Jede Schicht kann unabhängig skaliert werden
   - z.B. mehr Frontend-Server bei vielen Benutzern

3. **Wartbarkeit**
   - Änderungen in einer Schicht beeinflussen andere nicht
   - Code ist modularer

4. **Wiederverwendbarkeit**
   - Business Logic kann von mehreren Frontends genutzt werden
   - z.B. Web App + Mobile App nutzen gleiche API

5. **Testbarkeit**
   - Jede Schicht kann unabhängig getestet werden
   - Unit Tests für Business Logic
   - Integration Tests für API

6. **Security**
   - Datenbank ist nicht direkt vom Internet erreichbar
   - Business Logic prüft alle Requests

## ❌ Nachteile:

1. **Komplexität**
   - Mehr Schichten = mehr Code
   - Mehr Konfiguration

2. **Performance**
   - Kommunikation zwischen Schichten kostet Zeit
   - Netzwerk-Latenz

3. **Deployment**
   - Mehrere Komponenten müssen deployed werden

---

# Für die Klausur zeichnen! ✏️

## Basis-Zeichnung (MERKEN!):

```
┌─────────────────────┐
│  Presentation Tier  │
│  (Frontend/UI)      │
│                     │
│  React / Angular    │
└──────────┬──────────┘
           │ HTTP/REST
           │
┌──────────▼──────────┐
│ Business Logic Tier │
│  (Backend/API)      │
│                     │
│  Spring Boot / API  │
└──────────┬──────────┘
           │ SQL/JDBC
           │
┌──────────▼──────────┐
│     Data Tier       │
│  (Database)         │
│                     │
│  PostgreSQL / MySQL │
└─────────────────────┘
```

## Einfaches Beispiel (für Rand des Spickzettels):

```
Browser (HTML/JS)
    ↕ HTTP
Spring Boot (Java)
    ↕ JDBC
PostgreSQL (DB)
```

## Mit Technologien:

```
┌─────────────────────────┐
│ React, Vue.js, Angular  │  ← Presentation
│ HTML, CSS, JavaScript   │
└────────────┬────────────┘
             │
┌────────────▼────────────┐
│ Spring Boot, Node.js    │  ← Business Logic
│ Express, Django, .NET   │
└────────────┬────────────┘
             │
┌────────────▼────────────┐
│ PostgreSQL, MySQL       │  ← Data
│ MongoDB, Oracle         │
└─────────────────────────┘
```

---

# 📝 Zusammenfassung für Spickzettel

## Three-Tier Architecture:

**Schicht 1 - Presentation:**
- UI/Frontend
- Benutzerinteraktion
- Beispiel: React, Angular

**Schicht 2 - Business Logic:**
- API/Backend
- Geschäftslogik
- Beispiel: Spring Boot, Node.js

**Schicht 3 - Data:**
- Datenbank
- Datenspeicherung
- Beispiel: PostgreSQL, MySQL

**Kommunikation:**
- Tier 1 → Tier 2: HTTP/REST
- Tier 2 → Tier 3: SQL/JDBC

**Vorteile:**
- ✅ Separation of Concerns
- ✅ Skalierbar
- ✅ Wartbar

---

# 🎯 Übung für die Klausur

## Aufgabe 1: Zeichne Three-Tier Architecture für ein Blog-System

**Lösung:**
```
┌─────────────────────┐
│  Blog Frontend      │  ← Presentation
│  (React)            │
│  - Artikel lesen    │
│  - Kommentare       │
└──────────┬──────────┘
           │ REST API
┌──────────▼──────────┐
│  Blog API           │  ← Business Logic
│  (Spring Boot)      │
│  - CRUD Artikel     │
│  - Authentifizierung│
└──────────┬──────────┘
           │ SQL
┌──────────▼──────────┐
│  Database           │  ← Data
│  (PostgreSQL)       │
│  - articles table   │
│  - comments table   │
└─────────────────────┘
```

## Aufgabe 2: Ordne zu welche Tier

Wo gehören diese Komponenten hin?

1. Passwort hashen → **Business Logic Tier**
2. Button klicken → **Presentation Tier**
3. Daten in Tabelle speichern → **Data Tier**
4. HTML rendern → **Presentation Tier**
5. Preis berechnen → **Business Logic Tier**
6. SQL Query ausführen → **Data Tier**

---

# 🎯 Nächste Schritte

1. ✅ Three-Tier Architecture verstanden
2. ✅ Zeichnung auswendig lernen
3. ✅ Beispiele auf Spickzettel schreiben (AN DEN RAND!)
4. ✅ Zu [COMMUNICATION_PATTERNS.md](COMMUNICATION_PATTERNS.md)

**Tipp: Übe die Zeichnung mehrmals, bis du sie aus dem Gedächtnis kannst!** ✏️
