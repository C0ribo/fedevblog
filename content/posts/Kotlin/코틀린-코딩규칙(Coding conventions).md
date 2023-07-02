---
title: 코틀린(kotlin) 코딩 규칙(Coding conventions)
date: 2023-07-01
tags:
  - Kotlin
social_image: /media/kotlin.png
description: 코틀린의 코딩 규칙 공식 레퍼런스 번역
---
## 코틀린 공식 레퍼런스 번역
번역 오류가 있을 수 있습니다.

---

일반적으로 잘 알려져 있고 따라 하기 쉬운 코딩 규칙은 모든 프로그래밍 언어에 필수적입니다. 여기에서는 Kotlin을 사용하는 프로젝트를 위한 코드 스타일 및 코드 구성에 대한 지침을 제공합니다.

# IDE에서 스타일 구성(**Configure style in IDE)**

---

Kotlin에서 가장 많이 사용되는 두 가지 IDE인 IntelliJ IDEA와 Android Studio는 코드 스타일 지정에 대한 강력한 지원을 제공합니다. 지정된 코드 스타일에 따라 코드 서식을 자동으로 지정하도록 구성할 수 있습니다.

## 스타일 가이드 적용(Apply the style guide)

---

1. 설정/환경 설정 | 편집기 | 코드 스타일 | Kotlin으로 이동합니다.
2. 다음에서 설정을 클릭합니다.…
3. Kotlin 스타일 가이드를 선택합니다.

## 코드가 스타일 가이드를 따르는지 확인

---

1. 설정/환경설정 | 편집기 | 검사 | 일반으로 이동합니다.
2. 잘못된 서식 검사를 켭니다. 스타일 가이드에 설명된 다른 문제(예: 명명 규칙)를 확인하는 추가 검사는 기본적으로 활성화되어 있습니다.

# 소스 코드 정리

---

## 디렉토리 구조

---

순수 Kotlin 프로젝트에서 권장되는 디렉터리 구조는 공통 루트 패키지가 생략된 패키지 구조를 따릅니다. 예를 들어 프로젝트의 모든 코드가 `org.example.kotlin` 패키지와 그 하위 패키지에 있는 경우 `org.example.kotlin` 패키지가 있는 파일은 소스 루트 바로 아래에 배치해야 하며, `org.example.kotlin.network.socket`에 있는 파일은 소스 루트의 `network/socket` 하위 디렉터리에 배치해야 합니다.


```JVM에서: Kotlin이 Java와 함께 사용되는 프로젝트에서 Kotlin 소스 파일은 Java 소스 파일과 동일한 소스 루트에 있어야 하며, 동일한 디렉토리 구조를 따라야 합니다.```



## 소스 파일 이름(Source file names)

---

Kotlin 파일에 단일 클래스 또는 인터페이스(관련 최상위 선언이 포함될 수 있음)가 포함된 경우 해당 이름은 클래스 이름과 동일해야 하며 확장자 `.kt`가 추가되어야 합니다. 이는 모든 유형의 클래스와 인터페이스에 적용됩니다. 파일에 여러 클래스가 포함되어 있거나 최상위 선언만 있는 경우에는 파일에 포함된 내용을 설명하는 이름을 선택하고 그에 따라 파일 이름을 지정합니다. 첫 글자가 대문자(파스칼 대소문자라고도 함)인 camel case로 예를 들어,  `ProcessDeclarations.kt`를 사용합니다.

파일 이름은 파일의 코드가 수행하는 작업을 설명해야 합니다. 따라서 파일 이름에 `Util`와 같은 의미 없는 단어를 사용하지 않아야 합니다.

## 멀티플랫폼 프로젝트(Multiplatform projects)

---

멀티플랫폼 프로젝트에서 플랫폼별 소스 집합의 최상위 선언이 있는 파일에는 소스 집합의 이름과 연결된 접미사가 있어야 합니다. 예를 들어: 

- **jvm**Main/kotlin/Platform.**jvm**.kt
- **android**Main/kotlin/Platform.**android**.kt
- iosMain/kotlin/Platform.ios.kt

공통 소스 집합의 경우 최상위 선언이 있는 파일에는 접미사가 없어야 합니다. 예를 들어, `commonMain/kotlin/Platform.kt.`

