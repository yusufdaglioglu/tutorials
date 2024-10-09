############################

############################
# JAVA JDBC AND ORM
############################

############################

# ORM (or Object to Relational Mapping)
ORM programlama dilinden bağımsız, veritabanı objelerinin programlamada haritalanmasının (mapping) genel ismidir.

# Persistence (or süreklilik)
Bilişim dünyasında persistence, bir işlemin sürekliliğinin sağlanması kastedilir. işlemin sürekliliği olabilmesi için state'in (data'nın) bir yere kaydedilmesi gerekir.

# JPA (or Java Persistence API)
Java için ORM spesifikasyonudur.

# Eclipselink (or old-name:Toplink) vs Hibernate vs openJPA
Birer JPA implementasyonudur.

# @javax.persistence.Entity vs @org.hibernate.annotations.Entity
javax kullanırsak runtime sırasında implementasyon arar ve onu çağırır. Hibernate olan direk kendini çağırır. Hibernate ne kadar javax ile aynı görevi görse de, javax'ta olmayan ekstra property v fonksiyonlar sunuyor.

# Exception sınıfları
Benzer şekilde or.hibernate.StaleObjectStateException'ı Hibernate fırlatırken, JPA javax.persistence.OptimisticLockException fırlatır. Bunlar birbirini implemente etmez. Benzer şekilde Spring-Data'da farklı exception sınıfları kullanıyor olabilir. Tamamen aynı hataya tekabül edebilirler.

Ne zaman hangi hatanın fırlatılacağı kimin metodunu çağırdığımıza bağlı değişir. Direk Hibernate'in "session" sınıfı üzerinden SQL işlettiysek onun özel hatasını alırız. Direk JPA'nın EntityManager sınıfını çağırırsak, JPA'nın hatasını alırız. Fakat Hibernate implementasyonunu kullanıyorsak, JPA'nın stack-trace'ini incelerken, her 2 hatayı görmemiz gerekir. Çünkü Hibernate hata fırlatır JPA onu wrap eder. Fakat istisna olarak; JPA hatayı wrap ederken exception'ı log'a basmamış (yutmuş) olabilir. Bu durumda sadece JPA'nın hatasını görürüz.

# Entity Framework
Microsoft'un ORM framework'ü'nün özel ismidir. İsim karmaşasına yol açmaktadır, çünkü çok genel bir terimdir.

# JDBC (or Java Database Connectivity) vs JPA
JDBC, Java için database spec'idir. (driver olmadan bir işe yaramaz.) JPA kendi içinde JDBC kullanır. JDBC database'ye bağlanan ve üzerinde işlem yapmaya yarayan API'dir. Her database bunun implementasyonlarını (driver'larını) yapar (MySQL, Oracle...). Bunun üzerine ORM yeteneğini katarak JPA ortaya çıkmıştır.

# OJDBC
Oracle database'sinin JDBC driver'ıdır.

