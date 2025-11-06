# 4. Sequence diagram

## 1) 회원 가입하기


## 2) 로그인하기


## 3) 로그아웃하기


## 4) 회원 탈퇴하기


## 5) 가게 검색하기
![5_BakerySearch](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/5-Bakery-Search.jpg?raw=true)

- 사용자가 가게 검색할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- BakeryController가 DTO를 받아 searchBakeries() 메소드를 실행하여 BakeryService를 호출한다.
- BakeryService는 searchBakeries() 메소드를 실행한다.
- 이 메소드는 먼저 받아온 키워드를 기반으로 검색어가 포함된 Bakery 엔티티들을 데이터베이스에서 찾아낸다. 
- Bakery 엔티티 리스트를 SearchBakeryResponse DTO 리스트로 변환시킨다.
- BakeryService가 완성된 SearchBakeryResponse 리스트를 BakeryController에게 전달한다.
- 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 검색이 완료된다.

## 6) 가게 정렬하기
![6_BakerySort](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/6-Bakery-Sort.jpg?raw=true)

- 사용자가 가게 목록을 정렬을 할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 BakeryController는 정렬 기준 정보가 들어있는 DTO를 가지고 searchBakeries() 메소드를 실행하여 BakeryService를 호출한다.
- BakeryService는 searchBakeries() 메소드를 실행한다. 
- BakeryService는 사용자가 보낸 정렬 기준 순으로 리스트를 정렬한다.
- 정렬까지 완료된 Bakery 엔티티 리스트를 SearchBakeryResponse DTO 리스트로 변환시킨다.
- BakeryService가 완성된 SearchBakeryResponse 리스트를 BakeryController에게 전달한다.
- 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 목록 정렬이 완료된다.

## 7) 가게 정보 보기
![7_BakeryDetailShow](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/7-Bakery-Detail-Show.jpg?raw=true)

- 사용자가 원하는 가게의 자세한 정보를 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- BakeryController는 요청을 받아서 bakeryId, 사용자 정보를 가지고 getBakeryDetail() 메소드를 실행하여 BakeryService를 호출한다.
- BakeryService는 getBakeryDetail() 메소드를 실행한다.
- 이 메소드는 bakeryRepository에 있는 findById()를 호출해서 사용자가 원하는 가게의 데이터를 데이터베이스에서 찾아낸다.
- 만약 해당 가게가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 데이터를 찾아낸 뒤, BakeryService는 조회한 Bakery 엔티티를 BakeryDetailResponse DTO로 변환시킨다.
- 그런 뒤 BakeryService가 완성된 BakeryDetailResponse를 BakeryController에게 전달한다.
- 이렇게 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 정보 조회가 완료된다.

## 8) 가게 관심 추가하기(스크랩하기)


## 9) 가게 관심 삭제하기


## 10) 가게 리뷰 보기
![10_BakeryReviewShow](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/10-Bakery-Review-Show.jpg?raw=true)

- 사용자가 가게 리뷰들을 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 BakeryController가 bakeryId, 사용자 정보를 가지고 getBakeryReviews() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 getBakeryReviews() 메소드를 실행한다.
- 이 메소드는 bakeryReviewRepository.findAll()를 호출해서 해당 빵집에 있는 모든 BakeryReview 엔티티 리스트를 데이터베이스에서 찾아낸다.
- BakeryReview 엔티티 리스트를 찾아낸 뒤, ReviewService는 조회한 BakeryReview 엔티티들을 BakeryReviewResponse DTO 리스트로 변환시킨다.
- 그러고 나서 ReviewService가 완성된 BakeryReviewResponse 리스트를 BakeryController에게 전달한다.
- 이렇게 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 리뷰 목록 조회가 완료된다.

## 11) 가게 리뷰 쓰기
![11_BakeryReviewAdd](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/11-Bakery-Review-Add.jpg?raw=true)

