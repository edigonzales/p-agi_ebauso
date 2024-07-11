# p-agi_ebauso

## OEREB

Welche Daten sind nicht im ÖREB:

- FFF
- Naturgefahren
- Grundnutzung: Wald und Landwirtschaftstypen fehlen (?)
- Natur / Landschaft (Richtplan): Falls nur Juraschutzzone interessant, gibt es die sicher auch in der Nutzungsplanung (?). Aber auch im ÖREB?

Problematischer dürfte die Nicht-Fähigkeit von Buffer-Abfragen zu sein. Bei welchen Themen wird gebuffert abgefragt:

- Waldabstand von was genau? 20m
- Hecken / Ufergehölz (Nutzungsplanung) 10m
- Baulinie 6m







Naturreservate, Geotope, Archäologie und Denkmalschutz sind im Einzelschutz.

## Grundstückinformation (im engeren Sinn)

### Gebäudeadressierung
Wie soll mit Gebäudeadressen umgegangen werden? Diese sind an einen Punkt gebunden (dem Eingang). Wie funktioniert das heute in eBauSO? Wann/wie schlägt es mir eine Adresse vor?

Neu gibt es bei uns auch einen Layer, der die Adressen an die Gebäudefläche bindet (als JSON-Struktur). Wäre sowas eine Lösung? Dann aber gleich auf das Grundstück?

Beispiel mit Grundnutzung (Richtplan). Dazu habe ich in simi auf der Integratiosnumgebung die Dokumente als Attribut freigeschaltet und anschliessend wieder zurückgesetzt.

Wie sieht JSON in JSON aus?

```
"properties": {
"grundnutzungsart": "Siedlungsgebiet.Wohnen_oeffentliche_Bauten",
"dokumente": "[{\"@type\": \"SO_ARP_Nutzungsplanung_Publikation_20201005.Nutzungsplanung.Dokument\", \"Titel\": \"Baureglement\", \"Kanton\": \"SO\", \"Gemeinde\": 2461, \"TextimWeb\": \"https://geo.so.ch/docs/ch.so.arp.zonenplaene/Zonenplaene_pdf/38-Schnottwil/Reglemente/38_BR.pdf\", \"Abkuerzung\": \"BR\", \"OffizielleNr\": \"38_BR\", \"Rechtsstatus\": \"inKraft\", \"publiziertAb\": \"2006-06-20\", \"OffiziellerTitel\": \"Baureglement\", \"Rechtsvorschrift\": true}, {\"@type\": \"SO_ARP_Nutzungsplanung_Publikation_20201005.Nutzungsplanung.Dokument\", \"Titel\": \"Zonenreglement\", \"Kanton\": \"SO\", \"Gemeinde\": 2461, \"TextimWeb\": \"https://geo.so.ch/docs/ch.so.arp.zonenplaene/Zonenplaene_pdf/38-Schnottwil/Reglemente/038_ZR.pdf\", \"Abkuerzung\": \"ZR\", \"OffizielleNr\": \"038_ZR\", \"Rechtsstatus\": \"inKraft\", \"publiziertAb\": \"2020-11-20\", \"OffiziellerTitel\": \"Gesamtrevision der Ortsplanung\", \"Rechtsvorschrift\": true}, {\"@type\": \"SO_ARP_Nutzungsplanung_Publikation_20201005.Nutzungsplanung.Dokument\", \"Titel\": \"Regierungsratsbeschluss\", \"Kanton\": \"SO\", \"Gemeinde\": 2461, \"TextimWeb\": \"https://geo.so.ch/docs/ch.so.arp.zonenplaene/Zonenplaene_pdf/38-Schnottwil/Entscheide/38-64_67-E.pdf\", \"Abkuerzung\": \"RRB\", \"OffizielleNr\": \"38-64_67-E\", \"Rechtsstatus\": \"inKraft\", \"publiziertAb\": \"2020-09-22\", \"OffiziellerTitel\": \"Gesamtrevision der Ortsplanung\", \"Rechtsvorschrift\": true}]"
},
```

Dann müsste aber das GUI Gebäudeadressen vorschlagen.

