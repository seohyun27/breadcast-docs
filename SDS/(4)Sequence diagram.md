# 4. Sequence diagram

### 1) 회원 가입하기


### 2) 로그인하기


### 3) 로그아웃하기


### 4) 회원 탈퇴하기


### 5) 가게 검색하기


### 6) 가게 정렬하기


### 7) 가게 정보 보기
위의 그림[4-7]은 사용자가 원하는 가게의 자세한 정보를 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 시스템에 GET /api/bakeries/{bakeryId}라는 API를 보낸다. BakeryController는 이 요청을 받아서 bakeryId는 경로 변수(@PathVariable)로, 사용자 정보는 @AuthenticationPrincipal UserDetailsImpl userDetails를 통해 획득한다. 확보한 bakeryId와 사용자 ID(memId)를 가지고 getBakeryDetail() 메소드를 실행하여 BakeryService를 호출한다. BakeryService는 getBakeryDetail(Long bakeryId, Long memId) 메소드를 실행한다. 이 메소드는 bakeryRepository에 있는 findById(bakeryId)를 호출해서 사용자가 원하는 가게의 데이터를 데이터베이스에서 찾아낸다. 만약 해당 가게가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다. 데이터를 찾아낸 뒤, BakeryService는 조회한 Bakery 엔티티를 BakeryDetailResponse DTO로 변환시킨다. 그런 뒤 BakeryService가 완성된 BakeryDetailResponse를 BakeryController에게 전달한다. 이렇게 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 정보 조회가 완료된다.

### 8) 가게 관심 추가하기(스크랩하기)


### 9) 가게 관심 삭제하기


### 10) 가게 리뷰 보기
위의 그림[4-10]은 사용자가 가게 리뷰들을 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 조회할 bakeryId를 경로 변수에 경로 변수에 담아 GET/api/bakeries/{bakeryId}로 요청을 보낸다. 이 요청을 받은 BakeryController가 @PathVariable에서 bakeryId를 얻어내고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 정보를 획득한다. 얻어낸 bakeryId와 memId를 가지고 getBakeryReviews() 메소드를 실행하여 ReviewService를 호출한다. ReviewService는 getBakeryReviews(Long bakeryId, Long memId) 메소드를 실행한다. 이 메소드는 bakeryReviewRepository.findAll(bakeryId)를 호출해서 해당 bakeryId에 종속된 모든 BakeryReview 엔티티 리스트를 데이터베이스에서 찾아낸다. BakeryReview 엔티티 리스트를 찾아낸 뒤, ReviewService는 조회한 BakeryReview 엔티티들을 BakeryReviewResponse DTO 리스트로 변환시킨다. 그러고 나서 ReviewService가 완성된 BakeryReviewResponse 리스트를 BakeryController에게 전달한다. 이렇게 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 리뷰 목록 조회가 완료된다.

### 11) 가게 리뷰 쓰기
위의 그림[4-11]은 사용자가 가게 리뷰를 쓸 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 리뷰할 bakeryId를 경로 변수에 담고 리뷰 내용이 포함된 BakeryReviewRequest DTO를 본문에 실어 POST/api/bakeries/{bakeryId}/reviews로 요청을 보낸다. 이 요청을 받은 BakeryController가 @PathVariable에서 bakeryId를 얻고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 정보를 획득한다. 또, @Valid 어노테이션을 통해 BakeryReviewRequest의 기본적인 유효성을 검증한다. 검증을 통과하면, 컨트롤러는 bakeryId, memId, 그리고 BakeryReviewRequest request DTO를 인수로 ReviewService의 addBakeryReview() 메소드를 호출하며 비즈니스 로직을 위임한다. ReviewService는 위임을 받은 후, 비즈니스 로직에 따른 추가적인 유효성 확인한다. 유효성을 통과하지 못하면 예외를 발생시킨다. 그리고 사용자의 권한을 확인하여 권한이 없으면 예외를 발생시킨다. 이를 통과하면, createBakeryReview 메소드를 통해 memId, bakeryId, DTO의 내용을 포함하는 BakeryReview 엔티티를 생성한다. 생성된 엔티티는 bakeryReviewRepository.save(bakeryReview)를 호출하여 데이터베이스에 저장된다. 저장 후, ReviewService는 저장된 BakeryReview 엔티티 정보를 BakeryReviewResponse DTO로 변환시키며, 이 DTO를 BakeryController에게 반환한다. 이렇게 받은 최종 응답을 BakeryController가 사용자에게 넘겨줌으로써 가게 리뷰 작성이 성공적으로 완료된다.

