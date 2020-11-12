## Configure Hierarchical Search facet

Hierarchical structure is very common in metadata, from organizational structures to primary/secondary/tertiary subject categories, from geographic (World |US | California | Los Angeles...) to folder structure in data and metadata storage, it is possible to configure geoportal to use this hierarchical structure for releases after 2.6.4, the hierarchical structure would help user to quickly narrow down to the desired records.

Lets say you want to add a hierarchical search facet for metadata element such as <gmd:MD_TopicCategoryCode>Category|Human|Economy|Recreation</gmd:MD_TopicCategoryCode>. Following are the steps to add a hierarchical search facet to geoportal:  * Update EvaluatorFor_ISO.js
  * Open EvaluatorFor_ISO.js in geoportal/src/main/resources/metadata/js/ for ISO metadata 
  * Find  section about hierarchical configuration (    /* hierarchical category */), there are some description about additional configuration options there.
  * Uncomment "//    G.evalProps(task,item,root,"src_category_cat","//gmd:MD_TopicCategoryCode");"
  * if you are configuring a different metadata element and field, please update the field and make sure it ends with "_cat"
  * Save the update
* Harvest ISO metadata
  * Create input broker that contain the desired metadata
  * Harvest the metadata into Geoportal
  * Please note that the metadata has to be reharvested if the metadata is already in geoportal prior to this update
* Configure search facet
  * Open SearchPanel.html (at geoportal/app/main/templates/) in a html editor 
  * Find the following section, and update its property as needed. please visit xxx on how to update the search facet

    ``
        <div class="g-filter-collapse" data-dojo-type="app/search/HierarchyTree"
          data-dojo-props="
            field:'src_category_cat',
            label:'${i18n.search.criteria.hierarchicalCategory}'">
        </div>  
    ``
  * Save the updates.

* Open geoportal to verify the facet is in the search panel 
In some cases, there might be situations that the metadata itself does not have all the hierarchy information and the hierarchy information need to be updated and managed after the metadata has already been harvested, this is possible as well in geoportal, following are the steps (in addition to the steps above):

* Enable edit Field
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
  * Options >   Set Field > Advanced
  * Enter the field whose value will be updated
  * Enter the value for that field
  * Select the option > Update
 
   
    

 * **Frequently Asked Questions**
* Can I use a different hierarchy separator such as "<" instead of "|"?
  Yes. If other separator is desired, replace delimiter declaration in hierarchy_tokenizer and reverse_hierarchy_tokenizer within elastic-mappings.json and elastic-mappings-7.json, then recreate Elastic Search index.

* Can I configure hierarchy to be similar to a my WAF/UNC folder structure ?
  * Yes

* Can I manage/update the hierarchy after the metadata has been harvested?
  * Yes
  
* Can I configure hierarchy using fields in a database table?
  * Yes


