---
layout: post
title: "13살짜리 아이에게 Netlifify에서 웹 페이지 호스팅 방법 가르치기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/hosting-webpage-netlify.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/hosting-webpage-netlify.png?fit=730%2C487&ssl=1)

Netliify는 개발자가 정적 사이트를 프로덕션에 배포하는 지루한 작업을 자동화할 수 있도록 지원하는 웹 개발 플랫폼입니다. Netlifify를 사용하면 정적 사이트를 자체 도메인이나 사용자 지정 가능한 Netlif 도메인으로 호스팅할 수 있습니다.

Netlifify의 많은 기능에는 지속적인 통합 및 지속적인 배포(CI/CD), 자체 백엔드를 구축하지 않고도 사용자로부터 데이터를 수집할 수 있는 인스턴트 양식, 분석, 서버 없이도 백엔드 작업을 수행할 수 있는 Netliify 기능 등이 있습니다. Netliify는 또한 견고성과 사용 편의성 때문에 JAM 스택 애플리케이션을 구축하는 데 일반적으로 사용됩니다.

이 기사에서는 Netlifify를 사용하여 웹 페이지를 호스팅하는 방법을 가장 간단한 용어로 설명합니다.

### 전제조건

이 문서는 코드 집약적이지 않으므로 코딩 경험이 많이 필요하지 않습니다. GitHub 계정과 Netliify 계정이 있어야 합니다. 여기서 Netliify 계정과 GitHub 계정을 만들 수 있습니다.

## 간단한 정적 웹 페이지 구축

우리가 호스팅할 웹 페이지는 최소 CSS가 있는 단일 HTML 파일로 구성되어 있습니다. HTML 파일의 코드는 다음과 같습니다.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            text-align: center;

        }
    </style>
</head>
<body>
    <div>
        <h1>Deploying a basic webpage to netlify</h1>
    </div>
