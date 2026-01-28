# 🔥 WICHTIGE THEMEN - Die Themen mit den MEISTEN PUNKTEN!

## ⚠️ ACHTUNG: Diese Themen sind ENTSCHEIDEND für deine Klausur!

Dein Kumpel Halil hat gesagt: **"Bei HTTP, Liquibase und Docker Compose gibt es die meisten Punkte!"**

Also konzentriere dich BESONDERS auf diese drei Themen!

---

# 1. HTTP - Hypertext Transfer Protocol 🌐

## Was ist HTTP?

HTTP ist das Protokoll für die Kommunikation im Web. Client (z.B. Browser) sendet Request, Server sendet Response.

---

## HTTP Methoden (SEHR WICHTIG!)

### GET - Daten abrufen
```
GET /api/users/123
```
- Holt Daten vom Server
- Keine Änderungen am Server
- Idempotent (mehrfaches Ausführen = gleiches Ergebnis)
- Erfolgreich: **200 OK**

**Beispiel:**
```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}
```

---

### POST - Neue Ressource erstellen
```
POST /api/users
```
- Erstellt eine neue Ressource
- Body enthält die Daten
- NICHT idempotent
- Erfolgreich: **201 Created**

**Beispiel:**
```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    User created = userService.create(user);
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}
```

---

### PUT - Ressource vollständig ersetzen
```
PUT /api/users/123
```
- Ersetzt komplette Ressource
- Body enthält ALLE Daten
- Idempotent
- Erfolgreich: **200 OK** oder **204 No Content**

**Beispiel:**
```java
@PutMapping("/users/{id}")
public User updateUser(@PathVariable Long id, @RequestBody User user) {
    return userService.update(id, user);
}
```

---

### PATCH - Ressource teilweise ändern
```
PATCH /api/users/123
```
- Ändert nur bestimmte Felder
- Body enthält nur geänderte Daten
- Erfolgreich: **200 OK**

**Beispiel:**
```java
@PatchMapping("/users/{id}")
public User patchUser(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
    return userService.partialUpdate(id, updates);
}
```

---

### DELETE - Ressource löschen
```
DELETE /api/users/123
```
- Löscht eine Ressource
- Idempotent
- Erfolgreich: **204 No Content** oder **200 OK**

**Beispiel:**
```java
@DeleteMapping("/users/{id}")
public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
    userService.delete(id);
    return ResponseEntity.noContent().build();
}
```

---

## HTTP Status Codes (MERKEN!)

### 2xx - Erfolg ✅
- **200 OK** - Request erfolgreich, Response enthält Daten
- **201 Created** - Neue Ressource wurde erstellt
- **204 No Content** - Erfolgreich, aber keine Response-Daten

### 3xx - Umleitung 🔄
- **301 Moved Permanently** - Ressource permanent verschoben
- **302 Found** - Ressource temporär verschoben

### 4xx - Client-Fehler ❌
- **400 Bad Request** - Ungültige Anfrage
- **401 Unauthorized** - Nicht authentifiziert
- **403 Forbidden** - Keine Berechtigung
- **404 Not Found** - Ressource nicht gefunden
- **409 Conflict** - Konflikt (z.B. Ressource existiert schon)

### 5xx - Server-Fehler 💥
- **500 Internal Server Error** - Server-Fehler
- **503 Service Unavailable** - Service nicht verfügbar

---

## REST Principles (Wichtig für Theorie!)

1. **Stateless** - Jeder Request ist unabhängig
2. **Client-Server** - Trennung von Client und Server
3. **Cacheable** - Responses können gecacht werden
4. **Uniform Interface** - Einheitliche Schnittstelle
5. **Layered System** - Schichtenarchitektur möglich

---

## HTTP Headers (Kann drankommen!)

### Request Headers:
```
GET /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer eyJhbGc...
Accept: application/json
```

### Response Headers:
```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1234
Cache-Control: max-age=3600
```

### Wichtige Header:
- **Content-Type**: Art der Daten (application/json, text/html, etc.)
- **Authorization**: Authentifizierung (Bearer Token, Basic Auth)
- **Accept**: Welche Datenformate akzeptiert werden
- **Cache-Control**: Caching-Verhalten

---

## Code-Lücken Beispiel HTTP:

```java
// Lücken: Methode, Pfad, Status Code
@___Mapping("/_____/{id}")  
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.findById(id);
    if (user == null) {
        return ResponseEntity.status(HttpStatus.___).build();
    }
    return ResponseEntity.status(HttpStatus.___).body(user);
}
```

**Lösung:**
```java
@GetMapping("/users/{id}")  
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.findById(id);
    if (user == null) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).build(); // 404
    }
    return ResponseEntity.status(HttpStatus.OK).body(user); // 200
}
```

---

# 2. Liquibase - Datenbank Versionierung 🗄️

## Was ist Liquibase?

