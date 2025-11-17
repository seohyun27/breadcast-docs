# 3. Class diagram



## 3.1. Class diagram

- 이곳에 작성된 클래스 다이어그램은 시스템의 핵심 도메인 모델을 표현하는 것을 목적으로 하였다. 
- 큰 규모의 어플리케이션을 효과적으로 설명하기 위해 아래 6개의 분야별로 클래스 다이어그램을 나눠 작성하였다.
  - member 클래스 다이어그램
  - bakery 클래스 다이어그램
  - course 클래스 다이어그램
  - menu 클래스 다이어그램
  - favorite 클래스 다이어그램
  - review 클래스 다이어그램
- 가독성을 위해 메인 로직에서 벗어난 Getter/Setter 메소드는 의도적으로 생략하였다.
- 엔티티의 생성은 createMember(...)와 같은 정적 메소드를 통해 수행하도록 통일하여 작성하였다.
  - 생성 메소드를 따로 구현함으로서 필수적인 어트리뷰트가 누락되는 것을 막았다.
  - 기본 생성자의 경우 JPA의 내부적 사용으로 인해 구현해두었으나 접근제한자를 protected로 설정하여 외부에서의 호출을 막았다.
  - 따라서 모든 클래스 다이어그램에서 기본 생성자를 생략하고 외부에서 호출이 가능한 생성 메소드만을 표기하였다.
- 모든 Repository 클래스는 Spring Data JPA의 JpaRepository를 상속받는 인터페이스로 구현되었으며 이는 공통 아키텍처 패턴이므로 다이어그램 상에서 상속 관계를 생략하였다.
- 각 클래스 다이어그램에 계층간 데이터 전송을 위한 DTO 클래스들은 포함하지 않았다. 
- DTO와 API의 경우 `3.1. Class diagram` 아래 `3.2. DTO`와 `3.3. API` 파트에서 해당 클래스들의 구성을 따로 설명하였다.


## 1) Member class diagram
![member_class_diagram.png](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/class/1-member.png?raw=true)

- Member class diagram은 사용자 관련 로직들을 처리한다.
- 크게 보안을 담당하는 Auth 관련 클래스와 사용자의 정보를 관리하는 Member 관련 클래스로 나뉜다.

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
| update | String newNickname | void | 멤버의 닉네임을 변경                         |
| deactivate |  | void | 멤버 계정을 비활성화                         |
---


### UserDetailsImpl
Member 객체를 감싸 Spring Security의 UserDetails 인터페이스 규격에 맞게 변환하는 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| member | Member | private | Spring Security가 사용할 사용자 정보를 담고 있는 Member 객체 |

#### 2. Operations
| Name | Argument      | Returns | Description                                           |
| :--- |:--------------| :--- |:------------------------------------------------------|
| UserDetailsImpl | Member member | (constructor) | Member 객체를 주입받는 생성자                                   |
| getUserId |               | long | Member의 고유 ID를 반환하는 메소드                               |
| getMember |          | Member | 내부의 Member 객체를 반환하는 메소드                               |
| getAuthorities |          | `Collection<? extends GrantedAuthority>` | 사용자의 권한 목록을 반환 (UserDetails 인터페이스 구현)                 |
| getPassword |          | String | Member의 password를 반환 (UserDetails 인터페이스 구현)           |
| getUsername |           | String | Member의 loginId를 username으로 반환 (UserDetails 인터페이스 구현) |
| isEnabled |           | boolean | Member의 active 상태를 반환 (UserDetails 인터페이스 구현)          |
| isAccountNonExpired |           | boolean | 계정 만료 여부를 반환 (UserDetails 인터페이스 구현)                   |
| isAccountNonLocked |           | boolean | 계정 잠금 여부를 반환 (UserDetails 인터페이스 구현)                   |
| isCredentialsNonExpired |           | boolean | 비밀번호 만료 여부를 반환 (UserDetails 인터페이스 구현)                 |

---

### MemberRepository
Member 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
|:-----|:-----|:-----------| :--- |
|      |      |            | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns           | Description                              |
| :--- | :--- |:------------------|:-----------------------------------------|
| findByLoginId | String loginId | `Optional<Member>` | loginId를 기준으로 Member 객체를 찾아 Optional로 반환 |
| existsByNickname | String nickname | boolean           | 해당 nickname을 가진 Member가 존재하는지 여부를 반환     |

---

### MemberService
회원 가입, 로그인, 탈퇴, 정보 수정 등 Member와 관련된 핵심 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| memberRepository | MemberRepository | private | Member 엔티티의 DB 작업을 위한 리포지토리 |
| passwordEncoder | PasswordEncoder | private | 비밀번호 암호화 및 비교를 위한 인코더 |

#### 2. Operations
| Name | Argument | Returns | Description                     |
| :--- | :--- | :--- |:--------------------------------|
| addMember | SignupRequest request | MemberResponse | 회원 가입 요청을 처리하고 결과를 반환           |
| registerMember | LoginRequest request | MemberResponse | 로그인 요청을 받아 아이디, 비밀번호의 일치 여부를 검증 |
| deleteMember | Long memId | void | 회원 ID를 받아 해당 회원을 삭제             |
| updateNickname | Long memberId, MemberUpdateRequest request | MemberResponse | 회원의 닉네임을 변경                     |

---

### AuthService
Spring Security의 UserDetailsService를 구현하여, AuthenticationManager를 통한 실제 로그인 인증 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| memberRepository | MemberRepository | private | Member 엔티티를 DB에서 조회하기 위한 리포지토리 |
| authenticationManager | AuthenticationManager | private | Spring Security의 인증을 실제로 수행하는 관리자 객체 |

