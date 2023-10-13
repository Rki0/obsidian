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
- `isFetching`을 사용하면 캐시 데이터가 있더라도 true가 되기 때문에, pagination 같은 경우 UX를 위해 `isLoading`을 사용하는게 더 좋다.

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

# Pagination
- Track current page in component state(`currentPage`)
- Use query keys that include the page number(`["posts", currentPage]`)
- User clicks "next page" or "previous page" button
	- update `currentPage` state
	- fire off new query

# Pre-fetching
- Pagination를 구현해서 다음 페이지로 넘어갈 때, 데이터를 불러오는 것 때문에 로딩이 걸린다.
- 이는 UX에 좋지않다. Pre-fetching을 통해 미리 다음 데이터를 가져와 캐시에 넣어놓는 것으로 로딩이 보이는 문제를 해결해보자.
- adds data to cache
- automatically stale(configurable)
- shows while re-fetching
	- as long as cache hasn't expired!
- Prefetching can be used for any anticipated data needs
	- not just pagination!

# Mutations
- Mutation : making a network call that changes data on the server
	- jsonplaceholder API doesn't change server
	- go through the mechanics of making the change
- Day Sap app will demonstrate showing changes to user:
	- 변경 사항을 유저에게 보여주는 방법은 여러 가지가 있다.
	- Optimistic updates(assume change will happen) : 서버로 전달되는 변경이 잘 됐다고 가정하고 진행하는 방법. 만약, 잘 안 됐다면 롤백한다.
	- Update React Query cache with data returned from the server
	- Trigger re-fetch of relevant data(invalidation)

# useMutation
- Similar to `useQuery`, but :
	- returns `mutate` function
	- doesn't need query key
	- `isLoading` but no `isFetching`
	- by default, no retries(configurable!)

# Infinite Scroll
- fetch new data "just in time" as user scrolls
- more efficient than fetching all data at once
- Fetch new data when...
	- user click a button
	- user scrolls to certain point on the page
## vs pagination
- pagination
	- track current page in component state
	- new query updates page number
	- 다른 페이지 데이터 셋에 생기는 방식
- `useInfiniteQuery` tracks next query
	- next query is returned as part of the data
	- 즉, 원래 데이터 배열에 추가되는 방식

# Shape of useInfiniteQuery Data
- Shape of data different than `useQuery`
- Object with two properties : 
	- pages
	- pageParams
- Every query has its own element in the `pages` array
- `pageParams` tracks the keys of queries that have been retrieved
	- rarely used, won't use here

# useInfiniteQuery Syntax
- `pageParams` is a parameter passed to the queryFn
- Current value of `pageParams` is maintained by React Query
- `useInfiniteQuery` options
	- `getNextPageParam` : lastPage, allPages
		- 다음 페이지로 가는 방식을 정의한다.
		- Updates `pageParam`
		- Might use all of the pages of data(`allPages`)
		- we will use just the `lastPage` of data(specifically the `next` property)

# useInfiniteQuery Return Object Properties
- `fetchNextPage`
	- function to call when the user needs more data
	- 즉, 스크롤에서 데이터가 소진되는 지점에서 사용될 함수
- `hasNextPage`
	- Based on return value of `getNextPageParam`
	- 마지막 쿼리의 데이터를 어떻게 사용할지 지시한다.
	- if `undefined`, no more data
- `isFetchingNextpage`
	- For displaying a loading spinner
	- We'll see an example of when it's useful to distinguish between `isFetching` and `isFetchingNextPage`

# Bi-directional Scrolling
- Bi-directional is useful when starting in the middle
- All `next` methods and properties have equivalent for `previous`

# React Query in Larger App
- centralizing fetching indicator / error handling
- refetching data
- integrating with auth
- dependent queries
- testing

# custom hooks
- in larger apps, make custom hook for each type of data
	- can access from multiple components
	- no risk of mixing up keys
	- query function encapsulated in custom hook
	- abstracts implementation from display layer
		- update hook if you change implementation
		- no need to update components

