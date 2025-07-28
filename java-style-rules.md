# Java Style Guide

Our style guide is mainly inspired by [Google's](https://google.github.io/styleguide/javaguide.html), we also took some elements from [Oracle's](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf) ruleset. We use the following conventions:

### Line length

To read full lines on our 13" screens, lines should be at **max 140 characters long**.

**Don't 🚫**

```java
public void foo(String firstParameter, String secondParameter, String thirdParameter) {
  String result = methodWithReallyLongSignatureNameWhichAlsoTakesALotOfParameters(firstParameter, secondParameter, thirdParameter, firstField, secondExtraField)
  System.out.println(result);
}
```

**Do ✅**

```java
public void foo(String firstParameter, String secondParameter, String thirdParameter) {
  String result = methodWithReallyLongSignatureNameWhichAlsoTakesALotOfParameters(
    firstParameter, secondParameter, thirdParameter, firstField, secondExtraField
  )
  System.out.println(result);
}
```

### Indentation

We use 2 whitespaces for an indent to make the best use of the horizontal space on our screens.

**Don't 🚫**

```java
if (foo) {
    bar();
}
```

**Do ✅**

```java
if (foo) {
  bar();
}
```

### Whitespaces

We don't like trailing whitespaces because they're useless, make the code less compact, and may lead to inconsistencies.

**Don't 🚫**

```java
public void foo(String      bar ) {
  System.out.println(bar);
}
```

**Do ✅**

```java
public void foo(String bar) {
  System.out.println(bar);
}
```

### Single Statements

Although Java allows multiple statements on a single line, it's confusing to read and not useful.

**Don't 🚫**

```java
public void foo(String bar) {
  String barToUpperCase = bar.toUpperCaser(); System.out.println(bar);
}
```

**Do ✅**

```java
public void foo(String bar) {
  String barToUpperCase = bar.toUpperCaser();
  System.out.println(bar);
}
```

### Import Statements

Imports are part of the code that needs to be read too! To make them easier to read we prefer the following rules:

* One import per line
* No wildcard imports
* Imports separated into 2 groups static and non-static (separated by a single blank line)
* No blank lines between same group imports
* Imports in the same group alphabetically sorted

**Don't 🚫**

```java
import static com.mercadolibre.baz // static import not separated
import com.mercadolibre.* // wildcard

import com.mercadolibre.foo // blank line between same group imports
import com.mercadolibre.bar // not sorted
```

**Do ✅**

```java
import static com.mercadolibre.baz // static imports in the different group separated by a blank line

import com.mercadolibre.bar
import com.mercadolibre.foo // alphabetically sorted
```

### Braces

For convention popularity reasons, we've decided to go with Google's [K&R style](https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style).

**Don't 🚫**

```java
public void foo()
{
  if (bar) baz(); // optional braces not used
}
```

**Do ✅**

```java
public void foo() {
  if (bar) { baz(); } // optional braces used
}
```

### Line Wrapping

Wrapping lines may help improve readability in certain statements. To get the best out of it, we use the following rules:

* No line-wrapping in import statements
* Mandatory line-wrapping in chained method calls
* Optional line wrapping in other cases

**Don't 🚫**

```java
import com.mercadolibre
  .bar // line-wrapping in import statement

public void foo() {
  Baz baz = new Foo().getBar().getBaz(); // chained method calls not wrapped
}
```

**Do ✅**

```java
import com.mercadolibre.bar // single line for import statement

public void foo() {
  Baz baz = new Foo()
              .getBar()
              .getBaz(); // wrapped chained method calls

  printObject("baz", Baz); // OK but not mandatory
  printObject(
    "baz",
    Baz
  ) // also OK but not mandatory
}
```

### Final Variables

Java allows the use of the `final` keyword when variables' values are not modified to guarantee immutability.

* Class's fields: required when the field is never modified
* Methods' parameters: accepted but not required
* Methods' local variables: accepted but not required

**Don't 🚫**

```java
public class Foo {
  private Logger logger = LoggerFactory.getLogger(this.class); // Assumes logger is never modified so should be final
}
```

**Do ✅**

```java
public class Foo {
  private final Logger logger = LoggerFactory.getLogger(this.class); // use of final keyword
  private int count = 0; // it's ok because the counter is modified

  public void bar(final int baz) { // accepted but not required
    count++;
    final int barPlusCount = count + baz; // accepted but not required
    System.out.println(barPlusCount);
  }

}
```

#### Naming convention

| Type            | Case                       |
| ---             | ---                        |
| Packages        | lowercase (no underscores) |
| Classes         | UpperCamelCase             |
| Enum name       | UpperCamelCase             |
| Enum value      | CAPS_WITH_UNDER            |
| Exceptions      | UpperCamelCase             |
| Methods         | lowerCamelCase             |
| Constants       | CAPS_WITH_UNDER            |
| Fields          | lowerCamelCase             |
| Parameters      | lowerCamelCase             |
| Local variables | lowerCamelCase             |

#### Documentation

Javadoc comments should be present in every public method following the [Javadoc format](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html).

**Don't 🚫**

```java

public int successfullyAttacked(int incomingDamage) { // undocumented public method
    health -= incomingDamage;
    return health;
}

// attack monster
public int attack(int outgoingDamage, Monster monster) { // poorly documented method
  monster.successfullyAttacked(damage);
}

```

**Do ✅**

```java
/**
 * <p>This is a simple description of the method. . .
 * <a href="http://www.supermanisthegreatest.com">Superman!</a>
 * </p>
 * @param incomingDamage the amount of incoming damage
 * @return the amount of health hero has after attack
 * @see <a href="http://www.link_to_jira/HERO-402">HERO-402</a>
 * @since 1.0
 */
public int successfullyAttacked(int incomingDamage) {
    health -= incomingDamage;
    return health;
}
```

## [Full Java Standard Ruleset](/languages/java_rules.md)