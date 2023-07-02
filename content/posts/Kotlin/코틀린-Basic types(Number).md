---
title: 코틀린(kotlin) Basic types(Numbers)
date: 2023-07-02
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 기본 유형 공식 레퍼런스 번역, Numbers
---
## 코틀린 공식 레퍼런스 번역
번역 오류가 있을 수 있습니다.

---


Kotlin에서는 모든 변수에서 멤버 함수와 속성을 호출할 수 있다는 점에서 모든 것이 객체입니다. 일부 유형은 특별한 내부 표현(예: 숫자, 문자 및 부울은 런타임에 원시 값으로 표현될 수 있음)을 가질 수 있지만 사용자에게는 일반 클래스처럼 보입니다.

This section describes the basic types used in Kotlin:

- 숫자형
- 부호없는 정수 유형
- 부울
- 문자
- 문자열
- 배열

## 수(Numbers)
---
### 정수 유형(Integer types)
---
Kotlin은 숫자를 나타내는 기본 제공 유형 집합을 제공합니다.
정수 숫자의 경우 크기와 값 범위가 서로 다른 네 가지 유형이 있습니다:

| Type | Size(bits) | Min value | Max value |
| ---- | ---------- | --------- | --------- |
| Byte | 8 | -128 | 127 |
| Short | 16 | -32768 | 32767 |
| Int | 32 | -2,147,483,648(-2^31) | 2,147,483,647 (2^31 - 1) |
| Long | 64 | -9,223,372,036,854,775,808(-2^63) | 9,223,372,036,854,775,807 (2^63 - 1) |

명시적인 타입 지정이 없는 변수를 초기화하면 컴파일러는 값을 표현하기에 가장 작은 범위의 타입을 자동으로 추론합니다. `Int` 범위를 초과하지 않는 경우 유형은 `Int`입니다. 범위를 초과하면 유형은 `Long`입니다. `Long` 값을 명시적으로 지정하려면 값에 접미사 `L`을 추가합니다. 명시적 유형 지정은 컴파일러가 값이 지정된 유형의 범위를 초과하지 않는지 확인하도록 트리거합니다.

`_`를 넣어 가독성을 높일 수 있다. ⇒ 큰 수를 나타낸 때 유용

```kotlin
val n1 = 12345 // 12345
val n2 = 12_345 // 12345
```

```kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```
```
정수 유형 외에도 Kotlin은 부호 없는 정수 유형도 제공합니다. 자세한 내용은 부호 없는 정수 유형을 참조하십시오.
```


### 부동 소수점 유형(Floating-point types)
---
실수의 경우 Kotlin은 IEEE 754 표준을 준수하는 부동 소수점 유형 `Float` 및 `Double`을 제공합니다. `Float`는 IEEE 754 단정밀도, `Double`은 배정밀도를 반영합니다.

이러한 유형은 크기가 다르며 정밀도가 다른 부동 소수점 숫자를 저장할 수 있습니다: 

| Type | Size(bits) | 중요한 비트(Significant bits) | 지수 비트(Exponent bits) | 10진수(Decimal digits) |
| --- | --- | --- | --- | --- |
| Float | 32 | 24 | 8 | 6-7 |
| Double | 64 | 53 | 11 | 15-16 |

분수 부분이 있는 숫자로 `Double` 및 `Float` 변수를 초기화할 수 있습니다. 정수 부분과 마침표(`.`)로 구분됩니다. 분수 부분으로 초기화된 변수의 경우 컴파일러는 `Double` 타입을 유추합니다:

```kotlin
val pi = 3.14 // Double
// val one: Double = 1 // Error: type mismatch
val oneDouble = 1.0 // Double
```

값의 `Float` 유형을 명시적으로 지정하려면 접미사 `f` 또는 `F`를 추가합니다. 이러한 값에 소수점 자릿수가 6~7자리 이상인 경우 반올림됩니다:

```kotlin
fun main() {
    fun printDouble(d: Double) { print(d) }

    val i = 1
    val d = 1.0
    val f = 1.0f

    printDouble(d)
//    printDouble(i) // Error: Type mismatch
//    printDouble(f) // Error: Type mismatch
}
```

숫자 값을 다른 유형으로 변환하려면 명시적 변환을 사용합니다.

