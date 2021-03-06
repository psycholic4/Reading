## Item 11: .NET 리소스 관리에 대한 이해

- .NET 가비지 콜렉터는 managed memory를 관리하며 Mark/Compact 알고리즘에 기반하여 사용되지 않는 객체를 자동으로 제거한다.
- finalizer를 가지고 있는 객체는 finalizer 호출을 위해 바로 제거되지 않고 별도로 관리되어서 finalizer 호출 후에 제거된다.
- finalizer 대신 `IDisposable` 인터페이스와 표준 Dispose 패턴을 활용하자.

## Item 12: 할당 구문보다 멤버 초기화 구문이 좋다

- 멤버 변수는 생성자 본문에서 할당하지말고 멤버 초기화 구문을 사용해서 할당하는 것이 좋다.
- 멤버 초기화 구문은 생성자 본문의 앞쪽에 덧붙여지고 상속된 클래스의 경우에도 베이스 클래스의 생성자가 호출되기 전에 멤버 초기화 구문이 먼저 실행된다.
- 객체를 default 값으로 초기화하는 경우에는 멤버 초기화 구문을 쓰지 않아야 불필요한 반복 초기화를 방지할 수 있다.
- 멤버 초기화 구문은 모든 생성자에서 같은 로직으로 초기화하는 경우에만 사용해야 한다.
- 예외 처리가 필요한 경우에는 반드시 생성자에서 초기화 해야 한다.

## Item 13: 정적 클래스 멤버를 올바르게 초기화하라

- 정적 멤버를 초기화하는 경우에도 멤버 초기화 구문을 사용하는 것이 좋다.
- 타입 초기화 과정은 단 한 번만 일어나기 때문에, 정적 생성자 내에서 예외가 발생한 것을 호출하는 쪽에서 catch 해버리면 타입이 올바르게 초기화되지 않아서 타입에 접근할 때 오류가 발생한다.
- 정적 멤버 초기화 과정에서 예외가 발생할 가능성이 있을 때는 정적 생성자 내에서 try-catch를 활용해서 처리해야 한다.

## Item 14: 초기화 코드가 중복되는 것을 최소화하라

- 여러 생성자에서 동일한 초기화 코드를 실행해야 하는 경우 헬퍼 메서드를 만들지 말고 공용 생성자를 작성하자.
- 공용 생성자는 컴파일러가 최적화를 통해 변수 중복 초기화 코드를 제거해주고 베이스 클래스의 생성자가 반복 호출되는 것도 막아준다.
- `readonly` 필드는 생성자 초기화 구문이 아닌 곳에서는 초기화가 되지 않는다.
- 특정 타입의 인스턴스를 생성할 때 수행되는 과정
  1. 정적 변수의 저장 공간을 0으로 초기화
  2. 정적 변수에 대한 초기화 구문 수행
  3. 베이스 클래스의 정적 생성자 수행
  4. 정적 생성자 수행
  5. 인스턴스 변수의 저장 공간을 0으로 초기화
  6. 인스턴스 변수에 대한 초기화 구문 수행
  7. 적절한 베이스 클래스의 인스턴스 생성자 수행
  8. 인스턴스 생성자 수행

## Item 15. 불필요한 객체를 만들지 말라

- 가비지 수집기가 과도하게 동작하지 않도록 주의해야 가비지 수집기에 의한 프로그램 성능 저하를 막을 수 있다.
- 자주 호출되는 메서드 내에서 자주 사용되는 동일한 reference type 객체를 매번 생성하는 경우에는 멤버 변수로 변경하는 것이 좋다.
- `string`은 immutable 하기 때문에 `+` 로 연산을 하는 것보다는 보간 문자열을 사용하거나 복잡한 경우에는 `StringBuilder`를 사용하자.

## Item 16. 생성자 내에서는 절대로 가상 함수를 호출하지 말라

- 베이스 클래스의 생성자에서 가상 함수를 호출하면 파생 클래스의 생성자가 호출되지 않은 상태라서 일관성 문제가 발생한다.
- 정적 분석기가 이런 패턴에 대해 경고를 띄워준다.

## Item 17. 표준 Dispose 패턴을 구현하라

- 비관리 리소스를 포함하는 객체는 표준화된 `Dispose` 패턴을 사용해서 리소스를 정리하자.
- 베이스 클래스의 경우
  1. 리소스를 정리하기 위해서 `IDisposable` 인터페이스를 구현해야 한다.
  2. 멤버 필드로 비관리 리소스를 포함하는 경우에 한해 방어적으로 동작할 수 있도록 finalizer를 추가해야 한다.
  3. `Dispose`와 finalizer는 실제 리소스 정리 작업을 수행하는 다른 가상 메서드에 작업을 위임하도록 작성돼야 한다. 파생 클래스가 고유의 리소스 정리 작업이 필요한 경우 이 가상 메서드를 재정의할 수 있도록 하기 위함이다.
- 파생 클래스의 경우
  1. 파생 클래스가 고유의 리소스 정리 작업을 수행해야 한다면 베이스 클래스에서 정의한 가상 메서드를 재정의한다.
  2. 멤버 필드로 비관리 리소스를 포함하는 경우에만 finalizer를 추가해야 한다.
  3. 베이스 클래스에서 정의하고 있는 가상 함수를 반드시 재호출해야 한다.
- `IDisposable.Dispose()`
  1. 모든 비관리 리소스를 정리한다.
  2. 모든 관리 리소스를 정리한다.
  3. 객체가 이미 정리되었음을 나타내기 위한 상태 플래그 설정. 앞서 이미 정리된 객체에 대하여 추가로 정리 작업이 요청될 경우 이 플래그를 확인하여 `ObjectDisposed` 예외를 발생시킨다.
  4. finalizer 호출 회피. 이를 위해 `GC.SuppressFinalize(this)`를 호출한다.
