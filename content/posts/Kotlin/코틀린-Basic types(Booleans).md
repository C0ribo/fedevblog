---
title: 코틀린(kotlin) Basic types(Booleans)
date: 2023-07-02
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 기본 유형 공식 레퍼런스 번역, Booleans
---
## 코틀린 공식 레퍼런스 번역
번역 오류가 있을 수 있습니다.

---


부울 유형은 참과 거짓의 두 가지 값을 가질 수 있는 부울 객체를 나타냅니다.

부울에는 널 값을 갖는 대응하는 `Boolean?`이 있습니다.

부울에 대한 기본 제공 연산은 다음과 같습니다:

- `||` - 분리(논리적 OR)
- `&&` - 접속사(논리적 AND)
- `!` - 부정(논리적 NOT)

`||` 및 `&&` 느리게 작업합니다.

```kotlin
fun main() {
    val myTrue: Boolean = true
    val myFalse: Boolean = false
    val boolNull: Boolean? = null

    println(myTrue || myFalse)
    println(myTrue && myFalse)
    println(!myTrue)
}
```
```
JVM에서: 부울 객체에 대한 널 가능 참조는 숫자와 유사하게 박스 처리됩니다.
```

# Ref
---
[kotlin-Basic types(Booleans)](https://kotlinlang.org/docs/booleans.html)