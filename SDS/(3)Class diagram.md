# 3. Class diagram



## 3.1. Class diagram

- 이곳에 작성된 클래스 다이어그램은 시스템의 핵심 도메인 모델을 표현하는 것을 목적으로 하였다. 
- 클래스 다이어그램은 Member 클래스 다이어그램, bakery 클래스 다이어그램, Course 클래스 다이어그램, Menu 클래스 다이어그램, Favorite(좋아요) 클래스 다이어그램, review 클래스 다이어그램으로 나누어 작성하였다.
- 가독성을 위해 메인 로직에서 벗어난 Getter/Setter 메소드를 의도적으로 생략하였으며, DTO 클래스들을 포함하지 않았다. 
- 계층간 데이터 전송을 위한 DTO와 프론트와의 소통을 위한 API의 경우 `3.1. Class diagram` 아래 `3.2. DTO`와 `3.3. API`에서 따로 서술하였다.


## 1) Member Class diagram
(여기에 클래스 다이어그램 그림)
(해당 클래스 다이어그램에 대한 설명) 보안 관련은 auth로 빼고 본인 관련은 전부 Member 도메인을 사용함. 회원가입 로그인부터 시작해 본인의 글 모아보기 등의 마이페이지 기능들도 모두 여기에서 처리

---

### Member
사용자의 정보를 저장하고 로그인/로그아웃 등의 기능을 책임지는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description            |
| :--- | :--- | :--- |:-----------------------|
| id | long | private | 멤버를 구분하기 위한 고유 ID (PK) |
| loginId | String | private | 로그인을 위한 아이디            |
| password | String | private | 로그인을 위한 비밀번호           |
| nickname | String | private | 사용자의 닉네임               |
| active | boolean | private | 계정 활성화 여부 (true/false) |


#### 2. Operations
| Name | Argument | Returns | Description                         |
| :--- | :--- | :--- |:------------------------------------|
| createMember | String loginId, String password, String nickname | Member | 새로운 Member 객체를 생성해서 반환하는 static 메소드 |
| update | String newNickname | void | 멤버의 닉네임을 변경하는 메소드                   |
| deactivate |  | void | 멤버 계정을 비활성화하는 메소드                   |
---


### UserDetailsImpl
Member 객체를 감싸 Spring Security의 UserDetails 인터페이스 규격에 맞게 변환하는 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| member | Member | private | Spring Security가 사용할 사용자 정보를 담고 있는 Member 객체 |

#### 2. Operations
| Name | Argument      | Returns | Description |
| :--- |:--------------| :--- | :--- |
| UserDetailsImpl | Member member | (constructor) | Member 객체를 주입받는 생성자 |
| getUserId |               | long | Member의 고유 ID(PK)를 반환하는 메소드 |
| getMember |          | Member | 내부의 Member 객체를 반환하는 메소드 |
| getAuthorities |          | Collection<? extends GrantedAuthority> | 사용자의 권N한 목록을 반환 (UserDetails 인터페이스 구현) |
| getPassword |          | String | Member의 password를 반환 (UserDetails 인터페이스 구현) |
| getUsername |           | String | Member의 loginId를 username으로 반환 (UserDetails 인터페이스 구현) |
| isEnabled |           | boolean | Member의 active 상태를 반환 (UserDetails 인터페이스 구현) |
| isAccountNonExpired |           | boolean | 계정 만료 여부를 반환 (UserDetails 인터페이스 구현) |
| isAccountNonLocked |           | boolean | 계정 잠금 여부를 반환 (UserDetails 인터페이스 구현) |
| isCredentialsNonExpired |           | boolean | 비밀번호 만료 여부를 반환 (UserDetails 인터페이스 구현) |

---

### MemberRepository
Member 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
|:-----|:-----|:-----------| :--- |
|      |      |            | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByLoginId | String loginId | Optional<Member> | `loginId`를 기준으로 `Member` 객체를 찾아 `Optional`로 반환 |
| existsByNickname | String nickname | boolean | 해당 `nickname`을 가진 `Member`가 존재하는지 여부를 반환 |

---

### MemberService
회원 가입, 로그인, 탈퇴, 정보 수정 등 Member와 관련된 핵심 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| memberRepository | MemberRepository | private | Member 엔티티의 DB 작업을 위한 리포지토리 |
| passwordEncoder | PasswordEncoder | private | 비밀번호 암호화 및 비교를 위한 인코더 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| addMember | SignupRequest request | MemberResponse | 회원 가입 요청을 처리하고 (비밀번호 암호화, 저장), 결과를 반환 |
| registerMember | LoginRequest request | MemberResponse | 로그인 요청을 받아 ID와 비밀번호 일치 여부를 검증 |
| deleteMember | Long memId | void | 회원 ID를 받아 해당 회원을 삭제 |
| updateNickname | Long memberId, MemberUpdateRequest request | MemberResponse | 닉네임 중복 검사 후, 회원의 닉네임을 변경 |

---

### AuthService
Spring Security의 UserDetailsService를 구현하여, AuthenticationManager를 통한 실제 로그인 인증 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| memberRepository | MemberRepository | private | Member 엔티티를 DB에서 조회하기 위한 리포지토리 |
| authenticationManager | AuthenticationManager | private | Spring Security의 인증을 실제로 수행하는 관리자 객체 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| login | LoginRequest loginRequest | MemberResponse | 로그인 요청을 받아 인증을 수행하고, 성공 시 SecurityContext에 저장 |
| loadUserByUsername | String loginId | UserDetails | (UserDetailsService 구현) loginId로 DB에서 사용자를 찾아 UserDetails로 반환 |

---

