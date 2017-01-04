# A1. DB 다운로드 및 업로드 명령 수신시 응답형태.
## DB 다운로드
- Verification 모듈이 sqlite의 db파일을 호스트로 다음(#별첨)과 같이 보낸다.
## DB 업로드
- 호스트가 sqlite의 db파일을 모듈로 다음(#별첨)과 같이 보낸다.


# A2. 펌웨어 제공 방법에 대해
## 펌웨어 버젼 읽기 응답 프로토콜의 VALUE 
    - module type(1 byte)   : 0x00 = face, 0x01 = finger
    - major version(1 byte) : 0x00 - 0xFF
    - minor version(1 byte) : 0x00 - 0xFF
    - patch version(1 byte) : 0x00 - 0xFF
## 펌웨어 업로드
- 호스트가 펌웨어를 압축파일 형태로 다음(#별첨)과 같이 모듈로 보낸다.


# A3. 환경정보 읽기 명령 수신시 응답 형태.
## 환경정보 읽기
- 모듈이 환경정보파일을 호스트로 다음(#별첨)과 같이 보낸다.
 
## 환경정보 저장
- 호스트가 환경정보파일을 모듈로 다음(#별첨)과 같이 보낸다.


# A4. 실패코드 리스트업
- 각 모듈이 자체 검출가능한 오동작 상황을 코드로 제공
#
	- 0x01				: Duplicated
	- 0x02				:
	- 0x03				:
	- 0x04				:
	- 0x05				:
	- 0x06				: 
	


#별첨. 파일 송/수신시 프로토콜의 VALUE(n byte)필드의 내용 
    - file id(8 byte)           : 보내는 파일의 고유번호
    - file type(1 byte)         : 0x00 = RAW, 0x01 = tar.gz, 0x02 = tar.bz2
    - total file count(2 byte)  : 총 파일 개수 (최소값 0x0001)
    - current file count(2 byte): 현재 파일 번호 (최소값 0x0001)
    - file data(n byte)         : binary
    - md5 hash(16 byte)         : 파일 무결성 검사용 해쉬


# 0x00. 명령취소
## 명령취소 요청
- 호스트가 명령취소 요청 패킷을 #별첨2 형태로 모듈로 보낸다.
#
	- msg type 			: 0x01
	- return code 		: 0x00
	- type				: 0x00
	- length 			: 0x0000
## 명령취소 응답
- 모듈이 명령취소 응답 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x00
	- type				: 0x00
	- length 			: 0x0000
# 0x01. 등록
## 등록 요청
- 호스트가 Enrollment 시작 패킷을 #별첨2 형태로 모듈로 보낸다.
#
	- msg type 			: 0x01
	- return code 		: 0x00
	- type				: 0x01
	- length 			: 0x0000
	
## 등록 응답(성공)
- 모듈이 촬영한 이미지 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x02
	- type				: 0x01
	- length 			: 0x0004 + n byte
	- value				: id(4 byte) + 촬영이미지(n byte)
- 모듈이 변환한 템플릿 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x00
	- type				: 0x01
	- length 			: 0x0005 + n byte
	- value				: id(4 byte) + 권한(1 byte) + 템플릿(n byte)
## 등록 응답(실패)
- 모듈이 실패 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x01
	- type				: 0x01
	- length 			: 0x0001
	- value				: 실패코드(1 byte)

# 0x02. 인증
## 인증 요청
- 호스트가 Verification 시작 패킷을 #별첨2 형태로 모듈로 보낸다.
#
	- msg type 			: 0x01
	- return code 		: 0x00
	- type				: 0x02
	- length 			: 0x0000
## 인증 응답(성공)
- 모듈이 인증한 결과 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x00
	- type				: 0x02
	- length 			: 0x0005
	- value				: id(4 byte) + 권한(1 byte)
## 인증 응답(실패)
- 모듈이 실패 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x01
	- type				: 0x02
	- length 			: 0x0001
	- value				: 실패코드(1 byte)

# 0x03. 계정추가
## 계정추가 요청
- 호스트가 계정추가 패킷을 #별첨2 형태로 모듈로 보낸다.
#
	- msg type 			: 0x01
	- return code 		: 0x00
	- type				: 0x03
	- length 			: 0x0005 + n byte
	- value				: id(4 byte) + 권한(1 byte) + 템플릿(n byte)
## 계정추가 응답(성공)
- 모듈이 계정추가 결과 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x00
	- type				: 0x03
	- length 			: 0x0000
	
## 계정추가 응답(실패)
- 모듈이 실패 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x01
	- type				: 0x03
	- length 			: 0x0001
	- value				: 실패코드(1 byte)
# 0x04. 계정삭제
## 계정삭제 요청
- 호스트가 계정삭제 패킷을 #별첨2 형태로 모듈로 보낸다.
#
	- msg type 			: 0x01
	- return code 		: 0x00
	- type				: 0x04
	- length 			: 0x0004
	- value				: id(4 byte)
## 계정삭제 응답(성공)
- 모듈이 계정삭제 결과 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x00
	- type				: 0x04
	- length 			: 0x0004
	- value				: id(4 byte)
	
## 계정삭제 응답(실패)
- 모듈이 실패 패킷을 #별첨2 형태로 호스트로 보낸다.
#
	- msg type 			: 0x02
	- return code 		: 0x01
	- type				: 0x04
	- length 			: 0x0001
	- value				: 실패코드(1 byte)
#별첨2. BACS Module Protocol 
    - STX(1 byte)           	: 0x02 = 패킷의 시작
	- TOTAL LENGTH(2 byte)     	: STX, CHECK, ETX 제외한 패킷 전체 길이
    - MSG TYPE(1 byte)  		: 0x01 = 호스트, 0x02 = 모듈
    - RETURN CODE(1 byte)		: 0x00 = 성공, 0x01 = 실패, 0x02 = 촬영이미지, 0x03 = 가이드
    - TYPE(1 byte)         		: 명령 코드(#별첨3)
    - LENGTH(2 byte)         	: VALUE 길이
    - VALUE(n byte)         	: Return code, Type에 따른 가변 데이터
    - CHECK(1 byte)				: STX부터 VALUE까지 XOR값
    - EXT(1 byte)				: 0x03 = 패킷의 끝

#별첨3. 명령 코드(TYPE) 필드의 내용
    - 0x00          	: 명령 취소
    - 0x01           	: 등록
    - 0x02           	: 인증
    - 0x03           	: 계정 추가
    - 0x04           	: 계정 삭제
    - 0x05           	: DB 다운로드
    - 0x06           	: DB 업로드
    - 0x07           	: 펌웨어 버전 읽기
    - 0x08           	: 펌웨어 업로드
    - 0x09           	: 환경정보 읽기
    - 0x0A           	: 환경정보 저장
 