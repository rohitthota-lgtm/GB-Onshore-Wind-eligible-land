# GB Wind Evidence

**Explore how spatial constraints affect the land potentially suitable for onshore wind across Great Britain.**

GB Wind Evidence is an interactive map for investigating where land passes a configurable onshore wind screening test. It brings wind, terrain, protected landscapes, settlements and NATS safeguarding data into a single dashboard, with separate controls for each constraint.

Its central question is: **how much land remains when we apply different siting assumptions?** The dashboard supports early research, comparison and discussion across England, Wales and Scotland. Grid connection is outside its scope.

> The headline percentage describes **land passing the selected spatial screen**. It does not establish that land is available, consentable or suitable for construction. Connected screening areas are not development sites or land parcels.

## Get started

### Open locally

Download `index.html` and open it in a current browser that supports `DecompressionStream`, such as a recent Chrome, Edge, Firefox or Safari. Allow a moment for the embedded data to load and the initial screen to run.

The dashboard is a self-contained HTML file of approximately **14.5 MB**. Its map data and calculations run locally in the browser; no installation, build step, API key or backend is required. The core dashboard also works offline.

### Publish with GitHub Pages

1. Upload `index.html` and this `README.md` to the top level of your GitHub repository. Ensure the HTML file is named exactly `index.html`.
2. In the repository, open **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**.
4. Choose **main** and **/ (root)**, then click **Save**. Use the corresponding branch if your repository has a different default.
5. Once deployment finishes, open the website address shown by GitHub Pages.

See [GitHub’s publishing instructions](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) for further details. On GitHub Free, Pages is available for public repositories.

The repository contains the website file; enabling Pages is what publishes it as a website. After deployment, check that the default Great Britain result is **49.8%**, with a **120 m** blade-tip height and **250 m** settlement buffer.

## Using the dashboard

1. **Choose a study area:** Great Britain, England and Wales combined, or an individual nation.
2. **Set your assumptions:** adjust wind speed, slope, settlement buffer, blade-tip height and minimum wind-farm size.
3. **Choose how each constraint is treated:**

   | Setting | Effect |
   | --- | --- |
   | **Exclude** | Removes affected cells from the passing area. |
   | **Flag only** | Keeps affected cells in the screen and marks them for review. |
   | **Off** | Ignores that constraint in the scenario. |

4. **Explore the map:** pan, zoom and click a land cell to see its values and screening result. The map-layer selector also shows wind, slope, landscapes, settlement buffers and NATS overlaps.
5. **Inspect the evidence:** use **View breakdown** for national results, constraint overlaps and connected screening areas. Use **Export results** for a CSV, **Save a copy** to preserve your scenario in an HTML file, or **How it works** for the embedded methodology and downloadable study note.

In the main screening layer, green and amber both count as passing land: amber carries an active review flag. Grey is excluded and sand falls below the minimum size. Use the map key when switching layers, as colours have different meanings in other views.

## Default assumptions

| Parameter | Default |
| --- | --- |
| Study area | Great Britain |
| Screening resolution | 250 m × 250 m cells |
| Minimum wind speed | 7 m/s in the supplied 150 m wind raster |
| Maximum terrain slope | 15° |
| National Parks | Exclude |
| National Landscapes / AONBs and Scottish National Scenic Areas | Exclude |
| Built-up areas | Exclude, including an illustrative 250 m buffer |
| NATS constraints | Flag only |
| Blade-tip height for primary radar screening | 120 m above ground |
| Minimum wind-farm size | 10 MW |
| Assumed power density | 10 MW/km² of full wind-farm layout area |

At the assumed density, a 10 MW minimum requires **1 km²**, equivalent to **16 connected cells**. This is an area-based screening proxy, not a turbine-layout design or a capacity estimate. Full layout area is different from the land physically occupied by foundations and infrastructure.

The wind raster height and radar blade-tip height are separate inputs. Changing blade-tip height does **not** recalculate wind speed. Buffers and designation exclusions are scenario assumptions, not universal planning requirements.

## Datasets

The release uses processed snapshots of the supplied datasets, rather than live feeds.

| Dataset | Role in the screen | Qualification |
| --- | --- | --- |
| ONS December 2025 country boundaries | Defines England, Wales and Scotland and the area denominator | Country polygons include some inland waters. Northern Ireland is outside scope. |
| `GBR_wind-speed_150m.tif` | Wind-speed threshold | Height is inferred from the filename; provider and model vintage are unconfirmed. |
| OS Terrain 50 | Terrain slope | Uses the maximum of 25 underlying 50 m slope values per screening cell. |
| National Parks designation files | Landscape constraint | Uploaded England, Wales and Scotland snapshots. |
| National Landscapes / AONBs and Scottish National Scenic Areas | Landscape constraint | Uploaded national designation snapshots; these do not capture all landscape sensitivity. |
| OS Open Built Up Areas | Settlement footprint and distance buffer | 8,716 features; does not comprehensively represent isolated homes. |
| NATS safeguarding polygons | Primary radar, secondary radar, air–ground communications and navigation aids | Model vintage is unconfirmed; coverage does not provide a complete airport or defence assessment. |

