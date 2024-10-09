############################

############################
# JAVA HASH CODE
############################

############################

# Nesne eşitliği

# Nesne eşitliğini bulma

Java'da == ifadesi iki nesnenin referanslarının aynı olup olmadığına bakar. eğer karşılaştırılan nesneler primitive ise (int, boolean...), sadece değerleri karşılaştırılır.

Java'da her nesne, object sınıfından türemiştir ve equals(Object) metotu içerir. Bu metot ile iki obje karşılaştırılabilir. Bu karşılaştırmada metotunu yazılımcı override ettiği için istediğini döner. fakat asıl yapılması (beklenen) dönüş; sınıfın içindeki unique değerlerin kontrol edilerek dönüş almaktır. Tabi sadece unique değerler karşılaştırılarak olmaz, zira karşılaştırılan sınıflarda aynı tipte olmalıdır.

Eğer equals metotu override edilmez ise; object sınıfındaki metota göre referansları aynı olan sınıflar için true dönüşü alırız. Yani; override edilmezse == 'e denk geliyor.

hashCode veya equals kullanırken tüm elemanları göz önünde bulundurmak zorunda değiliz.

# toString() default implementation
```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

# hashcode() default implementation
bu başlık için kaynak: (source-id: 465)

"java.lang.Object.hashcode()" implementasyonu Java değil, native dilde yazılmıştır. Bu sebeple JRE'nin kendisi ne istiyorsa onu döndürür. Hashcode metotu spesification gereği tüm runtime süresi boyunca aynı instance için aynı değeri döndürmesi beklenir. Bu sebeple instance'ın RAM'deki adresini dönmeyi tercih etmez. Çünkü garbage collector arada temizlik yapmak için çalıştığında, bellekteki instance'ların yerini değiştirebilir.

Native kodlar incelendiğinde; bu dönen sayı ne olursa olsun, önce bu değeri instance'ın meta bilgilerinde sakladığı görülür. Çünkü eğer bir JDK implementasyonu memory-adres'ini döndürmeye kalkarsa, ilgili instance için önceden alınmış hash-code varsa onu döndürür, yoksa yeni hash-code üretir.

Yeni hash-code üretirken ise her sürüm kendi yöntemini kullanır. Seçenekler şunlardır:

0. A randomly generated number.
1. A function of memory address of the object.
2. A hardcoded 1 (used for sensitivity testing.)
3. A sequence.
4. The memory address of the object, cast to int.
5. Thread state combined with Xorshift (https://en.wikipedia.org/wiki/Xorshift)

JDK'lar ise şunu yaparlar:
- OpenJDK 6 --> 1inci seçenek
- OpenJDK 7 --> 1inci seçenek
- OpenJDK 8 --> 5inci seçenek
- OpenJDK 9 --> 5inci seçenek

# hashcode
Equals ile benzer mantıkta olan hashCode() bir integer ile unique bir değer döndürmeli. hashtable gibi veri yapılarında, bir sınıfın hash değerini saklamak gerekir diye bu metot hazır olmalıdır. Integer dönmesinin sebebi; eşitlik kontrolünün equals'a göre daha hızlı yapılmasını sağlamaktır. Yani; equals ile aynı amaçta kullanılır fakat hashCode Integer olduğundan equals gibi her dahi güvenebileceğimiz bir bilgi değildir. Dolayısı ile avantajlar/dezavantajlar konusuna dönmekteyiz.

Hashcode'un dönüş değeri integer olduğundan, kısıtlı çıktı kümesi vardır. Bu sebeple aslında; unique değildir. Bu sebeple bazı instance'ların hashcode'ları çakışır (hash collision). Buna en basit örnek string'lerdir. Bir string'in sonsuz farklı instance'ını yaratabiliriz. Bu durumda hashcode'lar illaki çakışacaktır. Örneğin Java'da; "Siblings" ve "Teheran", "Aa" ve "BB" aynı hashcode döndürür. Bu sebeple Java-core'daki HashMap sınıfı önce hashCode'a bakar, eğer hashcode'u aynı olan nesneler tutuyor olursa, equals veya referans (==) kontrolüne tabi tutar. Böylece çakışmaları çözmüş olur. HashMap'in önce hashcode'a bakmasının sebebi, integer ile eşitlik kontrolünün bilgisayarlar tarafından daha hızlı yapılmasıdır.

Eğer DTO'muz immutable ise, hashcode'u her defa hesaplatmak yerine, initialize olduğunda yada bir kere hashCode çağrıldığında bunu bir field'da yerde tutup, hashcode'u sürekli bu field'dan döndürebiliriz. bu bize performans avantajı sağlar.

Hashcode'u (hiç özel kütüphane kullanmadan) yazmak için genelde bu yöntem tercih edilir:

```java
// Intellij (version 2020) auto-generated equals ve hashCode örneği

