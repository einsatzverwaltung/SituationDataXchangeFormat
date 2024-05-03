
# Lagekarte JSON v0.1
Lagekarte JSON ist eine Konvention, basierend auf dem GeoJSON Schema ([RFC 7946](https://datatracker.ietf.org/doc/html/rfc7946)) zum Austausch von georeferenzierten Lagekarten im Kontext der Behörden und Organisationen mit Sicherheitsaufgaben. Bei der Definition wurde auf eine größtmögliche Kompatibilität zu vorhandenen Standards geachtet und die Empfehlung der [DV 102](https://www.kritis.bund.de/SharedDocs/Downloads/BBK/DE/Publikationen/Broschueren_Flyer/Empfehlungen_Takt_Zeichen_im_BevSch.pdf?__blob=publicationFile) des Bundesamtes für Bevölkerungs- und Katastrophenschutz beachtet.
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

Attribut| [Point](#point-punkt) | [LineString](#line-string-linie) | [Polygon](#polygon) | [Circle](#circle--kreis) | [TZ](#Taktisches-Zeichen) | 
|--|--|--|--|--|--|
| [stroke](#styling-properties)          | - | O | O | O | - |
| [stroke-width](#styling-properties)    | - | O | O | O | - |
| [stroke-opacity](#styling-properties)  | - | O | O | O | - |
| [fill](#styling-properties)            | - | - | O | O | - |
| [fill-opacity](#styling-properties)    | - | - | O | O | - |
| [radius](#circle--kreis)         | - | - | - | M | - |
| [lage:typ](#lage:typ)        | O | O | O | O | M |
| [lage:erstellt](#Erstellungszeitpunkt-lage-erstellt)   | M | M | M | M | M |
| [lage:autor](#Autor-lage-autor)   | M | M | M | M | M |
| [lage:bearbeitet](#Änderungszeitpunkt-lage-bearbeitet)   | O | O | O | O | O |
| [lage:name](#lage:name)   | O | O | O | O | O |
| [lage:beschreibung](#lage:beschreibung)   | O | O | O | O | O |
| [lage:tz:grundzeichen](#lage:tz:grundzeichen)  | - | - | - | - | C |
| [lage:tz:fachaufgabe](#lage:tz:fachaufgabe)   | - | - | - | - | M |
| [lage:tz:formation](#lage:tz:formation)     | - | - | - | - | C |
| [lage:tz:organisation](#lage:tz:organisation)  | - | - | - | - | O |
| [lage:tz:ordnung](#lage:tz:ordnung)       | - | - | - | - | O |
| [lage:tz:personalfunktion](#lage:tz:personalfunktion) | - | - | - | - | O |
| [lage:tz:ortsfest](#lage:tz:ortsfest)      | - | - | - | - | O |
| [lage:tz:text](#lage:tz:text)      | - | - | - | - | C |
| [lage:image:size](#bildgröße-lage-image-size)   | O | - | - | - | O |
| [lage:image:anchor](#bildanker-lage-image-anchor)   | O | - | - | - | O |
| [lage:bereich:art](#lage:bereich:art)         | - | - | O | O | - |
| [lage:bereich:gefahr](#lage:bereich:gefahr)   | - | - | O | O | - |

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

Properties können Mandatory, Optional oder Conditional sein. Mandatory-Properties müssen immer gesetzt sein. Optional-Properties sind optional und können bei Bedarf gesetzt werden.
Conditional-Properties sind in bestimmten Fällen Mandatory. Es hängt also von anderen Properties ab, ob die Conditional-Property übermittelt werden muss. 

### Allgemeine Attribute

#### Erstellungszeitpunkt (lage:erstellt)

Zeitpunkt der Erstellung des Lagekartenelementes.

#### Änderungszeitpunkt (lage:bearbeitet)

Zeitpunkt der letzten Änderung an diesem Lagekartenelement.

#### Autor (lage:autor)

Der Autor sollte der Name, der Rufname oder die Funktion einer Person sein, die dieses Lagekartenelement erstellt hat.

### Point of Interest
Dieses Element ist für die grundsätzliche Darstellung eines Bildes ohne weitere fachliche Definition zu verwenden. Hiermit können Beispielsweise besondere Punkte markiert werden.

|Property| Beschreibung | M/C/O | Fester Wert |
|--|--|--|--|
| lage:typ | Typ des Lageelementes | M | poi
| lage:name | Name des POI, wird auf Lagekarte dargestellt | O | 
| lage:description | Beschreibung des POI (Beispielsweise für Popup) | O |
| lage:imageUrl | URL zum Bild (Kann auch Data-URL sein) | M |

Beispiel:

    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [102.0, 0.5]
      },
      "properties": {
        "lage:typ": "poi",
        "lage:name" : "Toiletten",
        "lage:description" : "Tolietten mit Handwaschmöglichkeit",
        "lage:url": "https://cdn-icons-png.flaticon.com/512/185/185547.png"
      }
    }

### Taktisches Zeichen
Dieses Element wird für die Darstellung von taktischen Zeichen verwendet. Durch die fachlichen Informationen kann das anzeigende System die taktischen Zeichen selbstständig generieren oder für die Erzeugung von Übersichten oder anderen Informationen verwenden.
Das Element basiert auf dem "Point of Interest". Die URL zu der Bilddatei ist dabei immer anzugeben.

|Property| Beschreibung | M/C/O | Typ |
|--|--|--|--|
| lage:typ | Typ des Lageelementes | M | tz
| lage:tz:grundzeichen | Grundzeichen des taktischen Zeichens | M | Enum |
| lage:tz:fachaufgabe | Fachaufgabe des taktischen Zeichens. Sollte immer gesetzt sein, wenn kein Text gesetzt ist. | C | Enum |
| lage:tz:text | Textdarstellung anstelle einer Fachaufgabe. Soll nur gesetzt sein, wenn keine Fachaufgabe gewählt wurde. | C | Enum |
| lage:tz:formation | Art der taktischen Formation, immer notwendig, wenn für das Grundzeichen die taktische Formation gewählt wurde | C | Enum |
| lage:tz:organisation | Organisation der Einheit | O | Enum |
| lage:tz:ordnung | Ordnung bzw. Stärke der Einheit | O | Enum |
| lage:tz:personalfunktion | Besondere Funktion einer Person, kann nur für das Grundzeichen Person angegeben werden | O | Enum |
| lage:tz:staerke | Stärke der Einheit | O | Objekt |

    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [102.0, 0.5]
      },
      "properties": {
        "lage:typ": "tz",
        "lage:tz:grundzeichen" : "taktische-formation",
        "lage:tz:ordnung" : "gruppe",
        "lage:tz:fachaufgabe" : "brandbekaempfung",
        "lage:tz:formation" : "kraftfahrzeug-mehrspurig",
        "lage:tz:organisation" : "feuerwehr",
        "lage:name" : "MTL 1-46-1",
        "lage:description" : "LF 20 Maintal",
        "lage:url" : "https://rev.fwmtl.de/elp/api/tz-generator?typ=Brandbekaempfung&zeichen=Einheit&organisation=Feuerwehr&einheit=KraftfahrzeugMehrspurig"
      }
    }

#### lage:tz:grundzeichen
Entspricht dem Punkt 1. Grundzeichen der DV102. Mögliche Werte:

|Symbol| Wert |
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

|Größenordnung| Wert |
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

|Größenordnung| Wert |
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

|Verwaltungsstufe| Wert |
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

#### lage:tz:text

Alternativ zu einer Fachaufgabe kann auch ein Text in das Zeichen generiert werden. Dies ist z.B. bei der Generierung von Fahrzeugen des THW sinnvoll.

#### Bildgröße (lage:image:size)

Größenangabe des Bildes in Pixeln.

    [10, 20]

#### Bildanker lage:image:anchor

Ankerpunkt des Bildes in Pixeln. An diese Stelle wird das Bild verschoben, um die Koordinate zu markieren. 

    [5, 20]

## Bereiche

Für die Darstellung von Bereichen werden i.d.R. Polygone oder Kreise verwendet. Für die Darstellung beschreibt die Empfehlung zur Darstellung von taktischen Zeichen folgende Gebietsarten:

|Gebiet| Hintergrundfarbe | Rahmenfarbe |
|--|--|--|
| Allgemeine Fläche | Transparent | Schwarz |
| Flächenbrand | Rot | Nicht festgelegt |
| Überschwemmtes Gebiet | Blau | Nicht festgelegt |
| Dürregebiet | Braun | Nicht festgelegt |
| Einschränkung oder Ausfall der Versorgung | Violett | Nicht festgelegt |
| Sonstiges Schadensgebiet | Orange | Nicht festgelegt |
| Kontaminiertes Gebiet | Gelb | Nicht festgelegt |
| KatS Alarm | Transparent | Rot |
| Einsatzraum | Transparent | Schwarz mit Beschriftung |

Zusätzlich ist das Muster der Hintergrundfarbe für verschiedene Zustände definiert:

|Gefahrenart | Muster |
|--|--|
| Vorhandene Gefahr | Vollflächig |
| Drohende Gefahr | Gestreift 45° Winkel |
| Noch oder ehemalsbetroffenes Gebiet | Horizontal gestreift |

Zur Darstellung von Gebieten werden folgende Properties verwendet:

|Property| Beschreibung | M/C/O | Fester Wert |
|--|--|--|--|
| lage:typ | Typ des Lageelementes | M | bereich
| lage:bereich:art | Art des Bereichs | M | 
| lage:bereich:gefahr | Gibt an ob die Gefahr Drohend oder Ehemals ist | O | 
| lage:name | Name oder Bezeichnung des Gebietes | O | 

Zusätzlich zu den fachlichen Properties sollten auch alle [Styling Properties](#styling-properties) gesetzt werden um eine größtmögliche Kompatibilität zu gewährleisten.

### lage:bereich:art

Beschreibt die Art des Bereichs

 Mögliche Werte
 - Allgemein
 - Flächenbrand
 - Überschwemmung
 - Dürre
 - Versorgungsausfall
 - Sonstiges
 - Kontaminiert
 - KatsAlarm
 - Einsatzraum

### lage:bereich:gefahr

Beschreibt das Vorhandenseins der Gefahr für einen Bereich

 Mögliche Werte
 - Akut
 - Drohend
 - Ehemalig

## Name (lage:name)

Diese Eigenschaft gibt dem Objekt einen Namen. Dieser Name bezeichnet das Objekt und sollte immer in der Lagekarte dargestellt werden.

## Beschreibung (lage:beschreibung)

Diese Eigenschaft beschreibt das Objekt und sollte in der Lagekarte z.B. als Tooltip dargestellt werden.