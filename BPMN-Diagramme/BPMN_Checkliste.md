# BPMN-Pruefcheckliste

Systematische Checkliste zur Pruefung und Verbesserung aller BPMN-Diagramme.
Quellen: Vorlesung "GP-Orientierte Analyse" (Folien 04/04.02), "Diagrammtechniken" (Folie 03), Aufgabenstellung Fallstudie, Betreuer-Feedback (08.09.2026).

---

## A. Strukturelle Anforderungen (Aufgabenstellung + Betreuer-Feedback)

- [ ] Diagramm ist ein **Kollaborationsdiagramm** (mehrere Pools)
- [ ] **Ca. 10 Aktivitaeten (+/- 2)** im gesamten Diagramm (ueber alle Pools) -- Richtwert vom Betreuer
- [ ] **Lanes/Pools** sind vorhanden und sinnvoll eingesetzt
- [ ] **Datenobjekte** (Einzelobjekte) und/oder **Datenspeicher** (Objektmengen) sind modelliert
- [ ] Dateiname folgt Schema: `XX-PraegnanterName` (z.B. `01-TerminSuchenBuchen`)
- [ ] Diagramm oeffnet fehlerfrei in **Camunda Modeler** oder **bpmn.io**
- [ ] Syntaktisch korrektes **BPMN 2.0** XML-Format
- [ ] Diagramm funktioniert als **Onboarding-Dokument** -- ein neuer Mitarbeiter versteht den Prozess auf den ersten Blick

## B. Aktivitaeten (Folien 4-17 bis 4-22 + Betreuer-Feedback)

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
- [ ] **Aufeinanderfolgende einfache Activities zusammengefasst**, wenn:
  - Gleiche Person/Rolle fuehrt sie aus
  - Kein Ortswechsel und kein Systemwechsel dazwischen
  - Keine Entscheidung (Gateway) dazwischen noetig
  - Schritte treten immer gemeinsam und in fester Reihenfolge auf
  - Beispiel: "Plattform aufrufen" + "Fachrichtung waehlen" + "Termine suchen" -> "Termin suchen und auswaehlen"

## C. Ereignisse / Events (Folien 4-24 bis 4-29)

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

## D. Gateways (Folien 4-30 bis 4-36)

- [ ] **XOR-Gateway (exklusiv)**: Genau ein Pfad wird gewaehlt
  - [ ] Split: Bedingungen an ausgehenden Kanten beschriftet
  - [ ] Join: Zusammenfuehrung sobald ein Pfad abgeschlossen
- [ ] **AND-Gateway (parallel)**: Alle Pfade werden parallel ausgefuehrt
  - [ ] Split UND Join vorhanden (jeder Split muss einen Join haben)
- [ ] **OR-Gateway (inklusiv)**: Einer oder mehrere Pfade
  - [ ] Verkettung inklusiver Verzweigungen korrekt (Best Practice, Folie 4-52/53)
- [ ] **Ereignisbasiertes Gateway**: Fuer Timeout-/Wettlauf-Situationen
- [ ] Jeder Gateway-Split hat einen passenden **Gateway-Join** (Symmetrie pruefen)

## E. Swimlanes: Pools und Lanes (Folien 4-15)

- [ ] **Pool = ein Unternehmen/Organisation** (z.B. Arztpraxis, Patient, Apotheke)
- [ ] **Lanes = Rollen/Organisationseinheiten innerhalb eines Pools** (z.B. Empfang, Arzt)
- [ ] Kommunikation zwischen Pools **ausschliesslich ueber Nachrichtenfluesse** (Message Flows)
- [ ] Kein Sequenzfluss zwischen verschiedenen Pools
- [ ] Alle Aktivitaeten sind einer Lane/einem Pool zugeordnet
- [ ] "Empty Pool" / Black Box wo interne Logik nicht relevant (Folie 4-44/45)

## F. Nachrichtenfluesse / Message Flows (Folie 4-43/44)