- 사용자가 가게 리뷰를 쓸 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 BakeryController가 bakeryId, 사용자 정보, DTO를 받아 addBakeryReview() 메소드를 실행해서 ReviewService를 호출한다.
- ReviewService는 사용자의 권한을 확인하여 권한이 없으면 예외를 발생시킨다.
- 이를 통과하면, ReviewService는 DTO 유효성 확인하여 유효성이 없면 예외를 발생시킨다.
- 모든 검증을 만족하면 createBakeryReview 메소드를 통해 DTO의 내용을 포함하는 BakeryReview 엔티티를 생성한다.
- 생성된 엔티티는 bakeryReviewRepository.save()를 호출하여 데이터베이스에 저장된다.
- 저장 후, ReviewService는 저장된 BakeryReview 엔티티 정보를 BakeryReviewResponse DTO로 변환시킨다.
- 이 DTO를 BakeryController에게 반환한다.
- 최종 응답을 BakeryController가 사용자에게 넘겨줌으로써 가게 리뷰 작성이 성공적으로 완료된다.

## 12) 가게 리뷰 수정하기
![12_BakeryReviewUpdate](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/12-Bakery-Review-Update.jpg?raw=true)

- 사용자가 가게 리뷰를 수정할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 BakeryController가 bakeryId와 reviewId, 사용자 정보, DTO를 가지고 updateBakeryReview() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 updateBakeryReview() 메소드를 실행한다.
- 이 메소드는 먼저 DTO의 유효성을 검사하고 만족하지 못하면 적절한 예외를 처리한다.
- bakeryReviewRepository.findById()를 호출해서 수정할 리뷰 엔티티를 데이터베이스에서 찾아낸다.
- 만약 해당 리뷰가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 사용자의 수정 권한을 검증하고 권한이 없으면 예외를 발생시켜 중단시킨다.
- 모든 검증을 통과하면 조회된 BakeryReview 엔티티의 update() 메소드를 호출하여 DTO의 새로운 내용으로 엔티티 필드를 수정한다.
- 수정이 끝난 후, ReviewService는 수정된 BakeryReview 엔티티 정보를 BakeryReviewResponse DTO로 변환시키고, 이 DTO를 BakeryController에게 전달한다.
- 이렇게 전달받은 정보를 BakeryController가 최종적으로 사용자에게 넘겨줌으로써 가게 리뷰 수정이 완료된다.

## 13) 가게 리뷰 삭제하기
![13_BakeryReviewDelete](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/13-Bakery-Review-Delete.jpg?raw=true)

- 사용자가 가게 리뷰를 삭제할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 BakeryController는 bakeryId와 reviewId, 사용자 정보를 사용하여 bakeryReviewDelete() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 deleteBakeryReview() 메소드를 실행한다.
- 먼저 bakeryReviewRepository.findById()를 호출해서 삭제할 리뷰 엔티티가 데이터베이스에 존재하는지 확인한다.
- 만약 해당 리뷰가 존재하지 않으면 적절한 예외를 발생시켜 처리한다.
- 사용자가 삭제 권한이 있는지 검증하고 권한이 없으면 예외를 발생시켜 처리한다.
- 권한이 있고 리뷰 엔티티도 존재한다면 ReviewService는 bakeryReviewRepository.deleteById()를 호출하여 해당 엔티티를 데이터베이스에서 삭제한다.
- 삭제 작업이 성공적으로 끝나면 ReviewService는 void를 반환하며 BakeryController에게 성공을 알린다.
- BakeryController가 최종적으로 ResponseEntity<Void>를 사용자에게 넘겨줌으로써 가게 리뷰 삭제가 완료된다.

## 14) 가게 메뉴 목록 보기
![14_MenuListShow](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/14-Menu-List-Show.jpg?raw=true)

