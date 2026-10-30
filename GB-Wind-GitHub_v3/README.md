# GB Wind Evidence

**Explore land passing an onshore wind screen, alongside operational and planned projects across Great Britain.**

GB Wind Evidence combines spatial screening data with a project layer from the supplied **Onshore Wind - REPD Q2 2026** worksheet. It helps investigate two questions:

1. Where are operational, under-construction and planned onshore wind projects located?
2. How much screened land remains after allowing for their estimated layout areas?

> “Remaining land” is a model result. It does not establish that land is unoccupied, available, consentable or suitable for construction. Project circles are estimated layout areas, not surveyed boundaries.

## Open or publish

Extract the ZIP and open `index.html` in a current browser. The dashboard is self-contained, works offline and needs no build command, API key or backend. Its HTML file is approximately **14.7 MB**, below the requested 25 MB limit. Allow a moment for the data and initial calculations to load.

To publish on GitHub Pages:

1. Upload the **extracted contents** to the top level of your repository. Keep the website filename exactly `index.html`.
2. Open **Settings → Pages → Deploy from a branch**.
3. Select **main → / (root)**, or your equivalent publishing branch, and save.
4. Open the website address provided by GitHub after deployment.

Uploading the ZIP alone does not publish the website. See [GitHub’s publishing guide](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site). GitHub Free supports Pages for public repositories.

The package includes `index.html`, this README, an updated study note and `.nojekyll`. It does not contain the source workbook or its other worksheets.

## What is new

- **Project markers by development group:** operational, under construction, and planned / other active.
- **A searchable project list:** inspect the source status, recorded capacity, region, REPD reference and missing data; locate projects or export the filtered list.
- **Independent project allowances:** use Exclude, Flag only or Off for each development group.
- **A before/after land comparison:** hold the existing spatial assumptions constant and measure the effect of selected project allowances, including further land lost below the minimum wind-farm size.
- **Optional estimated circles:** display the assumed layout areas separately from the project markers. Display switches do not change the calculation.

All earlier wind, terrain, landscape, settlement and NATS controls remain available. This settlement-only update adds independent **England 600 m, Wales 500 m and Scotland 600 m** controls, source notes and a settlement-only reset. Wind-speed settings, source data and blade-tip height remain unchanged.

## Project selection

Only the worksheet **`Onshore Wind - REPD Q2 2026`**, in the workbook’s Raw Data section, supplies the project data. The filter uses:

- Technology Type = **Wind Onshore**.
- Country = **England, Wales or Scotland**.
- Installed Capacity (MWelec) **≥10 MW or blank**.
- An included development status, as shown below.

| Display group | Included source statuses | GB records |
| --- | --- | ---: |
| Operational | Operational | 335 |
| Under construction | Under Construction | 26 |
| Planned / other active | Planning Application Submitted; Planning Permission Granted; Appeal Granted; Secretary of State - Granted; No Application Required | 289 |
| **Total** | | **650** |

