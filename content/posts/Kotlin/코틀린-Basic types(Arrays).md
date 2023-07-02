---
title: 코틀린(kotlin) Basic types(Arrays)
date: 2023-07-02
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 기본 유형 공식 레퍼런스 번역, Arrays
---
## 코틀린 공식 레퍼런스 번역   

번역 오류가 있을 수 있습니다.

---

Kotlin의 배열은 `Array` 클래스로 표현됩니다. 이 클래스에는 연산자 오버로드 규칙에 따라 `[]`로 변환되는 `get()` 및 `set()` 함수와 `size` 속성 및 기타 유용한 멤버 함수가 있습니다:

```kotlin
class Array<T> private constructor() {
    val size: Int
    operator fun get(index: Int): T
    operator fun set(index: Int, value: T): Unit

    operator fun iterator(): Iterator<T>
    // ...
}
```

배열을 만들려면 `arrayOf()` 함수를 사용하고 항목 값을 전달하면 `arrayOf(1, 2, 3)`이 배열 `[1, 2, 3]`을 생성합니다. 또는 `arrayOfNulls()` 함수를 사용하여 `null` 요소로 채워진 지정된 크기의 배열을 만들 수 있습니다.

또 다른 옵션은 배열 크기를 취하는 `Array` 생성자와 인덱스가 주어진 배열 요소의 값을 반환하는 함수를 사용하는 것입니다:

```kotlin
fun main() {
    // Creates an Array<String> with values ["0", "1", "4", "9", "16"]
    val asc = Array(5) { i -> (i * i).toString() }
    asc.forEach { println(it) }
}
```

연산은 멤버 함수 `get()` 및 `set()` 호출을 나타냅니다.

Kotlin의 배열은 불변입니다. 즉, Kotlin에서는 `Array<String>`을 `Array<Any>`에 할당할 수 없으므로 런타임 오류 가능성을 방지할 수 있습니다(단, `Array<out Any>`를 사용할 수 있습니다. 유형 투영 참조).

# 프리미티브 유형 배열(Primitive type arrays)

---

Kotlin에는 박싱 오버헤드 없이 원시 유형의 배열을 나타내는 클래스도 있습니다: `ByteArray`, `ShortArray`, `IntArray`등이 있습니다. 이러한 클래스는 `Array` 클래스와 상속 관계가 없지만 동일한 메서드 및 프로퍼티 집합을 갖습니다. 또한 각각에 해당하는 팩토리 함수도 있습니다:

```kotlin
val x: IntArray = intArrayOf(1, 2, 3)
x[0] = x[1] + x[2]
```

```kotlin
// Array of int of size 5 with values [0, 0, 0, 0, 0]
val arr = IntArray(5)

// Example of initializing the values in the array with a constant
// Array of int of size 5 with values [42, 42, 42, 42, 42]
val arr = IntArray(5) { 42 }

// Example of initializing the values in the array using a lambda
// Array of int of size 5 with values [0, 1, 2, 3, 4] (values initialized to their index value)
var arr = IntArray(5) { it * 1 }
```

# Ref
---
[Kotlin-Basic types(Arrays)](https://kotlinlang.org/docs/arrays.html#primitive-type-arrays)