### MemberController
클라이언트의 회원 관련 요청을 받아 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| memberService | MemberService | private | 회원 관련 비즈니스 로직을 처리하는 서비스 |
| bakeryReviewRepository | BakeryReviewRepository | private | 빵집 리뷰 데이터에 접근하는 리포지토리 |
| menuReviewRepository | MenuReviewRepository | private | 메뉴 리뷰 데이터에 접근하는 리포지토리 |
| courseRepository | CourseRepository | private | 코스 데이터에 접근하는 리포지토리 |
| courseReviewRepository | CourseReviewRepository | private | 코스 리뷰 데이터에 접근하는 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| deleteMember | UserDetailsImpl userDetails | ResponseEntity<Void> | 인증된 사용자의 회원 탈퇴 요청을 처리 |
| updateNickname | UserDetailsImpl userDetails, MemberUpdateRequest request | ResponseEntity<MemberResponse> | 인증된 사용자의 닉네임 변경 요청을 처리 |
| getMyBakeryReview | UserDetailsImpl userDetails | List<GetMyBakeryReviewResponse> | 인증된 사용자가 작성한 빵집 리뷰 목록을 조회 |
| getMyMenuReview | UserDetailsImpl userDetails | List<GetMyMenuReviewResponse> | 인증된 사용자가 작성한 메뉴 리뷰 목록을 조회 |
| getMyCourse | UserDetailsImpl userDetails | List<GetMyCourseResponse> | 인증된 사용자가 생성한 코스 목록을 조회 |
| getMyCourseReview | UserDetailsImpl userDetails | List<GetMyCourseReviewResponse> | 인증된 사용자가 작성한 코스 리뷰 목록을 조회 |

---

### AuthController
클라이언트의 회원가입 및 로그인 요청을 받아 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| memberService | MemberService | private | 회원 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| signup | SignupRequest request | ResponseEntity<MemberResponse> | 회원 가입 HTTP 요청을 처리 |
| login | LoginRequest request | ResponseEntity<MemberResponse> | 로그인 HTTP 요청을 처리 |

---




## 2) Bakery Class diagram
(여기에 클래스 다이어그램 그림)
(해당 클래스 다이어그램에 대한 설명) 빵집과 빵집의 제보에 대한 것들을 처리하기 위한 클래스들.

---

### Bakery
빵집의 기본 정보를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 빵집을 구분하기 위한 고유 ID (PK) |
| name | String | private | 빵집 이름 |
| address | String | private | 빵집 주소 |
| phone | String | private | 빵집 전화번호 |
| latitude | double | private | 빵집 위치의 위도 (y좌표) |
| longitude | double | private | 빵집 위치의 경도 (x좌표) |
| URL | String | private | 빵집 관련 웹사이트 URL |
| photo1 | String | private | 빵집 사진1 경로 |
| photo2 | String | private | 빵집 사진2 경로 |

#### 2. Operations
| Name | Argument | Returns | Description                                 |
| :--- | :--- | :--- |:--------------------------------------------|
| createBakery | String name, String address, String phone, double latitude, double longitude, String URL, String photo1, String photo2 | Bakery | 모든 정보를 받아 새로운 Bakery 객체를 생성하는 static 생성 메소드 |

---

### BakeryReport
사용자가 남긴 특정 빵집에 대한 제보(빵 소진 등)를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 빵집 제보를 구분하기 위한 고유 ID (PK) |
| text | String | private | 제보 내용 |
| date | LocalDateTime | private | 제보 생성 날짜 (@CreatedDate) |
| member | Member | private | 제보를 작성한 회원 (FK) |
| bakery | Bakery | private | 제보 대상 빵집 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                                               |
| :--- | :--- | :--- |:----------------------------------------------------------|
| createBakeryReport | String text, Member member, Bakery bakery | BakeryReport | 제보 내용, 회원, 빵집 정보를 받아 새 `BakeryReport` 객체를 생성하는 static 메소드 |

---

### BakeryRepository

Bakery 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
|:-----| :--- | :--- | :--- |
|      | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
|:-----| :--- | :--- | :--- |
|      | | | (JpaRepository의 기본 CRUD 메소드들을 상속받아 사용) |

---

### BakeryReportRepository
BakeryReport 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByMemberId | Long memId | List<BakeryReport> | 특정 회원이 작성한 모든 빵집 제보 목록을 조회 |
| findByBakeryIdOrderByCreatedAtDesc | Long bakeryId, Pageable pageable | Page<BakeryReport> | 특정 빵집의 제보 목록을 최신순으로 페이징하여 조회 |
| findByCreatedAtBefore | LocalDateTime createdAt | List<BakeryReport> | 특정 시각 이전에 생성된 모든 빵집 제보 목록을 조회 |

---

### BakeryService
빵집 조회, 검색 등 빵집 관련 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| bakeryRepository | BakeryRepository | private | 빵집 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getBakeryDetail | Long bakeryId, Long memId | BakeryDetailResponse | 특정 빵집의 상세 정보를 조회하여 반환 |
| searchBakeries | SearchBakeryRequest request | List<SearchBakeryResponse> | 검색 조건에 맞는 빵집 목록을 검색하여 반환 |

---

### ReportService

빵집 제보의 생성, 조회, 삭제 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| bakeryReportRepository | BakeryReportRepository | private | 빵집 제보 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getReports | Long bakeryId, Long memId | List<ReportsResponse> | 특정 빵집의 제보 목록을 페이징하여 조회 |
| addReport | Long bakeryId, Long memId, AddReportRequest request | ReportsResponse | 새로운 빵집 제보를 DB에 저장 |
| deleteBakeryReport | Long bakeryReportId, Long memId | void | 사용자가 자신의 빵집 제보를 삭제 |
| cleanupExpiredReports | | void | 게시 기간(일주일)이 만료된 제보들을 일괄 삭제 |

---

### BakeryController
클라이언트의 빵집 조회, 검색 및 빵집 리뷰 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| bakeryService | BakeryService | private | 빵집 관련 비즈니스 로직을 처리하는 서비스 |
| reviewService | ReviewService | private | 리뷰 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getBakeryDetail | Long bakeryId, UserDetailsImpl userDetails | BakeryDetailResponse | 특정 빵집의 상세 정보 조회 요청을 처리 |
| getBakeryReviews | Long bakeryId, UserDetailsImpl userDetails | List<BakeryReviewResponse> | 특정 빵집의 리뷰 목록 조회 요청을 처리 |
| addBakeryReview | Long bakeryId, UserDetailsImpl userDetails, BakeryReviewRequest request | ResponseEntity<BakeryReviewResponse> | 특정 빵집에 대한 리뷰 작성 요청을 처리 |
| updateBakeryReview | Long bakeryReviewId, UserDetailsImpl userDetails, BakeryReviewRequest request | ResponseEntity<BakeryReviewResponse> | 특정 빵집 리뷰의 수정 요청을 처리 |
| bakeryReviewDelete | Long bakeryReviewId, UserDetailsImpl userDetails | ResponseEntity<Void> | 특정 빵집 리뷰의 삭제 요청을 처리 |
| searchBakeries | SearchBakeryRequest request | List<SearchBakeryResponse> | 빵집 검색 요청을 처리 |

