############################

############################
# BROKER PLATFORMS TERMINOLOGY
############################

############################

# Messaging system
Messaging system'a aracılık eden tool'a __messaging broker (or message broker)__ denir.

# JMS (or Java Message Service)
- JMS is part of Oracle's JEE specification. It's a Java API that encapsulates both message queue and publish-subscribe messaging patterns.
- JMS is a very popular API and is implemented by most messaging systems. JMS is only designed for Java clients.
- JMS does not define a standard wire (data-transfer) format. it only defines a programmatic API.

source for all above: https://activemq.apache.org/components/artemis/documentation/2.0.0/messaging-concepts.html title: "Java Message Service (JMS)".

- JMS dünyasında broker'lara __JMS provider__ deniliyor.
- "Apache ActiveMQ Artemis" provides a fully compliant JMS 1.1 & 2.0 + Jakarta Messaging 2.0 & 3.0 API. source: https://activemq.apache.org/

# AMQP (or Advanced Message Queuing Protocol)
AMQP'nun farklı versiyonları var.

__wire-level protocol__ network üzerinden atılan data'nın fromatının belirlendiği stadartlardır. Örneğin; JMS bir API'dir, fakat implementasyonun network'ten hangi data ile nasıl haberleşeceğine karışmaz. Fakat AMQP __wire-level protocol__'dür. Data'nın kendisine karışır. source: https://www.amqp.org/resources/developer-faqs

__wire-level protocol__ (kelime kökeni bunu ifade etmese de) fiziksel network katmanındaki formatlarla ilgilenmez, onun yerine API-level'daki (daha üst katmanların) yollayacağı data formatını ifade etmek için kullanılır. TCP/UDP katmanının wired-level'a girip girmediği bir tartışma konusudur.

# JMS Interface and Clasess
- Yollanan mesaj bunlardan biri ile yollanır: Message, BytesMessage, MapMessage, ObjectMessage, StreamMessage and TextMessage.
- Queue: sadece P2P için gerekli objedir.
- Topic: sadece Pub/Sub için gerekli objedir.
- Destination: the common supertype of Queue and Topic.

# message queuing vs publish-subscribe (or P/S) & topic vs queue
Kafka ve alternatifi sistemler 2 tipte mesaj yollama işlemini yapabiliyor. __publish-subscribe__'da tüm consumer'lara mesaj giderken, __queuing (or Point-to-Point Messaging Model or P2P Model or PTP)__ modunda sadece 1 adet consumer mesajı okuyabiliyor. kaynak: (source-id: 388) "Kafka as a Messaging System" başlığı ilk paragraf.

JMS dünyasında terimler yukarıdaki gibidir ve JMS dünyasında P/S __topic__'ler aracılığı ile yapılırken, P2P'de queue aracılığı ile yapılır.

Kafka yukarıda yazan "message queuing" ve "publish-subscribe" özelliklerini sadece "topic" üzerinden destekliyor. Bu bazen makalelerde terimlerin yanlış anlaşılmasına sebep olabiliyor. Kafka yukarıda yazan özelliklere ek olarak şu özelliği sunuyor: streaming. Yani offset'i bilen client'lar eski bilgileri istediği okuyabiliyor, yani stream edebiliyor.

# Kafka vs RabbitMQ
Kafka streaming platform olarak geçer, RabbitMQ ise messaging broker'dır. temelde fark şuradadır: Kafka-server kuyruğu silmez. Kafka-client kuyrukta nerede olduğunu (index'i) kendi tutmak zorundadır. bu onu "consumer-centric" yapar. bu sebeple streaming platform olarak kullanılır. çünkü her client istediği gibi geçmişe ileriye gidebilir, yani stream edebilir. RabbitMQ da ise push vardır. RabbitMQ mesajları karşıya push'lar. eski mesajları client göremez. RabbitMQ'daki bu çalışma modeline "__smart-broker/dumb-consumer (or smart broker/dumb consumer)__" denir. Oysa Kafka'nın çalışma modeline tersi olarak; "__smart-consumer/dumb-broker (or smart consumer/dumb broker)__" denir.

