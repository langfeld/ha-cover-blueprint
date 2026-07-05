# Smarte Rollladensteuerung mit Wetter & Sonnenstand

Dieser Blueprint steuert deine Rollläden automatisch nach Wettervorhersage und Sonnenstand.

- **Morgens** fahren die Rollläden zu einer festen Uhrzeit hoch.
- Je nach Wetter, Temperatur und Sonnenposition wird eine passende Höhe gewählt.
- Optional wird die **Tageshöchsttemperatur** berücksichtigt, damit schon morgens auf einen später warmen Tag reagiert wird.
- In regelmäßigen Abständen (wählbar: 15, 30 oder 60 Minuten) findet eine Neubewertung statt, falls sich Wetter oder Temperatur ändern.
- **Abends** werden die Rollläden bei Sonnenuntergang geschlossen.
- Im **Ruhemodus** kannst du einen Zeitraum festlegen, in dem die Rollläden nicht bewegt werden.
- Im **Testmodus** wird nur berechnet, aber nicht bewegt. Die Zielposition kann optional in einem Helper gespeichert werden, um sie über Tage hinweg zu verfolgen.

---

## Was brauche ich?

- Home Assistant mit aktivierten Automationen
- Ein Wetter-Entity (z. B. OpenWeatherMap, Deutscher Wetterdienst oder eine andere Wetter-Integration)
- Rollläden, die über Home Assistant steuerbar sind

Optional:

- Ein eigener Außentemperatur-Sensor (ansonsten wird die Temperatur aus dem Wetter-Entity gelesen)

---

## Installation

1. Kopiere die folgende URL:

   ```
   https://raw.githubusercontent.com/langfeld/ha-cover-blueprint/refs/heads/main/smart_cover_weather_sun.yaml
   ```

2. Öffne Home Assistant und gehe zu:

   **Einstellungen → Automatisierungen & Szenen → Blueprints**

3. Klicke oben rechts auf **„Blueprint importieren“**.

4. Füge die URL ein und klicke auf **„Vorschau“** und dann **„Importieren“**.

5. Der Blueprint erscheint nun in deiner Blueprint-Liste.

---

## Erste Automation anlegen

1. Gehe zu **Einstellungen → Automatisierungen & Szenen → Blueprints**.
2. Klicke auf den Blueprint **„Smarte Rollladensteuerung mit Wetter & Sonnenstand“**.
3. Gib der Automation einen Namen, zum Beispiel:
   - `Rollläden Ostfenster`
   - `Rollläden Wohnzimmer Süd`
4. Fülle die Einstellungen aus (siehe nächster Abschnitt).
5. Speichere die Automation.

Wiederhole die Schritte für jede Fenstergruppe oder Himmelsrichtung.

---

## Einstellungen erklärt

### Rollläden

Wähle hier die Rollläden aus, die gemeinsam gesteuert werden sollen. Das können einzelne Rollläden oder gleich mehrere sein.

### Wetter-Entity

Wähle deine Wetter-Integration aus. Diese liefert die aktuelle Temperatur und den Wetterzustand (z. B. sonnig, bewölkt, regnerisch).

### Morgendliche Hochfahrzeit

Uhrzeit, zu der die Rollläden morgens hochfahren sollen. Zum Beispiel `07:00:00`.

### Morgens aktivieren / Abends aktivieren

Hier kannst du einstellen, ob die Automation morgens hochfahren und/oder abends herunterfahren soll.

Wenn du bereits eine eigene Abend-Automation hast, kannst du **„Abends aktivieren“** einfach ausschalten.

### Fenster-Ausrichtung (Azimuth)

Gib an, in welche Himmelsrichtung dein Fenster zeigt:

| Richtung | Wert |
|----------|------|
| Norden   | 0°   |
| Osten    | 90°  |
| Süden    | 180° |
| Westen   | 270° |

Beispiel: Ein Fenster, das nach Süden zeigt, bekommt den Wert `180`.

### Toleranzwinkel links/rechts

Gibt den Bereich an, in dem die Sonne noch als „direkt auf das Fenster gerichtet“ gilt. Ein Wert von `45` bedeutet: 45° links und 45° rechts der Fenster-Ausrichtung.

Beispiel:

- Fenster zeigt nach Süden (180°)
- Toleranz 45°
- Sonnenschutz ist aktiv, wenn die Sonne zwischen 135° und 225° steht

### Minimale Sonnenhöhe

Die Sonne muss mindestens so h über dem Horizont stehen, damit der Sonnenschutz greift. Damit wird verhindert, dass der Rollladen schon früh am Morgen oder spät am Abend in die Sonnenschutzposition fährt, wenn die Sonne noch tief steht.

Wenn du nur den Azimuth-Winkel berücksichtigen willst, setze den Wert auf `-90`.

### Temperatur-Schwelle

Temperatur, ab der die Sonnenschutz-Position verwendet wird. Liegt die Temperatur darunter, wird die normale Hoch-Position genutzt.

Beispiel: Bei `22` wird der Sonnenschutz erst ab 22 °C aktiv.

### Temperatur-Sensor (optional)

Wenn du einen eigenen Außentemperatur-Sensor hast, kannst du ihn hier auswählen. Falls du mehrere auswählst, wird der erste Sensor verwendet. Wenn du keinen Sensor auswählst, wird die Temperatur aus dem Wetter-Entity gelesen.

### Tageshöchsttemperatur verwenden

Aktiviere diese Option, wenn du einen Sensor hast, der die vorhergesagte Tageshöchsttemperatur liefert. Statt der aktuellen Temperatur wird dann die Tageshöchsttemperatur genutzt.

Das ist besonders sinnvoll, wenn es morgens noch kalt ist, der Tag aber später warm wird. Der Rollladen fährt dann morgens schon in die Sonnenschutz-Position.

### Tageshöchsttemperatur-Sensor (optional)

Wähle hier den Sensor für die vorhergesagte Tageshöchsttemperatur aus. Wird nur verwendet, wenn „Tageshöchsttemperatur verwenden“ aktiviert ist.

### Regelmäßige Neubewertung aktivieren

Wenn aktiviert, wird im gewählten Intervall geprüft, ob sich die Position ändern sollte. So kann die Automation auf Wetteränderungen während des Tages reagieren.

### Neubewertungsintervall

Hier wählst du den Abstand zwischen den automatischen Neubewertungen:

- 15 Minuten
- 30 Minuten
- 60 Minuten

Dieser Wert wird nur verwendet, wenn die regelmäßige Neubewertung aktiviert ist.

### Minimale Positionsänderung

Der Rollladen fährt nur, wenn sich die neue Position um mindestens diesen Wert von der aktuellen Position unterscheidet. Damit wird verhindert, dass der Rollladen ständig kleine Korrekturen macht.

Standard: `5 %`.

### Bewölkte Wetterbedingungen

Hier wählst du aus, welche Wetterzustände als „bewölkt“ gelten sollen. Bei diesen Zuständen fahren die Rollläden auf die eingestellte bewölkte Position (meist ganz oben).

Standardmäßig sind die meisten nicht-sonnigen Zustände ausgewählt.

### Positionen

In diesem Blueprint gilt folgende Logik:

- **100 %** = Rollladen komplett offen
- **0 %** = Rollladen komplett geschlossen

- **Position bei Sonne im Winkel & warm:** Sonnenschutz-Position, wenn die Sonne auf das Fenster scheint und es warm genug ist. Standard: `60 %`.
- **Position bei Sonne außerhalb oder zu kalt:** Normale Morgen-Position, wenn kein Sonnenschutz nötig ist. Standard: `100 %` (komplett offen).
- **Position bei bewölktem Wetter:** Position bei bewölktem Wetter. Standard: `100 %` (komplett offen).
- **Abend-Position:** Position, auf die die Rollläden abends geschlossen werden. Standard: `0 %`.

### Positionen umkehren

Manche Rollläden sind andersherum kalibriert:

- **100 %** = geschlossen
- **0 %** = offen

Falls das bei dir der Fall ist, aktiviere einfach **„Positionen umkehren“**. Dann werden alle eingestellten Positionen automatisch invertiert, ohne dass du sie neu eingeben musst.

### Ruhemodus

Wenn du möchtest, dass die Rollläden in einer bestimmten Zeit nicht bewegt werden (z. B. nachts), kannst du den **Ruhemodus** aktivieren.

