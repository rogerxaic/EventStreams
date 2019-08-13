---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Grenzwerte und Kontingente
{: #kafka_quotas }

{{site.data.keyword.messagehub}} verwendet Kontingente zur Steuerung der Ressourcen, wie z. B. der Netzbandbreite, die ein Service in Anspruch nehmen kann. Die Art und Höhe der Kontingente hängt davon ab, ob Sie den Plan "Standard" oder "Enterprise" verwenden.

## Plan "Standard"
{: #limits_standard }

### Netzdurchsatz
{: #standard_throughput }

Der maximale Durchsatz für jede Serviceinstanz entspricht 1 MB pro Sekunde pro Partition bis zu einem Maximum von 20 MB pro Sekunde. Beispiel: Für eine Serviceinstanz mit 10 Partitionen beträgt der maximale Durchsatz 10 MB pro Sekunde und für 30 Partitionen 20 MB pro Sekunde.

Der Durchsatz wird separat für Producer und Consumer gemessen. Im Fall einer Überschreitung wird eine Drosselung durchgeführt, indem die Antworten auf Anforderungen geringfügig verzögert werden, wodurch Producer und Consumer leicht gebremst werden, bis die Bandbreite reduziert ist.

### Partitionen
{: #standard_partitions}

100 Partitionen für jede Serviceinstanz.

### Aufbewahrung
{: #standard_retention}

Maximal 1 GB für jede Partition.

### Andere Grenzwerte
{: #standard_limits}

* Maximale Nachrichtengröße: 1 MB
* Maximal gleichzeitig aktive Kafka-Clients: 100
* Maximale Anforderungsrate [HTTP Produce API]: 100 pro Sekunde
* Maximale Anforderungsrate [HTTP Admin API]: 10 pro Sekunde

## Plan "Enterprise"
{: #limits_enterprise }

### Netzdurchsatz
{: #enterprise_throughput }

Empfohlenes Maximum von 40 MB pro Sekunde mit einem Grenzwert für Lastspitzen von 75 MB pro Sekunde. Der Durchsatz wird als Anzahl der Byte pro Sekunde ausgedrückt, die in einem Cluster sowohl gesendet als auch empfangen werden können.

Der empfohlene Wert basiert auf einer typischen Workload und berücksichtigt die mögliche Auswirkung von operativen Aktionen, wie z. B. internen Aktualisierungen, oder von Schadensmodi, wie z. B. dem Verlust einer Verfügbarkeitszone. Wenn der durchschnittliche Durchsatz den empfohlenen Wert überschreitet, kann es während dieser Situationen zu Leistungseinbußen kommen.


### Partitionen
{: #enterprise_partitions}

1000 Partitionen für jede Serviceinstanz.

### Aufbewahrung
{: #enterprise_retention}

Unbegrenzt bis zum Speichergrenzwert Ihres Plans.

### Andere Grenzwerte
{: #enterprise_limits}

*  Maximale Nachrichtengröße: 1 MB
*  Maximal gleichzeitig aktive Kafka-Clients: 10000




















