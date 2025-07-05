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

> [!TIPS]
> 1. wget: 웹에서 파일 다운로드
> 2. SCP: SSH를 통한 안전한 파일 전송
> 3. Python 웹 서버: 로컬 파일을 웹으로 제공

