# 🛠️ Instandhaltungssystem

Ein modernes Wartungs- und Auszeitenverwaltungssystem für die Überwachung und Planung von Systemausfällen und Wartungsarbeiten.

![Next.js](https://img.shields.io/badge/Next.js-15.0-black?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue?logo=typescript)
![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?logo=prisma)
![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?logo=mysql)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3.0-38B2AC?logo=tailwind-css)
![Vercel](https://img.shields.io/badge/Vercel-Hosting-000000?logo=vercel)
![Resend](https://img.shields.io/badge/Resend-Email-000000)

---

## 📋 Inhaltsverzeichnis

- [Überblick](#-überblick)
- [Funktionen](#-funktionen)
- [Technologie-Stack](#-technologie-stack)
- [Externe Dienste](#-externe-dienste)
- [Projektstruktur](#-projektstruktur)
- [Installation](#-installation)
- [Datenbank](#-datenbank)
- [E-Mail-System](#-e-mail-system)
- [Cron-Jobs](#-cron-jobs)
- [Sicherheit & Rollen](#-sicherheit)
- [API-Endpunkte](#-api-endpunkte)
- [Deployment](#-deployment)
- [Entwickler](#-entwickler)

> **Neu:** Das System enthält jetzt ein [Statusmeldung-System](#-statusmeldung-system) für die Kommunikation von Systemproblemen und ein [Wartungsanfragen-System](#-wartungsanfragen-system) für einen strukturierten Genehmigungsprozess.

---

## 🎯 Überblick

Das **Instandhaltungssystem** ist eine Webanwendung zur Verwaltung von geplanten Wartungsarbeiten und Systemauszeiten. Es besteht aus zwei Hauptbereichen:

### 🖥️ Öffentlicher Status-Monitor
- Echtzeit-Anzeige aller aktiven und geplanten Auszeiten
- Automatische Aktualisierung alle 10 Sekunden
- Kategorisierung nach: Heute, Nächste 7 Tage, Zukünftig
- Live-Uhr und Datumsanzeige

### 🔐 Admin-Panel
- Geschützter Bereich mit Benutzerauthentifizierung
- Erstellen, Bearbeiten und Löschen von Auszeiten
- Manuelles Starten/Beenden von Wartungsarbeiten
- Umfangreiche Statistiken und Berichte
- PDF-Export für Präsentationen

---

## ✨ Funktionen

### Auszeiten-Verwaltung
- ✅ Erstellen von geplanten Wartungsarbeiten
- ✅ Status-Workflow: PLANNED → ACTIVE → COMPLETED
- ✅ Manuelle Start/Ende/Abbruch-Steuerung
- ✅ Validierung: Enddatum muss nach Startdatum liegen

### Statistik & Berichte
- 📊 Quartals-/Halbjahres-/Jahresübersicht
- 📈 Jährliche und monatliche Diagramme (Recharts)
- 🔍 System- und Anlageteil-Analyse
- 📄 PDF-Export mit Firmenbranding
- 🔎 Filterung nach Datum, System, Anlageteil

### Stammdaten-Verwaltung
- 🏢 Systeme (z.B. ULD, Dolly, GPU)
- ⚙️ Anlageteile (pro System)
- 📝 Gründe (pro Anlageteil)
- 👥 Benutzerverwaltung mit Rollen

### 📧 E-Mail-Benachrichtigungssystem
- ✉️ Automatische E-Mail-Benachrichtigungen bei Auszeit-Ereignissen
- 📋 Anpassbare E-Mail-Vorlagen (Erstellt, Bearbeitet, Gestartet, Beendet, Ungeplant)
- ⏰ Erinnerungs-E-Mails (1 Tag vorher, 2 Stunden vorher)
- 👥 Flexible Empfängerverwaltung (einzeln oder alle auswählen)
- 🔄 Resend-API-Integration für zuverlässigen E-Mail-Versand
- ⚙️ Mail-Einstellungen im Admin-Panel konfigurierbar

### 🕐 Automatische Erinnerungen (Cron-Job)
- 📅 Erinnerung 1 Tag vor geplantem Start
- ⏱️ Erinnerung 2 Stunden vor geplantem Start
- 🔁 Automatische Überprüfung alle 5 Minuten
- ✅ Verhindert doppelten Versand bereits gesendeter Erinnerungen
- 🧠 Intelligente Deaktivierung: Erinnerungs-Optionen werden automatisch deaktiviert, wenn die Auszeit zu kurzfristig geplant ist

### 📋 Logbuch (Audit-Log)

Das Logbuch protokolliert alle wichtigen Benutzeraktionen im System:

#### Protokollierte Ereignisse

| Kategorie | Ereignisse |
|-----------|------------|
| **Authentifizierung** | Login, Logout, Fehlgeschlagene Anmeldeversuche |
| **Auszeiten** | Erstellen, Bearbeiten, Löschen, Starten, Beenden, Abbrechen |
| **Statusmeldungen** | Erstellen, Bearbeiten, Löschen, Abschließen, Kommentieren |
| **Systemdaten** | Systeme, Anlageteile, Gründe (Erstellen, Bearbeiten, Löschen) |
| **Benutzerverwaltung** | Benutzer erstellen, bearbeiten, löschen, Passwort ändern |
| **Export** | PDF-Export von Statistiken |

#### Logbuch-Funktionen
- 🔍 Filterung nach Benutzer, Aktion und Zeitraum
- 📅 Sortierung nach Datum (neueste zuerst)
- 👤 Anzeige des verantwortlichen Benutzers
- 📝 Detaillierte Aktionsbeschreibungen

### 📋 Wartungsanfragen-System

Ein strukturierter Genehmigungsprozess für geplante Wartungsarbeiten:

#### Workflow
```
GTMI erstellt Anfrage → GTMC/Administrator prüft → Genehmigung/Ablehnung → Auszeit wird automatisch erstellt
```

#### Status-Übergänge
| Status | Beschreibung |
|--------|--------------|
| **AUSSTEHEND** | Anfrage wartet auf Prüfung durch GTMC/Administrator |
| **GENEHMIGT** | Anfrage wurde genehmigt, Auszeit automatisch erstellt |
| **ABGELEHNT** | Anfrage wurde mit Begründung abgelehnt |

#### Berechtigungen
| Rolle | Anfrage erstellen | Eigene bearbeiten | Alle bearbeiten | Genehmigen/Ablehnen |
|-------|-------------------|-------------------|-----------------|---------------------|
| **Administrator** | ✅ | ✅ | ✅ | ✅ |
| **GTMC** | ✅ | - | ✅ | ✅ |
| **GTMI** | ✅ | ✅ | ❌ | ❌ |

#### Genehmigungsprozess
Bei der Genehmigung kann der GTMC/Administrator folgende Optionen konfigurieren:
- 📧 **E-Mail-Benachrichtigungen:** Welche E-Mails sollen gesendet werden?
- 👥 **Empfänger:** Wer soll benachrichtigt werden?
- 📝 **Notiz:** Optionale Bemerkung zur Genehmigung
- ⏰ **Erinnerungen:** 1 Tag / 2 Stunden vor Start (wenn zeitlich möglich)

#### Funktionen
- ✅ Anfrage erstellen mit System, Anlageteil, Grund, Zeitraum und Beschreibung
- ✅ Ausstehende Anfragen bearbeiten (eigene für GTMI, alle für GTMC/Administrator)
- ✅ Genehmigung mit konfigurierbaren E-Mail-Optionen
- ✅ Ablehnung mit Pflichtbegründung
- ✅ Automatische Auszeit-Erstellung bei Genehmigung
- ✅ Status-Filter (Alle, Ausstehend, Genehmigt, Abgelehnt)

### 📢 Statusmeldung-System

Eine zentrale Kommunikationsplattform für Systemprobleme und bekannte Störungen – unabhängig von geplanten Wartungsarbeiten.

#### Status-Typen
| Status | Beschreibung |
|--------|--------------|
| **GESPERRT** | System oder Komponente ist vollständig gesperrt |
| **NICHT VERFÜGBAR** | System ist derzeit nicht erreichbar |
| **KEIN KOMMUNIKATION** | Kommunikationsproblem mit dem System |
| **UNBEKANNT** | Status kann nicht ermittelt werden |

#### Funktionen
- ✅ Statusmeldungen erstellen mit System, Anlageteil, Grund und Status
- ✅ Aktiv/Abgeschlossen-Tabs zur Übersicht
- ✅ Meldung als abgeschlossen markieren (mit Erfassung des Bearbeiters)
- ✅ Kommentar-System für Teamkommunikation
- ✅ Rollenbasierte Berechtigungen (Administrator kann alle bearbeiten/löschen, andere nur eigene)
- ✅ Vollständige Protokollierung im Logbuch

#### Berechtigungen
| Aktion | Administrator | Andere |
|--------|-------|--------|
| Alle Meldungen bearbeiten/löschen | ✅ | ❌ |
| Eigene Meldungen bearbeiten/löschen | ✅ | ✅ |
| Abgeschlossene Meldungen löschen | ✅ | ❌ |
| Abgeschlossene Meldungen bearbeiten | ❌ | ❌ |
| Kommentare hinzufügen | ✅ | ✅ |

---

## 🛠️ Technologie-Stack

### Frontend
| Technologie | Version | Verwendung |
|-------------|---------|------------|
| **Next.js** | 15.0 | React-Framework mit App Router |
| **React** | 19.0 | UI-Bibliothek |
| **TypeScript** | 5.0 | Typsichere Entwicklung |
| **TailwindCSS** | 3.4 | Utility-First CSS-Styling |
| **Recharts** | 2.x | Interaktive Diagramme |
| **Material Symbols** | - | Google Icon-Bibliothek |

### Backend
| Technologie | Version | Verwendung |
|-------------|---------|------------|
| **Next.js API Routes** | 15.0 | RESTful API-Endpunkte |
| **Prisma ORM** | 6.x | Datenbank-Abstraktion & Migrations |
| **MySQL** | 8.x | Relationale Datenbank (PlanetScale kompatibel) |
| **bcryptjs** | 2.x | Passwort-Hashing mit Salt |

### Export & Reporting
| Bibliothek | Verwendung |
|------------|------------|
| **jsPDF** | PDF-Generierung für Berichte |
| **jspdf-autotable** | Tabellen in PDF-Berichten |

---

## 🌐 Externe Dienste

### Hosting: Vercel
| Aspekt | Details |
|--------|---------|
| **Platform** | [Vercel](https://vercel.com) |
| **Framework** | Next.js (optimiert) |
| **Region** | Frankfurt (fra1) |
| **Build** | Automatisch bei Git Push |
| **Domain** | Custom Domain möglich |

### E-Mail: Resend
| Aspekt | Details |
|--------|---------|
| **Dienst** | [Resend](https://resend.com) |
| **API** | REST API mit API-Key |
| **Vorlagen** | 7 anpassbare HTML-Templates |
| **Rate Limit** | 100 E-Mails/Tag (Free), 50.000/Monat (Pro) |

### Cron-Jobs: cron-job.org
| Aspekt | Details |
|--------|---------|
| **Dienst** | [cron-job.org](https://cron-job.org) |
| **Intervall** | Alle 5 Minuten |
| **Endpoint** | `/api/cron/reminders` |
| **Authentifizierung** | Optional via `CRON_SECRET` |

### Datenbank: PlanetScale / MySQL
| Aspekt | Details |
|--------|---------|
| **Typ** | MySQL 8.x kompatibel |
| **Hosting** | PlanetScale oder selbst-gehostet |
| **Connection** | Via `DATABASE_URL` Umgebungsvariable |
| **SSL** | Erforderlich für Cloud-Datenbanken |

---

## 📁 Projektstruktur

```
src/
├── app/                          # Next.js App Router
│   ├── page.tsx                  # Öffentlicher Status-Monitor
│   ├── layout.tsx                # Root Layout
│   ├── admin/                    # Admin-Bereich
│   │   ├── dashboard/            # Dashboard mit Statistiken & Trends
│   │   ├── downtimes/            # Auszeiten-Verwaltung
│   │   │   ├── page.tsx          # Aktive & Geplante Auszeiten
│   │   │   ├── new/              # Neue Auszeit erstellen
│   │   │   └── [id]/edit/        # Auszeit bearbeiten
│   │   ├── wartungsanfragen/     # Wartungsanfragen-System
│   │   │   ├── page.tsx          # Anfragen-Übersicht
│   │   │   └── new/              # Neue Anfrage (GTMI)
│   │   ├── statusmeldung/        # Statusmeldung-System
│   │   │   └── page.tsx          # Meldungen-Übersicht mit Tabs
│   │   ├── statistics/           # Statistiken
│   │   │   ├── page.tsx          # Übersicht mit Diagrammen
│   │   │   └── completed/        # Beendete Auszeiten
│   │   ├── system-settings/      # Systemdaten-Verwaltung
│   │   │   ├── systems/          # Systeme
│   │   │   ├── components/       # Anlageteile
│   │   │   └── reasons/          # Gründe
│   │   ├── users/                # Benutzerverwaltung
│   │   ├── logbuch/              # Audit-Log
│   │   ├── mail-settings/        # E-Mail-Konfiguration
│   │   ├── help/                 # Hilfe & FAQ
│   │   ├── documentation/        # Dokumentation
│   │   └── login/                # Login-Seite
│   └── api/                      # API-Routen
│       ├── auth/                 # Authentifizierung (login, logout, change-password)
│       ├── downtimes/            # Auszeiten-CRUD + Status
│       ├── wartungsanfragen/     # Anfragen-CRUD + Approve/Reject
│       ├── statusmeldung/        # Statusmeldung-CRUD + Comments
│       ├── statistics/           # Statistik-Daten
│       ├── mail/                 # Mail-System (settings, recipients, templates)
│       ├── cron/                 # Cron-Jobs (reminders)
│       ├── system-categories/    # System-Stammdaten
│       ├── anlageteile/          # Anlageteil-Stammdaten
│       ├── gruende/              # Gründe-Stammdaten
│       ├── users/                # Benutzer-CRUD
│       └── audit-log/            # Logbuch-Einträge
│
├── components/                   # React-Komponenten
│   ├── AdminLayout.tsx           # Admin-Layout mit Sidebar & Navigation
│   ├── DowntimeForm.tsx          # Auszeit-Erstellungsformular
│   ├── DowntimeEditForm.tsx      # Auszeit-Bearbeitungsformular
│   ├── DowntimeActions.tsx       # Aktions-Buttons (Edit, Delete, View)
│   ├── DowntimeStatusActions.tsx # Start/Ende/Abbruch-Buttons
│   ├── TrendChart.tsx            # Jahres-Trend-Diagramm
│   ├── StatisticsCharts.tsx      # Monatliche Diagramme
│   ├── YearlyCharts.tsx          # Jährliche Vergleichsdiagramme
│   ├── QuarterlyCards.tsx        # Quartals-Statistik-Karten
│   ├── SystemAnalysis.tsx        # System-Analyse-Komponente
│   ├── AnlageteilAnalysis.tsx    # Anlageteil-Analyse-Komponente
│   ├── ExportStatisticsModal.tsx # PDF-Export-Modal
│   ├── UngeplanteOverview.tsx    # Ungeplante Auszeiten Übersicht
│   ├── UngeplanteExportModal.tsx # Ungeplante Export-Modal
│   ├── UserManagement.tsx        # Benutzerverwaltung-Komponente
│   ├── SystemsManager.tsx        # Systeme-Verwaltung
│   ├── AnlageteileManager.tsx    # Anlageteile-Verwaltung
│   ├── GruendeManager.tsx        # Gründe-Verwaltung
│   ├── SearchableSelect.tsx      # Suchbare Dropdown-Komponente
│   ├── ContactTab.tsx            # Mail-Kontakt-Tab
│   ├── RecipientsTab.tsx         # Mail-Empfänger-Tab
│   ├── TemplatesTab.tsx          # Mail-Vorlagen-Tab
│   ├── LogbuchFilters.tsx        # Logbuch-Filterkomponente
│   ├── CompletedDowntimesFilters.tsx # Filter für beendete Auszeiten
│   ├── StatisticsPageClient.tsx  # Client-Komponente für Statistiken
│   ├── LiveClock.tsx             # Echtzeit-Uhr
│   └── AutoRefresh.tsx           # Auto-Refresh für Status-Monitor
│
├── lib/                          # Hilfsfunktionen & Utilities
│   ├── prisma.ts                 # Prisma-Client Singleton
│   ├── session.ts                # Session-Verwaltung (Cookie-basiert)
│   ├── auth.ts                   # Authentifizierungs-Helpers
│   ├── mail.ts                   # E-Mail-Versand (Resend)
│   └── mail-templates.ts         # E-Mail-Template-Generierung
│
├── hooks/                        # Custom React Hooks
│   └── useAutoLogout.ts          # Auto-Logout nach Inaktivität
│
└── prisma/
    ├── schema.prisma             # Datenbank-Schema (10 Models)
    ├── seed-statistics.ts        # Test-Daten 2025
    └── seed-statistics-2024.ts   # Test-Daten 2024
```

---

## 🚀 Installation

### Voraussetzungen
- Node.js 18+
- MySQL-Datenbank
- npm oder yarn

### Schritte

1. **Repository klonen**
```bash
git clone https://github.com/w4ngu/Instandhaltungssystem.git
cd Instandhaltungssystem
```

2. **Abhängigkeiten installieren**
```bash
npm install
```

3. **Umgebungsvariablen konfigurieren**
```bash
# .env erstellen
DATABASE_URL="mysql://user:password@host:3306/database"
```

4. **Datenbank migrieren**
```bash
npx prisma db push
npx prisma generate
```

5. **Entwicklungsserver starten**
```bash
npm run dev
```

6. **Öffnen im Browser**
- Status-Monitor: http://localhost:3000
- Admin-Panel: http://localhost:3000/admin

---

## 🗄️ Datenbank

### Schema-Übersicht (12 Models) - Normalisiert

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  SystemCategory │────<│   Anlageteil    │────<│      Grund      │
├─────────────────┤     ├─────────────────┤     ├─────────────────┤
│ ID              │     │ ID              │     │ ID              │
│ Name            │     │ Name            │     │ Name            │
│ CreatedAt       │     │ SystemCategoryID│     │ AnlageteilID    │
└────────┬────────┘     │ CreatedAt       │     │ CreatedAt       │
         │              └────────┬────────┘     └────────┬────────┘
         │                       │                       │
         │              ┌────────┴────────┐              │
         │              │                 │              │
         │      ┌───────▼─────────────────▼───────┐      │
         │      │         Downtime                │      │
         │      ├─────────────────────────────────┤      │
         │      │ ID                              │      │
         │      │ AnlageteilID (FK) ◄─────────────┼──────┘
         │      │ GrundID (FK) ◄──────────────────┘
         │      │ Restriction                     │
         │      │ StartDate, EndDate              │
         │      │ Status (PLANNED/ACTIVE/COMPLETED)
         │      │ IsUnplanned                     │
         │      │ MailSent* (7 Flags)             │
         │      │ MailRecipientIds                │
         │      │ MailSelectedEvents              │
         │      │ CreatedAt, CompletedAt          │
         │      └──────────────┬──────────────────┘
         │                     │
         │      ┌──────────────┴──────────────────┐
         │      │     MaintenanceRequest          │
         │      ├─────────────────────────────────┤
         │      │ ID                              │
         │      │ AnlageteilID (FK)               │
         │      │ GrundID (FK)                    │
         │      │ Restriction                     │
         │      │ StartDate, EndDate              │
         │      │ Description                     │
         │      │ Status (PENDING/APPROVED/REJECTED)
         │      │ RequestedByID, ReviewedByID     │
         │      │ ReviewNote, DowntimeID          │
         │      │ CreatedAt, UpdatedAt            │
         │      └─────────────────────────────────┘

┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│      User       │     │    AuditLog     │     │  MailSettings   │
├─────────────────┤     ├─────────────────┤     ├─────────────────┤
│ ID              │     │ ID, UserID      │     │ SMTP Config     │
│ Username        │     │ ActionDescription│    │ Sender Info     │
│ Password (Hash) │     │ Timestamp       │     │ Notification    │
│ Role (String)   │     └─────────────────┘     │  Preferences    │
│                 │                             └─────────────────┘
└─────────────────┘     
                        ┌─────────────────┐     ┌─────────────────┐
┌─────────────────┐     │  MailTemplate   │     │  SystemSetting  │
│  MailRecipient  │     ├─────────────────┤     ├─────────────────┤
├─────────────────┤     │ Type, Name      │     │ ID, Key, Value  │
│ ID, Email, Name │     │ Subject, Body   │     │ CreatedAt       │
│ IsActive        │     │ IsActive        │     └─────────────────┘
└─────────────────┘     └─────────────────┘
```

### Normalisierung (v2.0 - Dezember 2025)
Die Datenbank wurde vollständig normalisiert:
- **Vorher:** `system`, `component`, `reason` als String-Felder
- **Nachher:** `anlageteilId`, `grundId` als Foreign Keys

**Vorteile:**
- ✅ Keine Dateninkonsistenz mehr möglich
- ✅ Änderungen an System/Komponente wirken sich automatisch aus
- ✅ Referentielle Integrität durch Foreign Key Constraints

### Status-Enum
| Status | Beschreibung |
|--------|--------------|
| `PLANNED` | Geplante Auszeit (noch nicht gestartet) |
| `ACTIVE` | Laufende Wartungsarbeit |
| `COMPLETED` | Abgeschlossene Auszeit |

---

## 🔒 Sicherheit

### Authentifizierung
- **Cookie-basierte Sessions**: `auth-session` Cookie mit JSON-Payload
- **Passwort-Hashing**: bcryptjs mit Salt-Rounds
- **Geschützte Routen**: Server-seitige Session-Prüfung

### Granulares Berechtigungssystem

Das System verwendet ein **Discord-ähnliches granulares Berechtigungssystem** mit Systemrollen und der Möglichkeit, benutzerdefinierte Rollen zu erstellen.

#### Systemrollen (nicht löschbar/umbenennbar)

| Rolle | Beschreibung | Standard-Berechtigungen |
|-------|--------------|-------------------------|
| `Administrator` | Vollzugriff | Alle Berechtigungen aktiviert |
| `GTMC` | Steuerzentrale | Auszeiten, Wartungsanfragen, Statistiken, E-Mail-Einstellungen |
| `GTMI` | Instandhaltung | Dashboard, Statistiken, Wartungsanfragen erstellen |

#### Benutzerdefinierte Rollen
Administratoren können eigene Rollen erstellen und jede Berechtigung einzeln aktivieren/deaktivieren.

#### Berechtigungskategorien

| Kategorie | Berechtigungen |
|-----------|----------------|
| **Dashboard** | Anzeigen, Statistiken, Auszeit-Button, Letzte Aktivitäten |
| **Auszeiten** | Seite anzeigen, Erstellen, Bearbeiten, Löschen, Starten, Beenden |
| **Statusmeldung** | Anzeigen, Erstellen, Eigene/Alle bearbeiten, Eigene/Alle löschen |
| **Wartungsanfragen** | Erstellen, Anzeigen, Genehmigen, Bearbeiten, Löschen |
| **Statistiken** | Anzeigen, PDF-Export |
| **Benutzerverwaltung** | Anzeigen, Erstellen, Bearbeiten, Löschen, Passwort ändern |
| **Rollen** | Anzeigen, Verwalten (erstellen/bearbeiten/löschen) |
| **Berechtigungen** | Anzeigen, Verwalten |
| **E-Mail** | Anzeigen, Einstellungen, Vorlagen, Empfänger, Kontakt |
| **Stammdaten** | Anzeigen, Systeme, Anlageteile, Gründe verwalten |
| **Logbuch** | Anzeigen |

#### Berechtigungsmatrix (Standard)

| Funktion | Administrator | GTMC | GTMI |
|----------|---------------|------|------|
| Dashboard | ✅ | ✅ | ✅ |
| Auszeiten erstellen/bearbeiten | ✅ | ✅ | ❌ |
| **Wartungsanfragen erstellen** | ✅ | ✅ | ✅ |
| **Wartungsanfragen genehmigen** | ✅ | ✅ | ❌ |
| Benutzerverwaltung | ✅ | Ansicht | ❌ |
| Rollen & Berechtigungen | ✅ | ❌ | ❌ |
| Systemdaten | ✅ | ✅ | ❌ |
| Mail Einstellungen | ✅ | ✅ | ❌ |
| Logbuch | ✅ | ✅ | ❌ |

> **Hinweis:** Diese Berechtigungen sind nur Standardwerte. Administratoren können jede Rolle individuell konfigurieren.

### Sicherheitsmaßnahmen
- ✅ Passwort-geschütztes Löschen von Auszeiten
- ✅ Session-Validierung bei API-Aufrufen
- ✅ Audit-Logging für wichtige Aktionen
- ✅ Input-Validierung auf Client und Server
- ✅ CSRF-Schutz durch Next.js
- ✅ Systemrollen können nicht gelöscht oder umbenannt werden

---

## 🔌 API-Endpunkte

### Authentifizierung
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| POST | `/api/auth/login` | Benutzer-Anmeldung |
| POST | `/api/auth/logout` | Benutzer-Abmeldung |

### Auszeiten
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| GET | `/api/downtimes` | Alle Auszeiten abrufen |
| POST | `/api/downtimes` | Neue Auszeit erstellen |
| GET | `/api/downtimes/[id]` | Einzelne Auszeit abrufen |
| PATCH | `/api/downtimes/[id]` | Auszeit aktualisieren |
| DELETE | `/api/downtimes/[id]` | Auszeit löschen |
| PATCH | `/api/downtimes/[id]/status` | Status ändern (start/end/cancel) |

### Statistiken
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| GET | `/api/statistics?type=quarterly` | Quartals-Statistiken |
| GET | `/api/statistics?type=monthly` | Monatliche Statistiken |
| GET | `/api/statistics?type=yearly` | Jährliche Statistiken |
| GET | `/api/statistics?type=system` | System-Analyse |
| GET | `/api/statistics?type=anlageteil` | Anlageteil-Analyse |

### Wartungsanfragen
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| GET | `/api/wartungsanfragen` | Alle Anfragen abrufen |
| POST | `/api/wartungsanfragen` | Neue Anfrage erstellen |
| GET | `/api/wartungsanfragen/[id]` | Einzelne Anfrage abrufen |
| PATCH | `/api/wartungsanfragen/[id]` | Anfrage aktualisieren |
| POST | `/api/wartungsanfragen/[id]/approve` | Anfrage genehmigen (erstellt Auszeit) |
| POST | `/api/wartungsanfragen/[id]/reject` | Anfrage ablehnen |

### Statusmeldungen
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| GET | `/api/statusmeldung` | Alle Meldungen abrufen (?completed=true/false) |
| POST | `/api/statusmeldung` | Neue Meldung erstellen |
| GET | `/api/statusmeldung/[id]` | Einzelne Meldung abrufen |
| PUT | `/api/statusmeldung/[id]` | Meldung aktualisieren |
| DELETE | `/api/statusmeldung/[id]` | Meldung löschen |
| PATCH | `/api/statusmeldung/[id]` | Meldung abschließen (action: complete) |
| GET | `/api/statusmeldung/[id]/comments` | Kommentare abrufen |
| POST | `/api/statusmeldung/[id]/comments` | Kommentar hinzufügen |

### Stammdaten
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| GET/POST | `/api/systems` | Systeme verwalten |
| GET/POST | `/api/anlageteile` | Anlageteile verwalten |
| GET/POST | `/api/gruende` | Gründe verwalten |
| GET/POST | `/api/users` | Benutzer verwalten |

### E-Mail-System
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| GET/PUT | `/api/mail/settings` | Mail-Einstellungen verwalten |
| GET/POST/DELETE | `/api/mail/recipients` | Empfänger verwalten |
| GET/PUT | `/api/mail/templates` | E-Mail-Vorlagen verwalten |
| POST | `/api/mail/send-template` | Test-E-Mail senden |

### Cron-Jobs
| Methode | Endpunkt | Beschreibung |
|---------|----------|--------------|
| GET | `/api/cron/reminders` | Erinnerungs-E-Mails prüfen und senden |

---

## 📊 Test-Daten generieren

Für Entwicklungs- und Testzwecke können simulierte Auszeiten generiert werden:

```bash
# 2025 Test-Daten
npx ts-node prisma/seed-statistics.ts

# 2024 Test-Daten
npx ts-node prisma/seed-statistics-2024.ts
```

---

## 📧 E-Mail-System

### Übersicht
Das System verwendet **Resend** als E-Mail-Dienst für zuverlässigen Versand von Benachrichtigungen.

### E-Mail-Vorlagen (7 Typen)
| Vorlage | Beschreibung | Header-Farbe |
|---------|--------------|--------------|
| **Ungeplant** | Sofortige Warnung bei ungeplanten Ausfällen | 🔴 Rot |
| **Erstellt** | Neue Auszeit wurde angelegt | 🟢 Grün |
| **Bearbeitet** | Auszeit wurde geändert | 🟣 Indigo |
| **Erinnerung 1 Tag** | 24 Stunden vor Beginn | 🟡 Amber |
| **Erinnerung 2 Std.** | 2 Stunden vor Beginn | 🟠 Orange |
| **Gestartet** | Auszeit ist jetzt aktiv | 🔵 Cyan |
| **Beendet** | Auszeit wurde abgeschlossen | 🟢 Grün |

### Verfügbare Template-Variablen
```
{{system}}        - Name des Systems (z.B. ULD)
{{component}}     - Anlageteil (z.B. ET00C/1)
{{startDate}}     - Startdatum und -zeit
{{endDate}}       - Enddatum und -zeit
{{reason}}        - Grund der Auszeit
{{restriction}}   - Betriebseinschränkung
{{duration}}      - Dauer (nur bei Beendet)
{{contactEmail}}  - Kontakt-E-Mail für Rückfragen
```

### Intelligente Erinnerungen
Das Formular deaktiviert automatisch Erinnerungs-Optionen basierend auf der Startzeit:
- **< 2 Stunden bis Start** → "Erinnerung (2 Std.)" wird deaktiviert
- **< 24 Stunden bis Start** → "Erinnerung (1 Tag)" wird deaktiviert

---

## ⏰ Cron-Jobs

### Erinnerungs-Cron (`/api/cron/reminders`)

**Funktion:** Prüft alle geplanten Auszeiten und sendet Erinnerungs-E-Mails.

**Zeitfenster:**
| Erinnerung | Fenster |
|------------|---------|
| 1 Tag vorher | 22-26 Stunden vor Start |
| 2 Stunden vorher | 1,5-2,5 Stunden vor Start |

**Timezone-Handling:**
- Alle Zeiten werden als "Fake UTC" gespeichert (lokale Zeit mit Z-Suffix)
- Cron berücksichtigt dynamisch CET/CEST-Offset
- Display: `timeZone: 'UTC'` für konsistente Anzeige

**Setup bei cron-job.org:**
1. Account erstellen auf [cron-job.org](https://cron-job.org)
2. Neuer Cronjob:
   - URL: `https://ihre-domain.vercel.app/api/cron/reminders`
   - Ausführung: Alle 5 Minuten
   - Methode: GET
   - Optional: Header `Authorization: Bearer YOUR_CRON_SECRET`

---

## 🌐 Deployment

### Vercel (Empfohlen)

1. **Repository verbinden**
   - GitHub-Repo mit Vercel verbinden
   - Framework: Next.js (automatisch erkannt)

2. **Umgebungsvariablen setzen**
```env
DATABASE_URL="mysql://user:pass@host:3306/db?ssl={"rejectUnauthorized":true}"
RESEND_API_KEY="re_xxxxxxxxxxxxx"
CRON_SECRET="optional-sicherer-string"
```

3. **Build-Einstellungen**
   - Build Command: `prisma generate && next build`
   - Output Directory: `.next`

4. **Deployment**
   - Automatisch bei jedem Push auf `main`

### Umgebungsvariablen (Vollständig)
| Variable | Beschreibung | Erforderlich |
|----------|--------------|--------------|
| `DATABASE_URL` | MySQL Connection String | ✅ Ja |
| `RESEND_API_KEY` | API-Key von Resend | ✅ Ja (für E-Mails) |
| `CRON_SECRET` | Schutz für Cron-Endpoint | ❌ Optional |

---

## 📱 Screenshots

### Öffentliche Statusseite
- Echtzeit-Anzeige aktiver Auszeiten
- Automatische Aktualisierung alle 10 Sekunden
- Kategorisierung: Heute, Nächste 7 Tage, Zukünftig

### Admin-Dashboard
- Statistik-Karten (Aktiv, Heute, Diese Woche, Gesamt)
- Trend-Diagramm
- Letzte Aktivitäten

### Auszeiten-Verwaltung
- Filterfunktion nach System, Typ, Status
- Inline-Aktionen (Starten, Beenden, Bearbeiten, Löschen)
- Mail-Status-Indikatoren

---

## 👨‍💻 Entwickler

**Entwickelt von E. Ugurcan Cam, GTMC**

### Kontakt
- GitHub: [@w4ngu](https://github.com/w4ngu)

---

## 📄 Lizenz

Dieses Projekt wurde von E. Ugurcan Cam unter Einsatz von KI-Technologien und eigenständigem Fachwissen Aentwickelt.

---

*Letzte Aktualisierung: Januar 2026*