import java.util.Arrays;
import java.util.List;

public class TestDTO {

  private String valStr;
  private int valInt;
  private Integer valInteger;
  private TestDTO valTestDTO;
  private long valLong;
  private char valChar;
  private boolean valBoolean;
  private String[] valStringArray;
  private List<String> valStringList;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;

    TestDTO testDTO = (TestDTO) o;

    if (valInt != testDTO.valInt) return false;
    if (valLong != testDTO.valLong) return false;
    if (valChar != testDTO.valChar) return false;
    if (valBoolean != testDTO.valBoolean) return false;
    if (valStr != null ? !valStr.equals(testDTO.valStr) : testDTO.valStr != null) return false;
    if (valInteger != null ? !valInteger.equals(testDTO.valInteger) : testDTO.valInteger != null) return false;
    if (valTestDTO != null ? !valTestDTO.equals(testDTO.valTestDTO) : testDTO.valTestDTO != null) return false;
    // Probably incorrect - comparing Object[] arrays with Arrays.equals
    if (!Arrays.equals(valStringArray, testDTO.valStringArray)) return false;
    return valStringList != null ? valStringList.equals(testDTO.valStringList) : testDTO.valStringList == null;
  }

  @Override
  public int hashCode() {
    int result = valStr != null ? valStr.hashCode() : 0;
    result = 31 * result + valInt;
    result = 31 * result + (valInteger != null ? valInteger.hashCode() : 0);
    result = 31 * result + (valTestDTO != null ? valTestDTO.hashCode() : 0);
    result = 31 * result + (int) (valLong ^ (valLong >>> 32));
    result = 31 * result + (int) valChar;
    result = 31 * result + (valBoolean ? 1 : 0);
    result = 31 * result + Arrays.hashCode(valStringArray);
    result = 31 * result + (valStringList != null ? valStringList.hashCode() : 0);
    return result;
  }
}
```

yukarıda görüldüğü gibi Long, String gibi class'ların hashCode metotları zaten Java-core'da implemente edilmiş durumda. DTO'larımızın hashCode'ları olmazsa onu kullanan sınıflarda hashCode alamayız.

Integer sınıfının hashcode'u (tahmin edildiği gibi) kendisidir.

boolean'ın hashcode'unu 1 veya 0 şeklinde çevirebiliriz. en basit çözüm olacaktır.

her field'ın 31 gibi sabit bir asal sayı (or prime number) ile çarpılması önerilir. Tek sayı (or odd number) olması değil, asal sayı olması önemlidir. Bunun sebebi matematikseldir. Burada önemli olan nokta şudur: asal sayı ile çarpmak unique sayı dönmesinin ihtimalini arttırmaz, onun yerine Collection'larda matematiksel avantaj sağlar.

Bunun uzun açıklaması şudur: HashMap ve HashSet gibi collection'lar, her hash-code değerlerini "bucket" denilen yerde saklar. İnsert yapıldığında, insert edilen objenin hash-code değerini (integer) saklaması gerektiğinde, nokta atışı yerini çabuk bulabilmek/belirleyebilmek için hash değerini bucket'ın index'i olarak kabul eder. Fakat bucket Integer.MAX büyüklüğünde olamaz. Öyle olursa çok yer kaplar. İşte bu sebeple hash'in modülünü (%) alır. Örneğin; bucket-size 20 ise ve hash-code değerimiz 21 ise, 21%20=1'dir. Yani bu hash ilk bucket'a kaydedilir. Diğer gelecek hash-değerleri eğer index 1'e denk gelmezse hash-map daha hızlı çalışacaktır. İşte modül işlemine tabi tutulduğunda, sonucun dağıtık olması için en basit çözümü bir asal sayı ile çarpmaktır. Yani; asal sayı ile çarpmak hashcode'un tekil olmasını değil, modül işlemine tabi tutulduğunda dağıtık (farklı) sonuç vermesi ihtimalini arttırmaktadır.

Ek not:
aşağıdaki rakamlar kütüphaneye göre değişkenlik gösterir ama genelde ortalama rakamlar şu şekildedir:
- hash-map init edildiğinde 20 bucket alanı rezerv edilir.
- bucket sayısı %75 doluluk oranına ulaştığında yeni bucket alanları otomatik olarak rezerv edilir (bucket sayısı arttırılır).

Hashcode'un kalitesini 2 şey belirler:
- olabildiğince unique olması (eğer instance sayısı Integer.MAX 'ı geçemez ise, unique olması, sıralama işlemleri için şarttır)
- mod alındığında dağıtık sonuçlar verebilmesi (hash ile işlem yapan collection'ların performance 'ını arttır)

Asal sayı olarak özellikle "31" sıkça kullanılır. çünkü 31, standart CPU'larda performans olarak basit bir işlem olarak yapılabilir. interpreter'lar bunu genellikle otomatik olarak aşağıdaki işleme çevirir:

```
31 * i = (i << 5) - i
```

Google-Guava kütüphanesi var ise aşağıdaki yöntem de kullanılabilir:

```java
import com.Google.common.base.Objects;

