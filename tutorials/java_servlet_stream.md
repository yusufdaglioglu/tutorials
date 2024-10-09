############################

############################
# JAVA SERVLET STREAM
############################

############################

# javax.servlet.http.HttpServletRequestWrapper
Servlet veya onun implementasyonlarını wrap etmemizi sağlayan bir wrapper sınıfıdır. Bu sınıf Servlet'lerin davranışlarını değiştirmek istediğimizde kullanmamız için yaratılmıştır.

örnek bir kullanım case'i: ServletFilter'ımızda request'in body'sine ihtiyacımız var. Normal koşullarda, ServletFilter içerisindeyken 'body' henüz inputstream'den okunmamış durumdadır. biz okuruz:

```java
// Aşağıdaki satırda Apache-commons-io kullandım. stream'den string okumak için. o kullanılmadan da yapılabilirdi. basit okunabilir olsun diye böyle yazdım.
String body = IOUtils.toString(request.getReader()); // 'request' yerine httpServletRequest'te olabilirdi. fark etmez.

// do anything with 'body'

chain.doFilter(request, response); // burada kod fail eder. çünkü biz @Controller metotumuza geldiğimizde gelen request objesi tümüyle okunum DTO'ya map edilir. Fakat okunmak istediğinde request objemizin stream'i zaten okunmuş olacağından hata alınacaktır.
```

Bu davranışı ezebilmek için HttpServletRequestWrapper kullanılır. önce HttpServletRequestWrapper implemetasyonumuzu yazalım. Bu implementasyon bize kalmış. Biz sadece input stream'in davranışını ezeceğiz:

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;

public class RequestWrapper extends HttpServletRequestWrapper {
    private final String body;

    public RequestWrapper(HttpServletRequest request) throws IOException
    {
        super(request); //So that other request method behave just like before

        StringBuilder stringBuilder = new StringBuilder();
        BufferedReader bufferedReader = null;
        try {
            InputStream inputStream = request.getInputStream();
            if (inputStream != null) {
                bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
                char[] charBuffer = new char[128];
                int bytesRead = -1;
                while ((bytesRead = bufferedReader.read(charBuffer)) > 0) {
                    stringBuilder.append(charBuffer, 0, bytesRead);
                }
            } else {
                stringBuilder.append("");
            }
        } catch (IOException ex) {
            throw ex;
        } finally {
            if (bufferedReader != null) {
                try {
                    bufferedReader.close();
                } catch (IOException ex) {
                    throw ex;
                }
            }
        }

        body = stringBuilder.toString();
    }

    @Override
    public ServletInputStream getInputStream() throws IOException {
        final ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(body.getBytes());
        ServletInputStream servletInputStream = new ServletInputStream() {
            public int read() throws IOException {
                return byteArrayInputStream.read();
            }
        };
        return servletInputStream;
    }

    @Override
    public BufferedReader getReader() throws IOException {
        return new BufferedReader(new InputStreamReader(this.getInputStream()));
    }

    //Use this method to read the request body N times
    public String getBody() {
        return this.body;
    }
}
```

Artık servlet-filter'ımızda bu satırı kullanabiliriz:

```java
request = new RequestWrapper((HttpServletRequest) request);
String body = request.getBody();
chain.doFilter(request, response); // this will work properly
```

Wrapper'ımızın getInputStream() metotunu override ettik. getInputStream metotunun implementasyona dikkat edilirse, sürekli yeni bir inputStream dönüyor. Bu şekilde Spring framework bunu çağırdığında ona da yeni bir input stream dönülecektir.

HTTP'de "Body" stream'dir. Onun dışındaki alanlar önceden gelir (ilk okunur) ve onlar okunmadan hiçbir şekilde servlet metotu initialize edilmez. Bu sadece Spring'de değil, tüm framework'lerde bu şekildedir (örnek Javascript fetch API - bu konu başka başlıkta anlatılıyor). HTTP mantığı bu şekilde çalışacak şekilde tasarlanmıştır.

# sadece body'nin stream olması

aşağıdaki tüm bilgiler buradan alınmıştır: kaynak: (source-id: 223)

ServletRequest.getParameter metotu özel bir metottur. web tarayıcısı, form submit yapıldığında GET veya POST olmasına göre form bilgilerini ya URL'ye yada body'ye basar. getParameter metotu her 2 durumu göz önüne bulundurarak çalışır ve tek bir metot üzerinden parametrelerin okunmasını sağlar.

API dökümantasyonunda şu yazar:

```
If the parameter data was sent in the request body, such as occurs with an HTTP POST request, then reading the body directly via getInputStream() or getReader() can interfere with the execution of this method.
```

Yani (sadece "post" case için) biz inputStream okursak, getParameter metotunun işleyişi bozulacaktır. çünkü post case'leri için getParameter fonksiyonu da stream aracılığı ile bizim için data'yı okuyor.
