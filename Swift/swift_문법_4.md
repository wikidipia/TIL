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
열거라는 뜻을 가진 Enumeration에서 따온 용어이다. 한글로 번역할 때는 **열거형**이라고 한다. Enum은 다음과 같이 정의해서 사용할 수 있다.
```swift
enum Weekday {
    case mon
    case tue
    case wed
    case thu, fri, sat, sun
}

// 열거형 타입과 케이스를 모두 사용하여도 됩니다
var day: Weekday = Weekday.mon

// 타입이 명확하다면 .케이스 처럼 표현해도 무방합니다
day = .tue

print(day) // tue

// switch의 비교값에 열거형 타입이 위치할 때
// 모든 열거형 케이스를 포함한다면
// default를 작성할 필요가 없습니다
switch day {
case .mon, .tue, .wed, .thu:
    print("평일입니다")
case Weekday.fri:
    print("불금 파티!!")
case .sat, .sun:
    print("신나는 주말!!")
}
// 평일입니다
```

이때 Enum은 원시값(rawValue)을 가질 수 있는데 이때 case별로 각각 다른 값을 가져야한다.
```swift
enum Fruit: Int {
    case apple = 0
    case grape = 1
    case peach
    
    // mango와 apple의 원시값이 같으므로 
    // mango 케이스의 원시값을 0으로 정의할 수 없습니다
//    case mango = 0
}

print("Fruit.peach.rawValue == \(Fruit.peach.rawValue)")
// Fruit.peach.rawValue == 2
```

또한 swift에서는 다른 언어와 다른게 원시값에 `Int`형이 아닌 다른 모든 타입으로 원시값을 지정할 수 있다.
```swift
enum School: String {
    case elementary = "초등"
    case middle = "중등"
    case high = "고등"
    case university
}

print("School.middle.rawValue == \(School.middle.rawValue)")
// School.middle.rawValue == 중등

// 열거형의 원시값 타입이 String일 때, 원시값이 지정되지 않았다면
// case의 이름을 원시값으로 사용합니다
print("School.university.rawValue == \(School.university.rawValue)")
// School.middle.rawValue == university
```

이때 반대로 원시값을 통해 인스턴스를 초기화할 수 있다. 이때 원시값이 case에 존재하지 않을 수 있기 때문에 원시값을 통해 초기화한 인스턴스는 옵셔널 타입이다.
```swift
// rawValue를 통해 초기화 한 열거형 값은 옵셔널 타입이므로 Fruit 타입이 아닙니다
//let apple: Fruit = Fruit(rawValue: 0)
let apple: Fruit? = Fruit(rawValue: 0)

// if let 구문을 사용하면 rawValue에 해당하는 케이스를 곧바로 사용할 수 있습니다
if let orange: Fruit = Fruit(rawValue: 5) {
    print("rawValue 5에 해당하는 케이스는 \(orange)입니다")
} else {
    print("rawValue 5에 해당하는 케이스가 없습니다")
} // rawValue 5에 해당하는 케이스가 없습니다

```

또한 개별 열거값을 switch문과 일치시킬 수 있다.
```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Prints "Watch out for penguins"
```

이때 열거값에 존재하지 않는 경우 `default`로 예외처리하여 이용할 수 있다.
```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// Prints "Mostly harmless"
```

## 프로토콜 (Protocol)
***
구조체, 클래스, 열겨형은 프로토콜을 채택해서 특정 기능을 실행하기 위한 프로토콜의 요구사항을 실제로 구현할 수 있다. 이때 프로토콜은 정의를 하고 제시를 할 뿐 기능을 구현하지는 않는다.  
프로토콜에서 Property는 Stored 인지 computed 인지 명시하지 않고 이름, 타입 그리고 gettable,settable 한지 명시한다. 이때 Property는 항상 `var`로 선언해야 된다.
```swift
/// 전송가능한 인터페이스를 정의합니다.
protocol Sendable {
  var from: String? { get }
  var to: String { get }

  func send()
}
```

프로토콜을 적용하면 프로토콜에서 정의한 속성과 메서드를 모두 구현해야 된다.
```swift
struct Mail: Sendable {
  var from: String?
  var to: String

  func send() {
    print("Send a mail from \(self.from) to \(self.to)")
  }
}

struct Feedback: Sendable {
  var from: String? {
    return nil // 피드백은 무조건 익명으로 보냄
  }
  var to: String

  func send() {
    print("Send a feedback to \(self.to)")
  }
}
```

## 참조
***
[The swift programming language
swift 5.4](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)  
[오늘의 Swift 상식 (Protocol)](https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-protocol-f18c82571dad)