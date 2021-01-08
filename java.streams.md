# Stream API

- source
- transformation actions operations pipes
- terminal operations



**Sources**

- any collection (list.stream())
- Arrays.stream(new int[] {1,2,3})
- Stream.of(value1, value2), Stream.iterate, Stream.builder....
	Supplier<TO> - takes nothing, returns value (generating from nothing)
	
- bufferedReader.lines()

**Actions**

- sorted,
- map, (Function<FROM, TO> ) (BiFunction<FROM_A, FROM_B, RETURN>)
- filter, (Predicate<FROM> - return boolean)
- flatMap, 
- peek (Consumer<FROM> - return void) (BiConsumer<FROM_A, FROM_B> - return void)

**Terminal**

- forEach
	Consumer (x) -> System.out.println(x)
	BiConsumer (a, b) -> ...

- collect (Collectors)
	- groupingBy
	- toMap
	- toList
	- toSet
	- joining
	- mapping
	- counting
	
- reduce
- count
- findFirst
- anyMatch
- findAny

**Examples**

### Task 1 - Find max long

```java
@Test
public void testMax() {
    Long number = hello.max(new String[]{"77", "332", null, "10", "500", ""});
    assertEquals(new Long(500), number);
}

Long max(String[] values) {
     Optional<Long> max = Arrays.stream(values)
             .filter(s -> s != null && s.length() > 0)
             .map(s -> new Long(s.trim()))
             .max((a, b) -> a < b ? -1 : 0);
 
     return max.isPresent() ? max.get() : null;
}
```

### Task 2 - Build a big number

```java
@Test
public void testBigNumber() {
     Long number = hello.bignumber(new String[]{"77", "332", null, "10", "500"});
     assertEquals(new Long(5003327710L), number);
 
     Long n2 = hello.bignumber(new String[]{"332", "", "500", null});
     assertEquals(new Long(500332), n2);
}

Long bignumber(String[] values) {
     Comparator<Integer> desc = (a, b) -> a > b ? -1 : 0;
    
     String reduced = Arrays.stream(values)
           .filter(s -> s != null && s.length() > 0)
           .map(s -> new Integer(s.trim()))
           .sorted(desc)
           .map(i -> i + "").reduce("", (a, b) -> a + b);
 
      return new Long(reduced);
}
```

### Task 3 - Sort the characters in a string

```java
@Test
public void testSortString() {
    String reversed = hello.sortString("zabcxdef12");
    assertEquals("zxfedcba21", reversed);
}

String sortString(String value) {
    StringBuffer buffer = new StringBuffer();
    value.chars().boxed()
        .sorted(desc)
        .forEach(i -> buffer.append((char) (i.byteValue())));

    return buffer.toString();
}
```

### Task 4 - Find sum of prime numbers

```java
@Test
public void testSumPrimes() {
    Integer sum = hello.sumPrimes(new int[]{1, 4, 5, 5, 7, 9, 13, 21, 30, 7});
    assertEquals(new Integer(37), sum);
}
Integer sumPrimes(int[] numbers) {
    Integer sum = Arrays.stream(numbers).boxed()
        .filter(i -> {
            for (int a = 3; a <= i; a += 2) {
                if (i % a == 0) return false;
            }
            return true;
        })
        .reduce(0, (a, b) -> a + b);

    return sum;
}
//
Integer sumPrimes(int[] numbers) {
    Integer sum = Arrays.stream(numbers).boxed()
        .filter(i -> !(i % 2 == 0 || i == 1))
        .filter(i -> {
            int ceil = (int) Math.floor(Math.sqrt(i));

            for (int a = 3; a <= ceil; a += 2) {
                if (i % a == 0) return false;
            }
            return true;
        })
        .reduce(0, (a, b) -> a + b);

    return sum;
}
```

### Task 5 - Parse string and extract values

