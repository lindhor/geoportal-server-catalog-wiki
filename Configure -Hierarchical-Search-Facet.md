
Hierarchical structure is very common in metadata, from organizational structures to primary/secondary/tertiary subject categories, from geographic (e.g. World |US | California | Los Angeles...) to folder structure in data and metadata storage, it is possible to configure geoportal to use this hierarchical structure for releases after 2.6.4, the hierarchical structure will help user to quickly narrow down to the desired records.

## Configure hierarchical search

Lets say your metadata has primary/secondary/tertiary subject categories maintained in metadata element <gmd:MD_TopicCategoryCode>, the category are in the format of <gmd:MD_TopicCategoryCode>Category|Human|Economy|Recreation</gmd:MD_TopicCategoryCode>, and you want to add a hierarchical search facet for it so user can quickly search by multi level of category. Following are the steps to add a hierarchical search facet to geoportal:  * Update EvaluatorFor_ISO.js
  * Open geoportal/src/main/resources/metadata/js/EvaluatorFor_ISO.js in an editor
  * Find  section about hierarchical configuration "/* hierarchical category */", there are some description about additional configuration options there.
  * Uncomment "//    G.evalProps(task,item,root,"src_category_cat","//gmd:MD_TopicCategoryCode");"
  * If you are configuring a different metadata element field, please update to the correct field name and make sure it ends with "_cat"
  * Save the update
* Harvest ISO metadata
  * Create input broker that contain the desired metadata
  * Harvest the metadata into geoportal
  * Please note that the metadata need to be reharvested if the metadata is already in geoportal prior to this update in order to build the hierarchy
* Configure hierarchy search facet
  * Open geoportal/app/main/templates/SearchPanel.html in a html editor 
  * Find the following section, and update its property as needed. please visit [[Customize-search-panel.md]] on how to update the search facet

    ``
        <div class="g-filter-collapse" data-dojo-type="app/search/HierarchyTree"
          data-dojo-props="
            field:'src_category_cat',
            label:'${i18n.search.criteria.hierarchicalCategory}'">
        </div>  
    ``
  * Save the updates.

* Open geoportal to verify the facet is in the search panel. 
## Manage/update the hierarchy field

In some cases, there might be situation that the metadata itself does not have all the hierarchy information, or the hierarchy information need to be updated after the metadata has been harvested, geoportal make it possible to dynamically manage the hierarchy field, following are the steps (in addition to the steps above):

* Enable setField editing
  * Open geoportal/src/main/webapp/app/context/app-config.js in editor
  * Find the following section

    ``
  edit: {
    setField: {
      allow: false,
      adminOnly: false
    }
  },
    ``
  * Set allow to true
  * Set adminOnly to true
  * Save the changes
* Update hierarchy values for records
  * Open Geoportal > login as a administrator
  * Find the record to be updated
  * Go to Options >   Set Field > Advanced
  * Enter the field whose value will be updated, e.g. src_category_cat
  * Enter the value for that field, e.g. Category|Human|Economy|Recreation
  * Select one of the update option > Update
* Open geoportal to verify the hierarchy information has been updated for the record. 


## Frequently Asked Questions*
* Can I use a different hierarchy separator such as "<" instead of "|"?
  * Yes. By default, "|" is used as the separator, if other separator is desired, replace delimiter declaration in hierarchy_tokenizer and reverse_hierarchy_tokenizer within elastic-mappings.json and elastic-mappings-7.json, then recreate Elastic Search index.

* Can I configure hierarchy to be similar to a my WAF/UNC folder structure ?
  * Yes.

* Can I manage/update the hierarchy after the metadata has been harvested?
  * Yes. see section 2 above
  
* Can I configure hierarchy using fields in a database table?
  * Yes.


