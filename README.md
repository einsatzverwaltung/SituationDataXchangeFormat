# Lagekarte JSON
Lagekarte JSON ist eine Konvention, basierend auf dem GeoJSON Schema ([RFC 7946](https://datatracker.ietf.org/doc/html/rfc7946)) zum Austausch von georeferenzierten Lagekarten im Kontext der Behörden und Organisationen mit Sicherheitsaufgaben. Bei der Definition wurde auf eine größtmögliche Kompatibilität zu vorhandenen Standards geachtet und die Empfehlung der [DV 102](https://www.kritis.bund.de/SharedDocs/Downloads/BBK/DE/Publikationen/Broschueren_Flyer/Empfehlungen_Takt_Zeichen_im_BevSch.pdf?__blob=publicationFile) des Bundesamtes für Bevölkerungs- und Katastrophenschutz beachtet.
## Initatoren und Unterstützer
Initiiert wurde dieses Projekt von [REV Plus](https://www.einsatzverwaltung.de/) und [RescueTablet](https://rescuetablet.de/).
## Geometrische Elemente des geoJSON Standards
| Element | Beschreibung |
|--|--|
| Position| Basiselement mit Latitude und Longitude im WGS84 Koordinatensystem|
| Point| Einzelner Punkt, dargestellt durch eine Position  |
| LineString| Eine Linie, die aus mindestens zwei Punkte (Start- und Ende) aber auch aus beliebig vielen Zwischenpunkten, bestehen kann |
| Polygon| Ein Polygon ist eine in sich geschlossene Form und besteht aus mindestens drei Punkten, kann jedoch aus beliebig vielen Punkten bestehen |
### Position
Die Position wird aus einem Array mit mindestens zwei Koordinaten (Longitude und Latitude) beschrieben. Ein optionales drittes Element im Array beschreibt eine Höhe über N.N. in Metern.

    [50.1234, 8.1234, 200]
### Point (Punkt)
Ein Punkt beschreibt ein Element mit nur einer Position.

    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [50.1234, 8.1234, 200]
      }
    }
### Line String (Linie)
Ein LineString beschreibt eine beliebige Linie, die durch mindestens zwei Positionen repräsentiert wird.

    {
	    "type": "LineString", 
	    "coordinates": [
	        [30.0, 10.0], [10.0, 30.0], [40.0, 40.0]
	    ]
    }
### Polygon
Ein Polygon ist eine geschlossene Linie, die aus mindestens drei Positionen besteht.

    {
	    "type": "Polygon", 
	    "coordinates": [
	        [[30.0, 10.0], [40.0, 40.0], [20.0, 40.0], [10.0, 20.0], [30.0, 10.0]]
	    ]
    }
## Erweiterung der Standardobjekte
Die Standardelemente LineString und Polygon werden erweitert durch entsprechende Properties für die farbliche Gestaltung.
|Property| Beschreibung | Beispiel | Point | LineString | Polygon |
|--|--|--|--|--|--|
| stroke | HTML Farbe für den Strich | #FF00FF | Nein | Ja | Ja |
| stroke-width | Strichstärke in Pixeln | 2 | Nein | Ja | Ja |
| stroke-opacity | Strich-Deckkraft in Prozent (0.0 - 1.0) | 1 | Nein | Ja | Ja |
| fill | HTML Füllfarbe | #00FF00 | Nein | Nein | Ja |
| fill-opacity | Deckkraft der Füllfarbe in Prozent | 0.5 | Nein | Nein | Ja |


## Lagekartenelemente
Zur Darstellung der Lagekarte ist es notwendig unterschiedliche Elemente darstellen zu können. Diese basieren auf den obigen geometrischen Elementen des geoJSON Standards. Über die Property-Eigenschaft ist es möglich weitere Attribute zu einem geometrischen Element (Geometry) hinzuzufügen.
Beispielhaft wird dazu folgend die Definition für einen Punkt mit Metadaten (Attribut "prop0" hat den Wert "value0" dargestellt:

    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [102.0, 0.5]
      },
      "properties": {
        "prop0": "value0"
      }
    }
Folgende Lagekartenelemente sind in dieser Konvention definiert:
Element| Basiert auf | Beschreibung |
|--|--|--|
| Point of Interest | Point | Darstellung eines Bilds / Icon|
| Taktisches Zeichen | Point of Interest | Darstellung eines taktischen Zeichens |
| Gebiet | Polygon | Darstellung eines Gebietes |
### Point of Interest
Dieses Element ist für die grundsätzliche Darstellung eines Bildes ohne weitere fachliche Definition zu verwenden. Hiermit können Beispielsweise besondere Punkte markiert werden.

|Property| Beschreibung | M/C/O | Fester Wert |
|--|--|--|--|
| lage:type | Typ des Lageelementes | M | POI
| lage:name | Name des POI, wird auf Lagekarte dargestellt | O | 
| lage:description | Beschreibung des POI (Beispielsweise für Popup) | O |
| lage:url | URL zum Bild | M |
### Taktisches Zeichen
Dieses Element wird für die Darstellung von taktischen Zeichen verwendet. Durch die fachlichen Informationen kann das anzeigende System die taktischen Zeichen selbstständig generieren oder für die Erzeugung von Übersichten oder anderen Informationen verwenden.
Das Element basiert auf dem "Point of Interest". Die URL zu der Bilddatei ist dabei immer anzugeben.

