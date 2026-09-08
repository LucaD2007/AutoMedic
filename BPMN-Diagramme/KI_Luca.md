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

## Prozesse (10 BPMN-Kollaborationsdiagramme + 2 Reserve)

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

### 01 - Termin s...(argument truncated)
| 07  | Notfallpatient                         | Entwurf    | 3 Pools (Patient/Begleitperson, System, Praxis). Notfall-Triage, Terminverschiebung regulaerer Patienten, Notfallprotokoll, Parallele Benachrichtigung. |
| 08  | Apotheke suchen                        | Entwurf    | 3 Pools (Patient, System, Apotheke). Standortbasierte Suche, Medikamentenverfuegbarkeit, optionale Reservierung. |
| 09  | Post-Termin (aus Arztsicht)            | Entwurf    | 3 Pools (Arzt, System, Patient). Behandlungsdokumentation, digitale Rezeptfreigabe, Folgetermin/Ueberweisung (3-Wege-Gateway), Feedback, Abrechnung. |
| 10  | Patientenregistrierung / Erstanmeldung | Entwurf    | 3 Pools (Patient, System, Arztpraxis). Kontoerstellung, Versicherungsdaten, E-Mail-Verifizierung, Datenschutz, Praxis-Freigabe. |
| 11  | Rezept verlaengern / Folgerezept       | Entwurf    | 4 Pools (Patient, System, Arzt, Apotheke). Folgerezeptanforderung ohne Termin, Arztpruefung, digitale Signatur, optionale Apothekenweiterleitung. |
| 12  | Videosprechstunde buchen & durchfuehren| Entwurf    | 3 Pools (Patient, System, Arzt). Online-Terminbuchung, Technikcheck, Videositzung, digitale Nachbereitung (Rezept/Ueberweisung/Krankschreibung). |

---

## BPMN-Pruefcheckliste (basierend auf Vorlesungsmaterial)

Die folgende Checkliste dient zur systematischen Pruefung und Verbesserung aller BPMN-Diagramme. Quellen: Vorlesung "GP-Orientierte Analyse" (Folien 04/04.02), "Diagrammtechniken" (Folie 03), Aufgabenstellung Fallstudie.

### A. Strukturelle Anforderungen (Aufgabenstellung)

- [ ] Diagramm ist ein **Kollaborationsdiagramm** (mehrere Pools)
- [ ] Durchschnittlich **ca. 10 Aktivitaeten** pro Diagramm
- [ ] **Lanes/Pools** sind vorhanden und sinnvoll eingesetzt
- [ ] **Datenobjekte** (Einzelobjekte) und/oder **Datenspeicher** (Objektmengen) sind modelliert
- [ ] Dateiname folgt Schema: `XX-PraegnanterName` (z.B. `01-TerminSuchenBuchen`)
- [ ] Diagramm oeffnet fehlerfrei in **Camunda Modeler** oder **bpmn.io**
- [ ] Syntaktisch korrektes **BPMN 2.0** XML-Format

### B. Aktivitaeten (Folien 4-17 bis 4-22)

- [ ] **Benennung einheitlich im Infinitiv + Objekt** (z.B. "Termin buchen", "Rezept freigeben" -- NICHT "Termin gebucht" oder "Buchung")
- [ ] **Aktivitaetstypen** korrekt verwendet:
  - [ ] Manuelle Aktivitaet (Mensch, ohne Software)
  - [ ] Benutzer-Aktivitaet (Mensch, mit Software)
  - [ ] Automatisierte Aktivitaet (Software extern)
  - [ ] Regelbasierte Aktivitaet (Geschaeftsregel)
  - [ ] Skript-Aktivitaet (Engine-intern)
  - [ ] Sendende/Empfangende Aktivitaet (Nachricht an anderen Pool)
