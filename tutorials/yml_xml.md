############################

############################
# YML AND XML
############################

############################

# YAML (or YAML Ain't Markup Language)
- boşlukların, satır sonlarının önemli olduğu XML ve JSON'a alternatif bir formattır.
- ".yml" veya ".yaml" uzantıları ile dosyalarda tutulurlar.
- JSON'a göre daha çok özelliğe sahiptir ve okunaklığı daha fazladır.
- YAML editor kullanmadan düzenlemek çok zordur. ve tab karakteri kabul görmemektedir/kullanılmamaktadır.
- YAML dökümanı isteğe bağlı olarak "---" ilen başlayıp "..." ile bitebilir.

örnek yml:

(YAML kurallarının neye tekabül etitğini anlamanın en basit ve kolay yolu JSON veya XML gibi bir formata convert etmektir.)

```yml
# YAML supports comments, JSON does not.

array:
  - first element of array
  - second element of array

# string yazarken tırnak koymasakta olabilir
stringObject:
  'key': 'any string here'
  "key2": "any other string here"
  key3  : any other string here

# prop kısmında tırnak kullanımı
my "prop": my value

# tipler otomatik algılanır. Eğer bir tamsayıyı string algılatmak istiyorsak; o zaman tırnak kullanmak zorundayız.
types:
  - 1
  - "1"
  - true
  - "true"

# yes ve no'lar true false olarak algılanıyor
booleanTypes:
  - no
  - yes
  - NO

# aşağıdaki paragraf değerini programatik okumaya çalışırsak, sonuncu satırın sonunda, satır sonu karakteri olduğunu okuruz. YY sonrasında, ne kadar çok yeni boş satır eklesekte en sonda sadece 1 adet satır sonu karakteri okuyabiliriz.
paragraph1: |
    1
    XX
    YY
# aşağıdaki paragraf değerini programatik okumaya çalışırsak, sonuncu satırın sonunda, satır sonu karakteri yoktur.
paragraph2: |-
    2
    XX
    YY
# bu kullanım paragraph1 ile aynı sonucu verir. tek farkı şudur: eğer YY sonrası boş satır sonları YAML'a eklersek, o zaman o satır sonları da programatik olarak okunabilirler.
paragraph3: |+
    3
    XX
    YY
# aşağıdaki paragraf değerini programatik okumaya çalışırsak, her satırın arasında satır sonu karakterini okuyamayız. tümü 1 satırdır. sadece en sonda 1 satır sonu karakteri vardır.
paragraph4: >
    4
    XX
    YY
# biraz farklı da olsa JSON tarzı bir formatta da veriler tutulabiliyor
ornekJson: {name: Martin Developer}
```

yukarıdaki YAML'a denk gelen JSON aşağıdaki gibidir:

```json
{
  "array": [
    "first element of array",
    "second element of array"
  ],
  "stringObject": {
    "key": "any string here",
    "key2": "any other string here",
    "key3": "any other string here"
  },
  "my \"prop\"": "my value",
  "types": [
    1,
    "1",
    true,
    "true"
  ],
  "booleanTypes": [
    false,
    true,
    false
  ],
  "paragraph1": "1\nXX\nYY\n",
  "paragraph2": "2\nXX\nYY",
  "paragraph3": "3\nXX\nYY\n",
  "paragraph4": "4 XX YY\n",
  "ornekJson": {
    "name": "Martin Developer"
  }
}
```

# YAML format standartları

> YAML 1.2 (3rd Edition) - 2009-10-01-  http://yaml.org/spec/1.2/spec.html

> YAML 1.1 (2nd Edition) - 2005-01-18 - http://yaml.org/spec/1.1/

> YAML 1.0 (1st Edition) - 2004-01-29 - http://yaml.org/spec/1.0/

# JSON format standartları

> March 2017 - RFC 7493 - This document specifies I-JSON, short for "Internet JSON"

> March 2014 - RFC 7159 - draft of "I-JSON"

> March 2013 - RFC 7158 - draft of "I-JSON"

> October 2013 - ECMA-404 - JSON standard at ECMA

