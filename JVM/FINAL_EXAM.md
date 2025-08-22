## Introduction to Kotlin

-----

### 1\. What will be printed?

```kotlin
fun main() {
    var pretzel = 5
    val doner = 10
    pretzel = readLine()?.toInt() ?: pretzel + doner
    println(pretzel)
}
// Input: "abc"
```

  - a) 5
  - b) 15
  - c) null
  - d) NumberFormatException

<!-- end list -->

  * **Your Answer:** (b) 15
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (d) NumberFormatException

> **Reasoning:** `readLine()?.toInt()` on the input "abc" throws a `NumberFormatException`. The elvis operator `?:` **does not catch exceptions**, so the program crashes.

-----

### 2\. What is the result?

```kotlin
fun main() {
    val prices = mutableMapOf("Pretzel" to 0.99)
    val otherPrices = prices
    otherPrices["Currywurst"] = 6.99
    println(prices.size)
}
```

  - a) 1
  - b) 2
  - c) 0
  - d) Compilation error

<!-- end list -->

  * **Your Answer:** (b) 2
  * **Grade:** ✅ Correct

> **Reasoning:** `otherPrices` is a reference to the same map object as `prices`. Adding an element to `otherPrices` also adds it to `prices`, making the final size **2**.

-----

### 3\. What will this print?

```kotlin
fun main() {
    val range = 10 downTo 1 step 2
    println(range.filter { it % 5 }.sum())
}
```

  - a) 30
  - b) 24
  - c) 40
  - d) 18

<!-- end list -->

  * **Your Answer:** (b) 24
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (a) 30

> **Reasoning:** ⚠️ **This question is flawed.** The code `filter { it % 5 }` causes a **compilation error** because `filter` requires a lambda that returns a `Boolean`, but `it % 5` returns an `Int`. Assuming the `filter` was a typo and should be ignored, the code would sum the range `10, 8, 6, 4, 2`, which equals **30**.

-----

### 4\. What happens here?

```kotlin
fun functionArguments(log: (String) -> Unit) {
    log("Starting")
    throw Exception("Error")
    log("Ending")
}

fun main() {
    var count = 0
    try {
        functionArguments { count++ }
    } catch (e: Exception) { }
    println(count)
}
```

  - a) 0
  - b) 1
  - c) 2
  - d) Exception propagates

<!-- end list -->

  * **Your Answer:** (b) 1
  * **Grade:** ✅ Correct

> **Reasoning:** The lambda `{ count++ }` is executed once for `log("Starting")`, incrementing `count` to 1. The `Exception` is then thrown and caught. The final value of `count` is **1**.

-----

## OOP Basics

-----

### 5\. What is the output?

```kotlin
class Probability(value: Double) {
    var value: Double = value
        set(value) {
            if (value in 0.0..1.0) field = value
        }
}

fun main() {
    val p = Probability(0.5)
    p.value = 2.0
    println(p.value)
}
```

  - a) 2.0
  - b) 0.5
  - c) 0.0
  - d) Exception

<!-- end list -->

  * **Your Answer:** (b) 0.5
  * **Grade:** ✅ Correct

> **Reasoning:** The custom setter only updates the value if it's within the `0.0..1.0` range. Since `2.0` is outside this range, the value remains at its initial **0.5**.

-----

### 6\. What will be printed?

```kotlin
data class Point(var x: Int, var y: Int)

fun main() {
    val p1 = Point(1, 2)
    val p2 = p1.copy()
    p2.x = 5
    println("${p1.x},${p2.x}")
}
```

  - a) 1, 1
  - b) 5, 5
  - c) 1, 5
  - d) 5, 1

<!-- end list -->

  * **Your Answer:** (c) 1, 5
  * **Grade:** ✅ Correct

> **Reasoning:** The `copy()` function creates a new `Point` object. Changing `p2` does not affect `p1`. The output is **1, 5**.

-----

### 7\. What is the result?

```kotlin
class Counter {
    private var count = 0
    operator fun plusAssign(value: Int) { count += value }
    operator fun invoke() = count
}

fun main() {
    val c = Counter()
    c += 5
    c += 3
    println(c())
}
```

  - a) 0
  - b) 5
  - c) 8
  - d) Compilation error

