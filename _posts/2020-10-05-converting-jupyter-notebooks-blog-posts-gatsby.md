---
layout: post
title: "개츠비와 함께 주피터 노트북을 블로그 게시물로 변환"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/converting-jupyter-notebooks-blog-posts-gatsby.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/converting-jupyter-notebooks-blog-posts-gatsby.png?fit=730%2C487&ssl=1)

데이터 과학에 정통한 모든 사람들은 주피터 노트북이 좋은 길이라는 것을 알고 있다. Markdown을 실제 코드와 쉽게 혼합하여 연구와 학습을 위한 활기찬 환경을 만들 수 있습니다. 코드가 사용자 친화적이고 적절하게 포맷됩니다. 코드를 기록하고 이동 중에 동적 차트, 테이블 및 이미지를 생성합니다.

노트북을 쓰는 것은 매우 좋기 때문에 인터넷에서 노트북을 공유하고 싶을 수도 있다고 상상하는 것은 자연스러운 일입니다. GitHub에서 호스팅할 수도 있고 Google Colab에서도 호스팅할 수 있지만, 실행 중인 커널이 필요하며, 이는 확실히 좋은 웹 페이지만큼 친절하지는 않습니다.

더 나아가기 전에 주피터 노트북은 입력, 출력 및 수많은 메타데이터를 포함하는 JSON 객체 모음에 불과하다는 점을 이해하는 것이 중요합니다. 그런 다음 출력을 구성하고 HTML과 같은 다른 형식으로 쉽게 변환할 수 있습니다.

노트북이 HTML 문서가 될 수 있다는 것만 알면 됩니다. 이제 이 프로세스를 자동화할 수 있는 방법을 찾아서 `.ipynb` 파일이 인터넷에서 정적 페이지가 될 수 있습니다. 이 문제에 대한 나의 해결책은 개츠비를 이용하는 것이다.JS - 특히 단일 정적 사이트 생성기는 아니더라도 최고의 정적 사이트 생성기 중 하나입니다.

Gatsby는 JSON, Markdown, YAML 등 다양한 형식의 데이터를 쉽게 소싱하고 웹 상에서 호스팅할 수 있는 웹 페이지를 정적 방식으로 생성합니다. 마지막 부분은 마크다운을 게시물로 변환하는 대신 `.ipynb` 파일로 변환하는 것이다. 이 게시물의 목표는 이 프로세스를 안내하는 것입니다.

## 기술적 문제

웹에서 빠른 검색을 통해 개츠비-트랜스포머-아이핀을 볼 수 있을 것이다. 기본적으로 이것은 나중에 그래프QL 쿼리에서 액세스할 수 있는 방법으로 노트북 파일을 구문 분석할 수 있는 개츠비 플러그인이다. 사실이라고 하기엔 거의 너무 좋아요!

그리고, 사실, 그렇습니다. 그 힘든 일은 훌륭한 사람들이 서로 연락해서 한 것이다. 그러나 플러그인이 한동안 유지 관리되지 않았으며, 플러그인에 대한 사용자 지정이 부족한 것은 말할 것도 없고, 모든 것이 즉시 해결되지는 않습니다.

지루한 일은 삼가 드리겠지만, 깃허브의 어두운 모퉁이에서 소란을 피운 후, Specific Solutions의 이 게시물의 중요한 도움으로, 저는 제 문제를 해결하고 이 게시물의 목적에 충분할 개츠-트랜스포머-아이핀이라는 나만의 포크를 만들 수 있었습니다.

하지만, 저는 적극적인 유지보수가 될 의사가 없으며, 제가 한 일의 대부분은 오로지 제가 해야 할 일을 얻기 위한 것이었으며, 여러분 스스로 위험을 무릅쓰고 그것을 사용한다는 것을 명심하세요.

프리앰블은 이제 그만하고 코드 좀 찾아보자.

## 프로젝트를 만들고 있습니다.

첫째, 우리가 만들려는 것의 소스 코드는 여기 깃허브에서 찾을 수 있다. 우리는 개츠비 프로젝트를 만드는 것으로 시작할 것이다. 개츠비가 설치되어 있는지 확인하고 다음을 실행하여 새 프로젝트를 만드십시오.

```bash
gatsby new jupyter-blog
cd jupyter-blog
```

gats by develop을 실행하고 http://localhost:8000/로 이동하여 모든 것이 정상적으로 작동하는지 확인합니다.

## 첫 번째 노트북 만들기

주피터 노트북이 새로운 블로그의 데이터 소스가 될 것이기 때문에, 컨텐츠 추가를 시작해야 합니다. 프로젝트 폴더 내에서 `src`로 이동하여 `노트북` 폴더를 만듭니다. 나중에 이 폴더에서 읽을 수 있도록 하겠습니다.

