1. `ls -la */`: 현재 위치에서의 폴더와 그 안에 포함된 파일 목록까지 표시

2. `find -name 파일이름.파일형식`: 파일 이름과 형식은 알지만, 어느 경로에 있는지 모를 때
	- ex: `find -name note.txt`

3. `find -name *.파일형식`: 파일 형식만 알고, 경로나 파일 이름 모두 모를 때
	- ex: `find -name *.txt`

4. `wc 파일이름`: word count의 줄임말. 파일이나 입력의 행(line), 단어(word), 문자(character) 수를 세는 명령어
	- 주요 옵션들
		- `-l`: 행 수만 출력
		- `-w`: 단어 수만 출력
		- `-c`: 바이트 수 출력
		- `-m`: 문자 수 출력
	- ex: `wc -l file.txt`
	- ex: `echo "hello world" | wc -w`

5. `grep "찾길 원하는 키워드" 파일이름.파일형식`: 해당 파일에서 찾길 원하는 키워드를 출력
	- ex: `grep "hello" access.log`