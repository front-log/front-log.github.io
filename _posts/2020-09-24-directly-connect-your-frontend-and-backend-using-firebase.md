---
layout: post
title: "Firebase를 사용하여 프런트 엔드 및 백엔드 직접 연결"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/firebase.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/firebase.png?fit=1240%2C700&ssl=1)

## 도입

새로운 프로젝트를 시작하고, 요구 사항을 포착하고, 사용할 기술을 선택하고, 데이터를 모델링하고, 팀이 코드를 만들 준비를 하고 있습니까? 독자 분이 솔로 개발자든 팀이든 프로젝트 시작 시 내리는 모든 결정에는 장단점이 있습니다.

Ruby on Rails 또는 Django와 같은 모노리스로 시작합니까, 아니면 SPA(Single Page Application)에서 사용할 API를 생성하여 프런트 엔드와 백엔드를 완전히 분리하는 것으로 시작합니까?

요즘 SPA와 Serverless가 대유행하고 있는 상황에서, 우리는 당신이 API와 SPA를 만들기로 결정했다고 가정하겠습니다. 이제 API 구축 및 호스팅 방법을 결정해야 합니다. 하지만 실제로 API가 필요한가요?

사용자 환경 및 SPA에 집중하면서 시간을 보내는 것이 좋겠습니까?

SPA에서 데이터스토어에 직접 안전하게 연결할 수 있는 툴이 있다면 어떻겠습니까? 우리는 노트 사용 권한을 포함할 회사용 노트 작성 애플리케이션을 구축할 것입니다.

오늘은 구글 클라우드의 파이어베이스라는 특별한 기술과 제품군에 초점을 맞추겠습니다.

Firebase는 무료 SSL 인증서와 글로벌 CDN, 인증, 데이터스토어, Blob 스토리지 등을 통해 프로젝트를 시작하는 데 필요한 모든 도구를 제공합니다.

자, 이제 그만 얘기해요. 이제 암호로 넘어가야 할 시간이에요.

나는 당신의 프런트 엔드 선택권에는 들어가지 않겠지만, 옥탄가가 도착한 지금 Ember나 React를 선호한다면 Nextjs를 적극 추천합니다. 이 말이 나온 김에, 나는 당신의 프로젝트를 진행시키기 위해 필요한 자바스크립트만 보여줄 것입니다.

그러나 시작하기 전에 https://firebase.google.com을 방문하여 무료 계정을 만드십시오.

시작하려면 Firebase CLI를 설치하고 Firebase 계정에 로그인합니다.

```shell
$: npm i -g firebase-tools
$: firebase login
```

SPA에서 이미 프로젝트를 설정했다고 가정하면, 호스팅, 인증 및 Firestore와 같이 사용할 Firebase 기능을 활성화해 보겠습니다.

```undefined
$: firebase init
? Which Firebase CLI features do you want to set up for this folder? Press Space
 to select features, then Enter to confirm your choices. 
 ◯ Database: Deploy Firebase Realtime Database Rules
 ◉ Firestore: Deploy rules and create indexes for Firestore
 ◯ Functions: Configure and deploy Cloud Functions
❯◉ Hosting: Configure and deploy Firebase Hosting sites
 ◯ Storage: Deploy Cloud Storage security rules
 ◯ Emulators: Set up local emulators for Firebase features
=== Project Setup
```

먼저 이 프로젝트 디렉토리를 Firebase 프로젝트에 연결하겠습니다.

`--add`를 사용하여 Firebase를 실행하여 여러 프로젝트 별칭을 만들 수 있지만, 지금은 기본 프로젝트만 설정합니다.

```undefined
? Please select an option: (Use arrow keys)
  Use an existing project 
❯ Create a new project 
  Add Firebase to an existing Google Cloud Platform project 
  Don't set up a default project 

i  If you want to create a project in a Google Cloud organization or folder, please use "firebase projects:create" instead, and return to this command when you've created the project.
? Please specify a unique project id (warning: cannot be modified afterward) [6-30 characters]: logrocket-notes

? What would you like to call your project? (defaults to your project ID) 
✔ Creating Google Cloud Platform project
✔ Adding Firebase resources to Google Cloud Platform project

🎉🎉🎉 Your Firebase project is ready! 🎉🎉🎉

Project information:
   - Project ID: logrocket-notes
   - Project Name: logrocket-notes

Firebase console is available at
i  Using project logrocket-notes (logrocket-notes)

=== Firestore Setup

Error: It looks like you haven't used Cloud Firestore in this project before. Go to https://console.firebase.google.com/project/logrocket-notes/database to create your Cloud Firestore database.
```

