- 어제 면접의 여파로 테스트 코드에 관심을 가지게 되었다. 라이브 코드를 치는데 갑자기 외계어의 등장마냥,, 정말 그냥 멘붕을 겪었지
- 말로만 듣던 테스트 코드 jest 어쩌고 막 하면서 이게 뭔지만 대충 공부하려고 했던 내 자신 반성
- 면접이 끝나고 어떻게 사용했는지 알고 싶어졌다. 그래서 벨로퍼트님의 오래된 글이지만 간단한 테스트 코드 포스팅 발견 [자바스크립트 테스팅의 기초](https://velog.io/@velopert/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%85%8C%EC%8A%A4%ED%8C%85%EC%9D%98-%EA%B8%B0%EC%B4%88)
- 쉽지 않겠지만 조금씩 해보고싶다. 뭔가 내가 이해하고 남도 이해하는 말이되는 코드를 짜고 싶다. 그리고 좋은 방법들을 고민해보고 싶다라는 작은 마음에서 출발..

(연습은 여러 테스트 도구 중 Jest를 이용해서 만들어 볼 예정..)

### 환경 설정

- yarn 또는 npm으로 설치 가능하나 나는 이미 npm이 깔려있었기 때문에 이를 이용했다.
  (요즘 추세도 그렇고 벨로퍼트님도 yarn을 추천하시는거 같긴 함..)

1. 디렉토리 만들고 `npm init -y`
2. 그 안에 jest 설치 `npm install --save jest`

### 나의 첫번째 테스트!

1. 일단 테스트를 할 대상인 간단한 함수파일 생성

```
// sum.js

function sum(a, b) {
  return a + b;
}

module.exports = sum; // 내보내기
```

2. 같은 디렉토리에 테스트 파일 생성

```
//sum.test.js

const sum = require('./sum');

test('1 + 2 = 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

- test: 새로운 테스트 케이스를 만드는 함수, "test" 대신 "it" 사용 가능 작동방식은 똑같음
- expect: 케이스들에 대해 기대하는 값들을 넣어준다. sum이라는 함수에 1과 2를 넣으면 3을 결과로 받을 것이다.
- toBe: matchers라고 부르는 함수로 특정 조건을 만족하는지, 실행 되었는지, 에러났는지 확인한다고 한다. 여기서 toBe는 우리가 정한 결과값을 의미함

3. 테스트 실행

- 터미널을 켜고 `npm test` 명령어 입력
- _!벨로퍼트님의 블로그에는 pakage.json에 scripts를 추가하라고 했는데 갔더니 스크립트에 뭔가 적혀 있길래 걍 나는 명령어를 입력했다. _

```
 PASS  ./sum.test.js
  sum
    ✓ calculates 1 + 2 (1 ms)
    ✓ calculates all numbers (1 ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        0.14 s, estimated 1 s
Ran all test suites.

Watch Usage: Press w to show more.
```

- 짜라란! 위의 결과를 얻게 됨

### 여러 테스트 케이스를 묶어야 할 때

1. describe의 사용

- 테스트 케이스 작성 시 `describe`라는 키워드를 사용하면 여러 테스트 케이스를 묶을 수 있음

- sum.js 파일에 새로운 함수 추가

```
// sum.js
function sum(a, b) {
  return a + b;
}

function sumOf(numbers) {
  let result = 0;
  numbers.forEach(n => {
    result += n;
  });
  return result;
}

// 각각 내보내기
exports.sum = sum;
exports.sumOf = sumOf;

// sumjs 밑에 sumOf라는 새로운 함수 생성
// 여기서 궁금한 점은 함수 표현식으로 사용하려고 했는데 오류가 뜬다. 선언을 해야한다는 오류였는데 왜일까..?
// 그리고 앞서 썼던 module.exports = sum; 이 식은 왜 쓰지 않는걸까..?
```

- sum.test.js 파일에 test묶기

```
const { sum, sumOf } = require('./sum');

describe('sum', () => {
  it('calculates 1 + 2', () => {
    expect(sum(1, 2)).toBe(3);
  });

  it('calculates all numbers', () => {
    const array = [1, 2, 3, 4, 5];
    expect(sumOf(array)).toBe(15);
  });
});

// 위와 같이 describe로 감싸주고 여러 테스트 케이스를 묶을 이름으로 'sum'을 지정하고
안에 콜백의 형태로 실행될 케이스들을 넣어준다.
```
