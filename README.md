# DokuMark

**Fotos, markierte Bereiche und Notizen zu einem Projekt zusammenführen – direkt im Browser.**

DokuMark unterstützt die Dokumentation von Restaurierungsarbeiten, Baustellen und anderen Arbeiten am Objekt. Ein Übersichtsbild dient als Orientierung: Nummerierte Punkte verbinden eine Stelle im Bild mit ihren Fotos und Notizen. Aus diesen Informationen lässt sich ein PDF-Bericht erstellen.

Die Anwendung heißt inzwischen **DokuMark**; das GitHub-Repository trägt weiterhin den ursprünglichen Namen **RestauroMap**. Die aktuelle Startdatei ist [`DokuMark.html`](DokuMark.html).

## Funktionen

- Projekte mit Name, Ort und Übersichtsbild anlegen.
- Bereiche durch einen einzelnen, gut sichtbaren Punkt markieren. Die Punkte werden mit **1, 2, 3 …** nummeriert.
- Einen Punkt antippen, um die zugehörigen Fotos und Notizen zu öffnen.
- Fotos aus Kamera oder Galerie hinzufügen, Notizen bearbeiten und Fotos annotieren.
- Annotationen standardmäßig als **Kopie** speichern. Das Überschreiben des gespeicherten Fotos muss bewusst gewählt werden.
- PDF-Berichte mit Projektübersicht, nummerierten Bereichen, Fotos, Notizen, Firmenangaben und Seitenzahlen erzeugen.
- Einzelne Projekte oder den gesamten Bestand als JSON sichern und wieder importieren.
- Projekte nach einer geprüften Sicherung archivieren und später wiederherstellen.
- Auf dem Handy kompakte Kopfzeilen und aufklappbare Werkzeuge nutzen; zwischen heller und dunkler Darstellung wechseln.

Bestehende Flächenmarkierungen aus älteren Versionen bleiben erhalten. Neue Markierungen benötigen kein Zeichnen oder Schließen eines Umrisses.

## Starten

1. Dieses Repository über **Code → Download ZIP** herunterladen und entpacken oder mit Git klonen.
2. `DokuMark.html` im Browser öffnen. Für die PWA die mitgelieferten Dateien und ihre Ordnerstruktur beibehalten. Die PDF-Bibliothek ist direkt in der HTML-Datei enthalten; der PDF-Export einer lokalen Datei benötigt keine Internetverbindung.
3. Ein Projekt anlegen und ein Übersichtsbild auswählen.

Ein Build-Schritt, ein Benutzerkonto und ein Anwendungsserver sind für die grundlegende Nutzung nicht erforderlich. Benötigt wird ein Browser mit Unterstützung für IndexedDB, Canvas und Pointer Events.

Für die Nutzung als installierbare **Progressive Web App (PWA)** die Dateien über HTTPS oder einen lokalen Webserver auf `localhost` bereitstellen. Nach dem erfolgreichen ersten Laden kann der Service Worker die Anwendung und die PDF-Bibliothek für den Offlinebetrieb vorhalten. Die Installation und einzelne Gerätefunktionen hängen vom Browser ab.

## Ein Projekt dokumentieren

1. In der Projektübersicht über **+** ein Projekt mit Name, Ort und Übersichtsbild erstellen.
2. Im Projekt **+ Punkt** wählen und die gewünschte Stelle im Bild einmal antippen.
3. Den Bereich benennen, seine Wichtigkeit wählen und speichern. Der nummerierte Punkt bleibt im Übersichtsbild sichtbar.
4. Den Punkt öffnen und über **Kamera**, **Galerie** oder **Notiz** Einträge hinzufügen.
5. Weitere Bereiche auf dieselbe Weise markieren.
6. Über das Werkzeugmenü **⋯** den PDF-Bericht erzeugen, die Bereichsliste öffnen oder die Bildansicht anpassen.

Die Nummerierung in der Bildübersicht entspricht den Bereichsseiten im PDF. Lange Notizen werden auf mehrere Seiten verteilt. Firmenangaben und ein Logo können für die Berichte hinterlegt werden.

## Speicherung und Sicherungen

### Hauptspeicher: Browser

Projekte werden lokal in **IndexedDB** gespeichert. Die Projektübersicht lädt nur Metadaten; Bilder werden getrennt gespeichert und bei Bedarf geladen. DokuMark hat keine integrierte Cloud-Synchronisierung und keinen gemeinsamen Datenbestand für mehrere Geräte.

Der Speicherstatus ist sichtbar. In den Projekt- und Detailansichten lassen sich die vollständigen Angaben aufklappen. Daten gehören zum jeweiligen Browserprofil und zur Startadresse der Anwendung. Ein anderer Browser, eine andere Adresse oder das Löschen der Browserdaten kann daher einen anderen beziehungsweise leeren Bestand zeigen. Für den Gerätewechsel JSON-Sicherungen verwenden.

### Projektdateien und Gesamtbackups

