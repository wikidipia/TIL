## Null

* 코틀린의 변수 선언은 기본적으로 null을 허용하지 않는다.
```kotlin
var str1 : String = null // 불가능
```
* null 가능한 선언이 있다.

```kotlin
val a : Int? = null
vla b : String? = null
```

* 이때 널 가능성 검사를 해주어야 하는데 단순 출력에서는 상관없으나 널인 상태에서 연산되는 멤버에 접근하게 되면 NPE(Null Point Exception)이 발생한다.
* Swift에서 사용한 옵셔널과 비슷하다고 생각하면 된다.
* 이때 `?`사용을 Safe-call이라고 한다.
* kotlin에서는 `str!!.length` 처럼 non-null 단정 기호가 `!!`이다.

### null을 사용한 예시

```Kotlin
var len1 = if (str1 != null) str1.length else -1 // kotlin은 한 줄로 if문을 표현 가능

var len2 = str1?.length ? : -1 // 위의 if문과 같은 표현, 엘비스 연산자
```

엘비스 연산자를 이용한 방법이 더 안전하고 간략한 방법이다.