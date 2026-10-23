# User Stories -- Digitale Terminbuchungsplattform fuer Arztpraxen

Jede User Story beschreibt den Mehrwert eines Sequenzdiagramms aus der Perspektive des jeweils ausfuehrenden Akteurs.

---

## SD 01 -- Termin suchen & buchen

**Als Patient** moechte ich online nach einem freien Arzttermin suchen und ihn direkt buchen koennen,
sodass ich keine Zeit am Telefon verliere und meinen Termingrund vorab mitteilen kann.

**Akzeptanzkriterien:**
- Ich kann Fachrichtung und Arzt auswaehlen
- Das System zeigt mir verfuegbare Zeitslots an
- Ich kann einen Slot auswaehlen und einen Termingrund eingeben
- Ich erhalte eine Buchungsbestaetigung
- Die Arztpraxis wird automatisch benachrichtigt

---

## SD 05 -- Patient ueberweisen

**Als Hausarzt** moechte ich einen Patienten digital an einen Facharzt ueberweisen und dabei direkt
einen passenden Termin vermitteln lassen, damit der Patient nahtlos weiterversorgt wird und ich eine
Statusrueckmeldung ueber die erfolgreiche Vermittlung erhalte.

**Akzeptanzkriterien:**
- Ich kann Diagnose und Fachrichtung im System erfassen
- Das System sucht automatisch passende Fachaerzte im Umkreis
- Der Patient erhaelt Terminvorschlaege und kann auswaehlen
- Der Facharzt wird ueber die Ueberweisung und den gebuchten Termin informiert
- Ich erhalte eine Statusrueckmeldung nach abgeschlossener Vermittlung

---

## SD 09 -- Post-Termin / Behandlung & Rezept

**Als Arzt** moechte ich nach einer Behandlung die Diagnose dokumentieren, ein Rezept digital
ausstellen und freigeben sowie einen Folgetermin anordnen koennen, damit der Patient alle
Informationen sofort digital erhaelt und die Patientenakte aktuell bleibt.

**Akzeptanzkriterien:**
- Ich kann Diagnose und Massnahmen im System erfassen
- Die Patientenakte wird automatisch aktualisiert
- Ich kann ein Rezept digital signieren und freigeben
- Der Patient erhaelt das Rezept digital (optional: Weiterleitung an Apotheke)
- Ich kann einen Folgetermin anordnen; der Patient erhaelt Terminvorschlaege
- Der Patient erhaelt eine Zusammenfassung der Konsultation

---

## SD 08 -- Apotheke suchen

**Als Patient** moechte ich nach meinem Arzttermin in der App eine Apotheke in meiner Naehe finden,
die mein Medikament auf Lager hat, und es bei Bedarf direkt reservieren koennen, damit ich keine
unnoetigen Wege mache.

**Akzeptanzkriterien:**
- Ich kann Standort und Suchkriterien (Medikament) eingeben
- Das System zeigt mir Apotheken im Umkreis mit Verfuegbarkeitsangabe an
- Ich kann eine Apotheke auswaehlen und Details (Adresse, Oeffnungszeiten) einsehen
- Ich kann ein Medikament optional reservieren und erhalte eine Bestaetigung
- Wenn keine Apotheke gefunden wird, erhalte ich einen entsprechenden Hinweis

---

## SD 10 -- Patientenregistrierung

**Als Patient** moechte ich mich einmalig online auf der Plattform registrieren und meine
Versicherungsdaten hinterlegen, damit mir die Arztpraxis nach einer kurzen Pruefung den Zugang
freischaltet und ich anschliessend alle Dienste nutzen kann.

**Akzeptanzkriterien:**
- Ich kann ein Konto mit Stammdaten und Versicherungsinformationen anlegen
- Ich verifiziere meine E-Mail-Adresse per Link
- Die Arztpraxis prueft meine Daten und erteilt die Freigabe
- Bei ungueltigen Daten erhalte ich einen Hinweis und kann korrigieren
- Nach Freigabe erhalte ich eine Willkommensnachricht und Zugang zur Plattform
