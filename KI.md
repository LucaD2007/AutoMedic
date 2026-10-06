# Fallstudie -- Digitale Terminbuchungsplattform fuer Arztpraxen

**Modul:** Methoden der Wirtschaftsinformatik (3. Semester)
**Stand:** 08. September 2026
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

## Abgaben & Aufgaben -- Uebersicht

### Fortschritt Gesamtuebersicht

| Artefakt                       | Anzahl | Erledigt | Offen |
|--------------------------------|--------|----------|-------|
| BPMN-Kollaborationsdiagramme   | 10     | 10       | 0     |
| Use-Case-Diagramm              | 1      | 0        | 1     |
| Klassendiagramm                | 1      | 0        | 1     |
| Sequenzdiagramme               | 5      | 0        | 5     |
| Projektdokumentation (20 S.)   | 1      | 0        | 1     |
| Abschlusspraesentation         | 1      | 0        | 1     |
| Abgabe-ZIP                     | 1      | 0        | 1     |
| **Gesamt**                     | **20** | **10**   | **10**|

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

## Naechste Schritte

1. BPMN-Dateinamen vereinheitlichen (ohne Umlaute/Suffixe) und Pool-Benennung in Diagramm 08 angleichen
2. BPMN-Diagramme ins Camunda Cloud Repository ablegen
3. UML-Modelle erstellen (Use Cases, Klassendiagramme, 5 Sequenzdiagramme) in Visual Paradigm
4. Projektdokumentation anfertigen (ca. 20 Seiten)
5. Abschlusspraesentation vorbereiten
6. Alles im Abgabe-ZIP buendeln und in Moodle hochladen

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
