<h1 style="font-size: 48px;">Next.js</h1>

1. 폰트 바꾸기
   ex> .postForm textarea::placeholder {
   font-family: Malgun Gothic;
   }
2. z-index ?
3. dayjs -> 날짜 계산해주는 라이브러리 (게시물 몇 시간 전 생성 표시할 때 사용)
4. dayjs는 플러그인방식을 사용해서 따로 필요한 기능별 임포트를 해줘야 한다. -> 라이브러리에서 플러그인 방식이 무엇인지 찾아보기
   ex> import dayjs from 'dayjs';
   dayjs.locale('ko'); // 한국어 플러그인
   dayjs.extend(relativeTime) // 상대 시간 플러그인
5. 조건부로 클래스가 달라질때 classnames라는 라이브러리를 사용한다.
6. 모달 만들 때에는 Default를 항상 만들어주자 -> 새로 고침 때문에 그런건가?
7. footerButtonLeft {
   flex: 1;
   } -> 푸터를 최대한 길게해서 게시하기를 가장 오른쪽에 짱박기
8. 왼쪽 사이드바 각 페이지별로 모달 띄울 때 뒤에 나오는 배경 라우팅 개별적으로 맞게 하는 방법: /compose/tweet만들기 마지막 부분에 있음.
9. 주소마다
10. usePathname()
11. 라디오 버튼 다르게 하는 법: 가짜로 div를 만든다. 라디오 인풋 위에다가 div를 올려놓은 것이다.
    검색 필터 예시보고 따라서 만들어보기.
12. 사진 업로드 할 때에 버튼 클릭하면 인풋 클릭하게 만드는 원리와 마찬가지로 Ref로 연결해주면 된다.  *useRef 를 말하는 것임
13. 컴포넌트는 어떤 범위에서 사용되는지에 따라서 각 라우팅 경로별 세팅을 해주면 된다. 공통적으로 쓰이는 컴포넌트는 맨 위에다
14. useSearchParams
15. 서버 컴포넌트는 클라이언트 컴포넌트 자식일 때, 칠드런이나 프롭스로 보낸다.(이벤트 캡처링과 /status/[id]페이지 4:00 정도)
16. 클라이언트 컴포넌트에서 서버 컴포넌트를 임포트한다면 서버 컴포넌트가 클라이언트 컴포넌트로 성격이 바뀌어버린다.
17. 이벤트 캡처링 : onClickCapture { onClick }
18. YAGNI 원칙(You Aren't Gonna Need It)은 소프트웨어 개발에서 자주 언급되는 원칙으로,
    **"나중에 필요할 거라고 생각되는 기능을 미리 구현하지 말라"**는 의미입니다.
19. faker.js**는 더미 데이터를 생성하기 위한 JavaScript 라이브러리
20. 공통된 컴포넌트에서 조금씩 variation이 생기는건 Props로 해결 할 수 있어?
21. &nbsp;
22. App라우터가 아직 불안정해서 css.module로 사용하는 중이다.


### Next

4. parallel route
5. server component vs client component
6. 앱 라우팅은 소스코드 디렉토리 기반으로 라우팅을 만들어 낸다.
9. 인터셉트 라우팅: (.), (..)로
10. private 폴더: _component 디렉토리
11. router.replace vs router.push: 리플레이스는 브라우저 히스토리 스택을 대체 하거나 새 항목을 추가하거나 차이이다.
7. 동적 라우팅: [] 로 표기된 디렉토리는 동적으로 경로가 변할때 변수처럼 사용하는 표기법이다.
8. 경로 세그먼트 그룹화: ()로 디렉토리를 묶으면 그룹화를 시켜 구조적 복잡성을 줄여줄 수 있다.
3. 폴더명을 (something) [something2] 이런식으로 적어 놓게 되면 폴더명도 변수처럼 사용할 수 있게 된다.

### TS

18. ts에선 타입 or로 묶을 때 ||가 아니라 |로 하는 것이다.

### React

19. useRef 뭔지 알아내기
17. contextAPI , import { createContext } from 'react'
12. ReactNode
2. 컴포넌트를 중심으로 응집해서 설계한 코드는 생산성이 훨씬 좋다.
1. tsx 파일로 작업한다.

### Logic

20. optional chaining 이용해서 null 케이스 처리하기
    - ex> const onClickButton = () => {
      imageRef.current?.click();
      }
13. 액티브 링크 -> 현재 사용자가 들어가 있는 페이지 표시

### CSS
21. transition-duration: 0.2s; -> 이미지 바뀌는데 걸리는 시간 설정
22. .uploadButton svg { fill: rgb:(29,155,240); } -> svg 색 채우기 (아이콘 색 변경한다고 보면 된다.) 
16. backdrop-filter: blur(12px); -> 살짝 투명하게 만들어줌
15. 왜 css 파일에서 something.module.css 에서 module은 왜 필요한거지?
14. inherit -> 부모 요소 특성 상속