<!-- end list -->

  * **Your Answer:** (c) 8
  * **Grade:** ✅ Correct

> **Reasoning:** The `+=` operator calls `plusAssign`, so `count` becomes 8. The `c()` call uses the `invoke` operator, which returns the final `count` of **8**.

-----

### 8\. What happens here?

```kotlin
enum class Status { ACTIVE, INACTIVE }

fun main() {
    val status = Status.values().find { it.name.length > 6 }
    println(status ?: "NONE")
}
```

  - a) ACTIVE
  - b) INACTIVE
  - c) NONE
  - d) null

<!-- end list -->

  * **Your Answer:** (a) ACTIVE
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b) INACTIVE

> **Reasoning:** The code finds the first enum whose name has a length greater than 6. "ACTIVE" has length 6. "INACTIVE" has length 8. Thus, **INACTIVE** is found and printed.

-----

## OOP Part 2 & Generics

-----

### 9\. What will this print?

```kotlin
interface Logger {
    fun log(msg: String) = println("LOG: $msg")
}
class ConsoleLogger : Logger {
    override fun log(msg: String) = println("CONSOLE: $msg")
}
fun main() {
    val logger: Logger = ConsoleLogger()
    logger.log("Test")
}
```

  - a) LOG: Test
  - b) CONSOLE: Test
  - c) Test
  - d) Compilation error

<!-- end list -->

  * **Your Answer:** (b) CONSOLE: Test
  * **Grade:** ✅ Correct

> **Reasoning:** Due to polymorphism, the overridden `log` method in the `ConsoleLogger` class is executed, printing **CONSOLE: Test**.

-----

### 10\. What is the compilation result?

```kotlin
class Box<T: Number>(val value: T) {
    fun getValue(): T = value
}
fun main() {
    val box = Box("Hello")
    println(box.getValue())
}
```

  - a) Hello
  - b) Compilation error: String is not a Number
  - c) ClassCastException
  - d) null

<!-- end list -->

  * **Your Answer:** (c) ClassCastException
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b) Compilation error: String is not a Number

> **Reasoning:** The generic type `T` is constrained to `Number`. Trying to create a `Box` with a `String` violates this constraint, which is an error caught at **compile time**.

-----

### 11\. What will be printed?

```kotlin
fun <T> List<T>.secondOrNull(): T? = if (size >= 2) this[1] else null

fun main() {
    println(listOf("A").secondOrNull() ?: "Empty")
}
```

  - a) A
  - b) null
  - c) Empty
  - d) IndexOutOfBoundsException

<!-- end list -->

  * **Your Answer:** (c) Empty
  * **Grade:** ✅ Correct

> **Reasoning:** The list has only one element, so `secondOrNull()` returns `null`. The elvis operator `?:` then provides the default value, printing **Empty**.

-----

### 12\. What happens with this code?

```kotlin
interface Producer<out T> {
    fun produce(): T
}
fun useProducer(p: Producer<Number>) {
    val num: Number = p.produce()
}
fun main() {
    val intProducer: Producer<Int> = object : Producer<Int> {
        override fun produce(): Int = 42
    }
    useProducer(intProducer)
}
```

  - a) Compilation error
  - b) Works fine
  - c) ClassCastException
  - d) 42 is printed

<!-- end list -->

  * **Your Answer:** (b) Works fine
  * **Grade:** ✅ Correct

> **Reasoning:** The type `T` is covariant (`out`), meaning a `Producer<Int>` can be used where a `Producer<Number>` is expected. The code compiles and runs correctly.

-----

## Variances & Collections

-----

### 13\. What will this output?

```kotlin
fun main() {
    val set = mutableSetOf(1, 2, 3)
    val result = set.apply {
        add(1)
        remove(2)
    }
    .filter { it % 2 == 0 }
    println(result)
}
```

  - a) [2, 4]
  - b) [4]
  - c) [2]
  - d) []

<!-- end list -->

  * **Your Answer:** (b) [4]
  * **Grade:** ✅ Correct

