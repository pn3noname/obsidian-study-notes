source: https://tryhackme.com


---

## 1. Linux commands

1. `ls -la */`: 현재 위치에서의 폴더와 그 안에 포함된 숨겨진 파일 목록까지 표시
	- 1-1. `ls -lh`: 일반 파일과 디렉토리만 표시
	- 1-2. `ls -lah`: 일반 파일 + 숨겨진 파일 + 현재 디렉토리(.) + 상위 디렉토리(..) 모두 표시
	- ex:
	```bash
# ls -lh 결과
-rw-r--r-- 1 user group 1.2K Jan 15 10:30 file.txt
drwxr-xr-x 2 user group 4.0K Jan 15 09:15 folder

# ls -lah 결과  
drwxr-xr-x 3 user group 4.0K Jan 15 10:30 .
drwxr-xr-x 5 user group 4.0K Jan 15 09:00 ..
-rw-r--r-- 1 user group  156 Jan 15 08:45 .bashrc
-rw-r--r-- 1 user group 1.2K Jan 15 10:30 file.txt
drwxr-xr-x 2 user group 4.0K Jan 15 09:15 folder
```

2. `find -name 파일이름.파일형식`: 파일 이름과 형식은 알지만, 어느 경로에 있는지 모를 때
	- ex: `find -name note.txt`

3. `find -name *.파일형식`: 파일 형식만 알고, 경로나 파일 이름 모두 모를 때
	- ex: `find -name *.txt`

4. `wc 파일이름`: word count의 줄임말. 파일이나 입력의 행 (line), 단어 (word), 문자 (character) 수를 세는 명령어
	- 주요 옵션들
		- `-l`: 행 수만 출력
		- `-w`: 단어 수만 출력
		- `-c`: 바이트 수 출력
		- `-m`: 문자 수 출력
	- ex: `wc -l file.txt`
	- ex: `echo "hello world" | wc -w`

5. `grep "찾길 원하는 키워드" 파일이름.파일형식`: 해당 파일에서 찾길 원하는 키워드를 출력
	- ex: `grep "hello" access.log`

6. Linux Operators (연산자)
	1. `&` 연산자 (백그라운드 실행)
		- 명령어를 백그라운드에서 실행할 수 있게 해줌
		- 이를 통해 시간이 오래 걸리는 작업을 실행하면서도 다른 작업을 계속할 수 있음
		- ex:
			```bash
# 큰 파일을 복사하면서 백그라운드에서 실행
cp large_file.zip backup/ &

# 백그라운드에서 로그 파일 모니터링
tail -f /var/log/system.log &

# 백그라운드에서 파일 압축
tar -czf backup.tar.gz /home/user/documents &
```

	2. `&&` 연산자 (조건부 실행)
		- 여러 명령어를 연결하되, 앞의 명령어가 성공했을 때만 다음 명령어를 실행함
		- ex: 
			```bash
# 디렉토리 생성 후 그 안으로 이동
mkdir project && cd project

# 파일 업데이트 후 서비스 재시작
apt update && apt upgrade && systemctl restart apache2

# 파일 존재 확인 후 내용 출력
ls file.txt && cat file.txt
```

	3. `>` 연산자 (출력 리디렉션)
		- 명령어의 출력을 파일로 리디렉션함
		- 파일이 이미 존재하면 내용을 덮어씀
		- ex:
			```bash
# "안녕하세요"라는 내용으로 greeting.txt 파일 생성
echo "안녕하세요" > greeting.txt

# 현재 디렉토리 목록을 파일로 저장
ls -la > directory_list.txt

# 시스템 정보를 파일로 저장
uname -a > system_info.txt
```
		- 실행 결과:
			```bash
$ echo "안녕하세요" > greeting.txt
$ cat greeting.txt
안녕하세요
```

	4. `>>` 연산자 (출력 추가)
		- `>` 연산자와 비슷하지만, 파일 내용을 덮어쓰지 않고 파일 끝에 추가함
		- ex:
			```bash
# 기존 파일에 새로운 내용 추가
echo "반갑습니다" >> greeting.txt

# 로그 파일에 새로운 항목 추가
echo "$(date): 사용자 로그인" >> app.log

# 여러 줄을 차례로 추가
echo "첫 번째 줄" >> notes.txt
echo "두 번째 줄" >> notes.txt
echo "세 번째 줄" >> notes.txt
```
		- 실행 결과:
			```bash
$ echo "안녕하세요" > greeting.txt
$ echo "반갑습니다" >> greeting.txt
$ cat greeting.txt
안녕하세요
반갑습니다
```

	5. 실제 활용 예시
		- ex:
			```bash
# 백업을 만들고 성공하면 로그에 기록
tar -czf backup.tar.gz /home/user/ && echo "$(date): 백업 완료" >> backup.log &

# 시스템 상태 확인 후 결과를 파일로 저장
ps aux > processes.txt && df -h >> processes.txt && free -h >> processes.txt
```

