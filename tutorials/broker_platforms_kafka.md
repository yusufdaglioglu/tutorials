############################

############################
# BROKER PLATFORMS KAFKA
############################

############################

# Apache Kafka
- Kafka is open source and developing by Apache. But Confluent's Kafka is a downstream (fork) of Apache Kafka which has additional features.
- Kafka is a broker. Publishers send messages to topics, and the consumers read the messages from subscribed topics.
- Kafka stores the messages in files which named __segment file__. In this way the messages are not store only on RAM.

# Zookeeper
Kafka requires Zookeeper for the following reasons:
- store leader and follower informations
- which nodes are up/available in cluster
- list of topics
- which topics are storing in which nodes and partition lists
- Quotas - how much data is each client allowed to read and write
- ACLs - who is allowed to read and write to which topic (old high level consumer) - Which consumer groups exist, who are their members and what is the latest offset each group got from each partition.

In the "/kafka-server/bin/" directory there are command line scripts which can easily allow developer to call the APIs of Kafka server. Examples:

```sh
# first we start the kafka server
/bin/kafka-server-start.sh "config/server.properties"

/bin/kafka-topics.sh --Zookeeper "localhost:2181" --delete --topic "DEMO"

/bin/kafka-console-producer.sh --broker-list "localhost:9092" --topic "sampleTopic"

/bin/kafka-topics.sh --create --Zookeeper "localhost:2181" --replication-factor "1" --partitions "1" --topic "sampleTopic"
```

As it can be seen on above example, operations like creating or removing topics requires Zookeeper's socket information. Because this requests are not sending to Kafka, they sending directly to Zookeeper. But the commands like consuming or publishing messages are sending directly to Kafka server.

If we read the script file: (source-id: 372), we will see that it calls this class: (source-id: 373). As it can be seen inside the Scala class "org.I0Itec.zkclient.ZkClient", it sends request to Zookeeper via Zookeeper client.

Kafka client APIs are offers methods like listing the all topics (source-id: 374):

```java
KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(props);
topics = consumer.listTopics();
```

Kafka can send requests to Zookeeper to create or delete topics. Those APIs are considered as admin operations. (source-id: 375) title: "Running from JAR", paragraph: 2 + (source-id: 376)

# Consumer and Publisher API
- producer example:

```java
Properties props = new Properties();
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9990,localhost:9991");//Kafka servers which are on the same cluster
props.put(ProducerConfig.CLIENT_ID_CONFIG, "client99");
props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, LongSerializer.class.getName()); // record key serializer
props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());// record value serializer
props.put(ProducerConfig.PARTITIONER_CLASS_CONFIG, CustomPartitioner.class.getName()); // the details are below
// other props may come here...

Producer<String, String> producer = new KafkaProducer<String, String>(props);

// we create a message here
final ProducerRecord<Long, String> record = new ProducerRecord<>("my-topic", "my-key", "my-value");

// we send the message
RecordMetadata metadata = producer.send(record).get();

print("record sent to partition: " + metadata.partition() );
print("record sent to offset: " + metadata.offset() );
```

- consumer example:

Consumer'lar sadece spesific 1 partition ve topic'ten de çekebilir:

```java
// example using Spring Boot
@KafkaListener(topicPartitions = @TopicPartition(topic = "topicName", partitions = { "0", "1" }))
```

Yada tüm topic'teki tüm partition'lardan okuyabilir:

```java
// Example without Spring Boot (core Java).
KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(props);

// we should subscribe to get a message form that topic
consumer.subscribe("my-topic")

ConsumerRecords<String, String> records = consumer.poll(100); // 100 is the time in milliseconds consumer will wait if no record is found at broker.

while(true){
    //we loop one by one for all messages from that topic
    for (ConsumerRecord<String, String> record : records){
            //we got the message
            print(record.offset(), record.key(), record.value());
    }
    consumer.commitAsync(); // or 'commitSync' to block the thread
}

consumer.close();
```

# CustomPartitioner
CustomPartitioner includes a method which is calling before we send a message to Kafka server. This method returns the partition number for that record (message).

```java
import java.util.Map;
import org.Apache.Kafka.clients.producer.Partitioner;
import org.Apache.Kafka.common.Cluster;

public class CustomPartitioner implements Partitioner{

  private static final int PARTITION_COUNT=50;

  @Override
  public void configure(Map<String, ?> configs) {
  }

  @Override
  public int partition(String topic, Object key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster) {

    // custom (any) logic here...
    Integer keyInt=Integer.parseInt(key.toString());
    return keyInt % PARTITION_COUNT;
  }

  @Override
  public void close() {
  }
}
```

