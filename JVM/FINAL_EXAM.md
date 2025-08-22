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

## OOP & Generics

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

## Threads & Coroutines

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

## Collections & Sequences

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

-----

## Serialization & DSLs

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

## Jetpack Compose & Backend

-----

### 42\. What is the issue here?

```kotlin
@Composable
fun ItemList(items: List<String>) {
    Column {
        items.forEach { item ->
            var selected by remember { mutableStateOf(false) }
            Row { /* ... */ }
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
