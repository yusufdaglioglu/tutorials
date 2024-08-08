############################

############################
# ALGORITHMS COMPLEXITY
############################

############################

Important note for this document: The base of all the mathematical "log" functions is 2.

# Analysis of algorithms

# algoritma karmaşıklığı (or algorithm Complexity)
They are analysis of __time Complexity__ and __space Complexity__ (memory usage) of programs.

- __O (or Büyük O or Big-Oh or Big-O or Big O)__

  It is the notation which used for __worst-case analysis (or En kötü durum analizi)__.

  To calculate the worst case we should choose the slowest parts of if-else statements.

- __Θ (or Big Theta)__

  _.It is the notation which used for __average-case analysis (or ortalama durum analizi)__.

  There is no any standard way of this calculation. Therefor it is a bit difficult comparing to worst or best case analysis. Because there is no any standard way to predict which input should be given to program.

- __Ω (or big Omega)__

  It is the notation which is using for __best-case analysis (or En iyi durum analizi)__.

- __küçük-omega (or little-omega or ω)__

  It is the tolerated analysis of the best case.

- __küçük-o (or little-o)__

  It is the tolerated analysis of the worst case.

Not-1: The "O" character which is used on the big-O and little-o are small and big case of the same character. But that "O" character is not the character from English alphabet. It is a special character/symbol which is little italic. The name of character is "Landau symbol". (source-id: 418) title: "More information".

Not-2: The calculations based on Asymptotic upper limit and lower limits are used.

# example usage of notation

f(x) = 6x^4 + 9x^4 + 80 = O(x^4) = O(x)

In above statement, "O" is not a function. It's a notation.

# Growth rates

From small to larger:

```
- (constant or sabit)                   1
- (logarithmic or logaritmik)           logn
- (linear or tr:lineer or doğrusal)     n
- (linearithmic or doğrusal logaritmik) n*log-n
- (quadratic)                           n^2
- (cubic or kübik)                      n^3
- (exponential or üstel)                2^n
- (factorial)                           n!
```

# geometrik artış vs üstel artış
__üstel artış__ (tersi __üstel azalma__), isminden de net anlaşıldığıda gibi serideki her elemanın üstel oranda arttığını ifade eder.

__geometrik büyüme__ (tersi __geometrik azalma__) terimleri çok doğru değildir, fakat piyasada çokça "__üstel artış__ (tersi __üstel azalış__)" terimi yerine kullanılır. matematikte "__geometrik seri__" diye bir seri çeşidi vardır. bu serideki her eleman üstel artış veya azalış gösterir. "Geometrik" terimi kullanılmasının sebebi, belli oranda üstel artan veya azalan serilerin değerleri, geometrik şekildeki alan üzerinde gösterdiğimizde human-readable şekil oluşturması ve bu sebepten de geometrik analiz yapılabilmesini kolaylaştırmasıdır. Örneğin; wikipedia sayfalarındaki ana şekilden bunu kolayca anlayabiliriz: https://en.wikipedia.org/wiki/Geometric_series https://tr.wikipedia.org/wiki/Geometrik_seri

# Space Complexity Examples

- example:

O(n)

```java
int sum(int n) {
  if (n <= 0) {
    return 0;
  }
  return n + sum(n-1);
}
```

- example:

O(1)

```java
int pairSumSequence(int n) {
  int sum = 0;
  for (int i = 0; i < n; i++) {
    sum += pairSum(i, i + 1)j
  }
  return sum;
}

int pairSum(int a, int b) {
  return a + b;
}
```

# calculation time complexity of code

## Brief definition of logarithm

```
If    10^3 = 1000;
Then  log1000 = 3
```

- The base value should be written on the lower right side of the 'log' function. If there is not "base" value, then the base is 10 as default.
- In some articles "lg" is preferred instead of "log".
- On the binary algorithm we use base as 2, and it's abbreviation is "lb".
- If the base is Euler number , then the abbreviation is "ln".

Important note for this document: The base of all the mathematical "log" functions is 2.

## example of time complexity calculations

```java
// O(1)
print(hello)
```

