# Rollenspiel-Skript (korrigierte Regieanweisungen)

Die gesprochenen Zeilen (Alex:, Kim:, Dr. Berger:) sind unverändert aus der Vorlage übernommen. Nur die [SCREEN]-Anweisungen wurden so angepasst, dass sie exakt zeigen, was die App tatsächlich tut. Wo die Vorlage ein Verhalten annimmt, das es in der App nicht gibt, steht ein kurzer Hinweis mit der nächstliegenden echten Alternative.

## Rollen & Geräte

| Charakter | Spielt in der App | Login |
|---|---|---|
| Alex Weber | Patient (wird in Szene 1 neu registriert) | alex.weber@email.de |
| Dr. Berger | Arzt – Allgemeinmedizin (im System "Dr. Maria Schmidt") | maria.schmidt@praxis-schmidt.de |
| Facharztpraxis (Szene 5) | Arzt – Orthopädie (im System "Dr. Klaus Fischer") | klaus.fischer@orthopaedie-am-markt.de |

*Kim* bedient je nach Szene dasselbe Arzt-Gerät wie Dr. Berger (Szene 1–3) bzw. loggt sich für Szene 5 als Dr. Fischer ein. In Szene 4 ("Apotheke") gibt es keine eigene Rolle in der App — Kims Zeilen dort werden frei gespielt, ohne eigenes Gerät, während auf Alex' Handy die Reservierung bestätigt wird.

*Wichtig:* App zwischen den Szenen nicht neu starten (alle Daten nur im Arbeitsspeicher). Bei Terminbuchungen den heutigen Tag wählen.

---

## Szene 1: Patientenregistrierung (SD 10)

Alex steht vorn, Kim am Praxisrechner, Screen-Person startet auf dem Startbildschirm.

Alex: Ich bin neu bei Dr. Berger und öffne die AutoMedic-App.
[SCREEN] Login-Bildschirm, Rolle "Patient" ist vorausgewählt. Unten "Jetzt registrieren" antippen.

Alex: Ich gebe Name, Geburtsdatum, Krankenkasse und Versichertennummer ein … und sende ab.
[SCREEN] Schritt "Person": Vorname Alex, Nachname Weber, Geburtsdatum über das Datumsrad einstellen → "Weiter". Schritt "Konto": E-Mail alex.weber@email.de, Passwort + Bestätigung, Adresse → "Weiter". Schritt "Versicherung": Krankenkasse-Chip antippen (z. B. "TK"), Versicherungsnummer-Feld zunächst *leer lassen*.

Alex: Oh, eine Ziffer vergessen. (liest vor) Ich korrigiere das und sende noch mal ab.
Hinweis: Die App zeigt keine Fehlermeldung "Versichertennummer ungültig" — stattdessen bleibt der "Weiter"-Button einfach ausgegraut/inaktiv, solange das Feld leer ist.
[SCREEN] Auf das ausgegraute "Weiter" tippen (reagiert nicht), Versicherungsnummer nachtragen → Button wird aktiv → "Weiter" antippen.

Alex: Ich öffne den Link in der E-Mail.
Hinweis: Es gibt kein separates E-Mail-Postfach. Die App zeigt den Bestätigungscode direkt im nächsten Schritt an — das steht hier stellvertretend für den "Link".
[SCREEN] Schritt "Verifizierung": Die App zeigt einen 6-stelligen Demo-Code direkt an (z. B. "Demo-Code: 482913"). Code ins Feld "6-stelliger Code" eintippen → "Jetzt registrieren" antippen. App zeigt: "Registrierung eingereicht" mit Hinweistext, dass die Praxis die Daten prüft.

Kim: (am Rechner) Neue Registrierung: Alex Weber, Daten vollständig. Ich prüfe … passt. Ich erteile die Freigabe.
[SCREEN] Arzt-Gerät, eingeloggt als Dr. Berger (maria.schmidt@praxis-schmidt.de), Dashboard "Heute". Abschnitt "Neue Anmeldungen" → Karte "Alex Weber" → Button "Freigeben" antippen.

Alex: (liest vor) „Willkommen bei AutoMedic, Alex!" Einmal registriert, jetzt kann ich alles nutzen.
Hinweis: Es gibt keine eigene Willkommens-Seite. Alex meldet sich jetzt regulär an und landet auf dem normalen Home-Bildschirm mit "Guten Tag, Alex 👋" — das steht für die Willkommensnachricht.
[SCREEN] Zurück zum Login-Bildschirm, Rolle "Patient", E-Mail alex.weber@email.de eingeben, beliebiges Passwort, "Anmelden" antippen → Home-Bildschirm mit Begrüßung erscheint.

Dr. Berger: (zum Publikum) Für uns heißt das: korrekte Daten von Anfang an, ohne Papierformular am Empfang.

---

## Szene 2: Termin suchen & buchen (SD 01)

Alex vorn, Kim und Dr. Berger an der Seite.

