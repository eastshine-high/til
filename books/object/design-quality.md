# 설계 품질

[오브젝트](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791158391409&orderClick=LAG&Kc=)(조영호 저)의 4단원을 요약 정리한 내용입니다.

---

> **객체지향 설계란 올바른 객체에게 올바른 책임을 할당하면서 낮은 결합도와 높은 응집도를 가진 구조를 창조하는 활동이다[Evers09]**.
> 

이 정의에는 객체지향 설계에 관한 두 가지 관점이 섞여 있다. 첫 번째 관점은 객체지향 설계의 핵심이 책임이라는 것이다. **두 번째 관점은 책임을 할당하는 작업이 응집도와 결합도 같은 설계 품질과 깊이 연관돼 있다는 것이다.**

설계는 변경을 위해 존재하고 변경에는 어떤 식으로든 비용이 발생한다. **훌륭한 설계란 합리적인 비용 안에서 변경을 수용할 수 있는 구조를 만드는 것**이다. 적절한 비용 안에서 쉽게 변경할 수 있는 설계는 응집도가 높고 서로 느슨하게 결합돼 있는 요소로 구성된다.

**결합도와 응집도를 합리적인 수준으로 유지할 수 있는 중요한 원칙**이 있다. **객체의 상태가 아니라 객체의 행동에 초점을 맞추는 것이다.** 객체를 단순한 데이터의 집합으로 바라보는 시각은 객체의 내부 구현을 퍼블릭 인터페이스에 노출시키는 결과를 낳기 때문에 결과적으로 설계가 변경에 취약해진다. 이런 문제를 피할 수 있는 가장 좋은 방법은 객체의 책임에 초점을 맞추는 것이다. **책임은 객체의 상태에서 행동으로, 나아가 객체와 객체 사이의 상호작용으로 설계 중심을 이동시키고, `결합도`가 낮고 `응집도`가 높으며 구현을 효과적으로 `캡슐화`하는 객체들을 창조할 수 있는 기반을 제공한다.**

설계의 세 가지 품질 척도(캡슐화, 응집도, 결합도)의 의미를 살펴보자.

## 캡슐화

상태와 행동을 하나의 객체 안에 모으는 이유는 객체의 내부 구현을 외부로부터 감추기 위해서다. 여기서 구현이란 나중에 변경될 가능성이 높은 어떤 것을 가리킨다. 객체지향이 강력한 이유는 한 곳에서 일어난 변경이 전체 시스템에 영향을 끼치지 않도록 파급효과를 적절하게 조절할 수 있는 장치를 제공하기 때문이다. 객체를 사용하면 변경 가능성이 높은 부분은 내부에 숨기고 외부에는 상대적으로 안정적인 부분만 공개함으로써 변경의 여파를 통제할 수 있다.

변경될 가능성이 높은 부분을 `구현`이라고 부르고 상대적으로 안정적인 부분을 `인터페이스`라고 부른다는 사실을 기억하라. 객체를 설계하기 위한 가장 기본적인 아이디어는 변경의 정도에 따라 구현과 인터페이스를 분리하고 외부에서는 인터페이스에만 의존하도록 관계를 조절하는 것이다.

객체지향에서 가장 중요한 원리는 `캡슐화`다. 캡슐화는 외부에서 알 필요가 없는 부분을 감춤으로써 대상을 단순화하는 추상화의 한 종류다. 객체지향 설계의 가장 중요한 원리는 불안정한 구현 세부사항을 안정적인 인터페이스 뒤로 캡슐화하는 것이다.

> 복잡성을 다루기 위한 가장 효과적인 도구는 추상화다. 다양한 추상화 유형을 사용할 수 있지만 객체지향 프로그래밍에서 복잡성을 취급하는 주요한 추상화 방법은 캡슐화다. 그러나 프로그래밍할 때 객체지향 언어를 사용한다고 해서 애플리케이션의 복잡성이 잘 캡슐화될 것이라고 보잘할 수 없다. 훌륭한 프로그래밍 기술을 적용해서 캡슐화를 향상시킬 수는 있겠지만 객체지향 프로그래밍을 통해 전반적으로 얻을 수 있는 장점은 오직 설계 과정 동안 캡슐화를 목표로 인식할 때만 달성될 수 있다[Wirfs-Brock89]
> 

설계가 필요한 이유는 요구사항이 변경되기 때문이고, 캡슐화가 중요한 이유는 불안정한 부분과 안정적인 부분을 분리해서 변경의 영향을 통제할 수 있기 때문이다. 따라서 변경의 관점에서 설계의 품질을 판단하기 위해 캡슐화를 기준으로 삼을 수 있다.

