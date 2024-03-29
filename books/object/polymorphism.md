# 다형성

[오브젝트](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791158391409&orderClick=LAG&Kc=)(조영호 저)의 12단원을 요약 정리한 내용입니다.

---

다형성(polymorphism, polys "many" + morphē "shape, form")은 여러 형태를 가질 수 있는 능력을 의미한다. 컴퓨터 과학에서는 다형성을 하나의 추상 인터페이스에 대해 코드를 작성하고 이 추상 인터페이스에 대해 서로 다른 구현을 연결할 수 있는 능력으로 정의한다(자바에서는 **한 타입의 참조변수로 여러 타입의 객체를 참조**할 수 있도록 함으로써 다형성을 프로그램적으로 구현하였다). 간단하게 말해서, **다형성은 여러 타입을 대상으로 동작할 수 있는 코드를 작성할 수 있는 방법**이라고 할 수 있다.

객체지향 프로그래밍에서 사용되는 다형성은 유니버설(Universal) 다형성과 임시(Ad Hoc) 다형성으로 분류할 수 있다. 유니버설 다형성은 다시 **매개변수(Parametric) 다형성**과 **포함(Inclusion) 다형성**으로 분류할 수 있고, 임시 다형성은 **오버로딩(Overloading) 다형성**과 **강제(Coercion) 다형성**으로 분류할 수 있다.