| Datei | Inhalt | Verwendung |
| --- | --- | --- |
| Projektexport | Ein Projekt mit Übersichtsbild, Bereichen, Fotos, Notizen und enthaltenen Firmenangaben | Ein Projekt übertragen oder sichern |
| Gesamtbackup | Alle Projekte einschließlich archivierter Projekte sowie zugehörige Daten und Firmenangaben | Den gesamten Bestand sichern |
| PDF-Bericht | Lesbare Dokumentation mit Bildern und Texten | Dokumentation weitergeben; kein rückimportierbares Datenbackup |

Der Import prüft die Daten und legt Projekte mit neuen IDs an. Bestehende Projekte werden nicht überschrieben. Vorhandene Firmenangaben werden beim Projektimport nicht durch importierte Angaben ersetzt. Auch die unterstützten älteren RestauroMap-JSON-Dateien können importiert werden. Ein erneuter Import derselben Datei erzeugt weitere Projektkopien.

### Optionaler Geräteordner

Über **Speicher & Archiv → Geräteordner wählen** kann ausdrücklich ein Ordner für Sicherungen ausgewählt werden, sofern der Browser die benötigte Dateisystem-API unterstützt. **Der Browser bleibt der Hauptspeicher**; der Ordner enthält Sicherungsdateien.

Änderungen werden gebündelt und nach einem Fünf-Minuten-Intervall gesichert. **Ordner sichern** startet eine Sicherung unmittelbar. Nach erfolgreichem Schreiben und Rücklesen einer neuen Sicherung wird die eigene Gesamtbackup-Historie auf fünf Stände begrenzt. Archivdateien und fremde Dateien werden dabei nicht bereinigt. Bei Schreib- oder Prüfproblemen bleiben vorherige Sicherungen erhalten; der Status zeigt den Fehler an. Bei einem Bereinigungsfehler kann die Historie vorübergehend mehr als fünf Stände enthalten.

Ohne Ordnerunterstützung stehen JSON-Download und Import weiterhin zur Verfügung. Ausstehende automatische Sicherungen benötigen eine laufende Anwendung; vor dem Schließen oder Gerätewechsel die Sicherung abschließen.

### Archivierung

Vor der Archivierung wird die Projektdatei mit derselben Datenprüfung wie beim Import geprüft. Die externe Sicherung muss erfolgreich geschrieben und zurückgelesen werden. Beim Download-Fallback muss die heruntergeladene Datei anschließend erneut ausgewählt und geprüft werden.

**Archivierte Projekte bleiben mit ihren Bildern und Einträgen im Browser gespeichert. Archivieren gibt keinen Speicherplatz frei.** Über **Archiv anzeigen** lassen sie sich wieder einblenden und wiederherstellen.

## Grenzen und Kompatibilität

- Bilder werden beim Hinzufügen komprimiert. Für die unveränderten Kamera- oder Galerieoriginale weiterhin eine eigene Aufbewahrung vorsehen.
- Ungültige Punkte und Flächen werden vor dem Speichern oder Import abgewiesen. Problematische Altbereiche bleiben erhalten und bieten in der Bereichsliste **Punkt neu setzen** als gezielte Reparatur an; ihre Fotos und Notizen bleiben bestehen.
- Für den Import gilt eine Dateigrenze von **256 MiB** sowie weitere Schutzgrenzen für Datensätze, Texte, Bilder und Geometrien. Der verfügbare Browserspeicher und Arbeitsspeicher können die praktische Größe zusätzlich begrenzen.
- Kamera, Datei-Freigabe, Geräteordner und PWA-Installation sind browserabhängig.
- PDF-Berichte werden im ausdrücklich gewählten Geräteordner gespeichert und zurückgelesen. Ohne Ordner oder bei einem Schreibfehler wird der Bericht zum Herunterladen oder Teilen angeboten. Welche Speicherziele verfügbar sind, bestimmt der Browser.
- Eine heruntergeladene HTML-Datei aktualisiert sich nicht automatisch. Vor einem Versions- oder Browserwechsel den Bestand als JSON sichern und anschließend die aktuelle Startdatei verwenden.

## Technischer Aufbau

Die Anwendung verwendet HTML, CSS und JavaScript ohne Framework. IndexedDB verwaltet die lokalen Daten, Canvas zeichnet Bildansicht und Markierungen, und [jsPDF](https://github.com/parallax/jsPDF) erzeugt die Berichte.

| Datei / Ordner | Aufgabe |
| --- | --- |
| `DokuMark.html` | Aktuelle Anwendung mit Oberfläche, Datenverwaltung und Exportfunktionen |
| `jspdf.umd.min.js` | Mitgelieferte PDF-Bibliothek; zusätzlich direkt in der HTML-Datei eingebunden |
| `manifest.json` | Name, Startadresse und Darstellung der PWA |
| `sw.js` | Offline-Cache der Anwendung |
| `icons/` | PWA-Symbole |
| `RestauroMap_v9.6_fixed.html` | Historische Version; nicht die aktuelle Startdatei |

Die bisherige interne Datenbankkennung wird aus Kompatibilitätsgründen beibehalten. Die Umbenennung zu DokuMark soll vorhandene Daten weiter erreichbar halten.
