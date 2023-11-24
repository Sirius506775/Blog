---
title: "Hugo 블로그로 이전했습니다"
date: 2023-09-10T04:38:41+09:00
tags: [
    "hugo",
    "githubpages"
]
weight: 50
show_comments: true
katex: false
draft: false
authorbox: false
sidebar: false
categories:
  - "Essay"
lead: velog는 이제 안녕
---

#### 기술블로그를 Velog에서 github.io로 이전한다.

Tistory와 Jekyll, Oopy를 전전하다가 Velog로 정착했었던 까닭은 아티클 생성이 Markdown 문법에 맞추어져 작성하기 편하단 점과 단조로운 디자인 및 옵션들이 사용자들에게 낮은 접근성을 제공한다고 느꼈기 때문이었다.

더욱이 게으른 완벽주의자인 나는 글을 쓰는 것에 같잖은 고집 같은게 있었어서, 적지 않은 시간이 소요되었었고, 이는 인사이트를 만들어내는 과정, 글을 쓰는 행위 자체가 힘든 일이 되기 일쑤였다. 그래서 비교적 쉽게 쓸 수 있는 플랫폼이 한결 편했다.

하지만 최근부터 Velog의 태그 기반 정렬 방식이 오히려 글이 많아질 수록, 집중이 되지 않는 느낌을 강하게 받았고, 이따금씩 나타나는 내가 작성한 글이 사라지거나, 임시 글이 삭제되는 버그들이 내 신경을 거슬렸다. 

뭐 이제는 글을 쓰기 위해 공부하고, 생각을 기록하는 이 행위가 힘들기는 커녕 즐기게 되었기 때문에 다시 정적 웹도 건드려볼겸 스스로에게 의지를 부여하고, 휘발되지 않아야할 기록과 나의 생각들을 꾸준히 담아내고자 기술블로그를 이전하기로 했다.



이번에 내가 선택한 SSG(Static Site Generators)는 Go언어 기반의 [Hugo](https://gohugo.io/)이다. 확실히 국내 레퍼런스가 적긴 하지만 이전에 사용하던 Ruby 기반의 Jekyll은 Content가 많아질수록 Build 속도가 느린게 체감이 되어서 OUT, Hugo를 선택한 이유는 다음과 같았다.

> - Jekyll보다 Hugo가 카테고리 구성이나 테마 관리가 더 편하다.(Jekyll은 jamstack 랭킹에서도 밀린지 오래임)
> ![Alt text](/images/image-1.png)
> 
> - Local 환경에서도 아티클 생성 시, flutter의 hot reload 못지 않은 반영 속도를 보여준다.(좀 과장이긴 한데 아티클 꽤나 쌓인 jekyll의 리로드 속도 아는 사람들은 그렇게 느낄거다)
> 
> - 주인장인 내가 Go에 관심이 좀 있음
> 
> - React를 했었어서, Gatsby로도 publish 해봤었는데 난 그저 정적웹만 만들고 싶은데 graphql도 신경써야하고(물론 REST 보다가 GraphQL은 흥미로웠음), 내부의 추상화가 너무 되어있어서 디버깅하다가 열받음. 반면 hugo는 열받을 포인트가 없음! Documentation가 너무 잘되어있다. 사랑해요 [BEP](https://github.com/bep)
> 
> - 이번 블로그는 타채널 연동이나, SEO을 신경쓰지 않고 오직 기록용이라서 사실 빌드 속도에 하나에 크게 만족한다. 
> 

**고민할 거 없이 Hugo로 결정**








