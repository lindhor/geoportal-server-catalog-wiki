Geoportal Server Catalog supports faceted search. The panel on the left-hand side of the search page shows available facets. With these facets, users can limit the items from the catalog that will show by zooming/panning to a specific map extent, selecting dates, types, etc. The following types of facets are supported:

- SearchBox - this is the typical free text search box. While a simple box, we support wildcards and a lot of ElasticSearch query language.
- SpatialFilter - no geospatial metadata catalog is complete without a spatial filter. Use the map widget to zoom/pan to your area and select what type of constraint to apply with that extent. Ours supports both intersect and within types of searches.
- CollectionsFilter - since not all items are alike, searching for them is neither. With the CollectionsFilter you can associate search facets to collections of catalog items.
- TemporalFilter - the temporal filter allows you to select periods of time for example based on the publication date or the time period for which the data is applicable. Essentially any date field in the metadata may be used in such a filter.
- BasicNumericFilter - If you have a field in the index such as year, or day of the year, or such, use this to provide a basic facet.
- NumericFilter - If you need more (you do!), the NumericFilter allows more control over what is displayed, intervals, etc.
- TermsAggregation - TermsAggregation is a powerful facet as it provides lists of terms and indicates the number of documents in the catalog that contain that term. The terms can originate from organization names, keywords, field names, etc.

For every filter there will be a basic set of HTML attributes to set on the ```div```:
- data-dojo-type - the type of filter (see above)
- data-dojo-props - a set of properties controlling the behavior and presence of the facet:
  - field: '<elastic field name>' - name of the field to be used in this facet
  - toField:'<elastic field name>' - for TemporalFilter facets, this and the field attribute complete the period to search in
  - interval:'<interval>' - for date facets: year | month, for numeric facets: 10, 100, 1000 ...
  - ticks: <number> - number of ticks in the NumericFilter facet
  - pointField: '<elastic field name>' - used to aggregate numbers of items in the SpatialFilter facet, typically 'envelope_cen_pt'
  - allowAggregation: true|false,
  - open: true|false,
  - label:'${i18n.search.criteria.map}'
  - conditionallyDisabled: 

            
