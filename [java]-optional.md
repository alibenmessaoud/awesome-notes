# Java 8 Optional Usage and Best Practices

Optional is a container object that may or may not contain a non-null value. It was introduced in Java 8 to cure the curse of `NullPointerExceptions`. In essence, Optional is a wrapper class that contains a reference to some other object. 

**Life Before Optional**

```java
private void getIsoCode( User user){
    if (user != null) {
        Address address = user.getAddress();
        if (address != null) {
            Country country = address.getCountry();
            if (country != null) {
                String isocode = country.getIsocode();
                if (isocode != null) {
                    isocode = isocode.toUpperCase();
                }
            }
        }
    }
}
```

**Creating an Optional**

- static <T> Optional<T> empty()

`Optional<String> empty = Optional.empty();`

- static <T> Optional<T> of(T value)

`Optional<String> opt = Optional.of("Ali");`

- static <T> Optional<T> ofNullable(T value)  => If null it return an empty;

`Optional<String> optional = Optional.ofNullable(null);`

**Dealing with an Optional**

- boolean isPresent()

`if (Optional.of("javaone").isPresent()) //Do something, normally a get` 

- boolean isEmpty()

`if (Optional.of("javaone").isEmpty()) //Do something, normally a get` 

- void ifPresent(Consumer<? super T> consumer)

`Optional.of("javaone").ifPresent(s -> System.out.println(s.length()))` 

- T get()

`String value = Optional.of("javaone").get();` 

- T orElse(T other)

`String value = Optional.ofNullable(null).orElse("default_name");` 

- T orElseGet(Supplier<? extends T> other)

`String name = Optional.ofNullable(nullName).orElseGet(() -> "john");`

So, what is the difference between  `orElse()` and  `orElseGet()` ? `orElseGet()` is lazy;

**Best Practices for Using Optional**

- Optional is an attempt to reduce the number of null pointer exceptions in Java.
- Optional is not meant to be a mechanism to avoid all types of null pointers. 
- The intended use of Optional is mainly as a return type.
- Do not use it as a field in a class as it is not serializable.
- Do not use it as a parameter for constructors and methods.