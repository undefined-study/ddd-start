# 5장. 스프링 데이터 JPA를 이용한 조회 기능

## 📚 목차
- 5.1 시작에 앞서
- 5.2 검색을 위한 스펙
- 5.3 스프링 데이터 JPA를 이용한 스펙 구현
- 5.4 리포지터리/DAO에서 스펙 사용하기
- 5.5 스펙 조합
- 5.6 정렬 지정하기
- 5.7 페이징 처리하기
- 5.8 스펙 조합을 위한 스펙 빌더 클래스
- 5.9 동적 인스턴스 생성
- 5.10 하이버네이트 @Subselect 사용

---

### 🔍 시작에 앞서
CQRS는 명령(Command) 모델과 조회(Query) 모델을 분리하는 패턴이다.  
명령 모델은 상태를 변경하는 기능을 구현할 때 사용하고 조회 모델은 데이터를 조회하는 기능을 구현할 때 사용한다.  
상태(데이터)를 변경하는 기능을 구현할 때는 명령 모델을 사용하고, 데이터를 보여주는 기능을 구현할 때는 조회 모델을 사용한다.  
도메인 모델은 명령 모델로 주로 사용되는데, 이 장에서는 조회 모델 구현에 대하여 살펴보도록 한다.

### 🔍 검색을 위한 스펙
검색 조건을 다양하게 조합해야 할 때 사용할 수 있는 것이 스펙이다.  
스펙 인터페이스는 다음과 같이 정의한다.
```java
public interface Speficiation<T>{
    public boolean isSatisfiedBy(T agg);
}
```

### 🔍 정렬 지정하기
스프링 데이터 JPA는 두 가지 방법을 사용해서 정렬을 지정할 수 있다.
- 메서드 이름에 OrderBy를 사용해서 정렬 기준 지정
```java
public interface OrderSummaryDao extends Repository<OrderSummary, String>{
    List<OrderSummary> findByOrdererIdOrderByNumberDesc(String ordererId);
}
```
- Sort를 인자로 전달
```java
import org.springframework.data.domain.Sort;

public interface OrderSummaryDao extends Repository<OrderSummary, String>{
    List<OrderSummary> findByOrdererId(String ordererId, Sort sort);
}
```

### 🔍 페이징 처리하기
스프링 데이터 JPA는 페이징 처리를 위해 Pageable 타입을 이용한다.
```java
import org.springframework.data.domain.Pageable;

public interface MemberDataDao extends Repository<MemberData, String>{
    List<MemberData> findByNameLike(String name, Pageable pageable);
}
```