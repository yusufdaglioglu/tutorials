############################

############################
# C MEMORY STRUCTURE
############################

############################

# memory structure

```
High Addresses ---> .----------------------.
                    |      Environment     |  CommandLine Argument + Enviroment variables
                    |----------------------|
                    |                      |   variable are declared on the stack
                    |         STACK        |
stack pointer ->    | - - - - - - - - - - -|
                    |           |          |
                    |           v          |
                    :                      :
                    .                      .   The stack grows down into unused space
                    .         Empty        .   while the heap grows up.
                    .                      .
                    .                      .   (other memory maps do occur here, such
                    .                      .    as dynamic libraries, and different memory
                    :                      :    allocate)
                    |           ^          |
                    |           |          |
                    | - - - - - - - - - - -|   Dynamic memory is declared on the heap
                    |          HEAP        |
                    |                      |
                    |----------------------|
                    |          BSS         |   Uninitialized data (BSS)
                    |----------------------|
                    |          Data        |   Initialized data (DS)
                    |----------------------|
                    |          Text        |   Binary code
Low Addresses ----> '----------------------'
```

C ile derlenmiş bir uygulama çalışırken, memory'deki görüntüsü yukarıdaki gibidir. bu parçaları incelersek:

- stack

  static olmayan değerler burada tutulur.

  her fonksiyonun bir stack bloğu vardır.

  LIFO mantığında çalışır. bu sebeple __stack pointer__ listenin tepesinin adresini tutar.

- heap

  memory allocation (alloc, realloc...) işlemleri ile buradan yer ayırtılır. buradaki değerler fonksiyonlardan bağımsızdır.

- BSS (or Uninitialized data segment)

  static yada global olan fakat initialize olmamış değerler burada tutulur.

- DS (or Initialized data segment)

  static veya global olan ve initialize olmuş değerler burada tutulur.

- Text

  kodumuzun binary hali burada tutulur. bu kısım read-only'dir. böylece yanlışlıkla buraya yazma gibi bir durum söz konusu olamaz.

# segmentation fault (or segfault)
eğer memory'de erişmeye yetkimiz olmayan bir adrese erişmek istediğimizde bu hatayı alırız.