- 사용자가 가게 메뉴 목록을 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 MenuController는 bakeryId를 가지고 getMenus() 메소드를 실행하여 MenuService를 호출한다.
- MenuService는 getMenus() 메소드를 실행한다.
- 이 메소드는 먼저 menuRepository.findByBakeryId()를 호출해서 해당 bakeryId에 종속된 모든 Menu 엔티티 리스트를 데이터베이스에서 찾아낸다.
- Menu 엔티티 리스트를 찾아낸 뒤, MenuService는 이 리스트를 순회하며 각 메뉴에 대한 추가 정보를 계산한다.
- MenuReviewRepository의 countByMenuId()를 호출하여 해당 메뉴의 총 리뷰 수를 얻고, getAverageRating() 내부 메소드를 통해 MenuReviewRepository.findByMenuId()를 호출하여 모든 리뷰 데이터를 가져와 평균 별점을 계산한다.
- 계산된 평균 별점과 리뷰 수를 포함하여 Menu 엔티티들을 GetMenusResponse DTO 리스트로 변환시킨다.
- 그러고 나서 MenuService가 완성된 GetMenusResponse 리스트를 MenuController에게 전달한다.
- 이렇게 전달받은 정보를 MenuController가 최종적으로 사용자에게 넘겨줌으로써 빵집 메뉴 목록 조회가 완료된다.

## 15) 메뉴 리뷰 보기
![15_MenuDetailShow](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/15-Menu-Detail-Show.jpg?raw=true)

- 사용자가 가게 메뉴 리뷰 정보를 포함한 메뉴 정보를 볼 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 MenuController는 menuId, 사용자 정보를 가지고 getMenuDetail() 메소드를 실행하여 MenuService를 호출한다.
- MenuService는 getMenuDetail() 메소드를 실행한다.
- 이 메소드는 먼저 menuRepository.findById()를 호출해서 사용자가 원하는 Menu 엔티티를 데이터베이스에서 찾아낸다.
- 만약 해당 메뉴가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 존재한다면, MenuService는 메뉴의 상세 정보를 구성하기 위해 MenuReviewRepository를 호출한다.
- countByMenuId()를 호출하여 총 리뷰 수를 가져오고, 내부 메소드인 getAverageRating() 실행을 위해 findByMenuId()를 호출하여 모든 리뷰 데이터를 가져온 뒤 이를 활용하여 평균 별점을 계산한다.
- 계산된 평균 별점과 리뷰 수, 그리고 메뉴의 기본 정보를 통합하여 GetMenuDetailResponse DTO를 구성한다.
- MenuService는 이 DTO에 계산된 평균 별점과 리뷰 수를 직접 설정한다.
- MenuService가 모든 정보가 반영된 GetMenuDetailResponse DTO를 MenuController에게 전달한다.
- 이렇게 전달받은 정보를 MenuController가 최종적으로 사용자에게 넘겨줌으로써 메뉴 상세 정보 조회가 완료된다.

## 16) 메뉴 리뷰 쓰기
![16_MenuReviewAdd](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/16-Menu-Review-Add.jpg?raw=true)

- 사용자가 가게 메뉴 리뷰를 쓸 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 MenuController가 menuId, 사용자 정보, DTO를 가지고 addMenuReview() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 addMenuReview() 메소드를 실행한다.
- 이 메소드는 먼저 현재 로그인한 사용자의 작성 권한을 검증한다.
- 만약 아니면 예외를 발생시켜 처리한다.
- menuRepository.findById()를 호출하여 리뷰를 작성할 메뉴가 실제로 존재하는지 확인한다.
- 만약 메뉴가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 조건을 다 통과하면, ReviewService는 DTO의 내용을 포함하는 MenuReview 엔티티를 생성한다.
- 생성된 엔티티는 menuReviewRepository.save()를 호출하여 데이터베이스에 저장된다.
- 저장이 완료된 후, ReviewService는 저장된 MenuReview 엔티티 정보를 MenuReviewResponse DTO로 변환시키며, 이 DTO를 MenuController에게 전달한다.
- 이렇게 받은 최종 응답을 MenuController가 ResponseEntity에 담아 사용자에게 넘겨줌으로써 메뉴 리뷰 작성이 성공적으로 완료된다.

## 17) 메뉴 리뷰 수정하기
![17_MenuReviewUpdate](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/17-Menu-Review-Update.jpg?raw=true)

