**Encapsulation**

Encapsulation is the core principle of OOP that makes objects solid, cohesive, trustworthy, etc. It makes more than we think. Encapsulation leads to the absence of *naked* data on all levels and in all forms [1];

For example, the only way to encapsulate the temperature in an object is to make sure nobody can touch the temperature value it either directly or by retrieving it from Temperature object. How do we do that? Just <u>stop exposing data and start exposing functionality</u>. No accessors or modifiers! Just add a method to format and return the temperature. If we need other functions in the future, like math operations or conversion to Celsius, we add more methods to class Temperature. But we never let anyone touch or know about t.



<u>References</u>

[1]: https://www.yegor256.com/2016/11/21/naked-data.html. 	"@yegor256"