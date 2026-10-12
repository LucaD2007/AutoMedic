# Fallstudie -- Digitale Terminbuchungsplattform fuer Arztpraxen

**Modul:** Methoden der Wirtschaftsinformatik (3. Semester)
**Stand:** 12. Oktober 2026
**Abgabefrist:** 13.11.2026, 23:59 Uhr (Moodle-Upload)
**Abschlusspraesentation:** Mitte Oktober 2026

> Hinweis: Diese Datei hiess frueher `Fallstudie.md` bzw. `KI_Luca.md` und wird ab jetzt als `KI.md` gefuehrt.

---

## Team & Rollen

| Person   | Rolle                              |
|----------|------------------------------------|
| Fabian   | --                                 |
| Lennart  | --                                 |
| Thilo    | --                                 |
| Helen    | --                                 |
| Luca     | Scrum Master                       |
| Niharini | KI-Manager                         |

---

## Projektbeschreibung

Entwicklung einer digitalen Terminbuchungsplattform fuer Arztpraxen. Das System soll Patienten ermoeglichen, eigenstaendig Termine zu suchen, zu buchen, zu verschieben und abzusagen. Die Praxis kann Sprechzeiten, Verfuegbarkeiten und Patienteninformationen verwalten. Automatisierungspotenzial liegt in der Terminverfuegbarkeitspruefung, Erinnerungen/Bestaetigungen sowie automatischer Aktualisierung bei Absagen.

---

## Prozesse (10 BPMN-Kollaborationsdiagramme + 2 Reserve)

Alle 12 Diagramme liegen final vor (nach Betreuer-Feedback ueberarbeitet). Status "Final" = aktuelle, abgestimmte Version im Sandbox.

| Nr. | Prozess                                 | Status | Aktuelle Datei                          |
|-----|-----------------------------------------|--------|-----------------------------------------|
| 01  | Termin suchen & buchen                  | Final  | 01-TerminSuchenBuchen-2.bpmn            |
| 02  | Termin absagen                          | Final  | 02-TerminAbsagen_v3_neu.bpmn            |
| 03  | (Regelmaessige) Terminbenachrichtigung  | Final  | 03-Terminbenachrichtigung_v3.bpmn       |
| 04  | Termin verschieben                      | Final  | 04-TerminVerschieben EDITED.bpmn        |
| 05  | Patient ueberweisen                     | Final  | 05-PatientUeberweisen.bpmn              |
| 06  | Check-In beim Arzt                      | Final  | 06-CheckInBeimArzt.bpmn                 |
| 07  | Notfallpatient                          | Final  | 07-Notfallpatient.bpmn                  |
| 08  | Apotheke suchen                         | Final  | 08-ApothekeSuchen_v3_neu.bpmn           |
| 09  | Post-Termin (aus Arztsicht)             | Final  | 09-PostTermin_v2.bpmn                   |
| 10  | Patientenregistrierung / Erstanmeldung  | Final  | 10-Patientenregistrierung EDITED.bpmn   |
| 11  | Rezept verlaengern / Folgerezept (Reserve) | Final | 11-RezeptVerlaengern.bpmn             |
| 12  | Videosprechstunde (Reserve)             | Final  | 12-Videosprechstunde.bpmn               |

> **To-do vor Abgabe:** Dateinamen auf einheitliches Schema `XX-PraegnanterName` ohne Umlaute/Suffixe (-2, _v3_neu, EDITED) bringen, bevor das BPMN-ZIP eingereicht wird.

---

## Detailaufbau der BPMN-Diagramme

