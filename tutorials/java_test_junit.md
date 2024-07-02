############################

############################
# JAVA TEST JUNIT
############################

############################

# JUnit
test sınıfları verilecek aşağıdaki şekilde çağrılabilir:

```java
JUnitCore junit = new JUnitCore();
Result result = junit.run(testClasses);
```

Fakat IDE'lerdeki JUnit eklentisi @test annotation'ları içeren metotların sınıflarını otomatik olarak çağırmaktadır.

# JUnit 4 vs 5
5 inci sürüm ile büyük değişiklikler oldu.

# modülerlik
JUnit 5, 3 temel parçadan oluşuyor:
- JUnit Platform: API (programatik çağrılabilmesi için arayüzler(API'ler) burada)
- JUnit Jupiter: API implementasyonu. sadece junit5 annotation'larını çağırıyor.
- JUnit Vintage: JUnit 3 and JUnit 4 kodlarını çalıştırabilmek için gerekli API implementasyonu.

Her implementasyon (Jupiter, Vintage...), "Platform" modülündeki "TestEngine" class'ını implemente eder. kaynak: (source-id: 181)

# Launcher
JUnit 5 ile; Launcher kavramı geldi. "Launcher" programatik olarak testleri class'larda arayabiliyor, filtreleyebiliyor ve yürütebiliyor. Launcher'ın programatik olarak çağrılıyor dedik, işte bu API; "JUnit Platform Launcher API" olarak isimlendiriliyor ve artifact-ID'si: "junit-platform-launcher".

# JUnit 4 integration to 5
eğer runtime'da JUnit-vintage-engine varsa; JUnit-4 annotation'ları otomatik olarak launcher tarafından bulunur ve execute edilir. hiçbir ek ayara gerek yoktur. kaynak: (source-id: 182) "3.1. Running JUnit 4 Tests on the JUnit Platform" başlığı.

Eğer junit4'ten tamamen geçilmek isteniyor ise buradakiler yapılmalıdır: kaynak: (source-id: 182) "3.2. Migration Tips" başlığı.

# TestEngine
JUnit 5 ile gelen bir kavramdır. Şu anda iki farklı TestEngine var:
- junit-jupiter-engine
- junit-vintage-engine

Her test engine implementasyonu "JUnit-platform-engine" paketindeki arayüzleri implemente etmelidir.

## annotations
| purpose of annotation                                 | JUnit 4      | JUnit 5      |
|------------------------------------------------------|--------------|--------------|
| Declare a test method                                | @Test        | @Test        |
| Execute before all test methods in the current class | @BeforeClass | @BeforeAll   |
| Execute after all test methods in the current class  | @AfterClass  | @AfterAll    |
| Execute before each test method                      | @Before      | @BeforeEach  |
| Execute after each test method                       | @After       | @AfterEach   |
| Disable a test method / class                        | @Ignore      | @Disabled    |
| Test factory for dynamic tests                       | NA           | @TestFactory |
| Nested tests                                         | NA           | @Nested      |
| Tagging and filtering                                | @Category    | @Tag         |
| Register custom extensions                           | NA           | @ExtendWith  |

## min java
Junit 4 min Java 5 isterken, JUnit 5, min Java 8 istemektedir.

## test suite'ler

JUnit 4:

```java
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({
        ExceptionTest.class,
        TimeoutTest.class
})
public class JUnit4Example
{
}
```

JUnit 5:

```java
import org.junit.platform.runner.JUnitPlatform;
import org.junit.platform.suite.API.SelectPackages;
import org.junit.runner.RunWith;

@RunWith(JUnitPlatform.class)
@SelectPackages("ExceptionTest.class", "TimeoutTest.class")
public class JUnit5Example
{
}
```

# JUnit 5 örnek kod

```java
JUnit 5 örnek test:

import org.junit.jupiter.API.AfterAll;
import org.junit.jupiter.API.AfterEach;
import org.junit.jupiter.API.Assertions;
import org.junit.jupiter.API.BeforeAll;
import org.junit.jupiter.API.BeforeEach;
import org.junit.jupiter.API.Disabled;
import org.junit.jupiter.API.Tag;
import org.junit.jupiter.API.Test;

import com.myproject.junit5.examples.Calculator;

public class AppTest {

    @BeforeAll
    static void setup(){
        System.out.println("@BeforeAll executed");
    }

    @BeforeEach
    void setupThis(){
        System.out.println("@BeforeEach executed");
    }

    @Tag("DEV")
    @Test
    void testCalcOne()
    {
        System.out.println("======TEST ONE EXECUTED=======");
        Assertions.assertEquals( 4 , Calculator.add(2, 2));
    }

    @Tag("PROD")
    @Disabled
    @Test
    void testCalcTwo()
    {
        System.out.println("======TEST TWO EXECUTED=======");
        Assertions.assertEquals( 6 , Calculator.add(2, 4));
    }

    @AfterEach
    void tearThis(){
        System.out.println("@AfterEach executed");
    }

    @AfterAll
    static void tear(){
        System.out.println("@AfterAll executed");
    }
}
```

```java
@RunWith(JUnitPlatform.class)
@SelectPackages("com.test.examples") //yada @SelectClasses( com.test.examples.ClassA.class )
@IncludePackages("com.test.example.packageB")
@ExcludeTags("PROD")
public class JUnit5TestSuiteExample
{
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
