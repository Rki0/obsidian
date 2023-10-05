# Client State vs Server State
1. Client State
	- Information relevant to browser session
	- EX) user's chosen language or theme
2. Server State
	- Information stored on server
	- EX) blog post data from database
	- 여러 클라이언트에 표시할 수 있도록 서버에 저장

# What problem does React Query solve?
- React Query maintains cache of server data on client
- Fetch나 Axios를 사용해서 서버로 바로 이동하지 않고, RQ의 cache를 요청한다.

# React Query Manage Data
- 데이터를 관리하지만 설정은 개발자의 몫이다!
- indicate when to update cache with new data from server
- 그러면 어떻게 서버 데이터와 캐시 데이터가 다른지 판단하지?
- imperatively(명령적) : invalidate data(클라이언트의 데이터를 무효화하고, 캐시에 교체할 새 데이터를 서버에서 가져오게 명령한다)
- declaratively(선언적) : configure how(e.g. window focus) & when to trigger a re-fetch(Refetch를 트리거하는 조건을 구성한다. 예를들어, 브라우저 창이 다시 포커스 되었을 때 처럼.)

# Plus...
- Loading / Error states
	- 서버에 대한 모든 쿼리의 로딩 및 오류 상태를 maintain하기 때문에, 이를 수동(manually)으로 처리해줄 필요가 없다.
- Pagination / infinite scroll
- Prefetching
	- 유저가 원할만할 데이터를 예측해서 미리 가져와 캐시에 넣어놓으면, 그 데이터가 필요할 때 서버에 연결할 때까지 기다릴 필요가 없다.
- Mutations
	- 서버 데이터의 변화를 관리할 수 있다.
- De-duplication of requests
	- 요청을 관리할 수 있고, 여러 페이지가 동일한 요청을 보내는 경우 하나로 묶어서 보낼 수 있다. 기존 쿼리가 나가는 동안 다른 구성 요소가 데이터를 요청하는 경우, 중복 요청을 제거할 수 있다.
- Retry on error
- Callbacks
	- 에러나 응답에 대해 콜백을 전달할 수 있다.

# isFetching vs isLoading
1. isFetching
	- the async query function hasn't yet resolved
2. isLoading
	- no cached data, plus isFetching
	- 즉, `isFetching` + 이 쿼리를 만든 적이 없다. fetch 중이고, 표시할 캐시 데이터도 없다는 뜻.

- 즉, `isLoading`은 `isFetching`의 subset이다.
- 큰 의미가 없어