Liquibase ist ein Tool für Datenbank-Versionierung. Es definiert Schema-Änderungen in XML/YAML/SQL Dateien (Changesets).

---

## Changeset Struktur (WICHTIG!)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
    http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.0.xsd">

    <changeSet id="1" author="student">
        <!-- Hier kommen die Änderungen -->
    </changeSet>
    
</databaseChangeLog>
```

---

## Tabelle erstellen (CREATE TABLE)

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
        <column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP">
            <constraints nullable="false"/>
        </column>
    </createTable>
</changeSet>
```

---

## Datentypen (MERKEN!)

| Typ | Verwendung | Beispiel |
|-----|------------|----------|
| **BIGINT** | Große Ganzzahlen | ID, Zähler |
| **INTEGER / INT** | Ganzzahlen | Alter, Anzahl |
| **VARCHAR(n)** | Text mit max. Länge | Namen, E-Mail |
| **TEXT** | Unbegrenzter Text | Beschreibung |
| **BOOLEAN** | Wahr/Falsch | aktiv, gelöscht |
| **TIMESTAMP** | Datum + Uhrzeit | created_at |
| **DATE** | Nur Datum | Geburtstag |
| **DECIMAL(p,s)** | Dezimalzahl | Preis (10,2) |

---

## Constraints (WICHTIG!)

### Primary Key
```xml
<constraints primaryKey="true" nullable="false"/>
```
- Eindeutiger Identifikator
- Kann nicht NULL sein

### Foreign Key
```xml
<changeSet id="add-foreign-key" author="student">
    <addForeignKeyConstraint 
        constraintName="fk_orders_user_id"
        baseTableName="orders"
        baseColumnNames="user_id"
        referencedTableName="users"
        referencedColumnNames="id"
        onDelete="CASCADE"/>
</changeSet>
```

### Unique Constraint
```xml
<constraints unique="true" nullable="false"/>
```
- Wert muss einzigartig sein

### Not Null
```xml
<constraints nullable="false"/>
```
- Wert darf nicht leer sein

---

## Spalte hinzufügen (ADD COLUMN)

```xml
<changeSet id="add-phone-column" author="student">
    <addColumn tableName="users">
        <column name="phone" type="VARCHAR(20)">
            <constraints nullable="true"/>
        </column>
    </addColumn>
</changeSet>
```

---

## Default Values

```xml
<column name="status" type="VARCHAR(20)" defaultValue="ACTIVE">
    <constraints nullable="false"/>
</column>

<column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP">
    <constraints nullable="false"/>
</column>
```

---

## Code-Lücken Beispiel Liquibase:

```xml
<changeSet id="create-products-table" author="student">
    <createTable tableName="products">
        <column name="id" type="______" autoIncrement="true">
            <constraints ________="true" nullable="false"/>
        </column>
        <column name="name" type="VARCHAR(___)">
            <constraints nullable="______"/>
        </column>
        <column name="price" type="DECIMAL(10,2)">
            <constraints nullable="false"/>
        </column>
        <column name="created_at" type="_________" defaultValueComputed="CURRENT_TIMESTAMP">
            <constraints nullable="false"/>
        </column>
    </createTable>
</changeSet>
```

**Lösung:**
```xml
<changeSet id="create-products-table" author="student">
    <createTable tableName="products">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="name" type="VARCHAR(100)">
            <constraints nullable="false"/>
        </column>
        <column name="price" type="DECIMAL(10,2)">
            <constraints nullable="false"/>
        </column>
        <column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP">
            <constraints nullable="false"/>
        </column>
    </createTable>
</changeSet>
```

---

# 3. Docker Compose - Container Orchestrierung 🐳

## Was ist Docker Compose?

Docker Compose ist ein Tool zum Definieren und Ausführen von Multi-Container Docker Anwendungen mit einer YAML-Datei.

---

## docker-compose.yml Struktur

```yaml
version: '3.8'

services:
  # Service-Definitionen hier

volumes:
  # Volume-Definitionen hier

networks:
  # Netzwerk-Definitionen hier
```

---

## Service Definition (WICHTIG!)

### Basis-Service:
```yaml
services:
  app:
    image: myapp:latest
    container_name: myapp-container
    ports:
      - "8080:8080"
    environment:
      - APP_ENV=production
      - DB_HOST=database
    depends_on:
      - database
    restart: always
```

---

## Wichtige Konfigurationsoptionen:

### 1. Image vs. Build
```yaml
# Bestehendes Image verwenden:
services:
  app:
    image: nginx:latest

# Image aus Dockerfile bauen:
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
```

---

### 2. Ports (PORT MAPPING)
```yaml
services:
  web:
    ports:
      - "8080:80"      # Host:Container
      - "443:443"
      - "3000:3000"
```
- **Links**: Port auf dem Host (dein Computer)
- **Rechts**: Port im Container

---

### 3. Environment Variables
```yaml
services:
  app:
    environment:
      - DB_HOST=database
      - DB_PORT=5432
      - DB_USER=admin
      - DB_PASSWORD=secret
      - APP_ENV=production
```

