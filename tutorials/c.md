############################

############################
# C
############################

############################

# C (language)

# Preprocessor directives
directive'lere compiler dünyasında "__pragma__" da deniliyor. directive'ler, ilgili programlama dilinin syntax'ında olmak zorunda değiller. bu sebeple compiler'dan compiler'a farklılıklar söz konusu olabilir.

derleme öncesi komutlardır. derleme aşamasın başlamadan kod üzerinde manipülasyon sağlarlar. 

C'deki bazı direktifler:

- ## include

ilgili kod dosyasını, include yazdığımız satıra ekler. kodların kopyasını ekler, referans etmez.

```c
//iki farklı syntax vardır:

#Include <DosyaIsmi.H>

#Include "DosyaIsmi.H"
```

Header dosyası eklersek fonksiyonların implementasyonları gelmez. çünkü onlar header dosyasında değildirler. (header dosyası ve build, link gibi fazların olduğu başlıkta bu anlatılıyor).

- ## define

sabit bir değer veya fonksiyon yerine alias kullanabilmemizi sağlar. bu şekilde __macro (or makro)__ tanımlamış oluruz.

```c
#Define YARI_CAP 987
```

ile sabit değerler kullanılabilir. aynı zamanda fonksiyon da tanımlanabilir:

```c
#Define  buyuk(a,b)   (  (a>b) ? a:b  )
```

- ## undefine

define ile tekrar tanımlama yaparsak hata alırız. eğer değeri güncelleyecek ise önce undefined yapmak zorundayız.

```c
#Define X 1
#Undef X
#Define X 2
```

- ## If, Elif, Else, EndIf

```c
#If (sizeof ( int ) == 2)
#Define MAKINA "16 bitlik isletim sistemi"
#Elif (sizeof ( int ) == 1)
#Else
#Define MAKINA "unknown bitlik isletim sistemi"
#Endif

main() {
  printf(MAKINA);
}
```

- ## ifdef, ifndef

Eğer önceden define edilmemiş ise aşağısındaki if bloğunun çalışmasını sağlar.

```c
#Ifndef PI
#Define PI "3"
```

- ## error

derleyicinin hata vermesini ve durmasını sağlar

```c
#if (size of ( int ) = = 2 )
#Error bu program ( 16 bit'lik ) bu işletim sisteminde çalışmaz!
#Endif
```

# literals

some examples:

```c
85         /* decimal */
0213       /* octal */
0x4b       /* hexadecimal */
30         /* int */
30u        /* unsigned int */
30l        /* long */
30ul       /* unsigned long */
3.14159       /* Legal */
314159E-5L    /* Legal */
510E          /* Illegal: incomplete exponent */
210f          /* Illegal: no decimal or exponent */
.e55          /* Illegal: missing integer or fraction */
```

# bitwise operations

```c
unsigned int a = 60; /* 60 = 0011 1100 */
unsigned int b = 13; /* 13 = 0000 1101 */
int c = 0;

c = a & b;       /* 12 = 0000 1100 */

c = a | b;       /* 61 = 0011 1101 */

c = a ^ b;       /* 49 = 0011 0001 */

c = ~a;          /*-61 = 1100 0011 */

c = a << 2;     /* 240 = 1111 0000 */

c = a >> 2;     /* 15 = 0000 1111 */


```

# sizeof

sizeof(3); ile byte cinsinden uzunluk alınabilir. burada 3 bir int olduğundan integer uzunluğu dönecektir.

benzer şekilde: sizeof(int); komutu bir integer biriminin uzunluğunu dönecektir.

# bellek adresi

&myValue şeklinde adres bilgisi alınır. bu logical adres'tir (başka başlıkta anlatılıyor).

Java'da bu adresi elde etmenin hiçbir mantığı yoktur. çünkü garbage collector (veya JVM'in farklı bir özelliği) verimliliği arttırmak için veya herhangi farklı bir sebepten, bellekteki data'ların logical adreslerini değiştirebilir. bu sebeple bu değere güvenip işlem yapmamız yanlış olacaktır.