- Technical details
    
    멀티플랫폼 프로젝트에서는 최상위 멤버(함수, 속성)를 허용하지 않는 JVM 제한 사항으로 인해 이 파일 명명 체계를 따르는 것이 좋습니다.
    
    이 문제를 해결하기 위해 Kotlin JVM 컴파일러는 최상위 멤버 선언을 포함하는 래퍼 클래스(소위 "파일 파사드")를 생성합니다. 파일 파사드에는 파일 이름에서 파생된 내부 이름이 있습니다.
    
    결과적으로 JVM은 동일한 정규화된 이름(FQN)을 가진 여러 클래스를 허용하지 않습니다. 이로 인해 Kotlin 프로젝트를 JVM으로 컴파일할 수 없는 상황이 발생할 수 있습니다:
    
    ```kotlin
    root
    |- commonMain/kotlin/myPackage/Platform.kt // contains 'fun count() { }'
    |- jvmMain/kotlin/myPackage/Platform.kt // contains 'fun multiply() { }'
    ```
    
    여기서 두 `Platform.kt` 파일은 모두 동일한 패키지에 있으므로 Kotlin JVM 컴파일러는 두 개의 파일 파사드를 생성하며, 두 파일 모두 FQN이 `myPackage.PlatformKt`입니다. 이로 인해 "중복 JVM 클래스" 오류가 발생합니다.
    
    이를 방지하는 가장 간단한 방법은 위의 가이드라인에 따라 파일 중 하나의 이름을 변경하는 것입니다. 이 명명 체계는 코드 가독성을 유지하면서 충돌을 방지하는 데 도움이 됩니다.
    
    ```
    📖 이러한 권장 사항이 중복되어 보일 수 있는 두 가지 경우가 있지만, 그래도 권장 사항을 따르는 것이 좋습니다:
    
    - JVM이 아닌 플랫폼에서는 파일 파사드 복제에 문제가 없습니다. 하지만 이 명명 체계를 사용하면 파일 명명을 일관성 있게 유지할 수 있습니다.
    - JVM에서는 소스 파일에 최상위 선언이 없는 경우 파일 파사드가 생성되지 않으므로 명명 충돌이 발생하지 않습니다.
    
    그러나 이 명명 체계를 사용하면 간단한 리팩터링이나 추가에 최상위 함수가 포함되어 동일한 "중복 JVM 클래스" 오류가 발생하는 상황을 피할 수 있습니다.
    ```
    
    

## 소스 파일 정리(Source file organization)

---

선언(클래스, 최상위 함수 또는 프로퍼티)이 의미적으로 서로 밀접하게 연관되어 있고 파일 크기가 수백 줄을 넘지 않는 합리적인 수준이라면 동일한 Kotlin 소스 파일에 여러 선언을 배치하는 것을 권장합니다.

특히 이 클래스의 모든 클라이언트와 관련된 클래스의 확장 함수를 정의할 때는 클래스 자체와 같은 파일에 넣어야 합니다. 특정 클라이언트에만 해당하는 확장 함수를 정의할 때는 해당 클라이언트의 코드 옆에 배치하세요. 특정 클래스의 모든 확장 함수를 담기 위해 파일을 만들지 마세요.

## 클래스 레이아웃(Class layout)

---

수업의 내용은 다음 순서로 진행해야 합니다:

1. 속성 선언 및 이니셜라이저 블록
2. 보조 생성자
3. 메서드 선언
4. 컴포넌트 개체

메서드 선언을 알파벳순이나 표시 여부에 따라 정렬하지 말고, 일반 메서드와 확장 메서드를 구분하지 마세요. 대신, 클래스를 위에서 아래로 읽는 사람이 무슨 일이 일어나고 있는지 논리를 따라갈 수 있도록 관련 내용을 함께 배치하세요. 순서를 정하고(상위 수준에서 먼저 또는 그 반대로) 그 순서를 고수하세요.

중첩된 클래스는 해당 클래스를 사용하는 코드 옆에 배치합니다. 클래스가 외부에서 사용되어야 하고 클래스 내부에서 참조되지 않는 경우 컴패니언 객체 뒤의 마지막에 배치합니다.

## 인터페이스 구현 레이아웃(Interface implementation layout)

---

인터페이스를 구현할 때 구현 멤버를 인터페이스의 멤버와 동일한 순서로 유지합니다(필요한 경우 구현에 사용되는 추가 비공개 메서드가 산재되어 있음).

## 과부하 레이아웃(Overload layout)

---

클래스에서 항상 과부하를 나란히 배치하세요.

# 이름 지정 규칙(Naming rules)

---

Kotlin의 패키지 및 클래스 명명 규칙은 매우 간단합니다:

- 패키지 이름은 항상 소문자로 사용하고 밑줄을 사용하지 마세요(`org.example.project`). 일반적으로 여러 단어의 이름을 사용하지 않는 것이 좋지만, 여러 단어를 사용해야 하는 경우 여러 단어를 연결하거나 camel case(`org.example.myProject`)를 사용할 수 있습니다.
- 클래스 및 객체의 이름은 대문자로 시작하고 camel case를 사용합니다:

```kotlin
open class DeclarationProcessor { /*...*/ }

object EmptyDeclarationProcessor : DeclarationProcessor() { /*...*/ }
```

## 함수 이름(Function names)

---

함수, 프로퍼티 및 로컬 변수의 이름은 소문자로 시작하고 camel case를 사용하며 밑줄을 사용하지 않습니다:

```kotlin
fun processDeclarations() { /*...*/ }
var declarationCount = 1
```

예외: 클래스의 인스턴스를 생성하는 데 사용되는 팩토리 함수는 추상 반환 유형과 동일한 이름을 가질 수 있습니다:

```kotlin
interface Foo { /*...*/ }

class FooImpl : Foo { /*...*/ }

fun Foo(): Foo { return FooImpl() }
```

## 테스트 방법의 이름(Names for test methods)

---

테스트(테스트에서만)에서는 공백으로 둘러싸인 메서드 이름을 사용할 수 있습니다. 이러한 메서드 이름은 현재 안드로이드 런타임에서 지원되지 않는다는 점에 유의하세요. 메서드 이름의 밑줄은 테스트 코드에서도 사용할 수 있습니다.

```kotlin
class MyTestCase {
     @Test fun `ensure everything works`() { /*...*/ }

     @Test fun ensureEverythingWorks_onAndroid() { /*...*/ }
}
```

## 속성 이름(Property names)

---

