---
title: 코틀린(kotlin) Basic types(Character)
date: 2023-07-02
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 기본 유형 공식 레퍼런스 번역, Character
---
## 코틀린 공식 레퍼런스 번역   

번역 오류가 있을 수 있습니다.

---

문자는 `Char` 유형으로 표시됩니다. 문자 리터럴은 작은따옴표로 묶습니다: `'1'`.

특수 문자는 이스케이프 백슬래시 `\`로 시작합니다. 지원되는 이스케이프 시퀀스는 다음과 같습니다:

- `\t` - tab
- `\b` - backspace
- `\n` - new line(LF)
- `\r` - carriage return (CR)
- `\’` - 작은 따옴표(single quotation mark)
- `\’’` - 큰 따옴표(double quotation mark)
- `\\` - 백슬래쉬
- `\$` - 달러 기호(dollar sign)

다른 문자를 인코딩하려면 유니코드 이스케이프 시퀀스 구문을 사용합니다: `'\uFF00'`.

```kotlin
fun main() {
    val aChar: Char = 'a'

    println(aChar)
    println('\n') // Prints an extra newline character
    println('\uFF00')
}
```

문자 변수 값이 숫자인 경우 `digitToInt()` 함수를 사용하여 명시적으로 `Int` 숫자로 변환할 수 있습니다.

```
JVM에서: 숫자와 마찬가지로 문자도 널링 가능한 참조가 필요할 때 박스 처리됩니다. 박싱 작업으로 인해 ID가 보존되지 않습니다.
```

# Ref
---
[Kotlin-Basic types(Character)](https://kotlinlang.org/docs/characters.html)