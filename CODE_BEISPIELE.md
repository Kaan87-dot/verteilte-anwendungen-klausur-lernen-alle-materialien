# 💻 CODE BEISPIELE - Übe für die Klausur!

## 🎯 Ziel: Code-Lücken ausfüllen können wie in der Klausur

**Tipp:** Decke die Lösungen ab und versuche die Lücken selbst auszufüllen!

---

# HTTP Code Beispiele 🌐

## Beispiel 1: REST Controller - Basic CRUD

### Aufgabe (mit Lücken):
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.http.*;

@RestController
@RequestMapping("/api/users")
public class UserController {
    
    // GET: Alle Benutzer abrufen
    @___Mapping
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.findAll();
        return ResponseEntity.status(HttpStatus.___).body(users);
    }
    
    // GET: Einzelnen Benutzer abrufen
    @___Mapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long ___) {
        User user = userService.findById(___);
        if (user == null) {
            return ResponseEntity.status(HttpStatus.___).build();
        }
        return ResponseEntity.status(HttpStatus.___).body(user);
    }
    
    // POST: Neuen Benutzer erstellen
    @___Mapping
    public ResponseEntity<User> createUser(@RequestBody User ___) {
        User created = userService.create(___);
        return ResponseEntity.status(HttpStatus.___).body(created);
    }
    
    // PUT: Benutzer aktualisieren
    @___Mapping("/{id}")
    public ResponseEntity<User> updateUser(
            @PathVariable Long id, 
            @RequestBody User user) {
        User updated = userService.update(id, user);
        return ResponseEntity.status(HttpStatus.___).body(updated);
    }
    
    // DELETE: Benutzer löschen
    @___Mapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.status(HttpStatus.___).build();
    }
}
```

### ✅ Lösung:
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.http.*;

@RestController
@RequestMapping("/api/users")
public class UserController {
    
    // GET: Alle Benutzer abrufen
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.findAll();
        return ResponseEntity.status(HttpStatus.OK).body(users);
    }
    
    // GET: Einzelnen Benutzer abrufen
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        if (user == null) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }
        return ResponseEntity.status(HttpStatus.OK).body(user);
    }
    
    // POST: Neuen Benutzer erstellen
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User created = userService.create(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
    
    // PUT: Benutzer aktualisieren
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(
            @PathVariable Long id, 
            @RequestBody User user) {
        User updated = userService.update(id, user);
        return ResponseEntity.status(HttpStatus.OK).body(updated);
    }
    
    // DELETE: Benutzer löschen
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
    }
}
```

---

## Beispiel 2: HTTP Status Codes

### Aufgabe (mit Lücken):
```java
@PostMapping("/register")
public ResponseEntity<User> register(@RequestBody User user) {
    // Prüfe ob Benutzer bereits existiert
    if (userService.existsByEmail(user.getEmail())) {
        return ResponseEntity.status(HttpStatus.___).build(); // Konflikt
    }
    
    // Validiere Eingabe
    if (user.getEmail() == null || user.getPassword() == null) {
        return ResponseEntity.status(HttpStatus.___).build(); // Ungültige Anfrage
    }
    
    try {
        User registered = userService.register(user);
        return ResponseEntity.status(HttpStatus.___).body(registered); // Erstellt
    } catch (Exception e) {
        return ResponseEntity.status(HttpStatus.___).build(); // Server-Fehler
    }
}

@GetMapping("/profile")
public ResponseEntity<User> getProfile(@RequestHeader("Authorization") String token) {
    // Prüfe Authentifizierung
    if (token == null || !isValidToken(token)) {
        return ResponseEntity.status(HttpStatus.___).build(); // Nicht authentifiziert
    }
    
    User user = userService.getUserFromToken(token);
    
    // Prüfe Berechtigung
    if (!user.hasPermission("VIEW_PROFILE")) {
        return ResponseEntity.status(HttpStatus.___).build(); // Keine Berechtigung
    }
    
    return ResponseEntity.status(HttpStatus.___).body(user); // OK
}
```

