---
layout: post
title: "Spring 의존성 주입(Dependency Injection)과 제어의 역전(Inversion of Control)"
tags: ["java", "spring"]
comments: true
---

일반적으로 한 객체는 다른 객체에 의존적이다. 객체 지향 프로그래밍에서 유연성 있는 프로그램 개발을 위해 객체 간의 의존성을 관리하는 것은 매우 중요하다. 그렇다면 도대체 의존성은 무엇일까?

## 의존성(Dependency) 이해

임의의 배열을 정렬한 뒤 반환하는 서비스가 있다고 가정해보자.

```java
public class BusinessService {
  private SelectionSortServiceImpl sortService = new SelectionSortServiceImpl();

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

```java
public class SelectionSortServiceImpl {
  public int[] sort(int[] a) {
    // implement selection sort algorithm
  }
}
```

`BusinessService`는 `SelectionSortServiceImpl`에 의존적이며 매우 강하게 결합되어 있다. 만약 `SelectionSortServiceImpl`가 `InsertionSortServiceImpl`로 바뀐다면 어떻게 될까?

```java
public class BusinessService {
  // private SelectionSortServiceImpl sortService = new SelectionSortServiceImpl();
  private InsertionSortServiceImpl sortService = new InsertionSortServiceImpl();

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

`BusinessService`를 직접 수정하지 않고는 해결할 수가 없다. 이는 변경에 닫혀 있고 확장에 열려 있다는 `OCP` 원칙에 어긋난다. 어떻게 하면 둘 사이의 결합을 느슨하게 관리할 수 있을까? 인터페이스를 선언하고 이를 확장하는 구조로 두 객체 간의 관계를 느슨하게 만들어보자.

```java
public interface SortService {
  int[] sort(int[] a);
}
```

```java
public class SelectionSortServiceImpl implements SortService {
  @Override
  public int[] sort(int[] a) {
    // implement selection sort algorithm
  }
}
```

```java
public class InsertionSortServiceImpl implements SortService {
  @Override
  public int[] sort(int[] a) {
    // implement insertion sort algorithm
  }
}
```

`BusinessService`에는 `SortService`를 할당할 수 있는 `Setter` 메서드를 정의함으로써 확장적인 구조를 가져갈 수 있다.

```java
public class BusinessService {
  private SortService sortService;

  public void setSortService(SortService sortService) {
    this.sortService = sortService;
  }

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

`BusinessService`를 활용한 `Main` 예제를 살펴보자.

```java
public class Main {
  public static void main(String[] args) {
    BusinessService businessService = new BusinessService();
    businessService.setSortService(new SelectionSortServiceImpl());
    businessService.sort(new int[]{1, 2, 3});
  }
}
```

`SortService`를 확장한 새로운 클래스가 등장하더라도 `BusinessService`는 수정할 필요가 없다. 사용자는 필요에 따라 원하는 정렬 알고리즘을 할당하여 서비스를 이용할 수 있게 된다.

## 빈(Bean)과 스프링 컨테이너

스프링(`Spring`)은 클래스를 빈(`Bean`)으로 관리한다. 위 예제에 사용된 클래스를 빈으로 등록하고 관리하려면 어떻게 해야 할까?

`@Component` 어노테이션은 스프링 컨테이너에게 빈으로 등록해야 하는 클래스임을 알려줄 수 있다. 물론 특성에 따라 `@Controller`, `@Service`, `@Repository` 어노테이션을 활용하는 것이 일반적이지만 여기서는 `@Component`를 사용한다.

`BusinessService`와 `SelectionSortServiceImpl`에 `@Component` 어노테이션을 붙여 빈으로 등록해보자.

```java
@Component
public class BusinessService {
  private SortService sortService;

