---
title: 'Rayshader 1.0: Exit the Matrix'
link: https://www.tylermw.com/posts/rayverse/rayshader-1-0-exit-the-matrix.html
source: hnrss-org
published: 2026-11-27T14:30:20Z
updated: 2026-11-27T14:30:20Z
first_seen: 2026-11-28T14:03:39.806524745Z
authors:
- tylermw
content: extracted
html: 2026-11-27-rayshader-1-0-exit-the-matrix.html
remote_preview:
  url: https://www.tylermw.com/posts/images/2026/rayshader-100/construct-matrix-featured.png
extra:
  content:interactive: true
---

Eight years in the making—rayshader 1.0 is here!

![](https://www.tylermw.com/posts/images/2026/rayshader-100/construct-matrix-featured.png)

ImportantStay for the credits

Be sure to read to the end for what I can guarantee is the **most exciting and cinematric** R package bibliography you’ve ever seen. Everything you see in the below blog post was rendered in R with the rayverse.

Eight years ago, rayshader started as a small experiment in making beautiful maps in R. The first version was based on a simple question: what if we combined raytracing and cartography? I had always been interested in learning both about mapping and pathtracing, and what better way than to combine the two ideas into a single R package? The big “aha!” moment came when I applied that method with interactive 3D terrain for the first time and took it for a spin.

The surface was no longer just a shaded map: it felt tangible and real, a 3D rectangle twirling like a Little Debbie cosmic brownie on a microwave turntable. The combination of cartographic hillshading and 3D raytraced terrain looked spectacular in a way that, at the time, mostly lived in bespoke Blender scenes or other advanced 3D modeling tools. And unlike Blender, you didn’t need to learn a whole new ecosystem to visualize your data in 3D: it lived in the same place your data did (R!), which allowed for a gentler learning curve.

But as time went on and the package and userbase expanded, I had a nagging feeling: rayshader was still too hard.

Not always, of course: plenty of people were using it, quite effectively. Simple use cases were fine: pass in a matrix, add a few layers, call `plot_3d()`, and suddenly you had a cool-looking 3D landscape. If that’s all you needed: great! But if the story you were trying to tell was about data within the landscape–well, that’s where the friction began to burn.

## Beauty is table stakes

I built rayshader for people who wanted to make data beautiful without leaving R. Beauty has a real purpose in the world: it draws people’s eyes. It doesn’t matter how accurate or well-constructed your map is if the reader swipes on by: in the market for human attention, beauty and form are table stakes.

Journalists understood that quickly, and were some of rayshader’s earliest adopters.

The Wall Street Journal’s [*The Future of Arctic Shipping*](https://www.wsj.com/graphics/the-future-of-arctic-shipping/) used detailed shipping, bathymetric, and ice coverage data to explain the complexities of the Northern Sea Route. The piece was recognized by the [Information is Beautiful Awards](https://www.informationisbeautifulawards.com/showcase/3823-the-future-of-arctic-shipping), which described it as a deeply researched story built from shipping, bathymetric, and ice coverage data. It appeared only a few months after I released rayshader: talk about an introduction! Bloomberg’s 2024 Gaza rebuilding story, [*Gaza Reduced to 42 Million Tonnes of Rubble. What Will It Take to Rebuild?*](https://www.bloomberg.com/graphics/2024-gaza-who-will-pay-to-rebuild/), used R, rayshader, and rayrender as part of the visual system for communicating the scale of destruction and debris in Gaza. And in June 2022, a rayshader 3D flood visualization appeared on the front page of *The Sydney Morning Herald*, inset into coverage of the Lismore flooding.

![The front page of The Sydney Morning Herald with the headline Victims left in the dark and an inset 3D flood visualization.](https://www.tylermw.com/posts/images/2026/rayshader-100/smh-front-page-lismore-flood.jpeg)

A June 30, 2022 front page of The Sydney Morning Herald showing a rayshader flood visualization inset into Lismore flooding coverage.

That last one stuck with me (and not only because a rayshader map was featured “above the fold” in a major newspaper!). Rayshader had started as a way to make beautiful data visualizations in R, but here it was being used in deadline journalism to help explain a disaster to a general audience. The package had left the domain of “fun engaging social media posts” and entered the visual language of public-interest reporting.

But getting a reader to stop scrolling is only half the problem. Working with journalists taught me how much friction stood between having the data and making the final product. A newsroom deadline is a very good detector of bad API design. If adding buildings takes half a day of coordinate fiddling, that feature might as well not exist. If a 3D path requires a hand-rolled coordinate transform pipeline, nobody is doing that 30 minutes before a deadline. If a pathtraced render looks great but takes too long to converge, it is not going to make it to print. Especially if the editor needs a slight tweak: “Oh, sure–just give me 8 hours to re-render the animation.” Ha. Nope.

> Working with journalists taught me how much friction stood between having the data and making the final product.

The problem wasn’t so much with rayshader itself: it was how much spatial bookkeeping and manual fiddling you had to do to get data into rayshader before you could do all the fun stuff. And at the root of all that bookkeeping was the humble matrix, rayshader’s original choice for representing terrain.

## The problem with matrices

Rayshader used matrices to represent terrain because it simplified a lot of the work that went into turning a bunch of elevation values in a 3D object. But most real data does not arrive nicely packaged as a plain rectangular numeric array. It arrives as georeferenced rasters, polygons, lines, and points. It arrives cloaked in an unknown coordinate reference system. It arrives in meters, feet, yards, degrees, or [German legal metres](https://epsg.io/9031-units). Choosing the matrix as the base input made it easy for me, the developer, by putting the spatial problem (mostly) on the user.

Is there an… illegal german metre?

Convert the raster to a matrix. Save the extent. Transform the vector data. Save the CRS. Crop the linework. Pass the heightmap again. Pass the extent again. Wait, is the extent format lat/long/lat/long or lat/lat/long/long? I never remember–whatever, try both. The first choice is (somehow!) always wrong. Pass `zscale` again. Resize the overlay. Pass the CRS again. Check whether the map is oriented correctly. No? Surprise! Errant minus sign. Yuck.

This was not just annoying for beginners. It annoyed me, and I wrote the damn thing: now that’s a usability red flag! The new version of rayshader includes a series of changes that gets rid of all of this overhead and allows you to focus on presentation instead of the data processing.

## rayshader has gained object permanence

The big change in rayshader 1.0 is this: rayshader no longer has [short-term amnesia](https://www.tylermw.com/posts/images/2026/rayshader-100/zoo-img.png). Rayshader now keeps track of all the spatial details from your underlying map so you don’t have to worry about it any more.

This changes the user experience from:

> Transform and pass in your data into an aligned matrix coordinate system, and rayshader will render it in beautiful 3D.

To:

> Transform and **Pass in your data** into an aligned matrix coordinate system, **and rayshader will render it in beautiful 3D.**

The black diamond learning curve is now a gentle bunny slope. Terrain can stay a spatial raster. Road can stay a spatial line and will automatically be converted to the correct coordinate system when plotting. Buildings can be passed in as spatial polygons and will automatically be cropped to the underlying map’s extent.

A rayshader scene now extracts and caches the important metadata from the input data: heightmap, extent, CRS, `zscale`, vertical exaggeration, and rendered-object metadata. That cache lets downstream `render_*()` calls do the thing users always wanted them to do: simply drop data into the scene.

Most of the functions can now add to the scene simply by passing the data, plus an optional argument that specifies altitude or height, either directly or through a column in an `sf` object. The rest of the arguments then focus on how you want to style the data.

For example, let’s load in some road and elevation data and see what the new API looks like. The code below defines the helpers and loads the data into R. There’s no rayshader in it, so you have absolutely no obligation to read it :).

Data loading code (boring, you can skip)

```
microsoft_building_links_url =
  "https://minedbuildings.z5.web.core.windows.net/global-buildings/dataset-links.csv"
worldcover_base_url =
  "https://esa-worldcover.s3.eu-central-1.amazonaws.com/v200/2021/map"

# Helper function to get buildings from world-wide building dataset
get_microsoft_building_footprints = function(
  scene,
  n = Inf,
  cache_dir = file.path(tempdir(), "microsoft-building-footprints"),
  links_url = microsoft_building_links_url,
  zoom = 9
) {
  dir.create(cache_dir, recursive = TRUE, showWarnings = FALSE)

  links_file = file.path(cache_dir, "dataset-links.csv")
  if (!file.exists(links_file)) {
    download.file(links_url, links_file, mode = "wb", quiet = TRUE)
  }

  links = utils::read.csv(
    links_file,
    colClasses = c(
      Location = "character",
      QuadKey = "character",
      Url = "character",
      Size = "character",
      UploadDate = "character"
    )
  )
  tile_links = links[
    links$QuadKey %in% scene_quadkeys(scene, zoom),
    ,
    drop = FALSE
  ]
  if (nrow(tile_links) == 0) {
    stop(
      "No Microsoft building footprint tiles found for this scene.",
      call. = FALSE
    )
  }

  buildings = lapply(
    tile_links$Url,
    read_microsoft_building_tile,
    cache_dir = cache_dir
  )
  buildings = do.call(rbind, buildings) |>
    sf::st_make_valid() |>
    sf::st_transform(sf::st_crs(scene))
  buildings = suppressWarnings(sf::st_intersection(buildings, scene))
  buildings = suppressWarnings(
    sf::st_collection_extract(buildings, "POLYGON", warn = FALSE)
  )
  buildings = buildings[!sf::st_is_empty(buildings), ]
  if (nrow(buildings) == 0) {
    stop(
      "No Microsoft building footprints intersected this scene.",
      call. = FALSE
    )
  }

  buildings$footprint_area_m2 = as.numeric(sf::st_area(buildings))
  if (is.finite(n) && nrow(buildings) > n) {
    buildings = buildings[
      order(buildings$footprint_area_m2, decreasing = TRUE),
    ]
    buildings = buildings[seq_len(n), ]
  }

  height = if ("height" %in% names(buildings)) {
    buildings$height
  } else {
    rep(NA_real_, nrow(buildings))
  }
  height[!is.finite(height) | height <= 0] = NA_real_
  height[is.na(height)] = runif(sum(is.na(height)), 6, 18)
  buildings$height_m = height

  keep_cols = intersect(
    c("height_m", "confidence", "footprint_area_m2"),
    names(buildings)
  )
  buildings[, keep_cols]
}


lonlat_to_bing_tile = function(lon, lat, zoom) {
  lat = pmax(pmin(lat, 85.05112878), -85.05112878)
  tile_count = 2^zoom
  x = floor((lon + 180) / 360 * tile_count)
  y = floor(
    (1 - log(tan(lat * pi / 180) + 1 / cos(lat * pi / 180)) / pi) /
      2 *
      tile_count
  )

  data.frame(
    x = pmax(pmin(x, tile_count - 1), 0),
    y = pmax(pmin(y, tile_count - 1), 0)
  )
}

bing_tile_to_quadkey = function(x, y, zoom) {
  vapply(
    seq_along(x),
    function(i) {
      paste0(
        vapply(
          seq.int(zoom, 1),
          function(bit) {
            digit = 0L
            mask = bitwShiftL(1L, bit - 1L)
            if (bitwAnd(x[i], mask) != 0L) {
              digit = digit + 1L
            }
            if (bitwAnd(y[i], mask) != 0L) {
              digit = digit + 2L
            }
            as.character(digit)
          },
          character(1)
        ),
        collapse = ""
      )
    },
    character(1)
  )
}

scene_quadkeys = function(scene, zoom = 9) {
  bbox = sf::st_bbox(sf::st_transform(scene, 4326))
  tiles = lonlat_to_bing_tile(
    lon = c(bbox[["xmin"]], bbox[["xmax"]]),
    lat = c(bbox[["ymax"]], bbox[["ymin"]]),
    zoom = zoom
  )
  tile_grid = expand.grid(
    x = seq(min(tiles$x), max(tiles$x)),
    y = seq(min(tiles$y), max(tiles$y))
  )
  unique(bing_tile_to_quadkey(tile_grid$x, tile_grid$y, zoom))
}

read_microsoft_building_tile = function(url, cache_dir) {
  tile_file = file.path(cache_dir, basename(url))
  if (!file.exists(tile_file)) {
    download.file(url, tile_file, mode = "wb", quiet = TRUE)
  }

  geojsonl_file = tempfile(fileext = ".geojsonl")
  on.exit(unlink(geojsonl_file), add = TRUE)
  writeLines(
    readLines(gzfile(tile_file), warn = FALSE),
    geojsonl_file,
    useBytes = TRUE
  )
  sf::st_read(geojsonl_file, quiet = TRUE)
}

# Load the data using the helpers above.
library(terra)
library(sf)
library(dplyr)
library(elevatr)
library(tigris)

options(tigris_use_cache = TRUE)

bbox_ll = c(
  xmin = -119.03,
  ymin = 37.60,
  xmax = -118.94,
  ymax = 37.67
)

target_crs = "EPSG:32611"
scene_ll = sf::st_as_sfc(
  sf::st_bbox(bbox_ll, crs = sf::st_crs(4326))
) |>
  sf::st_as_sf()
scene = sf::st_transform(scene_ll, target_crs)

dem = elevatr::get_elev_raster(
  locations = scene_ll,
  z = 12,
  clip = "bbox"
) |>
  terra::rast()

names(dem) = "elevation_m"
scene_crop = sf::st_transform(scene_ll, terra::crs(dem))
dem_crop = terra::crop(dem, scene_crop)

roads = tigris::roads(
  state = "CA",
  county = "Mono",
  year = 2024,
  class = "sf"
)

water = tigris::area_water(
  state = "CA",
  county = "Mono",
  year = 2024,
  class = "sf"
)

buildings = get_microsoft_building_footprints(scene, n = 3910)
```

1. Transform to a UTM CRS (so everything is in meters)
2. Pass in our lat/long bounding box

Pretty straightforward… just kidding, loading geospatial data is always a pain. Nothing I can do about that! But the real question is: how complex is visualizating all this data once it’s loaded?

```
# Run once in the R console to install rayshader from my R-universe.
if (interactive()) {
  install.packages(
    "rayshader",
    repos = c("https://tylermorganwall.r-universe.dev", "https://cloud.r-project.org")
  )
}
library(rayrender)
library(rayshader)

# Plot in 3D
dem_crop |>
  height_shade() |>
  plot_3d(window_size = c(1500, 500))

# Data
render_roads(roads)
render_water(water)
render_buildings(buildings)

# Camera angle
render_camera(fov = 80, phi = 40, theta = 135, zoom = 0.25)

render_highquality(
  datetime = as.POSIXct("2025-12-21 09:00:00", tz = "America/Los_Angeles"),
  iso = 5
)
rgl::close3d()
```

![](https://www.tylermw.com/posts/images/2026/rayshader-100/fig-just-pass-your-data-1.png)

 Figure 4: The new workflow: download a DEM, pull in your vector data, then render!

Not complex at all! You call the function named after the type of data… and it renders the data in the scene. Any additional arguments are just for styling the data to your liking.

And the payoff is not just less typing: it lets us get to the fun part of 3D data visualization faster. The coolest part of rayshader has always been building strikingly beautiful 3D landscapes from your data. But the third dimension raises the cognitive cost of entry: before you can play director and cinematographer, you have to build the set. When every point, path, and polygon needs the user to track the CRS and extent info themselves and transform the data appropriately, many users never get to the stage where they can experiment with lighting and composition, styling the data (costume design!), or focus on editing the overall narrative.

But also, less typing!

> The third dimension raises the cognitive cost of entry: before you can play director and cinematographer, you have to build the set.

This release automates almost all of the behind-the-scenes work. The boring parts–CRS juggling, extent passing, heightmap repetition, matrix conversion, and other bookkeeping–stay in the package internals. The user is then free to jump to the enjoyable creative roles: the executive producer of the analysis, the production designer of the world, or the sound designer of a multi-sensory data experience.

## Spatial data: free yourself from the matrix!

![](https://www.tylermw.com/posts/images/2026/rayshader-100/rayshader-matrix-pod.png)

Rayshader now accepts many more spatial inputs for the heightmap. The `plot_3d()` function can now accept a heightmap as a matrix, a `terra::SpatRaster`, a `raster` object, or a raster filename. When the input is spatial, rayshader extracts and caches the extent and CRS. If `zscale` is not supplied, rayshader derives it from raster cell resolution. If the raster is geographic longitude/latitude, rayshader estimates the meter-per-cell scale at the raster center. The data was always there; it just had to be piped around to the right place! Now I do the plumbing for you.

Let’s look at an example. The same DEM and main-road data feed both versions below. The difference is who’s now doing the spatial bookkeeping (hint: it’s not you).

Data load

```
bbox_ll = c(
  xmin = -119.03,
  ymin = 37.60,
  xmax = -118.86,
  ymax = 37.74
)
target_crs = "EPSG:32611"
scene_ll = sf::st_as_sfc(
  sf::st_bbox(bbox_ll, crs = sf::st_crs(4326))
) |>
  sf::st_as_sf()
scene = sf::st_transform(scene_ll, target_crs)

dem = elevatr::get_elev_raster(
  locations = scene,
  z = 12,
  src = "aws",
  clip = "bbox",
  tmp_dir = tempdir()
) |>
  terra::rast() |>
  terra::project(target_crs, res = 30) |>
  terra::crop(terra::vect(scene))

roads = tigris::roads(
  state = "CA",
  county = "Mono",
  year = 2024,
  class = "sf"
) |>
  sf::st_make_valid() |>
  dplyr::filter(MTFCC == "S1200")
```

### Old workflow

```
1scene_extent = terra::ext(dem)
2scene_crs = terra::crs(dem)
3heightmap = raster_to_matrix(dem)
4zscale = 30

heightmap |>
5  height_shade() |>
  plot_3d(
6    heightmap = heightmap,
    zscale = zscale,
    extent = scene_extent
  )

render_path(
7  roads |>
    sf::st_transform(scene_crs),
8  extent = scene_extent,
  heightmap = heightmap,
  zscale = zscale,
  crs = scene_crs
)
9render_highquality()
```

1

Pull the raster extent out into a separate object.

2

Pull the raster CRS out into a separate object.

3

Convert the spatial raster into the matrix rayshader expects.

4

Manually specify the zscale

5

Color the elevation matrix before constructing the terrain.

6

Re-specify all the inputs to plot the scene in 3D.

7

Manually transform the vector layer at the point where it enters the scene.

8

Repeat the scene metadata again when adding a path.

9

Pathtrace the scene with rayrender.

### new workflow:

```
1dem |>
2  height_shade() |>
3  plot_3d()

4render_path(roads)
5render_highquality()
```

1

Keep the DEM as a spatial raster.

2

Create a hillshade.

3

Pass the hillshade into `plot_3d()`. Let the cached heightmap, CRS, extent, and derived `zscale` carry forward into the scene.

4

Pass the lines; rayshader transforms and filters it against the cached scene.

5

Pathtrace the scene with rayrender.

Internally, rayshader still converts the raster to something it can render. Externally, the scene remembers enough to let the `render_*()` functions just work.

There is now also a cleaner division between physical scale and visual exaggeration. `zscale` now describes the relationship between horizontal and vertical units, while `vertical_exaggeration` describes the stylistic exaggeration. For spatial rasters, rayshader can derive `zscale` from the raster itself, placing the data at its physical scale. You can then adjust `vertical_exaggeration` to emphasize the terrain without changing the underlying unit conversion.

And this spatial data directionality can now be reversed. `spatialize_image()` turns a rayshader image, matrix, RGB array, or RGBA array back into a georeferenced `terra::SpatRaster`. It can inherit the cached extent and CRS from the active scene, convert rayshader’s linear color values to sRGB, and optionally append the source heightmap as an additional raster layer. So rayshader’s output can now feed back into other GIS software, rather than just assume rayshader is the final destination itself.

```
library(rayshader)

# Color cells below sea level before exporting the RGB raster.
rgb_raster = montereybay_spatial |>
  sphere_shade(vertical_exaggeration = 15) |>
  add_water(montereybay_spatial < 2, color = "dodgerblue4") |>
  spatialize_image(
    include_alpha = TRUE,
    include_height = TRUE
  )

terra::writeRaster(
  rgb_raster,
  "../images/2026/rayshader-100/monterey-georeferenced.tif",
  overwrite = TRUE
)
terra::plotRGB(rgb_raster, r = 1, g = 2, b = 3)
```

![](https://www.tylermw.com/posts/images/2026/rayshader-100/fig-spatialize-image-1.png)

 Figure 5: A styled rayshader image exported as a georeferenced raster. Move over the map to inspect longitude, latitude, and elevation.

This release lets rayshader fit into a real spatial workflow: spatial input, then rayshader styling, followed by spatial output. So you can generate beautiful hillshades and terrains in R, and then bring them into other spatial software like QGIS or ArcGIS. Work in the GIS interface you already know, render the terrain in R, and drop the result right back into your map.

## A watershed moment for rayshader water

Rayshader’s water was always a bit of an afterthought: it was good for rendering an ocean with bathymetry data, but it didn’t really work if the water was localized to one portion of the map. A lake halfway up a mountain shouldn’t require flooding everything below it to see the water, but rayshader’s old water meshing method assumed one water level for the whole map. Which was perfect if all you needed was that one large body of water: set a level and fill the scene to that height. Separate lakes, reservoirs at different elevations, and imperfect shorelines called for something more flexible.

Now water can arrive as a raster or a polygon, high in the mountains or below sea level. `render_water()` uses the terrain and coordinates already stored in the scene to properly place and render the water in a sane way.

The old single-level behavior remains available as `water_render_method = "legacy"`, as it is much faster and is perfectly suited for coastlines and topobathymetry data.

Water can follow grid cells with `water_render_method = "raster"` or preserve polygon boundaries with the default, `water_render_method = "polygon"`. Both draw a watertight mesh at the footprint and elevation you supply, even when that means an exposed vertical water wall where the supplied shoreline doesn’t quite meet the terrain. Fast, flexible, and suitable for the rare case where you need to [visualize Moses crossing the Red Sea](https://www.tylermw.com/posts/images/2026/rayshader-100/moses-red-sea.png).

Another (much cooler, IMO) option is `water_render_method = "optimized"`. It fits a closed shoreline to the actual 3D terrain mesh, adjusting the water elevation to match the supplied area. Using the elevations beneath your water footprint to seed candidate levels, the optimizer searches for a contour that comes as close as possible to the supplied water area without spilling out of the terrain. The animation below shows how candidate levels change the flooded area.

If no closed water body is possible—for example, the supplied water would flood the entire map—rayshader falls back to raster water.

A different problem arises when a DEM has no bathymetry: the lake bed and its surface occupy the same plane. `shift_terrain()` is a new function that can lower the terrain beneath a lake polygon before you add water, making room for the lake instead of letting the surfaces overlap. It can also raise terrain SimCity 2000-style (no tornados or alien invasions… yet).

Additionally, in `render_highquality()`, water can now use a rough dielectric material to spread reflections into the broad highlights you expect from a rippled lake. A perfectly smooth dielectric surface behaves like glass: reflected light stays coherent, so lakes and rivers can turn into polished sheets. Real water has small wind-driven ripples, each reflecting light in a slightly different direction. The microfacet material models this give a more realistic appearance to bodies of water, especially when viewed at a low angle.

![Smooth water reflecting a compact solar disk.](https://www.tylermw.com/posts/images/2026/rayshader-100/water-sunset-glassy.png)

![Rough water spreading the same sunset into an elongated highlight.](https://www.tylermw.com/posts/images/2026/rayshader-100/water-sunset-microfacet.png)

 Figure 8: Glassy water on the left; microfacet water on the right.

## Rayshader: now streaming!

Streams need to follow the terrain as they wind downhill. `render_streams()` takes spatial line data, samples the elevation along each path, and builds ribbons of water that hug the surface. Widths can vary by feature, and connected streams can be merged to reduce visible seams in high-quality renders. Put those streams together with lakes and you have a hydrologic scene driven by data.

Load and prepare Mono County terrain, lakes, and streams

```
library(rayshader)
library(sf)
library(terra)
library(dplyr)
library(tigris)
library(elevatr)

options(tigris_use_cache = TRUE)

target_crs = "EPSG:32611"
dem_z = 12

scene_ll = sf::st_as_sfc(
  sf::st_bbox(
    c(xmin = -119.12, ymin = 37.52, xmax = -118.68, ymax = 37.86),
    crs = sf::st_crs(4326)
  )
) |>
  sf::st_as_sf()

scene = sf::st_transform(scene_ll, target_crs)

water = tigris::area_water(
  state = "CA",
  county = "Mono",
  year = 2024,
  class = "sf"
) |>
  sf::st_make_valid() |>
  sf::st_transform(target_crs) |>
  sf::st_crop(scene) |>
  dplyr::filter(MTFCC %in% c("H2030", "H2040")) |>
  dplyr::mutate(area_m2 = as.numeric(sf::st_area(geometry))) |>
  dplyr::filter(area_m2 >= 5000)

dem_raw = elevatr::get_elev_raster(
  locations = scene,
  z = dem_z,
  src = "aws",
  clip = "bbox"
)

dem = terra::rast(dem_raw) |>
  resize_spatial(scale = 0.5, method_down = "mean")
names(dem) = "elevation_m"

water_level = terra::extract(
  dem,
  terra::vect(water),
  fun = median,
  na.rm = TRUE,
  touches = TRUE
)
water$water_level_m = round(water_level$elevation_m, 1)
water = water[is.finite(water$water_level_m), ]

streams = tigris::linear_water(
  state = "CA",
  county = "Mono",
  year = 2024,
  class = "sf"
) |>
  sf::st_make_valid() |>
  sf::st_transform(target_crs) |>
  sf::st_crop(scene) |>
  sf::st_collection_extract("LINESTRING", warn = FALSE)
```

```
dem |>
  shift_terrain(
    geometry = water,
    amount = -10
  ) |>
  height_shade() |>
  plot_3d(
    phi = 10,
    zoom = 0.15,
    fov = 29.1,
    shadow = FALSE,
    normals = FALSE
  )

render_water(
  water_input = water,
  water_render_method = "polygon",
  water_edge_extension = 0.25,
  water_color = "dodgerblue",
  water_polygon_failure = "raster"
)

render_streams(
  streams = streams,
  water_color = "dodgerblue",
  width = 1
)

render_highquality(
  camera_location = c(-1752.16, 563.86, 415.55),
  camera_lookat = c(-155.98, -12.60, -246.84),
  fov = 10.5,
  width = 1500,
  height = 500,
  samples = 128,
  water_material = "microfacet",
  water_ior = 1.5,
  iso = 5,
  sky_sun_elevation = 20,
  sky_sun_azimuth = -113,
  sky_args = list(hosek = FALSE)
)
rgl::close3d()
```

![](https://www.tylermw.com/posts/images/2026/rayshader-100/fig-water-streams-1.png)

 Figure 9: A spatial hydrology workflow: water polygons become per-feature water levels, the terrain is indented underneath, and stream linework is rendered directly into the scene.

Note that there’s no complex GIS concepts on the rayshader side: all the complexity is related to defining the visual style and appearance. As it should be–rayshader is a visualization package! The remaining complexity is just standard data complexity present in any analysis: get the data loaded, cleaned, and in a form that represents the thing you care about.

## The road to believable roads

3D maps might seem like a fun little toy rather than a serious class of data visualization, but the third dimension is often surprisingly important for understanding geospatial data. This becomes less surprising when you realize that the most common mapping tool used by regular people–GPS apps–use 3D quite extensively. When you’re traveling through a city, buildings give you landmarks to match to your surroundings; on a walking or cycling route, the terrain shows where you’ll be climbing; and at a complex highway interchange, seeing which roads pass over one another helps you follow the right ramp and choose the right exit.

I already had surface-hugging polygons working for streams. Swap the water for asphalt and there’s a road! Easy, peasy, right?

![](https://www.tylermw.com/posts/images/2026/rayshader-100/roads-basic.png)

 Figure 10: A Houston interchange with unmarked asphalt following the terrain.

No! What’s the issue? We’re spoiled with our GPS apps: we’re used to roads looking like roads. A strip of grey asphalt needs lane markings for someone to know it’s a road. With the help of some fun repeating UV coordinate tricks and our new spatial-first workflow, we can paint properly-sized lines on the road at the cost of a few small textures. And we can use the lane counts supplied in OpenStreetMap data to add the correct number of lanes to each road. Rayshader uses those counts to mesh and texture the road, while the scene’s spatial information keeps the markings correctly sized.

![](https://www.tylermw.com/posts/images/2026/rayshader-100/roads-flat.png)

 Figure 11: Without height separation, every road hugs the unchanged terrain, with no grade or junction constraints.

In [Figure 11](https://www.tylermw.com/posts/rayverse/rayshader-1-0-exit-the-matrix.html#fig-roads-flat), each dash is 10 feet long and each gap is 30 feet. Those familiar markings do double duty: they improve the picture and act as little scale bars scattered through the scene. Beautiful! Pack it up, put a bow on it, and move on to the next feature, right?

The spacing is configurable for local standards.

No! Our roads are still smooshed together on one layer. At an interchange, that borders on uninterpretable.

OpenStreetMap doesn’t provide a full 3D road model. But it does carry one tiny (but extremely useful!) nugget of vertical information needed to draw roads correctly in 2D: **drawing order**. Draw layer 0, then layer 1, then layer 2, and the right road appears on top. That tells us which road crosses over which; it doesn’t tell us their heights or how the ramps return to ground level. Is this enough information to fully reconstruct our 3D roads?

![](https://www.tylermw.com/posts/images/2026/rayshader-100/road-layer-order.png)

 Figure 12: Drawing order determines which road appears to cross above the other, implying a 3D arrangement.

The tempting shortcut is to turn each layer into a fixed height: find the crossings, lift each road by a standard overpass height, and ease it back toward the ground with some sort of ad hoc relaxation rule. What could go wrong?

![A rendered road interchange with exaggerated humps and steep slopes where stacked roads meet.](https://www.tylermw.com/posts/images/2026/rayshader-100/crazy-roads.png)

 (a) Abrupt humps and steep ramps.

![A rendered interchange with uneven elevated road decks and misaligned connections.](https://www.tylermw.com/posts/images/2026/rayshader-100/crazy-roads2.png)

 (b) Road decks that fail to meet cleanly.

 Figure 13: Two ways a simple layer-based height offset can go wrong: implausible grades and awkward connections between road segments.

Everything. Everything can go wrong.

Simple hacks? Don’t work here. Turns out the world is filled with little annoying complications like a bridge crossing a sunken highway (the highway is already below ground, so the bridge needs no change in elevation), or short segments in interchanges that are technically not stacked but fall between stacked layers, or places where the road is technically stacked over a traversable water body (which also counts as a layer!), or a thousand other pesky little details.

What are we to do then? How can we implement realistic highways? It’s not like there’s a manual with explicit guidance and standards for how roads should be built in 3D—wait, what is that? **There is an explicit manual**?

![AASHTO's May 2016 A Policy on Design Standards—Interstate System](https://www.tylermw.com/posts/images/2026/rayshader-100/aashto.png)

 Figure 14: AASHTO’s *A Policy on Design Standards—Interstate System*

The American Association of State Highway and Transportation Officials (AASHTO) has a [design manual for Interstate highways](https://store.transportation.org/Item/PublicationDetail/2624). Two of the things it covers are exactly what our map is missing. How much room does a truck need under a bridge? How steep can the road get on its way up there? Now that rayshader knows the scale of the scene, I can give the solver a clearance in feet and a maximum grade in percent. Here are the default settings I gave the road solver (these are customizable):

1. Keeping crossing road surfaces at least 14 feet apart
2. Limiting the road grade to 7%
3. Ensuring roads default to hugging the ground when not part of an intersection and do not rise unnecessarily

We can turn these requirements into constraints on the road heights. While the rules aren’t guaranteed to generate roads that match the real-world roads exactly, it’s going to be pretty close: excursions from the bare minimum allowable ruleset will usually only increase cost, and we’re talking government contracts here. Around each interchange, we discretize the roads into short segments and solve for their elevations together: maintain the crossing order and clearance, limit the grades, and keep the roads close to the terrain wherever possible.

> It’s not like there’s a manual with explicit guidance and standards for how roads should be built in 3D—wait, what is that? **There is an explicit manual**?

For example, a road can weave over and under several others. Lifting one crossing changes the grades leading into the next, so solving each crossing independently won’t work–we have to solve all the linked constraints at once. We build a graph of the road connections with {igraph}, locate crossings with {sf}, and follow the constraints along connected road segments. Each connected optimization region gets a sparse quadratic problem, assembled with {Matrix} and solved with {osqp}. It turns out, R has everything you need to build and optimize road profiles.

![](https://www.tylermw.com/posts/images/2026/rayshader-100/road-active-nodes.png)

 Figure 15: Roads at a Bernese Oberland interchange with teal profile control points and orange rings at seven crossing constraints.

It’s pretty cool how closely a set of road-design rules and OSM’s layer ordering can recreate a complex interchange. The layer tags tell us which road goes over which, and the rules guide how the ramps connect them. Here’s Houston, now modeled in a sane way:

![](https://www.tylermw.com/posts/images/2026/rayshader-100/houston-roads.png)

 Figure 16: Elevated decks separate the crossings, with Houston buildings providing context. Now, the merges still need work, but that’ll have to wait for the next update :)

Beautiful. And since I went to Houston for this year’s Posit::conf, I was able to drive over specific intersections I rendered here and confirm that yes: the roads were accurately meshed. Minus the traffic.

Note“Why not cars?”

The novelty seeking half of my brain immediately followed this thought with: “You should put in cars now!” to which the logical, killjoy side of my brain responded “NO! STOP IT! YOU’VE IMPLEMENTED TOO MANY FEATURES ALREADY! You will never release this update if you keep indulging yourself! Stop it!”

Maybe in 1.1.0…

## A human battery? No–a human measuring stick.

Lane markings made the scale of a scene easier to read. Naturally, that led me to the ultimate human-scale feature: humans! But I couldn’t just throw the first set of free human models I found on the internet into the package. I did a fairly extensive search for a suitable pre-made model with both male and female variants, but to no avail. I had some requirements:

1. Low-poly enough to fit in the package when compressed (an R package is limited to 5 MB).
2. Proportioned like actual people, which rules out a surprising number of free models.
3. Not, uh, overly detailed. [You can imagine what I’m referring to.](https://www.tylermw.com/posts/images/2026/rayshader-100/sickos.jpg)

I couldn’t find a model that met these requirements with a suitable redistribution license. So I decided this was a great opportunity to learning something new: mesh sculpting in VR! I pulled out my Quest 3 and downloaded Shapelabs, which after a bit of practice and futzing around got me exactly what I wanted. Low-poly, but still human enough that you can read the pose at a glance.

![Male and female walking 3D models angled slightly inward on a blue-gray studio floor](https://www.tylermw.com/posts/images/2026/rayshader-100/people-walking-studio.png)

 Figure 17: The male and female raypeople models, in the “walking” pose.

From Shapelabs, the models went into Blender as GLB files for rigging and posing, then out as compact OBJ meshes for the package:

Render both pose galleries

```
options(rgl.useNULL = TRUE, cores = 4)
library(rayshader)
pose_heightmap = matrix(1:16 / 2, nrow = 4, byrow = TRUE) |>
  rayimage::render_reorient(flipy = TRUE, transpose = TRUE) |>
  rayimage::render_resized(mag = 50, method = "box")
pose_positions = expand.grid(
  x = seq(25, 175, by = 50),
  y = seq(25, 125, by = 50)
)
poses = c(
  "standing",
  "walking",
  "stop",
  "stop_one_hand",
  "clapping",
  "yelling",
  "stretch",
  "slipping",
  "stack",
  "stack",
  "ironman",
  "rocky"
)
pose_positions$offset = ifelse(poses == "ironman", 1, 0)
# Keep labels below the platform edges and closer to their own models.
pose_positions$label_altitude = pose_positions$offset + 1.75
# Keep the flying pose label below the top edge of its platform.
pose_positions$label_altitude[poses == "ironman"] = 2.1
pose_positions$label_altitude[poses == "slipping"] = 1.6
stack_index = 10L
label_indices = setdiff(seq_along(poses), stack_index)
platform_colors = c(male = "#102D54", female = "#800000")
dir.create("figures", showWarnings = FALSE)

for (model_sex in names(platform_colors)) {
  platform_color = platform_colors[[model_sex]]
  pose_heightmap |>
    constant_shade(platform_color) |>
    plot_3d(
      heightmap = pose_heightmap,
      zscale = 1 / 25,
      vertical_exaggeration = 1,
      shadow = FALSE,
      solid = TRUE,
      solid_depth = 0,
      solid_color = platform_color,
      window_size = c(1000, 1000)
    )
  render_camera(
    theta = -28,
    phi = 16,
    zoom = 0.80,
    fov = 0,
    shift_vertical = -10
  )
  render_people(
    pose = poses,
    sex = model_sex,
    color = "white",
    x = pose_positions$x,
    y = pose_positions$y,
    offset = pose_positions$offset
  )

  # Measure this variant so the upper model rests on the lower one's hands.
  model_file = system.file(
    "extdata",
    "raypeople",
    if (model_sex == "male") {
      "person_man_stack.txt"
    } else {
      "person_woman_stack.txt"
    },
    package = "rayshader"
  )
  stack_vertices = rayvertex::read_obj(model_file)$vertices[[1]]
  stack_height = diff(range(stack_vertices[, 2]))
  render_people(
    pose = "stack",
    sex = model_sex,
    color = "white",
    x = pose_positions$x[stack_index],
    y = pose_positions$y[stack_index],
    offset = stack_height
  )
  # Keep the long right-edge label fully on its platform in both galleries.
  for (i in label_indices) {
    render_label(
      text = poses[i],
      font = "Helvetica",
      family = "serif",
      font_type = "standard",
      text_size = 1.5,
      text_color = "white",
      adjust_vec = if (poses[i] == "stop_one_hand") {
        c(0.85, 0)
      } else if (poses[i] == "slipping") {
        c(0.65, -0.5)
      } else {
        NULL
      },
      line = FALSE,
      x = pose_positions$x[i],
      y = pose_positions$y[i],
      altitude = pose_positions$label_altitude[i],
      relative_z = TRUE
    )
  }
  render_label(
    text = "stack",
    font = "Helvetica",
    family = "serif",
    font_type = "standard",
    text_size = 1.5,
    text_color = "black",
    line = FALSE,
    x = pose_positions$x[stack_index],
    y = pose_positions$y[stack_index],
    altitude = 2 * stack_height - 0.25,
    relative_z = TRUE
  )
  set.seed(42)
  render_highquality(
    filename = file.path("../images/2026/rayshader-100", paste0("people-poses-", model_sex, ".png")),
    width = 1000,
    height = 1000,
    sky_sun_elevation = 10,
    sky_sun_azimuth = 160,
    iso = 5,
    samples = 128,
    integrator_type = "metal",
    transparent_background = TRUE,
    interactive = FALSE,
    preview = FALSE,
    progress = FALSE
  )
  rgl::close3d()
}
```

![Male models demonstrating every bundled pose on a solid dark blue stepped podium](https://www.tylermw.com/posts/images/2026/rayshader-100/people-poses-male.png)

Male models on a dark blue podium.

![Female models demonstrating every bundled pose on a solid dark red stepped podium](https://www.tylermw.com/posts/images/2026/rayshader-100/people-poses-female.png)

Female models on a dark red podium.

 Figure 18: My inspiration for these poses were 1) many hours playing Human Fall Flat, 2) the guy who’s about to crack his head open on every “wet floor” caution sign, and 3) philadelphia

Compressed, they’re roughly 18–21 KB each, checking the filesize box. Both model variants stand about 6 feet tall, with consistent proportions across their poses. Finally, a unit of measurement you can relate to!

## “I know ggplot2.”

The original `plot_gg()` was based on a simple idea: take a ggplot2 object, turn a fill or color aesthetic into height, and map the original plot onto that extruded surface. This made it possible to create 3D ggplots without learning a separate 3D plotting API. Just drop in your existing plot and poof! A 3D version would appear.

But this came with one limitation: the data had to be attached to the surface.

Floating data needs something to ground it. I originally kept the data attached to the surface deliberately: actual 3D data is harder to understand on a 2D screen, since depth is ambiguous. Extruded 2D surfaces provide a continuous underlying substrate, which makes interpretation easier–the surface itself provides depth cues, by nature of surface continuity providing cues on depth ordering. It’s not hard to plop down some floating 3D data in a box and call it a day: before rayshader, that was what pretty much every 3D plotting library I encountered did. In my opinion, that lazy approach helped give 3D dataviz its terrible reputation. Adding 3D data disconnected from that substrate requires additional care and thought to keep the data interpretable.

Which rayshader now provides!

In rayshader 1.0, `plot_gg()` is much more tightly integrated with the `render_*()` family of functions that add real 3D data to the scene. Rayshader now extracts the data extent from the existing ggplot2 scene. Basically, we now track how ggplot2 transforms data into the image, so we can use the same transformation for our 3D data. Downstream render calls can use that extent to place real 3D geometry in the correct part of the plot. The ggplot is no longer just a surface extrusion: it’s now the base coordinate system for a complete 3D scene.

And this can really help visually, as that surface extrusion wasn’t ideal for certain types of data. The original raster extrusion worked well for smooth surfaces, density plots, and continuous fields, but sharp polygon boundaries and abrupt vertical edges were harder.

Take [Figure 19](https://www.tylermw.com/posts/rayverse/rayshader-1-0-exit-the-matrix.html#fig-ggplot-states-edges): it shows a map of US states with height mapped to population. States share an boundary in the source polygons, but when rasterized to row of pixels, you can see the colors bleed down the step between states. This is an unavoidable problem when you try to force vector data into a raster format. So we avoid it by drawing the actual 3D data. On the right, each state’s own polygon makes a crisp vertical wall.

Compare raster extrusion with vector polygons

```
library(ggplot2)
plot_colors = scales::hue_pal()(6)[c(5, 4, 3, 2, 1, 6)]
us_states = spData::us_states
states_gg = ggplot(us_states) +
  geom_sf(aes(fill = total_pop_15), color = NA) +
  scale_fill_gradientn(colors = plot_colors, name = "Population (2015)")

plot_gg(
  states_gg,
  width = 6,
  height = 5,
  height_aes = "fill",
  raytrace = FALSE,
  solid_depth = -0.2,
  shadow = FALSE,
  theta = -15,
  phi = 35,
  zoom = 0.54,
  window_size = c(1200, 700)
)
render_camera(shift_vertical = -90)
render_highquality(
  width = 1200,
  height = 700,
  light_direction = 90,
  light_altitude = 35,
  samples = 48,
  transparent_background = TRUE
)

# Keep the ggplot as a base and add actual polygon geometry.
plot_gg(
  states_gg,
  width = 6,
  height = 5,
  height_aes = "fill",
  flat_substrate = TRUE,
  raytrace = FALSE,
  solid_depth = -0.2,
  shadow = FALSE,
  theta = -15,
  phi = 35,
  zoom = 0.54,
  window_size = c(1200, 700)
)
render_polygons(
  us_states,
  data_column_top = "total_pop_15",
  color = "height",
  clear_previous = TRUE
)
render_camera(shift_vertical = -90)
render_highquality(
  width = 1200,
  height = 700,
  light_direction = 90,
  light_altitude = 35,
  samples = 48,
  transparent_background = TRUE
)
```

![](https://www.tylermw.com/posts/images/2026/rayshader-100/gg-states-raster.png)

Raster extrusion: transition regions get blurred between colors.

![](https://www.tylermw.com/posts/images/2026/rayshader-100/gg-states-vector.png)

Vector extrusion: each state has a continuous polygon edge.

 Figure 19: State population mapped to the z-axis.

The same integration lets individual points float above a flat ggplot2 substrate (enabled with `plot_gg(..., flat_substrate = TRUE)`). The ggplot establishes the scales and coordinate system, and rayshader adds the observed values as actual 3D points, positioned correctly. `render_zaxis()` gives those floating marks a scale you can read. It picks up the height scale from the ggplot, and you can put the axis inside a panel or along the outer edge of the plot. If you have facets, you can pick the panel. It works on regular terrain maps, too.

Add points above a ggplot coordinate grid

```
p = ggplot(mtcars, aes(wt, mpg)) +
  geom_point(aes(color = hp), alpha = 0) +
  scale_color_gradientn(colors = rep("#62676d", 2), limits = c(0, 350)) +
  coord_cartesian(xlim = c(1, 6), ylim = c(10, 35)) +
  labs(x = "Weight (1,000 lb)", y = "Fuel economy (mpg)") +
  theme_minimal() +
  theme(legend.position = "none")

plot_gg(
  p,
  height_aes = "color",
  vertical_exaggeration = 280,
  width = 5,
  height = 5,
  phi = 40,
  theta = -27,
  fov = 33,
  zoom = 0.83,
  flat_substrate = TRUE,
  raytrace = FALSE,
  window_size = c(800, 800)
)

render_points(
  x = mtcars$wt,
  y = mtcars$mpg,
  altitude = mtcars$hp,
  color = "dodgerblue",
  size = 2,
  offset = 0
)

render_zaxis(
  data = "point",
  breaks = c(0, 100, 200, 300),
  title = "Horsepower (hp)"
)

render_highquality(
  point_radius = 5,
  sky_sun_elevation = 90,
  sky_sun_azimuth = 20,
  light = FALSE,
  iso = 2,
  ground_material = rayrender::glossy(color = "rgb(130, 151, 134)")
)
```

This removes a ton of visual compromises. Instead of forcing every feature to become part of the surface texture, we use the surface texture as the base and add the features as real 3D geometry. The plot layout and elements are still determined by ggplot2: we’re just adding data to the z-axis.

As a note, while it raises the bar on what you **can do**, it also lowers the bar for making bad 3D visualizations, but with great power comes great responsibility! Oh, wait, wrong movie.

Faceted plots can receive geometry one panel at a time. Spatial plots built with `coord_sf()` carry their geographic transform into the scene, too. Your base plot can use familiar geoms—`geom_hex()`, `geom_point()`, `geom_path()`, `geom_raster()`, or `geom_sf()`—while rayshader adds polygons, points, paths, and a labeled z-axis in those coordinates.

Extrude county values and add a z-axis

```
counties = sf::st_read(system.file("shape/nc.shp", package = "sf"))
p = ggplot(counties) +
  geom_sf(aes(fill = BIR74), color = "white") +
  scale_fill_gradientn(
    colors = plot_colors,
    name = "Births (1974)",
    limits = c(0, 25000)
  ) +
  theme_void() +
  theme(legend.position = "none")
plot_gg(
  p,
  height_aes = "fill",
  flat_substrate = TRUE,
  raytrace = FALSE,
  width = 6,
  height = 3,
  window_size = c(1500, 500)
)
render_camera(theta = 25, phi = 25, zoom = 0.35, fov = 0)
render_polygons(counties, data_column_top = "BIR74", color = "height")
render_zaxis(
  data = "polygon",
  breaks = c(0, 10000, 20000),
  labels = c("0", "10,000", "20,000"),
  title = "Births (1974)",
  title_location = "top",
  title_offset = 1.5,
  label_size = 1,
  title_size = 1.1
)
render_highquality(
  sky_sun_elevation = 20,
  sky_sun_azimuth = 315,
  iso = 10,
  text_render = "screen",
  text_occlusion = FALSE,
  screen_text_args = list(halo_color = "white", halo_expand = 5),
  ground_material = rayrender::diffuse(color = "grey20")
)
rgl::close3d()
```

![](https://www.tylermw.com/posts/images/2026/rayshader-100/gg-counties.png)

 Figure 21: Each North Carolina county rises in proportion to the number of births recorded there in 1974. The z-axis gives the scale in births.

3D ggplots were always a cool, sometimes useful novelty. Now they can be a foundation for complex 3D visualizations. How complex are we talking about? Watch me demo it below with a data visualization showing ADSB flight path data over Washington, DC. Come fly over the National Mall with me and I’ll walk you through how deep the 3D rabbit hole goes.

[How much can you fit in one ggplot?](https://www.youtube.com/watch?v=3K3f1mUUidA)

Figure 22: A narrated flyover of Washington, DC, with buildings, roads, and labeled landmarks around the National Mall. [Watch the higher-quality version (MP4, 250 MB)](https://www.tylermw.com/posts/videos/2026/rayshader-100/dcbetter-60fps-voiceover-captioned.mp4)

## Better buildings, paths, labels, and objects

The `render_*()` functions have been rewritten around the same principle: spatial data already encodes where it is, so there is no reason to throw that information out.

`render_points()` can accept spatial point objects. `render_path()` can accept spatial line objects. `render_label()` can accept points, multipoints, polygons, and multipolygons, converting polygon inputs to centroids when needed. `render_polygons()` and `render_beveled_polygons()` use cached heightmap and extent metadata. `render_buildings()` uses spatial footprints, roof materials, roof angles, height columns, and CRS-aware extent filtering. `render_obj()` can place OBJ models using spatial point data. You can see most of these in action in [Figure 22](https://www.tylermw.com/posts/rayverse/rayshader-1-0-exit-the-matrix.html#fig-buildings-and-paths).

## New hillshading options

There’s some new basic hillshading options as well. `haze_shade()` adds an RGBA haze overlay based on an exponential atmospheric model. It is useful for low-elevation haze, valley haze, and scenes where you want the atmosphere to participate in the map rather than sit behind it. It is a good way to subtly separate mountain tops from valleys.

Add haze to the inland valleys

```
library(rayshader)

# Crop the bundled DEM to the inland hills and valleys east of Monterey Bay.
hills = terra::crop(
  montereybay_spatial,
  terra::ext(-121.73, -121.38, 36.20, 36.48)
)

dark_terrain = create_texture(
  light_color = "#879581",
  shadow_color = "#15252b",
  left_color = "#54634b",
  right_color = "#283f42",
  center_color = "#354c41"
)

hills |>
  sphere_shade(texture = dark_terrain, vertical_exaggeration = 5) |>
  add_overlay(haze_shade(
    color = "#dfe8f2",
    optical_depth = 1.5,
    scale_height = 200
  )) |>
  plot_3d(
    theta = 220,
    phi = 30,
    zoom = 0.4,
    fov = 50,
    background = "#20282f",
    solid_color = "#161e23"
  )
render_snapshot()
```

`radiance_shade()` renders a top-down radiance pass from a cached hillshade context, the active 3D scene, or a supplied heightmap. It gives 2D maps and 3D ggplot2 workflows access to more visually-striking physically based lighting without forcing the entire project into a final path-traced render. This is a nice way to get higher-quality shadows on a model you can still rotate and revise quickly. The result is still an RGBA overlay, so it can be composited back into the usual rayshader map pipeline or exported to other programs.

```
library(rayshader)

montereybay_spatial |>
  height_shade() |>
  add_overlay(
    radiance_shade(
      vertical_exaggeration = 10,
      integrator_type = "metal",
      samples = 16,
      sky_sun_elevation = 1,
      sky_sun_azimuth = 315
    ),
    alpha_layer = 0.9
  ) |>
  plot_3d(
    vertical_exaggeration = 4,
    theta = -70,
    phi = 35,
    zoom = 0.30,
    fov = 90,
    window_size = c(1200, 500)
  )
render_snapshot()
```

![](https://www.tylermw.com/posts/images/2026/rayshader-100/fig-radiance-1.png)

 Figure 24: A radiance pass composited back onto a 2D rayshader map, adding path-traced lighting while staying in the (fast) rgl workflow.

## Earth’s curvature

For a small area, we can usually ignore Earth’s curvature. As a map grows to cover hundreds or thousands of kilometers, the bend of the Earth can become an important part of the story. For satellite orbits, long-distance flight paths, and regional coverage maps, curvature can help signal to the reader that we’re dealing with global-scale data without an explicit scale bar.

Setting `earth_curvature = TRUE` in `plot_3d()` adds that curvature to the scene. Mountains and valleys can look flatter beside the much larger bend of the Earth when no vertical exaggeration is applied, but the adjustment gives those other GIS layers a more useful spatial context.

For example, [Figure 25](https://www.tylermw.com/posts/rayverse/rayshader-1-0-exit-the-matrix.html#fig-alaska-earth-curvature) shows three Starlink satellites cross Alaska along paths calculated from real orbital data. Their simulated beams track populated places below, switching targets as they move within reach. Both views show the same moment, but on the right, the surface and orbital altitudes are adjusted to match the global curvature. This helps give the viewer important context clues for the spatial scales we’re dealing with here that the flat projection does not provide.

## Even higher quality `render_highquality()`

Want to realistically light a scene at a very specific time and date? Just give rayshader a date and time–it already knows the place. For a scene with cached spatial coordinates, `render_highquality(datetime = ...)` can infer the location and generate the corresponding sky and sunlight. You don’t need to look up the latitude, longitude, or sun angle again. Depending on the sky model, that can include stars, planets, and the moon’s phase and brightness.

As an example, around the equinoxes, the low sun lines up with the National Mall’s east–west axis—a [favorite alignment for photographers](https://cherryblossomwatch.com/exploredc/photo-guide-lincoln-memorial-statue-golden-sunlight/). On March 25, 2026, at 6:56:11 p.m. EDT, the modeled shadow of the Washington Monument runs parallel to the Mall’s main paths. A few days past the equinox, the sun is still high enough at that alignment to keep the shadow within this view.

Set up an overhead view of the National Mall

```
library(rayshader)
mall = readRDS("../include/rayshader-100/data/mall-scene.rds")
mall_dem = terra::rast("../images/2026/rayshader-100/mall-dem.tif")

mall_dem |>
  constant_shade("#819369") |>
  plot_3d(
    vertical_exaggeration = 1,
    solid = FALSE,
    shadow = FALSE,
    theta = 0,
    phi = 80,
    fov = 0,
    window_size = c(1600, 900)
  )
render_water(mall$water, water_color = "#537b86")
render_roads(mall$roads, lane_texture = TRUE)
render_trails(
  mall$walkways,
  color = "#d9cbb0",
  width = 3,
  width_units = "meters",
  offset = 0.2
)
render_multipolygonz(mall$buildings, color = "#dcd8ce")
render_multipolygonz(mall$monument, color = "#f3eadd")
render_tree(
  mall$trees,
  tree_height_column = "MAX_CROWN_HEIGHT",
  crown_color = "#516c3d",
  preview_simple = TRUE
)
render_label(
  x = mall$center[1],
  y = mall$center[2] - 210,
  altitude = 20,
  text = "Washington Monument",
  line = FALSE,
  adjust_vec = c(0.5, 0.5)
)
```

```
render_highquality(
  datetime = as.POSIXct("2026-03-25 18:56:11", tz = "America/New_York"),
  integrator_type = "metal",
  ortho_dimensions = c(560, 315),
  light = FALSE,
  iso = 16,
  text_size = 24,
  text_occlusion = FALSE,
  screen_text_args = list(halo_color = "white", halo_expand = 2),
  samples = 256
)
```

![](https://www.tylermw.com/posts/images/2026/rayshader-100/mall-equinox.png)

 Figure 26: The Washington Monument at 6:56:11 p.m. EDT on March 25, 2026. Its shadow aligns with the National Mall’s main paths, and it only required passing the datetime to `render_highquality()`.

And separately, text and annotation lines were physical objects in the rendered scene. Their letters and lines cast shadows across the terrain and buildings, giving annotations the same simulated weight as the things they were annotating. A label should help you read the map without becoming another object in it.

The labels in [Figure 27](https://www.tylermw.com/posts/rayverse/rayshader-1-0-exit-the-matrix.html#fig-screen-valley-label) are drawn in screen space: their positions stay attached to the map, while the text keeps its size and faces the camera throughout the orbit, without casting shadows over the volcanoes. The horsepower axis in [Figure 20](https://www.tylermw.com/posts/rayverse/rayshader-1-0-exit-the-matrix.html#fig-mtcars-horsepower) uses the same approach. The lines and text are composited in real time when you’re looking at the high quality preview render in rayrender, so you can set up the shot exactly how you want to. Oh, and you can set `deferred_render = TRUE` now as well, which removes the sample timer so you can have all the time in the world to set up your render. This is especially important now, since rayrender is now fully GPU-accellerated on ARM macs, leading to 30-50x speedups in rendering performance, so your render can often end before you got a chance to click on the screen. This blog post, however, is about rayshader, so I won’t steal its thunder talking about the awesome improvements in rayrender :)

## 1.0 and beyond!

![A complete terrain scene with water and roads.](https://www.tylermw.com/posts/images/2026/rayshader-100/waterroads.png)

It’s been a long, properly textured road–why 1.0 after eight years? I now feel that rayshader has firmly found its niche and purpose, and this overhaul feels like a graduation moment for the package. I’m no longer figuring out what a 3D GIS workflow in R should feel like–it’s now about making that workflow work for you. And thinking about all the people who used rayshader to produce everything from beautiful generative art and frameable cartographic works to scientific articles and award winning journalism made me decide to give rayshader the graduation it deserves, and make sure it knows: You’re number 1.0!

## The grand finale and most exciting part of any publication: the bibliography.

Rayshader wouldn’t be possible without the work of all the packages and people that supported it and provided great user feedback during its development. In a normal blog post, this is where normally you’d have a list of citations and people to thank. But how often does R’s most cinematic package get a 1.0 release? I decided the people, packages, and data behind rayshader deserved the full cinematic treatment that rayshader has provided the R ecosystem. And that the cinematic treatment should be created entirely in R, using rayrender, of course. **Please stick around and hit play for the credits below—and be sure to turn the sound on.**
