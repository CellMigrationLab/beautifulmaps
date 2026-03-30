# beautifulmaps

**A reproducible OSMnx workflow for generating a beautiful print map**

This repository contains the original Jupyter notebook to generate a high-resolution, print-ready city map with selected landmarks highlighted.

## Repository contents

- `notebooks/BeautifulMaps.ipynb` — original notebook

---

# Materials and Methods

## Study design

The aim of this workflow is to generate a stylized, static cartographic figure suitable for high-quality printing. The figure represents the urban street network and hydrological features surrounding a user-defined central address in Turku, Finland, and highlights selected buildings or landmarks of significance.

## Materials

### Software

The figure was generated in Python using:
- `osmnx` for geocoding, downloading geospatial features, and retrieving the street network
- `matplotlib` for figure composition and export

### Data sources

Street-network geometry and hydrographic features were retrieved from OpenStreetMap using OSMnx. OSMnx provides functions to download a graph within a specified distance of an address (`graph_from_address`), to retrieve tagged map features around an address (`features_from_address`), and to geocode place names either as coordinates or as polygonal geometries (`geocode`, `geocode_to_gdf`). OpenStreetMap data are distributed under the Open Database License (ODbL), and reuse requires attribution to OpenStreetMap and its contributors.


## Methods

### Retrieval of the street network

The urban transportation network was downloaded with `ox.graph_from_address(center_address, dist=4500, network_type="all")`. This returned an OpenStreetMap-derived graph containing roads and related pathways within the requested distance of the central address.

### Retrieval of water features

Hydrological features were retrieved with `ox.features_from_address(...)` using the tag dictionary:

```python
water_tags = {
    "natural": ["water", "bay", "strait"],
    "waterway": ["river", "stream", "canal"],
}
```

This step extracted mapped water bodies and waterways within the same spatial radius as the street network.

### Geocoding and highlighting of landmarks

Each highlighted location was first queried with `ox.geocode_to_gdf`. When a polygon or multipolygon building footprint was available, the corresponding geometry was plotted directly. If only a point location could be resolved, the location was plotted as a point marker after fallback geocoding with `ox.geocode`. This strategy enabled visual emphasis of meaningful sites even when full building outlines were unavailable.

### Cartographic rendering

Street segments were styled according to their OpenStreetMap `highway` class to create a visual hierarchy. Motorways and trunk roads were rendered with the largest line widths and darkest tones, followed by primary and secondary roads, then tertiary or residential streets, and finally minor roads. Water features were rendered in blue, while highlighted buildings or locations were shown in red tones to maximize contrast against a white background.

### Cropping and composition

The figure canvas was manually cropped around the geocoded center coordinate to achieve a portrait-oriented composition suitable for printing. Axis labels and tick marks were removed, and the map border was rendered using visible axis spines. Typography, a scale bar, and a bottom-left title block were added directly in Matplotlib.

### Figure export

The final figure was exported as a PNG file at 300 dpi with transparency enabled. The current script writes the rendered map by default to:


## Data availability

No new dataset was generated for this project. All underlying geographic features were obtained from OpenStreetMap through the OSMnx interface at runtime. Because OpenStreetMap is a living database, repeated execution at later dates may produce minor differences if the underlying map data have changed.

## Code availability

All custom code required to reproduce the map is available in this repository under the MIT License.

## Attribution and licensing

- **Code license:** MIT
- **Map data:** © OpenStreetMap contributors, available under the ODbL
- **Attribution requirement:** any public reuse of derived maps should retain OpenStreetMap attribution

---
