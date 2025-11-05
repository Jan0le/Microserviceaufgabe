# Microservices-Monorepo

Microservices-basierte Anwendung für Event-Management mit Teilnehmern, Workouts, Analytics und mehr.

## Projektstruktur

### Services

- **participant** - Teilnehmerverwaltung
- **workout** - Workout-Management
- **analytics** - Datenanalyse und Statistiken
- **recommendation** - Empfehlungssystem
- **event** - Event-Management
- **ticket** - Ticket-Verwaltung
- **vehicle** - Fahrzeugverwaltung
- **booking** - Buchungssystem
- **payment** - Zahlungsabwicklung

### Frontend

- **view/web** - Web-Frontend

### Dokumentation

- **docs/openapi** - OpenAPI-Spezifikationen für alle Services

### Operations

- **ops/ci** - CI/CD-Konfigurationen
- **ops/docker** - Docker-Konfigurationen

## Git-Flow Workflow

### Branches

- **main** - Produktions-Branch (nur über Release/Hotfix)
- **develop** - Entwicklungs-Branch (Hauptentwicklungszweig)
- **feature/\<service>/\<beschreibung>** - Feature-Branches
- **release/\<version>** - Release-Branches (optional)
- **hotfix/\<beschreibung>** - Hotfix-Branches für Produktionsfixes

### Workflow

#### Feature-Branch erstellen

```bash
git checkout develop
git pull
git checkout -b feature/workout/create-endpoint
# ... Code ändern ...
git add .
git commit -m "feat(workout): create endpoint POST /workouts"
git push -u origin feature/workout/create-endpoint
```

#### Pull Request

1. PR von `feature/*` → `develop` erstellen
2. Reviewer aus CODEOWNERS werden automatisch zugewiesen
3. Checkliste im PR-Template abarbeiten
4. Nach Review: Rebase vor Merge
5. Merge per Squash oder Rebase (kein Merge-Commit)

#### Rebase vor Merge

```bash
git fetch origin
git rebase origin/develop
# Konflikte lösen, dann:
git push --force-with-lease
```

#### Release-Branch (optional)

```bash
git checkout develop
git pull
git checkout -b release/2025-11-sprint2
git push -u origin release/2025-11-sprint2
```

Nach Stabilisierung:
- PR: `release/*` → `main`
- Tag setzen: `git tag -a v2025.11.2 -m "Sprint 2 release"`
- `main` → `develop` zurückmergen

#### Hotfix

```bash
git checkout main
git pull
git checkout -b hotfix/payment-3ds-timeout
# Fix implementieren
git commit -m "fix(payment): handle 3DS timeout properly"
git push -u origin hotfix/payment-3ds-timeout
```

Nach Merge:
- Tag: `v2025.11.2-hotfix1`
- `main` → `develop` zurückmergen

## Commit-Messages

Verwende [Conventional Commits](https://www.conventionalcommits.org/):

- `feat(service): Beschreibung` - Neue Features
- `fix(service): Beschreibung` - Bugfixes
- `chore(service): Beschreibung` - Wartungsaufgaben
- `docs(service): Beschreibung` - Dokumentation
- `refactor(service): Beschreibung` - Refactoring

## Branch-Schutz-Regeln

### main

- ✅ Require PR before merging (2 Reviewer)
- ✅ Dismiss stale approvals
- ✅ Require status checks to pass
- ✅ Require linear history
- ✅ Restrict who can push (nur Admins/CI)

### develop

- ✅ Require PR before merging (1-2 Reviewer)
- ✅ Require status checks to pass
- ✅ Allow merge: Squash oder Rebase

**Hinweis:** Branch-Schutz-Regeln müssen in GitHub UI konfiguriert werden (Settings → Branches → Add rule).

## CI/CD

GitHub Actions CI läuft automatisch bei:
- Pull Requests zu `develop` oder `main`
- Pushes zu `develop`

Alle Services werden parallel gebaut und getestet.

## OpenAPI und Contract-Tests

Jede API-Änderung muss:
1. OpenAPI-Spec in `/docs/openapi/<service>.yaml` aktualisieren
2. Contract-Tests in `services/<service>/tests` aktualisieren

CI schlägt fehl, wenn Spec fehlt oder Tests rot sind.

## Setup

### Voraussetzungen

- Git installiert
- GitHub-Konto
- .NET 8.0 SDK (für Services)
- Node.js 20 (für Frontend)

### Repository klonen

```bash
git clone https://github.com/Jan0le/Microserviceaufgabe.git
cd Microserviceaufgabe
git checkout develop
```

## Team-Rollen

- **Infra/CI/Docs** - Pflege von Workflows, CODEOWNERS, OpenAPI-Ordnung
- **Service-Maintainer** - Pro Service min. 2 Maintainer als Reviewer
- **Frontend-Team** - Review von View-PRs plus API-Verträge

## Labels und Issues

- **service:** workout, payment, etc.
- **type:** feat, fix, etc.
- **priority:** high, medium, low
- **breaking:** Breaking Changes

Pro Task ein Issue. PRs müssen Issues referenzieren: "Fixes #123".