#### 2. Operations
| Name | Argument | Returns | Description                                                   |
| :--- | :--- | :--- |:--------------------------------------------------------------|
| login | LoginRequest loginRequest | MemberResponse | 로그인 요청을 받아 인증을 수행하고 성공 시 SecurityContext에 저장                  |
| loadUserByUsername | String loginId | UserDetails | loginId로 DB에서 사용자를 찾아 UserDetails로 반환 (UserDetailsService 구현) |

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
| Name | Argument | Returns | Description               |
| :--- | :--- | :--- |:--------------------------|
| deleteMember | UserDetailsImpl userDetails | `ResponseEntity<Void>` | 회원 탈퇴 HTTP 요청을 처리         |
| updateNickname | UserDetailsImpl userDetails, MemberUpdateRequest request | `ResponseEntity<MemberResponse>` | 닉네임 변경 HTTP 요청을 처리        |
| getMyBakeryReview | UserDetailsImpl userDetails | `List<GetMyBakeryReviewResponse>` | 인증된 사용자가 작성한 빵집 리뷰 목록을 조회 |
| getMyMenuReview | UserDetailsImpl userDetails | `List<GetMyMenuReviewResponse>` | 사용자가 작성한 메뉴 리뷰 목록을 조회     |
| getMyCourse | UserDetailsImpl userDetails | `List<GetMyCourseResponse>` | 사용자가 생성한 코스 목록을 조회        |
| getMyCourseReview | UserDetailsImpl userDetails | `List<GetMyCourseReviewResponse>` | 사용자가 작성한 코스 리뷰 목록을 조회     |

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
| signup | SignupRequest request | `ResponseEntity<MemberResponse>` | 회원 가입 HTTP 요청을 처리 |
| login | LoginRequest request | `ResponseEntity<MemberResponse>` | 로그인 HTTP 요청을 처리 |

---


<br>


## 2) Bakery class diagram
![bakery_class_diagram.png](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/class/2-bakery.png?raw=true)

- Bakery class diagram는 빵집에 관련된 로직과 제보에 대한 로직들을 처리한다.
- 크게 빵집을 담당하는 Bakery 관련 클래스와 제보를 담당하는 Report 관련 클래스로 나뉜다.
- 사용자는 Bakery 관련 클래스를 통해 빵집에 대한 정보들을 확인하거나 원하는 빵집을 검색할 수 있다.
- Report 관련 클래스를 통해서는 원하는 빵집에 제보를 작성하거나 썼던 제보를 삭제하거나 다른 사람의 제보를 확인할 수 있다.

---

### Bakery
빵집의 기본 정보를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description            |
| :--- | :--- | :--- |:-----------------------|
| id | long | private | 빵집을 구분하기 위한 고유 ID (PK) |
| name | String | private | 빵집 이름                  |
| address | String | private | 빵집 주소                  |
| phone | String | private | 빵집 전화번호                |
| latitude | double | private | 빵집 위치의 위도 (y좌표)        |
| longitude | double | private | 빵집 위치의 경도 (x좌표)        |
| URL | String | private | 빵집과 관련된 웹사이트 URL       |
| photo1 | String | private | 빵집 사진1 경로              |
| photo2 | String | private | 빵집 사진2 경로              |

#### 2. Operations
| Name | Argument | Returns | Description                                 |
| :--- | :--- | :--- |:--------------------------------------------|
| createBakery | String name, String address, String phone, double latitude, double longitude, String URL, String photo1, String photo2 | Bakery | 모든 정보를 받아 새로운 Bakery 객체를 생성하는 static 생성 메소드 |

---

### BakeryReport
사용자가 남긴 특정 빵집에 대한 제보를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description               |
| :--- | :--- | :--- |:--------------------------|
| id | long | private | 빵집 제보를 구분하기 위한 고유 ID (PK) |
| text | String | private | 제보 내용                     |
| date | LocalDateTime | private | 제보 생성 날짜                  |
| member | Member | private | 제보를 작성한 회원 (FK)           |
| bakery | Bakery | private | 제보 대상 빵집 (FK)             |

#### 2. Operations
| Name | Argument | Returns | Description                                             |
| :--- | :--- | :--- |:--------------------------------------------------------|
| createBakeryReport | String text, Member member, Bakery bakery | BakeryReport | 제보 내용, 회원, 빵집 정보를 받아 새 BakeryReport 객체를 생성하는 static 메소드 |

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
| Name | Argument | Returns              | Description |
| :--- | :--- |:---------------------| :--- |
| findByMemberId | Long memId | `List<BakeryReport>` | 특정 회원이 작성한 모든 빵집 제보 목록을 조회 |
| findByBakeryIdOrderByCreatedAtDesc | Long bakeryId | `List<BakeryReport>` | 특정 빵집의 제보 목록을 최신순으로 조회 |
| findByCreatedAtBefore | LocalDateTime createdAt | `List<BakeryReport>` | 특정 시각 이전에 생성된 모든 빵집 제보 목록을 조회 |

---

### BakeryService
빵집 조회, 검색 등 빵집 관련 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| bakeryRepository | BakeryRepository | private | 빵집 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument                    | Returns | Description                  |
| :--- |:----------------------------| :--- |:-----------------------------|
| getBakeryDetail | Long bakeryId, Long memId   | BakeryDetailResponse | 특정 빵집의 상세 정보를 조회하여 반환        |
| searchBakeries | String keyword, String sort | `List<SearchBakeryResponse>` | 검색 조건(제목)에 맞는 빵집 목록을 검색하여 반환 |

---

### ReportService

빵집 제보의 생성, 조회, 삭제 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| bakeryReportRepository | BakeryReportRepository | private | 빵집 제보 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description                |
| :--- | :--- | :--- |:---------------------------|
| getReports | Long bakeryId, Long memId | `List<ReportsResponse>` | 특정 빵집의 제보 목록을 조회           |
| addReport | Long bakeryId, Long memId, AddReportRequest request | ReportsResponse | 새로운 빵집 제보를 DB에 저장          |
| deleteBakeryReport | Long bakeryReportId, Long memId | void | 사용자가 자신이 작성했던 빵집 제보를 삭제    |
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
| getBakeryReviews | Long bakeryId, UserDetailsImpl userDetails | `List<BakeryReviewResponse>` | 특정 빵집의 리뷰 목록 조회 요청을 처리 |
| addBakeryReview | Long bakeryId, UserDetailsImpl userDetails, BakeryReviewRequest request | `ResponseEntity<BakeryReviewResponse>` | 특정 빵집에 대한 리뷰 작성 요청을 처리 |
| updateBakeryReview | Long bakeryReviewId, UserDetailsImpl userDetails, BakeryReviewRequest request | `ResponseEntity<BakeryReviewResponse>` | 특정 빵집 리뷰의 수정 요청을 처리 |
| bakeryReviewDelete | Long bakeryReviewId, UserDetailsImpl userDetails | `ResponseEntity<Void>` | 특정 빵집 리뷰의 삭제 요청을 처리 |
| searchBakeries | String keyword, String sort | `List<SearchBakeryResponse>` | 빵집 검색 요청을 처리 |

