# Chapter 17 - 생성자 함수에 의한 객체 생성

이번 장에서는 다양한 객체 생성 방식 중에서 **Constructor(생성자 함수)를 사용해 객체를 생성하는 방식**을 살펴보자.

## 1. Object 생성자 함수
다음과 같이 `new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성해 반환할 수 있다.<br/>
빈 객체를 생성 후 프로퍼티 또는 메서드를 추가해 객체를 완성하면 된다.

![image](https://github.com/user-attachments/assets/dd61cb9b-b2e0-41a8-96b5-bfc121b48ea1)

**용어 정리:**
* Constructor(생성자 함수): `new` 연산자와 함께 호출해 객체(인스턴스)를 생성하는 함수를 말한다.
* Instance(인스턴스): Constructor에 의해 생성된 객체를 말한다.

그러나! Object 생성자 함수를 사용해 객체를 생성하는 방식은,<br/>
특별한 이유가 없다면 그다지 유용하진 않다. (그냥 객체 리터럴을 쓰는 게 낫다.)

그럼 이제 생성자 함수를 배워보자.


## 2. Constructor: 생성자 함수
### 2-1. 객체 리터럴로 객체를 만들 때 문제점
객체 리터럴로 객체를 생성하는 방식은 직관적이고 간편하다는 장점이 있지만,<br/>
동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우에는 비효율적이라는 단점이 있다.

### 2-2. 생성자 함수로 객체를 만들 때 문제점
그에 비해 생성자 함수로 객체를 생성하는 방식은, 마치 다른 언어의 클래스처럼<br/>
프로퍼티 구조가 동일한 객체 여러 개를 아래처럼 간편하게 생성할 수 있다.

![{1016F6F5-0E1E-4595-A20C-F9C88F2345D7}](https://github.com/user-attachments/assets/248d26a5-531f-428b-9c35-6a22b282d4ef)

이처럼 생성자 함수는 객체(인스턴스)를 생성하는 함수다.<br/>
**하지만!** 다른 클래스 기반 언어의 생성자와는 달리, 자바스크립트의 생성자 함수는 **형식이 정해져 있지 않다**.<br/>
**일반 함수**와 동일한 방법으로 생성자 함수를 정의하고 **`new` 연산자와 함께 호출하면 그 함수는 생성자 함수**가 된다.

### 2-3. 생성자 함수의 인스턴스 생성 과정
생성자 함수의 역할은 1. **인스턴스를 생성하는 것**과<br/>
2. 생성한 인스턴스의 프로퍼티를 추가하고, 초기값을 할당하는 **초기화를 하는 것**이다.<br/>
자바스크립트 엔진은 `new` 연산자와 함께 생성자 함수를 호출하면,<br/>
다음과 같이 세 단계에 걸쳐 암묵적으로 인스턴스를 생성, 초기화, 그리고 반환한다.

![{31654410-9A7A-4418-9BD8-1661F8E4BBB5}](https://github.com/user-attachments/assets/90354fbc-7691-4679-9f13-8bd54ecfc040)

위 예제를 차례대로 살펴보자.

#### 1) 인스턴스 생성과 `this` 바인딩
`new` 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 **암묵적으로 빈 객체를 생성**하고,<br/>
이 객체(인스턴스)를 **`this`에 바인딩**한다. 이 과정은 함수 몸체의 코드를 한 줄씩 실행하는 **런타임 이전에 처리**된다.
#### 2) 인스턴스 초기화
`this`에 바인딩한 인스턴스를 초기화한다. 이 처리는 개발자가 기술한다.
#### 3) 인스턴스 반환
초기화를 끝낸 인스턴스를 바인딩한, `this`를 암묵적으로 반환한다.<br/>
**그러나!** 다음과 같은 예외가 있다:
* 만약 `this`가 아닌 다른 객체를 명시적으로 반환하면, `this`가 아닌 return 문에 명시한 그 객체를 반환한다.
* 만약 `this`가 아닌 어떤 원시 값을 명시적으로 반환하면, 원시 값 반환은 무시하고 암묵적으로 `this`를 반환한다.

**따라서, 이렇게 생성자 함수의 기본 동작을 훼손하지 않으려면, 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.**

### 2-4. 내부 메서드 [[Call]]과 [[Construct]]
함수는 객체이므로 **ordinary object(일반 객체)가 지닌 내부 슬롯과 내부 메서드를 똑같이** 지닌다.<br/>
뿐만 아니라, 함수로서 동작하기 위해 **함수 객체만을 위한 내부 슬롯과 내부 메소드**도 추가로 가지고 있다.

그 중 이번 절에서 살펴볼 내부 메서드는 [[**Call**]]과 [[**Construct**]]이다.<br/>
[[Call]]은 함수가 **일반 함수로서** 호출될 때 쓰이는 내부 메서드이고,<br/>
[[Construct]]는 `new` 연산자와 함께 **생성자 함수로서** 호출될 때 쓰이는 내부 메서드이다.

[[Call]]을 갖는 함수 객체를 우리는 **callable**이라고 부르고,<br/>
[[Construct]]를 갖는 함수 객체를 우리는 **constructor**라고 부른다. (갖지 않는 함수 객체는 **non-constructor**라고 부른다.)

중요한 사실은, 모든 함수 객체는 호출할 수 있지만, 생성자 함수로서 호출할 수 있지는 않다는 것이다.<br/>
**즉, 함수 객체는 무조건 callable이지만, 경우에 따라 constructor일 수도 있고, non-constructor일 수도 있다.**

그렇다면 어떤 함수 객체가 constructor인지 non-constructor인지 어떻게 구분할까?


### 2-5. constructor와 non-constructor의 구분
자바스크립트 엔진은 다음과 같이 **함수 정의** 방식에 따라 constructor와 non-constructor를 구분한다.
* constructor: 함수 선언문, 함수 표현식, 클래스(클래스도 함수임)
* non-constructor: ECMAScript 사양에서 인정하는 메서드(=ES6 메서드 축약 표현), 화살표 함수


### 2-6. `new` 연산자
`new` 연산자와 함께 함수를 호출하면, 그 함수는 이제 생성자 함수가 된다.<br/>
다시 말해, 내부 메서드 [[Call]]이 호출되는 것이 아니라, 이제 [[Construct]]가 호출되는 것이다.

반대로, `new` 연산자 없이 생성자 함수를 호출하면 그 함수는 일반 함수다.<br/>
다시 말해, 내부 메서드 [[Construct]]이 호출되는 것이 아니라, 이제 [[Call]]가 호출되는 것이다.

생성자 함수로서 호출되는지, 일반 함수로서 호출되는지에 따라 함수 내부의 `this`가 바인딩되는 대상이 달라진다.<br/>
`new` 연산자와 함께 생성자 함수로서 호출하면 `this`는 생성자 함수가 만든 인스턴스를 가리키고,<br/>
일반 함수로서 호출하면 `this`는 전역 객체 `window`를 가리키게 된다.

생성자 함수와 일반 생성자 함수는 형식적 차이가 없으므로,<br/>
**생성자 함수의 이름**을 지을 땐 일반적으로 **PascalCase**로 명명해 일반 함수와 구분해야 한다.


### 2-7. `new.target`
앞서 이야기한 PascalCase를 쓴다고 하더라도 위험성이 여전히 존재하므로,<br/>
ES6부터는 함수 내부에서 본인이 생성자 함수로서 호출되었는지 확인하게 해주는 new.target라는 메타 프로퍼티를 지원한다.

끝.