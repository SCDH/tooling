# Edition Archetype

Watch out for `<<<====` and the description of properties:

```
mvn archetype:generate -DarchetypeCatalog=local
...
Choose archetype:
1: local -> de.uni-ms.scdh.tei:edition-tooling (Tooling for TEI Editions in tooling folder)
2: local -> de.uni-ms.scdh.tei:processing-tooling (Tooling for TEI Processing in base folder)
3: local -> de.uni-ms.scdh.tei:edition-tooling-flat (Tooling for TEI Editions, Maven also in tooling folder)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): : 1
[INFO] Using property: tooling-dir = tooling                                 <<<==== fixed value
[INFO] Using property: git-instance = zivgitlab.uni-muenster.de              <<<==== The git instance, domain part of the URL
Define value for property 'git-repo-path': /SCDH/herbert-saeculum/edition    <<<==== The git instance, domain part of the URL
Define value for property 'distribution-url' https://${git.instance}/api/v4/projects/\${env.CI_PROJECT_ID}/packages/maven: :    <<<==== The git instance, domain part of the URL
Define value for property 'groupId': de.uni-ms.herbert                       <<<==== The git instance, domain part of the URL
Define value for property 'artifactId': saeculum-edition                     <<<==== The identifier of the project. Type something reasonable here!
Define value for property 'version' 1.0-SNAPSHOT: :                          <<<==== this version information is not used. See README
Define value for property 'package' de.uni-ms.herbert: :                     <<<==== not used
Confirm properties configuration:
tooling-dir: tooling
git-instance: zivgitlab.uni-muenster.de
git-repo-path: /SCDH/herbert-saeculum/edition
distribution-url: https://${git.instance}/api/v4/projects/\${env.CI_PROJECT_ID}/packages/maven
groupId: de.uni-ms.herbert
artifactId: saeculum-edition
version: 1.0-SNAPSHOT
package: de.uni-ms.herbert
 Y: : 
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: edition-tooling:0.1.0-SNAPSHOT
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: de.uni-ms.herbert
[INFO] Parameter: artifactId, Value: saeculum-edition
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: de.uni-ms.herbert
[INFO] Parameter: packageInPathFormat, Value: de/uni-ms/herbert
[INFO] Parameter: tooling-dir, Value: tooling
[INFO] Parameter: package, Value: de.uni-ms.herbert
[INFO] Parameter: git-repo-path, Value: /SCDH/herbert-saeculum/edition
[INFO] Parameter: groupId, Value: de.uni-ms.herbert
[INFO] Parameter: artifactId, Value: saeculum-edition
[INFO] Parameter: git-instance, Value: zivgitlab.uni-muenster.de
[INFO] Parameter: distribution-url, Value: https://${git.instance}/api/v4/projects/\${env.CI_PROJECT_ID}/packages/maven
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[WARNING] CP Don't override file /tmp/saeculum-edition/tooling/scripts
[WARNING] CP Don't override file /tmp/saeculum-edition/.mvn
[INFO] Project created from Archetype in dir: /tmp/saeculum-edition
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  02:18 min
[INFO] Finished at: 2024-04-20T21:37:38+02:00
[INFO] ------------------------------------------------------------------------
```

This will result in a new folder `saeculum-edition` (from the
`artifactId` property) with the following contents:

```
saeculum-edition
├── .mvn
│   └── maven.config
├── pom.xml
└── tooling
    └── scripts
        ├── ant.sh
        ├── classpath.sh
        ├── query.sh
        ├── test.sh
        └── xslt.sh
```



## Adding Tooling to an Existing Project

Running the archetype will create a new folder with all Tooling stuff
in it. You can simple copy the contents (including hidden files) to
your edition. You can also try the `-Dbasedir=PATH` command line
option.