7. `rm`: removing files and folders

8. `rm -r 폴더이름`: removing folder

9. `cp 복사하고 싶은 파일 복사된 파일 이름`: 파일 복사하기
	- ex: `cp note.txt note1.txt`

10. `mv 원본파일 대상파일`: 파일을 이동하거나 파일 이름 변경시 사용
	- ex1: 파일 이름 변경
	```bash
# note2 파일을 note3으로 이름 변경
mv note2 note3
```
		- 결과: `note2` 파일이 `note3`으로 이름이 바뀜
		- `note3`은 기존 `note2`의 내용을 그대로 가지게 됨
		- 원본 `note2` 파일은 더 이상 존재하지 않음

	- ex2: 파일을 다른 폴더로 이동
		```bash
# document.txt를 Documents 폴더로 이동
mv document.txt ~/Documents/
```

	- ex3: 파일 이동과 동시에 이름 변경
		```bash
# report.txt를 Documents 폴더로 이동하면서 final_report.txt로 이름 변경
mv report.txt ~/Documents/final_report.txt
```

	- ex4: 폴더 이름 변경
		```bash
# old_folder를 new_folder로 이름 변경
mv old_folder new_folder
```

> [!WARNING]
> - `mv`는 파일을 **이동**시키므로 원본 파일이 사라짐
> -  대상 위치에 같은 이름의 파일이 있으면 덮어쓰기 됨
> -  실수로 중요한 파일을 덮어쓰지 않도록 주의해야 함

11. `file 파일이름`: 파일의 확장자가 정해지지 않은 파일의 형식을 보는 명령어

12. `su 전환하려는 사용자명`: 원하는 유저로 전환되지만, 이전 사용자의 홈 디렉토리에 그대로 남아있음
	- ex:
	```bash
tryhackme@linux2:~$ su user2
Password:
user2@linux2:/home/tryhackme$
```

13. `su -l 전환하려는 사용자명`: 원하는 유저로 전환되면서, 원하는 홈 디렉토리로 자동으로 이동하며, 완전한 로그인 환경을 제공받음
	- ex:
	```bash
tryhackme@linux2:~$ su -l user2
Password:
user2@linux2:~$ pwd
user2@:/home/user2$
```


---

## 2. Root directories

1. /etc 디렉토리
	- etc 폴더는 운영체제에서 사용하는 시스템 파일들을 저장하는 일반적인 위치
	- 주요 파일들
		- sudoers 파일: sudo를 실행하거나 root 사용자 권한으로 명령을 실행할 수 있는 사용자와 그룹 목록이 포함됨
		- passwd와 shadow 파일: 리눅스에서 각 사용자의 비밀번호를 sha512라는 암호화 형식으로 저장하는 방법을 보여주는 특별한 파일들
	
2. /var 디렉토리
	- variable data (가변 데이터)의 줄임말
	- 리눅스 설치에서 발견되는 주요 루트 폴더 중 하나
	- 이 폴더는 시스템에서 실행되는 서비스나 애플리케이션이 자주 접근하거나 작성하는 데이터를 저장함
	- 주요 용도
		- `/var/log`: 실행 중인 서비스와 애플리케이션의 로그 파일 저장
		- 특정 사용자와 연관되지 않은 데이터 (ex: 데이터베이스 등) 저장
	
3. /root 디렉토리
	- `/home` 디렉토리와 달리, 실제로 "root" 시스템 사용자의 홈 디렉토리임
	- 이 폴더는 root 사용자의 홈 디렉토리라는 점을 이해하는 것이 중요함
	- 일반적으로 `/home/root`에 있을 것으로 예상하지만, 실제로는 `/root`에 위치함
	
4. /tmp 디렉토리
	- 리눅스 설치에서 발견되는 독특한 루트 디렉토리
	- temporary의 줄임말
	- `/tmp` 디렉토리는 휘발성이며, 한두 번만 접근하면 되는 데이터를 저장하는 데 사용됨
	- 컴퓨터의 메모리와 유사하게, 컴퓨터가 재시작되면 이 폴더의 내용은 모두 삭제됨
	- pen testing 관점에서의 유용성
		- 기본적으로 모든 사용자가 이 폴더에 쓸 수 있음
		- 시스템에 접근한 후 열거 스크립트 등을 저장하기 좋은 장소로 활용 가능


