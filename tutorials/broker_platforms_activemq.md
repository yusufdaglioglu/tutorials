############################

############################
# BROKER PLATFORMS ACTIVEMQ
############################

############################

# Topic vs address
ActiveMQ'da, hem topic, hemde adres kavramı vardır.

client, ActiveMQ'daki bir address'e mesajı yollar. ActiveMQ bu mesajı okur ve kendi içinde tuttuğu birden fazla queue'ya yönlendirir. yönlendirirken, eğer routing-type-config'i __anycast__ ise, 1 mesaj bu queue'lardan sadece 1'ine yollanır. Oysa routing-type-config __multicast__ ise, 1 mesaj, tüm queue'lara yollanır.

ActiveMQ server'ındaki config dosyalarının örnekleri:

active-mq'da address'e yazılıyor ve queue'dan okunuyor. bu sebeple active-mq'da şu ayar olmalıdır:

```xml
<addresses>
  <address name="address.foo">
    <anycast>
      <queue name="q1"/>
    </anycast>
  </address>
</addresses>
```

multicast örneği:

```xml
<addresses>
  <address name="address.foo">
    <multicast>
      <queue name="q1"/>
      <queue name="q2"/>
    </multicast>
  </address>
</addresses>
```

Yukarıdaki multicast örneği genelde bu şekilde implemente edilmez. Genelde aşağıdaki gibi implemente edilir. çünkü active-mq zaten her bağlanan consumer için yeni bir topic kendi otomatik yaratır. bu sebeple aşağıdaki tercih edilir:

```xml
<addresses>
  <address name="address.foo">
    <multicast/>
  </address>
</addresses>
```

Yukarıdaki son örnekte, her mesaj, her queue'ya clonlanır. bu tarz queue'lara piyasada __subscription queues__ denir.

# Fully Qualified Queue Names
JMS gibi sistemler "address" kavramını bilmeyebilir. Bu sebeple JSM'in destination'ı, active-mq'daki bir queye'ya bağlanırken şu şekilde olmalıdır:

```
address::queue
```

örnek JMS kodu:

```java
Queue q1 session.createQueue("a1::q1");
MessageConsumer consumer = session.createConsumer(q1);
```