As seen above each partition ID is an integer. Therefore TopicPartition is creating using topic name and partition ID:

```java
TopicPartition(String topic, int partition)
```

source: (source-id: 377)

# committing an offset
"Committing an offset" means that consumer client got the message. That means a spesific record is "consumed".

As default Kafka server has a topic which stores the offsets. Kafka follows the offsets from this partititon. In other words, Kafka knows which record (message) consumed via that partition. (source-id: 378) title: "Atomic multi-partition writes", paragraph: 3.

- # record (or message)
    The each message which send by client to Kafka server. Each record is key-value. Kafka guarantees the consumption of messages in order (The order is based on partition only - not topic!).

- # Broker
    Each kafka server (node).

- # Kafka cluster
    A group of nodes can create one cluster.

- # partition
    It is the same thing with the "partition" term that we use on other NoSQL databases. In many NoSQL databases, we distribute the data depending on the partition ID. In most NoSQL databases as default the partition ID is the hash of the data (on Kafka the "record"). In kafka we can implement CustomPartitioner class to choose the partition of each record.

    Some rules:
    - A broker may store multiple topics.
    - Each topic must store at least in one partition.
    - 1 partition must have only 1 topic.

    Partition ID and topics are both unique together. In other words, "user-created-topic" has partition ID "1", but "warnings-topic" may also have partition ID "1". Both these partitions are completly different and independed.

    Topics are logical consepts, partition are physically divided portions.

    Partitions are like queues. We can consume a record from partition using partition id and topic name. (source-id: 379) title: "KafkaConsumer", 3rd code example.

    To understant clearly we can give real examples:
    - Topic: "completed-payment-transactions", Partition: Store-ID.
    - Topic: "order-started", Partition: Store-ID.

    Each store should have is equivalent partition ID. Examples:
    - store-id: 100 ---> partition-id: 1
    - store-id: 200 ---> partition-id: 2
    - store-id: 400 ---> partition-id: 4

    The partition-ID's on Kafka start from zero. So we should find a mathematical operation to find the equivalent partition ID from store ID.

    (source-id: 379) title: "KafkaConsumer", 3rd code example is written on Python. But the same object exist also on Java. TopicPartition instance is creating using topic name and the partition ID. (source-id: 377):

    ```java
    TopicPartition(String topic, int partition)
    ```

    Kafka does not store which client/consumer readed which data. Therefore each client should remember it's own offset (topic-offset ve partition-ID) he read last. But kafka has another feature: It stores the last readed record (offset) for each "__consumer group__".

    But even Kafka has this feature it is recommended to implement idempotent consumer functions. If we can not implement idempotent functions, it is recommended to write the unique information from consumed messages to a cold storage. So we can verify if the new consumed message is consumed already or not.

- # offset
    offset meaning in IT: the distance to an element within a data object.

    Each record has an offset in a specific partition. This value never changes.

- # replication factor
    her partition'un kaç adet broker'da klonlanacağını belirtebiliyoruz. mesajımızı X node'unun A partition'una gönderelim. X node'u mesajı aldığında diğer node'ların ilgili partition'larına yollar. bu şekilde bir makinemiz down olduğunda, diğer makineler mesajı saklamaya ve dağıtmaya devam edecektir.

    her partition için __leader__ ve __follower (or in-sync replicas or ISR)__ broker'lar vardır. follower'lar (ilgili partition için) sadece data'nın klonlanmasında görevlidirler. aynı zamanda leader çökerse follower'lardan biri leader olarak devreye girmektedir. kaynak: (source-id: 381) "Replication: Kafka Partition Leaders, Followers, and ISRs." başlığı. + Diğer kaynak: (source-id: 382) "What is Replication?" başlığı "Leader for a partition:" ile başlayan 7inci paragraf.

    X partition'a bir mesaj atıldı. Bu mesaj X partition'un liderine yollanmamış olsun. O zaman bu isteği alan node, ilgili mesajı o partition'un liderine forward eder. kaynak: (source-id: 383) "Kafka topic partition" başlığı 5inci paragraf.

    bir topic yaratma örneği:

    ```sh
    /bin/kafka-topics.sh --create --Zookeeper "localhost:2181" --replication-factor "1" --partitions "1" --topic "test"
    ```

    Bu komutta:

    - replication-factor parametresinde, bu topic ve ona bağlı partition'ın kaç adet node(broker)'da tutulacağını belirttik.
    - partitions ile bu topic'e bağlı kaç farklı partition olması gerektiğini belirlemiş olduk
    - Zookeeper parametresi başka yerde anlatılıyor (bu konu için önemsiz bilgi)