**정리하면 캡슐화란 변경 가능성이 높은 부분을 객체 내부로 숨기는 추상화 기법이다.** 객체 내부에 무엇을 캡슐화해야 하는가? 변경될 수 있는 어떤 것이라도 캡슐화해야 한다. 이것이 바로 객체지향 설계의 핵심이다.

> 유지보수성이 목표다. 여기서 유지보수성이란 두려움 없이, 주저함 없이, 저항감 없이 코드를 변경할 수 있는 능력을 말한다. ... 가장 중요한 동료는 캡슐화다. 캡슐화란 어떤 것을 숨긴다는 것을 의미한다. 우리는 시스템의 한 부분을 다른 부분으로부터 감춤으로써 뜻밖의 피해가 발생할 수 있는 가능성을 사전에 방지할 수 있다. 만약 시스템이 완전히 캡슐화된다면 우리는 변경으로부터 완전히 자유로워질 것이다. 만약 시스템의 캡슐화가 크게 부족하다면 우리는 변경으로부터 자유로울 수 없고, 결과적으로 시스템은 진화할 수 없을 것이다. 응집도, 결합도, 중복 역시 훌륭한(변경 가능한) 코드를 규정하는 데 핵심적인 품질인 것이 사실이지만 캡슐화는 우리를 좋은 코드로 안내하기 때문에 가장 중요한 제 1원리다.[Bain08]
> 

### 캡슐화 위반

```java
public class Movie {
	private Money fee;

	public Money getFee() {
		return fee;
	}
	
	public void setFee(Money fee) {
		this.fee = fee;
	}
}
```

위 코드는 직접 객체의 내부에 접근할 수 없기 대문에 캡슐화의 원칙을 지키고 있는 것처럼 보인다. 정말 그럴까? 안타깝게도 접근자와 수정자 메서드는 객체 내부의 상태에 대한 어떤 정보도 캡슐화하지 못한다. getFee 메서드와 setFee 메서드는 Movie 내부에 Money 타입의 fee라는 이름의 인스턴스 변수가 존재한다는 사실을 퍼블릭 인터페이스에 노골적으로 드러낸다.

Movie가 캡슐화의 원칙을 어기게 된 근본적인 원인은 객체가 수행할 책임이 아니라 내부에 저장할 데이터에 초점을 맞췄기 때문이다. 객체에게 중요한 것은 책임이다. 그리고 구현을 캡슐화할 수 있는 적절한 책임은 협력이라는 문맥을 고려할 때만 얻을 수 있다.

설계할 때 협력에 관해 고민하지 않으면 캡슐화를 위반하는 과도한 접근자와 수정자를 가지게 되는 경향이 있다. 객체가 사용될 문맥을 추측할 수 밖에 없는 경우 개발자는 어떤 상황에서도 해당 객체가 사용될 수 있게 최대한 많은 접근자 메서드를 추가하게 되는 것이다.

앨런 홀럽(Allen Holub)은 이처럼 접근자와 수정자에 과도하게 의존하는 설계 방식을 추측에 의한 설계 전략이라고 부른다. 이 전략은 객체가 사용될 협력을 고려하지 않고 객체가 다양한 상황에서 사용될 수 있을 것이라는 막연한 추측을 기반으로 설계를 진행한다. 따라서 프로그래머는 내부 상태를 드러내는 메서드를 최대한 많이 추가해야 한다는 압박에 시달릴 수밖에 없으며 결과적으로 대부분의 내부 구현이 퍼블릭 인터페이스에 그대로 노출될 수밖에 없는 것이다. 그 결과, 캡슐화의 원칙을 위반하는 변경에 취약한 설계를 얻게 된다.

## 결합도

**`결합도(coupling)`는 의존성의 정도를 나타내며** 다른 모듈에 대해 얼마나 많은 지식을 갖고 있는지를 나타내는 척도다. **어떤 모듈이 다른 모듈에 대해 너무 자세한 부분까지 알고 있다면 두 모듈은 높은 결합도를 가진다.** 어떤 모듈이 다른 모듈에 대해 꼭 필요한 지식만 알고 있다면 두 모듈은 낮은 결합도를 가진다. **객체지향의 관점에서 결합도는 객체 또는 클래스가 협력에 필요한 적절한 수준의 관계만을 유지하고 있는지를 나타낸다.**

