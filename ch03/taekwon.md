# 3. 함수

## 작게 만들어라

함수를 만드는 첫째 규칙은 `작게` 다.  다시말해 `if문 / else 문 / while문` 등에 들어가는 블록은 한 줄이어야 한다는 의미다.또한 블록 안에서 호출하는 함수 이름을 적절히 짓는다면, 코드를 이해하기도 쉬워진다.(**그러므로 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다.**)

## 한 가지만 해라

함수가 한 가지일을 하기는 쉽지않다. 그러나 여기서 말하는 한 가지 일이란, 지정된 함수 이름 아래서 추상화 수준이 하나라는 의미다.

즉 함수가 여러 일을 하고 있다는 것을 판단하는 방법은 의미 있는 이름으로 다른 함수를 추출할 수 있을 경우 이다.

## 함수 당 추상화 수준은 하나로

한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.

**코드는 위에서 아래로 이야기처럼 읽혀야 좋다.** 한 함수 다음에는 추상화 수준이 한 단계 낮은 함수가 온다. 즉 , 위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아진다.

## Swith 문(47 p)

본질적으로 switch 문은 N가지를 처리한다. 완전히 피할 방법은 없지만, 각 switch문을 저차원 클래스에 숨기고 절대로 반복하지 않는 방법은 있다.(`다형성`을 이용)

switch 문을 추상 팩토리에 꽁꽁 숨겨서 사용하는 방법이 있다. → 저차원 클래스에 숨기고 반복하지 않는다.(흠..)

## 서술적인 이름을 사용하라

이름이 길어도 괜찮다. 길고 서술적인 이름이 짧고 어려운 이름보다 좋다. 길고 서술적인 이름이 길고 서술적인 주석보다 좋다.

## 함수 인수

함수에서 이상적인 인수 개수는 0개이며 그다음 1개 2개 … 이다. 4개 이상은 특별한 이유가 필요지만 사실상 특별한 이유가 있어도 안 쓰는게 좋다.

예를 들어 StringBuffer를 인수로 넘기는거보다 함수 내에서 생성해서 처리하는게 코드를 읽는 사람에게는 이해가 더 쉽다.

테스트 관점에서 보면 인수는 더 어렵다. 갖가지 인수 조합으로 함수를 검증하는 테스트 케이스를 작성한다고 상상해보자 인수가 없다면 간단하다.

[많이 쓰는 단항 형식]

가장 흔한 경우는 두가지로 먼저 인수에 질문을 던지는 경우다. 또다른 경우는 인수를 뭔가로 변환해 결과를 반환하는 경우다. 이런경우가 아니라면 단항 함수는 가급적 피하는게 좋다.

[플래그 인수]

플래그 인수는 추하다. 함수로 부울 값을 넘기는 관례는 정말로 끔찍하다, 함수가 한꺼번에 여러 가지 처리를 한다고 대놓고 공표하는 셈이다.

[이항함수]

물론 단항보다 이해하기 어렵다. 그만큼 위험이 따른다는 사실을 이해하고 가능하면 단항 함수로 바꾸도록 애써야 한다.

## 부수효과를 일으키지 마라.

함수에서 한 가지를 하겠다고 약속하고선 남몰래 다른 짓도한다. 즉 함수에서 안할거 같은 Session.initalize() 등의 작업을 하는 것은 좋지 않다.

[출력 인수]

일반적으로 우리는 인수를 함수 입력으로 해석한다. 인수를 출력으로 사용하는 함수에 어색함을 느끼게 된다.

```java
appendFotter(s);
이 함수는 무언가에 s를 바닥글로 첨부할까? 아니면 s에 바닥글을 첨부할까?

public void appendFooter(StringBuffer report)
인수 s가 출력 인수라는 사실은 분명하지만, 함수 선언부를 보고서야 알 수 있다.
즉 StringBuffer 객체를 입력받아서 footer가 추가된 StringBuffer를 만드는 함수이다.
이는 X -> f -> X 형태이다.  이는 인지적인 이유로 가독성을 떨어트린다.

그러나 StringBuffer appendFooter(StringBuffer report)라고 표현하지 않는다. 
같은 객체이기 때문에 굳이 반환 안해줘도 된다.  
void appendFooter(StringBuffer report)로 표현한다. 그래서 개발자가 코드 상에서 
appendFooter(report)라는 함수를 보면 인지적으로 혼란이 생긴다. 
함수는 인수를 받으면 조회하거나 변환해야 하는데 appendFooter(report)는 
report 객체를 인수로 받아서 Footer를 append하고 무엇으로 반환한다는 말인지 감을 잡지 못한다.

함수는 X → f → X이고 인수는 변환되지 않는다. 
조회도 아니고 변환도 아니므로 굳이 X를 인수로 받을 필요가 없다. 
그러므로 X → f → X의 형태의 함수는 인수를 가지지 않아야 자연스럽다.

변경 전 : appendFooter(report)
변경 후 : report.appendFooter();

즉 객체를 이용하자. this를 사용하면 인수를 사용하지 않고도 함수의 처리 대상이 자기 자신임을
알 수 있다.

예를 들어 void updateCountryKorea(Country country) 보다
Country 클래스 내에 updateCountryKorea() 를통해 country.setNationality("KOREA")
형태가 더 낫다는 뜻이다.

```

## 오류 코드보다 예외를 사용하라

예외를 던져라 그럼 오류처리 코드가 원래 코드에서 분리되므로 깔끔해진다.

[try/catch 블록 뽑아내기] - 58p 참조

원래 해당 블록은 추하다.  그러므로 try/catch 블록을 별도 함수로 뽑아내는 편이 좋다.
