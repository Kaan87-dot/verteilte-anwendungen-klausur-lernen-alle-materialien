# 🆘 MINIMAL SURVIVAL GUIDE - Nur das Nötigste!

## 🎯 ZIEL: 12-15 Punkte in der Klausur = BESTANDEN!

**Du hast bereits:** 37.5 Punkte ✅  
**Du brauchst noch:** ~12-15 Punkte  
**Gesamt zum Bestehen:** ~50 Punkte  
**Dein Vorteil:** Spickzettel erlaubt! 📄

---

## ⚡ DIE 3 GOLDENEN REGELN

1. **SPICKZETTEL = DEIN BESTER FREUND**
   - Alles was du brauchst steht da drin!
   - Du musst nur WISSEN wo was steht
   - Übung: Finde Sachen in 30 Sekunden!

2. **CODE-LÜCKEN = EINFACHSTE PUNKTE**
   - Mit Spickzettel sind sie geschenkt
   - Syntax musst du NICHT auswendig können
   - Muster erkennen genügt!

3. **FOKUS AUF HTTP, LIQUIBASE, DOCKER**
   - Das sind 80% der Code-Punkte
   - Rest ignorieren wenn Zeit knapp!

---

## 📚 ABSOLUTE MINIMUM: Was du WISSEN musst

### 1. HTTP (5-7 Punkte) - HÖCHSTE PRIORITÄT! 🔥

#### Was du im KOPF haben musst:

**HTTP Methoden - NUR DAS:**
```
GET    = Daten ABRUFEN      → Status: 200 OK
POST   = Neu ERSTELLEN      → Status: 201 Created  
PUT    = ÄNDERN (komplett)  → Status: 200 OK
DELETE = LÖSCHEN            → Status: 204 No Content
```

**Das reicht! Mehr brauchst du nicht im Kopf!**

#### Wie du Code-Lücken füllst:

**Muster 1 - Methode:**
```java
@___Mapping     // Frage dich: Was macht die Methode?
                // Abrufen? → GetMapping
                // Erstellen? → PostMapping
                // Ändern? → PutMapping
                // Löschen? → DeleteMapping
```

**Muster 2 - Status Code:**
```java
HttpStatus.___  // Frage dich: Was passiert?
                // Erfolgreich abgerufen? → OK (200)
                // Neu erstellt? → CREATED (201)
                // Nicht gefunden? → NOT_FOUND (404)
                // Erfolgreich gelöscht? → NO_CONTENT (204)
```

**Muster 3 - Request Body:**
```java
@RequestBody User ___  // Variable Name, meist "user", "data", "request"
```

**DAS WARS! Mit Spickzettel für Syntax-Details!**

---

### 2. Liquibase (4-5 Punkte) - WICHTIG! 🔥

#### Was du im KOPF haben musst:

**Grundstruktur - NUR DAS:**
```xml
<changeSet id="..." author="student">
    <createTable tableName="...">
        <column name="..." type="...">
            <constraints ... />
        </column>
    </createTable>
</changeSet>
```

**Datentypen - NUR DIE 3 WICHTIGSTEN:**
```
BIGINT        → IDs (immer!)
VARCHAR(100)  → Text (Namen, Email)
TIMESTAMP     → Datum/Zeit
```

**Das reicht! Rest im Spickzettel!**

#### Wie du Code-Lücken füllst:

**Muster 1 - ID Spalte (kommt IMMER):**
```xml
<column name="id" type="BIGINT" autoIncrement="true">
    <constraints primaryKey="true" nullable="false"/>
</column>
```

**Muster 2 - Text Spalte:**
```xml
<column name="name" type="VARCHAR(100)">
    <constraints nullable="false"/>
</column>
```

**Muster 3 - Zeitstempel:**
```xml
<column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP">
    <constraints nullable="false"/>
</column>
```

**DAS WARS! Spickzettel für komplexere Sachen!**

---

### 3. Docker Compose (3-4 Punkte) - GUT FÜR SCHNELLE PUNKTE! 🔥

#### Was du im KOPF haben musst:

**Grundstruktur - NUR DAS:**
```yaml
version: '3.8'

services:
  app:
    image: ...
    ports: ["8080:8080"]
    environment: [DB_HOST=database]
    
volumes:
  data:
```

**Wichtigste Felder - NUR DIE 4:**
```
image:       Was für Container? (postgres:14, nginx:latest)
ports:       "HOST:CONTAINER" (z.B. "8080:8080")
environment: Variablen (z.B. DB_HOST=database)
volumes:     Datenspeicher (z.B. data:/var/lib/...)
```

**Das reicht! Rest im Spickzettel!**

#### Wie du Code-Lücken füllst:

**Muster 1 - Ports:**
```yaml
ports:
  - "____:8080"    # Host-Port fehlt? Meist gleich wie Container!
                   # Antwort: "8080:8080"
```