# useIsFetching
- in smaller apps
	- used `isFetching` from `useQuery` return object

- in larger app
	- Loading spinner whenever any query `isFetching`
	- `useIsFetching` tell us this!

- No need for `isFetching` on every custom hook / `useQuery` call

# QueryClient default onError option
- No `useError` analogy for `useIsFetching`
	- need more than integer; unclear how to implement
- Instead, set default `onError` handler for QueryClient
	- defaults for QueryClient

# Alternative to onError : Error Boundary
- handler errors with React Error Boundary
- `useErrorBoundary` for `useQuery`
- option to `useQuery / useMutation`
	- or in `defaultOptions` when creating QueryClient
- Set to true to propagate errors to the nearest error boundary

# Options for pre-populating data
- method
	- where to use?
	- data from?
	- added to cache?

- `prefetchQuery`
	- method to `queryClient`
	- server
	- yes

- `setQueryData`
	- method to `queryClient`
	- client
	- yes

- `placeholderData`
	- method to `useQuery`
	- client
	- no

- `initialData`
	- method to `useQuery`
	- client
	- yes

# Prefetch Treatments
- saw prefetch with pagination
	- prefetch next page
- different trigger : prefetch treatments on home page load
	- user research : 85% of home page loads are followed by treatments tab loads
	- 그러면 유저가 클릭하기 전에 미리 treatments 데이터를 가져오면 좋겠다!
	- Treatments don't change often, so cached data isn't really a problem
- garbage collected if no `useQuery` is called after `cacheTime`
	- if typically not loaded by default `cacheTime`(5 min), specify longer cacheTime

- `prefetchQuery` is a method on the `queryClient`
	- adding to the client cache
- `useQueryClient` returns `queryClient`(with Provider)
- Make a `uesPrefetchTreatments` hook within `useTreatments.ts`
	- uses the same query function and key as the `useTreatments` call
	- call `usePrefetchTreatments` from Home component

# Filtering with the select option
- Allow user to filter out any appointments that aren't available
- Why is the select option the best way to do this?
	- React Query memo-izes to reduce unnecessary computation
	- tech details : 
		- triple equals comparison of select function
		- only runs if data changes and the function has changed
	- need a stable function(`useCallback` for anonymous function)

# Re-fetching! Why? When?
- Re-fetch ensures stale data gets updated from server
	- seen when we leave the page and refocus
- Stale queries are re-fetched automatically in the background when :
	- New instances of the query mount
	- Every time a react component(that has a useQuery call) mounts
	- The window is refocused
	- The network is reconnected
	- configured `refetchInterval` has expired
		- Automatic polling

# Re-fetching! How?
- Control with global or query-specific options :
	- `refetchOnMount`, `refetchOnWindowFocus`, `refetchOnReconnect`, `refetchInterval`
	- Or, imperatively : `refetch` function in `useQuery` return object

# Suppressing Re-Fetch
- How?
	- Increase stale time
	- turn off `refetchOnMount`, `refetchOnWindowFocus`, `refetchOnReconnect`
- Only for very rarely changed, not mission-critical data
	- treatments or staff(definitely no appointments!)
- Ask : is it worth it?

# Update Global Settings
- Global default options vs individual query options
- Here, want settings for everything but appointments
	- User profile and user appointments invalidated after mutations
	- Appointments get special settings(including auto-refetching on interval)

# Polling / Auto Re-Fetching
- Appointments is the opposite of treatments and staff
	- We want it to be updated even if the user doesn't take action
	- Appointments may change on server, want to keep up-to-date
- Overrode defaults for `staleTime`, `cacheTime`, `refetchOn*`
- Use `refetchInterval` option to useQuery

# JWT Authentication
- server sends token on successful login(or user creation)
- client sends token in headers with requests as proof of identify
## Security
- token contains encoded information such as the username and user ID
- decoded and matched on the server

