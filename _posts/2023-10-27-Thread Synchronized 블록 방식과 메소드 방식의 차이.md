---
category: Java
tags: [java, thread]
---


## 차이점

Thread synchronization은 여러 스레드가 공유된 리소스에 동시에 접근하는 것을 조절하기 위해 사용됩니다. synchronized 블록 방식과 synchronized 메소드 방식은 둘 다 스레드 동기화를 달성하는 데 사용되지만, 구현 방식에 약간의 차이가 있습니다.

1. synchronized 블록 방식:
  - synchronized 키워드를 사용하여 특정 블록을 동기화합니다.
  - 특정 블록 내에서만 스레드 동기화가 보장됩니다.
  - 다른 스레드는 해당 블록이 잠길 때까지 대기해야 합니다.
  - 여러 개의 synchronized 블록을 사용하여 다중 리소스에 대한 동기화를 구현할 수 있습니다.

```java
synchronized (lockObject) {
    // 동기화가 필요한 코드
}
```

2. synchronized 메소드 방식:
  - 메소드 전체를 동기화합니다.
  - 메소드 내의 모든 코드가 동기화됩니다.
  - 다른 스레드는 해당 메소드가 잠길 때까지 대기해야 합니다.
  - 단일 리소스에 대한 동기화에 적합합니다.

```java
public synchronized void myMethod() {
    // 동기화가 필요한 코드
}
```

두 방식 모두 스레드 동기화를 달성할 수 있지만, synchronized 메소드 방식은 메소드 전체를 동기화하므로 코드가 단순해지고 실수를 줄일 수 있습니다. 그러나 여러 리소스에 대한 동기화가 필요한 경우 synchronized 블록 방식을 사용해야 할 수도 있습니다.

스레드 동기화에 대한 자세한 내용은 자바의 스레드 동기화 및 관련된 개념인 모니터, 락 등에 대해 공부하시는 것을 추천드립니다.

## 예제 코드

아래는 synchronized 블록 방식과 synchronized 메소드 방식의 간단한 예제 코드입니다.

1. synchronized 블록 방식 예제 코드:

```java
public class SynchronizedExample {
    private final Object lockObject = new Object();
    private int count = 0;
    
    public void increment() {
        synchronized (lockObject) {
            count++;
        }
    }
    
    public int getCount() {
        synchronized (lockObject) {
            return count;
        }
    }
}
```

2. synchronized 메소드 방식 예제 코드:

```java
public class SynchronizedExample {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```

위의 예제 코드에서는 `count`라는 공유 변수를 `increment()` 메소드로 증가시키고, `getCount()` 메소드로 값을 반환하는 간단한 클래스를 보여주고 있습니다. synchronized 블록 방식에서는 `synchronized` 키워드를 사용하여 `count` 변수에 접근하는 부분을 동기화하고 있습니다. synchronized 메소드 방식에서는 메소드 자체에 `synchronized` 키워드를 사용하여 메소드 전체를 동기화하고 있습니다.

이 예제 코드를 참고하여 스레드 동기화를 실제로 구현해보시면 됩니다.
