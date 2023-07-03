---
title: 코틀린(kotlin) 반환 및 점프(Returns and jumps)
date: 2023-07-03
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 제어 흐름 공식 레퍼런스 번역, Returns and jumps
---
## 코틀린 공식 레퍼런스 번역   

번역 오류가 있을 수 있습니다.

---

코틀린에는 세 가지 구조 점프 표현식이 있습니다:

- `return`은 기본적으로 가장 가까운 둘러싸는 함수 또는 익명 함수에서 반환합니다.
- `break`는 가장 가까운 둘러싸는 루프를 종료합니다.
- `continue` 를 누르면 가장 가까운 둘러싸는 루프의 다음 단계로 진행합니다.

이러한 모든 표현식은 더 큰 표현식의 일부로 사용할 수 있습니다:

```kotlin
val s = person.name ?: return
```

이러한 표현식의 유형은 Nothing 유형입니다.

# 중단 및 계속 라벨(Break and continue labels)

---

Kotlin의 모든 표현식은 라벨로 표시할 수 있습니다. 레이블은 식별자 뒤에 `@` 기호가 붙는 형식(예: `abc@` 또는 `fooBar@`)입니다. 표현식에 레이블을 지정하려면 표현식 앞에 레이블을 추가하기만 하면 됩니다.

```kotlin
loop@ for (i in 1..100) {
    // ...
}
```

이제 레이블을 사용하여 휴식 또는 계속을 지정할 수 있습니다:

```kotlin
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (...) break@loop
    }
}
```

레이블이 지정된 `break`은 해당 레이블이 표시된 루프 바로 뒤에 있는 실행 지점으로 이동합니다. `continue`은 해당 루프의 다음 반복으로 진행합니다.

# 라벨로 반환(Return to labels)

---

Kotlin에서 함수는 함수 리터럴, 로컬 함수 및 개체 표현식을 사용하여 중첩할 수 있습니다. 정규화된 반환을 사용하면 외부 함수에서 `반환`할 수 있습니다. 가장 중요한 사용 사례는 람다 표현식에서 반환하는 것입니다. 다음을 작성할 때 `반환` 표현식은 가장 가까운 둘러싸는 함수인 `foo`에서 반환된다는 것을 기억하세요:

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return // non-local return directly to the caller of foo()
        print(it)
    }
    println("this point is unreachable")
}

fun main() {
    foo()
}
```

이러한 비로컬 반환은 인라인 함수에 전달된 람다 표현식에 대해서만 지원된다는 점에 유의하세요. 람다 표현식에서 반환하려면 레이블을 지정하고 `반환`에 한정합니다:

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit // local return to the caller of the lambda - the forEach loop
        print(it)
    }
    print(" done with explicit label")
}

fun main() {
    foo()
}
```

이제 람다 표현식에서만 반환됩니다. 이러한 레이블은 람다가 전달되는 함수와 이름이 같기 때문에 암시적 레이블을 사용하는 것이 더 편리할 때가 많습니다.

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // local return to the caller of the lambda - the forEach loop
        print(it)
    }
    print(" done with implicit label")
}

fun main() {
    foo()
}
```

또는 람다 표현식을 익명 함수로 대체할 수도 있습니다. 익명 함수의 `반환` 문은 익명 함수 자체에서 반환됩니다.

```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach(fun(value: Int) {
        if (value == 3) return  // local return to the caller of the anonymous function - the forEach loop
        print(value)
    })
    print(" done with anonymous function")
}

fun main() {
    foo()
}
```

앞의 세 예제에서 로컬 리턴을 사용하는 것은 일반 루프에서 `continue`을 사용하는 것과 유사합니다.

`break`에 직접적으로 대응하는 것은 없지만, 다른 중첩 람다를 추가하고 비로컬 리턴을 통해 시뮬레이션할 수 있습니다:

```kotlin
fun foo() {
    run loop@{
        listOf(1, 2, 3, 4, 5).forEach {
            if (it == 3) return@loop // non-local return from the lambda passed to run
            print(it)
        }
    }
    print(" done with nested loop")
}

fun main() {
    foo()
}
```

값을 반환할 때 구문 분석기는 정규화된 반환을 우선시합니다:

```kotlin
return@a 1
```

즉, "레이블이 지정된 표현식(`@a 1`)을 반환한다"가 아니라 "레이블 `@a`에서 `1`을 반환한다"는 의미입니다.

# Ref
---
[kotlin-Returns and jumps](https://kotlinlang.org/docs/returns.html#return-to-labels)