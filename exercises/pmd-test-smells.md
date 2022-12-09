# Detecting test smells with PMD

In folder [`pmd-documentation`](../pmd-documentation) you will find the documentation of a selection of PMD rules designed to catch test smells.
Identify which of the test smells discussed in classes are implemented by these rules.

Use one of the rules to detect a test smell in one of the following projects:

- [Apache Commons Collections](https://github.com/apache/commons-collections)
- [Apache Commons CLI](https://github.com/apache/commons-cli)
- [Apache Commons Math](https://github.com/apache/commons-math)
- [Apache Commons Lang](https://github.com/apache/commons-lang)

Discuss the test smell you found with the help of PMD and propose here an improvement.
Include the improved test code in this file.

## Answer

Les "test smells"  En classe, les "test smells" étudiés que nous retrouvons dans le dossier pmd-documentation sont : JUnit4TestShouldUseAfterAnnotation ; JUnit4TestShouldUseBeforeAnnotation ; JUnit4TestShouldUseBeforeAnnotation ; JUnitAssertionsShouldIncludeMessage ; JUnitTestContainsTooManyAsserts ; JUnitTestsShouldIncludeAssert ; UseAssertEqualsInsteadOfAssertTrue .

Nous nous servons des règles PMD JUnitTestContainsTooManyAsserts sur les projet commons-collection-master et nous obtenons ce résultat :

/commons-collections-master/src/test/java/org/apache/commons/collections4/iterators/SingletonIteratorTest.java:68:JUnitTestContainsTooManyAsserts:	Unit tests should not contain more than 1 assert(s).

Il nous faut faire une réduction du nombre d'assertions dans le test pour apporter une correction des erreurs :
```java
  @Test
    public void testIterator() {
        final Iterator<E> iter = makeObject();
        assertTrue("Iterator has a first item", iter.hasNext());

        final E iterValue = iter.next();
        assertEquals("Iteration value is correct", testValue, iterValue);

        assertFalse("Iterator should now be empty", iter.hasNext());

        try {
            iter.next();
        } catch (final Exception e) {
            assertEquals("NoSuchElementException must be thrown", e.getClass(), new NoSuchElementException().getClass());
        }
    }
```
Nous remarquons que ce test comporte 4 assertions. Le soucis, est que, si ce test échouait, nous ne connaitrions pas exactement l'origine de l'erreur. Nous allons donc faire une séparation de ce test en 04 tests unitaires différents :

```java

@Test
    public void testIterator() {
        final Iterator<E> iter = makeObject();
        assertTrue("Iterator has a first item", iter.hasNext());
    }

```

```java

@Test
    public void testIterator2() {
        final E iterValue = iter.next();
        assertEquals("Iteration value is correct", testValue, iterValue);
    }

```

```java

@Test
    public void testEmptyIterator() {
        assertFalse("Iterator should now be empty", iter.hasNext());
    }

```

```java

@Test
    public void testexceptionIterator(){
       try {
            iter.next();
        } catch (final Exception e) {
            assertEquals("NoSuchElementException must be thrown", e.getClass(), new NoSuchElementException().getClass());
        } 
    }

```
