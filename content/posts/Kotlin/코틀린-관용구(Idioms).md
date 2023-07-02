---
title: 코틀린 관용구(Idioms)
date: 2023-07-01
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 관용구 공식 레퍼런스 번역
---
## 코틀린 공식 레퍼런스 번역
번역 오류가 있을 수 있습니다.

---

Kotlin에서 자주 사용되는 숙어 모음입니다. 마음에 드는 관용구가 있으면 풀 리퀘스트를 보내 기여하세요.

# DTO(POJO/POCO)만들기(Create DTOs)
---
```Kotlin
data class Customer(val name: String, val email: String)
```
다음과 같은 기능을 갖춘 ```Customer``` 클래스를 제공합니다:
* 모든 프로퍼티에 대한 getter(및 ```var```의 경우 setters)
* `equals()`
* `hashCode()`
* `toString()`
* `copy()`
* `component1()`,`component2()`,..., 모든 프로퍼티들(데이터 클래스 참조)

# 함수 매개변수의 기본값(Default values for function parameters)
---
```kotlin
fun foo(a: Int = 0, b: String = "") { ... }
```

# 목록 필터링(Filter a list)

---

```kotlin
val positives = list.filter { x -> x > 0 }
```

또는 그보다 더 짧을 수도 있습니다:

```kotlin
val positives = list.filter { it > 0 }
```

Java 필터링과 Kotlin 필터링의 차이점을 알아보세요.

# 컬렉션에 요소가 있는지 확인(Check the presence of an element in a collection)

---

```kotlin
if ("john@example.com" in emailsList) { ... }

if ("jane@example.com" !in emailsList) { ... }
```

# 문자열 보간(String interpolation)

---

```kotlin
println("Name $name")
```

Java와 Kotlin 문자열 연결의 차이점을 알아보세요.

# 인스턴스 확인(Instance check)

---

```kotlin
when (x) {
    is Foo -> ...
    is Bar -> ...
    else   -> ...
}
```

# 읽기 전용 목록(Read-only list)

---

```kotlin
val list = listOf("a", "b", "c")
```

# 읽기 전용 맵(Read-only map)

---

```kotlin
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

# 맵 항목에 액세스(Access a map entry)

---

```kotlin
println(map["key"])
map["key"] = value
```

# 맵 또는 페어 목록 탐색하기(Traverse a map or a list of pairs)

---

```kotlin
for ((k, v) in map) {
    println("$k -> $v")
}
```

`k`와 `v`는 `name`, `age` 등 편리한 이름으로 사용할 수 있습니다.

# 범위 반복(Iterate over a range)

---

```kotlin
for (i in 1..100) { ... }  // closed range: includes 100
for (i in 1 until 100) { ... } // half-open range: does not include 100
for (x in 2..10 step 2) { ... }
for (x in 10 downTo 1) { ... }
(1..10).forEach { ... }
```

# Lazy 속성(Lazy property)

---

```kotlin
val p: String by lazy { // the value is computed only on first access
    // compute the string
}
```

# 확장 기능(Extension functions)

---

```kotlin
fun String.spaceToCamelCase() { ... }

"Convert this to camelcase".spaceToCamelCase()
```

# 싱글톤 생성(Create a singleton)

---

```kotlin
object Resource {
    val name = "Name"
}
```

# 추상 클래스 인스턴스화(Instantiate an abstract class)

---

```kotlin
abstract class MyAbstractClass {
    abstract fun doSomething()
    abstract fun sleep()
}

fun main() {
    val myObject = object : MyAbstractClass() {
        override fun doSomething() {
            // ...
        }

        override fun sleep() { // ...
        }
    }
    myObject.doSomething()
}
```

# If-not-null 단축키(shorthand)

---

```kotlin
val files = File("Test").listFiles()

println(files?.size) // size is printed if files is not null
```

# If-not-null-else 단축키(shorthand)

---

```kotlin
val files = File("Test").listFiles()

println(files?.size ?: "empty") // if files is null, this prints "empty"

