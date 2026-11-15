# Painting

![Screenshot](images/painting-card.png)

Your house as an oil painting in a gilded frame, next to a museum label with the house's readings.

- Sky follows the sun (`sun.sun`) and the weather: dawn, daylight, dusk, stars and moon, rain and snow.
- Windows glow while their light is on. Tap a window to toggle it.
- Smoke rises from the chimney while the burner is running.
- The label tells the time of day, weather, heating, gas, power, who is home and the next departure, and lists all rooms and lights.

## House

The painted house is set up under `house`. Without settings it has two floors, three window axes, the door in the middle, an attic window and the chimney on the right.

```yaml
house:
  floors: 3              # 1 to 4
  axes: 5                # window axes, 2 to 6
  door: 2                # axis of the front door, counted from the left
  attic: true            # gable window
  chimney: left          # left, right or false
  cellar: true           # basement windows in the plinth
  wall: "#d9c7a3"        # facade colour
  roof: "#463a36"        # roof colour
```

## Windows

`windows` assigns a light to each window. Windows are named `w<floor>_<axis>`; the ground floor is 0, axes count from the left starting at 1. Basement windows are `c_<axis>`. Then there are `attic`, `door` and `lantern`. Several windows may share a light; windows not listed stay dark.

```yaml
windows:
  w0_1: light.kitchen          # ground floor, left
  w0_3: light.living_room      # ground floor, right
  w1_1: light.robins_room      # first floor, left
  w1_3: light.bedroom
  c_1: light.basement          # basement window, left
  attic: light.attic           # gable window
  door: light.hallway          # front door
  lantern: light.yard          # lantern next to the door
```

Also: `painting_title` (default "Das Haus").

## Configuration

All cards share the same keys. Anything missing is hidden or shown as "–".

```yaml
type: custom:painting-card
title: Home
language: en                          # any language with a file in i18n/; defaults to the Home Assistant language
outside_temperature: sensor.outdoor_temperature
outside_humidity: sensor.outdoor_humidity
weather: weather.forecast_home        # condition, and fallback for the outdoor temperature
flame: binary_sensor.burner_flame     # heating, optional
boiler: sensor.boiler_temperature
hot_water: sensor.hot_water_temperature
gas_today: sensor.gas_today
gas_cost: sensor.gas_cost_today
power: sensor.house_power             # W
download: sensor.wan_download         # optional, converted to Mbit/s whatever the unit
upload: sensor.wan_upload
battery_threshold: 20                 # battery sensors below this are reported
idle_timeout: 300                     # idle screen after this many seconds without touch (omit = off)
idle_wake:                            # optional, keeps the dashboard awake while active
  - binary_sensor.kitchen_presence
rooms:
  - name: Living room
    temperature: sensor.living_room_temperature
    humidity: sensor.living_room_humidity
    co2: sensor.living_room_co2           # optional, ppm; yellow from 1000, red from 1200
    pm25: sensor.living_room_pm25         # optional, µg/m³; yellow from 15, red from 35
lights:
  - name: Kitchen
    entity: light.kitchen
departure:                            # optional, sensor holding the timestamp of the next departure
  name: Bus 12 › Centre
  entity: sensor.next_departure
  delay: sensor.delay
contacts:                             # optional, windows and doors
  - name: Bathroom window
    entity: binary_sensor.bathroom_window
```

The idle screen puts a black layer with a faint clock over the dashboard; a tap wakes it without switching anything underneath. A browser cannot dim the tablet's backlight itself. While idle, all animations pause and the card redraws only every 30 seconds, which helps older tablets.

People come from all `person.*` entities, low batteries from all sensors with `device_class: battery`.

The cards speak English and German. Without `language` they follow the Home Assistant language; languages without a translation fall back to English.

## Translations

All texts live in `i18n/<language>.json`, one file per language, keyed by short names such as `boiler` or `departs_in`. Placeholders in braces (`{mins}`, `{name}`) are filled in by the card and may move freely within the sentence. To add a language, copy `i18n/en.json` to e.g. `i18n/fr.json`, translate the values and open a pull request. Missing keys fall back to English.

## Installation

Via HACS as a custom repository (type Dashboard), or by hand: copy `painting-card.js` to `/config/www/` and add `/local/painting-card.js` as a JavaScript module under Settings → Dashboards → Resources.

A discreet signature sits in the bottom right corner; `signature: false` hides it.

The card is meant for a panel view (`panel: true`) so it fills the screen.

```yaml
views:
  - title: Home
    panel: true
    cards:
      - type: custom:painting-card
        # configuration as above
```

The fonts are embedded in the card file; the card loads nothing from external servers.

## License

Apache License 2.0, copyright 2026 Axel Eble. If you redistribute or modify the card, include `LICENSE` and `NOTICE`.


---

## Deutsch

### Gemälde

![Screenshot](images/painting-card-de.png)

Das Haus als Ölbild in einem Goldrahmen, daneben ein Museumsschild mit den Werten des Hauses.

- Himmel nach Sonnenstand (`sun.sun`) und Wetter: Morgenrot, Tag, Dämmerung, Sterne und Mond, Regen und Schnee.
- Fenster leuchten, wenn ihre Lampe an ist. Ein Klick auf ein Fenster schaltet sie.
- Läuft der Brenner, steigt Rauch aus dem Kamin.
- Das Schild nennt Tageszeit, Wetter, Heizung, Gas, Strom, Anwesende und Abfahrten und führt alle Räume und Lampen auf.

#### Haus

Das gemalte Haus lässt sich unter `house` einstellen. Ohne Angaben ist es zweigeschossig mit drei Fensterachsen, Tür in der Mitte, Giebelfenster und Kamin rechts.

