Redpill Linpro Alfresco Base
=============

This module acts as an overlay over the Alfresco SDK. It is 100% compatible with the Alfresco SDK which means switching to it which means that activating the overlay is a simple matter of swithing the SDK parent in your project to this artifact.

Orignal definition in your project pom.xml:

    <parent>
        <groupId>org.alfresco.maven</groupId>
        <artifactId>alfresco-parent-sdk</artifactId>
        <version>2.2.0</version>
    </parent>

Parent definition for a project based on this overlay:

    <parent>
        <groupId>com.redpill-linpro.alfresco</groupId>
        <artifactId>alfresco-base</artifactId>
        <version>2.2.0</version>
    </parent>


# Features
So what value does this module overlay provide?


* Integration tests support
* Functional tests support (rest service) 
* Metrics using Jacoco
* CMIS upload of artifacts
* Local development deployments to a standalone tomcat

## Integration tests support
Integration tests bootstraps a whole Alfresco environment which allows for testing using the internal alfresco services.

To use the integration test features the alfresco-test project should be included as dependencies in your amp project. See https://github.com/Redpill-Linpro/alfresco-test for more information.

Integration tests are triggered using `mvn clean verify -Pit`. Optionally also clean the db and data store `mvn clean verify -Pit,purge`. Also if you want to run a specific test you can use `mvn clean verify -Pit,purge -Dit.test=org.redpill.alfresco.demo.DemoComponentIntegrationTest`. 

Note that all integration tests must be suffixed with "IntegrationTest" to function.

## Functional tests support
Functional tests launches an embedded tomcat with your deplyed amp customizations. This allows for testing webscripts and other APIs.

To use the functional test features the alfresco-test project should be included as dependencies in your amp project. See https://github.com/Redpill-Linpro/alfresco-test for more information.

Functional tests are triggered using `mvn clean verify -Pft`. Optionally also clean the db and data store `mvn clean verify -Pft,purge`. Also if you want to run a specific test you can use `mvn clean verify -Pft,purge -Dft.test=org.redpill.alfresco.demo.DemoComponentFunctionalTest`. 

Note that all functional tests must be suffixed with "FunctionalTest" to function.

## Metrics using Jacoco
To trigger collection of Jacoco metrics use the `jenkins` profile in coordination with functional or integration tests. These metrics can then be used in Jenkins using the Jacoco plugin or in other analytic tools. 

Example:
`mvn clean verify -Pit,ft,purge,jenkins`

## CMIS upload of artifacts
You can also upload built artifacts to a CMIS repository such as Alfresco. Configure a profile in your local maven settings file (~/.m2/settings.xml). 

	<profile>
      <id>alfresco-dep</id>
      <properties>
        <artifact-upload.username>username</artifact-upload.username>
        <artifact-upload.password>password</artifact-upload.password>
        <artifact-upload.client.upload.path>Folder/CI</artifact-upload.client.upload.path>
        <artifact-upload.cmis.url>http://alfresco.example.com/alfresco/service/cmis</artifact-upload.cmis.url>
        <artifact-upload.site>sitename</artifact-upload.site>
      </properties>
    </profile>
    
Use `mvn clean install -Palfresco-dep,upload` to upload all built artifacts to Alfresco.

## Local development deployments to a standalone tomcat
You can also deploy your maven project to your local tomcat installation. Use the following maven command:
`mvn clean package -Pdev,alfresco-dev1`

This will look up settings of your local alfresco environment among your profiles. It is best practice to keep these profiles in your local maven settings file (~/.m2/settings.xml).

	<profile>
      <id>alfresco-dev1</id>
      <properties>
        <tomcat.repo.home>/opt/alfresco/tomcat-repo</tomcat.repo.home>
        <tomcat.share.home>/opt/alfresco/tomcat-share</tomcat.share.home>
        <tomcat.repo.port>8080</tomcat.repo.port>
        <tomcat.share.port>8081</tomcat.share.port>
      </properties>
    </profile>

If you're using version 2.2.4 or later, remember to make sure you have the
following line in your pom.xml:
   <app.amp.client.war.artifactId>share</app.amp.client.war.artifactId>
