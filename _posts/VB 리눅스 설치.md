# 버추얼박스 리눅스 설치

![install01](./img/LinuxInstall/스크린샷(1).png)
```
버추얼 박스 설치 전 비주얼 C++ 설치
```
![install01](./img/LinuxInstall/스크린샷(2).png)

![install01](./img/LinuxInstall/스크린샷(3).png)

![install01](./img/LinuxInstall/스크린샷(4).png)

![install01](./img/LinuxInstall/스크린샷(5).png)
```
머신 - 새로만들기
```
![install01](./img/LinuxInstall/스크린샷(6).png)
```
이름 입력 - 이미지 설정(이미지 설정을 하지 않고 진행해도 상관없다. 나중에 실행과정에서 이미지를 선택해서 인스톨이 가능)
```
![install01](./img/LinuxInstall/스크린샷(7).png)

![install01](./img/LinuxInstall/스크린샷(8).png)
```
유저네임, 비밀번호, 호스트 네임을 설정
```

![install01](./img/LinuxInstall/스크린샷(9).png)
```
기본메모리, CPU 코어 개수 설정
```

![install01](./img/LinuxInstall/스크린샷(10).png)
```
디스크 사이즈 설정
```
![install01](./img/LinuxInstall/스크린샷(11).png)

![install01](./img/LinuxInstall/스크린샷(12).png)

![install01](./img/LinuxInstall/스크린샷(13).png)
```
가상머신을 실행했을 때 이미지를 선택하지 않았기 대문에 부팅할 이미지가 없다고 출력, Centos7 이미지를 넣어주고 실행
```

![install01](./img/LinuxInstall/스크린샷(14).png)
```
인스톨
```

![install01](./img/LinuxInstall/스크린샷(15).png)
```
언어 선택
```
![install01](./img/LinuxInstall/스크린샷(16).png)

![install01](./img/LinuxInstall/스크린샷(17).png)
```
소프트웨어 선택 - 인프라 서버 - 필요한 기능 선택
```
![install01](./img/LinuxInstall/스크린샷(18).png)

![install01](./img/LinuxInstall/스크린샷(19).png)
```
설치 대상 - 디스크 파트 설정 - 파티션을 설정합니다 체크 - 완료
```
![install01](./img/LinuxInstall/스크린샷(20).png)
```
LVM을 표준파티션으로 변경 - + 선택 -  새 마운트 지점 추가에서 마우트 지점과 용량 할당(이때 처음에 설정하는 마운트 지점은 시스템이 담기는 부분이므로 적절한 용량 선택)
```

![install01](./img/LinuxInstall/스크린샷(21).png)
```
장치 유형은 표준 파티션, 파일 시스템은 xfs

현재 설정이 위와 같이 때문에 이렇게 설정했을 뿐 LVM선택을 하면 설정은 바뀌게 된다 이때 시스템이 설치되는 마운트 지점은 LVM 선택이 불가능하다
```
![install01](./img/LinuxInstall/스크린샷(23).png)
```
변경사항 적용
```
![install01](./img/LinuxInstall/스크린샷(24).png)
```
설치완료
```