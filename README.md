# Website Zahnarztpraxis Michael Schulz

Website der Zahnarztpraxis Michael Schulz, Dillsteiner Str. 7, 75173 Pforzheim.

Dieses Dokument richtet sich an zwei Leser: an Herrn Schulz, damit er weiß, was
er hat und was er beachten muss, und an eine Entwicklerin oder einen Entwickler,
der die Seite später übernimmt. Technische Abschnitte sind als solche
gekennzeichnet.

---

## Das Wichtigste in Kürze

| | |
|---|---|
| **Domain** | zahnarzt-schulz-pforzheim.de (liegt bei IONOS) |
| **Hosting** | IONOS |
| **Technik** | Statisches HTML, kein Baukasten, kein Redaktionssystem, keine Datenbank |
| **Umfang** | Eine einzige Datei `import/index.html`, rund 3.700 Zeilen |
| **Abhängigkeiten** | Keine. Keine Frameworks, keine npm-Pakete, kein Build-Schritt |
| **Externe Dienste** | FormSubmit (Bewerbungsformular), Google Analytics (nur nach Einwilligung) |
| **Schriften** | Liegen lokal im Projekt, werden nicht von Google geladen |

Die Seite läuft ohne Installation. Datei im Browser öffnen genügt.

---

## Ordner

```
import/
  index.html      Die komplette Website: Aufbau, Gestaltung und Funktionen in einer Datei
  wartung.html    Baustellenseite für Umbauten und Störungen (siehe unten)
  robots.txt      Regeln für Suchmaschinen
  sitemap.xml     Seitenverzeichnis für Suchmaschinen
  images/         Alle Fotos, dazu images/icons/ mit den Leistungs-Symbolen (SVG)
  fonts/          Die beiden Schriften Fraunces und Inter als Webfont-Dateien
  favicon/        Browser- und App-Symbole, dazu site.webmanifest
  index_BACKUP_vor_redesign_2026-07-16.html   Alter Stand, nur Archiv
```

Beim Hochladen auf den Webspace kommt der **Inhalt** von `import/` ins
Stammverzeichnis, nicht der Ordner selbst. `index.html` muss also direkt im
Web-Root liegen, sonst stimmen die Pfade zu Bildern und Schriften nicht.

---

## Warum alles in einer Datei liegt

Das ist eine bewusste Entscheidung, kein Versehen.

Die Praxis hat kein Redaktionssystem und niemanden, der eines pflegt. Eine
einzelne Datei ohne Abhängigkeiten lässt sich in zehn Jahren noch öffnen,
verstehen und hochladen. Es gibt nichts, was veralten, brechen oder ein
Sicherheitsupdate brauchen könnte. Wer die Seite übernimmt, braucht keine
Entwicklungsumgebung, kein `npm install`, keinen Build.

Der Preis dafür ist eine lange Datei. Sie ist dafür durchgehend kommentiert und
in klar benannte Abschnitte geteilt.

---

## Aufbau der `index.html`

Von oben nach unten:

1. **Kopfbereich** mit Meta-Angaben, Favicon-Verweisen und den Schema.org-Daten
   (strukturierte Angaben vom Typ `Dentist`, damit Google die Praxis als solche
   erkennt)
2. **`@font-face`-Blöcke** für die beiden lokalen Schriften
3. **`:root`-Block** mit allen Farben als benannte Variablen
4. **CSS** für die gesamte Seite, nach Abschnitten gegliedert
5. **Markup** der Abschnitte: Hero, Leistungen, Über die Praxis, Team,
   Patientenstimmen, Galerie, Kontakt, Footer
6. **Weitere Ansichten**, die per Klick erscheinen: Karriere mit
   Bewerbungsformular, Datenschutz und Impressum
7. **JavaScript** am Ende: Menü, Galerie-Großansicht, Leistungs-Detailfenster,
   Cookie-Banner, Formularversand

### Technisch: Wo man was ändert

Nicht nach Zeilennummern suchen, die verschieben sich. Nach diesen Begriffen
suchen:

| Was geändert werden soll | Wonach suchen |
|---|---|
| Farben der ganzen Seite | `:root {` — alle Farben stehen dort als Variablen |
| Öffnungszeiten | `kontakt-zeiten` |
| Telefonnummer, E-Mail, Adresse | `07231` beziehungsweise `info@zahnarzt-schulz` |
| Texte und Bilder einer Leistung | `LEISTUNG_DETAILS` — ein Eintrag je Leistung |
| Teammitglieder | `team-grid-v4` |
| Patientenstimmen | `PATIENTENSTIMMEN` |
| Galeriebilder und ihre Beschriftung | `galerie-mosaic-v4` |
| Stellenangebote | `stelle-v4` |
| Empfänger der Bewerbungen | `BEWERBUNG_MAIL` |
| Google-Analytics-Kennung | `GA_MESS_ID` |