import java.util.List;

public class TestDTO {

  private String valStr;
  private int valInt;
  private Integer valInteger;
  private TestDTO valTestDTO;
  private long valLong;
  private char valChar;
  private boolean valBoolean;
  private String[] valStringArray;
  private List<String> valStringList;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    TestDTO testDTO = (TestDTO) o;
    return valInt == testDTO.valInt && valLong == testDTO.valLong && valChar == testDTO.valChar && valBoolean == testDTO.valBoolean && Objects.equal(valStr, testDTO.valStr) && Objects.equal(valInteger, testDTO.valInteger) && Objects.equal(valTestDTO, testDTO.valTestDTO) && Objects.equal(valStringArray, testDTO.valStringArray) && Objects.equal(valStringList, testDTO.valStringList);
  }

  @Override
  public int hashCode() {
    return Objects.hashCode(valStr, valInt, valInteger, valTestDTO, valLong, valChar, valBoolean, valStringArray, valStringList);
  }
}
```

Apache-commons kullanırsak bu şekilde de yapabiliriz:

```java
// Apache-commons-lang v3 ve öncesi arasında hiçbir metot ve class ismi fark etmiyor. sadece package fark ediyor.

import org.Apache.commons.lang.builder.EqualsBuilder;
import org.Apache.commons.lang.builder.HashCodeBuilder;

import java.util.List;

public class TestDTO {

  private String valStr;
  private int valInt;
  private Integer valInteger;
  private TestDTO valTestDTO;
  private long valLong;
  private char valChar;
  private boolean valBoolean;
  private String[] valStringArray;
  private List<String> valStringList;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;

    if (o == null || getClass() != o.getClass()) return false;

    TestDTO testDTO = (TestDTO) o;

    return new EqualsBuilder().append(valInt, testDTO.valInt).append(valLong, testDTO.valLong).append(valChar, testDTO.valChar).append(valBoolean, testDTO.valBoolean).append(valStr, testDTO.valStr).append(valInteger, testDTO.valInteger).append(valTestDTO, testDTO.valTestDTO).append(valStringArray, testDTO.valStringArray).append(valStringList, testDTO.valStringList).isEquals();
  }

  @Override
  public int hashCode() {
    return new HashCodeBuilder(17, 37).append(valStr).append(valInt).append(valInteger).append(valTestDTO).append(valLong).append(valChar).append(valBoolean).append(valStringArray).append(valStringList).toHashCode();
  }
}
```

# java.lang.String.hashCode()

(source-id: 323)

bu şekilde tanımlanmıştır:

```java
/**
* Returns a hash code for this string. The hash code for a
* {@code String} object is computed as
* <blockquote><pre>
* s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
* </pre></blockquote>
* using {@code int} arithmetic, where {@code s[i]} is the
* <i>i</i>th character of the string, {@code n} is the length of
* the string, and {@code ^} indicates exponentiation.
* (The hash value of the empty string is zero.)
*
* @return  a hash code value for this object.
*/
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;

        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```

Doc okunduğunda üstel işlem görünmektedir. Fakat metot implementasyonunda üstel işlem görünmüyor. Aslında for loop debug edip tamamlandığında doc'taki işleme denk gelmektedir:

1. döngü:
> h = 31 * hash + s[0]

2.döngü:
> h = 31 * (31 * hash + s[0]) + s[1]

yada bu şekilde de yazabiliriz:

> h = 31^2 * hash + 31 * s[0] + s[1]

3. döngü:

> h = 31 * (31 * (31 * hash + s[0]) + s[1]) + s[2]

yada bu şekilde de yazabiliriz:

> h = 31^3 * hash + 32^2 * s[0] + 31 * s[1] + s[2]

Sonuç olarak dikkat edersek doc'taki açıklama ile aynı kapıya çıktığını görebiliriz.

# primitive'lerin obje seviyesinde karşılıklarındaki eşitlik kontrolleri (primitive wrapper'ların eşitlik kontrolleri)
new Integer(5) = new Integer(5); --> false vermektedir. referansları yeni obje olduğu için farklı.

Integer.valueOf(5) == Integer.valueOf(5) --> valueOf metotu Integer objesi döndürmektedir. normalde false olması beklenmektedir. fakat true vermektedir. çünkü; Integer sınıfı kendi içinde initialize olurken static olarak 1'den 256'ya kadar olan sayılar için birer Integer objesi saklamaktadır. çünkü bunlar genelde cok kullanılan sayılardır. valueOf metotu bu sayılar haricinde "new Integer()" çalıştırıyor. fakat bu sayılardan ise hızlı olsun diye bunlardan donduruyor. bazı JVM'ler 256'dan daha fazla saklamaktadır.

Java'da aynı string'ler aynı değeri göstermektedir. çünkü JVM, her string'i "__string pool__" denilen yerde saklamakta ve yeni oluşturulan string'ler aynı ise, referansını buradan dönmektedir. bu sebeple == equals yerine kullanılmaktadır. fakat new String("text") her zaman yeni bir referansla obje yaratmaktadır. bu sebeple bunlarda == kullanılamaz. new String("text").intern() metotu var olan JVM'deki string'lere referans etmemizi sağlıyor. bu şekilde bellekten tasarruf etmiş oluyoruz.

# hashcode and equals method for DB entity classes
lets define simply what is equals: Equals method should return true, if the identifiers of them are same.

- 1

if we don't implement the hashcode and equals methods, then if we read multiple times the same row from DB, the equals and the hashcode methods will return false for both instances because Java will check the instance memory references.

- 2

if we will implement hashcode and equals method using all fields, that not's gonna work in this case:

```java
user1 = new User("Jack").
repository.save(user1);