### 01 - Termin suchen & buchen
- **Pools:** Patient (7), Terminbuchungsplattform (8, 1 Gateway), Arztpraxis (4, 2 Gateways) -- 19 Activities, 5 Message Flows
- **Patient:** Plattform aufrufen und anmelden, Fachrichtung/Arzt auswaehlen, Verfuegbare Termine suchen, Termin auswaehlen, Termingrund/Notiz eingeben, Buchung bestaetigen, Buchungsbestaetigung erhalten
- **System:** Suchanfrage verarbeiten, Verfuegbarkeit im Kalender pruefen, Verfuegbare Termine anzeigen, Buchungsanfrage empfangen, Termin im Kalender reservieren, Notiz zum Termin speichern, Bestaetigung an Patient senden, Praxis ueber neuen Termin benachrichtigen
- **Arztpraxis:** Terminbenachrichtigung pruefen, Termin im Praxiskalender einsehen, Vorbereitung fuer Termin planen, Geraete reservieren
- **Datenobjekte:** Notiz (Was soll gemacht werden) | **Data Stores:** Terminkalender, Geraeteverwaltung

### 02 - Termin absagen
- **Pools:** Patient (2), Terminbuchungsplattform (3, 1 Gateway), Arztpraxis (3, 2 Gateways) -- 8 Activities, 3 Message Flows
- **Patient:** Termin zur Absage auswaehlen, Absage bestaetigen und senden
- **System:** Termin identifizieren und Stornierbarkeit pruefen, Absagebestaetigung an Patient senden, Praxis ueber Absage benachrichtigen
- **Arztpraxis:** Absage pruefen und Kalender aktualisieren, Nachruecker ueber freien Slot informieren, Raumbuchung und Geraete freigeben
- **Datenobjekte:** Absagegrund | **Data Stores:** Terminkalender, Geraeteverwaltung

### 03 - (Regelmaessige) Terminbenachrichtigung
- **Pools:** Terminbuchungsplattform (5, 4 Gateways), Patient (3, 2 Gateways), Arztpraxis (2) -- 10 Activities, 4 Message Flows
- **System:** Anstehende Termine abfragen, Erinnerung erstellen und versenden, Terminstatus aktualisieren, Termin stornieren, Tagesbericht senden
- **Patient:** Erinnerungsnachricht pruefen, Termin absagen, Neuen Termin vorschlagen
- **Arztpraxis:** Tagesbericht pruefen und nachfassen, Tagesplan anpassen
- **Datenobjekte:** Erinnerungsnachricht | **Data Stores:** Terminkalender, Benachrichtigungslog

### 04 - Termin verschieben
- **Pools:** Patient (3, 2 Gateways), Terminbuchungsplattform (6, 1 Gateway), Arztpraxis (1) -- 10 Activities, 6 Message Flows
- **Patient:** Termin auswaehlen und Verschiebungsanfrage stellen, Neuen Termin auswaehlen und bestaetigen, Ablehnungshinweis erhalten
- **System:** Anfrage pruefen und Alternativen ermitteln, Ablehnungshinweis an Patient senden, Alternative Termine an Patient senden, Terminauswahl empfangen, Terminverschiebung verarbeiten (Subprozess), Bestaetigung und Benachrichtigung versenden
- **Arztpraxis:** Kalender und Ressourcen aktualisieren
- **Datenobjekte:** Verschiebungsanfrage | **Data Stores:** Terminkalender

### 05 - Patient ueberweisen
- **Pools:** Hausarzt (2), Terminbuchungsplattform (5), Patient (3), Facharzt (2) -- 12 Activities, 6 Message Flows
- **Hausarzt:** Ueberweisung erstellen und im System erfassen, Statusrueckmeldung erhalten
- **System:** Passenden Facharzt und Termine ermitteln (Subprozess), Terminvorschlaege an Patient senden, Termin buchen, Termin mit Ueberweisung verknuepfen, Patient/Facharzt/Hausarzt benachrichtigen
- **Patient:** Ueberweisung und Terminvorschlaege einsehen, Termin auswaehlen und bestaetigen, Buchungsbestaetigung erhalten
- **Facharzt:** Ueberweisungsdaten und Diagnose pruefen, Patientenakte anlegen
- **Datenobjekte:** Ueberweisungsschein, Patientenakte | **Data Stores:** Ueberweisung(sdatenbank)

