## controller 패키지

| 클래스 이름 | 설명 |
|:-------|:---|
AuthController		|	로그인, 회원가입 관련 API|
MemberController	|	프로필 조회/수정, 탈퇴, 내 리뷰 보기 등|
BakeryController	|	빵집 검색, 정보 조회 등|
MenuController	    |	메뉴 조회, 메뉴 리뷰 관련|
ReportController	|	제보 관련 API|
CourseController	|	빵지순례글 관련 API|
FavoriteController	|	즐겨찾기 관련 API|

<br/>

## 각 클래스의 소속 기능들

### 1. AuthController
로그인, 회원가입 관련 API
- 로그인하기
- 회원가입하기
- 로그아웃하기


### 2. MemberController
프로필 조회/수정, 탈퇴 등
- 회원 탈퇴하기
- 프로필 수정

- 내가 작성한 루트 보기

- 내가 작성한 가게 리뷰 보기
- 내가 작성한 메뉴 리뷰 보기
- 내가 작성한 루트 리뷰 보기

- 관심 가게 목록 보기
- 관심 루트 목록 보기



### 3. BakeryController
빵집 검색, 정보 조회 등
- 가게 정보 보기 
- 가게 검색하기 
- 가게 정렬하기 

- 가게 리뷰 쓰기 
- 가게 리뷰 수정하기 
- 가게 리뷰 삭제하기


### 4. MenuController
메뉴 조회, 메뉴 리뷰 관련 
- 가게 메뉴 목록 보기 
- 메뉴 세부 정보 보기 

- 메뉴 리뷰 쓰기 
- 베뉴 리뷰 수정하기 
- 메뉴 리뷰 삭제하기


### 5. ReportController
제보 관련 API
- 제보 보기 
- 제보하기 
- 제보 수정하기 
- 제보 삭제하기


### 6. CourseController
빵지순례글(루트) 관련 API
- 루트 작성하기
- 루트 수정하기
- 루트 삭제하기

- 루트 세부 글 보기 
- 루트 검색하기 
- 인기 루트 목록 보기

- 루트 리뷰 쓰기 
- 루트 리뷰 수정하기 
- 루트 리뷰 삭제하기


### 7. FavoriteController
즐겨찾기(관심) 관련 API
- 관심 가게 추가하기 
- 관심 가게 삭제하기

- 관심 루트 추가하기 
- 관심 루트 삭제하기

<br/>

## 클래스 설계
Controller는 외부의 관점에서 리소스 중심으로(호출 시 연관을 가진 애들끼리) 통합하고, Service는 내부의 관점에서 기능 중심으로 분리

### 서비스 클래스와의 차이점
1. ReviewController → 각 도메인 컨트롤러로 통합. 가게 리뷰는 가게에서 메뉴 리뷰는 메뉴에서 루트 리뷰는 리뷰에서 처리
2. MenuController와 BakeryController는 분리된 채 사용. 단, 메뉴는 반드시 베이커리의 하위에서 존재해야 하므로 api 경로 상에서는 메뉴의 앞에 항상 베이커리가 존재해야 함
3. 내가 즐겨찾기 한 가게 보기 등 ‘나’를 기준으로 이루어지는 모든 기능은 MemberController로 통합함
4. 인증이 필요한 기능들의 경우 프론트 측에서 헤더에 토큰 정보를 포함해야 함

<br/>

## API 명세서
컨트롤러 클래스 내부 메소드의 api만을 설계. 메소드명, 파라미터, 리턴 등은 추후 추가할 예정

### 1. AuthController : 인증 API

| 기능 | HTTP Method | API 경로         | 설명 |
| :--- | :--- |:---------------| :--- |
| 회원가입하기 | `POST` | `/auth/signup` | 사용자 정보를 받아 계정을 생성합니다. |
| 로그인하기 | `POST` | `/auth/login`  | 로그인 정보를 받아 JWT 같은 인증 토큰을 발급합니다. |
| 로그아웃하기 | `POST` | `/auth/logout` | 현재 사용자의 인증 상태를 무효화합니다. |

로그인과 로그아웃 시 서버의 상태를 변경하여야 하므로 **POST** 메소드를 사용한다. 또한 로그인 시 사용자의 아이디와 비밀번호를를 본문에 담아 보내야 하므로 POST를 사용할 수밖에 없다

---

### 2. MemberController : 사용자 정보 및 활동 API

| 기능 | HTTP Method | API 경로 | 설명                                                 |
| :--- | :--- | :--- |:---------------------------------------------------|
| 내 프로필 조회 | `GET` | `/api/members/me` | 현재 로그인된 사용자의 프로필 정보를 조회합니다.                        |
| 프로필 수정 | `PATCH` | `/api/members/me` | 현재 로그인된 사용자의 프로필 정보를 수정합니다.                        |
| 회원 탈퇴하기 | `DELETE` | `/api/members/me` | 현재 로그인된 사용자의 계정을 삭제(비활성화)합니다.                      |
| 내가 작성한 루트 보기 | `GET` | `/api/members/me/courses` | 내가 작성한 모든 빵지순례 루트 목록을 조회합니다.                       |
| 내가 작성한 리뷰 보기 | `GET` | `/api/members/me/reviews` | `?category=` 쿼리 스트링으로 리뷰 종류(베이커리, 메뉴, 루트)를 필터링합니다. |
| 관심 목록 보기 | `GET` | `/api/members/me/favorites`| `?category=` 쿼리 스트링으로 관심 목록 종류(베이커리, 루트)를 필터링합니다.  |

