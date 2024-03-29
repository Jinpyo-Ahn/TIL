# 12장 타입추론

**타입 추론을 배우고 나면 더 간결하게 타입스크립트 코드를 작성할 수 있다.**

# 12.1 타입 추론이란?

**타입추론**

: 타입스크립트가 코드를 해석해 적절한 타입을 정의하는 동작

```tsx
let a = 10;
```

<img src="image/12/1.png">

→ 변수에 숫자 10이 할당되어 타입이 number(10)으로 추론되는 모습

다음과 같이 타입을 지정한 것과 같은 효과와 같다.

```tsx
let a: number = 10;
```

변수를 초기화하거나 함수의 파라미터에 기본값을 설정하거나 반환값을 설정했을 때, 지정된 값을 기반으로 적당한 타입을 제시하고 정의해 주는 것이 타입추론이다.

# 12.2 변수의 타입 추론 과정

변수의 값을 할당하지 않고 선언하면 어떤 타입이 될까?

```tsx
let a;
```

<img src="image/12/2.png">

→ any 타입으로 추론

a 변수에 값을 할당하지 않아 어떤 값이 들어올지 모른다.

→ 어떤 값이든 받을 수 있게 any로 지정

이번엔 변수를 선언하면서 초깃값으로 문자열을 할당

```tsx
let a = 'hi';
```

<img src="image/12/3.png">

→ string 타입으로 추론

변수를 선언할 때 할당된 초깃값에 따라서 적절한 타입이 추론

하지만 변수를 선언한 이후에 값을 변경하면 타입이 해당 데이터에 맞는 타입으로 변경되지 않는다.

```tsx
let a;
a = 10;
```

<img src="image/12/4.png">

**위와 같이 동작하는 이유**

: **변수 타입은 선언하는 시점에 할당된 값을 기반으로 추론**되기 때문이다.

# 12.3 함수의 타입 추론: 반환 타입

`ex`

두 수의 합을 구하는 함수

```tsx
function sum(a: number, b: number): number {
  return a + b;
}
```

함수를 호출해 반환된 결과 값을 변수에 할당하면 number 타입으로 추론

```tsx
let result = sum(1, 2);
```

<img src="image/12/5.png">

변수의 타입 추론과 마찬가지로 함수도 주어진 입력값에 따라 함수의 반환 타입이 추론된다.

sum 함수에서 반환 타입 제거

```tsx
function sum(a: number, b: number) {
  return a + b;
}

let result = sum(1, 2);
```

<img src="image/12/6.png">

위와 같이 result 변수의 타입이 추론되는 이유는 sum 함수의 반환 타입이 number 타입으로 추론되기 때문이다.

<img src="image/12/7.png">

파라미터 a와 b가 모두 number 타입으로 지정되어 있고, 이 두 숫자를 더한 결과는 당연히 숫자이기 때문에 반환 타입이 number로 정의된 것이다.

두 숫자를 받아서 비교를하면 어떻게 될까?

```tsx
function isEqual(a: number, b: number) {
  return a === b;
}
```

<img src="image/12/8.png">

반환 타입이 boolean으로 추론되는 것을 확인할 수 있다.

→ **함수의 파라미터 타입과 내부 로직으로 반환 타입이 추론되는 것**을 확인할 수 있다.

# 12.4 함수의 타입 추론: 파라미터 타입

```tsx
function getA(a) {
  return a;
}
```

<img src="image/12/9.png">

파라미터 타입을 지정하지 않았으므로 기본 타입은 any가 된다.

→ any로 지정되어 있는 파라미터를 그대로 반환해 주었기 때문에 파라미터 타입과 반환 타입이 모두 any로 지정되는 것을 확인

a의 파라미터를 number로 바꿔보자.

```tsx
function getA(a: number) {
  return a;
}
```

<img src="image/12/10.png">

함수의 파라미터 타입은 함수 내부에서도 동일하게 추론된다.

a 파라미터를 그대로 반환했기 때문에 파라미터 타입과 반환 타입 모두 number로 추론된다.

파라미터에 기본값을 지정

```tsx
function getA(a = 10) {
  return a;
}
```

<img src="image/12/11.png">

기본값이 숫자 10이기 때문에 파라미터 타입은 number로 추론된다.

함수의 파라미터에 값을 넘기거나 넘기지 않아도 되기 때문에 옵셔널 파라미터를 의미하는 `?` 가 붙는다.

함수 안에 변수를 선언해 파리미터와 연산해보자.

```tsx
function getA(a: number) {
  let c = 'hi';
  return a + c;
}
```

<img src="image/12/12.png">

함수의 반환 타입이 string으로 추론된다.

`why?`

자바스크립트는 문자열과 숫자를 더할 때 숫자를 문자열로 변환하여 문자열 2개를 합친 것과 결과가 같다.

→ 위와 같은 문제를 타입스크립트는 타입을 명시함으로써 예상치 못한 결과를 미리 알아차릴 수 있다.

# 12.5 인터페이스와 제네릭 추론 방식

`ex`

```tsx
interface Dropdown<T> {
  title: string,
  value: T
}
```

객체를 하나 선언

```tsx
let shoppingItem: Dropdown<number> = {
  
}
```

<img src="image/12/13.png">

변수를 Dropdown 인터페이스 타입을 정의하였기 때문에 자동완성할 수 있는 속성들을 확인할 수 있다.

title 속성은 문자열 타입으로 선언 → string 타입으로 추론

<img src="image/12/14.png">

value 속성은 number 타입으로 추론

<img src="image/12/15.png">

이처럼 **인터페이스에 제네릭을 사용할 때도 타입스크립트 내부적으로 적절한 타입을 추론**해준다.

# 12.6 복잡한 구조에서 타입 추론 방식

인터페이스에 제네릭을 2개 연결

```tsx
interface Dropdown<T> {
  title: string,
  value: T
}

interface DetailedDropdown<K> extends Dropdown<K> {
  tag: string,
  description: string
}
```

Dropdown 인터페이스와 이 인터페이스를 상속받는 DetailedDropdown 인터페이스를 선언

DetailedDropdown 인터페이스를 사용하여 객체 정의

```tsx
let shoppingDetailItem: DetailedDropdown<number> = {
  
}
```

객체에 사용할 수 있는 속성이 미리보기로 표시

<img src="image/12/16.png">

Dropdown 인터페이스의 value 속성이 number 타입으로 추론되는 이유

<img src="image/12/17.png">

→ DetailedDropdown 인터페이스에 넘긴 제네릭 타입이 Dropdown 인터페이스의 제네릭 타입으로 전달되었기 때문이다.

<img src="image/12/18.png">

결과적으로 Dropdown 인터페이스의 value 속성은 number 타입을 갖게 되고 shoppingDeatailItem 객체는 다음과 같이 선언할 수 있다.

```tsx
const shoppingDetailItem: DetailedDropdown<number> = {
  title: '길벗 책',
  description: '쉽고 유용하다',
  tag: '타입스크립트',
  value: 1,
}
```

위와 같이 복잡한 인터페이스 사이의 상속과 제네릭이 얽혀 있는 구조에서도 타입스크립트가 내부적으로 해당 타입들을 올바르게 추론할 수 있다.