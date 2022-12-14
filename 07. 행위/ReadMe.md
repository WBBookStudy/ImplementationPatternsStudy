# 07. 행위
이 장에서는 프로그램의 행위를 표현하는 방법에 대해 알아본다.

## 제어 흐름 : 연산을 여러 단계로 나타낸다. 
자바의 경우 제어 흐름은 언어의 근간 중 하나이며, 인접한 구문은 순서대로 수행된다.  
제어흐름은 관련된 것들끼리 모아서 처음보는 독자도 손쉽게 이해할 수 있게 하는 것이 좋다.

## 주요 흐름 : 주요 제어 흐름을 명확하게 표현한다. 
연산에는 큰 흐름이 있고, 프로그래밍 언어를 통해 주요 흐름을 명확히 표현하라.  
흔치 않은 상황이나 에러 상황은 예외와 조건절을 사용해서 표현하라.  

## 메시지 : 메시지를 보내서 제어 흐름을 표현한다.
메시지를 제어 흐름의 메커니즘으로 사용하면 프로그램에서는 상태의 변화가 중요해진다.  
메시지 수신자가 메시지를 받으면, 발신자의 상태는 바뀌지 않지만 수신자의 상태는 바뀔 수 있다.  
이러한 유연성을 지혜롭게 사용해서 로직을 명확하고 직접적으로 표현하고, 세부 구현을 적당히 미루는 것은  
효과적인 커뮤니케이션을 하는 코드를 작성하기 위한 중요한 기술이다.

## 선택 메시지 : 여러 선택 사항을 나타내기 위해 메시지 구현자를 다양화 한다. 
선택 메시지를 사용하면 명시적 조건문의 사용을 크게 줄일수 있으며 추후 확장이 쉽다.  
프로그램에서 명시적 조건문을 사용할 경우 나중에 전체 프로그램의 행위를 바꿀 때마다 명시적으로 수정해야 한다.  
예를 들어 여러 방법 중 한 가지로 그래픽을 표시해야 할 때, 런타임에 선택이 일어남을 나타내기 위해 다형적 메시지를 사용한다. 
```Java
public void displayShape(Shape subject, Brush brush) {
	brush.display(subject);
}
```
display() 메시지는 런타임에 브러시의 타입에 따라 구현을 선택한다. 

## 더블 디스패치 : 두 가지 축으로 메시지 구현자를 다양화해서 중첩된 선택을 표현한다.
예를 들어 포스트스크립트로 출력할 타원과 스크린에 출력할 직사각형의 연산과정의 차이점을 표현하고 싶다고 하자.   
```Java
displayShape(Shape subject, Brush brush) {
	subject.displayWith(brush);
}

// 각 shape은 다른 방식으로 displayWith()를 구현할 수 있다. 
//하지만 세부 구현을 대신하는 자신의 타입을 Brush로 넘겨주자

Oval.displayWith(Brush brush) {
	brush.displayOval(this);
}
Rectangle.displayWith(Brush brush) {
	brush.displayRectangle(this);
}

//이제 각 브러시는 경우에 따라 어떤일을 해야 할지 알 수 있다.
PostscriptBrush.displayRectangle(Rectangle subject) {
	writer print(subject.left() + " " + ... + rect);
}
```
더블 디스패치를 사용하면 유연성은 조금 잃게 되며, 코드 중복이 발생한다.

## 분리(순차) 메시지 : 복잡한 연산은 밀접한 단위의 연산으로 나눈다.
여러 단계로 구성되는 복잡한 알고리즘이 있다면 , 관련된 단계들을 모으고 이를 수행하기 위해 메시지를 보낼 수 있다.  
분리 메시지에는 이름을 잘 지어야 하며, 코드를 보고 이름만으로 이후 단계에서 어떤 일어날지 짐작할 수 있어야 한다.  
만약 분리 메시지의 이름을 짓기 어렵다면 분리메시지 패턴을 사용하는 것이 적합하지 않다는 신호이다.