이제 첫 번째 노트북을 만들 시간입니다. 이 튜토리얼을 위해 이 간단한 노트북을 기본으로 사용하겠습니다. GitHub에서는 동적 출력을 볼 수 있지만 원하는 것을 자유롭게 사용할 수 있습니다.

어쨌든 Plotly에서 생성된 동적 차트 등 일부 리치 출력에 각별한 주의가 필요할 수 있습니다. 나중에 게시물에서 다루길 원하는 경우 알려 주십시오! 그러나 이 게시물을 짧게 유지하기 위해 정적 이미지, 테이블 및 마크다운만 처리합니다.

데이터가 있는 개츠비 프로젝트가 있으니 다음 단계는 GraphQL을 사용하여 쿼리하는 것입니다.

## 데이터 쿼리 중

개츠비의 가장 큰 장점 중 하나는 데이터를 소싱할 때 유연성이다. 원하는 거의 모든 데이터가 정적 컨텐츠를 생성하는 데 사용될 수 있는 데이터 원본이 될 수 있습니다.

위에서 언급했듯이, 우리는 내 버전의 변압기를 사용할 것이다. 설치를 계속합니다.

```css
yarn add @rafaelquintanilha/gatsby-transformer-ipynb
```

다음 단계는 플러그인을 구성하는 것입니다. `gatsby-config.js`에서 `plugins` 어레이에 다음을 추가합니다(의심할 때 GitHub를 항상 확인할 수 있음).

```coffeescript
...
{
  resolve: `gatsby-source-filesystem`,
  options: {
    name: `notebooks`,
    path: `${__dirname}/src/notebooks`,
    ignore: [`**/.ipynb_checkpoints`],
  },
},
{
  resolve: `@rafaelquintanilha/gatsby-transformer-ipynb`,
  options: {
    notebookProps: {
      displayOrder: ["image/png", "text/html", "text/plain"],
      showPrompt: false,
    },
  },
},
...
```

그것을 분해하자.

먼저 어레이에 `gats by source-filesystem` 옵션을 추가합니다. 우리는 개츠비에게 우리의 .ipynb 파일이 있는 `src/노트북`에서 파일을 찾아보라고 말하고 있다. 다음으로, 변압기를 구성하고 몇 가지 소품을 설정할 것입니다.

- `디스플레이 순서` – 표시 중인 출력의 MIME 유형
- ` 프롬프트 표시` – 프롬프트 표시 여부

알림 메시지는 노트북의 정적 페이지에서는 의미가 있지만 목적을 달성하지 못합니다. 그 부분에 대해서는 명확한 내용을 담기 위해 저희가 숨기도록 하겠습니다.

모든 것이 계획대로 진행되었는지 확인할 시간입니다. `http://localhost:8000/___graphql`로 이동하여 GraphiQL을 열고 다음 쿼리를 실행합니다.

```undefined
query MyQuery {
  allJupyterNotebook {
    nodes {
      html
    }
  }
}
```

성공! 노트북의 HTML이 어떻게 생성되었는지 주목하세요. 이제 이 HTML을 리액트 컴포넌트에 주입하기만 하면 우리의 프로세스가 완료됩니다.

## 게시물 자동 생성

최악의 상황은 지금 우리 뒤에 있다. 다음 단계는 이 데이터를 `gatsby-node.js`로 쿼리하여 `src/노트북`의 각 노트북에 대한 정적 페이지를 생성할 수 있도록 하는 것이다.

그러나 작성자 및 게시 제목과 같은 추가 메타데이터를 노트북에 추가해야 합니다. 여러 가지 방법이 있는데 가장 간단한 것은 아마도 .ipynb 파일이 JSON이라는 사실을 이용하여 자신의 메타데이터 필드를 사용하는 것이다. .ipynb를 열고 필요한 정보를 추가합니다.

```undefined
{
 "metadata": {
  "author": "Rafael Quintanilha",
  "title": "My First Jupyter Post",
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.4-final"
  },
  "orig_nbformat": 2,
  "kernelspec": {
   "name": "python3",
   "display_name": "Python 3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2,
 "cells": [
  ...
 ]
}
```

> 팁: VS Code를 사용하는 경우 파일을 열면 Jupyter 커널이 시작될 수 있습니다. 원시 콘텐츠를 편집하기 위해 구성에서 사용하지 않도록 설정할 수 있지만, 저는 보통 다른 편집기(예: gedit 또는 메모장++)로 파일을 엽니다.

지금 그 과정은 개츠비의 모든 데이터 소스에 대해 정확히 동일하다. 우리는 `gatsby-node.js`의 데이터를 쿼리하고 관련 정보를 포스트 템플릿에 전달할 것이며, 이는 다시 우리 도메인의 고유한 페이지가 될 것이다.

