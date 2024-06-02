
# Situation Map JSON v0.2
Situation Map JSON ist eine Konvention, basierend auf dem GeoJSON Schema ([RFC 7946](https://datatracker.ietf.org/doc/html/rfc7946)) zum Austausch von georeferenzierten Lagekarten im Kontext der Behörden und Organisationen mit Sicherheitsaufgaben. Bei der Definition wurde auf eine größtmögliche Kompatibilität zu vorhandenen Standards geachtet und die Empfehlung der [DV 102](https://www.kritis.bund.de/SharedDocs/Downloads/BBK/DE/Publikationen/Broschueren_Flyer/Empfehlungen_Takt_Zeichen_im_BevSch.pdf?__blob=publicationFile) des Bundesamtes für Bevölkerungs- und Katastrophenschutz beachtet.
## Initatoren und Unterstützer
Initiiert wurde dieses Projekt von [REV Plus](https://www.einsatzverwaltung.de/) und [RescueTablet](https://rescuetablet.de/).
## Geometrische Elemente des geoJSON Standards (RFC7946)
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

### Circle / Kreis
Die Standardelemente bilden leider nicht alle Möglichkeiten ab. Was aktuell noch fehlt, sind Kreise. Als Konvention wird derzeit von den meisten Client-Bibliotheken ein Point mit Radius verwendet.
Sobald ein Point mit Radius versehen ist, soll dieser als Circle gerendert werden. Der Radius wird in Metern angegeben.
Beispiel:

    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [102.0, 0.5]
      },
      "properties": {
        "radius": 100
      }
    }

### Styling Properties
Die Standardelemente werden durch entsprechende Properties für die farbliche Gestaltung erweitert. Dies gilt für fast alle Elemente der geoJSON Spezifikation. Die meisten dieser Properties werden auch durch geoJSON Viewer unterstützt. Somit wäre ein Anzeigen der farbigen Elemente auch in anderen Programmen möglich.
|Property| Beschreibung | Beispiel | Point | LineString | Polygon |
|--|--|--|--|--|--|
| stroke | HTML Farbe für den Strich | #FF00FF | Nein | Ja | Ja |
| stroke-width | Strichstärke in Pixeln | 2 | Nein | Ja | Ja |
| stroke-opacity | Strich-Deckkraft in Prozent (0.0 - 1.0) | 1 | Nein | Ja | Ja |
| fill | HTML Füllfarbe | #00FF00 | Nein | Nein | Ja |
| fill-opacity | Deckkraft der Füllfarbe in Prozent | 0.5 | Nein | Nein | Ja |

Hier ein Beispiel eines farbigen Rechtecks:

    {
      "type": "Feature",
      "properties": {
        "stroke": "#ff0000",
        "stroke-width": 2,
        "stroke-opacity": 1,
        "fill": "#555555",
        "fill-opacity": 0.5
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [
              8.908538818359375,
              50.20986732327653
            ],
            [
              8.97857666015625,
              50.20986732327653
            ],
            [
              8.97857666015625,
              50.22744158978946
            ],
            [
              8.908538818359375,
              50.22744158978946
            ],
            [
              8.908538818359375,
              50.20986732327653
            ]
          ]
        ]
      }
    }

## Übersicht über alle Attribute

