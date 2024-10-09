############################

############################
# JAVA JSP AND JSF
############################

############################

# JSP (or JavaServer Pages)

JSP'de sayfalar, sunucu tarafta derlenip client'a gönderilir. her JSP sayfası, bir servlet'e çevrilir. servlet Java sınıfıdır.

JSP dosyaları, Java servlet sınıfına çevrilirken:

JSP dosyası:

```xml
<html>
  <%
    double num = Math.random();
    if (num > 0.95) {
  %>
      <h2>You will have a luck day!</h2><p>(<%= num %>)</p>
```

Java servlet sınıfı:

```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
public class MyServlet extends HttpServlet {

  // Runs when the servlet is loaded onto the server.
  public void init() {
     // servlet init codes here...
  }

  // Runs on a thread whenever there is HTTP GET request

  // Take 2 arguments, corresponding to HTTP request and response

  public void doGet(HttpServletRequest request, HttpServletResponse response)

        throws IOException, ServletException {

     // Set the MIME type for the response message

     response.setContentType("text/html");

     // Write to network

     PrintWriter out = response.getWriter();

     // Your servlet's logic here

     out.write("<html>\r\n  ");

     double num = Math.random();

     if (num > 0.95) {

        out.write("<h2>You will have a luck day!");

        ....

  }
```

# JSP ile single page application
Bu konu başlığı da okunması faydalı olacaktır: [file:"web.md" - title:"submit request vs Ajax"](./web.md#submit-request-vs-ajax)

JSP ile single page application yapısına uygun değildir. fakat eğer yapılmak isteniyor ise bir çok farklı yöntem uygulanabilir.

normalde JSP tüm sayfayı döner ve HTML'e o output yazılır. fakat biz eğer servlet'e isteği, JS'te oluşturduğumuz manuel bir Ajax işlemi ile yapar ve dönüş değerini manuel sayfanın bir kısmını güncelleyecek şekilde ayarlarsak o zaman single page application yapmış oluruz. aynı şey Spring MVC içinde geçerlidir.

# JSP debug
Eclipse debug perspektifindeyken, variables view'ı JSP dosyasında debug point koymamıza izin veriyor. runtime'da hangi değerlerin olduğunu da gösteriyor. bunun için Eclipse'te WTP (or web tools platform) plugin'i yüklü olmalıdır.

# JavaServer Pages Standard Tag Library (or JSTL)
JSTL JavaEE'nin bir modülüdür.

jst JSP için core seviyede JSP tag'leri içerir. Gruplanmışlardır.

sadece ihtiyacımız olan grubu JSP dosyamıza import ederiz:

```jsp
<%@ taglib prefix = "c" uri = "http://java.sun.com/jsp/jstl/core" %>
```

- ## Core Tags
     ```jsp
     <c:set>
     <c:forEach>
     <c:when>
     ```

- ## Formatting tags
     ```jsp
     <fmt:formatNumber>
     <fmt:parseDate>
     ```

- ## SQL tags
     SQL sorgusu çalıştırmayı ve bunu bir değere atayabilmemizi sağlar.
     ```jsp
     <sql:query>
     <sql:transaction >
     ```

- ## XML tags
     XML parse işlemleri yapmamızı sağlayan metotlar içerir.

- ## JSTL Functions
     Hazır temel fonksiyonlar içerir. Çoğu string işlemleridir.

# JSF (or Java Server Faces)

JSF; JSP üzerine kurulu bir yapıdır. JSF'te, JSP gibi derleme ve servlet'in cevabı dönme işlemine ek olarak; altyapısı lifecycle'lar üzerinden hareket etmesini sağlamaktadır.

JSP'de her JSP dosyası bir class'a denk gelirken, JSF'te her JSF dosyası tek bir servlet üzerinden (faces servlet) dağıtılır.

JSF; MVC mimarisinde şu şekilde ayrılır:

- M: modellerimiz (veritabanı modelleri gibi)

- V: XHTML dosyalarımız

- C: FacesServlet ---> çünkü view'lardaki değerleri güncelleyecek olan kodlar, aynı zamanda modelleri güncelleyecek olan kodlar buradadır.

# Primefaces vs Icefaces vs Richfaces vs Myfaces

Primefaces ve Icefaces ve Richfaces; JSF üzerine kuruludur. JSF tag'lerine ek kendi tag'lerini sunar.

Myfaces; bir JSF implementasyonudur.
