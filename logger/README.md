# logger

Schlichtes, level-basiertes Logging-Modul für [Pipe](https://pipe-lang.com).

## Eigenschaften

- Fünf einfache Level-Funktionen: `debug`, `info`, `warn`, `error` und `file`.
- Konsolenausgabe mit Zeitstempel-Prefix im Format `[LEVEL] YYYY-MM-DD HH:MM:SS`.
- `file()` hängt Zeilen an eine beliebige Logdatei an.
- Keine externen Abhängigkeiten.

## Installation

```sh
pipe install logger
```

## Verwendung

```pipe
import logger as log

log.info("Pipe gestartet")
log.debug("Lade Konfiguration")
log.warn("Veraltetes Format erkannt")
log.error("Kritischer Fehler")

# In Datei schreiben
log.file("/tmp/app.log", "Startup abgeschlossen")
```

## API

| Funktion | Beschreibung |
|----------|--------------|
| `debug(msg)` | Gibt eine DEBUG-Meldung auf der Konsole aus. |
| `info(msg)` | Gibt eine INFO-Meldung auf der Konsole aus. |
| `warn(msg)` | Gibt eine WARN-Meldung auf der Konsole aus. |
| `error(msg)` | Gibt eine ERROR-Meldung auf der Konsole aus. |
| `file(path, msg)` | Hängt eine formatierte Zeile an `path` an. |

## Lizenz

MIT