Fotos werden ausgetauscht, indem die neue Datei unter dem **gleichen Namen** in
`images/` gelegt wird. Dann muss im Code nichts angefasst werden. Die
Seitenverhältnisse sollten passen, sonst wird beschnitten:

| Zweck | Format | Größe |
|---|---|---|
| Hero | freigestelltes PNG/WebP mit Transparenz | Höhe ca. 1500 px |
| Über die Praxis | 5:4,4 | 1600 × 1408 |
| Teamporträts | 4:5 | 800 × 1000 |
| Galerie | 4:3 | 1600 × 1200 |
| Leistungen | 16:10 | 1600 × 1000 |

---

## Laufende Dienste

### FormSubmit (Bewerbungsformular)

Das Bewerbungsformular verschickt über [formsubmit.co](https://formsubmit.co).
Kostenlos, kein Konto nötig. Der Empfänger steht im Code unter
`BEWERBUNG_MAIL`.

**Wichtig beim Wechsel der Empfängeradresse:** FormSubmit schickt an die neue
Adresse eine Bestätigungsmail. Bis jemand den Link darin angeklickt hat, kommen
**keine Bewerbungen an**, und der Absender merkt davon nichts. Wer die Adresse
ändert, muss also sofort danach eine Testbewerbung abschicken und prüfen, ob sie
ankommt.

FormSubmit nennt keinen Serverstandort. Eine Verarbeitung außerhalb der EU ist
deshalb nicht auszuschließen; genau so steht es auch in der
Datenschutzerklärung. Wer das sauberer haben will, ersetzt den Dienst durch ein
Formular-Tool des eigenen Hosters und passt die Datenschutzerklärung an.

### Google Analytics

Eingebaut, aber **noch nicht scharf**: Unter `GA_MESS_ID` steht ein Platzhalter.
Erst wenn dort eine echte Mess-ID eingetragen ist, wird überhaupt gezählt.

Analytics lädt erst, **nachdem** jemand im Cookie-Banner zugestimmt hat. Ohne
Zustimmung wird nichts geladen und nichts gesetzt. Die IP-Adresse wird gekürzt.
Das ist so gewollt und darf nicht umgebaut werden, ohne die
Datenschutzerklärung mit anzupassen.

### Terminformular

Vollständig vorhanden, aber **absichtlich stillgelegt**. Im Code ist es
auskommentiert und mit Beispieladressen versehen; es besteht keine Verbindung zu
einem echten Postfach. Wenn die Praxis Terminanfragen über die Website annehmen
will, muss es aktiviert und ein Empfänger eingetragen werden — und die
Datenschutzerklärung braucht dann einen Abschnitt dazu.

---

## Noch offen vor dem Livegang

Diese Punkte sind absichtlich im Code markiert und müssen vor der
Veröffentlichung abgearbeitet werden:

- [ ] **Fachliche Freigabe der Leistungstexte durch Herrn Schulz.** Die Texte zu
      Kosten, Kassenleistungen und Behandlungsdauer sind Entwürfe auf Basis
      üblicher Richtwerte. Sie müssen geprüft werden.
- [ ] **Prüf-Markierung entfernen.** Drei Leistungen (Parodontologie,
      Aligner-Therapie, Kinder & Angstpatienten) zeigen im Detailfenster einen
      auffälligen Hinweisbalken. Suche nach `PRÜF-MARKIERUNG`; der Block
      beschreibt selbst, wie er zu entfernen ist.
- [ ] **`BEWERBUNG_MAIL` auf eine Praxisadresse ändern** und die
      Bestätigungsmail von FormSubmit anklicken lassen, danach Testbewerbung.
- [ ] **`GA_MESS_ID` eintragen**, falls Besucherstatistiken gewünscht sind.
- [ ] **Google Search Console** einrichten und `sitemap.xml` einreichen.
- [ ] **Google-Unternehmensprofil** der Praxis mit der neuen Adresse verlinken.
- [ ] **SSL** im IONOS-Konto aktivieren und Weiterleitung von `http` auf
      `https` einschalten.

---

## Rechtliches — bitte nicht ohne Prüfung ändern

**Impressum und Datenschutzerklärung sind keine Fließtexte, sondern
Pflichtangaben.**

Die Datenschutzerklärung beruht auf dem Mustertext der Landeszahnärztekammer
Baden-Württemberg. Der Abschnitt zu Bewerberdaten wurde an die Vorgaben der
KZV Baden-Württemberg angelehnt und auf eine private Praxis angepasst: Als
Rechtsgrundlage ist § 26 BDSG genannt, nicht § 15 LDSG — letzterer gilt nur für
öffentlich-rechtliche Stellen. Die Speicherfrist für Bewerbungen von sechs
Monaten stammt aus § 15 Abs. 4 AGG und wurde vom Datenschutzbeauftragten der
Kammer so bestätigt.

Das Impressum folgt § 5 DDG (dem Nachfolger von § 5 TMG) und enthält die bei
Heilberufen zusätzlich vorgeschriebenen Angaben: zuständige Kammer,
Berufsbezeichnung samt verleihendem Staat, berufsrechtliche Regelungen und
Aufsichtsbehörden. Zuständige Kammer ist die Landeszahnärztekammer in Stuttgart,
zuständige Aufsichtsbehörde das Regierungspräsidium **Karlsruhe** — Pforzheim ist
Stadtkreis im Regierungsbezirk Karlsruhe. Das sind zwei verschiedene
Pflichtangaben, beide sind korrekt so.

**Jede Änderung an den eingesetzten Diensten zieht eine Änderung der
Datenschutzerklärung nach sich.** Wer eine Karte einbettet, ein Social-Media-
Plugin hinzufügt oder den Formularanbieter wechselt, muss den Text anpassen.

---

## Pflichten im laufenden Betrieb

Diese Punkte liegen beim Betreiber der Website, also bei der Praxis:

- **Bewerbungen nach sechs Monaten löschen.** Eingegangene Bewerbungen liegen im
  Mailpostfach der Praxis. Sie sind spätestens sechs Monate nach Abschluss des
  Verfahrens zu löschen, sofern die Bewerberin oder der Bewerber einer längeren
  Speicherung nicht zugestimmt hat.
- **Auftragsverarbeitungsvertrag mit dem Hoster.** Mit IONOS abgeschlossen. Bei
  einem Hosterwechsel ist ein neuer nötig.
- **Angaben aktuell halten.** Ändern sich Öffnungszeiten, Team, Telefonnummer
  oder Zuständigkeiten, gehört das auf die Seite — die Impressumsangaben sind
  gesetzlich verpflichtend aktuell zu halten.

---

## Die Wartungsseite

`import/wartung.html` ist eine eigenständige Seite für Umbauten und Störungen.
Sie zeigt Telefonnummer, E-Mail, Adresse, Öffnungszeiten und den Notfallhinweis
und enthält Impressum und Datenschutzerklärung in Kurzfassung — denn auch eine
Baustellenseite ist nach § 5 DDG impressumspflichtig.

Sie ist bewusst ohne JavaScript und ohne fremde Server gebaut und lädt nur
Schriften und Favicon aus dem Projekt. Sie soll gerade dann funktionieren, wenn
mit der Hauptseite etwas nicht stimmt.

**Einschalten:** `index.html` in `index-live.html` umbenennen, danach
`wartung.html` in `index.html`. **Ausschalten:** umgekehrt. Zwei Umbenennungen
im Dateimanager, kein Eingriff in den Code.

Die Seite trägt `noindex`, damit Google sie nicht an Stelle der echten Website
in den Suchindex aufnimmt.

---

## Für Entwickler: Arbeiten am Projekt

Es gibt keinen Build-Schritt. Datei ändern, im Browser öffnen, fertig.

Vor dem Hochladen sollte geprüft werden:

- Darstellung bei 360, 768 und 1440 Pixeln Breite
- Kein waagerechtes Scrollen auf dem Handy
- Beide Formulare, Cookie-Banner (annehmen und ablehnen), Galerie-Großansicht
- Browser-Konsole frei von Fehlern

Bei Fotos: Neue Bilder in der oben genannten Größe exportieren, als JPG mit
Qualität 80, Dateinamen klein und mit Bindestrich, keine Umlaute und keine
Leerzeichen.

---

## Kontakt

Erstellt von Kevin Wormsbecher, Pforzheim.

Für Rückfragen zum Aufbau der Seite, zu Änderungen oder zur Übergabe an eine
andere Agentur steht er zur Verfügung.
