---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka API 사용
{: #kafka_using}

Java 클라이언트를 사용 중인 경우에는 오픈 소스 방식으로 사용 가능한 Kafka 클라이언트 0.10.x 이상을 사용할 수 있습니다. 

Kafka 클라이언트는 여러 개의 언어로 존재하며 해당 언어 중 일부에 대한 지시사항이 제공됩니다. 다른 것을 사용할 수 있지만 인증 정보를 제공하기 위해서는 SASL PLAIN 지원이 필요합니다. 또한 엔터프라이즈 플랜을 사용하는 경우에는 TLSv1.2 프로토콜에 대한 SNI(Server Name Indication) 확장도 사용해야 합니다. 

<table>
    <caption>표 1. 표준 및 엔터프라이즈 플랜에서의 Kafka 클라이언트 지원</caption>
      <tr>
	        <th></th>
		    <th>표준 플랜</th>
		    <th>엔터프라이즈 플랜</th>
        </tr>
	  		<tr>
			<td>**클러스터의 Kafka 버전**</td>
			<td>Kafka 0.10.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**지원되는 클라이언트 버전**</td>
			<td>Kafka 0.10.x 이상</td>
			<td>Kafka 0.10.x 이상</td>
		</tr>
		<tr>
			<td>**Kafka Connect, Kafka Streams 및 KSQL 지원 여부 **</td>
			<td>예</td>
			<td>예</td>
		</tr>

			<td>**인증 요구사항**</td>
			<td>클라이언트는 SASL PLAIN 메커니즘을 사용하여 인증을 지원해야 합니다.</td>
			<td>클라이언트는 SASL PLAIN 메커니즘을 사용한 인증을 지원해야 하며 TLSv1.2 프로토콜에 대한 SNI(Server Name Indication) 확장을 사용해야 합니다. </td>
		</tr>

</table>

제작자 및 이용자 API에 대한 자세한 정보는
[Kafka Producer API 0.11.0.X ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 및
[Kafka Consumer API 0.11.0.X ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}를 참조하십시오. 

