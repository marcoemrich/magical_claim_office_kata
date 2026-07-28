# Trainer-Material — Magische Schadenskasse

**Nicht für Teilnehmer.** Alles in diesem Verzeichnis verrät die
Auflösungen: welche Lesart die Schadenskasse bei jeder Mehrdeutigkeit
gewählt hat, und welche Werte dabei herauskommen.

## Was hier liegt

| Datei | Inhalt |
|-------|--------|
| [`trainer-notes.md`](trainer-notes.md) | Auflösung aller Mehrdeutigkeiten und der übrigen Festlegungen. |
| [`workshop-durchfuehrung.md`](workshop-durchfuehrung.md) | Ablaufvorschlag, Trainer-Tipps, Stolpersteine, Tonalität. |
| [`verifikation/`](verifikation/) | Zehn Akzeptanz-Szenarien als JSON-Paare, mit [eigener Anleitung](verifikation/README.md). |

Das Teilnehmer-Material liegt eine Ebene höher: [`kata.md`](../kata.md)
und [`io-format.md`](../io-format.md), jeweils auch in englischer
Fassung.

## Warum ein eigener Branch

Dieses Verzeichnis existiert **nur im Branch `trainer`**. Der Branch
`main` enthält ausschließlich das Teilnehmer-Material — wer das Repo
klont oder als ZIP herunterlädt, bekommt die Auflösungen also nicht
versehentlich mit.

Als Trainer arbeitest du auf `trainer`:

```bash
git clone git@github.com:marcoemrich/magical_claim_office_kata.git
cd magical_claim_office_kata
git checkout trainer
```

Die Gruppe bleibt auf `main` — dort ist `trainer/` schlicht nicht
vorhanden.

**Beim Pflegen:** Änderungen an der Aufgabe gehören auf `main` und
werden von dort nach `trainer` gemerged, nicht umgekehrt:

```bash
git checkout trainer && git merge main
```

So bleibt `trainer` immer ein Vorlauf von `main` und die
Teilnehmer-Dateien driften nicht auseinander.
