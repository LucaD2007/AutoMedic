# Fallstudie -- Digitale Terminbuchungsplattform fuer Arztpraxen

**Modul:** Methoden der Wirtschaftsinformatik (3. Semester)
**Stand:** 01. September 2026
**Abgabefrist:** 13.11.2026, 23:59 Uhr (Moodle-Upload)
**Abschlusspraesentation:** Mitte Oktober 2026

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

## Prozesse (10 BPMN-Kollaborationsdiagramme)

| Nr. | Prozess                                | Status     | Anmerkungen                                          |
|-----|----------------------------------------|------------|------------------------------------------------------|
| 01  | Termin suchen & buchen                 | Entwurf    | inkl. Notiz (Was soll gemacht werden), Geraete reservieren/entbuchen. BPMN-Diagramm erstellt (3 Pools: Patient, System, Praxis). |
| 02  | Termin absagen                         | Entwurf    | 3 Pools (Patient, System, Praxis). Stornierungspruefung, Geraete entbuchen, Wartelisten-Nachruecker. |
| 03  | (Regelmaessige) Terminbenachrichtigung | Entwurf    | 3 Pools (System, Patient, Praxis). Timer-Start, 3-Wege-Gateway (Bestaetigung/Absage/Keine Reaktion), Tagesbericht an Praxis. |
| 04  | Termin verschieben                     | Entwurf    | 3 Pools (Patient, System, Praxis). Fristpruefung, Alternativtermine, Geraete umbuchen, Abbruchpfad. |
| 05  | Patient ueberweisen                    | Entwurf    | 4 Pools (Hausarzt, System, Patient, Facharzt). Facharztsuche, Parallele Benachrichtigung (Parallel Gateway), Ueberweisungsdatenbank. |
| 06  | Check-In beim Arzt                     | Entwurf    | 3 Pools (Patient, System, Praxis). Digitaler Check-In, Versichertenkartenpruefung, Wartezimmermanagement, Notfallpriorisierung. |

---

## Detailaufbau der BPMN-Diagramme

### 01 - Termin suchen & buchen
- **Pools:** Patient, Terminbuchungsplattform, Arztpraxis
- **Elemente:** 8 + 10 + 3 Aktivitaeten, 3 Gateways, 6 Message Flows
- **Patient (8 Aktivitaeten):** Plattform aufrufen, Fachrichtung waehlen, Termine suchen, Termin auswaehlen, Notiz eingeben, Buchung bestaetigen, Bestaetigung erhalten
- **System (10 Aktivitaeten + 3 Gateways):** Suchanfrage verarbeiten, Verfuegbarkeit pruefen (XOR: verfuegbar?), Termine anzeigen, Buchung empfangen, Termin reservieren, Geraete benoetigt? (XOR), Geraete reservieren, Notiz speichern, Bestaetigung an Patient, Benachrichtigung an Praxis
- **Arztpraxis (3 Aktivitaeten):** Benachrichtigung pruefen, Kalender einsehen, Vorbereitung planen
- **Datenobjekte:** Notiz (Was soll gemacht werden)
- **Data Stores:** Terminkalender, Geraeteverwaltung

### 02 - Termin absagen
- **Pools:** Patient, Terminbuchungsplattform, Arztpraxis
- **Elemente:** 7 + 8 + 3 Aktivitaeten, 4 Gateways, 6 Message Flows
- **Patient (7 Aktivitaeten + 2 Gateways):** Anmelden, gebuchte Termine einsehen, Termin auswaehlen, Absagegrund angeben (optional), Absage bestaetigen, Bestaetigung erhalten
- **System (8 Aktivitaeten + 3 Gateways):** Absageanfrage verarbeiten, Termin identifizieren, Stornierung moeglich? (XOR), Buchung stornieren, Zeitslot freigeben, Geraete reserviert? (XOR: ja -> Geraete entbuchen), Bestaetigung an Patient, Benachrichtigung an Praxis
- **Arztpraxis (3 Aktivitaeten + 1 Gateway):** Benachrichtigung pruefen, Kalender aktualisieren, Wartelisten-Check (XOR: ja -> Nachruecker informieren)
- **Datenobjekte:** Absagegrund
- **Data Stores:** Terminkalender, Geraeteverwaltung