- 사용자가 가게 메뉴 리뷰를 수정할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다..
- 요청을 받은 MenuController는 reviewId, 사용자 정보, DTO를 가지고 updateMenuReview() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 updateMenuReview() 메소드를 실행한다.
- 이 메소드는 먼저 menuReviewRepository.findById()를 호출해서 수정할 리뷰 엔티티를 데이터베이스에서 찾아낸다.
- 만약 해당 리뷰 ID가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 리뷰 엔티티를 찾은 뒤, ReviewService는 수정 권한을 검증한다.
- 수정 권한이 없으면 예외를 발생시켜 처리한다.
- UpdateMenuReviewRequest의 유효성도 검증한다.
- 검증이 완료되면, 조회된 리뷰 엔티티에 MenuReview.update() 메소드를 호출하여 DTO의 새로운 내용(별점, 내용)을 반영한다.
- 수정이 완료된 후, ReviewService는 수정된 MenuReview 엔티티 정보를 MenuReviewResponse DTO로 변환시키며, 이 DTO를 MenuController에게 전달한다.
- 이렇게 전달받은 정보를 MenuController가 최종적으로 사용자에게 넘겨줌으로써 메뉴 리뷰 수정이 완료된다.

## 18) 메뉴 리뷰 삭제하기
![18_MenuReviewDelete](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/18-Menu-Review-Delete.jpg?raw=true)

- 사용자가 가게 메뉴 리뷰를 삭제할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 MenuController는 reviewId, 사용자 정보를 가지고 deleteMenuReview() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 deleteMenuReview() 메소드를 실행한다.
- 이 메소드는 먼저 menuReviewRepository.findById()를 호출해서 삭제할 리뷰 엔티티가 데이터베이스에 존재하는지 확인한다.
- 만약 해당 리뷰가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 리뷰 엔티티를 찾은 뒤, ReviewService는 삭제 권한을 검증한다.
- 권한이 없으면 예외를 발생시켜 처리한다.
- 권한 확인이 끝나면 menuReviewRepository.deleteById()를 호출하여 데이터베이스에서 메뉴 리뷰를 삭제한다.
- 삭제 작업이 성공적으로 완료된 후, ReviewService는 void를 반환하며 MenuController에게 성공을 알린다.
- 이렇게 전달받은 성공 응답을 MenuController가 ResponseEntity<Void>로 최종적으로 사용자에게 넘겨줌으로써 메뉴 리뷰 삭제가 완료된다.

## 19) 제보 보기
![19_ReportShow](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/19-Report-Show.jpg?raw=true)

- 사용자가 제보글을 조회할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 ReportController는 bakeryId, 사용자 정보를 사용하여 getReports() 메소드를 실행하여 ReportService를 호출한다.
- ReportService는 getReports() 메소드를 실행한다.
- 이 메소드는 bakeryReportRepository.findByBakeryIdOrderByCreatedAtDesc()를 호출하여 해당 bakeryId에 종속된 BakeryReport 엔티티 목록을 최신순으로 정렬하여 데이터베이스에서 찾아낸다.
- BakeryReport 엔티티 리스트를 찾아낸 뒤, ReportService는 이 리스트를 순회하며 추가 정보를 포함시킨다.
- 모든 정보가 반영되면, ReportService는 BakeryReport 엔티티 리스트를 ReportsResponse DTO 리스트로 변환시킨다.
- 그러고 나서 ReportService가 완성된 ReportsResponse 리스트를 ReportController에게 전달한다.
- 이렇게 전달받은 정보를 ReportController가 최종적으로 사용자에게 넘겨줌으로써 빵집 제보 보기가 완료된다.

## 20) 제보하기
![20_ReportAdd](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/20-Report-Add.jpg?raw=true)

