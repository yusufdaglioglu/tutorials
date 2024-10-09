############################

############################
# JAVA
############################

############################

#  java.util.concurrent.atomic package
AtomicIntegerArray, AtomicInteger, AtomicLong gibi birçok sınıf barındırır. Tümü multithread uygulamalarda aynı variable'ları yönetmek için kullanılır. Bu sebeple buna uygun metodlar içerir. Örnekler:

- AtomicInteger class --> public int addAndGet(int delta)

  integer'a değer ekler ve yeni değeri döndürür. Eğer AtomicInteger olmasaydı bunu bu şekilde kulanacaktık:

  ```java
  int a = 1;
  a = a + delta;
  return a;
  ```

  Aynı işlemi iki satırda yaptığımız için, "a" variable'ı başka thread'lerden kullanılıyor olsaydı, işlem atomic olmayabilirdi. Bu tarz durumları engellemek için AtomicInteger.addAndGet() metodunu kullanmalıyız.

  Normal koşullarda bu kod dahi multi-thread uygulamalarda hatalı çalışabilir:

  ```java
  private int counter;

  public int getNextUniqueIndex() {
      // Burası tek satırda olsa da, arkaplanda birden fazla CPU operation'ı içeriyor olabilir.
      // Bu sebeple burası bazen hatalı çalışabilir.
      // Kesin olarak bildiğimiz şey şudur ki; counter++ işlemi, en az iki işlemden meydana gelmektedir:
      // int tmp = this.counter;
      // this.counter = tmp + 1;
      return counter++;
  }
  ```

  Yukarıdaki kodu bu şekilde yapmak şart:

  ```java
  private AtomicInteger counter;

  public int getNextUniqueIndex() {
      return counter.getAndIncrement();
  }
  ```

- AtomicInteger class --> public boolean compareAndSet(int expect, int update)

  değeri set ediyor ve daha sonra expected value ile eşit mi diye karşılaştırıyor. Yine 2 satırda yapacağımız işlemi tek satıra indirgemiş olduk.

- AtomicInteger class --> public int decrementAndGet()

  1 azaltıp değeri döndürüyor.

- AtomicInteger class --> public double doubleValue()

  integer'ı double'ye cast edip döndürüyor. Bu metod multi-thread için özel tasarlanmamış. Bu şekilde ek yardımcı metodlarda barındırabiliyor.

- AtomicInteger class --> public void lazySet(int newValue)

  Eventually sets to the given value. Farklı thread'ler aynı anda değeri güncleliyor ise, bu metod'un ne zaman işini yapacağı kesin değildir. Fakat eninde sonunda metod çalışacak.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Access level modifiers
modifier kelime anlamı: 1.düzenleyici 2.niteleyici

Classes can only get both below modifiers:
- public: can be access by any where
- no modifier: can be access only by classes of same package

```java
// Above rule is written only for MyClass modifiers.
public class MyClass {

  int a;
}
```

Class members (which includes the sub-class definitions inside class) can get only below modifiers:
- public
- private
- protected
- no modifier

```java
// Above rule is written only for "a" and "Abc" modifiers.
public class MyClass {

  int a;

  class Abc {

  }
}
```

On the above example, Abc class is only a definition. Is not an instance. There is no meaning to "access" them from another class (it is not logical). BUt to create a new instance from another class is to make them static:

```java
public class MyClass {

  int a;

  static class Abc {

  }
}

public class Anotherlass {

  new MyClass.Abc(); // this is valid.

  // Note:
  // If "static class Abc" is private we can not create an instance from other classes.
  // Actually, the "access modifiers" of "static class Abc" applys for the rules to create new instance of it.
}
```

On the official Java doc of Oracle, no-modifier is defined as __package-private (or no explicit modifier)__.

Class member access table:

| Modifier/access from | Class itself | other classes on the same package | Subclass (extend) in same package | other packages |
|----------------------|--------------|-----------------------------------|-----------------------------------|----------------|
| public               | Y            | Y                                 | Y                                 | Y              |
| protected            | Y            | Y                                 | Y                                 | x              |
| no modifier          | Y            | Y                                 | x                                 | x              |
| private              | Y            | x                                 | x                                 | x              |

\*Y = can be accessed

\*x = can not be accessed

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Annotation based vs Java based (JavaConfig) vs XML based configuration
Spring kendi kütüphaneleri ile "Annotation based" veya "XML based" işlem sağlayabilir. fakat Spring JSR standartları (Java standartları) dışında kendi paketlerini sunmaktadır. Spring dökümanlarında; "Annotation based"'Den kasıt Spring'in kendi kütüphaneleridir. Spring aynı zamanda Java standart annotation'ları ile de işlem yapabilmemizi sağladığı için "Java based" configuration olarak ayrı bir kavram mevcuttur. Oysa JavaEE kitaplarında "Annotation based vs Java based" kavramları aynı şeye denk gelmektedir, çünkü JavaEE zaten sadece JSR kütüphanelerini kullanmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# variable scope

Java standartlarında aşağıdaki terimler kullanılmaktadır. Fakat aynı terimler farklı programlama dillerinde farklı seviyeler için kullanılıyor olabilir.

## Local variables
   kod bloğu tamamlandığında, yok edilen değerlerdir.

## Instance variables
   garbage collector'ün temizlemesine girme durumu olan variable'lar.

## Class variables
   tüm program sonlanana kadar kullanılan değerlerdir.

```java
public class MyClass {

  final static int MAX_LENGTH = 5;  // class variable
  int length;                       // instance variable

  public void method() {
    int inches = 40;                // local variable
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# template engine
Java için açık kaynaklı template engine'ler:
- Thymeleaf
- Freemarker

- her ikisi de sadece HTML dosyalarını değil, herhangi bir dosyayı da model ile doldurabilmektedirler.
- her ikisini de özellikle Spring ile de opsiyonel kullanılabilen entegrasyon modülleri vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Apache's Commons-beanutils BeanUtils class vs org.springframework.beans.BeanUtils
Spring'in BeanUtils'i Spring Bean'lerinin instance'larında değişiklik yapabilmek için tasarlanmıştır. Oysa Apache'ninki sadece getter-setter içeren Java objelerinin instance'ları üzerinde işlem yapabilmek için tasarlanmıştır.

Apache's Commons-beanutils BeanUtils example:

```java
Course course = new Course(); // Course has "String name" and "List codes".

String name = "Computer Science";
List<String> codes = Arrays.asList("CS", "CS01");

PropertyUtils.setSimpleProperty(course, "name", name);
PropertyUtils.setSimpleProperty(course, "codes", codes);
```

Apache's Commons-beanutils BeanUtils example:

```java
// copies from different types of classes, if fields are same name.
BeanUtils.copyProperties(courseEntity, course);
```

org.springframework.beans.BeanUtils example:

```java
// copies from different types of classes, if fields are same name.
BeanUtils.copyProperties(courseEntity, course);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# LLVM
dilden bağımsız bir derleyicidir. LLVM, klasik VM'ler ile (Java VM gibi) benzerlikler göstermektedir. Bu sebeple LLVM ismi, "Low Level Virtual Machine"'den esinlenerek verilmiştir. Dolayısı ile "Low Level Virtual Machine" kısaltması değildir. LLVM özel bir isimdir. kaynak: (source-id: 341)

C ve C++ için full support'u vardır. Rust dili de LLVM ile çalışacak şekilde tasarlanmıştır.

LLVM programatik olarak kullanılabilecek bir API'ye de sahiptir. Programcı bir Java dili için JIT yazıyor olsun (örneğin JVM'e alternatif bir VM geliştiriyor olsun). Programcı, Java kodunu önce  __intermediate representation (or IR)__'a, daha sonrada makine koduna çevirebilir. Bu 2 işi yapmak içinde LLVM'in sunduğu programatik API'leri kullanabilir. kaynak: (source-id: 342) "LLVM defined" başlığı.

LLVM-frontend saf kodu IR'a çevirirken, IR'ı native binary'ye LLVM-backend ile çevrilir. kaynak: (source-id: 343) "C to IR" başlığı ilk paragraf.

LLVM-optimiser ise IR'a çevrilmiş kodu optimize eder. örneğin kullanılmayan kod akışlarını siler gibi... kaynak: (source-id: 343) "The stages are" altındaki ikinci madde.

__intermediate representation (or IR or Intermediate code or Intermediate language)__ genel olarak programlama dünyasında; derlemenin öncesinde, kodun çevrildiği bir biçimdir. birçok IR standardı vardır.

Aslında bakıldığında JIT'lerin ve VM'lerin kabul ettiği native kodlar (örneğin JVM Bytecode) bir IR'dır.

__Common Language Infrastructure (or CLI)__ Microsoft'un  ".Net" platformu için geliştirdiği IR ve bunu işletecek JIT standartlarının tümünün ismidir. __Common Intermediate Language (or CIL or old-name:Microsoft Intermediate Language or old-name:MSIL or old-name:Intermediate Language or old-name:IL)__ ise bu standartlara uygun olarak implemente edilmiş bir IR'dır. Şekil üzerinden bu durumu göstermek gerekirse:

(source-id: 345)

# GraalVM
Oracle tarafından geliştirilen OpenJDK türevi VM'dir. İsminde sadece VM ifadesi olsada, aslında SDK ve VM biraradadır. OpenJDK'ya ek olarak sunduğu özellikler:

- OpenJDK'nın JIT compiler'ına göre bazı durumlarda avantaj sağlayacak ek özellikleri/optimizasyonları var.

- LLVM toolchain

  Birçok farklı dilde yazılmış kodun 'LLVM bitcode'una çevrilmesi için derleyici içeriyor.

  C'de yazdığımız bir kodu aşağıdaki komut ile 'LLVM bitcode'una çevirebiliyoruz:

  ```sh
  $LLVM_TOOLCHAIN/clang my_c_code.c -o "my_llvm_bitcode_binary"
  ```

  Bazı diller için direk olarak support gelmemektedir. Onları "GraalVM Updater" ile kurmamız gerekiyor:

  ```sh
  gu install python
  ```

- LLVM-based diller için run-time içeriyor. Yani; "LLVM bitcode"'unda olan dosyalardan native-executable üretebiliyor.

  aşağıdaki komut ile bunu yapabiliriz:

  ```sh
  lli "my_llvm_bitcode_binary"
  ```

- polyglot API (Interoperability)

  Bu kısım için kaynak: https://www.graalvm.org/latest/reference-manual/llvm/Interoperability/

  LLVM'e çevrilebilen birçok programlama dili için, yine LLVM kodunu çağırabilmemizi sağlayan API yazılmış. Ek olarak Java, LLVM'e çevrilmemesine rağmen, oda LLVM destekleyen kodları çağırabiliyor veya LLVM'e çevrilebilen dillerden çağrılabiliyor. Örnekler:

  LLVM kodunu execute edebilen API sağlamak sanıldığı gibi kolay olmayabiliyor. Örneğin; JavaScript'te diğer objelere erişirken isim ile erişiyoruz. JavaScript kodunu LLVM'e çevirdiğimizde, LLVM'in isimleri tutuma gibi bir özelliği olmadığı için, JavaScript ile Interoperability'yi kaybediyoruz. Fakat GraalVM, burada meta-data (debug information) tutarak bunun üstesinden gelmemizi sağlıyor.

  - C kodumuzda şu kütüphaneyi kullanarak:

    ```c
    #include <graalvm/llvm/polyglot.h>
    ```

    Java kodumuzu vererek, Java kodumuzdaki nesnelere ve fonksiyonlara erişebiliyoruz.

  - Java kodumuzda şu kütüphaneyi kullanarak C kodunu çağırabiliyoruz:

    ```java
    import org.graalvm.polyglot.*;
    ```

