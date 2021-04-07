# Kafka Backup and Restore

Kafka Backup is a tool to back up and restore your Kafka data including consumer group offsets. Kafka Backup is the only viable solution to take a cold backup of Kafka data and restore
it correctly.

It is designed as two connectors for Kafka Connect: 

* Sink connector (backing data and consumers group offset) 
* Source connector (restoring data and consumers group offset) 

## Features

* Backup and restore topic data
* Backup and restore consumer-group offsets
* Supports only backup/restore to/from S3

# Prerequisites

The following are required to run the Kafka Connect

* Java 1.8
* [Apache Maven](https://maven.apache.org/install.html)
* [kafkacat](https://github.com/edenhill/kafkacat)
* Folders ~/kafka , ~/kafka/plugins and /ect/kafka : Create these folders and provide full permission 
* [AWS Account Setup](https://bugcrowd.atlassian.net/wiki/spaces/DEV/pages/80445478/AWS+your+Account+Setup):- Make sure S3 bucket has access.

# Getting Started

* Download [from GitHub](https://github.com/bugcrowd/kafka-connect-connectors) and unzip it.
* At the root directory of Kafka Backup, Just rrun the followingcommand: `mvn clean compile package -DskipTests=true`.
  You will find `target/kafka-connect-instaclustr-0.1.3-uber.jar` jar file. 
* Copy the jar file to `~/kafka/plugin` folder using the following command: `cp target/kafka-connect-instaclustr-0.1.3-uber.jar ~/kafka/plugins`
* Go to the `bin` directory 

* Create and start Kafka Backup, Kafka Restore Docker along with Kafka connect standalone.
```sh
./setup.sh
```
* Start Kafka connect backup,consumers,scenario-1 data ingestion and export consumer group offset details in `bin/offsetdetails` to verify from backup and restore by running `scenario_1.sh`
```sh
shellscripts/scenario_1.sh
```
* Verify scenario-1 kafka topic data and offset backup in S3(`$bucket/databackup/datetime`).

* Start Kafka connect backup,consumers,scenario-2 data. Replace tumbstone message with value by running `scenario_2.sh`

```sh
shellscripts/scenario_2.sh
```
* Verify scenario-2 kafka topic data and offset backup in S3(`$bucket/databackup/datetime`). 

* Restore data and consumer group offset for scenario 1 and 2 by providing S3 prefix `databackup/datetime`
```sh
shellscripts/restore-standalone.sh `S3-Prefix`
```
* Verify data is copied from S3 folder to the restored kafka topics in kafka-restore cluster. 

* Verify consumer group offset backup and tranlated offset in `bin/offsetdetails` folder and kafka restored cluster. 

## Additional Information

* [Managing Kafka Connector](ManagingConnector.md)
* [Kafka Backup Architecture](Kafka_Backup_Architecture.md)
* [Pre-built Kafka Connect Plugins](https://www.instaclustr.com/support/documentation/kafka-connect/pre-built-kafka-connect-plugins/)