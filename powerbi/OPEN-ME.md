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
3. After the refresh, check page 1. Pick **17:00** and **Weekday** on the slicers: the line chart peaks at about the README's weekday 5 pm figure. Pick **Southern Cross Station** on the Weekday city page: weekend ÷ weekday reads 0.16. If both match, everything is joined correctly.

## The model

- **Sensors**: one row per sensor (`data/counts/sensors.csv`), with the zone's width and the checks. All the measures live here.
- **Hours**: `hour_of_day.csv` unpivoted in Power Query to one row per sensor, hour and day type (Weekday, Weekend).
- **Daily**: people a day per sensor.
- **HourOfDay** (0 to 23, with a `clock` label sorted by the hour) and **DayType**: for the slicers and the play axis.
- Measures: `People an hour`, `Width m`, `People an hour per m`, `m2 each` (1.2 m/s × 3600 ÷ people an hour per metre), `Car spaces each`, `Weekend vs weekday`, `People a day`.

## Two map settings Power BI asks for by hand

Power BI only keeps a custom map file and a reference layer once you have chosen them in Desktop, so do this once and save:

- **Shape Map** (page 1). Select the map. In **Format visual, Visual, Map settings**, set the map type to **Custom map**, choose **Add map type**, and pick `..\data\geometry\footpath_zones.topojson`. Under **View map keys** the sensor ids (4, 41, 66...) should match. Zoom in on the city. If the Shape Map isn't in the Visualizations pane, turn it on in **Options, Preview features, Shape map visual**. The zones are thin, so set the map's border width to 2 or more.
- **Azure Maps** (Map of the sensors page). Your organisation's admin must allow it (tenant setting "Use Azure Maps visual"); a personal account has it on by default. Select the map, then go to **Format visual, Reference layer**, set the data source to **File** and choose `..\data\geometry\footpath_zones.geojson`, with join property `key`. Use the **Grayscale (light)** or **Blank** map style for screenshots. Don't publish satellite basemap screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Sensors[sensor_key]`, Color saturation `[People an hour per m]`.
- Azure Maps: Location `Sensors[sensor_name]`, Latitude and Longitude, Size `[People an hour]`.

Then save with **File, Save**.

## Save a .pbix too

After the refresh and the map settings, use **File, Save as** with the type **Power BI file (\*.pbix)** to save `powerbi\MelbourneFootpaths.pbix`. It includes the data, so it opens without a refresh.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Use the Snipping Tool (Win+Shift+S) for stills and the Xbox Game Bar (Win+Alt+R) or Clipchamp for clips. Save them to `powerbi/screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Through a day, 5 pm weekday | Hour 17:00, Day Weekday | still |
| 2 | Through a day, 4 am | Hour 04:00: the map goes pale | still |
| 3 | The hour stepping | Click 06:00, 08:00, 12:00, 17:00, 21:00 on the slicer, about 1 s each | 6 to 8 s clip |
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
