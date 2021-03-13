==Introduction==
The CSW Clients bring geoportal discovery functionality into your ArcGIS Desktop applications. They enable searching [http://www.opengeospatial.org/standards/cat OGC Catalog Service for the Web](OGC CSW) catalogs directly from your favorite desktop GIS applications. Once resources are returned, they can be viewed or downloaded. Additionally, if the resources are live services they can be added to ArcGIS Pro or ArcMap. 

'''Note:''' The CSW Client for ArcGIS Pro and for ArcMap are separate addins built specifically for those products.

==Install the CSW Clients for ArcGIS==
===Steps to install===
*Download the CSW Client code that corresponds to your ArcGIS application. Follow the instructions given with the source code to setup your development environment and build the addin.

Follow the instructions for your desktop GIS to install the addin:
- [ArcGIS Pro](https://pro.arcgis.com/en/pro-app/latest/get-started/manage-add-ins.htm)
- [ArcMap](https://desktop.arcgis.com/en/arcmap/latest/analyze/python-addins/sharing-and-installing-add-ins.htm) 

The CSW Client is now ready to be used. Read the section below for how to use the CSW Client.

==Use the CSW Clients for ArcGIS==

The CSW Clients are used the same way in both ArcGIS Pro and ArcMap. How to search a catalog for resources, view metadata, download metadata, and add a resource to the map are described below.

===Search a Catalog===
To conduct a search:
*Click on the '''Find''' tab.
*Type a term in the '''Find''' text box.
'''Note:'''  If you do not input search criteria, the maximum results that can be retrieved is 100. If you attempt to retrieve more than 100, you will get a "no results found" message.
*Choose one of the catalog services from the '''In Catalog''' dropdown menu. To configure additional catalogs, see '''How to Configure Additional Catalogs''' below.
*Enter parameters to further refine your search (optional). Search parameters are described below.
**'''Live Data and Maps Only''': The search can be limited to live service resources by checking the '''Live Data and Maps Only''' checkbox. Live services can be added to the ArcMap or ArcGIS Explorer document as a layer.
'''Note:''' If you have not input a search term but you do check the '''Live Data and Maps Only''' box, it is possible to retrieve more than 100 records because the search query is then populated with a search parameter, ''content type = live service''.
**'''Maximum''': To limit the number of records returned in the search results, change the value in the '''Maximum''' box. The limit is 500 when search parameters are input, and 100 for a search with no parameters.
**'''Use Current Extent''' (ArcMap only): If an extent is already defined in the ArcMap document window, this box can be checked and the CSW Client will only retrieve results that fall into the defined extent. Some CSW service URLs do not enable extent requests, so this may affect the number of results returned.
*Click the '''Find''' button to execute the search. Results are returned in the '''Results''' window. For each result resource returned, the '''Abstract''' window will display the abstract information for the selected resource if abstract information is provided in its metadata.

===Use Search Results===

Notice the tools located above the '''Results''' window. There are seven buttons that display, representing functionality related to the search results. Each button's associated functionality is described below.
*The '''View Metadata''' tool [[view_metadata.png]] will open the resource's metadata document as XML in a new window.
*The D'''ownload Metadata''' tool [[download_metadata.png]] allows the resource's metadata XML to be downloaded and saved.
*The '''Add to Map''' tool [[addtomap.png]] will add the service described by the resource to the map or globe as a new layer. This button will be grayed out unless the resource has a content type of "Live Data and Maps". If you click the Add to Map button and no data is added to the map, it is possible that the resource's metadata does not include service information. In order to display the live service, service connection information must be provided in the metadata xml for that resource.

The next four buttons are related to the display of a resource's spatial footprint in the current map window. The extent shown in the footprint corresponds to the bounding box defined in the resource's metadata.
*The '''Display Footprint''' tool will show the extent of the selected resource overlaid on the map document.
*The '''Zoom to Footprint''' tool [[zoom_footprint.png]] will zoom to the extent of the selected resource. This is especially useful if the extent is a small region or the basemap uses a different projection.
*The '''Show All Footprint''' tool [[show_footprint.png]] will add all the footprints in the Results window to the map at once. To hide them, click the same button again.
*The '''Clear All Footprint''' tool [[clear_footprint.png]] will remove all footprints that have been added to the map.

Use these tools to view resource metadata, download it, and add the service or its footprint to the map or globe.

==How the CSW Clients Work==

The CSW Clients need three pieces of information in order to retrieve records from a catalog service for display in ArcMap or ArcGIS Explorer.
#'''A GetCapabilities URL to the catalog:''' This is input in the Catalog Service URL field on the Configure tab of the CSW Clients. The GetCapabilities URL provides the catalog endpoint to which the CSW Clients send the search request and from which they receive the response.
#'''Search criteria:''' This is entered into the fields on the Find tab of the CSW Clients. Search criteria define what resources are returned by the CSW Clients. Search criteria act as filters, such as filtering by a search term, if data should be Live Data only, and the maximum number of search results returned.
#A '''catalog profile:''' The catalog service's profile is selected from a dropdown menu on the Configure tab of the CSW Clients. The catalog profile designation indicates to the CSW Clients which profile the catalog service uses. The mapping between the catalog service and its profile is defined in a file called <tt>CSWCatalogs.xml</tt>. This file is updated each time a new catalog is configured with the CSW Clients Configure tab. The CSW Clients reference <tt>CSWCatalogs.xml</tt> to find out what profile the catalog uses. The list of available profiles is defined in another file, <tt>CSWProfiles.xml</tt>. The profile will indicate which xslt files will be used to formulate requests between the CSW Clients and the catalog.

The CSW Clients maintain a list of supported profiles in a file called CSWProfiles.xml. Navigate to the <tt>\\CSWClients\Data</tt> directory and open the <tt>CSWProfiles.xml</tt> file. Notice that for each profile in the list, there are three xslt files referenced: GetRecords_Request, GetRecords_Response, and GetRecordByID_Response. These three xslt's transform requests and responses to and from the catalog service for interaction with the ArcMap or ArcGIS Explorer interface. An excerpt from the <tt>CSWProfiles.xml</tt> file, showing the ArcGIS Server Geoportal Extension profile, is shown below.

```
<?xml version="1.0" encoding="utf-8" ?> 
<CSWProfiles> 
  <!-- OGCCORE ESRI GPT -->
  <Profile>
    <ID>urn:ogc:CSW:2.0.2:HTTP:OGCCORE:ESRI:GPT</ID> 
    <Name>ArcGIS Server Geoportal Extension</Name>
    <CswNamespace>http://www.opengis.net/cat/csw/2.0.2</CswNamespace> 
    <Description /> 
    <GetRecords>
      <XSLTransformations> 
        <Request>CSW_2.0.2_OGCCORE_ESRI_GPT_GetRecords_Request.xslt</Request>
        <Response>CSW_2.0.2_OGCCORE_ESRI_GPT_GetRecords_Response.xslt</Response>
      </XSLTransformations>
    </GetRecords>
    <GetRecordByID> 
      <RequestKVPs>
        <![CDATA[service=CSW&request=GetRecordById&version=2.0.2&ElementSetName=full]]> 
      </RequestKVPs>
      <XSLTransformations> 
        <Response>CSW_2.0.2_OGCCORE_ESRI_GPT_GetRecordByID_Response.xslt</Response>
      </XSLTransformations> 
    </GetRecordByID> 
    <SupportSpatialQuery>True</SupportSpatialQuery>
    <SupportContentTypeQuery>True</SupportContentTypeQuery> 
    <SupportSpatialBoundary>True</SupportSpatialBoundary> 
  </Profile>
```

===Flow of information using the xslt's===

Conceptually, there are three spaces of interaction when using the CSW Clients: the CSW Clients user interface (UI), the CSW Clients translation activity, and the catalog service with which the CSW Clients interact. Interactions take place when the user initiates a search using the CSW Clients and when the user initiates communication with the catalog service using the CSW Clients' '''View Metadata, Download Metadata, Add to Map''', and '''View Footprint''' buttons. The three diagrams below show how the UI, the CSW Clients translation activity, and the catalog service interact to carry out search and display of metadata information in ArcMap or ArcGIS Explorer.

====Initial Request to a Service====
The initial communication flow for the CS-W request and response is shown in the diagram below, and an explanation of the numbered steps follows.

[[initial_communication_csw.png]]

*'''1.''' When a user clicks the '''Find''' button for the CSW Clients, the CSW Clients references the <tt>CSWCatalogs.xml</tt> file to find the corresponding profile for that catalog service. The corresponding profile determines which GetRecordsRequest, GetRecordsResponse, and GetRecordByID_Response files will be used. Then, the CSW Clients create a generic xml. The generic xml looks like this:

```
<csw:GetRecords 
 version="2.0.2" 
 service="CSW"
 resultType="RESULTS" 
 startPosition="1" 
 maxRecords="10"
 xmlns:csw="http://www.opengis.net/cat/csw/2.0.2">
  <csw:Query
   typeNames="csw:Record"
   xmlns:ogc="http://www.opengis.net/ogc"
   xmlns:dc="http://purl.org/dc/elements/1.1/"
   xmlns:gml="http://www.opengis.net/gml">
    <csw:ElementSetName>full</csw:ElementSetName>
    <csw:Constraint version="1.0.0">
      <ogc:Filter xmlns="http://www.opengis.net/ogc">
        <ogc:And>
          <ogc:PropertyIsLike
           wildCard=""
           escape=""
           singleChar="">
            <ogc:PropertyName>AnyText</ogc:PropertyName>
            <ogc:Literal>wms</ogc:Literal>
          </ogc:PropertyIsLike>
          <ogc:PropertyIsEqualTo>
            <ogc:PropertyName>Format</ogc:PropertyName>
            <ogc:Literal>liveData</ogc:Literal>
          </ogc:PropertyIsEqualTo>
        </ogc:And>
      </ogc:Filter>
    </csw:Constraint>
  </csw:Query>
</csw:GetRecords>
```

Look closely at what information is being transferred in this piece of code. Each bit of information corresponds to what is input into the CSW Clients UI. In the GetRecords element, there is an attribute telling how many records should be retrieved - maxRecords, and in this example, ten records will be returned. There is an element defining the search term that is input - PropertyName AnyText, and in this example the search term is 'wms'. Then there is a parameter for retrieving the content type of the document, if available - PropertyName Format, and in this example, records to be retrieved should be liveData (the Live Data and Maps Only checkbox in the UI).
'''Note:''' There can be additional parameters in this generic xml for spatial extent, ISO Topic Category, and Modified Date. The parameters for ISO Topic and Modified Date are for use within the geoportal web application alone, and do not need to be specified for use with the CSW Clients.

*'''2.'''The CSW Clients interpret the generic XML, and applies a transformation according to the GetRecords_Request.xslt that translates the generic XML into a CS-W GetRecordsRequest so the catalog service can interpret the search criteria.
*'''3.'''The transformed xml is passed as a GetRecordsRequest to the catalog service.
*'''4.'''The catalog service responds with a list of records that match the search criteria, formatted in CS-W syntax.
*'''5.'''The CSW Client applies the GetRecords_Response.xslt to translate the catalog service response into resulting records for display by the CSW Client interface. Abstract, Footprint information for the Footprint buttons, and if the records are Live Data are also returned for each record.

====View Metadata and Download Metadata buttons====
For the '''View Metadata''' and '''Download Metadata''' buttons, the CSW Clients do not need to apply stylesheets to interpret user input or display the catalog service's response. Instead, the CSW Clients use the catalog service's native GetRecordById requests and responses to retrieve the metadata. This communication flow is shown in the diagram below, and an explanation of the numbered steps follows.

[[view_download_communication_csw.png]]

#When a user clicks the '''View Metadata''' or '''Download Metadata''' buttons, the CSW Clients request the full metadata document for the record using the catalog service's innate GetRecordById request defined in its GetCapabilities.
#The catalog service responds with a GetRecordById response, and the full metadata is returned to the user interface.

====Add to Map button====
An additional transformation, called the GetRecordByID_Response.xslt, comes into play when the user clicks the '''Add to Map''' button. The communication flow is shown in the diagram below, and an explanation of the numbered steps follows.

[[get_record_by_id_csw.png]]

#When a user clicks the Add to Map or Add Footprints button, the CSW Clients retrieve the GetRecordByID response from the catalog service.
#The CSW Clients then apply the GetRecordByID_Response.xslt to the response to extract the URL that is used for the live service.
#The URL is then sent to the user interface for Add to Map, or the footprint of the metadata as defined by the envelope information in the metadata.

==How to Configure Additional Catalogs==

The CSW Clients are preconfigured to search Geospatial OneStop - the GOS2 OGC Core catalog in the search interface - by default. But if you want to search a different catalog service than Geospatial OneStop, it is possible by registering the catalog service in the CSW Clients' Configure tab.

===How to register a catalog service for use in the CSW Clients===
*Start by clicking the '''Configure''' tab. On this tab you will see three input fields and three buttons. You will enter information into these fields that define the connection to the catalog service and provide the name for the service in the user interface.
*Click the '''New''' button [[new_csw_profile.png]] to clear the form for your new catalog service entry.
*In the '''Catalog Service URL''' input box, type the GetCapabilities URL of the service you wish to add. If you are registering your Geoportal Server catalog service, the format for the URL will be similar to the following:

```
http://serverName/geoportal/csw/discovery?Request=GetCapabilities&Service=CSW&Version=2.0.2
```

*In the '''Profile''' drop-down list, specify the profile that the catalog service uses. It is sometimes possible to find this information by pasting the GetCapabilities URL in a web browser and reading the resulting response for clues. If you cannot determine the profile from looking at the GetCapabilities response, you may have to contact the catalog service's host organization and ask about the profile. If you are registering a Geoportal Server catalog service, the profile will be the '''ArcGIS Server Geoportal Extension''' profile. If the catalog service uses a profile that is not in the default dropdown list, you can add the profile. See How to Author and Add a New Profile section for instructions.
*Provide a display name for the catalog service by typing a name in the '''Display Name''' field.
*Click the '''Save''' button [[save_csw_profile.png]] to add the new catalog to the catalog list.
*Now, activate the '''Find''' tab. Your newly registered catalog service should appear in the '''In Catalog''' drop-down list.

===How to update an existing catalog service from the list===
*Click the '''Configure''' tab.
*Highlight the catalog service that is going to be updated in the Catalogs list. The Catalog Service URL, Profile, and Display Name will be loaded into the interface.
*Make your changes to these fields.
*Click the '''Save''' button.

===How to delete an existing catalog service from the list===
*Click the '''Configure''' tab.
*Highlight the catalog service that you want to delete from the registered catalog list.
*Click the '''Delete''' button [[delete_csw_profile.png]].
*The system will display a confirmation dialog box. Click '''Yes''' to confirm the delete.

==How to Author and Add a New Profile==
If you want to register a catalog service that does not use one of the profiles that the CSW Clients provide by default, then you will need to author the three xslt's used for retrieving the search results and extracting the URL used to add live data to the map. These three files were referenced in the How the CSW Clients Work section. After authoring these three xslt files, you must reference them with a new profile entry in the <tt>CSWProfiles.xml</tt> file. The section below provides details about how to author each xslt, and then how to add the reference to the <tt>CSWProfiles.xml</tt> file.

===Author the GetRecords_Request.xslt===
Remember that the CSW Clients apply a transformation that translates the generic XML from the CSW Clients user interface into a CS-W GetRecordsRequest that the catalog service can interpret. The code snippet below shows a generic GetRecords_Request.xslt with annotation in the commented sections describing details for important parts of the file. Copy this text into a text editor such as Notepad, and make the adjustments described in the comments.

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet
 version="1.0"
 xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output
   method="xml"
   indent="no"
   encoding="UTF-8"
   omit-xml-declaration="yes"/>

    <xsl:template match="/">
    <xsl:element
     name="csw:GetRecords"
     use-attribute-sets="GetRecordsAttributes"
     xmlns:csw="http://www.opengis.net/cat/csw/2.0.2"
     xmlns:ogc="http://www.opengis.net/ogc"
     xmlns:dc="http://purl.org/dc/elements/1.1/"
     xmlns:gml="http://www.opengis.net/gml">
      <csw:Query typeNames="csw:Record">
        <csw:ElementSetName>full</csw:ElementSetName>
        <csw:Constraint version="1.0.0">
          <ogc:Filter xmlns="http://www.opengis.net/ogc">
            <ogc:And>
              <!--
                CSW Client must retrieve the following information to
                define the search the user specifies in the User Interface:
                'KeyWord' corresponds to the search term the user inputs,
                'LiveDataMap' corresponds to if the user specifies "Live Data and
                Maps Only", and 'Envelope' specifies the bounding box for a search
                based on the current extent of the ArcMap window. The 'tmpltDate'
                corresponds to searching a catalog via the geoportal, in the
                Additional Options, Modified Date search on the Search page.
              -->
              <!-- Key Word search -->
              <xsl:apply-templates select="/GetRecords/KeyWord"/>
              <!-- LiveDataOrMaps search -->
              <xsl:apply-templates select="/GetRecords/LiveDataMap"/>
              <!-- Envelope search, e.g. ogc:BBOX --> 
              <xsl:apply-templates select="/GetRecords/Envelope"/>
              <!-- Date Range Search -->
              <xsl:call-template name="tmpltDate"/>
            </ogc:And>
          </ogc:Filter>
        </csw:Constraint>
      </csw:Query>
    </xsl:element>
  </xsl:template>

  <!--
    key word search : This template is used to pass the search
    term to the catalog service. The PropertyName 'AnyText' is
    variable, depending on what your catalog service accepts. 'AnyText'
    will search all fields, irrespective of which XML element. If you
    wanted to search only the title or abstract, you could change this
    PropertyName parameter accordingly. The 'PropertyIsLike' elements
    (wildCard="" escape= "" singleChar="") are specific to the CSW
    specification your catalog service follows. 
  -->
  <xsl:template match="/GetRecords/KeyWord"
   xmlns:ogc="http://www.opengis.net/ogc">
    <xsl:if test="normalize-space(.)!=''">
      <ogc:PropertyIsLike wildCard="" escape="" singleChar="">
        <ogc:PropertyName>AnyText</ogc:PropertyName>
        <ogc:Literal>
          <xsl:value-of select="."/>
        </ogc:Literal>
      </ogc:PropertyIsLike>
    </xsl:if>
  </xsl:template>

  <!-- 
    LiveDataOrMaps search: This template is used to pass the
    requirement to retrieve only live data/map records from the catalog
    service. The PropertyName 'Format' depends on the parameter your
    catalog service accepts to define the type of resource the
    resulting record describes. The Literal element "liveData" can be
    changed to indicate the term your service may use to retrieve live
    data records.
  -->
  <xsl:template match="/GetRecords/LiveDataMap"
   xmlns:ogc="http://www.opengis.net/ogc">
    <xsl:if test="translate(normalize-space(./text()),'true', 'TRUE') ='TRUE'">
      <ogc:PropertyIsEqualTo>
        <ogc:PropertyName>Format</ogc:PropertyName>
        <ogc:Literal>liveData</ogc:Literal>
      </ogc:PropertyIsEqualTo>
    </xsl:if>
  </xsl:template>

  <!--
    Envelope search: This template is used to define a bounding
    box for resulting records returned from the catalog service if the
    "Use Current Extent" option is selected (ArcMap CSW Client only).
    Resulting records must fall within this bounding box. Do not change
    the PropertyName, Box, or coordinates elements.
  -->
  <xsl:template match="/GetRecords/Envelope"
   xmlns:ogc="http://www.opengis.net/ogc">
    <!-- generate BBOX query if minx, miny, maxx, maxy are provided -->
    <xsl:if test="./MinX and ./MinY and ./MaxX and ./MaxY">
      <ogc:BBOX xmlns:gml="http://www.opengis.net/gml">
        <ogc:PropertyName>Geometry</ogc:PropertyName>
        <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
          <gml:coordinates>
            <xsl:value-of select="MinX"/>,<xsl:value-of
             select="MinY"/>,<xsl:value-of
             select="MaxX"/>,<xsl:value-of select="MaxY"/>
          </gml:coordinates>
        </gml:Box>
      </ogc:BBOX>
    </xsl:if>
  </xsl:template>
 
  <!--
    tmpltDate: This template is used to define the date range
    for when resulting records returned from the catalog service were
    modified. This is only used for the geoportal search, and not the
    CSW Client search. This section needs to be included only if you
    want to apply your custom profile to the geoportal itself as well
    as the CSW Client (the custom profile would appear in the dropdown
    list of profiles when a publisher user registers a CSW repository).
    Do not change this section.
  -->
  <xsl:template name="tmpltDate" xmlns:ogc="http://www.opengis.net/ogc">
    <xsl:if test="string-length(normalize-space(/GetRecords/FromDate/text())) &gt; 0">
      <ogc:PropertyIsGreaterThanOrEqualTo>
        <ogc:PropertyName>Modified</ogc:PropertyName>
        <ogc:Literal>
          <xsl:value-of select="normalize-space(/GetRecords/FromDate/text())"/>
        </ogc:Literal>
      </ogc:PropertyIsGreaterThanOrEqualTo>
    </xsl:if>
    <xsl:if test="string-length(normalize-space(/GetRecords/ToDate/text())) &gt; 0">
      <ogc:PropertyIsLessThanOrEqualTo>
        <ogc:PropertyName>Modified</ogc:PropertyName>
        <ogc:Literal>
          <xsl:value-of select="normalize-space(/GetRecords/ToDate/text())"/>
        </ogc:Literal>
      </ogc:PropertyIsLessThanOrEqualTo>
    </xsl:if>
  </xsl:template>

  <xsl:attribute-set name="GetRecordsAttributes">
    <xsl:attribute name="version">2.0.2</xsl:attribute>
    <xsl:attribute name="service">CSW</xsl:attribute>
    <xsl:attribute name="resultType">RESULTS</xsl:attribute>
    <xsl:attribute name="startPosition">
      <xsl:value-of select="/GetRecords/StartPosition"/>
    </xsl:attribute>
    <xsl:attribute name="maxRecords">
      <xsl:value-of select="/GetRecords/MaxRecords"/>
    </xsl:attribute>
  </xsl:attribute-set>
</xsl:stylesheet>
```

When you have finished editing the text, save the file as ''GetRecords_Request.xslt''.

===Author the GetRecords_Response.xslt===
Remember that the CSW Clients apply a transformation that translates the catalog service response to the submitted search criteria into resulting records for display by the CSW Client interface. The code snippet below shows a generic GetRecords_Response.xslt with annotation in the commented sections describing details for important parts of the file. Copy this text into a text editor and make the adjustments described in the comments. Note that where specific metadata elements are referenced in select statements in this example, you need to verify the metadata schema that your catalog service uses and that the selected elements comply with that schema. For example, in the <ID> element below, the example shows that the ''dc:identifier'' element will be selected. This is correct if the catalog service's response is based on Dublin Core. But if your catalog service's response is based on ISO 19139, the <ID> element's select statement should be changed to ''gmd:fileIdentifier/gco:CharacterString'' instead.

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0"
 xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
 xmlns:csw="http://www.opengis.net/cat/csw/2.0.2"
 xmlns:dct="http://purl.org/dc/terms/"
 xmlns:ows="http://www.opengis.net/ows"
 xmlns:dc="http://purl.org/dc/elements/1.1/"
 exclude-result-prefixes="csw dc dct ows">
  <xsl:output method="xml" indent="no" encoding="UTF-8"
   omit-xml-declaration="yes" />

  <xsl:template match="/">
    <xsl:choose>
      <!-- if CSW response returns some exception, do the following -->
      <xsl:when test="/ows:ExceptionReport">
        <exception>
          <exceptionText>
            <xsl:for-each
             select="/ows:ExceptionReport/ows:Exception">
              <xsl:value-of select="ows:ExceptionText"/>
            </xsl:for-each>
          </exceptionText>
        </exception>
      </xsl:when>
      <xsl:otherwise>
        <Records>
          <xsl:attribute name="maxRecords">
            <xsl:value-of
             select="/csw:GetRecordsResponse/csw:SearchResults/@numberOfRecordsMatched"/>
          </xsl:attribute>
          <xsl:for-each
           select="/csw:GetRecordsResponse/csw:SearchResults/csw:Record |
                   /csw:GetRecordsResponse/csw:SearchResults/csw:BriefRecord |
                   /csw:GetRecordByIdResponse/csw:Record |
                   /csw:GetRecordsResponse/csw:SearchResults/csw:SummaryRecord">
            <Record>
              <ID>
                <xsl:choose>
                  <xsl:when
                     test="string-length(normalize-space(dc:identifier[@scheme=
                          'urn:x- esri:specification:ServiceType:ArcIMS:Metadata:DocID']/text())) > 0">
                    <xsl:value-of
                     select="normalize-space(dc:identifier[@scheme='urn:x-
                     esri:specification:ServiceType:ArcIMS:Metadata:DocID'])"/>
                  </xsl:when>
                  <xsl:otherwise>
                    <xsl:value-of select="normalize-space(dc:identifier)"/>
                  </xsl:otherwise>
                </xsl:choose>
              </ID>
              <Title>
                <xsl:value-of select="dc:title"/>
              </Title>
              <Abstract>
                <xsl:value-of select="dct:abstract"/>
              </Abstract>
              <Type>
                <xsl:value-of select="dc:type"/>
              </Type>
              <!--
                each profile could have a different type of
                bounding box. Some of have two, some have four. This one has four
              -->
              <LowerCorner>
                <xsl:value-of
                 select="ows:WGS84BoundingBox/ows:LowerCorner"/>
               </LowerCorner>
              <UpperCorner>
                <xsl:value-of
                 select="ows:WGS84BoundingBox/ows:UpperCorner"/>
              </UpperCorner>
              <MaxX>
                <xsl:value-of
                 select="normalize-space(substring-before(ows:WGS84BoundingBox/ows:UpperCorner,' '))"/>
              </MaxX>
              <MaxY>
                <xsl:value-of
                 select="normalize-space(substring-after(ows:WGS84BoundingBox/ows:UpperCorner,' '))"/>
              </MaxY>
              <MinX>
                <xsl:value-of
                 select="normalize-space(substring-before(ows:WGS84BoundingBox/ows:LowerCorner,' '))"/>
              </MinX>
              <MinY>
                <xsl:value-of
                 select="normalize-space(substring-after(ows:WGS84BoundingBox/ows:LowerCorner,' '))"/>
              </MinY>
              <ModifiedDate>
                <xsl:value-of select="./dct:modified"/>
              </ModifiedDate>
              <!-- 
                used to extract any urls in the document. It gets the urls
                and the scheme attribute (@scheme, which determines their type
                (type of url it is (thumbnail, website, server, contact, etc.
                urls)). We always choose the "server" url when we add to map.
              -->
              <References>
                <xsl:for-each select="./dct:references">
                  <xsl:value-of select="."/>
                  <xsl:text>&#x2714;</xsl:text>
                  <xsl:value-of select="@scheme"/>
                  <xsl:text>&#x2715;</xsl:text>
                </xsl:for-each>
              </References>
              <!-- 
                this extracts content type information. Can be used to
                highlight add to map button
              -->
              <Types>
                <xsl:for-each select="./dc:type">
                  <xsl:value-of select="."/>
                  <xsl:text>&#x2714;</xsl:text>
                  <xsl:value-of select="@scheme"/>
                  <xsl:text>&#x2715;</xsl:text>
                </xsl:for-each>
              </Types>
            </Record>
          </xsl:for-each>
        </Records>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```

When you have finished editing the text, save the file as 'GetRecords_Response.xslt'.

===Author the GetRecordByID_Response.xslt===
Remember that the CSW Clients apply the GetRecordByID_Response.xslt to a GetRecordsByID response from the catalog service only to extract the URL that is used to display a live service in the interface. The code snippet below shows a generic GetRecordsByID_Response.xslt. The important part of customizing this is to identify where in the CS-W response information about a live service would be found. This should be defined in the ''<xsl:template>'' section with select statements. The example shown below is for a Dublin Core catalog service, and we see that the xpath to the service information can be found at the ''csw:record/dct:references'' element. For examples that use ArcIMS CS-W connectors or ISO 19139-based connectors, open other GetRecordByID_Response xslt's within the <tt>\\CSWClients\Data</tt> folder and look for an example that is similar to the standard that your catalog service uses.

```
<?xml version="1.0" encoding="UTF-8"?> 
<xsl:stylesheet version="1.0"
 xmlns:xsl="http://www.w3.org/1999/XSL/Transform" 
 xmlns:csw="http://www.opengis.net/cat/csw/2.0.2"
 xmlns:dct="http://purl.org/dc/terms/"
 xmlns:ows="http://www.opengis.net/ows"
 exclude-result-prefixes="csw dct">
  <xsl:output method="text" indent="no" encoding="UTF-8"/> 
  <xsl:template match="/"> 
    <xsl:choose>
      <xsl:when test="/ows:ExceptionReport">
        <exception>
          <exceptionText>
            <xsl:for-each select="/ows:ExceptionReport/ows:Exception">
              <xsl:value-of select="ows:ExceptionText"/> 
            </xsl:for-each>
          </exceptionText>
        </exception>
      </xsl:when> 
      <xsl:otherwise>
        <xsl:apply-templates select="//csw:Record/dct:references"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template match="//csw:Record/dct:references"> 
    <xsl:value-of select="."/>
    <xsl:text>&#x2714;</xsl:text>
    <xsl:value-of select="@scheme"/>
    <xsl:text>&#x2715;</xsl:text>
  </xsl:template>
</xsl:stylesheet>
```

When you have finished editing the text, save the file as 'GetRecordByID_Response.xslt'.

===Add your profile to the CSWProfiles.xml file===
After you have authored the three xslt's, you need to reference them in the <tt>CSWProfiles.xml</tt> file.
*Decide on a prefix that will distinguish your xslt's from the others found in the <tt>\\CSWClients\Data</tt> directory. Change the names of your three xslt's to include this prefix. For example, <tt>GetRecords_Request.xslt</tt> becomes <tt>MyOrganizationName_GetRecords_Request.xslt</tt>.
*Navigate to the <tt>\\CSWClients\Data</tt> folder and open the <tt>CSWProfiles.xml</tt> file in a text editor.
*Copy the following text into an entry at the top of the file, just below the '''<CSWProfiles>''' tag. Update the commented sections to reference the name of your new profile and the three xslt's that you have authored.

```
 <?xml version="1.0" encoding="utf-8" ?> 
 <CSWProfiles>
   <!-- insert the name of your profile here within comments -->
   <Profile>
     <!-- 
       update the ID value with a unique urn value. Doesn't matter what it is 
       as long as it is unique within this file 
     -->
     <ID>urn:ogc:CSW:2.0.2:HTTP:YourProfile </ID> 
       <!-- 
         update the Name value with the name you want to display for your 
         profile in the Profile drop-down on the Register network resource 
         page in the geoportal.
       -->
       <Name> Unique name for display of Profile </Name>
       <!-- 
         update the CswNamespace value with the namespace for your CSW service. 
         For CSW 2.0.2 services, this will typically be 
         http://www.opengis.net/cat/csw/2.0.2. For earlier CSW services, this 
         may be http://www.opengis.net/cat/csw or another namespace 
       -->
       <CswNamespace> http://www.opengis.net/cat/csw/2.0.2</CswNamespace> 
       <Description />
       <GetRecords> 
         <XSLTransformations>
           <!-- 
             update with the name of your custom profile's GetRecords_Request.xslt file
           -->
           <Request>MyOrganizationName_GetRecords_Request.xslt</Request>
           <!-- 
             update with the name of your custom profile's GetRecords_Response.xslt file
           -->
           <Response>MyOrganizationName_GetRecords_Response.xslt</Response>
         </XSLTransformations>
       </GetRecords>
       <GetRecordByID>
         <RequestKVPs>
           <!-- 
             update with syntax appropriate for issuing a GetRecordById request
             to the type catalog service to which you want to connect. In this 
             example, the service would use CS-W 2.0.2 protocol and the full set
             of metadata record elements from each source record that should be 
             presented in the response (ElementSetName)
           -->
           <![CDATA[service=CSW&request=GetRecordById&version=2.0.2&ElementSetName=full]]>
         </RequestKVPs> 
         <XSLTransformations> 
           <!-- 
             update with the name of your custom profile's 
             GetRecordByID_Response.xslt file
           --> 
           <Response>MyOrganizationName_GetRecordByID_Response.xslt</Response>
         </XSLTransformations>
       </GetRecordByID>
       <!-- 
         update with True if the CS-W service supports returning queries defined by an extent
       -->
       <SupportSpatialQuery>True </SupportSpatialQuery> 
       <!-- 
         update with True if the CS-W service supports returning the content 
         type of a metadata record
       --> 
       <SupportContentTypeQuery> True </SupportContentTypeQuery> 
       <!-- 
         update with True if the CS-W service supports returning bounding
         box information
       -->
       <SupportSpatialBoundary> True </SupportSpatialBoundary>
     </Profile>
```

*By default, when a user clicks the '''View Metadata''' button, the metadata XML is returned from the catalog service's GetRecordByID response without any styling applied. If you want to define the styling on the metadata document so it is easier to read, you can use the '''<StyleResponse>''' tag instead of the '''<Response>''' tag within the '''<GetRecordByID>''' section for your profile in the <tt>CSWProfiles.xml</tt> file. Do the following:
**Author a stylesheet (.xsl) that will interpret and render the metadata returned from the GetRecordById response from the catalog service. Save the stylesheet in the same folder as your CSWProfiles.xml file.
**In the <tt>CSWProfiles.xml</tt> file, scroll to the section that defines the profile to which you want to apply the styling. Within that section, find the '''<GetRecordByID>''' element. Within the <GetRecordByID> element, find the '''<XSLTransformations>''' element.
**Replace the '''<Response>''' element section with '''<StyleResponse>'''.
**Within the '''<StyleResponse>''' open and closed tags, type the name of the stylesheet that you created to render the GetRecordByID response. An example is shown below:
```
<GetRecordByID>
  <RequestKVPs>
    <![CDATA[service=CSW&request=GetRecordById&version=2.0.2&ElementSetName=full&outputSchema=original]]>
  </RequestKVPs>
  <XSLTransformations> 
    <StyleResponse>Custom_Render.xsl</StyleResponse> 
  </XSLTransformations>
</GetRecordByID>
```

*Save the <tt>CSWProfiles.xml</tt> file. The next time you open the CSW Clients in ArcMap or ArcGIS Explorer, you should see your profile listed.