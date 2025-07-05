1. `ls -la */`: 현재 위치에서의 폴더와 그 안에 포함된 파일 목록까지 표시

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