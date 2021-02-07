---
layout: post
title: "스트라이프를 사용하여 Ract Native 앱 수익화"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/monetizing-react-native-app-stripe.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/monetizing-react-native-app-stripe.png?fit=730%2C487&ssl=1)

## 준비

이 게시물을 읽기로 선택하셨으니, 자바스크립트 생태계가 얼마나 빨리 진화하는지 잘 알고 계실 것입니다. 웹을 정복한 후, 모바일 앱의 경우 리액트 네이티브, 데스크톱 앱의 경우 일렉트로닉과 같은 툴로 현재 토종 앱 업계에서도 만연하고 있다.

요즘은 React Native로 구축된 앱과 실제 기본 스택으로 구축된 앱 간의 격차가 점점 좁혀지고 있습니다. 인디와 대기업 앱 개발자에게 모두 혜택이 돌아가는 주요 내용 중 하나는 큰 소란 없이 앱 내 결제를 처리할 수 있다는 점이다.

앱에 대한 좋은 아이디어가 있다면, 어떻게 돈을 벌고 가능한 한 마찰을 적게 가지고 사용자들로부터 직접 결제를 받을 수 있는지 알아보는 것은 시간문제이다. 따라서 이 게시물에서는 React Native(Expo를 통해), UI Kitten, Stripe 및 백엔드 Node.js 프레임워크인 AdonisJs를 사용하여 전체 앱을 구축하는 과정을 안내하겠습니다.

후속 작업을 시작하기 전에 이 게시물은 일반적으로 Git, React Native, React, TypesScript 및 REST API의 기본 사항에 대해 잘 알고 있다고 가정합니다.

만약 여러분이 이런 것들을 처음 접하는 사람이라면, 여러분은 여전히 읽으면서 여러분이 할 수 있는 것을 배우는 것을 환영할 것입니다. 그러나 이 게시물을 계속해서 주제에 대해 게시하기 위해서는 게시물 전체에 걸쳐 하위 레벨의 많은 세부 사항들이 설명되지 않을 것입니다. 또한 React Native 앱 개발을 위한 환경이 설정되어 있는지 확인하십시오.

서버 앱과 클라이언트 앱이 있는 이런 어플리케이션은 보통 컨테이너 디렉토리에 넣어서 프로젝트 이름을 붙입니다. 저는 이름이 정말 서툴러서 돈을 주는 것보다 더 좋은 게 떠오르지 않았는데, 이것은 단지 지불이 부족해서 돈을 요구하는 것처럼 들립니다. 오, 그럼!

먼저 `payme`라는 이름의 새 디렉토리를 생성하고 터미널에서 해당 디렉토리 내부를 탐색합니다.

```bash
mkdir payme
cd payme
```

저처럼 마음이 조급하고 어떤 일에 전념하기 전에 코드를 먼저 보고 싶다면, 여기 전체 코드베이스를 포함하는 GitHubrepo가 있습니다. 파고들어요! 🙂 또는 최종 결과를 보고 싶다면, 간단한 비디오 미리보기가 있습니다.

## 엑스포 앱

Expo는 React Native를 기반으로 구축된 플랫폼/툴 키트로 개발자의 작업 환경을 훨씬 쉽고 원활하게 지원합니다. 스테로이드에 대한 RN이라고 생각할 수 있다. 기본 기능에 대해서는 몇 가지 제한이 있지만, 항상 그런 종류의 해결 방법이 있습니다.

먼저 전 세계에 `expo-cli`가 설치되어 있어야 하며 CLI를 사용하여 새로운 보일러 플레이트 앱을 생성할 것입니다.

```coffeescript
npm install -g expo-cli
expo init payme
```

두 번째 명령은 템플릿을 선택하라는 메시지를 표시하고 화살표 키를 사용하여 아래 이미지와 같이 목록에서 두 번째 템플릿을 선택합니다. 그러면 빈 TypeScript Expo 앱이 생성됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/blank-typescript-expo-app.png?resize=730%2C122&ssl=1)

내가 엑스포에 몇 가지 제한이 있다고 말했던 거 기억나? 죄송합니다. 그 중 하나를 맞췄습니다. 그리고 곧 그 자리에 오를 수 있습니다!

만약 당신이 이것 iOS와 함께 일하게 할 필요가 있기 때문 엑스포 iOS에 아직은 스트라이프 모듈을 지원하지 않으면 기본적인 작업 흐름 대신 관리되는 사용해야 할 것이다. 자세한 내용은 Expo의 이 설명서를 참조하십시오.

그러나 이 게시물에 작성할 모든 코드는 베어 워크플로와 관리 워크플로 모두에서 호환됩니다.

완료되면 `payme`라는 이름의 새 디렉토리가 남게 됩니다. 서버측 API 앱과 클라이언트측 Expo 앱이 있으니 생성된 앱 폴더의 이름을 `페이미`에서 `앱`으로 바꾸자. 그런 다음 곧 사용할 몇 가지 패키지를 설치하십시오.

```java
mv payme app
cd app
expo install @react-native-community/async-storage @ui-kitten/eva-icons @ui-kitten/components @eva-design/eva react-native-svg expo-payments-stripe
```

설치가 실행되는 동안 브랜드에 대한 UI Kitten 설명서와 앱에 사용할 기본 색상 팔레트로 테마 파일을 생성하는 방법을 살펴보십시오. 설명서의 단계에 따라 여러 가지 색상이 있는 테마.json을 구입해야 합니다. 이 파일을 앱 디렉터리의 루트에 넣습니다.

마침내, 우리는 코딩에 도달할 수 있다! `App.tsx` 파일을 열고 기존 코드를 다음과 같이 바꿉니다.

```js
import { StatusBar } from 'expo-status-bar';
import React, {useEffect, useState} from 'react';
import * as eva from '@eva-design/eva';
import { ApplicationProvider, IconRegistry } from '@ui-kitten/components';
import { default as theme } from './theme.json';
import { EvaIconsPack } from '@ui-kitten/eva-icons';
import { AuthPage } from "./AuthPage";
import { ProductPage } from "./ProductPage";
import { Auth } from "./auth";
import { Payment } from "./payment";

const auth = new Auth();
const payment = new Payment(auth);
export default function App() {
    const [isLoggedIn, setIsLoggedIn] = useState(false);
    const [isLoggingIn, setIsLoggingIn] = useState(true);

    useEffect(() => {
        auth.getToken().then((token) => {
            setIsLoggingIn(false);
            if (!!token) setIsLoggedIn(true);
        });
    });

    return (
        <>
            <IconRegistry icons={EvaIconsPack} />
            <ApplicationProvider {...eva} theme={...eva.light, ...theme}>
                <StatusBar style="auto" />
                { isLoggedIn
                    ? <ProductPage {...{payment} />
                    : <AuthPage {...{auth, setIsLoggedIn, setIsLoggingIn, isLoggingIn} />
                }
            </ApplicationProvider>
        </>
    );
}
```

이 코드 중 일부는 그냥 판에 불과하지만, 좀 더 세분화해 봅시다. 생성한 사용자 정의 테마로 UI Kitten을 설정하기 위해 테마.json 파일을 가져와 테마 객체를 Application Provider로 전달합니다.