---

### ReportController
클라이언트의 빵집 제보 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| reportService | ReportService | private | 빵집 제보 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns                         | Description |
| :--- | :--- |:--------------------------------| :--- |
| getReports | Long bakeryId, UserDetailsImpl userDetails | `List<ReportsResponse>`         | 특정 빵집의 제보 목록 조회 요청을 처리 |
| addReport | Long bakeryId, UserDetailsImpl userDetails, AddReportRequest request | `ResponseEntity<ReportsResponse>` | 특정 빵집에 대한 제보 작성 요청을 처리 |
| deleteBakeryReport | Long bakeryReportId, UserDetailsImpl userDetails | `ResponseEntity<Void>`            | 특정 빵집 제보의 삭제 요청을 처리 |

---


<br>


## 3) Menu class diagram
![menu_class_diagram.png](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/class/3-menu.png?raw=true)

- Menu class diagram은 메뉴에 관련된 로직들을 처리한다.
- 해당 클래스 다이어그램의 핵심이 되는 Menu 엔티티와 연관된 엔티티로는 빵의 종류를 나타내는 Bread 엔티티와 메뉴와 빵 사이의 연결하는 Classfy 엔티티가 존재한다.
- 사용자는 각 bakery의 메뉴 목록을 보거나 메뉴 하나하나의 상세한 정보를 확인할 수 있다.

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
| Name | Argument | Returns | Description                  |
| :--- | :--- | :--- |:-----------------------------|
| createMenu | String name, int price, String inform, String photo, Bakery bakery | Menu | 새로운 Menu 객체를 생성하는 static 메소드 |

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
| Name | Argument | Returns | Description                   |
| :--- | :--- | :--- |:------------------------------|
| createBread | String name, String category | Bread | 새로운 Bread 객체를 생성하는 static 메소드 |

---

### Classfy
Menu 엔티티와 Bread 엔티티를 연결(매핑)하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description                 |
| :--- | :--- | :--- |:----------------------------|
| id | long | private | Classfy를 구분하기 위한 고유 ID (PK) |
| menu | Menu | private | 연결된 메뉴 (FK)                 |
| bread | Bread | private | 연결된 빵 (FK)                  |

#### 2. Operations
| Name | Argument | Returns | Description                                   |
| :--- | :--- | :--- |:----------------------------------------------|
| createClassfy | Menu menu, Bread bread | Classfy | Menu와 Bread를 받아 새 Classfy 객체를 생성하는 static 메소드 |

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
| findByBakeryId | Long bakeryId | `List<Menu>` | 특정 bakeryId에 해당하는 모든 Menu 엔티티 목록을 조회 |

---

### MenuService
메뉴 목록 조회, 메뉴별 상세 조회 및 평균 별점 계산 등 메뉴 관련 비즈니스 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| menuRepository | MenuRepository | private | 메뉴 엔티티의 DB 작업을 위한 리포지토리 |
| menuReviewRepository | MenuReviewRepository | private | 메뉴 리뷰 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description                          |
| :--- | :--- | :--- |:-------------------------------------|
| getMenus | Long bakeryId | `List<GetMenusResponse>` | 특정 빵집의 모든 메뉴 목록을 (평균 별점, 리뷰 수 포함) 조회 |
| getMenuDetail | Long menuId, Long memId | GetMenuDetailResponse | 특정 메뉴의 상세 정보(리뷰 목록, 평균 별점 포함)를 조회    |
| getAverageRating | Long menuId | double | (private) 특정 메뉴의 평균 별점을 계산하는 내부 메소드  |

---

### MenuController
클라이언트의 메뉴 조회 및 메뉴 리뷰 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| menuService | MenuService | private | 메뉴 관련 비즈니스 로직을 처리하는 서비스 |
| reviewService | ReviewService | private | 리뷰 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getMenus | Long bakeryId | `List<GetMenusResponse>` | 특정 빵집의 메뉴 목록 조회 요청을 처리 |
| getMenuDetail | Long menuId, UserDetailsImpl userDetails | GetMenuDetailResponse | 특정 메뉴의 상세 정보 조회 요청을 처리 |
| addMenuReview | Long menuId, UserDetailsImpl userDetails, AddMenuReviewRequest request | `ResponseEntity<MenuReviewResponse>` | 특정 메뉴에 리뷰를 추가하는 요청을 처리 |
| updateMenuReview | Long menuReviewId, UserDetailsImpl userDetails, UpdateMenuReviewRequest request | `ResponseEntity<MenuReviewResponse>` | 특정 메뉴 리뷰를 수정하는 요청을 처리 |
| deleteMenuReview | Long menuReviewId, UserDetailsImpl userDetails | `ResponseEntity<Void>` | 특정 메뉴 리뷰를 삭제하는 요청을 처리 |

---


<br>


## 4) Course class diagram
![course_class_diagram.png](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/class/4-course.png?raw=true)

- Course class diagram은 빵지순례 글에 관련된 로직들을 처리한다.
- 빵지순례 글은 Course 엔티티와 CoursePart 엔티티로 나누어 저장된다.
- Course 엔티티는 사용자가 생성한 빵지순례 글의 메타 데이터를 저장하는 클래스이다. 글의 제목, 부제목, 빵지순례 코스의 총거리, 총소요 시간에 대한 정보를 포함한다.
- CoursePart 엔티티는 빵지순례 글의 실질적인 내용을 포함한다.
- 사용자는 빵지순례 글을 작성할 때 원하는 빵집을 선택한 뒤 해당 빵집에 대한 리뷰를 남길 수 있다.
- CoursePart는 사용자가 빵지순례 글에 빵집을 추가할 때마다 새로 생성되며 최종적으로는 Course 엔티티와 함께 하나의 글로 표현되어 사용자에게 제공된다.
- 빵지순례 글의 생성, 수정, 삭제, 조회를 포함해 좋아요순으로 정렬된 목록 보기 기능을 제공한다.