- [ ] **Granularitaet angemessen**: Jede Aktivitaet hat eine bekannte, abschaetzbare Dauer
  - Manuelle Aktivitaet: von einer Person, ohne Ortswechsel ausfuehrbar
  - Automatisierte Aktivitaet: ohne Wechsel des Anwendungssystems ausfuehrbar
  - Nicht zu grob (z.B. "System verwalten") und nicht zu fein (z.B. "Button klicken")

### C. Ereignisse / Events (Folien 4-24 bis 4-29)

- [ ] **Startereignis** vorhanden (obligatorisch) -- genau eines pro Pool/Prozess
- [ ] **Endereignis** vorhanden (obligatorisch) -- genau eines pro Pool/Prozess (Best Practice)
- [ ] Vermeidung **multipler Start- bzw. Endereignisse** (Best Practice, Folie 4-50/51)
- [ ] Benennung von Ereignissen im **Partizip Perfekt** (z.B. "Nachricht erhalten", "Pruefung beendet")
- [ ] Ereignisse sind **passive Elemente** -- stehen nicht fuer Taetigkeiten
- [ ] Korrekte **Ereignistypen** verwendet:
  - [ ] Nachricht-getriggertes Startereignis (z.B. bei Empfang einer Nachricht aus anderem Pool)
  - [ ] Zeit-getriggertes Startereignis (z.B. Timer fuer regelmaessige Prozesse)
  - [ ] Zwischenereignisse (z.B. Timer, Nachricht) wo sinnvoll
  - [ ] Endereignistypen (Nachricht, Fehler, Signal) wo passend

### D. Gateways (Folien 4-30 bis 4-36)

- [ ] **XOR-Gateway (exklusiv)**: Genau ein Pfad wird gewaehlt
  - [ ] Split: Bedingungen an ausgehenden Kanten beschriftet
  - [ ] Join: Zusammenfuehrung sobald ein Pfad abgeschlossen
- [ ] **AND-Gateway (parallel)**: Alle Pfade werden parallel ausgefuehrt
  - [ ] Split UND Join vorhanden (jeder Split muss einen Join haben)
- [ ] **OR-Gateway (inklusiv)**: Einer oder mehrere Pfade
  - [ ] Verkettung inklusiver Verzweigungen korrekt (Best Practice, Folie 4-52/53)
- [ ] **Ereignisbasiertes Gateway**: Fuer Timeout-/Wettlauf-Situationen
- [ ] Jeder Gateway-Split hat einen passenden **Gateway-Join** (Symmetrie pruefen)

### E. Swimlanes: Pools und Lanes (Folien 4-15)

- [ ] **Pool = ein Unternehmen/Organisation** (z.B. Arztpraxis, Patient, Apotheke)
- [ ] **Lanes = Rollen/Organisationseinheiten innerhalb eines Pools** (z.B. Empfang, Arzt)
- [ ] Kommunikation zwischen Pools **ausschliesslich ueber Nachrichtenfluesse** (Message Flows)
- [ ] Kein Sequenzfluss zwischen verschiedenen Pools
- [ ] Alle Aktivitaeten sind einer Lane/einem Pool zugeordnet
- [ ] "Empty Pool" / Black Box wo interne Logik nicht relevant (Folie 4-44/45)

### F. Nachrichtenfluesse / Message Flows (Folie 4-43/44)

- [ ] Nachrichtenfluesse verbinden **verschiedene Pools** (nie innerhalb eines Pools)
- [ ] Jede gesendete Nachricht hat einen **Empfaenger** im anderen Pool
- [ ] Nachrichtenfluesse sind **beschriftet** (was wird gesendet?)
- [ ] Sendende/Empfangende Aktivitaeten oder Nachrichtenereignisse korrekt verwendet

### G. Datenobjekte und Datenspeicher (Folie 4-49/50)

