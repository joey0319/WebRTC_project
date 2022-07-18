<필요한 state>

0. token: localStorage.getItem('token') || ''
1. isLoggedIn: !!state.token -> token state가 먼저 갱신되어야 한다.
2. currentUser: {}
3. authError: null
4. authHeader : null
5. profile: {}


#1. login 프로세스

1. login 버튼 누르면 login url로 로그인 요청(user가 입력한 credentials 데이터를 post)
2. 실패하면 catch 응답받은 에러 메세지를 출력한다.
3. 성공하면 응답으로 {"Token":token}이 온다.
4. 위의 응답을 로컬 스토리지에 저장한다.(token: 응답받은 token)
state의 token 값은 localStorage.getItem('token') || ''으로 가지고 온다.(앞이 참이면 앞을 반환 앞이 거짓이면 뒤를 반환) - useEffect 통해서 항상 반복해서 가져온다.
5. isLoggedIn state는 toekn이 있으면 true, token이 없으면 false로 바꾼다.(!!state.token) - useEffect 통해서 항상 한다.
6. currentUser 정보를 state에서 업데이트한다.
currentUser -> isLoggedIn == true이면 user 정보를 얻는 요청을 보낸다.
응답으로 user 정보가 온다.
이 유저 정보를 currentUser state에 저장한다.
-> 메인페이지로 이동
catch(error)이면 토큰을 로컬 스토리지에서 삭제하고 로그인 뷰로 이동한다.


#2. signup 프로세스

1. front에서 user가 입력한 credentials를 signup url로 백엔드에 post 요청 보낸다.
credentials = {username:'', password1:'','password2':''}
2. 실패하면 응답받은 에러메세지 출력
3. 성공하면 token을 로컬스토리지에 저장 -> token state는 localStorage.getItem('token') || ''
-> isLoggedIn state를 !!state.token 으로 바꾼다.
-> isLoggedIn이 true이면 currentUser 정보를 get 요청으로 가져온다.
-> 가져온 user 정보를 state의 currentUser에 저장한다.
4. 성공하면 메인페이지로 이동한다.


#3. logout 프로세스

1. logout 요청 url로 post 요청 보낸다.
(headers에 authHeader Token값 함께 post)
2. 실패하면 응답받은 에러메세지 출력
3. 성공하면 로컬 스토리지에서 token 삭제(빈문자열로 설정)
localStorage.setItem('token','')
4. 메인페이지로 이동


#4. fetchCurrentUser 프로세스

1. isLoggedIn이 true이면 user 정보 확인용 url에 get 요청
authHeader에 token 추가하여 요청
2. 성공하면 currentUser를 state에서 응답받은 객체로 갱신
3. 실패하면(토큰이 잘못되었다면) 토큰 삭제하고(localStorage.setItem('token',''))로그인페이지로 이동


#5. fetchProfile 프로세스

1. authHeader에 토큰 값 추가하여 profile 조회 get요청
2. 성공하면 profile state를 갱신