> **Reasoning:** ⚠️ **Based on the grading, the question likely had a typo.** As written, the code produces `[]`. To get the graded answer of `[4]`, the line `add(1)` would have to be `add(4)`. In that case, the set becomes `{1, 3, 4}`, and filtering for even numbers results in `[4]`.

-----

### 14\. What is the result?

```kotlin
class Container<in T> {
    private val items = mutableListOf<Any?>()
    fun add(item: T) { items.add(item) }
    fun getSize() = items.size
}

fun main() {
    val container: Container<String> = Container<Any>()
    container.add("Hello")
    println(container.getSize())
}
```

  - a) Compilation error
  - b) 0
  - c) 1
  - d) ClassCastException

<!-- end list -->

  * **Your Answer:** (c) 1
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (a) Compilation error

> **Reasoning:** ⚠️ **This grading appears incorrect.** Due to contravariance (`in`), a `Container<Any>` (a consumer of a supertype) can be legally assigned to a `Container<String>` variable (a consumer of a subtype). The code should compile and print **1**.

-----

### 15\. What happens here?

```kotlin
fun main() {
    val map = sortedMapOf(2 to "B", 1 to "A", 3 to "C")
    println(map.keys.first())
}
```

  - a) 2
  - b) 1
  - c) 3
  - d) Random order

<!-- end list -->

  * **Your Answer:** (b) 1
  * **Grade:** ✅ Correct

> **Reasoning:** `sortedMapOf` keeps its keys sorted. The first key in the natural order of `1, 2, 3` is **1**.

-----

### 16\. What will be printed?

```kotlin
fun main() {
    val queue = ArrayDeque<String>()
    queue.addFirst("A")
    queue.addLast("B")
    queue.addFirst("C")
    println(queue.removeFirst())
}
```

  - a) A
  - b) B
  - c) C
  - d) null

<!-- end list -->

  * **Your Answer:** (c) C
  * **Grade:** ✅ Correct

> **Reasoning:** The deque's state becomes `[C, A, B]`. `removeFirst()` removes and returns the first element, which is **C**.

-----

## Reflections & Threads

---

### 17\. What will this code output?

```kotlin
fun main() {
    val klass = String::class.java
    val methods = klass.methods.count { it.name.startsWith("to") }
    println(methods > 5)
}
```

  - a) true
  - b) false
  - c) Compilation error
  - d) SecurityException

<!-- end list -->

  * **Your Answer:** (a) true
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b) false

> **Reasoning:** ⚠️ **This grading is likely incorrect.** In modern JDKs, the `String` class has at least 6 public methods starting with "to" (e.g., `toString`, `toCharArray`, `toLowerCase`, `toUpperCase`, plus locale-specific variants), which would make the result `true`.

-----

### 18\. What happens in this code?

```kotlin
fun main() {
    var counter = 0
    val thread = thread(isDaemon = true) {
        while (true) {
            counter++
        }
    }
    Thread.sleep(10)
    println(counter > 0)
}
```

  - a) The code always prints true
  - b) The code always prints false
  - c) Depends on timing
  - d) Program doesn't terminate

<!-- end list -->

  * **Your Answer:** (c) Depends on timing
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (d) Program doesn't terminate

> **Reasoning:** ⚠️ **This grading is incorrect.** A program with only daemon threads running **will terminate**. The correct answer is **(c) Depends on timing**, as it's not guaranteed the daemon thread will get a time slice to increment the counter in 10ms.

-----

### 19\. What is the issue here?

```kotlin
data class Person(val name: String)

fun main() {
    val personClass = Person::class.java
    val instance = personClass.newInstance()
    println(instance.name)
}
```

  - a) Works fine
  - b) No default constructor
  - c) name is private
  - d) Reflection not allowed

<!-- end list -->

  * **Your Answer:** (a) Works fine
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b) No default constructor

> **Reasoning:** `personClass.newInstance()` tries to call a no-argument constructor. Since `Person` only has a constructor that requires a `String`, this fails.

-----

### 20\. What will be printed?

```kotlin
fun main() {
    val thread = thread {
        Thread.sleep(100)
        println("Done")
    }
    thread.join(50)
    println(thread.state)
}
```

  - a) NEW
  - b) TERMINATED
  - c) TIMED\_WAITING
  - d) RUNNABLE

