---
title: 코틀린(kotlin) Basic types(Strings)
date: 2023-07-02
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 기본 유형 공식 레퍼런스 번역, Strings
---
## 코틀린 공식 레퍼런스 번역   

번역 오류가 있을 수 있습니다.

---

Kotlin의 문자열은 `String` 유형으로 표시됩니다. 일반적으로 문자열 값은 큰따옴표(`"`)로 묶인 문자 시퀀스입니다:

```kotlin
val str = "abcd 123"
```

문자열의 요소는 인덱싱 작업을 통해 액세스할 수 있는 문자(`s[i]`)입니다. 이러한 문자는 `for` 루프를 사용하여 반복할 수 있습니다:

```kotlin
fun main() {
val str = "abcd"
    for (c in str) {
        println(c)
    }
}
```

문자열은 불변입니다. 문자열을 초기화하면 값을 변경하거나 새 값을 할당할 수 없습니다. 문자열을 변환하는 모든 연산은 원래 문자열은 변경되지 않은 채 새 `String` 개체에 결과를 반환합니다:

```kotlin
fun main() {
    val str = "abcd"
    println(str.uppercase()) // Create and print a new String object
    println(str) // The original string remains the same
}
```

문자열을 연결하려면 `+` 연산자를 사용합니다. 이 연산자는 표현식의 첫 번째 요소가 문자열인 경우 다른 유형의 값과 문자열을 연결할 때도 작동합니다:

```kotlin
fun main() {
    val s = "abc" + 1
    println(s + "def")
}
```
```
대부분의 경우 문자열 템플릿이나 원시 문자열을 사용하는 것이 문자열 연결보다 바람직합니다.
```
# 문자열 리터럴(String literals)

---

Kotlin에는 두 가지 유형의 문자열 리터럴이 있습니다:

- 이스케이프 문자열(Escape strings)
- 원시 문자열(Raw strings)

## 이스케이프 문자열(Escape strings)

---

이스케이프 문자열에는 이스케이프 문자가 포함될 수 있습니다.

다음은 이스케이프된 문자열의 예입니다:

```kotlin
val s = "Hello, world!\n"
```

이스케이프는 백슬래시(`\`)를 사용하여 기존 방식으로 수행됩니다.
지원되는 이스케이프 시퀀스 목록은 문자 페이지를 참조하세요.

## 원시 문자열(Raw strings)

---

원시 문자열에는 개행과 임의의 텍스트가 포함될 수 있습니다. 원시 문자열은 큰따옴표(`"""`)로 구분되며 이스케이프 이스케이프가 없고 개행 및 기타 문자를 포함할 수 있습니다:

```kotlin
val text = """
    for (c in "foo")
        print(c)
"""
```

원시 문자열에서 선행 공백을 제거하려면 `trimMargin()` 함수를 사용합니다:

```kotlin
val text = """
    |Tell me and I forget.
    |Teach me and I remember.
    |Involve me and I learn.
    |(Benjamin Franklin)
    """.trimMargin()
```

기본적으로 파이프 기호 `|`가 여백 접두사로 사용되지만 다른 문자를 선택하여 `trimMargin(">")`과 같이 매개변수로 전달할 수 있습니다.

# 문자열 템플릿(String templates)

---

문자열 리터럴에는 템플릿 표현식(평가되고 그 결과가 문자열로 연결되는 코드 조각)이 포함될 수 있습니다. 템플릿 표현식은 달러 기호(`$`)로 시작하며 이름 중 하나로 구성됩니다:

```kotlin
fun main() {
    val i = 10
    println("i = $i") // Prints "i = 10"
}
```

또는 중괄호로 묶인 표현식입니다:

```kotlin
fun main() {
    val s = "abc"
    println("$s.length is ${s.length}") // Prints "abc.length is 3"
}
```

템플릿은 원시 문자열과 이스케이프된 문자열 모두에서 사용할 수 있습니다. 식별자의 시작 부분으로 허용되는 기호 앞에 달러 기호 `$`를 원시 문자열(백슬래시 이스케이프가 지원되지 않음)에 삽입하려면 다음 구문을 사용하세요:

```kotlin
val price = """
${'$'}_9.99
"""
```

# Ref
---
[Kotlin-Basic type(Strings)](https://kotlinlang.org/docs/strings.html#string-templates)