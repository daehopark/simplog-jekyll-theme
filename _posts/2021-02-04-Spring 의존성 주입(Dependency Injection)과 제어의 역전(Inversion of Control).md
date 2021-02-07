---
layout: post
title: "Spring 의존성 주입(Dependency Injection)과 제어의 역전(Inversion of Control)"
tags: ["java", "spring"]
comments: true
---

일반적으로 한 객체는 다른 객체에 의존적이다. 객체 지향 프로그래밍에서 유연성 있는 프로그램 개발을 위해 객체 간의 의존성을 관리하는 것은 매우 중요하다. 그렇다면 도대체 의존성은 무엇일까?

## 의존성(Dependency) 이해

임의의 배열을 받아 정렬한 뒤 반환해주는 사업 모델이 있다고 가정해보자. 코드로 구현하면 아래와 같다.

```java
public class BusinessService {
  private InsertionSortService sortService = new InsertionSortService();

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

```java
public class InsertionSortService {
  public int[] sort(int[] a) {
    // implement insertion sort
    return a;
  }
}
```

`BusinessService` 클래스는 `InsertionSortService` 클래스에 의존적이며 매우 강하게 결합되어 있다. 현 상황에서 삽입 정렬의 성능이 좋지 않은 것을 발견하고 선택 정렬로 구현체를 바꾼다면 코드가 어떻게 바뀌겠는가?

```java
public class BusinessService {
  // private InsertionSortService sortService = new InsertionSortService();
  private SelectionSortService sortService = new SelectionSortService();

  public int[] sort(int[] a) {
    return sortService.sort(a);
  }
}
```

`BusinessService` 클래스를 직접 수정하지 않고는 해결할 수가 없다. 이는 변경에 닫혀 있고 확장에 열려 있어야 한다는 `OCP` 원칙에 어긋난다.

어떻게 하면 두 클래스 사이의 결합을 느슨하게 관리할 수 있을까? 인터페이스를 사용하면 둘 사이의 관계를 훨씬 느슨하게 만들 수 있다.

```java
public interface SortService {
  int[] sort(int[] a);
}
```

```java
public class InsertionSortService implements SortService {
  public int[] sort(int[] a) {
    // implement insertion sort
    return a;
  }
}
```

```java
public class SelectionSortService implements SortService {
  public int[] sort(int[] a) {
    // implement insertion sort
    return a;
  }
}
```

`BusinessService` 클래스는 `SortService` 변수를 필드로 정의하고 `Setter` 메서드를 정의함으로써 확장적인 구조를 가져갈 수 있다.

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

`Main` 예제를 확인해보자.

```java
public class Main {
  public static void main(String[] args) {
    BusinessService businessService = new BusinessService();
    businessService.setSortService(new InsertionSortService());// or new SelectionSortService()
    businessService.sort(new int[]{1, 2, 3});
  }
}
```

`SortService` 인터페이스를 구현한 새로운 클래스가 등장하더라도 `BusinessService` 클래스를 직접 수정하는 일은 없을 것이다. 클라이언트는 필요에 따라 원하는 정렬 알고리즘을 할당하여 서비스를 이용할 수 있다.

## 스프링 컨테이너와 빈(Bean)

스프링에서 스프링 컨테이너에 의해 관리되는 자바 객체를 빈이라고 한다. 위 예제를 빈으로 등록하고 관리해보자.

자바 객체를 빈으로 등록하기 위해서는 스프링 컨테이너가 인지할 수 있도록 적절한 어노테이션을 추가해야 한다. 클래스의 특성에 따라 `@Controller`, `@Service`, `@Repository` 어노테이션을 활용하는 것이 일반적이지만 여기서는 `@Component` 어노테이션을 사용한다.

`BusinessService` 클래스와 `InsertionSortService` 클래스 위에 `@Component` 어노테이션을 추가하자.

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
public class InsertionSortService implements SortService {
  public int[] sort(int[] a) {
    // implement insertion sort
    return a;
  }
}
```

