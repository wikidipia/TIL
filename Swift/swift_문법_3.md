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