### 06 - Check-In beim Arzt
- **Pools:** Patient (3), Terminbuchungsplattform (4), Arztpraxis (3) -- 10 Activities, 5 Message Flows
- **Patient:** Check-In starten und Versichertenkarte einlesen, Wartenummer und Wartezeit erhalten, Aufruf erhalten und zum Behandlungszimmer gehen
- **System:** Check-In-Daten validieren (Subprozess), Patient als anwesend registrieren und Wartenummer vergeben, Praxis ueber Ankunft benachrichtigen, Aufruf an Patient weiterleiten
- **Arztpraxis:** Patientendaten und Termingrund einsehen, Behandlungsreihenfolge festlegen, Patient aufrufen
- **Datenobjekte:** Versichertenkarte

### 07 - Notfallpatient
- **Pools:** Patient/Begleitperson (3), Terminbuchungsplattform (3), Arztpraxis (3) -- 9 Activities, 3 Message Flows
- **Patient/Begleitperson:** Notfall melden und Symptome schildern, Sofort-Bestaetigung erhalten, Praxis aufsuchen und Erstversorgung erhalten
- **System:** Notfall bewerten und einplanen (Subprozess), Notfallprotokoll anlegen, Patient bestaetigen und Praxis alarmieren
- **Arztpraxis:** Behandlungsraum vorbereiten, Notfallpatienten behandeln, Behandlung dokumentieren und Protokoll abschliessen
- **Datenobjekte:** Symptombeschreibung | **Data Stores:** Terminkalender, Notfallregister

### 08 - Apotheke suchen
- **Pools:** Patient (5, 2 Gateways), Gesundheitsplattform (5, 3 Gateways), Apotheke (2, 1 Gateway) -- 12 Activities, 8 Message Flows
- **Patient:** Apothekensuche mit Kriterien starten, Apotheke aus Ergebnisliste auswaehlen, Apothekendetails einsehen, Reservierung beauftragen, Apotheke aufsuchen
- **Gesundheitsplattform:** Apothekensuche durchfuehren, Hinweis senden (keine Ergebnisse), Verfuegbarkeit pruefen und Ergebnisse senden, Apothekendetails bereitstellen, Reservierung vermitteln
- **Apotheke:** Medikamentenverfuegbarkeit rueckmelden, Medikament reservieren und bestaetigen
- **Data Stores:** Apothekendatenbank, Medikamentenbestand, Lagerbestand
- *Hinweis: Plattform-Pool hier als "Gesundheitsplattform" benannt -- vor Abgabe mit den anderen Diagrammen (Terminbuchungsplattform) vereinheitlichen.*

### 09 - Post-Termin (aus Arztsicht)
- **Pools:** Arzt (3, 4 Gateways), Terminbuchungsplattform (4, 2 Gateways), Patient (3, 4 Gateways) -- 10 Activities, 4 Message Flows
- **Arzt:** Behandlung dokumentieren, Rezept erstellen und freigeben, Folgetermin anordnen
- **System:** Patientenakte aktualisieren, Rezept verarbeiten und versenden, Rueckmeldungen verarbeiten, Zusammenfassung versenden
- **Patient:** Zusammenfassung pruefen, Folgetermin waehlen, Feedback geben
- **Datenobjekte:** Diagnosebericht, Rezept | **Data Stores:** Patientendatenbank, Rezeptdatenbank, Terminkalender

### 10 - Patientenregistrierung / Erstanmeldung
- **Pools:** Patient (3), Terminbuchungsplattform (5), Arztpraxis (2) -- 10 Activities, 6 Message Flows
- **Patient:** Registrierungsformular ausfuellen und absenden, E-Mail-Adresse verifizieren, Kontofreigabe entgegennehmen
- **System:** Registrierung validieren und speichern (Subprozess), Verifizierungs-E-Mail senden, Verifizierung empfangen, Freigabe bei Arztpraxis anfordern, Konto aktivieren und Willkommensnachricht senden
- **Arztpraxis:** Patientendaten und Versicherung pruefen, Freigabe erteilen
- **Datenobjekte:** Patientenstammdaten | **Data Stores:** Patientendatenbank