---

### Course
사용자가 생성한 빵지순례글의 제목, 총거리, 총시간 등의 정보를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description            |
| :--- | :--- | :--- |:-----------------------|
| id | long | private | 코스를 구분하기 위한 고유 ID (PK) |
| member | Member | private | 코스를 생성한 회원 (FK)        |
| title | String | private | 코스 제목                  |
| subTitle | String | private | 코스 부제목                 |
| allDistance | double | private | 코스의 총거리                |
| allTravelMinute | long | private | 코스의 총소요 시간 (분)         |

#### 2. Operations
| Name | Argument | Returns | Description                    |
| :--- | :--- | :--- |:-------------------------------|
| createCourse | Member member, String title, String subTitle, double allDistance, long allTravelMinute | Course | 새로운 Course 객체를 생성하는 static 메소드 |
| update | String title, String subTitle, double allDistance, long allTravelMinute | void | 코스의 정보를 수정하는 메소드               |

---

### CoursePart
빵지순례글(Course)을 구성하는 개별 파트의 빵집 정보, 들른 순서, 내용, 사진 등을 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description                      |
| :--- | :--- | :--- |:---------------------------------|
| id | long | private | 코스 파트를 구분하기 위한 고유 ID (PK)        |
| travelOrder | long | private | 해당 코스 파트(빵집)이 코스 내에서 몇 번째 순서인지   |
| body | String | private | 해당 코스 파트에 대한 본문                  |
| photo | String | private | 해당 코스 파트 첨부 사진                   |
| distance | double | private | 이전 코스 파트에서 해당 코스 파트까지의 거리        |
| travelMinute | long | private | 이전 코스 파트에서 해당 코스 파트까지의 소요 시간 |
| course | Course | private | 해당 파트가 속한 코스 (FK)                |
| bakery | Bakery | private | 해당 파트에서 들른 빵집 (FK)               |

#### 2. Operations
| Name | Argument | Returns | Description                           |
| :--- | :--- | :--- |:--------------------------------------|
| createCoursePart | long travelOrder, String body, String photo, double distance, long travelMinute, Course course, Bakery bakery | CoursePart | 새로운 CoursePart 객체를 생성하는 static 메소드    |

---

### CourseRepository

Course 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
|:-----| :--- | :--- | :--- |
|      | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns        | Description |
| :--- | :--- |:---------------| :--- |
| findByMemberId | Long memId | `List<Course>` | 특정 회원이 생성한 모든 코스 목록을 조회 |
| findByTitleContainingOrKeywordContaining | String title, String keyword | `List<Course>`   | 제목 또는 키워드에 특정 문자열이 포함된 코스를 검색 |

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
| findByCourseId | Long courseId | `List<CoursePart>` | 특정 코스 ID에 속한 모든 CoursePart 목록을 조회 |

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
| Name | Argument | Returns | Description                                          |
| :--- | :--- | :--- |:-----------------------------------------------------|
| createCourse | Long memId, CourseRequest request | CourseResponse | 새로운 코스를 생성하고 CoursePartService로 CoursePart 생성을 위임    |
| updateCourse | Long courseId, Long memId, CourseRequest request | CourseResponse | 특정 코스의 정보를 수정하고 CoursePartService로 CoursePart 수정을 위임 |
| deleteCourse | Long courseId, Long memId | void | 특정 코스와 관련된 모든 데이터(코스 파트, 코스 리뷰)를 함께 삭제               |
| getPopularCourses | | `List<GetSimpleCoursesResponse>` | 좋아요 수를 기준으로 인기 코스 목록을 조회                             |
| searchCourses | String keyword | `List<GetSimpleCoursesResponse>` | 키워드를 기반으로 코스 목록을 검색                                  |
| getCourseDetail | Long courseId, Long memId | CourseDetailResponse | 특정 코스의 상세 정보를 조회                                     |
| getMyCourse | Long memId | `List<Course>` | 특정 회원이 생성한 코스 목록을 조회                                 |

---

### CoursePartService
CoursePart의 생성 및 수정 로직을 처리하는 서비스 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| coursePartRepository | CoursePartRepository | private | 코스 파트 엔티티의 DB 작업을 위한 리포지토리 |
| bakeryRepository | BakeryRepository | private | 빵집 엔티티의 유효성 검증을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns                    | Description                             |
| :--- | :--- |:---------------------------|:----------------------------------------|
| createCourseParts | Long courseId, `List<CoursePartRequest>` courseParts | `List<CoursePartResponse>` | 빵집 유효성 검사 후 CoursePart 목록을 일괄 생성(저장)    |
| updateCourseParts | Long courseId, `List<CoursePartRequest>` courseParts | `List<CoursePartResponse>` | 특정 코스의 기존 CoursePart를 모두 삭제하고 새 목록으로 교체 |

---

### CourseController
클라이언트의 코스 생성/수정/삭제 및 코스 리뷰 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| reviewService | ReviewService | private | 리뷰 관련 비즈니스 로직을 처리하는 서비스 |
| courseService | CourseService | private | 코스 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| addCourseReview | Long courseId, UserDetailsImpl userDetails, CourseReviewRequest request | `ResponseEntity<CourseReviewResponse>` | 특정 코스에 리뷰를 추가하는 요청을 처리 |
| updateCourseReview | Long courseReviewId, UserDetailsImpl userDetails, CourseReviewRequest request | `ResponseEntity<CourseReviewResponse>` | 특정 코스 리뷰를 수정하는 요청을 처리 |
| deleteCourseReview | Long courseReviewId, UserDetailsImpl userDetails | `ResponseEntity<Void>` | 특정 코스 리뷰를 삭제하는 요청을 처리 |
| createCourse | UserDetailsImpl userDetails, CourseRequest request | `ResponseEntity<CourseResponse>` | 새로운 코스를 생성하는 요청을 처리 |
| updateCourse | Long courseId, UserDetailsImpl userDetails, CourseRequest request | `ResponseEntity<CourseResponse>` | 특정 코스를 수정하는 요청을 처리 |
| deleteCourse | Long courseId, UserDetailsImpl userDetails | `ResponseEntity<Void>` | 특정 코스를 삭제하는 요청을 처리 |
| getPopularCourses | | `List<GetSimpleCoursesResponse>` | 인기 코스 목록을 조회하는 요청을 처리 |
| searchCourses | String keyword | `List<GetSimpleCoursesResponse>` | 코스를 검색하는 요청을 처리 |
| getCourseDetail | Long courseId, UserDetailsImpl userDetails | CourseDetailResponse | 특정 코스의 상세 정보를 조회하는 요청을 처리 |

