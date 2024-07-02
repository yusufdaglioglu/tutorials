############################

############################
# UNIX LIKES SHELL SYNTAX
############################

############################

# shell

# shebang (or sha-bang or hashbang or pound-bang or hash-pling)
bir terminal emulator'a bir script dosyasını çalıştırmasını istediğimizde:

> /path/my_script

bu script'i hangi interpreter ile çalıştıracağını bilemez. dolayısı ile my_script'in tepesine shebang koyulur:

```bash
#!/bin/sh
```

\#! karakterleri sonrasında aslında execute edilecek interpreter'ın executable'ının path'ini yazıyoruz. Örneğimizdeki script sh programına parametre olarak geçilecek. yani aşağıdaki kod çalıştırılacak:

```sh
/bin/sh <dosyamızın içeriği buraya>
```

bash, bir sh implementasyonudur. fakat daha sonraki zamanlarda bazı değişikliklere uğradı. bu sebeple script dosyamızın üzerine /bin/sh veya /bin/bash yazmamış çok şeyi değiştirir. /bin/sh bir kısayol dosyasıdır. dolayısı ile farklı makinede farklı bir implementasyona referans ediyor olabilir.

başka örnek:

```sh
#!/usr/bin/env python
```

env komutu Python komutu çalıştırmaktadır. çoğu sistemde env, /usr/bin içerisinde olduğundan bu shebang ile sıkça karşılaşırız. env kullanmasaydık şunu da yazabilirdik:

```sh
#!/usr/bin/php
```

Eğer shebang tanımı yok ise, o anda script'i execute eden interpreter direk olarak kendi dilinde script'i yürütecektir.

Script dosyaları çalıştırıldıklarında, çalıştırılan dosyanın her satırı gerektiğinde okunur ve sadece ilgili satır okunur. yani sh dosyası çalışırken o dosyayı değiştirsek, çalıştırılan kodda bu durumdan etkilenecektir.

## function return value

fonksiyonlar exit code 0 ise başarılı değilse başarısız kabul edilir. bu sebeple || && gibi syntax'lardan yararlanabiliriz. örnek:

```sh
myFunc(){
  return 1; # this is the exit code.
}

# if we call it
myFunc "myParam1" "myParam2" || echo "this is an error"

# or

myFunc "myParam1" "myParam2" || { echo "this is an error"; echo "not good"; }
```

fonksiyonlar değer return etmez. Birkaç kaçamak yol uygulanabilir:

- ortak parametre kullanımı

  myFunc kendi için return edeceği değeri environment variable'lara özel bir isimli bir variable'a atar. daha sonra myFunc'u çağıran fonksiyon bu değeri environment variable'larından okur.

- __subshell__ ve echo aracılığı ile

  ```sh
  myFunc(){
    echo "abc"
  }

  RETURN_VALUE=$(myFunc "myParam1" "myParam2")
  echo $RETURN_VALUE
  ```

- exit code

  exit code ufak bir tamsayı (çok ufak limitli). string döndürmek için bu yöntem kullanılamaz.

  ```sh
  myFunc(){
    return 0
  }

  myFunc "myParam1" "myParam2"
  RETURN_VALUE=$?
  echo $RETURN_VALUE
  ```

## positional parameters

> my_bash_script_file.sh param1 param2

gibi komut çağrıldığında script dosyası içinde:

> $0

çağrılan scope'a göre dönüşü değişir.

/path/to/my_bash_script_file.sh

Eğer çağrılan bir script dosyası değilde bir fonksiyon olsaydı burada fonksiyon ismi dönecekti.

Eğer hiçbir fonksiyon yada script dosyası değilse; yani direk terminalimizden $0 yazarsak, "bash", "zsh" gibi dönüşler alırız.

> $1

param1

> $2

param2

> $3

boş

> ${12}

9'dan sonraki parametreleri ancak bu şekilde çekebiliyoruz.

> $@ veya $*