이제 여러분은 우리가 실수를 경험했다는 것을 눈치챘을지도 모릅니다. 그리고 이것은 제가 파이어베이스에 대해 좋아하는 것들 중 하나입니다. 그것은 여러분이 무언가를 해야 할 때를 알려주고 그것을 할 수 있는 링크를 줍니다!

이제 제공된 링크를 복사하고 데이터베이스 만들기를 선택하여 Firestore를 사용하도록 프로젝트를 설정합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/cloud-firestore-project-overview.png?resize=730%2C203&ssl=1)

기본적으로 데이터베이스를 시작할 규칙을 묻는 모달이 표시됩니다. 처음에 말씀드렸듯이, 이 규칙은 SPA/FE 클라이언트 앞에 있는 데이터베이스에 대한 액세스를 제어하는 데 사용됩니다. 그런 다음 Start in production(생산 모드에서 시작)을 선택합니다. 규칙을 사용하는 것은 처음부터 배우는 것이 좋다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/create-database.png?resize=730%2C483&ssl=1)

다음에는 사용자의 위치를 선택하라는 메시지가 표시됩니다. 사용자 및/또는 고객에게 가장 가까운 위치를 선택하고 몇 초 후에 데이터베이스를 만들 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/firebase-database.png?resize=730%2C563&ssl=1)

이제 데이터베이스를 설정했으므로 "firebase init" 명령을 다시 실행합니다. 다시 Firestore 및 Hosting을 선택하지만 프로젝트 선택에 대해서는 다시 묻지 않습니다.

이 경우 기존 프로젝트 사용을 선택하고 이전에 생성한 프로젝트 이름을 선택한 후 나머지 구성 과정을 거치면 됩니다.

```undefined
=== Firestore Setup

Firestore Security Rules allow you to define how and when to allow
requests. You can keep these rules in your project directory
and publish them with firebase deploy.

? What file should be used for Firestore Rules? (firestore.rules)
Firestore indexes allow you to perform complex queries while
maintaining performance that scales with the size of the result
set. You can keep index definitions in your project directory
and publish them with firebase deploy.

? What file should be used for Firestore indexes? (firestore.indexes.json)
=== Hosting Setup

Your public directory is the folder (relative to your project directory) that
will contain Hosting assets to be uploaded with firebase deploy. If you
have a build process for your assets, use your build's output directory.

? What do you want to use as your public directory? public
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
✔  Wrote public/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...
i  Writing gitignore file to .gitignore...

✔  Firebase initialization complete!
```

이제 우리는 갈 준비가 되었다! 이제 우리가 한 일을 살펴보자.

- Firebase 계정 생성
- 계정에 로그온됨
- 프로젝트 생성
- SPA 호스팅을 위한 Firebase 프로젝트 설정
- Firestore를 데이터베이스로 사용하도록 프로젝트 구성

"하지만 인증도 사용한다고 하셨는데!"라고 물으실 수 있습니다. 맞습니다.

Firebase Authentication을 사용할 예정이지만 Firebase CLI를 통해 구성하지는 않습니다. 잠시 후 이를 확인할 수 있습니다.

이제 프로젝트에서 다음과 같은 몇 가지 새로운 파일을 발견할 수 있습니다.

.firebaser //는 프로젝트 별칭 및 배포 대상을 관리합니다.

사격 기지json //`이(가) 필요하며, 프로젝트 디렉터리의 파일 및 설정이 Firebase 프로젝트에 배포되도록 지정합니다.

`firestore.rules //`는 Firestore 데이터베이스의 보안 규칙을 정의하는 데 사용되는 파일입니다.

`firestore.indexes.json //`은 Firestore 쿼리에 대한 인덱스를 정의하는 데 사용되는 파일입니다.

일부 데이터의 모델링을 시작할 때가 되었습니다. 하지만 Firestore가 NoSQL 문서 데이터 저장소라는 사실을 깨닫지 못한 경우 New York Times, Khan Academy 및 Now IMS를 비롯한 일부 대규모 조직 및 신생 기업에서 사용되고 있으며 MySQL 또는 Postgres를 사용하는 것과 다른 모델이 있을 수 있습니다.

나는 모델의 구조를 보여주기 위해 평범한 오래된 자바스크립트 객체를 사용할 것이다.

</users/{userId}

```css
User {
  firstName: string;
  lastName: string;
  avatar: string;
  email: string;
}
```

/notes/{noteId}