> July 2006 - RFC 4627 - first JSON public doc

Yukarıdaki standartlar az da olsa birbiri arasında fark ediyor. bazı parser/validator'larda bu bilgi lazım olabilir.

__JSON5, jsonc, Hjson__ JSON'dan bağımsız fakat ondan türemiş farklı standartlardır.

# XML format standartları
W3C.org'da XML standartları belirlenmiştir. W3C.org da bir spesifikasyon bu süreçlerden geçer:

1. Working Draft (WD)
2. Last Call Working Draft
3. Candidate Recommendation (CR)
4. Proposed Recommendation (PR)
5. W3C Recommendation (REC)

http://www.w3.org/TR/xml/ URL'si her zaman son sürüme otomatik yönlendirir.

> 1.0 (W3C Recommendation of Fifth Edition) - http://www.w3.org/TR/2008/REC-xml-20081126/

> 1.0 (Proposed Edited Recommendation of Fifth Edition) - https://www.w3.org/TR/2008/PER-xml-20080205/

> 1.0 (Fourth Edition)- https://www.w3.org/TR/2006/REC-xml-20060816/

> diğer sürümler...

# XML (or eXtensible Markup Language)

- # terminoloji:

Aşağıdaki tüm satır bir __element__'tir.

```xml
<tag attribute="attribute's value"> tag's value </tag>
```

