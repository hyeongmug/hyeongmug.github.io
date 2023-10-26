---
category: Java
tags: [java, thread]
---


## wait() 메서드와 notify(), notifyAll() 메서드의 상호관계

`wait()`, `notify()`, `notifyAll()`은 Java에서 스레드 간의 상호작용을 위해 사용되는 메서드입니다. 이들 메서드는 `Object` 클래스에 정의되어 있으며,  `synchronized` 키워드를 사용하여 동기화된 메서드 또는 블록 내에서 호출해야 합니다.

- `wait()`: 스레드가 대기 상태로 들어가며, 다른 스레드에 의해 `notify()` 또는 `notifyAll()`이 호출될 때까지 기다립니다. `wait()`을 호출하면 현재 스레드가 해당 객체의 모니터 락을 놓고 대기하게 됩니다.
- `notify()`: 대기 중인 스레드 중 하나를 임의로 선택하여 깨웁니다. 선택된 스레드는 실행 가능한 상태가 되지만, 모니터 락을 얻기 위해 경쟁해야 합니다. 여러 개의 스레드가 대기 중인 경우, 어떤 스레드가 깨어날지는 알 수 없습니다.
- `notifyAll()`: 대기 중인 모든 스레드를 깨웁니다. 대기 중인 모든 스레드가 실행 가능한 상태가 되지만, 모니터 락을 얻기 위해 경쟁해야 합니다.

`notify()`와 `notifyAll()`은 `wait()`을 호출한 스레드를 깨울 수 있습니다. 그러나 `notifyAll()`은 모든 대기 중인 스레드를 깨우므로, 경쟁 상황에서 성능 저하를 초래할 수 있습니다. 따라서, 필요한 경우에만 `notify()`를 사용하는 것이 좋습니다.

또한, `wait()`, `notify()`, `notifyAll()`은 반드시 동기화 블록 내에서 호출되어야 하며, 해당 객체의 모니터 락이 소유되어야 합니다. 그렇지 않으면 `IllegalMonitorStateException`이 발생합니다.

## 예시코드

아래는 `wait()` 메서드와 `notifyAll()` 메서드를 사용하는 예시 코드입니다:

```java
  
import org.junit.Test;  
  
import java.util.ArrayList;  
import java.util.List;  
  
class TestLibrary {  
  
    public List<String> books = new ArrayList<>();  
    private static TestLibrary library = null;  
  
    public static TestLibrary getLibrary() {  
  
        if (library == null) {  
            library = new TestLibrary();  
        }  
  
        return library;  
    }  
  
  
    public TestLibrary() {  
        books.add("자바의 정석1");  
        books.add("자바의 정석2");  
        books.add("자바의 정석3");  
    }  
  
  
    public synchronized String rendBook() throws InterruptedException {  
  
        Thread currentThread = Thread.currentThread();  
        while (books.isEmpty()) { // 스레드가 깨어 난 후에도 books가 비어있을 수 있기 때문에 while문으로 감싸줌
            System.out.println(currentThread.getName() + " waiting start");  
            wait(); // 스레드 대기
            System.out.println(currentThread.getName() + " waiting end");  
        }  
  
        if (!books.isEmpty()) {  
            String book = books.remove(0);  
            System.out.println(currentThread.getName() + ": " + book + " rend ");  
            return book;  
        } else return null;  
  
    }  
  
    public synchronized void returnBook(String book) {  
        books.add(book);  
        notifyAll(); // 대기중인 스레드를 깨움
        Thread currentThread = Thread.currentThread();  
        System.out.println(currentThread.getName() + ": " + book + " return ");  
    }  
  
}  
  
class Student extends Thread {  
  
    public Student(String name) {  
        super(name);  
    }  
  
    @Override  
    public void run() {  
  
        try {  
            TestLibrary library = TestLibrary.getLibrary();  
            String title = library.rendBook();  
            if (title == null) {  
                System.out.println("빌리지 못함");  
                return;            }  
            Thread.sleep(5000);  
            library.returnBook(title);  
        } catch (InterruptedException e) {  
            System.out.println(e);  
        }  
    }  
}  
  
public class ThreadWaitTest {  
  
    @Test  
    public void test() {  
  
        Student student1 = new Student("학생1");  
        Student student2 = new Student("학생2");  
        Student student3 = new Student("학생3");  
        Student student4 = new Student("학생4");  
        Student student5 = new Student("학생5");  
        Student student6 = new Student("학생6");  
  
        student1.start();  
        student2.start();  
        student3.start();  
        student4.start();  
        student5.start();  
        student6.start();  
  
        while (true); // 스레드 테스트이기 때문에 메인 스레드가 종료되지 않도록 함  
    }  
}
```

위의 예시 코드는 도서관을 구현한 `TestLibrary` 클래스와, 도서관에서 책을 빌리는 학생을 구현한 `Student` 클래스, 그리고 테스트를 위한 `ThreadWaitTest` 클래스로 구성되어 있습니다.
책 대여시 `rendBook()` 메서드를 호출하면, 도서관에 책이 없으면 대기 상태로 들어가고, 책이 있으면 책을 빌립니다. 책 반납시 `returnBook()` 메서드를 호출하면, 책을 반납하고 대기 중인 스레드를 깨웁니다.

결과:
```
학생2: 자바의 정석1 rend
학생1: 자바의 정석2 rend
학생3: 자바의 정석3 rend
학생4 waiting start
학생5 waiting start
학생6 waiting start
학생2: 자바의 정석1 return
학생4 waiting end
학생4: 자바의 정석1 rend
학생3: 자바의 정석3 return
학생1: 자바의 정석2 return
학생6 waiting end
학생6: 자바의 정석3 rend
학생5 waiting end
학생5: 자바의 정석2 rend
```
