**To be translated into Spanish following tmcw [instructions](CONTRIBUTING.md)**


% map school
% Tom MacWright

/** Blocked by juanluisrp */

# map school

What is a map? Until the 1980s, maps were painstaking documents created by hand. These days maps are almost always made with the help of a computer. Maps today are commonplace, interspersed in driving directions, visualizations, and political boundary disputes. Let's look deeper and think about the fundamental elements of maps from the eye of the creator.

Computer maps are fundamentally composed of data. Data is in the abstract, composed of billions of points or just a few polygons, or a photo-like recording of colors and temperature. It is important that data is not specific to a certain usage.

From data, we make numbers, pictures, and decisions. Most commonly we make pictures, in a process called 'symbolization' - deciding what visual elements will represent what different parts of a dataset. We analyze data, which means we transform, aggregate, and summarize it to give different answers and represent different aspects of knowledge. The two tasks of symbolization and analysis are often combined, with symbolization forming the limits of what we can represent and analysis defining the aspects of data we focus on.

# Datos

Fundamentalmente, los datos geográficos pueden ser bien **mapas de bits** (en adelante **ráster**) compuestos por píxeles o bien **vectoriales** compuestos por figuras geométricas. Los dos tipos a menudo se combinan como cuando se superponen datos vectoriales de carreteras sobre datos satelitales ráster.

## Raster

![](img/raster.png)

Los datos **Ráster** son como una fotografía que podrías haber tomado con una cámara digital: al menor nivel de abstracción es una lista de píxeles con valores. Cuando haces zoom y miras más de cerca los datos ráster, en algún momento llegarás a ver estos píxeles individuales. Entonces decimos que la imagen se ve pixelada.

Los datos ráster son usados en fotografías de la tierra, como las tomadas por los satélites, pero esto es solo el principio. Los píxeles no necesitan tener colores; en su lugar, cada píxel podría tener un número que representara la altitud y los datos ráster en su conjunto almacenaría los datos sobre el relieve. O, por ejemplo, podrían almacenar la temperatura o la radiación reflejada y se útiles para trabajos medioambientales.

##### Bandas Ráster

Los píxeles de los datos ráster no necesariamente está rellenos con colores: a su contenido le damos el nombre de "bandas". Una imagen normal tiene tres bandas familiares: Roja, Verde y Azul. Combinadas, forman una imagen como las que estamos acostumbrados a ver. Algunos datos ráster pueden tener menos bandas, como por ejemplo una sola para datos de elevación; otras pueden tener muchas más, -o solo colores visibles, sino longitudes de onda que los humanos no podemos ver, como el infrarrojo y el ultravioleta. Cuando los datos ráster son analizados y mostrados puedes combinar y elegir qué bandas usar de forma que se ajuste a lo que estás buscando. 

##### Formatos ráster

