# Using OGC API Records in the CSW Client
The CSW Client for ArcGIS Pro now includes initial support for the next generation of catalog specifications of OGC: OGC API Records.

The client itself works quite the same between CSW 2.0.2 and OGC API Records. 

## Adding an OGC API Records service

To add an OGC API Records service:
- Go to the settings tab and add the catalog.
- Enter the link to the OGC API Records service. For the time being, use the ```/items``` URL

```
https://demo.pygeoapi.io/master/collections/dutch-metadata/items
```

- Select Profile: 'OGC API: Records'
- Give the entry a name
- Click the 'save' button (yep, the floppy disk)

## Searching the OGC API Records service
on the Find tab:
- select the OGC API Records service
- enter your text and press Enter or click Find
- see the results. Using the toolbar, you can open the item details, which will use the HTML representation of the item directly from the source catalog. If the item includes an association to a WMS, this WMS can be added to the map.