- 사용자가 제보글을 쓸 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 ReportController는 bakeryId, 사용자 정보, DTO를 가지고 addReport() 메소드를 실행하여 ReportService를 호출한다.
- ReportService는 addReport() 메소드를 실행한다.
- 이 메소드는 먼저 사용자가 쓰기 권한이 있는지 검증한다.
- 권한이 없으면 예외를 발생시킨다.
- 그리고 나서 제보글에 필요한 필수 정보가 올바른지 확인하는 유효성 검증을 수행한다.
- 이 검증이 완료되면, ReportService는 memId, bakeryId, DTO의 내용을 포함하는 BakeryReport 엔티티를 생성한다.
- 생성된 엔티티는 bakeryReportRepository.save()를 호출하여 데이터베이스에 저장된다.
- 저장이 완료된 후, ReportService는 저장된 BakeryReport 엔티티 정보를 ReportsResponse DTO로 변환시키며, 이 DTO를 ReportController에게 전달한다.
- 이렇게 받은 최종 응답을 ReportController가 ResponseEntity에 담아 사용자에게 넘겨줌으로써 빵집 제보 작성이 성공적으로 완료된다.

## 21) 제보 삭제하기
![21_ReportDelete](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/21-Report-Delete.jpg?raw=true)

- 사용자가 제보글을 삭제할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 ReportController는 reportId와 사용자 정보를 가지고 deleteBakeryReport() 메소드를 실행하여 ReportService를 호출한다.
- ReportService는 deleteBakeryReport() 메소드를 실행한다.
- 이 메소드는 먼저 bakeryReportRepository.findById를 호출해서 삭제할 제보 엔티티가 데이터베이스에 존재하는지 확인한다.
- 만약 해당 제보글이 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 제보 엔티티를 찾은 뒤, ReportService는 삭제 권한을 검증한다.
- 권한이 없으면 예외를 발생시킨다.
- 권한 확인이 끝나면 bakeryReportRepository.deleteById()를 호출하여 데이터베이스에서 해당 제보를 삭제한다.
- 삭제 작업이 성공적으로 완료된 후, ReportService는 void를 반환하며 ReportController에게 성공을 알린다.
- 이렇게 전달받은 성공 응답을 ReportController가 ResponseEntity 상태로 최종적으로 사용자에게 넘겨줌으로써 빵집 제보 삭제가 완료된다.

## 22) 인기 빵지순례 보기
![22_CourseFavoriteShow](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/22-Course-Favorite-Show.jpg?raw=true)

- 사용자가 인기 있는 빵지순례 글 목록을 조회할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.-
- 요청을 받은 CourseController는 getPopularCourses() 메소드를 실행하며 CourseService를 호출한다.
- CourseService는 getPopularCourses() 메소드를 실행한다.
- 이 메소드는 먼저 courseRepository.findAll()을 호출하여 모든 Course 엔티티를 데이터베이스에서 가져온다.-
- 모든 Course 엔티티를 가져온 후, 코스 리스트를 순회하며 각 코스에 대해 favoriteCourseRepository.countByCourseId(course.getId())를 호출하여 해당 코스의 좋아요 수를 계산한다.
- 좋아요 수를 계산해낸 뒤, CourseService는 계산된 좋아요 수를 기준으로 코스 리스트를 내림차순으로 정렬한다.
- 최종 Course 엔티티 리스트를 GetSimpleCoursesResponse DTO 리스트로 변환시킨다.
- 그러고 나서 CourseService가 완성된 GetSimpleCoursesResponse 리스트를 CourseController에게 전달한다.
- 이렇게 전달받은 정보를 CourseController가 최종적으로 사용자에게 넘겨줌으로써 인기 빵지순례 글 목록 조회가 완료된다.

## 23) 빵지순례 검색하기
![23_CourseSearch](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/23-Course-Search.jpg?raw=true)

- 사용자가 빵지순례 글을 검색할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 CourseController는 SearchCourseRequest DTO를 가지고 searchCourses() 메소드를 실행하여 CourseService를 호출한다.
- CourseService는 searchCourses() 메소드를 실행한다
- 이 메소드는 courseRepository.findByTitleContainingOrKeywordContaining()를 호출하여 사용자가 입력한 키워드를 기반으로 여기에 맞는 Course 엔티티 목록을 데이터베이스에서 찾아낸다.
- 검색된 Course 엔티티 목록을 찾아낸 뒤, CourseService는 이 목록을 순회하며 각 코스에 대한 추가 정보를 계산한다.
- favoriteCourseRepository.countByCourseId(course.getId())를 호출하여 각 코스의 좋아요 수를 계산한다.
- 모든 정보가 포함되면, CourseService는 Course 엔티티 리스트를 GetSimpleCoursesResponse DTO 리스트로 변환시킨다.
- 그러고 나서 CourseService가 완성된 GetSimpleCoursesResponse 리스트를 CourseController에게 전달한다.
- 이렇게 전달받은 정보를 CourseController가 최종적으로 사용자에게 넘겨줌으로써 빵지순례 글 검색이 완료된다.