  public void setSortService(SortService sortService) {
    this.sortService = sortService;
  }

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

```java
@Component
public class SelectionSortServiceImpl implements SortService {
  public int[] sort(int[] a) {
    // implement selection sort algorithm
  }
}
```

어떤 클래스를 빈으로 등록해야 하는지 표시를 해두었다. 스프링 컨테이너는 어떻게 빈으로 등록할 클래스를 찾을 수 있을까?

`@Configurable` 어노테이션은 해당 클래스가 스프링의 환경 설정과 관련되어 있다는 것을 나타낸다. 또한 `@ComponentScan` 어노테이션을 통해 특정 패키지 하위에 있는 클래스 중 표시가 있는 클래스를 찾아 빈으로 등록할 수 있도록 돕는다.

```java
@Configurable
@ComponentScan(basePackages = "com.example")
public class AppContext {

}
```

스프링은 빈 팩토리(`Bean Factory`)에 저장된 빈을 찾아 쓸 수 있도록 `ApplicationContext`와 같은 훌륭한 추상 객체를 지원한다.

```java
public class Main {
  public static void main(String[] args) {
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppContext.class);
    BusinessService businessService = applicationContext.getBean("businessService", BusinessService.class);
    businessService.setSortService(new SelectionSortServiceImpl());
    businessService.sort(new int[]{1, 2, 3});
  }
}
```

## 의존성 주입(Dependency Injection)과 제어의 역전(Inversion of Control)

지금까지 예제는 여전히 개발자가 직접 의존성을 관리하고 있다. 의존성을 관리하는 주체가 스프링이 되도록 할 수 있을까?

`@Autowired` 어노테이션은 빈을 생성하는 과정에서 자동으로 의존성을 주입할 수 있게 도와준다. `BusinessService` 클래스 `Setter` 메서드에 `@Autowired` 어노테이션을 추가하면 `BusinessService` 클래스를 빈으로 등록하는 과정에서 자동으로 `SortService`에 적절한 객체가 할당된다.

```java
@Component
public class BusinessService {
  private SortService sortService;

  @Autowired
  public void setSortService(SortService sortService) {
    this.sortService = sortService;
  }

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

사실 `Setter` 메서드를 정의할 필요 없이 변수에 `@Autowired` 어노테이션을 추가하여 더 간단히 구현할 수도 있다.

```java
@Component
public class BusinessService {
  @Autowired
  private SortService sortService;

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

`@Autowired` 어노테이션은 생성자를 통해서도 구현할 수 있다.

```java
@Component
public class BusinessService {
  private SortService sortService;

  @Autowired
  BusinessService(SortService sortService) {
    this.sortService = sortService;
  }

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

`Setter` 메서드를 활용하든 생성자를 활용하든 상관은 없지만 생성자를 활용하면 의존적인 클래스가 늘어날 때마다 생성자를 매번 정의해줘야 해서 개인적으로는 변수에 직접 추가하는 것을 선호한다.

`Main` 예제에서 `setSortService()` 메서드 부분을 지워보자.

```java
public class Main {
  public static void main(String[] args) {
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppContext.class);
    BusinessService businessService = applicationContext.getBean("businessService", BusinessService.class);
    businessService.sort(new int[]{1, 2, 3});
  }
}
```

`setSortService()` 메서드를 사용하지 않았지만 정상적으로 동작한다. 스프링 컨테이너가 `BusinessService` 클래스를 빈으로 등록하는 과정에서 자동으로 의존성을 주입했기 때문이다. 이처럼 의존성 주입의 주체가 개발자가 아닌 스프링 컨테이너가 가져가는 것을 제어의 역전(`Inversion of Control`)이라고 한다. 그래서 스프링 컨테이너를 `IoC` 컨테이너라고도 부른다.

이번엔 `InsertionSortServiceImpl`에도 `@Component` 어노테이션을 붙여보자.

```java
@Component
public class InsertionSortServiceImpl implements SortService {
  public int[] sort(int[] a) {
    // implement insertion sort algorithm
  }
}
```

다시 `Main` 예제를 실행하면 에러가 발생할 것이다. 이는 `SortService`에 할당될 수 있는 객체가 2개 이상이기 때문이다. 이럴 경우에는 어떻게 문제를 해결할까?

### @Primary

`@Primary` 어노테이션은 의존성을 주입해야 할 객체가 2개 이상일 때 어떤 객체를 우선적으로 주입할 것인지 명시해줄 수 있다.

```java
@Component
@Primary
public class SelectionSortServiceImpl implements SortService {
  public int[] sort(int[] a) {
    // implement selection sort algorithm
  }
}
```

### @Qualifier

`@Qualifier` 어노테이션은 빈에 이름을 명명하고 명시적으로 어떤 객체를 주입할 것인지 정할 수 있다.

```java
@Component
@Qualifier("selectionSortService")
public class SelectionSortServiceImpl implements SortService {
  public int[] sort(int[] a) {
    // implement selection sort algorithm
  }
}
```

```java
@Component
public class BusinessService {
  @Autowired
  @Qualifier("selectionSortService")
  private SortService sortService;

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

## References

- [Mastering-Spring-5.0](https://github.com/PacktPublishing/Mastering-Spring-5.0)
