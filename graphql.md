# GraphQL

✔️ REST API를 알아야 GraphQL의 등장 배경과 목적을 알 수 있다.

### REST API

* GraphQL 이전부터 사용
* ‘다른’ 방식 - 용도와 작업특성에 따라 적합한 것 사용

REST API란 **소프트웨어간 정보를 주고 받는 방식 →** URI 형식 (어떤 정보를) \* 요청 방식 (어떻게 할 것인가)

| 요청 형식     | 용도      |
| --------- | ------- |
| GET       | 정보 받아오기 |
| POST      | 정보 입력하기 |
| PUT/PATCH | 정보 수정하기 |
| DELETE    | 정보 삭제하기 |

**REST API의 한계**

* 내가 원하지 않는 정보까지 받아와서 낭비되는 데이터 양 증가 (네트워크 비용, 시간) → overfetching
* 내가 원하는 정보가 여러 계층에 걸쳐 있을 때, 요청을 여러 번 해야함 → underfetching

***

### SQL vs GQL

**SQL(Structured Query Language)**

* 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는 것이 목적
* 주로 백엔드 시스템에서 작성하고 호출
*   _쿼리 예시_

    ```sql
    SELECT name, age, gender, weight, ROUND(weight / 1000.0, 2)
    FROM User;
    ```

**GQL(Graph Query Language)**

* 웹 클라이언트가 데이터를 서버로부터 효율적으로 가져오는 것이 목적
* 주로 클라이언트 시스템에서 작성하고 호출
*   _쿼리 예시_

    ```graphql
    {
    	hero {
    		name
    		friends {
    			name
    		}
    	}
    }
    ```

## GraphQL

* Facebook에서 개발된 오픈 소스 쿼리 언어
* 클라이언트는 GraphQL 서버로 쿼리를 전송하고, 서버는 해당 쿼리를 해석하고 데이터를 반환 (이 때, 클라이언트가 요청한 필드만 반환되므로 **over fetching**을 줄여 효율적이다)
* REST API 와 마찬가지로 어떠한 ‘형식’

### **GraphQL의 강점**

* 필요한 정보들만 선택하여 받아올 수 있음
  * Overfetching 문제 해결
  * 데이터 전송량 감소
* 여러 계층의 정보들을 한번에 받아올 수 있음
  * Underfetching 문제 해결
  * 요청 횟수 감소
* 하나의 endpoint에서 모든 요청을 처리
  * 하나의 URI에서 POST로 모든 요청 가능

### GraphQL의 구조

query와 mutation 그리고 응답 내용의 구조는 상당히 직관적

_GraphQL 쿼리문(좌측)과 응답 데이터 형식(우측)_

GQL에서 query와 mutation을 나누는데 사실상 별 차이는 없다. **query는 데이터를 읽는데(R)** 사용하고, **mutation은 데이터를 변조(CUD)** 하는데 사용한다는 개념적인 규약을 정해 놓은 것 뿐이다.

**오브젝트 타입과 필드**

```graphql
type EquipmentAdv {
    id: ID!
    used_by: Role!
    count: Int!
    use_rate: Float
    is_new: Boolean!
    users: [String!]
  }
```

* 오브젝트 타입: EquipmentAdv
* 필드: id, used\_by …
*   스칼라 타입: String, Int 등

    | 타입      | 설명                               |
    | ------- | -------------------------------- |
    | ID      | 기본적으로는 String이나, 고유 식별자 역할임을 나타냄 |
    | String  | UTF-8 문자열                        |
    | Int     | 부호가 있는 32비트 정수                   |
    | Float   | 부호가 있는 부동소수점 값                   |
    | Boolean | 참/거짓                             |
* 느낌표(!):  \*\*\*\*Non Null (_null_을 반환할 수 없음)
*   대괄호(\[, ]) : 배열(array)

    | 선언부         | users: null | users: \[] | users: \[…, null] |
    | ----------- | ----------- | ---------- | ----------------- |
    | \[String]   | O           | O          | O                 |
    | \[String!]  | O           | O          | X                 |
    | \[String]!  | X           | O          | O                 |
    | \[String!]! | X           | O          | X                 |

