# 12. 창발성

# Overview

**창발**(創發) 또는 **떠오름 현상**은 하위 계층(구성 요소)에는 없는 특성이나 행동이 상위 계층(전체 구조)에서 자발적으로 돌연히 출현하는 현상이다. 또한 불시에 솟아나는 특성을 창발성이라 한다.

## 창발적 설계로 깔끔한 코드를 구현하자

[켄트 벡](https://ko.wikipedia.org/wiki/%EC%BC%84%ED%8A%B8_%EB%B2%A1)은 아래와 같은 규칙을 따르면 설계는 `단순하다` 고 말한다.

- 모든 테스트를 실행한다.
- 중복을 없앤다.
- 프로그래머 의도를 표현한다.
- 클래스와 메서드 수를 최소로 줄인다.

### 단순한 설계 규칙 1: 모든 테스트를 실행하라

설계는 의도한 대로 돌아가는 시스템을 내놓아야 한다. 문서로는 완벽하게 설계했지만, 의도한 대로 돌아가는지 `검증할 간단한 방법이 없다면`, 노력에 대한 가치는 **인정받기 힘들다**.

모든 테스트 케이스를 항상 통과하는 시스템은 `테스트가 가능한 시스템` 이며, 테스트가 불가능한 시스템은 `검증도 불가능` 하다.

**테스트가 가능한 시스템을 만들려고 애쓰면** 크기가 작고 **목적 하나만 수행하는(`SRP`를 준수하는) 클래스가 나온다.**

`결합도가 높으면 테스트 케이스를 작성하기 어렵다.` 그러므로, DIP(의존성 역전) 와 같은 원칙을 적용하고 DI(의존성 주입), 인터페이스, 추상화 등과 같은 도구를 사용해 결합도를 낮춘다. 따라서 **설계 품질은 더욱 높아진다.**

<aside>
❓ **DIP - 의존성 역전 원칙**

`상위 수준의 모듈`은 하`위 수준의 모듈`에 **의존해서는 안 되며** 양쪽 모듈 모두 **추상화에 의존해야 한다는 원칙**이다.즉, 의존성은 추상화에 의존해야 하며, **세부 구현에 의존해서는 안 된다**는 것을 의미한다.쉽게 말 해 "구현 클래스에 의존하지 말고, 인터페이스에 의존하라는 뜻

![image](https://github.com/Zero-ToHero/202403-clean-code/assets/71249347/16de91d5-2c8f-49c3-bccf-2ed98e96b113)

**[IN Typescript]**

[Understanding the Dependency Inversion Principle (DIP) with TypeScript and Node.js](https://medium.com/@bubstack/understanding-the-dependency-inversion-principle-dip-with-typescript-and-node-js-a8c86f55f223)

</aside>

놀랍게도 `테스트 케이스를 만들고 계속 돌려라` 라는 간단하고 단순한 규칙을 따르면 시스템은 **낮은 결합도와 높은 응집력이**라는, **객체 지향 방법론이 지향하는 목표를 저절로 달성**한다.

### 단순한 설계 규칙 2~4: 리팩터링

테스트 케이스를 모두 작성했다면 이제 코드와 크래스를 정리해도 괜찮다. 구체적으로는 코드를 점진적으로 리팩터링 해나간다.

**코드를 정리하면서 시스템이 깨질까 걱정할 필요가 없다. 테스트 케이스가 있기 때문에**

리팩터링 단계에서는 소프트웨어 설계 품질을 높이는 기법이라면 무엇이든 적용해도 괜찮다. 응집도를 높이고 결합도를 낮추고, 관심사를 분리하고, 시스템 관심사를 모듈로 나누고, 함수와 클래스 크기를 줄이고, 더 나은 이름을 선택하는 등 다양한 기법을 동원한다.

### 중복을 없애라

우수한 설계에서 중복은 커다란 적이다. `추가작업` `추가 위험` `불필요한 복잡도` 를 뜻하기 때문이다.

똑같은 코드는 당연히 중복이다. 비슷한 코드는 더 비슷하게 고쳐주면 리팩터링이 쉬워진다.

- 각 메서드를 따로 구현하는 방법도 있다. 하지만 size()가 개수를 반환하는 로직이다.

```java
    int size() {}
    boolean isEmpty{}
```

- 따라서 isEmpty는 이를 이용하면 코드를 중복해서 구현할 필요가 없어진다.

```java
   boolean isEmpty() {
      return 0 == size();
    }
```

- 중복 제거 전

```java
    public void scaleToOneDimension(float desiredDimension, float imageDimension) {
      if (Math.abs(desiredDimension - imageDimension) < errorThreshold)
        return;
      float scalingFactor = desiredDimension / imageDimension;
      scalingFactor = (float)(Math.floor(scalingFactor * 100) * 0.01f);

      RenderedOpnewImage = ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor);
      image.dispose();
      System.gc();
      image = newImage;
    }

    public synchronized void rotate(int degrees) {
      RenderedOpnewImage = ImageUtilities.getRotatedImage(image, degrees);
      image.dispose();
      System.gc();
      image = newImage;
    }
```

- scaleToOneDimension 메서드와 rotate 메서드를 살펴보면 일부 코드가 동일하다. 다음과 같이 코드를 정리해 중복을 제거한다.

```java
    public void scaleToOneDimension(float desiredDimension, float imageDimension) {
      if (Math.abs(desiredDimension - imageDimension) < errorThreshold)
        return;
      float scalingFactor = desiredDimension / imageDimension;
      scalingFactor = (float) Math.floor(scalingFactor * 10) * 0.01f);
      replaceImage(ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor));
    }

    public synchronized void rotate(int degrees) {
      replaceImage(ImageUtilities.getRotatedImage(image, degrees));
    }

    private void replaceImage(RenderedOpnewImage) {
      image.dispose();
      System.gc();
      image = newImage;
    }
```

아주 적은 양이지만 공통적인 코드를 새 메서드로 뽑고 보니 클래스가 SRP를 위반한다. 따라서 새로만든 `replaceImage` 메서드를 다르 클래스로 옮겨도 좋다. 그러면 새 메서드의 가시성이 높아진다.

`소규모 재사용` 은 시스템 복잡도를 극적으로 줄여준다. 소규모 재사용을 재대로 익혀야 대규모 재사용이 가능하다.

**TEMPLATE METHOD** 패턴은 고차원 중복을 제거할 목적으로 자주 사용하는 기법이다.

```java
  public class VacationPolicy {
      public void accrueUSDDivisionVacation() {
        // 지금까지 근무한 시간을 바탕으로 휴가 일수를 계산하는 코드
        // ...
        // 휴가 일수가 미국 최소 법정 일수를 만족하는지 확인하는 코드
        // ...
        // 휴가 일수를 급여 대장에 적용하는 코드
        // ...
      }

      public void accrueEUDivisionVacation() {
        // 지금까지 근무한 시간을 바탕으로 휴가 일수를 계산하는 코드
        // ...
        // 휴가 일수가 유럽연합 최소 법정 일수를 만족하는지 확인하는 코드
        // ...
        // 휴가 일수를 급여 대장에 적용하는 코드
        // ...
      }
    }
```

- 템플릿 메서드 적용

```java
    abstract public class VacationPolicy {
      public void accrueVacation() {
        caculateBseVacationHours();
        alterForLegalMinimums();
        applyToPayroll();
      }

      private void calculateBaseVacationHours() { /* ... */ };
      abstract protected void alterForLegalMinimums();
      private void applyToPayroll() { /* ... */ };
    }

    public class USVacationPolicy extends VacationPolicy {
      @Override protected void alterForLegalMinimums() {
        // 미국 최소 법정 일수를 사용한다.
      }
    }

    public class EUVacationPolicy extends VacationPolicy {
      @Override protected void alterForLegalMinimums() {
        // 유럽연합 최소 법정 일수를 사용한다.
      }
    }
```

### 표현하라

`자신이` 이해하는 코드를 짜기는 쉽다. 코드를 짜는 동안에는 문제에 빠져 코드를 구석구석 이해하니까. 하지만 나중에 코드를 유지보수할 사람이 코드를 짜는 사람만큼이나 문제를 깊이 이해할 가능성은 희박하다.

소프트웨어 프로젝트 비용 중 대다수는 장기적인 유지보수에 들어간다. 코드를 변경하면서 버그의 싹을 심지 않으려면 유지보수 개발자가 시스템을 제대로 이해해야 한다.

그러므로 코드는 개발자의 의도를 분명히 표현해야 한다. 개발자가 코드를 명백하게 짤수록 다른 사람이 그 코드를 이해하기 쉬워진다.

1. **좋은 이름을 선택한다.**
2. **함수와 클래스 크기를 가능한 줄인다.**
3. **표준 명칭을 사용한다.**
4. **단위 테스트 케이스를 꼼꼼히 작성한다.**

표현력을 높이는 가장 중요한 방법은 `노력` 이며, 자신의 작품을 조금 더 사랑하는 것이 중요하다.

### 클래스와 메서드 수를 최소로 줄여라

때로는 무의미하고 독당적인 정책 탓에 클래스 수와 메서드 수가 늘어나기도 한다. 클래스마다 무조건 인터페이스를 생성하라고 요구하는 구현 표준이 좋은 예다.

목표는 함수와 클래스 크기를 작게 유지하면서 동시에 시스템 크기도 작게 유지하는 데 있다. 하지만 이는 간단한 설계 규칙 네 개 중 **우선순위가 가장 낮다**. 다시말해 수를 줄이는 작업도 중요하지만, 테스트 케이스를 만들고 중복을 제거하고 의도를 표현하는 작업이 더 중요하다.
