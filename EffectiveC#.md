# Effetive C# 요약

## Chapter 1: 지역변수를 선언할 때는 var를 사용하는 것이 낫다

- IQueryable<T>를 명시적으로 IEnumerable<T>로 형 변환해서 사용하면 IQueryable<T>이 제공하는 장점을 잃을 수 있다.
- 내장 숫자 타입들은 명시적으로 선언하는 편이 좋고 그 외는 가능하면 var를 쓰는 것이 좋다.

## Chapter 2: const보다는 readonly가 좋다

- const는 컴파일타임에 실제 값으로 대체되고 readonly는 런타임에 참조한다.
- const로 선언된 값을 변경하고 전체 리빌드를 하지 않으면 참조하는 다른 어셈블리의 값이 변경되지 않아 예상치 못한 에러가 발생할 수 있다.
- Optional parameter는 const처럼 컴파일타임 상수다.

## Chapter 3: 캐스트보다는 is, as가 좋다.

- as 연산자가 더 안전하고 읽기도 좋으며 런타임에도 효율적이다. (try-catch가 필요없다.)
- as는 캐스팅과 달리 사용자 정의 형변환을 수행하지 않는다.
- 사용자 정의 형변환은 컴파일타임에 정해지므로 주의해야 한다.
- as로 박싱된 값을 nullable value type 으로 변환하는 경우에는 새로운 객체가 생성된다.

## Chapter 4: string.Format()을 보간 문자열로 대체하라.

- 가능하면 무조건 보간 기능을 활용하자. (`$"..."`)
- 내부적으로 string.Format()으로 변환되는데 string.Format()은 object 배열을 전달받기 때문에 value type의 경우 박싱이 발생한다.
- `$("{doubleValue.ToString()}")`처럼 미리 문자열로 변경하면 불필요한 박싱을 방지할 수 있다.
- `:` 는 포맷 문자열의 시작으로 판단하기 때문에 3항 연산자 사용시에 괄호로 감싸야 컴파일 오류를 없앨 수 있다.