### 11 - Rezept verlaengern / Folgerezept (Reserve)
- **Pools:** Patient (2), Terminbuchungsplattform (4), Arzt (2), Apotheke (2) -- 10 Activities, 5 Message Flows
- **Patient:** Folgerezept fuer Medikament anfordern, Digitales Rezept empfangen und speichern
- **System:** Anfrage pruefen (Intervall und Historie, Subprozess), Anfrage an Arzt weiterleiten, Arztentscheidung verarbeiten und Rezept erstellen, Rezept an Patient und Apotheke senden
- **Arzt:** Patientenakte und Medikamentenhistorie pruefen, Rezept genehmigen und digital signieren
- **Apotheke:** Rezept pruefen und Medikament bereitstellen, Medikament reservieren und Abholung melden
- **Datenobjekte:** Patientennotiz, Signiertes Rezept | **Data Stores:** Rezeptdatenbank

### 12 - Videosprechstunde buchen & durchfuehren (Reserve)
- **Pools:** Patient (4), Terminbuchungsplattform (4), Arzt (3) -- 11 Activities, 7 Message Flows
- **Patient:** Videosprechstunde buchen und Anliegen beschreiben, Technikcheck durchfuehren, An Videositzung teilnehmen, Zusammenfassung und Dokumente empfangen
- **System:** Buchung verarbeiten und Zugangslink senden, Videositzung bereitstellen und ueberwachen, Nachbereitung verarbeiten (Subprozess), Zusammenfassung und Dokumente an Patient senden
- **Arzt:** Patientenakte und Anliegen einsehen, Videokonsultation durchfuehren, Befund dokumentieren und Folgedokumente erstellen (Subprozess)
- **Datenobjekte:** Anliegenbeschreibung, Befundbericht | **Data Stores:** Terminkalender, Patientendatenbank

---

## BPMN-Pruefcheckliste

Die vollstaendige Pruefcheckliste befindet sich in der separaten Datei `BPMN_Checkliste.md`
(Kategorien A-J: Struktur, Aktivitaeten, Ereignisse, Gateways, Swimlanes, Nachrichtenfluesse, Datenobjekte, Kontrollfluss, Subprozesse, Qualitaet/Konsistenz; inkl. Kurzanleitung zur Groessenreduktion).

---

## UML-Modellierung

### Use-Case-Diagramm (erledigt)
- Datei: `UseCase-Diagramm.drawio` (draw.io; fuer Abgabe in Visual Paradigm nachbauen)
- Darstellung als beispielhafter Patienten-Durchlauf, **12 Use Cases** (Anforderung >=10 erfuellt), natuerlich formuliert
- Akteure: Patient (primaer), Praxisteam, Arzt, Facharzt, Apotheke
- Use Cases: Online anmelden, Termin finden und buchen, Termin verschieben, Termin absagen, An den Termin erinnert werden, In der Praxis einchecken, Untersuchung und Diagnose erhalten, An einen Facharzt ueberwiesen werden, Rezept ausgestellt bekommen, Medikament in der Apotheke abholen, Folgetermin vereinbaren, Per Videosprechstunde beraten lassen
- Enthaltene Relationen: Mitwirkung, «include» (Verschieben enthaelt Buchen), «extend» (Absagen→Erinnerung, Folgetermin→Untersuchung), Vererbung (Videosprechstunde ist Spezialfall der Untersuchung); Systemgrenze vorhanden

### Use-Case-Syntaxregeln (laut Betreuer-Blatt)

Grundlagen:
- Verhaltensdiagramm; zeigt die aus Perspektive eines oder mehrerer Akteure bereitgestellten Anwendungsfaelle eines Systems
- "Anwendungsfall-Diagramm", 1. Schritt der OOA-Modellierung; Sammlung identifizierbarer Benutzungsmoeglichkeiten
- Fokus: prozessbasiertes System aus Sicht des Endbenutzers (hoher Abstraktionsgrad)
- **WICHTIG: Ein Use-Case-Diagramm stellt KEINEN Kontrollfluss oder Datenfluss dar -- keine Festlegung der Reihenfolge von Use-Cases!**
- Use-Cases werden spaeter durch UML-Interaktionsdiagramme (Sequenzdiagramme) verfeinert
- Mindestens 10 Use Cases