# pointer (or işaretçi)

myValue'nun başlangıç adresi de bir yerde tutulmaktadır. bu yere pointer denilmektedir.

örnek kullanım üzerinden incelersek;

```c
int *pointerSayi; //yıldız işareti; karakteri yönlendirme operatörü (or indirection operator) olarak adlandırılır

int sayi = 99;

pointerSayi = &sayi; // bu satır işlendikten sonra; artık pointerımız sayının adresinin tutmaktadır.
```

# array

arrayler sırası ile memory'de aynı tipte veri tutmaktadır. memoryde aralarında hiçbir bilgi bulundurmaz.

```c
dizi;
```

ile

```c
&dizi[0];
```

aynı bilgiyi döndürür. bu C'de arrayler için geliştirilmiş bir syntax'tır.

arraylar üzerinde adresleri ile gezinebiliriz. adresler ile gezinirken pointerları kullanmamız gerekir. çünkü arraylarda pointer'ı 1 arttırdığımızda, array'in tipi kadar byte arttırır. örneğin integer tutan bir array'in pointer'ını arttırırsak;

```c
pointer = pointer + 5;
```

aşağıdaki ile aynı şeyi yapmış oluruz.

```c
pointer = pointer + 5*sizeof(int);
```

## fonksiyona parametre geçme

aşağıdaki 1 ve 3üncü yöntemlerde size'ı farklı bir parametrede almak gerekli.

```c
void myFunction(int *param) {
   .
   .
   .
}

void myFunction(int param[10]) {
   .
   .
   .
}

void myFunction(int param[]) {
   .
   .
   .
}
```

# bellekte yer ayırma

__Malloc__ bellekten yer ayırmamızı söyler işletim sistemine:

```c
int *dizi;

dizi = dizi=(int*) malloc(20*sizeof(int));
```

__calloc__ da aynı görevi görür. calloc, mallac'a ek olarak tüm yer ayrılan bölgeye gerçekten 0 değerini atar. bu şekilde işletim sisteminin memory'yi tahsis etmekten ziyade, o bölgeyi gerçekten de fiziksel olarak hazır ettiğini garanti eder. tabi performans olarak daha yavaştır.

__free(dizi);__ şeklindeki satır ise dizi için tahsis edilmiş tüm alanı serbest bırakır.

__realloc__ ise önceden zaten tahsis edilmiş alana ek yeni alan tahsis eder:

```c
yeniAlan = (int*) realloc(eskiAlan, 100*sizeof(int));
```

bu fonksiyonlar "cstdlib" kütüphanesinin içindedir. C++ ta da aynı kütüphane kullanılarak işlem yapılabilir. Fakat C++'ta ek olarak, gömülü olarak, delete operatörü geliyor. "delete" ile alan silme işlemini yapabiliyoruz. C++'ta "new" ile zaten yeni alan yaratılıyor. delete'nin kullanımı array ve tek bir nesne silip silmediğimize göre değişiyor:

```c++
delete pointer_name

delete[] pointer_name_of_array
```

# goto

```c
one:
for (i = 0; i < number; ++i)
{
    test += i;
    goto two;
}

two:
if (test > 5) {
   //do something
}
```

# function(void) vs function()
function(void) ekstra parametre kesinlikle kabul etmezken (derleyici hata fırlatır), function() istenirse parametre ile çağrılabilir. fakat bu parametreler önemsizdir çünkü hiç kullanılmazlar.

# kod işleme sırası

```c
#include "main.h"

int main()
{
    function1("Hello");
}

int function1(char name)
{
    //do something
}
```

Yukarıdaki kod için derleyiciler hata verir. C dilinde main'den önce ilgili metot tümüyle tanımlanmalıdır yada sadece prototipi tanımlayabiliriz:

```c
int function1(char name); //bu tanımlamaya "prototype declaration" denir. fonksiyonun prototipini tanımlamış oluruz.
```

# string

