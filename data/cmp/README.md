This directory is Cleveland data, extracted from the database into shapefiles.
This includes:
- reservations (parks)
- water lines and water polygons, to supplement the OSM stuff
- contour lines provided by CMP
- those contours cropped to display only with reservation boundaries

The contours shapefile is cropped to have only contours within reservations (parks),
which improves performance and avoids visual clutter (contour lines on streets).
This is cropped by Steve Mather from their own datasets, and is imported as follows:
- he provided a PgSQL database dump file
- this is imported as any database restore file:
```BASH
   pg_restore contours.backup | psql -U postgres
```
- some simplifications to the geometry, make it 2D, reproject to WGS84
```sql
    alter table contour_2_cm_only rename column the_geom to old_geom;
    select addgeometrycolumn('','contour_2_cm_only','the_geom',4326,'LINESTRING',2);
    update contour_2_cm_only set the_geom=ST_TRANSFORM(ST_FORCE_2D(ST_SETSRID(old_geom,3734)),4326) WHERE substr(st_astext(old_geom),1,10) = 'LINESTRING';
    update contour_2_cm_only set the_geom=ST_TRANSFORM(ST_FORCE_2D(ST_SETSRID(ST_GEOMETRYN(old_geom,1),3734)),4326) WHERE substr(st_astext(old_geom),1,10) = 'MULTILINES';
    alter table contour_2_cm_only drop column old_geom;
```
- the table is then exported using pgsql2shp, and indexed for performance:
```BASH
    pgsql2shp -u postgres -f contours_cropped_1.shp path 'select * from contour_2_cm_only WHERE  gid % 2 = 0'
    pgsql2shp -u postgres -f contours_cropped_2.shp path 'select * from contour_2_cm_only WHERE  gid % 2 = 1'
    shapeindex --shape_files contours_cropped_1.shp
    shapeindex --shape_files contours_cropped_2.shp
```
