############################

############################
# JAVA TEST MOCKITO
############################

############################

# Mockito
Java kitaplığıdır. Mockito ile mock'lanmış instance'larda aşağıdaki gibi işlemler yapabilmekteyiz:
- metottan istediğimiz değerler dönmesini sağlamaktadır
- metota kaç kez girildiğini tespit edebilmekte
- metot hangi parametre ile çağrıldığını görebilmekteyiz

# @Mock
İlgili nesnenin instance'sını oluşturmaz. onun yerine mock bir obje oluşturur.

aşağıdaki kullanım:

```java
@Mock
MyService myservice;
```

Bununla aynı işi görmektedir:

```java
MyService myservice = Mockito.mock(MyService.class);
```

Eğer annotation kullanılırsa tüm testlerden önce, annotation'un bulunduğu sınıfta aşağıdaki komutun bir kerelik çalıştırılması şarttır:

```java
MockitoAnnotations.initMocks(this);
```

Mockito, sadece mock olan instance'ların return değerlerini değiştirebilir. yani "new Person()" diyerek oluşturduğumuz bir sınıfın "run()" metotunu çağırdığımızda return değerini değiştiremez.

# @InjectMocks
Test edeceğimiz sınıfı @InjectMocks ile instance'ını oluştururuz. fakat test edeceğimiz sınıfın içindeki dependency'leri @Mock ile aynı test-class'ında tanımlarız. örnek:

```java
public class ApplicationTest
{
    @InjectMocks
    MyService myService;

    // "aClassInsideMyService" MyService içerisinde kullanılan bir sınıf olsun.
    @Mock
    AClassInsideMyService aClassInsideMyService;

    // aynı şekilde "anotherClassInsideMyService" MyService içerisinde kullanılan bir sınıf olsun.
    @Mock
    AnotherClassInsideMyService anotherClassInsideMyService;

    @Test
    public void validateTest()
    {
        boolean saved = myService.save("hello");
        assertEquals(true, saved);
    }
}
```

# org.springframework.boot.test.mock.Mockito.MockBean
Spring içinde gelen bir annotation'dur. sadece Mockito kütüphanesi için geliştirilmiştir. mock'lamak istediğimiz component'ler bu annotation ile inject etmeliyiz. Spring Bean'i olduğu için bu annotation şarttır. eğer Mockito içindeki @Mock annotation'ını kullanırsak normal bir class olarak instance yaratmış oluruz.

MockBean'de bir Bean instance'ı singleton ise; o zaman o Bean'i mock edersek, tüm yazılım sürecinde o Bean'i mock etmiş oluruz. yani JUnit test case'imizde o Bean'i çağırmadıysak bile, onu çağıracak olan farklı thread'de bu durumdan etkilenmiş olacaktır.

# @ExtendWith(MockitoExtension.class)
MockitoJUnitRunner ile aynı görevi görmektedir.

Junit 5 ile artık JUnit'e extension yazılabiliyor. Mockito'nun MockitoExtension'u bir JUnit 5 extension'udur. onu devreye almak için aşağıdaki gibi annotation'a eklemek gerekiyor:

```java
import org.junit.jupiter.API.extension.ExtendWith;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
class MyServiceTest {
  // tests
}
```

İkinci extension ise:

```java
import org.junit.jupiter.API.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = {ConfigClass1.class, ConfigClass2.class})
public class UserTest {
  // tests
}
```

SpringExtension, JUnit 5 ile Spring'i aktif etmeyi (yani Bean'leri autowire edebilmemizi) sağlıyor. @ContextConfiguration ile Spring'in hangi Bean'leri ayağa kaldıracağımızı veriyoruz.

# @Rule
JUnit 5 ile gelen bir özelliktir. Extension gibi rule'larda JUnit akışına özellik katar. MockitoExtension kullanmak istemediğimizde, onun yerine aşağıdaki rule'u aktif etmemiz yeterli olacaktır:

```java
import org.junit.Rule;
import org.mockito.junit.MockitoJUnit;
import org.mockito.junit.MockitoRule;

public class ExampleTest {

    @Rule
    public MockitoRule rule = MockitoJUnit.rule();

    // tests
}
```

