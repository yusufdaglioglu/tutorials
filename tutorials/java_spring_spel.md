############################

############################
# JAVA SPRING SPEL
############################

############################


# spEL (or Spring Expression Language)

2 temel işlevi vardır:
- regex işlemleri
- Bean'e property atamalarında kullanılan özel bir syntax dilidir

Bean property atamalarına örnekler verelim:

örnek:

```xml
<property name="message" value="Available thread number is #{100+1}"/>
```

yada

```java
@Value("#{100+1}")
private String message;
```

Her property set edildiğinde (her yeni instance oluşturulduğunda), set edilecek 1 artacaktır. ilk değer 100 olacaktır.

Diğer örnekler:

Diğer örnekler:

```java
@Value("#{1 == 1}") // true
private boolean equal;

@Value("#{1 eq 1}") // true
private boolean equalAlphabetic;

@Value("#{1 != 1}") // false
private boolean notEqual;

@Value("#{1 ne 1}") // false
private boolean notEqualAlphabetic;

@Value("#{1 < 1}") // false
private boolean lessThan;

@Value("#{1 lt 1}") // false
private boolean lessThanAlphabetic;

@Value("#{1 <= 1}") // true
private boolean lessThanOrEqual;

@Value("#{1 le 1}") // true
private boolean lessThanOrEqualAlphabetic;

@Value("#{1 > 1}") // false
private boolean greaterThan;

@Value("#{1 gt 1}") // false
private boolean greaterThanAlphabetic;

@Value("#{1 >= 1}") // true
private boolean greaterThanOrEqual;

@Value("#{1 ge 1}") // true
private boolean greaterThanOrEqualAlphabetic;

@Value("#{250 > 200 && 200 < 4000}") // true
private boolean and;

@Value("#{250 > 200 and 200 < 4000}") // true
private boolean andAlphabetic;

@Value("#{400 > 300 || 150 < 100}") // true
private boolean or;

@Value("#{400 > 300 or 150 < 100}") // true
private boolean orAlphabetic;

@Value("#{!true}") // false
private boolean not;

@Value("#{not true}") // false
private boolean notAlphabetic;

@Value("#{'100' matches '\\d+' }") // true
private boolean validNumericStringResult;

@Value("#{'100xyzabc' matches '\\d+' }") // false
private boolean invalidNumericStringResult;

@Value("#{'valid alphabetic string' matches '[a-zA-Z\\s]+' }") // true
private boolean validAlphabeticStringResult;

@Value("#{'invalid alphabetic string #$1' matches '[a-zA-Z\\s]+' }") // false
private boolean invalidAlphabeticStringResult;

@Value("#{someBean.someValue matches '\d+'}") // true if someValue contains only digits
private boolean validNumericValue;
```

spEL aynı zamanda regex işlemlerini de yapıyor:

```java
Expression expression = expressionParser.parseExpression("'Any string'.bytes");
Expression expression = expressionParser.parseExpression("'Any string'.replace(\" \", \"\").length()");
Integer result = (Integer) expression.getValue();
```