$0 hariç tüm parametreleri döndürür: param1 param2

> $#

$0 hariç parametre sayısını döndürür: 2

Last command's last parameter:

> $_

example:

```sh
echo p1 p2
echo $_ # will print 'p2'
```

current-shell process-id:

> $$

process ID of the most recently executed background process:

> $!

Gives the exit status of the most recently executed command:

> $?

yukarıdaki bazı değerleri tablo üzerinde karşılaştırırsak:

| command / parameter                     | $0            | $1      | $@            | $*            | $#              |
|-----------------------------------------|---------------|---------|---------------|---------------|-----------------|
| zsh                                     | zsh           | (empty) | (empty)       | (empty)       | 0               |
| bash                                    | bash          | (empty) | (empty)       | (empty)       | 0               |
| /bin/zsh                                | /bin/zsh      | (empty) | (empty)       | (empty)       | 0               |
| /bin/bash                               | /bin/bash     | (empty) | (empty)       | (empty)       | 0               |
| bash -c "/path/file.sh param1 param2"   | /path/file.sh | param1  | param1 param2 | param1 param2 | 2 (param count) |
| zsh -c "/path/file.sh param1 param2"    | /path/file.sh | param1  | param1 param2 | param1 param2 | 2 (param count) |
| bash -c ". /path/file.sh param1 param2" | /path/file.sh | param1  | param1 param2 | param1 param2 | 2 (param count) |
| zsh -c ". /path/file.sh param1 param2"  | /path/file.sh | param1  | param1 param2 | param1 param2 | 2 (param count) |
| bash -c "func1 param1 param2"           | func1         | param1  | param1 param2 | param1 param2 | 2 (param count) |
| zsh -c "func1 param1 param2"            | func1         | param1  | param1 param2 | param1 param2 | 2 (param count) |

## $* vs $@

```sh
printAll(){
    echo 'Using "$*"'
    for a in "$*"; do
        echo $a;
    done

    echo
    echo 'Using $*'
    for a in $*; do
        echo $a;
    done

    echo
    echo 'Using "$@"'
    for a in "$@"; do
        echo $a;
    done

    echo
    echo 'Using $@'
    for a in $@; do
        echo $a;
    done

    echo
    echo "printing screen is always same result"
    echo '$* and $@ is important only when passing parameters or loops.'
    echo $@
    echo $*
    echo "$@"
    echo "$*"
}

printAll 1 2 "3 4"
```

output:

```
Using "$*"
1 2 3 4

Using $*
1
2
3
4

Using "$@"
1
2
3 4

Using $@
1
2
3
4

printing screen is always same result
$* and $@ is important only when passing parameters or loops.
1 2 3 4
1 2 3 4
1 2 3 4
1 2 3 4
```

# shell uyumlu dillerin syntax'ı

> command1 ; command2

ile bölünen kodlar sırası ile ne olursa olsun çalıştırılırlar. ; yerine "yeni satır" karakteri kullanılabilir. aynı görevi görecektir.

> command1 && command2

command1 başarılı dönerse command2 çağrılır.

> command1 || command2

command1 başarılı dönmezse command2 çağrılır.

> ls | grep abc

| işareti kendinden önceki komutun çıktısını sonrakine girdi olarak aktarır.

> myVar=$(command param1 param2)

çalıştırılan komutun çıktısını myVar'a atar.

> $

non-root user olduğunu gösterir

> \#

root user'ı olduğunu gösterir

> export MY_VARIABLE='Hello'

daha sonraki satırlarda kullanabilmek için değişken tanımlıyor. kullanımı için $MY_VARIABLE yazılması yeterlidir. bu değer komut satırından ve komut satırının çağırdığı diğer komutlar tarafından kullanılabilir. eğer komut satırından başlatılan uygulamalar bu değeri görmesi istenmiyor ise; export komutu yerine bash syntax'ı (bash'in default özelliği) kullanılmalıdır: MY_VARIABLE='Hello' yazılması yeterlidir ( = etrafında boşluk olamaz)

