# Some test files causing issues with Titiler

The files in the repo are just intented to debug Titiler and/or rio_tiler which do not expect to receive a list of pixel types (one for each band whereas they are all the same)  
COG file causing troubles:
* s250-advanced/IMG_MSI_TCI_R60m_S2A_MSI__SGP_20220930T161339_af98.COG.TIFF

Possible linked issue: https://github.com/rasterio/rasterio/issues/2180 and this PR:  https://github.com/rasterio/rasterio/pull/1595
This also may be related to this GDAL issue: https://github.com/OSGeo/gdal/issues/2298
GDAL does not seem to handle correctly Signed Bytes (at least on version <=3.6)
Seems that the image bands are considered to have different pixels'types and returns a list. 
According to the metadata this is obviously not the case.

## Here's the error message I got

```Text
2023-09-18T14:53:52.076875911Z   File "/usr/local/lib/python3.9/site-packages/rio_tiler/io/base.py", line 118, in tile_exists
2023-09-18T14:53:52.076877954Z     (tile_bounds[0] < dst_bounds[2])
2023-09-18T14:53:52.076880074Z TypeError: '<' not supported between instances of 'float' and 'list'
```


## GDAL info output on the test COG file

```text
gdalinfo IMG_COG.TIFF|grep -A3 -i structure

Image Structure Metadata:
  COMPRESSION=LZW
  INTERLEAVE=PIXEL
  LAYOUT=COG
--
  Image Structure Metadata:
    PIXELTYPE=SIGNEDBYTE
Band 2 Block=512x512 Type=Byte, ColorInterp=Green
  Overviews: 915x915, 458x458, 229x229
  Image Structure Metadata:
    PIXELTYPE=SIGNEDBYTE
Band 3 Block=512x512 Type=Byte, ColorInterp=Blue
  Overviews: 915x915, 458x458, 229x229
  Image Structure Metadata:
    PIXELTYPE=SIGNEDBYTE
```

## Operating system

Ubuntu 22.04 64 bit

## Software versions

rasterio-1.3.8
rio_tiler-4.1.13
GDAL 3.6.4, released 2023/04/17
