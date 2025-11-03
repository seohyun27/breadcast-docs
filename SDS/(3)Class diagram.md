# 3. Class diagram



## 3.1. Class diagram

- 이곳에 작성된 클래스 다이어그램은 시스템의 핵심 도메인 모델을 표현하는 것을 목적으로 하였다. 
- 클래스 다이어그램은 Member 클래스 다이어그램, bakery 클래스 다이어그램, Course 클래스 다이어그램, Menu 클래스 다이어그램, Favorite(좋아요) 클래스 다이어그램, review 클래스 다이어그램으로 나누어 작성하였다.
- 가독성을 위해 메인 로직에서 벗어난 Getter/Setter 메소드를 의도적으로 생략하였으며, DTO 클래스들을 포함하지 않았다. 
- 계층간 데이터 전송을 위한 DTO와 프론트와의 소통을 위한 API의 경우 3.1. Class diagram 아래 3.2. DTO와 3.3. API에서 따로 서술하였다.


### 1) Member Class diagram
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




### 2) Bakery Class diagram
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




### 3) Course Class diagram
(여기에 클래스 다이어그램 그림)
(해당 클래스 다이어그램에 대한 설명) 

---

### Course












### 4) Menu Class diagram

### 5) Favorite Class diagram

### 6) Review Class diagram
리뷰의 경우 각각 빵집 페이지, 메뉴 페이지, 빵지순례 페이지에서 사용되므로 리뷰 컨트롤러를 따로 두지 않고 베이커리 컨트롤러, 메뉴 컨트롤러, 코스 컨트롤러를 사용하였다.



## 3.2. DTO 



## 3.3. API