---


<br>


## 5) Favorite class diagram
![favorite_class_diagram.png](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/class/5-favorite.png?raw=true)

- Favorite class diagram은 사용자의 좋아요 기능과 관련된 로직들을 처리한다.
- 해당 기능은 반드시 사용자 로그인이 되어 있는 상태에서만 이용 가능해야 한다.
- 사용자가 좋아요를 누를 수 있는 엔티티는 빵집(Bakery)과 빵지순례글(Course)이 있다.
- 사용자는 두 엔티티에 좋아요를 누르거나 이미 눌렀던 좋아요를 취소할 수 있다.
- 또한 현재 좋아요를 누른 상태에 있는 빵집이나 빵지순례글 목록을 제공받을 수도 있다.

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
| Name | Argument | Returns | Description                            |
| :--- | :--- | :--- |:---------------------------------------|
| createFavoriteBakery | Member member, Bakery bakery | FavoriteBakery | 새로운 FavoriteBakery 객체를 생성하는 static 메소드 |

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
| Name | Argument | Returns | Description                            |
| :--- | :--- | :--- |:---------------------------------------|
| createFavoriteCourse | Member member, Course course | FavoriteCourse | 새로운 FavoriteCourse 객체를 생성하는 static 메소드 |

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
| findByMemberId | Long memId | `List<FavoriteBakery>` | 특정 회원이 즐겨찾기한 빵집 목록을 조회 |
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
| findByMemberId | Long memberId | `List<FavoriteCourse>` | 특정 회원이 즐겨찾기한 코스 목록을 조회 |
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
| favoriteBakeryRepository | FavoriteBakeryRepository | private | FavoriteBakery 엔티티의 DB 작업을 위한 리포지토리 |
| favoriteCourseRepository | FavoriteCourseRepository | private | FavoriteCourse 엔티티의 DB 작업을 위한 리포지토리 |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| getFavoriteBakeries | Long memberId | `List<GetFavoriteBakeriesResponse>` | 특정 회원이 즐겨찾기한 빵집 목록을 조회 |
| addFavoriteBakery | Long bakeryId, Long memId | void | 특정 빵집을 즐겨찾기에 추가 |
| deleteFavoriteBakery | Long bakeryId, Long memId | void | 특정 빵집을 즐겨찾기에서 삭제 |
| findFavoriteCourses | Long memId | `List<GetFavoriteCoursesResponse>` | 특정 회원이 즐겨찾기한 코스 목록을 조회 |
| addFavoriteCourse | Long courseId, Long memId | void | 특정 코스를 즐겨찾기에 추가 |
| deleteFavoriteCourse | Long courseId, Long memId | void | 특정 코스를 즐겨찾기에서 삭제 |

---

### FavoriteController
클라이언트의 즐겨찾기(빵집/코스) 관련 HTTP 요청을 처리하는 컨트롤러 클래스

#### 1. Attributes
| Name | Type | Visibility | Description               |
| :--- | :--- | :--- |:--------------------------|
| favoriteService | FavoriteService | private | Favorite 관련 비즈니스 로직을 처리하는 서비스 |

#### 2. Operations
| Name | Argument | Returns                           | Description |
| :--- | :--- |:----------------------------------| :--- |
| getFavoriteBakeries | UserDetailsImpl userDetails | `List<GetFavoriteBakeriesResponse>` | 즐겨찾기한 빵집 목록 조회 요청을 처리 |
| addFavoriteBakery | Long bakeryId, UserDetailsImpl userDetails | `ResponseEntity<Void>`              | 빵집 즐겨찾기 추가 요청을 처리 |
| deleteFavoriteBakery | Long bakeryId, UserDetailsImpl userDetails | `ResponseEntity<Void>`              | 빵집 즐겨찾기 삭제 요청을 처리 |
| getFavoriteCourses | UserDetailsImpl userDetails | `List<GetFavoriteCoursesResponse>`  | 즐겨찾기한 코스 목록 조회 요청을 처리 |
| addFavoriteCourse | Long courseId, UserDetailsImpl userDetails | `ResponseEntity<Void>`              | 코스 즐겨찾기 추가 요청을 처리 |
| deleteFavoriteCourse | Long courseId, UserDetailsImpl userDetails | `ResponseEntity<Void>`              | 코스 즐겨찾기 삭제 요청을 처리 |

---


<br>


## 6) Review class diagram
![review_class_diagram.png](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/class/6-review.png?raw=true)

- Review class diagram는 리뷰와 관련된 로직들을 처리한다.
- 해당 기능은 반드시 사용자 로그인이 되어 있는 상태에서만 이용 가능해야 한다.
- 사용자가 리뷰를 달 수 있는 엔티티는 빵집(Bakery)과 메뉴(Menu), 빵지순례글(Course)이 있다.
- 사용자는 각 엔티티에 리뷰를 작성하거나 수정하거나 삭제할 수 있다.
- 또한 자신이 작성한 리뷰를 타입별(Bakery/Menu/Course)로 모아보는 기능도 제공한다.
- 리뷰의 경우 상위 리소스(Bakery/Menu/Course)에 종속되어 있으므로 상위 리소스와 API 경로를 통일하기 위해 컨트롤러를 따로 구성하지 않았다.
- 따라서 리뷰와 연관된 컨트롤러 메소드들은 앞서 서술된 각 엔티티별 컨트롤러(BakeryController, MenuController, CourseController)에 포함되어 있다.

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
| date | LocalDateTime | private | 빵집 리뷰 생성 날짜 |
| member | Member | private | 리뷰를 작성한 회원 (FK) |
| bakery | Bakery | private | 리뷰 대상 빵집 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                          |
| :--- | :--- | :--- |:-------------------------------------|
| createBakeryReview | double rating, String text, String photo, Member member, Bakery bakery | BakeryReview | 새로운 BakeryReview 객체를 생성하는 static 메소드 |
| update | double rating, String text, String photo | void | 빵집 리뷰의 평점과 내용, 사진을 수정하는 메소드          |