### 03 - (Regelmaessige) Terminbenachrichtigung
- **Pools:** Terminbuchungsplattform, Patient, Arztpraxis
- **Elemente:** 10 + 5 + 3 Aktivitaeten, 4 Gateways, 2 Timer-Events, 5 Message Flows
- **System (10 Aktivitaeten + 2 Gateways):** Timer-Start (taeglich 08:00), anstehende Termine abfragen (48h), Termine vorhanden? (XOR), Erinnerung generieren, an Patient senden, Versand protokollieren, Wartezeit 12h (Timer Intermediate), Reaktion? (3-Wege-XOR: Bestaetigt/Absage/Keine Reaktion), Bestaetigung speichern / Termin stornieren / Praxis informieren, Tagesbericht an Praxis
- **Patient (5 Aktivitaeten + 1 Gateway):** Erinnerung empfangen (Message Start), Termindetails pruefen, Entscheidung (XOR: Bestaetigen/Absagen/Ignorieren), Systemrueckmeldung erhalten
- **Arztpraxis (3 Aktivitaeten):** Tagesbericht pruefen, Patienten ohne Reaktion nachfassen, Tagesplan anpassen
- **Data Stores:** Terminkalender, Benachrichtigungslog

### 04 - Termin verschieben
- **Pools:** Patient, Terminbuchungsplattform, Arztpraxis
- **Elemente:** 7 + 10 + 3 Aktivitaeten, 4 Gateways, 6 Message Flows
- **Patient (7 Aktivitaeten + 2 Gateways):** Anmelden, Termine einsehen, Termin auswaehlen, Verschiebungsgrund angeben (optional), Anfrage absenden, Verschiebung moeglich? (XOR: Ja -> Alternativtermine erhalten, neuen Termin waehlen, Bestaetigung / Nein -> Hinweis erhalten)
- **System (10 Aktivitaeten + 3 Gateways):** Anfrage empfangen, Termin identifizieren, Fristpruefung (XOR: Nein -> Abbruchmeldung), Alternative Termine suchen, an Patient senden, neuen Terminwunsch empfangen, alten Termin stornieren und Slot freigeben, Geraete reserviert? (XOR: Ja -> Geraete umbuchen), neuen Termin buchen, Bestaetigung an Patient, Praxis benachrichtigen
- **Arztpraxis (3 Aktivitaeten):** Aenderungsbenachrichtigung empfangen (Message Start), Kalender aktualisieren, Ressourcenplanung anpassen
- **Datenobjekte:** Verschiebungsgrund
- **Data Stores:** Terminkalender, Geraeteverwaltung

### 05 - Patient ueberweisen
- **Pools:** Hausarzt, Terminbuchungsplattform, Patient, Facharzt
- **Elemente:** 4 + 12 + 4 + 3 Aktivitaeten, 3 Gateways, 7 Message Flows
- **Hausarzt (4 Aktivitaeten):** Diagnose und Fachrichtung dokumentieren, Ueberweisungsschein erstellen, Ueberweisung im System erfassen, Statusrueckmeldung erhalten
- **System (12 Aktivitaeten + 3 Gateways):** Ueberweisung empfangen und validieren, Fachaerzte im Umkreis suchen, Facharzt verfuegbar? (XOR: Nein -> Patient informieren, Ende), Termine laden, Patient benachrichtigen, Terminvorschlaege senden, Terminwunsch empfangen, Termin buchen, Ueberweisung verknuepfen, Parallel Gateway -> gleichzeitig: Bestaetigung an Patient, Info an Facharzt, Status an Hausarzt -> Parallel Merge
- **Patient (4 Aktivitaeten):** Ueberweisungshinweis erhalten (Message Start), Ueberweisung einsehen, Terminvorschlaege pruefen, Termin waehlen, Bestaetigung erhalten
- **Facharzt (3 Aktivitaeten):** Ueberweisungsdaten pruefen (Message Start), Patientenakte anlegen/aktualisieren, Termin bestaetigen
- **Datenobjekte:** Ueberweisungsschein, Patientenakte
- **Data Stores:** Terminkalender, Ueberweisungsdatenbank