---

## 3. Transferring files

1. Downloading Files (Wget)
	- 프로그램, 스크립트, 또는 이미지를 다운로드해야 할 때
	- 사용법
		- `wget`은 웹에서 HTTP를 통해 파일을 다운로드할 수 있는 명령어
		- 브라우저에서 파일에 접근하는 것과 같은 방식으로 작동함
		- 다운로드하려는 리소스의 주소만 제공하면 됨
	- ex:
	```bash
wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt
```

2. Transferring files from your host - SCP (SSH)
	- 호스트 간 파일 전송
	- SCP (Secure Copy)는 파일을 안전하게 복사하는 방법
	- 일반적인 `cp`명령어와 달리, SSH 프로토콜을 사용하여 두 컴퓨터 간에 인증과 암호화를 제공함
	- SCP 작동 방식
		- SOURCE (소스)와 DESTINATION (목적지) 모델로 작동하며, 다음과 같은 작업이 가능함
			- 현재 시스템에서 원격 시스템으로 파일 및 디렉토리 복사
				```bash
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
```
			- 원격 시스템에서 현재 시스템으로 파일 및 디렉토리 복사
				```bash
scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt
```

3. Serving files from your host - WEB
	- 호스트에서 파일 제공 - 웹 서버
	- Ubuntu 머신에는 Python3가 기본으로 설치되어 있음
	- Python은 "HTTPServer"라는 가벼운 모듈을 제공하여 컴퓨터를 간단한 웹 서버로 만들 수 있음
	- Python3 웹 서버 시작
		```bash
# 이 명령어를 실행하면 현재 디렉토리의 파일들이 제공됨
python3 -m http.server
```
	- 웹 서버 사용 예시
		```bash
# 웹 서버 시작 (한 터미널에서)
tryhackme@linux3:/webserver# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...

# 다른 터미널에서 파일 다운로드
tryhackme@mymachine:~# wget http://10.10.25.83:8000/myfile
```
	- 주의사항
		- Python3 서버는 기본적으로 포트 8000에서 실해됨
		- wget 명령어에서 포트 번호를 명시해야 함
		- 웹 서버를 실행한 터미널은 유지해야 하며, wget은 다른 터미널에서 실행해야 함
		- 파일의 정확한 이름과 위치를 알아야 함 (인덱싱 기능이 없음)
	- 웹 서버의 한계
		- 이 모듈은 인덱싱 방법이 없어서 사용하려는 파일의 정확한 이름과 위치를 알아야 함
		- 더 고급 기능을 원한다면 Updog 같은 다른 웹 서버를 사용하는 것이 좋음

> [!INFO]
> ### 파이썬 웹 서버가 작동하는 방식
> #### 1. 웹 서버가 실행되는 과정
> ```bash
> python3 -m http.server
> ```
> 이 명령어를 실행하면:
> 	- 현재 터미널이 웹 서버 프로그램을 실행하는 상태가 됨
> 	- 서버가 계속 실행되면서 다른 컴퓨터의 요청을 기다림
> 	- 마치 식당에서 손님을 기다리는 웨이터처럼 계속 대기 상태
> #### 2. 왜 터미널이 "점유" 되는가?
> 	- 웹 서버는 '포그라운드 프로세스 (Foreground process)'로 실행됨
> 	- 즉, 서버가 실행되는 동안 그 터미널은 서버 프로그램이 사용하고 있어서 다른 명령어를 입력할 수 없음
> 	- 터미널 창이 서버의 상태 메시지를 계속 보여주며, 누군가 파일을 요청하면 로그가 출력됨
> #### 3. 왜 다른 터미널에서 wget을 실행해야 하는가?
> 상황 예시:
> 	- 터미널 1: 웹 서버 실행 중 (손님을 기다리는 웨이터)
> 	- 터미널 2: 손님 역할 (파일을 요청하는 클라이언트)
> ```bash
> # 터미널 1 (웹 서버)
$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
> # 여기서 멈춰있음 - 요청을 기다리는 중
> 
> # 터미널 2 (클라이언트)
> $ wget http://localhost:8000/myfile.txt
> # 이때 터미널 1에 접속 로그가 나타남
> ```
> #### 4. 실제 사용 시나리오
> 시나리오 A: 같은 컴퓨터에서 테스트
> 	- 터미널 1에서 웹 서버 실행
> 	- 터미널 2에서 `wget http://localhost:8000/파일명`으로 다운로드 테스트
> 시나리오 B: 다른 컴퓨터에서 접근
> 	- 컴퓨터 A: 웹 서버 실행 (IP: 192.168.1.10)
> 	- 컴퓨터 B: `wget http://192.168.1.10:8000/파일명`으로 다운로드
> #### 5. 왜 이렇게 복잡한가?
> 웹 서버는 `서비스`라서, 마치 가게를 운영하는 것과 같음
> 	- 가게 주인은 계속 가게에 있어야 함 (웹 서버 실행)
> 	- 손님은 가게에 와서 물건을 사감 (wget으로 파일 다운로드)
> 	- 가게 주인이 자리를 비우면 손님이 와도 물건을 살 수 없음
> #### 6. 백그라운드 실행 방법
> 만약 한 터미널에서 모든 것을 하고 싶다면:
> ```bash
> # 백그라운드에서 웹 서버 실행
   python3 -m http.server &
> # 같은 터미널에서 wget 실행 가능
> wget http://localhost:8000/myfile.txt
> ```
> 위와 같이 하면 웹 서버가 백그라운드에서 실행되어 같은 터미널에서 다른 명령어도 사용할 수 있음