### Rest API vs GraphQL

| Rest API                                                | GraphQL                                          |
| ------------------------------------------------------- | ------------------------------------------------ |
| HTTP 요청방식 (GET, POST, PUT, DELETE 등)을 사용하여 데이터를 요청하고 응답 | HTTP 요청방식을 사용하지만 POST만 사용                        |
| 상황에 따라 다른 Method를 사용해야 하고 API별 각각의 Endpoint 가짐          |                                                  |
| (각 Endpoint마다 데이터베이스 SQL 쿼리가 달라짐)                       | 단일 Endpoint                                      |
| (GQL 스키마의 타입마다 데이터베이스 SQL 쿼리가 달라진다)                     |                                                  |
|                                                         | 요청시 body schema에 맞춰 query로 요청하기 때문에 일관성 있는 통신 가능 |

_REST API와 GraphQL API의 사용_

💡 REST API와 GraphQL의 특성과 차이를 감안해서 더 적합한 방식을 선택해서 적용하는게 좋다.

### more..

* 서버사이드 GQL 어플리케이션은 GQL로 작성된 쿼리를 입력 받아 처리한 결과를 다시 클라이언트에 돌려줌
* HTTP API 자체가 특정 데이터베이스나 플랫폼에 종속적이지 않은것 처럼 GQL 역시 어떠한 특정 데이터베이스나 플랫폼에 종속적이지 않음 (네트워크 방식 포함)
* 일반적으로 GQL의 인터페이스간 송수신은 네트워크 레이어 L7의 HTTP POST 메서드와 웹소켓 프로토콜을 활용
* 필요에 따라서 L4의 TCP/UDP를 활용하거나 L2 형식의 이더넷 프레임을 활용할 수 있음

_GraphQL 파이프라인_

_HTTP와 gql의 기술 스택 비교_

✔️ \*\*GraphQL은 명세, 형식일 뿐. GraphQL을 구현할 솔루션이 필요함\*\*

* 백엔드에서 정보 제공 및 처리
* 프론트엔드에서 요청 전송
* [GraphQL.js](https://www.npmjs.com/package/graphql), [GraphQL Yoga](https://github.com/dotansimha/graphql-yoga), [AWS Amplify](https://docs.amplify.aws), [Relay](https://relay.dev/)…
* [기타 솔루션](https://graphql.org/community/tools-and-libraries)

1. [Hasura](https://hasura.io/)
2. [Apollo GraphQL](https://www.apollographql.com)
   1. 백엔드와 프론트엔드 모두 제공
   2. 간편하고 쉬운 설정
   3. 풍성한 기능들 제공

[**apollo-server**](https://www.apollographql.com/docs/apollo-server/api/apollo-server/) **생성자 인자 모듈화**

* **typeDefs**: 단일 변수 또는 배열로 지정 가능
* **resolvers**: 단일 Object 또는 Merge 된 배열로 가능

***

### 📌 참고

* [https://devocean.sk.com/blog/techBoardDetail.do?ID=164787](https://devocean.sk.com/experts/techBoardDetail.do?ID=164787\&boardType=experts\&page=\&searchData=\&subIndex=\&idList=)
* [https://blog.naver.com/PostView.naver?blogId=rinjyu\&logNo=223047988880\&categoryNo=67\&parentCategoryNo=0\&viewDate=\&currentPage=2\&postListTopCurrentPage=\&from=postList\&userTopListOpen=true\&userTopListCount=5\&userTopListManageOpen=false\&userTopListCurrentPage=2](https://blog.naver.com/PostView.naver?blogId=rinjyu\&logNo=223047988880\&categoryNo=67\&parentCategoryNo=0\&viewDate=\&currentPage=2\&postListTopCurrentPage=\&from=postList\&userTopListOpen=true\&userTopListCount=5\&userTopListManageOpen=false\&userTopListCurrentPage=2)
* [https://graphql-kr.github.io/learn/queries/](https://graphql-kr.github.io/learn/queries/)
