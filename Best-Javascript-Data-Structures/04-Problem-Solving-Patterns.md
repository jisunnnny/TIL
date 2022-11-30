# 04 - 문제해결을 위한 패턴 익히기

## 빈도수 세기(frequency counter)

**예제:** 2개의 배열 a, b를 입력받고, 배열 a의 각 원소의 제곱이 순서에 상관 없이 배열 b의 원소이면 true, 아니면 false

```jsx
same([1, 2, 3], [4, 1, 9]); // true
same([1, 2, 3], [1, 9]); // false
same([1, 2, 1], [4, 4, 1]); //false
```

**중첩 루프를 사용한 방법(순진한 접근법)**

```jsx
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }
  for (let i = 0; i < arr1.length; i++) {
    // indexOf는 배열에서 지정된 원소를 발견하고 첫번째 인덱스를 반환(없으면 -1 반환)하며, 그 자체로서 loop이다.
    let correctIndex = arr2.indexOf(arr1[i] ** 2);

    if (correctIndex === -1) {
      return false;
    }

    arr2.splice(correctIndex, 1); // arr2배열에서 arr1[i]의 제곱에 해당하는 원소를 제거해줌
  }
  return true;
}
```

위 코드의 `시간복잡도는 O(N^2)`이다. 모든 경우의 수를 확인하기 때문에 `순진한 접근법`이라고도 한다.
배열이 N만큼 증가하면 시간 복잡도는 N^2(제곱)만큼 늘어나기 때문에 가능하면 중첩된 루프는 사용하지 않는게 좋다.

그렇다면, 보다 더 효율적으로 구현할 순 없을까?
리팩토링 해보자.

**선형 시간으로 구현(리팩토링)**

```jsx
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }
  let frequencyCounter1 = {};
  let frequencyCounter2 = {};

  for (let val of arr1) {
    // frequencyCounter1 객체에 key값이 있으면 그 값에 +1, 없으면 key값을 추가하고 값에 +1
    frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;
  }

  for (let val of arr2) {
    frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1;
  }

  for (let key in frequencyCounter1) {
    if (!(key ** 2 in frequencyCounter2)) {
      return false;
    }

    if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) {
      return false;
    }
  }

  return true;
}
```

리팩토링한 위 코드의 `시간복잡도는 O(n)`이다.
중첩되지 않은 루프를 사용하여 시간복잡도는 O(n)을 유지할 수 있다.

## 다중 포인터 패턴(multiple pointer)

- 다중 포인터 패턴은 공식 이름은 아니다.
- 인텍스나 위치에 해당하는 포인터나 값을 만든 다음, 특정 조건에 따라 중간 지점에서부터 시작 지점, 끝 지점, 양쪽 지점을 향해 이동시키는 것이다.
- 결론적으로 배열이나 문자열과 같은 일종의 `선형 구조`, `이중 연결 리스트`, `단일 연결 리스트를` 만드는 것이다.
- 우선, **한 쌍의 값이나 조건을 충족시키는 무언가를 찾는다**는 개념에 주목해서 살펴보자.

**예제:** 정수가 원소인 배열을 입력받고, 합이 0이 되는 1쌍이 있으면 인덱스가 빠른 것을 리턴 (없으면 undefined를 리턴, 배열은 오름차순 정렬 되어있음)

**중첩 루프를 사용한 방법(순진한 접근법)**

```jsx
function sumZero(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] + arr[j] === 0) {
        return [arr[i], arr[j]];
      }
    }
  }
}
```

중첩 반복문을 사용했기 때문에 `시간복잡도는 O(N^2)`이다.
시간복잡도를 줄일 수 있는 다른 방법은 없을까?
리팩토링 해보자.

**리팩토링**

```jsx
function sumZero(arr) {
  let left = 0;
  let right = arr.length - 1;
  // left와 right가 서로를 향해 이동해 중앙에서 만날때까지 while
  // 합이 0인 쌍이 없다면, undefined 반환
  while (left < right) {
    let sum = arr[left] + arr[right];
    if (sum === 0) {
      return [arr[left], arr[right]];
    } else if (sum > 0) {
      right--;
    } else {
      left++;
    }
  }
}
```

리팩토링한 코드의 `시간복잡도는 O(N)`이다.

## 슬라이딩 윈도우(sliding window)

- 배열이나 문자열과 같은 일련의 데이터셋에서 특정 조건을 만족시키는 하위 집합을 찾을 때 유용하다.
- `window`는 어떤 조건을 만족하는지 판별하는 집합 단위이다.
- 이 **window가 slide하면서 배열이나 문자열에서 특정 집합을 찾는 것**을 슬라이딩 윈도우라고 한다.

**예제:** 배열에서 n개의 연속하는 수가 최대값인 것을 출력

```jsx
maxSubarraySum([1, 2, 5, 2, 8, 1, 5], 2); // 10
maxSubarraySum([1, 2, 5, 2, 8, 1, 5], 4); // 17 -> index(1~4) 총합
```

**중첩 루프를 사용한 방법(순진한 접근법)**

```jsx
function maxSubarraySum(arr, num) {
  if (num > arr.length) {
    return null;
  }
  var max = -Infinity; // 음의 무한대(모든 음수들보다 작음)
  // arr.length - num + 1 : window 수
  for (let i = 0; i < arr.length - num + 1; i++) {
    temp = 0;
    // 개별 window
    for (let j = 0; j < num; j++) {
      // arr[i + j]를 통해 i++ 될 때마다 연속하는 배열을 구할 수 있음
      temp += arr[i + j];
    }
    if (temp > max) {
      max = temp;
    }
  }
  return max;
}
```

**리팩토링**

```jsx
function maxSubarraySum(arr, num) {
  let maxSum = 0;
  let tempSum = 0;
  if (arr.length < num) return null;
  for (let i = 0; i < num; i++) {
    maxSum += arr[i]; // 첫 번째 합을 maxSum에 저장
  }
  tempSum = maxSum;
  for (let i = num; i < arr.length; i++) {
    // arr[0] 빼고 arr[num] 더하면 새로운 window
    tempSum = tempSum - arr[i - num] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}

maxSubarraySum([2, 6, 9, 2, 1, 8, 5, 6, 3], 3);
```

## 분할정복(Divide and Conquer)

- 데이터셋을 작은 조각들로 나누고, 그 하위집합을 처리하는 프로세스를 반복한다.
- 시간복잡도를 크게 낮출 수 있다.

**예제:** 정렬된 숫자 배열에서 특정 숫자의 인덱스를 리턴(없으면 -1 리턴)

```jsx
search([1, 2, 3, 4, 5, 6], 4); // 3
search([1, 2, 3, 4, 5, 6], 11); // -1
```

**선형 탐색(순진한 접근법)**
`시간복잡도는 O(N)`

```jsx
function search(arr, num) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === val) {
      return i;
    }
  }
  return -1;
}
```

**이진 탐색(리팩토링)**
`시간복잡도는 O(log N)`

```jsx
function search(array, val) {
  let min = 0;
  let max = array.length - 1;
  while (min <= max) {
    let middle = Math.floor((min + max) / 2);
    let currentElement = array[middle];
    if (array[middle] < val) {
      min = middle + 1;
    } else if (array[middle] > val) {
      max = middle - 1;
    } else {
      return middle;
    }
  }
  return -1;
}
```
