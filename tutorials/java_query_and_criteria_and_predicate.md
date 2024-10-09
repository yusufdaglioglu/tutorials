############################

############################
# JAVA QUERY AND CRITERIA AND PREDICATE
############################

############################

# javax.persistence.Query

```java
Query query = getEntityManager().createQuery("SELECT u FROM UserEntity u WHERE u.id=:param1");
query.setParameter("param1", 99);
UserEntity userEntity = (UserEntity) query.getSingleResult();
```

__TypedQuery__ is the implementation of the Query. TypedQuery only stores the result class as a result, so we don't need to cast the executed result.

```java
TypedQuery<UserEntity> query = getEntityManager().createQuery("SELECT u FROM UserEntity u WHERE u.id=:param1");
query.setParameter("param1", 99);
UserEntity userEntity = query.getSingleResult();
```

__org.hibernate.query.NativeQuery__ ise javax.persistence.Query implementasyonudur. Kullanımı:

```java
Query nativeQuery = getEntityManager().createNativeQuery("SELECT * FROM users WHERE id=:param1", UserEntity.class);
query.setParameter("param1", 99);
UserEntity userEntity = (UserEntity) query.getSingleResult();
```

__NamedQuery__, Query implementasyonu değildir. Bir annotation'dur. Direk entity'de tanımlanmak üzere tasarlanmışlardır:

```java
@org.hibernate.annotations.NamedQueries({

    @org.hibernate.annotations.NamedQuery(name = "query1",
      query = "from DeptEmployee where employeeNumber = :employeeNo"),

    @org.hibernate.annotations.NamedQuery(name = "quer2",
      query = "from DeptEmployee where designation = :designation"),

    @org.hibernate.annotations.NamedQuery(name = "quer3",
      query = "Update DeptEmployee set department = :newDepartment where employeeNumber = :employeeNo")
})
public class UserEntity {

    @Id
    private Long id;
    private String name;
}
```

"Named" prefix'i kullanıldığında ismi ile çağrıldığıdan dolayı verilmiştir. Örnek kullanım:

```java
Query namedQuery = getEntityManager().createNamedQuery("query1");
namedQuery.setParameter("employeeNo", id);
UserEntity userEntity = (UserEntity) namedQuery.getSingleResult();
```

# Criteria and Predicate classes of JPA

"Spring Data" da bu sınıfları kullanır. Çünkü Spring-Data, JPA'yı wrap ediyor.

Burada her satırı detaylı açıklanmış 3 önemli örnek var: https://github.com/yusuf-daglioglu/orm-examples-with-spring/

Burada birçok Criteria API örnekleri mevcut: https://en.wikibooks.org/wiki/Java_Persistence/Criteria

# fetch join
SQL'de böyle bir keyword yok ve zaten ihtiyaç yoktur. JPQL'e ait bir terimdir. Fakat "fetch" keyword'ü SQL'de tamamen farklı bir amaç için kullanılıyor.

```java
@Table
class Customer {
  String id;
  String name;

  @ManyToMany(fetch = FetchType.LAZY)
  String List<OrderEntity> orders;
}
```

Yukarıdaki entity üzerinden veritabanında data çekmeye kalktığımızda "order" bilgisi gelmeyecektir. Ancak "getOrders()" metotunu çağırdığımızda JPA otomatik olarak yeni bir SQL isteği atacaktır. Çünkü "LAZY" annotation'u vardır.

Eğer biz tek bir SQL sorgusunda tüm verinin dönmesini istiyorsak JPA'nın sunduğu "fetch join" özelliğini kullanabiliriz.

```java
Query query = em.createQuery("SELECT DISTINCT c FROM Customer c INNER JOIN FETCH c.oders o");
```

Ek not: Eğer alan LAZY olmasaydı ve biz sadece Customer tablosunu çekseydik, JPA arkaplanda "join" yapacaktı. Yani aşağıdaki 2 query aynı anlama gelmektedir:

```java
Query query1 = em.createQuery("SELECT DISTINCT c FROM Customer c");
Query query2 = em.createQuery("SELECT DISTINCT c FROM Customer c INNER JOIN c.orders o");
```

# JPA Metamodel

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-jpamodelgen</artifactId>
    <version>5.3.7.Final</version>
</dependency>
```

Yukarıdaki bağımlılık build fazında tüm entity class'larımızı tarayıp, target dizini altına aşağıdaki kodları oluşturuyor:

```java
@Generated(value = "org.hibernate.jpamodelgen.JPAMetaModelEntityProcessor")
@StaticMetamodel(Student.class)
public abstract class Student_ {

    public static volatile SingularAttribute<Student, String> firstName;
    public static volatile SingularAttribute<Student, String> lastName;
    public static volatile SingularAttribute<Student, Integer> id;

    public static final String FIRST_NAME = "firstName";
    public static final String LAST_NAME = "lastName";
    public static final String ID = "id";
}
```

Biz bu sınıflar sayesinde aşağıdaki gibi kod hazabiliyoruz:

```java
Predicate p = builder.equal(Student_.LAST_NAME, authorName);

// Eğer bu sınıflar oluşturulmasaydı field ismini hardcode yazmak zorunda kalacaktık:
Predicate p = builder.equal(from.get("lastName"), authorName);
```