Collections are used for scenarios where a different set of search facets would be preferred based on the metadata collection user select. For example, if a user picks Landsat image catalog, they would perform search based on cloud cover, azimuth, sun elevation, etc. If he picks regular metadata, they would search based on ISO category, organization, keywords, etc.

The collection functionality is implemented within Geoportal Server 2.6.5. The functionality is 

* Configurable - functionality could be enabled or disabled as requested,
* Ad hoc - collections could be added and deleted ad-hoc without elaborated configuration,
* Facets selection - selection of facets displayed on the screen can be determined upon selected collection.

## Enable Collections functionality

Following are the steps to enable collection in geoportal: 

* Edit app-context.xml configuration file in geoportal\WEB-INF\classes\config,
* Locate supportsCollections property within the geoportalContext bean,
* Change value of the property to true,
* Save the update,
* Restart the geoportal application.

## Configure facets available for each collection

Each facet can be configured to react to the selection of the collection. It will appear or it will be removed from the screen depending how it is configured and which collection has been selected by the user.

Configuration is done entirely through `SearchPanel.html`. Each facet is represented by any `<div>` element placed within parent <div> marked with `class="g-search-pane-left"`. Every facet is equipped with `data-dojo-props` property which is a comma separated list of configuration properties passed to facet object. In order to configure facet to react to user choice of collections, property named `allowedCollections` shall be added, for example:

```
<div class="g-filter-collapse" data-dojo-type="app/search/TemporalFilter"
  data-dojo-props="
    allowedCollections: ['data'],
    nestedPath:'timeperiod_nst',
    field:'timeperiod_nst.begin_dt',
    toField:'timeperiod_nst.end_dt',
    interval:'year',
    open:false,
    label:'${i18n.search.criteria.timePeriod}'">
</div>
```

### allowedCollections syntax
1. It has to be mentioned first that `allowedCollections `is a list of conditions which would turn facet on. It is enough to have one condition passing with multiple conditions failing to be able to turn facet on. For example:
```
allowedCollections: ['data','private']
```
will turn facet on if and only if one of the collections: 'data' or 'private' is selected by the user. Any other selection of collection or not having selected any of the collections will turn facet off.

2. For simplicity it is allowed to declare allowed collections without square brackets if facet should be activated for only one collection, for example:
```
allowedCollections: 'data'
```

3. Note that `allowedCollections: ['data']` will turn facet on only if user selected 'data' collection, but it will remain inactive if user did not select any collection. In order to express condition that facet shall be active if user eider selected particular collection or did not select any, empty string could be used, for example:
```
allowedCollections: ['','data']
```

4. It is also allowed to user '*' wildcard to express situation if facet should be turned on for any collection but turned off if not collection has been selected:
```
allowedCollections: ['*']
```

5. Condition could be negated by placing '!' in front of collection name, like below:
```
allowedCollections: ['!data']
```
In this case facet will be activated if any collection is selected except 'data' collection or if no collection has been selected.

6. Finally, each condition could be expressed using regular expressions, like below:
```
allowedCollections: ['/d.+/i']
```
This condition will react to any collection which name starts with the letter 'd' regardless whether it is lower or upper case.

## Assign records to a particular collection
* Sign in to geoportal as an administrative user,
* Search for records to be assigned,
* Click on "Options" in one of the records > choose "Set collections",
* Enter the collection name, use comma to separate them if multiple collections,
* Choose one of the options to be applied, Click on "Update".

## Harvest records to a particular collection
With Geoportal Harvester (https://github.com/Esri/geoportal-server-harvester), it is possible to harvest records into one particular or multiple collections in geoportal, following are the steps:
* Define a geoportal 2.x output broker (Harvester > Brokers > Output brokers > Add),
* For the "Collections" field, enter the name of the collection, use comma to separate them if multiple collections, 
* For the "Collections field name", leave it empty if using default field "src_collections_s" or enter a custom field name,
* Define a harvester task connecting an input broker and the defined output broker with collection information,
* Click on "run" in the task panel to harvest the records,
* In Geoportal interface, check the collections with matching records are harvested successfully. 