Elemente:
- **Use Case** = Ellipse (elementarer Geschaeftsprozess)
- **Akteur** = Strichmaennchen (beteiligte Person und/oder Rolle)
- **Systemgrenze** = Kasten um die Use Cases (optional)

Beziehungen:
- **Mitwirkung** (Assoziation): einfache Linie -- ein oder mehrere Akteure nehmen an einem Use-Case teil (Akteur ↔ Use-Case)
- **Komposition / «include»**: gestrichelter Pfeil -- ein Use-Case umfasst einen anderen vollstaendig ("enthaelt"; der eingebundene ist ein Subprozess)
- **Erweiterung / «extend»**: gestrichelter Pfeil -- ein Use-Case erweitert einen anderen logisch (optional, "folgt auf" / "kommt nach diesem"; ausdruecklich KEINE Vererbung)
- **Vererbung** (Generalisierung): Pfeil mit hohlem Dreieck -- ein Use-Case ist ein Spezialfall eines anderen
- Relationen moeglich zwischen Use-Case ↔ Use-Case sowie Akteur ↔ Akteur

### Klassendiagramm (erledigt)
- Datei: `Klassendiagramm.drawio` (draw.io; fuer Abgabe in Visual Paradigm nachbauen)
- **15 Klassen** (Anforderung >=10 erfuellt), aus BPMN-Datenobjekten/-speichern und Akteuren abgeleitet
- Klassen: Person (abstrakt), Patient, Arzt, Facharzt, Arztpraxis, Terminkalender, Termin, Ueberweisung, Patientenakte, Rezept, Medikament, Apotheke, Benachrichtigung, Videositzung, Notfallprotokoll
- Jede Klasse mit Attributen und (wo sinnvoll) Methoden
- Beziehungen: Vererbung (Patient/Arzt → Person, Facharzt → Arzt), 14 Assoziationen mit Multiplizitaeten (z.B. Patient 1 -- * Termin, Arztpraxis 1 -- 1 Terminkalender, Rezept * -- 1 Medikament)

### Noch offen (UML)
- 5 Sequenzdiagramme (je als Unterdiagramm des zugehoerigen Use Case). Ausgewaehlt nach Akteur-Vielfalt, UML-Fragmenten und Klassenabdeckung:
  1. Termin suchen & buchen (01) -- ERLEDIGT (Sequenzdiagramm-01-TerminBuchen.drawio); Patient/System/Terminkalender/Termin/Arztpraxis; alt-Fragment (Slot frei/vergeben); mit Nachrichten-Nummerierung, Aktivierungsbalken, unterstrichenen Objektnamen, = wert-Rueckgaben (syntaxkonform)
  2. Patient ueberweisen (05) -- ERLEDIGT (Sequenzdiagramm-05-PatientUeberweisen.drawio); 6 Lifelines (Hausarzt/System/Ueberweisung/Terminkalender/Patient/Facharzt); par-Fragment; mit Nachrichten-Nummerierung, Aktivierungsbalken, unterstrichenen Objektnamen, = wert-Rueckgaben (syntaxkonform)
  3. Post-Termin / Behandlung & Rezept (09) -- ERLEDIGT (03-PostTermin.drawio); Arzt/System/Patientenakte/Rezept/Patient; alt-Fragment (Rezept) + 2 opt-Fragmente (Folgetermin, Feedback); mit Nachrichten-Nummerierung, Aktivierungsbalken, unterstrichenen Objektnamen, = wert-Rueckgaben (syntaxkonform)
  4. Apotheke suchen (08) -- ERLEDIGT (Sequenzdiagramm-08-ApothekeSuchen.drawio); Patient/System/Apothekendatenbank/Apotheke; alt-Fragment (gefunden/keine) + opt-Fragment (Reservierung), verfuegbarkeitPruefen()/reservieren()
  5. Patientenregistrierung (10) -- ERLEDIGT (Sequenzdiagramm-10-Patientenregistrierung.drawio); Patient/System/Patientendatenbank/Arztpraxis; loop-Fragment (erneute Dateneingabe) + alt-Fragment (Freigabe/Ablehnung), Selbstaufruf datenPruefen()
  - Alle 5 Sequenzdiagramme erledigt.
  - Ersatzkandidaten (nicht umgesetzt): Notfallpatient (07), Videosprechstunde (12)