- Java kodlarını native derleme

  - Bunu yapmak için Java kodları önce LLVM'e çevrilip, sonra native'e çevrilmiyor.

  - Ahead-on-time compiler olduğu anlamına gelir. Her OS'a bağımlı native executable üretiyor.

  - GraalVM Java kodunu native derlendiğinde, derlemesi uzun sürüyor. Çünkü sadece çağrılabilecek sınıfları embedded hale getiriyor. Bunun için tüm kodlarda özel tarama yapması gerekiyor (Static Analysis yapıyor. yani "reachable code" ları sadece executable içine gömüyor.).

    Bu taramayı yaparken (build sırasında) aynı zamanda bazı sınıfları execute edebiliyor. Örneğin static initializer'ları execute ediyor. Bu şekilde initialier'ların runtime'da işletmekle zaman kaybetmemiz oluyoruz.

  - GraalVM reflection yapamıyor. Fakat Java kodumuzda reflection varsa şöyle 3 yöntem ile çözebiliyoruz: 
  
    1. Build aşamasından önce reflection ile çağrılabilecek bütün metotların imzalarını bir JSON dosyasında saklıyoruz. Build aldığımızda bu JSON dosyasındaki her metoda birer özel referans atanıyor. Sadece bu metodlar artık runtime'da programatik olarak reflection ile çağrılabilir hale geliyorlar.

    2. biz manuel yazdığımız kodların hangileri reflection ile çağrılabileceğini biliyoruz. Bunları JSON'a tek tek yazabiliriz. Fakat framework'lerin kendi içinde neyi çağırıyor olacağını bilemeyiz. Burada framework'ün desteği şarttır. Örneğin;
    - Spring, spring-boot-starter-parent içerisinde pom.xml'i içerisinde "native" isimli bir profil tanımlamış. Bunu "mvn -Pnative" parametresi ile çağırmamız şarttır.
    - Gradle projesi kullanırken eğer "org.graalvm.buildtools.native" plugin'i ekli ise, build sırasında Gradle'daki "bootBuildImage" metodu tetikleniyor. Bu metod zaten spring-boot-starter-parent tarafından da desteklendiği için otomatik olarak Spring devreye giriyor. Dolayısı ile Gradle kullanınca, özel bir profil execute edeceğimizi parametre olarak geçmeye gerek kalmıyor.

    3. Eğer framework'ün desteği yok ise o zaman şu şekilde ilerliyoruz: https://www.graalvm.org/latest/reference-manual/native-image/metadata/AutomaticMetadataCollection/ Burada özel bir komut ile JVM üzerinde (native olmayacak şekilde) projeyi run ediyoruz. Daha sonra runtime'da uygulamamızı son kullanıcı olarak kullanıyoruz. Kullandıkça çağrılan her reflenction, bir JSON dosyaya kaydediliyor. Bu şekilde hangi sınıfların reflection ile çağrıldığını bilebilmemizi sağlıyor.

  - GraalVM bir class'ın önüne runtime'da dinamik proxy oluşturamıyor. Reflection soununu nasıl çözdüysek, aynı 3 metodolojiyi kullanarak bu sorunu da çözebiliyoruz.

  - GraalVM ile "property" dosyaları runtime'da Java class'larına map'lenemiyor. Bu sebeple manuel build başlamadan önce, property sınıflarımızı bir JSON dosyasında belirtiyoruz. Build sırasında Java-native-compiler, bunları hard-code Java sınıflarımıza map'liyor.

  - GraalVM içinde gelen 'native-image' isimli executable ile compile edilen kodlar, herhangi bir OS'ta native çalışmaktadır (hangi OS'ta derlenirse, o OS için executable oluşturulur). Fakat native kod'da çalışması için reflection gibi teknolojileri kullanmamak gerekli. Örneğin Spring framework reflection kullandığı için şu anlık desteklenemiyor (yıl 2019 sonu). Bu sebeple; direk native derlenebilmesi için uygun __Quarkus__ ve __Micronaut__ projeleri geliştirildi. Bu projeler enterprise framework ve kütüphanelerden oluşan birbirlerine alternatif projelerdir.

  - Java ile yazılan kodlarda garbage-collector'un arkada çalışması şarttır. bu sebeple native executable'ın içerisinde ufak bir virtual-machine gömülüyor. JVM'e göre çok hafif ve daha az yeteneği var. Bu VM'e '__SubstrateVM__' ismi verilmiş.

# Quarkus

```xml
<dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-universe-bom</artifactId>
        <version>1.1.1.Final</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
```

Daha sonra buradan istediğimiz extension'u dependency olarak eklememiz gerekmektedir: (source-id: 346) Bu listedeki her bir extension ayrı bir Maven dependency'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Eclipse compiler Java (or ECJ)
- Eclipse IDE'nin Java compiler'ının ismidir.
- Java'nın default compiler'ından farklıdır.
- en temel farklar:
  - kod düzgün derlenmemiş olsa bile, onu paketleyebilir (derleyebilir, jar/war oluşturur). örnek bir class fiziksel olarak yoksa fakat bir başka class içinden referans edilmişse, JDK normalde hata verirken, Eclipse IDE bu hatayı esgeçebilir. dolayısı ile runtime'da classNotFound gibi bir hata alırız.
  - kısmi derlemelere imkan sunar. kodun sadece bir kısmını değiştirdiğimizde sadece o kısımları derler.
- Tomcat'te JSP'leri derlerken ECJ kullanır. bu şekilde en ufak hatada tüm sistemi derlememezlik yapmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JRE/JDK bin directory binaries

- java

  Java app starter

- javaw

  wraps the "java" command, and starts the application on a new terminal interpreter. kaynak: (source-id: 347)

- Javaws

  Java web starter

- javac

  Java code compiler

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java package names

- java.lang.* tüm buradaki paketler otomatik import olmuş oluyor.

- java.* - JRE içinde olan core paketler

- javax.* - JRE içinde gelen fakat genelde extension (daha çok core pakete girmesi zorunlu olmayan: örnek JAXB) sınıflarını içermektedir.

  Bazı paketler javax.\*'dan java.\*'ya geçmiştir.

- önerilen isimlendirme:

  com.company_name.region_or_project_name.package_purpose

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# InputStream
reads bytes from any type of stream

# OutputStream
writes bytes to any type of stream

# flush
Türkçe kelime anlamı: 1. su ile temizlemek. "The toilet doesn't flush." gibi kullanılıyor. 2. utançtan yüzü kızartmak.

OutputStream'de olan bir metottur. bazı stream'ler write ile yazdığımız bilgiyi karşıya hemen yollamaz. bunun bir çok sebebi olabilir: cache, buffer, protokol standartları gereği belli bir boyuta gelmesi beklenir... Flush metotu stream'e / protokole yazma sinyali göndermektedir. öncelikte stream'in flushable olması gereklidir. flushable olsa dahi bu metot işlemin fiziksel olarak karşıya gidip gitmeyeceğini garanti etmez. örneğin TCP soket'e yazarken flush metotu kesinlikle bunun garantisini vermiyor, çünkü TCP standartları belli data'nın bir byte'a gelmesini bekliyor. ancak TCP soket kapatılırken, data kesinlikle yollanıyor.

# InputStreamReader
kind of InputStream that read only character type from stream.

# DataInputStream
kind of InputStream that reads all kind of primitive types from stream.

# BufferedInputStream
buffer Türkçe kelime anlamı: tampon, koruma.

kind of InputStream that reads only bytes with buffer mechanism.

# Reader
read character from any type of stream

# BufferedReader
kind of Reader that reads only character with buffer mechanizm.

# Scanner
reads primitive types and string with regex from any type of stream.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# generics
"generic programming" tüm programlama dilleri için bir terimdir. Java da bu özelliği destekler. örnek:

generic'in Türkçe kelime anlamı 'genel'dir. fakat bazı yerlerde jenerik olarak çevrilir. jenerik yanlıştır. hiçbir resmi kaynak bu şekilde çevirmez. jenerik Türkçe'de 'tanıtım' anlamına gelir.

örnek generic Java kodu:

- kullanıldığı kod:

```java
List<String> c = new ArrayList<String>(); //Java 8 ile birlikte artık sağ tarafta generic-tipini belirtmeye gerek yoktur.

c.add(new Object()); // compile-time error fırlatır
```

- tanımlandığı kod:

```java
public class Entry<K, V> {

    private final K key;
    private final V value;

    public Entry(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public Object method1(){
        return new Collection()<? extends K>;
    }
}
```

K, V harfleri istenilen karakter yada string olabilir. Genelde alışkanlık olduğundan, T (type), E (Element), V (value) and K (key) kısaltmaları kullanır.

Yukarıdaki örnekte V yerine gerçek bir sınıf ismi yazmaya gerek yoktur. zaten o sınıf belli ise (fix ise) o zaman generic yapmaya gerek yoktur. Bu sebeple oraya ne yazarsak yazalım sınıf ismi değil, özel bir keyword olarak algılanacaktır.

Yukarıdaki örnekte new Collection()<? extends K>; satırındaki "extends" tahmin edilen anlamından farklı bir anlama geliyor. K'nın subclass'ı olduğu herhangi bir sınıf gelmeli buraya anlamına geliyor. eğer tersi olursa runtime'da hata alırız. extend yerine super yazarsa, aşağıdaki satırda hata almayız. çünkü süperi "String" olan bir sınıf anlamına gelecekti:

Box<? super String> list = new Box<String>();  //super'i String olan bir sınıfı gelecek anlamına geliyor

list.add(new String("Hello World"));

Oysa yukarıda super yerine extend yazsaydı daha compile sırasında hata alacaktık.

# PECS (or Producer Extends Consumer Super)
Yukarıdaki super ve extend'li kod satırlarına dikkat edersek; super yazdığımız zaman aşağıdaki satırlarında listeye ekleme yapabiliyoruz. çünkü ne ekleyeceğimizi biliyoruz. oysa extends'de ne ekleyeceğimizi bilmediğimizden, ekleme yapamıyoruz sadece okuma yapabiliyoruz. Bu sebeple extend keyword'ü ile işlem yaptığımızda kodumuz "Producer" görevi görüyor. bu sebeple producer'lar extend etmeli terimi ortaya çıkmıştır: "Producer Extends". Benzer şekilde "Consumer Super" terimi ortaya atılmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# static initializer

```java
class Person {
   static {
           int a = 10;
           System.out.print("this block is 'static initializer' ")
   }

   {
          int b = 20;
          System.out.print("this block is 'instance initializer' ")
   }
}
```

kod çalışma sırası:

1- static initializer

2- instance initializer

3- constructor (Türkçe kelime anlamı: inşaatçı)

Yukarıdaki kodda int a ve b class'ın field'ları değil. block'un local field'larıdır. initializer block'ları ile class'ın field'larını set etmek için kullanırız. class'ların field'larını aşağıdaki gibi de set edebiliriz:

```java
class ABC {
  private a = 5;
}
```

fakat "instance initializer" ek özellikler de sunar. örneğin; if koşullarına göre farklı değerler set edebiliriz. bazı durumlarda log attırabiliriz.

# instance initializer vs constructor
yukarıda bahsedilen set etme olayı neden constructor'da yapılmıyor?

1- instance initializer'ın sınıfın 'final' değişkenlerini set etme özelliği de bulunuyor.

2- birden fazla constructor olduğunda hepsinde aynı set etme işlemini yapmak zorunda kalabiliriz. bunun yerine tüm constructor'lar için ortak olan kodu "instance initializer"'a atmakta yarar vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# objenin metotları

aşağıda açıklaması geçmeyen metotlar başka yerde anlatılıyor.

- toString
- wait
- notify
- notifyAll
- finalize: object JVM tarafından destro edilmeden önce çağrılır.
- clone: tüm variable'ların aynı olduğu yeni bir obje yaratır. bunu user'ın kendisinin override etmesi gerekir. eğer user override etmediyse direk CloneNotSupportedException fırlatacak şekilde default implemente edilmiştir.
- getClass

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# annotation
Türkçe kelime anlamı: dipnot

annotation'lar birer @interface annotation'undan türemiş objelerdir. örnek:

tanım:

```java
@Target(ElementType.METHOD) //annotation nerelerin önüne koyulabilir

@Retention(RetentionPolicy.RUNTIME) //annotation hangi adımlarda kullanılacak (runtime, source, class olabilir)

@interface Todo { //annotation ismi

 public enum Priority {LOW, MEDIUM, HIGH} // annotation'un parantez içinde verilen bir parametresi

 public enum Status {STARTED, NOT_STARTED} // annotation'un parantez içinde verilen bir parametresi

 String author() default "Ahmet"; // annotation'un parantez içinde verilen bir parametresi

 Priority priority() default Priority.LOW; // annotation'un parantez içinde verilen bir parametresi

 Status status() default Status.NOT_STARTED; // annotation'un parantez içinde verilen bir parametresi

}
```

kullanım:

```java
@Todo(priority = Todo.Priority.MEDIUM, author = "Yashwant", status = Todo.Status.STARTED)
public void myMethod() {

          //Some code here...
}
```

# annotation kendi içinde nasıl business logic uyguluyor?
- RetentionPolicy.RUNTIME

  Runtime sırasında iş yapan annotation'lar (): Runtime sırasında annotation'u sunan framework reflection kullanarak hangi sınıflarda annotation varsa a onları buluyor. her annotation'un içinde tuttuğu bir data var. bu data'lar annotation çağrılınca yazılımcı tarafından veriliyor. bunları kullanarak runtime sırasında framework business logic uyguluyor.

  Diğer fazlarda (compile gibi) çalışan annotation'lar farklı şekilde işliyor.

- RetentionPolicy.SOURCE

  @Override, @SuppressWarnings gibi sadece IDE'lerin görmesi beklenen, derleme sırasında önemsenmeyen ve derlenmeyen annotation'lardır.

- RetentionPolicy.CLASS

  derleme sırasında kullanılırlar fakat derlenip class olarak kullanılmazlar. örneğin; compile sırasında dökümantasyon oluşturmak için kullanılabilirler.

  CLASS annotation'ları derleme sırasında Java derleyicisi tarafından çağrılırlar. İşte burada "Java source code generator" yazılımı ortaya çıkıyor. Java derleme yaparken farklı source code generator'lar kullanmamıza izin veriyor. eğer farklı bir generator (processor) kullanmak istiyorsak Java compiler'ına bunu belirtmek gerekiyor.

  örnek:

  - komut satırından: javac -processor com.MyProcessor Person.java

  - Maven'dan:

```xml
<build>
  <plugins>
      <plugin>
          <groupId>org.Apache.Maven.plugins</groupId>
          <artifactId>Maven-compiler-plugin</artifactId>
          <configuration>
              <annotationProcessors>
                  <annotationProcessor>
                      com.MyProcessor
                  </annotationProcessor>
              </annotationProcessors>
          </configuration>
      </plugin>
  </plugins>
</build>
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Class objesi
Class bir sınıftır. Bu sınıf her sınıfın içinden myClassInstance.getClass() yada (static) MyClass.class ile çağrılabilmektedir. bu iki değerin referansı birbirine eşittir.

Class sınıfı, MyClass'ın özelliklerini yansıtan metotlar içermektedir. Bu şekilde MyClass hakkında bilgiler alabilmekteyiz. örneğin; myClass'ın ismi, paket ismi gibi...

# Class objesinde olan bazı metotlar/property'ler:

- static forName(String className) : metottur. Bu metota herhangi bir sınıfın ismi string olarak gönderilebilir. bize gönderdiğimiz sınıf için Class objesini döndürecektir. Örnek: Class myCarClass = Class.forName("Car");

- getAnnotation(Class < A > annotationClass): parametre aldığı annotation sınıfından ilgili sınıfta var ise o annotation'u döner. java.lang.annotation.Annotation döner. bu sınıfı tüm annotation'lar extend eder.

- Annotation[] getAnnotations(): o sınıfta kullanılan tüm annotation'ları döndürür.

- ClassLoader getClassLoader(): ilgili sınıfın classloader'ını dönüyor.

- diğer metotlar: enum olup olmadığı, field'lar, anonim olup olmadığı...

# reflections (or yansımalar)
java.lang.reflections paketleri altında olan bu sınıflar, metotlar ve sınıflar hakkında meta bilgileri alabilmemiz ve hatta gerektiğinde ismi ile aşağıdaki örnekte görüldüğü gibi metotları execute edebilmemizi sağlayabilmektedir.

```java
Method m = valueObject.getClass().getMethod(methodName, new Class[] {});

Object ret = m.invoke(valueObject, new Object[] {}); //m isimli metota parametre yolluyoruz
```

Çok sade bir örnekle Java'da reflection'ları incelersek:

```java
Class insanClass = Insan.class;
Field surnameField = insanClass.getField("surname"); // surnameField sadece meta bilgidir. bir instance değildir.

Insan ahmet = new Insan("ahmet", "kara"); // name: ahmet, surname: kara

String surname = surnameField.get(ahmet); // ahmet instance'ının surname'ini çağırdık. bize dönüş değeri yine instance'tır.

surnameField.set(ahmet, surname); // ahmet instance'ının, surname değerini set ediyoruz (tekrar aynı değerle set etmiş oluyoruz. örnek olsun diye yapıldı.)
```

# Java arrays
Java'daki her native-array, java.lang.Obje sınıfından türemiştir. fakat runtime'de dinamik oluşturulurlar. örnek:

```java
// ornek 1
Integer[] array1 = new Integer[3];
Object array2 = new Integer[3];

// ornek 2
Integer[][] array3 = new Integer[3][3];
Object array4 = new Integer[3][3];

// Hatta java.lang.Object'in metodlarını override ettiklerini de görebiliriz:
array3.getClass();
array3.hashCode();
array3.toString();
array3.notify(); // IDE ile notify metodunun içerisine bakmak istediğimizde java.lang.Obje'nin içindeki notify metoduna yönlendirdiğini de göreceğiz.
```

"toString" metodu şu şekilde print eder:

```java
int[] x = new int[3];

System.out.println(x.getClass().getName()); //ekrana [I basar.
// int[][] olsaydı [[I basardı.
// [[I sınıfı dinamik oluşturulur.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jrebel
JVM için yazılmış bir bir Classloader extension'udur. var olan classloader'a ek olarak kendi logic'lerini çalıştırır. Birçok IDE için yazılmış eklentisi de vardır.

Jrebel ile deploy edilen uygulama (app server içerisinde çalışsa dahi) hot-deploy yapılabiliyor. sadece değişen Java sınıfları deploy ediliyor ve uygulama restart istemiyor. normalde Java'da hot deploy özelliği var fakat metot imzası değişmesi gibi durumları desteklemiyor. bu eklenti ile daha fazla durumda hot deploy yapılabilmesi sağlanıyor.

ASM ve Javassist framework'leri, Bytecode'u runtime sırasında manipüle edebilmek için kullanılan projelerdir. Jrebel'da bunlara alternatif bir yazılım geliştiriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# WAR (Web Application Archives) vs EAR (Enterprise Archives) vs JAR (or Java archive)

- JAR sadece Java sınıfları ve kaynak dosyalarını barındırır.

- WAR, JAR'a ekstradan, servlet ve JSP dosyalarını barındırır.

- EAR ise servlet (WAR) + "EJB modülü" olarak ayarlanmış JAR dosyaları bulundurması gerektiğinde kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JVM memory spaces

- __Heap Space__

  Heap space'in bu şekilde gruplara bölünmesinin sebebi şu: Garbage collector yeni yaratılan variable'lardan artık kullanılmayanları daha sıkça temizliyor. Oysa eskiler, artık muhtemelen hep kullanılcak düşüncesi ile daha az sıklıkla temizlenir.

  - __Young Generation__

    - __Eden Space__
    
    Eden kelime anlamı: Havva ve Ademin yaşadığı cennet.

    'new' keyword'ü ile oluşturulan her nesne bu alanda tutulur.
    
    - __Survivor Space__
    
    garbage collector (or GC), "eden space" alanını gezip ölü nesneleri yok eder. yaşayanları bu alana taşır.
  
  - __Old Generation__
    
    garbage collector, "Survivor space" alanını gezip ölü nesneleri yok eder. yaşayanları bu alana taşır.

- __Non-Heap (or Off-heap)__
  
  - __Permanent Generation (or PermGen)__

    Permanent kelime anlamı: kalıcı.
  
    JVM'in uygulamanın içindeki sınıfların ve onların meta bilgilerini (metot imzaları gibi) tutmak için kullandığı alandır.

    Java version 8 Update: __PermGen__ is replaced with __Metaspace__ which re-sizes dynamically at runtime. Options __PermSize__ and __MaxPermSize__ have also been removed in JDK 8.

  - __Thread Memory (or Stack Memory)__

    Her thread'in kendi Stack memory'si vardır. Her stack memory thread'de çağrılan her metodu ve local variable'ları saklar. Bunu yaparken stack data structure'sini kullanır.

  - __Code Cache__
  
    it can be Virtual or reserved.
  
    Hotspot kod kısımlarının, native kod'a çevrildiğinde saklandığı cache bölgesi.

  - __Garbage Collector Data__

# Different architectures on different JVMs
The IBM WebSphere JVM mimarisinde:
- old generation, __Tenured Space__ (kelime anlamı: imtiyazlı), 
- Young Generation, __Nursery space__ (kelime anlamı: kreş),

olarak kullanılıyor ve yapılarında ufak farklılıklar var.

"The layout of the generational heap (see Figure 2.9) is slightly different from that of the Oracle HotSpot JVM. The WebSphere nursery is equivalent to the HotSpot young generation, and the WebSphere tenured space is equivalent to the HotSpot old generation. The nursery is divided into two parts of equal size—allocate and survivor areas. Objects are always allocated in the nursery and copy garbage collection is used to copy surviving objects to the survivor area. After a successful GC cycle, the former survivor area becomes the nursery."

source: https://www.dynatrace.com/resources/ebooks/javabook/the-three-jvms/

# -XX:MetaspaceSize vs -XX:MaxMetaspaceSize
Default'ta Metaspace sınırsızdır. Fakat MaxMetaspaceSize buna limit getirebiliriz. MetaspaceSize ise şuna yarıyor: Eğer runtime'da Metaspace boyutu "MetaspaceSize'de belirttiğimiz sayıyı geçerse o zaman GC sıkça devreye girmeye başlıyor. Metaspace'te yer açmak için class unload işlemi yapılıyor.

# -Xms vs -Xmx
Eğer bu değerler verilmemiş ise; JVM default olarak bu değerleri verir. Her JVM şunlara göre default değerlere karar verir: OS, RAM ve JVM impelementasyonu farklı defualt değere karar verebilir.

Bu değerler Heap space'in size'ını belirler. -Xms (min size'ın baş harfleri) initial RAM değerini, -Xmx (max size'ın baş harfleri) maximum değeri belirler.

-XX:InitialRAMPercentage ve -XX:MaxRAMPercentage parametreleri -Xms ve -Xmx parametreleri ile tamamane aynı görevi görür. Sadece absolute değer (30GB gibi) değil, yüzdelik cinsinden değer (RAM'i %30'u gibi) ayarlayabilmemizi sağlamaktadır.

# commited memory
JVM'e bellek kullanımı için garanti edilen bellek miktarıdır. Bunun farkını net bir şekilde bulamadım. Fakat muhtemelen max manuel developer tarafından verilebilirken, commited OS tarafından verilen bir değerdir. Develoepr 100GB lik yer harcayabilsin diye parametre geçebilir (max değeri), fakat OS bunun tümünü atayamayabilir (commited değeri).

# Minor GC vs Major GC vs Full GC
These terms aren't clearly defined in the JVM specification. Here, we consider "Major GC" and "Full GC" to be equivalent.

Minor GC performs when Eden space is full. It removes all dead objects in young generation and moves live objects to from Eden space to s1 of survivor space, or from s1 to s2.

Full GC or major GC does garbage collection in the entire heap. Full GC can also collect parts like metaspace and direct memory, which can be cleaned only by full GC.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Apache commons
birçok Java kütüphanesi barındıran projenin ismidir. altındaki kütüphaneler birçok  amaç için yazılmıştır. sadece ihtiyaç duyulan kütüphanenin jar dosyası ayrı ayrı indirilmektedir. alt modüllerin bazıları:

- commons-codec: General encoding/decoding algorithms (example: phonetic, base64, URL).

- commons-lang: extra feature for java.lang (core Java library)

- commons-cli: command line argument passer

- commons-collection: datastructures library

- commons-dbcp: JDBC üzerinde pool'ların yönetimini yapmamızı sağlayan kütüphane. ORM kütüphanesi değildir.

# Google Guava
Apache commons gibi birçok farklı iş için geliştirilen Java kütüphaneleri grubudur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JNDI (or Java Naming and Directory Interface)
Java API'sidir.

property dosyalarından okurmuş gibi obje yaratabilmemizi sağlamaktadır:

```java
Context context = new InitialContext();

MyDataSource ds = (MyDataSource) context.lookup("service-name");
// genelde bu şekilde isimlendirilir: JDBC/UrDataSourceName
```

Yukarıdaki kodun çalışabilmesi için bu kod satırı çalışmadan önce uygun servis'in init edilmiş olması gerekmektedir:

```java
MyDataSource ds = new MyDataSource();

ds.setPassword("abc");

Context context = new InitialContext();

context.bind("service-name", ds);
```

JNDI en çok database bağlantıları ve LDAP gibi sistemler için kullanılıyor. Örneğin; Tomcat, jboss bize JNDI ile database'e bağlanabilmemiz için kendi data-source nesnesini döndürüyor. bu nesneyi kendi tanımlıyor, biz tüm deploy ettiğimiz Java uygulamalarından bu JNDI'ları okuyabiliyoruz. Okuduğumuzu bu JDNI objelerinden örneğin; database connection'ı açabiliyoruz.

MyDataSource objemiz istediğimiz custom metotları barındırabilir. örneğin; dbDataSOurce.getConnection(); gibi...

MyDataSource yerine herhangi bir EJB objemiz de olabilir. örnek:

```java
MyObject myObject = (MyObject) context.lookup("myObject");
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# modes of JVM
client ve server mod var. iki modda JIT kullanır. client mode, kodları işletirken çok çok az optimizasyon yapar. Bu da onun daha az RAM harcamasını ve ilk açılışının hızlı olmasını sağlar. Bu mode bu sebeple GUI uygulamalında tercih edilir. Oysa server mode,önceden execute edeceği kod bloklar için ön-inceleme yapıp, çok fazla optimizasyon yaptığı için, bu optimizasyon için RAM tüketir. Genelde uzun süre çalışacak server uygulamalarında bu mod tercih edilir.

yeni olarak "tiered compilation" modu gelmiştir. bu mod hem client hemde server karışımıdır. bu mod; uygulamanın başlangıcında client mod temelli çalışır, daha sonraki zamanlarda server mod temelli çalışır.

"java -version" komutu ile JVM'in çalıştığı mode çıkıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Garbage Collector
Yaptığı işlemlere teknik özel isimler verilmiştir. JVM parametresi geçmek gerektiğinde, yada GC'nin algortimaları araştırıldığında bu terimlere ihtiyaç duyulabilir.

# mark
scan and marking all reachable (kullanılan) objects

# sweep
scan and free unmarked objects

# copy collection
copying collection to another memory block (example: from eden to survivor space)

# paralel collection
GC uses multiple threads for itself

# concurrent collection
GC uses concurrent threads for itself

# compacting
moves objects inside memory for defrag

# stop the world
GC pause all app threads to finish it's job

# minor collection
garbage collection for young generation

# major collection
garbage collector for old generation

# Types of GC
GC kullandığı algoritmalara göre ayrılmaktadırlar:

- Serial collector
  - tek CPU'lu cihazlara uygun
  - GC işe başladığında tüm sistemi durdurur
- Parallel (throughput) collector
  - multiple CPU'lar için uygun
  - GC paralel thread'ler açarak işlem yapıyor
  - GC işe başladığında tüm sistemi durdurur
- concurrent (or CMS or concurrent-mark-sweep) collector
  - old generation için arkaplanda sürekli GC işlemi yapar. bazen sistemi durdurur bazen durdurmaz. bu mantığa low-pause collection deniliyor.
  - young generation için hep sistemi durduruyor
  - GC kendi içinde concurrent çalışmaktadır (paralel'den farklı mantıktır. başka başlıkta anlatıldı.)
- G1 (or garbage first) collector
  - CMS ile benzer mantıkta çalışmaktadır.
  - çok büyük heap size verilmiş sistemler için uygun.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JVM components

# byte code verifier

kodları okuyup JVM sürümüne uygunluğunu ve validation'ununu yapan mekanizmadır.

# class loader

class'ları bellekten okuyarak JVM'e aktarır. classloader'da bir Java sınıfıdır yada sınıflar grubudur. aynı JVM'de her app farklı classloader kullanabilir. Tomcat ve Spring'in kendi classloader'ları var.

- JVM'in içindeki default classloader'ın ismi "bootstrap class loader (or primordia class loader)"'dır. bunun asıl amacı /lib ($JAVA_HOME\lib\rt.jar ve diğerleri) dizinindeki core JRE kütüphanelerini import etmektir.

- "Extension Class Loader" ise bootstrap'in bir child'ıdır. bootstrap tarafından çağrılır. \lib\ext\ içini yükler.

- "Application Class Loader (or System Class Loader)" ise Extension tarafından çağrılır. Extension'ın child'dıdır. JVM başlatılırken, JVM'e parametre geçilen path'lerden class'ları yüklemekle görevlidir.

# execution engine

ya JIT yada interpreter'a iletilerek kodun çalıştırılmasını sağlar.

# garbage collector

boş referanslı verileri temizlemektedir.

# security manager

private metotları çağrılmasını engellemek gibi güvenlik duvarı görevi görmektedir. JVM'e parametre geçilerek herşey serbest bırakılabilmektedir.

# Tomcat classloader

Tomcat sırası ile bunları işletir:

- JVM's bootstrap classloader (with it's child-class loaders)

- System class loader

  $CLASSPATH variable'ında olan sınıfları yükler. fakat /tomcat/bin/catalina.sh or /tomcat\bin\catalina.bat script'i CLASSPATH değerini ignore eder ve CLASSPATH'e kendisi /tomcat/bin/ içerisindeki bazı lib'leri yükler.

- Common class loader

  Aşağıdakileri load eder:

  - unpacked classes and resources in $CATALINA_BASE/lib
  - unpacked classes and resources in $CATALINA_HOME/lib
  - JAR files in $CATALINA_BASE/lib
  - JAR files in $CATALINA_HOME/lib

- WebappX

  sadece bu classloader'dan yüklenen class'lar webapp'a özeldir. diğer web uygulamaları bunları göremez. bu dizinleri sırası ile yükler:

  - /WEB-INF/classes
  - /WEB-INF/lib

Tomcat'in loader konfigürasyonunda aşağıdaki belirtildikçe yukarıdaki sıra geçerlidir. eğer aşağıdaki belirtilmişse sıra aşağıdaki gibi olur:

```xml
<Loader delegate="true"/>
```

- bootstrap of JVM
- WebappX
- System
- Common

Tomcat'te default açık olmayan classloader'lar:

- Server: sadece Tomcat'in görebildiği kütüphaneler.
- Shared: tüm web uygulamalarından görülebilen kütüphaneler. buradakilerin tekrar yüklenmesi için Tomcat'in restart edilmesi gerekir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JVM parametreleri
JVM parametrelerinde -x ile başlayanlar her JVM de farklı implemente edilmiş olabilir. fakat x'siz başlayanlar her VM'de olmak zorundadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# exceptions

# try with resource
try parantezinin içinde olan BufferedReader java.lang.AutoCloseable'dan implemente eder. bu şekilde Java onu hata olursa otomatik kapatır.

```java
try( BufferedReader br =new BufferedReader(new FileReader(path)) ){

   return br.readLine();

} catch (Exception e) {
    //no need to close the "BufferedReader br"
}
```

# class hiyerarşisi

- Object
  - Throwable
    - Error
      - VirtualMachineError
        - InternalError
        - OutOfMemoryError
        - StackOverflowError
        - UnknownError
      - AssertionError
      - ThreadDeath
      - AnnotationFormatError
    - Exception
      - RuntimeException
        - NullPointerException
        - ArithmeticException
      - IOException
        - FileNotFoundException

- Tüm throwable nesneleri throw/catch edilebilirdir.

- Error ve altındaki objeler manuelde "throw new OutOfMemoryError()" şeklinde fırlatılabilirler. fakat catch edilmek zorunda dillerdir.

- Error'lar ne kadarda genel VirtualMachine sorunları gibi gözükselerde, Exception'lar gibi belirli bir satırdan fırlatılırlar. Örneğin OutOfMemoryError tam olarak bu satırdan fırlatılabilir:

```java
byte[] array1 = new byte[999999999]
```

İstersek OutOfMemoryError'yi catch edip, thread'e devam edebiliriz. Throw edilen satırdaki "array1" için bellekte yer ayırtılmamış olacaktır. Bu sebeple, hala memory'de boş alanımız var anlamına gelecektir.

JVM'e opsiyonel parametre geçerek, OutOfMemoryError fırlatılması yerine, JVM'in kill edilmesini sağlayabiliriz. Hatta JVM yine ek parametre alarak, OutOfMemoryError fırlatırldığında istediğimiz shell script'ini execute edebilmemize de olanak sağlıyor.

- Eğer herhangi bir thread'den exception catch edilmez ise, sadece o thread kill edilir.

# Checked exception vs unchecked exception
bazı Throwable'lar catch edilmek zorundadır. yoksa kod compile edilemez. catch edilmek zorunda olan Throwable'lar "checked"'dır.

RuntimeException ve Error ve bunların subclass'ları haricindeki tüm Throwable'lar checked'tır.

Eğer unchecked exception yaratmak istiyorsak, yeni exception'umuzu unchecked exception'lardan türetmeliyiz.

Checked exception'lar handle edilmesi beklnen durumlarda fırlatılmalıdır. İlk bakışta her exception handle edilmeli gibi düşünülebilir fakat önemli olan husus, hadle edilip yeni bir aksiyon alınabilecek mi? Örneğin NullPointerException, ArithmeticException checked değildir. Çünkü bu hatayı alan kişinin bir çözüme gitmesi beklenmez. ArithmeticException alan bir algoritma düşünelim, 0'a bölüm işlemi yaptığında hat aalıyorsa, farklı bir yoldan gitmesi beklenmez. Oysa dosyyaya yazma işleminde hata alınan bir durumda, permission'larla ilgili bir son kullanıcıdan yetki isteği yapabilir veya yetkisi olduğundan emin olunan geçici bir yere yazmayı deneyebilir gibi. Bu sebeple dosya yazmalardaki yetki işlemlerindeki hatalar (IOException) checked'tır.

# finally block on java

finally metotu her türlü çalışır. thread'in try veya exception bloğuna girmiş olması, hatta önceden "return" çağrılmış olması bir şey ifade etmez. örnek:

```java
public static void main(String[] args) {
  System.out.println(test1());
}

public static int test1() {
  try{
      System.out.println("try");
      return 1;
  }catch (Exception e){
      // any code here...
  }finally {
      System.out.println("finally");
      return -1;
  }
}
```

prints:

```
try
finally
-1
```

# best practices
- istisnalar dışında; sadece checked olan Throwable'lar fırlatılmalıdır. örneğin; ArithmeticException (uncheckted) fırlatılmamalıdır. çünkü zaten unchecked exception'lar JVM tarafından fırlatılıyor. Tekrar yazmanın bir anlamı yok. kod kalabalığı yaratırız. örnek:

  ```java

  if( a == 0){
    throw new ArithmeticException();
  }
  return b/a;
  ```

  Yukarıdaki kod sadece kod kalabalığı yaratmaktadır. zira arithmetic exception bu metotu çağıran yazılımcı tarafından yakalanmak zorunda değildir.

- her çeşit exception ayrı ayrı fırlatılmalıdır. hepsi gruplanıp fırlatılmamalıdır. böylece yakalan metot ne olduğu bilinir ve her exception çeşidi için ayrı ayrı ne yapacağına karar verebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java Specification Requests (or JSR)

Java ve JavaEE için toplulukların yayımladığı spesifikasyon yayımlarıdır.

# JDK Enhancement Proposal (or JEP)

OpenJDK için toplulukların yayımladığı spesifikasyon yayımlarıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# @transient
Core Java annotation'ıdır. serialize edilirken bu objenin esgeçilmesini sağlar. Bunu ignore edip etmemek kütüphanenein kendisine kalmıştır. Örneğin;
- GSON ve diğer birçok serialize etme kütüphaneleri bu değeri kullanır.
- ORM kütüphaneleri bu field'ı DB'ye yazmaz, ve entity DB'den okunduğunda buraya r mapping yapmaz. Genelde bu field'a ait getter fonksiyonu logic içerir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# remote debug

Java uygulaması çalıştırıldığında debug parametresi ile başlatılır ise; JVM bir port üzerinden "debugger" uygulamasının kendisine bağlanmasına izin verir. debugger uygulaması genelde bir IDE'dir. IDE uygulamaya bağlanır. IDE üzerinde önceden proje tanımlanmıştır, bu şekilde source code ile Java uygulaması arasında ilişki kurulabilir.

bir uygulamayı lokalde IDE ile debug ettiğimizde, IDE arkaplanda bu işlemi yapar fakat yazılımcıya fark ettirmez. remote=localhost olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java Serializable

bir sınıfın içindeki anlık değerler ile byte olarak kaydedilmesidir. De-Serialization işlemi ise byte dizisinden Java sınıfına aktarılmasıdır. byte'a çevrilecek sınıf  Serializable sınıfının implemente etmek zorundadır.

# serialVersionUID

bu değer Serializable'dan implemente edilmiş sınıflarda olmalıdır. bu değer byte'a çevrilen sınıfın içine gömülür. byte dizisinden De-Serialization yapıldığında eğer sınıfın serialVersionUID'si aynı ise De-Serialization uyumlu bulduğu tüm değerleri sınıf değerlerine doldurur. eğer serialVersionUID farklı ise, direk olarak hata alınacaktır. bu sebeple yazılımı geliştirirken serialVersionUID'leri manuel atamakta yarar var. aksi durumda derleme sırasında bu değerler rastgele atanacak ve dosyadan De-Serialization işlemlerinde aynı serialVersionUID olmayacağı için, yeni alan eklenmiş sınıflara dahi De-Serialization olumlu sonuçlanmayacaktır.

# Eclipse'in sunduğu otomatik serialVersionUID seçenekleri

1- default serialVersionUID: 1L (long tipinde 1 değeri) atanır. serialVersionUID değeri önemsiz olan sınıflara atılması için önerilir.

2- oto generated serialVersionUID: sınıfın içindeki tüm değişkenlere bakılır. tüm bu değerlere göre bir hash üretilir. bu hash ile serialVersionUID oluşturulur. bu algoritma ile aynı aynı olan tüm sınıfların serialVersionUID'leri aynı olacaktır. bu şekilde başka bir yazılım tarafından okunan serialize edilmiş veri başarılı şekilde okunabilecektir. Java sınıfımızda bir değişiklik yaptığımızda serialVersionUID otomatik değişecektir. artık modelimiz serialize edilmiş dosyadan okuyamayacaktır. zaten beklenen de budur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# inheritance of class fields
bu Java'da mümkün değil. örnek üzerinden gidelim:

```java
class Animal {
    
    String name = "unknown";
    
    String run(){
        return "Animal is running";
    }
}

class Dog extends Animal {
    
    String name = "dog";
    
    String run(){
        return "Dog is running";
    }
}

