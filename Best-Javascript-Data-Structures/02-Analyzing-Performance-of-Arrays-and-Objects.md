# 02 - 배열과 객체의 성능 평가(Big O)

## 객체(Objects)

- 정렬되어 있을 필요가 없을 때 잘 작동한다.
- 빠른 접근, 입력과 제거를 원할 때 좋다.
- 객체는 순서가 없지만 key와 value을 사용해 빠르다는 장점이 있다.

- **Big O로 보는 성능**

  | 삽입(입력) | O(n) |
  | ---------- | ---- |
  | 제거       | O(n) |
  | 검색(탐색) | O(n) |
  | 접근       | O(n) |

- **객체 메서드의 Big O**
  | Object.keys    | O(n) |
  | -------------- | ---- |
  | Object.values  | O(n) |
  | Object.entries | O(n) |
  | hasOwnProperty | O(1) |
  ⇒ hasOwnProperty는 접근의 개념이다.
  ⇒ hasOwnProperty를 제외한 나머지 메서드들은 Obejct에 할당된 모든 데이터들을 1번은 순회해야하므로 선형 복잡도를 갖는다.\*\*\*\*

## 배열

- 배열의 가장 중요한 점은 정렬되어 있다는 것이다.
- 정렬되어 있는 데이터가 필요할 때 유용하다.
- 연산을 하는 시간이 오래 걸리는 경우도 있기 때문에, 정렬되어 있는 것이 필요 없다면 사용하지 않는 것이 좋다.
- 성능을 최적화하고 싶다면 배열말고 다른것을 선택하는 것이 좋다.
- index만 있으면 특정 data의 위치에 접근할 수 있기 때문에 접근은 일정하다.
- 검색은 Object와 마찬가지로 O(n)\*\*\*\*

- **배열의 메소드들과 시간복잡도**
  | Array.push                           | O(1)     |
  | ------------------------------------ | -------- |
  | Array.pop                            | O(1)     |
  | Array.shift                          | O(n)     |
  | Array.unshift                        | O(n)     |
  | Array.concat                         | O(n)     |
  | Array.slice                          | O(n)     |
  | Array.splice                         | O(n)     |
  | Array.sort                           | O(nlogn) |
  | Array.forEach/map/filter/reduce/etc. | O(n)     |
  ⇒ index를 새로 정리해야하기 때문에 `push`, `pop` 보다 `shift`, `unshift`가 훨씬 느리다.