- Finale Abgabe als VPP-Datei aus Visual Paradigm (alle UML-Diagramme liegen als draw.io-Entwuerfe vor und sind in Visual Paradigm nachzubauen)
---

## Abgaben & Aufgaben -- Uebersicht

### Fortschritt Gesamtuebersicht

| Artefakt                       | Anzahl | Erledigt | Offen |
|--------------------------------|--------|----------|-------|
| BPMN-Kollaborationsdiagramme   | 10     | 10       | 0     |
| Use-Case-Diagramm              | 1      | 1        | 0     |
| Klassendiagramm                | 1      | 1        | 0     |
| Sequenzdiagramme               | 5      | 5        | 0     |
| Projektdokumentation (20 S.)   | 1      | 0        | 1     |
| Abschlusspraesentation         | 1      | 0        | 1     |
| Abgabe-ZIP                     | 1      | 0        | 1     |
| **Gesamt**                     | **20** | **17**   | **3** |

> Zusaetzlich liegen 2 Reserve-Diagramme (11, 12) final vor.

### 1. BPMN-Modellierung (Gewicht: 15%, gruppenbasiert)
- 10 BPMN-Kollaborationsdiagramme, durchschnittlich je 10 Aktivitaeten
- Beteiligte Ressourcen (Lanes/Pools) und Datenobjekte/Speicher modellieren
- Werkzeug: Camunda Modeler oder BPMN.io
- Repository: Camunda.io Cloud
- Abgabeformat: ZIP aller exportierten Diagramme -- Dateiname: `BPMN-<KURS>-<GRUPPE>.zip`
- **Status:** Alle 12 Diagramme final; vor Abgabe Dateinamen vereinheitlichen und ins Repository ablegen

### 2. OO-Modellierung / UML (Gewicht: 15%, gruppenbasiert)
- Use-Case-Diagramm mit mindestens 10 Use-Cases
- UML-Klassendiagramm mit mindestens 10 Klassen
- 5 UML-Sequenzdiagramme (je eines pro ausgewaehltem Use-Case, als Unterdiagramm)
- Werkzeug: Visual Paradigm; Repository: VP-Server (nur ueber DHBW-Netz / Lehre-VPN)
- Abgabeformat: VPP-Datei -- Dateiname: `UML-<KURS>-<GRUPPE>.vpp`
- **Status:** Offen

### 3. Projektdokumentation (Gewicht: 5%, gruppenbasiert)
- Umfang: ca. 20 Seiten, PDF
- Inhalt: Mitglieder, Projektbeschreibung, Vorgehen, Projektmanagement, Artefakte-Ueberblick, Probleme, Feedback
- Dateiname: `Projekt-<KURS>-<GRUPPE>.pdf`
- **Status:** Offen

### 4. Abschlusspraesentation (Gewicht: 10%, individuell bewertet)
- Dauer: 15-20 Minuten, alle Gruppenmitglieder muessen mitwirken
- PDF-Export mit Angabe, wer welchen Teil verantwortet hat
- Termin: Mitte Oktober 2026
- **Status:** Offen

### 5. Engagement (Gewicht: 5%, individuell)
- Bewertung ueber das gesamte Semester hinweg -- **Status:** Laufend

### 6. Projektmanagement (Gewicht: 50%)
- Separate Modul-Unit, zusammen mit LV "Projektmanagement" bewertet (50:50)

---

## Abgabeformat

- Alles in ein ZIP-File: `Fallstudie-<KURS>-<GRUPPE>.zip`
- Elektronische Einreichung ueber Moodle-Upload-Link
- Frist: **13.11.2026, 23:59 Uhr**
- Pro Person muss eine individuelle, archivierbare Version in Moodle vorliegen

---

## Eingesetzte Software

