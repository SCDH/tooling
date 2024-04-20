# Tooling for Digital Editions

Every digital edition needs some tools: E.g., an XSLT processor or
XQuery implementation is required to transform TEI to HTML; an Relax
NG processor is required to validate the TEI documents against a
schema; some program may be required to convert embedded RDF to an
other serialization; or a build tool may be required to run
processes in an automated fashion.

This Tooling project provides a *simple*, *portable*, *extensible* and
*reproducible* means for providing software needed in the context of
an edition. Tooling is for both uses: on your personal machine and in
a CI/CD pipeline.

[Apache Maven](https://maven.apache.org/index.html) is used for
getting all tools and setting up wrapper scripts and the classpath for
using them.


## Tools

The following Tools and their dependencies are downloaded:

- Saxon HE: an XSLT and XQuery processor
- extensions to Saxon
  - an implementation of TEI XPointer schemes
  - ICU-XPath-Bindings for normalizing and transliterating Unicode
- Jing: a Relax NG validator
- schXslt: a Schematron validator
- XSpec: a unit testing framework for XSLT and XQuery
- Apache Ant and AntContrib: an incremental build tool with great
  support for XSLT and other X-technologies
- Apache Jena RIOT: an RDF converter

The Tooling can easily be extended, so that you can download further
tools or your own packetized XSLT libraries, etc.  It also helps with
packetizing your edition and XSLT library and deploying it as a Maven
artifact.

# Getting started

## Using Tooling

A project with Tooling let's users obtain the required tools in a one-liner:

```
mvn package
```

This requires a [Java installation](#requirements) and an installation
of Apache Maven. Even better: Maven is not required if there are a
`mvnw` (Linux/Mac), `mvnw.cmd` (Windows) files and a
`.mvn/wrapper/maven-wrapper.jar` file present. In this case, you can
just run the wrapper script to get tooling:

```
./mvnw package
```

After that, there are, e.g., scripts in `target/bin/`.


## Setting up Tooling

First, choose one of the archetypes provided:

- [`edition-tooling`](edition): generate a Maven `pom.xml` file in the base
  directory and all other required resources in a `tooling`
  directory.
  ```
  edition-base-folder
  ├── .mvn
  │   └── maven.config
  ├── pom.xml
  ├── tooling
  │   └── scripts
  │       ├── ant.sh
  │       ├── classpath.sh
  │       ├── query.sh
  │       ├── test.sh
  │       └── xslt.sh
  │
  ├── edition stuff
  ├── more stuff
  ```

- [`edition-tooling-flat`](edition-flat): same as `edition-tooling`, but all the maven
  stuff also goes into the tooling folder.
  ```
  edition-base-folder
  ├── tooling
  │   ├── .mvn
  │   │   └── maven.config
  │   ├── pom.xml
  │   └── scripts
  │       ├── ant.sh
  │       ├── classpath.sh
  │       ├── query.sh
  │       ├── test.sh
  │       └── xslt.sh
  │
  ├── edition stuff
  ├── more stuff
  ```

- [`processing-tooling`](processing): Bootstrap a Maven project for
  processing XML with a `pom.xml` in the root folder. All resources
  for testing, packetizing etc. will live in subfolders of the root
  folder.




Right now, you still have to clone this project and run `./mvnw
package install` before you can set up tooling; this will install the
archetypes, i.e., recipes for setting up a Tooling Maven
project. Note: This is a one-time process. After things are set up,
the tooling can be used on other machines, without ever knowing about
this project.

After you have installed the archetypes, just run the following
command to use them:

```
mvn archetype:generate -DarchetypeCatalog=local
```

This will start an interactive Maven session which first lets you
choose an archetype and then asks you for some parameters how to set
up the project. The parameters are explained on the archetypes'
folders (see links above).

```
Choose archetype:
1: local -> de.uni-ms.scdh.tei:edition-tooling (Tooling for TEI Editions in tooling folder)
2: local -> de.uni-ms.scdh.tei:processing-tooling (Tooling for TEI Processing in base folder)
3: local -> de.uni-ms.scdh.tei:edition-tooling-flat (Tooling for TEI Editions, Maven also in tooling folder)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): : 
...
```


### Maven wrapper

On top of Tooling, you can provide Maven wrapper to further facilitate
the usage of your project. Therefore change into the directory where
the `pom.xml` file is and run the following command:

```
mvn wrapper:wrapper
```

This will create wrapper scripts and the contents of the
`.mvn/wrapper` subfolder. Commit them to your git repository to use
Tooling without a Maven installation.


## Requirements

A Java Runtime Environment (JRE) has to be installed on the
system. Apache Maven is not required, when Maven wrapper is provided.

On CI/CD pipelines, we recommend to use the
[`eclipse-temurin`](https://hub.docker.com/_/eclipse-temurin) docker
image, e.g., `eclipse-temurin:17-alpine`.

On a Linux desktop, run `apt install openjdk-17-jre` to get Java.



## Version Numbers

You can write version numbers of your project directly into the
`pom.xml` file. However, that's not how it it set up. The `version`
element contains two Maven variables / properties:

	<version>${revision}${changelist}</version>
	
These properties are set in `.mvn/maven.config`. Why? Because that's
the way to make [git commit tags the single source of
truth](https://maven.apache.org/maven-ci-friendly.html) in a Maven
project. It allows you to say something like `mvn
-Drevision=$CI_COMMIT_TAG -Dchangelist="" deploy` in your pipeline.

If you do not want to use git as a single source of truth for version
numbers, feel free to replace the properties in the pom file with a
version number.


# Further Reading

- https://stackoverflow.com/questions/7223031/how-to-embed-archetype-project-version-in-maven-archetype