**결합도는 한 모듈이 변경되기 위해서 다른 모듈의 변경을 요구하는 정도로 측정할 수 있다.** 다시 말해 하나의 모듈을 수정할 때 얼마나 많은 모듈을 함께 수정해야 하는지를 나타낸다. 따라서 결합도가 높으면 높을수록 함께 변경해야 하는 모듈의 수가 늘어나기 때문에 변경하기 어려워진다.

**영향을 받는 모듈의 수 외에도 변경의 원인을 이용해 결합도의 개념을 설명할 수도 있다.** 내부 구현을 변경했을 때 이것이 다른 모듈에 여향을 미치는 경우에는 두 모듈 사이의 결합도가 높다고 표현한다. 반면 퍼블릭 인터페이스를 수정했을 때만 다른 모듈에 영향을 미치는 경우에는 결합도가 낮다고 표현한다. 따라서 클래스의 구현이 아닌 인터페이스에 의존하도록 코드를 작성해야 낮은 결합도를 얻을 수 있다. 이것은 “인터페이스에 대해 프로그래밍하라[GOF94]”라는 격언으로도 잘 알려져 있다.

**결합도가 높아도 상관 없는 경우도 있다. 일반적으로 변경될 확률이 매우 적은 안정적인 모듈에 의존하는 것은 아무런 문제도 되지 않는다.** 표준 라이브러리에 포함된 모듈이나 성숙 단계에 접어든 프레임워크에 의존하는 경우가 여기에 속한다. 예를 들어, 자바의 String이나 ArrayList는 변경될 확률이 매우 낮기 때문에 결합도에 대해 고민할 필요가 없다.

그러나 직접 작성한 코드의 경우에는 이야기가 다르다. 직접 작성한 코드는 항상 불안정하며 언제라도 변경될 가능성이 높다. 코드 안에 버그가 존재할 수도 있고 갑자기 요구사항이 변경될 수도 있다. 코드를 완성한 그 순간부터 코드를 수정할 준비를 해야 한다. 따라서 직접 작성한 코드의 경우에는 낮은 결합도를 유지하려고 노력해야 한다.

### 높은 결합도

데이터 중심의 설계는 접근자(Getter)와 수정자(Setter)를 통해 내부 구현을 인터페이스의 일부로 만들기 때문에 캡슐화를 위반한다. 객체 내부의 구현이 객체의 인터페이스에 드러난다는 것은 클라이언트가 구현에 강하게 결합된다는 것을 의미한다. 그리고 더 나쁜 소식은 단지 객체의 내부 구현을 변경했음에도 이 인터페이스에 의존하는 모든 클라이언트들도 함께 변경해야 한다는 것이다.

```java
public class ReservationAgency{
		...
		Money fee;
		if (discountable) {
			...

		} else {
				fee = movie.getFee();
		}
}
```

이 코드에서 알 수 있는 것처럼 ReservationAgency는 한 명의 예매 요금을 계산하기 위해 Movie의 getFee메서드를 호출하는 계산된 결과를 Money 타입에 fee에 저장한다. 이때 fee의 타입을 변경한다고 가정해보자. 이를 위해서는 getFee 메서드의 반환 타입도 함께 수정해야 할 것이다. 그리고 getFee 메서드를 호출하는 ReservationAgency의 구현도 변경된 타입에 맞게 함께 수정해야 할 것이다.

fee의 타입 변경으로 인해 협력하는 클래스가 변경되기 때문에 getFee 메서드는 fee를 정상적으로 캡슐화하지 못한다. 사실 getFee 메서드를 사용하는 것은 인스턴스 변수 fee의 가시성을 private에서 public으로 변경하는 것과 거의 동일한다. 이처럼 데이터 중심 설계는 객체의 캡슐화를 약화시키기 때문에 클라이언트가 객체의 구현에 강하게 결합된다.

결합도 측면에서 데이터 중심 설계가 가지는 또 다른 단점은 여러 데이터 객체들을 사용하는 제어 로직이 특정 객체 안에 집중되기 때문에 하나의 제어 객체가 다수의 데이터 객체에 강하게 결합된다는 것이다. 이 결합도로 인해 어떤 데이터 객체를 변경하더라도 제어 객체를 함께 변경할 수밖에 없다.

## 응집도

**응집도(cohesion)는 모듈에 포함된 내부 요소들이 연관돼 있는 정도를 나타낸다.** 모듈 내의 요소들이 하나의 목적을 위해 긴밀하게 협력한다면 그 모듈은 높은 응집도를 가진다. **모듈 내의 요소들이 서로 다른 목적을 추구한다면 그 모듈은 낮은 응집도**를 가진다. **객체지향의 관점에서 응집도는 객체 또는 클래스에 얼마나 관련 높은 책임들을 할당했는지를 나타낸다.**