### 12) 가게 리뷰 수정하기
위의 그림[4-12]은 사용자가 가게 리뷰를 수정할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 수정할 bakeryId와 reviewId를 경로 변수에 담고 수정된 내용이 포함된 BakeryReviewRequest DTO를 본문에 실어 PATCH/api/bakeries/{bakeryId}/reviews/{reviewId}로 요청을 보낸다. 이 요청을 받은 BakeryController가 @PathVariable에서 bakeryId와 reviewId를 얻어내고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 로그인한 사용자의 정보를 획득한다. 또한, @Valid 어노테이션을 통해 BakeryReviewRequest의 기본적인 유효성을 검증한다. 확보한 bakeryId, reviewId, memId, 그리고 request DTO를 가지고 updateBakeryReview() 메소드를 실행하여 ReviewService를 호출한다. ReviewService는 updateBakeryReview(Long bakeryReviewId, Long memId, BakeryReviewRequest request) 메소드를 실행한다. 이 메소드는 먼저 현재 로그인한 사용자가 해당 리뷰의 작성자인지 확인하여 수정 권한을 검증한다. 만약 아니면 예외를 발생시켜 중단시킨다.  권한 검증이 완료되면, bakeryReviewRepository.findById(bakeryReviewId)를 호출해서 수정할 리뷰 엔티티를 데이터베이스에서 찾아낸다. 만약 해당 리뷰가 존재하지 않으면 .orElseThrow()를 통해 적절한 예외를 발생시켜 처리를 중단한다. 리뷰 엔티티가 존재한다면, 조회된 BakeryReview 엔티티의 update(double rating, String text, String photo) 메소드를 호출하여 DTO의 새로운 내용으로 엔티티 필드를 수정한다. 이때 JPA의 Dirty Checking이 적용되므로 별도의 save() 호출 없이 변경 사항이 트랜잭션 종료 시점에 데이터베이스에 반영된다. 수정이 끝난 후, ReviewService는 수정된 BakeryReview 엔티티 정보를 BakeryReviewResponse DTO로 변환시키고, 이 DTO를 BakeryController에게 전달한다. 이렇게 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 리뷰 수정이 완료된다.

### 13) 가게 리뷰 삭제하기
위의 그림[4-13]은 사용자가 가게 리뷰를 삭제할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 삭제할 bakeryId와 reviewId를 경로 변수에 담고 DELETE/api/bakeries/{bakeryId}/reviews/{reviewId}로 요청을 보낸다. 이 요청을 받은 BakeryController는 @PathVariable에서 bakeryId와 reviewId를 얻는다. 그리고 @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 memId를 획득한다. 얻어낸 bakeryId, reviewId, memId를 가지고 bakeryReviewDelete() 메소드를 실행하여 ReviewService를 호출한다. ReviewService는 deleteBakeryReview(Long bakeryReviewId, Long memId) 메소드를 실행한다. 이 메소드는 먼저 현재 로그인한 사용자가 해당 리뷰의 작성자인지 확인하여 삭제 권한을 검증한다. 만약 아니면 예외를 발생시켜 중단시킨다. 그리고 나서 bakeryReviewRepository.findById(bakeryReviewId)를 호출해서 삭제할 리뷰 엔티티가 데이터베이스에 존재하는지 확인한다. 만약 해당 리뷰 ID가 존재하지 않으면 .orElseThrow()를 통해 적절한 예외를 발생시켜 처리를 중단한다. 권한이 있고 리뷰 엔티티도 존재한다면 ReviewService는 bakeryReviewRepository.deleteById(bakeryReviewId)를 호출하여 해당 엔티티를 데이터베이스에서 삭제한다. 삭제 작업이 성공적으로 끝나면 ReviewService는 void를 반환하며 BakeryController에게 성공을 알린다. 이렇게 전달받은 정보를 BakeryController가 최종적으로 ResponseEntity<Void>를 사용자에게 넘겨줌으로써 가게 리뷰 삭제가 완료된다

### 14) 가게 메뉴 목록 보기
위의 그림[4-14]은 사용자가 가게 메뉴 목록을 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 조회할 bakeryId를 경로 변수에 담아 GET/api/bakeries/{bakeryId}/menus 엔드포인트로 요청을 보낸다. 이 요청을 받은 MenuController는 @PathVariable에서 bakeryId를 얻어낸다. 컨트롤러는 이 bakeryId를 가지고 getMenus() 메소드를 실행하여 MenuService를 호출한다. MenuService는 getMenus(Long bakeryId) 메소드를 실행한다. 이 메소드는 먼저 menuRepository.findByBakeryId(bakeryId)를 호출해서 해당 bakeryId에 종속된 모든 Menu 엔티티 리스트를 데이터베이스에서 찾아낸다. Menu 엔티티 리스트를 찾아낸 뒤, MenuService는 이 리스트를 순회하며 각 메뉴에 대한 추가 정보를 계산한다. MenuReviewRepository의 countByMenuId(menuId)를 호출하여 해당 메뉴의 총 리뷰 수를 얻고, getAverageRating(menuId) 내부 메소드를 통해 MenuReviewRepository.findByMenuId(menuId)를 호출하여 모든 리뷰 데이터를 가져와 평균 별점을 계산한다. 계산된 평균 별점과 리뷰 수를 포함하여 Menu 엔티티들을 GetMenusResponse DTO 리스트로 변환시킨다. 그러고 나서 MenuService가 완성된 GetMenusResponse 리스트를 MenuController에게 전달한다. 이렇게 전달받은 정보를 MenuController가 최종적으로 사용자에게 넘겨줌으로써 빵집 메뉴 목록 조회가 완료된다.

