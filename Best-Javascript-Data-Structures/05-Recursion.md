# 05 - 재귀 함수

## 재귀함수

- 재귀 함수는 자기 `자신을 호출`하는 함수이며, 종료점이 있어야한다.
- 재귀 함수는 매우 많은 곳에서 쓰이며 어려운 자료구조를 이해하기 위한 핵심 개념이다.
- 재귀 함수가 사용되는 예로는 다음과 같다

  - JSON.parse / JSON.stringify
  - document.getElementById and DOM traversal (트리 순회)
  - Object traversal (객체 순회)
  - 기타 어려운 자료구조 등등

- 때로는 재귀 함수 대신, 반복(Iteration)을 사용하는 것이 깔끔할 때도 있다.

## 스택 호출하기

JavaScript에서 함수를 호출하면 콜 스택 맨 꼭대기에 쌓인다. 즉, 새로 호출할 때마다 제일 위에 쌓인다.

```js
// call stack 예시
function takeShower() {
  return "Showering!";
}

function eatBreakfast() {
  let meal = cookFood(); // 바닥에서 3번쨰
  return `Eating ${meal}`;
}

function cookFood() {
  let items = ["Oatmeal", "Eggs", "Protein Shake"];
  return items[Math.floor(Math.random() * item.length)];
}

function wakeUp() {
  takeShower(); // 콜스택 바닥에서 1번째
  eatBreakfast(); // 바닥에서 2번쨰
  console.log("Ok ready to go to work!");
}

wakeUp();
```

위 코드처럼 콜스택에 쌓이는 함수의 순서와 실행되는 함수의 순서는 정 반대이며, 실행된 함수는 콜스택에서 지워진다.

그런데, 재귀함수는 새로운 함수를 콜스택에 계속 추가한다.

## 종료조건

재귀 함수가 멈추는 시점

1. `base case` : 목록이나 배열에 아무것도 없는가? 를 확인하고 멈춘다
2. `different input` 이면 멈춘다

```js
// 첫번째 재귀 함수
// 단순 반복문과 같은 형태
// num 에서 0까지 1씩 줄여서 출력 후 마지막엔 all done 출력
function countDown(num) {
  if (num <= 0) {
    console.log("All done!");
    return; // return이 없으면 num이 음수로 작아지면서 all done이 계속 출력됨
  }
  console.log(num);
  num--;
  countDown(num);
}
```

```js
// 두번째 재귀 함수
// return에 재귀 함수를 포함하는 형태
// num
function sumRange(num) {
  if (num === 1) return 1; // 중단점
  return num + sumRange(num - 1);
}

console.log(sumRange(3)); // 6

// 함수 동작 과정
//
// sumRange(3)
//   return 3 + sumRange(2) <- sumRange(3)이 기다리고 있음
//                return 2 + sumRange(1) <- sumRange(2)가 기다리고 있음
//                             return 1 <- sumRange(1)은 1을 리턴
// 최종적으로 6 리턴
```

## 팩토리얼 재귀로 구현하기

두번째 재귀함수 패턴 이용

```js
function factorial(num) {
  if (num === 1) return 1; // 중단점
  return num * factorial(num - 1);
}

console.log(factorial(5)); // 120
```

## 재귀 함수를 만들 때 자주 등장하는 에러

### 1. 중단점 없음 -> 스택 오버플로 에러 발생시킴

### 2. 잘못된 값 반환

위에서 만든 팩토리얼 재귀 함수에 중단점이 없으면 0을 지나서 계속 곱셈이 반복된다.  
Maximum call stack size exceeded (최대 스택 크기 초과) 에러 발생 -> stack overflow 에러  
Node 이거나 다른 브라우저처럼 환경에 따라 최대 콜 스택의 크기는 다르다 (대충 1000개)

## 헬퍼 메서드 재귀 (헬퍼 함수)

지금까지 작성한 재귀 함수는 팩토리얼처럼 single standalone function (단일 단독 함수)

```js
function outer(input) {
  var outerScopedVariable = [];
  function helper(helperInput) {
    helper(helperInput--);
  }
  helper(input);
  return outerScopedVariable;
}
```

ㄴ 외부 함수 내에 재귀적인 내부 함수를 호출하는 패턴

```js
// 배열에서 홀수만 출력하는 함수 (외부 + 재귀내부함수)
function collectOddValue(arr) {
  let result = [];
  function helper(helperInput) {
    if (helperInput.length === 0) {
      return;
    }
    if (helperInput[0] % 2 !== 0) {
      result.push(helperInput[0]);
    }
    helper(helperInput.slice(1));
  }
  helper(arr);
  return result;
}
```

## 순수 재귀

헬퍼 함수를 사용하지 않는 재귀 함수

```js
function collectOddValues(arr) {
  let newArr = [];

  if (arr.length === 0) {
    return newArr;
  }

  if (arr[0] % 2 !== 0) {
    newArr.push(arr[0]);
  }

  newArr = newArr.concat(collectOddValues(arr.slice(1)));
  return newArr;
}
```

- 배열을 복사하는 `slice()`, `spread 연산자`, `concat()` 같은 메서드를 사용하면 배열을 변경할 필요가 없다
- 문자열을 복사하기 위해서는 `slice()`, `substr()` 또는 `substring()` 메서드를 사용한다 (문자열은 변경 불가)
- 객체를 복사하기 위해서는 `Object.assign()`, `spread 연산자`를 사용한다
