---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Partitions-Leadership
{: #partition_leadership }

Jede Partition verfügt über einen Server im Cluster, der als Leader der Partition fungiert, und über andere Server, die als Follower fungieren. Alle Anforderungen zur Erstellung und Verarbeitung für die Partition werden vom Leader behandelt. Die Follower replizieren die Partitionsdaten des Leaders mit dem Ziel, mit dem Leader Schritt zu halten. Wenn ein Follower mit dem Leader einer Partition mithält, ist die Replik des Followers synchron. 

Wenn eine Nachricht an den Leader der Partition gesendet wird, ist diese Nachricht für die Consumer nicht sofort verfügbar. Der Leader hängt den Datensatz für die Nachricht an die Partition an und weist ihr die nächste Offset-Nummer für diese Partition zu. Nachdem alle Follower für die synchronen Replikate den Datensatz repliziert und bestätigt haben, dass sie den Datensatz in ihre Replikate geschrieben haben, ist der Datensatz jetzt *per Commit festgeschrieben*. Die Nachricht ist jetzt für die Consumer verfügbar.

Wenn der Leader für eine Partition fehlschlägt, übernimmt einer der Follower mit einer synchronen Replik automatisch die Rolle des Leaders der Partition. In der Praxis heißt das, dass jeder Server der Leader für einige Partitionen und der Follower für andere Partitionen ist. Das Leadership von Partitionen ist dynamisch und ändert sich mit den Servern.

Anwendungen müssen keine spezifischen Maßnahmen ergreifen, um die Änderung im Leadership einer Partition zu handhaben. Die Kafka-Client-Bibliothek stellt automatisch wieder eine Verbindung mit dem neuen Leader her, obwohl die Latenz während der Cluster-Einstellung erhöht wird.