---

### ReportController
클라이언트의 빵집 제보 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| reportService | ReportService | private | 빵집 제보 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getReports | Long bakeryId, UserDetailsImpl userDetails | List<ReportsResponse> | 특정 빵집의 제보 목록 조회 요청을 처리 |
| addReport | Long bakeryId, UserDetailsImpl userDetails, AddReportRequest request | ResponseEntity<ReportsResponse> | 특정 빵집에 대한 제보 작성 요청을 처리 |
| deleteBakeryReport | Long bakeryReportId, UserDetailsImpl userDetails | ResponseEntity<Void> | 특정 빵집 제보의 삭제 요청을 처리 |

---




## 3) Course Class diagram
(여기에 클래스 다이어그램 그림)
(해당 클래스 다이어그램에 대한 설명) 빵지순례가 어떤 식으로 코스와 코스 파트로 나누어져 저장되는지

---

### Course
사용자가 생성한 코스의 제목, 총 거리, 총 시간 등의 정보를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 코스를 구분하기 위한 고유 ID (PK) |
| member | Member | private | 코스를 생성한 회원 (FK) |
| title | String | private | 코스 제목 |
| subTitle | String | private | 코스 부제목 |
| allDistance | double | private | 코스의 총 거리 |
| allTravelMinute | long | private | 코스의 총 소요 시간 (분) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| createCourse | Member member, String title, String subTitle, double allDistance, long allTravelMinute | Course | 새로운 Course 객체를 생성하는 정적(static) 메소드 |
| update | String title, String subTitle, double allDistance, long allTravelMinute | void | 코스의 정보를 수정하는 메소드 |

---

### CoursePart
코스를 구성하는 개별 파트의 빵집 정보, 들른 순서, 내용, 사진 등을 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description                      |
| :--- | :--- | :--- |:---------------------------------|
| id | long | private | 코스 파트를 구분하기 위한 고유 ID (PK)        |
| travelOrder | long | private | 해당 코스 파트(빵집)이 코스 내에서 몇 번째 순서인지   |
| body | String | private | 해당 코스 파트에 대한 본문                  |
| photo | String | private | 해당 코스 파트 첨부 사진                   |
| distance | double | private | 이전 코스 파트에서 해당 코스 파트까지의 거리        |
| travelMinute | long | private | 이전 코스 파트에서 해당 코스 파트까지의 소요 시간 (분) |
| course | Course | private | 해당 파트가 속한 코스 (FK)                |
| bakery | Bakery | private | 해당 파트에서 들른 빵집 (FK)               |

#### 2. Operations
| Name | Argument | Returns | Description                            |
| :--- | :--- | :--- |:---------------------------------------|
| createCoursePart | long travelOrder, String body, String photo, double distance, long travelMinute, Course course, Bakery bakery | CoursePart | 새로운 CoursePart 객체를 생성하는 정적(static) 메소드 |

---

### CourseRepository

Course 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
|:-----| :--- | :--- | :--- |
|      | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByMemberId | Long memId | List<Course> | 특정 회원이 생성한 모든 코스 목록을 조회 |
| findByTitleContainingOrKeywordContaining | String title, String keyword, Pageable pageable | Page<Course> | 제목 또는 키워드에 특정 문자열이 포함된 코스를 페이징하여 검색 |

---

### CoursePartRepository
CoursePart 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description                       |
| :--- | :--- | :--- |:----------------------------------|
| deleteAllByCourseId | Long courseId | void | 특정 코스 ID에 속한 모든 CoursePart를 삭제    |
| findByCourseId | Long courseId | List<CoursePart> | 특정 코스 ID에 속한 모든 CoursePart 목록을 조회 |

---

### CourseService
코스의 생성, 수정, 삭제, 조회 등 코스 관련 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| courseRepository | CourseRepository | private | 코스 엔티티의 DB 작업을 위한 리포지토리 |
| coursePartService | CoursePartService | private | 코스 파트 관련 로직을 처리(위임)하는 서비스 |
| coursePartRepository | CoursePartRepository | private | 코스 파트 엔티티의 DB 작업을 위한 리포지토리 |
| favoriteCourseRepository | FavoriteCourseRepository | private | 코스 좋아요 엔티티의 DB 작업을 위한 리포지토리 |
| courseReviewRepository | CourseReviewRepository | private | 코스 리뷰 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description                        |
| :--- | :--- | :--- |:-----------------------------------|
| createCourse | Long memId, CourseRequest request | CourseResponse | 새로운 코스를 생성하고, CoursePart 생성을 위임    |
| updateCourse | Long courseId, Long memId, CourseRequest request | CourseResponse | 특정 코스의 정보를 수정하고, CoursePart 수정을 위임 |
| deleteCourse | Long courseId, Long memId | void | 특정 코스와 관련된 모든 데이터(파트, 리뷰 등)를 함께 삭제 |
| getPopularCourses | | List<GetSimpleCoursesResponse> | '좋아요' 수를 기준으로 인기 코스 목록을 조회         |
| searchCourses | SearchCourseRequest request | List<GetSimpleCoursesResponse> | 키워드를 기반으로 코스 목록을 검색                |
| getCourseDetail | Long courseId, Long memId | CourseDetailResponse | 특정 코스의 상세 정보(파트, 리뷰, 좋아요 수 등)를 조회  |
| getMyCourse | Long memId | List<Course> | 특정 회원이 생성한 '내 코스' 목록을 조회           |

---