## 24) 빵지순례 상세 페이지 보기
![24_CourseDetailShow](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/24-Course-Detail-Show.jpg?raw=true)

- 사용자가 빵지순례 글 상세 정보를 조회할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 이 요청을 받은 CourseController는 courseId와 UserDetailsImpl를 가지고 getCourseDetail() 메소드를 실행하여 CourseService를 호출한다.
- CourseService는 getCourseDetail() 메소드를 실행한다.
- 이 메소드는 코스 상세 정보 조회를 위해 여러 리포지토리에서 데이터를 가져와 하나의 객체로 통합한다.
- 먼저 courseRepository.findById()를 호출하여 코스의 기본 정보를 가져온다.
- 이때 코스가 없으면 적절한 예외 처리를 해준다.
- 만약 있다면, favoriteCourseRepository.countByCourseId()를 호출하여 좋아요 총 개수를 가져온다.
- 이어서 coursePartRepository.findByCourseId()를 호출하여 코스에 포함된 빵집 목록(경로)을 가져온다.
- 마지막으로 courseReviewRepository.findByCourseId()를 호출하여 해당 코스의 리뷰 목록을 가져온다.
- 가져온 모든 데이터를 처리하는 과정에서 CourseService는 코스의 기본 정보와 좋아요 수, 빵집 목록, 리뷰를 통합한다.
- 모든 정보가 통합된 뒤, CourseService는 이를 CourseDetailResponse DTO 객체에 담아 CourseController에게 반환한다.
- 이렇게 전달받은 정보를 CourseController가 최종적으로 사용자에게 넘겨줌으로써 빵지순례 글 상세 정보 조회가 완료된다.

## 25) 빵지순례 관심 추가하기


## 26) 빵지순례 관심 삭제하기


## 27) 빵지순례 리뷰 쓰기
![27_CourseReviewAdd](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/27-Course-Review-Add.jpg?raw=true)

- 사용자가 빵지순례 글에 리뷰를 달 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 CourseController는 courseId, UserDetailsImpl, CourseReviewRequest DTO를 가지고 addCourseReview() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 addCourseReview(Long courseId, Long memId, CourseReviewRequest request) 메소드를 실행한다.
- 이 메소드는 먼저 courseRepository.findById()를 호출하여 리뷰를 작성할 코스가 실제로 존재하는지 확인한다.
- 만약 코스가 존재하지 않으면 예외를 발생시켜 중단한다. 코스의 존재가 확인되면, 그 다음으로 사용자의 권한을 확인한다.
- 권한이 없으면 예외를 발생시킨다.
- 권한이 존재한다면 ReviewService는 DTO의 내용을 포함하는 CourseReview 엔티티를 생성한다.
- 생성된 엔티티는 courseReviewRepository.save()를 호출하여 데이터베이스에 저장된다.
- 저장이 완료된 후, ReviewService는 저장된 CourseReview 엔티티 정보를 CourseReviewResponse DTO로 변환시키며, 이 DTO를 CourseController에게 전달한다.
- 이렇게 받은 최종 응답을 CourseController가 ResponseEntity에 담아 사용자에게 넘겨줌으로써 빵지순례글 리뷰 작성이 성공적으로 완료된다.

## 28) 빵지순례 리뷰 수정하기
![28_CourseReviewUpdate](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/28-Course-Review-Update.jpg?raw=true)

