# Tooling for Digital Editions

Every digital edition needs some tools: E.g., an XSLT processor or
XQuery implementation is required to transform TEI to HTML; an Relax
NG processor is required to validate the TEI documents against a
schema; some program may be required to convert embedded RDF to an
other serialization; or a build tool may be required to run
processes in an automated fashion.

This Tooling project provides a *simple* and *extensible* means for
getting common software needed in the context of an edition in a
*reproducible* way. Tooling is for both uses: on your personal machine
and in a CI/CD pipeline.

Instead of providing shell scripts for downloading tools, this project
provides tooling in a portable way, so that the tools can be used on
any operating system. Therefore [Apache
Maven](https://maven.apache.org/index.html) is used to get all tools.
Scripts (shell and batch) are provided to offer convenient use of the
tools.


## Tools

The following Tools and their dependencies are downloaded:

- Saxon HE: an XSLT and XQuery processor
- extensions to Saxon
  - an implementation of TEI XPointer schemes
  - ICU-XPath-Bindings for normalizing and transliterating Unicode
- Jing: a Relax NG validatorfast and versatile 
- schXslt: a Schematron processor
- XSpec: a unit testing framework for XSLT and XQuery
- Apache Ant and AntContrib: an incremental build tool with great
  support for XSLT and other X-technologies
- Apache Jena RIOT: an RDF converter

The Tooling can easily be extended, so that you can download further
tools or your own packetized XSLT libraries, etc.  It also helps with
packetizing your edition and XSLT library and deploying it as a Maven
artifact.

# Getting started




## Requirements

A Java Runtime Environment (JRE) has to be installed on the
system. Apache Maven is not required, since we use Maven wrapper.

On CI/CD pipelines, we recommend to use the
[`eclipse-temurin`](https://hub.docker.com/_/eclipse-temurin) docker
image, e.g., `eclipse-temurin:17-alpine`.

On a Linux desktop, run `apt install openjdk-17-jre` to get Java.