- [ ] Nachrichtenfluesse verbinden **verschiedene Pools** (nie innerhalb eines Pools)
- [ ] Jede gesendete Nachricht hat einen **Empfaenger** im anderen Pool
- [ ] Nachrichtenfluesse sind **beschriftet** (was wird gesendet?)
- [ ] Sendende/Empfangende Aktivitaeten oder Nachrichtenereignisse korrekt verwendet

## G. Datenobjekte und Datenspeicher (Folie 4-49/50)

- [ ] **Datenobjekte** (Einzelobjekte, z.B. Rezept, Ueberweisungsschein) vorhanden wo relevant
- [ ] **Datenspeicher** (Objektmengen, z.B. Terminkalender, Patientendatenbank) vorhanden wo relevant
- [ ] Datenobjekte/Datenspeicher sind mit den richtigen Aktivitaeten **verknuepft** (Datenassoziationen)

## H. Sequenzfluss und Kontrollfluss (Folien 4-23, 4-51/52)

- [ ] **Klare Kantenfuehrung** entlang des Kontrollflusses (keine kreuzenden Linien, Best Practice Folie 4-51/52)
- [ ] Kanten sind immer **gerichtet** (Pfeilrichtung klar)
- [ ] Logischer Fluss von links nach rechts / oben nach unten
- [ ] Keine "toten Enden" (jede Aktivitaet hat ein- und ausgehende Kanten, ausser Start/Ende)
- [ ] Keine unerreichbaren Elemente

## I. Subprozesse und Schleifen (Folien 4-37 bis 4-43 + Betreuer-Feedback)

- [ ] **Details ueber Subprozesse abstrahieren** -- Hauptebene zeigt nur den uebergeordneten Ablauf
- [ ] **Lokale Subprozesse** (collapsed, mit + Marker) fuer Gruppen von 3-5 zusammenhaengenden Activities
  - Beispiel: "Buchung verarbeiten" als Subprozess fuer Termin reservieren + Geraete pruefen + Notiz speichern
- [ ] **Call Activities** (globale Subprozesse) fuer wiederverwendbare Prozesse ueber Diagramme hinweg
- [ ] **Schleifen** (Loop) fuer iterative Aktivitaeten eingesetzt
- [ ] **Multi-Instanzen** (sequentiell/parallel) wo mehrere gleichartige Instanzen noetig
- [ ] **Fehler-/Sonderpfade** in Subprozesse verlagert oder ueber Error-Boundary-Events am Subprozess dargestellt
- [ ] Hauptebene zeigt primaer den **Happy Path** (Normalfall) -- Sonderfaelle stecken in Subprozessen

## J. Qualitaet und Konsistenz (uebergreifend)

- [ ] Diagramm ist **selbsterklaerend** -- ein Aussenstehender versteht den Prozess
- [ ] Einheitliche **Sprache** (durchgehend Deutsch oder Englisch)
- [ ] Einheitliche **Schreibweise** und Terminologie ueber alle 10 Diagramme hinweg
- [ ] **Automatisierungspotenzial** ist erkennbar (welche Schritte kann Software uebernehmen?)
- [ ] Konsistenz zwischen Diagrammen (z.B. gleiche Pool-Namen, gleiche Datenspeicher-Bezeichnungen)
- [ ] Keine redundanten Aktivitaeten oder ueberfluessigen Elemente

---

## Kurzanleitung: Groesse reduzieren (Betreuer-Feedback)

Fuer jedes Diagramm diese Schritte durchgehen:

1. **Zaehlen**: Wie viele Activities hat das Diagramm aktuell (ueber alle Pools)?
2. **Happy Path markieren**: Welche Activities gehoeren zum Normalfall?
3. **Zusammenfassen**: Aufeinanderfolgende Activities gleicher Person/gleiches System ohne Gateway dazwischen buendeln
4. **Subprozesse bilden**: Restliche Gruppen von 3-5 Activities als Subprozess kapseln (collapsed, + Marker)
5. **Sonderpfade kuerzen**: Gateways fuer Ausnahmefaelle in Subprozesse verlagern oder als Boundary-Event darstellen
6. **Zaehlen**: Ziel **8-12 Activities** auf der Hauptebene
7. **Onboarding-Test**: Wuerde ein neuer Mitarbeiter den Prozess auf den ersten Blick verstehen?