### 숫자의 리터럴 상수(Literal constant for numbers)
---
적분 값에 대한 리터럴 상수에는 다음과 같은 종류가 있습니다:

- 소수점(Decimals): `123`
- Long은 대문자 `L`로 태그된다: `123L`
- 2진수, 16진수(Hexadecimals): `0b(2진수)` , `0x0F`
- 바이너리(Binaries): `0b00001011`
```
kotlin에서는 8진법 리터럴이 지원되지 않습니다.
```
Kotlin은 부동 소수점 숫자에 대한 기존 표기법도 지원합니다:

- Doubles by default: 123.5 , 123.5e10
- Floats are tagged by `f` or `F` : 123.5f

밑줄을 사용하여 숫자 상수를 더 읽기 쉽게 만들 수 있습니다:

```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

```
부호 없는 정수 리터럴을 위한 특수 태그도 있습니다.
부호 없는 정수 유형에 대한 리터럴에 대해 자세히 알아보세요.
```

### JVM의 숫자 표현(Numbers representation on the JVM)
---
JVM 플랫폼에서 숫자는 `int`, `double` 등과 같은 기본 유형으로 저장됩니다. `Int?` 같은 널링 가능한 숫자 참조를 만들거나 제네릭을 사용하는 경우는 예외입니다. 이러한 경우 숫자는 Java 클래스 `Integer`, `Double` 등에서 박스형으로 표시됩니다.

같은 번호에 대한 무효화 가능한 참조는 서로 다른 객체를 참조할 수 있습니다:

```kotlin
fun main() {
    val a: Int = 100
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a

    val b: Int = 10000
    val boxedB: Int? = b
    val anotherBoxedB: Int? = b

    println(boxedA === anotherBoxedA) // true
    println(boxedB === anotherBoxedB) // false
}
```

JVM이 `-128에서 127` 사이의 `Integer(정수)`에 메모리 최적화를 적용하기 때문에 `a`에 대한 모든 널 가능 참조는 실제로 동일한 객체입니다. `b` 참조에는 적용되지 않으므로 서로 다른 객체입니다.

다른 한편으로는 여전히 동등합니다:

```kotlin
fun main() {
    val b: Int = 10000
    println(b == b) // Prints 'true'
    val boxedB: Int? = b
    val anotherBoxedB: Int? = b
    println(boxedB == anotherBoxedB) // Prints 'true'
}
```

### 명시적 숫자 전환(Explicit number conversions)
---
표현이 다르기 때문에 작은 유형은 큰 유형의 **하위 유형이 아닙니다**. 만약 그렇다면 다음과 같은 종류의 문제가 발생할 것입니다:

```kotlin
// Hypothetical code, does not actually compile:
val a: Int? = 1 // A boxed Int (java.lang.Integer)
val b: Long? = a // Implicit conversion yields a boxed Long (java.lang.Long)
print(b == a) // Surprise! This prints "false" as Long's equals() checks whether the other is Long as well
```

따라서 정체성은 말할 것도 없고 평등도 소리 없이 사라졌을 것입니다.

결과적으로 작은 유형은 암시적으로 큰 유형으로 변환되지 않습니다. 즉, `Byte` 타입의 값을 `Int` 변수에 할당하려면 명시적으로 변환해야 합니다:

```kotlin
fun main() {
    val b: Byte = 1 // OK, literals are checked statically
    // val i: Int = b // ERROR
    val i1: Int = b.toInt()
}
```

모든 번호 유형은 다른 유형으로의 전환을 지원합니다:

- toByte() : Byte
- toShort() : Short
- toInt() : Int
- toLong( ) : Long
- toFloat( ): Float
- toDouble( ): Double

문맥에서 유형을 유추하고 적절한 변환을 위해 산술 연산에 과부하가 걸리기 때문에 명시적인 변환이 필요하지 않은 경우가 많습니다:

```kotlin
val l = 1L + 3 // Long + Int => Long
```

### 숫자에 대한 연산(Operations on numbers)
---
Kotlin은 숫자에 대한 표준 산술 연산 집합을 지원합니다: `+`, `-`, `*`, `/`, `%`. 이러한 연산은 적절한 클래스의 멤버로 선언됩니다:

```kotlin
fun main() {
    println(1 + 2)
    println(2_500_000_000L - 1L)
    println(3.14 * 2.71)
    println(10.0 / 3)
}
```

사용자 정의 클래스에 대해 이러한 연산자를 재정의할 수도 있습니다. 자세한 내용은 연산자 오버로드를 참조하세요.

### **정수의 나눗셈(Division of integers)**
---
정수 사이의 나눗셈은 항상 정수를 반환합니다. 분수 부분은 모두 버려집니다.

```kotlin
fun main() {
	val x = 5 / 2
	//println(x == 2.5) // ERROR: Operator '==' cannot be applied to 'Int' and 'Double'
	println(x == 2)
}
```

이는 두 정수 유형 사이의 나눗셈에 해당합니다:

```kotlin
fun main() {
    val x = 5L / 2
    println(x == 2L)
}
```

부동 소수점 유형을 반환하려면 인수 중 하나를 부동 소수점 유형으로 명시적으로 변환합니다:

```kotlin
fun main() {
    val x = 5 / 2.toDouble()
    println(x == 2.5)
}
```

### 비트 단위 연산(Bitwise operations)
---
Kotlin은 정수에 대한 비트 연산 집합을 제공합니다. 이러한 연산은 숫자 표현의 비트를 사용하여 이진 수준에서 직접 작동합니다. 비트 단위 연산은 접미사 형식으로 호출할 수 있는 함수로 표현됩니다. `Int` 및 `Long`에만 적용할 수 있습니다:

```kotlin
val x = (1 shl 2) and 0x000FF000
```

비트 단위 연산의 전체 목록은 다음과 같습니다:

- shl(bits) - 양수(signed) shift left
- shr(bits) - 양수(signed) shift right
- ushr(bits) - 음수(unsigned) shift right
- and(bits) - 비트단위(bitwise) AND
- or(bits) - 비트단위(bitwise) OR
- xor(bits) - 비트단위(bitwise) XOR
- inv() - 비트단위(bitwise) 반전(inversion) NOT

### 부동 소수점 숫자 비교(Floating-point numbers comparison)
---
이 섹션에서 설명하는 부동 소수점 숫자에 대한 연산은 다음과 같습니다:

- 동일성 검사(Equality check): `a == b` and `a != b`
- 비교 연산자(Comparison operation): `a < b` , `a > b` , `a <= b` , `a >= b`
- 범위 인스턴스화 및 범위 확인(Range instantiation and range checks): `a..b` , `x in a .. b` , `x !in a .. b`

피연산자 `a`와 `b`가 정적으로 `Float` 또는 `Double` 또는 그 널 가능 대응(유형이 선언되거나 추론되거나 스마트 캐스팅의 결과)으로 알려진 경우, 숫자에 대한 연산과 이들이 형성하는 범위는 부동 소수점 산술에 대한 IEEE 754 표준을 따릅니다.

그러나 일반적인 사용 사례를 지원하고 전체 순서를 제공하기 위해 정적으로 부동 소수점 숫자로 유형화되지 않은 피연산자의 경우 동작이 다릅니다. 예를 들어, `Any`, `Comparable<...>` 또는 `Collection<T>` 유형이 이에 해당합니다. 이 경우 연산은 `Float` 및 `Double`에 대한 `equals` 및 `compareTo` 구현을 사용합니다. 결과적으로: 

- `NaN`은 자신과 동등한 것으로 간주됩니다.
- `NaN`은 `POSITIVE_INFINITY`를 포함한 다른 어떤 요소보다 큰 것으로 간주됩니다.
- `-0.0`은 `0.0` 미만으로 간주됩니다.

다음은 부동 소수점 숫자로 정적으로 타입이 지정된 피연산자(`Double.NaN`)와 부동 소수점 숫자로 정적으로 타입이 지정되지 않은 피연산자(`listOf(T)`) 간의 동작 차이를 보여주는 예시입니다.
```Kotlin
fun main() {
    println(Double.NaN == Double.NaN)                 // false
    println(listOf(Double.NaN) == listOf(Double.NaN)) // true

    println(0.0 == -0.0)                              // true
    println(listOf(0.0) == listOf(-0.0))              // false

    println(listOf(Double.NaN, Double.POSITIVE_INFINITY, 0.0, -0.0).sorted())
    // [-0.0, 0.0, Infinity, NaN]

}
```

# Ref
---
[kotlin-Basic types(Number)](https://kotlinlang.org/docs/numbers.html)