- 사용자가 빵지순례 글에 리뷰를 수정할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 CourseController는 @PathVariable에서 courseReviewId, UserDetailsImpl, 그리고 CourseReviewRequest DTO를 가지고 updateCourseReview() 메소드를 실행하여 ReviewService를 호출한다.
- ReviewService는 updateCourseReview() 메소드를 실행한다.
- 이 메소드는 먼저 courseReviewRepository.findById()를 호출해서 수정할 리뷰 엔티티가 데이터베이스에 존재하는지 확인한다.
- 만약 해당 리뷰가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 리뷰 엔티티를 찾은 뒤, ReviewService는 사용자가 조회된 리뷰 엔티티의 수정 권한을 가지고 있는지 검증한다.
- 권한이 일치하지 않으면 수정 권한이 없으므로 예외를 발생시킨다.
- 검증이 완료되면, courseReviewId를 이용해 데이터베이스 내 영속성 객체를 가져온 뒤 CourseReview.update() 메소드를 호출하여 조회된 리뷰 엔티티에 DTO의 새로운 내용을 반영한다.
- 수정이 완료된 후, ReviewService는 수정된 CourseReview 엔티티 정보를 CourseReviewResponse DTO로 변환시키며, 이 DTO를 CourseController에게 전달한다.
- 이렇게 받은 최종 응답을 CourseController가 ResponseEntity에 담아 사용자에게 넘겨줌으로써 루트 글 리뷰 수정이 완료된다.

## 29) 빵지순례 리뷰 삭제하기
![29_CourseReviewDelete](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/29-Course-Review-Delete.jpg?raw=true)

- 사용자가 빵지순례 글에 리뷰를 삭제할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 이 요청을 받은 CourseController는 courseReviewId, UserDetailsImpl을 이용하여 deleteCourseReview() 메소드를 실행하여서 ReviewService를 호출한다.
- ReviewService는 deleteCourseReview() 메소드를 실행한다.
- 이 메소드는 먼저 courseReviewRepository.findById()를 호출해서 삭제할 리뷰 엔티티가 데이터베이스에 존재하는지 확인한다.
- 만약 해당 리뷰가 존재하지 않으면 적절한 예외를 발생시켜 처리를 중단한다.
- 리뷰 엔티티를 찾은 뒤, ReviewService는 조회된 리뷰 엔티티의 삭제 권한이 사용자에게 있는지 확인하여 삭제 권한을 검증하는 중요한 역할을 수행한다.
- 만약 권한이 없으면 삭제를 진행하지 않고 예외를 발생시킨다.
- 권한 확인이 완료되면, courseReviewRepository.deleteById()를 호출하여 데이터베이스에서 해당 리뷰를 삭제한다.
- 삭제 작업이 성공적으로 완료된 후, ReviewService는 void를 반환하며 CourseController에게 성공을 알린다.
- 이렇게 전달받은 성공 응답을 CourseController가 ResponseEntity를 사용하여 사용자에게 넘겨줌으로써 루트 글 리뷰 삭제가 완료된다.

## 30) 빵지순례 만들기
![30_CourseAdd](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/30-Course-Add.jpg?raw=true)

- 사용자가 빵지순례 글을 작성할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다. 
- 요청을 받은 CourseController는 사용자 정보와 DTO를 가지고 createCourse() 메소드를 실행하여 CourseService를 호출한다.
- CourseService는 createCourse() 메소드를 실행한다.
- 이 메소드는 먼저 사용자에게 빵지순례글을 쓸 권한이 있는지 확인한다.
- 권한이 없으면 적절한 예외로 처리한다.
- 권한이 있다면, DTO의 정보를 사용하여 Course 엔티티를 생성한다.
- 그리고 courseRepository.save() 메소드를 이용해 데이터베이스에 저장하고 생성된 코스를 얻어낸다.
- CourseService는 CoursePartService의 createCourseParts() 메소드를 호출한다.
- CoursePartService는 createCourseParts() 메소드를 실행한다.
- 이 메소드는 반복문을 돌며 빵집이 존재하는지 검증한다.
- 빵집이 존재한다면 CoursePart 엔티티를 생성하고 coursePartRepository.save() 메소드를 이용해 데이터베이스에 저장한다.
- 저장이 완료되면 만들어진 CoursePart 객체들의 정보를 List<CoursePartResponse>에 담아 CourseService로 반환한다.
- CourseService는 CoursePartService에서 반환받은 List<CoursePartResponse>와 처음에 만든 Course의 정보를 하나의 CourseResponse DTO로 묶어 CourseController로 반환한다. 
- 이렇게 전달받은 최종 응답을 CourseController가 ResponseEntity에 담아 사용자에게 넘겨줌으로써 빵지순례 글 작성이 성공적으로 완료된다.