# ODM (or Object Document Mapping)
ORM relational database'ler için mapping yaparken, ODM sadece NoSQL'ler (relation olmayan database'ler) içindir.

# database Dialect vs database Driver
dialect Türkçe anlamı: lehçe

Driver bir database'ye giderken soket üzerinden kullandığı erişimi sağlayan yazılımdır. örneğin bir SQL komutunu şu IP'li database'de çalıştır dediğimizde bu işi halleder.

Dialect ise SQL türevi (ve o türevin versiyonudur). SQL dilinin birçok türevi ve versiyonu çıkmıştır. bazı database'ler birçok farklı SQL türevine ve versiyonuna destek verir. işte bu durumlar için Dialect özelliğini kullanırız.

bir Spring uygulamasını JPA ile ayağa kaldırırken, hem dialect hemde driver vermemiz gerekir. eğer dialect vermezsek default dialect'i kullanacaktır.

# SQL2o
açık kaynaklı Java kütüphanesinin ismi. bu kütüphane ile aşağıdaki gibi resultSet'lerle uğraşmadan query atabiliyoruz. Hibernate'ten ortalama 5 kat daha hızlı.

```java
public List<Customer> fetchCustomers(customerId) {
  try (Connection con = sql2o.open()) {
    final String query =
        "SELECT id, name, address " +
        "FROM customers WHERE id = :customerId";

    return con.createQuery(query)
        .addParameter("customerId", customerId)
        .executeAndFetch(Customer.class);
  }
}
```

# HikariCP
JDBC için connection pool kütüphanesidir. Buraya bakılırsa: (source-id: 397) "What does correctness and reliability mean?" başlığı. Connecion pool yönetiminin ne gibi yetenekleri olduğu görülmektedir.

# connection pool-size
HDD'lerde cihaz yavaşlığından kaynaklı bloklanma vardı. Fakat SSD'lerde block'lanma daha az çünkü SSD cihaz hızlı cevap dönüyor. SSD'ler hızlı olduğundan yazılımcılar direk olarak güçlü makine psikolojisi ile pool sayısını arttırıyor, oysa; bloklanma az olduğundan CPU'nun ve diğer tüm cihazların thread switch maliyetini düşürmenin avantajlı olduğu görülmektedir.

ortalama connection pool sayısı database çalıştıran makinanın şu özelliklerinden hesaplanır:

connections = (CPU_core_count * 2) + effective_spindle_count

Hyperthreading aktif ise bile HT sayısı ile değil, gerçek fiziksel core CPU sayısı referans alınmalıdır.

yukarıdaki tüm bilgiler için kaynak: (source-id: 398)

spindle; HDD'nin okuma kafasını temsil eder. Birden fazla HDD'miz olsun. bu HDD'lerdeki kayıtların tümü tek bir DB instance'ını (sunucusunu) oluştursun. Bu durumda HDD sayımız, spindle count'a eşit olur. Eğer her HDD'mizi paralelden 2 kafa okuyor olsaydı o zaman; spindle HDD adetinin 2 katı olacaktı.

Yukarıda 1 DB sunucusundan bahsedildi. 10 pool sunucu tarafından ayarlanmış olsun. Eğer sadece 1 instance uygulama bu sunucuya istek yapıyor ise; client taraftaki ayara max ve min pool count 10 verilebilir.

yukarıdaki bahsi geçen pool sayısı sadece 1 database ve 1 app bu database'de işlem yaparsa geçerlidir. diğer durumlarda farklı connnection pool sayısı ile çalışmak daha verimli olabilir. kaynak: (source-id: 399) "brettwooldridge" comment on November 20, 2017.

# Mybatis (or old-name:IBatis)
IBatis sonlandırıldı ve isim değişikliği ile Mybatis ile geliştirilmelerine devam edildi.

Mybatis veritabanı işlemleri yapmaya yarayan Java kütüphanesi. SQL sonuçlarını Java objelerine mapping yapabildiği için ORM aracıdır. fakat JPA implementasyonu değildir.

Mybatis çok gelişmiştir:
- lazyload desteği var
- annotation desteği var
- mapping desteği var (ORM)
- XML ile önceden query'ler tanımlanıp çağrıldığı gibi, runtime'da da query'ler dinamik yaratılabilmektedir.

örnek bir işlem:

```java
Reader reader = Resources.getResourceAsReader("SqlMapConfig.xml");
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
SqlSession session = sqlSessionFactory.openSession();

session.delete("Student.deleteById", 2); // "Student.deleteById" query is declared on xml
session.commit();
session.close();
System.out.println("Record deleted successfully");
```

# JDBC driver types
JDBC'nin çalışma prensiplerini belirlemektedir. client tarafta URL girilirken type belirtilir. örnek Oracle-JDBC URL girerken şunu gireriz:

> JDBC:oracle:oci:@myhost:1521:inst1

burada "oci" Oracle'ın belirlediği bir keyword'dür. "oci", type-2'ye referans etmektedir.

- Type 1 driver – JDBC-ODBC bridge

- Type 2 driver – Native-API driver

  client tarafın direk database sunucusunun native client protokolünü kullanır. native protokolü desteklemek için saf Java kullanılmayabilir. native client sistemde yüklü bir arkaplan servisi dahi kullanabilir. bu tipin taşınabilirliği azdır.

- Type 3 driver – Network-Protocol driver (middleware driver)

- Type 4 driver – Database-Protocol driver/Thin Driver(Pure Java driver)

  OJDBC'de "thin" olarak geçmektedir. bu tipte bağlantılar %100 Java kodu kullanır. database sunucusu ile Java soketi açıp haberleşirler. %100 Java olduklarından her tarafta kullanılabilirler.

# Oracle DB version history

i, g, c keyword'leri tamamen rastgele verilmiştir. teknik anlamda hiçbir önemi yok.

| full name and version         | initial version of series | latest version of series |
|-------------------------------|---------------------------|---------------------------|
| Oracle 7.2                    | 7.2.0                     |                           |
| Oracle 7.3                    | 7.3.0                     | 7.3.4                     |
| Oracle8 Database              | 8.0.3                     | 8.0.6                     |
| Oracle8i Database             | 8.1.5.0                   | 8.1.7.4                   |
| Oracle9i Database             | 9.0.1.0                   | 9.0.1.5                   |
| Oracle9i Database Release 2   | 9.2.0.1                   | 9.2.0.8                   |
| Oracle Database 10g Release 1 | 10.1.0.2                  | 10.1.0.5                  |
| Oracle Database 10g Release 2 | 10.2.0.1                  | 10.2.0.5                  |
| Oracle Database 11g Release 1 | 11.1.0.6                  | 11.1.0.7                  |
| Oracle Database 11g Release 2 | 11.2.0.1                  | 11.2.0.4                  |
| Oracle Database 12c Release 1 | 12.1.0.1                  | 12.1.0.2                  |
| Oracle Database 12c Release 2 | 12.2.0.1                  |                           |
| Oracle Database 18c           | 18.1.0                    |                           |
| Oracle Database 19c           | 19.0.0                    |                           |

Oracle OJDBC, JDK, JDBC spesifikasyonu sürümleri uyumu için burada resmi compability matrix table'lar mevcut:

(source-id: 400)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JDBC URL form examples

# Definitions
Her DB instance, database, şema kavramları farklıdır. Örneğin;
- MongoDB'de şema kavramı opsiyoneldir.
- Oracle'da şema kavramı user kavramı ile aynıdır. Aslında şema tanımı yoktur. User'ların neleri görebileceğini tanımlarız. Yani; bir şema tanımı yapıp altına hangi DB'lerin ve diğer hangi objelerin olacağını tanımlayacağımıza, user tanımlayıp, onun nelere erişip erişmediğini tanımlamaktayız.
- MSSQL ve PostgreSQL'de şema tanımı vardır.
- MySQL'de şema ve database kavramları aynı şeydir.

# Multi database on same DB instance
- Oracle supports one database per instance unless you are working with 12c and above and using Oracle Multitenant.
- MongoDB, MSSQL, PostgreSQL, MySQL supports multiple databases per instance.

## JDBC URL password

with username pass:

```
JDBC:oracle:thin:ahmet/12345@//myhost:1111/myInsatance1
```

without username pass:

```
JDBC:oracle:oci:@myhost:1111:myInsatance1
```

# System Identifier (or SI) (For Oracle)

An Oracle DB server must have at least 1 SID per instance.

using SID:

```
jdbc:oracle:thin:@myoracle.db.server:1111:MY_SERVICE_ID
```

using service name: (service name is the label of SID)

```
jdbc:oracle:thin:@myoracle.db.server:1111/MY_SERVICE_ID
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JDBC

```java
PreparedStatement pstmt = null;
try {
   String SQL = "Update Employees SET age = ? WHERE id = ?";
   pstmt = conn.prepareStatement(SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   pstmt.close();
}
```

Her açılan JDBC connection'ı için birçok statement tanımlanabilir. statement'lerin birçok türevi vardır. statement objelerimizi bir kere oluşturduktan sonra dilediğimiz kadar üstüste aynı connection'da çağırabiliriz. statement'ler (tipine göre) gerektiğinde database-side caching'de kullanmaktadır. statement tipleri:

- Statement

```java
Statement stmt = con.createStatement();

stmt.executeUpdate("CREATE TABLE STUDENT(ID NUMBER NOT NULL, NAME VARCHAR)");
```

- PreparedStatement

```java
PreparedStatement pstmt = con.prepareStatement("update STUDENT set NAME = ? where ID = ?");
pstmt.setString(1, "ahmet");
pstmt.setInt(2, 111);
pstmt.executeUpdate();
```

- CallableStatement

store prosedürleri çağırmak için kullanılır.

```java
CallableStatement statement = con.prepareCall("{call anyProcedure(?, ?, ?)}");
//Use statement.setter() methods to pass IN parameters
statement.execute();
```

# Cancel statement

Stamement instance'ındaki query çalıştırma metodları sync çalışıyor. Bu sebeple cancel etmek için statement instance'ını başka thread'ten çağırıp cancel etmek gerekiyor.

```java
if(statement != null && !statement.isClosed()) {
    statement.cancel();
}
```

https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html#cancel() writes: "Cancels this Statement object if both the DBMS and driver support aborting an SQL statement.".