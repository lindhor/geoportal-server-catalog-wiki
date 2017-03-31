## Customize search panel

In Progress  

The search panel consists of many search facets, you can add, delete or update the search facets by simply editing a html file named SearchPanel.html in [Tomcat8]/webapps/geoportal/app/main/templates/. Each search facets is included in a `<div> ...</div>` block.

* To delete a search facet
  * Open SearchPanel.html in a html editor
  * Find the `<div> ...</div>` block for the facets to be deleted
  * Comment out the block or delete the block
  * Save the file and refresh geoportal to verify if the facet is removed from the search panel
 
* To add a search facet
  * Open SearchPanel.html in a html editor
  * Find the `<div> ...</div>` block similar to the facets to be added
  * Copy and paste the `<div> ...</div>` block
  * Perform necessary editing
  * Save the file and refresh geoportal to verify if the facet is added to the search panel
  
* To update a search facet 
  * Open SearchPanel.html in a html editor
  * Find the `<div> ...</div>` block similar to the facets to be added
  * Make the necessary changes
  * Save the file and refresh geoportal to verify if the facet is updated in the search panel
  
 * **Description for some of the parameters**
   
Parameter Name | Description
-------------- | ------------
allowAggregation |
data-dojo-props |
data-dojo-type |
field |
interval |
label |
nestedPath |
open |
pointField |
toField |

