# Backend- und API-Faehigkeiten

## Ueberblick

Backend- und API-Faehigkeiten werden fuer den Aufbau von Serverseit-Anwendungen, API-Design und Datenbankoperationen verwendet.

## Kernfaehigkeiten

### backend-patterns

**Zweck**: Backend-Entwicklungs-Best-Practices

**Kernkonzepte**:
- Schichtarchitektur (Controller → Service → Repository)
- Abhaengigkeitsinjektion
- Fehlerbehandlungsstrategien
- Caching-Muster
- Message-Queue-Integration

---

### api-design

**Zweck**: API-Design und RESTful-Konventionen

**Kernkonzepte**:
- RESTful-Routing-Design
- Request-/Response-Format
- Authentifizierung und Autorisierung
- Versionierungsstrategien
- Rate-Limiting und Caching

---

### api-connector-builder

**Zweck**: API-Connector-Builder-Tool

**Anwendungsszenarien**:
- Drittanbieter-API-Integration
- Webhook-Behandlung
- OAuth-Flow-Implementierung
- API-Fehler-Retry-Mechanismus

---

## Datenbankfaehigkeiten

### database-migrations

**Zweck**: Datenbank-Migrations-Management

**Kernkonzepte**:
- Migrationsdatei-Organisation
- Daten-Rollback-Strategien
- Seed-Daten-Management
- Zero-Downtime-Deployment

---

### mysql-patterns

**Zweck**: MySQL-Datenbank-Best-Practices

**Kernkonzepte**:
- Index-Optimierung
- Query-Optimierung
- Tabellen-Partitionierung
- Master-Slave-Replikation

---

### postgres-patterns

**Zweck**: PostgreSQL-Datenbank-Best-Practices

**Kernkonzepte**:
- JSONB-Operationen
- Volltextsuche
- Partitionierte Tabellen
- Erweiterte Indexstrategien

---

### redis-patterns

**Zweck**: Redis-Caching-Muster

**Kernkonzepte**:
- Caching-Strategien (Cache-Aside, Write-Through)
- Verteilte Locks
- Session-Storage
- Message Publish/Subscribe

---

### clickhouse-io

**Zweck**: ClickHouse Columnar-Storage

**Anwendungsszenarien**:
- Analytische Abfragen
- Logs-Speicherung
- Echtzeitanalyse
- Big-Data-Pipelines

---

## Message-Queues

### logistics-exception-management

**Zweck**: Logistik-Exception-Management

**Anwendungsszenarien**:
- Bestellausnahme-Behandlung
- Rueckgabe-Flows
- Bestandssynchronisation

---

## Zugehoerige Faehigkeiten

- `backend-patterns` - Backend-Muster
- `api-design` - API-Design
- `database-migrations` - Datenbank-Migrationen
- `mysql-patterns` - MySQL-Muster
- `postgres-patterns` - PostgreSQL-Muster
- `redis-patterns` - Redis-Muster
- `clickhouse-io` - ClickHouse