</body>
</html>
```

이제 코드를 작성했으니 GitHub 저장소에 넣어야 합니다.

GitHub 저장소는 기본적으로 프로젝트의 소스 코드를 포함하는 원격 폴더이며, 이 경우 HTML과 CSS입니다. GitHub 저장소(또는 구어체에서 부르는 repo)의 코드를 사용하면 Netliify와 같은 많은 서비스와 상호 작용할 수 있다. 또한 팀의 다른 구성원들과 협력하여 단일 코드베이스에 기여할 수 있습니다.

깃과 깃허브는 모든 개발자가 최소한 기본적인 업무 능력을 갖춰야 하는 필수적인 도구이지만, 우리는 이 글에서 그것들을 깊이 있게 논의하지 않을 것이다. 그러나 우리는 깃허브 저장소에 우리의 코드를 넣는 과정을 밟을 것이다.

먼저 여기서 GitHub 계정을 만들어야 합니다. 작업이 완료되면 새로운 GitHub 저장소를 만들어야 합니다. GitHub 홈페이지 오른쪽 상단 모서리에 있는 더하기(+) 아이콘을 클릭하고 New Repository(새 리포지토리)를 선택하면 됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/github-create-new-repo.jpeg?resize=730%2C389&ssl=1)

리포지토리의 이름을 지정하고 공용(다른 사용자가 볼 수 있도록 허용)을 선택한 후 [README 파일 추가] 확인란을 선택하여 README 파일로 리포지토리를 초기화합니다.

이제 우리는 다음과 같은 것을 가져야 한다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/repo-readme-file.jpeg?resize=730%2C418&ssl=1)

GitHub 저장소에 파일을 추가하는 가장 일반적인 방법은 명령줄의 Git 명령을 통해 파일을 추가하는 것이지만 Git/GitHub에만 초점을 맞춘 가이드는 아니기 때문에 단순히 저장소에 `index.html` 파일을 드래그 앤 드롭합니다.

이렇게 하려면 파일 추가 드롭다운을 전환하고 파일 업로드를 선택합니다. 그러면 `index.html` 파일을 간단히 삭제할 수 있는 새로운 페이지로 이동하게 됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/github-drag-add-files.jpeg?resize=730%2C238&ssl=1)

파일을 삭제한 후 Commit changes 버튼을 클릭하면 변경 내용이 GitHub 저장소에 추가됩니다. 이제 리포지토리의 홈페이지에서 index.html 파일을 볼 수 있습니다.

이제 GitHub 저장소에 웹 페이지의 코드가 있으므로 여기에 Netlifify 계정을 만들어야 합니다.

로그인한 후 새 팀을 만들면 Netlifify 대시보드는 다음과 유사하게 보입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/netlify-dashboard.jpeg?resize=730%2C244&ssl=1)

이제 GitHub 계정을 Netliify에 연결해야 합니다. 이렇게 하려면 Git에서 새 사이트 단추를 클릭하고 원하는 Git 공급자로 GitHub를 선택합니다. GitHub 계정이 Netliify에 연결되면 웹 페이지의 리포지토리를 선택할 수 있습니다. 이제 다음과 같은 페이지가 나타납니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/adding-new-site-netlify.jpeg?resize=607%2C611&ssl=1)

이 페이지에서 앱을 배포하는 데 사용할 빌드 설정을 구성할 수 있습니다. 간단한 웹 페이지만 배포하기 때문에 여기서는 많은 작업을 수행할 필요가 없습니다. 배포할 분기 옵션을 마스터(또는 코드가 있는 분기)로 설정하고 사이트 배포를 클릭합니다.

이제 배포된 웹 페이지의 대시보드로 리디렉션해야 합니다. 다음과 같은 경우가 많습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/deployed-page-dashboard-netlify.jpeg?resize=730%2C272&ssl=1)

배포된 웹 페이지를 체크 아웃하려면 녹색으로 강조 표시된 링크를 클릭하십시오.

이제 적절한 지점에서 리포지토리에 새로운 변경 사항을 적용할 때마다 이러한 변경 사항이 몇 분 내에 배포된 사이트에 반영된다는 사실을 알아야 합니다.

### Netliify 드롭

Netliify Drop이라고 하는 Netliify에서 사이트를 호스팅하는 더 빠르고 덜 일반적인 방법도 있습니다. Netliify Drop은 개발자가 사이트의 폴더를 드래그 앤 드롭하기만 하면 사이트를 호스팅할 수 있는 Netliify 서비스입니다.

이를 사용하려면 Netlifify Drop 웹 사이트를 방문해야 합니다. 사이트에 접속한 후에는 제공된 공간에 정적 웹 사이트의 모든 파일이 들어 있는 폴더를 삭제하기만 하면 이 문서의 앞부분과 유사한 대시보드로 리디렉션됩니다. 그런 다음 새로 배포된 사이트나 페이지에 대한 링크를 거기서 가져올 수 있습니다.

### 사용자 지정 도메인

이제 Netlifify에서 웹 페이지를 호스트하는 방법을 알았으므로 도메인 이름을 사용자 지정하는 방법에 대해 살펴보겠습니다. Netlifify는 일반적으로 고유한 Netlifify 도메인을 제공하며 사용자 지정할 수 있습니다. 현재 도메인 이름을 사용자 지정하려면 Netliify의 프로젝트 대시보드 페이지로 이동하여 도메인 설정을 클릭해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/adding-custom-domain-netlify.jpeg?resize=730%2C303&ssl=1)

이제 옵션 드롭다운을 전환하고 사이트 이름 편집을 클릭해야 합니다. 이제 사이트의 도메인 이름을 편집할 수 있는 입력 필드가 표시됩니다. 그러나 이 도메인 이름은 여전히 Netlifify 하위 도메인이며 프로젝트에 자체 도메인을 사용하고 싶은 경우가 있습니다.

그러기 위해서는 Namecheap과 같은 사이트에서 구입할 수 있는 자체 사용자 지정 도메인이 필요합니다. 사용자 지정 도메인이 있으면 프로젝트의 도메인 설정 Netlifine 페이지로 이동하여 사용자 지정 도메인을 추가하려면 Add custom domain(사용자 지정 도메인 추가)

이제 도메인 이름을 입력해야 하는 입력 필드가 나타납니다. 사용자 지정 도메인을 입력하고 예, Netlifify에서 확인을 요청하면 도메인 추가를 클릭합니다.

이제 도메인에 대한 Netliify DNS를 설정할 수 있는 도메인 설정 페이지로 다시 리디렉션됩니다. 기본 도메인의 DNS 구성 확인 링크를 클릭하면 이 페이지로 리디렉션됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/configuring-netlify-dns.jpeg?resize=730%2C371&ssl=1)

확인을 클릭하고 마지막 단계로 계속 진행합니다. 여기서 Netlifify는 도메인의 이름 서버를 업데이트하라는 메시지를 표시합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/netlify-domain-namerservers.jpeg?resize=730%2C428&ssl=1)

도메인을 구입한 사이트에서 제공하는 대시보드에서 이 작업을 수행해야 합니다. 완료되면 완료를 클릭하면 다시 홈페이지로 리디렉션됩니다. 이제 기다리기만 하면 됩니다(내 경우처럼 몇 분 또는 몇 시간이 걸릴 수 있음). 이제 도메인 이름이 웹 페이지를 가리켜야 합니다.

도메인 설정 페이지에서 보안을 위해 사이트/웹 페이지에서 HTTPS를 사용하도록 설정할 수도 있습니다. DNS 구성을 확인하기만 하면 HTTPS를 사용할 수 있습니다.

## 결론

축하합니다! 사용자 지정 도메인을 사용하여 Netlifify에서 간단한 웹 페이지를 호스팅하고 HTTPS로 안전하게 보호했습니다. Netlifify를 통해 얻을 수 있는 다른 놀라운 사항들도 확인해 보시기 바랍니다. 언제든지 Netliify의 공식 웹사이트에서 필요한 모든 정보를 찾을 수 있습니다.