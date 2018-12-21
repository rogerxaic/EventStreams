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


# Liderança da partição
{: #partition_leadership }

Cada partição tem um servidor no cluster que age como líder da partição e outros servidores que agem
como os seguidores. Todas as solicitações de produção e de consumo para a partição são manipuladas
pelo líder. Os seguidores replicam os dados da partição do líder com o objetivo de acompanhar o ritmo do líder. Se um seguidor está acompanhando o ritmo do líder de uma partição, a réplica do seguidor está
sincronizada. 

Quando uma mensagem for enviada para o líder de partição, essa mensagem não estará imediatamente disponível para os consumidores. O líder anexa o registro para a mensagem para a partição, designando o próximo número de deslocamento para essa partição. Depois que todos os seguidores para as réplicas sincronizadas replicam o registro e reconhecem que
gravaram o registro em suas réplicas, o registro está agora *confirmado*. A mensagem está
disponível para os consumidores.

Se o líder de uma partição falha, um dos seguidores com uma réplica sincronizada assume o
controle automaticamente como o líder da partição. Na prática, cada servidor é o líder de algumas partições e
o seguidor de outras. A liderança de partições é dinâmica e muda conforme a rotatividade dos servidores.

Os aplicativos não precisam executar ações específicas para manipular a mudança na liderança de uma
partição. A biblioteca do cliente Kafka se reconecta automaticamente ao novo líder, embora você
veja um aumento de latência enquanto o cluster é estabelecido.
