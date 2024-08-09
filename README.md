# p-agi_ebauso

## Bemerkungen (tbd)

- es werden jeweils die öffentlichen Daten / Layer verwendet. Weil sie eben in einem öffentlichen System verwendewt werden. D.h. z.B. für KbS könenn wir nur den geschützten Layer verwenden. ADA-Gedöns ist auch so. -> Muss man die Ämter noch informieren. Bei Denkmalschutz wird der geschützte (nicht-öffentlich) Layer über die Darstellung geregelt (schutzstatus_code=geschuetzt). Der bestehende Dataservice-Layer verwendet also heute bereits alle. Will man so?
- KbS: Auch Bundesthemen?
- Juraschutzzone: Gibt es nur im Richtplan, oder? -> Thema ch.so.richtplan.juraschutzzone?
- Aus Bodenbedeckung nur Wald und Gewässer. Was ist mit Hecken, übrig_bestockt, Rinnsale (aus Einzelobjekte)

- Achtung: "Rechtsstatus": in solothurn nur Daten mit AenderungMitVorwirkung -> wohl im Request mitangeben.
- Was eBauSO was mit welchen Daten macht, muss eBauSO wissen. Falls sie eher nicht gefiltert requestet, muss sie wissen, ob eine Antwort relevant ist. Arbeitet eBauSO immer mit Filter pro Request, kann keine Antwort auch eine Antwort sein (nämlich, dass es z.B. keine FFF gibt.)

TODO: uppercase thema (analog öreb)

-> Doku: Was kommt von wo? Projekt muss das verstehen und akzeptieren.
-> Doku: Aufteilung "Struktur" und "Inhalt" (Inhalt pro Thema aufzeigen)

-> Andi S. "https://gretl-i.so.ch/view/GRETL-Jobs/job/dsbjd_ebauso_rahmenmodell_pub/1/console" -> Zeigt das noch afu oereb und nicht oereb_v2?


[pid: 7|app: 0|req: 13827/13827] 100.66.7.116 () {74 vars in 1947 bytes} [Thu Aug 8 12:03:33 2024] GET /api/data/v1/ch.so.agi.av.gebaeudeadressen.gebaeudeeingaenge/?filter=[[%22t_id%22,%22=%22,3896076]] => generated 421 bytes in 4 msecs (HTTP/1.0 200) 2 headers in 72 bytes (1 switches on core 1)


[pid: 7|app: 0|req: 13532/13529] 100.66.7.116 () {74 vars in 1689 bytes} [Thu Aug 8 11:34:43 2024] GET /api/data/v1/ch.so.agi.av.grundstuecke.rechtskraeftig.data/?&bbox=2634375.453457647,1243292.2974765466,2634375.453457647,1243292.2974765466 => generated 5886 bytes in 9 msecs (HTTP/1.0 200) 2 headers in 73 bytes (1 switches on core 2)



https://geo.so.ch/api/data/v1/ch.so.agi.av.grundstuecke.rechtskraeftig.data/?&bbox=2634375,1243292,2634375,1243292 

https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_fachthemen_flaechen.data/?&bbox=2634375,1243292,2634375,1243292 



https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_lokalisation_gebaeudeeingang.data/?&bbox=2610530,1229440,2610570,1229480
https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_lokalisation_grundstueck.data/?&bbox=2610550,1229460,2610550,1229460


Drei Themen:
https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_fachthemen_flaechen.data/?&bbox=2610550,1229460,2610550,1229460


Nur Bodenbedeckung (für Wald und Gewässer) / Request pro Thema:
https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_fachthemen_flaechen.data/?&bbox=2610460,1227980,2610500,1228020&filter=[[%22thema%22,%22=%22,%22ch.SO.Bodenbedeckung%22]]

Linien:
https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_fachthemen_linien.data/?&bbox=2605130,1228960,2605170,1229000
https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_fachthemen_linien.data/?filter=[[%22t_id%22,%22=%22,6742179]]

Test Erschliessung:
https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_fachthemen_flaechen.data/?&bbox=2605130,1228960,2605170,1229000

Änderung mit Vorwirkung (Rechtsstatus):
https://geo-i.so.ch/api/data/v1/ch.so.dsbjd.ebauso_fachthemen_flaechen.data/?&bbox=2607890,1228270,2607890,1228270


Sind da auch gemäss Konzept alle mögliche?

SELECT DISTINCT 
    thema
FROM 
    dsbjd_ebauso_rahmenmodell_pub_v1.fachthemen_fachthema_polygon 

UNION 

SELECT DISTINCT 
    thema
FROM 
    dsbjd_ebauso_rahmenmodell_pub_v1.fachthemen_fachthema_linie 

UNION 

SELECT DISTINCT 
    thema
FROM 
    dsbjd_ebauso_rahmenmodell_pub_v1.fachthemen_fachthema_punkt




2605150,1228980
2605130,1228960
2605170,1229000



2610480,1228000
2610460,1227980
2610500,1228020


2596250,1228060
2596230,1228040
2596270,1228100




2.610.556, 1.229.463

2610550,1229460
2610530,1229440
2610570,1229480

## GRETL-Zeugs

```
docker compose run --rm -u $UID --workdir //home/gradle/schema-jobs/shared/schema \
  gretl -PtopicName=dsbjd_ebauso_rahmenmodell -PschemaDirName=schema_pub createSchema configureSchema
```


```
docker compose run --rm -u $UID --workdir //home/gradle/schema-jobs/shared/schema \
  gretl -PtopicName=dsbjd_ebauso_rahmenmodell -PschemaDirName=schema_stage createSchema configureSchema
```


```
docker compose run --rm -u $UID gretl --project-dir=dsbjd_ebauso_rahmenmodell_pub
```


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

Naturreservate, Geotope, Archäologie und Denkmalschutz sind im Einzelschutz. Aber ist alles davon im ÖREB, was interessiert?

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

