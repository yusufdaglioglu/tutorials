############################

############################
# ALGORITHMS DATA STRUCTURES MAP
############################

############################

# Map
Working mechanism of structures like HashSet, HashMap which are based on "hash" are explained to another topic (at the topic about "hashcode").

The Map interface of Java does not have any super class. It defined as:

```java
Map<Class1, Class2>
```

For each Class1 object, there should be Class2 object. It stores the data as key-value. Some implementations of "Map":

- __HashMap__: It does not guarantees the order of a for loop which will be based on "getKeys" method.

- __LinkedHashMap__: It guarantees the order of a for loop which will be based on "getKeys" method. It orders the data in sequence of data insertion method call. In order to do that It implements the HashMap and additionally it use doubly linked list to store the order under the hood.

- __TreeMap__: It guarantees the order based on below techniques.

  - __java.lang.Comparable__

    The data classes which implemented this interface, have to implemented "int compareTo(object1)" method. TreeMap uses that method to sort the classes.

    In Java articles we can see the "natural ordering" term. This term is using only for classes which implemented "Comparable".

  - __java.util.Comparator__

    We create a class which implements this interface with "int compare(object1, object2)" method.

    This class is not implementing by any class which is storing on Map. "Comparator" implementation is a separated class from our data classes. It is an external class.

- __IdentityHashMap__: It has 2 main differences with HashMap:
  - 1- HashMap uses "equals" method to compare the data which putted inside. But instead of that IdentityHashMap uses "==" (reference compare).
  - 2- HashMap use "hashCode" method, but IdentityHashMap uses "System.identityHashCode(object)" method which return the default "hashCode" which would be returning if the class hadn't override "hashCode".

# SortedMap
It's an interface which implements Map. It is using by order matter classes.

# HashTable
It is the general data structure name of "HashMap" class of Java.

For the Java world there is a difference between HashMap vs HashTable: "java.util.Hashtable" class is thread-safe, while "java.util.HashMap" is not.

It is slow to add elements because it calculates the hash code of each object but lookup is fast.

# EnumMap
It only accepts Java Enum for the key side.
