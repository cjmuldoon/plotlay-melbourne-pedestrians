# Opening the Power BI report

`MelbourneFootpaths.pbip` is a Power BI Project: the report (`MelbourneFootpaths.Report`, PBIR format) and the semantic model (`MelbourneFootpaths.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). Projects and the PBIR report format are standard in current versions. If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine. In Parallels the Mac home folder is usually shared as `\\Mac\Home`.

## Open it

1. In Windows, go to the repo's `powerbi` folder and double-click `MelbourneFootpaths.pbip`, or in Desktop use **File, Open report, Browse reports** with the file type set to *Power BI project files (\*.pbip)*.
2. The visuals are empty the first time. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-melbourne-pedestrians/main/data/`). If Desktop asks how to connect, choose **Anonymous** and then **Connect**.
   - To read the files from disk instead, go to **Home, Transform data, Edit parameters** and set `DataFolder` to the full path of the repo's `data` folder, **ending with a backslash** (for example `\\Mac\Home\plotlay-melbourne-pedestrians\data\`). Then select **OK** and **Apply changes**.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** (all the sources are public open data).
3. Check the numbers (below).

## Check the numbers after Refresh

After **Refresh**, with the slicers on **17:00** and **Weekday** (as the report opens; each takes one value at a time):

| Through a day card | Should read |
|---|---|
| People an hour, all sensors, hour picked | 67,022 (Power BI may show 67.0K) |
| Median m² each, compared footpaths, hour picked | 18.6 |
| Median m² each at each footpath's busiest hour, day type picked | 16.2 (the README's weekday median) |

The bar chart lists the 8 compared footpaths with the most people an hour per metre of width at 5 pm, busiest first: Spencer St-Collins St (North) 534, Southern Cross Station 524, Bourke St - Spencer St (North) 503, and so on.

Set Day to **Weekend** and Hour to **15:00** (the weekend peak): the cards read 60,716, 18.8 and 18.1. Set Hour to **04:00** on a weekday: 1,051 people an hour and a median 1,420.6 m² each.

On **Weekday city, weekend city**, Southern Cross Station's bar reads 0.16 and Lygon St (West) 1.58.

## The model

- **Sensors**: one row per sensor (`data/counts/sensors.csv`), with the zone's width and the checks. All the measures live here.
- **Hours**: `hour_of_day.csv` unpivoted in Power Query to one row per sensor, hour and day type (Weekday, Weekend).
- **Daily**: people a day per sensor.
- **HourOfDay** (0 to 23, with a `clock` label sorted by the hour) and **DayType**: for the slicers and the play axis.
- Measures: `People an hour`, `Width m`, `People an hour per m`, `m2 each` (1.2 m/s × 3600 ÷ people an hour per metre), `Median m2 each`, `Median m2 each at peak`, `Car spaces each`, `Weekend vs weekday`, `People a day`. Every people-an-hour measure counts one hour on one day type: the ones picked on the slicers, or 17:00 on a weekday when none is (`Hour shown`, `Day type shown`). The medians are over the compared footpaths of each footpath's own value.

## The two maps

- **Shape Map** (page 1). The custom map is saved inside the report (`MelbourneFootpaths.Report/StaticResources/RegisteredResources/footpath_zones.topojson`, a copy of `data/geometry/footpath_zones.topojson` that keeps only the sensor code), so the footpath zones draw on open, coloured by people an hour per metre of width. It matches `Sensors[map_key]` (the sensor code as text). The Shape Map has no base map of its own, so the saved file also carries the context: the street outlines (City of Melbourne road corridors) and suburbs (ABS) in a pale ground tone, the Yarra and Victoria Harbour showing as the map's pale blue background (cut out using ABS water mesh blocks and Vicmap Hydro), and a coloured ring round each footpath zone, as the zones are too thin to see at city scale. If the map shows the United States instead (an older Desktop can ignore a saved map): select the map, **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\geometry\footpath_zones.topojson`. Under **View map keys** the sensor codes (4, 41, 66...) should be matched. The plain file from `data/` has no context and no rings; the zones are thin, so a border width of 2 or more helps. If the Shape Map isn't in the Visualizations pane at all, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- **Azure Maps** (Map of the sensors page). The sensors are plotted from their latitude and longitude (averaged, one point per sensor), with the sensor code in Location and the name in the tooltip. Your organisation's admin must allow Azure Maps (tenant setting "Use Azure Maps visual"); a personal account has it on by default. The footpath outlines are a reference layer, which Power BI can't take from a file in the project, so add it by hand: select the map, **Format visual**, **Visual**, **Reference layer**, **Data source: File** (or **URL**), and choose `..\data\geometry\footpath_zones.geojson`. Its `key` property is the sensor code, the same value as the map's Location. Use the **Grayscale (light)** or **Blank** style for screenshots; don't publish satellite basemap screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Sensors[map_key]`, Color saturation `[People an hour per m]`.
- Azure Maps: Location `Sensors[map_key]`, Latitude `Sensors[latitude]` (Average), Longitude `Sensors[longitude]` (Average), Size `[People an hour]`, Tooltips `Sensors[sensor_name]` (First).

## Save a .pbix too

After the refresh, use **File, Save as** with the type **Power BI file (\*.pbix)** to save `powerbi\MelbourneFootpaths.pbix`. It includes the data, so it opens without a refresh.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Use the Snipping Tool (Win+Shift+S) for stills and the Xbox Game Bar (Win+Alt+R) or Clipchamp for clips. Save them to `powerbi/screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Through a day, 5 pm weekday | Hour 17:00, Day Weekday (as it opens) | still |
| 2 | Through a day, 4 am | Hour 04:00: the map goes pale | still |
| 3 | The hour stepping | Pick 06:00, 08:00, 12:00, 17:00, 21:00 in the Hour dropdown, about 1 s each | 6 to 8 s clip |
| 4 | Play the day | Press the play button under the scatter chart and let it run 0 to 23 | 10 to 15 s clip |
| 5 | Weekday city, weekend city | The ranked bar chart with the Southern Cross and Lygon St lines | still |
| 6 | Busiest days | The daily line, hovering over Saturday 7 March 2026 | still |
| 7 | Map of the sensors | Azure Maps with the reference layer, grayscale | still |
| 8 | Model view and Power Query | The five tables and their relationships; the Hours query's unpivot step | 2 stills |

Keep the footer with the attribution visible in every shot.

## What's in the folder

- `MelbourneFootpaths.pbip`: open this.
- `MelbourneFootpaths.Report/`: the pages and visuals (PBIR JSON) and the Plotlay theme.
- `MelbourneFootpaths.SemanticModel/`: the tables, the Power Query (M) and the measures (TMDL).
