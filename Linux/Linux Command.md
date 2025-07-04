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
