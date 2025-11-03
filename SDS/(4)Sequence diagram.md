# 4. Sequence diagram

### 1) 회원 가입하기


### 2) 로그인하기


### 3) 로그아웃하기


### 4) 회원 탈퇴하기


### 5) 가게 검색하기


### 6) 가게 정렬하기


### 7) 가게 정보 보기
위의 그림[4-7]은 사용자가 원하는 가게의 자세한 정보를 볼 수 있게 해주는 Use Case를 시퀀스 다이어그램으로 나타낸 것이다. 먼저 사용자가 시스템에 GET /api/bakeries/{bakeryId}라는 API를 보낸다. BakeryController는 이 요청을 받아서 bakeryId는 경로 변수(@PathVariable)로, 사용자 정보는 @AuthenticationPrincipal UserDetailsImpl userDetails를 통해 획득한다. 확보한 bakeryId와 사용자 ID(memId)를 가지고 getBakeryDetail() 메소드를 실행하여 BakeryService를 호출한다. BakeryService는 getBakeryDetail(Long bakeryId, Long memId) 메소드를 실행한다. 이 메소드는 bakeryRepository에 있는 findById(bakeryId)를 호출해서 사용자가 원하는 가게의 데이터를 데이터베이스에서 찾아낸다. 만약 해당 가게가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다. 데이터를 찾아낸 뒤, BakeryService는 조회한 Bakery 엔티티를 BakeryDetailResponse DTO로 변환시킨다. 그런 뒤 BakeryService가 완성된 BakeryDetailResponse를 BakeryController에게 전달한다. 이렇게 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 정보 조회가 완료된다.

### 8) 가게 관심 추가하기(스크랩하기)


### 9) 가게 관심 삭제하기


### 10) 가게 리뷰 보기


### 11) 가게 리뷰 쓰기
위의 그림[4-11]은 사용자가 가게 리뷰를 쓸 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 먼저 사용자가 리뷰할 bakeryId를 경로 변수에 담고 리뷰 내용이 포함된 BakeryReviewRequest DTO를 본문에 실어 POST/bakery/{bakeryId}/reviews 엔드포인트로 요청을 보낸다. 이 요청을 받은 BakeryController가 @PathVariable에서 bakeryId를 얻고, @AuthenticationPrincipal UserDetailsImpl을 사용하여 현재 로그인한 사용자의 정보를 획득한다. 또, @Valid 어노테이션을 통해 BakeryReviewRequest의 기본적인 유효성을 검증한다. 검증을 통과하면, 컨트롤러는 bakeryId, memId, 그리고 BakeryReviewRequest request DTO를 인수로 ReviewService의 addBakeryReview() 메소드를 호출하며 비즈니스 로직을 위임한다. ReviewService는 위임을 받은 후, 비즈니스 로직에 따른 추가적인 유효성 확인한다. 유효성을 통과하지 못하면 예외를 발생시킨다. 그리고 사용자의 권한을 확인하여 권한이 없으면 예외를 발생시킨다. 이를 통과하면, createBakeryReview 메소드를 통해 memId, bakeryId, DTO의 내용을 포함하는 BakeryReview 엔티티를 생성한다. 생성된 엔티티는 bakeryReviewRepository.save(bakeryReview)를 호출하여 데이터베이스에 저장된다. 저장 후, ReviewService는 저장된 BakeryReview 엔티티 정보를 BakeryReviewResponse DTO로 변환시키며, 이 DTO를 BakeryController에게 반환한다. 이렇게 받은 최종 응답을 BakeryController가 사용자에게 넘겨줌으로써 가게 리뷰 작성이 성공적으로 완료된다.

### 12) 가게 리뷰 수정하기


### 13) 가게 리뷰 삭제하기


### 14) 가게 메뉴 목록 보기


### 15) 메뉴 리뷰 보기


### 16) 메뉴 리뷰 쓰기


### 17) 메뉴 리뷰 수정하기


### 18) 메뉴 리뷰 삭제하기


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

