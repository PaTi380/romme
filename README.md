# Rommé für Till & Oma – Einrichtung

Das Spiel besteht aus **einer einzigen Datei: `index.html`**. Sie läuft auf GitHub Pages, Firebase gleicht den Spielstand zwischen euren Geräten ab.

Ausprobieren geht auch ohne Einrichtung: `index.html` im Browser öffnen. Solange keine Firebase-Daten eingetragen sind, läuft der **Testmodus** – oben gibt es einen Link „Zweiten Spieler in neuem Tab öffnen“, dann spielst du in zwei Tabs gegen dich selbst.

---

## 1. Firebase einrichten (ca. 10 Minuten)

1. <https://console.firebase.google.com> öffnen → **Projekt hinzufügen**, z. B. `romme-oma` (Google Analytics kannst du ausschalten).
2. **Anmeldung einschalten:** Links *Erstellen → Authentication* → *Jetzt starten* → Reiter *Anmeldemethode* → **Anonym** aktivieren → Speichern.
3. **Datenbank anlegen:** *Erstellen → Realtime Database* → *Datenbank erstellen* → Standort **Belgien (europe-west1)** → *Im gesperrten Modus starten*.
4. **Regeln eintragen:** In der Realtime Database den Reiter **Regeln** öffnen, alles löschen, den Inhalt von `firebase-regeln.json` einfügen → **Veröffentlichen**.
5. **Web-App anlegen:** Zahnrad → *Projekteinstellungen* → unten *Meine Apps* → Symbol **`</>`** → Name z. B. `Romme` (Hosting nicht ankreuzen) → *App registrieren*.
   Firebase zeigt jetzt einen Block `const firebaseConfig = { … }`.
6. **Daten in `index.html` eintragen:** Datei mit einem Texteditor öffnen. Ganz oben steht der Abschnitt `EINSTELLUNGEN`. Dort die Werte übertragen:

   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "AIza…",
     authDomain: "romme-oma.firebaseapp.com",
     databaseURL: "https://romme-oma-default-rtdb.europe-west1.firebasedatabase.app",
     projectId: "romme-oma",
     appId: "1:…:web:…"
   };
   ```

   Wichtig ist die **databaseURL** – falls sie im Block fehlt: Sie steht oben in der Realtime Database (die Adresse mit `…firebasedatabase.app`).
   Die Namen stehen direkt darüber in `ROMME_NAMEN` (Platz 1 = Till, Platz 2 = Oma).
7. **Domain erlauben:** *Authentication → Einstellungen → Autorisierte Domains* → `pati380.github.io` hinzufügen.

> Der `apiKey` darf öffentlich auf GitHub stehen – das ist bei Firebase so vorgesehen. Geschützt wird der Tisch durch die Regeln aus Schritt 4.

## 2. Auf GitHub Pages stellen

1. Auf GitHub ein neues **öffentliches** Repository anlegen, z. B. `romme`.
2. `index.html` hochladen (*Add file → Upload files*).
3. *Settings → Pages* → Source **Deploy from a branch** → Branch `main`, Ordner `/ (root)` → Save.
4. Nach ein bis zwei Minuten ist das Spiel unter **https://pati380.github.io/romme/** erreichbar.

## 3. Spieltisch anlegen

1. `https://pati380.github.io/romme/` öffnen. Es erscheint **„Neuer Spieltisch“** mit zwei Links, die einen geheimen Tischcode enthalten:
   - `…/romme/?t=CODE&p=1` → **dein Link**
   - `…/romme/?t=CODE&p=2` → **Omas Link**
2. Beide Links gut aufheben (z. B. in deinen Notizen). Den Tischcode **nicht** in die `index.html` schreiben, denn das Repository ist öffentlich.
3. Deinen Link öffnen – damit gehört Platz 1 deinem Gerät.

## 4. Omas Tablet einrichten (am besten bei einem Besuch)

1. Omas Link im normalen Browser des Tablets öffnen (iPad: Safari, Android: Chrome). Damit gehört Platz 2 diesem Tablet.
2. Symbol auf den Startbildschirm legen:
   - **iPad:** Teilen-Knopf (Quadrat mit Pfeil) → *Zum Home-Bildschirm*.
   - **Android:** Menü ⋮ → *Zum Startbildschirm hinzufügen*.
3. Ab jetzt öffnet Oma das Spiel nur noch über das **Rommé-Symbol**. Kein Passwort, keine Eingabe.