<!-- end list -->

  * **Your Answer:** (b) TERMINATED
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (c) TIMED\_WAITING

> **Reasoning:** The main thread waits for only 50ms. At that point, the other thread is still sleeping for its 100ms duration. A sleeping thread is in the **TIMED\_WAITING** state.

-----

## Threads

---

### 21\. What is the final value?

```kotlin
import java.util.concurrent.atomic.AtomicInteger
import kotlin.concurrent.thread

fun main() {
    val counter = AtomicInteger(0)
    val threads = List(10) {
        thread {
            repeat(100) {
                counter.incrementAndGet()
            }
        }
    }
    threads.forEach { it.join() }
    println(counter.get())
}
```

  - a) Less than 1000
  - b) Exactly 1000
  - c) More than 1000
  - d) Unpredictable

<!-- end list -->

  * **Your Answer:** (b) Exactly 1000
  * **Grade:** ✅ Correct

> **Reasoning:** `AtomicInteger` is thread-safe. Ten threads each increment it 100 times, resulting in a predictable final value of **1000**.

-----

### 22\. What happens here?

```kotlin
import java.util.concurrent.locks.ReentrantLock

fun main() {
    val lock = ReentrantLock()
    lock.lock()
    val acquired = lock.tryLock()
    println(acquired)
    lock.unlock()
}
```

  - a) true
  - b) false
  - c) Deadlock
  - d) IllegalMonitorStateException

<!-- end list -->

  * **Your Answer:** (b) false
  * **Grade:** ✅ Correct

> **Reasoning:** ⚠️ **This grading is incorrect.** A `ReentrantLock` allows the same thread to acquire a lock it already holds. The `tryLock()` call would succeed, and the code would print **true**.

-----

### 23\. What will be printed?

```kotlin
import java.util.Collections

fun main() {
    val list = Collections.synchronizedList(mutableListOf<Int>())
    list.addAll(listOf(1, 2, 3))
    var sum = 0
    list.forEach { sum += it } // Is this thread-safe?
    println(sum)
}
```

  - a) Always 6
  - b) Sometimes less than 6
  - c) ConcurrentModificationException possible
  - d) Compilation error

<!-- end list -->

  * **Your Answer:** (a) Always 6
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (c) ConcurrentModificationException possible

> **Reasoning:** Iteration is not a single atomic operation. If another thread modified the list while the `forEach` loop was running, a **ConcurrentModificationException** would be thrown. Iteration over synchronized collections must be done within a manual `synchronized` block.

-----

### 24\. What is the result?

```kotlin
import kotlin.concurrent.thread

fun main() {
    val value = ThreadLocal<Int>()
    value.set(42)
    thread {
        value.get() // This thread gets null
    }.join()
    println(value.get()) // Main thread still has its value
}
```

  - a) 42
  - b) 0
  - c) null
  - d) ThreadLocal not accessible

<!-- end list -->

  * **Your Answer:** (a) 42
  * **Grade:** ✅ Correct

> **Reasoning:** `ThreadLocal` variables are isolated per-thread. The `set(42)` and `get()` calls in `main` happen in the same thread, so the value is preserved. The other thread's access doesn't affect the main thread's value.

-----

## Executors & Coroutines

-----

### 25\. What will happen?

```kotlin
import java.util.concurrent.Executors

fun main() {
    val executor = Executors.newFixedThreadPool(2)
    val future1 = executor.submit { Thread.sleep(100); 1 }
    val future2 = executor.submit { Thread.sleep(100); 2 }
    println(future1.get() + future2.get())
    executor.shutdown()
}
```

  - a) 3
  - b) Blocks forever
  - c) TimeoutException
  - d) RejectedExecutionException

<!-- end list -->

  * **Your Answer:** (a) 3
  * **Grade:** ✅ Correct

> **Reasoning:** The two tasks run concurrently. `future1.get()` waits \~100ms. By then, `future2` is also done. The results `1` and `2` are added, printing **3**.

-----

### 26\. What is printed?

