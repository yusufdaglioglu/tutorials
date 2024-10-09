############################

############################
# GROOVY
############################

############################

# Groovy language syntax
Groovy syntax başlığı, Gradle, Grovvy ile yazıldığı için önemli. sadece en temel seviyede syntax anlatılmıştır.

Groovy her sınıfa otomatik olarak aşağıdaki paketleri import eder. Bu sebeple print gibi metotlar direk çağrılıyor. bu durumun Groovy'nin syntax'ı ile bir bağlantısı yoktur.

```groovy
import java.lang.*
import java.util.*
import java.io.*
import java.net.*
import groovy.lang.*
import groovy.util.*
import java.math.BigInteger
import java.math.BigDecimal
```

# special characters calls methods
"technologies" instance'ı bir liste ise;

```groovy
technologies = technologies - 'Grails' // removes an element form list
technologies << "Groovy" // adds an element to list
```

her işaret spesific bir metotu çağırıyor. yani; technologies liste objesinde - işareti minus isimli metotu execute ediyor. biz de yaptığımız sınıfta minus metotunu - işareti ile çağırabiliriz. << gibi diğer karakterler:

> +

a.plus(b)

> a[b]

a.getAt(b)

> -

a.minus(b)

> a[b] = c

a.putAt(b, c)

> *

a.multiply(b)

> a in b

b.isCase(a)

> /

a.div(b)

> <<

a.leftShift(b)

> %

a.mod(b)

> \>>

a.rightShift(b)

> **

a.power(b)

> \>>>

a.rightShiftUnsigned(b)

> |

a.or(b)

> ++

a.next()

> &

a.and(b)

> --

a.previous()

> ^

a.xor(b)

> +a

a.positive()

> as

a.asType(b)

> -a

a.negative()

> a()

a.call()

> ~a

a.bitwiseNegate()

# closure

```groovy
class MyClass {

    def str1 = 'Merhaba'

}

static void main(String[] args) {

      def str1 = "Hello";

      def clos = { param -> println "${str1} ${param}" } //this is closure

      clos.call("World"); // prints "Hello World"

      // now we are changing the value of the String str1 which is referenced in the closure

      str1 = "Welcome";

      clos.call("World"); // prints "Welcome World";

      clos("World"); // yukarıdaki satır ile (call metotu ile) aynı görevi görüyor.

       clos.setDelegate(new MyClass() ); // we change the context of closure.

       clos("World"); //prints Merhaba World



     def myClosure2 = { println it }; // closure sadece 1 parametre alıyorsa, parametre tanımı yazılmak zorunda değildir. değişkenin ismi "it" olur.

     def myClosure3 = { String param -> println "${str1} ${param}" }; // closure'larda, istenirse parametre tipi verilebilir.

   }
```

# DSL support

```groovy
a b c d
```

yukarıdaki kod satırı bunu yapıyor:

```groovy
a(b).c(d)
```

# sadece Java ile farklılık gösteren kısımlarla bir örnek sınıf:

```groovy
class Example {

static void main(String[] args) {

//def keyword

def x = 5;  //def keywordu Java'daki Object'e denk geliyor. def istenirse kullanılır.

// range

def range = 5..10;

println(range); //prints 5 6 7 8 9 10

println(range.get(2)) // prints 7 . noktalı virgül kullanılmayabilir. yeni satır yapmak yeterli.

//for in statement

for(int i in array) {

         println(i);

}

// for statement

for(emp in employee) {

         println(emp);

}

} //end of main method

//default parameter (if parameter will not pass than default value will be used)

static void sum(int a,int b = 5) {

      //method code here

}

} //end of class
```