> [!TIPS]
> ### Summary
> 1. wget: 웹에서 파일 다운로드
> 2. SCP: SSH를 통한 안전한 파일 전송
> 3. Python 웹 서버: 로컬 파일을 웹으로 제공

---

## 4. Linux process management

1. 프로세스란?
	- 컴퓨터에서 실행되는 프로그램을 의미
	- 각 프로세스는 커널에 의해 관리되며, PID (Process ID)라는 고유한 식별자를 가짐
	- PID는 프로세스가 시작되는 순서대로 증가함
	- ex: 60번째로 시작된 프로세스 => PID 60

> [!INFO]
> ### Kernel
> - 운영체제의 핵심 부분
> - 하드웨어와 소프트웨어 사이의 중개자 역할
> - 기본 개념
> 	- 시스템의 가장 낮은 수준에서 동작하며, 컴퓨터의 모든 자원을 관리하고 제어함
> 	- 사용자 프로그램들이 하드웨어에 직접 접근할 수 없게 하고, 대신 커널을 통해서만 접근할 수 있도록 함
> - 프로세스 관리에서 커널의 역할:
> 	- 커널은 프로세스의 생성, 실행, 종료를 담당함
> 	- 새로운 프로세스가 만들어질 때 메모리 공간을 할당하고, CPU 시간을 분배하며, 프로세스 간의 통신을 관리함
> 	- 스케쥴링도 커널의 중요한 기능
> 	- 여러 프로세스가 동시에 실행되는 것처럼 보이게 하기 위해 CPU 시간을 아주 짧은 단위로 나누어 각 프로세스에게 번갈아 할당함
> 	- 커널 모드 vs. 사용자 모드: 프로세스는 두 가지 모드에서 실행됨
> 		- 사용자 모드에서는 제한된 권한으로 실행됨
> 		- 시스템 호출을 통해 커널 모드로 전환되어 하드웨어 자원에 접근할 수 있음
> 	- 메모리 관리: 커널은 각 프로세스마다 독립적인 메모리 공간을 제공하고, 프로세스들이 서로의 메모리를 침범하지 않도록 보호함
> 	- 정리하면, 커널은 프로세스들이 안전하고 효율적으로 실행될 수 있도록 모든 시스템 자원을 관리하는 교통정리 역할

2. 프로세스 확인하기
	1. `ps` 명령어
		- 현재 사용자 세션에서 실행 중인 프로세스 목록을 확인할 수 있음
		- 이 명령어는 다음 정보를 제공함
			- 상태 코드
			- 실행 중인 세션
			- CPU 사용 시간
			- 실제 실행 중인 프로그램 또는 명령어 이름
	
	2. ps aux` 명령어
		- 다른 사용자가 실행한 프로세스와 시스템 프로세스까지 모두 확인하기 위한 명령어

	3. `top` 명령어
		- 실시간으로 프로세스 상태를 모니터링할 수 있음
		- 10초마다 자동으로 새로고침
		- 방향키로 다양한 행을 탐색 가능

3. 프로세스 관리하기
	1. 프로세스 종료
		- `kill` 명령어를 사용하여 프로세스를 종료할 수 있음
		- ex:
	```bash
