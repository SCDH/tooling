# Edition flat Archetype

## Properties

The archetype properties are the same as in the [Edition Archetype](../edition). See there.

## Setup

The archetype should be run from the root folder of an edition. After
the generation is complete, the new folder named after the
`artifactId` property should be renamed to `tooling` (or what ever you
like).

Example:

1. Edition given in base folder `herbert-saeculum`.
1. CD into it and run `mvn archetype:generate
   -DarchetypeCatalog=local`.
1. Result looks like:
   ```
   saeculum-edition/
   ├── EDITION-STUFF
   └── saeculum-edition
       ├── .mvn
       │   └── maven.config
       ├── pom.xml
       └── scripts
           ├── ant.sh
           ├── classpath.sh
           ├── query.sh
           ├── test.sh
           └── xslt.sh
   ```
1. Rename the nested `saeculum-edition` folder to `tooling`, so:
   ```
   saeculum-edition/
   ├── EDITION-STUFF
   └── tooling
       ├── .mvn
       │   └── maven.config
       ├── pom.xml
       └── scripts
           ├── ant.sh
           ├── classpath.sh
           ├── query.sh
           ├── test.sh
           └── xslt.sh
   ```