스프링 컨테이너는 어떻게 클래스를 찾아 빈으로 등록할까? `@Configuration` 어노테이션을 사용하면 해당 클래스가 스프링 구성 클래스임을 나타낸다. 또한 `@ComponentScan` 어노테이션을 사용하면 특정 패키지 하위에 빈으로 등록할 클래스를 찾아 스프링 컨테이너에 등록한다.

```java
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {

}
```

스프링 컨테이너에 등록된 빈 객체를 어떻게 사용할 수 있을까? `ApplicationContext`은 빈 팩토리에 저장된 빈 객체를 사용할 수 있도록 훌륭한 인터페이스를 제공한다.

```java
public class Main {
  public static void main(String[] args) {
    ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
    BusinessService businessService = context.getBean(BusinessService.class);
    // BusinessService businessService = new BusinessService();
    businessService.setSortService(new InsertionSortService());// or new SelectionSortService()
    businessService.sort(new int[]{1, 2, 3});
  }
}
```

## 의존성 주입(Dependency Injection)과 제어의 역전(Inversion of Control)

이제 의존성 주입에 대해 생각해보자. `BusinessService` 객체를 사용하는 클라이언트는 그 서비스가 어떻게 구성되어 있는지 알지 못해야 한다. 클라이언트는 서비스의 사용 방법만 알면 된다. 의존성 관리는 스프링 컨테이너에게 맡기면 된다.

`@Autowired` 어노테이션은 스프링 컨테이너가 자동으로 의존성을 주입할 수 있게 해준다. `BusinessService` 클래스 `Setter` 메서드에 `Autowired` 어노테이션을 추가해보자.

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

의존성 관리를 스프링 컨테이너에게 맡기기 위해 `Main` 예제에서 `setSortService()` 메서드를 사용하는 코드를 지우고 실행해보자.

```java
public class Main {
  public static void main(String[] args) {
    ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
    BusinessService businessService = context.getBean(BusinessService.class);
    // BusinessService businessService = new BusinessService();
    // businessService.setSortService(new InsertionSortService());// or new SelectionSortService()
    businessService.sort(new int[]{1, 2, 3});
  }
}
```

정상적으로 동작한다. `BusinessService` 클래스가 빈 객체로 등록될 때 스프링 컨테이너가 자동으로 `SortService`에 대한 의존성을 주입하기 때문이다. 이처럼 클라이언트가 아닌 스프링 컨테이너가 의존성 관리의 주체가 되는 것을 제어의 역전(`Inversion of Control`)이라고 하며 스프링 컨테이너를 `IoC` 컨테이너라고 부른다.

### @Autowired

`@Autowired` 활용법에 대해서 더 알아보자. 위에서 `Setter` 메서드에 `@Autowired` 어노테이션을 추가했는데 사실 그럴 필요 없이 변수에 바로 추가할 수도 있다.

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

또한 `@Autowired` 어노테이션은 생성자에 추가하여 구현할 수도 있다.

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

이번엔 `SelectionSortService` 클래스에 `@Autowired` 어노테이션을 추가하고 `Main` 예제를 실행시켜 보자.

```java
@Component
public class SelectionSortService implements SortService {
  public int[] sort(int[] a) {
    // implement insertion sort
    return a;
  }
}
```

정상적으로 동작하지 않는다. `SortService`에 주입될 수 있는 빈 객체가 2개 이상이기 때문에다. 스프링 컨테이너는 이 부분을 능동적으로 처리할 수 없다. 어떻게 해결할 수 있을까?

### @Primary

`@Primary` 어노테이션은 의존성을 주입해야 할 객체가 2개 이상일 때 어떤 객체를 우선적으로 주입할 것인지 명시해줄 수 있다.

```java
@Component
@Primary
public class InsertionSortService implements SortService {
  public int[] sort(int[] a) {
    // implement insertion sort
    return a;
  }
}
```

### @Qualifier

`@Qualifier` 어노테이션은 빈 객체에 이름을 명명하고 명시적으로 어떤 객체를 주입할 것인지 정할 수 있다.

```java
@Component
@Qualifier("selectionSortService")
public class SelectionSortService implements SortService {
  public int[] sort(int[] a) {
    // implement insertion sort
    return a;
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
