
Infinity Query를 내장으로 하고 있으며, 이를 스크롤로 마지막 요소 혹은 원하는 요소가 보이거나
스크롤의 값에 따라 다음 page를 fetch 해주는 기능이다.

[참고](https://tanstack.com/query/v4/docs/react/reference/useInfiniteQuery)


```jsx

const {
fetchNextPage,  fetchPreviousPage,  hasNextPage,  hasPreviousPage,  isFetchingNextPage,  isFetchingPreviousPage,  data, isLoading...
} = useInfinityQuery(queryKey, ({ pageParam = 1 }) => queryFn(pageParam), {
	option
})

```


### 1. 첫 번째 인수 - queryKey

이는 useQuery에서 사용했던 것과 같이 고유값을 지정해준다.
임의로 지정할 수 있으,며, 불러오는 데이터 마다 다른 key 값을 사용해주어야 한다.


### 2. 두 번째 인수 - fetch 함수

pageParam을 동해서 페이지의 기본 값을 지정해줄 수 있으며, 다음에는
2 페이지의 데이터를 요청할 수 있도록 반환값에 결과값과 함께 담아서 내보낼 수 있다.
이에 관련된 기능들은 option을 통해서 설정해줄 수 있다.

#### 3. 세 번째 인수 - options

```jsx
{
	...options,
	getNextPageParam : (lastPage, allPages) => lastPage ...
}
```

options는 추가적인 기능을 제공한다.


`getNextPageParam : (lastData, allData) => unknown | undefined`
 
-  불러올 데이터가 더 있는지의 여부, fetch할 정보를 결정할 때 사용할 수 있음. 이 정보는 query 함수에 추가 매개 변수로 제공된다고 한다.
- `undefined` 가 아닌 다른 값을 반환할 경우 `hasNextPage`는 `True`가 된다.
