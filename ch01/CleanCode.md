# 깨끗한 코드

- 나쁜 코드는 개발 속도를 크게 떨어트린다. 코드를 고칠 때 마다 엉뚱한 곳에서 문제가 생긴다.
- 나쁜 코드는 읽기 어려운 코드. 어떤 의도로 코드를 작성했는지 이해하기 어려운 코드.
- 여러 외부 요인으로 인해 나쁜코드가 생겼다고 해도 그건 모두 프로그래머에 잘못이다
  좋은 코드를 사수하는 일은 바로 프로그래머의 책임이다.
- 깨끗한 코드와 나쁜코드를 구분할 줄 안다고 깨끗한 코드를 작성할 줄 안다는 뜻은 아니다. 힘겹게 스득한 감각을 활용해 자잘한 기법들을 적용하는 절제와 규율이 필요하다.
- 그때그때 해결하지 못한 나쁜 코드들은 추후 리팩토링을 하기 위한 일정이 필요해 당장 해야 하는 업무를 진행하지 못 하는 문제가 생긴다.

> 르블랑의 법칙 (Leblance's Law)
>
> - Later equals never 나중은 결코 오지 않는다. 한번 작성한 쓰레기 코드를 나중에 수정하는 일은 결코 없다.

---

> 깨끗한 코드

비야네 스트롭스트룹 (C++ 창시자)

- 우아하고 효울적인 코드. 논리가 간단해서 오류가 숨어들지 못한다.
- 유혹에 빠지지 않는다. 나쁜 코드는 나쁜 코드를 만든다. (깨진 창문 비유)

- 철저한 오류 처리.
- 깨끗한 코드, 코드는 한 가지를 제대로 수행한다.

그래디 부치

- 깨끗한 코드는 단순하고 직접적이다.(가독성)

데이브 토마스

- 깨끗한 코드는 작성자가 아닌 사람도 읽기 쉽고 고치기 쉽다.
- 단위 테스트와 인수 테스트 케이스가 존재한다.

마이클 페더스

- 주의 깊게 짯다는 느낌을 준다. 작성자가 이미 모든 사항을 고려했으며, 고칠 곳이 없는 코드.

론 제프리스

- 모든 테스트를 통과한다.
- 중복이 없다.
  - 집합을 추상화하면 진짜 문제에 신경 쓸 여유가 생긴다. 간단한 찾기 기능이 필요한데 온갖 집합 기능을 구현하느라 시간과 노력을 낭비할 필요가 없어진다.
- 시스템 내 모든 설계 아이디어를 표현한다.
- 클래스, 메서드, 함수 등을 최대한 줄인다.
  - 객체가여러 기능을 수행한다면 여러 객체로 나눈다

워드 커닝햄

- 코드를 독해하느라 머리를 쮜어짤 필요가 없다.

보이스카우트 규칙

- 시간이 자나면서 엉망으로 전략하는 코드가 한둘이 아니다, 한꺼번에 많은 시간과 노력을 투자해 코드를 정리할 필요가 없다.

---

- 읽기 좋은코드
- 한가지 역할
- 중복이 없는 코드

---

```java
// 계좌 객체
public class Account {
    private String accountNumber;
    private BigDecimal balance;

    public Account(String accountNumber) {
        this.accountNumber = accountNumber;
        this.balance = BigDecimal.ZERO; // 초기 잔액은 0으로 설정
    }

    // 잔액 조회 메서드
    public BigDecimal getBalance() {
        return balance;
    }

    // 계좌 번호 조회 메서드
    public String getAccountNumber() {
        return accountNumber;
    }

    public void setAccountNumber(String accountNumber) {
        this.accountNumber = accountNumber;
    }

    public void setBalance(BigDecimal balance) {
        this.balance = balance;
    }
}

```

```java
// 송금 서비스 객체
class TransferRequest {
    private Account sender;
    private Account receiver;
    private BigDecimal amount;

    public TransferRequest(Account sender, Account receiver, BigDecimal amount) {
        this.sender = sender;
        this.receiver = receiver;
        this.amount = amount;
        isValidTransfer();
    }

    public Account getSender() {
        return sender;
    }

    public Account getReceiver() {
        return receiver;
    }

    public BigDecimal getAmount() {
        return amount;
    }

    private void isValidTransfer() {
        if (sender == null || receiver == null) {
            throw new IllegalArgumentException("계좌 정보가 올바르지 않습니다.");
        }

        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("송금액은 양수이어야 합니다.");
        }

        if (sender.getBalance().compareTo(amount) < 0) {
            throw new IllegalArgumentException("송금할 금액이 출금 계좌의 잔액보다 많습니다.");
        }
    }
}

```

```java
// 입금 서비스 클래스 정의
class DepositService {
    public void deposit(Account account, BigDecimal amount) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("입금액은 양수이어야 합니다.");
        }

        account.setBalance(account.getBalance().add(amount));
    }
}

```

```java
// 출금 서비스 클래스 정의
class WithdrawService {
    public void withdraw(Account account, BigDecimal amount) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("출금액은 양수이어야 합니다.");
        }

        if (account.getBalance().compareTo(amount) < 0) {
            throw new IllegalArgumentException("잔액이 부족합니다.");
        }

        account.setBalance(account.getBalance().subtract(amount));
    }
}

```

```java
// 송금 서비스 클래스 정의
class TransferService {
    public void transfer(TransferRequest transferRequest) {
        Account sender = transferRequest.getSender();
        Account receiver = transferRequest.getReceiver();
        BigDecimal amount = transferRequest.getAmount();

        // 출금
        WithdrawService withdrawService = new WithdrawService();
        withdrawService.withdraw(sender, amount);

        // 입금
        DepositService depositService = new DepositService();
        depositService.deposit(receiver, amount);

        System.out.println("송금이 완료되었습니다.");
    }
}

```

```java
    public static void main(String[] args) {
        // 송신자와 수신자 계좌 생성
        Account senderAccount = new Account("123456");
        Account receiverAccount = new Account("654321");

        // 송금 서비스 실행
        TransferRequest transferRequest = new TransferRequest(senderAccount, receiverAccount, new BigDecimal("100"));
        TransferService transferService = new TransferService();
        transferService.transfer(transferRequest);
    }

```
