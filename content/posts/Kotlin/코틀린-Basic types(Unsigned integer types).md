---
title: 코틀린(kotlin) Basic types(Unsigned integer types)
date: 2023-07-02
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 기본 유형 공식 레퍼런스 번역, Unsigned integer types
---
## 코틀린 공식 레퍼런스 번역   

번역 오류가 있을 수 있습니다.

---

정수 유형 외에도 Kotlin은 부호 없는 정수에 대해 다음과 같은 유형을 제공합니다:

- UByte : 부호 없는 8비트 정수, 범위는 0에서 255입니다.
- UShort : 부호 없는 16비트 정수, 범위는 0에서 65535입니다.
- UInt : 부호 없는 32비트 정수, 범위는 0에서 2^32 - 1입니다.
- ULong : 부호 없는 64비트 정수, 범위는 0에서 2^64 - 1입니다.

부호없는 유형은 부호 유형의 대부분의 연산을 지원합니다.  
```
부호 없는 숫자는 동일한 폭의 부호 있는 대응하는 유형에 해당하는 단일 저장 프로퍼티를 가진 인라인 클래스로 구현됩니다. 그럼에도 불구하고 부호 없는 유형에서 부호 있는 대응 유형으로(또는 그 반대로) 유형을 변경하는 것은 이진 호환되지 않는 변경입니다.
```

# 부호 없는 배열 및 범위(Unsigned arrays and ranges)

---
```
부호없는 배열과 이에 대한 연산은 베타 버전입니다. 언제든지 호환되지 않게 변경될 수 있습니다. 옵트인이 필요합니다(아래 세부 정보 참조).
```
프리미티브와 마찬가지로 부호 없는 각 유형에는 해당 유형의 배열을 나타내는 해당 유형이 있습니다:

- UByteArray : 부호 없는 바이트 배열
- UShortArray : 부호 없는 short 배열
- UIntArray : 부호 없는 int 배열
- ULongArray : 부호 없는 long 배열

부호화된 정수 배열과 동일하게 박스형 오버헤드 없이 `Array` 클래스와 유사한 API를 제공합니다.

부호없는 배열을 사용하면 이 기능이 아직 안정적이지 않음을 나타내는 경고가 표시됩니다. 경고를 제거하려면 `@ExperimentalUnsignedTypes` 주석을 옵트인하세요. 클라이언트가 API 사용을 명시적으로 옵트인해야 하는지 여부는 개발자가 결정할 수 있지만, 부호없는 배열은 안정적인 기능이 아니므로 이를 사용하는 API는 언어 변경으로 인해 손상될 수 있다는 점에 유의하세요. 옵트인 요건에 대해 자세히 알아보세요.

범위와 진행은 `UIntRange`, `UIntProgression`, `ULongRange`, `ULongProgression` 클래스에 의해 `UInt` 및 `ULong`에 지원됩니다. 이러한 클래스는 부호 없는 정수 유형과 함께 안정적입니다.

# 부호없는 정수 리터럴(Unsigned integers literals)

---

부호 없는 정수를 더 쉽게 사용할 수 있도록 Kotlin은 정수 리터럴에 특정 부호 없는 유형을 나타내는 접미사를 태그하는 기능을 제공합니다(`Float` 또는 `Long`과 유사하게):

- `u` 및 `U`태그는 부호 없는 리터럴용입니다. 정확한 유형은 예상 유형에 따라 결정됩니다. 예상 유형이 제공되지 않으면 컴파일러는 리터럴의 크기에 따라 `UInt` 또는 `ULong`을 사용합니다:

```kotlin
val b: UByte = 1u  // UByte, expected type provided
val s: UShort = 1u // UShort, expected type provided
val l: ULong = 1u  // ULong, expected type provided

val a1 = 42u // UInt: no expected type provided, constant fits in UInt
val a2 = 0xFFFF_FFFF_FFFFu // ULong: no expected type provided, constant doesn't fit in UInt
```

- `uL` 및 `UL`은 리터럴을 부호 없는 long으로 명시적으로 태그합니다:

```kotlin
val a = 1UL // ULong, even though no expected type provided and constant fits into UInt
```

# 사용 사례(Use cases)

---

부호 없는 숫자의 주요 사용 사례는 정수의 전체 비트 범위를 활용하여 양수 값을 표현하는 것입니다.
예를 들어 색상과 같이 부호화된 유형에 맞지 않는 16진수 상수를 32비트 `AARRGGBB` 형식으로 표현할 수 있습니다:

```kotlin
data class Color(val representation: UInt)

val yellow = Color(0xFFCC00CCu)
```

부호 없는 숫자를 사용하여 명시적인 `toByte()` 리터럴 형 변환 없이 바이트 배열을 초기화할 수 있습니다:

```kotlin
val byteOrderMarkUtf8 = ubyteArrayOf(0xEFu, 0xBBu, 0xBFu)
```

또 다른 사용 사례는 네이티브 API와의 상호 운용성입니다. Kotlin에서는 시그니처에서 부호없는 유형이 포함된 네이티브 선언을 표현할 수 있습니다. 매핑은 부호 없는 정수를 부호 있는 정수로 대체하지 않으므로 의미는 변경되지 않습니다.

## Non-goals

---

부호 없는 정수는 양수와 0만 나타낼 수 있지만, 애플리케이션 도메인에서 음수가 아닌 정수가 필요한 경우에는 부호 없는 정수를 사용할 수 없습니다. 예를 들어 컬렉션 크기 또는 컬렉션 인덱스 값의 유형으로 사용할 수 있습니다.

몇 가지 이유가 있습니다:

- 부호 있는 정수를 사용하면 실수로 인한 오버플로를 감지하고 빈 목록에 대해 List.lastIndex가 -1인 것과 같은 오류 상태를 알리는 데 도움이 될 수 있습니다.
- 부호 없는 정수는 값의 범위가 부호 있는 정수 범위의 하위 집합이 아니므로 부호 있는 정수의 범위 제한 버전으로 취급할 수 없습니다. 부호 있는 정수와 부호 없는 정수는 서로의 하위 유형이 아닙니다.

# Ref
---
[kotlin-Basic types(Unsigned integer types)](https://kotlinlang.org/docs/unsigned-integer-types.html)