### ✅ Lösung:
```java
@PostMapping("/register")
public ResponseEntity<User> register(@RequestBody User user) {
    // Prüfe ob Benutzer bereits existiert
    if (userService.existsByEmail(user.getEmail())) {
        return ResponseEntity.status(HttpStatus.CONFLICT).build(); // 409 Konflikt
    }
    
    // Validiere Eingabe
    if (user.getEmail() == null || user.getPassword() == null) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).build(); // 400 Ungültige Anfrage
    }
    
    try {
        User registered = userService.register(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(registered); // 201 Erstellt
    } catch (Exception e) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build(); // 500 Server-Fehler
    }
}

@GetMapping("/profile")
public ResponseEntity<User> getProfile(@RequestHeader("Authorization") String token) {
    // Prüfe Authentifizierung
    if (token == null || !isValidToken(token)) {
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build(); // 401 Nicht authentifiziert
    }
    
    User user = userService.getUserFromToken(token);
    
    // Prüfe Berechtigung
    if (!user.hasPermission("VIEW_PROFILE")) {
        return ResponseEntity.status(HttpStatus.FORBIDDEN).build(); // 403 Keine Berechtigung
    }
    
    return ResponseEntity.status(HttpStatus.OK).body(user); // 200 OK
}
```

---

## Beispiel 3: Request/Response mit Headers

### Aufgabe (mit Lücken):
```java
@PostMapping("/login")
public ResponseEntity<LoginResponse> login(@RequestBody LoginRequest request) {
    User user = userService.authenticate(request.getEmail(), request.getPassword());
    
    if (user == null) {
        return ResponseEntity.status(HttpStatus.___).build();
    }
    
    String token = jwtService.generateToken(user);
    
    HttpHeaders headers = new HttpHeaders();
    headers.add("Authorization", "Bearer " + token);
    headers.add("Content-Type", "___/___");
    
    LoginResponse response = new LoginResponse(user, token);
    
    return ResponseEntity
            .status(HttpStatus.___)
            .headers(___)
            .body(response);
}
```

### ✅ Lösung:
```java
@PostMapping("/login")
public ResponseEntity<LoginResponse> login(@RequestBody LoginRequest request) {
    User user = userService.authenticate(request.getEmail(), request.getPassword());
    
    if (user == null) {
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
    }
    
    String token = jwtService.generateToken(user);
    
    HttpHeaders headers = new HttpHeaders();
    headers.add("Authorization", "Bearer " + token);
    headers.add("Content-Type", "application/json");
    
    LoginResponse response = new LoginResponse(user, token);
    
    return ResponseEntity
            .status(HttpStatus.OK)
            .headers(headers)
            .body(response);
}
```

---

# Liquibase Code Beispiele 🗄️

## Beispiel 1: Tabelle erstellen - Users

### Aufgabe (mit Lücken):
```xml
<changeSet id="create-users-table" author="student">
    <createTable tableName="users">
        <column name="id" type="______" autoIncrement="true">
            <constraints ________="true" nullable="false"/>
        </column>
        <column name="username" type="VARCHAR(___)">
            <constraints nullable="______" unique="true"/>
        </column>
        <column name="email" type="________(100)">
            <constraints nullable="false"/>
        </column>
        <column name="password" type="VARCHAR(255)">
            <constraints nullable="______"/>
        </column>
        <column name="active" type="_______" defaultValueBoolean="true">
            <constraints nullable="false"/>
        </column>
        <column name="created_at" type="_________" defaultValueComputed="CURRENT_TIMESTAMP">
            <constraints nullable="false"/>
        </column>
    </createTable>
</changeSet>
```

### ✅ Lösung:
```xml
<changeSet id="create-users-table" author="student">
    <createTable tableName="users">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="username" type="VARCHAR(50)">
            <constraints nullable="false" unique="true"/>
        </column>
        <column name="email" type="VARCHAR(100)">
            <constraints nullable="false"/>
        </column>
        <column name="password" type="VARCHAR(255)">
            <constraints nullable="false"/>
        </column>
        <column name="active" type="BOOLEAN" defaultValueBoolean="true">
            <constraints nullable="false"/>
        </column>
        <column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP">
            <constraints nullable="false"/>
        </column>
    </createTable>
</changeSet>
```

---

## Beispiel 2: Tabelle mit Foreign Key - Orders

### Aufgabe (mit Lücken):
```xml
<changeSet id="create-orders-table" author="student">
    <createTable tableName="orders">
        <column name="id" type="______" autoIncrement="true">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="user_id" type="______">
            <constraints nullable="false"/>
        </column>
        <column name="total_price" type="DECIMAL(__,__)">
            <constraints nullable="false"/>
        </column>
        <column name="status" type="VARCHAR(20)" defaultValue="PENDING">
            <constraints nullable="false"/>
        </column>
        <column name="order_date" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP">
            <constraints nullable="false"/>
        </column>
    </createTable>
    
    <addForeignKeyConstraint 
        constraintName="fk_orders_user_id"
        baseTableName="______"
        baseColumnNames="user_id"
        referencedTableName="______"
        referencedColumnNames="id"
        onDelete="________"/>
</changeSet>
```

