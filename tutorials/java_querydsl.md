############################

############################
# JAVA QUERY DSL
############################

############################

# QueryDSL
build sırasında code generation'ı yaparak, birçok Java kütüphanesi için ek query sağlayan kütüphanedir. desteklediği kütüphaneler:

- JPA
- SQL
- MongoDB
- Collections

JPA için örnek olarak kullanımına bakalım:

```xml
<dependencies>

    <dependency>
        <!-- this is for code generation on build time. so it is on provided scope which means this library exist only on build phase. -->
        <groupId>com.querydsl</groupId>
        <artifactId>querydsl-apt</artifactId>
        <version>${querydsl.version}</version>
        <scope>provided</scope>
    </dependency>

    <dependency>
        <!-- burada kullanmak istediğimiz Java modülünü eklememiz şart -->
        <groupId>com.querydsl</groupId>
        <artifactId>querydsl-jpa</artifactId>
        <version>${querydsl.version}</version>
    </dependency>

</dependencies>

<plugin>
    <!-- this is for code generation on build time. -->
    <groupId>com.mysema.maven</groupId>
    <artifactId>apt-maven-plugin</artifactId>
    <version>1.1.3</version>
    <executions>
        <execution>
            <goals>
                <goal>process</goal>
            </goals>
            <configuration>
                <outputDirectory>target/generated-sources/java</outputDirectory>
                <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
            </configuration>
        </execution>
    </executions>
</plugin>
```

Java kodumuzda User isimli bir entity varsa; "mvn compile" komutunu çalıştırdığımızda QUser isimli bir sınıf generate edilecektir. QUser içerisinde birçok query yapabilmemizi sağlayan metot mevcuttur. örnek kullanım:

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory(); // default JPA class
EntityManager em = emf.createEntityManager(); // default JPA class

JPAQueryFactory queryFactory = new JPAQueryFactory(em); // com.querydsl.jpa.impl.JPAQueryFactory

QUser user = QUser.user; // QUser is auto-generated by QueryDSL library. QUser includes a default instance of QUser as static.

// simple get example
User c = queryFactory.selectFrom(user)
                              .where(user.login.eq("David"))
                              .fetchOne();

// order example
List<User> c = queryFactory.selectFrom(user)
                                  .orderBy(user.login.asc())
                                  .fetch();

// update example
queryFactory.update(user)
  .where(user.login.eq("Ash"))
  .set(user.login, "Ash2")
  .set(user.disabled, true)
  .execute();

// delete example
queryFactory.delete(user)
  .where(user.login.eq("David"))
  .execute();
```

# Predicate
Predicate kelime anlamı: yüklem, doğrulamak, belirtmek.

kelime kökü predict'ten (öngörmek) gelmiyor. tamamen farklı kelimelerdir.

Bilgisayar ve matematik bilimlerinde Predicate filtre yapıldığında uygulanacak kriterin tutulduğu instance/obje'ye verilen isimdir. Predicate, en basit örnek olarak aşağıdaki Java kodu ile örneklenmiştir. aşağıdaki filter metotu java.util.function.Predicate<T> (Functional Interface) 'i argüman olarak kabul eder.

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);

List<Integer> collect = list.stream().filter(x -> x > 3).collect(Collectors.toList());
```

java.util.function.Predicate'in kodu bu şekildedir:

Not: aşağıdaki koddaki yorumlara bakıldığında şu yorum çok önemli:

```
"predicate (boolean-valued function) of one argument."
```

```java
import java.util.Objects;

/**
 * Represents a predicate (boolean-valued function) of one argument.
 */
@FunctionalInterface
public interface Predicate<T> {

    /**
     * Evaluates this predicate on the given argument.
     *
     * @param t the input argument
     * @return {@code true} if the input argument matches the predicate,
     * otherwise {@code false}
     */
    boolean test(T t);

    /**
     Other code here!
     http://hg.openjdk.java.net/jdk8/jdk8/JDK/file/687fd7c7986d/src/share/classes/java/util/function/Predicate.java
     */
}
```

# QueryDSL Predicate

örnek:

```java
import org.springframework.data.querydsl.binding.QuerydslPredicate;
import com.querydsl.core.types.Predicate;

@GetMapping(value = "/users")
public Page<UserDto> fetchUsers(
                               @QuerydslPredicate(root = UserDBEntity.class) Predicate predicate,

                               // "pageable" başka başlıkta anlatılıyor. bu konudan bağımsız. burada sadece birlikte kullanılabileceğini göstermeye çalıştım. pageable'ın konulması zorunlu değil.
                               Pageable pageable
                               ) {

    // repository should extends org.springframework.data.querydsl.QuerydslPredicateExecutor<UserDBEntity>
    Page<User> userListPageable = userRepository.findAll(predicate, pageable);

    // mpToDto QueryDSL'den bağımsız bir konu. database entity'sini dönmemek için burada basit bir mapping yapıldı.
    return userListPageable.map(UserDBEntity -> mapperUtil.mapToDto(UserDBEntity));
}
```

@QuerydslPredicate direk query'nin parametre olarak inject edilmesini sağlıyor. Aynı Pageable 'in yaptığı gibi.

```java
// com.querydsl.core.types.dsl.BooleanExpression
BooleanExpression b = QUser.user.email.eq("ahmet@gmail.com");
```

BooleanExpression implements com.querydsl.core.types.Predicate interface.