```undefined
Note {
  title: string;
  content: string;
  roles: {
    userId: string; // e.g., ['owner', 'reader', 'editor', 'commenter']
  }  
}
```

/notes/{noteId}/댓글/{commentId}

```css
Comment {
  user: {
    name: string;
    id: string;
  };
  content: string;
}
```

모델을 빠르게 살펴보겠습니다.

보시는 바와 같이 사용자 모델은 사용자와 관련된 일반 정보를 저장합니다. 우리는 사용자 모델에 역할을 부여할 수 있지만 이 간단한 게시물을 위해 사용자 역할을 `노트`에 올릴 것입니다.

이 보안 모델에 대한 절충이 있습니다. 예를 들어, 사용자 역할에 대한 사용자 역할을 저장했지만 사용자와의 잠재적인 문제에 대한 내부 노트를 가지고 싶다고 가정해 보겠습니다.

사용자 기록에 관리자 등 적절한 역할이 있으면 그에 대한 메모를 볼 수 있다. 노트(Note)에 역할을 정의함으로써 노트(Note)에 사용자를 초대하고 다른 사용자를 초대하지 못하도록 노트(Note)에 대한 권한을 명시적으로 설정하고 있습니다.

노트 모델에는 노트의 제목과 내용이 포함됩니다. 한 가지 흥미로운 점은 노트에 있는 역할 객체입니다. 이것은 `노트`에 대한 액세스를 제한하는 데 사용될 것이기 때문에 사용자도 이름을 붙일 수 있다.

여러분이 아셨겠지만, 코멘트 모델에는 noteId에 대한 필드가 없으며 우리는 그것을 추가할 필요가 없습니다. 우리는 확실히 할 수 있지만, `댓글`은 `노트`의 하위 컬렉션에 속한다. 즉, 이것을 REST API와 유사한 액세스 패턴으로 생각한다.

메모에 대한 모든 주석을 검색하기 위해 `어디` 쿼리를 사용할 필요는 없습니다. 데이터 검색을 시작할 때 이에 대해 자세히 알아보겠습니다.

코멘트 사용자 객체에 이름 및 id가 포함되어 있는 또 다른 관찰 결과가 있습니다.

NoSQL로 데이터를 모델링할 때는 액세스 패턴 또는 보기에서 데이터가 어떻게 사용될지에 따라 데이터를 모델링하는 것이 중요합니다. 일반적으로 코멘트가 있을 때 누가 코멘트를 작성했는지 알고 싶습니다.

SQL 데이터베이스를 사용하여 데이터를 조인하고 보기 계층으로 보냅니다. 그러나 NoSQL을 사용하면 해당 데이터를 추가하고 레코드에 복제할 수 있습니다. 간단하고 빠른 액세스 패턴을 제공합니다. 이를 비정규화된 데이터라고 합니다. 이제 코멘트를 조회할 때 코멘트의 작성자와 작성자의 이름을 알 수 있습니다.

기본 모델을 사용하지 않고 데이터 액세스 규칙을 작성하겠습니다. SQL과 달리 NoSQL 데이터베이스는 일반적으로 스케줄이 없습니다. 즉, 데이터 모델을 쉽게 확장할 수 있지만 애플리케이션 코드 내에서 데이터 구조를 적용해야 합니다.

Firestore의 좋은 점은 보안 규칙 내에서 스키마 규칙과 액세스 패턴을 처리할 수 있지만 이러한 액세스 패턴과 스키마 규칙은 Google Cloud Functions와 같은 기능을 통해 제공되는 관리 API 액세스에는 적용되지 않습니다.

`firestore.rules` 파일을 열고 `클라이언트 측` 액세스에 대한 규칙 추가를 시작합니다.

소방서.규칙

