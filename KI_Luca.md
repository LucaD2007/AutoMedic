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

### 08 - Apotheke suchen
- **Pools:** Patient, Terminbuchungsplattform, Apotheke
- **Elemente:** 9 + 13 + 6 Aktivitaeten, 4 Gateways, 8 Message Flows
- **Patient (9 Aktivitaeten + 1 Gateway):** Plattform oeffnen und Apothekensuche starten, Standort freigeben und Suchkriterien eingeben, Ergebnisliste erhalten, Apotheken vergleichen (Entfernung, Oeffnungszeiten, Bewertungen), Apotheke auswaehlen, Detailseite einsehen (Adresse, Telefon, Route, Verfuegbarkeit), Medikament reservieren? (XOR: Ja -> Reservierung bestaetigen + Bestaetigung erhalten / Nein -> direkt weiter), Apotheke aufsuchen
- **System (13 Aktivitaeten + 2 Gateways):** Suchanfrage empfangen, Apotheken im Umkreis ermitteln, Apotheken gefunden? (XOR: Nein -> Hinweis, Prozessende / Ja -> Oeffnungszeiten und Entfernung berechnen), Verfuegbarkeitsanfrage an Apotheken senden, Bestandsrueckmeldungen empfangen, Ergebnisliste an Patient senden, Apothekenwahl empfangen, Detaildaten laden, Detailinfos an Patient senden, Reservierung angefragt? (XOR: Ja -> Reservierungsanfrage an Apotheke, Bestaetigung empfangen, an Patient weiterleiten / Nein -> Ende)
- **Apotheke (6 Aktivitaeten + 1 Gateway):** Verfuegbarkeitsanfrage empfangen, Medikamentenbestand pruefen und rueckmelden, Reservierungsanfrage abwarten (Message Intermediate Catch), Reservierungsanfrage empfangen, Medikament noch verfuegbar? (XOR: Nein -> Hinweis nicht verfuegbar, Ende / Ja -> Medikament reservieren und bestaetigen, Reservierung im System vermerken)
- **Datenobjekte:** --
- **Data Stores:** Apothekendatenbank, Medikamentenbestand, Lagerbestand

### 09 - Post-Termin (aus Arztsicht)
- **Pools:** Arzt, Terminbuchungsplattform, Patient
- **Elemente:** 9 + 14 + 7 Aktivitaeten, 7 Gateways, 9 Message Flows
- **Arzt (9 Aktivitaeten + 4 Gateways):** Behandlung dokumentieren, Behandlungsdaten im System erfassen, Diagnose und Massnahmen festhalten (Datenobjekt: Diagnosebericht), Rezept erforderlich? (XOR: Ja -> Rezept erstellen + digital signieren und freigeben (Datenobjekt: Rezept)), Folgeaktion noetig? (XOR: Folgetermin -> Folgetermin anordnen / Ueberweisung -> Ueberweisung ausstellen / Keine -> weiter), Folgeaktion im System erfassen, Patientenakte abschliessen
- **System (14 Aktivitaeten + 4 Gateways):** Behandlungsdaten empfangen, Patientenakte aktualisieren, Diagnose und Massnahmen speichern, Abrechnungsdaten vorbereiten, Rezeptfreigabe empfangen, Rezept validieren und speichern, Rezept digital an Patient senden, Apotheke vorgemerkt? (XOR: Ja -> Rezept an Apotheke weiterleiten / Nein -> weiter), Folgeaktion empfangen, Art der Folgeaktion? (XOR: Folgetermin -> Freie Termine ermitteln + Vorschlaege an Patient + Terminwahl empfangen und buchen / Ueberweisung -> Ueberweisung erstellen und speichern / Keine -> weiter), Zusammenfassung an Patient senden, Feedback-Anfrage senden, Feedback empfangen und speichern
- **Patient (7 Aktivitaeten + 1 Gateway):** Zusammenfassung einsehen, Digitales Rezept empfangen, Rezept pruefen und speichern, Folgetermin-Vorschlaege erhalten, Folgetermin auswaehlen und bestaetigen, Feedback-Anfrage erhalten, Feedback geben? (XOR: Ja -> Feedback verfassen und absenden / Nein -> Ende)
- **Datenobjekte:** Diagnosebericht, Rezept
- **Data Stores:** Patientendatenbank, Rezeptdatenbank, Terminkalender

