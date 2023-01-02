# NAS

```
NAS는 Network Attached Storage의 약자로 네트워크로 연결된 하드디스크(스토리지)를 의미하며 다양한 파일들(사진, 비디오, 음악, 문서 등)을 저장 및 관리 그리고 공유가 가능한 네트워크 저장 장치이다.
```

 ![install01](./img/KTcoud/NAS/NAS01.png)

 ```
 NAS - NAS volume - 볼륨생성
 ```

 ![install01](./img/KTcoud/NAS/NAS02.png)

 ```
 빈칸 입력, 이때 프로토콜 신경써야 한다.

 만약 리눅스 서버에 연결 할 예정이라면 NFS(Network File System)프로토콜을 윈도우 서버에 연결 할 예정이라면 CIFS(Common Internet File system) 프로토콜을 사용한다.

 그러나 일반적으로 윈도우든 리눅스 서버든 NFS를 권장한다.

 ㄴ버전 문제로 인해서 윈도우에선 CIFS를 이용했지만 현재는 지양한다.
 ```
 

# NAS - 리눅스 연동

![install01](./img/KTcoud/NAS/NAS03.png)

```
 리눅스 서버 접속

 -df -h #디스크 확인
```
![install01](./img/KTcoud/NAS/NAS04.png)

```
mount -t nfs [NAS서버 아이피/mount path] [내 디렉토리 경로] 

정상적으로 마운트 된 모습을 확인 할 수 있다.

그러나 이렇게 마운트만 했을 경우엔 서버를 재시작 했을 때 NAS가 떨어져 나감으로 /etc/fstab에 등록을 해 주어야 한다.
```

![install01](./img/KTcoud/NAS/NAS05.png)

```
 vi /etc/fstab
 [NAS 아이피]:[마운트 패스] [리눅스 서버의 마운트 할 디렉토리] [프로토콜(파일시스템 종류)] [옵션,방식(AUTO)] [우선순위]

 이후 재부팅을 해도 NAS가 해제되지 않고 잘 붙어있다.
```

# 오류

```
mount -t nfs 172.27.128.1/mountpath01 /root
ㄴ mount.nfs: remote share not in 'host:dir' format

mount -t nfs 172.27.128.1:mountpath01 /root
ㄴ 172.27.128.1/mountpath01 [/] -> 172.27.128.1:mountpath01 [:] 으로 변경


mount -t nfs 172.27.128.1:mountpath01 /root
ㄴroot에 마운트 시켜서 프롬프트가 bash로 변경되었다. 순간 뭐가 문제인지 생각했다가 root 디렉토리에 NAS를 마운트 시켰다는 걸 알게됐다.

꼭 디렉토리를 하나 생성해서 마운트 시켜주자.
```

# NAS - Window 연동

![install01](./img/KTcoud/NAS/NAS06.png)

```
 윈도우 서버 생성
```

![install01](./img/KTcoud/NAS/NAS07.png)

```
 생성된 서버 네트워크 연결.
```

![install01](./img/KTcoud/NAS/NAS08.png)

```
 네트워크 방화벽에서 3389 포트 등록, 윈도우 원격 데스크톱 연결을 위해선 3389번 포트가 허용되어 있어야 한다.

 이때 내 컴퓨터의 공인 IP를 넣어줄 것.
```

![install01](./img/KTcoud/NAS/NAS09.png)

![install01](./img/KTcoud/NAS/NAS10.png)

![install01](./img/KTcoud/NAS/NAS11.png)

```
 IP 입력 - 사용자 입력(Administrator) - 비밀번호 입력(서버가 생성 될 때 부여 된 비밀번호)
```

![install01](./img/KTcoud/NAS/NAS12.png)

```
 윈도우 서버에 접속 - 내 PC - 네트워크 드라이브 연결 - \\NAS(CIFS)아이피 주소\mountpath 입력.
```

![install01](./img/KTcoud/NAS/NAS13.png)

```
 연결 확인.
```
