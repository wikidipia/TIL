# swift_문법_2

## 옵서널 (Optional)

</br>

### 옵셔널

`""`이라는 문자열은 빈 값을 뜻한다.  
하지만 값이 없음은 `nil`이라는 값인데 다른 언와 다르게 모든 변수에 `nil`값이 들어갈 수 있는 것이 아니다.
```swift
var name: String = "전수열"
name = nil // 컴파일 에러
```
</br>

이런 상황에서 옵셔널을 사용한다. '선택적인' 이라는 단어의 뜻대로 값이 있을 수도 없을 수도 있음을 뜻한다. 사용할 떄는 타입 어노테이션에 `?`를 붙여서 사용한다.
```swift
var email: String?
print(email) // nil

email = "devxoul@gmail.com"
print(email) //Optional("devxoul@gmail.com")
```
</br>

옵셔널로 정의한 변수는 다른 변수와 다르기 때문에 다음과 같이 사용할 수 없다.

```swift
let optionalEmail: String? = "devxoul@gmail.com"
let requiredEmail: String = optionalEmail // 컴파일 에러
```
`requiredEmail`의 경우 항상 값을 가지고 있어야만 하지만 `optionalEmail`의 경우 값을 가지고 있는지 없는지 알 수 없기 때문에 대입할 수 없게 되어있다.

</br>

### 옵서녈 바인딩 (Optional Binding)
***
옵셔널 값을 가져오고 싶을 때 사용하는 것이 옵셔널 바인딩이다. 옵셔널 바인딩은 값이 존재하는지 검사한 뒤, 존재한다면 그 값을 다른 변수에 대입시켜준다.  
`if let` 혹은 `if var` 을 사용하는데 값이 있으면 `if` 문 안에 들어가게 되고 값이 `nil` 이라면 그냥 통과하게 된다.

```swift
if let email = optionalEmail {
  print(email) //값이 존재한다면 해당 값이 출력
}
//값이 존재하지 않는다면 pass
```

</br>

하나의 `if` 문에서 콤마로 구분하여 여러 옵셔널을 바인딩할 수 있다. 이때 바인딩한 모든 옵셔널 값이 존재해야 `if` 문으로 진입한다.

```swift
var optionalName: String? = "전수열"
var optionalEmail: String? = "devxoul@gmail.com"

if let name = optionalName, email = optionalEmail {
  // name과 email 값이 존재
}
```

>코드가 너무 길 경우, 줄을 나눠서 사용하는 것이 좋다.
>```swift
>if let name = optionalName,
>  let email = optionalEmail {
>  // name과 email 값이 존재
>}
>```
> 두 번재 `let`부터는 생략이 가능하다.

</br>

콤마를 사용해서 조건도 같이 지정할 수 있다. 콤마 이후의 조건절은 바인딩이 일어난 후 실행된다.
```swift
var optionalAge: Int? = 22

if let age = optionalAge, age >= 20 {
  // age의 값이 존재하고, 20 이상입니다.
}
```
</br>

### 옵셔널 체이닝 (Optional Chaining)
***

옵셔널 채이닝은 옵셔널의 속성에 접근할 때 옵셔널 바인딩 과정을 `?` 키워드로 줄여주는 역활을 한다. 다음과 같이 '빈 배열' 인지 검사하는 코드가 있다고 생각해보자.
```swift
let array: [String]? = []
var isEmptyArray = false

if let array = array, array.isEmpty {
  isEmptyArray = true
} else {
  isEmptyArray = false
}

isEmptyArray
```

옵셔널 체이닝을 이용하면 다음과 같이 더 간결하게 쓸 수 있다.
```swift
let isEmptyArray = array?.isEmpty == true
```

여기서 3가지의 경우를 생각해 볼 수 있다.
* `array`가 `nil`인 경우
    ```swift
      array?.isEmpty
      ~~~~~~
      여기까지 실행되고 `nil`을 반환

    ```
* `array`가 빈 배열인 경우
    ```swift
    array?.isEmpty
    ~~~~~~~~~~~~~~
    여기까지 실행되고 `true`를 반환
    ```
* `array`에 요소가 있는 경우
    ```swift
    array?.isEmpty
    ~~~~~~~~~~~~~~
    여기까지 실행되고 `false`를 반환
    ```
이때 `isEmpty` 의 반환값은 `Bool`인데 옵셔널 체이닝으로 인해 `Bool?` 을 반환하도록 바뀐 것이다. 그래서 실재로 `true` 인지 확인하기 위해서 `==true` 를 사용해야 한다.
</br>

