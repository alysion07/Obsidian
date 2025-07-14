# gdal2tiles 실행 예제 및 설명

## Best command to run gdal2tiles
gdal2tiles.py -z 8-21 -r bilinear -w leaflet -t "Ortho Map" -a 0 ortho\ortho_merged.tiff output_folder/

## Examples of running gdal2tiles
- gdal2tiles.py -z 3-18 ortho_merged.tiff output_folder/

- gdal2tiles.py -z 0-18 -r bilinear -w leaflet -t "Ortho Map" -a 0 ortho\ortho_merged.tiff output_folder/

- gdal2tiles.py -z 16-20 -r near -w google -t "JS Map" -a 0 ortho\ortho_merged.tiff output_folder/  

- gdal2tiles.py -z 15-21 -r bilinear -w leaflet -t "Ortho Map" -a 0 ortho\ortho_merged.tiff output_folder/

## Explanation of the command
Based on [official GDAL documentation](https://gdal.org/en/stable/programs/gdal2tiles.html)

`-z num-num` >> zoom level. 

`-r <RESAMPLING>` >> resampling method (e.g., bilinear, near, cubic).  
- `average`, `near`, `bilinear`, `cubic`, `cubicspline`, `lanczos`, `mode`, `max`, `min`, `med`, `q1`, `q3`

`-w <web viewer>` >> web viewer type (e.g., leaflet, google).  
- `leaflet`, `google`, `openlayers`, `mapserver`, `mapml`, `none` -default `all`

`-t <title>` >> title of the map.  

`-a alpha` >> alpha value for transparency.  

`input file` >> input raster file (e.g., ortho_merged.tiff).

`output folder` >> output folder for tiles (e.g., output_folder/).