// To calculate the fallback value in a code block, use `run`
val filesSize = files?.size ?: run {
    return someSize
}
println(filesSize)
```

# null이면 문 실행(Execute a statement if null)

---

```kotlin
val values = ...
val email = values["email"] ?: throw IllegalStateException("Email is missing!")
```

# 비어 있을 수 있는 컬렉션의 첫 번째 항목 가져오기(Get first item of a possibly empty collection)

---

```kotlin
val emails = ... // might be empty
val mainEmail = emails.firstOrNull() ?: ""
```

Java와 Kotlin의 첫 번째 항목 가져오기의 차이점을 알아보세요.

# null이 아닌 경우 실행(Execute if not null)

---

```kotlin
val value = ...

value?.let {
    ... // execute this block if not null
}
```

# 널이 아닌 경우 Map이 가능한 값(Map nullable value if not null)

---

```kotlin
val value = ...

val mapped = value?.let { transformValue(it) } ?: defaultValue
// defaultValue is returned if the value or the transform result is null.
```

# 반환 시 반환 문(Return on when statement)

---

```kotlin
fun transform(color: String): Int {
    return when (color) {
        "Red" -> 0
        "Green" -> 1
        "Blue" -> 2
        else -> throw IllegalArgumentException("Invalid color param value")
    }
}
```

# try-catch 표현식(try-catch expression)

---

```kotlin
fun test() {
    val result = try {
        count()
    } catch (e: ArithmeticException) {
        throw IllegalStateException(e)
    }

    // Working with result
}
```

# if 표현식(if expression)

---

```kotlin
val y = if (x == 1) {
    "one"
} else if (x == 2) {
    "two"
} else {
    "other"
}
```

# 단위(Unit)를 반환하는 메서드의 빌더 스타일 사용법(Builder-style usage of methods that return Unit)

---

```kotlin
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}
```

# 단일 표현식 함수(Single-expression function)

---

```kotlin
fun theAnswer() = 42
```

이는 다음과 같습니다.

```kotlin
fun theAnswer(): Int {
    return 42
}
```

다른 숙어와 효과적으로 결합하여 코드를 더 짧게 만들 수 있습니다. 예를 들어, `when` 표현식을 사용할 수 있습니다:

```kotlin
fun transform(color: String): Int = when (color) {
    "Red" -> 0
    "Green" -> 1
    "Blue" -> 2
    else -> throw IllegalArgumentException("Invalid color param value")
}
```

# 객체 인스턴스에서 여러 메서드 호출(와 함께)(Call multiple methods on an object instance(with))

---

```kotlin
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

val myTurtle = Turtle()
with(myTurtle) { //draw a 100 pix square
    penDown()
    for (i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
```

# 개체 속성 구성(적용)(Configure properties of an object(apply))

---

```kotlin
val myRectangle = Rectangle().apply {
    length = 4
    breadth = 5
    color = 0xFAFAFA
}
```

이 기능은 객체 생성자에 없는 프로퍼티를 구성할 때 유용합니다.

# Java 7의 리소스 사용 시도

---

```kotlin
val stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.buffered().reader().use { reader ->
    println(reader.readText())
}
```

# 제네릭 타입 정보가 필요한 제네릭 함수(Generic function that requires the generic type information)

---

```kotlin
//  public final class Gson {
//     ...
//     public <T> T fromJson(JsonElement json, Class<T> classOfT) throws JsonSyntaxException {
//     ...

inline fun <reified T: Any> Gson.fromJson(json: JsonElement): T = this.fromJson(json, T::class.java)
```

# 무효화 가능 부울(Nullable Boolean)

---

```kotlin
val b: Boolean? = ...
if (b == true) {
    ...
} else {
    // `b` is false or null
}
```

# 두 변수 바꾸기(Swap two varibles)

---

```kotlin
var a = 1
var b = 2
a = b.also { b = a }
```

# 코드를 완료되지 않은 것으로 표시(TODO)(Mark code as incomplete)

---

Kotlin의 표준 라이브러리에는 항상 `NotImplementedError`를 throw하는 `TODO()` 함수가 있습니다. 반환 유형은 `Nothing`이므로 예상 유형에 관계없이 사용할 수 있습니다. 이유 매개 변수를 허용하는 오버로드도 있습니다:

```kotlin
fun calcTaxes(): BigDecimal = TODO("Waiting for feedback from accounting")
```

IntelliJ IDEA의 kotlin 플러그인은 `TODO()`의 시맨틱을 이해하고 TODO 도구 창에 코드 포인터를 자동으로 추가합니다.

# Ref
---
[Kotlin Idioms](https://kotlinlang.org/docs/idioms.html)