### 옵셔널 벗기기
***
옵셔널을 사용할 때마다 옵셔널 바인딩을 하는 것이 가장 좋다. 하지만 개발 하다보면 값이 반드시 존재할 것이지만 옵셔널로 사용해야 하는 경우가 발생한다. 이럴 때에 `!` 을 사용해서 옵셔널에 값이 있다고 가정하고 값에 바로 접근할 수 있다.
```swift
print(optionalEmail) // Optional("devxoul@gmail.com")
print(optionalEmail!) // devxoul@gmail.com
```
</br>
이때 주의할 점이 있는데 옵셔널의 값이 `nil` 인 경우에 런타임 에러가 발생한다. 이는 Java의 NullPointException과 비슷하다.

```swift
var optionalEmail: String?
print(optionalEmail!) // 런타임 에러
```
런타임 에러가 발생하게 되면 앱이 강제로 종료될 수 있으니 굉장히 조심해야 한다.

</br>

### 암묵적으로 벗겨진 옵셔널 (Implicity Unwrapped Optional)
***
옵셔널을 정의할 때 `?` 대신 `!` 를 사용하면 `ImplicityUnwrappedOptional` 이라는 옵셔널로 정의된다.
```swift
var email: String! = "devxoul@gmail.com"
print(email) // Optional(devxoul@gmail.com)
```
이때 옵셔널 벗기기와 마친가지로 값이 없는데 접근을 시도하면 런타임 에러가 발생한다. 따라서 가급적이면 일반적인 옵셔널을 사용해서 정의하고, 옵셔널 바인딩이나 옵셔널 체이닝을 통해 값에 접근하는 것이 더 좋다.

## 함수와 클로저

</br>

### 함수
***
함수는 `func` 를 사용해서 정의하며 `->` 를 사용해서 함수의 변환 타입을 지정한다.
```swift
func hello(name: String, time: Int) -> String {
  var string = ""
  for _ in 0..<time {
    string += "\(name)님 안녕하세요!\n"
  }
  return string
}
```
Swift에서는 독특하게 함수를 호출할 때 파라미터의 이름을 함께 써주어야 한다.
```swift
hello(name: "전수열", time: 3)
```

만약 함수를 호출할 때 사용하는 파라미터 이름과 함수 내부에서 사용하는 파라미터 이름을 다르게 사용하고 싶으면 다음과 같이 사용할 수 있다.
```swift
func hello(to name: String, numberOfTimes time: Int) {
  // 함수 내부에서는 `name`과 `time`을 사용
  for _ in 0..<time {
    print(name)
  }
}

hello(to: "전수열", numberOfTimes: 3) // 이곳에서는 `to`와 `numberOfTimes`를 사용
```

파라미터의 이름을 `_` 로 정의하면 함수를 호출할 때 파라미터 이름을 생략할 수 있다.

```swift
func hello(_ name: String, time: Int) {
  // ...
}

hello("전수열", time: 3) // 'name:' 이 생략
```

파라미터의 기본 값을 지정할 수도 있다. 기본 값이 정해진 파라미터는 함수 호출시 생략할 수 있다.
```swift
func hello(name: String, time: Int = 1) {
  // ...
}

hello("전수열")
```
`...`을 사용하면 개수가 정해지지 않는 파라미터를 받을 수 있다.
```swift
func sum(_ numbers: Int...) -> Int {
  var sum = 0
  for number in numbers {
    sum += number
  }
  return sum
}

sum(1, 2)
sum(3, 4, 5)
```

함수 안에 함수를 또 작성할 수도 있다.
```swift
func hello(name: String, time: Int) {
  func message(name: String) -> String {
    return "\(name)님 안녕하세요!"
  }

  for _ in 0..<time {
    print(message(name: name))
  }
}
```

심지어 함수 안에 정의된 함수를 반환할 수 있다.
```swift
func helloGenerator(message: String) -> (String) -> String {
  func hello(name: String) -> String {
    return name + message
  }
  return hello
}

let hello = helloGenerator(message: "님 안녕하세요!")
hello("전수열")
```

여기서 중요한 점은 `helloGenerator()` 함수의 변환 타입이 `(String) -> String` 이라는 것이다. `helloGenerator()` 는 '문자열을 받아서 문자열을 반환하는 함수'를 반환하는 함수인 것이다.

만약 `helloGenerator()` 안에 정의한 `hello()` 함수가 여러 개의 파라미터를 받는 다면 다음과 같이 써야 한다.