Attribut| [Point](#point-punkt) | [LineString](#line-string-linie) | [Polygon](#polygon) | [Circle](#circle--kreis) | [TZ](#Taktisches-Zeichen) | [Patient](#massenanfall-von-verletzten-manv) | 
|--|--|--|--|--|--|--|
| Styling Properties |
| [stroke](#styling-properties)          | - | O | O | O | - | - |
| [stroke-width](#styling-properties)    | - | O | O | O | - | - |
| [stroke-opacity](#styling-properties)  | - | O | O | O | - | - |
| [fill](#styling-properties)            | - | - | O | O | - | - |
| [fill-opacity](#styling-properties)    | - | - | O | O | - | - |
| [radius](#circle--kreis)         | - | - | - | M | - | - |
| Allgemeine Eigenschaften |
| [id](#Erstellungszeitpunkt-created)   | M | M | M | M | M | M | M |
| [created](#Erstellungszeitpunkt-created)   | M | M | M | M | M | M |
| [author](#Autor-lage-author)   | O | O | O | O | O | O |
| [modified](#Änderungszeitpunkt-lage-bearbeitet)   | O | O | O | O | O | O |
| [name](#lage:name)   | O | O | O | O | O |
| [description](#beschreibung-description)   | O | O | O | O | O |
| [imageUrl](#)   | M | - | - | - | O |
| [situation:type](#arten-von-lagekartenelementen-situationtype)        | O | O | O | O | M |- |
| Taktische Zeichen |
| [situation:ts:grundzeichen](#lage:tz:grundzeichen)  | - | - | - | - | C |- |
| [situation:ts:fachaufgabe](#lage:tz:fachaufgabe)   | - | - | - | - | M |- |
| [situation:ts:formation](#lage:tz:formation)     | - | - | - | - | C |- |
| [situation:ts:organisation](#lage:tz:organisation)  | - | - | - | - | O |- |
| [situation:ts:ordnung](#lage:tz:ordnung)       | - | - | - | - | O |- |
| [situation:ts:personalfunktion](#lage:tz:personalfunktion) | - | - | - | - | O |- |
| [situation:ts:ortsfest](#lage:tz:ortsfest)      | - | - | - | - | O |- |
| [situation:ts:text](#lage:tz:text)      | - | - | - | - | C |- |
| Bereiche / Zonen |
| [situation:area:art](#situation:area:art)         | - | - | O | O | - |- |
| [situation:area:danger](#situation:area:danger)   | - | - | O | O | - |- |
| MANV / Patienten |
| [situation:patient:triage](#sichtungskategorie-situationpatienttriage)         | - | - | - | - | - | X |
| [situation:patient:triage-time](#zeitpunkt-der-sichtung-situationpatienttriage-time)         | - | - | - | - | - | O |


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

### Arten von Lagekartenelementen (situation:type)
Folgende Lagekartenelemente sind in dieser Konvention definiert:
Element| Basiert auf | Beschreibung |
|--|--|--|
| Point of Interest | Point | Darstellung eines Bilds / Icon|
| Tactical Symbol | Point of Interest | Darstellung eines taktischen Zeichens |
| Area | Polygon | Darstellung eines Gebietes |
| Patient | Point | Darstellung eines MANV Patienten

Properties können Mandatory, Optional oder Conditional sein. Mandatory-Properties müssen immer gesetzt sein. Optional-Properties sind optional und können bei Bedarf gesetzt werden.
Conditional-Properties sind in bestimmten Fällen Mandatory. Es hängt also von anderen Properties ab, ob die Conditional-Property übermittelt werden muss. 

### Allgemeine Attribute

#### Eindeutige ID (id)

Jedes Lagekartenelement sollte mit einer eindeutigen ID gekennzeichnet sein, damit den verarbeitenden Systemen ein Aktualisieren oder Löschen von Elementen möglich gemacht werden kann.

#### Name (name)

Diese Eigenschaft gibt dem Objekt einen Namen. Dieser Name bezeichnet das Objekt und sollte immer in der Lagekarte dargestellt werden.

#### Beschreibung (description)

Diese Eigenschaft beschreibt das Objekt und sollte in der Lagekarte z.B. als Tooltip dargestellt werden.

#### Erstellungszeitpunkt (created)

Zeitpunkt der Erstellung des Elements.

#### Änderungszeitpunkt (modified)

Zeitpunkt der letzten Änderung an diesem Element.

#### Autor (author)

Der Autor sollte der Name, der Rufname oder die Funktion einer Person sein, die dieses Lagekartenelement erstellt hat.

#### Bilddatei (imageUrl)

URL für das anzuzeigende Bild. Das Bild kann als Fallback-Bild verwendet werden, wenn das Zielsystem die Darstellung nicht selbst rendern kann (z.B. das taktische Zeichen) oder wenn das Bild der darzustellende Haupteinhalt ist. Es sind alle gütligen URI Formate möglich. Es wird empfohlen Ressourcen direkt als Daten-URL in das GeoJSON einzubetten oder per lokaler File-URL für mitgelieferte Dateien zu verknüpfen, damit auch Systeme ohne Internetverbindung die Lagekarte darstellen können.

### Point of Interest
Dieses Element ist für die grundsätzliche Darstellung eines Bildes oder Symbols ohne weitere fachliche Definition zu verwenden. Hiermit können Beispielsweise besondere Punkte markiert werden.

|Property| Beschreibung | M/C/O | Fester Wert | Beispiel Wert |
|--|--|--|--|--|
| situation:type | Typ des Lageelementes | M | poi
| name | Name des POI, wird auf Lagekarte dargestellt | O | | Toiletten
| description | Beschreibung des POI (Beispielsweise für Popup) | O | | Toiletten mit Handwaschmöglichkeiten
| imageUrl | URL zum Bild (Kann auch Data-URL sein) | M | | https://cdn-icons-png.flaticon.com/512/185/185547.png

Beispiel:

    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [102.0, 0.5]
      },
      "properties": {
        "name" : "Toiletten",
        "description" : "Toiletten mit Handwaschmöglichkeiten",
        "imageUrl": "https://cdn-icons-png.flaticon.com/512/185/185547.png",
        "situation:typ": "poi"                
      }
    }

### Taktisches Zeichen
Dieses Element wird für die Darstellung von taktischen Zeichen verwendet. Durch die fachlichen Informationen kann das anzeigende System die taktischen Zeichen selbstständig generieren oder für die Erzeugung von Übersichten oder anderen Informationen verwenden.
Das Element basiert auf dem "Point of Interest". Die URL zu der Bilddatei ist dabei immer anzugeben.

**Dieser Bereich muss überarbeitet werden!!**

|Property| Beschreibung | M/C/O | Typ | Fester Wert | Beispiel Wert
|--|--|--|--|--|--|
| situation:type | Typ des Lageelementes | M | Enum | ts
| situation:ts:grundzeichen | Grundzeichen des taktischen Zeichens | M | Enum | 
| situation:ts:fachaufgabe | Fachaufgabe des taktischen Zeichens. Sollte immer gesetzt sein, wenn kein Text gesetzt ist. | C | Enum |
| situation:ts:text | Textdarstellung anstelle einer Fachaufgabe. Soll nur gesetzt sein, wenn keine Fachaufgabe gewählt wurde. | C | Enum |
| situation:ts:formation | Art der taktischen Formation, immer notwendig, wenn für das Grundzeichen die taktische Formation gewählt wurde | C | Enum |
| situation:ts:organisation | Organisation der Einheit | O | Enum |
| situation:ts:ordnung | Ordnung bzw. Stärke der Einheit | O | Enum |
| situation:ts:personalfunktion | Besondere Funktion einer Person, kann nur für das Grundzeichen Person angegeben werden | O | Enum |
| situation:ts:staerke | Stärke der Einheit | O | Objekt |

    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [102.0, 0.5]
      },
      "properties": {
        "situation:type": "tz",
        "situation:ts:grundzeichen" : "taktische-formation",
        "situation:ts:ordnung" : "gruppe",
        "situation:ts:fachaufgabe" : "brandbekaempfung",
        "situation:ts:formation" : "kraftfahrzeug-mehrspurig",
        "situation:ts:organisation" : "feuerwehr",
        "name" : "MTL 1-46-1",
        "description" : "LF 20 Maintal",
        "imageUrl" : "https://rev.fwmtl.de/elp/api/tz-generator?typ=Brandbekaempfung&zeichen=Einheit&organisation=Feuerwehr&einheit=KraftfahrzeugMehrspurig"
      }
    }

#### lage:tz:grundzeichen
Entspricht dem Punkt 1. Grundzeichen der DV102. Mögliche Werte:

|Symbol| Enum-Wert |
|--|--|
| Abrollbehälter, Container | abrollbehaelter |
| Anhänger | anhaenger |
| Anlass, Ereignis | anlass |
| Befehlsstelle | befehlsstelle |
| Fahrrad | fahrrad |
| Fahrzeug | fahrzeug |
| Flugzeug | flugzeug |
| Gebäude | gebaeude |
| Gefahr | gefahr |
| Hubschrauber | hubschrauber |
| Kettenfahrzeug | kettenfahrzeug |
| Kraftfahrzeug, landgebunden | kraftfahrzeug-landgebunden |
| Kraftfahrzeug, geländegängig | kraftfahrzeug-gelaendegaengig |
| Kraftrad | kraftrad |
| Maßnahme | massnahme |
| Person | person |
| Schienenfahrzeug | schienenfahrzeug |
| Stelle, Einrichtung | stelle |
| Stelle, Einrichtung (ortsfest) | ortsfeste-stelle |
| Taktische Formation | taktische-formation |
| Wasserfahrzeug | wasserfahrzeug |
| Wechsellader | wechsellader |

#### lage:tz:organisation
Entspricht dem Punkt 2. Farbgebung zur Darstellung von Organisationen und Einrichtungen der Gefahrenabwehr der DV102. Mögliche Werte:

|Größenordnung| Enum-Wert |
|--|--|
| Feuerwehr | feuerwehr |
| Technisches Hilfswerk | thw |
| Hilfsorganisationen | hilfsorganisationen |
| Führung | fuehrung |
| Polizei | polizei |
| Sonstige Einrichtungen der Gefahrenabwehr  | sonstige |
| Bundeswehr | bundeswehr |

#### lage:tz:ordnung
Entspricht dem Punkt 4.1 und 4.2 Zeichen zur Darstellung von Größenordnungen, hierarchischen Zuordnungen und Ordnungsprinzipien der DV102. Mögliche Werte:

|Größenordnung| Enum-Wert |
|--|--|
| Trupp | trupp |
| Staffel | staffel |
| Gruppe | gruppe |
| Zug | zug |
| Bereitschaft (Verband I) | bereitschaft |
| Abteilung (Verband II)| abteilung |
| Großverband (Verband III)| grossverband |

#### Verwaltungsstufe (lage:tz:verwaltungsstufe)
Entspricht dem Punkt 4.3 Verwaltungsstufen der Empfehlung der SKK 2010. Mögliche Werte:

|Verwaltungsstufe| Enum-Wert |
|--|--|
| Gemeinde, kreisangehörige Stadt | gemeinde |
| Kreis / Landkreis oder kreisfreie Stadt | kreis |
| Bezirk | bezirk |
| Land / Freistaat | land |
| Bundesrepublik Deutschland | brd |
| Europäische Union | eu |

#### lage:tz:personalfunktion
Entspricht dem Punkt 5. Zeichen zur Darstellung von Personen mit besonderen Funktionen der DV102. Mögliche Werte:

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

#### lage:tz:staerke
Stärke nach FwDV 100 Führer, Unterführer und Mannschaft.

    {
      "fuehrer": "1",
      "unterfuehrer": "3",
      "mannschaft": "18"
    }

#### lage:ts:text

Alternativ zu einer Fachaufgabe kann auch ein Text in das Zeichen generiert werden. Dies ist z.B. bei der Generierung von Fahrzeugen des THW sinnvoll.




## Bereiche

Für die Darstellung von Bereichen werden i.d.R. Polygone oder Kreise verwendet. 

Zur Darstellung von Gebieten werden folgende Properties verwendet:

|Property| Beschreibung | M/C/O | Fester Wert |
|--|--|--|--|
| [situation:type](#lagekartenelemente) | Typ des Lageelementes | M | bereich
| [situation:area:type](#flächenart-situationareatype) | Art des Bereichs | M | 
| [situation:area:danger](#gefährdungssituation-situationareadanger) | Gibt an ob die Gefahr Drohend oder Ehemals ist | O | 
| [name](#name-name) | Name oder Bezeichnung des Gebietes | O | 

Zusätzlich zu den fachlichen Properties sollten auch alle [Styling Properties](#styling-properties) gesetzt werden um eine größtmögliche Kompatibilität zu gewährleisten.

### Flächenart (situation:area:type)

Für die Darstellung beschreibt die Empfehlung zur Darstellung von taktischen Zeichen folgende Gebietsarten:

|Gebiet| Hintergrundfarbe | Rahmenfarbe | Enum-Wert
|--|--|--|--|
| Allgemeine Fläche | Transparent | Schwarz | generic
| Flächenbrand | Rot | Nicht festgelegt | wildfire
| Überschwemmtes Gebiet | Blau | Nicht festgelegt | flooded
| Dürregebiet | Braun | Nicht festgelegt | drought
| Einschränkung oder Ausfall der Versorgung | Violett | Nicht festgelegt | outage
| Sonstiges Schadensgebiet | Orange | Nicht festgelegt | miscellaneous
| Kontaminiertes Gebiet | Gelb | Nicht festgelegt | contaminated
| KatS Alarm | Transparent | Rot | disaster-alert
| Einsatzraum | Transparent | Schwarz mit Beschriftung | operating

### Gefährdungssituation (situation:area:danger)

Zusätzlich ist das Muster der Hintergrundfarbe für verschiedene Zustände definiert:

|Gefährdungssituation | Muster | Enum-Wert
|--|--|--|
| Vorhandene Gefahr | Vollflächig | acute
| Drohende Gefahr | Gestreift 45° Winkel | imminent
| Noch oder ehemalsbetroffenes Gebiet | Horizontal gestreift | former

## Massenanfall von Verletzten (MANV)

Für die Abbildung eines MANV können die Positionen von Patienten in der Lagekarte dokumentiert werden. Dafür stehen die folgenden Eigenschaften zur Verfügung.

### Sichtungskategorie (situation:patient:triage)

|Sichtungskategorie| Beschreibung | Farbempfehlung | Enum-Wert
|--|--|--|--|
| SK I | Vital bedroht | rot | cat-1
| SK II | Schwer verletzt / erkrankt | gelb | cat-2
| SK III | Leicht verletzt / erkrankt | grün | cat-3
| SK IV | Ohne Überlebenschance | blau | cat-4
| EX | Tot | schwarz | dead

### Zeitpunkt der Sichtung (situation:patient:triage-time)

Zeitpunkt zu dem die letzte Sichtung durchgeführt wurde.

## Stoffnachweis / Messpunkte (GABC)

Für die Abbildung von Messpunkten können die Positionen der Messungen inkl. des Messergebnisses dargestellt werden.