### ✅ Lösung:
```xml
<changeSet id="create-orders-table" author="student">
    <createTable tableName="orders">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="user_id" type="BIGINT">
            <constraints nullable="false"/>
        </column>
        <column name="total_price" type="DECIMAL(10,2)">
            <constraints nullable="false"/>
        </column>
        <column name="status" type="VARCHAR(20)" defaultValue="PENDING">
            <constraints nullable="false"/>
        </column>
        <column name="order_date" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP">
            <constraints nullable="false"/>
        </column>
    </createTable>
    
    <addForeignKeyConstraint 
        constraintName="fk_orders_user_id"
        baseTableName="orders"
        baseColumnNames="user_id"
        referencedTableName="users"
        referencedColumnNames="id"
        onDelete="CASCADE"/>
</changeSet>
```

---

## Beispiel 3: Spalten hinzufügen und ändern

### Aufgabe (mit Lücken):
```xml
<!-- Spalte hinzufügen -->
<changeSet id="add-phone-to-users" author="student">
    <addColumn tableName="users">
        <column name="phone" type="________(20)">
            <constraints nullable="____"/>
        </column>
    </addColumn>
</changeSet>

<!-- Spalte ändern -->
<changeSet id="modify-username-length" author="student">
    <modifyDataType 
        tableName="users"
        columnName="username"
        newDataType="VARCHAR(____)"/>
</changeSet>

<!-- Index erstellen -->
<changeSet id="add-index-email" author="student">
    <createIndex indexName="idx_users_email" tableName="users">
        <column name="______"/>
    </createIndex>
</changeSet>
```

### ✅ Lösung:
```xml
<!-- Spalte hinzufügen -->
<changeSet id="add-phone-to-users" author="student">
    <addColumn tableName="users">
        <column name="phone" type="VARCHAR(20)">
            <constraints nullable="true"/>
        </column>
    </addColumn>
</changeSet>

<!-- Spalte ändern -->
<changeSet id="modify-username-length" author="student">
    <modifyDataType 
        tableName="users"
        columnName="username"
        newDataType="VARCHAR(100)"/>
</changeSet>

<!-- Index erstellen -->
<changeSet id="add-index-email" author="student">
    <createIndex indexName="idx_users_email" tableName="users">
        <column name="email"/>
    </createIndex>
</changeSet>
```

---

## Beispiel 4: Komplette Datenbank Schema

### Aufgabe (mit Lücken):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
    http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.0.xsd">

    <!-- Products Table -->
    <changeSet id="create-products-table" author="student">
        <createTable tableName="products">
            <column name="id" type="______" autoIncrement="true">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="name" type="VARCHAR(100)">
                <constraints nullable="______"/>
            </column>
            <column name="description" type="____">
                <constraints nullable="true"/>
            </column>
            <column name="price" type="DECIMAL(10,2)">
                <constraints nullable="false"/>
            </column>
            <column name="stock" type="INTEGER" defaultValueNumeric="0">
                <constraints nullable="false"/>
            </column>
            <column name="created_at" type="________" defaultValueComputed="CURRENT_TIMESTAMP"/>
        </createTable>
    </changeSet>
    
    <!-- Order Items Table (Many-to-Many zwischen Orders und Products) -->
    <changeSet id="create-order-items-table" author="student">
        <createTable tableName="order_items">
            <column name="id" type="BIGINT" autoIncrement="true">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="order_id" type="______">
                <constraints nullable="false"/>
            </column>
            <column name="product_id" type="______">
                <constraints nullable="false"/>
            </column>
            <column name="quantity" type="INTEGER">
                <constraints nullable="false"/>
            </column>
            <column name="price" type="DECIMAL(10,2)">
                <constraints nullable="false"/>
            </column>
        </createTable>
        
        <!-- Foreign Keys -->
        <addForeignKeyConstraint 
            constraintName="fk_order_items_order_id"
            baseTableName="order_items"
            baseColumnNames="order_id"
            referencedTableName="______"
            referencedColumnNames="id"
            onDelete="CASCADE"/>
            
        <addForeignKeyConstraint 
            constraintName="fk_order_items_product_id"
            baseTableName="order_items"
            baseColumnNames="product_id"
            referencedTableName="______"
            referencedColumnNames="id"
            onDelete="CASCADE"/>
    </changeSet>
    
