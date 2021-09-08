## 1강

immutability 개념 먼저 익히면 좋음

리덕스: 예측가능한 상태의 저장소 for js로 만든 애플리케이션

예측가능한: 우리에게 위험한건 복잡성임.애플리케이션의 복잡성을 획기적으로 낮추어 코드가 어떤 결과를 가져올지 예측가능하게 함.

어떻게 이렇게 할 수 있는가?

1. 단 하나의 진실의 원천(Single Source of Truth)

- 하나의 상태를 갖는다. 상태 단지 객체야. 하나의 객체 안에 앱에 필요한 모든 상태를 우겨넣어 관리함. 여러곳에 흩어져 있는것보다 관리하기가 쉬원짐. 단 하나의 상태를 유지하면서 복잡성을 낮춤

2.  외부로부터 철저하게 차단해서 수정하거나 바뀌는걸 막음. 데이터를 직접쓰지 못하도록 reducer, dispatcher통해서만 수정 가능할 수 있게끔. 데이터를 가져갈때도 삼엄하게(get data?)
    외부에서 직접적으로 상태 수정 못하도록 함. 그래서 더욱 예측 가능해짐. state값 바뀔 때마다 부품들에게 전화를 걸어서 할 일을 하도록 하는 그런 중간 통신원 있음.

이렇게 위와 같이 redux는 돌아가고, 이러다보니 undo와 redo가 쉬워짐. 원본을 건들지 않고 실행되기 때문에 독립된 형태로 이용할 수 있음. 리덕스 이용하면 현재 상태 뿐 아니라 과거의 상태또한 디버깅 하기 쉬워짐. 또한 모듈 리로딩 하기가 쉬워짐. 핫 모듈 리로딩 이용하면 애플리케이션은 새로 리로딩 되지만 데이터는 그대로 남아있어 다시 입력 작업을 할 필요 없도록 개발 환경 세팅이 가능해짐

## 2강

리덕스 상당히 추상적인 친구임. 흐름 파악하자!! 리덕스 어떻게 동작하는가 먼저 배울것임
그림 자세히 보기

### state와 render의 관계

redux의 핵심은 store! store 은행이라고 생각해보자. store는 정보가 저장되는 곳 store안에는 state가 저장되어 있음 근데 우리가 직접 state랑 직접 접촉하지 않음. 항상 누군가를 통해서

store만들면서 reducer라는 함수를 만들어서 공급해줘야함. 얘가 제일 어렵게 느껴질 수 있음

```
function reducer(oldState, action){
//...
}
var store = Redux.createStore(reducer);
createStore => store 생성
store생성 시 인자로 reducer(함수) 보냄
```

render

- store 안에 없음. redux 상관없이 ui만들어 주는 역할을 하는 우리가 짤 코드를 의미함.
- store(은행) 앞단에 함수들 dispatch, subscribe, getState(은행원들)있음.

```
function render(){
	var state = store.getState();
    //...
    document.querySelector('#app').innerHTML =
    <h1> WEB </h1>
    ...
}
```

- render는 내부적으로 store에서 데이터를 가져오고 getState는 state값을 가져오고 얘가 render한테 state값을 전달함.
- 이후 render 동작하면서 UI 그리게 됨 render는 state값 참조해서 UI만든다.
- render는 현재 state를 반영한 ui를 만들기 때문에 store의 state바뀔때마다 render호출 할 수 있도록. 그렇게 되면 render 함수만 잘 만들면 state 바뀔때마다 UI 갱신 될 수 있도록 그 때 사용하는게 subscribe 구독 함수.

```
store.subscribe(render);
```

- 위와 같이 subscribe에 render 등록되면 state 바뀔 때마다 state 바뀔때마다 render 호출되며 UI 갱신된다.

### action과 reducer

- render 통해서 state값 참조해서 UI 만든다.

```
<form onsubmit="
//...
store.dispatch({type:'create', payload:{title: title, desc: desc}});
">
```

- action이 dispatch에게 전달이 됨
- dispatch 두가지 일 함

  1. reducer 호출해서 state값을 바꿈
  2. subscribe 이용해서 render 함수를 호출해줌 => 화면 갱신됨

- dispatch가 reducer를 어떻게 다루는가
  dispatch가 reducer를 호출할 때 두 개의 값을 전달함.
  1. 첫번째는 현재의 state값
  2. action 데이터 객체를 전달함

=> reducer는 state를 입력값으로 받고, action을 참조해서 새로운 state값을 만들어서 return해주는 state를 가공해주는 가공자. reducer가 리턴하는 값 새로운 state값
=> 새로운 state값 변경이 되면 render가 다시 호출이 됨(이걸 dispatch가 subscribe에 등록되어있는 등록자들 다 호출하면서 render에 있는 애들이 갱신됨. 즉 UI 가 바뀌게 됨)

- 핵심은 state, state를 기반으로 화면에 그려준다, 중간에 store에 있는 state에 직접 접근할 수 없기 때문에 getState를 통해서 값을 가져오고 dispatch를 통해서 값을 변경하고 subscribe를 이용해서 값이 변경되었을 때 구동될 함수들을 등록해준다.
- reducer를 통해서 state 값을 변경한다.

