
# Situation Data Xchange Format (SDXF)
Das Situation Data Xchange Format ist eine Formatdefinition zum Austausch von Lageinformationen zwischen unterschiedlichen Softwaresystemen im Kontext der behördlichen Gefahrenabwehr.

## Verfügbare Spezifikationen
Da nicht alle Softwaresysteme im Umfeld der Behörden und Organisationen mit Sicherheitsaufgaben den gleichen Umfang an Daten abdecken, wurde das SDXF modularisiert aufgebaut.

| Bezeichnung | Name | Beschreibung | JSON Schema |
|--|--|--|--|
| Grundinformationen | metadata | Die Metadaten zur Lage (Muss von allen Systemen unterstützt werden) | [schema/metadata.schema.json]
| Einsätze | incident | Einsatzstellen innerhalb der Lage | [schema/incident.schema.json]
| Einsatzmittel | unit | Einsatzmittel, die innerhalb der Lage eingesetzt werden | [schema/unit.schema.json]
| MANV Patienten | patient | Patienten, die innerhalb der Lage erfasst werden | [schema/patient.schema.json]
| Einsatzabschnitte | section | Einsatzabschnitte für die Gliederung der Einsatz- und Führungsstruktur | [schema/section.schema.json]
| Einsatztagebuch | journal | Einträge im Einsatztagebuch | [schema/journal.schema.json]