---
layout: post
title: "Next.js의 성능 차이 반응 앱 만들기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2019/06/nextjs-vs-create-react-app.jpg"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/nextjs-vs-create-react-app.jpg?fit=730%2C480&ssl=1)

## 도입

next.js vs. 반응 앱 만들기: 어떤 앱이 더 성능이 좋습니까? 이 기사에서는 SSR 대 CSR을 포함한 이 질문을 일부 데이터로 풀려고 합니다. 먼저 정확히 무엇을 비교하고 있는지 이해해야 합니다.

## next.js vs. React App 생성: 프레임워크 이해

### Next.js란 무엇입니까?

Next.js는 Zeit에 의해 만들어진 리액트 프레임워크이며 nextjs.org에 따르면 다음과 같다.

> Next.js를 사용하면 데이터가 어디에서 오든 서버 렌더링 반응 응용 프로그램이 더 쉬워집니다.

next.js는 정적 내보내기(및 새로 추가된 정적 재생)도 지원하지만, 이 게시물의 목적상 위에서 언급한 SSR(서버 렌더링) 기능에 초점을 맞추고 있다.

### Create React App이란?

시작 페이지에 따라:

> Create React App은 공식적으로 지원되는 단일 페이지 React 응용 프로그램 생성 방법입니다.

다시 한 번, 이 게시물의 목적상, 우리는 "한 페이지"라는 용어에 주목하고 있다.

### SSR 대 CSR 비교

Next.js는 반응을 활용하여 SSR(서버 측 렌더링)을 지원하는 한 가지 방법입니다. 마찬가지로 Create React App은 반응을 활용하여 CSR(클라이언트 사이드 렌더링)을 지원하는 한 가지 방법입니다.

어느 한 가지 선택이든 다른 프레임워크가 있지만, 이 게시물에서 우리가 실제로 비교하고 있는 것은 각 렌더링 전략이 웹 응용 프로그램 성능에 어떻게 영향을 미치는지에 대한 것입니다. 우리는 그 비교를 위해 두 가지의 더 대중적인 프레임워크를 사용하고 있습니다.

## 실험: Next.js vs. 반응 앱 만들기

Next.js 대 Next.js를 비교할 때 물어볼 수 있는 많은 질문 중 하나로 실험을 시작하겠습니다. React App 생성: SSR는 애플리케이션 성능을 향상합니까?

### 가설

Walmart Labs는 "클라이언트 측면 렌더링에 대한 서버 측면 렌더링의 이점"이라는 제목의 훌륭한 게시물을 게시했습니다. 또한 SSR와 CSR 성능 간의 근본적인 차이를 보여주는 몇 가지 우수한 다이어그램도 제공합니다.

이러한 다이어그램은 SSR가 CSR보다 더 빠르게 브라우저에 HTML을 제공할 수 있다고 가정하므로, SSR로 구축된 웹 애플리케이션이 CSR로 구축된 애플리케이션보다 성능이 뛰어나다는 가설을 제시해 보겠습니다.

