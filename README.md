# pilz-wetter

Tageswerte von Lufttemperatur und Niederschlag für vier Stationen am Bodensee sowie
Perzentile der Niederschlagssummen je Monat und Fenster (Referenz innerhalb 1991–2020).
Die Datenstrecke erzeugt die Dateien täglich automatisch; den Zeitpunkt des letzten Laufs
nennt `stand.json`. Fehlt `stand.json`, ist noch kein Lauf erfolgt. Das Repository enthält
ausschliesslich Wetterdaten.

| Datei | Inhalt |
| --- | --- |
| `reihen/<Station>.csv` | Tagesreihe ab 1990-12-01: `datum,tmit,tmax,tmin,rr,q` (°C, mm; `q`: `hist` historisches Archiv, `akt` laufendes Fenster); der Dezember 1990 dient nur als Vorlauf für die Januar-Fenster |
| `perzentile.json` | P25, P50, P75 der rollenden Niederschlagssummen über 2, 10, 14, 26 und 28 Tage je Kalendermonat; `rr_regentag` nur 2-Tage-Fenster mit mindestens einem Tag ≥ 1 mm; mindestens 10 Referenzjahre |
| `pilz-wetter.js` | dieselben Perzentile und die Tagesreihe ab 1. Januar des Vorjahres als JavaScript |
| `stand.json` | Zeitpunkt des Laufs, erster und letzter Tag je Station |

Stationen: DWD 06263 Singen, DWD 06107 Sipplingen, DWD 02712 Konstanz, MeteoSchweiz GUT Güttingen.

## Quellen und Lizenzen

- Datenbasis: Deutscher Wetterdienst (DWD), Climate Data Center, Tageswerte KL
  (https://opendata.dwd.de/climate_environment/CDC/), Nutzung nach der Verordnung zur
  Festlegung der Nutzungsbestimmungen für die Bereitstellung von Geodaten des Bundes (GeoNutzV).
- Quelle: MeteoSchweiz, Open Government Data SwissMetNet (https://data.geo.admin.ch/ch.meteoschweiz.ogd-smn/),
  Lizenz CC BY 4.0.

Verändert: Spalten ausgewählt (Tagesmittel, Maximum und Minimum der Lufttemperatur in 2 m,
Niederschlag), historische und laufende Archive zusammengeführt, Perzentile berechnet. Der
DWD-Niederschlag eines Datums gilt von 06 UTC dieses Tages bis 06 UTC des Folgetags (ab
2001-04-01, davor 07:30 bis 07:30 MEZ); bei MeteoSchweiz ist es der Parameter `rre150d0`.
