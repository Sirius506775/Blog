---
title: "Raspberry Pi에 Ubuntu 설치하기"
date: 2023-07-01T13:32:49+09:00
tags: [
    'raspberrypi',
    'linux',
    'ubuntu'
]
categories: [
    'Linux'
]
weight: 50
show_comments: true
katex: false
draft: false
---

백엔드 공부 시에 항시 운영되는 외부 서버와의 통신 환경과 서버 구축 단계를 학습!
하기 위해 라즈베리 파이를 구매햇다. 
라즈베리파이에 우분투 서버를 설치해보고자 한다. 

![](https://velog.velcdn.com/images/sirius506775/post/dd62b4f9-c58f-4556-ad2c-7963a7c1eb93/image.jpg)


제품 구성 : 
- Raspberry Pi 4 Model B 4GB RAM
- SD Card 64GB


### 1. sd card format 
운영체제를 설치하기 전에 sd card format을 진행한다. 

라즈베리파이의 부트로더는 FAT16, FAT32 등의 FAT 파일 시스템을 사용하기 때문에 format을 두 번 진행한다.


#### 1.1 sd card Formatter 설치 및 실행


[SD Association](https://www.sdcard.org/downloads/formatter/) 사이트에 접속해서 다운로드한다. 필자는 windows 환경임
![](https://velog.velcdn.com/images/sirius506775/post/737edd7d-2386-4712-ac98-7821c6bcd300/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/343d8e2a-3100-4dd9-b024-955eee74a5e9/image.png)


#### 1.2 sd카드를 인식하고, formatter를 실행한다. 
![](https://velog.velcdn.com/images/sirius506775/post/90aa401a-c25a-431c-8fb2-f5ea2509655b/image.png)

포맷할 sd 카드 경로를 지정하고 포맷한다. 
![](https://velog.velcdn.com/images/sirius506775/post/287c251e-7cd2-4f88-9514-40671ff2e785/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/a87d4035-50d7-433d-b31f-2d8371f225d0/image.png)

포맷 완료 화면에서 File System이 FAT32라면 그대로 진행해도 되지만, 
만약 'exFAT'인 경우는 변경을 해줘야한다. 
![](https://velog.velcdn.com/images/sirius506775/post/513da4f8-88f5-44bb-92b2-cbbd5e8509ae/image.png)

[FAT32 Format](http://ridgecrop.co.uk/index.htm?guiformat.htm)으로 File System을 FAT32로 변경한다.
![](https://velog.velcdn.com/images/sirius506775/post/a65c84ea-2b48-4cef-9771-ce4942faa859/image.png)


> Volume label을 적어주지 않으면 에러가 발생할 수 있다. 

![](https://velog.velcdn.com/images/sirius506775/post/f4866146-23ef-4ef2-93bd-4816023c1230/image.png)

파일 시스템을 확인해보면 FAT32 형식으로 변경되어있는 것을 확인할 수 있다. 

![](https://velog.velcdn.com/images/sirius506775/post/5fe074e9-8af2-413f-b0db-187b8afde04a/image.png)



### 2. 라즈베리 파이 imager 다운로드
![](https://velog.velcdn.com/images/sirius506775/post/3ba720b3-b52a-4e5d-bb6c-56b9b265106e/image.png)

2. 설치 후 실행
![](https://velog.velcdn.com/images/sirius506775/post/3f1558a5-9923-4003-8dfc-02112f1bcdbf/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/4d7a5643-a097-405a-95fa-6631b5427066/image.png)

3. 운영체제 선택
![](https://velog.velcdn.com/images/sirius506775/post/922df18e-7a4e-4cc0-8570-bfc5a67e949f/image.png)


Ubuntu 이미지 파일을 조금전에 포맷한 sd카드에 로딩하여 부팅 디스크로 만들어준다. 

~~desktop LTS 버젼으로 난 할거임~~

( 23.08.22 : gui 땜에 그냥 server로 다시 깔거임 헤헤_)

![](https://velog.velcdn.com/images/sirius506775/post/6935633b-fd81-4454-9022-24abe805a34a/image.png)





sd 카드를 스토리지로 지정하고
![](https://velog.velcdn.com/images/sirius506775/post/cf634ad2-82f1-4b95-9a37-fe9999d4d3fd/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/a9e9f2ff-624c-48a5-aeab-ff1106d2a38a/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/6f78a354-de84-4c5d-ada7-c9a3cc28bfc9/image.png)

시간이 꽤 걸린당 
![](https://velog.velcdn.com/images/sirius506775/post/89f334d3-e798-499b-a6bb-c74f48f0f748/image.png)

![](https://velog.velcdn.com/images/sirius506775/post/1c1c083d-e990-413d-be23-3d38eb2e51f6/image.png)

완료가 되면 sd 카드를 pc에서 제거한다. 

### 3. 기기 연결 후 네트워크 설정

~~....근데 미국에서 직구한거라 
전압...돼지코 사러 가야겟다..ㅜ 사와서 다시 이어서 포스팅..~~
![](https://velog.velcdn.com/images/sirius506775/post/22b70a4a-5106-4e88-9ba5-6c8638fa2e3a/image.jpg)


우분투 로딩한 sd카드를 기존에 조립해놓은 라즈베리파이 기기에 연결한다. 조립하는 방식은 자세한 설명서가 없어서 [영상](https://www.youtube.com/watch?v=dB9BDq8C3qY)보고 그대로 따라했음. 매우 쉽습니다

![](https://velog.velcdn.com/images/sirius506775/post/5e1854e2-ffc5-453b-9e55-301d9dab1a58/image.jpg)


모니터 연결하고 로딩이 완료되면 안내에 따라서 os 설정을 진행하면 됩니당
![](https://velog.velcdn.com/images/sirius506775/post/f071f515-6db8-4b3b-80ea-6fb60ed366f3/image.jpg)

우분투 환경 설치 끝! 다음 포스팅은 진짜 웹서버 구축을 시작해보자
![](https://velog.velcdn.com/images/sirius506775/post/5cecaf49-9264-4a14-8d66-92766d1bdcf9/image.jpg)




#### * reference
- [How To Install Raspbian OS On The Raspberry Pi 4](https://www.youtube.com/watch?v=cxhctYvQomY)
- [Raspberry Pi 와 PC Serial 통신](https://post.naver.com/viewer/postView.nhn?volumeNo=31531516&memberNo=2534901)
- [라즈베리파이에 Ubuntu 20.04 LTS 설치하기](https://blog.may-i.io/tech-7/)
- [라즈베리파이4로 토이프로젝트용 서버 만들기 - 1편](https://ryan-han.com/post/dev/raspberry_server_1/)
- [Raspberry Pi 4 NEEDS A FAN - How to Install CanaKit Heatsink and Fan](https://www.youtube.com/watch?v=dB9BDq8C3qY)