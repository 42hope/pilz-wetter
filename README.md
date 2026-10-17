# pilz-wetter

Tageswerte von Niederschlag und Lufttemperatur für die Pilz-App am Bodensee sowie Perzentile der
Niederschlagssummen je Monat und Fenster (Referenz innerhalb 1991–2020, mindestens 10 Jahre).
Die Datenstrecke `42hope/pilz-datenstrecke` erzeugt alle Dateien täglich automatisch, auch dieses
README; den Zeitpunkt des letzten Laufs nennt `stand.json`. Das Repository enthält ausschliesslich
Wetterdaten.

## Dateien

| Datei | Inhalt |
| --- | --- |
| `reihen/<Kennung>.csv` | Tagesreihe ab 1990-12-01: `datum,tmit,tmax,tmin,rr,q` (°C, mm; `q`: `hist` historisches Archiv, `akt` laufendes Fenster, `hyras` Rasterwert); der Dezember 1990 dient nur als Vorlauf für die Januar-Fenster; Messpunkte nur `rr` |
| `perzentile.json` | P25, P50, P75 der rollenden Niederschlagssummen über 2, 10, 14, 26, 28 Tage je Kalendermonat, getrennt nach `stationen` und `messpunkte`; `rr_regentag` nur 2-Tage-Fenster mit mindestens einem Tag ≥ 1 mm |
| `pilz-wetter.js` | dieselben Perzentile und die Tagesreihe ab 1. Januar des Vorjahres als JavaScript |
| `stand.json` | Zeitpunkt des Laufs, Fehlerzahl, je Station und Messpunkt erster und letzter Tag und `werte_akt` |
| `pruefung-konstanz.json` | Jahressummen der Rasterzelle an der Station Konstanz gegen die Station (aus dem letzten Historienlauf) |

## Stationen (Prüfmass)

| Kennung | Name | Quelle | Grössen |
| --- | --- | --- | --- |
| `DWD-06263` | Singen | DWD Tageswerte KL | Temperatur, Niederschlag |
| `DWD-02712` | Konstanz | DWD Tageswerte KL | Temperatur, Niederschlag |
| `MCH-GUT` | Güttingen | MeteoSchweiz SwissMetNet | Temperatur, Niederschlag |

## Messpunkte

| Kennung | Name | Quelle | Grössen |
| --- | --- | --- | --- |
| `BR-W` | Bodanrück West/Zentral | DWD HYRAS-DE-PR, Rasterzelle 1 km | Niederschlag |
| `BR-O` | Bodanrück Ost | DWD HYRAS-DE-PR, Rasterzelle 1 km | Niederschlag |
| `BR-F` | Bodanrück Feucht-/Tieflage | DWD HYRAS-DE-PR, Rasterzelle 1 km | Niederschlag |
| `HG-B` | Hegaualb Engen-Bargen | DWD HYRAS-DE-PR, Rasterzelle 1 km | Niederschlag |
| `HG-S` | Hegau Braunerde Vulkantuff | DWD HYRAS-DE-PR, Rasterzelle 1 km | Niederschlag |
| `HG-K` | Hegau Pararendzina Deckentuff | DWD HYRAS-DE-PR, Rasterzelle 1 km | Niederschlag |

## Quellen und Lizenzen

- Datenbasis: Deutscher Wetterdienst (DWD), Climate Data Center (https://opendata.dwd.de/climate_environment/CDC/):
  Tageswerte KL der Stationen und HYRAS-DE-PR (Rasterdaten des täglichen Niederschlags, 1 km,
  https://opendata.dwd.de/climate_environment/CDC/grids_germany/daily/hyras_de/precipitation/; gelesene Version v6-1). Nutzung nach der Verordnung zur Festlegung der
  Nutzungsbestimmungen für die Bereitstellung von Geodaten des Bundes (GeoNutzV).
- Quelle: MeteoSchweiz, Open Government Data SwissMetNet (https://data.geo.admin.ch/ch.meteoschweiz.ogd-smn/),
  Lizenz CC BY 4.0.

Verändert: Spalten ausgewählt (Tagesmittel, Maximum und Minimum der Lufttemperatur in 2 m,
Niederschlag), historische und laufende Archive zusammengeführt, je Messpunkt die nächstgelegene
HYRAS-Rasterzelle ausgewählt, Perzentile berechnet.

## Grenzen

- HYRAS ist aus Stationsmessungen interpoliert und gibt den Mittelwert einer Zelle von 1 km wieder,
  nicht einen Punkt; kleinräumige Schauer können fehlen oder verschmiert sein.
- HYRAS deckt nur Deutschland ab. Messpunkte ausserhalb erhalten keine Werte.
- Der DWD stellt die Datei des laufenden Jahres täglich neu bereit und kann Werte nachträglich
  ändern; jeder Lauf übernimmt das laufende Jahr und das Vorjahr neu. Der jüngste Tag ist in der
  Regel der Vortag.
- Zeitbezug eines HYRAS-Tages laut Datei: 2026-01-01 06:00 bis 2026-01-02 06:00 (Zeitzone laut Datei: hours since 1931-01-01 00:00 +0:00). Der DWD-Stationsniederschlag
  eines Datums gilt von 06 UTC dieses Tages bis 06 UTC des Folgetags (ab 2001-04-01, davor 07:30
  bis 07:30 MEZ); bei MeteoSchweiz ist es der Parameter `rre150d0`.
