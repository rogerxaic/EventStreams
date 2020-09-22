---

copyright:
  years: 2020
lastupdated: "2020-09-22"

keywords: IBM Event Streams, reserved disk usage

subcollection: EventStreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# Understanding reserved disk usage
{: #ES_understanding_reserved_disk_usage}

This section describes how the usable storage of an {{site.data.keyword.messagehub}} instance is utilised by the topics and partitions 
created and the configuration settings you apply.

First of all, it's important to note, the defined storage of an {{site.data.keyword.messagehub}} instance is usable storage. 
So you don't need to worry about storage used by replicas (as all topics have their replication factor set to 3), as 
that is NOT taken from the usable storage. This keeps things simple and allows you to just plan how to map the retention 
of those messages to storage utilisation.

## Understanding how Kafka stores data
{: #ES_understanding_how_kafka_stores_data}

Kafka allows users to configure retention limits on topics.

The retention.bytes configuration is the total number of bytes allocated for messages for each partition of the topic. 
Once exceeded, Kafka will delete oldest messages. So for example, if you are generally sending in 200 MB a day of messages 
to a single partition topic, and you want to keep them for five days, you set retention.bytes to 1GB (200 MB x 5 days). 
If this was over 10 partitions, you set retention.bytes = 100 MB (1 GB/10 partitions).

Internally Kafka splits each partition into log segments. This again is a property you can set, log.segment.size, the default
being 512 MB. When we mentioned that Kafka deletes the oldest messages, it actually deletes the oldest log segment. For this 
reason, Kafka needs to keep space for one extra log segment over and above the number of log segments required to satisfy 
the retention.bytes configuration.

For example: 

     - retention.bytes = 1 GB
     - log.segment.size = 512 MB
     - Kafka needs approximately 1.5 GB per partition for the topic storage.

     - retention.bytes = 100 MB
     - log.segment.size = 512 MB
     - Kafka needs approximately 1 GB per partition for the topic storage.

There is additional storage needed per partition for indexes. For each log segment, Kafka also 
stores two indexes. Their size is defined by segment.index.size, which is also configurable and has a default of 10 MB. 
For reference, the storage used by indexes is calculated as follows: 

     2 x number.of.log.segments x segment.index.size

Where: 

     number.of.log.segments = floor(retention.bytes/segment.index.size) + 1
     
## Managing storage with {{site.data.keyword.messagehub}}
{: #ES_managing_storage_with_event_streams}     

When doing topic administration operations, such as creating topics, creating partitions or changing topic configurations, 
{{site.data.keyword.messagehub}} ensures that enough storage is available to satisfy the operation. To do this, for each topic, {{site.data.keyword.messagehub}} 
computes the "reserved size" per topic using the following method:

     Reserved size = retention.bytes + log.segment.size + (2 x segment.index.size x number.of.log.segments)

Where: 

     number.of.log.segments = floor(retention.bytes/segment.index.size) + 1


The total reserved storage percentage is also exposed in Sysdig via the [ibm_eventstreams_instance_reserved_disk_space_percent
metric](/docs/EventStreams?topic=EventStreams-metrics#ibm_eventstreams_instance_reserved_disk_space_percent).

If there is not enough unreserved storage to satisfy the topic operation, it is rejected with a PolicyViolation error 
detailing the reason.

**Note:** The reserved size calculation may change in the future if Kafka storage requirements are updated.

## Examples
{: #ES_managing_storage_with_event_streams_examples}  

A non obvious effect of the above is that Kafka can reserve more storage than expected depending on your 
topic configurations.

Let's consider a few examples:

1. A topic with retention.bytes of 1 GB, and with a log segment size of 512 MB:

    With one partition, it would reserve about 1.5 GB of storage.
   
    In this case, the reserved size is significantly larger than the retention size.


2. A topic with retention.bytes of 50 GB, and with a log segment size of 512 MB:

    With one partition, it would reserve about 50.5 GB of storage.
    
    In this case, the reserved size is very close to the retention size.


3. A topic with retention.bytes of 1 GB, and with a log segment size of 128 MB:

    With one partition, it would reserve about 1.1 GB of storage.
    
    In this case, the reserved size is very close to the retention size.
  
