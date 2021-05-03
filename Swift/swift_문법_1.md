# swift문법_1

## 변수와 상수
변수는 `var`로 선언하고 상수는 `let`으로 선언한다.

swift는 정적 타이핑 언어다. 변수나 상수를 정의할 때 그 자료형이 어떤 것인지를 명시해야 하는 언어를 말한다.
예를 들면 

```swift
var name: String = "Kim"
let year: Int = 2021
var langitude: Float = 13.14
```

이떄 `: String`이나 `: Int` 등을 가지고 타입 어노테이션 이라고 하며 Swift에서는 타입 추론이 가능하기 때문에 이를 생략이 가능하다.

Swift에서는 타입을 업격하게 다루기 때문에 다른 자료형끼리는 기본적인 연산조차 되지 않는다. 따라서
```swift
year + langitude
```
와 같은 연산을 하게되면 컴파일 에러가 발생한다.

이럴 때는 다음과 같이 사용해야 한다.
```swift
Float(year) + langitude
```
이를 응용해서 다음과 같이 사용할 수 있다.
```swift
String(year) + "년에 태어난 " + name + "학생"
```
더 간단하게 작성하자면 
```swift
"\(yaer)년에 태어난 \(name)학생"
```

***
## 배열과 딕셔너리

배열과 딕셔너리 모두 대괄호를 이용해서 정의할 수 있다.
```swift
var languages = ["Swift","Python","Java"]
var capitals = [
    "한국": "서울"
    "일본": "도쿄"
    "중국": "베이징"
]
```

배열과 딕셔너리에 접근하거나 값을 변경할 때도 대괄호를 사용한다.
```Swift
languages[0] //Swift
languages[1] = "Kotlin"

capitals["한국"] //서울
capitals["프랑스"] = "파리"
```

다른 상수와 마찬가지로 `let`으로 정의할 수 있으며 이때 값을 추가하거나 수정할 수 없다.

빈 배열이나 빈 딕셔너리를 정의하고 싶다면 다음과 같이 선언한다.
```swift
var langitude:[String] = []
var capitals:[String: String] = [:]
```
혹은 더 간결하게 하고 싶다면
```Swift
var langitude = [String]()
var capitals = [String: String]()
```
이때 타입 뒤에 괄호를 쓰는 것은 생성자를 호출하는 것이다.
***
## 조건문과 반복문

조건을 검사할 때는 `if`,`switch`를 사용한다.
```swift
var score = 30
var result = ""

if score >= 80 {
    result = "A"
} else if score >= 50 {
    result = "B"
} else {
    result = "F"
}

result // "F"
```
`if`문의 조건절에는 `Bool`타입을 사용해야 한다. Swift는 타입에 엄격하기 때문에 다른 언어에서 사용가능한 다음과 같은 코드는 사용하지 못한다.
``` swift
var num = 0
if !num {
    // ...
}
```
대신 이렇게 바꿔서 작성해야 된다.
```swift
if num == 0 {
    //...
}
```
빈 문자열이나 배열 등을 검사할 때도 python같은 언어는 빈 문자열을 `False`로 판단하지만 swift는 검사해주어야 한다.
```swift
if name.isEmpty(){...}
if langitudes.isEmpty(){...}
```
</br>
다른 언어를 사용해봤다면 `switch`구문이 단순히 값이 같은지 만을 검사하는 형식으로 사용된다는 것을 알고 있을 것이다. swift의 `switch` 구문은 이와 다르게 패턴 매칭이 가능한다.

```swift
switch age {
case 8..<14:
  student = "초등학생"
case 14..<17:
  student = "중학생"
case 17..<20:
  student = "고등학생"
default:
  student = "기타"
}
```

`8..<14`와 같이 범위 안에 `age`가 포험되었는지 여부를 검사할 수 있다.
</br>

반복되는 연산을 사용할 때는 `for`,`while`을 사용한다. `for`구문은 python의 리스트나 딕셔너리를 사용할 때와 유사하게 사용한다.
```swift
for language in languages {
  print("저는 \(language) 언어를 다룰 수 있습니다.")
}

for (country, capital) in capitals {
  print("\(country)의 수도는 \(capital)입니다.")
}
```
단순히 반복하는 반복문을 만들고 싶을 때는 Range를 사용하면 된다.
```swift
for i in 0..<100 {
    i
}
```
만약 `i`를 사용하고 싶지 않으면 `_`를 사용하여 대신할 수 있다.
```swift
for _ in 0..<100 {
    print("Hello!")
}
```
`_`는 어디서나 변수 이름을 대신해서 사용할 수 있음으로 알아두면 유용하게 사용할 수 있다.  
`while`은 조건문의 값이 `true`일 때 계속 반복된다.
```swift
var i = 0
while i < 100 {
    i += 1
} 
```

## 참고
[40시간만에 Switf로 IOS 앱 만들기](https://devxoul.gitbooks.io/ios-with-swift-in-40-hours/content/Chapter-2/control-flow.html)