상수(`const`로 표시된 프로퍼티, 또는 사용자 지정 `get` 함수가 없는 최상위 수준 또는 객체 `val` 프로퍼티는  불변의 데이터를 보유합니다.)의 이름은 대문자 밑줄로 구분된(screaming snake case) 이름을 사용해야 합니다:

```kotlin
const val MAX_COUNT = 8
val USER_NAME_FIELD = "UserName"
```

동작 또는 변경 가능한 데이터가 있는 객체를 보유하는 최상위 수준 또는 객체 속성의 이름은 대소문자 구분을 사용해야 합니다:

```kotlin
val mutableCollection: MutableSet<String> = HashSet()
```

싱글톤 객체에 대한 참조를 포함하는 프로퍼티 이름은 `object` 선언과 동일한 명명 스타일을 사용할 수 있습니다:

```kotlin
val PersonComparator: Comparator<Person> = /*...*/
```

열거형 상수의 경우, 용도에 따라 밑줄로 구분된 대문자 이름(screaming snake case)(`enum class Color { RED, GREEN }`)또는 용도에 따라 대문자 이름을 사용합니다.

## 백업 속성의 이름(Names for backing properties)

---

클래스에 개념적으로 동일한 두 개의 프로퍼티가 있지만 하나는 공개 API의 일부이고 다른 하나는 구현 세부 사항인 경우, 비공개 프로퍼티 이름의 접두사로 밑줄을 사용합니다:

```kotlin
class C {
    private val _elementList = mutableListOf<Element>()

    val elementList: List<Element>
         get() = _elementList
}
```

## 좋은 이름 선택(Choose good names)

---

클래스의 이름은 일반적으로 클래스가 무엇인지 설명하는 명사 또는 명사 구문으로 구성됩니다: `List`, `PersonReader`.

메서드의 이름은 일반적으로 메서드가 수행하는 작업을 나타내는 동사 또는 동사 구문입니다(예: `close`, `readPersons`). 메서드가 객체를 변경하는지 아니면 새 객체를 반환하는지도 이름에 표시해야 합니다. 예를 들어 `sort`은 컬렉션을 제자리에서 정렬하는 것이고, `sorted`은 컬렉션의 정렬된 사본을 반환하는 것입니다.

이름은 Entity(실체)의 목적을 명확하게 나타내야 하므로 이름에 의미 없는 단어(`Manager`, `Wrapper`)를 사용하지 않는 것이 좋습니다.

약어를 선언 이름의 일부로 사용하는 경우 두 글자로 구성된 경우 대문자로 표시하고(`IOStream`), 더 긴 경우 첫 글자만 대문자로 표시합니다(`XmlFormatter`, `HttpInputStream`).

# 서식 지정(Formatting)

---

## 들여쓰기(Indentation)

---

들여쓰기는 네 칸을 사용합니다. 탭을 사용하지 마십시오.

중괄호의 경우 여는 중괄호는 구조가 시작되는 선의 끝에, 닫는 중괄호는 여는 구조와 수평으로 정렬된 별도의 선에 배치합니다.

```kotlin
if (elements != null) {
    for (element in elements) {
        // ...
    }
}
```
```
⚠️ Kotlin에서 세미콜론은 선택 사항이므로 줄 바꿈이 중요합니다. 언어 설계에서는 Java 스타일 중괄호를 사용한다고 가정하므로 다른 서식 스타일을 사용하려고 하면 예상치 못한 동작이 발생할 수 있습니다.
```

## 가로 공백(Horizontal whitespace)

---

- 이진 연산자(`a + b`) 주위에 공백을 넣습니다. 예외: "범위" 연산자(`0..i`) 주위에 공백을 넣지 마세요.
- 단항 연산자(`a++`) 주위에 공백을 넣지 마십시오.
- 제어 흐름 키워드(`if`, `when`, `for`, `while`)와 해당 여는 괄호 사이에 공백을 넣습니다.
- 기본 생성자 선언, 메서드 선언 또는 메서드 호출에서 여는 괄호 앞에 공백을 넣지 마세요.

```kotlin
class A(val x: Int)

fun foo(x: Int) { ... }

fun bar() {
    foo(1)
}
```

- `(`, `[`, 또는 앞 `]`, `)` 뒤에 공백을 넣지 마십시오.
- `.`또는 `?.` : `foo.bar().filter { it > 2 }.joinToString()`, `foo?.bar()`
- `//` 뒤에 공백을 입력합니다 : `// This is a comment`
- 유형 매개 변수를 지정하는 데 사용되는 꺾쇠 괄호 주위에 공백을 넣지 마십시오: `class Map<K, V> { ... }`
- `::` 주위에 공백을 넣지 마십시오 : `Foo::class`, `String::length`
- null 가능한 유형을 표시하는 데 사용되는 `?` 앞에 공백을 넣지 마십시오: `String?`

일반적으로 어떤 종류의 가로 정렬도 피하세요. 식별자의 이름을 다른 길이의 이름으로 변경해도 선언이나 사용 서식에는 영향을 미치지 않습니다.

## 콜론(Colon)

---

다음과 같은 `:` 경우 앞에 공백을 둡니다 : 

- 유형과 상위 유형을 구분하는 데 사용되는 경우
- 슈퍼클래스 생성자나 같은 클래스의 다른 생성자에게 위임할 때
- `object` 키워드 뒤에

