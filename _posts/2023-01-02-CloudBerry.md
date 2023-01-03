---
title: (KT클라우드)cloudBerry
date: 2023-01-02 23:23:00 +09:00
categories: [클라우드, cloudBerry]
tags: [cloudBerry]		# TAG는 반드시 소문자로 이루어져야함!
---


# CloudBerry

```
윈도우즈 환경에서 KT Cloud storage에 간편하게 접속하여 파일 관리를 할 수 있는 프로그램으로 Cloudberry Explorer는 파일 업/다운로드 툴이다.

cb(cloudberry)를 사용하기 위해선 기본적으로 Object storage2.0이 필수적으로 존재해야 하며(KT클라우드를 사용하는 사용자들의 경우) CDN과 연계하여 사용할 수 있다.
```
 ![install01](/assets/img/KTcoud/cloudBerry/베리01.png)

 ```
 KT메뉴얼 - KT Cloud storage 2.0 - 클라우드 베리 이용가이드 및 다운로드 
 ```

 ![install01](/assets/img/KTcoud/cloudBerry/베리02.png)

 ![install01](/assets/img/KTcoud/cloudBerry/베리03.png)

 ```
 클라우드 베리 다운로드를 클릭하고 설치 된 툴을 실행, 해당 화면에서 소스 컴퓨터를 확인.
 ```

 ![install01](/assets/img/KTcoud/cloudBerry/베리04.png)
 
 ```
 새로운 계정(Object stroage2.0)을 추가해준다.
 ```

 ![install01](/assets/img/KTcoud/cloudBerry/베리05.png)

 ```
 KT클라우드 오브젝트 스토리지 2.0은 클라우드 스택기반, 오픈스택 클릭
 ```

 ![install01](/assets/img/KTcoud/cloudBerry/베리06.png)

 ```
 입력할 시에 주의해야할 사항.

 이용가이드에 명시가 되어있지만 한번 더 설명 하자면
 ㅁ Display name : CloudBerry에서 표시될 이름
 ㅁ User name : 사용자 이메일
 ㅁ Api key : 계정의 API key
 ㅁ Authentication Service: https://ssproxy2.ucloudbiz.olleh.com:5000/v3
 ㅁ Keystone version: 3으로 선택
 ㅁ Use scope: 해당 체크박스 체크
 ㅁ Domain ID: 사용자 계정의 domain ID 입력
 ㅁ Project ID: 사용자 계정의 project ID 입력

 주의 : Authentication Service는 꼭 메뉴얼에서 제공하는 url을 입력할 것, Use scope를 체크하면 생성되는 박스는 Domain name, Project name 으로 표시되기 때문에 이 두개를 꼭 ID로 변경해서 진행할 것.

 위의 내용을 입력하기 위해선 꼭 오브젝트 스토리지가 생성되고 API키가 발급 된 상태여야 한다.
 ```
 
 # Object Storage 2.0

![install01](/assets/img/KTcoud/Object2.0/OB01.png)

![install01](/assets/img/KTcoud/Object2.0/OB02.png)

![install01](/assets/img/KTcoud/Object2.0/OB03.png)

```
 모든 서비스 - 서비스 신청 - 원하는 서비스를 선택해서 완료하면 해당 서비스가 추가된다.

 만약 KTcloud object storage 2.0이 선택이 안된다면 구 콘솔 - 좌측 맨 하단에 상품 보기 - 스토리지 서비스 신청을 해줘야 한다.
```

![install01](/assets/img/KTcoud/Object2.0/OB04.png)

```
 서비스 신청이 완료되었다면 자동으로 좌측 메뉴바에 추가가 되기도 하지만 추가가 안되는 경우도 있기 때문에 만약 보이지가 않는다면 메뉴 설정에서 직접 추가해야 한다.
```

![install01](/assets/img/KTcoud/Object2.0/OB05.png)

![install01](/assets/img/KTcoud/Object2.0/OB08.png)

![install01](/assets/img/KTcoud/Object2.0/OB06.png)

![install01](/assets/img/KTcoud/Object2.0/OB07.png)

```
스토리지를 생성하고 스토리지 탭에 API 키에 들어가 보면 키들이 발급 된 것을 확인할 수 있다.
```

![install01](/assets/img/KTcoud/cloudBerry/베리08.png)
![install01](/assets/img/KTcoud/cloudBerry/베리09.png)

```
생성된 키를 양식에 맞춰서 집어 넣게 되면 test02라는 이름을 가진 Display name을 확인할 수 있다.

파일을 업로드 하는 방법은 좌측면 소스 컴퓨터의 경로를 찾아서 업로드 하면된다.

드래그 앤 드롭으로 파일 업로드 또한 가능하다.
```


![install01](/assets/img/KTcoud/CDN/CDN05.png)

```
 생성 된 파일박스를 체크하고 /assets/img. - 웹사이트 설정 클릭
```

![install01](/assets/img/KTcoud/CDN/CDN06.png)

```
 인덱스 페이지를 체크하고 조금 전에 업로드 해서 존재하는 index.html 파일 이름을 입력.
```

![install01](/assets/img/KTcoud/CDN/CDN07.png)

![install01](/assets/img/KTcoud/CDN/CDN08.png)

```
 상세 정보로 접근 권한이 공개로 되어 있는지 여부와 Index URL 확인
```

![install01](/assets/img/KTcoud/CDN/CDN09.png)

```
해당 URL을 주소창에 넣어서 들어가 보면 index.html을 확인할 수 있다.
```

# CDN

![install01](/assets/img/KTcoud/CDN/CDN01.png)

![install01](/assets/img/KTcoud/CDN/CDN02.png)

```
 서비스 도메인은 KT cloud 도메인 선택, 기존에 가입한 도메인이 있는 경우 고객 도메인 선택해서 사용
```

![install01](/assets/img/KTcoud/CDN/CDN03.png)

```
위치는 M2, 파일박스는 오브젝트 스토리지 2.0을 의미. 생성된 오브젝트 스토리지를 추가.
```

![install01](/assets/img/KTcoud/CDN/CDN04.png)

```
 다음을 클릭해서 생성을 완료.
```

![install01](/assets/img/KTcoud/CDN/CDN10.png)

![install01](/assets/img/KTcoud/CDN/CDN11.png)

![install01](/assets/img/KTcoud/CDN/CDN12.png)

```
 오브젝트 스토리지에서 진행했던 방법과 동일하게 해당 도메인을 입력해보면 정상 동작하고 있음을 알 수 있다.
```