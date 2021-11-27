Geoportal Server Catalog includes various java libraries as dependencies. These themselves may pull in further third-party dependencies. Each one of these external libraries is a potential source of vulnerabilities that may be abused to compromise the geoportal, the elastic index, or even the systems hosting the geoportal.

There are external parties actively testing many java libraries for vulnerabilities. This leads to frequent updates in the libraries, which then need to make their way into geoportal. This is controlled in the maven pom.xml file. Below is a basic process to keep Geoportal Server Catalog up to date.

- run `mvn dependency:tree'.
  - the output of this can be captured in a text file that lists the dependencies explicitly mentioned in the pom.xml file, but also the dependencies of those dependencies, with versions.
- run `mvn clean` to make sure current libraries in the target are removed
- run `mvn install` to compile geoportal and build a .war file
- run a code scan on the result
  - in our internal environment we can run a code scan (such as Protecode or Veracode) as part of the build process
  - the result of the scan is a list of the third-party libraries with known vulnerabilities
  - the result typically also includes a suggested version of the libraries without these vulnerabilities
- update pom.xml to include the recommended version of the library.
  - on the Maven Repository site, look for the library (https://mvnrepository.com/)
  - find the release recommended by the code scanner
  - copy the pom snippet, for example for spring-core 5.3.13 (https://mvnrepository.com/artifact/org.springframework/spring-core/5.3.13):

```
<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.13</version>
</dependency>
```
  - paste this in the pom.xml in the `dependencies` element
- if the library was a dependency of another library mentioned in the pom, you may need to exclude the library from being included as part of that pom (unless you also update that library). to do this, in the library that references the specific library to update, include an `exclusions` element with an `exclusion` element for each of its dependencies that should not be included automatically, but are included explicitly in the pom.xml. For example this will exclude commons-logging from being included as part of including spring-core:

```
    <!-- Spring -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${springframework.version}</version>
      <exclusions>
        <exclusion>
          <groupId>commons-logging</groupId>
          <artifactId>commons-logging</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
```

- repeat these steps for every library you want to update specifically

Throughout the process, make sure you test geoportal, look for compiler warnings or errors, etc. to make sure the process doesn't break geoportal server catalog.

Note: libraries do deprecate operations now and then. This may mean an update to a library can only be made if you also update the geoportal code.

Hack: if you do this manually, instead of 'mvn clean` you can also delete the `WEB-INF\lib` folder from the geoportal target. This will force maven to download updated versions of the libraries.




