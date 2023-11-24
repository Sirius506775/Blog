---
title: "Subtree를 이용한 프로젝트 관리"
date: 2023-06-18T13:29:38+09:00
tags: [
    'git'
]
categories: [
    'Git'
]
weight: 50
show_comments: true
katex: false
draft: false
---

![](https://velog.velcdn.com/images/sirius506775/post/8e762ea4-4499-4f9a-9095-2df87a75fc9c/image.png)


Git Subtree(이하 서브트리)는 SHA 값만 저장하는 서브모듈과 달리 상위 저장소에 파일을 직접 추가하고 트래킹한다. 서브트리의 파일 및 변경사항도 상위 저장소에 기록된다. 
서브트리의 원격에 있는 소스와 서브트리를 추가한 저장소의 소스가 서로 달라도 “subtree merge” 기능을 사용해서 양쪽의 변경사항을 모두 반영할 수 있다.

상위 저장소에서 서브트리를 직접 수정하고 원격에 푸시할 수 있다는 것이 서브모듈과 큰 차이점이다. 예를 들어 서브트리의 컴포넌트를 상위 저장소의 프로젝트에서 사용할 때 버그를 발견했다면, 즉시 수정해서 커밋한 후 버그를 해결한 상태로 서브트리의 원격에 반영할 수 있다는 편리함이 있다.

### Git 서브트리 사용법
현재 공부 중인 flutter 프로젝트들을 관리할 상위 저장소 repository를 생성했다. 

![](https://velog.velcdn.com/images/sirius506775/post/9d59006a-9bfc-497b-ab99-4b6a74117691/image.png)

![](https://velog.velcdn.com/images/sirius506775/post/387b66b9-fd7d-4966-9376-5e57d4c394c2/image.png)

![](https://velog.velcdn.com/images/sirius506775/post/27adcc69-baf9-4a72-8493-7d5098342733/image.png)

![](https://velog.velcdn.com/images/sirius506775/post/ae5fa475-d37a-43b8-ac77-fde41640424e/image.png)

![](https://velog.velcdn.com/images/sirius506775/post/71afaf95-1dd4-48e9-8f82-ff1c384bff30/image.png)

![](https://velog.velcdn.com/images/sirius506775/post/f56d91ce-dc83-4ca3-96f0-7b07566b31df/image.png)


1. 서브트리로 사용할 원격 저장소 추가
`git remote add <원격 저장소의 이름> <원격 저장소의 주소>`

2. 새로운 원격 저장소의 브랜치를 서브트리로 추가.
`git subtree add --prefix <클론할 폴더> <원격 저장소의 이름> <브랜치 이름>`

--prefix 옵션으로 서브트리를 클론할 폴더를 지정


#### Reference 
- https://zzang9ha.tistory.com/294
- https://www.atlassian.com/git/tutorials/git-subtree?clickid=QYV28a01NxyPWKpRFe308Qm0UkF0n0URf276180&irgwc=1&utm_medium=paid-affiliate&utm_source=impact&ircid=17715&irpid=27795&irmpname=Sovrn%20Commerce&irmptype=mediapartner&irshareid=&irmpgroupname=&iraid=1583141&utm_content=US%20%7C%20Jira%20Software%20-%2088x31--TEXT_LINK--&irclkid=QYV28a01NxyPWKpRFe308Qm0UkF0n0URf276180
- https://blog.rhostem.com/posts/2020-01-03-code-sharing-with-git-subtree