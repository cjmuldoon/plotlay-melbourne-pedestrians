# Data

All files are CC BY 4.0 (see `../LICENSE-DATA` and `../NOTICE`). Coordinates are WGS 84 longitude and latitude.

## geometry/

The 53 footpath zones, exported from Plotlay. `key` is the sensor's `location_id` on the City of Melbourne
portal. Each zone carries `area_m2`, `perimeter_m`, `source` (where its outline came from) and the formula columns
worked out in Plotlay: `width_m`, `weekday_day`, `weekend_day`, `weekend_vs_weekday` and `m2_each_at_peak`.

- `footpath_zones.geojson`: for Azure Maps, QGIS and Python.
- `footpath_zones.topojson`: for the Power BI Shape Map (object `footpaths`).
- `footpath_zones_wkt.csv`: one row per zone with its outline as WKT and its centre.

## counts/

- `sensors.csv`: the 57 sensors with at least 300 complete days from 1 November 2025 to 31 October 2026. Columns:
  - `zone_made_by` and `council_feature_id`: how the zone was made.
  - `zone_check`: `ok`, `adjusted`, `can’t verify` or `no zone`, with a `note` for every zone that isn't ok.
  - `compare`: true when the zone is used in comparisons.
  - The zone's numbers.
- `hour_of_day.csv`: the average people in each hour of the day, weekdays and weekends apart, as linked in Plotlay
  from the portal's API.
- `daily_totals.csv`: people a day per sensor, with `hours_reported` (24 is a complete day).

The hour averages were linked in Plotlay from these addresses. Refresh reads them again, and the 12 months are fixed
so the numbers repeat:

```
https://data.melbourne.vic.gov.au/api/explore/v2.1/catalog/datasets/pedestrian-counting-system-monthly-counts-per-hour/exports/csv?select=location_id%2Chourday%2Cavg(pedestriancount)%20as%20weekday_people&group_by=location_id%2Chourday&where=location_id%20in%20(4%2C41%2C66%2C35%2C3%2C84%2C5%2C181%2C24%2C184%2C47%2C59%2C45%2C185%2C2%2C58%2C133%2C53%2C19%2C67%2C162%2C9%2C79%2C63%2C61%2C134%2C135%2C56%2C21%2C30%2C17%2C52%2C49%2C20%2C29%2C23%2C143%2C182%2C72%2C62%2C187%2C36%2C117%2C77%2C31%2C141%2C132%2C109%2C131%2C107%2C27%2C51%2C179)%20and%20sensing_date%3E%3Ddate'2025-11-01'%20and%20sensing_date%3Cdate'2026-11-01'%20and%20date_format(sensing_date%2C'E')!%3D'Sat'%20and%20date_format(sensing_date%2C'E')!%3D'Sun'&delimiter=%2C

https://data.melbourne.vic.gov.au/api/explore/v2.1/catalog/datasets/pedestrian-counting-system-monthly-counts-per-hour/exports/csv?select=location_id%2Chourday%2Cavg(pedestriancount)%20as%20weekend_people&group_by=location_id%2Chourday&where=location_id%20in%20(4%2C41%2C66%2C35%2C3%2C84%2C5%2C181%2C24%2C184%2C47%2C59%2C45%2C185%2C2%2C58%2C133%2C53%2C19%2C67%2C162%2C9%2C79%2C63%2C61%2C134%2C135%2C56%2C21%2C30%2C17%2C52%2C49%2C20%2C29%2C23%2C143%2C182%2C72%2C62%2C187%2C36%2C117%2C77%2C31%2C141%2C132%2C109%2C131%2C107%2C27%2C51%2C179)%20and%20sensing_date%3E%3Ddate'2025-11-01'%20and%20sensing_date%3Cdate'2026-11-01'%20and%20(date_format(sensing_date%2C'E')%3D'Sat'%20or%20date_format(sensing_date%2C'E')%3D'Sun')&delimiter=%2C
```

Sensor locations:

```
https://data.melbourne.vic.gov.au/api/explore/v2.1/catalog/datasets/pedestrian-counting-system-sensor-locations/exports/csv?select=location_id%2Csensor_description%2Clatitude%2Clongitude&where=location_id%20in%20(4%2C41%2C66%2C35%2C3%2C84%2C5%2C181%2C24%2C184%2C47%2C59%2C45%2C185%2C2%2C58%2C133%2C53%2C19%2C67%2C162%2C9%2C79%2C63%2C61%2C134%2C135%2C56%2C21%2C30%2C17%2C52%2C49%2C20%2C29%2C23%2C143%2C182%2C72%2C62%2C187%2C36%2C117%2C77%2C31%2C141%2C132%2C109%2C131%2C107%2C27%2C51%2C179)&delimiter=%2C
```