---

### MenuReview
Member가 Menu에 대해 작성한 리뷰(평점, 내용)를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 메뉴 리뷰를 구분하기 위한 고유 ID (PK) |
| rating | double | private | 메뉴에 대한 평점 |
| text | String | private | 메뉴 리뷰 내용 |
| date | LocalDateTime | private | 메뉴 리뷰 생성 날짜 |
| member | Member | private | 리뷰를 작성한 회원 (FK) |
| bakery | Bakery | private | 리뷰 대상 빵집 (FK) |
| menu | Menu | private | 리뷰 대상 메뉴 (FK) |

#### 2. Operations
| Name | Argument | Returns | Description                             |
| :--- | :--- | :--- |:----------------------------------------|
| createMenuReview | double rating, String text, Member member, Bakery bakery, Menu menu | MenuReview | 새로운 MenuReview 객체를 생성하는 static 메소드 |
| update | double rating, String text | void | 메뉴 리뷰의 평점과 내용을 수정하는 메소드                 |

---

### CourseReview
Member가 Course에 대해 작성한 리뷰를 저장하는 엔티티 클래스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| id | long | private | 코스 리뷰를 구분하기 위한 고유 ID (PK) |
| text | String | private | 코스 리뷰 내용 |
| date | LocalDateTime | private | 코스 리뷰 생성 날짜 |
| member | Member | private | 리뷰를 작성한 회원 (FK) |
| course | Course | private | 리뷰 대상 코스 (FK) |

#### 2. Operations
| Name | Argument                                  | Returns | Description                             |
| :--- |:------------------------------------------| :--- |:----------------------------------------|
| createCourseReview | String text, Member member, Course course | CourseReview | 새로운 CourseReview 객체를 생성하는 static 메소드    |
| update | String text                               | void | 코스 리뷰의 내용을 수정하는 메소드                     |

---

### BakeryReviewRepository
BakeryReview 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByMemberId | Long memId | `List<BakeryReview>` | 특정 회원이 작성한 모든 빵집 리뷰 목록을 조회 |

---

### MenuReviewRepository
MenuReview 엔티티의 DB 접근을 담당하는 Spring Data JPA 리포지토리 인터페이스

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
|  | | | (Interface이므로 상속받은 JpaRepository 외에 별도 정의된 속성 없음) |

#### 2. Operations
| Name | Argument | Returns | Description |
| :--- | :--- | :--- | :--- |
| findByMemberId | Long memId | `List<MenuReview>` | 특정 회원이 작성한 모든 메뉴 리뷰 목록을 조회 |
| findByMenuId | Long menuId | `List<MenuReview>` | 특정 메뉴에 대한 모든 리뷰 목록을 조회 |
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
| findByCourseId | Long courseId | `List<CourseReview>` | 특정 코스 ID에 속한 모든 CourseReview 목록을 조회 |
| deleteAllByCourseId | Long courseId | void | 특정 코스 ID에 속한 모든 CourseReview를 삭제    |
| findByMemberId | Long memId | `List<CourseReview>` | 특정 회원이 작성한 모든 코스 리뷰 목록을 조회          |

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
| getBakeryReviews | Long bakeryId, Long memId | `List<BakeryReviewResponse>` | 특정 빵집에 대한 모든 리뷰 목록을 조회 |
| getMyBakeryReview | Long bakeryId, Long memId | `List<GetMyBakeryReviewResponse>` | 특정 회원이 작성한 빵집 리뷰 목록을 조회 |
| addMenuReview | Long menuId, Long memId, AddMenuReviewRequest request | MenuReviewResponse | 특정 메뉴에 대한 리뷰를 생성(저장) |
| updateMenuReview | Long menuReviewId, Long memId, UpdateMenuReviewRequest request | MenuReviewResponse | 특정 메뉴 리뷰를 수정 |
| deleteMenuReview | Long reviewId, Long memId | void | 특정 메뉴 리뷰를 삭제 |
| getMyMenuReview | Long memId | `List<GetMyMenuReviewResponse>` | 특정 회원이 작성한 메뉴 리뷰 목록을 조회 |
| addCourseReview | Long courseId, Long memId, CourseReviewRequest request | CourseReviewResponse | 특정 코스에 대한 리뷰를 생성(저장) |
| updateCourseReview | Long courseReviewId, Long memId, CourseReviewRequest request | CourseReviewResponse | 특정 코스 리뷰를 수정 |
| deleteCourseReview | Long courseReviewId, Long memId | void | 특정 코스 리뷰를 삭제 |
| getMyCourseReview | Long memId | `List<GetMyCourseReviewResponse>` | 특정 회원이 작성한 코스 리뷰 목록을 조회 |

---

<br>
<br>

## 3.2. DTO 
- 컨트롤러 클래스들에서 사용한 DTO를 설명하는 파트이다. 
- dto는 사용처에 따라 아래 6가지로 패키지를 나누어 구성하였다.

  | Package | Description   |
  |:--------|:--------------|
  | member  | 회원 관련 디토      |
  | bakery  | 빵집 관련 디토      |
  | report  | 제보 관련 디토      |
  | menu    | 메뉴 관련 디토      |
  | course  | 코스 관련 디토      |
  | myPage  | 마이페이지 관련 디토   |

- 티도는 요청을 받기 위한 디토인 Request 티도와 응답을 주기 위한 Response로 구분되어 작성되었다.
- 컨트롤러 메소드들의 인자값이나 리턴값이 비슷한 경우, 하나의 디토를 여러 메소드에 걸쳐 사용할 수 있도록 구성하였다.

## 1) member

### SignupRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
 | loninId | String | private    | 유저 로그인 ID   |
| password | String | private    | 유저 패스워드     |
| nickname | String | private    | 유저 닉네임      |