(von https://geo-i.so.ch/api/data/v1/ch.so.arp.richtplan.grundnutzung.data/124115)

### Lösungsvarianten

#### Eine Klasse

```
STRUCTURE Gebaeudeadresse
    Strassenname_benanntes_Gebiet : MANDATORY TEXT;
    Hausnummer : MANDATORY TEXT;
    PLZ : MANDATORY 1000..9999;
    Ortschaft : MANDATORY TEXT;
END Gebaeudeadresse;

CLASS Grundstueck
    Nummer : MANDATORY TEXT;
    EGRID : MANDATORY TEXT*14;
    Art : MANDATORY (
        Liegenschaft,
        SelbstRecht.Baurecht,
        SelbstRecht.Quellenrecht
    ); 
    Flaechenmass : MANDATORY 0..9999999999;
    BFSNr : MANDATORY 0..3000;
    Gemeinde : MANDATORY TEXT;
    Grundbuch : MANDATORY TEXT;
    Adressen : BAG {0..*} OF Gebaeudeadresse;
END Grundstueck;
```

Es gibt eine Klasse mit einem verschachtelten Attribut (Adressen). Diese verschachtelte Attribut kommt im Dataservice als escaptes JSON (siehe oben).

Entscheidung welche Adresse verwendet werden soll in eBauSO ist Aufgabe von eBauSO (und nicht der Schnittstelle). Ebenfalls unklar ist mir der Umgang mit Baurechten auf Liegenschaften. 

Offen ist die Frage, ob auch projektierten Grundstücke mitgeliefert werden sollen/müssen. Falls ja, braucht es ein zusätzliches Attribut.

#### Zwei Klassen
Weiterhin eine Klasse resp. ein Layer für Grundstück und Gebäudeeingang. Wie der Gebäudeeingang ermittelt wird ist mir nicht klar (Buffer?). Falls zwei Klassen, müssen sie auch nicht zwingen ins Rahmenmodell, sondern es werden weiterhin die stabilen Layer verwendet.

## Fachdaten

Es wird (immer) der gesamte Datensatz ins Rahmenmodell kopiert. Für welche Frage welche Typen massgebend sind, ist Sache der Software (eBauSO) und nicht der Schnittstelle. Z.B. will ich wissen, ob ich ausserhalb der Bauzone bin, muss ich für den geklickten Punkt die Antwort des Datendienstes für das Thema Nutzungsplanung_Grundnutzung auswerten. Dieser liefert "nur" den Typ zurück.

Anders rum wäre schon auch möglich. Dann gibt es jedoch eine festere Koppelung und bei einen neuen Frage in eBauSO besteht die Gefahr, dass das Rahmenmodell um einen Layer oder mindestens Thema erweitert werden muss.

Wahrscheinlich liefe es darauf hinaus, ob man Daten pro Thema oder pro Frage ins Rahmenmodell presst:

```
CLASS Fachdaten
    Thema : MANDATORY TEXT; !! z.B. Grundnutzung
    !! oder
    Frage : MANDATORY TEXT; !! Ausserhalb_Bauzone
END Fachdaten;
```

Im "Frage"-Fall wären nur Geometrien vorhanden, die tatsächlich Grundnutzung ausserhalb Bauzonen sind.

Entscheid, ob es die Dokumente benötigt. Wären wie Gebäudeadressen (siehe oben) ein JSON in JSON.

Es braucht wohl für jeden Geometrietyp eine Klasse (Tabelle oder View?).

### Themen

Erste Einschätzung von mir bezüglich Attribute (anhand Word-Datei Dani).

Attribute zusätzlich?

- Rechtsstatus


Kap. 3.2.1: Grundnutzung:

Quelle: Grundnutzung

- typ_bezeichnung 
- typ_kt

Kap. 3.2.2: Sondernutzung:

Quelle: nutzungsplanung_ueberlagernd_flaeche

- typ_bezeichnung
- typ_kt

Kap. 3.2.2 Ortsbildschutz (kantonal)

Hier dünkt mich noch was nicht passend. Es gibt einige Typen, die m.E. nicht Ortsbildschutz sind.

Langer Rede, kurzer Sinn: Es geht hier um überlagernde Flächen der Nutzungsplanung (alle drei Geometrietypen).

Kap. 3.2.5 Baulinien

Warum "kantonaler Erschliessungsplan"?

Daten entstammen auch aus der Nutzungsplanung (Erschliessung Linienobjekte).

Kap. 3.2.6 Baulinien (Nationalstrasse)

Haben wir nur im ÖREB-Kataster als Daten.

Kap. 3.3 Fruchtfolgeflächen

Hier wird es erstmals spannend, da nicht mehr Nutzungsplanung. Welche Attribute werden benötigt? Ggf. JSON in JSON oder Absprung in den Web GIS Client.

- bezeichnung
- speziallfall
- anrechenbar

Falls nur wichtig ist, ob man in FFF liegt, dann reicht bezeichnung

Kap. 3.4. Naturgefahren

- Gef_Stufe -> ist Aufzähltyp -> "typ_code"
- Index -> Freitext -> "bezeichnung"

Kap. 3.5. Altlasten

- Bewertung -> Freitext

Kap. 3.6. Gewässerschutz

Schutzzonen und Areale Tabelle.

- typ -> Aufzähltyp -> typ_code

In solchen Fälle bei bezeichnung auf typ reinfüllen?

Kap. 3.7.1 Natur- und Landschaftsschutz (Nutzungsplanung)

Ist wieder Nutzungsplanung überlagende Flächen und Punkte

Kap. 3.7.2 Natur / Landschaft (Richtplan)

Wirklich Richtplan? Und nicht Nutzungsplanung? Welche Objekttypen? Es wird von Juraschutzzone gesprochen aber sehr viele Objekttypen aufgelistet.

Kap. 3.7.3 Naturreservate

Reservate und Teilflächen.

- aname -> Freitext (kürzer)
- beschreibung -> Freitext (länger)

Kap. 3.7.4 Geotope

Hier gibt es noch eine Geologie-Layer im Worddokument ("karst"). Sonst wären wohl folgende Attribute Kandidaten:

- objektname
- beschreibung

Kap. 3.8.1 Denkmalschutz

Hier ist mir echt nicht mehr ganz klar, woher die Daten jetzt kommen resp. was geschützt ist und was nicht.

Attribute wahrscheinlich:

- objetkname -> Freitext
- schutzstufe_text -> Aufzähltyp

Kap. 3.8.2 Archäologie

Auch hier ist es mir bisschen ungeheuer bezüglich welche Daten verwendet werden. Vgl mit Ist-Zustand.

Attribut:

- funstellen_nummer -> Freitext 255
- fundstellen_art -> Freitext 255


-> Nummer könnte Identifikator werden und Art -> Bezeichung oder Beschreibung