### CoursePartService
CoursePart의 생성 및 수정 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| coursePartRepository | CoursePartRepository | private | 코스 파트 엔티티의 DB 작업을 위한 리포지토리 |
| bakeryRepository | BakeryRepository | private | 빵집 엔티티의 유효성 검증을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description                             |
| :--- | :--- | :--- |:----------------------------------------|
| createCourseParts | Long courseId, List<CoursePart> courseParts | List<CoursePart> | 빵집 유효성 검사 후 CoursePart 목록을 일괄 생성(저장)    |
| updateCourseParts | Long courseId, List<CoursePart> courseParts | List<CoursePart> | 특정 코스의 기존 CoursePart를 모두 삭제하고 새 목록으로 교체 |

---

### CourseController
클라이언트의 코스 생성/수정/삭제 및 코스 리뷰 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| reviewService | ReviewService | private | 리뷰 관련 비즈니스 로직을 처리하는 서비스 |
| courseService | CourseService | private | 코스 관련 비즈니스 로직을 처리하는 서비스 |
| coursePartService | CoursePartService | private | 코스 파트 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| addCourseReview | Long courseId, UserDetailsImpl userDetails, CourseReviewRequest request | ResponseEntity<CourseReviewResponse> | 특정 코스에 리뷰를 추가하는 요청을 처리 |
| updateCourseReview | Long courseReviewId, UserDetailsImpl userDetails, CourseReviewRequest request | ResponseEntity<CourseReviewResponse> | 특정 코스 리뷰를 수정하는 요청을 처리 |
| deleteCourseReview | Long courseReviewId, UserDetailsImpl userDetails | ResponseEntity<Void> | 특정 코스 리뷰를 삭제하는 요청을 처리 |
| createCourse | UserDetailsImpl userDetails, CourseRequest request | ResponseEntity<CourseResponse> | 새로운 코스를 생성하는 요청을 처리 |
| updateCourse | Long courseId, UserDetailsImpl userDetails, CourseRequest request | ResponseEntity<CourseResponse> | 특정 코스를 수정하는 요청을 처리 |
| deleteCourse | Long courseId, UserDetailsImpl userDetails | ResponseEntity<Void> | 특정 코스를 삭제하는 요청을 처리 |
| getPopularCourses | | List<GetSimpleCoursesResponse> | 인기 코스 목록을 조회하는 요청을 처리 |
| searchCourses | SearchCourseRequest request | List<GetSimpleCoursesResponse> | 코스를 검색하는 요청을 처리 |
| getCourseDetail | Long courseId, UserDetailsImpl userDetails | CourseDetailResponse | 특정 코스의 상세 정보를 조회하는 요청을 처리 |

---




## 4) Menu Class diagram
(여기에 클래스 다이어그램 그림)
(해당 클래스 다이어그램에 대한 설명)

---

### Menu
빵집에서 판매하는 개별 메뉴(빵)의 이름, 가격, 정보, 사진 등을 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 메뉴를 구분하기 위한 고유 ID (PK) |
| name | String | private | 메뉴 이름 |
| price | int | private | 메뉴 가격 |
| inform | String | private | 메뉴 설명 |
| photo | String | private | 메뉴 사진 |
| bakery | Bakery | private | 이 메뉴가 속한 빵집 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| createMenu | String name, int price, String inform, String photo, Bakery bakery | Menu | 새로운 Menu 객체를 생성하는 정적(static) 메소드 |

---

### Bread
빵의 이름과 카테고리 정보를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 빵을 구분하기 위한 고유 ID (PK) |
| name | String | private | 빵 이름 |
| category | String | private | 빵 카테고리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| createBread | String name, String category | Bread | 새로운 Bread 객체를 생성하는 정적(static) 메소드 |

---

### Classfy
Menu 엔티티와 Bread 엔티티를 연결(매핑)하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 'Classfy'를 구분하기 위한 고유 ID (PK) |
| menu | Menu | private | 연결된 메뉴 (FK) |
| bread | Bread | private | 연결된 빵 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                                       |
| :--- | :--- | :--- |:--------------------------------------------------|
| createClassfy | Menu menu, Bread bread | Classfy | Menu와 Bread를 받아 새 Classfy 객체를 생성하는 정적(static) 메소드 |

---

### MenuRepository
Menu 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description                          |
| :--- | :--- | :--- |:-------------------------------------|
| findByBakeryId | Long bakeryId | List<Menu> | 특정 bakeryId에 해당하는 모든 Menu 엔티티 목록을 조회 |

---

### MenuService
메뉴 목록 조회, 메뉴별 상세 조회 및 평균 별점 계산 등 메뉴 관련 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| menuRepository | MenuRepository | private | 메뉴 엔티티의 DB 작업을 위한 리포지토리 |
| menuReviewRepository | MenuReviewRepository | private | 메뉴 리뷰 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getMenus | Long bakeryId | List<GetMenusResponse> | 특정 빵집의 모든 메뉴 목록을 (평균 별점, 리뷰 수 포함) 조회 |
| getMenuDetail | Long menuId, Long memId | GetMenuDetailResponse | 특정 메뉴의 상세 정보(리뷰 목록, 평균 별점 등)를 조회 |
| getAverageRating | Long menuId | double | (private) 특정 메뉴의 평균 별점을 계산하는 내부 메소드 |

---

### MenuController
클라이언트의 메뉴 조회 및 메뉴 리뷰 관련 HTTP 요청을 처리하는 컨트롤러 클래스.

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| menuService | MenuService | private | 메뉴 관련 비즈니스 로직을 처리하는 서비스 |
| reviewService | ReviewService | private | 리뷰 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getMenus | Long bakeryId | List<GetMenusResponse> | 특정 빵집의 메뉴 목록 조회 요청을 처리 |
| getMenuDetail | Long menuId, UserDetailsImpl userDetails | GetMenuDetailResponse | 특정 메뉴의 상세 정보 조회 요청을 처리 |
| addMenuReview | Long menuId, UserDetailsImpl userDetails, AddMenuReviewRequest request | ResponseEntity<MenuReviewResponse> | 특정 메뉴에 리뷰를 추가하는 요청을 처리 |
| updateMenuReview | Long menuReviewId, UserDetailsImpl userDetails, UpdateMenuReviewRequest request | ResponseEntity<MenuReviewResponse> | 특정 메뉴 리뷰를 수정하는 요청을 처리 |
| deleteMenuReview | Long menuReviewId, UserDetailsImpl userDetails | ResponseEntity<Void> | 특정 메뉴 리뷰를 삭제하는 요청을 처리 |

