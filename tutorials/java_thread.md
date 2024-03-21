############################

############################
# JAVA THREAD
############################

############################

# Java thread
thread kelime anlamı: 1. iplik, 2. konu

# thread oluşturma
iki şekilde thread yaratılabilir:

1- Runnable arayüzü implemente edilerek

2- thread sınıfı extend edilerek

örnek:

```java
class ImplementsRunnable implements Runnable {

   // runnable sadece run metotunu içerir.

   private int counter = 0;

   public void run() {

         counter++;

         System.out.println("ImplementsRunnable : Counter : " + counter);
   }
 }

 class ExtendsThread extends Thread {

   private int counter = 0;

   public void run() {

         counter++;

         System.out.println("ExtendsThread : Counter : " + counter);
   }
 }
```

hangisini kullanmak gerekli:

- runnable kullanılırsa; ImplementsRunnable sınıfımız isterse başka sınıflardan extend edebilir. fakat ExtendsThread zaten extend hakkını kullandığı için tekrar extend edemez (Java kuralı)

- runnable'den implemente etmiş sınıfta başlatılan her thread "private int counter" değerini ortak kullanır.

ImplementsRunnable direk çağrılamaz. Thread içine atılmalıdır.

```java
Thread t1 = new Thread(new ImplementsRunnable());
t1.start();

Thread t2 = new Thread(new ImplementsRunnable());
t2.start();
```

yukarıda t1 ve t2 aynı "counter" değeri kullanır.

# void run vs void start
run metotu çağrıldığında yeni bir paralel thread yaratılmaz. fakat start metotu run metotunu thread yaratarak çağırır.

start metotu aynı sınıf için 2 kere çağrılırsa IllegalThreadStateException fırlatır.

# void sleep(long)
Millisecond cinsinden parametre alır ve ilgili thread'i o kadar bekletir.

# void join()
örnek:

```java
t1.join();
a++;
```

t1 thread'i sonlanmadan a++ çalışmayacaktır.

# void join(long)

örnek:

```java
t1.join(3000);
a++;
```

t1 ölürse a++ çalışır, yada 3 saniye içinde ölmezse a++ çalışır.

# static Thread currentThread()
bu metot o anda çalışmakta olan thread'in instance'ını dönüyor. static bir metot olmasına rağmen o anda bu metotu çağıran thread'in bilgisini alabiliyor.

# getName setName
thread sınıfına isteğe bağlı isim atanabiliyor.

# setDaemon geDaemon
boolean bir parametre alır. normalde Java tüm thread'ler sonlanmadan JVM'i kapatmaz. daemon thread'lar bu kurala dahil değildir. daemon thread'ler sorgulanmadan JVM kapanabilir.


# Executors
Birçok thread'i yönetebilmemizi sağlayan bir yapıdır.

ExecutorService executor = Executors.newFixedThreadPool(5);//onceden 5 thread açık tutuluyor. hızlı başlangıç için.

```java
for (int i = 0; i < 10; i++) {

   MyRunnableImpl runnable = new MyRunnableImpl(i);
   executor.execute(runnable);
}

executor.shutdown();

while (!executor.isTerminated()) {

     //wait
}

System.out.println("Finished all threads");
```

# ThreadGroup

```java
ThreadGroup tg1 = new ThreadGroup("Group A");

Thread t1 = new Thread(tg1,new MyRunnable(),"one");

Thread t2 = new Thread(tg1,new MyRunnable(),"two");
```

artık gruplandıklarından dolayı tümüne aynı işlemi uygulamayabiliriz:

```java
Thread.currentThread().getThreadGroup().interrupt();
```

# synchronized
bu Java keyword'ü metotun imzasına koyulduğunda o metot aynı anda en fazla 1 thread tarafından çalıştırılabilir olur. diğer thread'ler beklemeye alınır:

```java
public synchronized void anyMethod() {
    // any code here
}
```

İstersek sadece belli bir kod bloğunu da kilitleyebiliriz:

```java
public void anyMethod() {

    // some code here

    synchronized (monitorObj) {
        // some code here
    }

    // some code here
}
```

yukarıdaki synchronized bir metot değil. özel bir syntax. bu örnekte; monitorObj, bir variable'dır. monitorObj, bu kod bloğunun o andaki thread için bekletilip bekletilmeyeceğine karar veriyor. O anda monitorObj instance'ı aynı olan her thread bu metoda girmesi engelleniyor. monitorObj yerine istediğimiz şeyi kullanabiliriz. Örnek:

```java
synchronized (this) {
// Bu satırda, this'leri eşit olan her thread beklemeye alınacak ve sadece 1 thread bu kod bloğuna girebilecek.
```

synchronized'ın kullanımının birçok detayı var: Örneğin:

```java
synchronized (MyClass.class) {
// Bu özel bir durumdur.
```

Ek olarak static sınıfıların static metodlarında farklı davranış biçimleri mevcut.

# volatile
kelime anlamı: uçucu.

Java'da istenilen bir variable volatile yapılabilir:

```java
public class SharedObject {
    public volatile int counter = 0;
}
```

