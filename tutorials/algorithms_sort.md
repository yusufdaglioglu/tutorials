############################

############################
# ALGORITHM SORT
############################

############################

# sort vs order
They are both interchangeable. But "sorting" has 2 different meaning:
- ordering
- grouping. You can say "sort the books by size", but not "order the books by size.". This root of the verb "sorting" is "sort" which means: "a particular kind, species, class, or group, distinguished by a common character".

# natural ordering vs lexicographic (or lexicographical order or lexical order or dictionary order)
There is no any accepted standard about how they should work exactly:
- what will happen if special (non-alpha-numeric) characters exist?
- which will be first? 0 (any number) or A (any character)?
- must be ignored capital/lower case?
- which of them will be before: 0.1 and 0.10 (same valued numbers)

National Information Standards Organization (NISO) (organisation which is accredited by the American National Standards Institute (ANSI)) has published a duideline about the __alphanumeric (or alphanumerical or alfabetik or abecesel)__ impelementations: https://www.niso.org/sites/default/files/2017-08/tr03.pdf

natural sorting is more countable. But lexicographic is better(easier/faster) to see variants of any words (example: "photo", "photograph", "photographer").

# natural ordering
It is human countable.

```
img1.png
img2.png
img10.png
img12.png
```

The term "natural" comes from "natural numbers". As human, when we count natural numbers, we count as 1, 2, ... ,10, 11, 12...

## algorithm trick
In some resources, the non-alphanumeric characters are ignoring directly. And each string is dividing in different parts, so it will be easier to sort and visualise it:

```
foo       =>  "foo",  -1, ""
foobar    =>  "foo",  -1, "bar"
foo13     =>  "foo",  13, ""
foo13xyz  =>  "foo",  13, "xyz"
```

On above block, there are multiple columns. -1 and "" mean null.

## GNU ls commands implementation
"GNU ls" command has "version sort" feature. If that feature will be enabled (by sending a parameter to cammand) it sorts the result based on natural sorting.

## lexicographic
As human when we want to search a word on a dictionary, we want to see in this order:

```
img1.png
img10.png
img12.png
img2.png
```

So we can find the variants of a word near to root. Example:

```
phone
photo
photograph
photographer
```

## alphabetical vs alphanumerical
Akademide ortak hiçbir tanımı yoktur. Alfabetik sıra kullanıldığını belirtir. Çoğu kaynakta, lexicographic'ın bir türevi olduğu yazar, fakat neye göre böyle yazıldığı belirsizdir.

__alphanumerical__ ise sıralayacağımız String'in alpha numeric değerler içerdiğinde, "alfabetik" sıralama uygulayacağımızı belirtmek için kullanılan bir terimdir.

# Features of the sorting algorithms

- __Stability__

  If it is not necessary the elements does not shift.

  Let assume that we sort a table by it's "Name" column (the table example is below). If we don't shift the rows which have the same names, it would be performance advantage for us.

  ```
  Name   ID
  Ahmet  101
  Ahmet  102
  Ahmet  103
  Mehmet 105
  ```

- __in-place algorithm__

  The algorithms which does not require extra space (memory). Algortihms would require some small variables like:
  - counter
  - current index
  - max value of current array

  But those values are ignored.

  If an algorithm requires extra memory space to store a part of original array, they called __not-in-place (or out-of-place)__.

# Sorting Algorithm (or Sıralama algoritması)

- ## Kabarcık Sıralaması (or Baloncuk sıralaması or Bubble Sort)

Diziyi eleman sayısı kadar dolaşır. Her dolaşmada her elemanı sağ taraftaki ile kontrol edip yer değiştirtir.

- ## Seçerek Sıralama (or Selection Sort)

Diziyi sürekli dolaşır. En ufak elemanı geçici bir dizinin başına atar. Daha sonraki dolaşımlarda tekrar en ufak elemanı bulur geçici dizideki 2inci elemana atar.

- ## Hızlı Sıralama Algoritması (or Quick Sort Algorithm)

bir eleman seçilir. bu eleman pivot (Türkçe ve İngilizce'de bu şekilde kullanılır) adı verilir. bu elemanın, hangi index'teki eleman seçileceği hakkında birçok farklı görüş mevcuttur.

her pivotun sağ tarafında ondan büyük elemanlar yerleştirilir ve sol tarafında ise küçük elemanlar yerleştirilir. aynı işlem, sağ ve sol taraf içinde yapılacaktır. böyle olunca en ufak parçalara (3 elemana düşünceye kadar) bölündüğünde tekrar birleştirdiklerinde dizi sıralanmış olacaktır.

pivotun sağ ve sol tarafına yerleştirme işlemi şu şekilde yapılır: dizinin ilk ve son elemanı pivot ile karşılaştırılır. eğer ilk-elemen>=pivot>=son-eleman ise ilk-eleman ve son-eleman swap (yer değiştirir) edilir. Daha sonra dizinin baştan ikinci ve sonradan ikinci elemanı için aynı işlem yapılır. sonra 3üncü... tabi dizinin ortasına gelene kadar.

Buradan sonra dizinin sağ ve sol tarafı ayrı ayrı rekürsif olarak quick-sort algoritmasına sokulur. Burada ayrı ayrı quick-sort'a atılan dizilerin referanslarını quick-sort metotuna geçmemiz yeterli. yeni bir liste oluşturup klonlamaya gerek yok.

- ## Birleştirme Sıralaması (or Merge Sort)

Diziyi sürekli ikiye böler. her elemanı kendi içinde 2 şerli şekilde sıralar. Sonra bunları birleştirir. Birleştirirken de elemanların büyüklüklerini değerlendirir.

- ## Yığınlama Sıralaması (or Heap Sort)

öncelikle sıralanması istenen dizi heap ağacı kurallarına uygun şekilde ağaç haline getirilir. heap ağacı hale getirilmesi kısadır. büyük olan üstte ufak olan altında şeklinde dallandırılacaktır. fakat ağaç bir dizi yapısı olmadığından henüz elimizde sıralı bir dizi yoktur. bunu dizi hale getirmek gerekmektedir. heap ağacının en üstündeki en üst sayı olacağından, sonuç kümesine en büyük rakam olarak kaydedilir. daha sonra ağacın en üstteki node'u silerek ağaç tekrar heapfy işlemine sokulur. bu sefer ağacın en üstündeki değer sonuç kümemizin ikinci en büyük sayısı olmuş olur. bu şekilde tüm ağaç sıfırlandığında sonuç kümemizde sıralanmış bir dizi olacaktır.

- ## Radix sort
radix kelime anlamı: sayı tabanı (örnek: hexadecimal).

bazı kaynaklarda basamak olarak isimlendiriliyor. Fakat bu yanlış. "basamak" İngilizce'de "Positional (or place-value)" olarak çevrilir.

Diğer çok bilinen sort algoritmalarına göre bu algoritmanın çözümü için çok fazla farklı implementasyonlar vardır.

Radix sort temelde şunu söylüyor: önce en düşük seviyeli basamağa göre sayılar sıralanır. Sonra bir sonraki basamağa göre, sonra bir sonraki basamağa göre sıralanır... Zaten hepsini yapınca dizi sıralanmış olacaktır.

örnek:
- orjinal dizi: 55,45,35,33,38,48
- 1inci basamağa göre sıralanmış hali: 33,55,45,35,38,48
- 2inci basamağa göre sıralanmış hali: 33,35,38,45,48,55