우리는 지정된 파일에서 Auth와 Payment라는 두 개의 클래스를 가져오고 인스턴스화하고 있습니다. auth 객체는 종속성으로 payment 클래스에 주입됩니다. 이는 비즈니스 로직을 캡슐화하고 서버 API와 통신합니다.

이 구성 요소에는 두 가지 상태 변수가 있습니다. 하나는 사용자가 이미 로그인했는지 여부를 나타내고 다른 하나는 현재 로그인 작업이 수행 중인지 여부를 나타냅니다.

처음에는 로그인 모드에서 UI를 로드하고 로드 시 `auth.get`을 사용하여 기존 인증 정보를 확인합니다.토큰()포함 사용자가 한 번 로그인하면 앱을 열면 매번 로그인 화면이 표시되지 않기 때문에 이 작업이 필요합니다.

그런 다음 UI Kitten에서 아이콘 세트를 등록하고 레이아웃을 설정하고 상태 표시줄, yada, yada, yada, yada… UI Kitten의 공식 문서에서 복사/붙여넣은 더 많은 보일러 플레이트 자료를 표시합니다. 이 게시물이 게시된 후 오랫동안 이 문서를 읽고 있는 경우, 문서를 따라와 이 설정과 일치시키십시오.

마지막으로 로그인 상태에 따라 사용자가 로그인하지 않으면 AuthPage 구성 요소를 렌더링하고, 그렇지 않으면 ProductPage 구성 요소를 렌더링합니다. `ProductPage` 구성 요소는 결제 개체만 [rp][rp]로 가져옵니다.

그러나 AuthPage 구성 요소는 구성 요소 자체에서 발생하는 상호 작용을 기반으로 글로벌 상태를 변경할 수 있어야 합니다. 이것이 우리가 모든 상태 변수와 세터 함수를 통과시켜 변수를 수정하는 이유입니다.

앱이 커짐에 따라 이러한 종류의 빠르고 더러운 상태 관리 방식은 더 이상 이를 축소하지 않을 수 있으며, React Context와 같은 도구나 Redux나 Recil과 같은 고급 상태 관리 라이브러리를 선택해야 하지만, 이 게시물의 제한된 범위의 경우에는 이 방법이 효과적일 것입니다.

### 인증 페이지

인증은 프로젝트를 시작할 때 가장 먼저 수행해야 하는 작업 중 하나일 수 있습니다. 그러나 인증은 많은 에지 사례와 앱의 상황별 종속성으로 인해 가장 복잡한 작업 중 하나입니다.

이 게시물의 목적을 위해, 우리는 그것을 간단하고 간결하게 유지할 것입니다. 앱이 열리자마자 이메일 주소와 비밀번호로 가입하거나 이미 계정이 있는 경우 로그인할 수 있는 로그인 화면을 사용자에게 보여주려고 합니다.

먼저 앱의 루트에 `AuthPage.tsx`라는 새 파일을 생성하고 아래 코드를 삭제하십시오.

```coffeescript
import {Layout, Icon, Button} from "@ui-kitten/components";
import { Layout, Card, Button, Input, Text } from "@ui-kitten/components";
import { StyleSheet, View } from "react-native";
import React, { useState } from "react";

import { Auth } from "./auth";

const styles = StyleSheet.create({
    page: {
        flex: 1,
        padding: 15,
        alignItems: 'center',
        justifyContent: 'center',
    },
    card: {
      alignSelf: 'stretch',
    },
    formInput: {
        marginTop: 16,
    },
    footer: {
        marginTop: 10,
        alignSelf: 'stretch',
        flexDirection: 'row',
        justifyContent: 'space-between',
    },
    statusContainer: {
        alignSelf: 'center',
    },
    actionsContainer: {
        flexDirection: 'row-reverse',
    },
    button: {
        marginLeft: 10,
    }
});

type AuthPageProps = {
    auth: Auth,
    isLoggingIn: boolean,
    setIsLoggedIn: (isLoggedIn: boolean) => any,
    setIsLoggingIn: (isLoggedIn: boolean) => any,
};

export const AuthPage = ({ auth, isLoggingIn, setIsLoggedIn, setIsLoggingIn }: AuthPageProps) => {
    const [password, setPassword] = useState<string>();
    const [email, setEmail] = useState<string>();
    const [errors, setErrors] = useState<string[]>([]);

    const handlePrimaryButtonPress = async (action = 'signup') => {
        setIsLoggingIn(true);
        // when signing up, we want to use the signup method from auth class, otherwise, use the login method
        try {
            const { success, errors } = await auth.request(action, {email, password});
            setIsLoggedIn(success);
            setErrors(errors);
        } catch (err) {
            console.log(err);
        }

        setIsLoggingIn(false);
    };

    return (
        <Layout style={styles.page}>
            <Card style={styles.card} status='primary'>
                <Input
                    style={styles.formInput}
                    label='EMAIL'
                    value={email}
                    onChangeText={setEmail}
                />
                <Input
                    style={styles.formInput}
                    label='PASSWORD'
                    secureTextEntry={true}
                    value={password}
                    onChangeText={setPassword}
                />
                {errors.length > 0 && errors.map(message =>
                    <Text key={`auth_error_${message}`} status='danger'>{message}</Text>
                )}
            </Card>
            <View style={styles.footer}>
                <View style={styles.statusContainer}>
                    <Text>{isLoggingIn ? 'Authenticating...' : ''}</Text>
                </View>
                <View style={styles.actionsContainer}>
                    <Button
                        size="small"
                        style={styles.button}
                        disabled={isLoggingIn}
                        onPress={() => handlePrimaryButtonPress('signup')}>
                        Sign Up
                    </Button>
                    <Button
                        size="small"
                        status="info"
                        appearance="outline"
                        disabled={isLoggingIn}
                        onPress={() => handlePrimaryButtonPress('login')}>
                        Log In
                    </Button>
                </View>
            </View>
        </Layout>
    );
}
```

이상적으로는 실제 응용프로그램에서 사용자 자신의 아키텍처와 폴더 구조를 가질 수 있지만, 이 게시물의 목적과 범위를 위해 앱 폴더의 루트에 있는 모든 항목을 삭제합니다.

많은 코드처럼 보이지만, 저를 믿으세요: 실제 인증 페이지에서, 적어도 이것보다 5배는 더 많은 코드를 가질 수 있습니다. 두려워하지 마세요. 분해해서 덩어리로 소비하는 것은 꽤 쉽습니다.

먼저, 몇 가지 스타일시트 정의가 있습니다. UI 빌딩이 이 게시물의 주요 범위에 있지 않기 때문에, 이 게시물의 작동 이유와 방법에 대해서는 자세히 설명하지 않겠습니다. 이것을 완전히 처음 접하셨다면, 여기에서 자세히 읽어보시기 바랍니다. 요약하자면, `AuthPage` 구성 요소가 제공하는 다양한 요소에 나중에 적용할 몇 가지 스타일을 추가하는 것입니다.

그런 다음 이 구성 요소가 렌더러로부터 받을 것으로 예상되는 모든 다양한 소품을 정의하여 유형을 생성합니다.

구성 요소 자체에는 세 가지 상태 변수가 있습니다. 첫 번째 두 가지는 사용자가 UI를 통해 입력하는 문자열이 될 전자 메일과 암호입니다. 그러면 오류 메시지가 포함된 문자열이 배열됩니다. 이러한 오류는 인증 요청에 대한 응답으로 서버 측에서 수신할 수 있는 오류입니다. 가입을 위한 중복 전자 메일 주소, 로그인에 대한 잘못된 암호 등과 같은 오류를 상상해 보십시오.

