---
title: "Ubuntu 22.04 Server MariaDB 설치 및 환경설정"
date: 2023-08-23T13:35:44+09:00
tags: [
    'raspberrypi',
    'linux',
    'ubuntu'
]
categories: [
    'linux'
]
weight: 50
show_comments: true
katex: false
draft: false
---
> 본문은 Ubuntu-Server 22.04.3 LTS version 환경을 바탕으로 작성한 글이며, 혹시나 잘못된 부분이 있다면 과감하게 댓글 남겨주시면 감사하겠습니다. 

---

안녕하세요. [Heon](https://velog.io/@sirius506775)입니다. 이번 포스팅은 [Raspberry Pi 4 Model B](https://www.raspberrypi.com/)에 설치한 Ubuntu 환경에서 MariaDB를 설치하고, 외부 접속까지 설정하는 방법을 설명합니다.

최근 제가 참여하고 있는 프로젝트에서 네이버클라우드를 검토하게 되었는데요. 과금을 꺼려하는 저희 팀은 VPC와 AI 모델을 위한 서버도 사용해야하는 상황에서, DB까지는 보유 크레딧으로 온전히 비용 충당이 어렵다고 판단했기 때문에, 대신 개인적으로 보유하고 있는 라즈베리파이 우분투 서버를 db서버로 활용하기로 결정했습니다. 이번 프로젝트을 계기로 제가 겪은 과정을 이번 포스팅에서 여러분들께 공유해보고자 합니다.

---

# How to install MariaDB in Ubuntu Server(Raspberry Pi)

## 📦️ 1. apt mariadb-server package install
```$ sudo apt install mariadb-server mariadb-server``` 
apt로 mariadb-server 패키지를 설치합니다.




![](https://velog.velcdn.com/images/sirius506775/post/4b1a1d37-81d3-4279-9bd6-a673eee8d0da/image.png)
```$ sudo mariadb```

설치가 완료된 후에 정상적으로 DB 접속이 되는 것을 확인합니다.




![](https://velog.velcdn.com/images/sirius506775/post/af7b21a5-9768-4954-91eb-f2ba2f21ab6a/image.png)

``` > status```로 db 상세정보 또한 확인할 수 있습니다.
![](https://velog.velcdn.com/images/sirius506775/post/ab0264a9-1da9-4c31-83f4-d3aeb2ea5667/image.png)

---


## 🔒 2. 데이터베이스 보안 설정(mariadb-secure-installation)

```$ sudo mariadb-secure-installation ``` 

이제 설치한 데이터베이스의 보안 설정을 실시합니다. 

![](https://velog.velcdn.com/images/sirius506775/post/f3cf0d2d-caa2-40ac-a27e-ffc717709f63/image.png)
> - 최초 실행 시 password가 지정되어 있지 않기 때문에 공백으로 enter키를 눌러줍니다.
![](https://velog.velcdn.com/images/sirius506775/post/ef3c8a36-dffa-4b30-a5da-263cbc596b09/image.png)

>  - unix_socket은 default 값으로 유지하며, root의 새로운 패스워드를 지정해 줍니다. 이후 익명 계정들은 삭제해 줍니다.
![](https://velog.velcdn.com/images/sirius506775/post/1066f8c8-bd00-4ffe-9349-b66dfc939176/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/ac9e4f53-5644-473f-89dc-4d46e4546857/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/37f76b2d-7df0-4a3a-939a-3c941d997c1b/image.png)

>- root 계정에 원격접속 승인 유무인데 저는 승인하겠습니다.
![](https://velog.velcdn.com/images/sirius506775/post/40e41da8-c477-4a80-952f-0836414d972b/image.png)

> - 테스트 데이버테이스와 권한정보는 삭제 후, 테이블에 대한 권한 설정을 reload해줍니다.
![](https://velog.velcdn.com/images/sirius506775/post/3269ff93-9795-4c0f-82b9-07c1e19a45ae/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/0645a294-a3a6-40ec-9bc0-87a90e8c68eb/image.png)

설정 완료 후 접속이 정상적으로 되는지 확인해봅니다. 
```
//관리자 권한으로 DB접속
$ sudo mariadb
//일반 계정으로 DB접속
$ mariadb -uroot -p
$ password <root 비밀번호>
```

![](https://velog.velcdn.com/images/sirius506775/post/2725e11f-8c8d-48af-a4d2-1f7e3ad329ef/image.png)
![](https://velog.velcdn.com/images/sirius506775/post/7cba2d1b-1b6a-4b0f-afbc-c32b63a84536/image.png)



---

## 🗃 3. 테이블 대소문자 구분 설정 해제 및 기본 포트 변경

최초로 mariadb를 설치하면 리눅스 파일 시스템 기본 설정 상 윈도우와는 다르게 테이블 이름이 대소문자를 구분합니다.

db에 접속해서 ``` > show variables like 'lower_case_table_names';``` 명령어로 테이블의 대소문자 구분 설정을 확인해보면 default 값이 0임을 확인할 수 있는데요.

![](https://velog.velcdn.com/images/sirius506775/post/d67c82d2-c6b4-4283-aa00-5ec6632dfa39/image.png)

해당 value의 의미는 다음과 같습니다. 

|lower_case_table_names |Value |
|------|------|
|0|테이블 대소문자 구분|
|1|테이블 대소문자 구분 없이 소문자로 저장|
|2|윈도우에서 테이블 대소문자 구분|


### 3.1 데이터베이스 설정 파일 수정
Value 값을 변경하기 위해 데이터베이스 설정 파일을 에디터로 엽니다.

```$ sudo vim /etc/mysql/mariadb.conf.d/50- server.cnf```

다음로 사용할 포트와 대소문자 구분 설정값을 입력합니다. 이때 'port'와 'lower_case_table_names' 속성을 하단에 추가합니다.
![](https://velog.velcdn.com/images/sirius506775/post/0fc4bff0-5d83-4874-8632-7dee05cb1cc1/image.png)

### 3.2 서비스 재시작 
설정 파일 저장 후 mariaDB 서비스를 재시작해줍니다. 
```$ sudo service mariadb restart```
![](https://velog.velcdn.com/images/sirius506775/post/d0bc9b40-5d63-4276-ae4a-e5cf2037adf7/image.png)

### 3.3 port 확인
포트가 정상적으로 변경되었는지 telnet 명령어로 확인해봅니다.
``` $ telnet localhost (port) ```
![](https://velog.velcdn.com/images/sirius506775/post/7e5185fc-1c7a-45cb-8ee5-d025bef2c1c1/image.png)

### 3.4 lower_case_table_names 확인
대소문자 구분 설정이 1로 잘 변경되었는지 확인합니다.
``` > show variables like 'lower_case_table_names';```
![](https://velog.velcdn.com/images/sirius506775/post/dd9f7694-9047-45d5-a897-5208d753b4a6/image.png)

---

## 🛂 4. UFW 방화벽 활성화

ssh 설정 시 방화벽 오픈을 안했다면,utf 방화벽 오픈을 활성화해야합니다. 

### 4.1 서비스 파일 조회
```$ sudo vi /etc/services``` 
![](https://velog.velcdn.com/images/sirius506775/post/be4a4911-061e-4842-8387-eaeed394d2d5/image.png)

설정파일에서 원하는 서비스 검색 후 열고자 하는 방화벽 포트에 부합하는지 확인합니다. 조회해보니 mysql은 3306/tcp인 것을 확인할 수 있습니다.


### 4.2 포트와 프로토콜을 이용한 방화벽 오픈  
![](https://velog.velcdn.com/images/sirius506775/post/91f06e65-0328-4967-a8ce-2bf2d38a87e8/image.png)
```$ sudo ufw allow 3306/tcp```  

서비스 파일에서 확인했던 mysql 서비스의 포트와 프로토콜을 지정하여 방화벽을 오픈합니다.
 ```
$ sudo ufw allow <port>/<optional: protocol>
//protocol은 생략가능하며 생략 시 tcp와 udp 모두 적용됩니다.
```


  
#### + 서비스를 이용한 방화벽 오픈
![](https://velog.velcdn.com/images/sirius506775/post/e9408b02-7feb-40af-a823-4511321503f2/image.png)

```$ sudo ufw allow mysql``` 명령어로 사용할 mysql 서비스의 방화벽을 오픈할 수도 있습니다. 4.2에서 먼저 포트와 프로토콜로 오픈해주었기 때문에, 해당 방화벽 rule 추가는 skip하는 것을 확인할 수 있네요.
( 비활성화할 경우에는 ```$ sudo ufw disable``` )

---

### 4-3. UFW 방화벽 활성화 및 오픈 리스트 확인
  
```$ sudo ufw status```
  ![](https://velog.velcdn.com/images/sirius506775/post/b4f39b83-f547-4d8c-8c6f-3d4f5d5f7f9b/image.png)
현재 방화벽 상태를 확인해보면 inactive 상태인 것을 알 수 있는데요. 우분투에서 기본적으로 제공하는 방화벽인 UFW(Ubuntu Firewall)은 기본적으로 비활성화 되어 있기 때문에 ```ufw enable``` 명령어로 ufw를 활성화해야합니다. 
  
  
> **주의해야할 점 ** 
  ufw를 활성화하면 방화벽이 작동하기 떄문에 모든 포트가 자동으로 막히게 됩니다. 방화벽 설정을 하지 않고 그대로 종료하면 이후부터 ssh 접근을 못할 수도 있으니 유의하시기 바랍니다.


```$ sudo ufw enable``` 
 
ufw 방화벽을 활성화하고 다시 status를 조회하면 active 상태로 변경된 것을 확인할 수 있으며, 아래와 같이 기존에 오픈했던 서비스들에 대한 IPv4 및 IPv6에 대한 방화벽 허용 정책이 적용된 것을 확인할 수 있습니다.
![](https://velog.velcdn.com/images/sirius506775/post/a0b20531-1b50-4db7-b6ff-8a1d605f71ab/image.png)

![](https://velog.velcdn.com/images/sirius506775/post/2faf62cf-378e-43c5-ae83-f1c13145fc4d/image.png)


> ```$ sudo ufw status numbered```를 사용하면 방화벽 적용 순서에 맞춰 status를 확인할 수도 있습니다.
![](https://velog.velcdn.com/images/sirius506775/post/2cdc69d7-4e02-4df8-995a-f4b470582bae/image.png)
  
---
## 🗃️ 5. 데이터베이스 원격 접속 설정 및 접속 계정 생성
### 5.1 데이터베이스 바인딩 주소 변경 
```$ sudo vim /etc/mysql/mariadb.conf.d/50- server.cnf```
  
원격 접속을 위해서 기존에 에디터로 열었던 데이터베이스 설정 파일에 **bind-address**를 주석처리합니다. 
  
bind-address 초기 설정인 127.0.0.1은 로컬 주소로만 접속할 수 있는 상태를 뜻하므로, 다른 인터넷 환경에서도 접속할 수 있게 하려면 주석 처리 해야합니다.
  ![](https://velog.velcdn.com/images/sirius506775/post/977fd64b-e49e-4d0d-8515-994810a42b31/image.png)
  
![](https://velog.velcdn.com/images/sirius506775/post/82e26b85-b848-433c-be9a-5be3c0e6401a/image.png)


### 5.2 신규 데이터베이스 생성

데이터베이스 원격 접속 테스트를 위해 데이터베이스 하나를 새로 생성해봅시다.
  
``` > create database <데이터베이스 이름>;```
  
![](https://velog.velcdn.com/images/sirius506775/post/7db463d2-b921-41bd-af66-7711e48e5943/image.png)

데이터베이스에 접속하기 위한 port를 확인해봅니다.
```show global variables like 'port';```
![](https://velog.velcdn.com/images/sirius506775/post/1ec87064-eeec-4c68-b4ef-4225941ebd1e/image.png)

  
### 5.3 사용자 계정 생성 및 권한 부여
```> create user '계정'@'접속위치' identified by '패스워드';```
  ![](https://velog.velcdn.com/images/sirius506775/post/1dc7fcf5-9f6e-4e22-b6da-46b656b72b7d/image.png)
``` > grant all privileges on DB이름.테이블 to '계정'@'접속위치';```
``` > flush privileges;```
![](https://velog.velcdn.com/images/sirius506775/post/4edde9e1-32c5-4ea0-aa12-5f282d6aff0c/image.png)
  
이후 
```> select user,host from mysql.user;``` 명령어로 해당 데이터베이스의 세션관리자 계정을 조회해보면 방금 전 생성한 계정을 확인할 수 있습니다. 
![](https://velog.velcdn.com/images/sirius506775/post/7bef0c62-8a4d-4ecd-8915-faee8979370f/image.png)


---

## 🗃️ 6. Database Source Connection

자, 이제 그렇다면 지금까지 세팅한 DB를 직접 연결해봅시다. DB 핸들링을 위한 IDE로 저는 애용하는 JetBrains 사의 [Datagrip](https://www.jetbrains.com/datagrip/?source=google&medium=cpc&campaign=15034927843&term=datagrip&content=555122603676&gad=1&gclid=Cj0KCQjwuZGnBhD1ARIsACxbAVhZtQrk3cs7TxeiYnp-Ggiur7Ikszti8Qd7dy34dbU0nHaSePS34SgaAiW_EALw_wcB)을 사용하도록 하겠습니다.( heidiSQL 사용해도 무방 )


![](https://velog.velcdn.com/images/sirius506775/post/2910c3c4-62a7-4d20-a63f-f846d2eb752c/image.png)

MariaDB 데이터 소스를 생성하고, 접속할 Host과 port, 조금 전에 생성한 user와 password, 그리고 사용할 database 이름을 각각 입력하여 DB Connection을 진행해봅시다.
  

![업로드중..](blob:https://velog.io/f615b712-bb94-46e8-a33f-c02178558785)

여기서 Host는 공유기가 할당한 내부 ip인 192.168.x.x가 아닌 **현재 라즈베리파이가 사용하고 있는 공유기의 ip**, 즉 **외부 ip(External IP)**로 접속합니다. 이때 공유기는 ```포트 포워딩```을 필수로 진행하여야합니다. 
  
   ![업로드중..](blob:https://velog.io/8d502b5f-70d4-4236-af28-7f533083c2ac)

> _**Tip**_
원격으로 현재 나의 리눅스 서버에 접속할 외부 ip를 알고자한다면, 리눅스 서버 터미널 창에서 
  ```$ curl ifconfig.me``` 명령어를 입력하면 접속할 외부 ip를 알아낼 수 있습니다.  


이제 Test Connection 눌러서 Succeeded 가 표시되면 연결이 완료되었음을 의미합니다.
콘솔 창을 열어서 ```> select  now();``` 를 날려봅시다. 
  ![업로드중..](blob:https://velog.io/7e254f75-828d-4030-bc4b-f1b1be814c57)

 정상적으로 현재 시간이 출력되는 것을 확인할 수 있습니다. 이로써 DB 생성 및 외부 접속 설정이 완료되었네요. 
  
  
  
  
---  
 
### reference 
- https://yeopbox.com/%ec%9a%b0%eb%b6%84%ed%88%acubuntu-22-04-server-%eb%b0%a9%ed%99%94%eb%b2%bdufw-%ec%84%a4%ec%a0%95-%eb%b0%a9%eb%b2%95/
- https://velog.io/@whattsup_kim/Ubuntu-%EC%B4%88%EA%B8%B0-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0- 