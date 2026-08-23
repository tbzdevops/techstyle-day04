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

Diese Kriterien prueft die Pipeline bei jedem Push automatisch. **Die Haken
setzt die Pipeline selbst:** ein erfuelltes Kriterium wird abgehakt, und
sobald eine Aenderung es wieder bricht, verschwindet der Haken. Du musst hier
nichts von Hand pflegen — beim naechsten Push wird die Liste ueberschrieben.

<!-- c50:progress -->
**Fortschritt: 0 / 16 automatisch geprueften Kriterien erfuellt.** Noch nicht geprueft.
<!-- /c50:progress -->

- [ ] Aufgabe 1: CI-Workflow-Datei vorhanden (.github/workflows/ci.yml)
- [ ] Aufgabe 1: Workflow definiert mindestens einen Job (jobs:)
- [ ] Aufgabe 1: Pipeline wird bei Push ausgelöst (on: push)
- [ ] Aufgabe 1: Branching-Strategie berücksichtigt (main und day_*)
- [ ] Aufgabe 1: Abhängigkeiten werden installiert (pip install -r requirements.txt)
- [ ] Aufgabe 1: Test-Abhängigkeiten in requirements.txt (pytest, pytest-mock)
- [ ] Aufgabe 1: conftest.py im Projektstamm vorhanden
- [ ] Aufgabe 2: Unit-Tests vorhanden (tests/test_*.py)
- [ ] Aufgabe 2: Integrationstest vorhanden (tests/integration/test_*.py)
- [ ] Aufgabe 2: Tests prüfen die Flask-Anwendung (Test-Client)
- [ ] Aufgabe 2: Test-Schritt in der Pipeline (pytest)
- [ ] Aufgabe 2: Pipeline schlägt bei Fehlern fehl (kein continue-on-error)
- [ ] Aufgabe 3: Linter in requirements.txt (flake8/pylint/ruff/black)
- [ ] Aufgabe 3: Linter-Schritt in der Pipeline (flake8/pylint)
- [ ] Aufgabe 3: Linter-Regeln konfiguriert (max-line-length/ignore/select)
- [ ] Aufgabe 3: Pipeline bricht bei Style-Verstössen ab (kein --exit-zero / || true)

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

Der Workflow **🎓 Classroom Autograding** laeuft automatisch und hakt die
erfuellten Kriterien oben im README ab. Ergebnis im Tab
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