</databaseChangeLog>
```

### ✅ Lösung:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
    http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.0.xsd">

    <!-- Products Table -->
    <changeSet id="create-products-table" author="student">
        <createTable tableName="products">
            <column name="id" type="BIGINT" autoIncrement="true">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="name" type="VARCHAR(100)">
                <constraints nullable="false"/>
            </column>
            <column name="description" type="TEXT">
                <constraints nullable="true"/>
            </column>
            <column name="price" type="DECIMAL(10,2)">
                <constraints nullable="false"/>
            </column>
            <column name="stock" type="INTEGER" defaultValueNumeric="0">
                <constraints nullable="false"/>
            </column>
            <column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP"/>
        </createTable>
    </changeSet>
    
    <!-- Order Items Table (Many-to-Many zwischen Orders und Products) -->
    <changeSet id="create-order-items-table" author="student">
        <createTable tableName="order_items">
            <column name="id" type="BIGINT" autoIncrement="true">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="order_id" type="BIGINT">
                <constraints nullable="false"/>
            </column>
            <column name="product_id" type="BIGINT">
                <constraints nullable="false"/>
            </column>
            <column name="quantity" type="INTEGER">
                <constraints nullable="false"/>
            </column>
            <column name="price" type="DECIMAL(10,2)">
                <constraints nullable="false"/>
            </column>
        </createTable>
        
        <!-- Foreign Keys -->
        <addForeignKeyConstraint 
            constraintName="fk_order_items_order_id"
            baseTableName="order_items"
            baseColumnNames="order_id"
            referencedTableName="orders"
            referencedColumnNames="id"
            onDelete="CASCADE"/>
            
        <addForeignKeyConstraint 
            constraintName="fk_order_items_product_id"
            baseTableName="order_items"
            baseColumnNames="product_id"
            referencedTableName="products"
            referencedColumnNames="id"
            onDelete="CASCADE"/>
    </changeSet>
    
</databaseChangeLog>
```

---

# Docker Compose Code Beispiele 🐳

## Beispiel 1: Basis Web-App mit Datenbank

### Aufgabe (mit Lücken):
```yaml
version: '3.8'

services:
  web:
    image: myapp:latest
    container_name: webapp
    _____:
      - "8080:____"
    environment:
      - DB_HOST=________
      - DB_PORT=5432
      - DB_NAME=mydb
    ________:
      - database
    restart: ______
    
  database:
    image: postgres:14
    container_name: postgres-db
    ports:
      - "____:5432"
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
    volumes:
      - db_data:/___/___/postgresql/data
    restart: always

_______:
  db_data:
```

### ✅ Lösung:
```yaml
version: '3.8'

services:
  web:
    image: myapp:latest
    container_name: webapp
    ports:
      - "8080:8080"
    environment:
      - DB_HOST=database
      - DB_PORT=5432
      - DB_NAME=mydb
    depends_on:
      - database
    restart: always
    
  database:
    image: postgres:14
    container_name: postgres-db
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
    volumes:
      - db_data:/var/lib/postgresql/data
    restart: always

volumes:
  db_data:
```

---

## Beispiel 2: Multi-Service Anwendung (Web, Backend, DB, Cache)

### Aufgabe (mit Lücken):
```yaml
version: '3.8'

services:
  # Frontend
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "____:80"
    depends_on:
      - backend
    networks:
      - app-network
    
  # Backend API
  backend:
    build:
      context: ./backend
    ports:
      - "____:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://________:5432/appdb
      - REDIS_HOST=_____
      - REDIS_PORT=6379
    depends_on:
      - ________
      - redis
    networks:
      - app-network
    volumes:
      - ./logs:/app/logs
    restart: ________
    
  # PostgreSQL Database
  postgres:
    image: postgres:14
    environment:
      - POSTGRES_DB=appdb
      - POSTGRES_USER=appuser
      - POSTGRES_PASSWORD=apppass
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - app-network
    
  # Redis Cache
  redis:
    image: redis:7-alpine
    ports:
      - "____:6379"
    networks:
      - app-network

volumes:
  postgres_data:

________:
  app-network:
    driver: bridge
```