# React Query and Auth
- Who should "own" the user data, `useAuth` or `useQuery`?
	- Should `useAuth` call `useQuery`, or make the axios call directly?
	- Should `useAuth` have a provider that stores data, or store user data in React Query cache?
## Separation of Concerns
- React Query : provide cache for "sever state" on the client
- `useAuth` : provides functions for signin/signup/signout
- Conclusion : React Query will store data(via `useUser`)
- `useAuth` collects user data from call to server(add to cache)
## Role of useUser
- Return `user` data from React Query
	- Load from `localStorage` on initialization
- Keep user data up to date with server with `useQuery`
	- query function return `null` if no user logged in
- Whenever user updates(sign in/ sign out / mutation)
	- update React Query cache via `setQueryData`
	- update `localStorage` in `onSuccess` callback
		- `onSuccess` runs after :
		- `setQueryData`
		- query function

# Why not store user data in Auth provider?
- Definitely an option
- Disadvantage is added complexity
	- Separate Provider(Context) to create/maintain
	- Redundant data in React Query cache vs dedicated Auth Provider
- Starting fresh : store in React Query cache, forgo Auth Provider
- Legacy project : may be more expedient to maintain both

# Set query cache values in useAuth
- React Query acting as a provider for auth
- Use `queryClient.setQueryData`(쿼리 키와 값을 가져와 쿼리 캐시에 해당 키에 대한 값을 설정할 수 있다.)

# Setting Initial Value
- Use `initialData` value to `useQuery`
	- For use when you want initial value to be added to the cache
	- For placeholder, use `placehodlerData` or default destructed value
- Initial value will come from `localStorage`

# Dependent Queries
- Separate query for user appointments
	- Change more frequently than user data
- Make the query dependent on `user` being truthy

# Remove userAppointments Query
- Make sure user appointments data is cleared on sign out
	- `queryClient.removeQureis`
- Why not use `removeQureis` for user data?
	- `setQueryData` invokes `onSuccess`(`removeQuries` does not)
- `userAppointments` does not need `onSuccess` for useUser

# useMutation
- 일회성이다.
- very similar to `useQuery`!
- Differences
	- no cache data
	- no retries
	- no refetch
	- no `isLoading` vs `isFetching` : because there is no cache
	- returns `mutate` function which actually runs mutation
	- `onMutate` callback(useful for optimistic queries!)

# invalidateQueries
- Invalidate appointments cache data on mutation
	- so user doesn't have to refresh the page
- `invalidateQueries` effects : 
	- marks query as stale
	- triggers re-fetch if query currently being rendered

# Query key Prefixes
- Goal : invalidate all queries related to appointments on mutation
- `invalidateQueries` takes a query key **prefix**
	- invalidate all related queries at once
	- can make it exact with `{exact : true}` option
	- other queryClient methods take prefix too(like `removeQueries`)

# Optimistic Updates
- update cache before response from server
- you're "optimistic" that the mutation will work

- cache gets updated quicker
	- especially useful if lots of components rely on it
	- what if your optimism was unfounded and the server update fails?
## Rollback / Cancel Query
- `useMutation` has an `onMutate` callback
	- returns context value that's handed to `onError` for rollback
	- context value contains previous cache data
- `onMutate` function can also cancel refetches-in-progress
	- don't want to overwrite optimistic update with old data from server!

# Making Query "Cancel-able"
- Query functions haven't been cancelable by React Query
- In order to cancel from React Query, query function must :
	- return a promise with a `cancel` property that cancels query

# Manually Canceling Query
- React Query uses AbortController to cancel queries
	- standard JavaScript interface, send AbortSignal to DOM request
- Automatically canceled queries use this signal "behind the scenes"
	- out-of-date or inactive queries
- Manually canceled axios query :
	- pass `signal` to axios via argument to query function