```kotlin
import kotlinx.coroutines.*

suspend fun compute(): Int {
    delay(100)
    return 42
}

fun main() = runBlocking {
    val start = System.currentTimeMillis()
    val a = async { compute() }
    val b = async { compute() }
    println(a.await() + b.await())
    println(System.currentTimeMillis() - start > 150)
}
```

  - a) 84 true
  - b) 84 false
  - c) 42 true
  - d) Exception

<!-- end list -->

  * **Your Answer:** (a) 84 true
  * **Grade:** ✅ Correct

> **Reasoning:** ⚠️ **This grading is incorrect.** The two `async` calls run concurrently. The total time will be \~100ms, not \> 150ms. The first line will print **84**, and the second will print **false**. The correct answer should be (b).

-----

### 27\. What happens here?

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val job = launch {
        repeat(1000) { i ->
            println(i)
            delay(100)
        }
    }
    delay(250)
    job.cancel()
    println("Cancelled")
}
```

  - a) Prints 0, 1, 2, then Cancelled
  - b) Prints 0, 1, then Cancelled
  - c) Prints all 1000 numbers
  - d) CancellationException

<!-- end list -->

  * **Your Answer:** (b) Prints 0, 1, then Cancelled
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (a) Prints 0, 1, 2, then Cancelled

> **Reasoning:** The loop prints 0, delays 100ms, prints 1, delays 100ms, and prints 2. At 250ms, the main coroutine cancels the job. The output is **0, 1, 2, then Cancelled**.

-----

## Sequences & Collection Operations

-----

### 28\. What is the issue?

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val result = withTimeout(100) {
        delay(200)
        "Success"
    }
    println(result)
}
```

  - a) Prints Success
  - b) TimeoutCancellationException
  - c) Returns null
  - d) Blocks forever

<!-- end list -->

  * **Your Answer:** (b) TimeoutCancellationException
  * **Grade:** ✅ Correct

> **Reasoning:** The `delay(200)` takes longer than the 100ms timeout, causing a **TimeoutCancellationException** to be thrown.

-----

### 29\. How many times is "Transform" printed?

```kotlin
fun main() {
    val result = (1..1000000).asSequence()
        .map { println("Transform"); it * 2 }
        .filter { it > 10 }
        .take(2)
        .toList()
}
```

  - a) 2
  - b) 5
  - c) 6
  - d) 1000000

<!-- end list -->

  * **Your Answer:** (c) 6
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** None of the above.

> **Reasoning:** ⚠️ **This question is flawed.** The sequence processes elements until two items pass the filter (`6*2=12` and `7*2=14`). To find these, the `map` operation is called on numbers 1 through 7. "Transform" is printed **7 times**, which is not an option.

-----

### 30\. What will be printed?

```kotlin
fun main() {
    val numbers = listOf(1, 2, 3, 4, 5)
    val result = numbers
        .windowed(3, 2)
        .map { it.sum() }
    println(result)
}
```

  - a) [6, 9, 12]
  - b) [6, 9]
  - c) [6, 12]
  - d) [1, 2, 3]

<!-- end list -->

  * **Your Answer:** (b) [6, 9]
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (c) [6, 12]

> **Reasoning:** `windowed(3, 2)` creates windows of size 3, stepping by 2. The windows are `[1, 2, 3]` (sum=6) and `[3, 4, 5]` (sum=12). The result is **[6, 12]**.

-----

### 31\. What is the output?

```kotlin
fun main() {
    val map = listOf("a" to 1, "b" to 2, "a" to 3)
        .groupBy({ it.first }, { it.second })
    println(map["a"])
}
```

  - a) 1
  - b) 3
  - c) [1, 3]
  - d) null

<!-- end list -->

  * **Your Answer:** (a) 1
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (c) [1, 3]

> **Reasoning:** `groupBy` creates a map where each key corresponds to a list of values. Both pairs with the key `"a"` have their values (`1` and `3`) collected into a list.

-----

### 32\. What happens here?

```kotlin
fun main() {
    val result = generateSequence(1) { if (it < 5) it + 1 else null }
        .fold(0) { acc, value -> acc + value }
    println(result)
}
```

  - a) 10
  - b) 15
  - c) 21
  - d) Infinite loop