Source fingerprints and scenario settings are available through **How it works → Download scenario metadata**. Consult [NATS self-assessment guidance](https://www.nats.aero/our-solutions/wind-farms/self-assessment-maps/) when interpreting aviation overlaps.

## How the screening works

The application contains **3,680,510 cells** on a 250 m British National Grid. Each cell represents 0.0625 km², or 6.25 hectares.

- Country, landscape and NATS polygons are sampled at cell centres. Wind is sampled using bilinear interpolation; terrain uses the maximum underlying slope. Settlement setbacks use approximate raster distances.
- Active exclusions are combined so that overlapping constraints do not subtract the same land twice. Individual overlap areas should therefore not be added together.
- Cells missing information required by an active exclusion cannot pass. Flag-only overlaps remain included; an entirely unassessed constraint does not establish clearance.
- Retained cells are grouped by shared edges. Diagonal contact alone does not connect two areas. Groups below the minimum size are removed from the headline result.
- The passing area is divided by all sampled country-mask area in the selected study area.

In a GB-wide run, connected areas can cross national borders. Country breakdowns allocate the qualifying cells to each nation. Selecting one nation separately can change which groups meet the size threshold at its border.

## Example results and takeaways

The following results use the **29 October 2026 release** and its default settings. National figures are from the GB-wide run.

| Area | Sampled land passing the default screen |
| --- | ---: |
| Great Britain | **49.8%** |
| England | 49.9% |
| Wales | 41.9% |
| Scotland | 51.8% |
| England and Wales combined | 48.8% |

Two sensitivity checks illustrate how assumptions affect the result:

- **Exclude all four mapped NATS categories:** the GB result falls to **19.1%**. This shows the influence of aviation screening assumptions; it does not mean those consultation areas prohibit development.
- **Increase the minimum wind-farm size to 100 MW, keeping NATS flagged:** the GB result falls to **48.4%**. Larger contiguous areas remain in this model, but their shape and practical turbine capacity have not been assessed.

The default screen leaves a substantial area for further investigation. It does **not** establish that there are enough attractive, deliverable new-build projects, or resolve the balance between new development and repowering. Those questions require additional evidence at site level.

## Scope and limitations

This is an early spatial screening tool. The following are not comprehensively assessed:

- Heritage assets and their settings; habitats, birds, bats, ecology and peat.
- Isolated dwellings and detailed noise, shadow-flicker or residential amenity effects.
- Complete airport and MOD safeguarding, military low flying and private communications links.
- Water bodies, flooding, transport and construction access, land ownership and commercial viability.
- Existing wind-farm footprints, which have not been systematically removed.

Grid connection is deliberately outside scope. The model does not design turbine layouts or calculate wake losses, energy yield, consent likelihood or deliverable capacity. Narrow connections between cells can join areas that would not support a practical layout. Source dates, approximate transformations and 250 m sampling limit spatial precision; radar height bands above 200 m remain provisional.

An absence of mapped constraints is not clearance. Use the dashboard to identify questions and areas for closer study, followed by appropriate site-specific assessment and consultation.

## Technical notes and validation

The compact release stores the core grid using lossless byte-column shuffling followed by gzip compression. The browser restores the original records before running the calculations. This reduced the HTML from approximately **31.4 MB to 14.5 MB** without removing cells, reducing resolution or changing values.

Validation confirmed byte-for-byte reconstruction of the core grid, JavaScript syntax, agreement with independent Python screening results, the 10 MW and 100 MW size scenarios, NATS exclusion results, default resets and results exports. Browser appearance and a live GitHub Pages deployment were not verified in the build environment.

For the methodology matching this release, use the dashboard’s embedded **How it works** content and study note. These supersede the earlier document based on 200 m tip height and a 500 m settlement buffer.

## Attribution and reuse

Contains Ordnance Survey data © Crown copyright and database right 2026; ONS country boundaries; designation data from Natural England, Natural Resources Wales and Scottish Government / NatureScot; NATS safeguarding data; and the supplied wind raster.

The project owner has confirmed NATS publication permissions and that the wind-data licence is suitable for the intended publication. Original source terms and attributions continue to apply. No additional licence for the application code is specified in this README.
