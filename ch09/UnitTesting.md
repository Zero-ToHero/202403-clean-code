**TDD**

- TDD란 Test Driven Development의 약자로 ‘테스트 주도 개발
- TDD와 일반적인 개발 방식의 가장 큰 차이점은 테스트 코드를 작성한 뒤에 실제 코드를 작성.

TDD 법칙 세가지

- 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
- 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
- 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

깨끗한 테스트 코드 유지하기

- 테스트 코드가 지저분할수록 변경하기 어려워진다.
- 테스트 코드가 복잡할수록 실제 코드를 짜는 시간보다 테스트 코드를 추가하는 시간이 더 걸린다. 지저분한 코드로 인해 테스트 코드는 계속 늘어나는 부담이 된다.
- 테스트 코드는 실제 코드 못지 않게 중요하다.
- 코드에 유연성, 유지보수성, 재사용성을 제공하는 버팀목이 단위테스트다. 테스트 케이스가 없다면 모든 변경이 잠정적인 버그다.
- 테스트 코드는 가독성이 중요하다 **_BUILD_**, **_OPERATE_**, **_CHECK_** 잡다한 코드를 제거하고 진짜 필요한 자료 유형과 함수만 제공한다.

이중표준

- 테스트 API코드에 적용하는 표준은 실제 코드에 적용하는 표준과 확실히 다르다.
- 단순하고, 간결하고, 표현력이 풍부해야 하지만, 실제 코드만큼 효율적일 필요는 없다
- 실제 환경에서는 절대로 안 되지만 테스트 환경에서는 전혀 문제없는 방식이 있다. 이것이 이중 표준의 본질

Assert문 하나

- assert가 하나라면 결론이 하나기 때문에 코드를 이해하기 빠르고 쉽다.
- 단지 assert 문 개수는 최대한 줄여야 좋다는 생각이다.

```java
@Test
public void turnOnLoTempAlarmAtThreashold() throws Exception {
  hw.setTemp(WAY_TOO_COLD);
  controller.tic();
  assertTrue(hw.heaterState());
  assertTrue(hw.blowerState());
  assertFalse(hw.coolerState());
  assertFalse(hw.hiTempAlarm());
  assertTrue(hw.loTempAlarm());
}
```

```java
@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
  wayTooCold();
  assertEquals("HBchL", hw.getState());
}
```

```java
public void testGetPageHierarchyAsXml() throws Exception {
  givenPages("PageOne", "PageOne.ChildOne", "PageTwo");

  whenRequestIsIssued("root", "type:pages");

  thenResponseShouldBeXML();
}
```

테스트당 개념 하나

- 여러 개념을 한 함수로 몰아넣으면 독자가 각 절이 거기에 존재하닌 이유와 각 절이 테스트하는 개념을 모두 이해해야 한다.
- 가장 좋은 규칙은 "개념 당 assert 문 수를 최소로 줄여라" 와 "테스트 함수 하나는 개념 하나만 테스트하라”

```java
public void testAddMonths() {
  SerialDate d1 = SerialDate.createInstance(31, 5, 2004);

  SerialDate d2 = SerialDate.addMonths(1, d1);
  assertEquals(30, d2.getDayOfMonth());
  assertEquals(6, d2.getMonth());
  assertEquals(2004, d2.getYYYY());

  SerialDate d3 = SerialDate.addMonths(2, d1);
  assertEquals(31, d3.getDayOfMonth());
  assertEquals(7, d3.getMonth());
  assertEquals(2004, d3.getYYYY());

  SerialDate d4 = SerialDate.addMonths(1, SerialDate.addMonths(1, d1));
  assertEquals(30, d4.getDayOfMonth());
  assertEquals(7, d4.getMonth());
  assertEquals(2004, d4.getYYYY());
}
```

FIRST 규칙

- 빠르게(Fast)
- 독립적으로(Independent)
  - 각 테스트는 서로 독립해야지 의존하면 안 된다. 의존하게 되면 하나가 실패하면 나머지도 잇달아 실패하므로 진단하기 어려워진다.
- 반복가능하게(Repeatable)
  - 어떤 환경에서도 반복 가능해야 한다.
- 자가검증(Self-Validating)
  - 테스트 여부는 성공 아니면 실패다.
- 적시에(Timely)
  - 테스트는 적시에 작성해야 한다. 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 작성한다.