```java
// O(n)
for ( i = 0; i < n; i++) {
    print(hello)
}
```

```java
// O(n2)
for ( i = 0; i < n; i++) {
    for ( i = 0; i < n; i++) {
        print(hello)
    }
}
```

```java
// Below code time complexity: O(logn).
// How we found that solution:
// "k" is the maximum number of how many times we entered inside the loop.
// after each iteration the value of "i" is: 2^0, 2^1, 2^2, ..., 2^k.
// 2^k = n
// k = logn
for ( i = 0; i < n; i=i*2) {
    print(hello)
}

// Let's solve the above example with real numbers:
// For n=32

// var-i    time
// 2        1
// 4        2
// 8        3
// 16       4
// 32       5

// log32=5
```

If we have all above examples on the same program then we will have this time complexity:

```
O(1) + O(n) + O(n2) + O(logn) = 1 + n + n2 + logn = O(n2 + logn) = O(n2)
```

On above equation the numbers inside "O", depends on our precision decision. If the want to make a sensitive calculation or the number of "n" (input) will not be large, then we should can also increase the precision like this: O(n + n2 + logn).

Other examples:

```
O(n3 + n2) = O(n3 - n2) = 0(n3)  --> is correct
```

```
O(9999) = O(1) --> is correct
```

"O" notation is asymptotic upper limit. Therefor below formula should be always correct:

```
C ---> constant
f(n) ---> the time complexity function which we found after the calculation
g(n) ---> upper limit function (we wrote this statement inside the "O" notation)

f(n) <= c * g(n)
```

Another Example:

```
n! = O(n^n)
```

Above equation is correct. Because below equation is correct for each "n" value.

```
1*2*3*4*...*n <= C * n*n*...*n
```

When we calculate the upper limit function, we should keep the upper limit function close to real function. In this way the calculation result will be more realistic. If the real function and the upper limit function will have a long distance between them, then out equation will be also correct. But our calculation will not show us sensitive results.

# where "logn" comes from?

- let's start with the binary search algorithm. Let's say we have array with 16 elements and we catch the worst case.
- we divide the array into two parts, for each time we could not found the value/element.
- In the worst case we will divide the array 4 times.
- In this case we divide 16 to 4: 16 * (1/2)^4 = 1.
- Now we write the same equation with "n" element array: n * (1/2)^k = 1.
- In this equation "k" is the number of how many times I divided the array. We are searching this number. If we solve the equation:

  > k = log_2 n   ("log" base is 2)

- asymptote of (log_2 n) is (logn). Therefor the complexity of binary search is O(logn).

# Other examples for calculation

```java
int n=100;
int i=1;
int j=1;

while(s<=n){
  i++;
  s=s+i;
  doSomething();
}
```

| Iteration | value of i | value of s=s+i |
|-----------|------------|----------------|
| 1         | 1          | 1+1            |
| 2         | 2          | 1+1+2          |
| 3         | 3          | 1+1+2+3        |
| 4         | 4          | 1+1+2+3+4      |
| .         | .          | .              |
| .         | .          | .              |
| k         | k          | 1+1+2+3+4+..+k |

"k" is the value of how many times we execute the "doSomething" function. We don't know the "k" value and we wanna find it.

```
1+1+2+3+4+..+k > n   ---> In that case the program will terminate the loop. If we wrote again the left side of the equation with different form:

k(k+1)/2 > n (This is the well known and simple formula in mathematics)

We use above formula and we wrote again the above equation:
k^2 + k > n
We ignore the "k" value because it's not important in a statement which has "k^2":
k^2 > n
k > √n
```

Another example:

```java
int n=100;

for(int i=1; i*i<=n; i++){
  doSomething();
}
```

| Iteration | value of i |
|-----------|------------|
| 1         | 1          |
| 2         | 2          |
| 3         | 3          |
| 4         | 4          |
| .         | .          |
| .         | .          |
| k         | k          |


"k" is the value which points how many times we execute the "doSomething" function. We don't know this value and we wanna find it.

As can be seen above table "k" equals to "i".