“No Application Required” is grouped with planned / other active because it does not establish operational or construction status. Its exact status remains visible. Refused, withdrawn, expired, abandoned, decommissioned and **Revised** records are excluded. DESNZ identifies Revised records as superseded applications linked to another application in the database; see the [REPD publication page](https://www.gov.uk/government/publications/renewable-energy-planning-database-quarterly-extract).

There are **130 included records in England, 64 in Wales and 456 in Scotland**. These are distinct REPD IDs, not necessarily distinct physical sites. Extensions and repowering proposals can overlap existing developments. Recorded capacities should not be interpreted as net additional capacity or a delivery forecast.

### Missing data

- **646 records have usable coordinates** and can appear on the map.
- **41 have blank capacity.** They remain included in counts and lists, and on the map where located. Their capacity is unknown, not zero.
- **Four have no coordinates.** They remain in the list and counts but cannot be mapped: Alwen Forest Project (14796), Back Fell Wind Farm (14546), Scout Moor 2 (17672), and Hare Hill Wind Farm Repowering (15385).
- No area allowance is assigned to a record with missing capacity or coordinates.

One mapped point, Tilbury Docks (3982), lies outside a sampled country-mask cell. Its supplied location is retained; the model counts only its estimated circle’s intersection with sampled land. Coordinates are not snapped or invented.

## How the project-land estimate works

The worksheet supplies points, not wind-farm boundary polygons. For each located project with known capacity, the model applies the existing **10 MW/km²** density assumption:

**Estimated layout area (km²) = recorded capacity (MW) ÷ 10**

That area is represented by a circle centred on the supplied British National Grid coordinate. Its radius in kilometres is `sqrt(capacity / 10 / π)`. A 10 MW record therefore receives a 1 km² circle, with a radius of approximately 564 m.

This represents full wind-farm layout area, including spacing, rather than turbine foundations. It is neither a statutory setback nor a claim about the project’s actual boundary or turbine arrangement. Actual footprints may extend beyond the circle while leaving land inside it unused.

The model marks 250 m cells whose centres fall within the circles. It unions overlapping allowances, clips them to sampled land and combines them with the other selected exclusions. The minimum connected-area test then runs again. A circle on already excluded land does not subtract that land twice.

The comparison distinguishes:

1. **Before project allowances:** land passing the other selected constraints and the minimum-size test.
2. **Direct overlap:** previously qualifying land removed by selected project circles.
3. **Further size loss:** remaining cells that become part of groups smaller than the minimum after the circles are removed.
4. **After selected allowances:** land that still passes both tests.

All three project groups default to **Exclude**. **Flag only** retains their estimated areas with review flags; **Off** ignores their allowances. Markers can remain visible in either mode.

## Reading the dashboard

Choose a study area, adjust the constraints, and compare the percentages above the map. Both percentages use the same denominator: sampled country-mask area in the selected scope.

- **Green** passes without an active review flag; **amber** passes with a review flag.
- **Grey** is excluded; **sand** falls below the minimum connected-area size.
- Project markers use **blue circles** for operational, **orange squares** for construction and **purple diamonds** for planned / other active records. A white centre indicates unknown capacity.
- Click a marker for project details or a land cell for its screening result. At wider zoom levels, clicking can list several nearby records.
- Use **Projects** for the searchable list and **View breakdown** for the land comparison and country results.
- **Export results** includes the current project-land comparison. **Save a copy** preserves the scenario and embedded data in an HTML file.

The project list follows the selected study area. Its search and status filter affect the list/export, not the land calculation. Marker and circle visibility also leave the calculation unchanged.

## Spatial data and assumptions

| Input | Role / default |
| --- | --- |
| ONS December 2025 country boundaries | GB study area and denominator; includes some inland waters |
| Supplied `GBR_wind-speed_150m.tif` | Wind ≥7 m/s; 150 m height inferred from filename, provider and vintage unconfirmed |
| OS Terrain 50 | Maximum terrain slope ≤15°; maximum of 25 underlying 50 m slope values per cell |
| National Parks | Excluded by default |
| National Landscapes / AONBs and Scottish National Scenic Areas | Excluded by default |
| OS Open Built Up Areas | 8,716 features; excluded with England 600 m, Wales 500 m and Scotland 600 m screening buffers |
| NATS safeguarding polygons | Primary/secondary radar, air–ground communications and navigation aids; Flag only by default |
| Radar blade-tip height | 120 m above ground |
| Minimum wind-farm size | 10 MW, equivalent to 1 km² at 10 MW/km² |

The grid contains **3,680,510 cells**, each 250 m × 250 m, or 0.0625 km². Country, landscape and NATS membership are sampled at cell centres. Wind uses bilinear interpolation and settlement buffers use approximate raster distances.

Cells connect through shared edges; diagonal contact alone does not connect them. The default minimum therefore requires 16 connected cells. Missing information required by an active exclusion cannot pass. Unassessed constraints do not establish clearance. Overlap areas must not be summed because constraints can coincide.

The wind raster’s height is independent of the radar tip-height selector. Changing tip height does not recalculate wind speed. Buffers and designation exclusions are scenario assumptions, not universal planning requirements.

## Settlement controls and evidence

Country-specific settlement assumptions
England 600 m: Arup South Warwickshire Local Plan Climate Evidence, 7 May 2024, uses 600 m around major settlements. This is a local strategic screening precedent, not a national setback. EN-3 (2025), paragraph 2.12.16, addresses appropriate residential separation case by case.
Wales 500 m: Welsh Government Planning for Renewable and Low Carbon Energy: A Toolkit for Planners (September 2015), page 139, proposes 500 m around dwellings for a 2 MW machine. Here it is adapted to built-up areas; adequacy for larger turbines is not established. The toolkit itself distinguishes screening buffers from noise limits.
Scotland 600 m: dashboard modelling assumption matching England's starting distance; not a Scottish Government-prescribed setback. NPF4 Policy 11(e) requires assessment of residential amenity, visual impact, noise and shadow flicker. The older up-to-2 km community separation approach was for visual assessment, not a prohibition. NPF4 replaced Scottish Planning Policy.
The cell's country selects the buffer. Distance is to the nearest mapped GB built-up area, including across national borders. A country border does not trigger a buffer by itself. Distances use the existing approximate 250 m raster. Built-up areas do not cover all individual dwellings. Controls accept 0-10,000 m and retain Off / Flag only / Exclude. Reset settlement distances changes only these three values. Save a copy, metadata and CSV exports preserve all three settings.
Sources:
https://www.southwarwickshire.org.uk/doc/213168/name/Task%203%20Decentralised%20energy%20report%20ISSUED.pdf
https://www.gov.uk/government/publications/national-policy-statement-for-renewable-energy-infrastructure-en-3-2025/national-policy-statement-for-renewable-energy-infrastructure-en-3-2025-accessible-webpage
https://www.gov.wales/sites/default/files/publications/2018-09/renewable-energy-toolkit.pdf
https://www.gov.scot/publications/national-planning-framework-4/pages/3/
https://www.gov.scot/publications/onshore-wind-planning-faq/

## Default findings

Results for the **30 October 2026 release**, with all three project allowances excluded. Country figures are allocated from the GB-wide run.

| Area | Before project allowances | After estimated allowances |
| --- | ---: | ---: |
| Great Britain | 44.1% | 43.3% |
| England | 41.4% | 41.2% |
| Wales | 39.4% | 38.9% |
| Scotland | 50.0% | 47.9% |

In GB, retained land changes from **101,536.8 km² to 99,590.7 km²**. The reduction comprises **1,901.7 km²** of direct project overlap and **44.4 km²** of further minimum-size loss. With the same project allowances, a 100 MW minimum leaves **41.8%**; excluding all NATS categories leaves **17.2%**. These are sensitivity scenarios, not development prohibitions.

The modest change in the national percentage is a result of this screening model. It does not prove that enough attractive new-build sites exist. Missing project information, excluded smaller projects, actual layouts and unassessed constraints could materially change site-level conclusions. Restricting the study area can also change how cross-border connected areas qualify.

## Limits and provenance

Grid connection remains outside scope. Heritage, ecology and peat, isolated homes, detailed noise and shadow flicker, complete airport/MOD safeguarding, low flying, private communications, water/flooding, access, ownership and economics are not comprehensively assessed. No turbine layouts, wake losses, energy yield, consent likelihood or deliverable capacity are calculated. Narrow cell connections may not support practical layouts. Radar bands above 200 m remain provisional.

The project layer is the **Q2 2026 snapshot as supplied**, not a live check of current project status. Known-capacity projects below 10 MW are deliberately omitted. Blank-capacity and unlocated projects cannot be deducted. Consequently, the result must not be described as land confirmed free of existing or proposed wind farms.

The selected workbook sheet was extracted without changing the workbook. Source row numbers, exact development statuses and REPD IDs are retained. Scenario metadata includes source fingerprints, selection rules and current constraints.

Workbook SHA-256: `6f5fbb4171537b39b736ef4b9776f07615537f227790826bab893879808356fc`.

## Validation and attribution

Settlement-update checks verified independent country thresholds, invalid inputs, settlement-only reset, saved country settings, CSV and metadata exports, and agreement with independent Python calculations. Restoring 250 m in all three countries reproduces the previous baseline. Wind, aviation and settlement source payloads were verified unchanged. Wind settings and blade-tip-height defaults were preserved.

Checks covered source selection, blank capacities, unique REPD IDs, coordinate handling and exact agreement of every project-circle grid flag with an independent Python calculation. GB and country results matched independent calculations for the default, 100 MW, operational-only, England-only and NATS-exclusion scenarios. Flag-only mode restored the spatial baseline. Searches, missing-data display, project inspection, scenario saving and JavaScript syntax were checked. Browser layout and live GitHub Pages hosting were not verified in this environment.

The original grid remains losslessly compressed. No cells or spatial precision were removed to reduce file size.

Contains Ordnance Survey data © Crown copyright and database right 2026; ONS country boundaries; national designation data from Natural England, Natural Resources Wales and Scottish Government / NatureScot; NATS safeguarding data; the supplied wind raster; and DESNZ REPD data supplied via the workbook. Original source terms and attributions continue to apply. The project owner previously confirmed NATS publication permissions and suitability of the wind-data licence. No additional application-code licence is specified here.

Use this README, the included study note and the dashboard’s embedded method for this release. Earlier documents with a single GB-wide settlement buffer describe previous versions.