- [ ] **Datenobjekte** (Einzelobjekte, z.B. Rezept, Ueberweisungsschein) vorhanden wo relevant
- [ ] **Datenspeicher** (Objektmengen, z.B. Terminkalender, Patientendatenbank) vorhanden wo relevant
- [ ] Datenobjekte/Datenspeicher sind mit den richtigen Aktivitaeten **verknuepft** (Datenassoziationen)

### H. Sequenzfluss und Kontrollfluss (Folien 4-23, 4-51/52)

- [ ] **Klare Kantenfuehrung** entlang des Kontrollflusses (keine kreuzenden Linien, Best Practice Folie 4-51/52)
- [ ] Kanten sind immer **gerichtet** (Pfeilrichtung klar)
- [ ] Logischer Fluss von links nach rechts / oben nach unten
- [ ] Keine "toten Enden" (jede Aktivitaet hat ein- und ausgehende Kanten, ausser Start/Ende)
- [ ] Keine unerreichbaren Elemente

### I. Subprozesse und Schleifen (Folien 4-37 bis 4-43)

- [ ] **Lokale Subprozesse** fuer Verfeinerung komplexer Aktivitaeten genutzt (wo sinnvoll)
- [ ] **Call Activities** (globale Subprozesse) fuer wiederverwendbare Prozesse ueber Diagramme hinweg
- [ ] **Schleifen** (Loop) fuer iterative Aktivitaeten eingesetzt
- [ ] **Multi-Instanzen** (sequentiell/parallel) wo mehrere gleichartige Instanzen noetig

### J. Qualitaet und Konsistenz (uebergreifend)

- [ ] Diagramm ist **selbsterklaerend** -- ein Aussenstehender versteht den Prozess
- [ ] Einheitliche **Sprache** (durchgehend Deutsch oder Englisch)
- [ ] Einheitliche **Schreibweise** und Terminologie ueber alle 10 Diagramme hinweg
- [ ] **Automatisierungspotenzial** ist erkennbar (welche Schritte kann Software uebernehmen?)
- [ ] Konsistenz zwischen Diagrammen (z.B. gleiche Pool-Namen, gleiche Datenspeicher-Bezeichnungen)
- [ ] Keine redundanten Aktivitaeten oder ueberfluessigen Elemente

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
| 08.09.2026 | BPMN-Diagramm 08 (Apotheke suchen) als Entwurf erstellt inkl. Detailaufbau |
| 08.09.2026 | BPMN-Diagramm 09 (Post-Termin) als Entwurf erstellt inkl. Detailaufbau |
| 08.09.2026 | Prozesse 10-12 festgelegt: 10 Patientenregistrierung, 11 Rezept verlaengern (Reserve), 12 Videosprechstunde (Reserve) |
| 08.09.2026 | BPMN-Diagramm 10 (Patientenregistrierung) als Entwurf erstellt inkl. Detailaufbau -- Alle 10 BPMN-Diagramme fertig |
| 08.09.2026 | BPMN-Diagramm 11 (Rezept verlaengern) als Reserve-Entwurf erstellt inkl. Detailaufbau |
| 08.09.2026 | BPMN-Diagramm 12 (Videosprechstunde) als Reserve-Entwurf erstellt inkl. Detailaufbau -- Alle 12 BPMN-Diagramme fertig |
| 08.09.2026 | BPMN-Pruefcheckliste erstellt (basierend auf Vorlesungsfolien 03, 04, 04.02) |
| 08.09.2026 | Betreuer-Feedback BPMN 01: zu komplex, Richtwert 10 Activities (+/-2), als Onboarding gedacht, Details ueber Subprozesse abstrahieren |
| 08.09.2026 | Alle 12 BPMN-Diagramme ueberarbeitet: reduziert auf ~10 Activities, Subprozesse eingefuegt, Benennung vereinheitlicht (Infinitiv+Objekt / Partizip Perfekt), Happy-Path-Fokus, ein Start-/Endereignis pro Pool |
| 01.09.2026 | Datei umbenannt von Fallstudie.md zu KI_Luca.md |