Tipp: Bei der Bildschirmsperre des Tablets ruhig 5–10 Minuten einstellen. Das Spiel hält den Bildschirm während des Spielens ohnehin wach, sofern das Tablet das unterstützt.

## 5. Wenn mal etwas klemmt

| Problem | Lösung |
|---|---|
| Oma sieht „Dieses Gerät ist noch nicht freigegeben“ (neues Tablet, Browserdaten gelöscht) | Bei dir: ⚙ → **Omas Gerät neu freigeben**. Omas Bildschirm geht danach von selbst weiter (spätestens nach 8 Sekunden). |
| Du hast selbst ein neues Gerät | In der Firebase-Konsole → Realtime Database → `tische` → dein Code → `plaetze` → den Eintrag `1` löschen. Dann deinen Link neu öffnen. |
| Spiel hängt oder ihr wollt neu anfangen | ⚙ → **Spiel beenden (zurück zum Start)**. |
| „Keine Verbindung“ | Internet prüfen, Seite neu laden. Der Spielstand bleibt erhalten. |

Ein ⚙-Knopf ist nur bei dir sichtbar – Oma kann nichts verstellen.

## 6. Updates einspielen

Neue `index.html` hochladen und dabei den Abschnitt `EINSTELLUNGEN` (Namen + Firebase-Daten) aus der alten Datei übernehmen. Laufende Spiele bleiben erhalten.

---

## Die Regeln im Programm

- 2 Kartenspiele mit 6 Jokern (110 Karten). Jeder bekommt 13 Karten, wer beginnt, bekommt eine 14. Karte **offen** – beide sehen sie. Der Beginner zieht im ersten Zug nicht, sondern wirft ab.
- In der ersten Runde fängt Oma an, danach wird abgewechselt. Die Rundenzahl wird vor dem Spiel eingestellt.
- **Reihen:** mindestens 3 Karten einer Farbe, lückenlos. „Um die Ecke“ ist erlaubt (König-Ass-2).
- **Gleiche Werte:** 3 oder 4 Karten, jede Farbe nur einmal, Reihenfolge egal.
- **Erstes Auslegen:** mindestens 30 Punkte in einem Zug. Zahlen zählen ihren Wert, Bilder 10, Ass 11 (in A-2-3 oder K-A-2 nur 1). Ein Joker zählt so viel wie die Karte, die er ersetzt.
  Solange die 30 nicht erreicht sind, liegen die Karten gestrichelt als „noch nicht fest“ da und lassen sich mit **Zurücknehmen** wieder aufnehmen.
- Die oberste Karte vom **Ablagestapel** darf man jederzeit nehmen und behalten.
- **Anlegen** und **Joker tauschen** erst, wenn man selbst ausgelegt hat – dann auch bei den Karten des anderen. Ein getauschter Joker kommt auf die Hand.
- Man muss immer mit dem **Abwerfen** der letzten Karte fertig werden.
- Die Handkarten kommen unsortiert. Jeder sortiert selbst durch Verschieben, die Reihenfolge merkt sich das Gerät.
- **Minuspunkte** für den Verlierer: Zahlen ihren Wert, Bilder 10, Ass und Joker 20. Bei **Handrommé** (alles auf einmal ausgelegt, ohne vorher draußen gewesen zu sein) zählt alles doppelt.
- Am Ende gewinnt, wer die wenigsten Minuspunkte hat.

## Kurzanleitung für Oma (zum Ausdrucken)

1. Auf das **Rommé-Symbol** tippen.
2. Deine Karten kommen gemischt. **Sortieren:** Karte mit dem Finger festhalten und an die gewünschte Stelle schieben – ein gelber Strich zeigt, wo sie landet. Das geht auch, während Till dran ist.
3. Ist das gelbe Feld oben zu sehen, bist du dran.
4. Auf den **Stapel** oder auf die **Ablage** tippen – du bekommst eine Karte (sie ist mit „neu“ markiert).
5. Karten antippen, um sie auszuwählen (sie rutschen nach oben). Nochmal tippen = abwählen.
6. **Auslegen:** 3 oder mehr passende Karten wählen → „Auslegen“.
   **Anlegen:** Karten wählen → auf die Gruppe auf dem Tisch tippen (passende Gruppen leuchten grün).
7. Zum Schluss **eine** Karte wählen → „Abwerfen“. Dann ist Till dran.

Wenn etwas nicht geht, erklärt ein Hinweis in der Mitte, warum.