## 되돌림 메시지 : 메시지를 같은 수신자에게 보내서 제어 흐름에 대칭성을 부여한다. 
대칭성을 이용하면 코드의 가독성을 높일 수 있다.  
떄로는 대칭성과 같은 '미학적' 만족만을 위해 새로운 메소드를 만드는 것이 의미없게 느껴질수도 있지만,  
코드의 미학에 대한 식견을 높이게 되면, 미학적 느낌을 통해 코드의 품질을 평가 할 수 있게 된다.  
코드에서 받는 느낌은 이미 유용성이 알려져 있는 다른 패턴만큼이나 중요하다.

## 초청 메시지 : 다른 방식으로 구현될 수 있는 메시지를 보내서 미래에 일어날 변형을 대비한다.
떄로 코드를 짜면서 사람들이 하위클래스의 어떤 연산을 변형시킬 것이라 예상할 수 있다.  
이런 경우에는 적당한 이름의 메시지를 통해 변형의 여지를 마련했음을 알려주는 것이 좋다.  
이러한 메시지는 프로그래머로 하여금 이후 목적에 맞게 연산을 구현하도록 초청하는 의미를 갖는다.

## 설명 메시지 : 로직을 설명하기 위해 메시지를 보낸다.
소프트웨어 개발에서 개발자의 의도와 구현을 구분하는 것은 언제나 중요하다.  
독자는 이를 통해 먼저 연산의 핵심을 파악할 수 있고, 필요한 경우에만 세부 구현에 관심을 기울이면 된다.  
메시지를 사용하면 먼저 풀려는 문제의 이름을 반영하는 메시지를 보낸 후, 문제 푸는 방식을 반영하는 메시지를 보내서 이 구분을 명시할 수 있다.

## 예외 흐름 : 주요 흐름에 대한 표현을 방해하지 않으면서, 가급적 명확하게 예외적 제어 흐름을 표현한다. 
코드를 읽고 이해하기 가장 쉬운 경우는 프로그램의 각 문장이 순서대로 하나씩 수행될 때다.  
하지만 때로는 프로그램에 다양한 수행 경로가 존재 할 수 있다.  
주요 흐름을 최대한 명료하게 표현하라. 다른 경로는 예외를 사용해서 표현하면 된다.

## 보호절 : 지역적 예외 흐름은 이른 반환을 통해 표현한다. 
프로그램에는 주요 흐름이 있지만 때로는 주요 흐름에서 벗어나야 할 떄가 있다.  
보호절을 사용하면 간단한 지역적 예외 상황을 지역적인 변화만을 수반하며 표현할 수 있다.  
특히 중첩된 조건을 사용하게 되면 코드에 문제가 발생할 확률이 높다.  
보호절을 사용하면 복잡한 제어구조를 사용하지 않고도 같은 코드를 표현할 수 있다.

## 예외 : 비 지역적 예외 흐름은 예외로 표현한다. 
예외는 여러 함수 호출을 걸쳐서 제어 흐름을 바꾸는 경우를 표현할 때 유용하게 사용된다.  
예외를 발견한 쪽에서는 예외를 던지고, 에외를 처리하는 쪽에서 예외를 받는 편이 그 사이의 모든 코드에서 예외를 처리하지도 못하면서  
예외를 체크하고 전달하며 코드를 지저분하게 하는 것 보다는 휠씬 낫다.

## 체크 예외 : 명시적 선언으로 예외를 처리한다.
예외의 위험성 중 하나는 예외를 던졌을 때 아무도 그 예외를 받지 않을 수 있다는 것이다.  
이 경우 프로그램 수행은 종료되는데, 이렇게 예기치 않은 상태로 프로그램이 종료되는 경우 이러한 문제가 발생한 이유를 알고  
사용자에게 어떤 일이 일어났는지 알려주기 위한 정보를 출력하는 편이 좋다.

## 예외 전달 : 예외를 전달할 때에는 예외 처리자에게 적합한 정보를 전달할 수 있도록 필요에 따라 예외의 형태를 변화한다. 
하위 수준의 예외는 문제를 진단하는 데 유용한 정보를 제공해주는 경우가 많다.  
하위 수준 예외를 상위 수준 예외로 포장하라. 예를들어 로그를 통해 문제 해결을 돕는 메시지를 출력할 수 있을 것이다.