class HelloWorld {
    public static void main(String[] args) {
        Animal instance1 = new Dog();
        // prints: unknown & Dog is running
        System.out.println(instance1.name + " & " + instance1.run());
        
        Dog instance2 = new Dog();
        // prints: dog & Dog is running
        System.out.println(instance2.name + " & " + instance2.run());
    }
}
```

Super class'taki bir field'ı override edemeyiz. Zaten etmemeliyiz de. Çünkü super class o filed'ı kendi içinde tamamen özel bir logic için kullanıyor olabilir. Eğer biz o field'a erişmek istiyorsak, "super.name" veya getter-setter metodları aracılığı ile erişmemeiz daha doğrudur. Inheritance'ta sadece davranışları override etmeliyiz, yani metodları.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java SE (or Java standart edition or old-name:J2SE) vs Java EE (or Jakarta EE or Java enterprise edition or Java 2 Platforms or Java Platform or old-name:j2ee)

ortalama bir desktop bilgisayar ihtiyaçlarını karşılayan platformlar için Java-runtime içinde bulunan kütüphaneler JavaSE grubudur.

JavaEE, JavaSE'yi kullanan ekstra kütüphaneler içerir. runtime sırasında hangi kütüphanelerin olup olmayacağına çalışan uygulama (sunucu) belirler. ek kütüphaneler JAR'ın içinde yada classpath'te bulunmalıdır. bu kütüphaneleri barındıran sunucular Glashfish, Weblogic'tir.

# sürümler

JRE'nin sürümleri ile JDK aynı olmalıdır (or geriye uyumlu modda çalışılır). fakat JavaEE'nin sürümleri JRE'den bağımsızdır. Fakat JavaEE JRE ile aynı zaman dilimlerinde aynı sürüm numaralarını kullanmaya çalışır. bu tamamen isteğe bağlı yapılan bir seçimdir.

JavaSE version history:

(eskiden JDK ile bütünleşik geliyordu, bu sebeple ilk isimleri JDK olarak geçmektedir)

- JDK 1.0 (1996)
- JDK 1.1 (1997)
- J2SE 1.2 (1998)
- J2SE 1.3 (2000)
- J2SE 1.4 (2002)
- J2SE 5.0 (2004)
- Java SE 6 (2006)
- Java SE 7 (2011)
- Java SE 8 (2014)
- Java SE 9 (2017)
- Java SE 10 (2018)
- Java SE 11 (2018)
- Java SE 12 (2019)
- Java SE 13 (2019)
- Java SE 14 (2020)
- Java SE 15 (2020)
- Java SE 16 (2021)

JavaEE version history

- 2EE 1.2 (1999)
- J2EE 1.3 (2001)
- J2EE 1.4 (2003)
- Java EE 5 (2006)
- Java EE 6 (2009)
- Java EE 7 (2013)
- Java EE 8 (2017)

# JDK (or Java development kit)
JDK içinde JavaEE kütüphaneleri bulunmaz.

JDK içinde; Wsimport (web service yaratma tool'u), Keytool (sertifika import/export tool'u) gibi birçok utility + JRE + compile tool'ları bulunur.

# Oracle JDK (or old-name:Sun JDK) vs OpenJDK

(not: JDK'lar kendi içlerinde kendi JVM'lerini içermektedirler.)

OpenJDK, JavaSE'nin 7'inci sürümü ile artık referans implementasyon olmuştur. yani sadece desktop makinalar için JVM implementasyonu olarak OpenJDK gösterilir. yani; artık Oracle JRE'si OpenJDK üzerine kuruludur. eskiden de OpenJDK ve Oracle JDK birbirleri ile ortak kodları kullanarak ilerliyorlardı, fakat 7inci sürüm ile artık kesin olarak Oracle JDK, OpenJDK implementasyonu olacağı garantisi verilmiştir.

Oracle JDK ile OpenJDK arasındaki farklar şunlardır:

- Oracle JDK'nın lisansı farklıdır

- Java Plugin and Java WebStart

- JRockit Mission Control (detaylar başka yerde yazıyor)

- font'lar

- bazı ek kütüphaneler

- Oracle JDK 11 ve JVM 11 artık ücretli olmuştur. her canlıya atılan paket için Oracle'a ücret ödenmelidir.

OpenJDK aslında sadece source code'u temsil etmek için kullanılan bir terimdir. AdoptOpenJDK; OpenJDK'yı manipüle edip derlemektedir. aşağıda farklı başlıkta detaylı tablosu mevcut.

# IcedTea

OpenJDK'yı geliştiren community tüm OS'ları desteklememektedirler. bu sebeple farklı bir topluluk OpenJDK'yı diğer OS'lara entegre eder. yani OpenJDK'yı farklı platformlar için build eder.

# IcedTea-Web

OpenJDK'da olmayan bir ek modüldür. bu modül web tarayıcıları için Java eklentisi sunar. Java-plugin'e açık kaynaklı alternatiftir.

# Eclipse Enterprise for Java (or EE4J)

JavaEE Oracle tarafından geliştiriliyordu. 2017'de Oracle Eclipse vakfına geliştirmeyi bıraktı. Eclipse ise JavaEE'yi, JakartaEE olarak değiştirdi. JakartaEE ve onunla ilişkili diğer tüm projeler için root proje olarak EE4J ismi verildi.

# WebStart vs Java plugin

Java plugin; web tarayıcıya kurulan eklentinin ismidir.

WebStart; Applet'lerin bir sonraki adımı gibi düşünülebilir. local siteme uygulama (Applet benzeri ufak uygulama) indirme, çevirimdışı daha sonra açmak için işletim sistemine kısayol atama, kurulan uygulamaları yönetebilme, sürüm güncelleme gibi işlevlerin yapılmasını sağlar. bu uygulamalar tarayıcıdan direk olarak kurulabilir. bu uygulamacıklarda izinler çok kısıtlıdır.

# Java Applet

Java runtime'a ihtiyaç duyar. client tarafta tarayıcı üzerinden Java-plugin aracılığı ile uygulama çalıştırır.

# JavaFX

masaüstü uygulaması yazılmasını sağlıyor. ek olarak webview kullanımını kolaylaştırmıştır. WebView içerisini düzenleme ve üzerinde farklı syntax'lar ile düzenlemeler yapma özellikleri (entegrasyon) sunmaktadır. WebView içerdiğinden, birçok GUI elementi HTML elementi olacağından, Java'daki diğer GUI kütüphanelerine göre daha cross-platform olduğu söylenebilir.

# Hotspot

JVM spesifikasyonunu implemente eden birçok JVM mevcuttur. Oracle'ın resmi JVM'inin ismi "Hotspot"'tur. piyasada onlarca JVM implementasyonu mevcut. örnek: JRockit(Java 7 döneminde geliştirilmesi durduruldu), IBM J9, OpenJDK...

Oracle'ın JVM ismi olan Hotspot, içerdiği Hotspot teknolojisinden gelmektedir. sun JVM, Oracle tarafından satın alınmadan önce JRockit olarak adlandırılıyor.

Hotspot ilk bu isimle çıktığında, özel bir teknoloji barındırıyordu. bu teknoloji; sık kullanılan kod bloklarını, daha hızlı erişebileceği bir bellekte tutmakta ve o kod blokları için optimizasyonu arttırmaktadır. kodlar hakkında istatistikler tutmaktadır. bu istatistikler sonucunda;

- sık kullanılan kod blokları native makine koduna çevrili şekilde hazır tutulmaktadır

- gereksiz bazı kod blokları yürütülmemektedir

- bazı kod bloklarındaki sık girilen if-blokları başa taşınmakta, az kullanılan if-condition'ları alta taşınmaktadır

- ve daha birçok teknik kullanılmaktadır

"Hotspot" kelimesi sık kullanılan bölgeleri (sıcak bölgeleri/noktaları) belirtmek için kullanılan genel bir terimdir. JVM'in ismi de buradan gelmektedir.

Java dili eskiden interpreter gerektiren bir dildi. Artık güncel sürümlerde Hotspot teknolojisinin gelişmesi ile bazı kod blokları, native kod olarak saklandığı için, tekrar interpreter tarafından işletilmiyor. direk olarak makinada run ediliyor (native app gibi). dolayısı ile yeni JVM sürümleri yarı interpreter yarı native kod çalıştırır durumdadır.

java -version ile komut satırından bilgi alındığında "mixed mode" çıktısı; JIT'in de kullanıldığı anlamına gelmektedir. isteğe bağlı bu özellik devre dışı bırakılabilmektedir.

# native lib

Java Hotspot ile sık girilen kod bloklarını native makine koduna çevirip saklamaktadır. fakat önceden yazılmış default kütüphaneler (JVM içerisinde default gelen sınıflar) önceden makine koduna derlenmiş saklanabilmektedirler. bunlara native lib denmektedir.

Nativelib içerisindeki kütüphaneler işlemcilerin özelliklerinden de yararlanmaktadırlar. örneğin System.arraycopy() metotu array kopyalamayı işlemcinin özelliğinden yararlanarak yapar. bu tarz metotlardan faydalanmak uygulama hızını çok etkilemektedir.

# Technology Compatibility Kit (or TCK)
JVM olabilmek için belirlenmiş standartlardır. Tüm JVM'ler TCK'da belirtilen kurallara uymak zorundadırlar.

# OpenJDK builds
OpenJDK stabil olsa da, production-ready build'leri resmi olarak süreklilik garantisi verilerek dağıtılmamaktadır. dolayısı ile production-ready değildir. fakat production ortamında kullananlar vardır. bu sebeple; open-JDK kullanmak istemeyenler farklı JVM implementasyonları kullanırlar. zaten neredeyse tüm JVM'ler OpenJDK'yı referans alır ve üzerine eklemeler/çıkarmalar yapılarak dağıtılırlar.

- OpenJDK, Oracle tarafından geliştirildiği için OpenJDK'nın sitesinde https://OpenJDK.java.net/install/ indirme linkleri Oracle JDK'yı indirir. çünkü OpenJDK türevi olan production-ready örneği olarak Oracle kendini gösterir. dolayısı ile download linkleri buraya yönlenir: https://www.oracle.com/technetwork/java/javase/downloads/index.html

- OpenJDK build'leri ise buradan indirilebilir: https://JDK.java.net/archive/

- https://adoptopenjdk.Net projesi OpenJDK'yı direk olarak saf haliyle derleyip sunmaktadır. projenin arkasında güçlü destekçiler vardır.

- Ubuntu "apt install OpenJDK-8-JRE" komutu ile OpenJDK'yı kurar.

# Oracle JDK vs AdoptOpenJDK

detaylı bilgiye buradan ulaşılabilir: (source-id: 445)

| Oracle JDK 8 proprietary component         | Alternative component               | OpenJDK 8         | OpenJDK 11        |
|--------------------------------------------|-------------------------------------|-------------------|-------------------|
| Java Web Start                             | IcedTea-Web                         | yes               | no                |
| JavaFX                                     | OpenJFX                             | no                | no (coming soon)  |
| T2K font rendering engine                  | Freetype                            | yes               | yes               |
| Monotype Lucida fonts                      | Relicensed Lucida fonts             | no (coming soon)  | no (coming soon)  |
| Ductus 2D renderer                         | Pisces/Marlin                       | yes (Pisces)      | yes (Marlin)      |
| Kodac Color Matching System (KCMS) library | LCMS                                | yes               | yes               |
| SNMP                                       | Use JMX (or SNMP4J)                 | yes (not bundled) | yes (not bundled) |
| Sound drivers                              | Use Microsoft Windows sound drivers | yes (not bundled) | yes (not bundled) |
| Java Flight Recorder (JFR)                 | Java Flight Recorder                | no (coming soon)  | yes               |
| Java Mission Control (JMC)                 | Use JDK Mission Control             | no (coming soon)  | no (coming soon)  |

Not: JavaFX, JFR, and JMC, Java 8 ile Oracle tarafından OpenJDK projesine dahil edildi.

# JVM implementations
- OracleJDK - company:Oracle
- Redhat OpenJDK - company:Redhat
- Corretto - company:Amazon
- Zulu - company:Azul
- Zing - company:Azul (commercial of Zulu with it's own custom Garbage Collector and compiler)
- Semeru - company:IBM (it based on OpenJ9 - OpenJ9 is developing only by IBM)
- Temurin - company:Eclipse (project name: Adoptium)
- AdoptJDK - company:community based - (there were 2 builds: HotSpot and OpenJ9) (discontinued and moved to Temurin)
- Liberica - company:BellSoft

Burada bu konu hakkında güzel bilgiler yer almaktadır: https://github.com/whichjdk/whichjdk.com

# OpenJ9
OpenJDK ve Oracle'da gelen HotSpot teknolojisine alternatif bir yapıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java 8 news

- lambda expressions + function interface (konular başka başlıkta anlatılıyor)

- stream

- JavaFX tümüyle JDK içinde entegre gelmektedir.

- Nashorn (konu başka başlıkta anlatılıyor)

- "Optional" sınıfı geldi (konu başka başlıkta anlatılıyor)

- Java Misson Control: Java yazılımlarınızı kontrol edebileceğiniz bir tool. Mesela programınız ne kadar CPU, hafıza ve thread kullanıyor.

- Permgen politikası değiştirildi

- new Date-Time API. "java.time" isimli yeni kütüphane paketi eklendi. eski sürümlerde hem java.util.Calendar hemde java.util.Date kullanılıyordu. hala JVM'lerde geriye uyumlu çalışabilmesi için kaldırılmadılar.

- default keyword: arayüzde metot önüne "default" keyword'ü atılıp o metotun body'si yazılabiliyor. Eğer yazılırsa artık implementasyonlarda o metot override edilse bile arayüzdeki metot çağrılıyor. Yani abstract sınıftakilerden farklı bir durum var.

  Burada Java zorunlu implementasyon yaptırmak istemiyordu. sadece arayüzde metot implemente edilmesi isteniyordu. fakat zorunlu implementasyon geriye uyumluluğu sağlamak için eklendi. şu sebepten:  örnek üzerinden gidelim: List arayüzüne bir metot eklenmek isteniyor. bu metotu JDK eklerse tüm güncel yazılımlar artık kullanılamaz hale gelecektir. çünkü tüm yazılımdaki list implementasyonları çalışmaz olacaktır. çünkü arayüzdeki tüm metotları implemente etmek zorundalar. fakat artık implemente etme zorunlulukları olmadığı için bu metotlar sorunsuz çalışıyor olacaktır.

- java.util.Base64 ile base64 işlemleri birçok implementasyon destekleyecek şekilde tasarlandı (başka yerde anlatılıyor)

# Java 9 news

- Java REPL (or JSHELL or Java Shell). 'Kulla' projesi altında geliştiriliyor.

- modülerlik. 'Jigsaw' isimli projesidir. artık her jar, kendi descriptor'unda hangi paketleri dışarıya açılacağını (dışarıdan görülebilir olacağını) ve hangi modüllere depend ettiğini belirtebilmektedir.

- Process API geliştirmeleri. alt-process'leri yönetirken artık ok daha fazla API mevcuttur.

- Factory Methods for Collections. artık factory metotlar mevcut. örnek:

  > Set<Integer> intList = Set.of(1, 2, 3);

  > List<String> stringList = List.of("1", "2");

- Reactive Streams (başka yerde anlatılıyor)

- Optional ve Stream API'lerindeki gelişmeler

- \@Deprecated (forRemoval=true , since="9") ile artık hangi versiyondan sonra bu metotun var olmayacağını dökümante edebiliyoruz.

- HTTP/2 ve WebSocket geldi. Eski sürümlerde üçüncü parti kütüphanelerle kullanılıyordu. JavaEE ve Spring kendi içinde üçüncü parti kütüphaneleri gömülü getiriyor ve onları kullanıyordu. bu sebeple eski sürüm Java kullanılsa bile, JavaEE veya Spring kullanarak WebSocket ve HTTP2 kullanılabiliyordu.

  Aslında bu modül incubator olarak Java 9 ile geldi. kaynak: (source-id: 354) "2. Initial Setup" başlığı.

  incubator'dan çıkışı 11'inci sürüm oldu. kaynak: (source-id: 355) ilk cümle.

# Java 10 news

- type safe olmayan nesneler tutulabiliyor:

  > var list = new ArrayList<String>(); 

# Java 11 news

- yeni http-client incubator olmaktan çıktı ve en stabil sürümüne ulaştı.

- sadece 1 java dosyasını hzılıca komut satırından derlemeden run edebiliyoruz. (bu mimarisel bir değişiklik değil. basit bir değişiklik. JDK, sadece arkaplanda derlemeyi yapıp class dosyasını RAM'de tutuyor).

# Java 12 news

- "10K", "10 thousand" gibi gösterimler yapabilmek için Number-formatter geldi (date-time-formatter gibi).

- yeni switch-case syntax'ı eklendi. yeni syntax, birden fazla enum'ın aynı işlevi run ediyor ise, ayrı ayrı "case" yazmamızın gereksinimini ortadan kaldırıyor.

# Java 13 news

- switch-case bloklarının dönüş değerleri artık herhangi bir variable'a assign edilebiliyor.

- escape karakteri kullanımına gerek olmadan String oluşturulabiliyor:

```java
// Bu syntax çok yeni olduğu için şu anda ekranda yanlış renklendime ile gösteriliyor olabilir.

