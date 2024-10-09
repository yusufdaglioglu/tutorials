############################

############################
# JAVA FUTURE
############################

############################

# java.util.concurrent.Future

```java
public class SquareCalculator {

    // ExecutorService Java içinde gelen, Birçok thread'i yönetebilmemizi sağlayan bir yapıdır.
    private ExecutorService executor = Executors.newSingleThreadExecutor();

    public Future<Integer> calculate(Integer input) {

        return executor.submit(() -> {

            Thread.sleep(1000);

            return input * input;
        });
    }
}

Future<Integer> future = new SquareCalculator().calculate(10);

while(!future.isDone()) {
    System.out.println("Calculating...");
    Thread.sleep(300);
}

Integer result = future.get(); //get metotu eğer yukarıdaki while'ı yazmamış olsaydık, işlem tamamlanana kadar bekleyecekti. şimdiki durumda beklemesine gerek yok, çünkü zaten yukarıda işlemin bittiğine dair kontrolümüzü while içerisinde yaptık.
```

Future sınıfı şu metotları sunar:

- Boolean Cancel(boolean mayInterruptIfRunning)

  eğer task zaten kapanmışsa yada kapatılamadıysa false döner.

- V get()

  V dönüş objemiz. metotun ne yaptığı kod yukarıdaki kod içinde açıklandı.

- V get(long Timeout, TimeUnit unit)

  metot belli aralıkta dönmezse TimeoutException fırlatır.

- Boolean isCancelled()

- Boolean isDone()

# java.util.concurrent.CompletableFuture
java.util.concurrent.Future implementasyonudur. ek özellikler sunar. Future'nin eksiklerini kapatmak için daha sonradan geliştirilmiştir. kapattığı eksikler:

- zincir oluşturabilme

  üstüste Future'leri sırası ile execute etme

- exception handling

- manually complete the future

  ```java
  completableFuture.complete("Future's Result");
  ```

  bu satır sonrası, completableFuture'ı kullanan tüm  metotlar isDone=true olacaktır.

- callback metotu verebilme

  ```java
  CompletableFuture<Void> future = CompletableFuture.runAsync(new Runnable() {
    @Override
    public void run() {
        // any code here
    }
  });

  future.get()
  ```

  Aynı işi lambda ile çok daha kısa yapabiliriz:

  ```java
  CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
      // any code here
  });
  ```

  Sadece future kullansaydık; get()'ten sonra metotumuzu elle çalıştırmak zorunda kalacaktık ve işlemin success mi olup olmadığını da kontrol etmemiz gerekecekti.

  Burada callback metotumuz istersek obje de dönecek şekilde ayarlayabilirdik.

CompletableFuture'ın bazı metotları birbirlerine çok benziyor. Asıl farkları bir önceki callback metotundan dönen parametreyi alıp almadıkları ve kendilerinin ne döndürdükleridir. aşağıdaki tablo üzerinde özet bilgiye ulaşılabilir:

tablo kaynağı: (source-id: 420)

| Method         | Async method        | Arguments                               | Returns                        |
|----------------|---------------------|-----------------------------------------|--------------------------------|
| thenRun()      | thenRunAsync()      | –                                       | –                              |
| thenAccept()   | thenAcceptAsync()   | Result of previous stage                | –                              |
| thenApply()    | thenApplyAsync()    | Result of previous stage                | Result of current stage        |
| thenCompose()  | thenComposeAsync()  | Result of previous stage                | Future result of current stage |
| thenCombine()  | thenCombineAsync()  | Result of two previous stages           | Result of current stage        |
| whenComplete() | whenCompleteAsync() | Result or exception from previous stage | –                              |

# Promise
standart Java içerisinde Promise objesi yoktur. CompletableFuture, diğer dillerde kullanılan promise kavramına en yakın objedir.
