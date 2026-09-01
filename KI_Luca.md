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
| 02  | Termin absagen                         | Offen      |                                                      |
| 03  | (Regelmaessige) Terminbenachrichtigung | Offen      |                                                      |
| 04  | Termin verschieben                     | Offen      |                                                      |
| 05  | Patient ueberweisen                    | Offen      |                                                      |
| 06  | Check-In beim Arzt                     | Offen      |                                                      |
| 07  | Notfallpatient                         | Offen      |                                                      |
| 08  | Apotheke suchen                        | Offen      |                                                      |
| 09  | Post-Termin (aus Arztsicht)            | Offen      | inkl. Rezept freigeben                               |
| 10  | (noch offen)                           | Offen      |                                                      |

---

## Abgaben & Aufgaben -- Uebersicht

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