### 15) 메뉴 리뷰 보기
위의 그림[4-15]은 사용자가 가게 메뉴 상세 정보를 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 조회할 menuId를 경로 변수에 담아 GET/api/bakeries/{bakeryId}/menus/{menuId}로 요청을 보낸다. 이 요청을 받은 MenuController는 @PathVariable에서 menuId를 얻어내고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 memId를 획득한다. 확보한 menuId와 memId를 가지고 getMenuDetail() 메소드를 실행하여 MenuService를 호출한다. MenuService는 getMenuDetail(Long menuId, Long memId) 메소드를 실행한다. 이 메소드는 먼저 menuRepository.findById(menuId)를 호출해서 사용자가 원하는 Menu 엔티티를 데이터베이스에서 찾아낸다. 만약 해당 메뉴가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다. 존재한다면, MenuService는 메뉴의 상세 정보를 구성하기 위해 MenuReviewRepository를 호출한다. countByMenuId(menuId)를 호출하여 총 리뷰 수를 가져오고, 내부 메소드인 getAverageRating(menuId) 실행을 위해 findByMenuId(menuId)를 호출하여 모든 리뷰 데이터를 가져온 뒤 이를 활용하여 평균 별점을 계산한다. 계산된 평균 별점과 리뷰 수, 그리고 메뉴의 기본 정보를 통합하여 GetMenuDetailResponse DTO를 구성한다. MenuService는 이 DTO에 계산된 평균 별점과 리뷰 수를 직접 설정한다. 그러고 나서 MenuService가 모든 정보가 반영된 GetMenuDetailResponse를 MenuController에게 전달한다. 이렇게 전달받은 정보를 MenuController가 최종적으로 사용자에게 넘겨줌으로써 메뉴 상세 정보 조회가 완료된다.

### 16) 메뉴 리뷰 쓰기
위의 그림[4-16]은 사용자가 가게 메뉴 리뷰를 쓸 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 리뷰할 menuId를 경로 변수에 담고 리뷰 내용이 포함된 AddMenuReviewRequest DTO를 본문에 실어 POST/api/bakeries/{bakeryId}/menus/{menuId}/reviews로 요청을 보낸다. 이 요청을 받은 MenuController가 @PathVariable에서 menuId를 추출하고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 memId를 얻어낸다. 또한, @Valid 어노테이션을 통해 AddMenuReviewRequest의 기본적인 유효성을 검증한다. 확보한 menuId, memId, 그리고 AddMenuReviewRequest DTO를 가지고 addMenuReview() 메소드를 실행하여 ReviewService를 호출한다. ReviewService는 addMenuReview(Long menuId, Long memId, AddMenuReviewRequest request) 메소드를 실행한다. 이 메소드는 먼저 현재 로그인한 사용자의 작성 권한을 검증한다. 만약 아니면 예외를 발생시켜 중단시킨다. menuRepository.findById(menuId)를 호출하여 리뷰를 작성할 메뉴가 실제로 존재하는지 확인한다. 만약 메뉴가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다. 그리고 나서 추가로 DTO 유효성도 검증한다. 유효하지 않으면 예외를 발생시켜 적절하게 처리한다. 조건을 다 통과하면, ReviewService는 memId, menuId, DTO의 내용을 포함하는 MenuReview 엔티티를 생성한다. 생성된 엔티티는 menuReviewRepository.save(menuReview)를 호출하여 데이터베이스에 저장된다. 저장이 완료된 후, ReviewService는 저장된 MenuReview 엔티티 정보를 MenuReviewResponse DTO로 변환시키며, 이 DTO를 MenuController에게 전달한다. 이렇게 받은 최종 응답을 MenuController가 ResponseEntity에 담아 사용자에게 넘겨줌으로써 메뉴 리뷰 작성이 성공적으로 완료된다.

