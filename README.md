# MOCO Jira Connector

[English](README.en.md) · [Datenschutz / Privacy](PRIVACY.md) · [Sicherheit](SECURITY.md)

MOCO-Zeiterfassung direkt am Jira-Ticket – mit der richtigen Leistung auch beim **ersten** Start. Ein unabhängiger Fork von [MOCO Zeiterfassung](https://github.com/everii-Group/mocoapp-browser-extension), herausgegeben von [alpenblickdrei](https://www.alpenblickdrei.com/).

**Stand: 3.0.1 Veröffentlichungskandidat.** Der fertige Chrome-Build ist hier verfügbar; eine Chrome-Web-Store-Freigabe oder ein abgeschlossener Test mit echten Buchungen wird damit nicht behauptet. Ein abschließender installierter Chrome-Test und die Store-Prüfung stehen noch aus.

## Was macht dieser Fork anders?

Die offizielle Erweiterung unterstützt viele Anwendungen. Dieser Fork konzentriert sich auf Teams mit bereits verknüpften Jira- und MOCO-Daten. Der Vergleich bezieht sich auf den geprüften [Originalstand 2.19.0](https://github.com/everii-Group/mocoapp-browser-extension/tree/1254670424556d359ffe7748e35338ab18c8e23f).

| Ablauf                                  | Original, geprüfter Stand                                                                          | Dieser Fork                                                                                                         |
| --------------------------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Erstes Buchen auf einem neuen Jira-Task | Vorbelegung aus bisherigen Buchungen bzw. Projektkennung; weitere Rückfälle bis zum ersten Projekt | Explizite MOCO-IDs aus Jira haben Vorrang; ohne eindeutige Zuordnung manuelle Auswahl                               |
| Epic → Projekt / Task → Leistung        | Jira-Adapter liest keine solchen benutzerdefinierten ID-Felder                                     | Liest die Hierarchie und prüft die IDs gegen den zurückgegebenen MOCO-Katalog                                       |
| Subtask                                 | Keine Hierarchieauflösung im Jira-Adapter                                                          | Erbt die nächste Leistungszuordnung; eigene gültige Zuordnung möglich; eigener Jira-Schlüssel bleibt am Zeiteintrag |
| Einrichtung                             | Allgemeine Erweiterungsoptionen                                                                    | Einrichtung im Overlay; aktuelle Jira-Adresse und eindeutig benanntes numerisches MOCO-Feld automatisch erkennen    |
| Seitenzugriff                           | Allgemeine Integrationen                                                                           | Nur explizit freigegebene MOCO- und Jira-Adresse; dynamische Skriptregistrierung                                    |
| API-Schlüssel                           | Chrome-Sync-Speicher im geprüften Original                                                         | Lokaler Erweiterungsspeicher; kein Chrome Sync und kein Entwickler-Backend                                          |
| Fehler beim Start                       | Nicht-422-Fehler werden im geprüften Hintergrund-Handler nicht angezeigt                           | Zeitlimits und sichtbare Fehler; keine automatischen Buchungswiederholungen; doppelte Start-Klicks gesperrt         |
| Wechsel von Ticket oder Konto           | Kein entsprechender Abgleich vor dem Buchen im geprüften Handler                                   | Absende-Tab, aktuelle Ticket-Identität und Verbindungsversion werden geprüft                                        |

Der Fork synchronisiert keine Projekte, erstellt keine Jira-Tickets und ersetzt keine bestehende Jira↔MOCO-Verknüpfung. MOCO und Jira sind Marken ihrer jeweiligen Inhaber; dies ist keine offizielle Erweiterung dieser Anbieter.

## Die Zuordnung

```text
Jira Epic       → MOCO Projekt
  Jira Task     → MOCO Leistung (MOCO nennt sie task)
    Subtask     → dieselbe Leistung; eigener Ticketbezug im Zeiteintrag
```

Ein numerisches Jira-Feld enthält auf dem Epic die **Projekt-ID**, auf dem Task die **Leistungs-ID**. Beispiel mit erfundenen Daten: Epic `DEMO-1` → Projekt `1001`, Task `DEMO-2` → Leistung `2001`. Ein Subtask von `DEMO-2` bucht auf `2001`, behält jedoch seine eigene Ticketnummer und URL.

Es wird nur eine Leistung ausgewählt, die genau zum erkannten Projekt gehört und im Katalog des angemeldeten MOCO-Nutzers vorhanden ist. Fehlende Rechte, mehrdeutige IDs, fehlende Eltern oder unpassende Leistungen führen zur manuellen Auswahl. Die Erweiterung startet beim Öffnen **keine** Uhr.

## Einrichten

1. MOCO-Zugang mit persönlichem API-Schlüssel und Buchungsrechten sowie einen angemeldeten Jira-Cloud-Zugang bereithalten. Das Zuordnungsfeld muss bereits existieren und korrekt befüllt sein.
2. Das Plugin-ZIP herunterladen und entpacken, `chrome://extensions` öffnen, Entwicklermodus aktivieren und den enthaltenen Ordner mit `manifest.json` als entpackte Erweiterung laden. Im GitHub-Repository liegt derselbe Stand im Ordner `Chrome`. Die ursprüngliche MOCO-Erweiterung während des Tests deaktivieren, um doppelte Buttons zu vermeiden.
3. Ein Jira-Ticket öffnen und das Erweiterungssymbol anklicken. Die Einrichtung erscheint im Overlay. Adresse wird aus dem aktuellen Tab übernommen; bei genau einem numerischen Feld mit „MOCO“ im Namen wird auch dessen Feld-ID übernommen. Bei mehreren Feldern bitte auswählen, sonst die Feld-ID aus der Jira-Administration eintragen.
4. MOCO-Subdomain und **eigenen** MOCO-API-Schlüssel eingeben (Profil → Integrationen). Änderungen prüfen und speichern. Chrome fragt die beiden konkreten Domains ab. Es wird kein Jira-API-Schlüssel benötigt.
5. Bei schon geöffneten Tabs gegebenenfalls Jira neu laden. Task öffnen, Kunde/Projekt/Leistung im Dialog prüfen, dann ausdrücklich die Stoppuhr starten oder eine Dauer speichern.

Die Verbindung lässt sich über „Einstellungen“ im Zeitdialog ändern oder trennen. Falls ein Overlay nicht möglich ist, öffnet sich eine eigene Erweiterungsseite; du musst nicht durch Chromes Einstellungsmenüs navigieren. Eine Verbindung pro Chrome-Profil; jede Person verwendet ihren eigenen MOCO-Schlüssel.

## MOCO-Buchungsfreigabe und neue Mitarbeiter

Eine Jira-Zuweisung und eine korrekte MOCO-ID allein reichen nicht: Der Benutzer des persönlichen MOCO-Schlüssels muss im MOCO-Projekt für die Zeiterfassung eingetragen und aktiv sein. Auch Projekt und Leistung müssen buchbar sein. Das gilt ebenso für kurzfristige Sonderaufgaben aus Jira. Die Erweiterung legt keine Personen-Zuordnungen an und erweitert keine Rechte.

MOCO bietet dafür bereits Standardfunktionen. Dabei sind zwei Fälle zu unterscheiden:

- **Neues Projekt, vorhandene Mitarbeiter:** Unter MOCO **Einstellungen → Projekte** lässt sich festlegen, dass alle Personen bei der Projekterstellung hinzugefügt werden. Diese Voreinstellung muss die Agentur prüfen; sie wird vom Plugin nicht aktiviert.
- **Neuer Mitarbeiter, vorhandene Projekte:** Unter **Personal → Person → Projekte → +** können bestimmte oder alle bestehenden Projekte für die Zeiterfassung zugewiesen werden. Nicht davon ausgehen, dass die Einstellung für neue Projekte neue Mitarbeiter rückwirkend allen alten Projekten hinzufügt. Die Zuweisung gehört in die Onboarding-Prüfung.
- **Einzelne spontane Aufgabe:** Die Person zum betreffenden MOCO-Projekt für die Zeiterfassung hinzufügen lassen. Danach den Zeitdialog in Jira schließen und erneut öffnen, damit das Plugin den persönlichen Projektkatalog neu lädt. Die Zeit wird weiterhin auf die erkannte Leistung mit Jira-Ticketbezug gebucht.

Erscheint trotz korrekter Jira-IDs „Das Jira-Projekt ist in MOCO nicht eindeutig buchbar“, zuerst die persönliche Projektfreigabe, aktive Leistungen und den verwendeten MOCO-Benutzer prüfen. Die manuelle Auswahl kann ein von MOCO nicht freigegebenes Projekt ebenfalls nicht buchbar machen. Es wird nicht automatisch auf ein anderes Projekt ausgewichen.

[MOCO: Personen und Teams für die Zeiterfassung](https://www.mocoapp.com/funktionen/17-projekte/inhalt/22-projekte-allgemeines) · [MOCO: Warum sehe ich das Projekt nicht in der Zeiterfassung?](https://intercom.help/mocoapp/de/articles/6704447-zeiterfassung)

## Das AB3-Auge

- **Gelb:** mit MOCO verbunden, kein laufender Timer gemeldet.
- **Rot mit Aufnahmepunkt:** MOCO meldet einen laufenden Timer. Im Zeitdialog siehst du, worauf er läuft.
- **Grau:** nicht eingerichtet, getrennt oder MOCO gerade nicht erreichbar.

Das eigene AB3-Auge ersetzt das MOCO-Logo. Farbe, Aufnahmepunkt und Beschriftung machen den Status erkennbar. Auf sichtbaren Jira-Tabs wird der Status alle 30 Sekunden sowie beim Zurückkehren aktualisiert; Änderungen in anderen Fenstern können daher kurz verzögert erscheinen. Die Statusabfrage liest nur und startet oder stoppt keinen Timer.

## Grenzen und Fehlerfälle

- Automatik ist für Jira Cloud mit `parent`-Hierarchie und numerischem Zuordnungsfeld implementiert. Jira Server/Data Center, alte Epic-Link-Sonderfelder und mehrere parallele Agency-Verbindungen sind nicht zugesichert.
- Weiterleitungen werden über die tatsächlich geöffnete Zieladresse erkannt. Eigene HTTPS-Jira-Domains sind konfigurierbar; die REST-Erkennung bleibt davon abhängig, ob Atlassian die Aufrufe dort erlaubt. [Atlassian dokumentiert dazu Einschränkungen](https://jira.atlassian.com/browse/AX-667). Bei fehlender REST-Antwort manuell zuordnen; kein pauschales Kompatibilitätsversprechen.
- Ein anders benanntes oder verborgenes Feld kann nicht zuverlässig automatisch erkannt werden. Die Feld-ID bleibt deshalb korrigierbar.
- Persönliche Buchungsrechte und aktive Leistungen verwaltet MOCO. Ein vorhandener Timer wird angezeigt; Öffnen allein verändert ihn nicht.
- Ein verlorener Start-/Speicher-Response kann eine bereits erfolgreiche Buchung verbergen. Deshalb wird nicht automatisch wiederholt. Erst in MOCO nachsehen, dann den Dialog neu öffnen.
- Trennen entfernt lokale Zugangsdaten und Domain-Freigaben. Ein laufender Timer in MOCO wird dabei nicht gestoppt.
- Verwendet die vom Original genutzte MOCO-Browser-Extension-Schnittstelle `/api/browser_extensions`. Änderungen durch MOCO können Anpassungen erfordern.

## Dateien und Sicherheit

Dieses Repository enthält das fertige installierbare Plugin (`Chrome/` und `moco-jira-connector-3.0.1.zip`) sowie Anleitung, Datenschutz und Lizenz. Tests, Testdaten, Entwicklungsdateien und interne Prüfprotokolle werden hier nicht veröffentlicht. Der JavaScript-Code der Erweiterung ist im ausgelieferten Bundle enthalten. Das Plugin-ZIP enthält keine Tests oder Zugangsdaten.

Manifest V3, lokal gebündelter Code, kein Remote-Code. Der persönliche MOCO-Schlüssel wird erst bei der Einrichtung im eigenen Browser eingegeben. Sicherheitsprüfungen können bekannte Probleme finden, aber keine vollständige Schwachstellenfreiheit garantieren. Bitte keine Schlüssel oder Kundendaten in Issues posten. [Sicher melden](SECURITY.md).

MIT-Lizenz; ursprüngliche Copyright-Hinweise bleiben erhalten. Die öffentliche Lieferung enthält keine internen Datenexporte, Zugangsdaten oder private Entwicklungshistorie.
