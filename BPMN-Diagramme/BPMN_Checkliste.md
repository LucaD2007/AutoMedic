# BPMN-Pruefcheckliste

Systematische Checkliste zur Pruefung und Verbesserung aller BPMN-Diagramme.
Quellen: Vorlesung "GP-Orientierte Analyse" (Folien 04/04.02), "Diagrammtechniken" (Folie 03), Aufgabenstellung Fallstudie.

---

## A. Strukturelle Anforderungen (Aufgabenstellung)

- [ ] Diagramm ist ein **Kollaborationsdiagramm** (mehrere Pools)
- [ ] Durchschnittlich **ca. 10 Aktivitaeten** pro Diagramm
- [ ] **Lanes/Pools** sind vorhanden und sinnvoll eingesetzt
- [ ] **Datenobjekte** (Einzelobjekte) und/oder **Datenspeicher** (Objektmengen) sind modelliert
- [ ] Dateiname folgt Schema: `XX-PraegnanterName` (z.B. `01-TerminSuchenBuchen`)
- [ ] Diagramm oeffnet fehlerfrei in **Camunda Modeler** oder **bpmn.io**
- [ ] Syntaktisch korrektes **BPMN 2.0** XML-Format

## B. Aktivitaeten (Folien 4-17 bis 4-22)

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

## I. Subprozesse und Schleifen (Folien 4-37 bis 4-43)

- [ ] **Lokale Subprozesse** fuer Verfeinerung komplexer Aktivitaeten genutzt (wo sinnvoll)
- [ ] **Call Activities** (globale Subprozesse) fuer wiederverwendbare Prozesse ueber Diagramme hinweg
- [ ] **Schleifen** (Loop) fuer iterative Aktivitaeten eingesetzt
- [ ] **Multi-Instanzen** (sequentiell/parallel) wo mehrere gleichartige Instanzen noetig

## J. Qualitaet und Konsistenz (uebergreifend)

- [ ] Diagramm ist **selbsterklaerend** -- ein Aussenstehender versteht den Prozess
- [ ] Einheitliche **Sprache** (durchgehend Deutsch oder Englisch)
- [ ] Einheitliche **Schreibweise** und Terminologie ueber alle 10 Diagramme hinweg
- [ ] **Automatisierungspotenzial** ist erkennbar (welche Schritte kann Software uebernehmen?)
- [ ] Konsistenz zwischen Diagrammen (z.B. gleiche Pool-Namen, gleiche Datenspeicher-Bezeichnungen)
- [ ] Keine redundanten Aktivitaeten oder ueberfluessigen Elemente