다음으로, 사용자가 어떤 버튼을 누르느냐에 따라 로그인 또는 등록 작업에 대해 트리거되는 이벤트 핸들러 기능이 있습니다. 함수의 본체 안에서, 우리는 부모에게 무슨 일이 일어나고 있는지 알리기 위해 구성요소로 전달되는 다양한 상태 설정 함수를 호출한다.

마지막으로, 우리는 주의 이메일과 암호를 사용하여 `auth.request(action, {email, password})` 메서드를 호출합니다. 이렇게 하면 데이터가 API 서버로 전송되고 응답을 받을 수 있습니다.

이제 사용자가 실제로 볼 수 있는 것을 살펴보겠습니다. 이렇게 멋지고 예쁘게 만들기 위해 UI Kitten의 Layout과 Card 구성 요소를 사용하고 화면 중앙에 전자 메일용과 암호용 두 개의 입력으로 카드를 놓습니다. 예를 들어, 암호 필드를 분석하겠습니다.

```xml
<Input
   style={styles.formInput}
   label='PASSWORD'
   secureTextEntry={true}
   value={password}
   onChangeText={setPassword}
/>
```

우리는 위에 좋은 마진을 주는 `폼 입력` 스타일을 사용한다. 그런 다음 `라벨` 받침대는 입력 필드 위에 텍스트를 추가합니다. `secureTextEntry`는 사용자가 암호에 입력할 때 텍스트를 숨깁니다. onChangeText 이벤트의 핸들러로서 상태 값을 업데이트하는 setPassword 기능을 전달합니다. 그런 다음 이 값은 입력 필드 자체의 값으로 설정됩니다.

예쁜 표준 UI 키튼 마법의 스프레이로 반응합니다. 테스트에 따라 소품을 전달하기만 하면 이러한 입력을 보고 느낄 수 있도록 할 수 있는 일이 훨씬 더 많습니다. 자세한 내용은 이 내용을 읽어 보십시오.

아래에서는 모든 오류 메시지 문자열을 상태로부터 루프하고 메시지 자체를 나타내는 텍스트 구성 요소(있는 경우)를 렌더링한다. UI Kitten 덕분에 status=`t` 부분이 빨간색으로 표시됩니다.

```xml
{errors.length > 0 && errors.map(message =>
                    <Text key={`auth_error_${message}`} status='danger'>{message}</Text>
                )}
```

마지막으로 바닥글을 살펴봅니다. 오른쪽의 두 개의 수평 섹션으로 나누어진 이 섹션에는 로그인용 단추와 등록용 단추의 두 가지가 표시됩니다. 왼쪽에는 기본적으로 비어 있는 영역이지만 서버와 통신할 때 Authentication...이라는 텍스트가 표시되어 요청을 처리하고 있음을 사용자에게 알립니다.

두 버튼은 동일한 기능을 호출하지만 파라미터는 다릅니다. 그들은 또한 약간 다른 소품들을 가지고 있는데, 예를 들어, 테두리를 주고 배경색을 약간 희미하게 만드는 `contence=`가 있다. disabled={isLogging}도 전달합니다.In}에서 사용자가 버튼을 누르고 데이터를 서버로 전송하면 여러 번 제출하지 않도록 두 버튼을 모두 사용할 수 없습니다.

휴! 앱의 첫 번째 구성 요소가 완성되었습니다. 나쁘지 않죠? 그런데 나쁜 소식은 `auth` 객체나 `PaymentPage` 구성요소와 같은 조각이 없어져서 아직 그것을 실제로 볼 수 없다는 것이다. 하지만, 지금까지의 당신의 노고에 감사드리기 위해, 앱이 페이지를 영광스럽게 만들어 준다면 어떻게 보일지에 대한 스크린샷으로 보답하겠습니다. 보라구!

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/login-page-preview.png?resize=730%2C1298&ssl=1)

알았어, 미안, 내가 너무 과장해서 말한 것 같아. 별로 놀랍지는 않지만, 그래도 그렇게 나쁘진 않죠?

### 제품 페이지

인증 페이지와 마찬가지로 앱의 루트에 `ProductPage.tsx`라는 새 파일을 생성하고 아래 코드를 삭제해 보겠습니다.

```js
import {Layout, Icon, Button} from "@ui-kitten/components";
import { StyleSheet } from "react-native";
import React, {useEffect, useState} from "react";
import {Payment, PaymentData} from "./payment";

const styles = StyleSheet.create({
    container: {
        flex: 1,
        flexDirection: 'row',
        alignItems: 'center',
        justifyContent: 'center',
    },
    button: {
        margin: 2
    }
});

const products = [{
    status: 'primary',
    text: 'Buy Video',
    product: 'video',
    icon: 'film',
    price: 50,
}, {
    price: 30,
    status: 'info',
    product: 'audio',
    text: 'Buy Audio',
    icon: 'headphones',
}];

type ProductPageProps = {
    payment: Payment,
};

export const ProductPage = ({ payment }: ProductPageProps) => {
    const [paymentReady, setPaymentReady] = useState(false);
    const [paymentHistory, setPaymentHistory] = useState<PaymentData[]>([]);

    // Initialize the payment module, on android, this MUST be inside the useEffect hook
    // on iOS, the initialization can happen anywhere
    useEffect(() => {
        payment.init().then(() => {
            setPaymentReady(true);
            setPaymentHistory(payment.history);
        });
    });

    const handlePayment = async (price: number, product: string) => {
        await payment.request(price, product);
        setPaymentHistory(payment.history);
    };

    const hasPurchased = (product: string) => !!paymentHistory.find(item => item.product === product);

    return (
        <Layout style={styles.container}>
            {products.map(({product, status, icon, text, price}) => (
                <Button
                    status={status}
                    style={styles.button}
                    disabled={!paymentReady || hasPurchased(product)}
                    onPress={() => handlePayment(price, product)}
                    key={`${status}_${icon}_${text}`}
                    accessoryLeft={props => <Icon {...props} name={icon}/>}>
                    {`${text} $${price}`}
                </Button>
            ))}
        </Layout>
    );
};
```

여기 있는 코드를 좀 더 자세히 살펴봅시다. 이전과 마찬가지로, 나중에 다른 구성 요소에 추가할 몇 가지 스타일 정의가 있습니다.

그러면 제품 항목 두 개가 포함된 배열이 있습니다. 각 항목에는 상태, 가격, 제품, 아이콘, 텍스트 속성이 있습니다. 가격 및 제품 속성을 제외하고 모두 UI를 위한 것이며, 이전 두 가지는 나중에 API 통신에 사용될 것이다. 실제 앱에서는 이 목록을 앱 코드에 하드 코딩하는 대신 서버에서 가져올 수 있습니다.

구성요소 자체의 정의에서, 우리는 `App` 컴포넌트에서 전달한 결제 객체를 소품으로 받도록 설정했습니다, 기억하십니까? 로드 시, 우리는 init()를 호출하여 결제 모듈을 초기화하고 UI를 결제 모듈 로딩 상태와 동기화하기 위해 처음에 `false`로 설정되고 결제 모듈을 초기화할 때 켜지는 상태 변수를 도입한다.