| Werkzeug          | Zweck                        | Zugang                          |
|-------------------|------------------------------|---------------------------------|
| Camunda Modeler   | BPMN-Modellierung            | Lokal / BPMN.io                 |
| Camunda.io Cloud  | BPMN-Repository              | Ohne VPN nutzbar                |
| Visual Paradigm   | UML-Modellierung             | Lizenz ueber Projektgruppenleiter |
| VP-Server         | UML-Repository               | Nur im DHBW-Netz / Lehre-VPN   |

---

## Naechste Schritte (priorisiert, Stand 06.10.2026)

**Zeitrahmen:** Abschlusspraesentation Mitte Oktober 2026 (ca. 1-2 Wochen), vollstaendige Abgabe 13.11.2026.

| Prio | Aufgabe | Zustaendig | Hinweis |
|------|---------|-----------|---------|
| 1 | Abschlusspraesentation vorbereiten + proben | alle | Termin Mitte Oktober, individuelle Bewertung |
| 2 | Alle UML-Diagramme in Visual Paradigm nachbauen und als VPP exportieren | Thilo, Helen | UML komplett als draw.io-Entwurf: Use-Case, Klassendiagramm, 5 Sequenzdiagramme |
| 3 | BPMN-Dateinamen vereinheitlichen + Pool-Benennung 08 angleichen + Camunda-Ablage/Export | Lennart | schnell, blockt Abgabe |
| 4 | Projektdokumentation (~20 S.) finalisieren | Luca | Material groesstenteils in KI.md vorhanden |
| 5 | Abgabe-ZIP buendeln + Moodle-Upload | alle | Deadline 13.11., 23:59 |

**Ableitungshilfe UML aus BPMN:**
- Akteure: Patient, Arzt/Hausarzt, Facharzt, Praxismitarbeiter, Apotheke
- Use Cases (12 aus Prozessen): Registrieren, Termin suchen & buchen, Termin absagen, Termin verschieben, Terminerinnerung erhalten, Check-in, Notfall melden, Patient ueberweisen, Apotheke suchen, Folgerezept anfordern, Videosprechstunde buchen, Post-Termin-Nachbereitung
- Klassenkandidaten (aus Datenobjekten/Data Stores): Patient, Arzt, Facharzt, Praxis, Termin, Terminkalender, Rezept, Medikament, Apotheke, Ueberweisung, Patientenakte, Benachrichtigung, Notfallprotokoll, Videositzung, Benutzerkonto

---

## Aenderungsprotokoll

