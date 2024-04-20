# Processing Archetype

Watch out for `<<<====` and the description of parameters!

```
mvn archetype:generate -DarchetypeCatalog=local
...
Choose archetype:
1: local -> de.uni-ms.scdh.tei:edition-tooling (Tooling for TEI Editions in tooling folder)
2: local -> de.uni-ms.scdh.tei:processing-tooling (Tooling for TEI Processing in base folder)
3: local -> de.uni-ms.scdh.tei:edition-tooling-flat (Tooling for TEI Editions, Maven also in tooling folder)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): : 2
Define value for property 'credits' \${env.GITLAB_USER_NAME}: : Joe               <<<==== An author name used in meta data for distributions, e.g., for the oxygen plugin
[INFO] Using property: git-instance = zivgitlab.uni-muenster.de                   <<<==== The git instance, domain part of the URL
Define value for property 'git-repo-path': /SCDH/hees-alea/alea-transformations   <<<==== The path of the repository in the git instance
Define value for property 'distribution-url' https://${git.instance}/api/v4/projects/\${env.CI_PROJECT_ID}/packages/maven: :   <<<==== a reasonable generic default for gitlab instances
Define value for property 'groupId': de.uni-ms.alea                               <<<==== The Jave/Maven groupId of the project. Type something reasonable here!
Define value for property 'artifactId': alea-transformations                      <<<==== The identifier of the project. Type something reasonable here!
Define value for property 'version' 1.0-SNAPSHOT: :                               <<<==== this version information is not used. See below
Define value for property 'package' de.uni-ms.alea: :                             <<<==== not used
Confirm properties configuration:
credits: Joe
git-instance: zivgitlab.uni-muenster.de
git-repo-path: /SCDH/hees-alea/alea-transformations
distribution-url: https://${git.instance}/api/v4/projects/\${env.CI_PROJECT_ID}/packages/maven
groupId: de.uni-ms.alea
artifactId: alea-transformations
version: 1.0-SNAPSHOT
package: de.uni-ms.alea
 Y: :
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: processing-tooling:0.1.0-SNAPSHOT
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: de.uni-ms.alea
[INFO] Parameter: artifactId, Value: alea-transformations
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: de.uni-ms.alea
[INFO] Parameter: packageInPathFormat, Value: de/uni-ms/alea
[INFO] Parameter: package, Value: de.uni-ms.alea
[INFO] Parameter: credits, Value: Joe
[INFO] Parameter: git-repo-path, Value: /SCDH/hees-alea/alea-transformations
[INFO] Parameter: groupId, Value: de.uni-ms.alea
[INFO] Parameter: artifactId, Value: alea-transformations
[INFO] Parameter: git-instance, Value: zivgitlab.uni-muenster.de
[INFO] Parameter: distribution-url, Value: https://${git.instance}/api/v4/projects/\${env.CI_PROJECT_ID}/packages/maven
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[WARNING] CP Don't override file /tmp/alea-transformations/scripts
[WARNING] CP Don't override file /tmp/alea-transformations/.mvn
[WARNING] CP Don't override file /tmp/alea-transformations/distribution
[WARNING] CP Don't override file /tmp/alea-transformations/utils
[INFO] Project created from Archetype in dir: /tmp/alea-transformations
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  11:38 min
[INFO] Finished at: 2024-04-20T19:45:16+02:00
[INFO] ------------------------------------------------------------------------
```



## Folders

```
project-base
├── distribution
│   ├── oxygen
│   │   ├── assembly.xml
│   │   ├── descriptor.xml
│   │   └── plugin.xml
│   ├── seed
│   │   ├── assembly.xml
│   │   ├── hardened-saxon-config.xml
│   │   ├── package.json
│   │   ├── seed-config.xsl
│   │   └── sef-config.xsl
│   └── zip
│       └── assembly.xml
├── .mvn
│   └── maven.config
├── pom.xml
├── saxon-local.xml
├── scripts
│   ├── ant.sh
│   ├── classpath.sh
│   ├── query.sh
│   ├── test.sh
│   │   └── xslt.sh
│   └── utils
│       ├── saxon-config-edition.xsl
│       ├── saxon-config.xsl
│       └── saxon-dependencies.xsl
├── your-stuff
```

- `scripts`: templates for shell and batch scripts. After running
  `./mvnw package`, there will be executable derivations in
  `target/bin/`.
- `distribution`: contains definitions for assembling the project as
  different package types:
  - a simple zip package that contains XSLT, XQuery etc.
  - a package with resources that can be deployed on the SEED XML Transformer
  - an &lt;oXygen/&gt; plugin that brings your transformations to the
    &lt;oXygen/&gt; XML editor.
- `utils`: utilities that help building the package
- `.mvn`: a special folder with Maven-specific resources.
- `target`: created by running `./mvnw package`; contains build
  artifacts and downloaded dependencies


## Saxon Configuration

The archetype creates an empty [Saxon configuration]() in
`saxon-local.xml`. That's the place to put in your configuration.

Running `mvn package` will also generate `saxon.xml` and
`saxon.he.xml`, `saxon.pe.xml`, `saxon.ee.xml` for the different
editions of Saxon. These generated configurations will contain the
config from `saxon-local.xml` merged with the saxon configurations
under `target/dependencies/**/saxon.xml`. The merging is done by the
XSLT in the `utils` folder.


## Dependencies

If your XSLT project depends on an other XSLT project, the best
practise is to let Maven to the downloading and unpacking
steps. Therefore add another `<execution>` element to the the
`maven-dependency-plugin` declarations in the pom file. E.g.:

```xml
                    <execution>
                        <!-- get and unpack SEED TEI Transformations -->
                        <id>unpack-seed-tei-transformations</id>
                        <phase>generate-resources</phase>
                        <goals>
                            <goal>unpack</goal>
                        </goals>
                        <configuration>
                            <!-- unpacking dependencies to target/dependencies will enable merging of saxon configs -->
                            <outputDirectory>${project.build.directory}/dependencies</outputDirectory>
                            <artifactItems>
                                <artifactItem>
                                    <groupId>de.wwu.scdh.tei</groupId>
                                    <artifactId>seed-tei-transformations</artifactId>
                                    <version>0.5.0</version>
                                    <classifier>package</classifier>
                                    <type>zip</type>
                                </artifactItem>
                            </artifactItems>
                        </configuration>
                    </execution>
```

You may also want to tell Maven which Maven package registry
(repository) to use. Add it to the `<repositories>` section (XPath:
`/project/repositories`):

```xml
        <repository>
            <id>gitlab-seed-tei-transformations</id>
            <url>https://zivgitlab.uni-muenster.de/api/v4/projects/5496/packages/maven</url>
        </repository>
```


## Distributions

Running `mvn package` will packetize your project in differenz flavours:

- a zip package
- an oXygen plugin
- a package that can be deployed on the SEED XML Transformer

These packages will be located in the `target` folder.

You can use `mvn deploy` to push them to a Maven registry.

The assembly descriptors for the different build artifacts are located
in `distribution/*/assembly.xml`. These descriptors come with
reasonable defaults and collect XSLT and XQuery from any subfolder
except the ones the belong to Tooling.


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
