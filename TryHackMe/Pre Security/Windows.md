source: https://tryhackme.com


---

## 1. The file system

1. 파일 시스템 개요
	- 현대 Windows 버전에서 사용되는 파일 시스템은 `NTFS (New Technology File System)`
	- 이전 파일 시스템들
		- FAT16 / FAT32 (File Allocation Table)
		- HPFS (High Performance File System)
	- FAT 파티션은 현재도 USB 장치, MicroSD 카드 등에서 사용되지만, 개인용 Windows 컴퓨터나 서버에서는 일반적으로 사용되지 않음

2. NTFS의 특징
	- 저널링 파일 시스템
		- NTFS는 저널링 파일 시스템으로, 장애 발생 시 로그 파일에 저장된 정보를 사용하여 폴더 / 파일을 자동으로 복구할 수 있음
		- 이 기능은 FAT에서는 불가능

	- 주요 개선사항
		- 4GB 이상 파일 지원
		- 폴더 및 파일에 대한 세부 권한 설정
		- 폴더 및 파일 압축
		- 암호화 (EFS - Encryption File System)

3. 파일 시스템 확인 방법
	- Windows에서 사용 중인 파일 시스템을 확인하려면:
		1. C 드라이브 (C:)를 마우스 오른쪽 버튼으로 클릭
		2. "속성" 선택하여 확인 가능

4. NTFS 권한 시스템
	- 권한 유형
		- 모든 권한 (Full control)
		- 수정 (Modify)
		- 읽기 및 실행 (Read & Execute)
		- 폴더 내용 나열 (List folder contents)
		- 읽기 (Read)
		- 쓰기 (Write)

	- 권한 확인 방법
		1. 파일 또는 폴더를 마우스 오른쪽 버튼으로 클릭
		2. "속성" 선택
		3. "보안" 탭 클릭
		4. "그룹 또는 사용자 이름" 목록에서 확인하고자 하는 사용자 / 그룹 선택

5. 대체 데이터 스트림 (ADS; Alternate Data Streams)
	- ADS 개념: Windows NTFS 파일 시스템에만 있는 파일 속성

	- 특징
		- 모든 파일은 최소 하나의 데이터 스트림 ($DATA)을 가짐
		- ADS를 통해 파일이 하나 이상의 데이터 스트림을 포함할 수 있음
		- Windows 탐색기에서는 기본적으로 ADS를 표시하지 않음
		- PowerShell을 통해 ADS 확인 가능

	- 보안 관점
		- 악성 용도: 악성코드 작성자들이 데이터 숨김 목적으로 사용
		- 정상 용도: 인터넷 파일 다운로드 시 출처 식별 정보 저장

	- ADS에 대한 자세한 정보는 MalwareBytes의 문서 참고
		(https://www.malwarebytes.com/blog/101/2015/07/introduction-to-alternate-data-streams)










