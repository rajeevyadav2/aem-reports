Report on Components in AEM
========

[![Build Status](https://travis-ci.org/isobaraustralia/aem-reports-components.svg?branch=master)](https://travis-ci.org/isobaraustralia/aem-reports-components)
[![Download](https://api.bintray.com/packages/isobaraustralia/maven/aem-reports-components/images/download.svg) ](https://bintray.com/isobaraustralia/maven/aem-reports-components/_latestVersion)


This package adds custom report and report components that enable listing of component in AEM.

#### New Report List

* Component Group List Report - show grouping of reports by category for tracking
* Component List Report - show list of all reports for tracking
* Component List - show list of all components for export


#### New Report Template
* Component List Report Template - use this report generator to create a new Component List reports


#### New Report Components
* Component Group - component category group
* Description - component description
* Title - component title
* Has Classic Dialog - does component have classic dialog
* Has Classic Dialog Design - does component have classic design dialog
* Has Dialog - does component have touchui dialog
* Has Embedded ClientLibs - does component have local clientlibs
* Has Icon - does component have an icon
* Last Modified - last time component was modified
* Resource Type - type of component
* Sling Resource Super Type - which component is being inherited
* Sling Resource Type - which service being triggered for this component
* Allowed Parents - paths where component is allowed to be used
* Component Path - component location

*By Max Barrass @ Isobar Australia*

#### Maven Archetype

This a content package project generated using the multimodule-content-package-archetype.

```bash
mvn archetype:generate \
-DinteractiveMode=false \
-DarchetypeRepository=http://repo.adobe.com/nexus/content/groups/public/ \
-DarchetypeGroupId=com.day.jcr.vault \
-DarchetypeArtifactId=multimodule-content-package-archetype \
-DarchetypeVersion=1.0.2 \
-DgroupId=isobar-aem \
-DartifactId=aem-reports-components \
-Dversion=1.0-SNAPSHOT \
-Dpackage=aem-reports-components \
-DappsFolderName=aem-reports-components \
-DartifactName="Report on Components in AEM" \
-DcqVersion="6.2.0" \
-DpackageGroup="isobar/aem/reporting"
```

#### Building

This project uses Maven for building. Common commands:

From the root directory, run: 
```bash 
mvn -PautoInstallPackage clean install
``` 
to build the bundle and content package and install to a CQ instance.

#### Specifying CRX Host/Port

The CRX host and port can be specified on the command line with:
```bash
mvn -Dcrx.host=otherhost -Dcrx.port=5502 clean install
```

Release
--------------

To be able to do releases for thi project you will require permissions to 
- Git Hub (source code), and
- Bintray (artifacts)

To deploy artifacts to Bintray you will need to:
- setup a Master Key for you Maven in ~/.m2/settings-security.xml, and
```bash
echo "<settingsSecurity><master>$(read -s -p 'Input master password:' MPASS ; mvn -q --encrypt-master-password "$MPASS")</master></settingsSecurity>">~/.m2/settings-security.xml
```

- create an encrypted Bintray password using your API key from Bintray in ~/.m2/settings.xml
```bash
export BINTRAY_USER="~~~YOUR~~USERNAME~~~"; \
export BINTRAY_APIKEY="~~~YOUR~~APIKEY~~~"; \
export BINTRAY_MAVEN_CONFIG_SEARCH="<server><id>bintray-isobaraustralia-maven<\/id><username>$BINTRAY_USER<\/username><password>.*<\/password><\/server>"; \
export BINTRAY_MAVEN_CONFIG="<server><id>bintray-isobaraustralia-maven<\/id><username>$BINTRAY_USER<\/username><password>API Key: $(mvn -q --encrypt-password ${BINTRAY_APIKEY})<\/password><\/server>"; \
(grep -q "$BINTRAY_MAVEN_CONFIG_SEARCH" ~/.m2/settings.xml || (echo "Inserting..."; sed -i "/<\/servers>/i $BINTRAY_MAVEN_CONFIG" ~/.m2/settings.xml;)); \
(grep -q "$BINTRAY_MAVEN_CONFIG" ~/.m2/settings.xml || (echo "Updating..."; sed -i "s|${BINTRAY_MAVEN_CONFIG_SEARCH}|${BINTRAY_MAVEN_CONFIG}|g" ~/.m2/settings.xml;))
```
NOTE: New entry for server will be added before </servers> tag in settings file.


#### Do a Dry Run
Since the Release Plugin performs a number of operations that change the project, it may be wise to do a dry run before a big release or on a new project.

To do this, commit all of your files as if you were about to run a full release and run:
```bash
mvn release:prepare -DdryRun=true
```
This will ask all the same questions, run the same tests, and output a copy of how the POMs will look after transformation. 

You can check the output and review the POMs, then run:


```bash
mvn release:clean
```

This will remove all of the files created above, and the project will be ready to execute the proper release.


Run in Batch Mode
--------------
Sometimes it is desirable to do the commit/tag process on a regular interval (e.g. to produce nightly or integration builds through a build server).

To use the default inputs for the versions and tag information and not prompt for any values, use Maven's --batch-mode setting:
```bash
mvn --batch-mode -DautoVersionSubmodules=true release:clean release:prepare release:perform 
```

To set a specific version:
```bash
export majorVersion=1; \
export minorVersion=0; \
export incrementalVersion=0; \
mvn build-helper:parse-version versions:set -DnewVersion=${majorVersion}.${minorVersion}.${incrementalVersion} versions:commit deploy scm:tag
```
Source: https://blog.codecentric.de/en/2015/01/continuous-delivery-microservices-jenkins-job-dsl-plugin/
