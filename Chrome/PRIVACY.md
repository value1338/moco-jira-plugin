# Datenschutz / Privacy — MOCO Jira Connector

Stand / Updated: 23 October 2026. Version: 3.0.x.

## Deutsch

Herausgeber: **Alpenblickdrei Werbeagentur GmbH**, Lindauer Str. 2, 88069 Tettnang, Deutschland. Kontakt: **servus@alpenblickdrei.com**. [Impressum](https://www.alpenblickdrei.com/impressum/).

Die Erweiterung verbindet den Jira-Ticketkontext mit der persönlichen MOCO-Zeiterfassung. Der Herausgeber betreibt dafür keinen Datenserver, sammelt keine Nutzungsstatistiken und erhält keine API-Schlüssel oder Ticketdaten.

**Lokal gespeichert:** MOCO-Subdomain, persönlicher API-Schlüssel, Jira-Adresse, Jira-Feld-ID, Verbindungsversion und Anzeigeeinstellung. Für die Einrichtung wird außerdem die erkannte Jira-Adresse mit einer Liste passender Feldnamen/-IDs zwischengespeichert. Dies liegt im lokalen Erweiterungsspeicher des Chrome-Profils; kein Chrome Sync. Der Schlüssel ist durch den Browserprofilzugriff geschützt, nicht durch eine zusätzliche Hardwareverschlüsselung der Erweiterung.

**Jira:** Nach einem Klick zur Einrichtung werden Feldnamen/-IDs gelesen. Im Zeitdialog werden aktueller Ticketschlüssel, Titel, URL, MOCO-Zuordnungsfeld und Elternhierarchie gelesen. Dafür wird die vorhandene Jira-Anmeldung im selben Ursprung genutzt. Es gibt keine Jira-Schreibzugriffe, keine Jira-Token-Speicherung und kein Auslesen der allgemeinen Browserhistorie.

**MOCO:** Der persönliche Schlüssel wird nur an die eingerichtete HTTPS-MOCO-Subdomain gesendet. Die Erweiterung meldet sich über die Schnittstelle des ursprünglichen MOCO-Plugins an und liest persönliche buchbare Projekte/Leistungen, Zeiteinträge der angezeigten Woche, Planung/Sollzeiten, Anzeigevorgaben und laufenden Timer. Beim Öffnen kann die Sitzung auch den aktuellen Jira-Schlüssel an MOCO übermitteln, um dessen bestehende Buchungen zu ermitteln. Eine ausdrückliche Start-/Speicheraktion übermittelt Projekt, Leistung, Datum, Dauer bzw. Timerstart, Verrechenbarkeit, Beschreibung und Jira-Ticketbezug. Stoppen wird nur nach einem ausdrücklichen Klick angefordert. Inhalte verbleiben in euren eigenen MOCO-/Jira-Systemen und unterliegen deren Bedingungen und euren betrieblichen Regeln.

**Berechtigungen:** `storage` für lokale Konfiguration, `activeTab` für die Einrichtung im bewusst angeklickten Tab und `scripting` für den Jira-Button/das Overlay. Die optionale HTTPS-Deklaration erlaubt kundeneigene Jira-Domains; beim Speichern werden nur die zwei angezeigten konkreten Domains angefordert. Keine Werbung, kein Verkauf von Daten, kein Remote-Code, keine Telemetrie. Externe Anleitung-/Support-Links werden nur durch Nutzerklick geöffnet.

**Entfernen:** „Verbindung trennen“ löscht lokale Verbindungsdaten, Einrichtungsdaten und optionale Domain-Freigaben. Deinstallation entfernt den Erweiterungsspeicher im betreffenden Profil. Bereits in MOCO gespeicherte Zeiten oder laufende Timer werden dadurch nicht entfernt/gestoppt. Diese verwaltet ihr in MOCO. Daten, die ihr freiwillig in Support-Mails oder GitHub-Issues teilt, erhält der jeweilige Empfänger; bitte keine Schlüssel oder vertraulichen Daten beifügen.

## English

Publisher: **Alpenblickdrei Werbeagentur GmbH**, Lindauer Str. 2, 88069 Tettnang, Germany. Contact: **servus@alpenblickdrei.com**. [Legal notice](https://www.alpenblickdrei.com/impressum/).

The extension connects Jira ticket context to personal MOCO time tracking. The publisher operates no data backend for it, collects no usage analytics, and receives no API keys or ticket data.

**Local storage:** MOCO subdomain, personal API key, Jira origin, mapping field ID, connection revision and display preference. A setup suggestion temporarily stores the detected Jira origin and matching field names/IDs. These remain in the current Chrome profile's extension-local storage, never Chrome Sync. Storage relies on browser profile protection; the extension does not add hardware-backed key encryption.

**Jira:** Setup reads field metadata after an explicit user click. The time dialog reads the current issue key/title/URL, mapping field and parent hierarchy using the existing same-origin Jira session. It does not write to Jira, store a Jira API token, or read general browsing history.

**MOCO:** The key is sent only to the configured HTTPS MOCO subdomain. The extension uses upstream's browser-extension interface to authenticate and read personal bookable projects/services, displayed-week time entries, schedules/target hours, display preferences and running timer. Opening a dialog may send the current Jira key to MOCO to retrieve previous bookings for that issue. An explicit start/save action sends project, service, date, duration or timer start, billable flag, description and Jira ticket reference. Stopping also requires a user click. Data resides in your own MOCO/Jira systems under their terms and your organization's rules.

**Permissions:** `storage` saves local settings; `activeTab` supports setup in the deliberately clicked tab; `scripting` adds the Jira control and extension-origin overlay. Optional HTTPS patterns support custom Jira domains, but Save requests only the two concrete displayed origins. No advertisements, sale of data, remote code or telemetry. Documentation/support links open only on user clicks.

**Removal:** Disconnect deletes local connection/setup data and optional host grants. Uninstall removes extension storage in that profile. Existing MOCO time entries and timers remain unchanged; manage them in MOCO. Information voluntarily shared in support emails or GitHub Issues reaches those recipients; do not include credentials or confidential data.