<!-- end list -->

  * **Your Answer:** (b) 15
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (c) 21

> **Reasoning:** ⚠️ **The grading implies a typo.** The code as written generates the sequence `1, 2, 3, 4, 5`, and the sum is **15**. The graded answer of **21** would be correct if the condition were `it <= 5`, which would generate `1, 2, 3, 4, 5, 6`.

---

## DSL

-----

### 33\. What will this DSL code produce?

```kotlin
class Tag(val name: String) {
    val children = mutableListOf<Tag>()
    operator fun String.unaryPlus() {
        children.add(Tag(this))
    }
}
fun tag(name: String, init: Tag.() -> Unit): Tag {
    return Tag(name).apply(init)
}
fun main() {
    val result = tag("div") {
        +"Hello"
        +"World"
    }
    println(result.children.size)
}
```

  - a) 0
  - b) 1
  - c) 2
  - d) Compilation error

<!-- end list -->

  * **Your Answer:** (c) 2
  * **Grade:** ✅ Correct

> **Reasoning:** The `+` operator is overloaded to add a new `Tag` to the `children` list. It is called twice, so the size of the list is **2**.

-----

### 34\. What is the issue with this DSL?

```kotlin
fun table(init: Table.() -> Unit): Table = Table().apply(init)
class Table {
    fun row(init: Row.() -> Unit) = Row().apply(init)
}
class Row {
    fun cell(value: String) {}
}
fun main() {
    table {
        row {
            cell("A")
            row { } // Is this valid?
        }
    }
}
```

  - a) Works fine
  - b) row not accessible in Row scope
  - c) Should be prevented with @DslMarker
  - d) NullPointerException

<!-- end list -->

  * **Your Answer:** (c) Should be prevented with @DslMarker
  * **Grade:** ✅ Correct

> **Reasoning:** While the code compiles, it allows for nonsensical structures (a row inside a row). This is a common DSL design flaw that the **@DslMarker** annotation is specifically designed to prevent.

-----

### 35\. What will be printed?

```kotlin
inline fun <reified T> createList(size: Int): List<T> {
    return when (T::class) {
        Int::class -> List(size) { it } as List<T>
        String::class -> List(size) { "Item$it" } as List<T>
        else -> emptyList()
    }
}

fun main() {
    println(createList<String>(3))
}
```

  - a) [0, 1, 2]
  - b) [Item0, Item1, Item2]
  - c) []
  - d) ClassCastException

<!-- end list -->

  * **Your Answer:** (b) [Item0, Item1, Item2]
  * **Grade:** ✅ Correct

> **Reasoning:** Because `T` is `reified`, `T::class` works at runtime. The `String::class` branch is matched, generating a list of three strings: **[Item0, Item1, Item2]**.

-----

### 36\. What happens with this builder?

```kotlin
class JsonBuilder {
    private val map = mutableMapOf<String, Any>()
    infix fun String.to(value: Any) {
        map[this] = value
    }
    override fun toString() = map.toString()
}

fun json(init: JsonBuilder.() -> Unit) = JsonBuilder().apply(init)

fun main() {
    val result = json {
        "name" to "John"
        "age" to 30
    }
    println(result)
}
```

  - a) Compilation error
  - b) {name=John, age=30}
  - c) Empty map
  - d) "to" is not recognized

<!-- end list -->

  * **Your Answer:** (b) {name=John, age=30}
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (a) Compilation error

> **Reasoning:** ⚠️ **This grading appears incorrect.** The DSL builder pattern using a member extension infix function is valid. The code should compile and print `{name=John, age=30}`.

-----

## kotlin.serialization

-----

### 37\. What is the serialization result?

```kotlin
import kotlinx.serialization.*
import kotlinx.serialization.json.*

@Serializable
data class User(
    val name: String,
    @SerialName("user_age") val age: Int,
    val email: String? = null
)

fun main() {
    val json = Json { encodeDefaults = false }
    val user = User("John", 25)
    println(json.encodeToString(user))
}
```

  - a) `{"name":"John","user_age":25,"email":null}`
  - b) `{"name":"John","user_age":25}`
  - c) `{"name":"John","age":25}`
  - d) Compilation error