---




## 5) Favorite Class diagram
(여기에 클래스 다이어그램 그림)
(해당 클래스 다이어그램에 대한 설명) 사용자의 좋아요 기능을 담당하는 클래스들을 모아둠.

---

### FavoriteBakery
Member와 Bakery를 연결하여 빵집 즐겨찾기(좋아요)를 나타내는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 빵집 즐겨찾기를 구분하기 위한 고유 ID (PK) |
| member | Member | private | 즐겨찾기를 등록한 회원 (FK) |
| bakery | Bakery | private | 즐겨찾기 대상 빵집 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                                |
| :--- | :--- | :--- |:-------------------------------------------|
| createFavoriteBakery | Member member, Bakery bakery | FavoriteBakery | 새로운 FavoriteBakery 객체를 생성하는 정적(static) 메소드 |

---

### FavoriteCourse
Member와 Course를 연결하여 코스 즐겨찾기(좋아요)를 나타내는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 코스 즐겨찾기를 구분하기 위한 고유 ID (PK) |
| member | Member | private | 즐겨찾기를 등록한 회원 (FK) |
| course | Course | private | 즐겨찾기 대상 코스 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| createFavoriteCourse | Member member, Course course | FavoriteCourse | 새로운 `FavoriteCourse` 객체를 생성하는 정적(static) 메소드 |

---

### FavoriteBakeryRepository
FavoriteBakery 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
|:-----| :--- | :--- | :--- |
|      | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description            |
| :--- | :--- | :--- |:-----------------------|
| findByMemberId | Long memId | List<FavoriteBakery> | 특정 회원이 즐겨찾기한 빵집 목록을 조회 |
| deleteByMemberIdAndBakeryId | Long memId, Long bakeryId | void | 특정 회원의 빵집 즐겨찾기 항목을 삭제  |

---

### FavoriteCourseRepository
FavoriteCourse 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByMemberId | Long memberId | List<FavoriteCourse> | 특정 회원이 즐겨찾기한 코스 목록을 조회 |
| countByCourseId | Long courseId | long | 특정 코스의 총 즐겨찾기(좋아요) 개수를 반환 |
| existsByMemberIdAndCourseId | Long memberId, Long courseId | boolean | 특정 회원이 특정 코스를 즐겨찾기 했는지 여부를 확인 |
| deleteByMemberIdAndCourseId | Long memId, Long courseId | void | 특정 회원의 코스 즐겨찾기 항목을 삭제 |
| deleteAllByCourseId | Long courseId | void | 특정 코스와 관련된 모든 즐겨찾기 항목을 삭제 |

---

### FavoriteService
즐겨찾기(빵집/코스)의 등록, 삭제, 조회 관련 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| favoriteBakeryRepository | FavoriteBakeryRepository | private | '빵집 즐겨찾기' 엔티티의 DB 작업을 위한 리포지토리 |
| favoriteCourseRepository | FavoriteCourseRepository | private | '코스 즐겨찾기' 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getFavoriteBakeries | Long memberId | List<GetFavoriteBakeriesResponse> | 특정 회원이 즐겨찾기한 빵집 목록을 조회 |
| addFavoriteBakery | Long bakeryId, Long memId | void | 특정 빵집을 즐겨찾기에 추가 (중복 검사 포함) |
| deleteFavoriteBakery | Long bakeryId, Long memId | void | 특정 빵집을 즐겨찾기에서 삭제 |
| findFavoriteCourses | Long memId | List<GetFavoriteCoursesResponse> | 특정 회원이 즐겨찾기한 코스 목록을 조회 |
| addFavoriteCourse | Long courseId, Long memId | void | 특정 코스를 즐겨찾기에 추가 (중복 검사 포함) |
| deleteFavoriteCourse | Long courseId, Long memId | void | 특정 코스를 즐겨찾기에서 삭제 |

---

### FavoriteController
클라이언트의 즐겨찾기(빵집/코스) 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| favoriteService | FavoriteService | private | '즐겨찾기' 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getFavoriteBakeries | UserDetailsImpl userDetails | List<GetFavoriteBakeriesResponse> | 즐겨찾기한 빵집 목록 조회 요청을 처리 |
| addFavoriteBakery | Long bakeryId, UserDetailsImpl userDetails | ResponseEntity<Void> | 빵집 즐겨찾기 추가 요청을 처리 |
| deleteFavoriteBakery | Long bakeryId, UserDetailsImpl userDetails | ResponseEntity<Void> | 빵집 즐겨찾기 삭제 요청을 처리 |
| getFavoriteCourses | UserDetailsImpl userDetails | List<GetFavoriteCoursesResponse> | 즐겨찾기한 코스 목록 조회 요청을 처리 |
| addFavoriteCourse | Long courseId, UserDetailsImpl userDetails | ResponseEntity<Void> | 코스 즐겨찾기 추가 요청을 처리 |
| deleteFavoriteCourse | Long courseId, UserDetailsImpl userDetails | ResponseEntity<Void> | 코스 즐겨찾기 삭제 요청을 처리 |

---





## 6) Review Class diagram
(여기에 클래스 다이어그램 그림)
(해당 클래스 다이어그램에 대한 설명) 리뷰의 경우 각각 빵집 페이지, 메뉴 페이지, 빵지순례 페이지에서 사용되므로 리뷰 컨트롤러를 따로 두지 않고 베이커리 컨트롤러, 메뉴 컨트롤러, 코스 컨트롤러를 사용하였다.

---

### BakeryReview
Member가 Bakery에 대해 작성한 리뷰(평점, 내용, 사진)를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 빵집 리뷰를 구분하기 위한 고유 ID (PK) |
| rating | double | private | 빵집에 대한 평점 |
| text | String | private | 빵집 리뷰 내용 |
| photo | String | private | 빵집 리뷰 관련 사진 |
| date | LocalDateTime | private | 빵집 리뷰 생성 날짜 (@CreatedDate) |
| member | Member | private | 리뷰를 작성한 회원 (FK) |
| bakery | Bakery | private | 리뷰 대상 빵집 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                              |
| :--- | :--- | :--- |:-----------------------------------------|
| createBakeryReview | double rating, String text, String photo, Member member, Bakery bakery | BakeryReview | 새로운 BakeryReview 객체를 생성하는 정적(static) 메소드 |
| update | double rating, String text, String photo | void | 빵집 리뷰의 평점과 내용, 사진을 수정하는 메소드              |

