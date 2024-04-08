---
highlighter: prism 
---
# Class

1. SRP (Single Responsibility Principle) - 클래스는 단 하나의 책임만을 가져야 한다
2. DIP (Dependency Inversion Principle) - 변화하기 쉬운 것에 의존하지 말 것

---

1. 클래스를 만들 때 무조건 크기는 작아야한다. (함수를 작성할 때와 마찬가지로 작아야 한다. )

2. 작다의 기준을 함수에서는 물리적인 행의 개수, 크기로 측정했다. ( 기능의 수 )

3. 클래스에서의 작은 기준은 맡은 책임의 수로 측정한다.

4. 클래스가 작은 지 판별하기 어렵다면 해당 클래스를 설명할 때

if ( 만일 ), and ( 그리고 ), or ( ~하며 ), but ( 하지만 )을 제외하고 25단어 내외로 설명할 수 있어야 한다.

만약 ~하며( or )를 설명에 넣는다면 책임이 많다는 증거이다.

---

## User Class - 725 Line

![user_detail](/image2.png)

---

## User Detail Class - 783 Line

![user](/image1.png)

---

## 단일 책임 원칙

### 클래스는 단 하나의 책임만을 가져야 한다를 의미하는 규칙이다

* ( 하나의 책임은 클래스를 변경할 이유가 하나임을 의미 )

```java
// as-is -> 경직된 클래스
class User{
    constructor(name: string){ ... }
    getUserName() { ... }
    saveUser(a: User) { ... }
}




//to-be --> 응집된 클래스
class User{
    constructor(name: string){ ... }
    getUserName() { ... }
}

class UserDB {
    getUser(a: User) { ... }
    saveUser(a: User) { ... }
}
```

## 응집도

1. 클래스(모듈)에 포함된 내부 변수(요소)들이 하나의 책임을 위해 연결되어 있는 정도를 말한다.

2. 일반적으로 메서드가 변수를 많이 사용할 수록 메서드와 클래스는 응집도가 높다.

3. 응집도가 높을 수록 유지보수가 쉽다. (낮을 수록 유지보수가 어렵다.)

```javascript

export const ROUTE_PAGE = {
  SIGNIN: "/",
  DASHBOARD: "/dashBoard",
  DETAIL: "/detail/:id",
  NOT_FOUND: "/:id",
};

export const INPUT_ELEMENT_TYPE = {
  TEXT: "text",
  PASSWORD: "password",
};

export const INPUT_ELEMENT_LABEL = {
  ID: "ID",
  PASSWORD: "Password",
  REMEMBER_ME: "Remember Me",
  SIGN_IN: "Sign In",
  SIGN_UP: "Sign Up",
  GO_BACK: "뒤로가기",
};
```

---

## 응집도를 유지하면 작은 클래스로 분리된다

---

## 변경하기 쉬운 클래스

깨끗한 시스템은 클래스를 체계적으로 정리해 코드 변경에 동반되는 위험도를 낮춘다.