![https://res.cloudinary.com/practicaldev/image/fetch/s--qGNcgju4--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0ylxhoi8zvmmfif6yspq.png](https://res.cloudinary.com/practicaldev/image/fetch/s--qGNcgju4--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0ylxhoi8zvmmfif6yspq.png)

`매개변수 다형성`은 제네릭 프로그래밍과 관련이 높은데 클래스의 인스턴스 변수나 메서드의 매개변수 타입을 임의의 타입으로 선언한 후 사용하는 시점에 구체적인 타입으로 지정하는 방식을 가리킨다.

**`포함 다형성`은 메시지가 동일하더라도 수신한 객체의 타입에 따라 실제로 수행되는 행동이 달라지는 능력**을 의미한다. 포함 다형성은 **서브타입(Subtype) 다형성**이라고도 부른다. 포함 다형성은 객체지향 프로그래밍에서 가장 널리 알려진 형태의 다형성이기 때문에 특별한 언급 없이 다형성이라고 할 때는 포함 다형성을 의미하는 것이 일반적이다.

`오버로딩 다형성`은 하나의 클래스 안에 동일한 이름의 메서드가 존재하는 경우를 가리킨다.

`강제 다형성`은 언어가 지원하는 자동적인 타입 변환이나 사용자가 직접 구현한 타입 변환을 이용해 동일한 연산자를 다양한 타입에 사용할 수 있는 방식을 가리킨다. 예를 들어 자바에서 이항 연산자인 `+`는 피연산자가 모두 정수일 경우에는 정수에 대한 덧셈 연산자로 동작하지만 하나는 정수형이고 다른 하나는 문자열인 경우에는 연결 연산자로 동작한다.

상속의 진정한 목적은 코드 재사용이 아니라 다형성을 위한 서브타입 계층을 구축하는 것이다.

## 상속의 양면성

객체지향 패러다임의 근간을 이루는 아이디어는 데이터와 행동을 객체라고 불리는 하나의 실행 단위 안으로 통합하는 것이다. 따라서 객체지향 프로그램을 작성하기 위해서는 항상 데이터와 행동이라는 두 가지 관점을 함께 고려해야 한다.

상속 역시 예외는 아니다. 상속을 이용하면 부모 클래스에서 정의한 모든 데이터를 자식 클래스의 인스턴스에 자동으로 포함시킬 수 있다. 이것이 데이터 관점의 상속이다. 데이터뿐만 아니라 부모 클래스에서 정의한 일부 메서드 역시 자동으로 자식 클래스에 포함시킬 수 있다. 이것이 행동 관점의 상속이다. **단순히 데이터와 행동의 관점에서만 바로보면 상속이란 부모 클래스에서 정의한 데이터와 행동을 자식 클래스에서 자동적으로 공유할 수 있는 재사용 매커니즘으로 보일 것이다.** 하지만 이 관점은 상속을 오해한 것이다.

**상속의 목적은 코드 재사용이 아니다. 상속은 프로그램을 구성하는 개념들을 기반으로 다형성을 가능하게 하는 타입 계층을 구축하기 위한 것이다.**

**상속의 메커니즘**을 이해하는 데 필요한 몇 가지 개념을 나열하면 아래와 같다.

- 업캐스팅
- 동적바인딩
- 동적 메서드 탐색
- self 참조
- super 참조

이 개념들을 이해하고 나면 상속의 내부 메커니즘뿐만 아니라 타입 계층을 기반으로 한 다형성의 동작 방식을 이해할 수 있게 될 것이다.

## 상속의 메커니즘

코드 안에서 선언된 참조 타입과 무관하게 실제로 메시지를 수신하는 객체의 타입에 따라 실행되는 메서드가 달라질 수 있는 것은 업캐스팅과 동적 바인딩이라는 메커니즘이 작용하기 때문이다.

```java
interface Programmer{
    public void coding();
}

class Steve implements Programmer{
    public void coding(){
        System.out.println("fast");
    }
}

class Rachel implements Programmer{
    public void coding(){
        System.out.println("elegance");
    }
}

public class Workspace{
    public static void main(String[] args){
			List<Programmer> programmers = new ArrayList<>();
	    programmers.add(new Steve());
	    programmers.add(new Rachel());

	    for(Programmer programmer : programmers){
	        programmer.coding();
	    }
}

실행결과
fast
elegance
```

### 업캐스팅

부모 클래스 타입으로 선언된 변수에 자식 클래스의 인스턴스를 할당하는 것이 가능하다.

### 동적 바인딩

선언된 변수의 타입이 아니라 메시지를 수신하는 객체의 타입에 따라 실행되는 메서드가 결정된다. 이것은 객체지향 시스템이 메시지를 처리할 적절한 **메서드를 컴파일 시점이 아니라 실행 시점에 결정**하기 때문에 가능하다. 이를 동적 바인딩이라고 부른다.

### 동적 메서드 탐색

객체지향 시스템은 다음 규칙에 따라 실행할 메서드를 선택한다.

- 메시지를 수신한 객체는 **먼저 자신을 생성한 클래스에 적합한 메서드가 존재하는지 검사**한다. 존재하면 메서드를 실행하고 탐색을 종료한다.
- 메서드를 찾지 못했다면 **부모 클래스에서 메서드 탐색을 계속**한다. 이 과정은 적합한 메서드를 찾을 때까지 상속 계층을 따라 올라가며 계속된다.
- 상속 계층의 가장 최상의 클래스에 이르렀지만 **메서드를 발견하지 못한 경우 예외를 발생시키며 탐색을 중단**한다.

### Self 참조

메시지 탐색과 관련해서 이해해야 하는 중요한 변수가 하나 있다. **self 참조**(self reference)가 바로 그것이다. 객체가 메시지를 수신하면 컴파일러는 self 참조라는 임시 변수를 자동으로 생성한 후 메시지를 수신한 객체를 가리키도록 설정한다. **동적 메서드 탐색은 self가 가리키는 객체의 클래스에서 시작해서 상속 계층의 역방향으로 이뤄지며 메서드 탐색이 종료되는 순간 self 참조는 자동으로 소멸된다.** 시스템은 앞에서 설명한 class 포인터와 parent 포인터와 함께 self 참조를 조합해서 메서드를 탐색한다.

> 정적 타입 언어에 속하는 C++, 자바, C#에서는 self 참조를 this라고 부른다. 동적 타입 언어에 속하는 스몰토크, 루비에서는 self 참조를 나타내는 키워드로 self를 사용한다. 파이썬에서는 self 참조의 이름을 임의로 정할 수 있지만 대부분의 개발자들은 전통을 존중해서 self라는 이름을 사용한다.
> 

메서드 탐색은 자식 클래스에서 부모 클래스의 방향으로 진행된다. 따라서 항상 자식 클래스의 메서드가 부모 클래스의 메서드보다 먼저 탐색되기 때문에 자식 클래스에 선언된 메서드가 부모 클래스의 메서드보다 더 높은 우선순위를 가지게 된다.

메시지가 처리되는 문맥을 이해하기 위해서는 정적인 코드를 분석하는 것만으로는 충분하지 않다. 런타임에 실제로 메시지를 수신한 객체가 어떤 타입인지를 추적해야 한다. 이 객체의 타입에 따라 메서드를 탐색하는 문맥이 동적으로 결정되며, 여기서 가장 중요한 역할을 하는 것이 바로 self 참조다.

### self 참조 대 super 참조

self 참조의 가장 큰 특징은 동적이라는 점이다. self 참조는 메시지를 수신한 객체의 클래스에 따라 메서드 탐색을 위한 문맥을 실행 시점에 결정한다. self의 이런 특성과 대비해서 언급할 만한 가치가 있는 것이 바로 super 참조(super refence)다.

자식 클래스에서 부모 클래스의 구현을 재사용해야 하는 경우가 있다. 대부분의 객체지향 언어들은 자식 클래스에서 부모 클래스의 인스턴스 변수나 메서드에 접근하기 위해 사용할 수 있는 super 참조라는 내부 변수를 제공한다.

사실 super 참조의 용도는 부모 클래스에 정의된 메서드를 실행하기 위한 것이 아니다. super 참조의 정확한 의도는 ‘지금 이 클래스의 부모 클래스에서부터 메서드 탐색을 시작하세요'다. 만약 부모 클래스에서 원하는 메서드를 찾지 못한다면 더 상위의 부모 클래스로 이동하면서 메서드가 존재하는지 검사한다.

이것은 super 참조를 통해 실행하고자 하는 메서드가 반드시 부모 클래스에 위치하지 않아도 되는 유연성을 제공한다. 그 메서드가 조상 클래스 어딘가에 있기만 하면 성공적으로 탐색될 것이기 때문이다.

---

**참조**

<조영호, 오브젝트, 위키북스>
