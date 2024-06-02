
# Situation Data Xchange Format (SDXF)
Das Situation Data Xchange Format ist eine Formatdefinition zum Austausch von Lageinformationen zwischen unterschiedlichen Softwaresystemen im Kontext der behördlichen Gefahrenabwehr.

## Verfügbare Spezifikationen
Da nicht alle Softwaresysteme im Umfeld der Behörden und Organisationen mit Sicherheitsaufgaben den gleichen Umfang an Daten abdecken, wurde das SDXF modularisiert aufgebaut.

| Bezeichnung | Name | Beschreibung | JSON Schema |
|--|--|--|--|
| Grundinformationen | metadata | Die Metadaten zur Lage (Muss von allen Systemen unterstützt werden) | [metadata.schema.json](schema/metadata.schema.json)
| Einsätze | incident | Einsatzstellen innerhalb der Lage | [incident.schema.json](schema/incident.schema.json)
| Einsatzmittel | unit | Einsatzmittel, die innerhalb der Lage eingesetzt werden | [unit.schema.json](schema/unit.schema.json)
| MANV Patienten | patient | Patienten, die innerhalb der Lage erfasst werden | [patient.schema.json](schema/patient.schema.json)
| Einsatzabschnitte | section | Einsatzabschnitte für die Gliederung der Einsatz- und Führungsstruktur | [section.schema.json](schema/section.schema.json)
| Einsatztagebuch | journal | Einträge im Einsatztagebuch | [journal.schema.json](schema/journal.schema.json)
| Binärdateien | file | Dateien wie Luftbilder, Tonaufnahmen, Videoaufnahmen, Dokumente, etc. | [file.schema.json](schema/file.schema.json)
| Lagekarte | [situationMapJSON](SituationMapJson.md) | Georeferenzierte Lagekarten auf Basis von GeoJSON | [situationMapJSON](SituationMapJson.md)

## Aufbau der Austauschdatei
Für den Austausch der Daten müssen das Quell- sowie das Zielsystem das SDX-Format unterstützen. Der Offline-Austausch erfolgt in Form einer ZIP-Datei, die nach dem folgenden Format aufgebaut ist. Sollte ein Quellsystem, nicht alle Spezifikationen unterstützen, so sind die jeweiligen Dateien nicht Teil der ZIP-Datei. Die metadata.json ist verpflichtend immer Teil der SDXF Datei.

    ZIP-Datei (Endung *.sdxf)
    |-- metadata.json
    |-- incident.json
    |-- unit.json
    |-- patient.json
    |-- section.json
    |-- journal.json
    |-- file.json
    └-- Files
          |-- folderA
          |   |-- 2a8047b7-47bf-4592-aa1d-33c9ea520173
          |   |-- 5f6feefc-f2ff-4d16-a4c5-db7dc4f9d7c4
          |   └-- e8c32402-b2b8-4803-a5f8-cc6cf1716203
          └-- folderB
              |-- 15edc255-b859-44ea-9166-215fbb660e1c
              |-- f9e7f9d0-ad29-4b3b-8ce8-7546faa7ba5d
              └-- 1a939c25-6568-4ef2-bef8-475dc43a2290

### Prüfen der Austauschdatei
Zielsysteme müssen die SDXF Datei vor dem Importieren prüfen. Dabei sollten folgende Prüfungen durchgeführt werden:

 * Hat die Datei die Endung *.sdxf?
 * Kann die Datei mittels einer Zip-Funktion entpackt werden?
 * Enthält das ZIP-Archiv mindestens eine metadata.json?
 * Entspricht die metadata.json dem zugehörigen JSON Schema?
 * Sind weitere Dateien auf oberster Ebene im ZIP Archiv enthalten?
 * Entsprechen alle Dateien auf oberster Ebene dem jeweiligen JSON Schema?

Das Zielsystem kann entscheiden, ob es die Datei bei fehlgeschlagener Prüfung trotzdem importiert.

# Datenaustausch

Die Formatdefinition SDXF beschreibt nur den Aufbau der Daten, die für einen Austausch benötigt werden. Für die Übertragung der Daten werden keine Vorgaben gemacht. Alle Anwendungen müssen mindestens die Möglichkeit eines Offline-Datenaustausches, also ein Dateibasierten Ex- und Import der Daten bereitstellen.

Optional wäre auch ein Online-Datenaustausch möglich. Dafür ist jedoch noch eine Protokollbeschreibung notwendig.