### 10 - Patientenregistrierung / Erstanmeldung
- **Pools:** Patient, Terminbuchungsplattform, Arztpraxis
- **Elemente:** 10 + 12 + 5 Aktivitaeten, 5 Gateways, 7 Message Flows
- **Patient (10 Aktivitaeten + 1 Gateway):** Plattform oeffnen, Registrierungsformular aufrufen, Persoenliche Daten eingeben (Datenobjekt: Stammdaten), Versicherungsinformationen eingeben (Datenobjekt: Versicherungskarte), Datenschutzerklaerung lesen und akzeptieren, Registrierung absenden, Verifizierungs-E-Mail empfangen, Verifizierungslink anklicken, Auf Freigabe warten, Freigabe erhalten? (XOR: Ja -> Willkommensnachricht erhalten, Profil einsehen / Nein -> Hinweis auf fehlende Daten, Daten korrigieren und erneut einreichen)
- **System (12 Aktivitaeten + 3 Gateways):** Registrierungsdaten empfangen, Pflichtfelder vollstaendig? (XOR: Nein -> Fehlermeldung an Patient, Ende / Ja -> weiter), Duplikatpruefung (Konto existiert bereits?), XOR: Ja -> Hinweis an Patient, Ende / Nein -> weiter, Benutzerkonto anlegen, Verifizierungs-E-Mail generieren und senden, Verifizierung empfangen, E-Mail-Adresse als verifiziert markieren, Versicherungsdaten zur Pruefung an Praxis weiterleiten, Freigabe von Praxis empfangen, Konto aktivieren, Willkommensnachricht an Patient senden
- **Arztpraxis (5 Aktivitaeten + 1 Gateway):** Registrierungsanfrage empfangen, Patientendaten und Versicherung pruefen, Daten plausibel? (XOR: Ja -> Patient freigeben und Bestaetigung an System / Nein -> Ablehnungsgrund an System mit Hinweis auf fehlende/fehlerhafte Daten), Patientenakte im Praxissystem anlegen
- **Datenobjekte:** Stammdaten, Versicherungskarte, Datenschutzerklaerung
- **Data Stores:** Patientendatenbank, Versicherungsdatenbank

### 11 - Rezept verlaengern / Folgerezept (Reserve)
- **Pools:** Patient, Terminbuchungsplattform, Arzt, Apotheke
- **Elemente:** 7 + 12 + 6 + 5 Aktivitaeten, 5 Gateways, 10 Message Flows
- **Patient (7 Aktivitaeten + 1 Gateway):** Plattform oeffnen, Rezepthistorie einsehen, Medikament fuer Folgerezept auswaehlen, Anmerkung hinzufuegen (Datenobjekt: Patientennotiz), Anfrage absenden, Auf Arztentscheidung warten, Entscheidung erhalten -- XOR: Genehmigt -> Digitales Rezept empfangen und speichern / Abgelehnt -> Ablehnungsgrund einsehen und ggf. Termin buchen
- **System (12 Aktivitaeten + 3 Gateways):** Folgerezeptanfrage empfangen, Letztes Rezept und Behandlungshistorie laden, Rezeptintervall gueltig? (XOR: Nein -> Hinweis an Patient dass Termin noetig / Ja -> weiter), Anfrage an Arzt weiterleiten, Arztentscheidung empfangen, Genehmigt? (XOR: Nein -> Ablehnungsgrund an Patient / Ja -> Neues Rezept generieren + digital signiertes Rezept speichern), Rezept an Patient senden, Apotheke gewuenscht? (XOR: Ja -> Rezept an Apotheke weiterleiten + Bestaetigung empfangen + an Patient senden / Nein -> Ende)
- **Arzt (6 Aktivitaeten + 1 Gateway):** Folgerezeptanfrage empfangen, Patientenakte und Medikamentenhistorie pruefen, Medizinisch vertretbar? (XOR: Ja -> Rezept genehmigen + digital signieren (Datenobjekt: Signiertes Rezept) / Nein -> Ablehnung mit Begruendung verfassen), Entscheidung an System uebermitteln
- **Apotheke (5 Aktivitaeten + 1 Gateway):** Rezept empfangen, Rezept auf Gueltigkeit pruefen, Medikament verfuegbar? (XOR: Ja -> Medikament reservieren + Abholbereitschaft melden / Nein -> Alternative vorschlagen + Rueckmeldung an System)
- **Datenobjekte:** Patientennotiz, Signiertes Rezept
- **Data Stores:** Rezeptdatenbank, Patientendatenbank, Medikamentenbestand

