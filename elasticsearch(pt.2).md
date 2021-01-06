### 검색 API

`SearchRequest` 를 사용하는데 이는 아래 SearchSourceBuilder를 이용하여 입맛대로 검색을 꾸려 사용

`SearchSourceBuilder` 

- query

    `BoolQueryBuilder`

    여러 쿼리를 조합하기 위해 상위에 **bool** 쿼리를 사용하고 그 안에 다른 쿼리들을 넣는 식으로 사용 
    - must, must_not, should, filter
    - 최종적으로 쿼리들을 조합한 `BoolQueryBuilder` 를 query로 넣는다.

    `QueryBuilders`

    ```jsx
    1. existQuery({FIELD_NAME}) : 해당 필드가 존재하는 도큐먼트 검색

    2. matchQuery({FIELD_NAME}, {KEY_WORD}) : 해당 필드의 값이 해당하는 도큐먼트 검색
    3. multiMatchQuery({KEY_WORD}, {[MULTI_FIELDS]}) : matchQuery와 같지만 여러 필드 대상으로 검색 가능

    4. termQuery({FIELD_NAME}, {KEY_WORD}) : 키워드에 대한 분석을 하지않고 온전히 그 키워드와 일치하는 문서를 검색
    5. termsQuery({FIELD_NAME}, {[KEY_WORDS]}) : termQuery와 같지만 여러 키워드 검색 가능
    - 보통 숫자나 boolean 같은 값 검색에 사용
    - match VS term
    : match 쿼리는 키워드에 대해 분석을 통한 검색을하며 이는 bool 쿼리 중에서도 should 조건으로 검색을한다. 즉 검색어를 분석기를 통해 토크나이징해서 토크나이징된 단어들을 찾아 문서들을 검색하고 토크나이징된 단어들이 최소 1개라도 들어있다면 검색 결과에 포함

    6. rangeQuery({FIELD_NAME}) : 해당 필드의 범위에 해당하는 도큐먼트 검색
    - gte({KEY_WORD}) - 이상 
    - gt({KEY_WORD}) – 초과
    - lte({KEY_WORD}) - 이하
    - lt({KEY_WORD}) - 미만

    7. geoDistanceQuery({FIELD_NAME}) : 해당 필드의 위치가 특정 지점으로부터 일정 직선거리 내에 해당하는 도큐먼트 검색
    - distance({DISTANCE}, {UNIT})
    - point({LATITUDE}, {LONGITUDE})

    8. geoBoundingBoxQuery({FIELD_NAME}) : 해당 필드의 위치가 특정 바운더리 내에 해당하는 도큐먼트 검색
    - setCorners({double top}, {double left}, {double bottom}, {double right})

    ```

    `NestedQueryBuilder(String path, QueryBuilder query, ScoreMode scoreMode)`
    - nested 필드는 분리된 히든 도큐먼트로 인덱싱되기 때문에 직접 쿼리 할 수 없고 nested 쿼리 이용해야 함
    - optional : match 된 결과를 return 받기 위해 inner hits 옵션을 사용할 수 있음 (검색 결과는 같음)

    ```jsx
    nestedQueryBuilder.innerHit(new InnerHitBuilder());
    ```
