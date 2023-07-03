---
title: 코틀린(kotlin) 조건 및 루프(Conditions and loops)
date: 2023-07-03
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 제어 흐름 공식 레퍼런스 번역, Conditions and loops
---
## 코틀린 공식 레퍼런스 번역   

번역 오류가 있을 수 있습니다.

---

# If 표현식(If expression)

---

Kotlin에서 if는 값을 반환하는 표현식입니다. 따라서 이 역할에는 일반 `if`가 잘 작동하므로 삼항 연산자(`조건 ? then : else`)가 없습니다.

```kotlin
fun main() {
    val a = 2
    val b = 3

    var max = a
    if (a < b) max = b

    // With else
    if (a > b) {
        max = a
    } else {
        max = b
    }

    // As expression
    max = if (a > b) a else b

    // You can also use `else if` in expressions:
    val maxLimit = 1
    val maxOrLimit = if (maxLimit > a) maxLimit else if (a > b) a else b

    println("max is $max")
    println("maxOrLimit is $maxOrLimit")
}
```

`if` 표현식의 브랜치는 블록이 될 수 있습니다. 이 경우 마지막 표현식은 블록의 값입니다:

```kotlin
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```

예를 들어 값을 반환하거나 변수에 할당하기 위해 `if`를 표현식으로 사용하는 경우 `else` 브랜치는 필수입니다.

# when 표현식(when expression)

---

`when`을 사용하여 여러 분기가 있는 조건식을 정의할 수 있습니다. C 계열 언어의 `switch`문과 유사합니다. 간단한 형태는 다음과 같습니다.

```kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
        print("x is neither 1 nor 2")
    }
}
```

`when`을 사용하면 일부 분기 조건이 만족될 때까지 모든 분기에 대해 인자를 순차적으로 일치시킬 수 있습니다.

`when`은 표현식으로 사용하거나 문으로 사용할 수 있습니다. 표현식으로 사용되는 경우 일치하는 첫 번째 분기의 값이 전체 표현식의 값이 됩니다. 문으로 사용하면 개별 분기의 값은 무시됩니다. `if`와 마찬가지로 각 분기는 블록이 될 수 있으며, 그 값은 블록의 마지막 표현식의 값이 됩니다.

`else` 분기 조건이 충족되지 않으면 다른 분기가 평가됩니다.

`when`이 표현식으로 사용되는 경우, 컴파일러가 `enum class`항목 및 `sealed class` 서브타입과 같이 가능한 모든 경우가 분기 조건에 포함된다는 것을 증명할 수 없는 한, `else` 분기는 필수입니다.

```kotlin
enum class Bit {
    ZERO, ONE
}

val numericValue = when (getRandomBit()) {
    Bit.ZERO -> 0
    Bit.ONE -> 1
    // 'else' is not required because all cases are covered
}
```

`when` 문에서 `else` 분기는 다음 조건에서 필수입니다:

- `Boolean`,`enum`, `sealed` 타입의 객체 또는 그 무효화 가능한 객체가 있는 경우.
- `when` 의 브랜치가 이 주제에 대해 가능한 모든 경우를 다루지는 않습니다.

```kotlin
enum class Color {
    RED, GREEN, BLUE
}

when (getColor()) {
    Color.RED -> println("red")
    Color.GREEN -> println("green")
    Color.BLUE -> println("blue")
    // 'else' is not required because all cases are covered
}

when (getColor()) {
    Color.RED -> println("red") // no branches for GREEN and BLUE
    else -> println("not red") // 'else' is required
}
```

여러 사례에 대한 공통 동작을 정의하려면 쉼표로 한 줄에 조건을 결합합니다:

```kotlin
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```

상수뿐만 아니라 임의의 표현식을 분기 조건으로 사용할 수 있습니다.

```kotlin
when (x) {
    s.toInt() -> print("s encodes x")
    else -> print("s does not encode x")
}
```

범위 또는 컬렉션에 있는 값 `in` 또는 `!in`에 있는 값을 확인할 수도 있습니다:

```kotlin
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```

또 다른 옵션은 값이 특정 유형 `in`  또는 `!in`인지를 확인하는 것입니다. 스마트 형식을 사용하면 추가 확인 없이 해당 유형의 메서드와 프로퍼티에 액세스할 수 있습니다.

```kotlin
fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}
```

`when` 은 `if`-`else` `if` 체인을 대체하는 용도로도 사용할 수 있습니다. 인수가 제공되지 않으면 분기 조건은 단순히 부울 표현식이며 조건이 참일 때 분기가 실행됩니다:

```kotlin
when {
    x.isOdd() -> print("x is odd")
    y.isEven() -> print("y is even")
    else -> print("x+y is odd")
}
```

다음 구문을 사용하여 변수에 피사체가 있을 때 캡처할 수 있습니다:

```kotlin
fun Request.getBody() =
    when (val response = executeRequest()) {
        is Success -> response.body
        is HttpError -> throw HttpException(response.status)
    }
```

when 주제에 도입되는 변수의 범위는 이 when 본문으로 제한됩니다.

# 루프의 경우(For loops)

---

`for` 루프는 이터레이터를 제공하는 모든 것을 반복합니다. 이는 C#과 같은 언어의 `foreach` 루프와 동일합니다. `for`의 구문은 다음과 같습니다:

```kotlin
for (item in collection) print(item)
```

`for`의 본문은 블록일 수 있습니다.

```kotlin
for (item: Int in ints) {
    // ...
}
```

앞서 언급했듯이, `for`를 사용하면 반복자를 제공하는 모든 것을 반복할 수 있습니다. 이는 즉: 

- `Iterator<>`를 반환하는 멤버 또는 확장 함수 `iterator()`가 있습니다:
    - 멤버 또는 확장 함수인 `next()`
    - 에는 `부울`을 반환하는 멤버 또는 확장 함수 `hasNext()`가 있습니다.

이 세 가지 함수는 모두 `연산자`로 표시해야 합니다.

숫자 범위를 반복하려면 범위 표현식을 사용합니다:

```kotlin
fun main() {
    for (i in 1..3) {
        println(i)
    }
    for (i in 6 downTo 0 step 2) {
        println(i)
    }
}
```

범위 또는 배열에 대한 `for` 루프는 반복자 객체를 생성하지 않는 인덱스 기반 루프로 컴파일됩니다.

인덱스가 있는 배열이나 목록을 반복하려면 이런 식으로 하면 됩니다:

```kotlin
fun main() {
val array = arrayOf("a", "b", "c")
    for (i in array.indices) {
        println(array[i])
    }
}
```

또는 `withIndex` 라이브러리 함수를 사용할 수 있습니다:

```kotlin
fun main() {
    val array = arrayOf("a", "b", "c")
    for ((index, value) in array.withIndex()) {
        println("the element at $index is $value")
    }
}
```

# While loops

---

`while`과 `do-while` 루프는 조건이 충족되는 동안 지속적으로 실행됩니다. 이 둘의 차이점은 조건 확인 시간입니다:

- while을 실행하는 동안 조건을 검사하고 조건이 충족되면 본문을 실행한 다음 조건 검사로 돌아갑니다.
- `do-while`은 본문을 실행한 다음 조건을 확인합니다. 조건이 만족되면 루프가 반복됩니다. 따라서 `do-while`의 본문은 조건에 관계없이 적어도 한 번은 실행됩니다.

```kotlin
while (x > 0) {
    x--
}

do {
    val y = retrieveData()
} while (y != null) // y is visible here!
```

# 루프에서 중단 및 계속(Break and continue in loops)

---

kotlin은 루프에서 기존의 `break` 및 `continue` 연산자를 지원합니다. 반환 및 점프를 참조하세요.

# Ref
---
[kotlin-Conditions and loops](https://kotlinlang.org/docs/control-flow.html#break-and-continue-in-loops)