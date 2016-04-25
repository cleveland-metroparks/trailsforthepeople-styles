trailsforthepeople-styles
=========================

tilestache styles for the trailsforthepeople application at http://maps.clevelandmetroparks.com, evolved from styles developed by ![GreenInfo Network](http://www.greeninfo.org/).

![](http://smathermather.files.wordpress.com/2014/03/full-complement-of-features.png)

Configuration of individual map layers can be found in the mapfiles subdirectory. The interactions / combination of those layers in a baselayer are described in tilestache.cfg, an excerpt of which describing the order and relationship of those layers is as follows:

```json
            { "color":"#B6DEF1" }
            ,{ "src":"hillshade", "mask":"landmask" }
            ,{ "color":"#E7E5D8", "mask":"landmask", "opacity":0.80, "mode":"screen" }
            ,{ "src":"cuva", "opacity":0.90, "mode":"multiply" }
            ,{ "src":"parks_other", "opacity":0.80, "mode":"multiply" }
            ,{ "src":"parks", "opacity":0.80, "mode":"multiply" }
            ,{ "src":"canopy", "zoom":"15-20", "opacity":0.5, "mode":"multiply" }
            ,{ "src":"beaches", "zoom":"15-20", "opacity":1.0 }
            ,{ "src":"contours", "zoom":"15-20", "opacity":0.50, "mode":"multiply" }
            ,{ "src":"water", "opacity":1.00 }
            ,{ "src":"parking", "zoom":"17-20", "opacity":1.0, "mode":"multiply"}
            ,{ "src":"features", "opacity":1.00}
```
The magic is in the use of a DEM flattened with a multiplication factor of 0.00001, grey it back a bit with #E7E5D8, and then apply green to the parks (#AAD46E) with multiply as the composite mode. Given the brightness of this green, it mearly colorizes the DEM without changing the shading or making it muddy. At higher zoom levels (>15) forest canopy is added using the same approach as are contours.

Note: This configuration is the baselayer only and does not include symbolization of trails, highway shields, and other layers.

![](http://smathermather.files.wordpress.com/2014/03/custom_test.png)
