# gesetze-corpus-data

Versionierter Korpus deutscher Bundesgesetze. **Nur Daten**, kein Code.

Jeder Commit in diesem Repo repraesentiert entweder

- den initialen Snapshot aus dem amtlichen GII-XML, oder
- ein Inkrafttretensereignis (spaeter, via Events-Pipeline), oder
- einen explizit markierten `chore(canonical)`-Reformat-Lauf.

## Quellen

- Primaertext: gesetze-im-internet.de (GII), amtliches XML.
- Geplante Ergaenzung: recht.bund.de / NeuRIS (Verkuendungen, ELI).
- Geplante Ergaenzung: buzer.de (Aenderungs-Metadaten, niemals Text).

## Layout

Siehe `SCHEMA.md`.

## Lizenz

Die amtlichen Normtexte sind gemeinfrei nach § 5 UrhG. Die Struktur
und Metadaten dieses Repos werden unter CC0-1.0 veroeffentlicht
(siehe `LICENSE`).

## Erzeugt von

Das Tools-Repo `gesetze-online-corpus` betreibt die Pipeline.
Schema-Version: v1.