string diye bir tip yoktur. C "hello" yazdığımızda otomatik olarak char array yaratır. ve sonuna null karakterini koyar. aşağıdaki değerler aynıdır.

```c
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
// \0 null karakteri olarak kabul edilir.

char greeting[] = "Hello";
```

# struct

```c
struct Book {
   char  title[50];
   int   book_id;
};

int main() {
   struct Book book1;
   book1.title = "hello";
}
```

isteğe bağlı instance'ler yaratılabilir:

```c
struct Book {
   char  title[50];
   int   book_id;
} book1, book2, book3={"book3 name", 3};
```

İsteğe bağlı isimsiz struct'ta yaratılabilir:

```c
struct {
   char  title[50];
   int   book_id;
} book1, book2, book3={"book3 name", 3};
```

Pointer'lar aracılığı ile çalışmamız gerektiğinde;

```c
struct Book *struct_pointer;
struct_pointer = &book1;
char[] myStr = struct_pointer->title;
```

Aynı şekilde ilk başta da instance'ların pointer olup olmayacağını belirleyebliriz:

```c
struct {
   char  title[50];
   int   book_id;
} book1, *book2, book3={"book3 name", 3};

//Yukarıda sadece book2 pointer'dır.
```

## stuct üzerinde bit bazında bellek ayırma

struct yapısında tüm değeri değilde, sadece kaç bit kullanacağımızı da belirtebiliyoruz:

```c
struct myStruc1 {
   unsigned int a : 1;
   unsigned int b : 1;
};
```

yukarıda a değerinde sadece 1 bitlik yer kullanacağımızı belirtmiş olduk. aynı şey b içinde geçerli.

sizeOf(myStruc1) = 4 (byte). fakat 32 tane "unsigned int" olsaydı yine 4 byte yer kaplayacaktı.

```c
struct {
   unsigned int age : 3;
} Age;

int main( ) {

   Age.age = 4;
   printf( "Sizeof( Age ) : %d\n", sizeof(Age) );
   printf( "Age.age : %d\n", Age.age );

   Age.age = 7;
   printf( "Age.age : %d\n", Age.age );

   Age.age = 8;
   printf( "Age.age : %d\n", Age.age );

   return 0;
}

/*
output:

Sizeof( Age ) : 4
Age.age : 4
Age.age : 7
Age.age : 0
*/

```

## Struct'ların bellekteki yerleri

C'de struct'lar memory'de ardarda kaydedilir. bu sebeple arada boşluklar oluşur. çünkü CPU'dan en az word uzunluğunda byte okunabilir. eğer struct içindeki bir verinin boyutu word'den küçük ise, boşluk kalabilir.

örnek üzerinden gidelim:

32 bitlik bir CPU'da boyutlar byte cinsinden şu şekilde olur:

```c
struct structure1
{
   int num1;      //4

   int num2;      //4

   char char1;    //1
   char char2;    //1
                  //2 byte boş kalır

   float float1;  //4 bytes
};

struct structure2
{
   int num1;      // 4

   char char1;    //1
                  //3 byte boş kalır

   int num2;      //4

   char char2;    //1
                  //3 byte boş kalır

   float float1;  //4 byte
};
```

- structure1 total: 16 byte harcıyor. oysa beklenen 14 byte'tı. CPU word sebebi ile böyle oldu. 2 byte boşta kaldı.

- structure1 total: 20 byte harcıyor. oysa beklenen 14 byte'tı. CPU word sebebi ile böyle oldu. 6 byte boşta kaldı.

programın tepesine __#pragma pack(1)__ yazılırsa; C derleyicisi boşluklar olmayacak şekilde structure'yi oluşturuyor. bu durum programın bellekten daha az yer harcamasına sebep olurken, performans olarak dezavantaj sağlıyor. çünkü sıkıştırılan bilgiler birden fazla word'ün içine saklanıyor, buda her defasında CPU'dan daha fazla data okunmasına ve bu bilgileri parse edilmesine sebep oluyor.

