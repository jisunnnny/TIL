# replace()

## replace()

- 문자에서 조건에 match되는 첫 번째 문자 하나만을 치환해서 반환
- 전달받은 문자를 수정하는 것이 아니라, `새로운 값`을 만들어 반환

```javascript
const beforeStr = "Pen pineapple apple pen.";
const afterStr = beforeStr.replace("apple", "BANANA"); // 출력: Pen pineBANANA apple pen.
```

## replace()를 replaceAll 처럼 사용하기

- 모든 문자를 치환하고 싶은 경우, `정규식을 사용`하여 replace

```javascript
const beforeStr = "Pen pineapple apple pen.";
const afterStr = beforeStr.replace(/apple/g, "BANANA"); // 출력: Pen pineBANANA BANANA pen.
```