또한 사용자가 이전에 지불한 모든 금액을 포함하는 또 다른 상태 변수가 있습니다. 결제 객체에서 init()를 호출한 후 결제 객체에서 history 속성을 가진 상태 변수를 재설정합니다. 초기화 후 내역 속성에 결제 내역이 포함된 것처럼. 우리는 그것이 어떻게 처리되는지 나중에 볼 것이다.

우리는 또한 가격 및 제품 데이터로 트리거되어 결제 모듈에서 요청 방식을 호출하는 이벤트 핸들러 기능인 `핸들 페이먼트`도 가지고 있습니다. 통화가 종료된 후, 우리는 마치 결제 후 기록 자산이 이전과 다른 데이터를 포함하는 것처럼 결제 대상의 `이력` 속성을 가진 `결제 내역` 상태를 재조정하고 있습니다.

우리는 당분간 이러한 모든 방법을 억제하기 위해 지불 대상을 맹목적으로 신뢰하고 있지만, 걱정은 하지 않습니다. 우리는 곧 그것을 직접 건설할 것입니다.

그런 다음 UI 렌더링이 나타납니다. UI Kitten의 "Layout" 컴포넌트 안에서, 우리는 단순히 두 제품을 반복하여 각각의 제품에 대해 하나의 버튼을 렌더링하고 있다. 여기서 product 객체에 설정된 status, text 등의 속성을 볼 수 있습니다.

UI Kitten을 사용하면 버튼과 같은 다양한 구성 요소의 외관을 간단한 속성을 통해 쉽게 조작할 수 있습니다. 예를 들어, `status="primary"는 테마의 정의와 기본 색상을 기준으로 버튼의 스타일을 구분합니다.

또한 결제 초기화가 완료되지 않았거나 동일한 제품에 대한 결제 항목이 이미 있는 경우 해당 버튼을 비활성화하고 있습니다. 여기에 기록된 다양한 소품들을 변경/추가하여 UI 룩과 느낌을 가지고 놀 수 있습니다.

이전에 만든 핸들 결제 기능을 온프레스 이벤트의 수신기로 사용하고 있으며, 처음에 만든 다양한 스타일을 사용하여 버튼 사이의 간격을 더하고 화면 중앙에 수직으로 배치하고 있습니다.

이 모든 것이 어떻게 보이는지 보여드리기 위해, 여기 스크린샷이 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/product-page-preview.png?resize=730%2C1298&ssl=1)

그것으로, 우리는 사물의 프런트 엔드에서 잠시 휴식을 취하고 API를 구축할 것입니다. 준비가 되면, 우리는 다시 이 두 가지를 연결하고 마무리할 것입니다!

## 아도니스 앱

Expo와 마찬가지로, AdonisJs는 편리한 보일러 플레이트 생성기 명령을 제공합니다. 이를 실행하려면 `payme/` 디렉토리 내에 있는지 확인하고 다음 명령을 실행하십시오.

```undefined
yarn create adonis-ts-app payme
```

몇 가지 질문을 받고 아래 이미지와 같은 답변을 제공하면 payme이라는 이름의 디렉토리가 남게 됩니다.

이전과 마찬가지로, 우리는 이것을 모노레포 구조에 더 적합한 이름인 mvpayme api로 바꿀 것이다. 이제 디렉터리 내부를 탐색하고 앱을 실행할 수 있습니다.

```bash
cd api
yarn start
```

앱이 실행 중이며 포트 3333의 로컬 호스트에서 액세스할 수 있음을 알려주는 출력이 표시됩니다.

### 인증

도니스는 많은 유연성으로 사용자 인증을 즉시 쉽게 만들어 줍니다. 이제 곧 사용할 패키지인 yarn add stripe @adonisjs/auth@alpha를 설치하겠습니다.

우리가 그곳에 설치한 첫 번째 패키지는 스트라이프용 노드 패키지입니다. 두 번째는 Adonis의 auth 패키지로, `node acce incall @adonisjs/auth`를 실행하고 필요한 모든 입력을 제공한 후 사용자 인증에 필요한 모든 것을 제공합니다.

다음은 제가 제공한 답변이며, 이를 위해 생성되는 모든 파일을 볼 수 있습니다.

Lucid를 아직 설정하지 않은 데이터 스토리지 공급자로 사용하고 있습니다. 공식 아도니스 데이터베이스 ORM 계층입니다. 먼저 패키지 자체를 설치하여 설정합니다. `yarn add @adonisjs/lucid@alpha`. 데이터베이스 공급자를 선택하라는 메시지가 나타납니다. 일반적으로 MySQL이나 Postgre 같은 것을 선택할 수 있습니다.SQL, 하지만 이 게시물의 작은 범위를 위해서, 저는 SQLite를 고수할 것입니다.

이제 Adonis auth 패키지에 의해 생성된 마이그레이션을 실행할 준비가 되었습니다. 이렇게 하면 JWT 기반 사용자 인증을 지원하는 적절한 스키마가 있는 필수 테이블이 생성됩니다.

> 참고: 이 게시물을 작성할 때 Adonis에서 패키지가 설정되는 방식에 문제가 있으며 'yarn add pc-argon2' 명령을 실행하여 pc-argon2 패키지를 수동으로 설치해야 할 수 있습니다.

이 기본 앱의 경우 등록 및 로그인 작업만 설정하지만 실제로는 암호 재설정, 전자 메일 확인 등을 통해 훨씬 복잡한 흐름을 구축해야 합니다.

등록 및 로그인을 위해서는 REST 끝점이 두 개 필요하며, 이러한 끝점은 일반적으로 아도니스의 경로 및 컨트롤러를 통해 관리된다. 이제 `start/routs.ts` 파일에 이러한 끝점을 생성하고 기존 코드를 제거하겠습니다.

```coffeescript
import Route from '@ioc:Adonis/Core/Route';

Route.post('/login', 'AuthController.login');
Route.post('/signup', 'AuthController.signup');
```

그러면 POST 요청 끝점인 `/로그인`과 `/가입`이 두 개 등록되고, 요청이 해당 끝점으로 전송되면 Adonis는 `AuthController`라는 컨트롤러 클래스에서 관련 메서드를 실행합니다.

그러나 아직 존재하지 않으므로 `app/Controller/Http/AuthController.ts`에 새 파일(및 필요한 디렉터리, 즉 `Controller` 및 `Http`)을 생성하거나 `node ace make:Controller auth` 명령을 실행하면 파일과 디렉터리가 생성됩니다. 이제 컨트롤러 파일의 다음 코드에 배치하십시오.

```undefined
import { HttpContextContract } from '@ioc:Adonis/Core/HttpContext'
import User from 'App/Models/User';