Bu konu C'de "structure padding" yada "struct allign" olarak geçiyor.

Her derleyici ve CPU-tipi'ne göre farklı padding yöntemleri kullanılabilir.

Struct içindeki pointer'lar belli bir yer kaplar. örneğin 32 bit için 4 byte. Pointer'ın işaretlediği yerler struct'ın boyutuna dahil değildir. Fakat fixed-size array'lar pointer'lardan farklıdır. fixed-size Array içindeki bilgiler struct'ın içindedir.

Eğer sadece bir struct'taki padding'leri yoketmek istiyorsak şunu yazmalıyız:

```c
struct __attribute__((packed)) struct1 {

    int data1;

};
```

Eğer allign boyutunu kendimiz belirlemek istiyorsak:

```c
__attribute__((packed, aligned(4)))
```

olarak set edebiliriz.

Eğer tüm programda allign değerini set etmek istiyorsak, programımızın tepesine #pragma pack(push, 4) yazmalıyız. Eğer sadece belli struct'lara bu allign değerini set etmek istiyorsak:

```c
#pragma pack(push, 4)

//Structure 1
......

//Structure 2
......

#pragma pack(pop)
```

# union
struct ile syntax'ı aynıdır. fark şudur: union'da union içindeki instance'tan sadece 1 eleman kullanılabilir.

```c
union foo {
  int a;   // can't use both a and b at once
  char b;
};

struct bar {
  int a;   // can use both a and b simultaneously
  char b;
};

union foo x;
x.a = 3; // OK
x.b = 'c'; // bu beklediğimiz gibi çalışmaz. x.a = 'c' olarak çalışır. "union" yapılarında ilk atama nereye yapıldıysa o değer artık referans alınır.

// 'c' ascii tablosunda 99'a karşılık gelir.
// dolasyı ile artık bu eşitlik vardır: x.a = x.b = 01100011 (decimal:99)
```

Union'daki her değer bellekte aynı yerde tutulur. Dolayısı ile bir union'un kapladığı yer, içindeki elemanlara bağlıdır. içindeki elemanların en çok yer ayıranı kadar yer ayrılır. örneğin; foo union'umuz içinde char ve int var. char 1 byte. int daha büyük boyutlu olduğu için, foo union'u int boyutunda olacaktır.

Union genelde şu durumda kullanılır: sayısal bir değer birden fazla tip (primitive) ile farklı amaçla kullanılabilir. bu gibi durumlarda atamamızı yaptıktan sonra o değeri istediğimiz primitive tipte kullanabilir. örneğin "100" char[] olarakta, belki de özel string işlemlerimiz sırasında int olarak ihtiyacımız olacak.

# typedef

Aşağıdaki örnekte newType bir alias'tır. örneğin istediğimiz yerde "int" yerine bu alias'ımızı kullanabiliriz. benzer şekilde struc yada union'lara da alias ile ikinci isim takabiliriz.

```c
typedef int newType;

void main (void)
{
  newType myVar;

  myVar = 9;

  printf("%d", myVar);
}
```

C'de tiplerin boyutları cihazdan cihaza değişeceği için, her cihazda farklı tip kullanılabilir. bunlar pre-compiler (#ifdef... gibi) belirlenebilir. bu sebeple kodun içerisinde; her tip için (en temel tipleri için dahi) önceden belirlenmiş alias'lar kullanılmalıdır.

- typdef kullanımlarına örnekler:

  - örnek 1:

    ```c
    typedef struct {
        int x;
    } X;
    ```

    bu örnekte;
    - C case-sensitive olduğundan x ve X birbirine karışmamaktadır.
    - X alias'lı isimsiz bir struct yaratılmıştır. Bu durum tüm kod tek satıra indirgenince daha kolay anlaşılmaktadır:

      ```c
      typedef   struct{ int x; }   X;
      ```

    - C'ye yeni başlayanlar X'i değişken olması ile karıştırabiliyorlar. oysa X bir değişken değil, çünkü satırın en başında typedef kullanıldı.

  - örnek 2:

    ```c
    typedef struct A {
        int x;
    } A;
    ```

    bu örnekte;

    - örnek 1'deki struct, isimli olarak yaratıldı ve ismi ile alias'ı A olarak verildi.

    - Artık fonksiyonlarda bunu parametre olarak istediğimizde bu şekilde yazmamız yeterli olacaktır:

      ```c
      void myFunc( A param1, int param2)
      ```

      Oysa örnek 1'deki tanımı bu şekilde parametre olarak yazmak zorundaydık:

      ```c
      void myFunc( struct A param1, int param2)
      ```

