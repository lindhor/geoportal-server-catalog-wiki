Metadata profiles come in different flavors depending on the base specification they are based on, the industry, the purpose of metadata (documentation versus discovery), time, and more.

For example: ISO has several metadata standards available. While some of the older standards (like ISO 19139 XML encoding) may technically be superseded by a new standard (ISO 19115-3 in this case), there are many profiles still based on those previous standards. Understanding that base is critical in designing a custom metadata profile.

Once you understand the base, it needs to be defined how the profile is intending to deviate from that base. For metadata this may include:
- additional metadata sections, elements, or attributes capturing additional information in metadata
- changes in validation of existing elements, making optional elements mandatory or vice versa
- changes in domain lists by adding additional values or leaving out unused values
- setting default values or suggested values to certain elements or attributes

Most metadata standards nowadays are defined using XML schema. Changing the profile in an office document alone is not sufficient. When applying any of the above changes, those need to be reflected in the XML schema used to validate an instance document against the metadata profile.

Once all these modifications to the chosen base standard are known, make sure to create sample XML documents. These are not only useful for discussions with the end users, but may also be used when validating the custom metadata profile in Geoportal Server.