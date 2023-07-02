---
title: 코틀린(kotlin) 기본 구문(Basic syntax)
date: 2023-07-01
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 기본 구문 공식 레퍼런스 번역
---
## 코틀린 공식 레퍼런스 번역
번역 오류가 있을 수 있습니다.

---
예제가 포함된 기본 구문 요소 모음입니다. 모든 섹션의 끝에는 관련 주제에 대한 자세한 설명으로 연결되는 링크가 있습니다.

또한 JetBrains Academy의 무료 Kotlin Core 트랙을 통해 모든 Kotlin 필수 요소를 학습할 수도 있습니다.
# 패키지 정의 및 가져오기(Package definition and imports)
---
패키지 사양은 소스 파일의 맨 위에 있어야 합니다.
```Kotlin
package my.demo

import kotlin.text.*

// ...
```
디렉터리와 패키지를 일치시킬 필요는 없습니다. 소스 파일은 파일 시스템에 임의로 배치할 수 있습니다.
# 프로그램 시작 지점(Program entry point)
---
Kotlin 애플리케이션의 엔트리 포인트는 주요 기능입니다.
```Kotlin
fun main() {
	println("Hello world!")
}
```
또 다른 형태의 ```main```은 다양한 수의 ```String```인수를 허용합니다.
```Kotlin
fun main(args: Array<String>) {
	println(args.contentToString())
}
```
# 표준 출력으로 Print(Print to the standard output)
---
```print```는 인수를 표준 출력으로 나타낸다.
```Kotlin
fun main() {
	print("Hello ")
	print("world!")
}
```
```println```은 인수를 인쇄하고 줄 바꿈을 추가하여 다음에 인쇄할 내용이 다음 줄에 나타나도록 합니다.
```Kotlin
fun main() {
	println("Hello world!")
	println(42)
}
```
# 함수(functions)
---
두 개의 ```Int``` 매개변수와 ```Int``` 반환 유형을 가진 함수입니다.
```Kotlin
fun sum(a: Int, b: Int): Int {
	return a + b
}
fun main() {
	print("sum of 3 and 5 is ")
	println(sum(3, 5))
}
```
함수 본문은 표현식이 될 수 있습니다. 반환 유형은 추론됩니다.
```Kotlin
fun sum(a: Int, b: Int) = a + b

fun main() {
    println("sum of 19 and 23 is ${sum(19, 23)}")
}
```
의미 있는 값을 반환하지 않는 함수입니다.
```Kotlin
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}

fun main() {
    printSum(-1, 8)
}
```
```Unit``` 반환 유형은 생략할 수 있습니다.
```Kotlin
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}

fun main() {
    printSum(-1, 8)
}
```
함수를 참조하세요.
# 변수(Variables)
---
읽기 전용 로컬 변수는 ```val``` 키워드를 사용하여 정의됩니다. 값은 한 번만 할당할 수 있습니다.
```Kotlin
fun main() {
    val a: Int = 1  // immediate assignment
    val b = 2   // `Int` type is inferred
    val c: Int  // Type required when no initializer is provided
    c = 3       // deferred assignment
    println("a = $a, b = $b, c = $c")
}
```
재할당할 수 있는 변수는 ```var``` 키워드를 사용합니다.
```Kotlin
fun main() {
    var x = 5 // `Int` type is inferred
    x += 1
    println("x = $x")
}
```
최상위 수준에서 변수를 선언할 수 있습니다.
```Kotlin
val PI = 3.14
var x = 0

fun incrementX() { 
    x += 1 
}

fun main() {
    println("x = $x; PI = $PI")
    incrementX()
    println("incrementX()")
    println("x = $x; PI = $PI")
}
```
속성도 참조하세요.
# 클래스 및 인스턴스 만들기(Creating classes and instances)
---
클래스를 정의하려면 ```class``` 키워드를 사용합니다.
```Kotlin
class Shape
```
클래스의 프로퍼티는 선언 또는 본문에 나열할 수 있습니다.
```Kotlin
class Rectangle(var height: Double, var length: Double) {
    var perimeter = (height + length) * 2
}
```
클래스 선언에 나열된 매개변수가 있는 기본 생성자는 자동으로 사용할 수 있습니다.
```Kotlin
class Rectangle(var height: Double, var length: Double) {
    var perimeter = (height + length) * 2 
}
fun main() {
    val rectangle = Rectangle(5.0, 2.0)
    println("The perimeter is ${rectangle.perimeter}")
}
```
클래스 간의 상속은 콜론(```:```)으로 선언됩니다. 클래스는 기본적으로 최종 클래스이며, 클래스를 상속 가능하게 만들려면 클래스를 ```open``` 으로 표시하세요.
```Kotlin
open class Shape

class Rectangle(var height: Double, var length: Double): Shape() {
    var perimeter = (height + length) * 2
}
```
클래스와 객체 및 인스턴스를 참조하세요.
# 주석(Comments)
---
대부분의 최신 언어와 마찬가지로 Kotlin은 한 줄(또는 줄 끝) 및 여러 줄(블록) 주석을 지원합니다.
```Kotlin
// 줄 끝 주석

/* 여러 줄로 된
		주석 */
```
Kotlin의 블록 코멘트는 중첩할 수 있습니다.
```Kotlin
/* 주석이 여기에서 시작하여
/* 중첩된 주석이 포함되고 */
그리고 여기서 끝납니다 */
```
문서 주석 구문에 대한 자세한 내용은 Kotlin 코드 문서화를 참조하세요.
# 문자열 템플릿(String templates)
---
```Kotlin
fun main() {
    var a = 1
    // simple name in template:
    val s1 = "a is $a" 

    a = 2
    // arbitrary expression in template:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
    println(s2)
}
```
자세한 내용은 문자열 템플릿을 참조하세요.
# 조건부 표현식(Conditional expressions)
---
```Kotlin
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}

fun main() {
    println("max of 0 and 42 is ${maxOf(0, 42)}")
}
```
Kotlin에서는 ```if```를 표현식으로도 사용할 수 있습니다.
```Kotlin
fun maxOf(a: Int, b: Int) = if (a > b) a else b

fun main() {
    println("max of 0 and 42 is ${maxOf(0, 42)}")
}
```
```if```-표현식을 참조하세요.
# for loop
---
```Kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }
}
```
or
```Kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }
}
```
루프 참조.
# while loop
---
```Kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    var index = 0
    while (index < items.size) {
        println("item at $index is ${items[index]}")
        index++
    }
}
```
while loop 참조
# When expression
---
```Kotlin
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }

fun main() {
    println(describe(1))
    println(describe("Hello"))
    println(describe(1000L))
    println(describe(2))
    println(describe("other"))
}
```
when expression 참조
# 범위(Ranges)
---
연산자를 사용하여 숫자가 범위 내(```in```)에 있는지 확인합니다.
```Kotlin
fun main() {
    val x = 10
    val y = 9
    if (x in 1..y+1) {
        println("fits in range")
    }
}
```
숫자가 범위를 벗어났는지 확인합니다.
```Kotlin
fun main() {
    val list = listOf("a", "b", "c")

    if (-1 !in 0..list.lastIndex) {
        println("-1 is out of range")
    }
    if (list.size !in list.indices) {
        println("list size is out of valid list indices range, too")
    }
}
```
범위를 반복합니다.
```Kotlin
fun main() {
    for (x in 1..5) {
        print(x)
    }
}
```
또는 진행 상황을 통해.
```Kotlin
fun main() {
    for (x in 1..10 step 2) {
        print(x)
    }
    println()
    for (x in 9 downTo 0 step 3) {
        print(x)
    }
}
```
범위 및 진행 상황을 참조하세요.
# 컬렉션(Collections)
---
컬렉션을 반복합니다.
```Kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }
}
```
연산자를 사용하여 컬렉션에 객체가 포함되어 있는지 확인합니다.(Check if a collection contains an object using ```in``` operator.)
```Kotlin
fun main() {
    val items = setOf("apple", "banana", "kiwifruit")
    when {
        "orange" in items -> println("juicy")
        "apple" in items -> println("apple is fine too")
    }
}
```
람다 표현식을 사용하여 컬렉션을 필터링하고 매핑합니다:
```Kotlin
fun main() {
    val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
    fruits
        .filter { it.startsWith("a") }
        .sortedBy { it }
        .map { it.uppercase() }
        .forEach { println(it) }
}
```
컬렉션 개요를 참조하세요.
# 널 가능 값 및 널 검사(Nullable values and null checks)
---
참조는 ```null``` 값이 가능한 경우 명시적으로 널 가능으로 표시해야 합니다. 널 가능 타입 이름에는 끝에 ```?```이 붙습니다.   
```str```이 정수를 포함하지 않으면 ```null```을 반환합니다:
```Kotlin
fun parseInt(str: String): Int? {
    // ...
}
```
널 값을 반환하는 함수를 사용합니다:
```Kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // Using `x * y` yields error because they may hold nulls.
    if (x != null && y != null) {
        // x and y are automatically cast to non-nullable after null check
        println(x * y)
    }
    else {
        println("'$arg1' or '$arg2' is not a number")
    }    
}

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("a", "b")
}
```
or
```Kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)
    
    // ...
    if (x == null) {
        println("Wrong number format in arg1: '$arg1'")
        return
    }
    if (y == null) {
        println("Wrong number format in arg2: '$arg2'")
        return
    }

    // x and y are automatically cast to non-nullable after null check
    println(x * y)
}

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("99", "b")
}
```
널 안전을 참조하십시오.
# 유형 검사 및 자동 캐스트(Type checks and automatic casts)
---
```is``` 연산자는 표현식이 타입의 인스턴스인지 확인합니다. 불변 지역 변수나 프로퍼티가 특정 유형인지 확인하면 명시적으로 형 변환할 필요가 없습니다:
```Kotlin
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }

    // `obj` is still of type `Any` outside of the type-checked branch
    return null
}

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
or
```Kotlin
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
or even
```Kotlin
fun getStringLength(obj: Any): Int? {
    // `obj` is automatically cast to `String` on the right-hand side of `&&`
    if (obj is String && obj.length > 0) {
        return obj.length
    }

    return null
}

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength("")
    printLength(1000)
}
```
클래스 및 유형 형식을 참조하세요.


# Ref(공식문서)
---
[Kotlin-Basic syntax](https://kotlinlang.org/docs/basic-syntax.html#string-templates)