# Install Blazemeter


 ![install01](./img/Jmeter/Blaze01.png)

```
 크롬 웹 스토어에서 설치가 가능한 Blazemeter는 Jmeter를 보다 효율적이고 쉽게 사용하도록 도와주는 툴.
```

![install01](./img/Jmeter/Blaze02.png)

```
 웹 스토어에서 인스톨이 끝나면 웹 확장 - 블레이즈 미터 선택
```

![install01](./img/Jmeter/Blaze03.png)

```
 스타트 레코딩을 클릭, 특정 모션을 수행 하면 해당 모션을 통해서 발생한 이벤트가 저장이 된다.
```

![install01](./img/Jmeter/Blaze04.png)

```
 동영상 중지
```

![install01](./img/Jmeter/Blaze05.png)

```
 다시 한 번 확장 프로그램 클릭 - 블레이즈 미터 클릭 - 세이브 클릭
```

![install01](./img/Jmeter/Blaze06.png)

![install01](./img/Jmeter/Blaze07.png)

```
 Jmeter 선택 - Domains to includ에서 가장 상단 선택 - 세이브 클릭 
```

![install01](./img/Jmeter/Blaze08.png)

```
 JMX파일이 생성 된 걸 확인
```

# Install Jmeter

![install01](./img/Jmeter/Jmeter01.png)

```
 Jmeter 홈페이지 접속 - Download Releases
```

![install01](./img/Jmeter/Jmeter02.png)

```
 Apache Jmeter 5.5(Requires Java 8+) - zip 파일 다운로드

 이때 Jmeter는 Java 기반임으로 요구하는 Java 8+ 이상의 버전이 컴퓨터에 설치가 되어 있어야 한다.
```

![install01](./img/Jmeter/Jmeter03.png)

```
 Jmeter 설치가 완료 되고 나면 bin - jmeter.bat을 실행
```

![install01](./img/Jmeter/Jmeter04.png)

```
 만약 자바가 설치가 안됐다면 이런 에러 메시지가 출력된다.
```

![install01](./img/Jmeter/Jmeter05.png)

```
 Open JDK 8+ 이상이 설치되어 있다면 위와 같은 화면이 출력된다.
```

# Open JDK 11

![install01](./img/Jmeter/JDK01.png)

```
 JDK 홈페이지 접속 - Open JDK Archive 접속
```

![install01](./img/Jmeter/JDK02.png)

```
 원하는 버전 선택(단, Jmeter를 사용하기 위해선 8+ 이상을 선택해야한다.) Window.zip.18.0.2 선택
```

![install01](./img/Jmeter/JDK03.png)

```
 JDK zip을 해제하고 C:\\Program\\openJDK 폴더를 생성하고 압축을 해제해서 생성 된 내용물을 해당 폴더에 옮겨 놓기
```

![install01](./img/Jmeter/JDK04.png)

```
 시스템 환경 변수 편집을 입력.

 설치된 JAVA를 시스템 환경변수에 등록하는 작업을 해줘야 한다.
```

![install01](./img/Jmeter/JDK05.png)

![install01](./img/Jmeter/JDK06.png)

![install01](./img/Jmeter/JDK07.png)

![install01](./img/Jmeter/JDK08.png)

![install01](./img/Jmeter/JDK09.png)

```
 환경변수 적용까지 끝나고 cmd에서 java -version 입력시 정상적으로 버전이 나오면 끝.
```
# Config Jmeter

![install01](./img/Jmeter/Jmeter06.png)

```
Jmeter 화면에서 Open 클릭
```

![install01](./img/Jmeter/Jmeter07.png)

```
 Blaze meter에 의해서 생성 된 Jmx 파일을 선택
```

![install01](./img/Jmeter/Jmeter08.png)

```
 스레드 그룹에서 Number of threads(트래픽, 또는 유저의 수), Ramp-up period(부하를 주는 시간) 설정.
```

![install01](./img/Jmeter/Jmeter11.png)

```
 스레드를 설정하고 테스트라는 항목 아래에 생성된 HHTP Request(네이버에서 진행했기 대문에 네이버 도메인이 나온다) 우클릭.

 [이때 스레드를 설정한 사진과 현재 사진의 HTTP Request가 다른 이유는 편집 할 때 단순히 다른 JMX파일을 불러오면서 편집해서 그럼 - 실수]

 Add - Listener - View Resultes tree를 클릭.

 View Results는 부하를 테스트 하는데 실시간으로 해당 서버에 HTTP Request 요청이 정상적으로 잘 진행되는지 확인할 수 있도록 도와주는 역할을 한다.
```

![install01](./img/Jmeter/Jmeter12.png)


![install01](./img/Jmeter/Jmeter13.png)