---

### MenuReview
Member가 Menu에 대해 작성한 리뷰(평점, 내용)를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 메뉴 리뷰를 구분하기 위한 고유 ID (PK) |
| rating | double | private | 메뉴에 대한 평점 |
| text | String | private | 메뉴 리뷰 내용 |
| date | LocalDateTime | private | 메뉴 리뷰 생성 날짜 (@CreatedDate) |
| member | Member | private | 리뷰를 작성한 회원 (FK) |
| bakery | Bakery | private | 리뷰 대상 빵집 (FK) |
| menu | Menu | private | 리뷰 대상 메뉴 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                             |
| :--- | :--- | :--- |:----------------------------------------|
| createMenuReview | double rating, String text, Member member, Bakery bakery, Menu menu | MenuReview | 새로운 MenuReview 객체를 생성하는 정적(static) 메소드 |
| update | double rating, String text | void | 메뉴 리뷰의 평점과 내용을 수정하는 메소드                 |

---

### CourseReview
Member가 Course에 대해 작성한 리뷰를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 코스 리뷰를 구분하기 위한 고유 ID (PK) |
| text | String | private | 코스 리뷰 내용 |
| date | LocalDateTime | private | 코스 리뷰 생성 날짜 (@CreatedDate) |
| member | Member | private | 리뷰를 작성한 회원 (FK) |
| bakery | Bakery | private | (연관된) 빵집 (FK) |
| course | Course | private | 리뷰 대상 코스 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                              |
| :--- | :--- | :--- |:-----------------------------------------|
| createCourseReview | String text, Member member, Bakery bakery | CourseReview | 새로운 CourseReview 객체를 생성하는 정적(static) 메소드 |
| update | String text | void | 코스 리뷰의 내용을 수정하는 메소드                      |

---

### BakeryReviewRepository
BakeryReview 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |
Operations

Markdown

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByMemberId | Long memId | List<BakeryReview> | 특정 회원이 작성한 모든 빵집 리뷰 목록을 조회 |

---

# MenuReviewRepository
MenuReview 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByMemberId | Long memId | List<MenuReview> | 특정 회원이 작성한 모든 메뉴 리뷰 목록을 조회 |
| findByMenuId | Long menuId | List<MenuReview> | 특정 메뉴에 대한 모든 리뷰 목록을 조회 |
| countByMenuId | Long menuId | long | 특정 메뉴의 총 리뷰 개수를 반환 |

---

### CourseReviewRepository
CourseReview 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
|:----| :--- | :--- | :--- |
|     | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description                         |
| :--- | :--- | :--- |:------------------------------------|
| findByCourseId | Long courseId | List<CourseReview> | 특정 코스 ID에 속한 모든 CourseReview 목록을 조회 |
| deleteAllByCourseId | Long courseId | void | 특정 코스 ID에 속한 모든 CourseReview를 삭제    |
| findByMemberId | Long memId | List<CourseReview> | 특정 회원이 작성한 모든 코스 리뷰 목록을 조회          |

---

### ReviewService
빵집/메뉴/코스에 대한 리뷰의 생성, 수정, 삭제, 조회 로직을 총괄하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| bakeryRepository | BakeryRepository | private | 빵집 엔티티의 DB 작업을 위한 리포지토리 |
| bakeryReviewRepository | BakeryReviewRepository | private | 빵집 리뷰 엔티티의 DB 작업을 위한 리포지토리 |
| menuReviewRepository | MenuReviewRepository | private | 메뉴 리뷰 엔티티의 DB 작업을 위한 리포지토리 |
| menuRepository | MenuRepository | private | 메뉴 엔티티의 DB 작업을 위한 리포지토리 |
| courseReviewRepository | CourseReviewRepository | private | 코스 리뷰 엔티티의 DB 작업을 위한 리포지토리 |
| courseRepository | CourseRepository | private | 코스 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| addBakeryReview | Long bakeryId, Long memId, BakeryReviewRequest request | BakeryReviewResponse | 특정 빵집에 대한 리뷰를 생성(저장) |
| updateBakeryReview | Long bakeryReviewId, Long memId, BakeryReviewRequest request | BakeryReviewResponse | 특정 빵집 리뷰를 수정 |
| deleteBakeryReview | Long bakeryRevieweId, Long memId | void | 특정 빵집 리뷰를 삭제 |
| getBakeryReviews | Long bakeryId, Long memId | List<BakeryReviewResponse> | 특정 빵집에 대한 모든 리뷰 목록을 조회 |
| getMyBakeryReview | Long bakeryId, Long memId | List<GetMyBakeryReviewResponse> | 특정 회원이 작성한 빵집 리뷰 목록을 조회 |
| addMenuReview | Long menuId, Long memId, AddMenuReviewRequest request | MenuReviewResponse | 특정 메뉴에 대한 리뷰를 생성(저장) |
| updateMenuReview | Long menuReviewId, Long memId, UpdateMenuReviewRequest request | MenuReviewResponse | 특정 메뉴 리뷰를 수정 |
| deleteMenuReview | Long reviewId, Long memId | void | 특정 메뉴 리뷰를 삭제 |
| getMyMenuReview | Long memId | List<GetMyMenuReviewResponse> | 특정 회원이 작성한 메뉴 리뷰 목록을 조회 |
| addCourseReview | Long courseId, Long memId, CourseReviewRequest request | CourseReviewResponse | 특정 코스에 대한 리뷰를 생성(저장) |
| updateCourseReview | Long courseReviewId, Long memId, CourseReviewRequest request | CourseReviewResponse | 특정 코스 리뷰를 수정 |
| deleteCourseReview | Long courseReviewId, Long memId | void | 특정 코스 리뷰를 삭제 |
| getMyCourseReview | Long memId | List<GetMyCourseReviewResponse> | 특정 회원이 작성한 코스 리뷰 목록을 조회 |