# @MockitoJUnitRunner
JUnit'in "runner" özeliği var. runner'lar JUnit test akışını değiştirebilirler ve Junit'e özellik ekleyebilirler. birçok çeşit runner var. MockitoJUnitRunner bunlardan bir tanesi. MockitoJUnitRunner, @Mock gibi annotation'ların initialize edilmesini sağlıyor. bunu enable etmek için:

```java
import org.mockito.junit.MockitoJUnitRunner;

@RunWith(MockitoJUnitRunner.class)
public class ExampleTest {
    // tests
}
```

Mockito 2.2.20 öncesi "org.mockito.runners.MockitoJUnitRunner" sınıfı kullanılırken, bu sınıf artık "org.mockito.junit.MockitoJUnitRunner" olarak belirlendi. JUnit için geliştirilen sınıflar "org.mockito.junit.*" altına taşındı.

# stubbing
kelime anlamı: ağaç sökmek, saplamak

```java
public Student {

   public Solution solveProblem( Problem problem, int maxTime ){
        // any code here
   }
}
```

olsun. aşağıdaki kod ile fake data döndürebiliriz:

```java
Solution fakeSolution = new Solution();

fakeSolution.setSolution("Fake solution");

// stubbing the "solveProblem" method
Mockito.when( student.solveProblem( Mockito.any(Problem.class), Mockito.anyInt() ) ).thenReturn(fakeSolution);
```

Yukarıda "student" bir mock instance olmalıdır. yani "new Student()" ile yaratılmış bir sınıf olamaz. mock(Student.class) olmalıdır.

Mockito bir proxy instance'ı yaratıyor. örnek olarak yukarıdaki student'i mock'layalım:

```java
Student student1 = mock(Student.class);
```

student1 artık bir mock sınıftır.

Mockito proxy based bir kütüphanedir. Proxy olarak araya girer ve bir metot çağrılmadan önce ve sonra işlem yapabilir.

Mock'lama kütüphaneleri genel olarak 2'ye ayrılıyor:
- proxy based (example libs: Mockito, EasyMock, jMock)
- Bytecode manipulation (example libs: PowerMock)

# Mock vs Spy
Mockito ile mocklanan instance'lar tamamen fake'tir. örnek:

```java
Student student1 = mock(Student.class);
```

student1'den herhangi bir metot gerçekten çağrılmaz. Fakat spy'da öyle değildir. Spy gerçek instance'ın her metotunun önüne ve sonuna metotlar koyabilmemizi sağlar ve arada gerçek metot da çağrılır. örnek:

```java
List list = new LinkedList();
List spy = spy(list);
when(spy).get(0).thenReturn(anyObjectHere);
assertEquals(anyObjectHere, spy.get(0)); // burada gerçek liste elemanı çekilmeye çalışacağından ArrayIndexOutOfBoundException tarzı bir mesaj alırız.
```

Spy'a bazı yerlerde __partial mock__ da denilir.

# Stubbing consecutive calls (iterator-style stubbing)

```java
when(mock.someMethod("1"))
   .thenThrow(new RuntimeException())
   .thenReturn("hello");

//First call: throws runtime exception:
mock.someMethod("1");

//Second call: prints "hello"
System.out.println(mock.someMethod("1"));

// Any consecutive call: prints "hello" as well (last stubbing wins).
System.out.println(mock.someMethod("1"));
```

Another example:

```java
when(mock.someMethod("ABC")).thenReturn("1", "2");

mock.someMethod("ABC"); // returns 1
mock.someMethod("ABC"); // returns 2

// Any consecutive call: prints "hello" as well (last stubbing wins).
mock.someMethod("ABC"); // returns 2
mock.someMethod("ABC"); // returns 2
```

Farklı bir örnek:

```java
when(mock.someMethod("a"))
   .thenReturn("1")
   .thenReturn("2")
   .thenReturn("3");

// yada yukarıdaki aynı şu şekilde de yazılabilir:
when(mock.someMethod("a"))
   .thenReturn("1", "2", "3")
```

# ArgumentCaptor
bir metota geçilmiş parametreleri yakalayabilmemizi sağlar. örnek:

