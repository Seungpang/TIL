## 02. 이상한 나라의 객체

> 객체지향 패러다임은 지식을 추상화하고 추상화한 지식을 객체 안에 캡슐화함으로써 실세계 문제에 내재된 복잡성을 관리하려고 한다.
> 객체를 발견하고 창조하는 것은 지식과 행동을 구조화하는 문제다.

### 객체지향과 인지 능력
+ 객체란 인간이 분명하게 인지하고 구별할 수 있는 물리적인 또는 개념적인 경계를 지닌 어떤 것이다.
+ 객체지향 패러다임의 목적은 현실 세계를 모방하는 것이 아니라 현실 세계를 기반으로 새로운 세계를 창조하는 것
+ 소프트웨어 세계에서 살아가는 객체는 현실 세계에 존재하는 객체와는 전혀 다른 모습을 보이는것이 일반적이다.

### 객체, 그리고 이상한 나라

#### 앨리스 객체
+ 행동에 따라 상태가 변하게 된다.
+ 행동의 결과를 결정하는 것은 상태다.
  + 행동의 결과는 상태에 의존적이다.

+ 앨레스는 상태를 가지며 상태는 변경 가능하다.
+ 앨리스의 상태를 변경시키는 것은 앨리스의 행동이다.
  + 행동의 결과는 상태에 의존적이며 상태를 이용해 서술할 수 있다.
  + 행동의 순서가 결과에 영향을 미친다.
+ 앨리스는 어떤 상태에 있더라도 유일하게 식별 가능하다.


### 객체, 그리고 소프트웨어 나라

> 객체란 식별 가능한 개체 또는 사물이다. 객체는 자동차처럼 만질 수 있는 구체적인 사물일 수도 있고,
> 시간처럼 추상적인 개념일 수도 있다. 객체는 구별 가능한 식별자, 특징적인 행동, 변경 가능한 상태를 가진다.
> 소프트웨어 안에서 객체는 저장된 상태와 실행 가능한 코드를 통해 구현된다.

#### 상태
상태를 이용하면 과거에 얽매이지 않고 현재를 기반으로 객체의 행동 방식을 이해할 수 있다.
상태는 근본적으로 세상의 복잡성을 완화하고 인지 과부하를 줄일 수 있는 중요한 개념이다.

> 상태는 특정 시점에 객체가 가지고 있는 정보의 집합으로 객체의 구조적 특징을 표현한다.
> 객체의 상태는 객체에 존재하는 정적인 프로퍼티와 동적인 프로퍼티 값으로 구성된다.
> 객체의 프로퍼티는 단순한 값과 다른 객체를 잠조하는 링크로 구분할 수 있다.

+ 객체는 스스로의 행동에 의해서만 상태가 변경되는 것을 보장함으로써 객체의 자율성을 유지한다.

#### 행동
+ 객체의 행동은 상태에 영향을 받는다.
+ 객체의 행동은 상태를 변경시킨다.

상태라는 개념으로는
+ 상호작용이 현재의 상태에 어떤 방식으로 의존하는가
+ 상호작용이 어떻게 현재의 상태를 변경시키는가

상태를 이용하면 복잡한 객체의 행동을 쉽게 이해할 수 있다.

#### 협력과 행동
> 행동이란 외부의 요청 또는 수신된 메시지에 응답하기 위해 동작하고 반응하는 활동이다.
> 행동의 결과로 객체는 자신의 상태를 변경하거나 다른 객체에게 메시지를 전달할 수 있다.
> 객체는 행동을 통해 다른 객체와의 협력에 참여하므로 행동은 외부에 가시적이어야 한다.


#### 상태 캡술화

상태를 잘 정의된 행동 집합 뒤로 캡슐화하는 것은 객체의 자율성을 높이고 협력을 단수하고 유연하게 만든다.
이것이 상태를 캡슐화해야 하는 이유다.

### 식별자
> 식별자란 어떤 객체를 다른 객체와 구분하는 데 사용하는 객체의 프로퍼티다.
> 값은 식별자를 가지지 않기 때문에 상태를 이용한 동등성 검사를 통해 두 인스턴스를 비교해야 한다.
> 객체는 상태가 변경될 수 있기 때문에 식별자를 이용한 동일성 검사를 통해 두 인스턴스를 비교할 수 있다.

+ 객체는 상태를 가지며 상태는 변경 가능하다.
+ 객체의 상태를 변경시키는 것은 객체의 행동이다.
  + 행동의 결과는 상태에 의존적이며 상태를 이용해 서술할 수 있다.
  + 행동의 순서가 실행 결과에 영향을 미친다.
+ 객체는 어떤 상태에 있더라도 유일하게 식별 가능하다.

### 기계로서의 객체

### **행동이 상태를 결정한다** (2장에서 가장 중요)

1. 상태를 먼저 결정할 경우 캡슐화가 저해된다.
2. 객체를 협력자가 아닌 고립된 섬으로 만든다.
3. 객체의 재사용성이 저하된다.
-> 객체의 적합성을 결정하는 것은 상태가 아니라 객체의 행동이다.

### 은유와 객체