# hata yönetimi

C'de try-catch tarzı bir mekanizma yoktur. (C++'ta ise try/catch'lar vardır). eğer çağırdığımız bir fonksiyon -1 yada null dönerse errno değerini kontrol ederiz.

```c
#include <stdio.h>
#include <errno.h>
#include <string.h>

extern int errno ;

int main () {

   FILE * pf;
   int errnum;
   pf = fopen ("unexist.txt", "rb");

   if (pf == NULL) {

      errnum = errno;
      fprintf(stderr, "Value of errno: %d\n|", errno);
      perror("Error printed by perror");
      fprintf(stderr, "Error opening file: %s\n", strerror( errnum ));
   } else {

      fclose (pf);
   }

   return 0;
}

/*
output:

Value of errno: 2
Error printed by perror: No such file or directory
Error opening file: No such file or directory
*/
```

- perror aldığı string parametrenin yanına, o anda error no'ya denk gelen error string'i basıyor.

- strerror aldığı errorNo tamsayısına denk gelen error String'inin pointer'ını dönüyor.

Sıfıra bölünme hatasında uygulama kendini kapatacaktır. Bu sebeple önceden bölenin sıfır olup olmadığını manuel kontrol etmemiz gereklidir. Burada süreç şu şekilde işler:
- CPU işlemi gerçekleştiremez ve hata fırlatır
- OS bu hatayı yakalar ve yazılıma sinyal gönderir veya process kill edilebilir. bu OS'a kalmış bir durumdur. bazı OS'lar, process'e SIGFPE (or Signal Float Point Exception) sinyali yollar. bu sinyali handle edersek, handler fonksiyonumuz execute edildikten sonra, aynı satır (sıfıra bölünme satırı) tekrar işletilir. (handler bittiğinde neden devam ettiğini POSIX sinyalleri konusunda anlatılıyor).

Tüm sistemlerde kullanılan resmi standart error değerleri yoktur. Her sistem kendi standartlarını yayımlayabilir. İki örnek ele alalım: [file:"c_error_codes.md"](./c_error_codes.md)

# static

bir değerin önüne static koyulursa o değer tüm program boyunca tekrar initialize edilmez. aşağıdaki örnekte "count" sadece 1 kere initialize olur.

```c
#include<stdio.h>
int fun()
{
  static int count = 0;
  count++;
  return count;
}

int main()
{
  printf("%d ", fun());
  printf("%d ", fun());
  return 0;
}

/*
output:

1 2
*/
```

# register

bir değerin önüne register keyword'ü koyulursa o değer RAM'de değil, CPU'nun register'larında saklanır. bu da o değere daha hızlı erişilmesini sağlar. fakat register'lar maliyetli fiziksel bellekler olduğundan, mümkün olduğunca az kullanılmalıdırlar.

Eğer register'da yer yok ise bu değer RAM'de oluşturulur ve hata fırlatmaz.

register'da saklanan değerlerin & operatürü ile adresini almamız mümkün değildir. çünkü RAM adresleri yoktur.

# extern

extern bir değerin önüne atılırsa, o değer artık aynı isimde olan tüm değerlerle aynı yere referans edecektir. özellikle farklı kaynak dosyaları ile çalışıyorsak ve başka kaynak dosyanın global bir değişkenini değiştirmek istediğimizde extern iş görecektir.

