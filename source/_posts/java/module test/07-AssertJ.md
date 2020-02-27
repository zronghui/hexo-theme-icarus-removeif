---
title: 07 AssertJ
description: 07 AssertJ
date: 2020-02-23 18:00:34
categories:
- java
- module test
toc: true
tags:
---

[toc]

[AssertJ一分钟入门 - 简书](https://www.jianshu.com/p/e078043071ff)
[AssertJ / Fluent assertions for java](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html)

## maven

```xml
<dependency>
  <groupId>org.assertj</groupId>
  <artifactId>assertj-core</artifactId>
  <version>3.11.1</version>
</dependency>
```



## code

```java
package AssertJ;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

//import static org.assertj.core.api.Assertions.*;
import static org.assertj.core.api.Assertions.assertThat;

public class AssertJTest {
    public static void main(String[] args) {
        assertThat("aa".equals("aa"));
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 1, 2, 3);
        assertThat(list).contains(1);
        assertThat(list).containsOnly(1, 2, 3);
    }
}
```

### Basic tips :

#### Java 8 assertions, see release notes : [3.8.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.8.0) / [3.7.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.7.0) / [3.6.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.6.0) / [3.5.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.5.0) / [3.4.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.4.0) / [3.3.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.3.0) /[3.2.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.2.0) / [3.1.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.1.0) / [3.0.0](https://joel-costigliola.github.io/assertj/assertj-core-news.html#assertj-core-3.0.0)

#### [IDE configuration to directly get assertThat in code completion](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#ide-automatic-static-import)

#### [Describe your assertion using as(String description, Object... args)](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#describe-assertion)

*call as() before the assertion*

```java
assertThat(frodo.getAge()).as("check %s's age", frodo.getName()).isEqualTo(100);
// The error message starts with the given description in [] :
// [check Frodo's age] expected:<100> but was:<33>
```



#### [Exception assertions guide](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#exception-assertion)

#### [Using String assertions on the content of a file](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#file-content-string-assertions)

### Iterable and arrays assertions :

#### [Combining filtering and assertions on iterables or arrays](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#filters)

```java
// Filtering can be done on arrays or iterables. Filter criteria are expressed by :
// - a Java 8 Predicate
//    using simple predicate(谓语？), best expressed with a lambda
assertThat(fellowshipOfTheRing).filteredOn( character -> character.getName().contains("o") )
                               .containsOnly(aragorn, frodo, legolas, boromir);
// - Filtering on a property or a field
assertThat(fellowshipOfTheRing).filteredOn("race", MAN)
                               .filteredOn("name", not("Boromir"))
                               .containsOnly(aragorn);
// - Filtering with a Condition
import org.assertj.core.api.Condition;

Condition<Player> mvpStats= new Condition<Player>() {
  @Override
  public boolean matches(Player player) {
    return player.pointsPerGame() > 20 && (player.assistsPerGame() >= 8 || player.reboundsPerGame() >= 8);
  }
};

List<Player> players;
players.add(rose); // Derrick Rose : 25 ppg - 8 assists - 5 rebounds
players.add(lebron); // Lebron James : 27 ppg - 6 assists - 9 rebounds
players.add(noah); // Joachim Noah : 8 ppg - 5 assists - 11 rebounds

assertThat(players).filteredOn(mvpStats)
                   .containsOnly(rose, lebron);
```



#### [Assertions on extracted properties/fields of iterable/array elements](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#extracted-properties-assertion)

```java
assertThat(fellowshipOfTheRing).extracting("name")
                               .contains("Boromir", "Gandalf", "Frodo", "Legolas")
                               .doesNotContain("Sauron", "Elrond");

// when checking several properties/fields you have to use tuples :
import static org.assertj.core.api.Assertions.tuple;

// extracting name, age and and race.name nested property
assertThat(fellowshipOfTheRing).extracting("name", "age", "race.name")
                               .contains(tuple("Boromir", 37, "Man"),
                                         tuple("Sam", 38, "Hobbit"),
                                         tuple("Legolas", 1000, "Elf"));
```



#### [Flat(map) extracting](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#flat-extracted-properties-assertion)

```java
List<Player> reallyGoodPlayers = list(jordan, magic);

// check all team mates by specifying the teamMates property (Player has a getTeamMates() method):
assertThat(reallyGoodPlayers).flatExtracting("teamMates")
                             .contains(pippen, kukoc, jabbar, worthy);

// alternatively, you can implement an Extractor to extract the team mates:
assertThat(reallyGoodPlayers).flatExtracting(teamMates)
                             .contains(pippen, kukoc, jabbar, worthy);

// where teamMates is an instance of PlayerTeammatesExtractor:
public class PlayerTeammatesExtractor implements Extractor<Player, List<Player>> {
  @Override
  public List<Player> extract(Player input) {
    return input.getTeamMates();
  }
}
```



#### [Assertions on results of a method call on iterable/array elements](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#extracted-method-result-assertion)

```java
// Like extracting but instead of extracting properties/fields
// ** it extracts the result of a given method invocation on the elements of the Iterable/Array under test and puts the results into a new Iterable/Array which becomes the object under test.
// WesterosHouse class has a method: public String sayTheWords()
List<WesterosHouse> greatHouses = new ArrayList<WesterosHouse>();
greatHouses.add(new WesterosHouse("Stark", "Winter is Coming"));
greatHouses.add(new WesterosHouse("Lannister", "Hear Me Roar!"));
greatHouses.add(new WesterosHouse("Greyjoy", "We Do Not Sow"));
greatHouses.add(new WesterosHouse("Baratheon", "Our is the Fury"));
greatHouses.add(new WesterosHouse("Martell", "Unbowed, Unbent, Unbroken"));
greatHouses.add(new WesterosHouse("Tyrell", "Growing Strong"));

// let's verify the words of the great houses of Westeros:
// 对每个对象调用 sayTheWords 方法
assertThat(greatHouses).extractingResultOf("sayTheWords")
                       .contains("Winter is Coming", "We Do Not Sow", "Hear Me Roar")
                       .doesNotContain("Lannisters always pay their debts");
```



### Advanced tips :

#### [Gather all errors message with soft assertions](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#soft-assertions)

```java
// Using soft assertions, AssertJ collects all assertion errors instead of stopping at the first one.
// use SoftAssertions instead of direct assertThat methods
SoftAssertions softly = new SoftAssertions();
softly.assertThat(mansion.guests()).as("Living Guests").isEqualTo(7);
softly.assertThat(mansion.kitchen()).as("Kitchen").isEqualTo("clean");
softly.assertThat(mansion.library()).as("Library").isEqualTo("clean");
softly.assertThat(mansion.revolverAmmo()).as("Revolver Ammo").isEqualTo(6);
softly.assertThat(mansion.candlestick()).as("Candlestick").isEqualTo("pristine");
softly.assertThat(mansion.colonel()).as("Colonel").isEqualTo("well kempt");
softly.assertThat(mansion.professor()).as("Professor").isEqualTo("well kempt");
// Don't forget to call SoftAssertions global verification !
softly.assertAll();
// When the collected assertions are all asserted together they yield a more descriptive error message:
// org.assertj.core.api.SoftAssertionError:
//     The following 4 assertions failed:
//     1) [Living Guests] expected:<[7]> but was:<[6]>
//     2) [Library] expected:<'[clean]'> but was:<'[messy]'>
//     3) [Candlestick] expected:<'[pristine]'> but was:<'[bent]'>
//     4) [Professor] expected:<'[well kempt]'> but was:<'[bloodied and dishevelled]'>

// AssertJ also provides a few ways to avoid having to call softly.assertAll() manually:

// JUnitSoftAssertions with a JUnit rule
@Rule
public final JUnitSoftAssertions softly = new JUnitSoftAssertions();

@Test
public void host_dinner_party_where_nobody_dies() {
   Mansion mansion = new Mansion();
   mansion.hostPotentiallyMurderousDinnerParty();
   // use SoftAssertions instead of direct assertThat methods
   softly.assertThat(mansion.guests()).as("Living Guests").isEqualTo(7);
   // ...
   // No need to call softly.assertAll(), it is automatically done by JUnitSoftAssertions rule
}
// AutoCloseableSoftAssertions
@Test
public void host_dinner_party_where_nobody_dies() {
   Mansion mansion = new Mansion();
   mansion.hostPotentiallyMurderousDinnerParty();
   try (AutoCloseableSoftAssertions softly = new AutoCloseableSoftAssertions()) {
      softly.assertThat(mansion.guests()).as("Living Guests").isEqualTo(7);
      // ...
      // no need to call assertAll, it is done when softly is closed.
   }
}
// Using the static assertSoftly method
@Test
public void host_dinner_party_where_nobody_dies() {
   Mansion mansion = new Mansion();
   mansion.hostPotentiallyMurderousDinnerParty();
   SoftAssertions.assertSoftly(softly -> {
      softly.assertThat(mansion.guests()).as("Living Guests").isEqualTo(7);
      // ...
      // no need to call assertAll, it is done by assertSoftly.
   });
}
```

#### Using String assertions on the content of a file

```java
File xFile = writeFile("xFile", "The Truth Is Out There");

// classic File assertions
assertThat(xFile).exists().isFile().isRelative();

// String assertions on the file content : contentOf() comes from Assertions.contentOf static import
assertThat(contentOf(xFile)).startsWith("The Truth").contains("Is Out").endsWith("There");
```

### Exception assertions guide

```java
assertThatThrownBy(() -> { throw new Exception("boom!"); }).isInstanceOf(Exception.class)
                                                             .hasMessageContaining("boom");
// 或者
assertThatExceptionOfType(IOException.class).isThrownBy(() -> { throw new IOException("boom!"); })
                                               .withMessage("%s!", "boom")
                                               .withMessageContaining("boom")
                                               .withNoCause();

// 或者
// when
Throwable thrown = catchThrowable(() -> { throw new Exception("boom!"); });

// then
assertThat(thrown).isInstanceOf(Exception.class)
  .hasMessageContaining("boom");

// 无错
assertThatCode(() -> {
  // code that should throw an exception
  ...
}).doesNotThrowAnyException();

```



#### [Using a custom comparison strategy in assertions](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#custom-comparison-strategy)

```java
// usingComparator(Comparator) example :

// frodo and sam are instances of TolkienCharacter with Hobbit race (obviously :)), they are not equal
assertThat(frodo).isNotEqualTo(sam);
// ... but if we compare race only, they are (raceComparator implements Comparator<TolkienCharacter>)
assertThat(frodo).usingComparator(raceComparator).isEqualTo(sam);

// usingElementComparator(Comparator) example :
// standard comparison : the fellowshipOfTheRing includes Gandalf but not Sauron (believe me) ...
assertThat(fellowshipOfTheRing).contains(gandalf).doesNotContain(sauron);
// ... but if we compare race only, Sauron is in fellowshipOfTheRing (he's a Maia like Gandalf)
assertThat(fellowshipOfTheRing).usingElementComparator(raceComparator).contains(sauron);
```



#### [Field by field comparisons](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#field-by-field-comparison)

- [isEqualToComparingFieldByField](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#field-by-field) : compares all fields/properties including inherited ones - not recursive
- [isEqualToComparingOnlyGivenFields](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#field-by-field-only) : compares only the specified fields/properties - not recursive
- [isEqualToIgnoringGivenFields](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#field-by-field-ignoring) : compares all fields/properties except specified ones - not recursive
- [isEqualToIgnoringNullFields](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#field-by-field-null) : compares non null fields/properties only - not recursive
- [isEqualToComparingFieldByFieldRecursively](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#field-by-field-recursive) : compares all fields/properties recursively

```java
TolkienCharacter frodo = new TolkienCharacter("Frodo", 33, HOBBIT);
TolkienCharacter frodoClone = new TolkienCharacter("Frodo", 33, HOBBIT);
// 1. isEqualToComparingFieldByField
// Fail as equals compares object references
assertThat(frodo).isEqualsTo(frodoClone);
// frodo and frodoClone are equal when doing a field by field comparison.
assertThat(frodo).isEqualToComparingFieldByField(frodoClone);

// 2. isEqualToComparingOnlyGivenFields
// frodo and sam both are hobbits, so they are equal when comparing only race
assertThat(frodo).isEqualToComparingOnlyGivenFields(sam, "race"); // OK
// they are also equals when comparing only race name (nested field).
assertThat(frodo).isEqualToComparingOnlyGivenFields(sam, "race.name"); // OK
// ... but not when comparing both name and race
assertThat(frodo).isEqualToComparingOnlyGivenFields(sam, "name", "race"); // FAIL

// 3. isEqualToIgnoringGivenFields
// frodo and sam are equal when ignoring name and age as the only remaining field is race
assertThat(frodo).isEqualToIgnoringGivenFields(sam, "name", "age"); // OK both are HOBBIT
// ... but they are not equals if only age is ignored as their names differ.
assertThat(frodo).isEqualToIgnoringGivenFields(sam, "age"); // FAIL

// 4. isEqualToIgnoringNullFields
TolkienCharacter frodo = new TolkienCharacter("Frodo", 33, HOBBIT);
TolkienCharacter mysteriousHobbit = new TolkienCharacter(null, 33, HOBBIT);
// Null fields in expected object are ignored, mysteriousHobbit has a null name thus it's ignored
assertThat(frodo).isEqualToIgnoringNullFields(mysteriousHobbit); // OK
// ... but this is not reversible !
assertThat(mysteriousHobbit).isEqualToIgnoringNullFields(frodo); // FAIL


```



#### [Using a custom representation in assertions](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#custom-representation)

```java

```



#### [Extending assertions with conditions](https://joel-costigliola.github.io/assertj/assertj-core-conditions.html)

```java

```



#### [Creating assertions for your domain](https://joel-costigliola.github.io/assertj/assertj-core-custom-assertions.html)

```java

```