선언과 그 유형을 구분할 때 `:` 앞에 공백을 넣지 마세요.

`:` 뒤에 항상 공백을 넣어야 합니다.

```kotlin
abstract class Foo<out T : Any> : IFoo {
    abstract fun foo(a: Int): T
}

class FooImpl : Foo() {
    constructor(x: String) : this(x) { /*...*/ }

    val x = object : IFoo { /*...*/ }
}
```

## 클래스 헤더(Class headers)

---

기본 생성자 매개변수가 몇 개 있는 클래스는 한 줄로 작성할 수 있습니다:

```kotlin
class Person(id: Int, name: String)
```

헤더가 긴 클래스는 각 기본 생성자 매개변수가 들여쓰기가 있는 별도의 줄에 있도록 서식을 지정해야 합니다. 또한 닫는 괄호는 새 줄에 넣어야 합니다. 상속을 사용하는 경우 슈퍼클래스 생성자 호출 또는 구현된 인터페이스 목록은 괄호와 같은 줄에 위치해야 합니다:

```kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name) { /*...*/ }
```

여러 인터페이스의 경우 슈퍼클래스 생성자 호출을 먼저 배치한 다음 각 인터페이스를 다른 줄에 배치해야 합니다:

```kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name),
    KotlinMaker { /*...*/ }
```

긴 상위 유형 목록이 있는 클래스의 경우 콜론 뒤에 줄 바꿈을 넣고 모든 상위 유형 이름을 가로로 정렬합니다:

```kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne {

    fun foo() { /*...*/ }
}
```

클래스 헤더가 긴 경우 클래스 헤더와 본문을 명확하게 구분하려면 위의 예에서처럼 클래스 헤더 뒤에 빈 줄을 넣거나 여는 중괄호를 별도의 줄에 넣습니다:

```kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne
{
    fun foo() { /*...*/ }
}
```

생성자 매개변수에는 일반 들여쓰기(네 칸)를 사용합니다. 이렇게 하면 기본 생성자에서 선언된 프로퍼티가 클래스 본문에 선언된 프로퍼티와 동일한 들여쓰기를 갖도록 합니다.

## 수정자 순서(Modifiers order)

---

선언에 여러 개의 수정자가 있는 경우 항상 다음 순서로 배치하세요:

```kotlin
public / protected / private / internal
expect / actual
final / open / abstract / sealed / const
external
override
lateinit
tailrec
vararg
suspend
inner
enum / annotation / fun // as a modifier in `fun interface`
companion
inline / value
infix
operator
data
```

모든 주석을 수정자 앞에 배치합니다:

```kotlin
@Named("Foo")
private val foo: Foo
```

라이브러리에서 작업하는 것이 아니라면 중복되는 수정자(예: `public`)는 생략하세요.

## 주석(Annotations)

---

주석은 주석이 첨부된 선언 앞에 별도의 줄에 동일한 들여쓰기를 사용하여 배치합니다:

```kotlin
@Target(AnnotationTarget.PROPERTY)
annotation class JsonExclude
```

인수가 없는 주석은 같은 줄에 배치할 수 있습니다:

```kotlin
@JsonExclude @JvmField
var x: String
```

인수가 없는 단일 주석은 해당 선언과 같은 줄에 배치할 수 있습니다:

```kotlin
@Test fun foo() { /*...*/ }
```

## 파일 주석(File annotations)

---

파일 주석은 파일 주석(있는 경우) 뒤, `package` 문 앞에 배치되며 `package`와 빈 줄로 구분됩니다(패키지가 아닌 파일을 대상으로 한다는 사실을 강조하기 위해).

```kotlin
/** License, copyright and whatever */
@file:JvmName("FooBar")

package foo.bar
```

## 함수(Functions)

---

함수 시그니처 한 줄에 맞지 않는 경우 다음 구문을 사용합니다:

```kotlin
fun longMethodName(
    argument: ArgumentType = defaultValue,
    argument2: AnotherArgumentType,
): ReturnType {
    // body
}
```

함수 매개변수에는 일반 들여쓰기(네 칸)를 사용합니다. 생성자 매개변수와의 일관성을 유지하는 데 도움이 됩니다.

단일 표현식으로 구성된 본문이 있는 함수에 표현식 본문을 사용하는 것을 선호합니다.

```kotlin
fun foo(): Int {     // bad
    return 1
}

fun foo() = 1        // good
```

## 표현식 본문(Expression bodies)

---

함수에 선언과 같은 줄에 첫 줄이 맞지 않는 표현식 본문이 있는 경우, 첫 줄에 `=` 기호를 넣고 표현식 본문을 네 칸 들여쓰기합니다.

```kotlin
fun f(x: String, y: String, z: String) =
    veryLongFunctionCallWithManyWords(andLongParametersToo(), x, y, z)
```

## 속성(Properties)

---

매우 간단한 읽기 전용 속성의 경우 한 줄 서식을 사용하는 것이 좋습니다:

```kotlin
val isEmpty: Boolean get() = size == 0
```

더 복잡한 속성의 경우 항상 `get` 키워드와 `set` 키워드를 별도의 줄에 배치하세요:

```kotlin
val foo: String
    get() { /*...*/ }
```

이니셜라이저가 있는 속성의 경우 이니셜라이저가 긴 경우 `=`기호 뒤에 줄 바꿈을 추가하고 이니셜라이저를 네 칸 들여쓰기합니다:

```kotlin
private val defaultCharset: Charset? =
    EncodingRegistry.getInstance().getDefaultCharsetForPropertiesFiles(file)
```

## 제어 흐름 문(Control flow statements)

---

`if` 또는 `when` 문의 조건이 여러 줄인 경우 항상 문 본문 주위에 중괄호를 사용합니다. 조건의 각 후속 줄을 문 시작 부분을 기준으로 4칸씩 들여쓰기합니다. 조건의 닫는 괄호를 여는 중괄호와 함께 별도의 줄에 배치합니다:

```kotlin
if (!component.isSyncing &&
    !hasAnyKotlinRuntimeInScope(module)
) {
    return createKotlinNotConfiguredPanel(module)
}
```

이렇게 하면 조건과 문 본문을 정렬하는 데 도움이 됩니다.

`else`, `catch`, `finally` 키워드와 `do-while` 루프의 `while` 키워드를 앞의 중괄호와 같은 줄에 배치합니다:

```kotlin
if (condition) {
    // body
} else {
    // else part
}

try {
    // body
} finally {
    // cleanup
}
```

`when` 문에서 분기가 한 줄 이상인 경우 인접한 대/소문자 블록과 빈 줄로 구분하는 것이 좋습니다:

```kotlin
private fun parsePropertyValue(propName: String, token: Token) {
    when (token) {
        is Token.ValueToken ->
            callback.visitValue(propName, token.value)

        Token.LBRACE -> { // ...
        }
    }
}
```

중괄호 없이 조건과 같은 줄에 짧은 가지를 놓습니다.

```kotlin
when (foo) {
    true -> bar() // good
    false -> { baz() } // bad
}
```

## 메서드 호출(Method calls)

---

긴 인수 목록에서는 여는 괄호 뒤에 줄 바꿈을 넣습니다. 인수를 네 칸씩 들여쓰기합니다. 서로 밀접하게 관련된 여러 인수를 같은 줄에 그룹화합니다.

```kotlin
drawSquare(
    x = 10, y = 10,
    width = 100, height = 100,
    fill = true
)
```

인수 이름과 값을 구분하는 `=` 기호 주위에 공백을 넣습니다.

## 래핑 체인 호출(**Wrap chained calls)**

---

연쇄 호출을 묶을 때는 다음 줄에 `.` 문자 또는 `?` 연산자를 한 번 들여쓰기하여 넣습니다:

```kotlin
val anchor = owner
    ?.firstChild!!
    .siblings(forward = true)
    .dropWhile { it is PsiComment || it is PsiWhiteSpace }
```

체인의 첫 번째 호출에는 일반적으로 줄 바꿈이 있어야 하지만 코드가 더 이해하기 쉽다면 생략해도 괜찮습니다.

## 람다(Lambdas)

---

람다 표현식에서 중괄호 주변과 매개변수와 본문을 구분하는 화살표 주변에는 공백을 사용해야 합니다. 호출이 하나의 람다를 사용하는 경우 가능하면 괄호 안에 람다를 전달하세요.

```kotlin
list.filter { it > 10 }
```

람다에 레이블을 할당하는 경우 레이블과 여는 중괄호 사이에 공백을 두지 마십시오:

```kotlin
fun foo() {
    ints.forEach lit@{
        // ...
    }
}
```

여러 줄의 람다로 매개변수 이름을 선언할 때는 첫 줄에 이름을 입력한 다음 화살표와 줄 바꿈으로 이름을 입력합니다:

```kotlin
appendCommaSeparated(properties) { prop ->
    val propertyValue = prop.get(obj)  // ...
}
```

매개변수 목록이 너무 길어 한 줄에 넣을 수 없는 경우 화살표를 별도의 줄에 표시합니다:

```kotlin
foo {
   context: Context,
   environment: Env
   ->
   context.configureEnv(environment)
}
```

## 후행 쉼표(Trailing commas)

---

후행 쉼표는 일련의 요소에서 마지막 항목 뒤에 오는 쉼표 기호입니다:

```kotlin
class Person(
    val firstName: String,
    val lastName: String,
    val age: Int, // trailing comma
)
```

후행 쉼표를 사용하면 몇 가지 이점이 있습니다:

- 변경된 값에만 초점을 맞추기 때문에 버전 제어 Diff를 더 깔끔하게 관리할 수 있습니다.
- 요소를 조작할 때 쉼표를 추가하거나 삭제할 필요가 없으므로 요소를 쉽게 추가하고 순서를 바꿀 수 있습니다.
- 예를 들어 객체 이니셜라이저의 코드 생성을 간소화합니다. 마지막 요소에는 쉼표를 넣을 수도 있습니다.

후행 쉼표는 전적으로 선택 사항이며, 쉼표가 없어도 코드가 계속 작동합니다. Kotlin 스타일 가이드에서는 선언 사이트에서 후행 쉼표 사용을 권장하며 호출 사이트에서는 사용자의 재량에 맡깁니다.

IntelliJ IDEA 서식 파일에서 후행 쉼표를 활성화하려면 설정/환경 설정 | 편집기 | 코드 스타일 | Kotlin으로 이동하여 기타 탭을 열고 후행 쉼표 사용 옵션을 선택합니다.

## 열거형(Enumeration)

---

```kotlin
enum class Direction {
    NORTH,
    SOUTH,
    WEST,
    EAST, // trailing comma
}
```