- **Ruhemodus aktivieren:** Schaltet die Funktion ein oder aus.
- **Ruhemodus Beginn:** Uhrzeit, ab der keine Bewegung mehr erfolgen soll.
- **Ruhemodus Ende:** Uhrzeit, bis zu der keine Bewegung erfolgen soll.

Der Zeitbereich funktioniert auch über Mitternacht hinweg, zum Beispiel von `22:00` bis `07:00`.

Wenn der Entscheidungs-Helper konfiguriert ist, wird im Ruhemodus der Wert `quiet_time` gespeichert.

### Pause nach manueller Bedienung

Diese Option verhindert, dass der Intervall-Check eine gerade manuell eingestellte Position sofort wieder überschreibt.

- **⏸️ Pause nach manueller Bedienung:** Schaltet die Funktion ein oder aus.
- **⏸️ Dauer der manuellen Pause:** Zeit in Minuten, für die nach einer manuellen Bedienung pausiert wird.

Wenn eines der Rollläden in diesem Zeitraum manuell bewegt wurde, stoppt die Automation. Im Entscheidungs-Helper wird dann der Wert `manual_activity_pause` gespeichert.

---

## Beispiele für verschiedene Himmelsrichtungen

### Ostfenster

- Fenster-Ausrichtung: `90`
- Toleranzwinkel: `45`
- Position bei Sonne & warm: `40`

### Südfenster

- Fenster-Ausrichtung: `180`
- Toleranzwinkel: `45`
- Position bei Sonne & warm: `50`

### Westfenster

- Fenster-Ausrichtung: `270`
- Toleranzwinkel: `45`
- Position bei Sonne & warm: `40`

### Nordfenster

- Fenster-Ausrichtung: `0`
- Toleranzwinkel: `45`
- Position bei Sonne & warm: `100` (Sonnenschutz meist nicht nötig)

---

## Testmodus und Helper

### Testmodus

Der **Testmodus** blockiert die Bewegung der Rollläden. Die Automation läuft normal weiter, berechnet die Zielposition und speichert sie optional in den Helpern. So kannst du das Verhalten über mehrere Tage beobachten, ohne dass sich die Rollläden ständig bewegen.

### Zielposition-Helper (optional)

Wenn du einen `input_number`-Helper auswählst, wird bei jedem Lauf die berechnete Zielposition in diesen Helper geschrieben. Du kannst den Verlauf dann in einem Graphen oder einer Dashboard-Karte anzeigen.

**So legst du den Helper an:**

1. Einstellungen → Geräte & Dienste → Helfer
2. Auf **Hinzufügen** klicken
3. **Zahl** auswählen
4. Einen Namen vergeben, z. B. `rollladen_zielposition_buro`
5. Mindestwert `0`, Höchstwert `100`, Schrittweite `1`
6. Den Helper im Blueprint unter **„🎯 Zielposition-Helper“** auswählen

### Entscheidungs-Helper (optional)

Wenn du einen `input_text`-Helper auswählst, wird bei jedem Lauf der Grund für die Zielposition gespeichert. Mögliche Werte:

- `cloudy` – bewölktes Wetter
- `sunny_in_angle` – Sonne im Winkel und warm genug
- `sunny_outside_angle` – Sonne außerhalb des Winkels oder zu kalt
- `evening` – abendliches Schließen
- `quiet_time` – Ruhemodus aktiv

**So legst du den Helper an:**

1. Einstellungen → Geräte & Dienste → Helfer
2. Auf **Hinzufügen** klicken
3. **Text** auswählen
4. Einen Namen vergeben, z. B. `rollladen_entscheidung_buro`
5. Den Helper im Blueprint unter **„📝 Entscheidungs-Helper“** auswählen

---

## Tipps

- Lege für jede Fenstergruppe oder Himmelsrichtung eine eigene Automation aus diesem Blueprint an.
- Aktiviere zunächst den **Testmodus** und beobachte die Werte in den Helpern, bevor du die Rollläden wirklich fahren lässt.
- Passe die Temperatur-Schwelle und die Toleranzwinkel an deine Räume an.
- Wenn du den Abend-Teil nicht brauchst, schalte einfach **„Abends aktivieren“** aus.

---

## Lizenz

Dieser Blueprint ist frei nutzbar. Passe ihn gerne an deine Bedürfnisse an.