### 12 - Videosprechstunde buchen & durchfuehren (Reserve)
- **Pools:** Patient, Terminbuchungsplattform, Arzt
- **Elemente:** 11 + 14 + 8 Aktivitaeten, 6 Gateways, 9 Message Flows
- **Patient (11 Aktivitaeten + 2 Gateways):** Plattform oeffnen, Terminart "Videosprechstunde" auswaehlen, Fachrichtung/Arzt waehlen, verfuegbare Video-Slots einsehen, Termin auswaehlen, Symptome/Anliegen beschreiben (Datenobjekt: Anliegenbeschreibung), Buchung absenden, Bestaetigungsmail mit Zugangslink erhalten, Vor Termin: Technikcheck starten (Kamera/Mikrofon), Technik OK? (XOR: Nein -> Fehlerbehebungshinweise erhalten, erneut pruefen / Ja -> weiter), Videositzung beitreten, Konsultation durchfuehren, Zusammenfassung und ggf. Dokumente empfangen, Feedback geben? (XOR: Ja -> Feedback verfassen / Nein -> Ende)
- **System (14 Aktivitaeten + 3 Gateways):** Buchungsanfrage empfangen, Video-Slot verfuegbar? (XOR: Nein -> Alternativtermine vorschlagen / Ja -> weiter), Termin reservieren, Videoraum-ID generieren, Bestaetigung mit Zugangslink an Patient senden, Arzt ueber Termin benachrichtigen, Zum Terminzeitpunkt: Technikcheck-Anfrage an Patient senden, Ergebnis empfangen, Beiden Teilnehmern Beitritt ermoeglichen, Videositzung starten und ueberwachen, Verbindungsproblem? (XOR: Ja -> Automatische Wiederverbindung versuchen / Nein -> weiter), Sitzungsende registrieren, Nachbereitung vom Arzt empfangen, Dokumente an Patient senden, Feedback-Anfrage senden, Feedback speichern
- **Arzt (8 Aktivitaeten + 1 Gateway):** Terminbenachrichtigung erhalten, Patientenakte und Anliegenbeschreibung einsehen, Videositzung beitreten, Konsultation durchfuehren, Befund dokumentieren (Datenobjekt: Befundbericht), Folgedokumente noetig? (XOR: Rezept -> Rezept digital ausstellen / Ueberweisung -> Ueberweisung erstellen / Krankschreibung -> AU-Bescheinigung erstellen / Keine -> weiter), Dokumente an System uebermitteln, Sitzung beenden
- **Datenobjekte:** Anliegenbeschreibung, Zugangslink, Befundbericht, Rezept/Ueberweisung/AU
- **Data Stores:** Terminkalender, Patientendatenbank, Videositzungsprotokoll
| 07  | Notfallpatient                         | Entwurf    | 3 Pools (Patient/Begleitperson, System, Praxis). Notfall-Triage, Terminverschiebung regulaerer Patienten, Notfallprotokoll, Parallele Benachrichtigung. |
| 08  | Apotheke suchen                        | Entwurf    | 3 Pools (Patient, System, Apotheke). Standortbasierte Suche, Medikamentenverfuegbarkeit, optionale Reservierung. |
| 09  | Post-Termin (aus Arztsicht)            | Entwurf    | 3 Pools (Arzt, System, Patient). Behandlungsdokumentation, digitale Rezeptfreigabe, Folgetermin/Ueberweisung (3-Wege-Gateway), Feedback, Abrechnung. |
| 10  | Patientenregistrierung / Erstanmeldung | Entwurf    | 3 Pools (Patient, System, Arztpraxis). Kontoerstellung, Versicherungsdaten, E-Mail-Verifizierung, Datenschutz, Praxis-Freigabe. |
| 11  | Rezept verlaengern / Folgerezept       | Entwurf    | 4 Pools (Patient, System, Arzt, Apotheke). Folgerezeptanforderung ohne Termin, Arztpruefung, digitale Signatur, optionale Apothekenweiterleitung. |
| 12  | Videosprechstunde buchen & durchfuehren| Entwurf    | 3 Pools (Patient, System, Arzt). Online-Terminbuchung, Technikcheck, Videositzung, digitale Nachbereitung (Rezept/Ueberweisung/Krankschreibung). |

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
| 01.09.2026 | Datei umbenannt von Fallstudie.md zu KI_Luca.md |