- # committed record
    - record is committed only if all follower's are got the record successfully. kaynak: (source-id: 381) "Kafka Architecture: Kafka Replication – Replicating to Partition 0" başlığı, 1inci paragraf.
    - Client mesajın commit edilip edilmediğini API aracılığı ile bilebiliyor. kaynak: (source-id: 385) "KAFKA REPLICATION: 0 TO 60 IN 1 MINUTE" başlığı , 3üncü paragrafın ilk cümlesi.
    - Consumer'lar sadece commit edilmiş record'ları okuyabiliyor. kaynak: (source-id: 381) "Kafka Architecture: Kafka Replication – Replicating to Partition 0" başlığı, 1inci paragraf.

- # acknowledgment (or ACK)
  producer (client) tarafında ayarlanan bir parametredir. bu parametre şu değerleri alabilir:
  - 0: client isteği atar. isteğin bir yere ulaşıp ulaşmadığı ile hiç ilgilenmez.
  - 1: client sadece leader'ın isteği aldığından emin olana kadar bekler. hata alırsa tekrar istek atar.
  - all: o partition için tüm follower'ların bu record'u aldığından emin olana kadar bekler. hata alırsa tekrar istek atar.

  kaynak: (source-id: 387) "Apache Kafka ack-value" başlığı.

# Add new partitions to existing topics
Bu çok kolay şekilde yapılabiliyor:

```sh
bin/kafka-topics.sh --zookeeper zk_host:port/chroot --alter --topic my_topic_name --partitions 9 
```

Fakat Kafka arkaplanda eski data'ları ilgili partition'lara move etmez. Kafka'dan consume edilen mesajlar zaten consumer tarafından işlenmiştir. Eski mesajlar özellikle okunmuyor se, eski dataları move etmeye ihtiyacımız yok demektir.

Eğer mutlaka partition'lardaki data'ların partition key ile uyuşması bekleniyor ise, yeni topic oluşturmayı da alternatif bir çözüm olarak düşünebiliriz.

"./bin/kafka-reassign-partitions.sh" script'i ile var olan partition'ları farklı broker'lara taşıma tarzı işlemler yapılabilmektedir.

# Kafdrop
Kafka için açık kaynaklı GUI monitoring uygulaması. Kafdrop Kafka API'sini kullanarak web arayüzde monitor ettiği bilgileri gösteriyor. Sadece temel özellikleri içeriyor.

# MirrorMaker
Kafka default'ta (tek başına) multi DC hizmeti mevcut değil. Bunun için Kafka sunucusundan bağımsız yazılımlardan yararlanmak gerekiyor. Bunlar biride MirrorMaker.

MirrorMaker Kafka'lardan bağımsız olarak ayağa kaldırılıyor. Ayağa kaldırırken tüm data center'lardaki her Kafka'nın IP-port bilgileri config ile MirrorMaker'e veriliyor. Aynı zamanda MirrorMaker'a, hangi Kafka'lardaki topic'lerin hangi Kafka'lara sync olacağı gibi bilgiler veriliyor. Bu bilgiler doğrultusunda MirrorMaker consumer ve producer olarak davranıp, data center'lar arasında bu Kafka'ları sync etmeye çalışıyor.

'Confluent' firmasının geliştirdiği 'Replicator', 'MirrorMaker'a alternatiftir.

# Kafka connect
Kafka'dan bağımsız çalışan bir instance'dır. 2 farklı connector'ü vardır:
- __Source connector__

  farklı bağımsız sistemlerden (örneğin veritabanlarından), data okur ve bunları Kafka'da topic'lere kaydeder. örnek: PostgreSQL'deki bir tablodaki her değişikliğin, Kafka'da topic altında event'lerde görmek isteyebiliriz.

  Kafka connect, değişiklikleri okuyacağı sistemin connector'üne ihtiyaç duyar. örnek "MySQL connector jar"'ını path'e eklemek zorundayız. Yukarıda "debezium" gibi aracı uygulamalar kullanmak durumundayız.

- __Sink connector__

  source'nin tersine, Kafka topic'lerindeki data'ların bağımsız bir sisteme kaydedilmesini sağlar. örnek: her Kafka'daki data'nın ElasticSearch'e kaydedilmesini isteyebiliriz.

# Kafka Streams
Hem consumer hemde producer için API'dir. Bu API, Java'daki stream altyapısına uygun metodlar sunar.