## 값 인수(Value arguments)

---

```kotlin
fun shift(x: Int, y: Int) { /*...*/ }
shift(
    25,
    20, // trailing comma
)
val colors = listOf(
    "red",
    "green",
    "blue", // trailing comma
)
```

## 클래스 속성 및 매개변수(Class properties and parameters)

---

```kotlin
class Customer(
    val name: String,
    val lastName: String, // trailing comma
)
class Customer(
    val name: String,
    lastName: String, // trailing comma
)
```

## 함수 값 매개변수(Function value parameters)

---

```kotlin
fun powerOf(
    number: Int,
    exponent: Int, // trailing comma
) { /*...*/ }
constructor(
    x: Comparable<Number>,
    y: Iterable<Number>, // trailing comma
) {}
fun print(
    vararg quantity: Int,
    description: String, // trailing comma
) {}
```

## 옵션 유형이 있는 매개변수(세터 포함)(Parameters with optional type(including setters)

---

```kotlin
val sum: (Int, Int, Int) -> Int = fun(
    x,
    y,
    z, // trailing comma
): Int {
    return x + y + x
}
println(sum(8, 8, 8))
```

## 인덱싱 접미사(Indexing suffix)

---

```kotlin
class Surface {
    operator fun get(x: Int, y: Int) = 2 * x + 4 * y - 10
}
fun getZValue(mySurface: Surface, xValue: Int, yValue: Int) =
    mySurface[
        xValue,
        yValue, // trailing comma
    ]
```

## 람다의 매개변수(Parameters in lambdas)

---

```kotlin
fun main() {
    val x = {
            x: Comparable<Number>,
            y: Iterable<Number>, // trailing comma
        ->
        println("1")
    }
    println(x)
}
```

## When entry

---

```kotlin
fun isReferenceApplicable(myReference: KClass<*>) = when (myReference) {
    Comparable::class,
    Iterable::class,
    String::class, // trailing comma
        -> true
    else -> false
}
```

## 컬렉션 리터럴(주석 내)(Collection literals)(in annotations)

---

```kotlin
annotation class ApplicableFor(val services: Array<String>)
@ApplicableFor([
    "serializer",
    "balancer",
    "database",
    "inMemoryCache", // trailing comma
])
fun run() {}
```

## 유형 인자(Type arguments)

---

```kotlin
fun <T1, T2> foo() {}
fun main() {
    foo<
            Comparable<Number>,
            Iterable<Number>, // trailing comma
            >()
}
```

## 유형 매개변수(Type parameters)

---

```kotlin
class MyMap<
        MyKey,
        MyValue, // trailing comma
        > {}
```

## 파괴 선언(Destructuring declarations)

---

```kotlin
data class Car(val manufacturer: String, val model: String, val year: Int)
val myCar = Car("Tesla", "Y", 2019)
val (
    manufacturer,
    model,
    year, // trailing comma
) = myCar
val cars = listOf<Car>()
fun printMeanValue() {
    var meanValue: Int = 0
    for ((
        _,
        _,
        year, // trailing comma
    ) in cars) {
        meanValue += year
    }
    println(meanValue/cars.size)
}
printMeanValue()
```

# 문서 설명(Documentation comments)

---

문서 설명이 길어질 경우 첫 줄의 `/**`를 별도의 줄에 배치하고 별표로 각 후속 줄을 시작합니다:

```kotlin
/**
 * This is a documentation comment
 * on multiple lines.
 */
```

짧은 댓글은 한 줄로 작성할 수 있습니다:

```kotlin
/** This is a short documentation comment. */
```

일반적으로 `@param` 및 `@return` 태그는 사용하지 마세요. 대신 매개변수와 반환 값에 대한 설명을 문서 댓글에 직접 통합하고 매개변수가 언급되는 곳에 링크를 추가하세요. 본문 흐름에 맞지 않는 긴 설명이 필요한 경우에만 `@param` 및 `@return`을 사용하세요.

```kotlin
 // Avoid doing this:

/**
 * Returns the absolute value of the given number.
 * @param number The number to return the absolute value for.
 * @return The absolute value.
 */
fun abs(number: Int): Int { /*...*/ }

// Do this instead:

/**
 * Returns the absolute value of the given [number].
 */
fun abs(number: Int): Int { /*...*/ }
```

# 중복 구조 피하기(Avoid redundant constructs)

---

일반적으로 Kotlin의 특정 구문 구조가 선택 사항이고 IDE에서 중복으로 강조 표시되는 경우 코드에서 해당 구문 구조를 생략해야 합니다. "명확성을 위해" 코드에 불필요한 구문 요소를 남겨 두지 마세요.

## 단위 반환 유형(Unit return type)

---

함수가 Unit을 반환하는 경우 반환 유형을 생략해야 합니다:

```kotlin
fun foo() { // ": Unit" is omitted here

}
```

## 세미콜론(Semicolons)

---

세미콜론은 가급적 생략합니다.

## 문자열 템플릿(String templates)

---

문자열 템플릿에 간단한 변수를 삽입할 때는 중괄호를 사용하지 마세요. 중괄호는 긴 표현식에만 사용하세요.

```kotlin
println("$name has ${children.size} children")
```

# 언어 기능의 관용적 사용(Idiomatic use of language features)

---

## 불변성(Immutability)

---