<!-- end list -->

  * **Your Answer:** (b) `{"name":"John","user_age":25}`
  * **Grade:** ✅ Correct

> **Reasoning:** `encodeDefaults = false` omits the `email` field because it has its default value (`null`). `@SerialName` renames the `age` key to `user_age`.

-----

### 38\. What happens here?

```kotlin
import kotlinx.serialization.*
import kotlinx.serialization.json.*

@Serializable
data class Response(val code: Int, val message: String)

fun main() {
    val json = Json { ignoreUnknownKeys = true }
    val input = """{"code":200,"message":"OK","extra":"field"}"""
    val response = json.decodeFromString<Response>(input)
    println(response.code)
}
```

  - a) 200
  - b) Exception due to extra field
  - c) null
  - d) 0

<!-- end list -->

  * **Your Answer:** (a) 200
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b) Exception due to extra field

> **Reasoning:** ⚠️ **This grading appears incorrect.** The code explicitly sets `ignoreUnknownKeys = true`, which should cause the `"extra"` field to be ignored, allowing successful deserialization and printing **200**.

-----

### 39\. What will be printed?

```kotlin
import kotlinx.datetime.*

fun main() {
    val instant = Instant.fromEpochSeconds(0)
    val zone = TimeZone.of("UTC")
    val dateTime = instant.toLocalDateTime(zone)
    println(dateTime.year)
}
```

  - a) 1970
  - b) 0
  - c) 2024
  - d) null

<!-- end list -->

  * **Your Answer:** (a) 1970
  * **Grade:** ✅ Correct

> **Reasoning:** The Unix epoch (0 seconds) started on January 1, **1970**.

-----

### 40\. What will be the JSON output?

```kotlin
// Assume full setup for polymorphic serialization
@Serializable
sealed class Event(val timestamp: Long)
// ... subclasses ClickEvent and ScrollEvent
// val events: List<Event> = ...
// println(json.encodeToString(events))
```

  - a) `[{"timestamp":1000,...}]`
  - b) `[{"type":"click","timestamp":1000,...}]`
  - c) Compilation error
  - d) SerializationException

<!-- end list -->

  * **Your Answer:** (a)
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b)

> **Reasoning:** When serializing polymorphic types, `kotlinx.serialization` adds a **type discriminator** (by default `"type"`) to each object to identify its specific subclass.

-----

## Mobile & Compose

-----

### 41\. What will this Composable display?

```kotlin
@Composable
fun Counter() {
    val count by remember { mutableStateOf(0) }
    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}
```

  - a) Static text "Count: 0"
  - b) Clickable counter that updates
  - c) Compilation error
  - d) count resets on recomposition

<!-- end list -->

  * **Your Answer:** (b) Clickable counter that updates
  * **Grade:** ✅ Correct

> **Reasoning:** This is the basic pattern for state in Compose. `remember { mutableStateOf(...) }` holds the state, and updating it triggers recomposition, which redraws the UI with the new value.

-----

### 42\. What is the issue here?

```kotlin
@Composable
fun ItemList(items: List<String>) {
    Column {
        items.forEach { item ->
            var selected by remember { mutableStateOf(false) }
            Row {
                Checkbox(checked = selected, onCheckedChange = { selected = it })
                Text(item)
            }
        }
    }
}
```

  - a) Works correctly
  - b) All checkboxes share same state
  - d) `remember` called in wrong scope

<!-- end list -->

  * **Your Answer:** (a) Works correctly
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (d) `remember` called in wrong scope

> **Reasoning:** Calling `remember` inside a loop (`forEach`) is against the Rules of Compose and leads to unstable behavior.

-----

### 43\. What happens with navigation?

```kotlin
@Composable
fun MyApp() {
    val navController = rememberNavController()
    NavHost(navController = navController, startDestination = "home") {
        composable("home") {
            Button(onClick = { navController.navigate("details") }) {
                Text("Go to Details")
            }
        }
        composable("details/{id}") {
            Text("Details")
        }
    }
}
```

  - a) Navigation works
  - b) Missing argument for details route
  - c) navController not found
  - d) Circular navigation