---




## 3.2. DTO 
- 컨트롤러 클래스들에서 사용한 DTO를 설명하는 파트이다. 
- dto는 사용처에 따라 6가지(member, bakery, report, menu, course, myPage)로 패키지를 나누어 구성하였다.
- 티도는 요청을 받기 위한 디토인 Request 티도와 응답을 주기 위한 Response로 구분되어 작성되었다.
- 컨트롤러 메소드의 인자값이나 리턴값이 비슷한 경우, 하나의 디토를 여러 메소드에 걸쳐 사용할 수 있도록 구성하였다.

## 1) member

### SignupRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
 | loninId | String | private    | 유저 로그인 id   |
| password | String | private    | 유저 패스워드     |
| nickname | String | private    | 유저 닉네임      |

#### 2. Usage
- 회원가입하기

---

### LoginRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| LoginId | String | private | 유저 로그인 id |
| password | String | private | 패스워드 |

#### 2. Usage
- 로그인하기

---

### MemberUpdateRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| nickname | String | private | 유저 닉네임 |

#### 2. Usage
- 프로필 수정하기 (닉네임 변경하기)

---

### MemberResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| nickname | String | private | 유저 닉네임 |

#### 2. Usage
- 회원가입하기
- 로그인하기
- 프로필 수정하기

---




## 2) bakery

### BakeryDetailResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 빵집 id |
| name | String | private | 빵집 이름 |
| address | String | private | 빵집 주소 |
| phone | String | private | 빵집 연락처 |
| latitude | double | private | 빵집의 위도 (y좌표) |
| longitude | double | private | 빵집의 경도 (x좌표) |
| URL | String | private | 빵집 사이트 |
| photo1 | String | private | 빵집 사진 |
| photo2 | String | private | 빵집 사진 |
| rating | double | private | 빵집 평균 별점 |
| favorite_count | int | private | 빵집 스크랩 수 |
| review_count | int | private | 빵집 리뷰 수 |

#### 2. Usage
- 가게 정보 보기

---

### SearchBakeryRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| text | String | private | 검색어 |

#### 2. Usage
- 가게 검색 및 정렬하기

---

### SearchBakeryResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 빵집 id |
| name | String | private | 빵집 이름 |
| address | String | private | 빵집 주소 |
| photo1 | String | private | 빵집 사진 |
| rating | double | private | 빵집 평균 별점 |
| favorite_count | int | private | 빵집 스크랩 수 |
| review_count | int | private | 빵집 리뷰 수 |

#### 2. Usage
- 가게 검색 및 정렬하기

---

### BakeryReviewRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| rating | double | private | 리뷰 별점 |
| text | String | private | 리뷰 내용 |
| photo | String | private | 리뷰 사진 |

#### 2. Usage
- 가게 리뷰 쓰기
- 가게 리뷰 수정하기

---

### BakeryReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| writer | String | private | 리뷰 작성자 |
| rating | double | private | 리뷰 별점 |
| text | String | private | 리뷰 내용 |
| photo | String | private | 리뷰 사진 |
| date | LocalDateTime | private | 리뷰 쓴 날짜 |
| isMine | boolean | private | 현재 로그인 중인 사용자가 쓴 글인지 아닌지 |

#### 2. Usage
- 가게 리뷰 보기
- 가게 리뷰 쓰기
- 가게 리뷰 수정하기

---




## 3) report

### AddReportRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| text | String | private | 제보글 내용 |

#### 2. Usage
- 제보글 쓰기

---

### ReportsResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakeryreport_id | long | private | 제보 글 ID |
| title | String | private | 제보 글의 제목 |
| name | String | private | 작성자 이름 |
| date | LocalDateTime | private | 제보 글 작성 시간 |
| isMine | boolean | private | 이 글이 내가 작성한 글인지 아닌지 |

#### 2. Usage
- 제보글 보기
- 제보글 쓰기

---




## 4) menu

### GetMenusResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 메뉴 ID |
| name | String | private | 메뉴 이름 |
| rating | double | private | 메뉴 평균 별점 |
| count | long | private | 메뉴 리뷰 수 |

#### 2. Usage
- 가게 메뉴 목록 보기

---

### AddMenuReviewRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakery_id | long | private | 빵집 id |
| rating | double | private | 별점 |
| text | String | private | 리뷰 내용 |

#### 2. Usage
- 메뉴 리뷰 쓰기

---

### UpdateMenuReviewRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| rating | double | private | 별점 |
| text | String | private | 리뷰 내용 |

#### 2. Usage
- 메뉴 리뷰 수정하기

---

### GetMenuDetailResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 메뉴 ID |
| name | String | private | 메뉴 이름 |
| price | int | private | 메뉴 가격 |
| photo | String | private | 메뉴 사진 |
| inform | String | private | 메뉴 설명 |
| rating | double | private | 평균 별점 |
| count | int | private | 리뷰 수 |
| reviews | List<MenuReviewResponse> | private | 모든 메뉴 리뷰 목록 |

#### 2. Usage
- 메뉴 정보 보기 (리뷰 리스트 포함)

---

### MenuReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| reivew_id | long | private | 리뷰 아이디 |
| writer | String | private | 작성자 |
| rating | double | private | 별점 |
| text | String | private | 내용 |
| date | LocalDateTime | private | 작성일자 |
| isMine | boolean | private | 해당 로그인 유저의 리뷰인지 아닌지 |

#### 2. Usage 
- GetMenuDetailResponse 내부
- 메뉴 리뷰 쓰기
- 메뉴 리뷰 수정하기

---




## 5) course
빵지순례와 관련된 정보들을 주고 받기 위한 DTO이다.

### CourseRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| title | String | private | 글 제목 |
| subTitle | String | private | 부제목 |
| parts | List<CoursePartRequest> | private | 해당하는 코스 파트들. 순서대로 저장되어 있어야 함 |

#### 2. Usage
- 빵지순례 글쓰기
- 빵지순례 글 수정하기

---

### CoursePartRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakery_id | long | private | 해당 코스 파트에 해당하는 빵집의 id |
| text | String | private | 파트 내용 |
| photo | String | private | 첨부 사진 |
| distance | double | private | 프론트에서 받지 않는다면 지울 예정 |
| travelMinute | long | private | 프론트에서 받지 않는다면 지울 예정 |

#### 2. Usage
- CourseRequest 내부

---

### CourseResponse

#### 1. Attributes
| Name | Type | Visibility | Description        |
| :--- | :--- |:-----------|:-------------------|
| writer | String | private | 해당 글의 작성자(=본인 닉네임) |
| title | String | private | 글 제목               |
| subTitle | String | private | 부제목                |
| allDistance | double | private | 총길이                |
| allTravelMinute | long | private | 총시간                |
| parts | List<CoursePartResponse> | private | 해당하는 코스 파트들        |

#### 2. Usage
- 빵지순례 글쓰기
- 빵지순례 글 수정하기

---

### CoursePartResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakery_name | long | private | 해당 코스 파트에 해당하는 빵집의 이름 |
| address | String | private | 빵집의 주소 |
| latitude | double | private | 빵집의 위도 (y좌표) |
| longitude | double | private | 빵집의 경도 (x좌표) |
| text | String | private | 파트 내용 |
| photo | String | private | 첨부 사진 |
| distance | double | private | 거리 |
| travelMinute | long | private | 여행 시간 |

#### 2. Usage
- CourseResponse 내부

---

### CourseDetailResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| writer | String | private | 해당 글의 작성자(=본인 닉네임) |
| title | String | private | 글 제목 |
| subTitle | String | private | 부제목 |
| allDistance | double | private | 총길이 |
| allTravelMinute | long | private | 총시간 |
| parts | List<CoursePartResponse> | private | 해당하는 코스 파트들 |
| reviews | List<CourseReviewResponse> | private | 루트에 쓰인 리뷰들 |
| isMine | boolean | private | 이 루트가 나의 글인지 |

#### 2. Usage
- 빵지순례 세부 글 보기

---

### SearchCourseRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| title | String | private | 찾으려는 검색어 |

#### 2. Usage
- 빵지순례 검색하기

---

### GetSimpleCoursesResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| course_id | long | private | 루트 ID |
| title | String | private | 루트 제목 |
| description | String | private | 루트 글 간략한 설명 |
| name | String | private | 작성자 이름 |
| photo | String | private | 빵지 순례 사진 |
| count | long | private | 루트 글 좋아요 수 |
| isMine | boolean | private | 이 글이 내가 작성한 글인지 아닌지 -> 이거 목록에서는 수정 버튼을 못 누르는 거면 빼야 될 듯 |

#### 2. Usage
- 빵지순례 검색하기
- 빵지순례 목록 보기 (좋아요 순)

---

### CourseReviewRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| text | String | private | 리뷰 내용 |

#### 2. Usage
- 빵지순례 리뷰 쓰기
- 빵지순례 리뷰 수정하기

---

### CourseReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| reivew_id | long | private | 리뷰 아이디 |
| writer | String | private | 작성자 |
| text | String | private | 내용 |
| date | LocalDateTime | private | 작성일자 |
| isMine | boolean | private | 해당 로그인 유저의 리뷰인지 아닌지 |

#### 2. Usage
- 빵지순례 리뷰 쓰기
- 빵지순례 리뷰 수정하기

---




## 6) myPage

### GetFavoriteBakeriesResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 빵집 id |
| name | String | private | 빵집 이름 |
| address | String | private | 빵집 주소 |
| phone | String | private | 빵집 연락처 |
| photo1 | String | private | 빵집 사진1 |
| photo2 | String | private | 빵집 사진2 |

#### 2. Usage
- 관심 가게 목록 보기

---

### GetFavoriteCoursesResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 빵지순례(course) id |
| title | String | private | 빵지 순례 제목 |
| subTitle | String | private | 빵지 순례 부제목 |
| allDistance | double | private | 빵지 순례 총 거리 |
| allTravelMinute | long | private | 빵지순례 걸리는 시간 |
| photo1 | String | private | 빵지 순례 사진 |

#### 2. Usage
- 빵지순례 관심 목록 보기

---

### GetMyCourseResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| course_id | long | private | 빵지순례 id |
| title | String | private | 빵지 순례 제목 |
| photo | String | private | 빵지 순례 사진 |

#### 2. Usage
- 내가 작성한 빵지순례 보기

---

### GetMyBakeryReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakery_id | long | private | 빵집 id |
| name | String | private | 빵집 이름 |
| review_id | long | private | 빵집 리뷰 id |
| text | String | private | 빵집 리뷰 내용 |
| rating | double | private | 빵집 리뷰 별점 |
| photo | String | private | 빵집 리뷰 사진 |
| date | LocalDateTime | private | 빵집 리뷰 쓴 시간 |

#### 2. Usage
- 내가 작성한 가게 리뷰 보기

---

### GetMyMenuReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakery_id | long | private | 빵집 id |
| menu_id | long | private | 메뉴 id |
| review_id | long | private | 메뉴 리뷰 id |
| bakery_name | String | private | 빵집 이름 |
| meun_name | String | private | 메뉴 이름 |
| text | String | private | 메뉴 리뷰 내용 |
| rating | double | private | 메뉴 리뷰 별점 |
| date | LocalDateTime | private | 메뉴 리뷰 쓴 시간 |

#### 2. Usage
- 내가 작성한 메뉴 리뷰 보기

---

### GetMyCourseReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| course_id | long | private | 빵지 순례(course) id |
| review_id | long | private | 빵지 순례 리뷰 id |
| course_nickname | String | private | 빵지 순례 작성자 닉네임 |
| title | String | private | 빵지 순례 제목 |
| review_nickname | String | private | 빵지 순례 리뷰 작성자 (=본인) 닉네임 |
| text | String | private | 빵지 순례 리뷰 내용 |
| date | LocalDateTime | private | 빵지 순례 리뷰 쓴 시간 |

#### 2. Usage
- 내가 작성한 빵지순례 리뷰 보기

---







## 3.3. API