```java
//below code should be after "solveProblem" method invoked.

solveProblem("2 + 2 = ?", 2);
solveProblem("3 + 3 = ?", 3);

ArgumentCaptor<Problem> argumentCaptor = ArgumentCaptor.forClass(Problem.class);
Mockito.verify(student).solveProblem(argumentCaptor.capture());

argumentCaptor.getValue().getProblemDetails() // returns "2 + 2 = ?"

// the solveProblem called multiple time. Therefore:
Problem problemArg1 = argumentCaptor.getAllValues().get(0); // returns "2 + 2 = ?"
Problem problemArg2 = argumentCaptor.getAllValues().get(1); // returns "3 + 3 = ?"
```

# verify
aşağıdaki kod "solveProblem" metotunun 2 kere çağrılıp çağrılmadığını kontrol ediyor. Eğer 2 kere çağrılmamış ise hata fırlatıyor.

```java
Mockito.verify(student, new Mockito.Times(2)).solveProblem( any(Problem.class), anyInt());
```

Yukarıdaki metot 2 kere yandaki parametrelerle çağrılmasını beklediğini betimliyor. Yani aynı test akışı (thread'i) içerisinde aşağıdaki satırları yazabiliriz:

```java
// solveProblem metodu myProblemInstance1 instance'ı ve 3 ile birlikte 2 kere çağrıldı.
Mockito.verify(student, new Mockito.Times(2)).solveProblem(myProblemInstance1, 3);

// solveProblem metodu totalde 3 kere Problem class'ı ve herhangi bir integer alarak çağrıldı.
Mockito.verify(student, new Mockito.Times(3)).solveProblem( any(Problem.class), anyInt());

// solveProblem metodu totalde 3 kere çağrıldı (ne parametre aldığı önemli değil).
Mockito.verify(student, new Mockito.Times(3))
```

# doAnswer

```java
// normalde AtomicBoolean kullanmaya gerek yok. fakat aşağıda anonim fonksiyon içerisinden bu boolean'ın çağrılması gerekiyor. bu sebeple AtomicBoolean şart.
AtomicBoolean methodInvoked = new AtomicBoolean(false);

doAnswer(invocation -> {

    Message m1 = (Message) (invocation.getArguments()[0]);

    if (methodInvoked.get()) {
        return "message already sent";
    } else {
        methodInvoked.set(true);
        someObject.httpMessageSend(m1);
        return "message sent"
    }
}).when(client).sendMessage(Mockito.any()); // or any "Message" class
```

# answer

```java
when(client.sendMessage(Mockito.any()).thenAnswer(
    new Answer() {
        public Object answer(InvocationOnMock invocation) {

              Message m1 = (Message) (invocation.getArguments()[0]);

              if (methodInvoked.get()) {
                  return "message already sent";
              } else {
                  methodInvoked.set(true);
                  someObject.httpMessageSend(m1);
                  return "message sent"
              }
}});
```

# thenReturn vs doReturn vs doAnswer

- thenReturn sadece dönüş parametresini değiştirebiliyor.

- doReturn'de type yoktur. bu sebeple runtime sırasında dönüşün tipine karışmaz. bu önceden bilinmeyen sınıflar/metotlar (örneğin runtime'da yüklenen plugin'ler) gibi sistemleri test etmek için idealdir. bu nadir bir durum olduğundan; genelde doReturn yerine thenReturn kullanılır. doReturn örneği:

  ```java
  doReturn(true).when(user).getName()); // normalde name string döner, fakat doReturn ile true döndürdük.
  ```

  Fakat doReturn; spy instance'larda gerçek metotun çağrılmasını engeller. bu şekilde spy'larda gerçek metotun çağrılmasını engellemiş oluruz. örnek:

  ```java
  List list = new LinkedList();
  List spyList = spy(list);

  // at this line of code, the real method is calling.
  // this line throws IndexOutOfBoundsException (because the list is yet empty)
  when(spyList.get(0)).thenReturn("foo");

  // You have to use doReturn() for stubbing.
  // if you check below code, you will see that we pass only 'spyList' object as parameter. we don't call the real 'get' method. for that reason doReturn-when and when-thenReturn and some other Mockito methods have different order.
  doReturn("foo").when(spyList).get(0);
  ```

- doAnswer'ın aldığı Answer class'ının answer metotu, mock edilen metota gelen parametreleri de okuyabilmemizi ve dönüşünü ona göre değiştirebilmemizi sağlıyor.