<!-- end list -->

  * **Your Answer:** (b) Missing argument for details route
  * **Grade:** ✅ Correct

> **Reasoning:** The route `"details/{id}"` requires an `id` argument. The call `Maps("details")` does not provide one, causing a runtime error.

-----

### 44\. What will happen?

```kotlin
@Composable
fun AsyncData() {
    var data by remember { mutableStateOf<String?>(null) }

    LaunchedEffect(Unit) {
        delay(1000)
        data = "Loaded"
    }

    Text(data ?: "Loading...")
}
```

  - a) Shows "Loading..." forever
  - b) Shows "Loading..." then "Loaded" after 1 second
  - c) Compilation error
  - d) LaunchedEffect not allowed

<!-- end list -->

  * **Your Answer:** (a) Shows "Loading..." forever
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b) Shows "Loading..." then "Loaded" after 1 second

> **Reasoning:** `LaunchedEffect(Unit)` runs a coroutine once. It waits for 1 second, then updates the state variable `data`. This state change triggers a recomposition to show the "Loaded" text.

---

## Backend Development

-----

### 45\. What response code is returned?

```kotlin
// Ktor route
get("/users/{id}") {
    val id = call.parameters["id"]?.toIntOrNull()
    when {
        id == null -> call.respond(HttpStatusCode.BadRequest)
        id < 0 -> call.respond(HttpStatusCode.NotFound)
        else -> call.respond("User $id")
    }
}
// For a request: GET /users/abc
```

  - a) 200 HttpStatusCode.OK
  - b) 400 HttpStatusCode.BadRequest
  - c) 404 HttpStatusCode.NotFound
  - d) 500 HttpStatusCode.InternalServerError

<!-- end list -->

  * **Your Answer:** (b) 400 HttpStatusCode.BadRequest
  * **Grade:** ✅ Correct

> **Reasoning:** The path parameter "abc" cannot be converted to an integer, so `toIntOrNull()` returns `null`. The first branch of the `when` block is executed, returning a **400 Bad Request**.

-----

### 46\. What is sent to client?

```kotlin
@Serializable
data class ApiResponse(
    val status: String,
    @Transient val internalCode: Int = 0
)

// Ktor module...
call.respond(ApiResponse("OK", 200))
```

  - a) `{"status":"OK","internalCode":200}`
  - b) `{"status":"OK"}`
  - c) `{"status":"OK","internalCode":0}`
  - d) Error: missing serializer

<!-- end list -->

  * **Your Answer:** (a)
  * **Grade:** ❌ Incorrect
  * **Correct Answer:** (b)

> **Reasoning:** The **@Transient** annotation tells the serializer to completely ignore the `internalCode` field. It will be omitted from the JSON output.

-----

### 47\. What happens with this test?

```kotlin
@Test
fun testEndpoint() = testApplication {
    application {
        routing {
            get("/ping") {
                call.respondText("pong")
            }
        }
    }

    val response = client.get("/ping")
    assertEquals(HttpStatusCode.OK, response.status)
    assertEquals("pong", response.bodyAsText())
}
```

  - a) Test passes
  - b) client not initialized
  - c) bodyAsText() doesn't exist
  - d) routing not found

<!-- end list -->

  * **Your Answer:** (a) Test passes
  * **Grade:** ✅ Correct

> **Reasoning:** This is a standard and correct Ktor integration test. The application is configured, the client makes a valid request, and the assertions correctly verify the expected response.

-----

### 48\. What is the concurrency issue?

```kotlin
fun Application.module() {
    var counter = 0
    routing {
        get("/increment") {
            counter++
            call.respond(counter)
        }
    }
}
```

  - a) No issue
  - b) Race condition with concurrent requests
  - c) counter not accessible
  - d) respond takes String only

<!-- end list -->

  * **Your Answer:** (b) Race condition with concurrent requests
  * **Grade:** ✅ Correct

> **Reasoning:** The `counter` is shared, mutable state. Multiple concurrent requests can access it non-atomically, leading to lost updates. This is a classic **race condition**.