- # Node
Node terimi resmi olarak XML standartlarında geçmiyor. fakat "node" hep XML'in herhangi bir parçası olarak kullanılıyor. örneğin Java'da org.w3c.dom.Element (XML element'i), org.w3c.dom.Document (XML dosyasının tamamı), org.w3c.dom.Attr (element'in attribute'u) gibi sınıfların tümü org.w3c.dom.Node'dan türemiştir.

- # property vs attribute

"özellik" manasında kullanıldıklarında İngilizce'de de çok yakın anlamlı kelimelerdir. fakat farklı anlamları da vardır. bilişim dünyasında da birbirleri interchangable olarak kullanılırlar.

HTML dünyasında; browser HTML'i parse ederken attribute okur ("xml" standartlarındaki ile aynı anlamda kullanılır). Fakat HTML DOM objesi haline gelen attribute'ler, Javascript'in DOM nesnesinden okunmak istendiğinde "property" olarak okunur.

Not: Fakat DOM property'si ile HTML attribute'leri aynı isimde ve tipte oluşturulamazlar. Bu konudan bağımsız (web browser standartları ile ilgili) bir not. örnek:

```xml
<input id="input" type="checkbox" checked style="color:red">
```

- input.checked --> boolean dönüş yapar.
- input.style --> obje dönüş yapar.

Aynı zamanda DOM objelerinin hala getAttribute isimli fonksiyonu vardır:

```js
input.getAttribute("style") // her zaman string dönüş yapar.
```

Oysa DOM standartlarında input.style'daki "style" bir property olarak adlandırılır.

- # ? karakteri

ilk satırda encoding ve xml-version'u bu şekilde belirtilebilir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

- # XML Namespace

```xml
<table>

  <tr>

    <td>Apples</td>

    <td>Bananas</td>

  </tr>

</table>

<table>

  <name>African Coffee Table</name>

  <width>80</width>

  <length>120</length>

</table>
```

yukarıdaki iki farklı XML tek bir XML'de birleştirmiş olalım. fakat isimlendirmeler benzediği/aynı olduğu için sorun olur. bunu çözmek için elementlerin önüne prefix atılır.

```xml
<h:table>

  <h:tr>

    <h:td>Apples</h:td>

    <h:td>Bananas</h:td>

  </h:tr>

</h:table>

<f:table>

  <f:name>African Coffee Table</f:name>

  <f:width>80</f:width>

  <f:length>120</f:length>

</f:table>
```

namespace'lere isteğe bağlı "xmlns" ile ID verilebilir. fakat genelde ID yerine o namespace için dökümantasyon URL'si yazılır. çünkü URL'de genelde unique'tir. örnek:

```xml
<h:table xmlns:h="http://www.w3.org/TR/html4/">

  <h:tr>
```

yada şu şekilde:

```xml
<root xmlns:h="http://www.w3.org/TR/html4/">

   <h:table>
```

# XSL (or Extensible Stylesheet Language)

XML ile ilgili tüm standartların ailesine verilen isimdir. Altında XSLT, XPath gibi standartlar bulunmaktadır.

# XSLT (or XSL Transformations)

XML dökümanını, yine bir XML dosyasına dönüştürmek (farklı bir şema üzerine transfer etmek) için kullanılan XML tabanlı dildir. Yazılımcılar arasında XSL olarak adlandırılır, oysa sadece "XSL" farklı bir kavramdır.

Örnek XSLT aşağıdadır. Bu XSLT'yi XML dökümanını da vererek işletirsek; output olarak bir liste alırız.

XSLT:
```xml
<xsl:stylesheet>

  <xsl:output method="text"/>

  <xsl:template match="/">

    Article - <xsl:value-of select="/Article/Title"/>

    Authors: <xsl:apply-templates select="/Article/Authors/Author"/>

  </xsl:template>

  <xsl:template match="Author">

    - <xsl:value-of select="." />

  </xsl:template>

</xsl:stylesheet>
```

xml:

```xml
<Article>

  <Title>My Article</Title>

  <Authors>

    <Author>Mr. Foo</Author>

    <Author>Mr. Bar</Author>

  </Authors>

  <Body>This is my article text.</Body>

</Article>
```

output:

```
Article - My Article

Authors:

- Mr. Foo

- Mr. Bar
```

output olarak aldığımız listenin "Authors" string'inin sağına ve soluna <div> atsaydık, saf liste yerine HTML çıktısı almış olacaktık.

# XPath

XML dökümanındaki bir verinin yerini göstermek için kullanılan path yapısı. ELimizde XML olsun. İçerisinden aşağıdaki path'lerle açıklaması yazan bilgiler çekilebilir:

```
/items/philosophy[2] : Philosophy deki ikinci item

/items/*/dictionary : Bütün Sözlükler

/items/*/book[@id="12"]/@title : Id'si 12 olan kitabın title'ı

count(/items/sociology/book) : Sociology kitaplarının sayısı

/items/psychology[last()] : Son psychology kitabı

/items/*/[contains(@title,"Turk")] : İçerisinde Turk geçen bütün maddeler
```

# XQuery

SQL'e benzer şekilde; bir XML dökümanından veri çekebilmek için query dilidir. Örnek:

```xquery
for $x in doc("books.xml")/bookstore/book

where $x/price>30

order by $x/title

return $x/title
```

# XML Schema Definition (or XSD) vs document type definition (or DTD)

ikiside birbirinden bağımsız XML şeması belirtmek için yaratılmış dillerdir. XML içerisindeki elementlerin sırasını dahi belirtebilirsiniz.

# DTD

kurallara uymasını istediğimiz XML dosyasının en üstüne aşağıdaki satırı yerleştirmeliyiz.

DTD dosyası dışarıdan okunur:

```xml
<!DOCTYPE note SYSTEM "kurallar.dtd">

<note>

  <to>Jack</to>

</note>
```

yada DTD bilgileri XML'in içine gömülür:

```xml
<!DOCTYPE note [

<!ELEMENT note (to,from)>

<!ELEMENT to (#PCDATA)>

<!ELEMENT from (#PCDATA)>

]>

<note>

  <to>Jack</to>
  <from>Ayşe</from>

</note>
```

yukarıda 2'inci satırda elementlerin sırası verilmiş. 3. satırda "to" elementinin PCDATA, yani text olacağını belirtmiş.

# XSD

XML dökümanının şemasını referans etmek için:

```xml
<note h:schemaLocation="https://www.w3schools.com note.xsd">

<h:to>Jack</to>
```

XSD dosyasında her zaman root'ta "xs:schema" bulunmak zorunda.

sağ tarafta açıklamaları ile örnek:

(asadaki XSD'yi düzgün okuyabilmek için ekranı büyüt)

```xml
<xs:schema>

<xs:element name="note">    Note isimli bir element olması gerek.

  <xs:complexType>            Bu elementin içinde birden fazla element olacak

    <xs:sequence>             Child elementler XSD'de yazan sıra ile olmalı. sequence yerine all kullanırsak, sıralı olmak zorunda olmazlar.

      <xs:element name="to" type="xs:string"/>        child element olarak "to" isimli element olacak ve değeri string olacak. örnek <to>Ahmet</to>

      <xs:element name="from" type="xs:string"/>      child element olarak "from" isimli element olacak ve değeri string olacak. örnek <from>Ahmet</from>

    </xs:sequence>

  </xs:complexType>

</xs:element>

</xs:schema>
```

-  type="xs:string" yerine birçok farklı tip yazılabilir: Integer, Boolean, Date, negativeInteger, unsignedByte gibi birçok  standart belirlenmiştir.

- <xs:element name="to" type="xs:string" fixed="red"/>  to elementinin içindeki değerin sadece "red" olabilir anlamına geliyor

- fixed="red" yerine default="red" de yazabilir. bu durumda bu değer XML'de belirtilmezse, bir parser tarafından okunduğunda "red" okumasını emreder.

- aşağıdaki simpleType complex gibi değildir. simple olduğu child elementin olmayacağı anlamına gelir.

```xml
<xs:element name="age">

  <xs:simpleType>

    <xs:restriction base="xs:integer">

      <xs:minInclusive value="0"/>

      <xs:maxInclusive value="120"/>

    </xs:restriction>

  </xs:simpleType>

</xs:element>
```

restriction için farklı bir örnek:

```xml
<xs:element name="car">

  <xs:simpleType>

    <xs:restriction base="xs:string">

      <xs:enumeration value="Ferrari"/>

      <xs:enumeration value="Golf"/>

      <xs:enumeration value="BMW"/>

    </xs:restriction>

  </xs:simpleType>

</xs:element>
```

restriction kendi içinde birçok farklı özellik daha barındırıyor.

XSD'lerde tipleri önceden define edebiliriz. örnek:

```xml
<xs:element name="product" type="prodType"/>   Define ettik

<xs:complexType name="prodType">      Burada kullandık

  <xs:attribute name="prodId" type="xs:positiveInteger"/>   Attribute isteğe bağlı (konudan bağımsız bir özellik)

</xs:complexType>
```

# XML External Entities (or XXE)
XML dosyalarının tepesinde belirtilen dışa kaynakları temsil etmek için kullanılan bir terimdir. B u dış kaynaklar XML'in şemasını belirlemek için kullanılır.

XEE'de belirtilen şema dosyası dışarıda herhangi bir URL'de olduğundan, programımız o şemayı okumak istediğinde güvenlik açığı oluşabilir. Bu sebeple zorunlu değilse şema okumayı iptal etmeliyiz. Örnek Java kodu:

```java
import javax.xml.stream.XMLInputFactory;
import javax.xml.stream.XMLStreamException;
import javax.xml.stream.XMLStreamReader;

XMLInputFactory factory = XMLInputFactory.newInstance();

// disable resolving of external DTD entities
factory.setProperty(XMLInputFactory.IS_SUPPORTING_EXTERNAL_ENTITIES, Boolean.FALSE);

// or disallow DTDs entirely
factory.setProperty(XMLInputFactory.SUPPORT_DTD, Boolean.FALSE);

// load XML file
XMLStreamReader xmlStreamReader = factory.createXMLStreamReader(fis);
```

Bunun birçok güvenlik sorununa sebep olabilir:
- hacker, şemayı local dosya olarak gösterirse, uygulama local dosya okunmaya çalışacaktır. local dosya okunamazsa exception fırlatacaktır. hacker bu şekilde dosyanın varolup olmadığını anlayabilir.
- hacker, sistemi yavaşlatmak için dışarıdaki büyük bir dosyaya şema referansı verir. Şemayı parse etmek yorucu olacağından sistemi durdurma noktasına getirebilir.
