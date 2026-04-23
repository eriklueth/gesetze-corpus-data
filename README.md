# gesetze-corpus-data

**Versionierter Markdown-Korpus des deutschen Bundesrechts** — ein Commit pro Inkrafttretensdatum, backdated auf das `stand_datum` aus [gesetze-im-internet.de](https://www.gesetze-im-internet.de). `git log` auf eine Paragraphen-Datei ist dadurch direkt die Fassungsgeschichte dieses Paragraphen:

```bash
git log --follow --oneline -- laws/BJNR010050934/paragraphs/0007g.md
```

Dieses Repo enthält **ausschließlich Daten**, niemals Code. Die Pipeline, die die Daten erzeugt, liegt im Sibling-Repo [`gesetze-online-corpus`](https://github.com/eriklueth/gesetze-online-corpus). Die Webapp, die auf dem Korpus Suche und AI-Reasoning aufsetzt, liegt in [`gesetze-online-app`](https://github.com/eriklueth/gesetze-online-app).

## Kennzahlen

| | |
|---|---|
| Gesetze | **6876** |
| Abschnitte (§ / Art. / Anlage) | **98706** |
| `derived/corpus.jsonl` | **179.9 MB** |
| Letzter Export | **2026-04-18** UTC |
| Schema-Version | **v2** |

## Layout

```
laws/<BJNR>/
    meta.json                 # Metadaten des Gesetzes (jurabk, Titel, stand_datum, sha256, …)
    toc.json                  # kanonisch geordnete Abschnittsliste mit Dateiverweisen
    source.xml                # kanonisiertes GII-XML (c14n2)
    paragraphs/<NNNN[x]>.md   # eine Datei pro § oder Artikel
    annexes/<NNNN[x]>.md      # eine Datei pro Anlage

events/<YYYY>/<event_id>.json # eine Datei pro gruppiertem Inkrafttretensereignis

sources/current/gii-index.json
    # sha256 pro Gesetz-Slug, zur Delta-Erkennung zwischen Snapshots

derived/                      # wird von `gesetze-corpus export` neu erzeugt
    by-jurabk.json            # {jurabk: bjnr}
    by-bjnr.json              # {bjnr: {jurabk, title, stand_datum, section_counts}}
    corpus.jsonl              # eine JSON-Zeile pro gerendertem Abschnitt
```

Vollständiges Schema: [`SCHEMA.md`](SCHEMA.md).

## So sieht ein Abschnitt aus

```markdown
---
schema_version: v2
bjnr: BJNR001950896
jurabk: BGB
type: paragraph
number: "§ 14a"
heading: Begriffsbestimmung
breadcrumb:
  - Buch 1
  - Abschnitt 3
stand_datum: "2024-01-01"
source_xml: source.xml
---

# § 14a Begriffsbestimmung

(1) Erster Absatz.

(2) Zweiter Absatz.
```

Jede Paragraphen-Datei ist selbstbeschreibend: das YAML-Frontmatter trägt genug Metadaten, um Gesetz, Abschnitt, Breadcrumb und das `stand_datum` der im Commit eingefrorenen Fassung eindeutig zu identifizieren.

## Nutzung

### Repo klonen

```bash
git clone https://github.com/eriklueth/gesetze-corpus-data.git
```

Mit ~200 MB ist der Clone überschaubar; `--depth 1` reduziert auf einen Snapshot ohne Historie und ist in Sekunden durch.

### Volltext durchsuchen

```bash
jq -r 'select(.text | test("Investitionsabzug")) | [.jurabk, .number] | @tsv' \
    derived/corpus.jsonl
```

### BJNR zu einer jurabk nachschlagen

```bash
jq -r '."EStG"' derived/by-jurabk.json
# → BJNR010050934
```

### Alle Gesetze sehen, die an einem Tag geändert wurden

```bash
git log --since=2026-04-01 --until=2026-04-10 --oneline
```

### Einen Abschnitt in einer früheren Fassung lesen

```bash
git show <commit>:laws/BJNR001950896/paragraphs/0014a.md
```

### In eigenen Projekten konsumieren

Die Webapp [`gesetze-online-app`](https://github.com/eriklueth/gesetze-online-app) zeigt, wie man `derived/corpus.jsonl` + `derived/by-bjnr.json` als Quelle für einen Importer in Postgres / Supabase verwendet.

## Automatisierung

Dieses Repo wird **vollautomatisch** befüllt:

- Jeder Commit stammt aus der Pipeline (`law(...): stand <datum>` für Inhalt, `chore(sync): …` für Bookkeeping).
- Die History wird nach dem initialen Snapshot **niemals rebased** — Commit-SHAs sind dauerhaft stabil.
- Pull Requests auf die Daten selbst werden nicht angenommen. Datenfehler bitte als Issue im Tools-Repo melden: [`gesetze-online-corpus/issues`](https://github.com/eriklueth/gesetze-online-corpus/issues).
- Diese README-Datei wird bei jedem `gesetze-corpus export`-Lauf automatisch neu geschrieben; manuelle Änderungen gehen beim nächsten Sync verloren. Quelle des Templates: `gesetze-online-corpus/gesetze_corpus/ingest/export.py`.

## Lizenz

Die Gesetzestexte sind nach § 5 UrhG gemeinfrei. Struktur, Metadaten und abgeleitete Indizes in diesem Repo stehen unter **CC0-1.0** — siehe [`LICENSE`](LICENSE). Nutzung für beliebige Zwecke einschließlich kommerzieller Produkte ist zulässig, Attribution ist erwünscht, aber nicht erforderlich.

## Zitation

```
gesetze-corpus-data (2026). Versionierter Markdown-Korpus des deutschen Bundesrechts.
https://github.com/eriklueth/gesetze-corpus-data
```
