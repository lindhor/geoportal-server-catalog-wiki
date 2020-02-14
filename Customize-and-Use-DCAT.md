## Customize and Use DCAT

This page contains information about DCAT customization and usage.


### Use DCAT

There are two ways you can access DCAT output in Geoportal Server: 

1. Click on the "DCAT" link at the lower right corner of the search result page within Geoportal Server, it will show the current page of search result in DCAT format

2. Access the url at http://servername:8080/geoportal2/dcat.json, it will show the entire geoportal catalog in DCAT format. The DCAT output file is generated automatically by Geoportal Server on a daily basis (3:00 am everyday). Please note that new records created in Geoportal Server will not be shown in the DCAT output until next regeneration. However it is possible to manually update the DCAT output by deleting the old dcat.json file, then try to access http://servername:8080/geoportal2/dcat.json again, Geoportal Server will regenerate the dcat.json output (it might take some time).

### Customize DCAT

There are situation that you will need to customize the default DCAT output to meet the specification of your organization, the process would involve working with JavaScript files, following are the general steps:

1. Open \geoportal-search\src\main\resources\gs\writer\DcatWriter.js in an editor
2. You can change the default values such as bureauCode, programCode, publisher in the section `var DCAT_DEFAULTS  = {`
3. To add a custom field (such as language: "eng")  to the output it would involve the following:
   * Make sure the field you want to add to DCAT is in the index, you can check the name of field by open the JSON link in the search result page, in this example we will use field apiso_Language_s
   * in section `      var dcat = {`  after `        keyword: src.keywords_s || DCAT_DEFAULTS.keyword,` add the following :
  `
		language: src.apiso_Language_s || '<unknown>',
  `
   * Save the change, restart Geoportal Server and  access the DCAT output from search result.
**Note:** 

 * For DCAT output of the entire catalog, please regenerate the DCAT output after the customization.

 