```coffeescript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

소방서 규칙은 매우 유연하며 요청에 따라 실행됩니다. 우리는 재사용이 가능하도록 기능을 작성할 수 있으며, 이 예에서 그렇게 할 것입니다.

소방서.규칙

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isAuthenticated() {
      return request.auth != null;
    }
    function getRole(resource) {
      // Read from the "roles" map in the resource
      return resource.data.roles[request.auth.uid]
    }
    function isOneOfRoles(resource, array) {
      // Determine if the user is one of any array of roles
      return isAuthenticated() && (getRole(resource) in array);
    }
    function onlyNoteContentChanged() {
      // Ensure only the Note's content has changed
      return request.resource.data.title == resource.data.title
      && request.resource.data.roles == resource.data.roles
      && request.resource.data.keys() == resource.data.keys();
    }
    match /users/{user} {
      // Any user can see other user profiles
      allow read: if isAuthenticated();
      // only the current user can write to their own profile
      allow write: if  request.auth.uid == user;
    }
    match /notes/{note} {
      // Any authenticated user can create a note
      allow create: if isAuthenticated();
      // Only the note owner is permitted to delete it
      allow delete: if isOneOfRoles(resource, ['owner']);
      // The owner is permitted to update the note including the title, content and add users
      // Editors are only permitted to update the content of the note.
      allow update: if isOneOfRoles(resource, ['owner']) 
        || (isOneOfRoles(resource, ['editor']) && onlyNoteContentChanged());
      allow read: if isOneOfRoles(resource, ['owner', 'editor', 'commenter', 'reader'])
      
      // the rules below apply to comments of the note
      // /notes/{note}/comments/{comment}
      match /comments/{comment} {
        // we are using a rules get query to retrieve the note and check the 
        // roles to ensure the user can infact 
        allow read: if isOneOfRoles(
          get(/databases/$(database)/document/notes/$(note)), 
          ['owner', 'editor', 'commenter', 'reader']
        );
        allow create: if isOneOfRoles(
          get(/databases/$(database)/document/notes/$(note)), 
          ['owner', 'editor', 'commenter']
        ) && request.resource.data.user.id == request.auth.uid;
      }
    }
  }
}
```

규칙 엔진은 우리가 사용할 수 있는 `요청`과 `자원` 변수를 제공합니다. 제공되는 항목에 대한 정보는 여기에서 확인할 수 있습니다. 규칙을 살펴보고 추가한 내용을 살펴보겠습니다.

is Authentication은 여러 가지 규칙 내에서 사용할 수 있는 재사용 가능한 도우미입니다.

getRole은 다른 재사용 가능한 도우미입니다. 사용자 인증 ID를 사용하여 노트 문서의 역할을 캡처하는 데 사용됩니다.

isOneOfRoles는 사용자가 인증되었는지 확인하고 사용자의 인증된 id가 작업을 수행하기에 적절한 역할을 가지고 있는지 확인하는 도우미 기능입니다.

"Only NoteContentChanged"는 문서의 데이터 구조를 확인하는 도우미입니다. 앞에서 설명한 것처럼 Firestore는 스키마가 없으며 애플리케이션 또는 Firestore 규칙에서 데이터 유효성 검사를 수행해야 합니다.

위의 각 규칙에 대해 인라인 코멘트를 작성했는데, 이는 꽤 설명이 되는 것입니다. 소방서 규칙 문서는 환상적이에요. 여기서 한번 읽어보세요.

보안 규칙을 업데이트했으면 다음을 구현해 보겠습니다.

```undefined
$ firebase deploy --only firestore:rules
=== Deploying to 'logrocket-notes'...

i  deploying firestore
i  cloud.firestore: checking firestore.rules for compilation errors...
✔  cloud.firestore: rules file firestore.rules compiled successfully
i  firestore: uploading rules firestore.rules...
✔  firestore: released rules firestore.rules to cloud.firestore

✔  Deploy complete!
```

시간 인증 작동시키기 위해. 이 작업을 완료하는 데 필요한 JavaScript만 제공합니다. Firebase는 Authentication 사용에 대한 훌륭한 설명서를 제공하며 여기에서 해당 설명서를 검토하는 것이 좋습니다.

단순성을 유지하기 위해 Firebase UI 구성 요소를 사용합니다.

```js
let ui = new firebaseui.auth.AuthUI(firebase.auth());
let uiConfig = {
  callbacks: {
    signInSuccessWithAuthResult: function (authResult, redirectUrl) {
      // User successfully signed in.
      // Return type determines whether we continue the redirect automatically
      // or whether we leave that to developer to handle.
      return false;
    },
    uiShown: function () {
      // The widget is rendered.
      // Hide the loader.
      document.getElementById('loader').style.display = 'none';
    },
  },
  // Will use popup for IDP Providers sign-in flow instead of the default, redirect.
  signInFlow: 'popup',
  signInOptions: [
    // Leave the lines as is for the providers you want to offer your users.
    firebase.auth.GoogleAuthProvider.PROVIDER_ID,
  ],
};
ui.start('#auth', uiConfig);

// Create an auth listener to get the real-time auth status
let myUser = null;
firebase.auth().onAuthStateChanged(user => {
  if (!user) {
    // user is not authenticated and need to transition view
    // do something here with your framework
    myUser = user; // this will be null.
  }
  // user is authenticated - framework of choice code here.
  // in react you could use an AuthContext as an example
  myUser = user.uid // get user id to use for queries, etc.
})
```