String jsonString = """
{
    "name" : "jack",
    "website" : "https://www.mycompany.com/"
}
""";
```

- Dynamic CDS Archives (JEP 350): Farklı JVM instance'ları startup'ı hızlandırmak için derlenmiş kodların metadatalarını zaten ortak kullanabiliyordu. Bu sürümde bu ortak metadataları portable hale getirildi.

- Reimplement the Legacy Socket API (JEP 353): aynı soket API'si komple yeniden yazıldı.

# Java 14 news

- Bizim için otomatik getter, constructor ve to-string yaratılıyor:

```java
public record User(int id, String password) { };
```

- null-pointer hataları artık daha detaylı. aynı satırda birçok instance olduğunda, hangis instance'ın null olduğunu da yazıyor.

- artık sadece ihtiyacımız olan modülleri içine alan, OS bazlı executable üretebiliyoruz.

- instanceof'ta opsiyonel syntax değişikliği oldu.

eski Java ile kod:

```java
if (person instanceof Employee) {
    Employee employee = (Employee) person;
    Date hireDate = employee.getHireDate();
    //...
}
```

yeni Java ile kod:

```java
if (person instanceof Employee employee) {
    Date hireDate = employee.getHireDate();
    //...
}
```

# Java 15 news

- record'larda artık constrcutor'ı override edebiliyoruz.

```java
public record Person(String name, int age) {
    public Person {
        if(age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
}
```

- Service sınıfının sadece Tcp ve Http tarafından super class olabilecğeini belirtmiş oluyoruz.

```java
public sealed interface Service permits Tcp, Http {

    // any code here
}
```

- hidden class: class-loader ile bulunamayan bir class yaratılabiliyor. bu class'ı sadece ilgili thread yaratıyor ve o thread o anda kullanılabiliyor. bu özellikle framework'ler için iyi. tamamen private class yaratılabilmesini sağlıyor. eskiden private class yaratsak bile, reflection veya class-loader'lar aracılığı ile erişme ihtimalimizi olabiliyordu. şu anda o ihtimal tamamen kalktı.

- instanceof'ta opsiyonel syntax değişti. artık if condition'ı da yazabiliyoruz:

```java
if (person instanceof Employee employee && employee.getYearsOfService() > 5) {
    //...
}
```

- 8'inci sürüm ile gelen Nashorn JavaScript engine, GraalVM gibi teknolojilerin çıkması ile, Java 15 ile kaldırıldı.

# Java 16 news

önemli bir değişiklik yok.

# Java 17 news

- switch case'lere opsiyonel syntax eklendi:

```java
static record Human (String name, int age, String profession) {}

public String checkObject(Object obj) {
    return switch (obj) {
        case Human h -> "Name: %s, age: %s and profession: %s".formatted(h.name(), h.age(), h.profession());
        case Circle c -> "This is a circle";
        case Shape s -> "It is just a shape";
        case null -> "It is null";
        default -> "It is an object";
    };
}

public String checkShape(Shape shape) {
    return switch (shape) {
        case Triangle t && (t.getNumberOfSides() != 3) -> "This is a weird triangle";
        case Circle c && (c.getNumberOfSides() != 0) -> "This is a weird circle";
        default -> "Just a normal shape";
    };
}
```

# Java 18 news

- simple web server. it's not production ready. its only debug/test purpose.

run it via command line:

```sh
jwebserver -b 0.0.0.0 -p 8000
```

or via API:

```java
var server = SimpleFileServer.createFileServer(new InetSocketAddress(8000), Path.of("/home/java"), OutputLevel.VERBOSE);

server.start();
```

- UTF-8 character set by default. Before 18, JVM set locale of OS.

- Internet-Address Resolution SPI: artık domain'den IP bulmaya çalıştığımızda, OS'un native API'sini kullanmak zorunda değiliz. customda kullanabileceğiz.

- switch case'lere opsiyonel özellik geldi. artık tiplere göre case'ler işletilebiliyor:

```java
static int coverage(Object o) {
    return switch (o) {
        case String s  -> s.length();
        case Integer i -> i;
        default -> 0;
    };
}
```

- Java.lang.Object.finalize() metodu deprecated olarak işaretlendi.

genelde şu şekilde bir yazım vardı:

```java
// developer'lar genelde bu kod sattırını Java.lang.Object.finalize() metodunda yapıyor.
if (output != null) output.close();
if (input != null) input.close();
```

Oysa output.close() exception fırlatır ise input close edilmez. İşte bu sebeptendir ki şu şekilde her kaynağı zaten nerede açtıysak orada yönetmeliyiz:

```java
try (FileInputStream input = new FileInputStream(file1);
     FileOutputStream output = new FileOutputStream(file2)) {
        
      // copy files from file1 to file 2
}
```

# modülerlik hakkında

- Java 9 ile artık "java.se" modülü JRE içindeki kütüphaneleri barındırıyor.
- Java 9 ile java.se artık JAXB gibi JavaEE'ye ait olduğu düşünülen kütüphaneleri deprecated olarak işaretlendi.
- Java 9 ile JAXB gibi JavaEE'ye ait olduğu düşünülen kütüphaneler default classpath'te gelmiyorlar fakat JDK içerisinde yüklü geliyorlar. Yani development yaparken ilgili jar'ları hala bulabiliyoruz fakat projemizde ufak config yapmak gerekiyor.
- JDK 11 ile JAXB gibi JavaEE'nin olduğu düşünülen paketler JDK içinden tamamen kaldırıldı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Look And Feel
sadece tema değil, aynı zamanda efektler ve objeler davranış biçimlerini de kapsayan bir "UI" terimidir. Sadece "tema" terimi kullanıldığında son kullanıcının UI'a karşı tüm hissiyatını kapsamamaktadır. Bu sebeple "Look And Feel" terimi daha geniş bir terimdir.

# AWT (or Abstract Microsoft Windows Toolkit) vs SWING
AWT, OS'un native GUI objelerini kullanır. Fakat hepsini ortak bir Interface üzerinden yönettiği için, sadece ortak özellikler kullanılabilmektedir. AWT her platformda farklı göründüğünden, "Look And Feel" aynı olmamaktadır.

SWING ise, pixel pixel ekrana kendi GUI objelerini basmaktadır ve tüm platformlarda aynı görünüme ve davranışlara sahiptir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTTP clients for java
Client'lar sundukları özelliklere göre değerlendirilir. genel olarak 3'ye gruplayabiliriz (abstraction level):
- Response ve request'leri mapping yapanlar (high level)
- Response ve request'leri mapping yapmayanlar (low level).
- bazı client'lar ise (aşağıdaki listede "very high" olarak belirtildi) diğer tüm decoder/encoder'ları, HTTP client'ları wrap etmemizi sağlıyor. örneğin Feign-client bu kategoride ve bu şekilde setup'ı yapılabilir:

```java
BookClient bookClient = Feign.builder()
  .client(new OkHttpClient())
  .encoder(new GsonEncoder())
  .decoder(new GsonDecoder())
  .logger(new Slf4jLogger(BookClient.class))
  .logLevel(Logger.Level.FULL)
  .target(BookClient.class, "http://localhost:8081/API/books");
```

Yukarıdaki builder'a geçebileceğimiz parametreler iyi bir görsel ile burada gösterilmiştir: (source-id: 356). Görseldeki bazı kısımlar buraya kopyalandı:

- clients
  - java.net.URL
  - Apache http
  - Apache HC5
  - Google HTTP
  - Java v11 HTTP2
  - OKhttp
  - Ribbon
- async clients
  - java.net.URL
  - Apache HC5
- contracts
  - Feign
  - JAX-RS
  - JAX-RS 2
  - Spring 4
  - SOAP
  - Spring boot (3rd party)
- encoders/decoders
  - GSON
  - Jackson 1
  - Jackson 2
  - Jackson JAXB
  - Sax
- metrics
  - Dropwizard metrics 5
  - Micrometer
- extras
  - Hystrix
  - SL4J
  - Mock

| name                             | abstraction level | comment                                                                                                                                                                                                                                                                                           |
|----------------------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Apache commons HttpClient        | low               | Proje sonlandı. kaynak: (source-id: 357)                                                                                                                                                                                                                                                          |
| Apache HttpComponents HttpClient | low               | "Apache common HttpClient" has been replaced by the Apache HttpComponents project in its HttpClient and HttpCore modules                                                                                                                                                                          |
| java.net.http.HttpClient         | low               | Java 10+ ile geldi. dependency istenmeyen projelerde tercih edilebilir. Java da her zaman java.net.HttpURLConnection API'si vardı. Fakat bu API yeni HTTP sürümlerini desteklemiyor ve API'leri yeni metotolojileri desteklemiyordu ve async çalışmıyordu.                                        |
| Google-http-Java-client          | high              | Google'ın kendi Android için kütüphanelerinde kulanmak amaçlı geliştirdiği library                                                                                                                                                                                                                |
| async-http-client                | ?                 | Proje sonlandı. kaynak: (source-id: 358)                                                                                                                                                                                                                                                          |
| RestTemplate                     | high              | java.net.HttpURLConnection (Java 11 öncesinde var olan API) tabanlı. Yakın zamanda deprecated olacak. çünkü sadece sync işlemleri destekliyor. Spring, yeni olarak org.springframework.web.reactive.client.WebClient'ı çıkardı. WebClient hem sync hem async destekliyor. kaynak: (source-id: 359) |
| FeignClient                      | very high         | Declarative olarak client tanımlamamızı sağlıyor (Interface-based implementation). kendi içinde java.net.HttpURLConnection (Java 11 öncesinde var olan API) ve reflection'lardan yararlanıyor. kaynak: (source-id: 360)                                                                            |
| Retrofit                         | very high         | Feign client gibi Declerative client oluşturabilmemizi sağlıyor. Feign-client ile karşılaştırıldığında Retrofit daha az kütüphaneyi/özelliği wrap edebiliyor.                                                                                                                                     |
| OKHttp                           | ?                 | ?                                                                                                                                                                                                                                                                                                 |
| Unirest                          | ?                 | ?                                                                                                                                                                                                                                                                                                 |

# jax-ws

JavaEE de SOAP web servisi spesifikasyonudur.

# JAX-RS

JavaEE'nin REST spesifikasyonudur. Sadece API'dir. arayüzleri aşağıdaki dependency ile projemize ekleyebiliriz:

```xml
<dependency>
    <groupId>javax</groupId>
    <artifactId>javaee-API</artifactId>
    <version>7.0</version>
    <scope>provided</scope>
</dependency>
```

Runtime sırasında ise JavaEE implementasyonumuz (server'ımız örnek: tomee) bunu implemente eden paketi sunacaktır.

# jax-ws vs JAX-RS implementasyonları

JavaEE'de default implementasyon kavramı yoktur. hangi sunucuya atarsak jboss, Tomcat kendi implementasyonunu sunar. üzerinde çalıştırdığımız sunucununkinden farklı implementasyon istersek, biz spesifik jar'ları yükleyip configürasyon yapmak gerekecektir.

| name       | jax-ws | JAX-RS | Installed Default                                  |
|------------|--------|--------|----------------------------------------------------|
| Apache CXF | yes    | yes    | TomEE, WebSphere                                   |
| Axis       | yes    | no     |                                                    |
| Axis2      | yes    | yes    |                                                    |
| Jersey     | no     | yes    | GlassFish Server, Payara Server                    |
| RESTEasy   | no     | yes    | JBoss EAP (Enterprise Application Server), WildFly |
| Restlet    | yes    | ?      |                                                    |

Spring framework, Spring MVC modelinde bu hizmeti sunmaktadır. JAX-RS implementasyonu değildir. Spring'in aynı modülü model-view-controller özelliğini de sunmaktadır.

Yukarıdaki bazı implementasyonlar (bunlardan biri RESTEasy) hem server hem de client tarafını da sunmaktadır.

Spring ile Spring-MVC kullanılmak zorunda değiliz. alternatifleri de Spring ile kullanabiliriz. örnek: Spring-boot-starter-jersey. kaynak: (source-id: 448) "7.3. JAX-RS and Jersey" başlığı.

# wsimport
JDK içinde yüklü gelen komut satırı uygulaması. WSDL'den, SOAP web service client Java kodu generate etmemize yarıyor.

# Eclipse GUI client generator
elipse IDE'de herhangi bir projeye sağ tıklayıp, New --> Web Service Client --> seçeneğine tıklayıp WSDL'i veriyoruz bize Java kodu üretiyor. burada Eclipse IDE arkaplanda Axis'in generator'ını kullanıyor.

# Axis vs Axis2
Axis'te büyük değişiklikler yapıldı ve yeni proje olarak fork edildi. artık REST'te destekliyor. Axis2 projenin yeni adıdır. Axis2'nin yanına sürüm numarasını gelecektir.

# RPC (or Remote procedure call)

Uzak makinede yada aynı makinede farklı process'ler arasında metot çağırıp işletme mekanizmasıdır. bu bir consept'tir. protokol değildir. herkes uzaktaki bir metotu çağırıp dönüş alabilir. genelde programlama dilleri kendi protokollerini/standartlarını kullanıyor. bu sebeple cros-language değillerdir.

# Remote Method Invocation (or RMI)

Java'nın kullandığı RPC'dir.

# XML-RPC

bir çeşit RPC standardıdır.

# WSDL içinde geçen terimler

WSDL 2.0 sürümü HTTP isteklerini kısmen desteklemektedir.

aşağıdaki her tanımın "name" isminde property'si mutlaka olmalıdır.

tüm WSDL XML'inin root elementi < definition > etiketindedir.

| 1.1       | 2.0       | AÇIKLAMA                                                                                                                                        |
|-----------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| port      | Endpoint  | gidilecek olan isteğin URL'sidir (path ile birlikte)                                                                                            |
| service   | service   | her endpoint'i birer binding'le eşleştiren mapping'dir. örnek: X servisi A binding'i ile eşleşir.                                               |
| binding   | binding   | operation'lar kümesidir. + istek hakkında meta bilgiler içerir                                                                                  |
| portType  | Interface | operation'lar kümesidir. + her operation içinde gidip gelecek olan DTO'ları da tanımlamaktadır. + error durumunda dönecek DTO tanımı buradadır. |
| operation | operation | her metot bir operation'dur.                                                                                                                    |
| message   | (yok)     | bir request yada response'un tüm DTO'dur. her message bir type kümesidir. message etiketi içinde her type "part" isimli etiket ile tanımlanır.  |
| types     | types     | her DTO bir type'tır. type tüm request yada response olmak zorunda değildir. bir kısmı da olabilir.                                             |

# Subset WSDL (or SWSDL)

sadece bir metot kümesini kapsar. SWSDL alınıp client üretilirse, SWSDL içindeki metotlar çağrılabilir.

# marshalling

marshalling işlemi bir nesnenin farklı bir makineye parametre olarak geçilebilmesi için byte dizisi olarak (dosyaya yazılabilir halde oluyor doğal olarak) taşınması işlemidir. Kelime ve temel bilgisayar biliminde aslında "serialize" ile aynı şeye denk gelmektedir. fakat Java dünyasında bu iki işlem birbirinden farklıdır.

# Java dünyasında marshalling

Java dünyasında marshalling işlemi sonucu üretilen çıktıda Java sınıfının içindeki codebase + set edilmiş değeler tutulmaktadır. oysa serialize işleminde sadece sınıfın set edilmiş değerleri tutulmaktadır. marshalling işlemi Java standartlarında belirtilmiştir. Sadece RMI işleri için kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# codebase

- RMI uzaktan Java metotu çağırır. dönüşünde ise bir sınıf alır. dönüşte alacağı sınıfı client bilmeyebilir. client sadece o sınıfın arayüzünü bilir. fakat dönecek implementasyonu bilemez. böyle durumlarda RMI çağırdığı metottan dönen codebase bilgisine bakar. codebase sınıfın ID'sini barındırır. eğer aynı ID'de bir sınıf lokalde varsa; client lokaldeki sınıfı kullanır. fakat eğer lokalde yoksa; uzaktan o sınıfı download eder.

- codebase kelimesi yazılım dünyasında source-code'un tümüne verilen isimdir. genelde generated-file'ları barındırmaz. sadece insanların yazdığı kodların tümüne denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java stack yapısı
stack; sadece thread'e bağlı değerleri tutan veri yapısıdır. her thread için ayrı stack vardır. main thread'de buraya dahildir. primitive değerler ve objelerin referansları, stack deposunda tutulur. fakat objeler heap'te tutulur.

thread'de (stack'te) her metot, kendi içinde ona yollanan edilen tüm verileri de tutuyor. yani stack içinde her metot için ayrı bir bölge var. bu sebeple;

```java
metot1(){

     User user1 = new User("ahmet");

     metot2(user1);

     print( user1.name ); // burası ekrana ahmet basacaktır.

}

metot2(User user){

    // burada; metot2 bölgemizde; sadece user diye bir variable var.

    user = new User("Ayşe");  // Bu satırda user variable'nin üzerine yeni bir variable atamış oluyoruz. sadece metot2 bölgesinde bir değişiklik yaptık. eski instance yaşamaya devam ediyor.

}
```

# call by value (or pass by value) vs call by reference (or pass by reference)
genel programlama dillerinde kullanılan terimlerdir.

Java primitive ve objeler için (yani her zaman) için call-by-value yöntemini kullanır. burada bir konuyu netleştirelim: Java'da objenin value'si geçilirken, aslında objenin heap'teki referansı geçilir. buraya kadar herşey normal. fakat geçilen bu referans'ın kopyası (clone'u), stack içindeki sadece o metota ait yerde tutulur. yani hem dallandığımız metot içerisinde hemde metotu çağırdığımız metot içerisinde 2 adet kopya olmuş olur.

yani; 'reference' değilde, 'reference value' yollamış oluyoruz. aslında reference yollanıyor fakat kopyası yollanıyor. yukarıdaki kod örneğinde bu kopya değerinin üzerine yeni objemizin (Ayşe) adresini yazıyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java debugger features
Eclipse ve Intellij debug modundayken aşağıdaki butonları sunar:

"force" prefix on Intellij: Intellij'de neredeyse her özellik force kombinasyonu ile de sunuluyor. force ile bir işlem yapınca yapmak istediğimiz işlemin içerisinde debug point varsa, o debug point'ler ignore ediliyor.

- __step into__

  Eclipse simgesi: ↴   (rightwards arrow with corner downwards)

  Intellij simgesi: Eclipse ile aynı fakat altı çizgili.

  ilgili metotun içindeki ilk satıra gider.

  ek not: Intellij default olarak bazı metotların içine girmez. Bunlar JRE'nin içindeki bazı metotlardır (örneğin System class'ı). Bu davranış biçimi ayarlardan değiştirilebilir.

- __smart step into__

  bu sadece Intellij'de var. Eclipse'te yok.

  Intellij simgesi: ⇅   (upwards arrow leftwards of downwards arrow)

  "step into" ile aynı. fakat 1 satırda bazen birden fazla fonksiyon olabiliyor. örnek:

  > print( count() );

  Intellij her fonksiyonu renklendirip hangisine girmek istediğimizi soruyor.

- __force step into__

  bu sadece Intellij'de var. Eclipse'te yok.

  Intellij simgesi: "step into" ile aynı.

  "step into" eğer girdiğimiz metot 3üncü parti bir jar içinde ise ignore eder ve bir sonraki satıra geçer. fakat "force step into" 3üncü parti jar içine de olsa girer.

- __step over__

  Eclipse simgesi: ↷   (clockwise top semi-circle arrow)

  Intellij simgesi: Eclipse ile aynı fakat altı çizgili.

  ilgili satırı işletir ve sonraki satırda yine beklemede kalır.

- Eclipse name: __step return__

  Intellij name: __step out__

  Eclipse simgesi: ↱   (upwards arrow with tip rightwards)

  Intellij simgesi: ↑   (underlined upwards arrow)

  o anda bulunduğumuz metotun execute edilip sonlanmasını sağlıyor. farklı bir değiş ile: bulunduğumuz metod nroaml şekilde execute edilip bitmesini sağlıyoruz. Daha sonra parent stack'teki bir sonraki satırda duruyor (debug point olmasa bile duruyor). örnek:

  ```java
  mainMethod(){
    method1();
    method2();
  }

  method1(){
    innerMethod1(); // if we click "step over" here, the debugger will execute innerMethod1 and innerMethod2, and then it will stop on method2 (even if we dont have debug point on method2).
    innerMethod2();
  }
  ```

- __resume__

  simgesi: ->   (rightwards arrow)

- Eclipse name: __run to line__

  Intellij name: __run to cursor__

  Eclipse simgesi: ->|   (rightwards arrow and cursor)

  Intellij simgesi: Eclipse ile aynı.

  thread durdurulmuşken buna bastığımızda, caret'in (imleç'in) gösterdiği satıra kadar hiç durmadan gider. Yani kod execution'ı satır satır devam eder. Ancak bizim bulunduğumuz satırda yine beklemeye alınır.

- Eclipse name: __drop to frame__

  Intellij name: __drop frame__

  Eclipse simgesi: ☰->   (triagram and small rightwards arrow)

  IntelliJ simgesi: x☐   ("x" and empty box)

  o anda bulunduğumuz metotun tekrardan işletilmesini sağlıyor. bunun için:
  - o metotun içinde yaratılmış bütün kaynakları (string, int...) yok ediyor
  - debug noktasını metotun en başına taşıyor

  bu durumda metotun aldığı parametrelerin referanslarında değişiklik yapmışsak, o değişiklikler kalmaya devam ediyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# StringBuilder vs StringBuffer
aynı işi gören iki kütüphanedir. tek farkı StringBuffer'ın thread safe olmasıdır. bu da StringBuilder'ı daha performanslı yapıyor.

thread safe konusu StringBuilder ile başka başlıkta anlatılıyor.

# string vs StringBuilder

compiler "b" + "a" gibi satırları StringBuilder ile yapıyor ve bu şekilde memory'den kazanmamızı sağlıyor. fakat derleyici %100 her zaman StringBuffer'a çeviremeyebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# immutable types
- tersi mutable'dır.
- immutable‘ın Türkçe kelime anlamı: durağan, sabit, değişmeyen.

init dildikten sonra, nesnenin içindeki state değiştirilemez olduğunda, o nesne imutable'dır. örneğin unmodifiable collection'lar immutable'dır. çünkü içindeki referans değişkenler (liste elemanları) değiştirilemezler. burada karıştırılmaması gereken nokta;

```java
List<Character> immutablelist = Collections.unmodifiableList(list); //immutable yaptık

immutablelist.add("new value"); //burası hata vermeli

immutablelist = list2; //burası hata vermemeli. çünkü immutable-list kavramı nesnenin kendisinin bir özelliğidir, nesnenin referansı konu dışıdır.
```

User nesnesi içindeki TC değişmemeli (kanun gereği kesin olduğunu varsayarsak); User immutable mı olur?  Hayır. Çünkü User içindeki ad, soyad her zaman değişebilir. User nesnesinin içindeki hiçbir değişken değiştirilemez olursa ancak immutable olur.

# Immutability avantajları
- multithread uygulamalarda hata olması durumunu azaltır.
- immutable nesne başka bir nesne içinde field olarak bulunduğunu düşünelim. böyle bir durumda bu immutable objemizi dışarıya getter ile açarız. getter ile açılan instance'ımızı kimseni update(manipüle) edemeyeeğini biliyoruz. Bu yazılımcıya daha rahat bir ortam sunar. Her getter metodunu çağıran thread'in veya logic'in ne yaptığını kontrol etmek zorunda değilizdir.

# immutable vs thread-safe
ikisi tamamen apayrı kavramlardır. Immutable olan nesne, thread-safe olmayabilir. Örnek:

```java
public class CustomStringAppender {

    private final String internalStr;

    // constructor
    public CustomStringAppender(String initStr) {
        this.internalStr = "initted as: " + initStr;
    }

    public void appendTo(Appendable appendable) {
        // append method uses StringBuilder internally.
        // StringBuilder is non-thread safe class.
        // That means "append" method is not thread-safe.
        // That means "appendTo" method is not thread-safe.
        // But CustomStringAppender is immutable object. Because there is no way to change the state of CustomStringAppender after it's initialization.
        appendable.append(internalStr);
    }
}
```

Ek not: Thread-safe class geliştirildiğinde dikkat edilmesi gereken ince bir husus var: thread-safe olmayan bir sınıf parametre olarak alındığında, ne olacağına özellikle dikkat edilmeli (Yukarıdaki appendTo örneği gibi). Çünkü bu metod 1 satıur ve hızlı bakıldığında thread-safe gibi görünüyor. Fakat appendTo bizim geliştirdiğimiz bir metod değil. Dolayısı ile bu tzr ince noktalar kaşrımıza çıkabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# java.util.Optional
Java 8 ile gelmiştir. Java 9 ile ek metodlar bu sınıfa eklenmiştir.

NullPointer kontrollerinin gerekmediği daha temiz kod yazılabilmesini sağlamaktadır. Kod mantığında "if" bloğu olmamasını gösteren en basit örnek:

```java
String message = null;

Optional<String> opt = Optional.ofNullable(message); // ofNullable null değer alırsa NullPointer fırlatmaz.

opt.filter(m -> m.length() > 5) // filter, filtreleme yapmamızı sağlayan ek bir metottur.
      .ifPresent(System.out::println); // eğer filtre sonucundaki değeler null değilse, bu kod bloğu işletilir. böylece null kontrolü yapmamış oluyoruz.
```

Optinal class'ının sunduğu metodlara örnekler:

```java
///////////////////
// CREATOR METHODS
///////////////////
Optional<Double> opt = Optional.empty(); // boş bir Optional instance'ı oluşturur. isPresent'i false'tur.

Optional<String> opt = Optional.of("Merhaba"); // eğer null yollasaydık NullPointerException fırlatırdı.

Optional<Integer> opt = Optional.ofNullable(null); // eğer null değer alırsa hata fırlatmaz.

///////////////////
// VALUE CHECK METHODS
///////////////////

opt.isEmpty(); // value var ise false döner.

opt.isPesent(); // yukarıdakinin tersidir. boş ise false döner.

///////////////////
// GET METHODS
///////////////////

int value = opt.orElse(0); // o5 içindeki value null ise, 0 döndürür. null değil ise optinal içindeki value'yu döndürür.

int value = opt.orElseThrow(RuntimeException::new); // opt null ise parametrede geçeceğimiz exception'u fırlatır. eğer null değil ise optional içindeki değeri döndürür.

int value = opt.orElseThrow( () -> {
  return new Exception("custom error");
}); // opt null ise parametrede geçeceğimiz metodun içinden üretilen exception'ı fırlatır. eğer null değil ise optional içindeki değeri döndürür.

int value = opt.orElseGet(() -> { return 999}); // orElseGet metodu, orElse ile aynı mantıkta çalışmaktadır. tek bir obje yerine fonksiyon parametre alması. eğer opt null ise; parametre geçtiğimiz fonksiyonumuzun döndürdüğü değeri döndürür.

int value = opt.get(); // If a value is present, returns the value, otherwise throws NoSuchElementException.

int value = opt.or( () -> {
  return new Optional(9999);
} ); // If a value is present, returns the value. otherwise return the optional value from the function.

///////////////////
// IF PRESENT METHODS
///////////////////

opt.ifPresentOrElse(value -> { 
  // do something with value. 
  }, () -> {
    // do somthing if value does not exist.
  }); ​

// eğer optional içindeki değer null değilse, ifPresent metotu koşar. yoksa koşmaz.
// Preset kelime anlamı: mevcut,     önceden ayarlanmış.
opt.ifPresent(value -> {
    System.out.println(value);
});

///////////////////
// JAVA OBJECT CLASS METHODS
///////////////////

opt.toString(); // "Optional" prefix'i ile asıl value'nun toString'ini döner.

opt.hashCode(); // asıl value'nun hashCode metodunu döner.

opt.equals(); // asıl value'ların equals'larını kullanır.

///////////////////
// MAPPER METHODS
///////////////////

// asagida price() metodu direk olarak integer döndüğünü varsayıyorum.
Optional<Integer> opt2 = opt.map(value -> { return value.price(); } ); // if value is null, then returns empty optional. opt and opt2 don't have to store same class type.

// Yukaridaki price metodu direk integer değilde, Optional<Integer> dönseydi, o zaman aşağıdaki gibi bir durumla karşı karşıya kalırdık:
Optional<Optional<Integer>> opt2 = opt.map(value -> { return value.price(); } ); 

// İşte bu sebeple; eğer price, Optional<Integer> dönseydi, flatMap kullanmamız mantıklı olurdu:
Optional opt2 = opt.flatMap(value -> { return value.price(); } );

///////////////////
// OTHER METHODS
///////////////////

Optional opt2 = opt.filter(value -> value.price() > 5); // basit filtre yapar. Eğer filtre'den boş dönerse, boş bir Optinal instance'ı dönülür.

opt.ostream(); // tek 1 elemanlı stream döner. eğer value null ise, boş stream döner.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Nashorn
Java ile yazılmış, Javascript motorudur.

JS komutu ile Javascript dosyası execute edilebiliyor (NodeJS teki gibi).

# Java'dan JS çağırma örneği:

- JS kodu:

```js
var fun1 = function(name) {

    print('Hi there from Javascript, ' + name);

    return "greetings from Javascript";

};
```

- Java kodu

```java
ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn");

engine.eval(new FileReader("script.js"));

Invocable invocable = (Invocable) engine;

Object result = invocable.invokeFunction("fun1", "Peter Parker");

System.out.println(result);

System.out.println(result.getClass()); //prints java.lang.String
```

# Javascript tarafından Java kodu çağırma örneği:

- Java kodu:

```java
static String fun1(String name) {

    System.out.format("Hi there from Java, %s", name);

    return "greetings from java";

}
```

- JS kodu:

```js
var MyJavaClass = Java.type('my.package.MyJavaClass');

var result = MyJavaClass.fun1('John Doe');

print(result);
```

# object types

JS tarafından Java'ya obje parametresi geçerken;

- çevrilebilen değerler (JSON standartındakiler) "java.lang" paketi içindeki String, int, boolean gibi sınıflara çevriliyor.

- objeler Java tarafından bilinmeyen objeler ise (örneğin bir JSON objesi) ; o zaman "JDK.nashorn.internal.scripts.JO4" Sınıfına cast ediliyor. Bu sınıftan get("myJsValue") gibi metotlarla sınıf objelerini çekebiliyoruz.

- MyJavaClass.fun2(new Date()); gibi genel olarak kullanılabilecek sınıfları "JDK.nashorn.internal.objects.NativeDate" gibi sınıflara çeviriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Orika
object to object mapper for Java runtime.

örnek:

```java
mapperFactory.classMap(Source.class, Dest.class);
MapperFacade mapper = mapperFactory.getMapperFacade();
Source src = new Source("abc", 99);
Dest dest = mapper.map(src, Dest.class);
```

Yukarıda "Source" class ve "Dest" class'ında aynı isimde property'ler olmalı. eğer farklı property'leri map edecek isek;

```java
mapperFactory.classMap(Source.class, Dest.class).field("nom", "name").field("surnom", "nickname").register();
```

# Dozer
Orika'ya alternatif ve neredeyse aynı kod API'si olan kütüphane. Dozer, reflection'lardan yararlanarak mapping yaparken, Orika mapping yapılacak objeyi Bytecode'a çevirir ve sonrasında mapping yapar.

# JMapper vs ModelMapper vs MapStruct
Orika ve Dozer'a alternatif kütüphanelerdir.

# Mapstruct
örnek bir MapStruct mapper'ı:

```java
// import org.mapstruct.Mapper; aşağıdaki satırlar comment'e alındığı için burayı da comment'e aldım.
import org.mapstruct.Mapper;

@Mapper
public interface EmployeeMapper {

    /*

    istersek bu şekilde custom mapping'ler belirtebiliriz.
    bu örnekte comment olarak bıraktım.

    @Mappings({
      @Mapping(target="employeeId", source="entity.id"),
      @Mapping(target="employeeName", source="entity.name")
    })
    */
    EmployeeDTO employeeToEmployeeDTO(Employee entity);

    /*
    @Mappings({
      @Mapping(target="id", source="DTO.employeeId"),
      @Mapping(target="name", source="DTO.employeeName")
    })
    */
    Employee employeeDTOtoEmployee(EmployeeDTO DTO);
}
```

Mapstruct compile sırasında bu arayüzin bir implementasyonunu oluşturuyor "target" dizini içerisinde:

```java
public class SimpleSourceDestinationMapperImpl implements SimpleSourceDestinationMapper {

    @Override
    public SimpleDestination sourceToDestination(SimpleSource source) {
        if ( source == null ) {
            return null;
        }
        SimpleDestination simpleDestination = new SimpleDestination();
        simpleDestination.setName( source.getName() );
        simpleDestination.setDescription( source.getDescription() );
        return simpleDestination;
    }

    @Override
    public SimpleSource destinationToSource(SimpleDestination destination){
        if ( destination == null ) {
            return null;
        }
        SimpleSource simpleSource = new SimpleSource();
        simpleSource.setName( destination.getName() );
        simpleSource.setDescription( destination.getDescription() );
        return simpleSource;
    }
}
```

Compile sırasında MapStruct hata verebilirken, IDE'lerdeki eklentiler sayesinde compile etmeden de hataları görebiliriz.

Spring entegrasyonu için ilgili mapper'a bu eklenmelidir:

```java
import org.mapstruct.Mapper;

@Mapper(componentModel = "Spring")
```

Böyle olunca implementasyonun tepesine MapStruct sadece @Component annotation'unu ekliyor. Böylece Spring otomatik bunu Bean olarak algılıyor ve istediğimiz yerden Autowired edebiliyoruz.

MapStruct'ın en verimli yanı, basit bir yöntem tercih etmesi. Implementasyonları IDE'den direk görebiliyoruz. Hatta implementasyonları kopyalayıp projemize yapıştırırsak, MapStruct'ı tamamen (dependency olarak) projeden silebiliriz. Yani implementasyonlar annotation olmayan saf Java kodlarıdır. Mapping'ler bazen çok karışık olabiliyor (map yapılan DTO'larda içiçe objeler, abstract generic sınıflar olabiliyor). MapStruct'ın runtime sırasında ne yapacağında şüphe duymadan, direk implementasyonu (getter/setter'lardan oluşan core Java sınıflarını) inceleyebiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Enum

```java
public enum Age {

  // Java class'ı ile bakıldığında tek fark burada
  // instance'ları baştan constructor çağırarak init
  // ediyor olamızdır.
  NEW(1, "new_age"),
  VERY_OLD(2, "very_old_age", true),
  OLD(3, "old_age"); // burası aşağıdaki constructor'ı çağırıyor.

  private Integer id;
  private boolean dead;
  private String code;

  // standart constructor
  Age(Integer id, String code) {
      this.id = id;
      this.code = code;
  }

  // standart constructor
  Age(Integer id, String code, boolean dead) {
      this.id = id;
      this.code = code;
      this.dead = dead;
  }
}
```

# methods of JVM

```java
Age[] allAge = Age.values();

Age age = Age.valueOf("VERY_OLD");
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jeasy
Java için birçok birbirinden bağımsız kütüphaneler geliştiren projenin ismidir.

j-easy Github ve www.jeasy.org'u kullanırlar.

# easy-random
Jeasy'nin bir kütüphanesidir.

Easy-random'un API'sine bir Java sınıfı veririz, Easy-random bu Java sınıfının içindeki değerleri random bilgilerle doldurulmuş (değerleri set edilmiş) şekilde döndürür.

4.0 sürümü öncesi, projenin ismi Random-beans'tı. 4.0 ile Easy-random yapıldı. Aynı sürümde io.github.beans paket ismindeki kodlar, "org.jeasy" altına taşındı.

Bu kütüphane test yazımında büyük kolaylık sağlar.

örnek:

```java
EasyRandom easyRandom = new EasyRandom();
Person person = easyRandom.nextObject(Person.class);
```

Opsiyonel olarak, random olacak bilgilerin aralıklarını vs verebiliriz:

```java
EasyRandomParameters parameters = new EasyRandomParameters()
   .seed(123L)
   .objectPoolSize(100)
   .randomizationDepth(3)
   .charset(forName("UTF-8"))
   .timeRange(nine, five)
   .dateRange(today, tomorrow)
   .stringLengthRange(5, 50)
   .collectionSizeRange(1, 10)
   .excludeField(named("age").and(ofType(Integer.class)).and(inClass(Person.class)));

EasyRandom easyRandom = new EasyRandom(parameters);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Apache Camel
Enterprise Integration Patterns (or EIP)'nin implementasyonlarını sağlayan Java framework'ü. Kolaylaştırması açısından birçok farklı DSL desteklemektedir:

DSL for core Java:

```java
from ("file:/tmp").to("jms:aQueue");
```

DSL for Spring:

```xml
<route>
  <from uri="file:/tmp"/>
  <to uri="jms:aQueue"/>
</route>
```

DSL for Scala:

```scala
from "file:/tmp" -> "jms:aQueue"
```

Camel genel olarak kaynaktan bilgi okuma, kaynağa bilgi yazma, data-transofrmation gibi işlemleri çok kolay (sadece DSL tanımlamaları ile) yapabilmemizi sağlayan bir framework'tür:

```java
from("direct:DistributeOrderDSL")
   .choice()
      .when(header("order").isEqualTo("oil"))
         .to("direct:oil")
      .when(header("order").isEqualTo("milk"))
         .to("direct:milk")
      .otherwise()
         .to("direct:d");
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