---

### 3. BakeryController : 빵집 및 가게 리뷰 API

| 기능         | HTTP Method | API 경로 | 설명                                           |
|:-----------| :--- | :--- |:---------------------------------------------|
| 가게 검색/정렬하기 | `GET` | `/api/bakeries` | `?name=` 또는 `?sort=` 같은 쿼리 스트링으로 검색 및 정렬합니다. |
| 가게 정보 보기   | `GET` | `/api/bakeries/{bakeryId}` | 특정 가게의 상세 정보를 조회합니다. (리뷰 정보 포함)              |
| 가게 리뷰 쓰기   | `POST` | `/api/bakeries/{bakeryId}/reviews` | 특정 가게에 새로운 리뷰를 작성합니다.                        |
| 가게 리뷰 수정하기 | `PATCH` | `/api/bakeries/{bakeryId}/reviews/{reviewId}` | 특정 가게 리뷰를 수정합니다.                             |
| 가게 리뷰 삭제하기 | `DELETE` | `/api/bakeries/{bakeryId}/reviews/{reviewId}` | 특정 가게 리뷰를 삭제합니다.                             |

가게 리뷰 목록 보기의 경우 가게 정보 보기에 항상 포함되는 정보로 해당 api에 연결하였다

---

### 4. MenuController : 메뉴 및 메뉴 리뷰 API

| 기능 | HTTP Method | API 경로 | 설명 |
| :--- | :--- | :--- | :--- |
| 가게 메뉴 목록 보기 | `GET` | `/api/bakeries/{bakeryId}/menus` | 특정 가게의 모든 메뉴 목록을 조회합니다. |
| 메뉴 세부 정보 보기 | `GET` | `/api/bakeries/{bakeryId}/menus/{menuId}` | 특정 메뉴의 상세 정보를 조회합니다. |
| 메뉴 리뷰 쓰기 | `POST` | `/api/bakeries/{bakeryId}/menus/{menuId}/reviews` | 특정 메뉴에 새로운 리뷰를 작성합니다. |
| 메뉴 리뷰 수정하기 | `PATCH` | `/api/bakeries/{bakeryId}/menus/{menuId}/reviews/{reviewId}` | 특정 메뉴 리뷰를 수정합니다. |
| 메뉴 리뷰 삭제하기 | `DELETE` | `/api/bakeries/{bakeryId}/menus/{menuId}/reviews/{reviewId}` | 특정 메뉴 리뷰를 삭제합니다. |

---

### 5. ReportController : 제보 API

| 기능 | HTTP Method | API 경로                    | 설명                    |
| :--- | :--- |:--------------------------|:----------------------|
| 제보 목록 보기 | `GET` | `/api/bakeries/{bakeryId}/reports` | 특정 가게의 제보 목록을 조회합니다.  |
| 제보하기 | `POST` | `/api/bakeries/{bakeryId}/reports`            | 특정 가게에 새로운 제보를 작성합니다. |
| 제보 수정하기 | `PATCH` | `/api/bakeries/{bakeryId}/reports/{reportId}` | 특정 제보를 수정합니다.         |
| 제보 삭제하기 | `DELETE` | `/api/bakeries/{bakeryId}/reports/{reportId}` | 특정 제보를 삭제합니다.         |

---

### 6. CourseController : 빵지순례(루트) 및 루트 리뷰 API

| 기능 | HTTP Method | API 경로 | 설명 |
| :--- | :--- | :--- | :--- |
| 루트 검색/목록 보기 | `GET` | `/api/courses` | `?keyword=` 또는 `?sort=popular` 등으로 검색/정렬합니다. |
| 루트 세부 글 보기 | `GET` | `/api/courses/{courseId}` | 특정 루트의 상세 내용을 조회합니다. |
| 루트 작성하기 | `POST` | `/api/courses` | 새로운 루트를 작성합니다. |
| 루트 수정하기 | `PATCH` | `/api/courses/{courseId}` | 특정 루트를 수정합니다. |
| 루트 삭제하기 | `DELETE` | `/api/courses/{courseId}` | 특정 루트를 삭제합니다. |
| 루트 리뷰 쓰기 | `POST` | `/api/courses/{courseId}/reviews` | 특정 루트에 새로운 리뷰를 작성합니다. |
| 루트 리뷰 수정하기 | `PATCH` | `/api/courses/{courseId}/reviews/{reviewId}` | 특정 루트 리뷰를 수정합니다. |
| 루트 리뷰 삭제하기 | `DELETE` | `/api/courses/{courseId}/reviews/{reviewId}` | 특정 루트 리뷰를 삭제합니다. |

---

### 7. FavoriteController : 즐겨찾기(관심) 추가/삭제 API

| 기능 | HTTP Method | API 경로 | 설명 |
| :--- | :--- | :--- | :--- |
| 관심 가게 추가하기 | `POST` | `/api/favorites/bakeries/{bakeryId}` | 특정 가게를 즐겨찾기에 추가합니다. |
| 관심 가게 삭제하기 | `DELETE` | `/api/favorites/bakeries/{bakeryId}` | 특정 가게를 즐겨찾기에서 삭제합니다. |
| 관심 루트 추가하기 | `POST` | `/api/favorites/courses/{courseId}` | 특정 루트를 즐겨찾기에 추가합니다. |
| 관심 루트 삭제하기 | `DELETE` | `/api/favorites/courses/{courseId}` | 특정 루트를 즐겨찾기에서 삭제합니다. |

---