### 17) 메뉴 리뷰 수정하기
위의 그림[4-17]은 사용자가 가게 메뉴 리뷰를 수정할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 수정할 reviewId를 경로 변수에 담고 수정된 내용이 포함된 UpdateMenuReviewRequest DTO를 본문에 실어 PATCH/api/bakeries/{bakeryId}/menus/{menuId}/reviews/{reviewId}로 요청을 보낸다. 이 요청을 받은 MenuController는 @PathVariable에서 reviewId를 얻어내고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 memId를 획득한다. 또한, @Valid 어노테이션을 통해 UpdateMenuReviewRequest의 기본적인 유효성을 검증한다. 확보한 reviewId, memId, 그리고 UpdateMenuReviewRequest DTO를 가지고 updateMenuReview() 메소드를 실행하여 ReviewService를 호출한다. ReviewService는 updateMenuReview(Long menuReviewId, Long memId, UpdateMenuReviewRequest request) 메소드를 실행한다. 이 메소드는 먼저 menuReviewRepository.findById(menuReviewId)를 호출해서 수정할 리뷰 엔티티를 데이터베이스에서 찾아낸다. 만약 해당 리뷰 ID가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다. 리뷰 엔티티를 찾은 뒤, ReviewService는 조회된 리뷰 엔티티의 작성자 ID와 현재 로그인한 memId가 일치하는지 확인하여 수정 권한을 검증한다. 권한이 일치하지 않으면 수정 권한이 없으므로 예외를 발생시킨다. UpdateMenuReviewRequest의 유효성도 검증한다. 검증이 완료되면, 조회된 리뷰 엔티티에 MenuReview.update(double rating, String text) 메소드를 호출하여 DTO의 새로운 내용(별점, 내용)을 반영한다. 이때 Dirty Checking가 적용되어 변경 사항이 데이터베이스에 자동으로 반영된다. 수정이 완료된 후, ReviewService는 수정된 MenuReview 엔티티 정보를 MenuReviewResponse DTO로 변환시키며, 이 DTO를 MenuController에게 전달한다. 이렇게 전달받은 정보를 MenuController가 최종적으로 사용자에게 넘겨줌으로써 메뉴 리뷰 수정이 완료된다.

### 18) 메뉴 리뷰 삭제하기
위의 그림[4-18]은 사용자가 가게 메뉴 리뷰를 삭제할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 reviewId를 경로 변수에 담아 DELETE/api/bakeries/{bakeryId}/menus/{menuId}/reviews/{reviewId}로 요청을 보낸다. 이 요청을 받은 MenuController는 @PathVariable에서 reviewId를 얻어내고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 memId를 획득한다. 얻어낸 reviewId와 memId를 가지고 deleteMenuReview() 메소드를 실행하여 ReviewService를 호출한다. ReviewService는 deleteMenuReview(Long reviewId, Long memId) 메소드를 실행한다. 이 메소드는 먼저 menuReviewRepository.findById(reviewId)를 호출해서 삭제할 리뷰 엔티티가 데이터베이스에 존재하는지 확인한다. 만약 해당 리뷰가 존재하지 않으면 .orElseThrow()를 통해 적절한 예외를 발생시켜 처리를 중단한다. 리뷰 엔티티를 찾은 뒤, ReviewService는 조회된 메뉴 리뷰 엔티티의 작성자 ID와 현재 로그인한 memId가 일치하는지 확인하여 삭제 권한을 검증한다. 권한이 일치하지 않으면 예외를 발생시킨다. 권한 확인이 끝나면 menuReviewRepository.deleteById(reviewId)를 호출하여 데이터베이스에서 메뉴 리뷰를 삭제한다. 삭제 작업이 성공적으로 완료된 후, ReviewService는 void를 반환하며 MenuController에게 성공을 알린다. 이렇게 전달받은 성공 응답을 MenuController가 ResponseEntity<Void>로 최종적으로 사용자에게 넘겨줌으로써 메뉴 리뷰 삭제가 완료된다.

### 19) 제보 보기


### 20) 제보하기


### 21) 제보 삭제하기


### 22) 인기 빵지순례 보기


### 23) 빵지순례 검색하기


### 24) 빵지순례 상세 페이지 보기


### 25) 빵지순례 관심 추가하기


### 26) 빵지순례 관심 삭제하기


### 27) 빵지순례 리뷰 쓰기


### 28) 빵지순례 리뷰 수정하기


### 29) 빵지순례 리뷰 삭제하기


### 30) 빵지순례 만들기


### 31) 빵지순례 수정하기


### 32) 빵지순례 삭제하기


### 33) 닉네임 변경하기


### 34) 관심 가게 목록 보기


### 35) 관심 빵지순례 목록 보기


### 36) 내가 쓴 가게 리뷰 보기


### 37) 내가 쓴 메뉴 리뷰 보기


### 38) 내가 쓴 빵지순례 리뷰 보기


### 39) 내가 쓴 빵지순례 리뷰