kill [PID]
```

	2. 시그널 종류
		- 프로세스를 종료할 때 보낼 수 있는 시그널
			- `SIGTERM`: 프로세스를 종료하되, 정리 작업을 수행할 시간을 줌
				(if we wanted to `cleanly` kill a process)
			- `SIGKILL`: 프로세스를 즉시 강제 종료 (정리 작업 없음)
			- `SIGSTOP`: 프로세스를 일시 중단 / 정지

4. 프로세스 시작 방식
	1. 네임스페이스
		-  운영체제는 네임스페이스를 사용하여 CPU, RAM, 우선순위 등의 시스템 리소스를  프로세스별로 분할함 (케이크를 조각으로 나누는 것과 같은 개념)
		- 장점:
			- 보안성 향상
			- 프로세스 간 격리
			- 같은 네임스페이스 내의 프로세스만 서로 확인 가능

	2. `systemd`
		- PID 0번 프로세스는 시스템 부팅 시 시작됨
		- Ubuntu에서는 systemd가 init 프로세스 역할을 함
		- 모든 프로그램은 systemd의 자식 프로세스로 시작됨

5. 부팅 시 서비스 시작하기
	- `systemctl` 명령어
		- 시스템 서비스를 관리하는 명령어
		- 시스템의 서비스와 장치들의 상태를 보여주는 출력
		- macOS에서는 `launchctl`
		- ex:
			```bash
systemctl [옵션] [서비스명]
```

	- 주요 옵션
		- `start`: 서비스 시작
		- `stop`: 서비스 중지
		- `enable`: 부팅 시 자동 시작 설정
		- `disable`: 부팅 시 자동 시작 해제
		- ex:
			```bash
systemctl start apache2    # 아파치 웹서버 시작
systemctl enable apache2   # 부팅 시 자동 시작 설정
```

6. 백그라운드와 포그라운드 실행
	1. 백그라운드 실행
		- 프로세스를 백그라운드에서 실행하는 방법:
			1. `&` 연산자 사용
				```bash
echo "Hi THM" &
```

			2. `ctrl + z` 사용: 실행 중인 프로세스를 일시 중단하고 백그라운드로 보냄

	2. 포그라운드 복귀 
		- 백그라운드에서 실행 중인 프로세스를 다시 포그라운드로 가져오기:
			```bash
fg
```

	3. 백그라운드 실행의 장점
		- 파일 복사 등의 시간이 오래 걸리는 작업을 백그라운드에서 실행
		- 다른 명령어를 동시에 실행 가능
		- 스크립트 실행 시 터미널을 계속 사용 가능

7. 실용적인 활용 예시
	1. 긴 작업을 백그라운드에서 실행:
		```bash
cp large_file.txt /backup/ &
```

	2. 실행 중인 스크립트 일시 중단: 스크립트 실행 중 `ctrl + z` 를 눌러 백그라운드로 보내기
	
	3. 백그라운드 프로세스 확인:
		```bash
ps aux | grep 프로세스명
```

	4. 포그라운드로 복귀: `fg`

---

## 5. Maintaining your system: Automation

1. Cron과 Crontab 개요
	- 사용자들은 시스템이 부팅된 후, 특정 작업이나 태스크를 자동으로 실행하도록 예약하고 싶어할 수 있음
	- ex: 명령어 실행, 파일 백업, Spotify, Google Chrome과 같은 즐겨 사용하는 프로그램을 실행하는 것

2. Cron 프로세스와 Crontab
	- Cron은 부팅 시 시작되는 프로세스 중 하나로, cron 작업을 관리하고 실행하는 역할을 담당함
	- Crontab은 cron 프로세스가 인식할 수 있는 특별한 형식의 파일로, 각 줄을 단계별로 실행함

3. Crontab의 구조
	- Crontab은 6개의 특정 값이 필요함
	![[스크린샷 2025-07-06 오후 3.42.50.png]]

4. 실제 사용 예시
	- "pn3noname" 사용자의 "Documents" 폴더를 12시간마다 백업하는 예시:
```
0 */12 * * * cp -R /home/pn3noname/Documents /var/backups/
```

5. 와일드카드 사용
	- Crontab의 흥미로운 기능 중 하나는 와일드카드 (`*`) 지원
	- 특정 필드에 값을 지정하지 않으려면 (ex: 월, 일, 년도와 상관없이 12시간마다 실행하고 싶은 경우) 단순히 별표를 사용하면 됨

6. 도움이 되는 도구
	- Crontab Generator: 친화적인 애플리케이션을 통해 형식을 자동 생성
	- Cron Guru: 크론탭 형식 생성을 도와주는 사이트

7. 편집 방법
	- Crontab은 `crontab -e` 명령어를 사용하여 편집할 수 있으며, 이때 Nano와 같은 편집기를 선택하여 크론탭을 수정할 수 있음

---

## 6. Maintaining your system: Package Management

1. 






