**Muster 2 - Environment:**
```yaml
environment:
  - DB_HOST=_______   # Datenbank-Service Name
                      # Antwort: meist "database" oder "postgres"
```

**Muster 3 - Volumes:**
```yaml
volumes:
  - db_data:/var/lib/______/data
                # Was für DB? PostgreSQL!
                # Antwort: "postgresql"
```

**DAS WARS! Spickzettel für Details!**

---

### 4. Three-Tier Architecture (2-3 Punkte) - EINFACHE ZEICHNUNG! ✏️

#### Was du ZEICHNEN musst:

```
┌─────────────────────────┐
│   PRESENTATION TIER     │
│   (Frontend)            │
│                         │
│   React, Angular, HTML  │
└────────────┬────────────┘
             │
         HTTP/REST
             │
┌────────────▼────────────┐
│  BUSINESS LOGIC TIER    │
│  (Backend/API)          │
│                         │
│  Spring Boot, Node.js   │
└────────────┬────────────┘
             │
         SQL/JDBC
             │
┌────────────▼────────────┐
│      DATA TIER          │
│      (Database)         │
│                         │
│  PostgreSQL, MySQL      │
└─────────────────────────┘
```

**Beispiel AN DEN RAND schreiben:**
```
Beispiel Online-Shop:
- Frontend: Website (HTML/React)
- Backend: REST API (Spring Boot)  
- Database: Produktdaten (PostgreSQL)
```

**DAS WARS! Mehr brauchst du nicht!**

---

### 5. Communication Patterns (2-3 Punkte) - 2 PATTERNS GENÜGEN!

#### Pattern 1: Request/Response (SYNCHRON)

```
Client ────Request────→ Server
Client ←───Response──── Server
         (wartet)
```

**Eigenschaften:**
- Synchron (Client wartet)
- 1:1 Kommunikation
- Beispiel: HTTP REST API

#### Pattern 2: Publish/Subscribe (ASYNCHRON)

```
Publisher ────→ Topic ────→ Subscribers
                             (mehrere!)
```

**Eigenschaften:**
- Asynchron (Publisher wartet nicht)
- 1:N Kommunikation
- Beispiel: Event System

**Unterschied merken:**
- Request/Response = wartet = synchron
- Pub/Sub = wartet nicht = asynchron

**DAS WARS! Mehr brauchst du nicht!**

---

## 🎯 PUNKTEVERTEILUNG - Wo du punkten kannst

### Sichere Punkte (mit Spickzettel):
- ✅ **HTTP Code-Lücken**: 5-7 Punkte (EASY!)
- ✅ **Liquibase Code-Lücken**: 4-5 Punkte (MACHBAR!)
- ✅ **Docker Code-Lücken**: 3-4 Punkte (OK!)
- ✅ **Three-Tier Zeichnung**: 2-3 Punkte (GEÜBT!)

**TOTAL: 14-19 Punkte möglich!**

### Bonus-Punkte (wenn Zeit):
- ⭐ **Communication Patterns**: 2-3 Punkte
- ⭐ **Theoriefragen**: 1-2 Punkte
- ⭐ **OSI-Modell**: 1-2 Punkte

---

## ⚡ QUICK REFERENCE - Für Spickzettel-Rand

### HTTP Cheat:
```
GET    → 200
POST   → 201
PUT    → 200
DELETE → 204
404 = Not Found
500 = Server Error
```

### Liquibase Cheat:
```
ID:   BIGINT, primaryKey=true
Text: VARCHAR(100), nullable=false
Zeit: TIMESTAMP, defaultValueComputed
```

### Docker Cheat:
```
ports: ["8080:8080"]
environment: [DB_HOST=database]
depends_on: [database]
volumes: [data:/var/lib/...]
```

### Three-Tier Cheat:
```
Frontend (Presentation)
    ↓ HTTP
Backend (Business Logic)
    ↓ SQL
Database (Data)
```

---

## 🚨 IN DER KLAUSUR - STRATEGIE

### Phase 1: Erste 5 Minuten
1. **DURCHATMEN!** (3x tief)
2. **Übersicht:** Alle Seiten durchblättern
3. **Punkte zählen:** Wo gibt's wie viele Punkte?
4. **Spickzettel:** Neben die Klausur legen

### Phase 2: Easy Points First!
1. **HTTP Code-Lücken** (mit Spickzettel) → 5-7 Punkte ✅
2. **Liquibase Code-Lücken** (mit Spickzettel) → 4-5 Punkte ✅
3. **Docker Code-Lücken** (mit Spickzettel) → 3-4 Punkte ✅

**Nach 45 Min solltest du ~12-15 Punkte haben!**

### Phase 3: Zeichnungen
4. **Three-Tier Architecture zeichnen** → 2-3 Punkte ✅

**Nach 60 Min solltest du ~15-18 Punkte haben = BESTANDEN!**