변경 가능한 데이터보다 변경 불가능한 데이터를 사용하는 것을 선호합니다. 로컬 변수와 프로퍼티를 초기화 후 수정하지 않는 경우 항상 `var`가 아닌 `val`로 선언하세요.

변경되지 않는 컬렉션을 선언하려면 항상 변경 불가능한 컬렉션 인터페이스(`Collection`, `List`, `Set`, `Map`)를 사용하세요. 팩토리 함수를 사용하여 컬렉션 인스턴스를 생성할 때는 가능하면 항상 변경 불가능한 컬렉션 유형을 반환하는 함수를 사용하세요:

```kotlin
// Bad: use of a mutable collection type for value which will not be mutated
fun validateValue(actualValue: String, allowedValues: HashSet<String>) { ... }

// Good: immutable collection type used instead
fun validateValue(actualValue: String, allowedValues: Set<String>) { ... }

// Bad: arrayListOf() returns ArrayList<T>, which is a mutable collection type
val allowedValues = arrayListOf("a", "b", "c")

// Good: listOf() returns List<T>
val allowedValues = listOf("a", "b", "c")
```

## 기본 파라미터 값(Default parameter values)

---

오버로드된 함수를 선언하는 것보다 기본 매개변수 값으로 함수를 선언하는 것을 선호합니다.

```kotlin
// Bad
fun foo() = foo("a")
fun foo(a: String) { /*...*/ }

// Good
fun foo(a: String = "a") { /*...*/ }
```

## 별칭 입력(Type aliases)

---

코드베이스에서 여러 번 사용되는 함수형 유형이나 유형 매개 변수가 있는 유형이 있는 경우 해당 유형에 대한 유형 별칭을 정의하는 것이 좋습니다:

```kotlin
typealias MouseClickHandler = (Any, MouseEvent) -> Unit
typealias PersonIndex = Map<String, Person>
```

이름 충돌을 피하기 위해 비공개 또는 내부 유형 별칭을 사용하는 경우 패키지 및 가져오기에 언급된 `import…as…`  사용하는 것이 좋습니다.

## 람다 매개변수(Lambda parameters)

---

길이가 짧고 중첩되지 않은 람다에서는 매개변수를 명시적으로 선언하는 대신 `it` 규칙을 사용하는 것이 좋습니다. 매개변수가 있는 중첩된 람다에서는 항상 매개변수를 명시적으로 선언하세요.

## 람다로 반환(Returns in a lambda)

---

람다에 여러 개의 레이블이 지정된 반환을 사용하지 마세요. 하나의 종료 지점을 갖도록 람다를 재구성하는 것이 좋습니다. 이것이 불가능하거나 명확하지 않은 경우 람다를 익명 함수로 변환하는 것을 고려하세요.

람다의 마지막 문에 레이블이 지정된 반환을 사용하지 마십시오.

## 명명된 인수(Named arguments)

---

메서드가 동일한 기본 유형의 매개변수를 여러 개 받는 경우 또는 모든 매개변수의 의미가 문맥에서 명확하지 않은 경우 `Boolean` 유형의 매개변수에는 명명된 인수 구문을 사용합니다.

```kotlin
drawSquare(x = 10, y = 10, width = 100, height = 100, fill = true)
```

## 조건문(Conditional statements)

---

`try`, `if`, `when`의 표현식 형식을 사용하는 것을 선호합니다.

```kotlin
return if (x) foo() else bar()
```

```kotlin
return when(x) {
    0 -> "zero"
    else -> "nonzero"
}
```

위와 같은 방법을 사용하는 것이 좋습니다:

```kotlin
if (x)
    return foo()
else
    return bar()
```

```kotlin
when(x) {
    0 -> return "zero"
    else -> return "nonzero"
}
```

## if 대 when(if versus when)

---

이진 조건에는 `when` 대신 `if`를 사용하는 것을 선호합니다. 예를 들어 이 구문을 `if`와 함께 사용합니다:

```kotlin
if (x == null) ... else ...
```

다음과 같은 경우를 포함하는 것(instead of this one with) `when`:

```kotlin
when (x) {
    null -> // ...
    else -> // ...
}
```

옵션이 3개 이상일 때 `when`사용하는 것을 선호합니다.

## 조건에서 무효화 가능한 부울 값(Nullable Boolean values in conditions)

---

조건문에서 널 가능 `Boolean`을 사용해야 하는 경우 `if (value == true)` 또는 `if (value == false)` 확인을 사용합니다.

## 루프(Loops)

---

루프보다는 고차 함수(`filter`, `map` 등)를 사용하는 것을 선호합니다. 예외: `forEach`(`forEach`의 수신자가 널 가능하거나 `forEach`가 더 긴 호출 체인의 일부로 사용되는 경우가 아니라면 대신 일반 `for` 루프를 사용하는 것이 좋습니다).

여러 고차 함수와 루프를 사용하는 복잡한 표현식 중에서 선택할 때는 각 경우에 수행되는 연산 비용을 이해하고 성능 고려 사항을 염두에 두어야 합니다.

## 범위의 루프(Loops on ranges)

---

`until` 함수를 사용하여 열린 범위에서 반복합니다:

```kotlin
for (i in 0..n - 1) { /*...*/ }  // bad
for (i in 0 until n) { /*...*/ }  // good
```

## 문자열(Strings)

---

문자열 연결보다 문자열 템플릿을 선호합니다.