```java
@Test
public void testJoinNumber() {
    String joined = hello.joinNumber("11 ,  34,,   45 ,,  88, 99||22");
    assertEquals("998845342211", joined);
}

// clue
/* very simple - match one or more spaces or a comma*/
String[] words = value.split("\\s+|,");
 
 /* more complex - match one or more spaces or one or more commas or one or more | and repeat altogether one or more times */
String[] values = value.split("(\\s+|,+|\\|+)+");

String joinNumber(String value) {
        String[] values = value.split("(\\s+|,+|\\|+)+");
        String joined = Arrays.stream(values)
                .sorted(stringdesc)
                .reduce("", (a, b) -> a + b);
 
        return joined;
    }
 
Comparator<String> stringdesc = (a, b) -> b.compareTo(a);
```

### Task 6 - Compute the factorial of a number

```java
@Test
public void testFactorial() {
    /* use recursion */
    int factorial = hello.factorial(5);
    assertEquals(120, factorial);

    /* use streams */
    int fact2 = hello.fact2(5);
    assertEquals(120, factorial);

}
int factorial(int number) {
    if (number == 1) return 1;
    return number * factorial(number - 1);
}
int fact2(int number) {
    return IntStream.rangeClosed(2, number)
        .reduce(1, (a, b) -> a * b);
}
```

### Task 7 - Reverse a string

```java
@Test
public void testReverse() {
    /* classic solution */
    String reversed = hello.reverse("zabcxdef12");
    assertEquals("21fedxcbaz", reversed);

    /* stream based */
    String reversed2 = hello.reverse2("zabcxdef12");
    assertEquals("21fedxcbaz", reversed2);

    /* trivial */
    String reversed3 = hello.reverse3("zabcxdef12");
    assertEquals("21fedxcbaz", reversed3);
}

String reverse(String value) {
    StringBuffer buffer = new StringBuffer();
    for (int i = value.length(); i > 0; i--) {
        buffer.append(value.charAt(i - 1));
    }

    return buffer.toString();
}

String reverse2(String value) {
    StringBuffer buffer = new StringBuffer();
    value.chars().forEach( c -> buffer.insert(0, (char) c));

    return buffer.toString();
}

String reverse3(String value) {
    return new StringBuffer(value).reverse().toString();

}
```

### Task 8 - Read a file and parse numbers

```java
/* data.txt */
 100 28 11 1001
 10 09 35 44 51 543 34 21 27 1
 45 28 96 75 41 66 73 89 65 48
     
@Test
public void testLoadData() throws IOException {
     /* InputStream */
     String[] lines = hello.loadfile("data.txt");
     assertEquals(3, lines.length);

     /* Files */
     String[] ll = hello.loadfile2("data.txt");
     assertEquals(3, ll.length);
 }

String[] loadfile(String filename) throws FileNotFoundException, IOException {

    File file = new File(filename);
    InputStreamReader input = new InputStreamReader(new FileInputStream(file));
    BufferedReader reader = new BufferedReader(input);

    List<String> lines = new ArrayList<>();
    String line = null;
    while ((line = reader.readLine()) != null) {
        lines.add(line);
    }

    return lines.toArray(new String[lines.size()]);
}

String[] loadfile2(String filename) throws FileNotFoundException, IOException {
 
    File file = new File(filename);
    List<String> lines = Files.readAllLines(file.toPath());

    return lines.toArray(new String[lines.size()]);
}

@Test
public void testLoadNumbers() throws IOException {
    List<integer> numbers = hello.loadNumbers("data.txt");

    assertEquals(24, numbers.size());
    assertEquals(new Integer(1001), numbers.get(0));
    assertEquals(new Integer(1), numbers.get(23));
}

List<integer> loadNumbers(String filename) throws FileNotFoundException, IOException {
    File file = new File(filename);
    InputStreamReader input = new InputStreamReader(new FileInputStream(file));
    BufferedReader reader = new BufferedReader(input);

    List<integer> numbers = new ArrayList<>();
    String line;
    while ((line = reader.readLine()) != null) {
        numbers.addAll(Stream.of(line.split("\\s"))
                       .map(Integer::valueOf)
                       .collect(Collectors.toList()));
    }

    return numbers.stream()
        .sorted((a, b) -> a > b ? -1 : 0)
        .collect(Collectors.toList());
}
```