### Phase 4: Bonus (wenn Zeit)
5. **Communication Patterns** → 2-3 Punkte
6. **Theoriefragen** → was du weißt

### Wichtig:
- ❌ **NICHT hängen bleiben!** Schwere Aufgabe? Skip!
- ✅ **Spickzettel nutzen!** Immer nachschauen!
- ✅ **Zeit im Auge behalten!** 1 Punkt = 4 Min
- ✅ **15 Punkte = ZIEL ERREICHT!** Rest ist Bonus!

---

## 💡 SPICKZETTEL EFFEKTIV NUTZEN

### Vor der Klausur:
1. **Post-Its kleben** an wichtige Seiten
   - "HTTP" Post-It
   - "Liquibase" Post-It  
   - "Docker" Post-It
2. **Wichtiges markieren** (Gelb-Marker)
3. **Üben:** "Wo finde ich X?" unter 30 Sekunden!

### In der Klausur:
1. **Lücke lesen** → Was fehlt?
2. **Spickzettel aufschlagen** → Richtige Seite finden
3. **Syntax abschauen** → In Klausur übertragen
4. **Anpassen** → Namen ändern, Rest gleich!

**Beispiel:**
```
Klausur fragt:  @___Mapping("/users")
Spickzettel:    @GetMapping("/api/...")
Antwort:        @GetMapping  (nur die Methode nehmen!)
```

---

## 🎓 MENTALE VORBEREITUNG

### Positive Gedanken:

✅ "Ich habe schon 37.5 Punkte!"  
✅ "Ich brauche nur 12-15 mehr!"  
✅ "Ich habe meinen Spickzettel!"  
✅ "Code-Lücken sind einfach!"  
✅ "Ich schaffe das!"

### Bei Panik:

1. **STOP!** Augen zu
2. **ATMEN!** 3x tief ein und aus
3. **FOKUS!** Was ist die nächste einfache Aufgabe?
4. **MACHEN!** Eine Aufgabe nach der anderen

### Denk dran:

> **"Du musst nicht alles können. Du musst nur 12 Punkte holen. Das schaffst du!"**

---

## ✅ CHECKLISTE - Hast du alles?

### Wissen (Minimal):
- [ ] HTTP: GET=200, POST=201, PUT=200, DELETE=204
- [ ] Liquibase: BIGINT (ID), VARCHAR (Text), TIMESTAMP (Zeit)
- [ ] Docker: ports, environment, volumes
- [ ] Three-Tier: 3 Schichten zeichnen können
- [ ] Patterns: Request/Response vs. Pub/Sub

### Material:
- [ ] Spickzettel ausgedruckt
- [ ] Post-Its geklebt
- [ ] Quick-Reference erstellt
- [ ] Stifte (3x)
- [ ] Wasser

### Mental:
- [ ] 6 Stunden geschlafen
- [ ] Gefrühstückt
- [ ] Positiv eingestellt
- [ ] "Ich schaffe das!"

---

## 🚀 DU SCHAFFST DAS!

### Warum?

1. ✅ **37.5 Punkte** sind schon da!
2. ✅ **Spickzettel** ist dein Cheat-Code!
3. ✅ **Code-Lücken** sind einfach!
4. ✅ **Plan** ist klar!
5. ✅ **Motivation** ist da!

### Dein Mantra:

> **"37.5 + 12 = BESTANDEN!"**  
> **"Spickzettel = Mein Freund!"**  
> **"Ich bin vorbereitet!"**

---

## 📞 QUICK HELP

### Wenn du in der Klausur nicht weiter weißt:

**Problem:** "Ich verstehe die Aufgabe nicht!"  
**Lösung:** Lies sie 2x langsam. Was wird genau gefragt?

**Problem:** "Ich finde nichts im Spickzettel!"  
**Lösung:** Nutze die Post-Its! Welches Thema? Dorthin blättern!

**Problem:** "Die Zeit wird knapp!"  
**Lösung:** Skip zu nächster einfacher Aufgabe! Jeder Punkt zählt!

**Problem:** "Ich habe Panik!"  
**Lösung:** Augen zu, 3x atmen, dann weiter. Du schaffst das!

---

## 🎯 ZUSAMMENFASSUNG - Das Wichtigste

1. **Du brauchst nur 12-15 Punkte** (von ~50)
2. **Spickzettel ist erlaubt** = Riesenvorteil!
3. **Code-Lücken mit Spickzettel** = Einfachste Punkte!
4. **Fokus:** HTTP, Liquibase, Docker = 80% der Punkte
5. **Strategie:** Easy Aufgaben zuerst, schwere skippen!
6. **Mental:** Ruhig bleiben, du schaffst das!

---

## ⏰ NÄCHSTER SCHRITT

1. Lies `SPICKZETTEL_STRATEGIE.md`
2. Folge `NOTFALL_24H_PLAN.md`
3. **STARTE JETZT!**

**LOS GEHT'S! DU KANNST DAS! 💪🎓**
