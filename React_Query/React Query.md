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
- 큰 의미가 없어보이지만, cached data가 있을 때와 없을 때의 코드를 구분하는 경우가 있어 활용된다.(e.g. pagination)

# Stale Data
- Why does it matter if the data is stale?
- Data refetch only triggers for stale data
	- For example, component remount, window refocus
	- `staleTime` translates to "max age"(데이터를 허용하는 최대 나이)
	- How to tolerate data potentially being out of date?(데이터가 만료되었다고 판단하는 시간)
	- 즉, `staleTime`을 설정하는 것은 해당 데이터가 언제 만료되는 것인지를 설정하는 것과 같다.
	- `staleTime`이 지나면 데이터가 낡은(stale) 데이터가 된다.
	- 기본값이 0인데, 디폴트로는 항상 서버에서 다시 가져와야 한다고 가정하고 있다는 뜻이 된다. 실수로 클라이언트에 만료된 데이터를 줄 확률을 줄일 수 있기 때문이다.

# staleTime vs cacheTime
- `staleTime` is for re-fetching
- Cache is for data that might be re-used later
	- query goes into "cold storage" if there's no active `useQuery`
	- cache data expires after `cacheTime`(default : 5 min)
		- how long it's been since the last active `useQuery`.
		- 즉, 특정 쿼리에 대한 `useQuery`가 활성화된 후 경과한 시간을 의미한다.
	- After the cache expires, the data is garbage collected
- Cache is backup data to display while fetching
- 새로운 데이터를 수집하는 동안 빈 페이지가 보여지는 것을 막고, 조금 오래된 데이터라도 보여주고 있는 것이다. 이는 활용하기 나름이므로, 사용하지 말아야할 때는 `cacheTime`을 0으로 설정하면 된다.

# Why don't comments refresh?
- 특정 게시물의 코멘트를 fetch하는 `useQuery`를 사용했지만 게시물이 변경해도 코멘트 데이터가 변경되지 않았고, `stale` 상태로 이전과 동일한 데이터를 유지한다...왜???
- Every query uses the same key(comments)
- Data for queries with known keys only refetched upon trigger. 즉, 알려진 key를 사용하는 경우라면 어떠한 트리거가 있어야지만 refetch를 한다는 것이다!
- Example triggers:
	- component remount
	- window refocus
	- running refetch function
	- automated refetch
	- query invalidation after a mutation
- 이러한 이유로 인해서, 데이터가 만료되어도 새 데이터를 가져오지 않았던 것이다.
## solution
- Option : remove programmatically for every new title
	- it's not easy
	- it's not really what we want
- No reason to remove data from the cache...
	- we're not even performing the same query!
- Query includes Post Id
	- Cache on a per-query basis
	- don't share cache for any "comments" query regardless of post id
- What we really want : label the query for each post separately
- Pass array for the query key, not just a string(e.g. `['comments', post.id]`)
- Treat the query key as a dependency array
	- When key changes, create a new query
- Query function values should be part of the key