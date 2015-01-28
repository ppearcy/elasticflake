# Elasticflake

Sequential UUID / Flake ID generator pulled out of elasticsearch common

This library is 100% verbatim copy and paste from elasticsearch common and was 30 mins to package into a lib. All credit should go to:

* Brian Murphy / Mike McCandless - https://github.com/elasticsearch/elasticsearch/pull/7531/
* Elasticsearch team
* http://www.boundary.com/blog/2012/01/flake-a-decentralized-k-ordered-unique-id-generator-in-erlang/
* Twitter Snowflake - https://github.com/twitter/snowflake

And I'm sure many others...

# Why

Elasticsearch is a large dep to pull in just to get sequential uuids. If you already have elasticsearch as a dependency, you are better off using it the UUID generator from there. However, if you are generating time series data that may eventually live in elasticsearch (or any other datastore with primary key support), it is a best practice to generate the UUID as soon as possible in the pipeline. 

## Preventing duplicates

If you are generating time series data that doesn't have a natural primary key, you want to make sure that if there are hiccups throughout the system a unique UUID at message creation can give once and only once messaging guarantees. This is especially useful if using Kafka, RabbitMQ, or other messaging systems for guaranteed delivery. 

## Performance

Random UUIDs are not good for primary key/clustered index as they cause lots of page splits and overheads. Time based UUIDs alleviate these problems for both SQL and elasticsearch based indexes. 

## Building

Clone the repo and run

```
gradle publishToMavenLocal
```

Once published to your local Maven you can pick up from any other project, eg from sbt:

```
"org.limberware" % "elasticflake" % "0.1-SNAPSHOT"
```

If someone finds this useful, open a pull request to get published to Maven central and I can version and make that happen.