|Property| Beschreibung | M/C/O | Fester Wert |
|--|--|--|--|
| lage:type | Typ des Lageelementes | M | TZ
| lage:tz:grundzeichen | Grundzeichen des taktischen Zeichens | M | 
| lage:tz:fachaufgabe | Fachaufgabe des taktischen Zeichens | M | 
| lage:tz:formation | Art der taktischen Formation, immer notwendig, wenn für das Grundzeichen die taktische Formation gewählt wurde | C |
| lage:tz:organisation | Organisation der Einheit | O |
| lage:tz:ordnung | Ordnung bzw. Stärke der Einheit | O |
| lage:tz:personalfunktion | Besondere Funktion einer Person, kann nur für das Grundzeichen Person angegeben werden | O |
| lage:tz:ortsfest | Gibt an, ob das taktische Zeichen Ortsfest ist | O | true/false

#### lage:tz:grundzeichen
Entspricht dem Punkt 1. Grundzeichen der DV102. Mögliche Werte:

 - Ohne
 - Taktische Formation
 - Befehlsstelle
 - Stelle
 - Person
 - Massnahme
 - Anlass
 - Gefahr
 - Logistik
 - Fuehrungseinheit
 - Logistik

#### lage:tz:formation
Entspricht dem Punkt 6. Zeichen zur Darstellung von Gegenständen der DV102. Mögliche Werte:

 - Einheit
 - Fahrzeug
 - Kraftfahrzeug
 - KraftfahrzeugMehrspurig
 - Wechselladerfahrzeug
 - Abrollbehaelter
 - Anhaenger
 - Schienenfahrzeug
 - Kettenfahrzeug
 - Kraftrad
 - Fahrrad
 - Raeumgeraet
 - Hebegeraet
 - Bagger
 - Bruecke
 - Wasserfahrzeug
 - Flugzeug
 - Hubschrauber
 - 
#### lage:tz:organisation
Entspricht dem Punkt 2. Farbgebung zur Darstellung von Organisationen und Einrichtungen der Gefahrenabwehr der DV102. Mögliche Werte:

 - Feuerwehr
 - THW
 - Hilfsorganisationen
 - Fuehrung
 - Polizei
 - Sonstige
 - Bundeswehr
#### lage:tz:ordnung
Entspricht dem Punkt 4. Zeichen zur Darstellung von Größenordnungen, hierarchischen Zuordnungen und Ordnungsprinzipien der DV102. Mögliche Werte:

 - Trupp
 - Staffel
 - Gruppe
 - Zug
 - Bereitschaft
 - Abteilung
 - Großverband
 - Gemeinde
 - Kreis
 - Bezirk
 - Land
 - BRD
 - EU
#### lage:tz:personalfunktion
Entspricht dem Punkt 5. Zeichen zur Darstellung von Personen mit besonderen Funktionen der DV102. Mögliche Werte:

 - Keine
 - Fuehrungskraft
 - Sonderfunktion

#### lage:tz:fachaufgabe
Entspricht dem Punkt 3. Zeichen zur Darstellung von Fachaufgaben der Gefahrenabwehr der DV102. Mögliche Werte:

 - Brandbekaempfung 
 - RettenAusHoehenUndTiefen 
 - WasserversorgungUndFoerderung 
 - TechnischeHilfe 
 - Heben 
- Bergen 
 - Raeumen 
 - Entschaerfung 
- Sprengen 
 - Transport 
 - Beleuchtung 
 - EinsatzVonLuftfahrzeugen 
 - EinsatzVonWasserfahrzeugen
 - SuchenUndOrtenMitHunden 
 - Wasserrettung 
 - Pumpen
 - AbwehrVonWassergefahren 
 - GSG 
- Messen 
 - Dekontamination 
 - UmweltschadenGewaesser 
 - Betreuung 
- Rettungswesen 
 - Arzt 
 - Erkundung 
 - Deichverteidigung 
 - FuehrungsgruppeTEL 
- Brueckenbau 
 - RettenAusHoehen 
 - RettenAusTiefen 
 - Schlachten 
 - Warnen 
- Angeschlagen 
 - PersonBetroffen 
 - PersonGerettet 
 - PersonTot 
 - PersonVerletzt
- PersonVermisst 
 - PersonVerschuettet 
 - Blockiert
 - ElektrischeEnergie 
 - BrandEntstehung 
- BrandFortentwickelt 
 - BrandVollbrand 
 - Verpflegung
 - Teilblockiert 
 - Zerstört 
- Teilzerstört 
 - Seelsorge 
 - Sammelstelle 
 - Unterkunft 
 - EinsatzVonHubschraubern 
 - EinsatzVonFlugzeugen 
 - Veterinaerwesen 
 - SammelplatzFürBetroffene 
 - IuK 
 - Patientenablage 
 - PatientenablageArzt
 - HalteplatzVerletzte 
 - Sirene 
 - Trinkwasser 
 - Brauchwasser 
 - VersorgungBetriebsstoffe 
 - Geraete 