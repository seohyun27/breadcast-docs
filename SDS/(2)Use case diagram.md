# 2. Use case analysis

## [ Revision history ]

| Revision date | Version \# | Description | Author |
| :--- | :--- | :--- | :--- |
| 2025/00/00 | 1.00 | 초기 버전 작성 | 팀 전원 |
| 2025.09.09 | 1.01 | 규격 통일 및 세부 사항 추가 | 김서연 |
| 2025.09.23 | 1.10 | UI를 바탕으로 수정 | |
| 2025.09.23 | 1.11 | 이름 통일 및 전체 수정 | |

***
## Use case diagram
(이미지와 함께 설명을 넣어주세요)

***

## Use case description

### Use case #1: 회원 가입하기
<<<<<<< HEAD
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 시스템에 회원 정보를 등록하여 시스템의 각종 기능을 추가로 이용할 자격을 얻는 기능
**Scope** BreadCast: 회원 정보
**Level** User level
**Author** 김서연
**Last Update** 2025.08.12.
**Primary Actor** 사용자
**Preconditions** 회원가입 페이지에 접속한다.
**Trigger** 사용자가 회원가입 버튼을 누른다.
**Success Post Condition** 사용자의 회원가입 정보가 DB에 존재한다.
**Failed Post Condition** 사용자의 회원가입 정보가 DB에 존재하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 회원가입을 요청한다.
	1.	시스템이 사용자에게 회원가입 폼을 출력한다.
	2.	사용자가 회원가입 폼을 통해 필요한 정보를 시스템에 제출한다.
	3.	시스템이 사용자의 정보를 DB에 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	2.	2a.	사용자가 존재할 수 없는 정보나 부족한 정보를 제출한다.
			2a1. 화면에 오류 메시지를 출력하여 사용자에게 부적합한 제출임을 알린다.
			2a2. 메인 시나리오 2로 돌아간다.
		2b.	사용자가 중복된 정보를 제출한다.
			2b1. 화면에 오류 메시지를 출력하여 사용자에게 이미 존재하는 회원임을 알린다.
			2b2. 로그인하기(\#2) 시나리오로 이동한다.
		2c.	사용자가 회원가입을 중단한다.
			2c1. 사용자가 회원가입을 시도하기 전 화면으로 돌아간다.
	3.	3a.	시스템이 서버 오류 등으로 사용자 정보를 DB에 저장하지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 회원가입 처리 실패를 알린다.
			3a2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 0.5s$
**Frequency** 100회 / 1일
**Concurrency** 5개 요청
**Due Date** 2025.09.20.
***

### Use case #2: 로그인하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 로그인 상태를 획득하여 시스템의 각종 기능을 추가로 이용할 수 있는 기능
**Scope** BreadCast: 회원 정보
**Level** User level
**Author** 김서연
**Last Update** 2025.08.12.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 회원가입을 한 상태
**Trigger** 사용자가 로그인 버튼을 누른다.
**Success Post Condition** 사용자가 로그인 상태이다.
**Failed Post Condition** 사용자가 로그아웃 상태이다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 로그인을 요청한다.
	1.	시스템이 사용자에게 로그인 폼을 출력한다.
	2.	사용자가 로그인 폼을 통해 필요한 정보를 시스템에 제출한다. 이때, 사용자는 ‘로그인 상태 유지’를 선택할 수 있다.
	3.	시스템이 DB에서 유효한 회원 정보를 확인한다.
	4.	시스템이 사용자를 로그인 상태로 할당한다.
#### Extension Scenario
*Step.	Branching Action*
	2.	2a.	사용자가 부족한 정보를 제출한다.
			2a1. 화면에 오류 메시지를 출력하여 사용자에게 부적합한 제출임을 알린다.
			2a2. 메인 시나리오 2로 돌아간다.
		2b.	사용자가 로그인을 중단한다.
			2b1. 사용자가 로그인을 시도하기 전 화면으로 돌아간다.
	3.	3a.	사용자가 제출한 정보를 DB에서 찾지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 잘못된 정보 제출임을 알린다.
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	시스템이 서버 오류 등으로 로그인 정보를 처리하지 못한다.
			3b1. 화면에 오류 메시지를 출력하여 로그인 처리 실패를 알린다.
			3b2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 2s$
**Frequency** 1000회 / 1일
**Concurrency** 30개 요청
**Due Date** 2025.09.20.
***

### Use case #3: 로그아웃하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 로그인 상태를 해제하는 기능
**Scope** BreadCast: 회원 정보
**Level** User level
**Author** 김서연
**Last Update** 2025.08.12.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인을 한 상태
**Trigger** 사용자가 로그아웃 버튼을 누른다.
**Success Post Condition** 사용자가 로그아웃 상태이다.
**Failed Post Condition** 사용자가 로그인 상태이다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 로그아웃을 요청한다.
	1.	시스템이 사용자에게 할당한 로그인 상태를 해제한다.
#### Extension Scenario
*Step.	Branching Action*
	(확장 시나리오 없음)
#### RELATED INFORMATION
**Performance** $\le 0.2s$
**Frequency** 1회 / 1일
**Concurrency** 1개 요청
**Due Date** 2025.09.20.
***

### Use case #4: 회원 탈퇴하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 시스템에서 회원 정보를 삭제하는 기능
**Scope** BreadCast: 회원 정보
**Level** User level
**Author** 김서연
**Last Update** 2025.08.12.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 회원가입을 한 상태
**Trigger** 사용자가 회원 탈퇴 버튼을 누른다.
**Success Post Condition** 사용자의 회원가입 정보가 DB에 존재하지 않는다.
**Failed Post Condition** 사용자의 회원가입 정보가 DB에 존재한다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 회원 탈퇴를 요청한다.
	1.	시스템이 회원 탈퇴 의사 확인 메시지를 출력한다.
	2.	사용자가 시스템에 회원 탈퇴 의사를 전달한다.
	3.	시스템이 사용자의 정보를 DB에서 삭제한다.
#### Extension Scenario
*Step.	Branching Action*
	2.	2b.	사용자가 회원 탈퇴를 중단한다.
			2b1. 사용자가 회원 탈퇴를 시도하기 전 화면(마이페이지)으로 돌아간다.
	3.	3a.	시스템이 서버 오류 등으로 사용자 정보를 DB에서 삭제하지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 회원 탈퇴 처리 실패를 알린다.
			3a2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 0.5s$
**Frequency** 1회 / 1주
**Concurrency** 1개 요청
**Due Date** 2025.09.20.
***

### Use case #5: 가게 검색하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 가게명 또는 메뉴명(빵 이름)으로 가게를 검색하는 기능
(검색 결과는 지도와 리스트로 표시되며, 선택 시 가게 정보 보기(#7) 기능을 통해 가게 상세 페이지로 이동할 수 있음)
**Scope** BreadCast: 가게 탐색
**Level** User level
**Author** 노은재
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** -
**Trigger** 사용자가 검색창에 접근한다. / 사용자가 검색을 실행한다. / 사용자가 홈 화면에 접근한다.
**Success Post Condition** 조건에 맞는 가게 목록이 지도와 리스트 형태로 표시된다. (홈 화면에서는 지도 형태로만 표시된다.)
**Failed Post Condition** 조건에 맞는 가게 목록이 표시되지 않는다. / 가게 목록이 지도나 리스트 형태로 표시되지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 가게 검색을 요청한다.
	1.	시스템은 검색 화면을 출력한다.
	2.	사용자가 검색 화면에 가게명 및 메뉴명을 입력하여 제출한다.
	3.	시스템은 DB에서 조건에 맞는 가게 목록을 조회한다.
	4.	시스템은 결과를 마커 표시 지도와 리스트 형태로 출력한다.
#### Extension Scenario
*Step.	Branching Action*
	2.	2a.	사용자가 검색을 중단한다.
			2a1. 시스템은 사용자가 가게 검색에 접근하기 이전 화면으로 돌아간다.
	3.	3a.	검색 결과가 존재하지 않는다.
			3a1. 오류 화면을 출력한다: ‘검색 결과가 없습니다.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	시스템이 서버 오류 등으로 가게 정보를 DB에서 찾지 못한다.
			3b1. 오류 메시지를 출력한다: ‘검색 결과를 불러올 수 없습니다.’
			3b2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
	4.	4a.	시스템이 결과를 지도 형태로 출력하는 데 실패한다.
			4a1. 오류 화면을 출력한다: ‘지도를 불러올 수 없습니다.’
			4a2. 시스템은 오류 화면과 함께 리스트 형태만 이용하여 가게 목록을 출력한다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 10회 / 1명, 1일
**Concurrency** 150개 요청
**Due Date** 2025.10.10.
***

### Use case #6: 가게 정렬하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 검색 결과 또는 가게 목록을 리뷰순, 인기순(스크랩순) 중 한 기준을 선택하여 정렬하는 기능
(선택한 기준에 따라 가게 리스트가 재정렬되며, 필요한 경우 지도 표시 순서에도 반영함)
**Scope** BreadCast: 가게 탐색
**Level** User level
**Author** 노은재
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 가게 검색하기(#5) 기능을 실행하여 가게 목록(검색 결과 또는 기본 목록)을 화면에 출력한 상태
**Trigger** 사용자가 정렬 기준(리뷰순, 인기순) 중 하나를 선택할 때.
**Success Post Condition** 시스템이 선택한 정렬 기준에 맞춰 가게 목록을 출력한다. (필요한 경우 지도 표시 순서에도 반영한다.)
**Failed Post Condition** 시스템이 선택한 정렬 기준과 다른 가게 목록을 출력한다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 검색 화면 UI에서 원하는 정렬 기준(리뷰순, 인기순)을 선택하여 시스템에 가게 정렬을 요청한다. (기본 선택은 인기임)
	1.	시스템은 선택된 기준을 기반으로 가게 목록을 재정렬한다.
	2.	시스템은 선택한 정렬 기준과 재정렬된 결과를 화면에 출력한다. (필요 시 지도 표시 순서도 갱신함)
#### Extension Scenario
*Step.	Branching Action*
	2.	2a.	시스템이 결과를 지도 형태로 출력하는 데 실패한다.
			2a1. 오류 화면을 출력한다: ‘지도를 불러올 수 없습니다.’
			2a2. 시스템은 오류 화면과 함께 리스트 형태만 이용하여 가게 목록을 출력한다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 10회 / 1명, 1일
**Concurrency** 100개 요청
**Due Date** 2025.10.10.
***

### Use case #7: 가게 정보 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 선택한 빵집의 정보를 확인하는 기능
(확인할 수 있는 정보로는 위치, 영업 여부, 연락처(전화번호), 사이트 주소, 관심 여부 등이 있음)
**Scope** BreadCast: 가게 탐색: 가게 홈 페이지 탐색
**Level** User level
**Author** 이지원
**Last Update** 2025.08.12.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** -
**Trigger** 사용자가 가게 상세페이지에 최초 접근한다. / 사용자가 가게 상세페이지에서 ‘홈’ 탭을 누른다.
**Success Post Condition** 시스템이 사용자가 원하는 가게 정보 화면을 출력한다.
**Failed Post Condition** 시스템이 사용자에게 가게 정보 화면을 출력하지 않는다. / 시스템이 사용자가 원하지 않는 가게 정보 화면을 출력한다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 가게 정보 화면에 접근한다.
	Sa.	사용자는 다음 기능을 통해 가게 상세페이지에 최초 접근할 수 있다.
		가게 검색하기(#5), 빵지순례 상세 페이지 보기(#24), 관심 가게 목록 보기(#34)
	1.	시스템이 사용자가 확인하고자 하는 가게 데이터를 DB에서 가져온다.
	2.	시스템이 화면에 가게 정보 페이지를 출력한다.
	3.	사용자는 위치, 연락처를 복사하거나, 사이트 주소를 눌러 외부 사이트로 이동할 수 있다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 가게 정보 화면에 접근하며 사용한 기능에서 오류가 발생한다.
		Sa1. 접근한 기능의 시나리오에 따라 오류를 처리한다.
		Sa2. 메인 시나리오 S로 돌아간다.
	1.	1a.	사용자가 선택한 가게 ID가 존재하지 않는다.
			1a1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			1a2. 사용자가 유효한 이전 화면으로 돌아간다.
		1b.	시스템이 서버 오류 등으로 가게 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘페이지를 찾을 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 페이지 새로고침 버튼을 노출한다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 15-20회 / 1명, 1일 (피크: 퇴근 후, 주말)
**Concurrency** 동일한 정보에 대해 50개 요청
**Due Date** 2025.09.20.
***

### Use case #8: 가게 관심 추가하기(스크랩하기)
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 관심이 있는 가게의 스크랩 버튼을 눌러 마이페이지에서 관심 가게 목록 보기(#34) 기능을 통해 볼 수 있게 저장하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 관심
**Level** User level
**Author** 이지원
**Last Update** 2025.08.12.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 가게 상세 페이지에 접근한 상태
**Trigger** 사용자가 가게 상세 페이지에서 스크랩 버튼을 누른다. (스크랩 버튼의 예: 스크랩(하트)모양)
**Success Post Condition** 사용자가 누른 스크랩 버튼의 모양이 변한다. / 사용자의 마이페이지 내 ‘관심 가게 목록’에 스크랩한 가게가 추가된다.
**Failed Post Condition** 사용자가 누른 스크랩 버튼의 모양이 유지된다. / 사용자의 마이페이지 내 ‘관심 가게 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 ‘관심 가게 목록’에 추가하고 싶은 가게의 상세 페이지에서 스크랩 버튼을 누른다.
	1.	시스템은 관련 DB에 사용자 계정과 관심 가게 ID를 저장한다.
	2.	시스템은 스크랩 버튼의 모양을 변경한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 관심을 추가할 가게 ID가 존재하지 않는다.
			1a1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			1a2. 사용자가 유효한 이전 화면으로 돌아간다.
		1b.	시스템이 서버 오류 등으로 정보를 DB에 저장하지 못한다.
			1b1. 화면에 오류 메시지를 출력해 사용자에게 스크랩 실패를 알린다.
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 100ms$
**Frequency** 5-10회 / 1명, 1일
**Concurrency** 150개 요청
**Due Date** 2025.09.30.
***

### Use case #9: 가게 관심 삭제하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 특정 가게의 스크랩을 해제하고 마이페이지의 ‘관심 가게 목록’에서 해당 가게를 삭제하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 관심
**Level** User level
**Author** 이지원
**Last Update** 2025.08.17.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 가게 상세 페이지에 접근한 상태/마이페이지의 ‘관심 가게 목록’에 접근한 상태 / 사용자가 해당 가게를 스크랩한 상태
**Trigger** 사용자가 가게 상세 페이지에서 활성화된 스크랩 버튼을 누른다./사용자가 마이페이지에서 관심을 삭제할 가게 목록을 선택한다.
**Success Post Condition** 특정 가게의 스크랩 버튼의 모양과 상태가 기본형으로 변한다. / 사용자의 마이페이지 내 ‘관심 가게 목록’에서 스크랩한 가게가 삭제된다.
**Failed Post Condition** 특정 가게의 스크랩 버튼 모양과 상태가 변한 상태로 유지된다. / 사용자의 마이페이지 내 ‘관심 가게 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 가게 관심 삭제를 요청한다.
	1.	시스템은 관련 DB에서 사용자 계정과 관심 가게 ID 쌍을 찾아 삭제한다.
	2.	시스템은 스크랩 버튼의 모양과 상태를 변경한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 관심을 삭제할 가게 ID가 존재하지 않는다.
			1a1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			1a2. 사용자가 유효한 이전 화면으로 돌아간다.
		1b.	시스템이 서버 오류 등으로 정보를 DB에서 삭제하지 못한다.
			1b1. 화면에 오류 메시지를 출력해 사용자에게 스크랩 삭제 실패를 알린다.
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 500ms$
**Frequency** 0-3회 / 1명, 1일
**Concurrency** 30개 요청
**Due Date** 2025.09.30.
***

### Use case #10: 가게 리뷰 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 선택한 빵집의 리뷰 목록과 내용을 확인하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 리뷰
**Level** User level
**Author** 이지원
**Last Update** 2025.08.12.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** -
**Trigger** 사용자가 내가 쓴 가게 리뷰 보기(#36) 기능을 통해 가게 상세페이지에 접근한다. / 사용자가 가게 상세페이지에서 ‘리뷰’ 탭을 누른다.
**Success Post Condition** 시스템이 화면에 사용자가 원하는 가게의 모든 리뷰 목록과 내용을 출력한다.
**Failed Post Condition** 시스템이 화면에 가게의 모든 리뷰 목록과 내용을 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 가게 리뷰 화면에 접근한다.
	1.	시스템이 사용자가 확인하고자 하는 가게의 리뷰 데이터를 DB에서 가져온다.
	2.	시스템은 리뷰 목록과 요약을 화면에 출력한다.
	3.	사용자는 상세 리뷰를 확인할 수 있다.
		3a.	특정 리뷰를 펼쳐 상세 내용 및 사진 썸네일을 확인한다.
			3a1. 사진 썸네일을 클릭해 사진을 크게 확인한다.
		3b.	펼친 리뷰를 다시 눌러 요약으로 전환한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	리뷰 목록을 출력할 가게가 존재하지 않는다.
		Sa1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
		Sa2. 사용자는 유효한 이전 화면으로 돌아간다.
	1.	1a.	가게의 리뷰 데이터가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘아직 리뷰가 없습니다.’
		1b.	시스템이 서버 오류 등으로 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘페이지를 찾을 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 페이지 새로고침 버튼을 노출한다.
	3.	3a.	사진 뷰어 오류나 이미지 로딩 실패 등으로 사진을 출력할 수 없다.
			3a1. 오류 메시지를 출력한다: ‘이미지를 표시할 수 없습니다.’
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 4-8회 / 1명, 1일
**Concurrency** 150개 요청
**Due Date** 2025.09.30.
***

### Use case #11: 가게 리뷰 쓰기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 방문한 가게 리뷰를 작성하고 게시하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 리뷰
**Level** User level
**Author** 이지원
**Last Update** 2025.08.12.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 가게 상세 페이지의 리뷰 목록에 접근한 상태
**Trigger** 사용자가 리뷰 목록에서 리뷰 작성 버튼을 누른다.
**Success Post Condition** 특정 가게의 리뷰 목록에 새로 작성한 리뷰가 추가된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’에 해당 리뷰가 추가된다.
**Failed Post Condition** 특정 가게의 리뷰 목록이 유지된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 리뷰 작성을 요청한다.
	1.	시스템이 사용자에게 리뷰 작성 폼을 출력한다.
	2.	사용자가 리뷰 작성 폼을 따라 리뷰를 작성한다. 사용자는 사진을 업로드할 수 있다.
	3.	사용자가 리뷰 작성을 완료하고 시스템에 리뷰 게시를 요청한다.
	4.	시스템이 DB에 리뷰를 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 리뷰 작성을 중단한다.
			2a1. 사용자가 리뷰 작성을 시도하기 전 화면(가게 리뷰 목록)으로 돌아간다.
	3.	3a.	필수 입력 값이 누락된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	사용자가 리뷰를 작성한 가게가 존재하지 않는다.
			3b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			3b2. 사용자가 유효한 이전 화면으로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 리뷰 정보를 저장하지 못한다.
			4a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 3으로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 2s$
**Frequency** 2~4회 / 1명, 1주
**Concurrency** 10개 요청
**Due Date** 2025.09.30.
***

### Use case #12: 가게 리뷰 수정하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 기존에 작성한 가게 리뷰를 수정하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 리뷰
**Level** User level
**Author** 이지원
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 가게 상세 페이지의 리뷰 목록에 접근한 상태/마이페이지의 ‘내가 쓴 리뷰 목록’에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 가게 상세 페이지 또는 마이페이지의 ‘내가 쓴 리뷰 목록’에서 본인이 작성한 리뷰에 있는 '수정' 버튼을 클릭한다.
**Success Post Condition** 기존에 작성한 리뷰 내용이 수정된다.
**Failed Post Condition** 기존에 작성한 리뷰 내용이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 리뷰 수정을 요청한다.
	1.	시스템이 사용자에게 기존 리뷰 내용이 채워진 리뷰 작성 폼을 출력한다.
	2.	사용자가 리뷰 작성 폼에 따라 리뷰를 수정한다.
	3.	사용자가 리뷰 수정을 완료하고 시스템에 수정 리뷰 게시를 요청한다.
	4.	시스템이 DB에 수정한 리뷰를 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 리뷰 수정을 중단한다.
			2a1. 사용자가 리뷰 수정을 시도하기 전 화면(가게 리뷰 목록)으로 돌아간다.
	3.	3a.	필수 입력 값이 누락된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	사용자가 리뷰를 수정한 가게가 존재하지 않는다.
			3b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			3b2. 사용자가 유효한 이전 화면으로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 리뷰 정보를 저장하지 못한다.
			4a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 1–2회 / 1명, 1주
**Concurrency** 10개 요청
**Due Date** 2025.09.30.
***

### Use case #13: 가게 리뷰 삭제하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 기존에 작성한 가게 리뷰를 삭제하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 리뷰
**Level** User level
**Author** 이지원
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 가게 상세 페이지의 리뷰 목록에 접근한 상태/마이페이지의 ‘내가 쓴 리뷰 목록’에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 가게 상세 페이지의 리뷰 목록에서 본인이 작성한 리뷰에 접근해 ‘삭제’ 버튼을 누른다.
**Success Post Condition** 특정 가게의 리뷰 목록에서 해당 리뷰가 삭제된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’에서 해당 리뷰가 삭제된다.
**Failed Post Condition** 특정 가게의 리뷰 목록이 유지된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 리뷰 삭제를 요청한다.
	1.	시스템이 리뷰 삭제 의사 확인 메시지를 출력한다.
	2.	사용자가 시스템에 리뷰 삭제 의사를 전달한다.
	3.	시스템이 해당 리뷰를 DB에서 삭제한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 리뷰 삭제를 중단한다.
			2a1. 사용자가 리뷰 삭제를 시도하기 전 화면(가게 리뷰 목록)으로 돌아간다.
		2b.	사용자가 리뷰를 삭제한 가게가 존재하지 않는다.
			2b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			2b2. 사용자는 유효한 이전 화면으로 돌아간다.
	3.	3a.	시스템이 서버 오류 등으로 DB에서 리뷰 정보를 삭제하지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			3a2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 1–2회 / 1명, 1달
**Concurrency** 5개 요청
**Due Date** 2025.09.30.
***

### Use case #14: 가게 메뉴 목록 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 특정 가게의 전체 메뉴 리스트를 확인하는 기능
(각 메뉴에서 요약 정보로 이름, 가격, 썸네일 이미지, 별점, 메뉴 정보를 확인할 수 있고, 사용자는 메뉴를 클릭해 메뉴 리뷰 보기(#15) 기능 사용가능)
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색
**Level** User level
**Author** 노은재
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 가게 상세 페이지에 접근한 상태
**Trigger** 사용자가 가게 상세 페이지에서 ‘메뉴’ 탭을 클릭한다.
**Success Post Condition** 시스템이 해당 가게의 모든 메뉴 목록을 화면에 출력한다. / 사용자는 개별 메뉴를 클릭해 메뉴 상세 페이지로 이동할 수 있다.
**Failed Post Condition** 시스템이 해당 가게의 모든 메뉴 목록을 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 가게 상세 페이지의 ‘메뉴’ 탭을 클릭한다.
	1.	시스템이 사용자가 확인하고자 하는 가게의 메뉴 정보를 DB에서 가져온다.
		(메뉴의 이름, 가격, 썸네일 이미지, 별점 등)
	2.	시스템은 메뉴 목록을 화면에 출력한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 메뉴 목록을 찾으려는 가게가 존재하지 않는다.
		Sa1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
		Sa2. 사용자가 유효한 이전 화면으로 돌아간다.
	1.	1a.	시스템이 서버 오류 등으로 메뉴 목록을 DB에서 찾지 못한다.
			1a1. 오류 화면을 출력한다: ‘메뉴 목록을 불러올 수 없습니다.’
			1a2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	메뉴 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
#### RELATED INFORMATION
**Performance** $\le 2s$
**Frequency** 10-15회 / 1명, 1일
**Concurrency** 100개 요청
**Due Date** 2025.09.30.
***

### Use case #15: 메뉴 리뷰 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 특정 메뉴의 리뷰와 함께 상세 정보를 확인하는 기능
(상세 정보에는 메뉴 이름, 가격, 평균 별점, 설명이 포함되며, 리뷰 작성·수정·삭제 기능으로 이동할 수 있음)
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 메뉴
**Level** User level
**Author** 노은재
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 가게 상세 페이지에서 가게 메뉴 목록에 접근한 상태
**Trigger** 사용자가 가게 메뉴 목록에서 특정 메뉴의 메뉴 리뷰를 누른다.
**Success Post Condition** 사용자가 원하는 메뉴의 리뷰와 상세 정보가 화면에 출력된다.
**Failed Post Condition** 사용자가 원하는 메뉴의 리뷰와 상세 정보가 화면에 출력되지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 메뉴 리뷰와 정보 보기를 요청한다.
	1.	시스템이 사용자가 확인하고자 하는 메뉴 리뷰와 정보를 DB에서 가져온다.
		(이름, 가격, 평균 별점, 설명 등)
	2.	시스템은 메뉴 리뷰와 정보를 화면에 출력한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 메뉴 리뷰와 정보를 찾으려는 가게가 존재하지 않는다.
		Sa1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
		Sa2. 사용자가 유효한 이전 화면으로 돌아간다.
	1.	1a.	시스템이 서버 오류 등으로 메뉴 리뷰와 정보를 DB에서 찾지 못한다.
			1a1. 오류 화면을 출력한다: ‘메뉴 정보를 불러올 수 없습니다.’
			1a2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	메뉴 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 25-30회 / 1명, 1일
**Concurrency** 200개 요청
**Due Date** 2025.09.30.
***

### Use case #16: 메뉴 리뷰 쓰기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 특정 메뉴에 대한 리뷰를 작성하는 기능 (리뷰에는 별점과 텍스트를 입력할 수 있음)
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 메뉴
**Level** User level
**Author** 노은재
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 메뉴 정보 페이지에 접근한 상태
**Trigger** 사용자가 메뉴 정보 페이지에서 리뷰 작성 버튼을 누른다.
**Success Post Condition** 특정 메뉴의 리뷰 목록에 새로 작성한 리뷰가 추가된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’에 해당 리뷰가 추가된다.
**Failed Post Condition** 특정 메뉴의 리뷰 목록이 유지된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’이 유지된다.
#### Main Success Scenario
*Step. Action*
    S.  사용자가 시스템에게 리뷰 작성을 요청한다.
    1.  시스템이 사용자에게 리뷰 작성 폼을 출력한다. (리뷰 작성 폼: 별점, 텍스트 입력란)
    2.  사용자가 리뷰 작성 폼을 따라 가게 리뷰를 작성한다.
    3.  사용자가 리뷰 작성을 완료하고 시스템에 리뷰 게시를 요청한다.
    4.  시스템이 DB에 리뷰를 저장한다.
#### Extension Scenario
*Step. Branching Action*
    S.  Sa.사용자가 로그아웃 상태이다.
            Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
    2.  2a. 사용자가 리뷰 작성을 중단한다.
            2a1. 사용자가 리뷰 작성을 시도하기 전 화면(메뉴 상세페이지)으로 돌아간다.
    3.  3a. 필수 입력 값(별점, 텍스트 등)이 누락된 상태이다.
            3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
            3a2. 메인 시나리오 2로 돌아간다.
        3b. 사용자가 리뷰를 작성한 가게가 존재하지 않는다.
            3b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
            3b2. 사용자가 유효한 이전 화면으로 돌아간다.
    4.  4a. 시스템이 서버 오류 등으로 DB에 리뷰 정보를 저장하지 못한다.
            4a1. 화면에 오류 메시지를 출력한다: ‘리뷰를 저장할 수 없습니다.’
            4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 3으로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 0.5s$
**Frequency** 10-15회 / 1명, 1주
**Concurrency** 100개 요청
**Due Date** 2025.09.30.
***

### Use case #17: 메뉴 리뷰 수정하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 기존에 작성한 메뉴 리뷰를 수정하는 기능 (별점과 텍스트를 수정할 수 있음)
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 메뉴
**Level** User level
**Author** 노은재
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 메뉴 정보 페이지에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 메뉴 정보 페이지에서 본인이 작성한 리뷰에 접근해 '수정' 버튼을 누른다.
**Success Post Condition** 기존에 작성한 리뷰 내용이 수정된다.
**Failed Post Condition** 기존에 작성한 리뷰 내용이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 리뷰 수정을 요청한다.
	1.	시스템이 사용자에게 기존 리뷰 내용이 채워진 리뷰 작성 폼을 출력한다.
		(리뷰 작성 폼: 별점, 텍스트 입력란)
	2.	사용자가 리뷰 작성 폼에 따라 리뷰를 수정한다.
	3.	사용자가 리뷰 수정을 완료하고 시스템에 수정 리뷰 게시를 요청한다.
	4.	시스템이 DB에 수정한 리뷰를 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 리뷰 수정을 중단한다.
			2a1. 사용자가 리뷰 수정을 시도하기 전 화면(메뉴 상세페이지)으로 돌아간다.
	3.	3a.	필수 입력 값이 누락된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	사용자가 리뷰를 수정한 가게가 존재하지 않는다.
			3b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			3b2. 사용자가 유효한 이전 화면으로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 리뷰 정보를 저장하지 못한다.
			4a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 2-3회 / 1명, 1주
**Concurrency** 10개 요청
**Due Date** 2025.09.30.
***

### Use case #18: 메뉴 리뷰 삭제하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 본인이 작성한 메뉴 리뷰를 삭제하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 메뉴
**Level** User level
**Author** 노은재
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 메뉴 정보 페이지에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 가게 상세 페이지의 리뷰 목록에서 본인이 작성한 리뷰에 접근해 ‘삭제’ 버튼을 누른다.
**Success Post Condition** 특정 메뉴의 리뷰 목록에서 해당 리뷰가 삭제된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’에서 해당 리뷰가 삭제된다.
**Failed Post Condition** 특정 메뉴의 리뷰 목록이 유지된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 리뷰 삭제를 요청한다.
	1.	시스템이 리뷰 삭제 의사 확인 메시지를 출력한다.
	2.	사용자가 시스템에 리뷰 삭제 의사를 전달한다.
	3.	시스템이 해당 리뷰를 DB에서 삭제한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 리뷰 삭제를 중단한다.
			2a1. 사용자가 리뷰 삭제를 시도하기 전 화면(메뉴 상세페이지)으로 돌아간다.
		2b.	사용자가 리뷰를 삭제한 가게가 존재하지 않는다.
			2b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			2b2. 사용자는 유효한 이전 화면으로 돌아간다.
	3.	3a.	시스템이 서버 오류 등으로 DB에서 리뷰 정보를 삭제하지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			3a2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 2s$
**Frequency** 5–10회 / 1명, 1달
**Concurrency** 5개 요청
**Due Date** 2025.09.30.
***

### Use case #19: 제보 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 등록된 제보 목록을 최신순으로 확인하는 기능 (로그인 없이도 접근 가능, 자신이 한 제보는 수정/삭제 가능)
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 제보
**Level** User Level
**Author** 김서현
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 가게 상세 페이지에 접근한 상태
**Trigger** 사용자가 가게 상세 페이지에서 ‘제보’ 탭을 클릭한다.
**Success Post Condition** 시스템이 해당 가게의 유효 기간 내 모든 제보글을 최신순(등록 날짜순)으로 화면에 출력한다.
(사용자 본인이 작성한 제보에는 수정/삭제 버튼이 함께 표시됨)
**Failed Post Condition** 시스템이 해당 가게의 유효 기간 내 모든 제보글을 최신순(등록 날짜순)으로 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 가게 상세 페이지에서 ‘제보’ 탭을 클릭한다.
	1.	시스템이 사용자가 확인하고자 하는 가게의 제보 목록을 DB에서 가져온다. (유효 기간 내 정보, 최신순, 날짜)
	2.	시스템은 메뉴 목록을 화면에 출력한다. (무한 스크롤 방식 적용, 사용자가 작성한 제보글에는 수정/삭제 버튼을 함께 표시)
	3.	사용자는 스크롤을 수행하여 제보를 계속 확인할 수 있다.
#### Extension Scenario
*Step.	Branching Action*
	1.	1a.	현재 등록된 제보가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘지금은 제보가 없습니다.’
		1b.	시스템이 서버 오류 등으로 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘페이지를 찾을 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 페이지 새로고침 버튼을 노출한다.
	2.	2a.	사용자가 수정 버튼을 클릭한다.
			2a1. 시스템은 제보 수정하기(#21) 기능을 실행한다.
		2b.	사용자가 삭제 버튼을 클릭한다.
			2b1. 시스템은 제보 삭제하기(#22) 기능을 실행한다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 5-10회 / 1명, 1일
**Concurrency** 150개 요청
**Due Date** 2025.09.30.
***

### Use case #20: 제보하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 가게 관련 제보를 작성하여 등록하는 기능. (제보에는 텍스트만 입력하며, 시스템이 자동으로 등록 날짜 및 시각을 저장함)
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 제보
**Level** User Level
**Author** 김서현
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 가게 상세 페이지의 제보 페이지에 접근한 상태
**Trigger** 사용자가 제보 페이지에서 제보하기 버튼을 누른다.
**Success Post Condition** 특정 가게의 제보 목록에 새로 작성한 제보가 추가된다.
**Failed Post Condition** 특정 가게의 제보 목록이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 제보 페이지에서 제보하기 버튼을 누른다.
	1.	시스템이 제보 작성 폼을 출력한다.
	2.	사용자가 제보 작성 폼을 따라 제보를 작성한다.
	3.	사용자가 제보 작성을 완료하고 시스템에 게시를 요청한다.
	4.	시스템은 DB에 제보 내용과 등록 날짜 및 시각을 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 제보 작성을 중단한다.
			2a1. 사용자가 제보 작성을 시도하기 전 화면(제보 목록)으로 돌아간다.
	3.	3a.	필수 입력 값이 누락되거나 비허용 값이 입력된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	사용자가 제보를 작성한 가게가 존재하지 않는다.
			3b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			3b2. 사용자가 유효한 이전 화면으로 돌아간다.
	4.	4a.	시스템이 현재 시각을 불러오지 못한다.
			4a1. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 가장 최근 제보의 시각을 함께 저장한다.
		4b.	시스템이 서버 오류 등으로 DB에 제보 정보를 저장하지 못한다.
			4b1. 화면에 오류 메시지를 출력하여 제보 정보 처리 실패를 알린다.
			4b2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 3으로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 25-30회 / 1명, 1주
**Concurrency** 150개 요청
**Due Date** 2025.09.30.
***

### Use case #21: 제보 삭제하기
#### GENERAL CHARACTERISTICS
**Summery** 로그인한 사용자가 자신이 작성한 제보를 삭제하는 기능
**Scope** BreadCast: 가게 탐색: 가게 상세 페이지 탐색: 제보
**Level** User Level
**Author** 김서현
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 가게 상세 페이지의 제보 페이지에 접근한 상태 / 사용자가 작성한 제보가 있는 상태
**Trigger** 사용자가 제보 페이지에서 본인 제보의 ‘삭제’ 버튼을 누른다.
**Success Post Condition** 특정 가게의 제보 목록에서 해당 제보가 삭제된다.
**Failed Post Condition** 특정 가게의 제보 목록이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 제보 삭제를 요청한다.
	1.	시스템이 제보 삭제 의사 확인 메시지를 출력한다.
	2.	사용자가 시스템에 제보 삭제 의사를 전달한다.
	3.	시스템이 해당 제보를 DB에서 삭제한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 제보 삭제를 중단한다.
			2a1. 사용자가 제보 삭제를 시도하기 전 화면(제보 목록)으로 돌아간다.
		2b.	사용자가 제보를 삭제한 가게가 존재하지 않는다.
			2b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			2b2. 사용자는 유효한 이전 화면으로 돌아간다.
	3.	3a.	시스템이 서버 오류 등으로 DB에서 제보 정보를 삭제하지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 제보 정보 처리 실패를 알린다.
			3a2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 3-5회 / 1명, 1주
**Concurrency** 30개 요청
**Due Date** 2025.09.30.
***

### Use case #22: 인기 빵지순례 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자는 인기순으로 빵지순례 목록을 확인한다.
**Scope** BreadCast: 빵지순례 탐색
**Level** User Level
**Author** 김서현
**Last Update** 2025.08.13.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** -
**Trigger** 사용자가 ‘빵지순례’ 메뉴에 접근한다. (모든 페이지에서 접근할 수 있음)
**Success Post Condition** 빵지순례목록이 스크랩(하트)순(인기순)으로 정렬되어 리스트 형태로 화면에 표시된다.
**Failed Post Condition** 빵지순례 빵지순례 목록이 화면에 표시되지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 ‘빵지순례’ 메뉴에 접근한다.
	1.	시스템은 DB에서 빵지순례 목록을 인기순으로 조회한다.
	2.	시스템은 조회한 빵지순례를 인기순으로 화면에 출력한다. (무한 스크롤 로딩 방식을 적용함, 출력 요소: 제목, 좋아요 수, 이동 거리 및 소요 시간)
	3.	사용자는 스크롤을 수행하여 빵지순례를 계속 확인할 수 있다.
#### Extension Scenario
*Step.	Branching Action*
	1.	1a.	현재 등록된 빵지순례가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘빵지순례가 없습니다.’
		1b.	시스템이 서버 오류 등으로 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘페이지를 찾을 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 페이지 새로고침 버튼을 노출한다.
	2.	2a.	사용자가 세부 페이지를 확인하려는 빵지순례를 누른다.
			2a1. 시스템은 빵지순례 상세 페이지 보기 기능(#24)을 실행한다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 3회 / 1명, 1일
**Concurrency** 100개 요청
**Due Date** 2025.10.10.
***

### Use case #23: 빵지순례 검색하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 원하는 제목이나 키워드를 포함한 빵지순례를 검색하는 기능
**Scope** BreadCast:빵지순례 탐색
**Level** User Level
**Author** 김서현
**Last Update** 2025.08.14.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 빵지순례 페이지에 접근한 상태
**Trigger** 사용자가 빵지순례 페이지에서 검색창에 접근한다.
**Success Post Condition** 사용자가 입력한 검색어를 포함한 빵지순례들이 인기순으로 정렬되어 리스트 형태로 화면에 표시된다.
**Failed Post Condition** 빵지순례 목록이 화면에 표시되지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 빵지순례 검색을 요청한다.
	1.	시스템은 검색 화면을 출력한다.
	2.	사용자는 검색 화면에 검색어를 입력하여 제출한다. (검색 대상: 빵지순례 제목, 빵지순례 키워드)
	3.	시스템은 DB에서 사용자의 입력값을 제목이나 키워드에 포함하는 빵지순례를 조회한다.
	4.	시스템은 조회한 빵지순례를 인기순으로 화면에 출력한다. (무한 스크롤 로딩 방식을 적용함, 출력 요소: 제목, 좋아요 수, 이동 거리 및 소요 시간)
	5.	사용자는 스크롤을 수행하여 빵지순례를 계속 확인할 수 있다.
#### Extension Scenario
*Step.	Branching Action*
	2.	2a.	사용자가 검색을 중단한다.
		    2a1. 시스템은 사용자가 가게 검색에 접근하기 이전 화면으로 돌아간다.
        2b. GPS 검색 시 사용자가 위치 접근 권한을 허용하지 않은 상태이다.
            2b1. 오류 메시지를 출력한다: '위치 접근 권한이 없습니다.'
            2b2. 시스템이 사용자에게 위치 접근 권한 허용을 요청한다.
            2b3. 사용자가 위치 접근 권한 요청을 허용하거나 거부한다.
            2b4. 메인 시나리오 2로 돌아간다.
	3.	3a.	입력값이 누락되거나 비허용 값이 입력된 상태이다.
			3a1. 오류 메시지를 출력한다: ‘유효하지 않은 검색입니다.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	입력값을 만족하는 루트가 존재하지 않는다.
			3b1. 오류 화면을 출력한다: ‘빵지순례 루트가 존재하지 않습니다.’
        3c. 시스템이 서버 오류 등으로 루트 정보를 DB에서 찾지 못한다.
            3c1. 오류 메시지를 출력한다: '검색 결과를 불러올 수 없습니다.'
		    3c2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 페이지 새로고침 버튼을 노출한다.
	4.	4a.	사용자가 세부 페이지를 확인하려는 루트를 누른다.
			4a1. 시스템이 빵지순례 상세 페이지 보기(#25)를 실행한다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 10회 / 1명, 1일
**Concurrency** 150개 요청
**Due Date** 2025.10.20.
***

### Use case #24: 빵지순례 상세 페이지 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 선택한 빵지순례의 상세 페이지를 확인하는 기능 (작성된 글 내용을 확인할 수 있음)
**Scope** BreadCast: 빵지순례 탐색
**Level** User Level
**Author** 김서현
**Last Update** 2025.08.17.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** -
**Trigger** 사용자가 빵지순례 상세 페이지에 접근한다.
**Success Post Condition** 사용자가 선택한 빵지순례의 상세 정보가 전부 화면에 표시된다.
**Failed Post Condition** 사용자가 선택한 빵지순례의 상세 정보가 전부 화면에 표시되지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 빵지순례 상세 페이지에 접근한다.
	Sa.	사용자는 다음 기능을 통해 빵지순례 상세 페이지에 접근할 수 있다.
		    인기 빵지순례 보기(#22), 빵지순례 검색하기(#23), 관심 빵지순례 목록 보기(#35), 내가 쓴 빵지순례 리뷰 보기(#38), 내가 쓴 빵지순례 보기(#39)
	1.	시스템이 사용자가 확인하고자 하는 빵지순례 상세 페이지 정보를 DB에서 추가로 가져온다.
	2.	시스템이 화면 좌측의 기본 정보 하단에 빵지순례 상세 페이지를 출력한다. (구성: 구획으로 나뉜 세부 글, 구획에 포함되지 않은 일반 글, 댓글 형식 빵지순례 리뷰)
	3.	시스템은 스크롤 이동에 따라 좌측 상세 페이지에서 출력되는 가게가 변화하면, 해당 가게 마커를 화면 우측 지도에서 강조한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 가게 정보 화면에 접근하며 사용한 기능에서 오류가 발생한다.
		    Sa1. 접근한 기능의 시나리오에 따라 오류를 처리한다.
		    Sa2. 메인 시나리오 S로 돌아간다.
	1.	1a.	사용자가 선택한 빵지순례 ID가 존재하지 않는다.
			1a1. 오류 메시지를 출력한다: ‘빵지순례를 찾을 수 없습니다.’
			1a2. 사용자가 유효한 이전 화면으로 돌아간다.
		1b.	시스템이 서버 오류 등으로 빵지순례 상세 페이지 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘페이지를 찾을 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 페이지 새로고침 버튼을 노출한다.
	2.	2a.	빵지순례 정보 중 존재하지 않는 정보가 존재한다.
			2a1. 존재하지 않는 정보를 빈칸으로 대체하여 출력한다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 10회 / 1명, 1일
**Concurrency** 150개 요청
**Due Date** 2025.10.20.
***

### Use case #25: 빵지순례 관심 추가하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 관심이 있는 빵지순례의 스크랩 버튼을 눌러 마이페이지에서 관심 빵지순례 목록 보기(#35) 기능을 통해 볼 수 있게 저장하는 기능
**Scope** BreadCast: 빵지순례 탐색: 관심
**Level** User level
**Author** 박세은
**Last Update** 2025.08.17.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 빵지순례 상세 페이지에 접근한 상태
**Trigger** 사용자가 빵지순례 상세 페이지에서 스크랩 버튼을 누른다. (스크랩 버튼의 예: 하트 모양 등)
**Success Post Condition** 사용자가 누른 스크랩 버튼의 모양이 변하고, 활성화 상태가 된다. / 해당 빵지순례의 스크랩 수가 1 증가한다. / 사용자의 마이페이지 내 ‘관심 빵지순례 목록’에 스크랩을 누른 가게가 추가된다.
**Failed Post Condition** 사용자가 누른 스크랩 버튼의 모양과 상태가 유지된다. / 해당 빵지순례 의 스크랩의 수가 유지된다. / 사용자의 마이페이지 내 ‘관심 빵지순례 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 ‘관심 빵지순례 목록’에 추가하고 싶은 빵지순례 의 상세 페이지에서 스크랩 버튼을 누른다.
	1.	시스템은 관련 DB에 사용자 계정과 관심 가게 ID를 저장한다.
	2.	시스템은 스크랩 버튼의 모양을 변경하고, 스크랩 수가 1 증가한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 관심을 추가할 빵지순례 ID가 존재하지 않는다.
			1a1. 오류 메시지를 출력한다: ‘빵지순례를 찾을 수 없습니다.’
			1a2. 사용자가 유효한 이전 화면으로 돌아간다.
		1b.	시스템이 서버 오류 등으로 정보를 DB에 저장하지 못한다.
			1b1. 화면에 오류 메시지를 출력해 사용자에게 관심 추가 실패를 알린다.
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 100ms$
**Frequency** 5회 / 1명, 1일
**Concurrency** 100개 요청
**Due Date** 2025.10.20.
***

### Use case #26: 빵지순례 관심 삭제하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 특정 빵지순례 코스의 스크랩을 해제하고 마이페이지의 ‘관심 빵지순례 목록’에서 해당 빵지순례를 삭제하는 기능
**Scope** BreadCast: 빵지순례 탐색: 관심
**Level** User level
**Author** 박세은
**Last Update** 2025.08.17.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 빵지순례 상세 페이지에 접근한 상태/마이페이지의 ‘관심 빵지순례 목록’에 접근한 상태 / 사용자가 해당 빵지순례 를 스크랩한 상태
**Trigger** 사용자가 빵지순례 상세 페이지에서 활성화된 스크랩 버튼을 누른다./사용자가 마이페이지에서 관심을 삭제할 빵지순례 목록을 선택한다.
**Success Post Condition** 특정 빵지순례 의 스크랩 버튼의 모양이 기본형으로 변하고, 비활성화 상태가 된다. / 해당 빵지순례의 스크랩 수가 1 감소한다. / 사용자의 마이페이지 내 ‘관심 빵지순례 목록’에서 스크랩한 빵지순례 가 삭제된다.
**Failed Post Condition** 특정 가게의 스크랩 버튼 모양과 상태가 유지된다. / 해당 빵지순례의 스크랩 수가 유지된다. / 사용자의 마이페이지 내 ‘관심 빵지순례 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 빵지순례 관심 삭제를 요청한다.
	1.	시스템은 관련 DB에서 사용자 계정과 관심 빵지순례 ID 쌍을 찾아 삭제한다.
	2.	시스템은 스크랩 버튼의 모양과 상태를 변경한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 관심을 삭제할 빵지순례 ID가 존재하지 않는다.
			1a1. 오류 메시지를 출력한다: ‘빵지순례를 찾을 수 없습니다.’
			1a2. 사용자가 유효한 이전 화면으로 돌아간다.
		1b.	시스템이 서버 오류 등으로 정보를 DB에서 삭제하지 못한다.
			1b1. 화면에 오류 메시지를 출력해 사용자에게 관심 삭제 실패를 알린다.
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 3회 / 1명, 1일
**Concurrency** 100개 요청
**Due Date** 2025.10.20.
***

### Use case #27: 빵지순례 리뷰 쓰기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 방문한 빵지순례 리뷰를 작성하고 게시하는 기능
**Scope** BreadCast: 빵지순례 탐색: 리뷰
**Level** User level
**Author** 박세은
**Last Update** 2025.08.17.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 빵지순례 상세 페이지에 접근한 상태
**Trigger** 사용자가 빵지순례 상세 페이지에서 리뷰 작성 버튼을 누른다.
**Success Post Condition** 해당 빵지순례 상세 페이지 하단에 사용자가 작성한 리뷰가 추가된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’에 해당 리뷰가 추가된다.
**Failed Post Condition** 해당 빵지순례 상세 페이지 하단의 리뷰 목록이 유지된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 리뷰 작성을 요청한다.
	1.	시스템이 사용자에게 리뷰 작성 폼을 출력한다.
	2.	사용자가 리뷰 작성 폼을 따라 리뷰를 작성한다.
	3.	사용자가 리뷰 작성을 완료하고 시스템에 리뷰 게시를 요청한다.
	4.	시스템이 DB에 리뷰를 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 리뷰 작성을 중단한다.
			2a1. 사용자가 리뷰 작성을 시도하기 전 화면으로 돌아간다.
	3.	3a.	필수 입력 값이 누락된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	사용자가 리뷰를 작성한 빵지순례가 존재하지 않는다.
			3b1. 오류 메시지를 출력한다: ‘빵지순례를 찾을 수 없습니다.’
			3b2. 사용자가 유효한 이전 화면으로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 리뷰 정보를 저장하지 못한다.
			4a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 3으로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 500ms$
**Frequency** 2~4회 / 1명, 1주
**Concurrency** 10개 요청
**Due Date** 2025.10.20.
***

### Use case #28: 빵지순례 리뷰 수정하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 기존에 작성한 빵지순례 리뷰를 수정하는 기능
**Scope** BreadCast: 빵지순례 탐색: 리뷰
**Level** User level
**Author** 박세은
**Last Update** 2025.08.17.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 빵지순례 상세 페이지에 접근한 상태/마이페이지의 ‘내가 쓴 리뷰 목록’에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 빵지순례 상세 페이지 또는 마이페이지의 ‘내가 쓴 리뷰 목록’에서 본인이 작성한 리뷰에 있는 '수정' 버튼을 클릭한다.
**Success Post Condition** 기존에 작성한 리뷰 내용이 수정된다.
**Failed Post Condition** 기존에 작성한 리뷰 내용이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 리뷰 수정을 요청한다.
	1.	시스템이 사용자에게 기존 리뷰 내용이 채워진 리뷰 작성 폼을 출력한다.
	2.	사용자가 리뷰 작성 폼에 따라 리뷰를 수정한다.
	3.	사용자가 리뷰 수정을 완료하고 시스템에 수정 리뷰 게시를 요청한다.
	4.	시스템이 DB에 수정한 리뷰를 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그아웃 상태임을 알린다.
	2.	2a.	사용자가 리뷰 수정을 중단한다.
			2a1. 사용자가 리뷰 수정을 시도하기 전 화면(가게 리뷰 목록)으로 돌아간다.
	3.	3a.	필수 입력 값이 누락된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	사용자가 리뷰를 수정한 가게가 존재하지 않는다.
			3b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			3b2. 사용자가 유효한 이전 화면으로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 리뷰 정보를 저장하지 못한다.
			4a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 1–2회 / 1명, 1주
**Concurrency** 10개 요청
**Due Date** 2025.10.20.

### Use case #29: 빵지순례 리뷰 삭제하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 기존에 작성한 빵지순례 리뷰를 삭제하는 기능
**Scope** BreadCast: 빵지순례 탐색: 리뷰
**Level** User level
**Author** 박세은
**Last Update** 2025.08.17.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 빵지순례 상세 페이지에 접근한 상태/마이페이지의 ‘내가 쓴 리뷰 목록’에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 빵지순례 상세 페이지에서 본인이 작성한 리뷰에 있는 '삭제' 버튼을 클릭한다.사용자가 마이페이지의 ‘내가 쓴 리뷰 목록’에서 삭제할 리뷰 목록을 선택한다.
**Success Post Condition** 해당 빵지순례 상세 페이지 하단에서 해당 리뷰가 삭제된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’에서 해당 리뷰가 삭제된다.
**Failed Post Condition** 해당 빵지순례 상세 페이지 하단의 리뷰 목록이 유지된다. / 사용자의 마이페이지 내 ‘내가 쓴 리뷰 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 리뷰 삭제를 요청한다.
	1.	시스템이 리뷰 삭제 의사 확인 메시지를 출력한다.
	2.	사용자가 시스템에 리뷰 삭제 의사를 전달한다.
	3.	시스템이 해당 리뷰를 DB에서 삭제한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그아웃 상태임을 알린다.
	2.	2a.	사용자가 리뷰 삭제를 중단한다.
			2a1. 사용자가 리뷰 삭제를 시도하기 전 화면(가게 리뷰 목록)으로 돌아간다.
		2b.	사용자가 리뷰를 삭제한 가게가 존재하지 않는다.
			2b1. 오류 메시지를 출력한다: ‘가게를 찾을 수 없습니다.’
			2b2. 사용자는 유효한 이전 화면으로 돌아간다.
	3.	3a.	시스템이 서버 오류 등으로 DB에서 리뷰 정보를 삭제하지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 리뷰 정보 처리 실패를 알린다.
			3a2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 1–2회 / 1명, 1달
**Concurrency** 5개 요청
**Due Date** 2025.10.20.
***

### Use case #30: 빵지순례 만들기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 새 빵지순례를 작성하고 게시하는 기능
**Scope** BreadCast: 빵지순례 탐색
**Level** User level
**Author** 김서연
**Last Update** 2025.08.12
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 빵지순례 페이지에 접근한 상태
**Trigger** 사용자가 빵지순례 페이지에서 만들기 버튼을 누른다.
**Success Post Condition** 시스템의 빵지순례 빵지순례 목록에 새 빵지순례가 추가된다. / 사용자의 마이페이지 내 ‘내 빵지순례 목록’에 해당 빵지순례가 추가된다.
**Failed Post Condition** 시스템의 빵지순례 빵지순례 목록이 유지된다. / 사용자의 마이페이지 내 ‘내 빵지순례 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 빵지순례 만들기를 요청한다.
	1.	시스템이 사용자에게 빵지순례 만들기 폼을 출력한다.
	2.	사용자가 빵지순례 작성 폼에 따라 빵지순례를 수정한다.
		2a.	사용자는 가게 추가 버튼을 눌러 관심 빵지순례 목록 보기(#35) 기능을 사용할 수 있다.
		2b.	시스템은 기능 관련 가게 목록을 출력한다.
		2c.	사용자는 가게 목록에서 가게 이름을 눌러 빵지순례에 가게를 추가할 수 있다.
	3.	사용자가 빵지순례 작성을 완료하고 시스템에 빵지순례 게시를 요청한다.
	4.	시스템이 DB에 빵지순례를 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	사용자가 접근한 가게 검색하기 기능이나 관심 빵지순례 목록 보기, 가게 정보 보기 기능에서 오류가 발생한다.
		    2a1. 접근한 기능의 시나리오에 따라 오류를 처리한다.
		    2a2. 메인 시나리오 2로 돌아간다.
	3.	3a.	사용자가 빵지순례 만들기를 중단한다.
		    3a1. 사용자가 빵지순례 만들기를 시도하기 전 화면으로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 빵지순례 정보를 저장하지 못한다.
		    4a1. 화면에 오류 메시지를 출력하여 빵지순례 정보 처리 실패를 알린다.
		    4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 3으로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 0.5s$
**Frequency** 10회 / 1일
**Concurrency** 2개 요청
**Due Date** 2025.10.10.
***

### Use case #31: 빵지순례 수정하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 기존에 작성한 빵지순례 관련 내용을 수정하는 기능
**Scope** BreadCast: 빵지순례 탐색
**Level** User level
**Author** 박세은
**Last Update** 2025.08.19.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 자신이 작성한 빵지순례 상세 페이지에 접근한 상태/마이페이지의 ‘내 빵지순례 목록’에 접근한 상태 / 사용자가 작성한 빵지순례가 있는 상태
**Trigger** 사용자가 빵지순례 상세 페이지 또는 마이페이지의 ‘내 빵지순례 목록’에서 본인이 작성한 빵지순례 상세 페이지에 있는 '수정' 버튼을 누른다.
**Success Post Condition** 기존에 작성한 빵지순례 내용이 수정된다.
**Failed Post Condition** 기존에 작성한 빵지순례 내용이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 빵지순례 수정을 요청한다.
	1.	시스템이 사용자에게 기존 빵지순례 내용이 채워진 빵지순례 작성 폼을 출력한다.
	2.	사용자가 빵지순례 작성 폼에 따라 빵지순례를 수정한다.
		2a.	사용자는 가게 추가 버튼을 눌러 관심 가게 목록 보기(#34) 기능을 사용할 수 있다.
		2b.	시스템은 기능 관련 가게 목록을 출력한다.
		2c.	사용자는 가게 목록에서 가게 이름을 눌러 빵지순례에 가게를 추가할 수 있다.
	3.	사용자가 빵지순례 수정을 완료하고 시스템에 수정 빵지순례 게시를 요청한다.
	4.	시스템이 DB에 수정한 빵지순례를 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그아웃 상태임을 알린다.
	2.	2a.	사용자가 빵지순례 수정을 중단한다.
			2a1. 사용자가 빵지순례 수정을 시도하기 전 화면으로 돌아간다.
	3.	3a.	필수 입력 값이 누락된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	사용자가 빵지순례를 수정한 빵지순례가 존재하지 않는다.
			3b1. 오류 메시지를 출력한다: ‘빵지순례를 찾을 수 없습니다.’
			3b2. 사용자가 유효한 이전 화면으로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 빵지순례 정보를 저장하지 못한다.
			4a1. 화면에 오류 메시지를 출력하여 빵지순례 정보 처리 실패를 알린다.
			4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 1–2회 / 1명, 1주
**Concurrency** 10개 요청
**Due Date** 2025.10.20.
***

### Use case #32: 빵지순례 삭제하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 기존에 작성한 빵지순례를 삭제하는 기능
**Scope** BreadCast: 빵지순례 탐색
**Level** User level
**Author** 박세은
**Last Update** 2025.08.20.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 자신이 작성한 빵지순례 상세 페이지에 접근한 상태/마이페이지의 ‘내 빵지순례 목록’에 접근한 상태 / 사용자가 작성한 빵지순례가 있는 상태
**Trigger** 사용자가 빵지순례 상세 페이지 또는 마이페이지의 ‘내 빵지순례 목록’에서 본인이 작성한 빵지순례 상세 페이지에 있는 '삭제' 버튼을 누른다.
**Success Post Condition** 시스템의 빵지순례목록에서 해당 빵지순례가 삭제된다. / 사용자의 마이페이지 내 ‘내 빵지순례 목록’에서 해당 리뷰가 삭제된다.
**Failed Post Condition** 시스템의 빵지순례목록이 유지된다. / 사용자의 마이페이지 내 ‘내 빵지순례 목록’이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 빵지순례 삭제를 요청한다.
	1.	시스템이 빵지순례 삭제 의사 확인 메시지를 출력한다.
	2.	사용자가 시스템에 빵지순례 삭제 의사를 전달한다.
	3.	시스템이 해당 빵지순례를 DB에서 삭제한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그아웃 상태임을 알린다.
	2.	2a.	사용자가 빵지순례 삭제를 중단한다.
			2a1. 사용자가 빵지순례 삭제를 시도하기 전 화면으로 돌아간다.
		2b.	사용자가 삭제한 빵지순례가 존재하지 않는다.
			2b1. 오류 메시지를 출력한다: ‘빵지순례를 찾을 수 없습니다.’
			2b2. 사용자는 유효한 이전 화면으로 돌아간다.
	3.	3a.	시스템이 서버 오류 등으로 DB에서 빵지순례 정보를 삭제하지 못한다.
			3a1. 화면에 오류 메시지를 출력하여 빵지순례 정보 처리 실패를 알린다.
			3a2. 메인 시나리오 3을 재시도한다. 재시도가 실패할 경우, 메인 시나리오 S로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 1–2회 / 1명, 1달
**Concurrency** 5개 요청
**Due Date** 2025.10.20.
***

### Use case #33: 프로필 변경하기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 자신의 계정 정보 중 닉네임을 변경하는 기능
**Scope** BreadCast: 마이페이지
**Level** User level
**Author** 김현지
**Last Update** 2025.08.20.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 마이페이지에 접근한 상태
**Trigger** 사용자가 마이페이지에서 ‘닉네임 변경’ 버튼을 누른다.
**Success Post Condition** 사용자의 닉네임이 변경된다.
**Failed Post Condition** 사용자의 닉네임이 유지된다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에게 닉네임 변경을 요청한다.
	1.	시스템이 사용자에게 현재 닉네임이 채워진 닉네임 수정 폼을 출력한다.
	2.	사용자는 변경할 닉네임을 입력한다.
	3.	사용자가 시스템에 닉네임 변경을 요청한다.
	4.	시스템이 DB에 변경한 닉네임을 저장한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	2.	2a.	시스템이 프로필 정보를 불러오지 못함
		    2a1. “프로필 정보를 불러올 수 없습니다” 메시지 출력
		    2a2. 재시도 버튼 또는 이전 페이지로 이동
		2b.	사용자가 닉네임 변경을 중단한다.
			2b1. 사용자가 닉네임 변경을 시도하기 전 화면으로 돌아간다.
	3.	3a.	필수 입력 값이 누락된 상태이다.
			3a1. 시스템이 오류 메시지를 출력한다: ‘필수 항목을 입력하세요.’
			3a2. 메인 시나리오 2로 돌아간다.
		3b.	이미 사용 중인 닉네임을 입력한다.
			3b1. 시스템이 오류 메시지를 출력한다: ‘이미 존재하는 닉네임입니다.’
			3b2. 메인 시나리오 2로 돌아간다.
	4.	4a.	시스템이 서버 오류 등으로 DB에 변경한 닉네임을 저장하지 못한다.
			4a1. 화면에 오류 메시지를 출력하여 닉네임 정보 처리 실패를 알린다.
			4a2. 메인 시나리오 4를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 1s$
**Frequency** 1–2회 / 1명, 1주
**Concurrency** 10개 요청
**Due Date** 2025.09.20.
***

### Use case #34: 관심 가게 목록 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 ‘즐겨찾기’로 등록한 가게 목록을 확인하는 기능
**Scope** BreadCast: 마이페이지
**Level** User level
**Author** 김현지
**Last Update** 2025.08.20.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 마이페이지에 접근한 상태 / 사용자가 관심 등록한 가게가 있는 상태
**Trigger** 사용자가 마이페이지에서 ‘즐겨찾기: 가게’ 메뉴를 선택한다.
**Success Post Condition** 시스템이 사용자의 모든 관심 가게 목록을 화면에 출력한다. / 사용자는 가게 이름을 클릭해 가게 상세 페이지로 이동할 수 있다.
**Failed Post Condition** 시스템이 사용자의 모든 관심 가게 목록을 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 마이페이지에서 ‘즐겨찾기: 가게’ 메뉴를 선택한다.
	1.	시스템은 사용자가 즐겨찾기 한 가게 목록을 DB에서 가져온다.
	2.	시스템은 조회한 가게 목록을 리스트 형태로 화면에 출력한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 즐겨찾기 한 가게가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘등록된 관심 가게가 없습니다.’
		1b.	시스템이 서버 오류 등으로 가게 목록를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘관심 가게 목록을 불러올 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	가게 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 1-3회 / 1명, 1일
**Concurrency** 50개 요청
**Due Date** 2025.09.30.
***

### Use case #35: 관심 빵지순례 목록 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 ‘즐겨찾기’로 등록한 빵지순례 목록을 확인하는 기능
**Scope** BreadCast: 마이페이지
**Level** User level
**Author** 김현지
**Last Update** 2025.08.20.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 마이페이지에 접근한 상태 / 사용자가 관심 등록한 빵지순례가 있는 상태
**Trigger** 사용자가 마이페이지에서 ‘즐겨찾기: 빵지순례’ 메뉴를 선택한다.
**Success Post Condition** 시스템이 사용자의 모든 관심 빵지순례 목록을 화면에 출력한다. / 사용자는 빵지순례 이름을 클릭해 빵지순례 상세 페이지로 이동할 수 있다.
**Failed Post Condition** 시스템이 사용자의 모든 관심 빵지순례 목록을 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 시스템에 빵지순례 목록 조회를 요청한다.
	1.	사용자가 마이페이지에서 ‘즐겨찾기: 빵지순례’ 메뉴를 선택한다.
	2.	시스템은 사용자가 즐겨찾기 한 빵지순례 목록을 DB에서 가져온다.
	3.	시스템은 조회한 빵지순례 목록을 리스트 형태로 화면에 출력한다.
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 즐겨찾기 한 빵지순례가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘등록된 관심 빵지순례가 없습니다.’
		1b.	시스템이 서버 오류 등으로 빵지순례 목록을 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘관심 빵지순례 목록을 불러올 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	빵지순례 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 1-2회 / 1명, 1일
**Concurrency** 50개 요청
**Due Date** 2025.09.30.
***

### Use case #36: 내가 쓴 가게 리뷰 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 자신이 작성한 모든 가게 리뷰를 한 번에 확인하는 기능
**Scope** BreadCast: 마이페이지
**Level** User level
**Author** 김현지
**Last Update** 2025.08.20.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 마이페이지에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 마이페이지에서 ‘내가 작성한 리뷰보기: 가게 리뷰’ 메뉴를 선택한다.
**Success Post Condition** 시스템이 사용자의 모든 가게 리뷰 목록을 최신순으로 화면에 출력한다.
**Failed Post Condition** 시스템이 사용자의 모든 가게 리뷰 목록을 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 마이페이지에서 ‘내가 작성한 리뷰보기: 가게 리뷰’ 메뉴를 선택한다.
	1.	시스템은 사용자가 작성한 모든 가게 리뷰를 DB에서 가져온다.
	2.	시스템은 조회한 리뷰 목록을 리스트 형태로 화면에 표시한다. (출력 정보: 리뷰한 가게, 별점, 내용, 작성일 등)
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 작성한 리뷰가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘작성한 가게 리뷰가 없습니다.’
		1b.	시스템이 서버 오류 등으로 리뷰 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘리뷰를 불러올 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	리뷰 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
		2b.	사용자는 가게 리뷰 수정하기(#12), 가게 리뷰 삭제하기(#13) 기능에 접근할 수 있다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 1-2회 / 1명, 1일
**Concurrency** 10개 요청
**Due Date** 2025.09.30.
***

### Use case #37: 내가 쓴 메뉴 리뷰 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 자신이 작성한 모든 메뉴 리뷰를 한 번에 확인하는 기능
**Scope** BreadCast: 마이페이지
**Level** User level
**Author** 김현지
**Last Update** 2025.08.20.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 마이페이지에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 마이페이지에서 ‘내가 작성한 리뷰보기: 메뉴 리뷰’ 메뉴를 선택한다.
**Success Post Condition** 시스템이 사용자의 모든 메뉴 리뷰 목록을 최신순으로 화면에 출력한다.
**Failed Post Condition** 시스템이 사용자의 모든 메뉴 리뷰 목록을 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 마이페이지에서 ‘내가 작성한 리뷰보기: 메뉴 리뷰’ 메뉴를 선택한다.
	1.	시스템은 사용자가 작성한 모든 메뉴 리뷰를 DB에서 가져온다.
	2.	시스템은 조회한 리뷰 목록을 리스트 형태로 화면에 표시한다. (출력 정보: 리뷰한 가게, 메뉴, 별점, 내용, 작성일 등)
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 작성한 리뷰가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘작성한 메뉴 리뷰가 없습니다.’
		1b.	시스템이 서버 오류 등으로 리뷰 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘리뷰를 불러올 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	리뷰 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
		2b.	사용자는 메뉴 리뷰 수정하기(#17), 메뉴 리뷰 삭제하기(#18) 기능에 접근할 수 있다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 1-3회 / 1명, 1일
**Concurrency** 10개 요청
**Due Date** 2025.09.30.
***

### Use case #38: 내가 쓴 빵지순례 리뷰 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 자신이 작성한 모든 빵지순례 리뷰를 한 번에 확인하는 기능
**Scope** BreadCast: 마이페이지
**Level** User level
**Author** 김현지
**Last Update** 2025.08.21.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 마이페이지에 접근한 상태 / 사용자가 작성한 리뷰가 있는 상태
**Trigger** 사용자가 마이페이지에서 ‘내가 작성한 리뷰보기: 빵지순례 리뷰’ 메뉴를 선택한다.
**Success Post Condition** 시스템이 사용자의 모든 빵지순례 리뷰 목록을 최신순으로 화면에 출력한다.
**Failed Post Condition** 시스템이 사용자의 모든 빵지순례 리뷰 목록을 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 마이페이지에서 ‘내가 작성한 리뷰보기: 빵지순례 리뷰’ 메뉴를 선택한다.
	1.	시스템은 사용자가 작성한 모든 빵지순례 리뷰를 DB에서 가져온다.
	2.	시스템은 조회한 리뷰 목록을 리스트 형태로 화면에 표시한다. (출력 정보: 빵지순례명, 내용, 작성일 등)
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 작성한 리뷰가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘작성한 빵지순례 리뷰가 없습니다.’
		1b.	시스템이 서버 오류 등으로 리뷰 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘리뷰를 불러올 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	리뷰 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
		2b.	사용자는 빵지순례 리뷰 삭제하기(#28), 빵지순례 리뷰 삭제하기(#29) 기능에 접근할 수 있다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 1-5회 / 1명, 1일
**Concurrency** 10개 요청
**Due Date** 2025.10.20.
***

### Use case #39: 내가 쓴 빵지순례 보기
#### GENERAL CHARACTERISTICS
**Summery** 사용자가 자신이 생성한 모든 빵지순례를 한 번에 확인하는 기능
**Scope** BreadCast: 마이페이지
**Level** User level
**Author** 김현지
**Last Update** 2025.08.21.
**Status** Analysis
**Primary Actor** 사용자
**Preconditions** 사용자가 로그인한 상태 / 사용자가 생성한 빵지순례가 있는 상태
**Trigger** 사용자가 마이페이지에 최초 접근한다.
**Success Post Condition** 시스템이 사용자의 모든 빵지순례 목록을 화면에 출력한다.
**Failed Post Condition** 시스템이 사용자의 모든 빵지순례 목록을 화면에 출력하지 않는다.
#### Main Success Scenario
*Step.	Action*
	S.	사용자가 마이페이지에 최초 접근한다.
	1.	시스템은 사용자가 생성한 모든 빵지순례 정보를 DB에서 가져온다.
	2.	시스템은 조회한 빵지순례 목록을 카드 형태로 화면에 표시한다. (출력 정보: 대표사진, 제목 등)
#### Extension Scenario
*Step.	Branching Action*
	S.	Sa.	사용자가 로그아웃 상태이다.
		    Sa1. 화면에 일반 메시지를 출력해 사용자에게 로그인 후 사용가능한 기능임을 알린다.
	1.	1a.	사용자가 생성한 빵지순례가 존재하지 않는다.
			1a1. 오류 화면을 출력한다: ‘생성한 빵지순례가 없습니다.’
		1b.	시스템이 서버 오류 등으로 빵지순례 정보를 DB에서 찾지 못한다.
			1b1. 오류 화면을 출력한다: ‘빵지순례 빵지순례를 불러올 수 없습니다.’
			1b2. 메인 시나리오 1을 재시도한다. 재시도가 실패할 경우, 시나리오를 중단한다.
	2.	2a.	빵지순례 정보 일부가 누락된 상태이다.
			2a1. 시스템은 해당 필드에 ‘정보 없음’을 표시하고 나머지 정보를 정상 출력한다.
		2b.	사용자는 빵지순례 수정하기(#31), 빵지순례 삭제하기(#32) 기능에 접근할 수 있다.
		2c.	사용자는 빵지순례의 공개/비공개 여부를 변경할 수 있다.
			2c1. 공개/비공개 상태 변경 시 서버 오류 등 오류가 발생한다.
			2c2. 오류 메시지를 출력한다: ‘상태 변경에 실패했습니다.’
			2c3. 확장 시나리오 2c를 재시도한다. 재시도가 실패할 경우, 메인 시나리오 2로 돌아간다.
#### RELATED INFORMATION
**Performance** $\le 3s$
**Frequency** 1-5회 / 1명, 1일
**Concurrency** 30개 요청
**Due Date** 2025.10.20.
***