user2 = repository.readByPrimaryKey("12345");
user2.equals(user1); // returns false because "ID" field is null on user1 yet.
```

- 3

if we will implement the equals and hashcode based on "natural key", we will have un-consistent cases like this:

```java
user1 = new User("998877", "Jack").
repository.save(user1);

user1.setName(); // the user has a new identity number...

user2 = repository.readByNationalIdentityNumber("998877");
user2.equals(user1); // returns false.
```

The possible way is to do not use JPA's primary keys. we can manage them manually. We can initialize "ID" field on constructor. And we have to compare only "ID" field which is unique. For consistent ID generation we can use UUID.

Example abstract class for all entities on the project:

```java
public interface PersistentObject {
    public String getId();
    public void setId(String id);

    public Integer getVersion();
    public void setVersion(Integer version);
}

public abstract class AbstractPersistentObject
        implements PersistentObject {

    private String id = IdGenerator.createId();
    // JPA, DB'den okuduğu zaman burası da her defasında çalışacak fakat JPA sonrasında setter'ı çağıracağı için eski data yazılacak. yani bir sorun yok

    private Integer version;

    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }

    public Integer getVersion() {
        return version;
    }
    public void setVersion(Integer version) {
        this.version = version;
    }

    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null ||
            !(o instanceof PersistentObject)) {

            return false;
        }

        PersistentObject other
            = (PersistentObject)o;

        // if the ID is missing, return false
        if (id == null) return false;

        // equivalence by ID
        return id.equals(other.getId());
    }

    public int hashCode() {
        if (id != null) {
            return id.hashCode();
        } else {
            return super.hashCode();
        }
    }

    public String toString() {
        return this.getClass().getName()
            + "[id=" + id + "]";
    }
}
```

Yukarıdaki örnekte neden version kullandık? Çünkü JPA normal koşullarda ID field'ı boş ise bu instance'ı yeni row olarak algılıyor. Bu durumu çözebilmek için version sütunu kullandık. version sütunu eğer set edilmiş ise JPA bunu zaten kaydedilmiş bir instance olarak algılayacak. BUnun için şu config'leri de yapmamız gerekli:

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping SYSTEM
"http://hibernate.sourceforge.net/
hibernate-mapping-3.0.dtd">

<hibernate-mapping package="my.package">

  <class name="Person" table="PERSON">

    <id name="id" column="ID">
      <generator class="assigned" />
    </id>

    <version name="version" column="VERSION"
        unsaved-value="null" />

    <!-- Map Person-specific properties here. -->

  </class>

</hibernate-mapping>
```

Source:
- (source-id: 324)
- (source-id: 325)
- (source-id: 326)

# hashcode and equals for Java.util.ArrayList
Her class için olduğu gibi her collection içinde hashcode ve equals metodunun nasıl çalışacağı o class'ın kendisine (implementasyona) bırakılmıştır.

ArrayList'in hashCode metodu listenin her elemanı sırası ile dolaşarak hashCode'unu alıp matematksel bir işleme sokmaktadır. Dolayısı ile listedeki elemanların sırası farklı ise ama elemenlar aynı ise, hashCode farklı çıkacaktır. Buradan dökümana bakılabilir: https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html#hashCode() (Kod örneği ile gösterilmiş).

Benzer şekilde equals metodu da aynı şekilde çalışır. Tabi equals matematiksle işlem yerine sırası ile her elemanın eşit olup olmadığını kontrol eder.
