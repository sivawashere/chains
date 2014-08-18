chains
======

A library for chaining computations in Java 8.

### What are Chains?
Chains are data structures that allow the developer to chain computations together. For the mathematically inclined, they are like monads, but more permissive.

For example, say you want to get user input from the command line and print it to standard out. In Java, this would go like this:

```
import java.util.Scanner;

public class InputTest {
  public static void main(String[] args) {
    System.out.print("Enter input here: ");
    try (Scanner in = new Scanner(System.in)) {
      System.out.println(in.nextLine());
    }
  }
}
```
AHHH! THAT WAS A WHOLE FOUR LINES OF CODE! Now here's how you do it with chains.

```java
import chains.IO;

public class ChainsInputTest {
  public static void main(String[] args) {
    IO.print("Enter input here: ").link(IO.getLine()).chain(IO::println);
  }
}
```
Only one line of code? Amazing! But let's try something more involved, like writing user input to a file. This is regular Java:

```java
import java.io.PrintWriter;
import java.io.FileNotFoundException;

public class WriterTest {
  public static void main(String[] args) {
    System.out.print("Enter input here: ");
    try (Scanner in = new Scanner(System.in);
         PrintWriter out = new PrintWriter("testout")) {
      out.write(in.nextLine());
    }
  }
}
```

The number of lines this simple task took is uncountable. Luckily, the chains approach is much easier.

```java
import chains.IO;

public class ChainsWriterTest {
  public static void main(String[] args) {
           IO.print("Enter input here: ")
     .link(IO.getLine())
    .chain((String out) -> IO.writeFile("testout", out));
  }
}
```

Much cleaner, and no need to handle resources yourself! MINDBLOWING! Of course, IO isn't the only chain that comes with the package.

Now here's something monads wouldn't let you do: mixing and matching chains. For example, in Haskell you wouldn't be able to pipe the result of a Maybe monad into IO, then into Either, and then back to IO again. Because it's not type correct, apparently. In Java we say to hell with type correctness!