### 06 - Check-In beim Arzt
- **Pools:** Patient, Terminbuchungsplattform, Arztpraxis
- **Elemente:** 6 + 10 + 5 Aktivitaeten, 4 Gateways, 6 Message Flows
- **Patient (6 Aktivitaeten):** In der Praxis eintreffen, Check-In am Terminal/App starten, Versichertenkarte einlesen, Check-In-Bestaetigung und Wartenummer erhalten, geschaetzte Wartezeit einsehen, Aufruf erhalten und zum Behandlungszimmer gehen
- **System (10 Aktivitaeten + 4 Gateways):** Check-In-Anfrage empfangen, Termin fuer heute vorhanden? (XOR: Nein -> Spontanbesuch erfassen), Versichertendaten pruefen, Daten gueltig? (XOR: Nein -> Manuelle Pruefung an Praxis), Patient als anwesend markieren, Warteposition berechnen, Notfall? (XOR: Ja -> Prioritaet hochsetzen), Wartenummer und Wartezeit an Patient senden, Praxis ueber Ankunft benachrichtigen, Aufruf-Signal an Patient weiterleiten
- **Arztpraxis (5 Aktivitaeten + 1 Gateway):** Ankunftsbenachrichtigung erhalten, Patientendaten und Termingrund einsehen, ggf. manuelle Kartenpruefung (aus System-Eskalation), Behandlungsreihenfolge festlegen, Patient aufrufen
- **Datenobjekte:** Versichertenkarte, Wartenummer
- **Data Stores:** Terminkalender, Patientendatenbank

### 07 - Notfallpatient
- **Pools:** Patient/Begleitperson, Terminbuchungsplattform, Arztpraxis
- **Elemente:** 5 + 11 + 7 Aktivitaeten, 4 Gateways, 6 Message Flows
- **Patient/Begleitperson (5 Aktivitaeten):** Notfall melden (telefonisch/vor Ort/App), Symptome schildern, Sofort-Bestaetigung erhalten, Praxis aufsuchen / Einlass erhalten, Erstversorgung erhalten
- **System (11 Aktivitaeten + 3 Gateways):** Notfallmeldung empfangen, Notfall-Triage durchfuehren (Dringlichkeitsstufe bestimmen), Sofort-Eingriff noetig? (XOR: Ja -> Arzt direkt alarmieren / Nein -> Naechsten freien Slot identifizieren), Kapazitaet vorhanden? (XOR: Nein -> Regulaeren Termin verschieben + betroffenen Patienten benachrichtigen / Ja -> Notfallslot einplanen), Notfallprotokoll anlegen (Datenobjekt), Parallel Gateway: Bestaetigung an Patient + Arztpraxis benachrichtigen, Notfall im Kalender als Prioritaet markieren
- **Arztpraxis (7 Aktivitaeten + 1 Gateway):** Notfallalarm empfangen, Dringlichkeit pruefen, Sofort-Fall? (XOR: Ja -> Aktuelle Behandlung unterbrechen/delegieren / Nein -> Wartezimmer-Reihenfolge anpassen), Behandlungsraum vorbereiten, Notfallpatient behandeln, Behandlung dokumentieren, Notfallprotokoll abschliessen
- **Datenobjekte:** Symptombeschreibung, Notfallprotokoll, Triage-Einstufung
- **Data Stores:** Terminkalender, Patientendatenbank, Notfallregister
| 07  | Notfallpatient                         | Entwurf    | 3 Pools (Patient/Begleitperson, System, Praxis). Notfall-Triage, Terminverschiebung regulaerer Patienten, Notfallprotokoll, Parallele Benachrichtigung. |
| 08  | Apotheke suchen                        | Offen      |                                                      |
| 09  | Post-Termin (aus Arztsicht)            | Offen      | inkl. Rezept freigeben                               |
| 10  | (noch offen)                           | Offen      |                                                      |

---

## Abgaben & Aufgaben -- Uebersicht

### Fortschritt Gesamtuebersicht

| Artefakt                       | Anzahl | Erledigt | Offen |
|--------------------------------|--------|----------|-------|
| BPMN-Kollaborationsdiagramme   | 10     | 7        | 3     |
| Use-Case-Diagramm              | 1      | 0        | 1     |
| Klassendiagramm                | 1      | 0        | 1     |
| Sequenzdiagramme               | 5      | 0        | 5     |
| Projektdokumentation (20 S.)   | 1      | 0        | 1     |
| Abschlusspraesentation         | 1      | 0        | 1     |
| Abgabe-ZIP                     | 1      | 0        | 1     |
| **Gesamt**                     | **20** | **7**    | **13**|