일반 문자열 리터럴에 `\n` 이스케이프 시퀀스를 포함하는 것보다 여러 줄 문자열을 선호합니다.

여러 줄 문자열에서 들여쓰기를 유지하려면 결과 문자열에 내부 들여쓰기가 필요하지 않은 경우 `trimIndent`를 사용하고, 내부 들여쓰기가 필요한 경우 `trimMargin`을 사용합니다:

```kotlin
fun main() {
    println("""
    Not
    trimmed
    text
    """
           )

    println("""
    Trimmed
    text
    """.trimIndent()
           )

    println()

    val a = """Trimmed to margin text:
          |if(a > 1) {
          |    return a
          |}""".trimMargin()

    println(a)
}
```

Java와 Kotlin 다중 줄 문자열의 차이점을 알아보세요.

## 함수 대 속성(Functions vs properties)

---

경우에 따라 인수가 없는 함수를 읽기 전용 프로퍼티와 바꿔 사용할 수 있습니다. 의미는 비슷하지만 어느 것을 더 선호해야 하는지에 대한 몇 가지 문체 규칙이 있습니다.

기본 알고리즘이 함수일 때 함수보다 프로퍼티를 선호합니다:

- 던지지 않습니다.(does not throw)
- 계산 비용이 저렴합니다(또는 첫 번째 실행 시 캐시됨).(is cheap to calculate(or cached on the first run))
- 객체 상태가 변경되지 않은 경우 호출에 대해 동일한 결과를 반환합니다.(returns the same result over invocations if the object state hasn't changed)

## 확장 함수(Extension functions)

---

확장 함수를 자유롭게 사용하세요. 주로 객체에서 작동하는 함수가 있을 때마다 해당 객체를 수신자로 받아들이는 확장 함수로 만드는 것을 고려하세요. API 오염을 최소화하려면 확장 함수의 가시성을 최대한 제한하세요. 필요에 따라 로컬 확장 함수, 멤버 확장 함수 또는 비공개 가시성이 있는 최상위 확장 함수를 사용하세요.

## 중위 함수(Infix functions)

---

비슷한 역할을 하는 두 객체에서 작동하는 경우에만 함수를 `infix`로 선언하세요. 좋은 예: `and`, `to`, `zip`. 나쁜 예: `add`.

수신자 객체를 변경하는 경우 메서드를 `infix`로 선언하지 마십시오.

## 공장 함수(Factory functions)

---

클래스에 대한 팩토리 함수를 선언하는 경우 클래스 자체와 같은 이름을 지정하지 마세요. 팩토리 함수의 동작이 왜 특별한지 명확히 알 수 있도록 고유한 이름을 사용하는 것이 좋습니다. 특별한 의미가 없는 경우에만 클래스와 같은 이름을 사용할 수 있습니다.

```kotlin
class Point(val x: Double, val y: Double) {
    companion object {
        fun fromPolar(angle: Double, radius: Double) = Point(...)
    }
}
```

다른 수퍼클래스 생성자를 호출하지 않고 기본 인수 값을 사용하여 단일 생성자로 축소할 수 없는 여러 개의 오버로드된 생성자가 있는 객체가 있는 경우, 오버로드된 생성자를 팩토리 함수로 대체하는 것이 좋습니다.

## 플랫폼 유형(Platform types)

---

플랫폼 유형의 표현식을 반환하는 공용 함수/메서드는 해당 Kotlin 유형을 명시적으로 선언해야 합니다:

```kotlin
fun apiCall(): String = MyJavaApi.getProperty("name")
```

플랫폼 유형의 표현식으로 초기화된 모든 속성(패키지 수준 또는 클래스 수준)은 해당 Kotlin 유형을 명시적으로 선언해야 합니다:

```kotlin
class Person {
    val name: String = MyJavaApi.getProperty("name")
}
```

플랫폼 유형의 표현식으로 초기화된 로컬 값에는 유형 선언이 있을 수도 있고 없을 수도 있습니다:

```kotlin
fun main() {
    val name = MyJavaApi.getProperty("name")
    println(name)
}
```

## 범위 함수(scope functions) apply/with/run/also/let

---

Kotlin은 주어진 개체의 컨텍스트에서 코드 블록을 실행하기 위한 함수 집합인 `let`, `run`, `with`, `apply` ,`also` 등을 제공합니다. 사례에 적합한 범위 함수를 선택하는 방법에 대한 지침은 범위 함수를 참조하세요.

# 라이브러리 코딩 규칙(Coding conventions for libraries)

---

라이브러리를 작성할 때는 API 안정성을 보장하기 위해 추가 규칙을 따르는 것이 좋습니다:

- 항상 멤버 가시성을 명시적으로 지정하세요(실수로 선언이 공용 API로 노출되는 것을 방지하기 위해).
- 항상 함수 반환 유형과 속성 유형을 명시적으로 지정하세요(구현이 변경될 때 실수로 반환 유형이 변경되는 것을 방지하기 위해).
- 새 문서가 필요하지 않은 재정의(라이브러리용 문서 생성을 지원하기 위한)를 제외한 모든 공개 멤버에 대해 KDoc 주석을 제공합니다.

라이브러리 제작자 가이드라인에서 라이브러리용 API를 작성할 때 고려해야 할 모범 사례와 아이디어에 대해 자세히 알아보세요.

# Ref
---
[Kotlin-Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html#overload-layout)