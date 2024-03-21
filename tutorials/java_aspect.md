############################

############################
# JAVA ASPECT
############################

############################

# Spring-aop
Spring security web servise gelen istekleri kontrol ederken, loggining mekanizması debug log'ları atarken, hep AOP'den yararlanır. Spring-AOP, ona alternatif olan AspectJ'ye göre çok basit kalır.

AOP dünyasında genel terminoloji şu şekildedir:

- __Aspect__: tüm tanımlamaların (pointcuts, aspects...) olduğu kümedir (class'tır). Spring'deki @Aspect annotation'unu attığımız class'a denk gelir.

- __Joinpoint__: aspect'in gireceği event'e denir. örneğin; "metot çağrılmadan önce", "exception fırlatıldıktan sonra" event'leri joinpoint'tir. yani; aspect'in join olacağı noktadır.

- __Advice__: araya girecek yeni kod bloğumuza (fonksiyona) denir.

- __Pointcut__: hangi aspect'in hangi joinpoint'te çalışacağını belirten tanımlamadır (expression'dır).

- __Target Object__: advice'ın uygulandığı objeler/class'tır. örneğin; "Person" class'ının "die()" metotuna bir advice atamış olalım. bu durumda bu advice çalıştığında, target-objemiz "Person" class'ı olur.

# AspectJ kullanımı

```java
import org.aspectj.lang.*;

@Aspect
public class MyLoggingAspect
{
    @Before("execution(# com.mycustompackage.MyService.myMethod(..))")

    public void beforeMyMethod(JoinPoint joinpoint)

    {
        System.out.println("Before "+joinpoint.getSignature().getName());
    }

    @AfterThrowing(

            pointcut="execution(# com.mycustompackage.MyService.myMethod(..))",

            throwing="exception")

    public void afterMyMethodThrowException(JoinPoint joinPoint,Throwable exception)
    {
        System.out.println("After throw exception");
        System.out.println("Exception: "+exception);
    }
}
```

diğer bazı annotation'lar:

- AfterReturning - it returns (does not throws an exception)

- After -> return or throws an exception

aspect işlemleri Java-core'da desteklenmediği için bazı trick'lerle gerçekleştirilir. bu trick'lere __weawing__ denir.

weawing işlemi 3 farklı şekilde yapılabilir:

- derleme esnasında. Java'da bir proxy sınıf yaratılır. öncesinde ve sonrasında çalışacak metotlar burada çağrılır.

- özel bir classloader ile yüklenecek sınıfın Bytecode'u ile oynanır

- runtime sırasında AOP modülü dinamik olarak bir proxy sınıfı üretir. Spring-aspect modülü bu şekilde çalışır.

# Pointcut
aspect metotlarını gruplandırmak için kullanılır. direk örnek üzerinden gidersek;

```java
@Aspect
public class Audience {

  @Pointcut("execution(public String com.mypackage.myaction(Long) )")
  public void performance() {}

  @Before("performance()")
  public void myMethod1() {
    //do something
  }

  @Before("performance()")
  public void myMethod2() {
    //do something
  }

  @AfterReturning("performance()")
  public void myMethod3() {
    //do something
  }
}
```

YUkarıda before, AfterReturning gibi tüm metotlar aynı sınıfta olan Pointcut metotu performance'ı işaret ediyor. böylece Pointcut'ta bir değişiklik olduğunda tüm aspectlerin aksiyon alması gereken nokta tek bir yerden belirlenmiş oluyor.

Yukarıda performance() metotu tanımlamasına (annotationu ile birlikte) __pointcut signature__ deniliyor.

# pointcut designator (or PCD)

@Pointcut içerisindeki tanımlaya verilen isimdir. Birçok PCD vardır. örnek:

Aşağıdaki PCD, public/private ve protected olan tüm Class1 sınıfı içindeki metotları kapsamaktadır.

> @Pointcut("execution(* com.package.Class1.*(..))")

Aşağıdaki PCD, sadece Long alan findById metotunu kapsamaktadır.

> @Pointcut("execution(public String com.package.Class1.findById(Long))")

Aşağıdaki PCD, Class1 içindeki tüm metotları kapsamaktadır.

> @Pointcut("within(com.package.Class1)")

Aşağıdaki PCD, find ile başlayan ve sadece 1 adet long parametresi kabul eden tüm metotları kapsamaktadır:

> @Pointcut("execution(* *..find*(Long))")

Aşağıdaki PCD, find ile başlayan ve ilk parametresinde long kabul eden tüm metotları kapsamaktadır:

> @Pointcut("execution(* *..find*(Long,..))")

Aşağıdaki PCD, Repository annotation'u içeren sınıfın tüm metotlarını kapsamaktadır.

> @Pointcut("@target(org.springframework.stereotype.Repository)")

Aşağıdaki PCD, Entity annotation'unu içeren nesneleri parametre olarak kabul eden tüm metotları kapsamaktadır

> @Pointcut("@args(org.myapp.aop.annotations.Entity)")

Aşağıda iki adet PCD birleştirilmiştir:

```java
@Pointcut("@target(org.springframework.stereotype.Repository)")
public void repositoryMethods() {}

@Pointcut("execution(* *..create*(Long,..))")
public void firstLongParamMethods() {}

@Pointcut("repositoryMethods() && firstLongParamMethods()")
public void entityCreationMethods() {}
```

# @Around
bir Pointcut'ı referans etmiş tüm aspect metotlarını harmanlayabiliriz.

direk örnek üzerinden gidelim:

```java
@Aspect
public class Audience {

  @Pointcut("execution(*# concert.Performance.perform(..))")
  public void performance() {}

  @Around("performance()")
  public void watchPerformance(ProceedingJoinPoint jp) {

        System.out.println("before all aspects");
        jp.proceed();
        System.out.println("after all aspects");
  }
}
```

# handling parameters
aspect ettiğimiz metota geçilen parametreleri de alabiliriz:

```java
@Pointcut(
"execution(# soundsystem.CompactDisc.playTrack(int)) " +
"&& args(trackNumber)")
public void trackPlayed(int trackNumber) {}

@Before("trackPlayed(trackNumber)")
public void countTrack(int trackNumber) {

    //do something with trackNumber
}
```