diğer kaynak dosyasındaki değere referans eden değeri aynı isimde kendi kaynak dosyamızda oluştururuz. önüne extend koyarsak bu değer artık runtime sırasında aynı yere referans edecektir. "errno" değeri (konu başka başlıkta anlatılıyor) extern'dir ve extern'in kullanımı için çok iyi bir örnektir.

# auto

her değişkenin önüne eklenebilir fakat eklenmesede default identifier olduğu için etki etmeyecektir.

# const
sabit değerler yaratabilmek için kullanılır. Java'daki final identifier'ına benzer.

# neden fonksiyondan döndüreceğimiz değerleri memory allocation ile ayırlmalıyız

aşağıdaki örnekten gidelim:

```c
#include<stdio.h>
int * createArray() {
  int arr[10];
  return arr;
}

int main() {
  int * arr = createArray();

  arr[2] = 1; // bu satırda segfault hatası alırız

  return 0;
}
```

Yukarıdaki kodda segfault alırız. çünkü; createArray içindeki arr değerinin adresini döndürdük fakat fonksiyon tamamlanınca, createArray __stack frame__'i bellekten siliniyor. bu sebeple oraya main fonksiyonumuzdan erişmeye kalkdığımızda hata alırız. bu sebeple createArray metotunda heap'ten ayıracağımız array'i döndürmeliyiz.

aynı durum alt metotlara dallandığımızda parametre gönderdiğimizde geçerli değildir. çünkü alt metota dalışımızda, dalmadan önceki stack frame hala ramde kalmaya devam edecektir.

# enum

enum; enumeration'ın kısaltması olarak programlama dünyasına yansımıştır. enumeration; birer birer saymak anlamına gelmektedir.

```c
//tanımlama
enum state {
  working = 98,
  stoped = 99
};

//kullanımı
enum state personState;
personState = stoped;
```

İstenirse enum'lardaki gibi instance tanımlama satırında yapılabilir:

```c
//tanımlama
enum state {
  working = 98,
  stoped = 99
} personState;

//kullanımı
personState = stoped;
```

Eğer enum variable'larına değer verilmezse otomatik olarak 0, 1, 2 olarak set edilir. örnek:

```c
//tanımlama
enum state {
  working,  // working = 0
  stoped    // stoped = 1
} ;
```

# fork()
unistd.h içerisinde olan fonksiyondur.

fork() fonksiyonu system çağrısı yapar. yeni bir process yaratılır. bu yei process'in bellek ve açılmış tüm thread'leri aynen kopyalanır.

yeni process, parent process'in child process'i olmaktadır.

fork fonksiyonunun dönüş değeri:

- negatif ise; child process yaratılamamıştır (hata durumu)
- parent process'te isek, fork fonksiyonu, child process'in process ID'sini döner.
- child process'te isek burası 0 döner. böylece child process'te olduğumuzu anlarız. (child process'te aynı bu satırdan çalışmaya devam eder, zaten memory değerleri parent process ile aynıdır. yani child process main fonksiyonundan başlamaz.). Not: child process kendi process ID'sini getpid() fonksiyonu ile öğrenebilir.

fork fonksiyonu var olan process'i aynen clone'luyor gibi görünsede bazı istisnalar vardır. bu istisnalar: (source-id: 456) title: "DESCRIPTION", ilk maddeler.

ek not: alt sevyeli programlama dünyasında, unofficial olarak; process'lere "__heavyweight process__", thread'lere ise "__lightweight process__" denir.

# vfork
fork ile benzer yapıdadır. fakat child process başlar ve parent beklemeye alınır. child'ın işi bitince parent process çalışmaya devam eder.

vfork aynı address space'i üzerinde işlem yaptırır. her 2 process'e ayrı address space'i ayırmaz.

# clone
bu fonksiyon fork ile aynı işlevi görüyor fakat ekstra özellikler sunuyor. en önemli özelliği ise; yeni yaratılan process'in farklı namespace'lerde yaratılmasını sağlıyor.

# exec
unistd.h içerisinde olan fonksiyonlar grubudur. birçok fonksiyon var:

- execl
- execlp
- execle
- execv
- execvp
- execvpe

 aldıkları parametrelere göre isimleri değişiyor. temel 3 parametre geçiliyor:
- executable file path
- arguments (for the main process of the new process)
- environment variables for the new process

tüm exec grubu fonksiyonları aynı işlevi görüyor. sadece parametre yapısı farklı. bazı fonksiyonlar argümanları ayrı ayrı alırken, bazı fonksiyonlar array olarak tek parametrede alıyor gibi...

exec komutu çağrıldığında, var olan process-id ile yeni process replace edilir. yani, var olan process devam etmez. fakay yeni process-id oluşturulmaz.

exec komutunun return değerine gerek yoktur. fakat sadece process oluşturulamadığı durumda (hata durumunda) -1 dönüş yapar. hatanın detayı için errno değerine bakmak gerekmektedir (errno başka başlıkta anlatılıyor).

# starting process: POSIX vs Microsoft Windows
bu başlık için tüm kaynak: (source-id: 457)

exec ve fork fonksiyonları POSIX'lerde çalışabiliyor. Microsoft Windows için bu 2 fonksiyonu kullanabiliriz ve arkaplanda POSIX'tekilerden çalışma mantıkları farklı:

- CreateProcess(filename) - starts a brand new process for a given executable.
- ShellExecute(Ex)(command) - starts a shell (yep, Microsoft Windows also has a shell concept) process to execute provided command.

POSIX'lerde diğer exec ve fork'tan farklı 2 fonksiyon daha var, fakat bunlar arkaplanda fork ve exec fonksiyonlarını wrap ediyorlar.

- system(command)

  komutu çalıştırıyor ve bitmesini bekliyor (blocking).

  arkaplanda kendini fork ediyor ve exec ile fork olan process'i replace edip isteidğimiz komut çalıştırıyor.

  C kodundaki implementasyona bakarsak shell komutu çalıştırdığını görebiliriz çünkü sadece POSIX uyumlu:

  ```c++
  execl("/bin/sh", "sh", "-c", command, (char *)NULL);
  ```

- popen(command, mode)

  mode: r yada w verilebiliyor. mode'a göre açılan process'in stdin veya stout'larını kontrol edebiliyoruz.

  popen arkaplanda vfork ve exec kullanıyor.

# thread

pthread; POSIX thread'in kısaltmasıdır ve (pazarı yüksek olan OS'lardan sadece) Microsoft Windows'ta bunun implementasyonu mevcut değil. Microsoft Windows'ta "windows.h" import edilir ve bunun içindeki CreateThread gibi fonksiyonlar ile işlem yapılır.

```c
#include <pthread.h>
#include <unistd.h> // for sleep() only
#include <stdio.h> // for printf() only

void wait(void)
{
    sleep(2);
    printf("Done.\n");
}

int main(void)
{
    pthread_t thread;
    int err;

    err = pthread_create(&thread, NULL, wait, NULL);

    if (err)
    {
        printf("An error occured: %d", err);
        return 1;
    }

    printf("Waiting for the thread to end...\n");

    pthread_join(thread, NULL);

    printf("Thread ended.\n");

    return 0;
}
```

# mutex

```c
#include <pthread.h>
#include <unistd.h>
#include <stdio.h>

pthread_mutex_t lock;
int j;

void do_process()
{
    pthread_mutex_lock(&lock);
    int i = 0;

    j++;

    while(i < 5)
    {
        printf("%d", j);
        sleep(1);

        i++;
    }

    printf("...Done\n");

    pthread_mutex_unlock(&lock);
}

int main(void)
{
    int err;
    pthread_t t1, t2;

    if (pthread_mutex_init(&lock, NULL) != 0)
    {
        printf("Mutex initialization failed.\n");
        return 1;
    }

    j = 0;

    pthread_create(&t1, NULL, do_process, NULL);
    pthread_create(&t2, NULL, do_process, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
```