## 31) 빵지순례 수정하기
![31_CourseUpdate](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/31-Course-Update.jpg?raw=true)

- 사용자가 빵지순례 글을 수정할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 CourseController는 courseId와 사용자 정보, DTO를 가지고 updateCourse() 메소드를 실행하여 CourseService를 호출한다.
- CourseService는 updateCourse() 메소드를 실행한다.
- 이 메소드는 먼저 courseRepository.findById()를 호출하여 courseId에 해당하는 코스 엔티티를 데이터베이스에서 불러온다.
- 코스가 존재하지 않으면 적절한 예외를 발생시킨다.
- 코스를 불러온 뒤, CourseService는 사용자의 수정 권한을 검증한다.
- 권한이 확인되면, 불러온 코스 엔티티의 Course.update() 메소드를 사용하여 Course 내용을 변경한다.
- CourseService는 CoursePartService의 updateCourseParts()를 호출한다.
- CoursePartService는 updateCourseParts() 메소드를 실행한다.
- 이 메소드는 먼저 CoursePartRepository.deleteAllByCourseId()를 호출하여 해당 Course에 연결된 모든 기존 CoursePart를 일괄 삭제한다.
- 삭제 후에는 CoursePartService.createCourseParts()를 호출하여 새로운 CoursePart를 일괄 저장하는 작업을 진행한다.
- createCourseParts가 반환한 List<CoursePartResponse>를 CourseService에게 그대로 반환한다.
- CourseService는 CoursePartService가 반환한 List<CoursePartResponse>와 바뀐 Course의 정보를 묶어 CourseResponse DTO로 CourseController에 반환한다.
- 이렇게 전달받은 응답을 CourseController가 ResponseEntity에 담아 사용자에게 넘겨줌으로써 루트 글 수정이 완료된다.

## 32) 빵지순례 삭제하기
![32_CourseDelete](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/sequence/32-Course-Delete.jpg?raw=true)

- 사용자가 빵지순례 글을 삭제할 수 있게 해주는 Use Case를 sequence diagram으로 나타낸 것이다.
- 요청을 받은 CourseController는 courseId와 사용자 정보를 가지고 deleteCourse() 메소드를 실행하여 CourseService를 호출한다.
- CourseService는 deleteCourse() 메소드를 실행한다.
- 이 메소드는 먼저 사용자에게 삭제 권한이 있는지 확인한다.
- 만약 권한이 없으면 삭제를 진행하지 않고 예외를 발생시킨다.
- 권한 확인이 완료되면, CourseService는 코스에 종속된 모든 관련 데이터를 삭제하도록 한다.
- courseReviewRepository.deleteAllByCourseId(), favoriteCourseRepository.deleteAllByCourseId(), coursePartRepository.deleteAllByCourseId()를 순차적으로 호출하여 모두 데이터베이스에서 삭제한다.
- courseRepository.deleteById()를 호출하여 Course를 최종적으로 삭제한다.
- 성공적인 삭제 후 CourseService는 void를 반환하며 CourseController에게 성공을 알린다.
- 이렇게 전달받은 응답을 CourseController가 최종적으로 사용자에게 넘겨줌으로써 삭제가 완료된다.

## 33) 닉네임 변경하기


## 34) 관심 가게 목록 보기


## 35) 관심 빵지순례 목록 보기


## 36) 내가 쓴 가게 리뷰 보기


## 37) 내가 쓴 메뉴 리뷰 보기


## 38) 내가 쓴 빵지순례 리뷰 보기


## 39) 내가 쓴 빵지순례 리뷰