```swift
func helloGenerator(message: String) -> (String, String) -> String {
  func hello(firstName: String, lastName: String) -> String {
    return lastName + firstName + message
  }
  return hello
}

let hello = helloGenerator(message: "님 안녕하세요!")
hello("수열", "전")
```
문자열을 두 개 받아서 문자열을 반환한다는 의미이다.

</br>

### 클로저 (Closure)
***

클로저를 상요하면 코드를 조금 더 간결하게 만들 수 있다. 클로저란 중괄호로 감싸진 '실행 가능한 코드 블럭' 이다. 다른 언어에 있는 람다와 유사하다.

```swift
func helloGenerator(message: String) -> (String, String) -> String {
  return { (firstName: String, lastName: String) -> String in
    return lastName + firstName + message
  }
}
```

함수와 다르게 함수 이름 정의가 따로 존재하지 않는다. 하지만 파라미터도 받을 수 있고 반환 값도 존재한다는 점에서 함수와 동일하다.

함수와 다른 점은 `in` 을 사용해서 파라미터, 반환 타입 영역과 실제 클로저 코드를 분리하고 있다.

```swift
{ (firstName: String, lastName: String) -> String in
  return lastName + firstName + message
}
```

클로저의 장점은 간결함과 유연함에 있다. 하지만 위의 코드는 간결하다는 느낌을 받기 힘든데 이는 생략가능한 것을 생략하지 않고 모두 적었기 때문이다.

우선 타입 추론 덕분에 반환하는 타입을 생략할 수 있다.
```swift
func helloGenerator(message: String) -> (String, String) -> String {
  return { firstName, lastName in
    return lastName + firstName + message
  }
}
```

여기에서 첫 번쨰 파라미터는 `$0` 두 번째 파라미터는 `$1`로 바꿔 쓸 수 있다.
```swift
func helloGenerator(message: String) -> (String, String) -> String {
  return {
    return $1 + $0 + message
  }
}
```

또한 클로저 내부에 코드가 한 줄이라면 `return`까지 생략할 수 있다.
```swift
func helloGenerator(message: String) -> (String, String) -> String {
  return { $1 + $0 + message }
}
```

이렇게 단 한줄의 코드로 줄일 수 있다.

클로저는 변수처럼 졍의할 수 있다.
```swift
let hello: (String, String) -> String = { $1 + $0 + "님 안녕하세요!" }
hello("수열", "전")
```

옵셔널로도 정의할 수 있고 옵셔널 체이닝도 가능한다.
```swift
let hello: ((String, String) -> String)?
hello?("수열", "전")
```

클로저를 변수로 정의하고 함수에서 반환할 수 있는 것처럼 파라미터로 받을 수도 있다.
```swift
func manipulate(number: Int, using block: (Int) -> Int) -> Int {
  return block(number)
}

manipulate(number: 10, using: { (number: Int) -> Int in
  return number * 2
})
```

아까 했던 것처럼 생략도 가능하다.
```swift
manipulate(number: 10, using: {
  $0 * 2
})
```

만약 함수의 마지막 파라미터가 클로저라면 괄호와 파라미터 이름까지 생략할 수 있다.
```swift
manipulate(number: 10) {
  $0 * 2
}
```

### 클로저 활용하기
***
이런 구조로 만들어진 예가 `sorted()` 와 `filter()`이다. 함수가 피리미터로 클로저 하나만을 받는다면 괄호를 아예 생략해도 된다.
```swift
let numbers = [1, 3, 2, 6, 7, 5, 8, 4]

let sortedNumbers = numbers.sorted { $0 < $1 }
print(sortedNumbers) // [1, 2, 3, 4, 5, 6, 7, 8]

let evens = numbers.filter { $0 % 2 == 0 }
print(evens) // [2, 6, 8, 4]
```

```swift
let sortedNumbers = numbers.sorted(by: {s1, s2 in return s1 > s2})
``` 

에서 생략한 것이다. 

다른 예시로 `map()`과 `reduce()`가 있다.  
`map()`은 파라미터로 받은 클로저를 모든 요소에 실행하고 그 결과를 반환한다.
```swift
let arr1 = [1, 3, 6, 2, 7, 9]
let arr2 = arr1.map { $0 * 2 } // [2, 6, 12, 4, 14, 18]
```

`reduce()`는 초기값이 주어자고 초기값과 첫 번째 요소의 실행 결과, 그 결과와 두 번째 요소의 실행 결과, ... 를 반복하여 끝까지 실행한 후의 값을 반환한다.
```swift
arr1.reduce(0) { $0 + $1 } // 28
```