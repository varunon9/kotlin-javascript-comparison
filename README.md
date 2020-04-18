# Kotlin vs Javascript - Learning by comparison

## var, val & let, const

These are used to declare a variable.
`var` & `let` can be assigned multiple times wile `val` and `const` cannot.

Kotlin-

```
var a = 1
a = 2
println(a) // 2

val b = 1
b = 2 // error: val cannot be reassigned
```

Javascript-

```
let a = 1;
a = 2;
console.log(a); // 2

const b = 1;
b = 2; // Uncaught TypeError: Assignment to constant variable.
```

## Functions as first class citizen

It means that functions can be assigned to the variables, passed as an arguments or returned from another function.

Kotlin-

```
fun main(args: Array<String>) {
  fun greetings(greet: (String) -> Unit): Unit {
    greet("en")
    greet("hi")
  }

  val greet = fun(lang: String): Unit {
    if (lang == "en") {
      println("Hello")
    } else if (lang == "hi") {
      println("नमस्ते")
    }
  }

  greetings(greet) // Hello, नमस्ते
}
```

Javascript-

```
(function() {
  function greetings(greet) {
    greet("en");
    greet("hi");
  }

  const greet = function(lang) {
    if (lang == "en") {
      console.log("Hello")
    } else if (lang == "hi") {
      console.log("नमस्ते")
    }
  }

  greetings(greet) // Hello, नमस्ते
})();
```

## Default arguments

Function parameters can have default values, which are used when a corresponding argument is omitted.

Kotlin-

```
fun main(args: Array<String>) {
  fun greet(lang: String = "en") {
    if (lang == "en") {
      println("Hello")
    } else if (lang == "hi") {
      println("नमस्ते")
    }
  }

  greet() // Hello
  greet("hi") // नमस्ते
}
```

Javascript-

```
(function() {
  function greet(lang = "en") {
    if (lang == "en") {
      console.log("Hello");
    } else if (lang == "hi") {
      console.log("नमस्ते");
    }
  }

  greet(); // Hello
  greet("hi"); // नमस्ते
})();
```

## Extension function & prototype

Kotlin extension function provides a facility to "add" methods to class without inheriting a class or using any type of design pattern.
Same can be achieved using Prototype in Javascript.

Kotlin-

```
fun Int.isEven(): Boolean {
  return this % 2 == 0
}

fun main(args: Array<String>) {
  val a = 11
  val b = 12

  println(a.isEven()) // false
  println(b.isEven()) // true
}
```

Javascript-

```
(function() {
  Number.prototype.isEven = function() {
    return this % 2 === 0;
  };

  const a = 11;
  const b = 12;

  console.log(a.isEven()); // false
  console.log(b.isEven()); // true
})();
```

## Lambda & arrow functions

Kotlin-

```
fun main(args: Array<String>) {
  val timesTwo = { num: Int -> num * 2 }
  val add: (Int, Int) -> Int = { a, b -> a + b }
  val isEven: (Int) -> Boolean = { it % 2 == 0 }

  println(timesTwo(2)) // 4
  println(add(3, 4)) // 7
  println(isEven(4)) // true

  val list = listOf(1, 2, 3, 4, 5, 6)
  val modifiedList = list
    .filter({ isEven(it) })
    .map({ element -> timesTwo(element) })

  println(modifiedList) // [4, 8, 12]
}
```

Javascript-

```
(function() {
  const timesTwo = num => num * 2;
  const add = (a, b) => {
    return a + b;
  };
  const isEven = num => num % 2 === 0;

  console.log(timesTwo(2)) // 4
  console.log(add(3, 4)) // 7
  console.log(isEven(4)) // true

  const arr = [1, 2, 3, 4, 5, 6];
  const modifiedArr = arr
    .filter(element => isEven(element))
    .map(element => timesTwo(element));

  console.log(modifiedArr); // [4, 8, 12]
})();
```

## Import alias

Kotlin-

```
import java.sql.Date as SqlDate
```

Javascript-

```
import CustomDate from './util/Date'
// or
import { Date as CustomDate } from './util/Date'
```

## Variable number of arguments

Kotlin-

```
fun main(args: Array<String>) {
  fun sum(vararg args: Int): Int {
    var sum = 0
    for (arg in args) {
      sum += arg
    }
    return sum
  }

  val result = sum(1, 2, 3, 4, 5)
  println(result) // 15
}
```

Javascript-

```
(function() {
  function sum(...args) {
    let sum = 0;
    for (arg of args) {
      sum += arg;
    }
    return sum;
  }

  const result = sum(1, 2, 3, 4, 5);
  console.log(result); // 15
})();
```

## Closure

Closure means that an inner function always has access to the vars and parameters of its outer function, even after the outer function has returned.

Kotlin-

```
fun main(args: Array<String>) {
  fun nextChar(str: String): () -> Char {
    var i = 0
    return fun(): Char {
      return str[i++]
    }
  }

  val getNextChar = nextChar("goibibo")
  println(getNextChar()) // g
  println(getNextChar()) // o
  println(getNextChar()) // i
}
```

Javascript-

```
(function() {
  function nextChar(str) {
    let i = 0;
    return function() {
      return str[i++];
    };
  }
  const getNextChar = nextChar('goibibo');
  console.log(getNextChar()); // g
  console.log(getNextChar()); // o
  console.log(getNextChar()); // i
})();
```

## Coroutine & async, await

These help in writing asynchronous code in a synchronous way.

Kotlin-

```
val deferredHeroesList: Deferred<List<String>> = GlobalScope.async {
  delay(1000L) // mocking network call
  listOf("Captain America", "Batman", "Superman", "Iron Man") // data from backend
}

GlobalScope.launch {
  val heroesList = deferredHeroesList.await()
  println(heroesList) // printed after 1 sec
}

println("end of main function") // gets printed first
```

Javascript-

```
(function() {
  function deferredHeroesList() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(["Captain America", "Batman", "Superman", "Iron Man"]); // data from backend
      }, 1000); // mocking network call
    });
  }

  async function printHeroesList() {
    const heroesList = await deferredHeroesList();
    console.log(heroesList); // printed after 1 sec
  }

  printHeroesList();

  console.log('end of main function'); // gets printed first
})();
```

