# Effetive C# 요약

## Chapter 1: 지역변수를 선언할 때는 var를 사용하는 것이 낫다

- IQueryable<T>를 명시적으로 IEnumerable<T>로 형 변환해서 사용하면 IQueryable<T>이 제공하는 장점을 잃을 수 있다.
- 내장 숫자 타입들은 명시적으로 선언하는 편이 좋고 그 외는 가능하면 var를 쓰는 것이 좋다.

## Chapter 2: const보다는 readonly가 좋다

- const는 컴파일타임에 실제 값으로 대체되고 readonly는 런타임에 참조한다.
- const로 선언된 값을 변경하고 전체 리빌드를 하지 않으면 참조하는 다른 어셈블리의 값이 변경되지 않아 예상치 못한 에러가 발생할 수 있다.
- Optional parameter는 const처럼 컴파일타임 상수다.