### 1. BPMN-Modellierung (Gewicht: 15%, gruppenbasiert)
- 10 BPMN-Kollaborationsdiagramme, durchschnittlich je 10 Aktivitaeten
- Beteiligte Ressourcen (Lanes/Pools) und Datenobjekte/Speicher modellieren
- Werkzeug: Camunda Modeler oder BPMN.io
- Repository: Camunda.io Cloud
- Abgabeformat: ZIP aller exportierten Diagramme
- Dateiname: `BPMN-<KURS>-<GRUPPE>.zip`
- **Status:** Offen

### 2. OO-Modellierung / UML (Gewicht: 15%, gruppenbasiert)
- Use-Case-Diagramm mit mindestens 10 Use-Cases
- UML-Klassendiagramm mit mindestens 10 Klassen
- 5 UML-Sequenzdiagramme (je eines pro ausgewaehltem Use-Case)
- Sequenzdiagramme als Unterdiagramm des zugehoerigen Use-Cases anlegen
- Werkzeug: Visual Paradigm
- Repository: VP-Server (nur ueber DHBW-Netzwerk oder Lehre-VPN erreichbar)
- Abgabeformat: VPP-Datei
- Dateiname: `UML-<KURS>-<GRUPPE>.vpp`
- **Status:** Offen

### 3. Projektdokumentation (Gewicht: 5%, gruppenbasiert)
- Umfang: ca. 20 Seiten, PDF
- Inhalt:
  - Mitglieder der Gruppe
  - Ausfuehrliche Beschreibung des Projekts
  - Vorgehen bei Umsetzung
  - Projektmanagement
  - Ueberblick ueber die erstellten Artefakte
  - Probleme/Herausforderungen
  - Feedback
- Dateiname: `Projekt-<KURS>-<GRUPPE>.pdf`
- **Status:** Offen

### 4. Abschlusspraesentation (Gewicht: 10%, individuell bewertet)
- Dauer: 15-20 Minuten
- Alle Gruppenmitglieder muessen mitwirken
- PDF-Export mit Angabe, wer welchen Teil verantwortet hat
- Termin: Mitte Oktober 2026
- **Status:** Offen

### 5. Engagement (Gewicht: 5%, individuell)
- Bewertung ueber das gesamte Semester hinweg
- **Status:** Laufend

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

1. Geschaeftsprozesse im Detail abgrenzen und mit Coaching verfeinern
2. 10. Prozess festlegen
3. BPMN-Modelle erstellen (Kollaborationsdiagramme mit Automatisierungsfokus)
4. UML-Modelle erstellen (Use Cases, Klassendiagramme, Sequenzdiagramme)
5. Projektdokumentation anfertigen
6. Abschlusspraesentation vorbereiten

---

## Aenderungsprotokoll

| Datum      | Aenderung                                                    |
|------------|--------------------------------------------------------------|
| 01.09.2026 | Initiale Erstellung auf Basis von Ablauf-Dokument und Pitch  |
| 01.09.2026 | Rollenverteilung korrigiert: fachliche Zuordnungen aus Pitch entfernt, aktuelle Rollen eingetragen (Luca: Scrum Master, Niha: KI-Manager) |
| 01.09.2026 | Fabian ist nicht Projektleitung -- Rolle entfernt |
| 01.09.2026 | BPMN-Diagramm 01 (Termin suchen & buchen) als Entwurf erstellt |
| 08.09.2026 | BPMN-Diagramm 02 (Termin absagen) als Entwurf erstellt |
| 08.09.2026 | BPMN-Diagramm 03 (Terminbenachrichtigung) als Entwurf erstellt |
| 08.09.2026 | Fortschritt-Gesamtuebersicht in KI_Luca.md aufgenommen |
| 08.09.2026 | BPMN-Diagramm 04 (Termin verschieben) als Entwurf erstellt |
| 08.09.2026 | BPMN-Diagramm 05 (Patient ueberweisen) als Entwurf erstellt |
| 08.09.2026 | Detailaufbau aller 5 bisherigen BPMN-Diagramme in KI_Luca.md dokumentiert |
| 08.09.2026 | BPMN-Diagramm 06 (Check-In beim Arzt) als Entwurf erstellt inkl. Detailaufbau |
| 08.09.2026 | BPMN-Diagramm 07 (Notfallpatient) als Entwurf erstellt inkl. Detailaufbau |
| 01.09.2026 | Datei umbenannt von Fallstudie.md zu KI_Luca.md |
