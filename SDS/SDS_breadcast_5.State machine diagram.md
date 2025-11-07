# 5. State machine diagram

## 5-1. Userdata State
![1-Userdate-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/1-Userdata-State.jpg?raw=true)
위의 그림은 사용자의 회원 미로그인과 회원 로그인 과정을 state machine diagram으로 나타낸 것이다.


## 5-2. Bakery State
![2-Bakery-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/2-Bakery-State.jpg?raw=true)


## 5-3. Course State
![3-Course-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/3-Course-State.jpg?raw=true)


## 5-e1. Create State
![4-Create-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/e1-Create-State.jpg?raw=true)
위의 그림은 객체 생성 과정을 state machine diagram으로 나타낸 것이다. 첫 시작은 사용자가 객체 생성 요청을 시스템으로 보낸다. 이 요청으로 객체 작성이란 상태로 바뀌게 된다. 사용자가 원하는 내용을 다 입력하면  최종적으로 객체 생성 상태로 바뀌고 생명 주기가 종료된다. 

## 5-e2. Update State
![5-Update-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/e2-Update-State.jpg?raw=true)
위의 그림은 객체 수정 과정을 state machine diagram으로 나타낸 것이다. 첫 시작은 사용자가 객체 수용 요청을 시스템으로 보낸다. 사용자의 요청으로 객체 작성이란 상태로 바뀌게 된다. 사용자가 수정된 내용을 입력하면  최종적으로 객체 수정 상태로 바뀌고 생명 주기가 종료된다.


## 5-e3. Delete State
![6-Delete-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/e3-Delete-State.jpg?raw=true)
위의 그림은 객체 삭제 과정을 state machine diagram으로 나타낸 것이다. 첫 시작은 사용자가 객체 삭제 요청을 시스템으로 보낸다. 이 요청을 통해 객체 삭제 상태로 바뀌고 생명 주기가 종료된다.
