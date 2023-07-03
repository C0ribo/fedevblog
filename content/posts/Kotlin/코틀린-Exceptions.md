---
title: 코틀린(kotlin) 예외(Exceptions)
date: 2023-07-03
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 제어 흐름 공식 레퍼런스 번역, Exceptions
---
## 코틀린 공식 레퍼런스 번역   

번역 오류가 있을 수 있습니다.

---


# 예외 클래스(Exception classes)

---

Kotlin의 모든 예외 클래스는 `Throwable` 클래스를 상속합니다. 모든 예외에는 메시지, 스택 추적 및 선택적 원인이 있습니다.

예외 객체를 던지려면 `throw` 표현식을 사용합니다:

```kotlin
fun main() {
    throw Exception("Hi There!")
}
```

예외를 잡으려면 `try...catch` 표현식을 사용하세요:

```kotlin
try {
    // some code
} catch (e: SomeException) {
    // handler
} finally {
    // optional finally block
}
```

`catch` 블록은 0개 이상일 수 있으며, `finally` 블록은 생략할 수 있습니다. 그러나 하나 이상의 `catch` 또는 `finally` 블록이 필요합니다.

## Try는 표현식입니다.(Try is an expression)

---

`try`는 표현식이므로 반환값을 가질 수 있습니다:

```kotlin
val a: Int? = try { input.toInt() } catch (e: NumberFormatException) { null }
```

`try` 표현식의 반환 값은 `try` 블록의 마지막 표현식 또는 `catch` 블록(또는 블록)의 마지막 표현식입니다. `finally` 블록의 내용은 표현식의 결과에 영향을 주지 않습니다.

# 확인된 예외(Checked exception)

---

Kotlin에는 확인된 예외가 없습니다. 여기에는 여러 가지 이유가 있지만 그 이유를 설명하기 위해 간단한 예를 들어 보겠습니다.

다음은 `StringBuilder` 클래스가 구현한 JDK의 인터페이스 예제입니다:

```kotlin
Appendable append(CharSequence csq) throws IOException;
```

이 시그니처는 문자열을 무언가에 추가할 때마다(`StringBuilder`, 일종의 로그, 콘솔 등) `IOException`을 잡아야 한다고 말합니다. 왜 그럴까요? 구현이 IO 연산을 수행 중일 수 있기 때문입니다(`Writer`는 `Appendable`도 구현합니다). 그 결과 여기저기서 이런 코드가 나옵니다:

```kotlin
try {
    log.append(message)
} catch (IOException e) {
    // Must be safe
}
```

이는 좋지 않습니다. 효과적인 Java, 3판, 77번 항목: 예외를 무시하지 마세요를 살펴보시기 바랍니다.

브루스 에켈은 확인된 예외에 대해 이렇게 말합니다:

```
소규모 프로그램을 검토한 결과 예외 사양을 요구하면 개발자의 생산성을 높이고 코드 품질을 향상시킬 수 있다는 결론에 도달했지만, 대규모 소프트웨어 프로젝트의 경험에 따르면 생산성은 감소하고 코드 품질은 거의 향상되지 않는 다른 결과가 나타났습니다.
```

이 문제에 대한 몇 가지 추가 의견을 알려드립니다:

- Java의 확인된 예외는 실수였습니다(Rod Waldhoff).
- 확인된 예외의 문제(Anders Hejlsberg)

Java, Swift 또는 Objective-C에서 Kotlin 코드를 호출할 때 발생할 수 있는 예외에 대해 호출자에게 경고하려면 `@Throws` 어노테이션을 사용할 수 있습니다. 이 어노테이션을 Java와 Swift 및 Objective-C에 사용하는 방법에 대해 자세히 알아보세요.

# 없음 유형(The Nothing type)

---

`throw`는 Kotlin의 표현식이므로 예를 들어 Elvis 표현식의 일부로 사용할 수 있습니다:

```kotlin
val s = person.name ?: throw IllegalArgumentException("Name required")
```

`throw` 표현식의 유형은 `Nothing`입니다. 이 유형은 값이 없으며 절대 도달할 수 없는 코드 위치를 표시하는 데 사용됩니다. 자체 코드에서 `Nothing`을 사용하여 반환되지 않는 함수를 표시할 수 있습니다:

```kotlin
fun fail(message: String): Nothing {
    throw IllegalArgumentException(message)
}
```

이 함수를 호출하면 컴파일러는 호출 이후 실행이 계속되지 않는다는 것을 알 수 있습니다:

```kotlin
val s = person.name ?: fail("Name required")
println(s)     // 's' is known to be initialized at this point
```

유형 추론을 처리할 때도 이 유형이 발생할 수 있습니다. 이 유형의 null 가능 변형인 `Nothing?` 에는 가능한 값이 정확히 하나, 즉 `null`이 있습니다. `null`을 사용하여 추론된 유형의 값을 초기화할 때 더 구체적인 유형을 결정하는 데 사용할 수 있는 다른 정보가 없는 경우 컴파일러는 `Nothing?` 유형을 추론합니다:

```kotlin
val x = null           // 'x' has type `Nothing?`
val l = listOf(null)   // 'l' has type `List<Nothing?>
```

# Java 상호 운용성

---

Java 상호 운용성에 대한 자세한 내용은 Java 상호 운용성 페이지의 예외 섹션을 참조하세요.

# Ref
---
[kotlin-Exceptions](https://kotlinlang.org/docs/exceptions.html#java-interoperability)