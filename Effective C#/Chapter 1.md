## Item 1: 지역변수를 선언할 때는 var를 사용하는 것이 낫다

- `IQueryable<T>`를 명시적으로 `IEnumerable<T>`로 형 변환해서 사용하면 `IQueryable<T>`이 제공하는 장점을 잃을 수 있다.
- 내장 숫자 타입들은 명시적으로 선언하는 편이 좋고 그 외는 가능하면 `var`를 쓰는 것이 좋다.

## Item 2: const보다는 readonly가 좋다

- `const`는 컴파일타임에 실제 값으로 대체되고 `readonly`는 런타임에 참조한다.
- `const`로 선언된 값을 변경하고 전체 리빌드를 하지 않으면 참조하는 다른 어셈블리의 값이 변경되지 않아 예상치 못한 에러가 발생할 수 있다.
- Optional parameter는 `const`처럼 컴파일타임 상수다.

## item 3: 캐스트보다는 is, as가 좋다

- `as` 연산자가 더 안전하고 읽기도 좋으며 런타임에도 효율적이다. (try-catch가 필요없다.)
- `as`는 캐스팅과 달리 사용자 정의 형변환을 수행하지 않는다.
- 사용자 정의 형변환은 컴파일타임에 정해지므로 주의해야 한다.
- `as`로 박싱된 값을 nullable value type 으로 변환하는 경우에는 새로운 객체가 생성된다.

## Item 4: string.Format()을 보간 문자열로 대체하라

- 가능하면 무조건 보간 기능을 활용하자. (`$"..."`)
- 내부적으로 `string.Format()`으로 변환되는데 `string.Format()`은 `object` 배열을 전달받기 때문에 value type의 경우 박싱이 발생한다.
- `$("{doubleValue.ToString()}")`처럼 미리 문자열로 변경하면 불필요한 박싱을 방지할 수 있다.
- `:` 는 포맷 문자열의 시작으로 판단하기 때문에 3항 연산자 사용시에 괄호로 감싸야 컴파일 오류를 없앨 수 있다.

## Item 5: 문화권별로 다른 문자열을 생성하려면 FormattableString을 사용하라

- 문화권을 임의로 지정해야 하는 경우에는 명시적으로 `FormattableString` 객체를 생성하도록 코드를 작성하자.
- var로 선언된 보간 문자열의 타입은 컴파일러가 코드의 조건들을 파악한 결과에 따라 `string`일 수도 `FormattableString`일 수도 있다.

## Item 6: nameof() 연산자를 적극 활용하라

- `nameof()` 연산자는 항상 로컬 이름(System.Int.MaxValue: MaxValue)을 문자열로 반환한다.
- `nameof()` 연산자를 활용하면 rename 시 같이 변경이 되므로 하드코딩보다 훨씬 좋다.

## Item 7: 델리게이트를 이용하여 콜백을 표현하라

- 콜백은 `interface`대신 델리게이트를 사용해서 구현하자.
- 델리게이트는 멀티캐스트가 가능하다. 예를들어 `Func<bool>` 델리게이트에 `+=` 으로 여러개의 함수를 추가할 수 있다.
- 위처럼 멀티캐스트일 때 리턴 타입이 있는 경우 가장 마지막에 추가된 함수의 반환값이 반환된다.
- `GetInvocationList()`로 추가된 개별 함수들을 조회할 수 있다.

## Item 8: 이벤트 호출 시에는 null 조건 연산자를 활용하라

- `event` 핸들러에 아무것도 등록되어 있지 않으면 null이다.
- 멀티 스레드 환경을 고려했을 때 `?.Invoke`로 호출하면 안전한 호출이 보장된다.

## Item 9: 박싱과 언박싱을 최소화하라

- 박싱은 value type을 reference type으로 변경하는데 reference type을 힙에 생성하고 value type의 복사본이 힙에 생성된 reference type 객체 내부에 저장된다.
- Generic을 사용하면 박싱을 피할 수 있다.
- 보간 문자열은 Object를 인자로 받는 `string.format`을 호출하기 때문에 박싱이 발생한다.

## Item 10: 베이스 클래스가 업그레이드 된 경우에만 new 한정자를 사용하라

- `new` 한정자는 비가상 메서드를 가상 메소드로 바꿔주는 것이 아니라 클래스의 naming scope에 새로운 메소드를 추가한다.
- 비가상 메서드는 정적으로 바인딩되기 때문에 런타임에 파생 메소드가 재정의 되었는지를 체크하지 않는다.
- `new` 한정자는 베이스 클래스가 업데이트 되면서 파생 클래스의 메소드와 같은 이름의 메소드가 생긴 경우에만 사용하는 것이 좋다.
- 위처럼 충돌 문제가 발생하는 경우를 제외하고는 `new` 한정자는 절대로 사용하지 말자.
