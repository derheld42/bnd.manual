---
layout: default
class: Processor
title: -pom BOOLEAN | PROPERTIES
summary: Generate a maven pom in the JAR
---

The `-pom` instruction can generate a pom derived from the manifest and store it in the bundle. The default will be splitting up the Bundle Symbolic name in a `groupid` (everything until the last '.') and a `artifactid` (everything from the last '.' to the end. However, it is also specify overrides in properties. The following properties are supported:

|Key              |Default          |Description                         |
|`groupid`        |bsn prefix       |The group id used                   |
|`artifactid`     |bsn suffix       |The artifact id to use. If `groupid` is set, the default is the Bundle Symbolic Name|
|`version`        |bundle version   |The version to use.                 |
|`where`          |`pom.xml`        |The location of the pom.xml file.   |

The pom will also attempt to convert the following headers to their POM counterpart:

* `Bundle-Description`
* `Bundle-DocUrl`
* `Bundle-Vendor`  – If the  value ends with a HTTP or HTTPS url then this URL is used as the organization URL and the name with be the part without the URL. Otherwise the whole value is used as the value for the organization name.
* `Bundle-License`
* `Bundle-Developer` – This is an unofficial header. The key must be the email. It consist of the following parameters:

  	email                      Email address mandatory
  	id                         A developer id (default address)
	name                       Name of the developer
	organization               Name of the organization
	organizationUrl            URL of the organization
	roles                      Roles of the developer (comma separated)
	timezone                   Three letter time zone
	
	Bundle-Developer: Peter.Kriens@aQute.biz; \
	    name="Peter Kriens"; \
	    organization=aQute; \
	    roles="programmer,gopher"
	 
* `Bundle-SCM` – This is an unofficial header. The key must be the It consists of the following parameters:

    connection                 Read only connection
    developerConnection        Developer connection
    url                        The URL for a web front end to your SCM system.     

	Bundle-SCM: url=http://github.com/bndtools, \
	    connection=scm:git:https://github.com/bndtools/bnd, \
	    developerConnection=scm:git:git@github.com/bndtools/bnd
    
## Example

The following example bnd file:

	Bundle-SymbolicName: com.example.foo
	Bundle-Version: 1.2.3.qualifier
	-pom: true

Generates the following pom in `pom.xml`:

	<project 	xmlns="http://maven.apache.org/POM/4.0.0" 
			xmlns:xsi="" 
			xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  		<modelVersion>4.0.0</modelVersion>
  		<groupId>com.example</groupId>
  		<artifactId>foo</artifactId>
  		<version>1.2.3.qualifier</version>
  		<name>com.example.foo</name>
	</project>

You can override the different parts of the Maven coordinates:

	Bundle-SymbolicName: com.example.foo
	Bundle-Version: 1.2.3.qualifier
	-pom: groupid=com.example,where=META-INF/maven/pom.xml,version=${version;==;${Bundle-Version}}
	
Generates the following pom in `META-INF/maven/pom.xml`:

	<project 	xmlns="http://maven.apache.org/POM/4.0.0" 
			xmlns:xsi="" 
			xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  		<modelVersion>4.0.0</modelVersion>
  		<groupId>com.example</groupId>
  		<artifactId>com.example.foo</artifactId>
  		<version>1.2</version>
  		<name>com.example.foo</name>
	</project>

