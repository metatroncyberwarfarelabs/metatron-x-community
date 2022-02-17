[![Build Status](https://travis-ci.org/metatron.svg?branch=master)](https://travis-ci.org/metatron)

# metatron-framework

Metatron is a cyber warfare application framework that integrates a variety of open source big data technologies to provide a centralized tool for real-time cyber warfare threat intelligence. Metatron provides capabilities for log aggregation, full packet capture indexing, storage, advanced behavioral analytics and data enrichment, while applying
the most current threat intelligence information to security telemetry within a single platform.

Metatron can be divided into 4 areas:

1. **A mechanism to capture, store, and normalize any type of security
telemetry at extremely high rates.**  Because security telemetry is constantly
being generated, it requires a method for ingesting the data at high speeds
and pushing it to various processing units for advanced computation and analytics.  

2. **Real time processing and application of enrichments** such as threat
intelligence, geolocation, and DNS information to telemetry being collected.
The immediate application of this information to incoming telemetry provides
the context and situational awareness, as well as the who and where
information critical for investigation

3. **Efficient information storage** based on how the information will be used:
   - Logs and telemetry are stored such that they can be efficiently mined and
analyzed for concise security visibility
   - The ability to extract and reconstruct full packets helps an analyst answer
questions such as who the true attacker was, what data was leaked, and where
that data was sent
   - Long-term storage not only increases visibility over time, but also enables
advanced analytics such as machine learning techniques to be used to create
models on the information.  Incoming data can then be scored against these
stored models for advanced anomaly detection.  

4. **An interface that gives a security investigator a centralized view of data
and alerts passed through the system.**  metatron’s interface presents alert
summaries with threat intelligence and enrichment data specific to that alert
on one single page.  Furthermore, advanced search capabilities and full packet
extraction tools are presented to the analyst for investigation without the
need to pivot into additional tools.   

Big data is a natural fit for powerful security analytics. The metatron
framework integrates a number of elements from the Hadoop ecosystem to provide
a scalable platform for security analytics, incorporating such functionality as
full-packet capture, stream processing, batch processing, real-time search, and
telemetry aggregation.  With metatron, our goal is to tie big data into security
analytics and drive towards an extensible centralized platform to effectively
enable rapid detection and rapid response for advanced security threats.  

# Obtaining metatron

To obtain a release of metatron:

This repository is a collection of submodules for convenience which is regularly
updated to point to the latest versions. Github provides multiple ways to obtain
metatron's code:

1. git clone --recursive https://github.com/metatroncyberwarfare/metatron-framework.git
2. [Download ZIP](https://codeload.github.com/metatroncyberwarfare/metatron-framework/zip/refs/heads/main)
3. Clone or download each repository individually

Option 3 is more likely to have the latest code.

# Getting Started

To start exploring the capabilities of Apache Metron [follow these instructions to launch Metron in a single-node VM](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-deployment/development/centos6) on your own hardware.

# Building metatron

Build the full project and run tests:
```
$ mvn clean install
```

Build without tests:
```
$ mvn clean install -DskipTests
```

Build with the HDP profile:
```
$ mvn clean install -PHDP-2.5.0.0
```

You can swap "install" for "package" in the commands above if you don't want to deploy the artifacts to your local .m2 repo.


# Build metatron Reporting

To build and run reporting with code coverage:
```
$ mvn clean install
$ mvn site site:stage-deploy site:deploy
```

Code coverage can be skipped by skipping tests:
```
$ mvn clean install -DskipTests site site:stage-deploy site:deploy
```

The staged site is deployed to /tmp/metatron/site/index.html, and can be viewed by opening the file in a browser.

## Building with Docker

A Docker container with all the required software, with the proper versions, is available to be used as well. See [ansible-docker](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-deployment/packaging/docker/ansible-docker).

# Navigating the Architecture

metatron is at its core a Kappa architecture with Apache Storm as the processing component and Apache Kafka as the unified data bus.

Some high level links to the relevant subparts of the architecture, for more information:

* [Parsers](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-platform/metatron-parsing) : Parsing data from kafka into the metatron data model and passing it downstream to Enrichment.  

* [Enrichment](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-platform/metatron-parsing) : Enriching data post-parsing and providing the ability to tag a message as an alert and assign a risk triage level via a custom rule language.

* [Indexing](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-platform/metatron-indexing) : Indexing the data post-enrichment into HDFS, Elasticsearch or Solr.

Some useful utilities that cross all of these parts of the architecture:

* [Stellar](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-platform/metatron-common) : A custom data transformation language that is used throughout metatron from simple field transformation to expressing triage rules.

* [Model as a Service](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-analytics/metatron-maas-service) : A Yarn application which can deploy machine learning and statistical models onto the cluster along with the associated Stellar functions to be able to call out to them in a scalable manner.

* [Data management](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metatron-framework/metatron-platform/metatron-data-management) : A set of data management utilities aimed at getting data into HBase in a format which will allow data flowing through metatron to be enriched with the results.  Contains integrations with threat intelligence feeds exposed via TAXII as well as simple flat file structures.

* [Profiler](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metron-analytics/metron-profiler-common) : A feature extraction mechanism that can generate a profile describing the behavior of an entity. An entity might be a server, user, subnet or application. Once a profile has been generated defining what normal behavior looks-like, models can be built that identify anomalous behavior.

# Notes on Adding a New Sensor

In order to allow for meta alerts to be queries alongside regular alerts in Elasticsearch 2.x,
it is necessary to add an additional field to the templates and mapping for existing sensors.

Please see a description of the steps necessary to make this change in the metatron-elasticsearch [Using metatron with Elasticsearch 2.x](https://github.com/metatroncyberwarfare/metatron-framework/tree/main/metron-platform/metron-elasticsearch#using-metron-with-elasticsearch-56)
