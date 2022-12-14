# 6장. 응용 서비스와 표현 영역

## 📚 목차
- 6.1 표현 영역과 응용 영역
- 6.2 응용 서비스의 역할
- 6.2.1 도메인 로직 넣지 않기
- 6.3 응용 서비스의 구현
- 6.3.1 응용 서비스의 크기
- 6.3.2 응용 서비스의 인터페이스와 클래스
- 6.3.3 메서드 파라미터와 값 리턴
- 6.3.4 표현 영역에 의존하지 않기
- 6.3.5 트랜잭션 처리
- 6.4 표현 영역
- 6.5 값 검증
- 6.6 권한 검사
- 6.7 조회 전용 기능과 응용 서비스

---

### 🔍 표현 영역과 응용 영역
응용 영역과 표현 영역은 사용자와 도메인을 연결해 주는 매개체 역할을 한다.  
표현 영역은 사용자의 요청을 해석한다.  
응용 영역은 실제 사용자가 원하는 기능을 제공한다.  
응용 서비스의 메서드가 요구하는 파라미터와 표현 영역이 사용자로부터 전달받은 데이터는 형식이 일치하지 않기 때문에 표현 영역은 응용 서비스가 요구하는 형식으로 사용자 요청을 변환한다.  
응용 서비스를 실행한 뒤에 표현 영역은 실행 결과를 사용자에게 알맞은 형식으로 응답한다.  
사용자와 상호작용은 표현 영역이 처리하기 때문에 응용 서비스는 표현 영역에 의존하지 않는다.

### 🔍 응용 서비스의 역할
응용 서비스의 주요 역할은 도메인 객체를 사용해서 사용자의 요청을 처리하는 것이므로 표현(사용자) 영역 입장에서 보았을 때 응용 서비스는 도메인 영역과 표현 영역을 연결해주는 창구 역할을 한다.  
응용 서비스는 주로 도메인 객체 간의 흐름을 제어하기 때문에 단순한 형태를 갖는데, 복잡한 형태를 갖는다면 응용 서비스에서 도메인 로직의 일부를 구현하고 있을 가능성이 높다.  
이렇게 되면 코드 중복, 로직 분산 등 코드 품질에 안 좋은 영향을 줄 수 있다.  
응용 서비스는 트랜잭션 처리도 담당한다.  
도메인의 상태 변경을 트랜잭션으로 처리해야 한다.
- #### 도메인 로직 넣지 않기
    도메인 로직을 도메인 영역과 응용 서비스에 분산해서 구현하면 코드 품질에 문제가 발생한다.  
    첫 번째 문제는 코드의 응집성이 떨어진다는 것이다.  
    도메인 데이터와 도메인 로직이 서로 다른 영역에 위치한다는 것은 도메인 로직을 파악하기 위해 여러 영역을 분석해야 한다는 것을 의미한다.  
    두 번째 문제는 여러 응용 서비스에 동일한 도메인 로직을 구현할 가능성이 높아진다는 것이다.  
    위와 같은 두 가지 문제는 결과적으로 코드 변경을 어렵게 만든다.

### 🔍 응용 서비스의 구현
- #### 응용 서비스의 크기
    응용 서비스는 보통 다음의 두 가지 방법 중 한 가지 방식으로 구현한다.  
    - 한 응용 서비스 클래스에 도메인의 모든 기능 구현하기
      - 장점: 한 도메인과 관련된 기능을 구현한 코드가 한 클래스에 위치하므로 각 기능에서 동일 로직에 대한 코드 중복을 제거할 수 있다.
      - 단점: 한 서비스 클래스의 크기가 커진다. 코드 크기가 커지면 코드를 이해하기가 힘들어진다.
    - 구분되는 기능별로 응용 서비스 클래스를 따로 구현하기
      - 장점: 코드 품질을 일정 수준으로 유지하는 데 도움이 된다. 또한 각 클래스 별로 필요한 의존 객체만 포함하므로 다른 기능을 구현한 코드에 영향을 받지 않는다.
      - 단점: 클래스 개수가 많아진다.

- #### 응용 서비스의 인터페이스와 클래스
    인터페이스와 클래스를 따로 구현하면 소스 파일만 많아지고 구현 클래스에 대한 간접 참조가 증가해서 전체 구조가 복잡해진다.  
    따라서 인터페이스가 명확하게 필요하기 전까지는 응용 서비스에 대한 인터페이스를 작성하는 것이 좋은 선택이라고 볼 수는 없다.

- #### 메서드 파라미터와 값 리턴
    응용 서비스에 데이터로 전달할 요청 파라미터가 두 개 이상 존재하면 데이터 전달을 위한 별도의 클래스를 사용하는 것이 편리하다.  
    응용 서비스의 결과를 표현 영역에서 사용해야 하면 응용 서비스 메서드의 결과로 필요한 데이터를 리턴한다.  
    응용 서비스에서 애그리거트 자체를 리턴하면 코딩은 편할 수 있지만 도메인 로직 실행을 응용 서비스와 표현 영역 두 곳에서 할 수 있게 되므로 코드의 응집도가 낮아진다.  
    따라서 응용 서비스는 표현 영역에서 필요한 데이터만 리턴하는 것이 기능 실행 로직의 응집도를 높이는 확실한 방법이다.

- #### 표현 영역에 의존하지 않기
    응용 서비스의 파라미터 타입을 결정할 때 주의할 점은 표현 영역과 관련된 타입을 사용하면 안 된다는 점이다.  
    응용 서비스에 표현 영역에 대한 의존이 발생하면 응용 서비스가 표현 영역의 역할까지 대신하는 상황이 벌어질 수도 있다.  
    이렇게 되면 표현 영역의 응집도가 깨지게 되고 이것은 결과적으로 코드 유지 보수 비용을 증가시키는 원인이 된다.  
    응용 서비스가 표현 영역의 기술을 사용하지 않는 가장 쉬운 방법이 응용 서비스 메서드의 파라미터와 리턴 타입으로 표현 영역의 구현 기술을 사용하지 않는 것이다.

### 🔍 표현 영역
표현 영역의 책임은 크게 다음과 같다.
- 사용자가 시스템을 사용할 수 있는 흐름(화면)을 제공하고 제어한다.
- 사용자의 요청을 알맞은 응용 서비스에 전달하고 결과를 사용자에게 제공한다.
- 사용자의 세션을 관리한다.

### 🔍 값 검증
값 검증은 표현 영역과 응용 서비스 두 곳에서 모두 수행할 수 있는데, 원칙적으로 모든 값에 대한 검증은 응용 서비스에서 처리한다.  
이 때 표현 영역은 잘못된 값이 존재하면 이를 사용자에게 알려주고 값을 다시 입력받아야 한다.  
응용 서비스에서 값 검증을 할 때의 문제점은 사용자에게 좋지 않은 경험을 제공한다는 것이다.  
표현 영역에서 필수 값과 값의 형식을 검사하면 응용 서비스는 논리적 오류만 검사하면 된다.  
