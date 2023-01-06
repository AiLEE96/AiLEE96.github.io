---
title: (KT클라우드)cloudBerry
date: 2023-01-02 23:23:00 +09:00
categories: [클라우드, TOOL]
tags: [cloudberry]		# TAG는 반드시 소문자로 이루어져야함!
---


# CloudBerry


윈도우즈 환경에서 KT Cloud storage에 간편하게 접속하여 파일 관리를 할 수 있는 프로그램으로 Cloudberry Explorer는 파일 업/다운로드 툴이다.

cb(cloudberry)를 사용하기 위해선 기본적으로 Object storage2.0이 필수적으로 존재해야 하며(KT클라우드를 사용하는 사용자들의 경우) berry과 연계하여 사용할 수 있다.

 ![berry](./assets/img/KTcoud/cloudBerry/by01.png)

 
 KT메뉴얼 - KT Cloud storage 2.0 - 클라우드 by 이용가이드 및 다운로드 
 

 ![berry](./assets/img/KTcoud/cloudBerry/by02.png)

 ![berry](./assets/img/KTcoud/cloudBerry/by03.png)


 클라우드 by 다운로드를 클릭하고 설치 된 툴을 실행, 해당 화면에서 소스 컴퓨터를 확인.
 

 ![berry](./assets/img/KTcoud/cloudBerry/by04.png)
 
 
 새로운 계정(Object stroage2.0)을 추가해준다.
 

 ![berry](./assets/img/KTcoud/cloudBerry/by05.png)

 
 KT클라우드 오브젝트 스토리지 2.0은 클라우드 스택기반, 오픈스택 클릭
 

 ![berry](./assets/img/KTcoud/cloudBerry/by06.png)

 
 입력할 시에 주의해야할 사항.

 이용가이드에 명시가 되어있지만 한번 더 설명 하자면 <br/>
 ㅁ Display name : CloudBerry에서 표시될 이름 <br/>
 ㅁ User name : 사용자 이메일 <br/>
 ㅁ Api key : 계정의 API key <br/>
 ㅁ Authentication Service: https://ssproxy2.ucloudbiz.olleh.com:5000/v3 <br/>
 ㅁ Keystone version: 3으로 선택 <br/>
 ㅁ Use scope: 해당 체크박스 체크 <br/>
 ㅁ Domain ID: 사용자 계정의 domain ID 입력 <br/>
 ㅁ Project ID: 사용자 계정의 project ID 입력 <br/>

 주의 : Authentication Service는 꼭 메뉴얼에서 제공하는 url을 입력할 것, Use scope를 체크하면 생성되는 박스는 Domain name, Project name 으로 표시되기 때문에 이 두개를 꼭 ID로 변경해서 진행할 것.

 위의 내용을 입력하기 위해선 꼭 오브젝트 스토리지가 생성되고 API키가 발급 된 상태여야 한다.