```
i*i > n ---> We know that in this case the loop will be terminated.
```

```
i > √n
```

The max value of "i" (the last "i" value before termination of loop) will point us that how many times we executed the "doSomething" function.

Another example:

Let's take and example a program which return the "n" of Fibonacci.

```java
fibonacci(int n){
  if(n<1){
    return 1;
  } else {
    return fibonacci(n-1) * fibonacci(n-2);
  }
}
```

If we show all stack trace visually as a tree, we can understand that the operation size will increase exponentially of 2, for the each time we increment the "n".

```
F(6)                f(6)                  <--  1=2^0
F(5)             f(5)  f(4)               <--  2=2^1
F(4)        f(4) f(3)  f(3) f(2)          <--  4=2^2
F(3)                ****                  <--  8=2^3
F(2)              ********                <-- 16=2^4
F(1)          ****************            <-- 32=2^5
F(0)  ********************************    <-- 64=2^6
```

Therefor the complexity is 2^n.

Another example:

```java
f(int n){
  if(n<1){
    return 1;
  } else {
    return f(n-1) * f(n-1);
  }
}
```

```
F(6)                 f(6)                 <--  1=2^0
F(5)               f(5) f(5)              <--  2=2^1
F(4)          f(4) f(4) f(4) f(4)         <--  4=2^2
F(3)                 *****                <--  8=2^3
F(2)               *********              <-- 16=2^4
F(1)             *************            <-- 32=2^5
F(0)           *****************          <-- 64=2^6
```

Therefor the complexity is 2^n.

# ArrayList calculation of "insert" method - space and time complexity
It is really important how does ArrayList class has implemented. But in this tutorial we will accept general assumptions.

ArrayList stores a native array itself (For more about this topic read: [file:"array_native.md" - title:"dynamic array"](./array_native.md)). In this topic we will assume that the language does not supports dynamic size native arrays. So we will calculate how ArrayList manages that native array.

The insert method should add a new element to native-array which is storing inside ArrayList. we had accept that the programming language does not support dynamic arrays. Therefore insert method must create a new clone array of existing array with "currentArraySize+1" element. The clone element will be created with time and space O(n) complexity.

But most of ArrayList implementations double the capacity only when the array-size is power of 2. So, O(n) (explained above) is only occurs when the arrays-size is power of 2: 1, 2, 4, 8, 16... On the other cases insert method takes O(1) both for space and time complexity.

- worst-case is O(n)
- best-case is O(1)

Now let's have a look to specific point: What happens when we add n elements to ArrayList?

Until we add X elments to Array-List, it should took all below time complexities:

1 + 2 + 4 + ... + X = X + X/2 + X/4 ... 1 = (roughly) 2X

That means X insertions take O(2X) time and space complexity. But the __amortized time__ is O(1), because mostly O(1) will occur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Summation (or Άθροιση)

```
999
 ∑ 77n
n=1
```

is equals the below program:

```
sum = 1;
for( n=1; n<=999; n++ ){
  sum += 77*n;
}
```

Notasyon bazen şu şekilde de olabiliyor:

- 1 ile 99 arasındaki tüm değerler için x'i topluyor:

```
  ∑    x
1<n<99
```

- S kümesinin tüm integer elemanları için x'i topluyor:

```
   ∑  x
  n€S
```

- yanyana kullanım:

```
(eşitliğin sağ tarafı, sol tarafın sadece parantez eklenmiş halidir)

   3      4        3    [  4   ]
   ∑      ∑ i+j =  ∑   [  ∑ j ] 
  i=1    j=2      i=1   [ j=2  ]
```

Yukarıdaki buna eşittir:

```
   3
   ∑   [ (i+2) + (i+3) + (i+4) ] =  [(1+2)+(1+3)+(1+4)] + [(2+2)+(2+3)+(2+4)] + [(3+2)+(3+3)+(3+4)]  
  i=1
```

# product (or γινόμενο)

```
999
 Π 77n
n=1
```

is equals the below program:

```
prod = 1;
for( n=1; n<=999; n++ ){
  prod *= 77*n;
}
```