#### 2. Usage
- 회원가입하기

---

### LoginRequest

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| LoginId | String | private | 유저 로그인 ID |
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


<br>


## 2) bakery

### BakeryDetailResponse

#### 1. Attributes
| Name | Type | Visibility | Description  |
| :--- | :--- |:-----------|:-------------|
| id | long | private | 빵집 ID        |
| name | String | private | 빵집 이름        |
| address | String | private | 빵집 주소        |
| phone | String | private | 빵집 연락처       |
| latitude | double | private | 빵집의 위도 (y좌표) |
| longitude | double | private | 빵집의 경도 (x좌표) |
| URL | String | private | 빵집 사이트       |
| photo1 | String | private | 빵집 사진        |
| photo2 | String | private | 빵집 사진        |
| rating | double | private | 빵집 평균 별점     |
| favorite_count | int | private | 빵집 좋아요 수     |
| review_count | int | private | 빵집 리뷰 수      |

#### 2. Usage
- 가게 정보 보기

---

### SearchBakeryResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 빵집 ID       |
| name | String | private | 빵집 이름       |
| address | String | private | 빵집 주소       |
| photo1 | String | private | 빵집 사진       |
| rating | double | private | 빵집 평균 별점    |
| favorite_count | int | private | 빵집 좋아요 수    |
| review_count | int | private | 빵집 리뷰 수     |

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


<br>


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


<br>


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
| bakery_id | long | private | 빵집 ID |
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
| reviews | `List<MenuReviewResponse>` | private | 모든 메뉴 리뷰 목록 |

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


<br>


## 5) course
빵지순례와 관련된 정보들을 주고 받기 위한 DTO이다.

### CourseRequest

#### 1. Attributes
| Name | Type | Visibility | Description                   |
| :--- | :--- |:-----------|:------------------------------|
| title | String | private | 글 제목                          |
| subTitle | String | private | 부제목                           |
| parts | `List<CoursePartRequest>` | private | 해당하는 코스 파트들 (사용자의 작성 순서대로 저장) |

#### 2. Usage
- 빵지순례 글쓰기
- 빵지순례 글 수정하기

---

### CoursePartRequest

#### 1. Attributes
| Name | Type | Visibility | Description            |
| :--- | :--- |:-----------|:-----------------------|
| bakery_id | long | private | 해당 코스 파트에 해당하는 빵집의 ID  |
| text | String | private | 파트 내용                  |
| photo | String | private | 첨부 사진                  |
| distance | double | private | 이전 코스에서 현재 코스까지의 거리    |
| travelMinute | long | private | 이전 코스에서 현재 코스까지의 소요 시간 |

#### 2. Usage
- CourseRequest 내부

---

### CourseResponse

#### 1. Attributes
| Name | Type | Visibility | Description         |
| :--- | :--- |:-----------|:--------------------|
| writer | String | private | 해당 글의 작성자 (=본인 닉네임) |
| title | String | private | 글 제목                |
| subTitle | String | private | 부제목                 |
| allDistance | double | private | 총길이                 |
| allTravelMinute | long | private | 총시간                 |
| parts | `List<CoursePartResponse>` | private | 해당하는 코스 파트들         |

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
| Name | Type | Visibility | Description         |
| :--- | :--- |:-----------|:--------------------|
| writer | String | private | 해당 글의 작성자 (=본인 닉네임) |
| title | String | private | 글 제목                |
| subTitle | String | private | 부제목                 |
| allDistance | double | private | 총길이                 |
| allTravelMinute | long | private | 총시간                 |
| parts | `List<CoursePartResponse>` | private | 해당하는 코스 파트들         |
| reviews | `List<CourseReviewResponse>` | private | 루트에 쓰인 리뷰들          |
| isMine | boolean | private | 이 루트가 나의 글인지 아닌지    |

#### 2. Usage
- 빵지순례 세부 글 보기

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


<br>


## 6) myPage

### GetFavoriteBakeriesResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| id | long | private | 빵집 ID |
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
| Name | Type | Visibility | Description     |
| :--- | :--- |:-----------|:----------------|
| id | long | private | 빵지순례(course) ID |
| title | String | private | 빵지순례 제목         |
| subTitle | String | private | 빵지순례 부제목        |
| allDistance | double | private | 빵지순례 총거리        |
| allTravelMinute | long | private | 빵지순례 총소요 시간     |
| photo1 | String | private | 빵지순례 사진         |

#### 2. Usage
- 빵지순례 관심 목록 보기

---

### GetMyCourseResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| course_id | long | private | 빵지순례 ID     |
| title | String | private | 빵지순례 제목     |
| photo | String | private | 빵지순례 사진     |

#### 2. Usage
- 내가 작성한 빵지순례 보기

---

### GetMyBakeryReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakery_id | long | private | 빵집 ID       |
| name | String | private | 빵집 이름       |
| review_id | long | private | 빵집 리뷰 ID    |
| text | String | private | 빵집 리뷰 내용    |
| rating | double | private | 빵집 리뷰 별점    |
| photo | String | private | 빵집 리뷰 사진    |
| date | LocalDateTime | private | 빵집 리뷰 작성 시간 |

#### 2. Usage
- 내가 작성한 가게 리뷰 보기

---

### GetMyMenuReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description |
| :--- | :--- |:-----------|:------------|
| bakery_id | long | private | 빵집 ID       |
| menu_id | long | private | 메뉴 ID       |
| review_id | long | private | 메뉴 리뷰 ID    |
| bakery_name | String | private | 빵집 이름       |
| meun_name | String | private | 메뉴 이름       |
| text | String | private | 메뉴 리뷰 내용    |
| rating | double | private | 메뉴 리뷰 별점    |
| date | LocalDateTime | private | 메뉴 리뷰 작성 시간 |

#### 2. Usage
- 내가 작성한 메뉴 리뷰 보기

---

### GetMyCourseReviewResponse