그러나 이를 수행하기 전에 `gatsby-node.js`를 열고 다음을 추가하십시오.

```js
exports.onCreateNode = ({ node, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === "JupyterNotebook") {
    createNodeField({
      name: "slug",
      node,
      value: node.json.metadata.title
        .split(" ")
        .map(token => token.toLowerCase())
        .join("-"),
    })
  }
}
```

위의 발췌문은 GraphQL에서 생성된 모든 노드에 대해 주피터 노트북인 것을 확인하고 새로운 필드인 `슬러그`로 확장한다. 우리는 여기서 순진한 접근법을 쓰고 있지만, 당신은 슬러기라이프와 같은 강력한 라이브러리를 사용할 수 있다. 새 필드가 쿼리되어 사후 경로를 생성하는 데 사용됩니다. 동일한 파일에 다음을 추가합니다.

```js
const path = require(`path`);
exports.createPages = async ({ graphql, actions: { createPage } }) => {
  const blogPostTemplate = path.resolve(`src/templates/BlogPost.js`);
  const results = await graphql(
    `
      {
        allJupyterNotebook() {
          nodes {
            fields {
              slug
            }
          }
        }
      }
    `
  );
  const posts = results.data.allJupyterNotebook.nodes;
  posts.forEach((post) => {
    createPage({
      path: post.fields.slug,
      component: blogPostTemplate,
      context: {
        slug: post.fields.slug,
      },
    });
  });
};
```

기본적으로 데이터를 slug별로 쿼리하여 BlogPost.js로 보낸다. 지금 생성해 보겠습니다.

```coffeescript
import React from "react"
import { graphql } from "gatsby"
import SEO from "../components/seo"

const BlogPost = ({
  data: {
    jupyterNotebook: {
      json: { metadata },
      html,
    },
  },
}) => {
  return (
    <div>
      <SEO title={metadata.title} />
      <h1>{metadata.title}</h1>
      <p>Written by {metadata.author}</p>
      <div dangerouslySetInnerHTML={ __html: html } />
    </div>
  )
}
export default BlogPost
export const query = graphql`
  query BlogPostBySlug($slug: String!) {
    jupyterNotebook(fields: { slug: { eq: $slug } }) {
      json {
        metadata {
          title
          author
        }
      }
      html
    }
  }

```

바로 그거야! http://localhost:8000/my-first-jupter-post로 이동하여 노트북을 정적 HTML 페이지로 봅니다.

## 개선사항

보시다시피 스타일링과 디자인 면에서 많은 것을 개선할 수 있습니다. 이 게시물의 범위를 벗어나지만 CSS 모듈을 사용하여 레이아웃을 개선하고 불필요한 표준 출력(블로그 게시물에서 상관 없는 텍스트 출력)을 제거할 수 있습니다. `BlogPost.module.css`를 생성하고 다음을 추가하십시오.

```css
.content {
  max-width: 900px;
  margin-left: auto;
  margin-right: auto;
  padding: 40px 20px;
}
.content :global(.nteract-display-area-stdout),
.content :global(.nteract-outputs > .cell_display > pre) {
  display: none;
}
.content :global(.nteract-outputs > .cell_display > img) {
  display: block;
}
.content :global(.input-container) {
  margin-bottom: 20px;
}
.content :global(.input-container pre.input) {
  border-radius: 10px !important;
  padding: 1em !important;
}
.content :global(.input-container code) {
  line-height: 1.5 !important;
  font-size: 0.85rem !important;
}
.content :global(.input-container code:empty) {
  display: none;
}
@media only screen and (max-width: 940px) {
  .content {
    max-width: 100%;
    padding-left: 20px;
    padding-right: 20px;
    box-sizing: border-box;
  }
}
```

이제 `BlogPost.js`로 돌아가서 div에 클래스를 추가합니다.

```js
...
import css from "./BlogPost.module.css"
...
return (
  <div className={css['content']}>
     ...
  </div>
);
```

이제 얼마나 깨끗해 보이는지 주목하세요. 최종 결과(약간 수정)는 Netliify에서 호스팅됩니다. 모든 변경 사항은 소스 코드에 있습니다.

## 마지막 생각

주피터 노트북을 HTML 페이지로 변환하는 것은 복잡하지 않지만 많은 작은 단계와 조정이 필요합니다. 이 게시물이 어떻게 시작하는지에 대한 가이드이길 바랍니다.

풍부한 출력 지원(예: 동적 차트), 모바일 환경 개선, 메타데이터 관리 개선 등과 같이 수많은 변경 및 개선 작업을 수행할 수 있습니다.

노트북은 다재다능하고 작업하기에 재미있으며, 자동으로 웹 페이지로 변환하는 것이 노트북의 좋은 기능입니다.