```yaml
house:
  floors: 3              # Geschosse, 1 bis 4
  axes: 5                # Fensterachsen, 2 bis 6
  door: 2                # Achse der Haustür, von links gezählt
  attic: true            # Giebelfenster
  chimney: left          # left, right oder false
  cellar: true           # Kellerfenster im Sockel
  wall: "#d9c7a3"        # Fassade
  roof: "#463a36"        # Dach
```

#### Fenster

`windows` ordnet jedem Fenster eine Lampe zu. Die Fenster heißen `w<Geschoss>_<Achse>`, das Erdgeschoss ist 0, die Achsen zählen von links ab 1. Kellerfenster heißen `c_<Achse>`. Dazu kommen `attic`, `door` und `lantern`. Mehrere Fenster dürfen dieselbe Lampe haben, nicht genannte bleiben dunkel.

```yaml
windows:
  w0_1: light.kueche           # Erdgeschoss links
  w0_3: light.wohnzimmer       # Erdgeschoss rechts
  w1_1: light.zimmer_robin     # 1. Obergeschoss links
  w1_3: light.schlafzimmer
  c_1: light.keller            # Kellerfenster links
  attic: light.dachzimmer      # Giebelfenster
  door: light.flur             # Haustür
  lantern: light.hoflicht      # Laterne neben der Tür
```

Zusätzlich: `painting_title` (Standard „Das Haus“).
#### Konfiguration

Alle Karten teilen sich dieselben Schlüssel. Was fehlt, wird ausgeblendet oder als „–“ angezeigt.

```yaml
type: custom:painting-card
title: Haus
language: de                          # jede Sprache mit Datei in i18n/; Standard ist die Sprache von Home Assistant
outside_temperature: sensor.aussen_temperatur
outside_humidity: sensor.aussen_feuchte
weather: weather.forecast_home        # Zustand und Ersatz für die Außentemperatur
flame: binary_sensor.brenner_flamme   # Heizung, optional
boiler: sensor.kesseltemperatur
hot_water: sensor.warmwassertemperatur
gas_today: sensor.gas_heute
gas_cost: sensor.gaskosten_heute
power: sensor.hausleistung            # W
download: sensor.wan_download         # optional, Einheit wird nach Mbit/s umgerechnet
upload: sensor.wan_upload
battery_threshold: 20                 # Batteriesensoren darunter werden gemeldet
idle_timeout: 300                     # Ruhemodus nach so vielen Sekunden ohne Berührung (weglassen = aus)
idle_wake:                            # optional, hält das Dashboard wach, solange aktiv
  - binary_sensor.kueche_praesenz
rooms:
  - name: Wohnzimmer
    temperature: sensor.wohnzimmer_temperatur
    humidity: sensor.wohnzimmer_feuchte
    co2: sensor.wohnzimmer_co2            # optional, ppm; gelb ab 1000, rot ab 1200
    pm25: sensor.wohnzimmer_pm25          # optional, µg/m³; gelb ab 15, rot ab 35
lights:
  - name: Küche
    entity: light.kueche
departure:                            # optional, Sensor mit Zeitstempel der nächsten Abfahrt
  name: Linie 3 › Stadt
  entity: sensor.naechste_abfahrt
  delay: sensor.verspaetung
contacts:                             # optional, Fenster und Türen
  - name: Fenster Bad
    entity: binary_sensor.fenster_bad
```

Der Ruhemodus legt ein schwarzes Bild mit schwacher Uhr über das Dashboard; Antippen weckt es, ohne darunter etwas zu schalten. Die Helligkeit des Tablets selbst lässt sich aus dem Browser nicht steuern. Solange er aktiv ist, stehen alle Animationen still und die Karte zeichnet nur alle 30 Sekunden neu; das entlastet ältere Tablets.

Die Karten sprechen Deutsch und Englisch. Ohne `language` folgen sie der Sprache von Home Assistant; Sprachen ohne Übersetzung erscheinen auf Englisch.

Alle Texte stehen in `i18n/<sprache>.json`, eine Datei je Sprache, mit kurzen Schlüsseln wie `boiler` oder `departs_in`. Platzhalter in geschweiften Klammern (`{mins}`, `{name}`) füllt die Karte; sie dürfen im Satz beliebig stehen. Für eine neue Sprache `i18n/en.json` kopieren, etwa nach `i18n/fr.json`, die Werte übersetzen und einen Pull Request stellen. Fehlende Schlüssel fallen auf Englisch zurück.

Personen kommen automatisch aus allen `person.*`-Entitäten, schwache Batterien aus allen Sensoren mit `device_class: battery`.

#### Einbau

Über HACS als benutzerdefiniertes Repository (Typ Dashboard) oder von Hand: `painting-card.js` nach `/config/www/` kopieren und unter Einstellungen → Dashboards → Ressourcen als JavaScript-Modul `/local/painting-card.js` eintragen.

Unten rechts steht eine dezente Signatur; `signature: false` blendet sie aus.

Die Karte ist für eine Panel-Ansicht gedacht (`panel: true`), damit sie den ganzen Bildschirm füllt.

```yaml
views:
  - title: Haus
    panel: true
    cards:
      - type: custom:painting-card
        # Konfiguration wie oben
```

Die Schriften stecken in der Kartendatei; die Karte lädt nichts von fremden Servern.

#### Lizenz

Apache License 2.0, Copyright 2026 Axel Eble. Wer die Karte weitergibt oder verändert, liefert `LICENSE` und `NOTICE` mit.