## Customize search panel

The search panel consists of many search facets, you can add, delete or update the search facets by editing a html file named SearchPanel.html in [Tomcat8]/webapps/geoportal/app/main/templates/. Each search facets is included in a `<div> ...</div>` block.

* To delete a search facet
  * Open SearchPanel.html in a html editor
  * Find the `<div> ...</div>` block for the facets to be deleted
  * Comment out the block or delete the block
  * Save the file and refresh geoportal to verify if the facet is removed from the search panel
 
* To add a search facet
  * Open SearchPanel.html in a html editor
  * Find the `<div> ...</div>` block similar to the facets to be added, e.g.
    ```
        <div data-dojo-type="app/search/TermsAggregation"
        data-dojo-props="field:'sys_owner_s',open:false,label:'${i18n.search.criteria.owner}'">
      </div>
    ```
  * Copy and paste the `<div> ...</div>` block to the appropriate location
  * Perform necessary editing, for a facet for "contact_role_s", it will be something like below. For an explanation of the parameters, please see description in the table below
    ```
         <div data-dojo-type="app/search/TermsAggregation"
        data-dojo-props="field:'contact_role_s',open:true,label:'contact_role'">
      </div>
    ```
  * Save the file and refresh geoportal to verify the facet is added to the search panel
  
* To update a search facet 
  * Open SearchPanel.html in a html editor
  * Find the `<div> ...</div>` block for the facets to be updated
  * Make the necessary updates, for an explanation of the parameters, please see description in the table below
  * Save the file and refresh geoportal to verify if the facet is updated in the search panel
  
 * **Description for some of the parameters**
   
Parameter Name | Description
-------------- | ------------
allowAggregation | Showing aggregation of records
data-dojo-props | List of properties for the facet
data-dojo-type | Template type
field | Name of Elasticsearch field, an easy way to find the field name is to look at the json output in search results  
interval | Units for clustering, counts per year/day etc., used in the temporal facet
label | Label to be shown on the search panel, it can be either a string or variable pointing to a string
nestedPath | Nested query in Elasticsearch, e.g. nestedPath:'timeperiod_nst' in time period facets
open | If set to true, the facet will be open when geoportal starts.
pointField | Name of point field in Elasticsearch  
toField | Name of field in Elasticsearch, e.g. for end date in time period facet