not: Kafka-consumer(client) başarılı şekilde işlemin bittiğini ("acknowledgment") Kafka'ya bildirir. böylece Kafka bir cursor tutar. Dolayısı ile yeni ayağa kalkan bir client index'i bilmek zorunda değildir. Cursor'un olduğu yerden, pull etmeye başlayabilir. Fakat istenirse, index üzerinden de hareket edebilir. Eğer kesinlikle index üzerinden tekrar event okumayacak ise, Kafka kullanmamayı düşünmek gerekebilir.

RabbitMQ mesajları persist edebilir fakat bunların çekilmiş client'lar tarafından tekrar çekilebilmesini sağlayamaz.

RabbitMQ mesajların consumer'lara ulaşıp ulaşmadığını garanti edebilir (retry yapabilir).

# message queue vs message broker
__message broker__ yazılımı, __message queue__'ların management'ini sağlar. kaynak: (source-id: 389) title: "What is a message broker?", 4th paragraph.

- __enqueue__

  fiildir. sıraya almak.

- __dequeue__

  fiildir. kuyruktan çıkarmak.

# queue vs bus
"bus" genel bir terimdir. "queue" onun özel bir türevidir. queue'da first-in-first-out (FIFO) mutlaka olmalıdır. fakat bus'ta böyle bir zorunluluk söz konusu değildir.

# event bus için yazılımlar (broker'lar)
- __Apache Kafka__

- __Apache ActiveMQ__

  "Artemis" kod adı ile ActiveMQ projesi paralelden geliştiriliyor. Artemis stabil olduğunda eski proje sonlandırılacak. Artemis'in başlaması ile artık eski proje "Classic" olarak adlandırılıyor.

  __Advanced Message Queue Protocol (or AMQP)__ protokolü ile çalışır. Fakat MQTT, OpenWire, Stomp protokolleri ile de çalışabilir.

  - __OpenWire__: __AMQP__'ye çok benzer bir protokoldür.
  - __OpenWire__ vs __STOMP__: STOMP text based haberleşme sağlar ve OpenWire'a göre yavaştır. OpenWire ise binary haberleşme sağlar ve STOMP'a göre hızlıdır.

- __Open Message Queue (or OpenMQ or Open MQ)__

  Oracle tarafından geliştirilen, GlassFish içerisinde default olarak geliyor. İstenirse standalone da kullanılabiliyor.

- __OpenJMS__

- __Oracle Advanced Queuing (or AQ)__

- __IBM MQ (or old-name:MQSeries or old-name:WebSphere MQ)__

- __Rabbit mq__

  Pivotal Software tarafından geliştiriliyor. __AMQP__ protokolü ile çalışır.

# BROKER FEATURES
Broker olan sistemlerde şu özelliklerden bahsedebiliriz:

- ## CONSUMPTION MODE
Push vs Pull.

push: broker'ın push ettiği model.

pull: consumer'ın pull aldığı model.

Client'ın pull alması, client'ın kendine fazladan yük bindirmemesi konusunda avantaj sağlamaktadır. Çünkü broker sürekli push yaparsa sıkıntı olabilir.

- ## MESSAGE QUEUING MODEL
point-to-point (or P2P) vs __publish-subscribe (or pub/sub)

point-to-point, 1 mesaj en fazla 1 consumer tarafından okunabilir. "queue" vardır. queue'ya 1 den fazla consumer listen edebili fakat yine de 1 mesajı sadece 1 consumer okuyabilir.

P2P, Fire-and-forget (async) veya request/reply (synchronized) messaging olabilir.

pub/sub modelinde ise 'topic'ler vardır.

- ## DELIVERY SEMANTICS
birçok çeşit olabilir. bazı örnekler:
- __at-most-once__ (Kafka bunu destekliyor.)
- __at-least-once__ (Kafka bunu destekliyor.)
- __exactly-once__ (bunun %100 sağlanması çok maliyetli.)

- ## ORDERING GUARANTEE
- no-ordering:

  bazı broker'lar mesajların sırasını garanti edemeyebilir. performans açısından büyük avantaj sağlar.

- partition or queue ordering

  bazı brokerlar mesaj sırası garanti edebilir ancak sadece belli kısımları için edebilir. örneğin Kafka sadece partition bazında garanti verebiliyor.

- global ordering

  performans açısından önemli derecede yavaşlığa sebep olabilir. bu seviyede tüm partition'lar arası sürekli senkronizasyon gerekmektedir.