### ✅ Lösung:
```yaml
version: '3.8'

services:
  # Frontend
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:80"
    depends_on:
      - backend
    networks:
      - app-network
    
  # Backend API
  backend:
    build:
      context: ./backend
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/appdb
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - postgres
      - redis
    networks:
      - app-network
    volumes:
      - ./logs:/app/logs
    restart: unless-stopped
    
  # PostgreSQL Database
  postgres:
    image: postgres:14
    environment:
      - POSTGRES_DB=appdb
      - POSTGRES_USER=appuser
      - POSTGRES_PASSWORD=apppass
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - app-network
    
  # Redis Cache
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    networks:
      - app-network

volumes:
  postgres_data:

networks:
  app-network:
    driver: bridge
```

---

## Beispiel 3: Development Setup mit Live-Reload

### Aufgabe (mit Lücken):
```yaml
version: '3.8'

services:
  app:
    build:
      ______: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - ./src:/app/src
      - ./public:/app/public
      - node_modules:/app/node_modules  # Prevent overwrite
    environment:
      - NODE_ENV=development
      - CHOKIDAR_USEPOLLING=true  # For file watching
    command: npm run dev
    restart: ________
    
  database:
    image: ______:14
    environment:
      - POSTGRES_DB=devdb
      - POSTGRES_USER=devuser
      - POSTGRES_PASSWORD=devpass
    ports:
      - "5432:____"
    volumes:
      - db_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql:__

volumes:
  db_data:
  node_modules:
```

### ✅ Lösung:
```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - ./src:/app/src
      - ./public:/app/public
      - node_modules:/app/node_modules  # Prevent overwrite
    environment:
      - NODE_ENV=development
      - CHOKIDAR_USEPOLLING=true  # For file watching
    command: npm run dev
    restart: unless-stopped
    
  database:
    image: postgres:14
    environment:
      - POSTGRES_DB=devdb
      - POSTGRES_USER=devuser
      - POSTGRES_PASSWORD=devpass
    ports:
      - "5432:5432"
    volumes:
      - db_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql:ro

volumes:
  db_data:
  node_modules:
```

---

# 📝 Übungsstrategie

## Wie du am besten übst:

1. **Erste Durchgang**: Lösungen anschauen und verstehen
2. **Zweiter Durchgang**: Lösungen abdecken, Lücken selbst ausfüllen
3. **Dritter Durchgang**: Komplett aus dem Gedächtnis schreiben
4. **Vor der Klausur**: Nochmal alle Beispiele durchgehen

## Merktechniken:

### HTTP:
- **"GPS-Phone-Deleted"** = GET, POST, PUT, PATCH, DELETE
- **"201 = Created"** (POST)
- **"404 = Not Found"** (GET nicht gefunden)

### Liquibase:
- **"BIG VARCHAR TIMESTAMP"** = Häufigste Typen
- **"Primary = true, nullable = false"** = Standard für ID

### Docker Compose:
- **"PED-VER"** = Ports, Environment, Depends_on, Volumes, Restart
- **"Host:Container"** = Port Mapping

---

# 🎯 Quick Reference - Für den Spickzettel

## HTTP Cheat Sheet:
```
GET    → Abrufen   → 200 OK
POST   → Erstellen → 201 Created
PUT    → Ersetzen  → 200 OK
PATCH  → Ändern    → 200 OK
DELETE → Löschen   → 204 No Content

400 Bad Request | 401 Unauthorized | 403 Forbidden
404 Not Found   | 409 Conflict     | 500 Server Error
```

## Liquibase Cheat Sheet:
```xml
<column name="id" type="BIGINT" autoIncrement="true">
    <constraints primaryKey="true" nullable="false"/>
</column>
<column name="name" type="VARCHAR(100)">
    <constraints nullable="false"/>
</column>
<column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP"/>
```

## Docker Compose Cheat Sheet:
```yaml
services:
  app:
    image: myapp:latest
    ports: ["8080:8080"]
    environment: [DB_HOST=db]
    depends_on: [database]
    volumes: [data:/app/data]
    restart: always
volumes:
  data:
```

---

# ✅ Nächste Schritte

1. ✅ Alle Beispiele mindestens einmal durchgehen
2. ✅ Lücken selbst ausfüllen (ohne Lösung!)
3. ✅ Fehler identifizieren und nochmal lernen
4. ✅ Wichtigste Patterns auf Spickzettel schreiben

**Du bist ready für die Code-Aufgaben! 💪**