**응집도가 높은 설계에는 하나의 요구사항 변경을 반영하기 위해 오직 하나의 모듈만 수정하면 된다.** 반면 응집도가 낮은 설계에서는 하나의 원인에 의해 변경해야 하는 부분이 다수의 모듈에 분산돼 있기 때문에 여러 모듈을 동시에 수정해야 한다.

**응집도가 높을수록 변경의 대상과 범위가 명확해지기 때문에 코드를 변경하기 쉬워진다. 변경으로 인해 수정되는 부분을 파악하기 위해 코드 구석구석을 헤매고 다니거나 여러 모듈을 동시에 수정할 필요가 없으며 변경을 반영하기 위해 오직 하나의 모듈만 수정하면 된다.**

### 낮은 응집도

서로 다른 이유로 변경되는 코드가 하나의 모듈 안에 공존할 때 모듈의 응집도가 낮다고 말한다. 따라서 각 모듈의 응집도를 살펴보기 위해서는 코드를 수정하는 이유가 무엇인지 살펴봐야 한다. 만약 하나의 모듈을 수정하는 이유가 여러가지라면 응집도가 낮을 가능성이 높다.

## 좋은 설계

응집도와 결합도는 **구조적 설계 방법이 주도하던 시대에 소프트웨어의 품질을 측정하기 위해 소개된 기준**이지만 객체지향의 시대에서도 **여전히 유효**하다.

**좋은 설계란 오늘의 기능을 수행하면서 내일의 변경을 수용할 수 있는 설계다.** 설계의 변경을 쉽게 하기 위해서는 높은 응집도와 낮은 결합도를 추구해야 한다.

마지막으로 **캡슐화의 정도가 응집도와 결합도에 영향을 미친다**는 사실을 강조하고 싶다. 캡슐화를 지키면 모듈 안의 응집도는 높아지고 모듈 사이의 결합도는 낮아진다. 캡슐화를 위반하면 모듈 안의 응집도는 낮아지고 모듈 사이의 결합도는 높아진다. 따라서 응집도와 결합도를 고려하기 전에 먼저 **캡슐화**를 향상시키기 위해 노력하라.

### 단일 책임 원칙(Single Responsibility Principle, SRP)

로버트 마틴(Robert C. Martin)은 모듈의 응집도가 변경과 연관이 있다는 사실을 강조하기 위해 단일 책임 원칙이라는 설계 원칙을 제시했다[Martin02]. 단일 책임 원칙을 한마디로 요약하면 **클래스는 단 한 가지의 변경 이유만 가져야 한다는 것이다.** 아마 방금 전에 설명한 내용을 이해했다면 단일 책임 원칙이 클래스의 응집도를 높일 수 있는 설계 원칙이라는 사실을 이해했을 것이다.

**한 가지 주의할 점은 단일 책임 원칙이라는 맥락에서 ‘책임'이라는 말이 ‘변경의 이유’라는 의미로 사용된다는 점**이다. 단일 책임 원칙에서 책임은 지금까지 살펴본 역할, 책임, 협력에서 이야기하는 책임과는 다르며 변경과 관련된 더 큰 개념을 가리킨다.

## 자율적인 객체를 향해

### 캡슐화를 지켜라

**캡슐화는 설계의 제 1원리다.** 데이터 중심의 설계가 낮은 응집도와 높은 결합도라는 문제로 몸살을 앓게 된 근본적인 원인은 바로 캡슐화의 원칙을 위반했기 때문이다. 객체는 자신이 어떤 데이터를 가지고 있는지를 내부에 캡슐화하고 외부에 공개해서는 안된다. 객체는 스스로의 상태를 책임져야 하며 외부에서는 인터페이스에 정의된 메서드를 통해서만 상태에 접근할 수 있어야 한다.

여기서 말하는 메서드는 단순히 속성 하나의 값을 반호나하거나 변경하는 접근자나 수정자를 의미하는 것이 아니다.  객체에게 의미 잇는 메서드는 객체가 책임져야 하는 무언가를 수행하는 메서드다. 속성의 가시성을 private으로 설정했다고 해도 접근자와 수정자를 통해 속성을 외부로 제공하고 있다면 캡슐화를 위반하는 것이다.

---

**참조**

<조영호, 오브젝트, 위키북스>