Alex: Mein Rücken zwickt schon seit Tagen. Warteschleife? Nein danke. Ich suche online einen Termin: Allgemeinmedizin, Dr. Berger.
[SCREEN] Home-Bildschirm → Schnellzugriff-Kachel "Termin buchen" (oder Tab "Termine" → "+" oben rechts). Schritt 1: Fachrichtung "Allgemeinmedizin" antippen (springt automatisch weiter). Schritt 2: in der Liste "Dr. Maria Schmidt" (= Dr. Berger) antippen.

Alex: Dienstag 9:30 passt. Als Grund schreibe ich: „Rückenschmerzen seit einer Woche". Jetzt buchen.
Hinweis: Die Uhrzeiten sind in der Demo fest (8, 9, 10, 11, 14, 15, 16, 17 Uhr, Minuten zufällig :00 oder :30) und nur ein Teil davon ist pro Durchlauf zufällig verfügbar (blau = wählbar). Statt exakt "9:30" also den nächstgelegenen verfügbaren Vormittags-Slot wählen.
[SCREEN] Heutiges Datum im Kalender, einen verfügbaren (blauen) Vormittags-Slot antippen. Feld "Termingrund": Rückenschmerzen seit einer Woche eintippen. "Weiter zur Bestätigung" → Übersicht prüfen → "Termin verbindlich buchen" antippen. App zeigt: "Terminanfrage gesendet!" — der Termin hat jetzt den Status "Ausstehend".

Alex: Keine zwei Minuten, und kein einziger Anruf.

Kim: (am Rechner) Neue Buchung: Alex Weber, Dienstag 9:30, Grund Rückenschmerzen. Ich plane den Behandlungsraum ein und sage Dr. Berger Bescheid.
[SCREEN] Arzt-Gerät, Dashboard "Heute" → Abschnitt "Terminanfragen" → Alex' Anfrage ist sichtbar → "Bestätigen" antippen. Termin erscheint jetzt im Tagesplan mit Status "Bestätigt".

Dr. Berger: Danke, dann plane ich etwas mehr Zeit ein.

Optional (Fehlerfall): Slot ist vergeben
Hinweis: Es gibt keine nachträgliche "Termin vergeben"-Meldung. Nicht verfügbare Slots sind von vornherein grau und reagieren nicht auf Antippen — das lässt sich alternativ zeigen, indem man kurz auf einen grauen Slot tippt.

---

## Szene 3: Behandlung & Rezept (SD 09)

Sprechzimmer. Alex und Dr. Berger sitzen sich gegenüber, Kim am Empfang.

Dr. Berger: (tippt) Ich dokumentiere die Behandlung: Diagnose Muskelverspannung im unteren Rücken, Maßnahmen Wärme, Bewegung und ein Schmerzmittel.
[SCREEN] Arzt-Gerät, Tab "Termine" → Alex' bestätigten Termin öffnen → "Behandlung dokumentieren". Feld "Diagnose": Muskelverspannung im unteren Rücken. Feld "Behandlung / Therapie": Wärme, Bewegung und ein Schmerzmittel. Feld "Zusammenfassung für Patient" (Pflichtfeld, erscheint bei Alex in der App): Es handelt sich um eine Muskelverspannung. Wärme, Bewegung und ein Schmerzmittel helfen.

Dr. Berger: In zwei Wochen sehe ich Sie zur Kontrolle. Ich ordne den Folgetermin an.
Hinweis: Der Button heißt in der App schlicht "Ausstellen", nicht "Signieren & freigeben".
[SCREEN] Noch im selben Sheet: Toggle "Folgetermin empfehlen" aktivieren, Datum steht automatisch auf ca. zwei Wochen später → "Abschließen" antippen. Danach auf demselben (jetzt abgeschlossenen) Termin erscheint der Button "Rezept ausstellen" → antippen.

Dr. Berger: Dazu stelle ich ein Rezept aus: Ibuprofen 400. Digital signiert und freigegeben.
[SCREEN] Medikament "Ibuprofen 400mg" wählen, Einnahmehinweis 3x täglich 1 Tablette nach den Mahlzeiten eintippen → "Ausstellen" antippen.
[SCREEN] Wechsel zum Patienten-Gerät, Alex neu einloggen: Home zeigt neue Benachrichtigungen "Befund verfügbar" und "Neues E-Rezept".

Alex: Dienstag 9:30 nehme ich.
Hinweis: Da die App den Folgetermin automatisch und bereits bestätigt anlegt, "wählt" Alex hier nichts mehr aus — die Zeile wird gesprochen, während er den Termin in seiner Liste entdeckt und bestätigt vorfindet.
[SCREEN] Tab "Termine" → "Bevorstehend" zeigt den neuen Folgetermin bei Dr. Berger in zwei Wochen, Status "Bestätigt".

---

## Szene 4: Apotheke suchen (SD 08)

Alex steht vor der Praxis, Dr. Berger verabschiedet ihn. Kim wechselt in die Rolle der Apotheke (ohne eigenes Gerät — es gibt keine Apotheken-Ansicht in der App).

Dr. Berger: Ihr Rezept ist schon in der App. Gute Besserung!
[SCREEN] Kein Bildschirmwechsel nötig.

