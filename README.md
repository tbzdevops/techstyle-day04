# Tag 04 — CI Grundlagen und GitHub CI

> **Projektauftrag TechStyle Online Shop.** Dieses Repository ist dein
> Startpunkt fuer Tag 4 und enthaelt den Stand nach Tag 3.

## Ausgangslage

Die Entwickler bei TechStyle beklagen sich ueber stundenlange manuelle Build-
und Test-Prozesse. Das Team implementiert heute die erste Continuous
Integration Pipeline, die automatisch Code-Aenderungen erkennt, Tests
ausfuehrt und Feedback an die Entwickler gibt. Diese Pipeline wird zum
Rueckgrat des gesamten weiteren Entwicklungsprozesses.

## Vorbereitung

Damit die Unit-Tests ohne echte SQLite-Datenbank laufen, muss die Datenbank
gemockt werden.

1. Ergaenze `requirements.txt` um `pytest`, `pytest-mock==3.15.0` und `flake8`.
2. Lege `tests/test_app.py` (Unit-Test der Startseite) und
   `tests/integration/test_workflow.py` (Registrierungs-Flow) an — der Code
   steht in der Tagesplanung.
3. Lege `conftest.py` im Projektstamm an, damit das Projektverzeichnis im
   `sys.path` liegt.

Lokal pruefen, bevor die Pipeline gebaut wird:

```bash
pip install -r requirements.txt
pytest tests/ -v                                                   # erwartet: 2 passed
flake8 tests/ conftest.py --max-line-length=100 --ignore=E302,W503 # erwartet: keine Ausgabe
```

## Aufgaben

### Aufgabe 1 — Repository-Setup und erste CI-Pipeline (45 Min)

Entwickle eine grundlegende CI-Pipeline, die bei jeder Code-Aenderung
automatisch ausgefuehrt wird.

- Lege `.github/workflows/ci.yml` an.
- Die Pipeline wird bei Push-Events ausgeloest und beruecksichtigt die
  Branching-Strategie (`main` und `day_*`).
- Installiere die Abhaengigkeiten mit `pip install -r requirements.txt`.

**Deliverable:** eine funktionsfaehige erste CI-Pipeline, die bei Push-Events
auf relevante Branches ausgeloest wird.

### Aufgabe 2 — Automatisierte Tests und Code-Qualitaet (45 Min)

Erweitere die Pipeline um automatisierte Tests fuer die E-Commerce-Anwendung.

- Integriere einen `pytest`-Schritt fuer `tests/`.
- Die Pipeline muss bei Fehlern fehlschlagen — kein `continue-on-error: true`.

**Deliverable:** eine erweiterte CI-Pipeline mit automatisierten Tests.

### Aufgabe 3 — Code-Qualitaet und Linter-Integration (45 Min)

Erzwinge konsistente Coding-Standards.

- Ergaenze einen Linter-Schritt (flake8, pylint, ruff oder black).
- Konfiguriere die Regeln, z. B.
  `flake8 tests/ conftest.py --max-line-length=100 --ignore=E302,W503`.
- Die Pipeline muss bei Style-Verstoessen abbrechen — kein `--exit-zero`
  und kein `|| true`.

**Deliverable:** eine CI-Pipeline mit umfassenden Code-Qualitaetspruefungen.

## Abnahmekriterien

Diese 16 Kriterien werden bei jedem Push automatisch geprueft:

**Aufgabe 1: Repository-Setup und erste CI-Pipeline**

- [ ] CI-Workflow-Datei vorhanden (`.github/workflows/ci.yml`)
- [ ] Workflow definiert mindestens einen Job (`jobs:`)
- [ ] Pipeline wird bei Push ausgeloest (`on: push`)
- [ ] Branching-Strategie beruecksichtigt (`main` und `day_*`)
- [ ] Abhaengigkeiten werden installiert (`pip install -r requirements.txt`)
- [ ] Test-Abhaengigkeiten in `requirements.txt` (pytest, pytest-mock)
- [ ] `conftest.py` im Projektstamm vorhanden

**Aufgabe 2: Automatisierte Tests und Code-Qualitaet**

- [ ] Unit-Tests vorhanden (`tests/test_*.py`)
- [ ] Integrationstest vorhanden (`tests/integration/test_*.py`)
- [ ] Tests pruefen die Flask-Anwendung (Test-Client)
- [ ] Test-Schritt in der Pipeline (pytest)
- [ ] Pipeline schlaegt bei Fehlern fehl (kein `continue-on-error`)

**Aufgabe 3: Code-Qualitaet und Linter-Integration**

- [ ] Linter in `requirements.txt` (flake8/pylint/ruff/black)
- [ ] Linter-Schritt in der Pipeline (flake8/pylint)
- [ ] Linter-Regeln konfiguriert (max-line-length/ignore/select)
- [ ] Pipeline bricht bei Style-Verstoessen ab (kein `--exit-zero` / `|| true`)

> Die Autograding-Pipeline `classroom.yml` ist von der Erkennung
> ausgenommen — sie zaehlt nicht als deine CI-Loesung.

## Abnahmekriterien selber pruefen

**Lokal** — jederzeit, ohne Push:

```bash
bash .github/classroom/grade.sh
```

Das Skript liest die Tagesnummer aus `.classroom50.yaml`. Du kannst sie auch
erzwingen:

```bash
CLASSROOM_DAY=4 bash .github/classroom/grade.sh
```

Die Ausgabe listet jedes Kriterium mit ✅ oder ❌ und nennt bei jedem ❌ den
konkreten Loesungshinweis. Am Ende steht die Zusammenfassung; sobald ein
Kriterium fehlt, endet das Skript mit Exit-Code 1.

**In GitHub** — bei jedem Push:

Der Workflow **🎓 Classroom Autograding** laeuft automatisch. Ergebnis im Tab
**Actions** → letzter Run → Job *Abnahmekriterien pruefen*. Jedes Kriterium
erscheint zusaetzlich als Annotation direkt im Run.

## Anwendung lokal starten

```bash
./run_dev.sh
```

Legt ein venv an, installiert die Abhaengigkeiten, seedet die Datenbank und
startet den Dev-Server auf http://localhost:5000. Admin-Panel unter `/admin`.

Hinweise zur Anwendung:

- Die Datenbank liegt unter `/tmp/techstyle.db`.
- `python seed_data.py` (im aktivierten venv) setzt die Produkte zurueck.
- Das Admin-Panel hat noch kein Login — das ist zum jetzigen Zeitpunkt so gewollt.