다음은 Firebase 제공 구성 요소를 사용하는 간단한 UI입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/Logrocket-firebase-notes-demo.png?resize=1226%2C446&ssl=1)

이제 인증이 완료되었으므로 노트를 만들어 보겠습니다.

선호하는 프레임워크를 사용하여 간단한 양식을 작성하고 양식 값을 캡처합니다. 데이터베이스에 데이터를 보존하기 위해 Firestore 코드와 함께 샘플 기능을 제공하겠습니다.

```undefined
// 
function formSubmit() {
  const title = input.value;
  const content = input.value;
  const roles = {
    '124j243lk': 'owner',
    'fake_id_3': 'editor'
  }

  // save to firestore and have firestore assign a unique id
  firebase.firestore().collection('notes').add({
    title,
    content,
    roles
  });

  // if you would prefer to restrict documents by title uniqueness 
  firebase.firestore().collection('notes').doc(title).set({
    title,
    content,
    roles
  });
}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/create-new-firebase.png?resize=730%2C845&ssl=1)

노트 추가를 위한 샘플 양식입니다. 못생겼다는 거 알아요. 하지만 이건 스타일링에 관한 게 아니에요. 그래서 나는 테일윈드를 추천한다.

Firestore는 고객에게 실시간 데이터 동기화를 제공합니다. 이제 스냅샷 수신기를 설정해 보겠습니다. 자세한 내용은 다음과 같습니다.

```js
db.collection('notes')
  .where(`roles.fake_id`, '==', 'owner')
  .onSnapshot(querySnapshot => {
    // if the query is empty just return
    if (querySnapshot.empty) return;
    // we have some docs --
    // do something depending on your framework of choice.
    // I will create an array of documents with their data and id
    const notes = querySnapshot.docs.map(doc => ({...doc.data(), id: doc.id}))
    // as you can see, I need to call doc.data() to get the data of the document.
    // for this quick and dirty exmaple i will simply loop through the docs and add to an html element
    notesDiv.innerHTML = `<span>Notes: ${notes.length}</span><br><hr />`;
    for (const note of notes) {
      notesDiv.innerHTML += `
        <strong>${note.title}</strong><br>
        <em>${note.content}</em><br/><hr />
      `; 
    }
  });
```

이제 스냅샷 수신기가 생성되었으므로 UI에서 작동하는 것을 보겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/first-firebase-note.png?resize=730%2C943&ssl=1)

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/second-note.png?resize=730%2C977&ssl=1)

좋습니다! 쿼리에서 반환되는 노트 수, 제목 굵게 표시 및 내용 기울임꼴을 확인할 수 있습니다.

Firestore를 보면 관리 콘솔에서 다음과 같이 문서와 문서를 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/firebase-notes.png?resize=730%2C274&ssl=1)

## 결론

우리가 한 일과 그것이 당신에게 무엇을 의미하는지 분석해보자.

우리는 구글의 Firestore로 확장 가능한 실시간 NoSQL 데이터베이스를 설정하고, Firebase Authentication으로 인증을 구성 및 활성화했으며, Firestore 규칙을 통해 인증을 추가했으며, Firebase의 글로벌 CDN으로 정적 사이트를 호스팅하고 있습니다.

Firebase에서 제공하는 모든 기능은 확장 가능한 빌딩 블록을 제공하고 모범 사례를 통해 애플리케이션을 구축할 수 있도록 지원합니다.

그러나 Google Cloud Functions에서 제공하는 Firebase Functions나, 확장 가능한 API 및 백엔드 시스템을 구축하기 위한 환상적인 프리타이어를 제공하는 Google Cloud Run을 비롯한 다른 Firebase 제품에는 대해서는 언급하지 않았습니다. 다시, 모든 서버가 없습니다.

서버를 프로비저닝할 필요도 없었고, 서버 업데이트나 패치에 대해 걱정할 필요도 없었고, 노드 추가나 샤딩에 대한 걱정 없이 전 세계적으로 확장 가능한 데이터베이스를 보유하고 있으며, 빠른 글로벌 CDN과 넉넉한 무료 호스팅을 제공하고 있으며, 인증을 제공하는 모범 사례를 보유하고 있습니다.

파이어베이스와 구글 클라우드에서 할 수 있는 일이 훨씬 더 많다. 파이어베이스와 관련된 게시물을 더 많이 만들고 각 주제 영역에 대해 자세히 살펴보도록 하겠습니다. 계속 튜닝하십시오!