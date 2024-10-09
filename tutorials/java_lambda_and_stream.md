############################

############################
# JAVA LAMBDA AND STREAM
############################

############################

# lambda expression
- __Kalkülüs (or eng:calculus)__ hesap anlamına gelmektedir.
- __"lambda calculus (or λ-calculus)"__ matematikte fonksiyonların ifade edilmesi için bir çeşit syntax'tır. programlama dillerindeki aynı yapıya tekabül eder.
- Kalkülüs aynı zamanda matematiğin temel pozitif bilimler için gerekli birçok konuyu (türev ve integrali kapsayan matematiksel analitiğin giriş kısmı) kapsayan alanın adıdır.

- lambda programlama dünyasında anonim fonksiyon yaratmak için kullanılan özel bir syntax ismidir. örnek lamdba expression:

```
(param1) --> { print(param1) }
```

lambda, programlama dünyasında "anonim fonksiyon" anlamına gelmemektedir. fakat anonim fonksiyon yaratmak için kullanılan syntax'tır. Çünkü "anonim fonksiyon" için tek syntax lambda olmayabilir. örneğin Javascript'te şu şekilde de anonim metot yaratılabilir:

```
function(param1){ print(param1) }
```

Yani; lambda expression kullanılmadan da anonim metot yaratılabiliyor.

- Javascript'te "lambda expression" terimi yerine; "__arrow function expression__" kullanılır.

# Java dilinde lambda
Java 8 ile hem __lambda expression__ hemde __fonksiyon arayüz (or function interface)__ özelliği gelmiştir.

```java
@FunctionalInterface // bu notasyon isteğe bağlı. hiçbir özelliği yok. ama atılması öneriliyor. Çünkü normal arayüzler ile aynı syntax'ı var aşağıdaki kodun. farkını okuyan görebilmeli. (gelecekte bu iki farklı ama aynı syntaxa ait durumları derleyici vey akelnetilerin ayırt etmesi gerekebilir)
interface MyInterface {

    MyResultObj myMethod(int myParam);
}
```

ile yukarıda tanımlama yaptık. Aşağıda da kullandık:

```java
MyInterface myInterface = realParam -> {

            System.out.println(realParam);

            return new MyResultObj();
};

myInterface.myMethod(3);
```

myMethod'u implemente etmiş olduk. arayüzde myMethod gibi sadece bir metot olabilir. daha fazla olamaz. bu sebeple metotu implemente ederken hiç "myMethod" string'ini hiç kullanmadık.

- myMethod iki adet parametre alsaydı, kullanım kısmında şu satır olacaktı:

  ```java
  MyInterface myInterface = (realParam, realParam2) -> {
  ```

- myMethod metotumuz isterse parametre döndürebilir. doğal olarak dönüş parametresi kullanım kısmında myInterface.myMethod(3) ‘teki myMethod metotundan dönüyor olacaktı.

- Interface ‘ler extend edebilirler.

- kullanım kısmında; System.out.println(3);  etrafındaki süslü { } parantezler yazılmayabilir (tek satırlı kodlar için geçerli.)