Los formatos ráster tratan de compactar los datos y hacerlos rápidamente accesibles para que puedan ser analizados y mostrados. Algunos son versiones con datos geoespaciales de formatos de imagenes comunes, como [GeoTIFF](http://trac.osgeo.org/geotiff/) y JPEG2000. 

Internamente, los formatos de datos ráster cumplen dos tareas: empaquetar los datos en píxeles y después, almacenar la relación entre estos píxeles y lugares reales del mundo a los que representan (la extensión de los datos).

/* End of blocked by juanluisrp*/

## Vector

![](img/vector_types.png)

**Vector** data stores basic geometries rather than pixel data. No matter how much you 'zoom in' on vector data, you won't see pixels: the data stored is composed of geometric points and lines, and only converted into an image when necessary.

Vector data is used to store roads, buildings, points of interest, and other things that have some place in the world.

##### Vector Formats

The most established vector format is the [Shapefile](http://en.wikipedia.org/wiki/Shapefile) - a simple, file-based format that awkwardly spreads the necessary data between four separate files - `.shp` (where actual geometry data resides), `.prj` (a string describing the projection used), `.shx` (an index enabling faster searches), and `.dbf` (a database file containing all the data associated with a geometry of the .shp file). Most of these files are binary data, so opening them in a text editor won't show anything accessible, apart from the .prj file, which defines the projection in plain text. The .dbf database file can be read from LibreOffice Calc because its format is derived from an old database specification. However, the old database specification limits the attribute data you can store in a shapefile. For example: the size of the .dbf can't exceed 2 GB, field names can't contain spaces and can't exceed 10 characters, NULL values are not supported, nor are many special characters, [etc.](http://en.wikipedia.org/wiki/Shapefile#Limitations)

[GeoJSON](http://geojson.org/), [TopoJSON](https://github.com/mbostock/topojson), and [KML](http://developers.google.com/kml) are newer formats based on [JSON](http://www.json.org/) and [XML](http://en.wikipedia.org/wiki/XML) text encoding, respectively. Being text-based, they tend to be simpler to implement in software than Shapefiles, and combined with additional flexibility and features, they have become the standard in new web software. The drawback to GeoJSON is that there are fewer tools built for comparing properties across records easily, so data cleaning and analysis is challenging.

### Topology

In addition to storing places and shapes, some vector data keeps track of topology, the relationships between different shapes. For instance, political borders often touch - you can stand with one foot in Arizona and another in New Mexico. A lot of geospatial data, though, will have one shape that represents Arizona and another that represents New Mexico, with two borders that precisely overlap, but have no other association.

This gets tricky when you want to do something like ask 'which states touch?' or simplify the shapes of objects while preserving borders lined up. We use the concept of topology: some vector data, instead of storing a shape of Arizona and another of New Mexico, saves a bunch of lines and keeps track of which ones form the boundary of which object. So, the border between Arizona and New Mexico is a single line that's used to draw the border of both states, and if you modify the line, it changes the shape of both states.

## Geocoding

A great deal of geographic data is none of the above - instead of being composed of the numbers that computers understand, it is stored as text data - references to placenames, streets, addresses, and so on.

Unfortunately, this isn't directly mappable. Due to variance in naming and the many definitions of place, there's an indirect and often inaccurate process involved in transforming the words 'United States' into the point `-120, 40`. This process is what we call **geocoding**.

The opposite process is **reverse geocoding**, where the readily available data is precise geographic positions, and the objective is a human description, like `United States` or `1714 14th Street`. Likewise, this isn't always accurate - one place on earth can be inside of overlapping and conflicting boundaries or between address points.

Geocoding and reverse geocoding can be very tricky: coordinate position errors, poorly formatted address data, and an ever-changing grid of streets and buildings contribute to the difficulty in turning addresses into coordinates, or vice versa.

## Recogida de datos

Los datos para los mapas han sido recogidos de multitud formas a lo largo de los años – desde los registros de marinos hasta los tweets geocodificados. Actualmente hay unas cuantas fuentes que merece la pena mencionar:

![](img/gps.jpg)

**GPS**, la constelación de satélites que ubica con un punto azul tu teléfono móvil en un mapa es la base de la recogida de datos vectoriales precisos. Los topógrafos emplean unidades de GPS de alta precisión y combinan sus resultados obteniendo algo digno de confianza.

Los **satélites y aviones** de observación recogen la mayoría de los datos ráster actuales, tomando fotos continuamente desde diferentes alturas y combinándolas para obtener algo similar a una foto del mundo. Los mismos sensores también capturan lo que se denomina 'espectro no visible', como la luz infrarroja que es muy útil en cartografía de agricultura y deforestación. Algunas plataformas incluyen [LiDAR](http://en.wikipedia.org/wiki/Lidar), un tipo de sensor láser que mide la altitud y produce datos ráster de altitud.

**Empresas, gobiernos y comunidades** mantienen diferentesmapas del mundo con diferentes detalles. Por ejemplo, Google y [OpenStreetMap](http://www.openstreetmap.org/) se centran en cartografiar las carreteras y sus detalles, y fuentes como [CIA World Factbook](https://www.cia.gov/library/publications/the-world-factbook/) y [Natural Earth](http://www.naturalearthdata.com/) que realizan un seguimiento de las fronteras políticas.

### Almacenamiento

Hay muchas formas de almacenar la información geográfica. Los datos pueden guardarse como copias impresas pero recientemente es más común hacerlo digitalmente, mucho más sencillo de acceder y compartir.

Hay muchos tipos de archivos y estándares para almacenar datos geográficos, y en consecuencia también hay muchas herramientas para pasar de unos a otros. Los datos se almacenan en bases de datos o en archivos, aunque las diferencias entre las dos opciones no son necesariamente importantes.

# Información

Los mapas representan información: En un extremo se encuentran los datums, números y el almacenamiento - la maquinaria que empleamos para grabar y diferenciar los diferentes lugares de la tierra. Entre los datos y la visualización tenemos una transformación que denominamos 'proyección - con la que pasamos de zonas de la tierra a zonas generalmente en superficies planas, como mapas impresos o pantallas de ordenador. Y, finalmente, decidimos los detalles de color, tono y la simbología que tratan de trasladar los detalles de los datos a imágenes que la gente pueda entender e interpretar.

## Latitude & Longitude

The most common way to store places on earth is with latitude and longitude values. Historically latitude and longitude are sometimes represented in sexagesimal notation, like `38° 12'`, but the new standard is to represent them as simple numbers, like `38.2`, that are easy for computers to understand.

![](img/latlon.png)

**Latitude** values range from -90 at the South Pole to 90 at the North Pole. All along the equator the latitude value is 0.

**Longitude** start at 0 at the Prime Meridian which cuts through Africa and Greenwich, and extends to -180 and 180 at the ante meridian.

The combination of latitude and longitude is usually called a coordinate, and can be represented as 'latitude, longitude', or 'longitude, latitude': historically, the former was standard, but 'longitude, latitude' has recently gained popularity because it mirrors the 'X, Y' ordering of coordinates in math's euclidean space.

Coordinate ordering can cause some confusion, as browser based mapping software often expects 'latitude, longitude', whereas many wire formats specify 'longitude, latitude'.

Sometimes more than just the latitude, longitude position is recorded as data: altitude can also be included, as well as time of capture and other factors. In the case of including altitude, it's usually stored as a third coordinate, like 'longitude, latitude, altitude.'

## The Shape of the Earth

![](img/earth-shapes.jpg)

Storing and presenting the world brings us to the question of what shape it is - can latitude and longitude values be mapped to a perfect sphere and back, and retain their accuracy spatially?

Since the earth is a spinning object and its components can change shape, it bulges at the middle - so instead of being a sphere, it's actually more similar in shape to a [oblate spheroid](http://en.wikipedia.org/wiki/Oblate_spheroid). If you look even closer, that isn't entirely true either - the earth is covered in [elevation differences](http://en.wikipedia.org/wiki/Topography) like mountains and valleys, and even manmade changes like cities.

For day-to-day work, we use different estimates of this shape: standards like [WGS84](http://en.wikipedia.org/wiki/WGS84) define precise values for the length of both of the earth's axes, so we can estimate for a [reference ellipsoid](http://en.wikipedia.org/wiki/Reference_ellipsoid) rather than a sphere. Local measurements and science that relies on precise surface values can also use [geoid models](https://en.wikipedia.org/wiki/Geoid), which are three-dimensional calculations of theoretical ocean heights.

This branch of earth sciences, called [geodesy](https://en.wikipedia.org/wiki/Geodesy), is a continuing project since our ability to model and measure the earth moves quickly and the earth itself changes in shape.

## Projection

Projections are what we call the mathematical equations that do the trick of turning the world into some flat shape that fits on a printout or a computer screen. It's a messy task to do, this transformation - there's no way to smoosh the world onto a screen without distorting it in some way. You either lose direction, or relative size, or come out with something very weird looking.

## Symbolization

Symbolization is a fancy word for the particular ways that data is transformed into graphics in the world of maps.

Fundamentally, data doesn't look like anything: a list of pixel values or road lines is just as well represented in a spreadsheet or a chart as it is on a map. Thus to 'convert' is not the right word for what we do with data: at the most basic level, everything is a decision.

Symbolic techniques include anything representable in graphics or even 3D, so let's only look at a few:

## Sequential & Categorical

![](img/scales.jpg)

Symbolization tends to highlight two different characteristics of data: sequential and categorical. Sequential, or continuous data could also be called linear - it tends to be number values within a set range, like graduation rates between 0 and 100. Categorical, or discrete data is, instead, one of a set number of values - like 'true', 'false', 'democrat' or 'republican'.

This division between data is one of the main concerns for symbolization - a sequential datasource would fit with a scaled point map whereas categorical data would work with multiple symbols for markers, and so on.

### Choropleth

Choropleth maps preserve the existing boundaries and shapes of places and represent data by changing their colors, patterns or textures. A familiar example of this kind of map is in election results or demographic makeup, in which data is percentage values for some fixed piece of land - a value per voting precinct or census area.

Choropleth maps are a natural fit for data like rates, densities, or percentages. They aren't recommended for absolute values: since they keep the area of shapes the same, they tend to over-emphasize large features. Also, since choropleth maps rely on color differences to represent information, it's important that the colors are well-chosen to be colorblind-safe, understandable, and consistent.

##### Choropleths in Depth

The selection of colors in a choropleth map is generally called the 'palette' or the 'color ramp', especially when the colors are sequential. [ColorBrewer](http://colorbrewer2.org/) is a well-researched set of colors that are understandable and clear.

### Point

Point maps are a better alternative for absolute values - the only geometry that they preserve is a single point for each feature.

The specific point or marker used in this style varies tremendously - Coloring points based on their sequential or categorical value can be useful, but points can also be scaled to different sizes to show their relative value. These scaled symbols can be any shape or image, such as circles, squares, or pictures of what they represent. In cases where there are multiple values that total up, scaled pie charts can be a terrific way to visualize what would otherwise be a complex dataset.

Care must be taken to not show too many points at once, as this will make a map difficult to read. In cases where there are too many points, a choropleth with aggregated values from the points may be a good alternative. Another alternative is to use clustering, where crowded points are grouped together until the map is zoomed in closer.

## Publishing

## Analysis

Raster & vector analysis as aggregation & transformation

### Vector to Raster

It's possible to move between the two types of map data, as disparate as they might seem, and it's actually quite common: it's just not direct.

Typically vector data always becomes raster - what we call 'rasterized' or 'rendered' when it is displayed: computer screens and printers operate on the level of pixels, not lines or shapes. This conversion is imperfect: remember that vector data is not pixel-based, so you can never zoom in and see fuzzy features. Thus, generally, when vector data is converted into a raster format, you can't transform that raster representation back into exactly the source.

Sometimes we convert vector data to raster ahead of time - in order to run some kinds of analysis, it's easier to do the math on a pixel basis.

### Raster to Vector

Likewise, raster data can be vectorized in a number of ways. On raster satellite images of the earth, people draw, or 'trace', lines for streets, points for houses, or polygons for buildings. This lets us have a version of the data upon which we can do more things - you can figure out driving routes from vector data of streets, but can't do that with a satellite image of streets.

### Simulation

With geographic data, it's possible to simulate certain natural processes, and this simulation is a big part of what working mappers do. Given elevation data for a mountain range, it's possible to simulate highlights and shadows for those mountains in light, in a process called hillshading.

More complex processes are also possible, like determining where water will collect after rainfall, called a 'watershed', or determining everywhere on the map that will be visible if you're standing at a mountaintop, called a 'viewshed'.

### Aggregation

The most common form of aggregation is the idea of a sum - given a large group of numbers, you can add them together to get an idea of them all at once. For instance, the GDP of a country is more immediately informative than simply listing every individual contribution to an economy.

Aggregation in maps is similarly used. Given granular data, like millions of individual household incomes, you can use mapping algorithms to sum or average the values inside of specific areas of the world, to show average income per town or city.

Aggregation is also used along with a technique called **binning**: given many discrete points, you can draw arbitrary shapes on the map, like squares or hexagons, and total all of the points that fall in each shape. That way, instead of having millions of tiny points that are difficult to grasp at a glance, you can style the map as a choropleth.

### Interpolation

Whereas aggregation takes lots of data and distills it into something simpler to analyze and visualize, interpolation 'fills in the blanks' between values. Interpolation is often used for datasets like elevation, where you have raster data values that records the height of every inch of a mountain, but there are a few places missing from the data - what programmers call `null` values.

Interpolation looks at the values around the 'blanks' and assumes that the missing values are basically similar to what's around them - a missing pixel at the top of a mountain will be assumed to be pretty high, while one in a valley is assumed to be low.

There are many ways to interpolate point data:

- **Heatmaps** assign each point a weight and represent density of point values in "hotter" colors.
- **Contour lines** take point sample data and draw lines around them that represent a continuous estimated value. Elevation maps often use this technique.
- A **TIN** (Triangulated Irregular Network) draws triangles between points that can be used to visualize terrains.
- **Voronoi diagrams** take a set of points and turn them into polygons of all of the area around them.

## Afterword

We hope this has been an enlightening and inspiring read: there is so much potential in this field and so many unanswered questions. Maps are a connected topic, stretching into art, mathematics, physics, ecology, and so much more.

We'd love if you can [report any issues or file any suggestions](https://tmcw.wufoo.com/forms/mapschool-feedback/) that came to mind in your read.

### License

[Creative Commons Attribution 3.0 Unported](http://creativecommons.org/licenses/by/3.0/deed.en_US)
