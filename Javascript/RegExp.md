### RegExp

- replace를 이용하여 해당하는 모든 문자열을 치환/제거 할 때 사용
- 치환/제거할 문자를 유동적으로 바꾸고 싶을 때 사용

### RegExp 객체 사용법

- 일치하는 패턴 중 최초 등장하는 패턴 한 번만 찾음
  ```javaScript
  let regexOne = new RegExp(pattern);
  ```
- 일치하는 모든 패턴을 찾음
  ```javaScript
  let regexAll = new RegExp(pattern, "g");
  ```
- 대소문자 구분 없이 일치하는 모든 패턴을 찾음
  ```javaScript
  let regexAllCase = new RegExp(pattern, "gi");
  ```
