# Test-Driven Development by example
##### By Kent Beck

**Basic principles**
1. Quickly add a test.
2. Run all tests and see the new one fail.
3. Make a little change.
4. Run all tests and see them all succeed.
5. Refactor to remove duplication.

The main idea is to work quickly, add a small test and add the functionality to make it pass. Even if you have to "Fake it" and add constants or return nulls.

Have a list of functionality you need to add, if you have something that requires more than one test/is a bit complex, break it into smaller parts.

The slogan is clean code that works. Test to make sure you have the works part and clean up afterwards and use your tests to make sure you did not break anything.

* Three approaches to making a test work cleanly:
  * Fake it: Add constants or return the values you need to make your test pass.
  * Triangulation: Generalize code when we have two examples and more.
  * Obvious implementation: Type in the real implementation (if it's simple and straight forwards)
* Removing duplication between test and code to drive the design.

**Testing patterns**

Tests should be isolated from each other, so one test passing is not dependent on another test passing.

Start by writing a list of all the tests you know you have to write.

Test your code/functionality before you write it.

Assert first and then build the objects and methods you need from that
```java
testCompleteTransaction() {
  ...
  assertTrue(reader.isClosed());
  assertEquals("abc", reply.contents());
}
```
```java
testCompleteTransaction() {
  ...
  Buffer reply = eader.contents();
  assertTrue(reader.isClosed());
  assertEquals("abc", reply.contents());
}
```

```java
testCompleteTransaction() {
  ...
  Socket reader = socket("localhost", defaultPort);
  Buffer reply = eader.contents();
  assertTrue(reader.isClosed());
  assertEquals("abc", reply.contents());
}
```

```java
testCompleteTransaction() {
  Server writer = Server(defaultPort(), "abc");
  Socket reader = socket("localhost", defaultPort());
  Buffer reply = eader.contents();
  assertTrue(reader.isClosed());
  assertEquals("abc", reply.contents());
}
```

Use data that makes your code easy to read and follow

Include expected and actual output in the test