export default class AuthController {
  public async login ({ request, auth }: HttpContextContract) {
    const email = request.input('email');
    const password = request.input('password');

    const token = await auth.use('api').attempt(email, password);
    return token.toJSON();
  }
  public async signup ({ request, auth }: HttpContextContract) {
    const email = request.input('email');
    const password = request.input('password');

    const user = await User.create({email, password});
    const token = await auth.use('api').generate(user);
    return token.toJSON();
  }
}
```

로그인 방식은 POST 요청 본문으로 이메일과 비밀번호가 전송될 것으로 예상하며, 주입된 auth 모듈을 이용해 아도니스는 해당 인증서에 일치하는 사용자 항목이 데이터베이스에 있는지 여부를 시도 방식으로 확인한다.

사용자가 있으면 토큰을 발급하여 응답으로 보냅니다. 토큰 응답에 두 가지 속성 `{token: jwactual jwt 토큰`이 있습니다. 유형: <type of the 토큰, 보통 베어러>

여기서는 등록 방법이 약간 단순하지만 실제 응용 프로그램에서는 이름, 주소 등과 같은 다른 필드가 필요할 수 있습니다. 이 경우 로그인 방법과 동일한 데이터(전자 메일 및 암호)가 필요합니다. 이 두 가지를 사용하면 데이터베이스에 새 사용자 항목을 작성한 다음 새로 생성된 사용자에 대한 토큰을 생성하여 발신자에게 응답으로 다시 전송된다.

위의 설정을 React Native 앱을 위해 방금 만든 AuthPage 구성 요소의 상대라고 생각해 보십시오. 그래서 이제 우리의 API에 남은 것은 `PaymentPage` 컴포넌트의 상대입니다. 그걸 구축해 봅시다. 하지만 아도니스가 제작 모듈처럼 우리를 위해 모든 것을 만들어주지 않을 것이기 때문에, 우리는 더 경치 좋은 길을 택할 것입니다.

### 데이터 구조

기능을 처음부터 설계할 때는 대개 세분화된 데이터 계층에서 시작하는 것이 좋습니다. 기능이 필요로 할 수 있는 모든 것을 지원할 수 있는 데이터베이스 수준의 데이터 구조를 제시합니다. 아도니스에서 일하는 방법은 이주를 통해서이다.

마이그레이션을 통해 데이터베이스와 상호 작용하고 테이블 및 테이블 내부 열을 만들거나 업데이트할 수 있습니다. 우리의 경우, 우리는 Stripe를 사용하여 결제하는 방법을 시연하기 위한 앱을 만들고 있습니다. 그래서 우리가 데이터베이스에 보관하고자 하는 것은, 모든 지불에 대해, 한 지불에 대한 모든 세부 정보가 포함된 한 행의 테이블에 있는 것입니다.

적절하게, 우리는 그 표를 `지급`이라고 부르고 싶다. 말씀드렸듯이, 데이터베이스에 해당 테이블을 수동으로 생성하는 대신 node ace make:migration payment 명령을 실행하여 결제 테이블 마이그레이션을 생성할 것입니다. 그런 다음 `api/database/migration/` 디렉토리의 `_payments.ts`로 끝나는 생성된 마이그레이션 파일을 다음과 같은 새 줄로 업데이트하십시오.

```undefined
      table.increments('id');
//--> new lines start
      table.integer('user_id').unsigned().references('id').inTable('users').onDelete('CASCADE');
      table.string('charge_id').unique().notNullable();
      table.string('token_id').notNullable();
      table.string('product').notNullable();
      table.integer('price').notNullable();
//--> new lines end
      table.timestamps(true);
```

이제 방금 작성한 코드를 살펴보기 전에 `node ace migration:run` 명령을 실행하여 해당 코드 조각에 설명된 구조로 새 테이블을 생성해 보겠습니다.

네, 마이그레이션이 실행되는 동안 기존 부분부터 자세히 살펴보겠습니다. 우리는 지불이라는 이름의 새로운 표를 만들고 있는데 id란 이름의 필드는 테이블의 기본 키이며 새로운 행이 추가될 때마다 자동으로 증가된다.

기본적으로 타임스탬프(true)를 번역하는 타임스탬프(true)도 타임스탬프 필드인 created_at과 update_at라는 두 개의 열을 추가하는 마술이 벌어지고 있다. 이제 새 필드의 경우:

앞서 언급했듯이 마이그레이션은 데이터베이스 수준에서 데이터 구조만 생성하므로 앱에서 처리할 데이터의 종류를 여전히 알려줘야 합니다. 모델들이 들어오는 곳이죠.

`node ace make:model payment` 명령을 실행하여 결제 모델을 생성하면 `app/Models/Payment.ts`에 새 파일이 생성됩니다. 파일을 열고 다음 코드를 넣습니다.

```java
import { DateTime } from 'luxon'
import { BaseModel, column, belongsTo, BelongsTo } from '@ioc:Adonis/Lucid/Orm'

import User from 'App/Models/User';

export default class Payment extends BaseModel {
  @column({ isPrimary: true })
  public id: number

  @column()
  public price: number

  @column()
  public product: string

  @column()
  public tokenId: string

  @column()
  public chargeId: string

  @column()
  public userId: number

  @belongsTo(() => User)
  public user: BelongsTo<typeof User>

  @column.dateTime({ autoCreate: true })
  public createdAt: DateTime

  @column.dateTime({ autoCreate: true, autoUpdate: true })
  public updatedAt: DateTime
}
```

보다시피, 이것은 우리가 마이그레이션에 대해 무엇을 했는지를 보여주는 거울이지만 더 많은 TypeScript-esque입니다. 마이그레이션 시 타임스탬프(true) 호출에 의해 처리된 모델 수준에서 타임스탬프 필드의 철자를 지정합니다.

여기서 주목해야 할 또 다른 점은 ORM에게 모든 결제 항목에 상위 사용자가 있다는 것을 알려주는 관계 비트입니다.

```coffeescript
  @belongsTo(() => User)
  public user: BelongsTo<typeof User>
```

이것이 우리가 교묘하게 설계된 Lucid ORM을 사용하여 모든 종류의 관계형 질의를 실행할 수 있게 하는 것이다. 우리는 지불 제어기에 적어도 하나의 예를 곧 보게 될 것이다. 이렇게 하면 결제 모델에서 사용자 모델로의 연결만 설정됩니다. 사용자 모델이 결제 모델과의 관계를 알기 위해서는 앱/모델/사용자.ts로 들어가서 다음 줄을 추가해야 합니다.

```coffeescript
import {
  column,
  beforeSave,
  BaseModel,
  hasMany,
  HasMany,
} from '@ioc:Adonis/Lucid/Orm';

//....previously generated code
export default class User extends BaseModel {
//....previously generated code

  @hasMany(() => Payment)
  public payments: HasMany<typeof Payment>
}
```

> 참고: 관계형 데이터 구조에 대해 잘 모르거나 아도니스가 이를 어떻게 다루는지 자세히 알고 싶으시면 여기를 읽어보십시오.

이제 한 단계 더 올라서 HTTP 요청을 통해 데이터 관리를 공개해 보겠습니다. 이 단계에는 세 가지가 관련되어 있습니다.

### 1.) 경로

경로는 외부 세계가 HTTP를 통해 아도니스 앱과 통신할 수 있는 방법을 제공합니다. 다음 내용으로 `start/route.ts` 파일에 새 REST 끝점을 만드는 것부터 시작하겠습니다.

```undefined
Route.get('/payment', 'PaymentsController.list').middleware('auth');
Route.post('/payment', 'PaymentsController.charge').middleware('auth');
```

우리는 `/결제` 끝점에 GET 요청에 대한 처리기를 하나 추가하고 있으며, 요청이 도착하면, 아도니스에게 `결제 컨트롤러` 클래스의 메소드를 `목록`으로 표시하라고 말합니다. 다른 처리기는 동일한 엔드포인트의 POST 요청에 대한 것이며 컨트롤러 클래스에서 `충전` 메서드를 실행합니다. 미들웨어(auth)도 조만간 폄훼할 것으로 보인다.

### 2.) 컨트롤러

`node ace make:controller payment` 명령을 실행하여 결제 컨트롤러를 생성하면 `app/Http/Controller/Payment.ts`에 새 파일이 생성됩니다. 해당 파일을 열고 다음 코드에 배치합니다.

```js
import { HttpContextContract } from "@ioc:Adonis/Core/HttpContext";
import Env from '@ioc:Adonis/Core/Env'
import Payment from "App/Models/Payment";
import Stripe from 'stripe';

const stripeSecretKey = `${Env.get('STRIPE_SECRET_KEY')}`;
const stripe = new Stripe(stripeSecretKey, {
  apiVersion: '2020-08-27'
});

export default class PaymentsController {
  public async charge ({ request, auth }: HttpContextContract) {
    const payment = new Payment();
    payment.tokenId = request.input('tokenId');
    payment.price = request.input('price');

    const { id } = await stripe.charges.create({
      amount: payment.price * 100,
      source: payment.tokenId,
      currency: 'usd',
    });
    payment.chargeId = id;

    await auth.user?.related('payments').save(payment);
    return payment;
  }
  public async list ({ auth }: HttpContextContract) {
    const user = auth.user;
    if (!user) return [];
    await user?.preload('payments');
    return user.payments;
  }
}
```

짐 풀어야 할 게 많아요. 처음부터 다시 시작하겠습니다.

```js
import Stripe from 'stripe';

const stripeSecretKey = `${Env.get('STRIPE_SECRET_KEY')}`;
const stripe = new Stripe(stripeSecretKey, {
  apiVersion: '2020-08-27'
});
```

인증 라이브러리를 설치할 때 Stripe를 설치했습니다. 여기가 우리가 사용하는 곳입니다.

가져온 후에는 비밀 키로 Stripe를 인스턴스화합니다. 그러나 보안 모범 사례를 위해 키를 하드 코딩하는 대신 env 변수를 쉽게 읽을 수 있는 아도니스의 env 제공자를 사용하고 있다. api 디렉토리의 루트에서 `.env`를 업데이트하고 다음과 같은 새 항목을 추가합니다.

```undefined
STRIPE_SECRET_KEY=<test secret key from your stripe account>
```

비밀 열쇠를 얻으려면 스트라이프 계정으로 그 개발자-> API키로 이동하여 로그인 할 수도 있겠군요. 그런 다음 오른쪽 상단 모서리에서 테스트 데이터 보기를 전환합니다. 이제 새 비밀키를 만들 수도 있고, 이미 비밀키를 가지고 있다면 얼마든지 사용하세요.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/viewing-secret-key-data.png?resize=730%2C242&ssl=1)

실제 비용을 지출하거나 실제 카드를 충전하지 않고도 결제 기능을 테스트할 수 있기 때문에 테스트 데이터를 사용하고 있다는 점을 명심하십시오. 앱을 만들고 프로덕션에 릴리스하면 이 키를 실제 키로 교체해야 합니다.

다음으로 충전 방법은 다음과 같습니다.

```undefined
  public async charge ({ request, auth }: HttpContextContract) {
    const payment = new Payment();
    payment.tokenId = request.input('tokenId');
    payment.product = request.input('product');
    payment.price = request.input('price');

    const { id } = await stripe.charges.create({
      amount: payment.price * 100,
      description: payment.product,
      source: payment.tokenId,
      currency: 'usd',
    });
    payment.chargeId = id;

    await auth.user?.related('payments').save(payment);
    return payment;
  }
```

여기서는 고객이 Stripe `tokenId`, `가격`, `상품`을 보내 줄 것으로 예상하고 있으며, 이를 통해 새로운 결제 모델 객체를 인스턴스화합니다. 저장하기 전에 charge.create() 메서드를 호출하여 사용자의 카드를 충전하려고 합니다. 스트라이프는 센트로 처리되기 때문에 우리가 청구하고자 하는 총 금액이 50달러라면 청구 금액으로 5000달러를 보내야 합니다.

또한 화폐를 `usd`로 하드코딩하고 있지만, 실제 앱에서는 사용자 카드나 다른 앱 수준 변수를 기준으로 조정할 수 있습니다. 사용자의 카드를 충전할 때 Stripe에 보낼 수 있는 데이터가 훨씬 많습니다. 자세한 내용은 여기에서 API 설명서를 참조하십시오.

요금 청구에 성공하면 Stripe는 나중에 트랜잭션 정보를 검색하는 데 사용할 수 있는 ID(기타 많은 정보)를 반환합니다. 그것이 우리가 `chargeId`란 아래 데이터베이스에 저장할 유일한 데이터입니다.

Stripe가 카드를 충전할 수 없는 경우(카드 정보가 잘못되었거나, 카드가 만료되었거나, 다른 이유가 있을 경우)는 취급하지 않으며, `chargeId` 대신 오류 정보를 반환합니다. 나는 그것을 독자들을 위한 연습으로 남겨두고 있다.

그런 다음 결제 모델을 사용하여 직접 삽입하는 대신 행을 삽입하기 위해 `user.related(`payments)`.save()를 사용합니다. 이렇게 하면, 아도니스는 자동으로 로그인한 사용자와 결제 항목을 연결합니다.

특정 `auth` 개체가 사용자에게 전달되어 사용자 속성에 액세스할 수 있습니다. 인증 미들웨어에 의해 자동으로 주입되며 사용자 모델에 액세스할 수 있습니다. 사용자 모델에서 hasMany 관계를 설정했기 때문에, Adonis는 자동으로 하위 항목을 상위 항목에 연결하기 위한 세부 정보를 입력할 수 있다.

두 번째 방법인 list는 좀 더 간단하다. GET 요청을 하는 로그인한 사용자의 `결제` 테이블에서 모든 항목을 찾아 반환하기만 하면 됩니다.

```undefined
  public async list ({ auth }: HttpContextContract) {
    const user = auth.user;
    if (!user) return [];
    await user?.preload('payments');
    return user.payments;
  }
```

충전 방식처럼 사용자 모델에 액세스할 수 있는 `프리로드(preload)`를 실행하여 로그인한 사용자와 관련된 모든 결제 항목을 로드할 수 있습니다.

### 미들웨어

보안을 위해 로그인하지 않은 사용자는 결제 요청을 하지 않습니다. 모든 요청을 수동으로 검증하는 것은 지루하고 위험한 프로세스이기 때문에 아도니스는 미들웨어 사용을 통해 요청을 매우 쉽게 수행할 수 있습니다.

이전 단계에서, 우리는 우리의 컨트롤러 방법에 `auth` 물체가 주입되는 것을 보았는데, 그것은 미들웨어의 마법 때문에 가능한 것이다. 하지만 우리는 마법의 일을 하기 위해 약간의 일을 해야 합니다.

경로 정의에 .middleware(`auth`)를 이미 추가했습니다. 우리가 그것을 작동시키기 위해 해야 할 마지막 일은 미들웨어를 등록하는 것이다. `start/kernel.ts` 파일을 열고 아래쪽에서 `Server.middleware.registerName({})`처럼 보이는 줄을 다음과 같이 바꿉니다.

```css
Server.middleware.registerNamed({
  auth: 'App/Middleware/Auth',
});
```

이제 아도니스는 우리가 `auth` 미들웨어의 의미를 알게 되었고, 인증되지 않은 요청이 컨트롤러 방법에 도달하는 것을 막기 위한 모든 논리는 이미 인증 생성기에 의해 우리에게 생성되었다.

## 두 앱 결합

이제 REST API를 모든 엔드포인트에 사용할 수 있게 되었으므로 두 엔드포인트를 연결하는 마지막 단계로 넘어갑시다. 클라이언트와 서버 간에 통신하기 위해 전체 npm 라이브러리가 필요한 시대는 오래 전에 지났지만, 요즘은 `페치`만 있으면 됩니다. 먼저 앱/디렉토리의 루트에 `api.ts` 파일을 만들고 다음 코드를 넣는 것으로 시작합니다.

```js
const API_URL = 'http://192.168.1.205:3333';

export const postDataToApi = async ({endpoint = '', data = {}, headers = {}) => {
    const response = await fetch(`${API_URL}/${endpoint}`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            ...headers
        },
        body: JSON.stringify(data)
    });
    return response.json();
};

export const getDataFromApi = async ({endpoint = '', headers = {}) => {
    const response = await fetch(`${API_URL}/${endpoint}`, {
        headers: {
            'Content-Type': 'application/json',
            ...headers
        },
    });

    return response.json();
};
```

여기서는 API 끝점을 하드 코딩하고 가리켰지만 IP 부분은 사용자마다 다릅니다. 그러기 위해서, 여러분은 이런 것을 따라갈 수 있습니다. 앱은 모바일 기기에서 실행되지만 API 앱은 다른 기기에서 실행되기 때문에 이것이 필요합니다. 두 장치가 모두 동일한 네트워크에 연결되어 있다고 가정하면 IP 주소 컨테이너 192.168 접두사를 얻을 수 있습니다.

그러면 POST 요청을 하는 것과 GET 요청을 하는 것의 두 가지 기능이 있습니다. 두 기능 모두 끝점과 추가 헤더 매개 변수를 지정할 수 있습니다. POST 요청의 경우 요청 본문에 추가 `데이터`를 전달할 수도 있습니다.

API 서버와 통신하는 데 필요한 것은 이것뿐입니다. 우리는 이러한 도우미를 이용하여 사례별로 서버에 요청을 할 것입니다.

### auth 클래스

우리가 다루는 첫 번째 사례는 `Auth` 모듈이다. 앱/디렉토리의 루트에 `auth.ts`라는 새 파일을 만들고 그 안에 다음 코드를 넣어보자.

```undefined
import AsyncStorage from '@react-native-community/async-storage';
import { postDataToApi } from "./api";

interface AuthData {
    email: string | undefined;
    password: string | undefined;
}

export class Auth {
    tokenKey: string = '@authKey';

    async request(endpoint = 'login', data: AuthData) {
        const { token, errors } = await postDataToApi({ endpoint, data });
        if (token) {
            await AsyncStorage.setItem(this.tokenKey, token);
            return { success: true, errors: [] };
        }

        const errorMapper: (params: { message: string }) => string = ({ message }) => message;
        return { success: false, errors: errors.map(errorMapper) };
    }

    async getToken() {
        return AsyncStorage.getItem(this.tokenKey);
    }

    async getApiHeader() {
        const token = await this.getToken();
        return { 'Authorization': `Bearer ${token}` };
    }
}
```

이 클래스는 `postDataToApi` 도우미 기능을 사용하여 API 서버로 이메일 및 암호 자격 증명을 보내는 `요청` 방식으로, 잘못된 내용에 대한 세부 정보가 포함된 개체를 반환합니다. 토큰을 받으면 장치에 데이터를 로컬로 유지하는 가장 빠른 방법인 AsyncStorage 패키지를 사용하여 로컬 스토리지에 저장하는 것입니다.

로그인/가입 요청에 문제가 있을 경우 API는 오류가 포함된 개체 배열로 응답하고 각 개체에는 `메시지` 속성이 포함됩니다. 이 데이터를 단순화하기 위해 어레이를 통해 매핑하고 루프를 통과하여 사용자에게 표시할 수 있는 문자열 배열로 변환합니다.

그렇다면, 우리는 `get`이 있다.이전에 저장한 토큰을 스토리지에서 검색하는 토큰 방법. 이를 앱 구성 요소가 보기에 로드되면 호출합니다. 마지막 방법은 저장된 토큰으로 `Authorization`을 생성하는 도우미로, API 서버에 HTTP 요청과 함께 전송할 수 있습니다.

기존 토큰을 사용할 필요가 없기 때문에 여기서는 사용하지 않고 있지만 `결제` 클래스에서 사용할 수 있습니다.

### '지불'클래스

`app/` 디렉토리의 루트에 새 파일을 만들고 그 안에 코드를 넣으십시오.

```js
import { PaymentsStripe as Stripe } from 'expo-payments-stripe';
import {Auth} from "./auth";
import {getDataFromApi, postDataToApi} from "./api";

type PaymentData = {
    id: number,
    price: number,
    product: string,
};

export class Payment {
    auth: Auth;
    history: PaymentData[] = [];

    constructor(auth: Auth) {
        this.auth = auth;
    }

    async init() {
        await Stripe.setOptionsAsync({
            androidPayMode: 'test',
            publishableKey: 'pk_test_<rest_of_your_key>',
        });
        await this.getHistory();
        return true;
    }

    async getHistory() {
        const response = await getDataFromApi({endpoint: 'payment', headers: await this.auth.getApiHeader()});
        if (response.errors) {
            this.history = [];
            return false;
        }

        if (response.length > 0) {
            this.history = response;
            return true;
        }
    }

    async request(price: number, product: string) {
        try {
            const { tokenId } = await Stripe.paymentRequestWithCardFormAsync();
            const payment: PaymentData = await postDataToApi({
                endpoint: 'payment',
                data: {price, token: tokenId, product},
                headers: await this.auth.getApiHeader()
            });

            if (payment.id)
                this.history.push(payment);

            return payment;
        } catch(err) {
            console.log(err);
            return null;
        }
    }
}
```

이것은 단순히 서버 API 요청을 하는 것 이상의 역할을 합니다. 우선 인스턴스화 과정에서 auth급이 투입될 것으로 기대하고 있다. 그러면 Expo의 Stripe 라이브러리를 게시 가능한 키로 인스턴스화하는 init 방법이 있습니다.

게시 가능한 키를 가져오려면 서버 측 스트라이프 충전 메커니즘을 설정할 때 암호 키를 얻기 위해 수행한 것과 동일한 단계를 수행합니다. 암호 키와 함께 같은 페이지에 게시 가능한 키가 표시됩니다. Stripe를 인스턴스화한 후, 우리는 또 다른 클래스 메소드를 getHistory라고 부르고 있다.

getHistory는 현재 사용자가 지불한 모든 이전 결제를 검색하기 위해 API 서버에 대한 간단한 GET 요청입니다. HTTP 요청에서 현재 사용자를 식별하기 위해 `this.auth.getApiHeader()` 메서드 호출을 사용하여 인증 토큰 헤더를 전달합니다. 이 요청은 `/결제` 끝점으로 전송됩니다.

해당 엔드포인트에 대한 구현을 기억하면 인증된 사용자와 연결된 데이터베이스에서 모든 지불 행의 배열을 반환합니다. 따라서 응답에서 어레이를 수신하면 클래스의 `history` 속성에 저장하게 되는데, 이 속성은 구성 요소에서 상태를 업데이트하기 위해 사용됩니다.

request는 이 클래스의 마지막 방법입니다. 사용자가 제품 중 하나에서 구매 버튼을 누르면 가격과 제품 이름이 표시됩니다. 이 방법으로 우리가 가장 먼저 하는 일은 `Stripe.payment request`를 해제하는 것이다.카드 양식 Async() 호출을 사용하면 사용자가 카드 정보를 입력할 수 있는 멋진 모달모듈이 표시됩니다. 이것은 모두 Expo Stripe 라이브러리에 내장되어 있습니다. 다양한 구성 옵션을 이 호출에 전달하여 모달의 모양과 느낌을 사용자 정의할 수 있습니다. 다음은 모달의 모양을 보여주는 스크린샷입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/card-details-modal.png?resize=730%2C1298&ssl=1)

스트라이프는 테스트 카드 결제를 매우 간단하게 만듭니다. 기능을 테스트하기 위해 실제 카드가 필요하지 않습니다. 테스트 카드 세부 정보는 여기에서 확인할 수 있으며, 여기에서 테스트 카드를 사용하여 이 양식을 작성할 수 있습니다. 잘못된 카드 정보를 삽입하면 UI에서 자동으로 유효성 검사를 처리하고 오류를 표시합니다.

유효/테스트 카드를 입력하고 완료되면 Stripe가 다양한 정보를 개체로 반환하며, 그 중 가장 중요한 것은 `tokenId` 속성입니다. 유효한 카드 정보를 입력하고 완료를 눌렀다고 해서 카드가 충전된 것은 아닙니다. 그러기 위해서는 post 요청으로 tokenId를 API 서버에 전달해야 합니다.

우리는 이미 Stripe 라이브러리를 사용하여 charge() 메서드를 호출하도록 설정했습니다. 기억하십니까? 토큰 ID와 함께 가격과 제품명을 전달 중입니다. 가격은 달러로 전달되고 있지만 서버에서는 센트로 변환되고 요금 설명에 제품 이름이 추가된다는 점을 기억하십시오.

또한 어떤 사용자가 결제하는지 확인하기 위해 다시 한번 `this.auth.getApiHeader()` 방법을 사용하여 요청 헤더에 인증 토큰을 추가합니다.

일단 요금이 청구되면, 서버는 새로운 결제 항목을 만들어 우리에게 돌려줄 것입니다. 따라서 반환된 응답이 ID 속성을 가진 개체인 한, 결제가 성공했음을 알 수 있습니다. 서버 및 클라이언트 데이터의 동기화를 유지하기 위해 이 새로운 지불을 `history` 어레이 속성에 추가합니다. 이 호출 직후 구성 요소는 버튼을 사용하지 않는 `history` 속성을 사용하여 상태를 업데이트하기 때문입니다.

오류 사례를 취급하지 않습니다. 다시 한 번 말씀드리지만, 이 문제는 독자를 위한 연습으로 남깁니다.

아직도 나랑 같이 있어? 훌륭해! 여기까지 오느라 수고 많았어. 그리고 보상은 그만한 가치가 있을 거야. 약속할게.

## 그것을 한 바퀴 돌다.

이제 결제를 위해: `app/` 디렉토리에서 yarn start 명령을 실행하면 터미널에 QR 코드가 표시됩니다. 저는 이것을 테스트하기 위해 실제 기기를 사용하고 있지만, 원한다면 iOS나 안드로이드 에뮬레이터에서 쉽게 테스트할 수 있습니다.

아직 설치하지 않았다면 장치에 Expo 앱을 설치한 다음 열고 QR 코드를 스캔하십시오. 자동으로 단말기에서 앱을 로드하고 엽니다. 또한 API 서버가 여전히 실행 중인지 확인해야 합니다.

이 때 당신은 아까의 스크린샷처럼 인증 화면을 볼 수 있어야 합니다. 이메일 주소와 암호를 입력한 다음 등록을 눌러 계정을 만드십시오. 이렇게 하면 제품 페이지 부분의 스크린샷처럼 버튼 두 개가 있는 새 화면이 나온다.

해당 중 하나를 누르기 전에 앱을 닫고 다시 열어 다시 인증하라는 요청을 받지 않고 제품 페이지로 바로 이동되는지 확인했으면 합니다. 비동기 저장소 패키지 덕분에 앱을 종료해도 인증 토큰이 장치에 저장됩니다.

이제 버튼 중 하나를 누르면 위의 스크린샷과 같이 카드 입력 팝업이 나타납니다. 입력 정보를 가지고 놀면 실시간으로 다양한 검증이 이루어지는 것을 볼 수 있습니다.

유효/테스트 카드 세부 정보를 삽입하고 완료 키를 누르면 팝업이 나타나기 전에 방금 눌렀던 버튼이 비활성화됩니다. 이제 앱을 닫고 다시 열면 단추가 비활성화된 상태로 남아 있는 것을 볼 수 있습니다.

하지만 돈이 있을 때 무작정 UI를 신뢰할 수는 없습니다! 이제 스트라이프 대시보드로 이동하여 실제로 결제가 이루어졌는지 확인해 보겠습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/verify-payment-stripe-dashboard.png?resize=730%2C191&ssl=1)

