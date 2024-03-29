# ⚠️주의
- 클린 코드 !== 좋은 코드. 클린 코드에 매몰되어 광적으로 따르지말자.
- 좋은 코드로 소개되는게 전부 다 나한테 적용될 수 있는 좋은 코드가 아님에 주의하자.
- 유명한 사람들의 코드라고 전부 맞는 것은 아니다. 항상 면밀하게 코드를 살피고 이해하자.
# State
## 올바른 초기값 설정
### 초기값
- 가장 먼저 렌더링 될 때 순간적으로 보여질 수 있는 값이기도 하다.
- 당연히 초기에 렌더링 되는 값
### 초기값을 지키지 않은 경우
- 렌더링 이슈, 무한 루프, 타입 불일치로 의도치 않는 동작 => 런타임 에러
- 넣지 않으면? undefined
- 상태를 CRUD => 상태를 지울 때도 초기값을 잘 기억해놔야 원상태로 돌아간다.
- 빈 값? null 처리를 할 때 불필요한 방어 코드를 줄여준다!
## 업데이트 되지 않는 값
### React에 연결되어 있는 값이 아니면 컴포넌트 외부로 빼내자
![[Pasted image 20240128160328.png]]
위 코드에서 `INFO`가 변하는 로직은 어디에도 없다.
문제는 컴포넌트 내부에 선언되어 있기 때문에 다양한 상황에서 참조로 인한 사이드 이펙트가 발생할 수 있다.
따라서, `INFO`는 React State로 변경하든가, 아예 외부로 내보내는게 좋다.
전자의 방법은 그렇게 좋은 해결법은 아닌 것 같고, 후자를 시도해 보면 아래와 같다.
![[Pasted image 20240128160541.png]]
같은 파일 내에 있기 때문에 참조 에러가 발생하지 않을 뿐더러,
`App`이라는 컴포넌트가 다시 트리거되고, 다시 마운트되고 등등..이기 때문에
`INFO`를 참조하기 위한 불필요한 사이드 이펙트를 제거할 수 있다.
#### 요약
- 업데이터가 되지 않는 일반적인 객체라면 React 외부로 내보내기
## 플래그 상태
### 플래그 값?
- 프로그래밍에서 주로 특정 조건 혹은 제어를 위한 조건을 불리언으로 나타내는 값
![[Pasted image 20240128161600.png]]
위 코드에서는 하나의 상태를 위한 플래그 값이 정말 다양하게 존재하고 있다.
그런데 굳이 이렇게 state로 관리할 필요가 없다.
![[Pasted image 20240128161818.png]]
이렇게 간단하게 변경하면, `hasToken`, `hasCookie`가 변했을 때, 다시 계산되면서 `isLogin`이 재평가 될 것이기 때문이다.
이렇게 플래그 값이 많을 때는 굳이 전부 state로 관리하려고 하지말고, 표현식 하나로 만들어보자.
#### 요약
- `useState` 대신 플래그로 상태를 정의할 수 있다.
## 불필요한 상태
- 초기값을 변경해야하는 경우, 값을 변경해야하는 경우 주로 발생하는 문제
![[Pasted image 20240128171735.png]]
위 코드는 `userList`가 변경될 때마다 `completed` 속성이 `true`인 것들만 `completedUserList`에 넣고자 하는 경우이다.
그런데, 특정 조건인 것들만 따로 모아보기 위해서 state를 사용하는 것은 불필요하다!
그냥 변수로 관리하면 된다. 너무 하드하게 setState를 하려고 하지말자.
![[Pasted image 20240128171657.png]]
### 요약
1. 특정 조건의 값을 필터링하기 위해 새로운 state를 생성해서 관리하지 말자.
2. 컴포넌트 내부 변수는 렌더링 때마다 고유한 값을 가진다.
## useState 대신 useRef
리렌더링 방지가 필요하다면 useState 대신 useRef를 사용해보자.
컴포넌트의 전체적인 수명과 동일하게 지속된 정보를 일관적으로 제공해야하는 경우에 좋다.
![[Pasted image 20240128172054.png]]
아래와 같이 코드를 변경하면, useEffect 로직에서 렌더링을 유발하지 않기 때문에 불필요한 렌더링 방지가 가능하다.
![[Pasted image 20240128172433.png]]
또한, useRef 값이 컴포넌트 수명 주기와 동일하게 진행되기 때문에 일관적인 값을 안전하게 제공할 수 있다.
꼭 DOM에 붙여서 사용하는 것만이 useRef의 전부가 아니라는 것을 기억하자!
### 요약
- useState 대신 useRef를 사용하면, 컴포넌트의 생명주기와 동일한, 리렌더링되지 않는 상태를 만들 수 있다
## 연관된 상태 단순화하기
아래와 같은 상황을 가정해보자.
fetch가 시작되면 `isLoading`만 `true`일 것이 기대된다.
fetch가 성공하면 `isFinish`만 `true`일 것이 기대된다.
fetch가 실패하면 `isError`만 `true`일 것이 기대된다.
즉, 각 상황에 대하여 세 가지 state가 연관되어 있다는 것이다.
![[Pasted image 20240128180121.png]]
3가지의 상태를 따로 관리하지 않고 다음과 같은 방법을 사용해 볼 수 있겠다.
![[Pasted image 20240128181402.png]]
여러 개의 state를 관리하는 것으로 인한 복잡함을 훨씬 줄일 수 있다.
하나의 state 변경으로 인해 나머지 state를 그에 맞춰 동기화 해줘야 하는 상황을 보다 쉽게 처리할 수 있다.
## 연관된 상태 객체로 묶어내기
state 자체를 객체로 관리하는 방법도 있다!
![[Pasted image 20240128191829.png]]
또한 `useReducer`를 사용해도 된다. 다양한 관점이 있구나 생각하는 정도로 살펴보자.
## useState에서 useReducer로 리팩터링
`useReducer`를 사용하여 코드를 축약하면 추상화가 잘 이뤄진다는 것이 좋다!
`reducer` 코드를 다른 라이브러리, 프레임워크에 가져다가 쓸 수 있다는 점도 좋은 것 같다.
![[Pasted image 20240128195036.png]]
## 상태 로직 Custom Hooks로 뽑아내기
렌더링 되는 컴포넌트들만 남기고 나머지 로직을 커스텀 훅으로 추출해 추상화 할 수 있다.
return에 꼭 배열이나 객체의 형태를 넣어야하는게 아니니, 자유롭게 사용할 수 있다.
![[Pasted image 20240209142853.png]]
## 이전 상태 활용하기
setState를 할 때는 이전 값을 명확하게 불러와서 사용하는 것이 더 좋다.
setState가 비동기적으로 처리되기 때문에 예상치 못한 결과값이 나올 수 있기 때문이다.
![[Pasted image 20240209143333.png]]
이전 값을 완전히 덮어쓰는 경우에는 그냥 넣어도 된다.
fasle에서 true라던지..
![[Pasted image 20240209144256.png]]
# Props
## 불필요한 Props 복사 및 연산
불필요한 연산을 줄이기 위해서는
1. props를 바로 사용하기
2. 연산된 값을 props로 넘기기
3. useMemo로 연산 최적화하기
등의 방법이 있다.
![[Pasted image 20240209145824.png]]
## Curly Braces
문자열을 전달하는 경우에는 굳이 중괄호로 감싸서 쓰지말자.
값이 계산되서 들어가야하는 경우에는 중괄호로 감싸야한다.
![[Pasted image 20240209151317.png]]
## Shorthand Props
props를 스프레드 연산자와 사용하면 그대로 하위 컴포넌트에도 전달해야할 때 유용하다.
또한, 파라미터값을 그대로 사용하는 경우에는 true로 들어가기 때문에 코드를 훨씬 간결하게 유지할 수 있다.
![[Pasted image 20240209161459.png]]
## Single Quote vs Double Quotes
왜 주의해야하는가?
1. 팀에서 일반적인 규칙 => 일관성을 지키기 위함
2. HTML, JS에서의 차이를 두는지? => 리액트는 JSX를 사용하기 때문
3. 결정하면 eslint, prettier에 위임하고 개발에 집중하자.
꼭 일관된 스타일을 합의하고 유지하도록 하자.
Prettier의 경우 --single-quote 설정을 통해 JS 코드의 따옴표를 조정할 수 있고,
--jsx-single-quote 설정을 통해 JSX 코드의 따옴표를 조정할 수 있다.
두 차이를 명확히 인지하고 협의, 유지 하도록 하자.
eslint에서도 룰이 다르니 참고!
## 알아두면 좋은 Props 네이밍
리액트 컴포넌트 이름은 PascalCase를 사용하기 때문에
props로 넘기는 값들은 camelCase를 사용하도록하자.
snake_case 등은 일반적으로 사용되지 않으므로 지양하도록한다.
컴포넌트를 props로 넘기는 경우 ~Component 처럼 이름을 붙여 명확하게 확인 가능하도록 하자.
## 인라인 스타일 주의하기
key는 camelCase로 사용해야한다.
중괄호 안에 객체를 넣는 형태로 사용할 수 있다. JSX에서는 중괄호 안에 표현식을 넣어야하기 때문.
![[Pasted image 20240209163206.png]]
이 때, 렌더링 될 때마다 표현식이 평가되기 때문에 변하지 않는 스타일이라면 아래와 같이 컴포넌트 밖으로 빼낼 수도 있다.
![[Pasted image 20240209163357.png]]
객체를 사용한다는 점을 활용해 아래와 같이 만들 수도 있다.
![[Pasted image 20240209163656.png]]
## CSS in JS 인라인 스타일 지양하기
CSS in JS 라이브러리를 사용할 때, JSX에 직접적으로 스타일을 입력하기 보다는
위 예시 처럼 외부에 객체로 뽑아서 모아놓고 관리하는 것이 훨씬 깔끔하다.
가독성도 좋은 점이지만, 자동 완성이나 타입 추론 등을 사용할 수 있기 때문이기도 하다.
또한, 렌더링 마다 평가되지 않기 때문에 최적화에도 도움을 줄 수 있다.
CSS in JS에서 스타일 로직을 분리해서 JSX를 더 간결하게 볼 수 있다.
export 시켜서 재사용성을 키울 수도 있다.
라이브러리마다 다를 수 있지만, 객체로 사용할 경우 CSS property를 camelCase로 작성하도록 변경해줘야한다는 점을 주의하자.
## 객체 props 지양하기
리액트는 Object.is()를 사용하기 때문에 똑같이 생긴 객체, 배열이라도 다르게 인식한다.
이는 렌더링 성능에 큰 영향을 줄 수 있다.
따라서, 아래와 같은 개선 방법을 가져갈 수 있다.
1. 변하지 않는 값일 경우 컴포넌트 외부로 빼낸다
2. 필요한 값만 객체를 분해해서 props로 내려준다.(컴포넌트에 필요한 부분만 전달한다는 뜻)
3. 값 비싼 연산, 너무 잦은 연산이 있을 경우 useMemo 등을 사용해서 계산된 값을 메모이제이션한다.
4. 컴포넌트를 더 평탄하게 나누면, 나눌 props 또한 평탄하게 나눠서 내릴 수 있다.
![[Pasted image 20240209181235.png]]
## HTML Attribute 주의하기
JSX 문법에 익숙해져서 HTML 기본 속성을 잊게 되어버리는 일이 없도록 하자.
class와 className이 그런 대표적인 예시이다. 예약어들을 침범하지 않도록 주의하자!
![[Pasted image 20240209183554.png]]
## ...props 주의할 점
코드를 예측하기가 어려워진다는 단점이 있다. props가 뭔지 한번에 알 수 없기 때문이다.
HOC의 경우 아래와 같이 사용해도 문제는 없겠지만, 그게 아니라면 좀 더 알아보기 쉽게 전달하도록 하자.
![[Pasted image 20240209221522.png]]
관심 있는 props만 분리해서 전달하는 것이 그 방법이다.
![[Pasted image 20240209221735.png]]
## 많은 props 일단 분리하기
