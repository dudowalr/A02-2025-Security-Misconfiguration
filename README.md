🛡️ 웹 보안 진단 자동화 스캐너 (Security Scanner)
이 프로젝트는 OWASP Top 10 2025의 주요 항목 중 하나인 'A02: 보안 설정 미흡(Security Misconfiguration)'을 탐지하기 위한 보안 진단 도구입니다. 웹 서버의 HTTP 응답 헤더를 분석하여 발생 가능한 공격 벡터를 식별하고, 위험도별 해결 방안을 제시합니다.

1. 핵심 진단 지표 (10대 항목)

현재 소스코드는 총 10가지 보안 지표를 정밀 검사합니다.

분류	진단 항목	위험도	주요 방어 공격
서버 설정	
Content-Security-Policy	/ 🔴 High	 / XSS(크로스 사이트 스크립팅), 데이터 주입
X-Frame-Options	/ 🟡 Medium	 / 클릭재킹(Clickjacking)
Server Banner	/ 🔵 Low	/ 서버 정보 노출 및 알려진 취약점 공격
X-Content-Type-Options	/ 🔵 Low	/ MIME Sniffing (악성 스크립트 실행)
Strict-Transport-Security	/ 🔵 Low	/ 중간자 공격(MitM), HTTP 강제 전환
세션/사용자	Cookie (Secure, HttpOnly)	/ 🔴 High	/ 세션 하이재킹, 쿠키 탈취
Cookie (SameSite)	/ 🟡 Medium/ 	CSRF (사이트 간 요청 위조)
Cross-Origin-Opener-Policy	/ 🟡 Medium	/ 프로세스 격리 기반 사이드 채널 공격
Permissions-Policy	/ 🔵 Low	/ 하드웨어(카메라 등) 무단 접근
Referrer-Policy	/ 🔵 Low	/ 이전 페이지 경로 정보 유출

2. 소스코드 주요 기능 및 로직
🏗️ 객체 지향적 설계 (ExternalScanner 클래스)
초기화 (__init__): 진단 대상 URL을 입력받아 requests 라이브러리로 서버에 접속하고, 실시간 응답 헤더를 수집합니다.
작업 관리 (run_scan): 개별 진단 모듈(함수)들을 리스트 형태로 관리하여, 한 번의 실행으로 모든 항목을 순차적으로 점검하는 자동화 파이프라인을 갖추고 있습니다.

📊 위험도 기반 리포팅
각 진단 결과는 JSON 형식으로 출력되어 가독성이 높습니다.
risk_level: 발견된 취약점의 치명도에 따라 High, Medium, Low 등급을 부여하여 관리자가 우선순위를 정해 조치할 수 있게 돕습니다.
recommendation: 단순히 취약점을 찾는 데 그치지 않고, Apache 등 서버 설정 파일에서 즉시 적용 가능한 구체적인 해결 코드를 제공합니다.

3. 기술 스택 및 환경
언어: Python 3.11+
라이브러리: requests (HTTP 통신), json (결과 구조화)
환경 관리: venv(가상환경)를 통한 독립적인 패키지 관리 및 .gitignore를 활용한 깔끔한 코드 관리.

4. 기대 효과
이 소스코드를 적용하면 다음과 같은 효과를 얻을 수 있습니다.
보안 가시성 확보: 서버의 설정 상태를 수치와 등급으로 한눈에 파악할 수 있습니다.
협업 효율성 증대: 진단 결과와 조치 방법이 명확하여, 보안 담당자(재영)와 인프라 담당자(동준) 간의 의사소통 비용이 줄어듭니다.
지속적인 모니터링: 자동화된 스크립트이므로 주기적으로 실행하여 보안 구멍이 다시 생기지 않는지 감시할 수 있습니다.
