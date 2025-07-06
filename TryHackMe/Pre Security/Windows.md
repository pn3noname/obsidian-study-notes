source: https://tryhackme.com


---

## 1. The file system

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

---

## 2. Windows\System32 폴더

1. Windows 폴더 (`C:\Windows`)
	- Windows 운영체제가 설치되는 기본 폴더
	- 반드시 C 드라이브에 있을 필요는 없으며, 다른 드라이브나 폴더에도 위치할 수 있음
	- 시스템 환경변수 (Environment Variables) `%windir%`를 통해 Windows 디렉터리 위치를 참조

> [!INFO]
> ### Environment Variables
> - 환경변수는 운영체제 환경에 대한 정보를 저장
> - 이 정보에는 운영체제 경로, 운영체제에서 사용하는 프로세서 수, 임시 폴더 위치 등의 세부사항이 포함됨

2. System32 폴더의 중요성
	- Windows 폴더 내의 여러 하위 폴더 중 하나
	- 운영체제에 중요한 핵심 파일들이 저장되어 있음
	- 매우 주의 깊게 다뤄야 하는 폴더
	- ⚠️ 주의사항 ⚠️
		- System32 폴더 내의 파일이나 폴더를 실수로 삭제하면 Windows 운영체제가 작동하지 않을 수 있음
		- 극도의 주의가 필요함
	- 참고사항
		- Windows Fundamentals 시리즈에서 다룰 많은 도구들이 System32 폴더에 위치하고 있음
		- 이 폴더는 Windows 시스템의 핵심이므로, 시스템 관리나 문제 해결을 위해 접근할 때는 반드시 백업을 하고 신중하게 작업해야 함

---

## 3. User Accounts, Profiles, and Permissions

1. 로컬 사용자 및 그룹 관리 접근 방법
	- 시작 메뉴를 우클릭하고 실행 (`run`)을 클릭한 후, `lusrmgr.msc`를 입력
	![[Pasted image 20250706204555.png]]
	![[Pasted image 20250706204628.png]]

2. `lusrmgr`구성 요소
	- `lusrmgr`을 열면 두 개의 폴더를 볼 수 있음
		- 사용자 (Users)
		- 그룹 (Groups)

	- 그룹 관리 기능
		![[Pasted image 20250706204808.png]]
		- 그룹을 클릭하면 모든 로컬 그룹의 이름과 각 그룹에 대한 간단한 설명을 볼 수 있음
		- 각 그룹에는 권한이 설정되어 있으며, 관리자가 사용자를 그룹에 할당하거나 추가함
		- 사용자가 그룹에 할당되면 해당 그룹의 권한을 상속받게 됨
		- 한 사용자는 여러 그룹에 동시에 할당될 수 있음






