**Oder aus .env Datei:**
```yaml
services:
  app:
    env_file:
      - .env
```

---

### 4. Volumes (Datenpersistenz)
```yaml
services:
  database:
    volumes:
      - db_data:/var/lib/postgresql/data  # Named Volume
      - ./backup:/backup                  # Bind Mount
      - ./config.conf:/etc/app/config.conf:ro  # Read-only

volumes:
  db_data:  # Named Volume Definition
```

**Typen:**
- **Named Volume**: `db_data:/path` - Docker verwaltet
- **Bind Mount**: `./local:/container` - Lokaler Pfad
- **Read-only**: `:ro` anhängen

---

### 5. Depends On (Abhängigkeiten)
```yaml
services:
  web:
    depends_on:
      - database
      - cache
  
  database:
    image: postgres:14
  
  cache:
    image: redis:7
```
- **web** startet erst nach **database** und **cache**

---

### 6. Networks (Netzwerke)
```yaml
services:
  frontend:
    networks:
      - frontend-net
  
  backend:
    networks:
      - frontend-net
      - backend-net
  
  database:
    networks:
      - backend-net

networks:
  frontend-net:
  backend-net:
```

---

### 7. Restart Policy
```yaml
services:
  app:
    restart: always  # Immer neu starten
    # restart: on-failure  # Nur bei Fehler
    # restart: unless-stopped  # Außer manuell gestoppt
    # restart: no  # Nie automatisch
```

---

## Vollständiges Beispiel:

```yaml
version: '3.8'

services:
  # Web Application
  webapp:
    build:
      context: ./app
      dockerfile: Dockerfile
    container_name: mywebapp
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://database:5432/mydb
      - SPRING_DATASOURCE_USERNAME=dbuser
      - SPRING_DATASOURCE_PASSWORD=dbpass
    depends_on:
      - database
      - redis
    networks:
      - app-network
    restart: unless-stopped
    volumes:
      - ./logs:/app/logs

  # PostgreSQL Database
  database:
    image: postgres:14
    container_name: postgres-db
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=dbuser
      - POSTGRES_PASSWORD=dbpass
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - app-network
    restart: always

  # Redis Cache
  redis:
    image: redis:7-alpine
    container_name: redis-cache
    ports:
      - "6379:6379"
    networks:
      - app-network
    restart: always

volumes:
  postgres_data:

networks:
  app-network:
    driver: bridge
```

---

## Code-Lücken Beispiel Docker Compose:

```yaml
version: '3.8'

services:
  app:
    image: myapp:latest
    _________:
      - "____:8080"
    environment:
      - DB_HOST=________
      - DB_PORT=5432
    _________:
      - database
    restart: ______
  
  database:
    image: postgres:14
    volumes:
      - db_data:/___/___/postgresql/data
    environment:
      - POSTGRES_DB=mydb

_______:
  db_data:
```

**Lösung:**
```yaml
version: '3.8'

services:
  app:
    image: myapp:latest
    ports:
      - "8080:8080"
    environment:
      - DB_HOST=database
      - DB_PORT=5432
    depends_on:
      - database
    restart: always
  
  database:
    image: postgres:14
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=mydb

volumes:
  db_data:
```

---

## Docker Compose Befehle (Nice to know)

```bash
# Services starten
docker-compose up

# Im Hintergrund starten
docker-compose up -d

# Services stoppen
docker-compose down

# Services neu bauen
docker-compose build

# Logs anzeigen
docker-compose logs -f

# Services auflisten
docker-compose ps
```

---

# 📝 Zusammenfassung - Quick Reference

## HTTP - Merke dir:
✅ **GET** = Abrufen (200)
✅ **POST** = Erstellen (201)
✅ **PUT** = Komplett ersetzen (200/204)
✅ **PATCH** = Teilweise ändern (200)
✅ **DELETE** = Löschen (204)

## Liquibase - Merke dir:
✅ **BIGINT** für IDs
✅ **VARCHAR(n)** für Text
✅ **TIMESTAMP** für Datum/Zeit
✅ **primaryKey="true"** für Primary Keys
✅ **nullable="false"** für Pflichtfelder

## Docker Compose - Merke dir:
✅ **ports**: "Host:Container"
✅ **volumes**: Datenpersistenz
✅ **depends_on**: Startreihenfolge
✅ **environment**: Umgebungsvariablen
✅ **restart**: Neustart-Policy

---

# 🎯 Nächste Schritte

1. ✅ Diese Datei komplett durchlesen
2. ✅ Zu [CODE_BEISPIELE.md](CODE_BEISPIELE.md) für mehr Übungen
3. ✅ Code-Lücken selbst ausfüllen (Lösungen abdecken!)
4. ✅ Beispiele auf Spickzettel schreiben

**Du schaffst das! Diese drei Themen bringen dir die meisten Punkte!** 🚀