대시보드의 왼쪽 사이드바에서 테스트 데이터 보기를 전환한 다음 결제 창으로 이동합니다. 결제 목록 위에 테스트 데이터 레이블이 표시되는지 확인하십시오. 이 라벨의 최신 항목은 앱에서 방금 생성한 항목이어야 합니다. 축하합니다! 방금 공짜로 돈을 벌었구나!

## 마감발언

우리가 함께 놀라운 앱을 만들었지만, 우리는 이것이 우리가 실제 사용자의 손에 넣었던 광택이 나거나 완성된 제품과는 거리가 멀다는 것을 인정해야만 한다. 그럼 지금 현재 진행 중인 것 외에 개선할 수 있는 몇 가지 사항을 계획해 보는 것은 어떨까요?

- API에서 검색한 제품의 정적 어레이 인앱에서 동적 저장된 데이터베이스 목록으로 제품 목록 이동
- 이미지, 설명, 제목 등을 추가하여 제품을 화려하게 보이게 합니다.
- 결제 목록 끝점을 사용하여 사용자가 구입한 제품 목록 표시
- 사용자가 제품을 구입하면 제품에 표시기를 표시하여 이미 구입했음을 사용자에게 알리십시오. 또한 구입 옵션을 사용하지 않도록 설정하는 대신 사용자가 선택한 경우 동일한 제품을 다시 구입할 수 있도록 합니다.
- 결제 요청이 성공한 경우 성공 팝업 표시

지금까지 해오셨다면, 110%는 독자 분 스스로 위의 모든 것을 실행할 수 있다고 확신합니다. 그리고 저는 모든 사람들이 그것을 시도해 볼 것을 강력히 권하고 싶습니다. 만약 당신이 당신의 Gitrepo나 상점에 게재된 완성된 앱으로 트위터를 통해 나에게 손을 내밀어 위의 모든 아이템을 완성한다면 나에게 알려주세요!