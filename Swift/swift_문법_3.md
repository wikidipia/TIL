# swift_문법_3

## 클래스와 구조체
***
클래스는 `class`로 정의하고 구조체는 `struct`로 정의힌다.  
클래스는 상속이 가능하지만 구조체는 상속이 불가능하다.
```swift
class Animal {
  let numberOfLegs = 4
}

class Dog: Animal {
  var name: String?
  var age: Int?
}

var myDog = Dog()
print(myDog.numberOfLegs) // Animal 클래스로부터 상속받은 값 (4)
```

클래스는 참조하고 구조체는 복사한다.
```swift
var dog1 = Dog()  // dog1은 새로 만들어진 Dog()를 참조합니다.
var dog2 = dog1   // dog2는 dog1이 참조하는 Dog()를 똑같이 참조합니다.
dog1.name = "찡코" // dog1의 이름을 바꾸면 Dog()의 이름이 바뀌기 때문에,
print(dog2.name)  // dog2의 이름을 가져와도 바뀐 이름("찡코")이 출력됩니다.

var coffee1 = Coffee()   // coffee1은 새로 만들어진 Coffee() 그 자체입니다.
var coffee2 = coffee1    // coffee2는 coffee1을 복사한 값 자체입니다.
coffee1.name = "아메리카노" // coffee1의 이름을 바꿔도
coffee2.name             // coffee2는 완전히 별개이기 때문에 이름이 바뀌지 않습니다. (nil)
```

## 생성자 (Initializer)
***
클래스와 구조체 모두 생성자를 가지고 있다. 생성자에서는 속성의 초기값을 지정할 수 있다.
```swift
class Dog {
  var name: String?
  var age: Int?

  init() {
    self.age = 0
  }
}

struct Coffee {
  var name: String?
  var size: String?

  init() {
    self.size = "Tall"
  }
}
```

만약 속성이 옵셔널이 아니라면 항상 초기값을 가져야 한다. 만약 옵셔널이 아닌 속성이 초기값이 없다면 컴파일 에러가 발생한다.
```swift
class Dog {
  var name: String?
  var age: Int // 컴파일 에러!
}
```

초기값을 지정해 줄 때는 속성을 정의할 때 지정해주는 방법과
```swift
class Dog {
  var name: String?
  var age: Int = 0 // 속성을 정의할 때 초깃값 지정
}
```

생성자에서 지정해주는 방법이 있다.
```swift
class Dog {
  var name: String?
  var age: Int

  init() {
    self.age = 0 // 생성자에서 초깃값 지정
  }
}
```

생성자도 함수와 마친가지로 파라미터를 받을 수 있다.
```swift
class Dog {
  var name: String?
  var age: Int

  init(name: String?, age: Int) {
    self.name = name
    self.age = age
  }
}

var myDog = Dog(name: "찡코", age: 3)
```

만약 상속받은 클래스라면 생성자에서 상위 클래스의 생성자를 호출해주어야 한다. 이때 생성자의 파라미터가 상위 클래스의 파라미터와 같다면 `override`를 붙여주어야 한다. `super.init()` 은 클래스 속성들의 초기값이 모두 설정 된 후에 해야 한다. 그러고 나서 `self` 를 사용할 수 있다.
```swift
class Dog: Animal {
  var name: String?
  var age: Int

  override init() {
    self.age = 0 // 초깃값 설정
    super.init() // 상위 클래스 생성자 호출
    print(self.simpleDescription()) // 여기서부터 `self` 접근 가능
  }

  func simpleDescription() -> String {
    if let name = self.name {
      return "\(name)"
    } else {
      return "No name"
    }
  }
}
```

해당 클래스가 메모레에서 해제되면 `deinit` 이 호출되고 이는 클래스에서만 사용 가능하다.
```swift
class Dog {
  // ...

  deinit {
    print("메모리에서 해제됨")
  }
}
```

### 속성 (Properties)
***
속성은 크게 두 가지로 나뉜다. 값을 가지는 속성과 계산되는 속성이다. 영어로는 Stored Property와 Computed Property이다. 쉽게 속성이 값 자체를 가지고 있는지 혹은 어떤 연산을 수행한 뒤 그 결과를 반환하는 지의 차이다.

지금까지 정의하고 사용한 속성들은 모두 Stored Property이다.  Computed Property는 `get`, `set`을 이용해서 정의할 수 있다. `set`에서는 새로 설정될 값을 `newValue`라는 예약어를 통해서 접근할 수 있다.

```swift
struct Hex {
  var decimal: Int?
  var hexString: String? {
    get {
      if let decimal = self.decimal {
        return String(decimal, radix: 16)
      } else {
        return nil
      }
    }
    set {
      if let newValue = newValue {
        self.decimal = Int(newValue, radix: 16)
      } else {
        self.decimal = nil
      }
    }
  }
}

var hex = Hex()
hex.decimal = 10
hex.hexString // "a"

hex.hexString = "b"
hex.decimal // Optional(11)
```

위에서 `hexString`은 실제 값을 가지고 있지 않지만 `decimal`로 값을 받아와 16진수 문자열로 만들어서 반환한다.  
참고로 `get`만 정의할 경우 `get`을 생략할 수 있는데 이런 속성을 읽기 전용 (**Read Only**) 이라고 한다.

>### Getter와 Setter
> Getter와 Setter를 사용하는 이유는 객체의 데이터를 객체 외부에서 직접적으로 접근하게 되면 객체의 무결성이 깨질 수 있기 때문에 **메소드를 통해 데이터를 변경** 하는 방법을 선호한다.  
**외부에서 메소드를 통해서 데이터에 접근하도록 유도**하는 역활을 하는 것이 **Setter**이다.  
**외부에서 객체의 데이터를 읽을 때 메소드를 사용하는 것**이 **Getter**이다.

`get`, `set`과 비슷한 `willSet`, `didSet`이라는 것이 있는데 이를 프로퍼티 감시자 (**Property Observers**) 라고 한다. 오직 Stored Property에만 적용할 수 있으며 **프로퍼티 값이 새로 할당될 때마다 호출되고 변경되는 값이 현재의 값과 같더라도 호출**된다. 

`willSet`은 값이 변경되기 직전에 호출되고 `didSet`은 값이 변경된 직후에 호출된다. 이떄 `willSet`에는 `newValue`가 `didSet`에는 `oldValue`가 매개변수로 자동 저장된다.
```swift
struct Hex {
  var decimal: Int? {
    willSet {
      print("\(self.decimal)에서 \(newValue)로 값이 바뀔 예정입니다.")
    }
    didSet {
      print("\(oldValue)에서 \(self.decimal)로 값이 바뀌었습니다.")
    }
  }
}
```

일반적으로 어떤 속성의 값이 바뀌었을 때 UI를 업데이트하거나 특정 메소드를 호출하는 등의 역활을 할 때 주로 사용한다.

### 참고
***
[40시간 만에 Swift로 IOS 앱 만들기](https://devxoul.gitbooks.io/ios-with-swift-in-40-hours/content/Chapter-3/tuples.html)  
[Java 자바 - Getter와 Setter 메소드](https://kephilab.tistory.com/54)  
[[Swift]프로퍼티(Property)](https://jinshine.github.io/2018/05/22/Swift/6.%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0(Property)/)