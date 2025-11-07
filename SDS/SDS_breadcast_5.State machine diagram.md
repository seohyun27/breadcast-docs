# 5. State machine diagram

## 5-1. Userdata State
![1-Userdate-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/1-Userdata-State.jpg?raw=true)
위의 그림은 회원 관련 과정을 state machine diagram으로 나타낸 것이다.

사용자가 서비스 이용을 시작하면 회원 미로그인 상태로 이동한다.
- 사용자가 로그인 상태가 아니라면 회원 미로그인 상태가 유지된다.</br>
사용자가 회원가입을 요청하면 회원가입 진행 상태로 바뀐다.
회원가입이 끝나면 회원 생성 상태가 되고 회원 생성 완료 요청이 들어오면 다시 대기 상태로 돌아온다.</br>
로그인 요청이 들어오면 대기에서 로그인 진행 상태로 이동한다. 로그인이 완료되면 회원 미로그인 상태가 끝나고 회원 로그인 상태로 바뀌게 된다.

- 사용자가 이미 로그인한 채 서비스에 접근했다면 회원 미로그인 상태에서 바로 회원 로그인 상태로 넘어간다.</br>
사용자가 로그아웃을 요청하면 로그아웃 진행 상태가 되고 로그아웃을 완료하면 다시 회원 미로그인 상태로 돌아간다.</br>
회원 탈퇴를 요청하면 회원 탈퇴 진행 상태로 변경된다. 탈퇴가 끝나면 회원 삭제 상태로 이동한댜. 회원 삭제도 완료되면 회원 미로그인 상태로 이동한다.

사용자가 서비스 이용을 그만두면 회원 미로그인 상태, 회원 로그인 상태에 상관없이 둘 다 상태가 종료된다.


## 5-2. Bakery State
![2-Bakery-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/2-Bakery-State.jpg?raw=true)
위의 그림은 빵집 관련 과정을 state machine diagram으로 나타낸 것이다.

사용자가 서비스 이용을 시작하면 처음엔 대기 상태로 이동한다.
- 임시 글

- 사용자가 원하는 서비스에 접근하면 가게관련 서비스 제공의 대기 상태로 변경된다.</br>
임시

사용자가 서비스 이용을 종료하면 최종적으로 대기 상태가 종료된다.

## 5-3. Course State
![3-Course-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/3-Course-State.jpg?raw=true)
위의 그림은 빵지순례 관련 과정을 state machine diagram으로 나타낸 것이다.

사용자가 서비스 이용을 시작하면 처음엔 대기 상태로 바뀐다.
- 임시

사용자가 서비스 이용을 종료하면 대기 상태가 종료된다.

## 5-e1. Create State
![4-Create-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/e1-Create-State.jpg?raw=true)
위의 그림은 객체 생성 과정을 state machine diagram으로 나타낸 것이다.

첫 시작은 사용자가 객체 생성 요청을 시스템으로 보낸다. 이 요청으로 객체 작성이란 상태로 바뀌게 된다. 사용자가 원하는 내용을 다 입력하면  최종적으로 객체 생성 상태로 바뀌고 생명 주기가 종료된다. 

## 5-e2. Update State
![5-Update-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/e2-Update-State.jpg?raw=true)
위의 그림은 객체 수정 과정을 state machine diagram으로 나타낸 것이다.

첫 시작은 사용자가 객체 수용 요청을 시스템으로 보낸다. 사용자의 요청으로 객체 작성이란 상태로 바뀌게 된다. 사용자가 수정된 내용을 입력하면  최종적으로 객체 수정 상태로 바뀌고 생명 주기가 종료된다.


## 5-e3. Delete State
![6-Delete-State](https://github.com/seohyun27/breadcast-docs/blob/main/SDS/images/state-machine/e3-Delete-State.jpg?raw=true)
위의 그림은 객체 삭제 과정을 state machine diagram으로 나타낸 것이다.

첫 시작은 사용자가 객체 삭제 요청을 시스템으로 보낸다. 이 요청을 통해 객체 삭제 상태로 바뀌고 생명 주기가 종료된다.