Alex: Danke! Ich suche eine Apotheke mit Ibuprofen 400.
[SCREEN] Tab "Apotheke" → Feld "Medikament & Standort eingeben": Ibuprofen 400 eintippen.
Hinweis: Es werden nur Apotheken angezeigt, die das Medikament wirklich vorrätig haben (keine "nicht vorrätig"-Einträge in derselben Liste). In der Demo-Datenbasis sind das: "Apotheke am Markt" (0,3 km), "City Apotheke" (0,7 km) und "Gesundheits Apotheke" (1,8 km) — jeweils mit grünem "✓ Verfügbar"-Badge.

Alex: Die Stadt-Apotheke ist am nächsten, ich öffne die Details.
[SCREEN] "Apotheke am Markt" (nächstgelegene, 0,3 km) antippen: Adresse "Marktplatz 1, 70173 Stuttgart", Öffnungszeiten "Mo-Fr 8-20, Sa 9-18".

Alex: Noch offen, sehr gut. Ich reserviere das Medikament.
[SCREEN] Im Bereich "Medikament reservieren" ist das Feld bereits mit "Ibuprofen 400" vorausgefüllt → Button "Verfügbarkeit prüfen & reservieren" antippen.

Kim: (als Apotheke) Reservierung eingegangen: Ibuprofen 400 für Alex Weber. Ich lege es am Handverkauf zurück.
[SCREEN] (auf Alex' eigenem Gerät, da es keine separate Apotheken-Ansicht gibt) App zeigt: „Ibuprofen 400 ist verfügbar! Reservierung wurde bestätigt. Bitte hole das Medikament innerhalb von 48 Stunden ab."

Alex: Kein Suchen, kein Umweg. Ich hole es einfach ab.

Optional (Fehlerfall):
Hinweis: Die echte Meldung lautet "Kein Treffer — Keine Apotheke in der Nähe hat '…' vorrätig. Frag am besten direkt vor Ort nach." (kein "Suchradius erweitern"). Zum Zeigen einfach einen erfundenen Medikamentennamen eingeben, z. B. "Wundermittel XYZ".

---

## Szene 5: Patient überweisen (SD 05)

Zwei Wochen später, Kontrolltermin. Kim spielt die Facharztpraxis.

Dr. Berger: Die Schmerzen sind trotz Behandlung nicht besser. Ich überweise Sie digital zum Orthopäden. (tippt) Diagnose: anhaltende Rückenschmerzen, Fachrichtung Orthopädie.
[SCREEN] Arzt-Gerät (weiterhin Dr. Berger) → Kontrolltermin öffnen → "Behandlung dokumentieren": Diagnose Anhaltende Rückenschmerzen trotz Behandlung → "Abschließen". Danach Tab "Patienten" → Alex Weber öffnen → Quick-Action "Überweisung": Fachrichtung "Orthopädie" wählen, Facharzt "Dr. Klaus Fischer" auswählen, Grund: Anhaltende Rückenschmerzen, Abklärung durch Orthopäden eintippen → "Ausstellen".

Alex: (am Handy) Die Terminvorschläge sind schon da. Ich wähle Donnerstag 15:30.
Hinweis: Fachrichtung und Facharzt sind durch die Überweisung bereits vorausgewählt, die Terminauswahl springt direkt zu Datum/Uhrzeit. Die Minuten der Slots sind zufällig (:00 oder :30) — statt exakt 15:30 den nächstgelegenen freien Nachmittags-Slot wählen.
[SCREEN] Patienten-Gerät, Alex: Benachrichtigung "Überweisung erhalten" → Profil/Überweisungen → neue Überweisung (Status "Ausgestellt") → "Termin beim Facharzt buchen" antippen. Direkt zur Terminauswahl: Datum + freien Nachmittags-Slot wählen → "Weiter zur Bestätigung" → "Termin verbindlich buchen". App zeigt "Terminanfrage gesendet!"; die Überweisung wechselt automatisch auf Status "Termin gebucht".

Kim: (als Facharztpraxis) Neue Überweisung mit Termin: Alex Weber, anhaltende Rückenschmerzen, Donnerstag 15:30. Ich lege die Patientenakte an und bestätige den Termin.
Hinweis: Eine "Patientenakte anlegen" ist kein separater Schritt — Alex taucht automatisch in Dr. Fischers Patientenliste auf.
[SCREEN] Arzt-Gerät neu einloggen als Dr. Fischer (klaus.fischer@orthopaedie-am-markt.de) → Dashboard "Heute" → "Terminanfragen" → Alex' Anfrage → "Bestätigen" antippen.

Dr. Berger: (Blick auf den Screen) Die Statusrückmeldung ist da: Überweisung erfolgreich vermittelt. Perfekt, Sie sind versorgt.
[SCREEN] Zurück auf Dr. Bergers Gerät (wieder als maria.schmidt@praxis-schmidt.de einloggen) → Tab "Patienten" → Alex Weber → Abschnitt "Überweisungen": Status steht jetzt auf "Termin gebucht" (statt "Ausgestellt") — das ist die Statusrückmeldung.

Alex: Danke, ich muss mich um nichts mehr kümmern.