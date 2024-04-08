# [클린코드] chap11 시스템

### # 도입

- 우리 회사를 생각해보자.
- 온갖 세세한 사항을 혼자 직접 관리하는 건 불가능
  - 나 혼자 모든 API, UI 개발, 유지보수, 고객 대응, 재무, 회계, 컴플라이언스 업무를 하는 것은 극 불가능
- 와바 서비스는 잘 돌아감.
  - 각 부분을 맡은 직원들이 잘 해주고 있기 때문.
  - 각 팀이 해야하는 일들이 적절하게 구분되어 있고, 복잡한 업무를 요약해서 타팀과 공유가 되기 때문.
    - 여기서 `적절하게 구분`하는걸 **~모듈화~**, 공유를 보다 쉽게 하기 위해 `업무를 요약`하는 것을 **~추상화~**라고 이해할 수 있다.
- 근데 막상 시스템은 관심사 분리가 잘 되어 있거나 추상화를 이뤄내지 못함.
  - `어떻게 해야 관심사의 분리, 추상화를 잘 할 수 있을까?` 에 대한 내용이 11장에서 주로 다루는 내용.

### # 시스템 제작과 사용을 분리하기

- ~~제작과 사용~~ `생성`과 `사용`을 분리해야함.
  - 제작과 사용이라고 했지만, `객체 생성`과 `사용`으로 생각하는 것이 더 이해하기 좋을 듯.
  - 둘은 목적이 다르기 때문임.

```java
class UserService {
	public int getUserAgeById(int id) {
		// 객체 생성
		UserRepository userRepository = new UserRepository();

		// 객체 사용
		User user = userRepository.findById(id);
		return user.age;
	}
}
```

- `getUserById`라는 메소드는 현재 두 가지 역할을 하고 있음.
  - userRepository 생성
  - 유저 찾기
    **`-> SRP 위반`**

생성과 사용을 분리하는 다양한 방법을 소개함.

1. Main 분리
   - 생성 로직은 main함수나 main 내부의 모듈에서 관리.
     - 객체를 생성해서 넘겨줌
   - 사용 로직이 있는 app은 객체를 넘겨 받은 후, 할 일을 함.
     - `app은 생성 로직 모름 = 관심 없음 -> 관심사 분리`
2. 팩토리
   - 1번 방법에서는 app이 생성 로직에 대해서 아는 것(관심)이 없었음.
   - 객체가 생성되는 시점을 app이 결정해야할때 사용함.
     - 객체 생성 시점을 app이 결정하지만, 생성 로직은 모름
     - 이때 사용하는 것이 `추상 팩토리 패턴`
       - DIP
3. 의존성 주입

   - 제어의 역전(IoC)을 의존성 관리에 적용
     - 제어의 역전(IoC) - 생성 로직을 가지고 있던 클래스가 다른 모듈로 위임함.
   - 대표적인 예가 스프링
   - 생성자 호출하는 로직이 변경되어도 UserService 입장에서는 관심 밖 (개발자 관점에서 보는 글도 있었음)

   ```java
   // AS-IS
   class UserService {
   	public final UserRepository userRepository = new UserRepository();
   	// ...
   }


   // TO-BE
   class UserService {
   	public final UserRepository userRepository;
   	// ...
   }
   ```