## 3강 리덕스가 좋은 가장 중요한 이유

redux가 기존의 개발환경에 가져온 혁신,,
redux의 시간여행 도구 각각의 변화가 생겼을 때 어떤 상태인지 확인할 수 있음
redux 중앙집중적인 데이터 스토어를 통해서 앱을 쉽게 개발할 수 있음
리덕스만 가지고 있는 기능인 시간여행 기능

### redux 사용

- 리덕스를 이용한다는 것 -> 스토어를 만드는 것, 스토어의 상태를 바꾸는 것 핵심
- 스토어를 만들면 state 자동으로 생김, reducer함수 만들어서 store에 주입한다.
- reducer의 역할은 dispatch에 의해 action이 들어오면 reducer가 액션값과 기존 state값을 참조해서 새로운 state값을 만들어 주는 것.
- 은행으로 치면 dispatcher가 창구 직원 reducer가 실제 은행 장부에 누가 뭘했는지 적는 사람 state가 그 장부이다.
- reducer는 기존의 state값과 action을 인자로 받음
- store를 처음 만들때 state의 초기값을 필요로 함. 정의되지 않았으면 undefined

다시 정리하면 store를 만들면 내부적으로 state값이 생기고 state값을 가져올때는 getState를 가져와야 한다. 그리고 reducer를 통해서 state값을 만들어줘야하는데 그때의 reducer의 기존 state 값이 undefined라면 그건 초기화를 위해서 최초로 실행되는 reducer에 대한 호출값이기에 원하는 초기값 return하면 redux store에는 초기값이 지정이 됨.
가져온 state값으로 우리 컴포넌트의 초기값을 설정할 수 있음

### state의 값을 변경시키는 방법

- state를 변경시키기 위해서는 action이라는 것을 만들어야 함. 그것을 dispatch에게 제출하면 dispatch가 reducer를 호출하는데 그때 이전의 state값과 action의 값을 동시에 줌.
- reducer함수가 그것을 분석해서 state의 최종적인 값을 반환해주면 되는 것

-store에게 dispatch라는 함수 호출하면서 객체 하나 주는데 그 객체는 무조건 타입을 가지고 있어야 함.
-store의 state 값을 바꾸게 하기 위해서는

- 최초 한번은 무조건 시행이 됨

=> 정리하면 reducer라고 하는 것은 이전의 state와 action을 받아서 다음의 state값을 반환해주는 친구

객체복사 object.assign({},{name: egoing})
assign은 첫번째 인자로 무조건 빈 객체를 주어야 함. 왜냐하면 object assign의 반환값은 첫번째 인자이기 때문에

reducer 함수의 역할은 store의 state 값을 변경해준다. action의 값과 이전의 state값을 이용해서 새로운 state값을 반환하면 그 리턴ㄷ된 값이 새루운 state 값이 된다.
그 때 return 된 값은 원본의 값을 바꾸는 것이 아니라 이전의 값을 복제한 값을 return 해서 사용해야 redux를 최대한 활용해서 사용 가능함.

### state의 변화에 따른 UI 반영

- 이전: action이 일어났을 때 dispatch 통해서 store에 전달. store가 reducer를 호출해서 최종적인 state값을 결정한다.
- state값이 바뀌면 우리의 app UI도 바껴야함. render 호출해주면서 화면에 그려질 수 있게
- state값 바뀔 때마다(dispatch할 때마다) 함수 호출 시킬려면 subscribe에 렌더 등록해놓으면 됨
- redux를 사용하지 않았을 때의 함수는 서로 의존성이 심해졌는데, redux를 사용했을 때는 각각의 부품들이 서로 독립적으로 존재함.
- redux 상태 중앙 집중적으로 관리 각각의 부품들(action을 store에 dispatch해줌) 그에 따라 어떻게 변화되는지에 대한 코드 작성하고 store에 subscribe 하면 state가 바뀔때마다 통보를 받기 때문에 그때마다 자신의 모양을 바꿈.
- 자신의 일에만 집중을 하면 됨

! redux를 사용하는 이유, 부품들이 왜 좋아지는지, 디커플링, 의존성을 낮추고 stand alone할 수 있는지 정리해보기

## 4강 시간여행과 로깅

- dev tool을 이용한 버전 관리
- 버전관리를 통해서 store에 전달된 action들을 replay 할 수 있음
- redux의 dev tool 이용하면 디버깅이 훨씬 쉬워짐
- app을 단순한 코드로 복잡한 app을 만들수 있는게 장점임(redux가 아니더라도 다른걸 이용해서 할 수 있지만 시간여행기능은 redux가 가지는 특징적인 점)

### 불변성(Immutable)

- redux에서 reducer를 통해서 리턴하는 값은 불변해야 함!(원본이 변경되면 안됨. 원본 복제한걸 변경해서 return 해야함)
- action에 의해서 state가 바뀔 때마다 바뀌는 각각의 데이터는 서로 완전히 독립된 데이터여야 함
- 독립성으로 인해서 우리가 시간여행을 할 수 있는 것임

### 단일 스토어

- redux 단 하나의 스토어 유지 그리고 단 하나의 스토어는 reducer를 통해서 가공되기 때문에 app의 상태가 궁금할 때는 reducer를 이용하면 됨.