#### 1. Attributes
| Name | Type | Visibility | Description               |
| :--- | :--- |:-----------|:--------------------------|
| course_id | long | private | 빵지순례(course) ID           |
| review_id | long | private | 빵지순례 리뷰 ID                |
| course_nickname | String | private | 빵지순례 작성자 닉네임              |
| title | String | private | 빵지순례 제목                   |
| review_nickname | String | private | 빵지순례 리뷰 작성자 (=사용자 본인) 닉네임 |
| text | String | private | 빵지순례 리뷰 내용                |
| date | LocalDateTime | private | 빵지순례 리뷰 쓴 시간              |

#### 2. Usage
- 내가 작성한 빵지순례 리뷰 보기

---



<br>
<br>



## 3.3. API

해당 파트에서는 각 컨트롤러에서 사용하는 api를 정의한다.

### 1. AuthController

인증 API

- 로그인과 로그아웃 시 서버의 상태를 변경하여야 하므로 POST 메소드를 사용
- 로그인 시 사용자의 아이디와 비밀번호를를 body에 담아 보내야 하므로 POST 메소드를 사용

| 기능 | HTTP Method | API 경로         |
| :--- | :--- |:---------------|
| 회원가입하기 | `POST` | `/auth/signup` |
| 로그인하기 | `POST` | `/auth/login`  |
| 로그아웃하기 | `POST` | `/auth/logout` |

---

### 2. MemberController

사용자 정보 및 마이페이지 API

| 기능                | HTTP Method | API 경로                           |
|:------------------| :--- |:---------------------------------|
| 내 프로필 조회          | `GET` | `/api/members/me`                |
| 프로필 수정            | `PATCH` | `/api/members/me`                |
| 회원 탈퇴하기           | `DELETE` | `/api/members/me`                |
| 내가 작성한 루트 보기      | `GET` | `/api/members/me/courses`        |
| 내가 작성한 빵집 리뷰 보기   | `GET` | `/api/members/me/bakery-reviews` |
| 내가 작성한 메뉴 리뷰 보기   | `GET` | `/api/members/me/menu-reviews`   |
| 내가 작성한 빵지순례 리뷰 보기 | `GET` | `/api/members/me/course-reviews` |
| 가게 관심 목록 보기       | `GET` | `/api/members/me/favorites/bakeries`      | 
| 루트 관심 목록 보기       | `GET` | `/api/members/me/favorites/courses`      | 


---

### 3. BakeryController 

빵집 및 빵집 리뷰 API

| 기능         | HTTP Method | API 경로 | 추가 정보                              |
|:-----------| :--- | :--- |:-----------------------------------|
| 가게 검색하기    | `GET` | `/api/bakeries` | `?keyword=`와 `?sort=`로 검색 및 정렬을 지원. sort=popular일 때 인기순 정렬, sort=review일 때 리뷰순 정렬 |
| 가게 정보 보기   | `GET` | `/api/bakeries/{bakeryId}` |                                    |
| 가게 리뷰 보기   | `GET` | `/api/bakeries/{bakeryId}/bakery-reviews` |                                    |
| 가게 리뷰 쓰기   | `POST` | `/api/bakeries/{bakeryId}/bakery-reviews` |                                    |
| 가게 리뷰 수정하기 | `PATCH` | `/api/bakery-reviews/{reviewId}` | 리뷰 ID는 가게 ID와 무관하게 독립적으로 존재        |
| 가게 리뷰 삭제하기 | `DELETE` | `/api/bakery-reviews/{reviewId}` |                                    |

---

### 4. MenuController 

메뉴 및 메뉴 리뷰 API

| 기능 | HTTP Method | API 경로                             | 
| :--- | :--- |:-----------------------------------| 
| 가게 메뉴 목록 보기 | `GET` | `/api/bakeries/{bakeryId}/menus`   |
| 메뉴 세부 정보 보기 | `GET` | `/api/menus/{menuId}`              | 
| 메뉴 리뷰 쓰기 | `POST` | `/api/menus/{menuId}/menu-reviews` | 
| 메뉴 리뷰 수정하기 | `PATCH` | `/api/menu-reviews/{reviewId}`     | 
| 메뉴 리뷰 삭제하기 | `DELETE` | `/api/menu-reviews/{reviewId}`     | 

---

### 5. ReportController 

빵집 제보 API

| 기능 | HTTP Method | API 경로                    | 
| :--- | :--- |:--------------------------|
| 제보 목록 보기 | `GET` | `/api/bakeries/{bakeryId}/reports` |
| 제보하기 | `POST` | `/api/bakeries/{bakeryId}/reports`            |
| 제보 수정하기 | `PATCH` | `/api/reports/{reportId}` |
| 제보 삭제하기 | `DELETE` | `/api/reports/{reportId}` |

---

### 6. CourseController 

빵지순례글 및 빵지순례 리뷰 API

| 기능 | HTTP Method | API 경로 | 추가 정보                                    |
| :--- | :--- | :--- |:-----------------------------------------|
| 루트 검색/목록 보기 | `GET` | `/api/courses` | `?keyword=`로 검색을 지원                      |
| 루트 세부 글 보기 | `GET` | `/api/courses/{courseId}` |                                          |
| 루트 작성하기 | `POST` | `/api/courses` |                                          |
| 루트 수정하기 | `PATCH` | `/api/courses/{courseId}` |                                          |
| 루트 삭제하기 | `DELETE` | `/api/courses/{courseId}` |                                          |
| 루트 리뷰 쓰기 | `POST` | `/api/courses/{courseId}/course-reviews` |                                          |
| 루트 리뷰 수정하기 | `PATCH` | `/api/course-reviews/{reviewId}` |                                          |
| 루트 리뷰 삭제하기 | `DELETE` | `/api/course-reviews/{reviewId}` |                                          |

---

### 7. FavoriteController

즐겨찾기(관심) 추가/삭제 API

| 기능 | HTTP Method | API 경로 |
| :--- | :--- | :--- |
| 관심 가게 추가하기 | `POST` | `/api/members/me/favorites/bakeries/{bakeryId}` | 
| 관심 가게 삭제하기 | `DELETE` | `/api/members/me/favorites/bakeries/{bakeryId}` | 
| 관심 루트 추가하기 | `POST` | `/api/members/me/favorites/courses/{courseId}` | 
| 관심 루트 삭제하기 | `DELETE` | `/api/members/me/favorites/courses/{courseId}` | 

---

