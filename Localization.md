## Localization

In Progress  

Geoportal interface can be localized to display in other languages. Geoportal leverage dojo nls for localization support, please reference [dojo/i18n](https://dojotoolkit.org/reference-guide/1.9/dojo/i18n.html) for details about dojo nls. In general, to add support for a particular language in Geoportal, it would be involve the following steps:

* Create a folder  under [Tomcat8]/webapps/geoportal/app/nls and name it the same as the language code, e.g. "de" for German, "fr" for French, "es" for Spanish.
* Copy resources.js from [Tomcat8]/webapps/geoportal/app/nls to the sub folder just created.
* Open resources.js and localize the value of the variables to the specific language.
* Remove text `root: {"` (near the top) and ` "}" `(near the bottom)
   ```
     root: {                // just below line "define({"
     ...
       }                    // second last line
   });       
   ```    
* Save the updates
* Open resource.js under folder nls
* Add the language reference ` "de": 1` (near the bottom of the file) and save the updates.
   ```
   ...
     }),
     "de": 1
   });     
   ```  
* Comment out ` locale: "en", ` in index.html under [Tomcat8]/webapps/geoportal and save the changes


The language chosen for display is selected based on the settings in a user's Internet browser. 