JVM her performansı arttırmak amaçlı bazen CPU-cache'i kullanır. RAM'deki değerleri CPU-cache'de bırakır ve bu değerleri değiştirisek, tekrar memory'ye yazılması zaman alabilir. Çünkü CPU-cache bazen bu değerleri toplu şekilde memory'ye taşır. İlgili thread'in kendisi bir değer okumak istediği zaman önce CPU-cache sonra RAM'i okur. Multi-CPU cihazlarda her thread aynı CPUya gider. Fakat farklı thread farklı CPU'da olabilir. Bu sebeple güncel verimizi okuyamayacaktır. Bunlar sadece ihtimaller. Bu sebeple eğer bir değişken birden fazla thread tarafından okunacak ise onun değerini volatile keyword'ü ile işaretlememiz önemlidir. Fakat bu değer random number yaratıyorsa ve bizim için dublike olması önemli değil ise volatile yapmayabiliriz.

Java'daki volatile eğer her thread değişkeni manüpüle edecek ise %100 temiz çözüm olmamaktadır. Sadece 1 thread menipüle edip, diğer thread'lerin değişkeni okuduğu durumlarda %100 güvenli çözüm sunabilmektedir. Eğer birçok thread yazacak ve okuyacak ise, bu durumda AtomicInteger, synchronized gibi çözümlere gitmemiz gerekmektedir.

Java'da volatile keyword'ü CPU-Cache kullanımını (veya diğer optimizasyonları) o değişken için disable ettirdiğinden, performans açısından bir dezavantaj yaratmaktadır.

# Collections.synchronizedMap() vs ConcurrentHashMap
Collections.synchronizedMap() ve ConcurrentHashMap iki farklı thread-safe sunan alternatiflerdir. Çünkü standart Java HashMap thread-safe değildir.

Bazı farklar:

- ConcurrentHashMap kullandığımızda Iterator ile for loop içerisinde map.put() metotu ConcurrentModificationException fırlatmaz. Oysa synchronizedMap bu hatayı fırlatır.

- ConcurrentHashMap null key veya null value kabul etmez. Fakat synchronizedMap implementasyonuna göre kabul edebilir.

- synchronizedMap'te tüm metotlar synchronized'dır. Yani tüm işlemler blocking'dir. Bu sebeple yavaştır. Fakat ConcurrentHashMap'ta okuma işlemleri non-blocking'dir. ConcurrentHashMap'te aynı anda aynı bucket'a kayıt işlemleri blocking iken, farklı bucket'lara kayıt işlemi non-blocking'dir.

# Is "Iterator" thred safe
Collections sınıfı synchronizedList, synchronizedMap gibi birçok metot sunar. Bunlardan dahi dönen çoğu  sınıfı thread-safe değildir (bazıları hariç). Bu sebeple Iterator'ları manuel synchronized yapmak gerekmketedir:

```java
Collection c = Collections.synchronizedCollection(myCollection);

synchronized (c) {
    Iterator i = c.iterator(); // Must be in the synchronized block
    while (i.hasNext())
        foo(i.next());
}
```

Fakat bazı Iterator'lar yapısı gereği thread safe olabilir. Buna en bilinen örnek CopyOnWriteArrayList'tir. CopyOnWriteArrayList Iterator'a data'nın klonunu döndürüyor. Dolayısı ile asıl CopyOnWriteArrayList listesinde bir değişiklik olduğunda, Iterator'da sorunsuzca döngüye devam edebiliyoruz. Fakat CopyOnWriteArrayList'ın Iterator'daki remove işlemi direk olarak UnsupportedOperationException fırlatmaktadır.

# thread safe
bu terim tüm programlama dilleri için geçerlidir. bir metot aynı anda istenildiği kadar thread tarafından çağrılıyor ve sorunsuz çalışıyor ise o metot thread safe'tir. bir sınıf thread safe ise o sınıfın içindeki tüm public metotlar thread safe anlamına gelir.

örnek:

StringBuilder thread safe değildir. Bu durumda;

```java
private StringBuilder sb = new StringBuilder("abc");

public void addProperty(String name, String value) {

    sb.append(name).append('=').append(value);
}
```

Aşağıdaki gibi çağırırsak, kodun yanlış yapma olasılığı vardır:

```
Thread1: addProperty("a", "b");
Thread2: addProperty("c", "d");
Thread3: addProperty("e", "f");
```

# Threadlocal
Java'da bir sınıftır. aşağıdaki örnekte basit şekilde kullanımı mevcut. ThreadLocal her thread için yeni bir nesne yaratmaktadır. nesneyi yaratmak için initialValue() metotunu çağırır.

```java
public class Foo
{
    private final ThreadLocal<SimpleDateFormat> formatter = new ThreadLocal<SimpleDateFormat>(){

        @Override
        protected SimpleDateFormat initialValue()
        {
            return new SimpleDateFormat("yyyyMMdd HHmm");
        }
    };

    public String formatIt(Date date)
    {
        return formatter.get().format(date);
    }
}
```

Real world examples:

- RequestContextHolder
- SecurityContextHolder
- LocaleContextHolder
- DateTimeContextHolder

# wait notify
Java'nın obje sınıfına ait metotlarıdır. wait ile o thread artık bekleme durumuna girer. taki başka biri o thread üzerinde notify metotunu çağırana kadar.