> env MY_VARIABLE='Hello' my_command

env önce değişkeni tanımlar ve ardından komutu değişkeni görebileceği şekilde komutu çalıştırır.

> bash

yeni bir bash oturumu açar. yeni oturum yeni pencerede açılmaz çünkü zaten komut satırı penceresinde olduğumuzdan oradan devam eder. yeni session açma isteğimiz environment değerlerinden ve bulunduğumuz dizinden çıkıp varsayılan dizine geçişimiz olabilir. "bash -c komut" komutu yeni açılan session'da çalıştırmasını sağlar.

bash komutu sonrası; export ile belirlenen değişkenler silinmez. fakat bash'in syntax'ını kullanarak belirlediğimiz değişkenler silinir.

> type

alacağı parametrenin tipini döner. örneğin; "if" alırsa bunun bir komut satırı statement'i olduğunu döner. "pwd" alırsa bunun komut satırı programına gömülü bir program olduğunu döner. "vlc" alırsa external bir program olduğunu döner. gibi..

> ./file-name

path'te tanımlı olan dosyalara bakmadan o anda bulunan dizindeki dosyayı execute eder. dosya sh ise script yürütülür, executable binary ise işletim sistemi execute eder. executable olup olmayacağı yetkilerle alakalı bir konudur. eğer executable dosya değil ise; komut satırındayken tab tuşu o dosyayı otomatik tamamlamaz.

filename önüne bulunduğumuz directory belirteci girilmezse Linux sadece path'e bakar, bu sebeple filename dosyası execute edilmez.

> komut > dosya

komutun çıktısını dosyaya yazdırır. dosya eğer varsa içeriğini siler ve dosyanın başlangıcına yazar.

```sh
> file  ## redirects stdout to file
1> file ## redirects stdout to file
2> file ## redirects stderr to file
&> file ## redirects stdout and stderr to file
2>&1    ## redirects stdout to stderr. normalde beklenen "2>1" olmasıdır. fakat "2>1" olursa; o zaman "1" dosya ismi olarak algılanır. arada bir belirteç olması gerekmektedir. bu sebeple 1'in önüne "&" işareti konulması standartlara eklenmiştir.
```

> komut >> dosya

\> ile aynıdır. tek farkı dosyanın içeriğini silmez ve sonundan yazmaya devam eder.

> komut < dosya

dosyanın içeriğini komutun system.input'una yollar.

# string

aşağıdakilerin hepsi birbirine eşittir:

```sh
'aa''bb''cc'
'aabbcc'
'aabb''cc'
'aa'bbcc
"aa"bb'cc'
```

String'ler tırnak içine alınabilir. Alınmasındaki fayda şudur: eğer string içerisinde boşluk varsa ve bu string'imizi tırnak içine almadıysak diğer programlar tarafından birden fazla parametre olarak algılanacaktır. bu sebeple;

```sh
$MY_ENV
```

gibi değişkenler çoğu durumda tırnak içine alınır. çünkü eğer değişken içerisinde boşluk varsa, değişkenin tek parametre (tek string) olarak tanımlatmak isteriz. bunu yapmamamız için bazı istisnai durumlarda olabilir.

Hardcode girilmiş string'lerimizi de mutlaka tırnak içine almalıyız. yoksa bu gibi karışıklıklar olabilir:

```sh
echo $HOMEabc
echo $HOME'abc'
```

Yukarıda ilk satır, HOMEabc isimli değişkeni ekrana basarken, aşağısındaki satır HOME değişkeninin yanına abc'yi birleştirerek ekrana basar.

çift tırnak ve tek tırnak sabit olan string'lerimiz için (örnek: "abc") önemli değildir. fakat değişkenleri basarken önemlidir. örnek:

```sh
echo "$HOME"
# prints /home/user1

echo '$HOME'
# prints $HOME
```
