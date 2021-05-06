# swift_문법_4

## 튜플 (Tuple)
***
튜블은 어떠한 값들의 묶음이다. 배열과 비슷하지만 배열과 다르게 길이가 고정되어 있다. 값을 접근할 때에도 `[]` 대신에 `.` 을 사용한다.
```swift
var coffeeInfo = ("아메리카노", 5100)
coffeeInfo.0 // 아메리카노
coffeeInfo.1 // 5100
coffeeInfo.1 = 5100
```

튜플의 파라미터에는 이름도 붙일 수 있다.
```swift
var namedCoffeeInfo = (coffee: "아메리카노", price: 5100)
namedCoffeeInfo.coffee // 아메리카노
namedCoffeeInfo.price // 5100
namedCoffeeInfo.price = 5100
```

구조체와 비슷해서 간단한 자료형을 만들 때는 구조체 대신 튜플을 이용하기도 한다.

튜플의 타입 어노테이션은 다음과 같이 생겼다.
```swift
var coffeeInfo: (String, Int)
var namedCoffeeInfo: (coffee: String, price: Int)
```

튜플을 응용하면 다음과 같이 여러 변수에 값을 지정할 수도 있다.
```swift
let (coffee, price) = ("아메리카노", 5100)
coffee // 아메리카노
price // 5100
```

튜플이 가진 값을 가지고 변수에 값을 지정할 때, 무시하고 싶은 값이 있다면 `_` 를 사용할 수 있다.
```swift
let (_, latteSize, lattePrice) = ("라떼", "Venti", 5600)
latteSize // Venti
lattePrice // 5600
```

튜플을 반환하는 함수 또한 만들수 있다.
```swift
/// 커피 이름에 맞는 커피 가격 정보를 반환합니다. 일치하는 커피 이름이 없다면 `nil`을 반환합니다.
///
/// - parameter name: 커피 이름
///
/// - returns: 커피 이름과 가격 정보로 구성된 튜플을 반환합니다.
func coffeeInfo(for name: String) -> (name: String, price: Int)? {
  let coffeeInfoList: [(name: String, price: Int)] = [
    ("아메리카노", 5100),
    ("라떼", 5600),
  ]
  for coffeeInfo in coffeeInfoList {
    if coffeeInfo.name == name {
      return coffeeInfo
    }
  }
  return nil
}

coffeeInfo(for: "아메리카노")?.price // 5100
coffeeInfo(for: "에스프레소")?.price // nil

let (_, lattePrice) = coffeeInfo(for: "라떼")!
lattePrice // 5600
```

## Enum
***
열거라는 뜻을 가진 Enumeration에서 따온 용어이다. 한글로 번역할 때는 **열거형**이라고 한다.
