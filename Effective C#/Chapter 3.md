## Item 18. 반드시 필요한 제약 조건만 설정하라

- 아무런 제약 조건이 없다면 해당 제네릭 타입에 대해 `System.Object`가 제공하는 최소한의 기능만 사용할 수 있다.
- 값 타입의 경우 `struct` 제약 조건을 사용하면 boxing/unboxing을 막을 수 있다.
- `IEquatable<T>`를 예로 들면, 해당 제약 조건을 사용하면 `System.Object.Equals()` 대신 구체화된 `IEquatable<T>.Equals()`를 사용하게 된다.
- C#은 C++ 템플릿과 달리 컴파일 시점에 제약 조건에 설정된 내용만 이용해서 IL을 생성한다.
- `default(T)`를 사용하면 `new()` 제약 조건을 제거할 수 있는 경우도 있다. (`FirstOrDefault`)

> 아 이제 더 이상 정리하기 귀찮다.\
> 여기까지 정리했다는 것 자체가 기적.