(참고: 이 문서에서는 동기식 SSR를 렌더ToString의 병목 지점으로 불러옵니다. 이는 반응 16 및 섬유 향상과 무관합니다.

### 테스트 매개 변수

우리의 가설을 시험하는 가장 좋은 방법은 동일한 기능과 UI를 가진 두 개의 애플리케이션을 구축하는 것이다. 가능한 실제 응용 프로그램을 모방하길 원하므로 몇 가지 매개 변수를 설정할 것입니다.

응용 프로그램은 다음을 수행해야 합니다.

- API에서 데이터 가져오기
- 사소한 양의 콘텐츠 렌더링
- 일부 JavaScript 가중치 적용

### 모바일 사항

소프트웨어 개발자는 일반적으로 초고속 사무실 네트워크와 페어링된 고성능 컴퓨터에 의해 작동되지 않습니다. 우리는 항상 사용자들과 같은 방식으로 애플리케이션을 경험하지는 않습니다.

이러한 점을 염두에 두고 성능을 최적화할 때는 네트워크 및 CPU 제한을 모두 고려하는 것이 중요합니다. 모바일 장치는 일반적으로 처리 능력이 떨어지기 때문에 자바스크립트 파일 구문 분석 및 값비싼 렌더링으로 성능이 저하될 수 있다.

다행히 크롬은 라이트하우스라는 개발 도구를 제공해 사용자의 입장에서 쉽게 발을 들여놓고 그들의 경험을 이해할 수 있게 해준다. Chrome DevTools의 감사 탭에서 확인할 수 있습니다.

위에 표시된 정확한 설정을 사용합니다.

- 모바일 장치
- 고속 3G 적용, 4배 CPU 속도 저하
- 저장 지우기

### 지리가 중요하다.

북부 캘리포니아에 살고 있고 AWS us-west-1(N.California)에 하루 종일 상주하는 서버에 있는 경우, 미국 다른 지역이나 세계 다른 지역의 사용자와 동일한 방식으로 애플리케이션을 경험하지 못하고 있습니다.

따라서 이 테스트를 위해 데모 앱과 API가 호주 시드니(특히 Zeit`s sid1 지역)에 배포되었습니다. 클라이언트의 브라우저는 미국 CO 볼더에서 애플리케이션에 액세스할 것입니다.

볼더와 시드니 사이의 거리는 8,318 mi이다.

이 두 응용 프로그램 간의 데이터 가져오기가 무엇을 의미하는지 살펴 보십시오.

### 애플리케이션 2개, API 1개

두 앱의 코드는 내 GitHub에서 사용할 수 있다.

응용 프로그램은 다음과 같습니다.

- 반응 앱 만들기
- next.js

모든 코드는 모노레포입니다.

- `/cra`에 응용 프로그램의 Create React App 버전이 포함되어 있습니다.
- `/nextjs`에 Next.js 버전이 포함되어 있습니다.
- /api`에는 두 응용 프로그램이 모두 사용하는 모의 API가 포함되어 있습니다.

UI는 동일하게 나타납니다.

JSX는 거의 동일합니다.

```js
// Create React App
<ThemeProvider>
  <div>
    <Text as="h1">Create React App</Text>
    <PrimaryNav align="left" maxItemWidth="20rem">
      <NavItem href="/" selected>Create React App</NavItem>
      <NavItem href="/nextjs">Next.js</NavItem>
    </PrimaryNav>
    <Table
      data={users}
      rowKey="id"
      title="Users"
      hideTitle />
  </div>
</ThemeProvider>
```

```js
// Next.js
<ThemeProvider>
  <div>
    <Text as="h1">Next.js</Text>
    <PrimaryNav align="left" maxItemWidth="20rem">
      <NavItem href="/">Create React App</NavItem>
      <NavItem href="/nextjs" selected>Next.js</NavItem>
    </PrimaryNav>
    <Table
      data={users}
      rowKey="id"
      title="Users"
      hideTitle />
  </div>
</ThemeProvider>
```

잠시 후 테마 제공자와 기타 구성요소에 대해 알아보겠습니다.

그러나 API에서 데이터를 가져오는 방법은 다음과 같습니다.

```js
// Create React App
// This all executes in the browser
const [users, setUsers] = useState([]);
useEffect(() => {
  const fetchData = async () => {
    const resp = await axios.get('/api/data');
    const users = resp.data.map(user => {
      return {
        id: user.id,
        FirstName: user.FirstName,
        DateOfBirth: moment(user.DateOfBirth).format('MMMM Do YYYY'),
      }
    });
    setUsers(users);
  };
  fetchData();
}, []);
```

```js
// Next.js
// This all executes on the server on first load
Index.getInitialProps = async({ req }) => {
  const resp = await axios.get(`http://${req.headers.host}/api/data`);
  const users = resp.data.map(user => {
    return {
      id: user.id,
      FirstName: user.FirstName,
      DateOfBirth: moment(user.DateOfBirth).format('MMMM Do YYYY'),
    }
  });
  return { users };
}
```

getInitialProps는 Next.js에서 페이지의 초기 데이터를 채우는 데 Next.js가 사용하는 특수 함수이다. 문서에서 Next.js를 사용하여 데이터를 가져오는 방법에 대해 자세히 알아볼 수 있습니다.

### 그렇다면 이 모든 구성 요소는 무엇이며, 왜 Memote.js를 사용하고 있습니까?

원래의 테스트 매개 변수로 돌아가면, 우리는 적어도 생산에 선적할 수 있는 것과 다소 비슷한 응용 프로그램을 사용하여 테스트하려고 합니다. 테마 제공자 PrimaryNav 등은 모두 미네랄 UI라는 UI 컴포넌트 라이브러리에서 나왔다.

또한 구성 요소 트리를 렌더링할 때 발생해야 하는 일부 자바스크립트 가중치와 추가 처리를 추가하는 것이 더 큰 종속성이기 때문에 우리는 Mind.js를 끌어들이고 있다.

우리가 사용하고 있는 실제 라이브러리는 중요하지 않습니다. 중요한 것은 모든 것을 구축하는 데 시간을 들이지 않고도 일반 응용 프로그램의 무게에 조금 더 가까워지는 것입니다.

## next.js vs. 반응 생성: 성능 결과

다음은 각 응용 프로그램에 대한 전체 페이지 로드에 대한 등대 결과입니다.

이러한 메트릭의 세부 정보를 이해하려면 등대 채점 가이드를 참조하십시오.

우리의 목적에서 가장 주목할 만한 차이점 중 하나는 `최초의 의미 있는 그림`이다.

- CRA: 6.5초
- next.js: 0.8s

Google의 첫 번째 의미 있는 그림 문서에 따르면:

> 이 감사는 사용자가 페이지의 기본 내용이 보인다고 느끼는 시간을 식별합니다.

또한 등대는 이러한 차이를 시각화하는 데 도움이 됩니다.

위의 비주얼이 친숙해 보이나요? SSR가 CSR보다 더 빨리 브라우저에 HTML을 제공할 수 있다고 가정한 가설 섹션에 포함된 다이어그램을 모방하기 때문에 이러한 다이어그램이 필요합니다. 이 결과를 바탕으로, 할 수 있습니다!

Lighthouse 결과를 직접 보려면:

- CRA 및 Next.js용 파일 다운로드
- Chrome에서 https://googlechrome.github.io/lighthouse/viewer/ 열기
- 다운로드한 파일을 Chrome의 Lighthouse Viewer로 끌어다 놓으십시오.

## 결론

우리는 다음과 같은 질문을 가지고 실험을 시작했습니다. SSR는 애플리케이션 성능을 향상합니까?

거의 동일한 두 개의 애플리케이션을 구축했는데, 하나는 Create React App과 함께 클라이언트 측 렌더링을 사용하는 애플리케이션이고 다른 하나는 Next.js와 함께 서버 측 렌더링을 사용하는 애플리케이션입니다.

시뮬레이션의 Lighthouse 결과는 Next.js 애플리케이션에서 특히 첫 번째 의미 있는 그림판(87.69% 감소), 첫 번째 내용물 그림판(87.69% 감소) 및 대화형 시간(27.69% 감소)과 같은 모든 중요한 범주에서 더 나은 메트릭을 보여주었다.