- java.util.Function altında birçok hazır fonksiyon arayüzleri tanımlanmıştır. örnek: BiConsumer<T,U>, Consumer<T> gibi... Bunlar hazırdır. Çünkü; çoğu zaman genellikle fonksiyon arayüzleri 1-2-3 parametre alır ve bir parametre döner yada dönmez. Bunlar için sürekli yeni fonksiyon interface tanımlaması yapmamız gerekir. bu sebeple hazırları oluşturulmuştur. örnekler:

  - Consumer (tüketici), yani herhangi bir parametre alır, ama hiçbir şey dönmez. JDK içerisinde source code'a bakarsak:

  ```java
  @FunctionalInterface
  public interface Consumer<T> {
      void accept(T t);

      // Opsiyonel olarak burada farklı utility metodları olabilir.
      // Fakat bu ek metodlar muhahkak interface'de implemene edilmiş olmalıdır.
      // Çünkü functional interface'ler sadece 1 adet implemente edilmemiş metod içerebilir.
      // Bu metodu da developer, FunctionalInterface'i initialize ederken implemente eder.
  }
  ```

  Consumer'a alternatif olan diğer birçok örneği tabloda toplarsak:

  (T, U, R herhangi bir class'ı temsil eder)

| interface adı  | aldığı parametrelerin tipi | döndürdüğü parametre tipi |
|----------------|----------------------------|-------------------------|
| Consumer       | T                          | void                    |
| BiConsumer     | T U                        | void                    |
| Function       | T                          | R                       |
| BiFunction     | T U                        | R                       |
| UnaryOperator  | T                          | T                       |
| BinaryOperator | T T                        | T                       |
| Predicate      | T                          | boolean                 |
| BiPredicate    | T T                        | boolean                 |
| Supplier       | void                       | T                       |
| ToIntFunction  | T                          | int                     |
| DoubleConsumer | double                     | void                    |
| ObjIntConsumer | Object int                 | void                    |

"Predicate" konusu başka başlıklarda anlatılıyor (QueryDSL ve JPA Criteria konularına bakılabilir).

# real example

```java
// sumFunction'ı aşağıdaki ("bu yanlış" yazan satırdaki) gibi tanımlayabilirdik.
// Fakat stream'deki "mapToInt" metotu, parametre olarak "ToIntFunction" kabul ediyor.
ToIntFunction<Car> sumFunction; // bu doğru
Function<Car, Integer> sumFunction; // bu yanlış

// method reference:
sumFunction = Car::getSalePrice;
// lambda expression:
sumFunction = (car) -> car.getSalePrice();
// lambda expression (bazı kaynaklarda "lambda statement" olarak ifade ediliyor):
sumFunction = (car) -> { return car.getNumberOfAdults(); };

int total = carList
                .stream()
                .mapToInt(sumFunction)
                .sum();
```

# lambda vs closure
lambda closure ile aynı anlama gelmiyor. çoğu yerde aynıymış gibi yansıtılıyor. lambda bir syntax. closure ise return edilen bir fonksiyon aracılığı ile farklı scope'tan değer değiştirmemize yarayan metottur. lambda desteği olan yazılımda mutlaka closure olabileceği için bu iki terim aynıymış gibi kullanılıyor.

# metot referansları
tanımlama kısmında, implemente etmiş olduğumuz metot zaten başka bir yerde zaten var ise, başka yerdekini direk referans edebiliriz. örnek;

```java
MyInterface myInterface = AnotherClass::otherMehod; // "otherMehod" is static method or public and the return type of "otherMehod" is compatible with "myInterface".
MyInterface myInterface = anotherObj::otherMehod; // anotherObj is an instance
MyInterface myInterface = AnotherClass::new; // refers the constructor method
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java stream
Java 8 ile gelmiştir.

lambda'ya bağımlı olan (onu kullanarak yapılan) bir özelliktir. bu özellik Java programlama dilinden gelmemiştir. Java'daki kütüphaneler kendi içlerinde hazır, işe yarar arayüzleri bulundurmaya başlamışlardır. bu arayüzlere genel olarak stream denilmiştir. Collection'dan türemiş tüm sınıflar için işlemler ortak olduğundan, tüm stream kütüphaneleri ortak olarak java.util.stream paketi altında toplanmıştır. böylece tüm collection yada bir kütüphanede herkes ortak/bilindik metotları kullanabilmektedir. isteğe bağlı kendi stream arayüzlerimizi yazabiliriz.

Java'nın içinde bulunan Stream tanımına bakarsak daha iyi kavrayabiliriz:

```java
public interface Stream<T> extends BaseStream<T, Stream<T>> {

   Stream<T> filter(Predicate<? super T> predicate);

   LongStream mapToLong(ToLongFunction<? super T> mapper);

   //yukarıdaki gibi birçok metot daha burada...

}
```

Predicate konusu için buraya bakılmalı: [file:"java_querydsl.md" - title:"Predicate"](./java_querydsl.md#Predicate)

Yukarıdaki tanımlamada "Stream" sınıfı sade basit bir interface. içindeki metotlarda implemente edilmeyi bekleyen metotlar. metotlar dikkat edilirse lambda fonksiyonu kabul ediyor. işte bu yüzden Stream, lambda teknolojisine dayalı bir kütüphanedir.

Şimdi ise örnek kullanımına bakalım:

```java
// sum(), mapToLong'dan tamamen bağımsız metot.

long total = myList.stream().mapToLong(Long::parseLong).sum();

// yada

long total = myList.stream().mapToLong(

     valueFromStream -> Long.parseLong(valueFromStream)

    ).sum();
```

```java
// En temel stream örneği. collection'lar artık stream metotlarını içermektedir.

List names = Arrays.asList("Ali","Veli","Selami");

Stream stream = names.stream(); //burada bir Stream implementasyonu dönmektedir. bu implementasyon java.util.List tarafından hazırlanmış durumda.

stream.forEach(name -> {

    System.out.println(name);

});

names
    .stream()
    .filter(name -> name.length() == 4)
    .forEach(System.out::println);



//java.io içerisinde bazı sınıflarda stream döndüren metotlar gelmiştir.

Path dir = Paths.get("/var/log");

Stream pathStream = Files.list(dir); //list Java 8 ile gelen yeni bir metot'dur.

//dizi içeriğini alarak direk hazır stream döndüren metotlar vardır. önceden dizi oluşturmaya gerek kalmıyor.

IntStream intOf = IntStream.of(1, 2, 3);

IntStream intRange = IntStream.range(1, 10);

DoubleStream doubleOf = DoubleStream.of(1.0, 3.5, 6.6);

```

En son örnekte görüldüğü gibi sadece java.util.stream.Stream yok. Bunun gibi bir çok stream çeşidi var. örnek:
- java.util.stream.DoubleStream
- LongStream.

Bunun gibi kendi stream'lerimizi de yazabiliriz. LongStream ve Stream çok farklı metotlar sunuyorlar (LongStream collector ile ilgili örneğe bakın). Birbirinden bağımsız yapılardır. LongStream, Stream'in bir implementasyonu değildir fakat elinden geldiğince, ona benzemeye çalışmıştır.

Stream arayüzünde olan tüm metot imzalarından görüldüğü gibi çoğu metot yine Stream döndürüyor. Bu kolaylık olsun diye tasarlanmış bir yapı. böylece üstüste metot çağırmak istediğimizde her defasında stream'i tekrar çekmeye gerek kalmıyor.

java.util.stream.Stream içindeki bir kaç metota detaylı bakalım:

```java
/import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class TestClass {

public static void main(String[] args) {

    // collect: bir önceki stream'den dönen listedeki elemanları, collect'e parametre olarak verdiğimiz metota yollar. ve dönen değeri döndürür.
    List<Integer> numberList = Arrays.asList(1, 2, 3, 99);
    List result1 = numberList
            .stream()
            .map(x -> x + 1)
            .collect(Collectors.toList());
    // result1 = { 2, 3, 4, 100}

    // Collectors.toList() ---> Functional Interface dönüyor.
    // Collectors.toList() yerine toSet() kullansaydık Set dönerdi.

    // aşağıdaki kod derlenmeyecektir. Çünlü LongStream, Long değil primitive olan long tutan bir stream'dir.
    // dolayısı ile, Java'da, List<long> zaten oluşturamadığımız için aşağıdaki kod derleme aşamasında hata verecektir.
    LongStream.of(1l, 2l, 3l).
        .collect(Collectors.toList());

    // filter: map gibi yeni elemanlar döndürmüyor. sadece filtreden true dönen elemanları içeren yeni bir liste oluşturuyor.
    List<String> messageList = Arrays.asList("Hello", "Hi", "Maria");
    List result2 = messageList
            .stream()
            .filter(s -> s.startsWith("H"))
            .collect(Collectors.toList());

    // flatMap: Map'te obje dönerken, flatMap'te stream dönülür.
    // flatMap, stream elemanları for-each gibi döner, her elemanın ayrı ayrı stream'ini alır,
    // bütün bu stream'leri yeni bir stream yaratarak döner.
    // Örnek:
    String[][] arrayOfArray = new String[][]{{"a", "b"}, {"x", "y"}, {"1", "2"}};
    List<String> result3 = Stream.of(arrayOfArray)  // returns: Stream<String[]>
            .flatMap(Stream::of)                    // returns: Stream<String>
            .filter(x -> !"a".equals(x))            // returns: Stream<String>
            .collect(Collectors.toList());

    // Yukarıdaki Faltmap örnekğinde flatMap satırında ilk olarak bu elamanın stream'ini alıyor:
    // {"a", "b"}
    // Yani şunu yapıyor:
    // Stream.of( {"a", "b"} )
    // Tabi a ve b'yi ayrı ayrı eleman olarak tutan yeni bir stream dönüyor.
    // Sonra aynı işlemi {"x", "y"} ve diğer elemanlar için yapıyor.
    // Tüm elemanları ise tek bir stream'de birleştirip dönüyor.

    // flatMap another example:
    List<String> messageList3 = Arrays.asList("Hello", "World", "Maria");

    // Below code prints only the second character of each element.
    messageList3
            .stream()                             // returns: Stream<String>
            .flatMap(e -> Stream.of(e.charAt(2))) // returns: Stream<Character>
            .forEach(System.out::println);        // returns: void

    // sorted: based on "natural order".
    List<String> messageList2 = Arrays.asList("Hello", "Hi", "Maria");
    List result4 = messageList2
            .stream()
            .sorted()
            .collect(Collectors.toList());

    // forEach: her eleman için fonksiyon çağırır
    List numberList2 = Arrays.asList(1, 2, 3);
    numberList2
            .stream()
            .forEach(System.out::println); // returns: void
    // or
    // forEach(x -> System.out.println(x));

    // reduce
    // Ek not: "range" metotu 1, 2, 9 elementlerinden oluşan bir IntegerStream yaratıyor.
    // recude metotu kümülatif işlemler yapmamızı sağlıyor.
    // reduce önce 1 ve 2'yi bizim yolladığımız fonksiyona atıyor.
    // daha sonra sonucu "3" ile yine bizim yolladığımız parametreye yolluyor.
    // ve en sonda sonucu döndürüyor.
    // yani; şu işlemler sırası ile oluyor:

    // (1,2) -> 1+2   (buradan 3 geliyor)
    // (3,9) -> 3+9   (buradan 11 geliyor)

    // OptionalInt=9 olarak dönüyor.
    OptionalInt reduced = IntStream
            .range(1, 4)  // returns: IntStream
            .reduce((a, b) -> a + b);

    // Match operations
    List<String> messageList4 = Arrays.asList("Hello", "Hi", "Maria");

    // noneMatch: her elementi kontrol eder. Belirtilen kriter listede hiçbir elemanda bulunmuyor ise true döndürür.
    boolean result5 = messageList4.stream().noneMatch(message -> message.contains("x"));

    // allMatch: Belirtilen kriter listede tüm elemanlarda bulunuyor ise true döndürür.
    boolean result6 = messageList4.stream().allMatch(message -> message.contains("a"));

    // anyMatch: Belirtilen kriter listede herhangi bir elemanlarda bulunuyor ise true döndürür.
    boolean result7 = messageList4.stream().anyMatch(message -> message.contains("a"));

    // findAny: rastgele bir eleman döndürür.
    // paralel olmayan stream'lerde ilk elemanı dönüyor gibi gözüksede bunun garantisi verilmez.
    // paralel olan stream'lerde hep farklı eleman döner.
    Optional<String> result8 = messageList4.stream().findAny();

    // findFirst: mutlaka ilk elemanı döndürür.
    Optional<String> result9 = messageList4.stream().findFirst();
}
}
```

# intermediate vs terminal operations
filter() gibi metotlar yeni bir stream objesi döndürür, bu sebeple intermediate olarak nitelendirilir. oysa terminal operation metotları, "Stream.forEach" gibi, stream değilde, herhangi farklı bir obje döndürür.

intermediate metotlar 2'ye ayrılır:
- stateless

  bir önceki stream'den hiçbir state tutmazlar. örnek: filter and map.

- stateful

  örnek: sorted

# parallel vs serial
Collection sınıfı içinde stream() and parallelStream() metotları vardır. sadece buna göre stream'lerin paralel yürütülüp, yürütülmeyeceğine karar verilir.

örnek:

```java
List number = Arrays.asList(2,3,4,5);
number.parallelStream().forEach(y->System.out.println(y));
```

Paralel olan stream'ler paralel çalışacağı için sıra önemsiz olan kod logic'lerinde kullanılmalıdır. Paralel olanlar daha hızlıdır.

# StreamSupport
Iterable'larda stream dönen metot yok ( iterableInstance.stream() yok ). Bu sebeple özel olarak StreamSupport geliştirildi.

```java
Iterable<String> iterable = Arrays.asList("1", "2", "3", "4", "5");

List<String> result = StreamSupport.stream(iterable.spliterator(), false)
                                          .map(String::toUpperCase)
                                          .collect(Collectors.toList());
```

# java.util.stream.BaseStream
java.util.stream.Stream, BaseStream'den türemiştir. BaseStream ise sadece java.lang.AutoCloseable'dan türemiştir. AutoCloseable sadece "close" metotu içerir. Fakat her stream close metotunu implemente etmek zorunda değildir. I/O işlemi yapan stream'ler close'u implemente etmelidir.

public interface BaseStream<T, S extends BaseStream<T, S>> metotları:

- Iterator<T> iterator();

  Stream ile dönülecek data'ları java.util.Iterator instance'ına atar ve bu iterator'ı döner.

- Spliterator<T> spliterator();

  java.util.Spliterator, java.util.Iterator'a benzer bir yapıdır. iterator gibi çalışıyor, fakat tüm iterasyonu sıra ile yapmak zorunda bırakmıyor. onun yerine parça parça gruplara ayırıp, istersek her parçayı farklı thread'lerde iterate edebilmemizi sağlıyor.

- boolean isParallel();

  kullanılan stream'in paralel olup olmadığının bilgisi döner.

- S sequential();

  Aynı stream'in sıralı olan sıralı instance'ını döndürür. zaten sıralı bir stream ise, kendisini dönebilir.

- S parallel();

  Aynı stream instance'nın paralele olan instance'ını döndürüyor. eğer zaten paralel stream ise yine kendisini de dönebilir.

- S unordered();

  sequential()'ın tersini yapıyor.

- S onClose(Runnable closeHandler);

  AutoCloseable'ın metotu değildir. Burası close yapıldığında çalıştırılacak callback metotunu parametre alır.
