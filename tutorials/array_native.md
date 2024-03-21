############################

############################
# ARRAY NATIVE
############################

############################

# Multidimensional Array (or çokboyutlu dizi)
C# Example of: two-dimensional (2d) array of 4 rows and 2 columns.

```c#
string[,] array = new string[3, 2] {
                                      { "one", "two" },
                                      { "three", "four" },
                                      { "five", "six" }
                                    };

```

C# Example of: three-dimensional (3d) array:

```c#
int[,,] array = new int[2, 2, 3] {
                                   { { 1, 2, 3 }, { 4, 5, 6 } },
                                   { { 7, 8, 9 }, { 10, 11, 12 } }
                                 };

```

# jagged array (or ragged array or array of array)
jagged kelime anlamı: sivri.

ragged kelime anlamı: eski püskü.

"__multidimensional array__" is something different. It should not be mixed.

This feature is supporting by Java and C#.

The below __jagged array__ example is sytnax is valid for both Java and C#:

```c#
int[][] c;
c = new int[2][]; // creates 2 rows
c[0] = new int[5]; // 5 columns for row 0
c[1] = new int[3]; // create 3 columns for row 1
```

## variable-length array (or VLA or variable-sized array or runtime-sized array)
Java bu özelliği destekliyor.

C'ye C99 ile bu özellik gelmiştir. C'de daha öncesinde array ancak şu 2 şekilde oluşturulabiliyordu:

- array-size'ı ancak define'da tanımlanan bir değer ile oluşturulabiliyordu
- malloc ile bellek alanı tahsis edilen bir array pointer'ı yaratılıyordu

VLA sayesinde, C99 ile artık run-time'de oluşturulan bir integer değeri ile de dizi oluşturabiliyoruz. Örnek:

```c
int i = 5;
int array[i];
```

C++'ta bu özellik yok (eski sürümlerinde yoktu. yeni sürümlerinde gelmiş olabilir). Fakat gcc derleyicisi bu özelliği destekleyecek şekilde bir yeteneği var. fakat resmi C++ spesifikasyonlarında bu özellik desteklenmiyor.

__Flexible array member__ C99 ile gelen VLA'dan farklı bir özelliktir. VLA ile karıştırılmamalı.

## dynamic array (or growable array or resizable array)
__dynamic size array__ veya __dynamic length array__ isimlendirilebilir.

Bazı makalelerde "dynamic size array" veya "dynamic array", VLA yerine kullanılıyor. Çünkü array'i boyutu runtime'da belirlenip array init edilebiliyor. Oysa bu terimler farklıdır.

dynamic array; size'ı belirlenmiş ve init edilmiş bir dizinin boyutunun değiştirilebilir olması özelliğidir.

dynamic array'i bir programlama dilin desteklemesi mümkün değil. Genelde bunu 2 dolaylı yöntem ile çözmüş gibi gösteriyorlar:
- arkaplanda yeni array oluşturan wrapper sınıflar bu altyapıyı destekler gibi gözükmektedir. Örnek: Java'da ArrayList.
- Pointer destekleyen dillerde (örnek: C), pointer'lar ile bunu yapabiliriz. Fakat aslında pointer'lar, native array olarak sayılmazlar.