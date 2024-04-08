---
highlighter: prism 
---
# 의미 있는 이름

1. 코드는 문서
2. 의사소통
3. 직관성

---

## 의도를 분명하게 밝혀라

### AS-IS

```java
private static void append(String s, int length, ByteWriter writer) {
    for (int i = 0; i < length; ++i) {
        int d = Character.digit(s.charAt(i), 10);
        if (d == -1) {
            throw new IllegalArgumentException();
        }

        writer.write(d);
    }

    writer.reset();
}
```

---

### TO-BE

```java
private static void append(String cardNumber, int length, ByteWriter writer) {
    for (int i = 0; i < length; ++i) {
        int digit = Character.digit(cardNumber.charAt(i), 10);
        boolean isInvalid = digit == -1

        if (isInvalid) {
            throw new IllegalArgumentException();
        }

        byteWriter.write(digit);
    }

    byteWriter.reset();
}
```

---

## 그릇된 정보를 피하라

1. 불용어(필요 없는 단어)를 사용하지 말 것

```js
arr, list, data, a1, a2, userInfo
```

```js
getUser()
getUserInfo()
```

---

## 의미있게 구분하라

```ts
const user = ...
const userData = ...
const userInfo = ...

const country = ...
const countryData = ...
const nation = ...
```

---

## 발음하기 쉬운 이름를 사용하라

```ts
// generate date (year, month, day, hour, minute, second)

const genymdhms = ...
const modymdhms = ...

const generationTimeStamp = ...
const modificationTimeStamp = ..
```

---

## 검색하기 쉬운 이름을 사용하라

```java
private static void append(String s, int length, ByteWriter writer) {
    for (int i = 0; i < length; ++i) {
        int d = Character.digit(s.charAt(i), 10);
        if (d == -1) {
            throw new IllegalArgumentException();
        }

        writer.write(d);
    }

    writer.reset();
}
```

---

## 인코딩을 피하라

- 헝가리안 표기법

```js
// Unsafe 값을 Safe 변수에 바로 넣었다. 양변의 의미가 다르므로 문제가 있는 코드
let sValue = usValue; 

// us값을 체크해서 s로 바꿔주는 함수 sCheck를 거쳤다. 양변이 모두 s이므로 문제없는 코드
sValue = sCheck(usValue); 

// 의미는 맞지만 Locked 함수를 쓰기 전에 락을 걸지 않았다. 멀티스레드에서 동작시 문제가 생길 수 있다.
sValue = sCheckLocked(usValue); 
```

- 접두어

```java
public class Part {
  private String m_dsc;

  ...
}

public class Part {
  private String description;

  ...
}
```

---

## 자신의 기억력을 자랑하지 마라

---

## 클래스 이름

- 명사형로 사용할 것

```js
Job, Gender, Purpose
```

## 메서드 이름

- 동사형 사용할 것

```js
getCalcRate()
getFee()
calcFee()
```

---

## 기발한 이름은 피하라

---

## 한 개념에 한 단어를 사용하라

---

## 말 장난을 하지마라

```js
function add(a, b) {
    return a + b
}

function add(a, b){ // insert or append
    return a.append(a, b)
}
```

## 해법 영역에서 가져온 이름을 사용하라

```java
    @Override
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void sendQueueEduTx(EduTx eduTx) {
        if (eduTx.getSend() == Country.KR) {
            try {
                long startMs = System.currentTimeMillis();
                String targetQueueName = getTargetQueueName();
                SendMessageRequest sendMessageRequest = new SendMessageRequest(targetQueueName, getEduTxSqsMessageBody(eduTx))
                        .withMessageGroupId(targetQueueName)
                        .withMessageDeduplicationId(UUID.randomUUID().toString());

                oldPrdQueueService.sendMessageAsync(sendMessageRequest);
                log.info("send Queue sendQueueEduTx workingTime:{}, queueData:{},", System.currentTimeMillis() - startMs, sendMessageRequest);
            } catch (Exception e) {
                log.error("sendQueueEduTx eduTxId({}) Exception:{}", eduTx.getId(), WbUtils.stacktrace(e));
                eduTx.syncFail();
            }
        }
    }

```

---

## 문제 영역에서 가져온 이름을 사용하라

---

## 의미 있는 맥락을 추가하라

```
firstName, lastName, street, houseNumber, city, state, zipcode

addrState
```

```js
 const isNotNullReceiveNo = this.tx.receiveNo !== null

 //to-be
 const isReceiveNoAvailable = this.tx.receiveNo !== null

 const canUseTxStatus = status === 'ACTIVE'

 //to-be

 const txAvailableStatus = status === 'ACTIVE'
```

## 불필요한 맥락을 없애라

--