| Datum      | Aenderung                                                    |
|------------|--------------------------------------------------------------|
| 01.09.2026 | Initiale Erstellung auf Basis von Ablauf-Dokument und Pitch  |
| 01.09.2026 | Rollenverteilung korrigiert (Luca: Scrum Master, Niha: KI-Manager); Fabian ohne Rolle |
| 01.09.2026 | Datei umbenannt von Fallstudie.md zu KI_Luca.md |
| 08.09.2026 | BPMN-Diagramme 01-12 erstellt inkl. Detailaufbau |
| 08.09.2026 | BPMN-Pruefcheckliste erstellt (Vorlesungsfolien 03, 04, 04.02); separate Datei BPMN_Checkliste.md |
| 08.09.2026 | Betreuer-Feedback: zu komplex, Richtwert 10 Activities (+/-2), Onboarding-Fokus, Details via Subprozesse abstrahieren |
| 08.09.2026 | Alle Diagramme nach Feedback ueberarbeitet (reduziert, Subprozesse, einheitliche Benennung, Happy-Path) |
| 08.09.2026 | Alle 12 finalen BPMN-Dateien eingespielt; alte/defekte Duplikate entfernt; Detailaufbau an aktuellen Stand angepasst; Datei umbenannt von KI_Luca.md zu KI.md |
| 06.10.2026 | Naechste Schritte priorisiert (Praesentation + UML zeitkritisch) |
| 06.10.2026 | Use-Case-Diagramm in draw.io erstellt (5 Akteure, 12 Use Cases, include/extend) -- UseCase-Diagramm.drawio |
| 06.10.2026 | Use-Case-Diagramm neu als Patienten-Durchlauf (Journey) gestaltet: Online registrieren -> buchen -> verschieben -> Erinnerung -> Check-In -> Behandlung/Diagnose -> Facharzt-Ueberweisung -> Rezept -> Apotheke; nummerierter Ablauf |
| 06.10.2026 | Use-Case-Diagramm erweitert auf 12 Use Cases (= alle 12 BPMNs, 1:1): Hauptpfad + Zweige (Verschieben/Absagen optional, Notfall/Videosprechstunde alternativ, Folgerezept Folgebesuch) |
| 06.10.2026 | Use-Case-Diagramm final: natuerlich formulierte Patient-Journey mit 12 Use Cases (10 Hauptschritte + 2 optionale/alternative), ohne starre 1:1-BPMN-Beschriftung |
| 12.10.2026 | Use-Case-Diagramm an UML-Syntaxregeln (Betreuer-Blatt) angepasst: nummerierte Ablaufpfeile entfernt (Use-Case zeigt KEINEN Kontrollfluss/Reihenfolge); nur Mitwirkung, «include», «extend», Vererbung; Systemgrenze; 12 Use Cases |
| 12.10.2026 | KI.md aktualisiert: neuer Abschnitt UML-Modellierung inkl. Use-Case-Syntaxregeln; Stand-Datum auf 12.10.2026 |
| 12.10.2026 | Klassendiagramm in draw.io erstellt (15 Klassen, Vererbung + 14 Assoziationen mit Multiplizitaeten) -- Klassendiagramm.drawio |
| 12.10.2026 | Sequenzdiagramm 1/5 erstellt: Termin suchen & buchen (5 Lifelines, 14 Nachrichten, alt-Fragment) -- Sequenzdiagramm-01-TerminBuchen.drawio |
| 12.10.2026 | Sequenzdiagramm 2/5 erstellt: Patient ueberweisen (6 Lifelines, par-Fragment, Selbstaufruf patientenakteAnlegen) -- Sequenzdiagramm-05-PatientUeberweisen.drawio |
| 12.10.2026 | Fehlenden Download-Link zu Sequenzdiagramm 2 nachgereicht; Naechste-Schritte-Tabelle aktualisiert (Use-Case/Klassendiagramm erledigt, Sequenzdiagramm-Auswahl 09/08/10 offen) |
| 12.10.2026 | Sequenzdiagramm 3/5 erstellt: Post-Termin / Behandlung & Rezept (5 Lifelines, alt-Fragment Rezept + opt Folgetermin/Feedback) -- Sequenzdiagramm-09-PostTermin.drawio |
| 12.10.2026 | Sequenzdiagramm 4/5 erstellt: Apotheke suchen (4 Lifelines, alt-Fragment gefunden/keine + opt Reservierung) -- Sequenzdiagramm-08-ApothekeSuchen.drawio |
| 12.10.2026 | Sequenzdiagramm 5/5 erstellt: Patientenregistrierung (4 Lifelines, loop-Fragment + alt-Fragment, Selbstaufruf datenPruefen) -- Sequenzdiagramm-10-Patientenregistrierung.drawio. UML-Modellierung vollstaendig (Use-Case, Klassendiagramm, 5 Sequenzdiagramme) |
| 12.10.2026 | Sequenzdiagramm-Syntaxregeln (Betreuer-Blatt) analysiert: Nummerierung + Aktivierungsbalken fehlten. SD 01 ueberarbeitet (hierarchische Nummerierung, Aktivierungsbalken, unterstrichene Objekte, = wert-Rueckgaben) |
| 12.10.2026 | SD 05 (Patient ueberweisen) syntaxkonform ueberarbeitet (Nummerierung, Aktivierungsbalken, unterstrichene Objekte, = wert-Rueckgaben) |
| 12.10.2026 | SD 09 (Post-Termin, von Gruppe ueberarbeitete Version 03-PostTermin.drawio) syntaxkonform korrigiert (Nummerierung, Aktivierungsbalken, unterstrichene Objekte, = wert); alte Dublette Sequenzdiagramm-09 entfernt |
