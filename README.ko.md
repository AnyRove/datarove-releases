<p align="center"><img src="docs/images/icon.svg" width="96" alt="DataRove"></p>

<h1 align="center">DataRove</h1>

<p align="center"><b>모든 데이터베이스를 컴퓨터에서 관리하고, 휴대폰으로 언제든 확인하세요. 종단 간 암호화로.</b></p>

<p align="center">
<a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a> · <a href="README.zh-TW.md">繁體中文</a> · <a href="README.ja.md">日本語</a> · <b>한국어</b> · <a href="README.es.md">Español</a> · <a href="README.fr.md">Français</a> · <a href="README.de.md">Deutsch</a> · <a href="README.pt-BR.md">Português</a> · <a href="README.ru.md">Русский</a>
</p>

---

DataRove는 예약 백업과 동기화를 기본으로 갖춘 데스크톱 데이터베이스 클라이언트입니다. 컴퓨터에서(앱 또는 백그라운드 서비스로) 실행되며, 휴대폰은 로컬 네트워크로 직접 연결하거나 릴레이를 거쳐 연결합니다. 연결은 종단 간 암호화되며, 데이터베이스 비밀번호는 컴퓨터 밖으로 나가지 않습니다.

**지원 데이터베이스:** MySQL · MariaDB · PostgreSQL · SQLite · SQL Server · MongoDB · Redis (SSH 터널, TLS 지원)

> **상태:** 1.0.0입니다. 설치 파일은 [Releases](../../releases) 페이지에서 받을 수 있고, 설치한 뒤에는 앱이 스스로 업데이트됩니다. 새 버전 알림을 받으려면 이 저장소를 Watch 하세요. 변경 내역은 [CHANGELOG](CHANGELOG.md)에 있습니다.

## 4분 기능 소개 영상

<p align="center"><a href="docs/video/DataRove-promo.mp4"><img src="docs/video/DataRove-promo-poster.jpg" width="720" alt="DataRove 기능 소개 영상(영어 내레이션, 영어·중국어 자막)"></a></p>
<p align="center"><a href="docs/video/DataRove-promo.mp4">영상 재생</a>(MP4, 1080p, 영어 내레이션, 영어·중국어 자막) · 자막 파일: <a href="docs/video/DataRove-promo.en.srt">English</a>, <a href="docs/video/DataRove-promo.zh-CN.srt">简体中文</a></p>

![개요](docs/images/overview.png)

## 기능

### 데이터 탐색과 편집
- 가상 스크롤 그리드로 필터, 정렬, 페이지 이동을 지원합니다. 셀을 그 자리에서 편집하거나, 행 전체를 폼으로 열어 열 타입(불리언, 열거형, 날짜, JSON, NULL / 기본값)에 맞는 입력란으로 편집할 수 있습니다.
- 외래 키 열에서는 참조하는 테이블을 검색해 행을 고를 수 있습니다.
- 저장하기 전에 실행될 SQL을 미리 볼 수 있습니다. 모든 변경은 하나의 트랜잭션으로 커밋되며, 오류가 나면 함께 롤백됩니다. 삭제와 운영 환경 연결은 먼저 확인을 받습니다.
- CSV / TSV / JSON / SQL / Markdown으로 복사, 파일로 내보내기, CSV / JSON 가져오기.

![데이터 그리드](docs/images/data.png)

### 쿼리 작성
- 문맥에 맞는 SQL 자동 완성: `FROM` / `JOIN` 뒤에는 테이블, `별칭.` 뒤에는 열, `INSERT` 열 목록, 시그니처가 있는 함수, 구문 템플릿.
- 테이블이나 열에 마우스를 올리면 구조, 타입, 주석을 보여 줍니다. 잘못된 테이블 이름이나 `별칭.열`, `WHERE` 없는 `DELETE` / `UPDATE`는 실행하기 전에 밑줄로 표시됩니다.
- 기본 키를 포함한 단일 테이블 `SELECT` 결과는 바로 편집해 저장할 수 있습니다.
- MongoDB 셸 문법(`db.users.find({...})`)과 Redis 명령줄도 자동 완성과 함께 지원합니다.

![쿼리 편집기](docs/images/query.png)

### 스키마 파악
- 데이터베이스 → 테이블 / 뷰 / 컬렉션 → 열, 인덱스, 외래 키, 트리거, DDL, 행 수와 크기.
- ER 다이어그램, 두 데이터베이스의 스키마 비교와 마이그레이션 SQL 생성, Redis 키 브라우저, 서버 모니터(연결 수, QPS, 느린 쿼리).

![ER 다이어그램](docs/images/er.png)

### 가져오기와 내보내기
- 테이블 전체나 필터 결과를 CSV, TSV, JSON, JSON Lines, SQL `INSERT`, Markdown으로 내보내고, CSV / TSV / JSON / JSON Lines를 가져옵니다(UTF-8과 GBK 자동 인식).
- 대용량 전송은 컴퓨터의 백그라운드에서 진행되며 진행률 표시와 취소를 지원합니다. 작은 조각으로 보내므로 연결이 끊겨도 이어서 전송합니다.
- 가져오기는 하나의 트랜잭션으로 실행됩니다. 한 행이라도 실패하면 아무것도 쓰지 않고 실패한 행을 알려 줍니다.

### 믿을 수 있는 백업
- 네이티브 도구(mysqldump / pg_dump / mongodump) 또는 내장 내보내기로 파일에 백업합니다. gzip 압축, AES-256-GCM 암호화, 최근 N개 또는 N일 보관.
- 새 백업 파일은 쓰자마자 다시 읽어 검증(체크섬, 복호화, 파싱)하므로, 손상된 파일은 만들어진 그날 밤에 발견됩니다.
- 다른 데이터베이스로 동기화: 전체 교체, 비운 뒤 적재, 추가, 기본 키 기준 upsert, 워터마크 열(`updated_at`, 자동 증가 id 등) 기준 **증분** 동기화.
- 일정: N분마다, 매일, 매주, 매월 또는 cron. 놓친 실행은 시작할 때 따라잡습니다. 데스크톱 알림과 Webhook. 어떤 백업이든 원하는 연결로 클릭 한 번에 복원합니다.
- 데이터베이스 종류가 달라도 동기화할 수 있습니다(예: PostgreSQL → MySQL). 각 테이블은 완전히 쓰이거나 전혀 바뀌지 않으며, 커밋 전에 행 수를 대조합니다. 일시적인 오류는 자동으로 재시도하고, 그래도 실패하면 **실패한 부분만 다시 시도**할 수 있습니다. 없는 도구(pg_dump 등)는 앱에서 클릭 한 번으로 설치합니다.

![백업](docs/images/backups.png)

### 실수 되돌리기
- '운영'으로 표시한 연결에서는 `UPDATE`, `DELETE`, `DROP`, `TRUNCATE`, 그리드 편집, 덮어쓰기 가져오기, 동기화, 복원 전에 DataRove가 영향받는 테이블을 저장합니다.
- 나중에 클릭 한 번으로 되돌리기: 차이가 있는 행만 하나의 트랜잭션으로 되돌립니다. 스냅샷은 컴퓨터에 암호화되어 저장됩니다.

### 휴대폰도 안전하게
- 휴대폰에서도 같은 화면을 씁니다. QR 코드를 스캔해 페어링하며, 계정이 필요 없습니다.
- 휴대폰마다 권한(읽기 전용, 읽기·쓰기, 관리)을 정하고 언제든 바꾸거나 해제할 수 있습니다. 임시 권한은 기한이 되면 스스로 만료되고, 활동 기록에서 각 휴대폰이 한 일을 볼 수 있습니다.
- 같은 Wi-Fi에서는 컴퓨터에 직접 연결하고, 밖에서는 암호문만 전달하는 릴레이를 거칩니다.

<p align="center"><img src="docs/images/phone-data.png" width="300" alt="휴대폰의 DataRove"></p>

### 데스크톱
트레이에 상주하고 로그인 시 시작되며, 백그라운드 서비스(launchd / systemd / Windows 작업 스케줄러)로 스스로 설치할 수 있어 창을 닫아도 예약 백업이 계속 실행됩니다. 테마 10종, 라이트·다크, 인터페이스 언어 15개.

## 보안 요약

- 저장된 비밀번호는 컴퓨터에서 로컬 마스터 키로 암호화됩니다. 화면과 페어링된 휴대폰에는 `••••`만 보입니다.
- QR 코드 페어링은 256비트 비밀 값을 쓰고, 직접 입력하는 짧은 코드는 scrypt로 강화합니다. 양쪽이 서로의 키를 검증하므로 릴레이가 자기 기기를 몰래 끼워 넣을 수 없습니다.
- 모든 세션은 X25519 + ChaCha20-Poly1305를 쓰며 매번 새 임시 키를 만듭니다(전방향 안전성). 변조·재전송·순서가 바뀐 프레임은 거부되고, 다운그레이드 시도도 거부됩니다.
- 공용 릴레이는 반드시 `wss://`를 써야 합니다. 릴레이를 직접 운영할 수 있고, 로컬 네트워크에서만 페어링할 수도 있습니다.
- DataRove 밖에 영향을 주는 작업은 컴퓨터에서만 할 수 있습니다: 도구 경로, 파일 탐색, SQLite 파일 접근, 바뀐 주소로 저장된 비밀번호 보내기.

취약점 신고는 [SECURITY.md](SECURITY.md)를 참고하세요.

## 플랫폼

| | |
|---|---|
| macOS | Apple 실리콘, Intel |
| Windows | 10 / 11, x64 |
| Linux | AppImage, deb |
| Android | 앱(컴퓨터의 동반 앱 또는 휴대폰 직접 연결) |
| 휴대폰 | 최신 모바일 브라우저(홈 화면에 추가 가능) |

## 설치

[최신 릴리스](../../releases/latest)에서 사용하는 시스템용 파일을 내려받으세요. 각 릴리스의 `SHA256SUMS.txt`에 SHA-256 체크섬이 있습니다. 설치 파일은 아직 Apple / Microsoft 개발자 인증서로 서명되지 않아 처음 한 번 확인 창이 나타납니다.

- **macOS**: `.dmg`를 열고 DataRove를 응용 프로그램으로 끌어 놓습니다. 처음 열 때 확인할 수 없다는 메시지가 나오면 **시스템 설정 → 개인정보 보호 및 보안**에서 아래로 내려 **그래도 열기**를 클릭합니다. 손상되었다는 메시지가 나오면 터미널에서 `xattr -cr /Applications/DataRove.app`을 실행한 뒤 다시 엽니다.
- **Windows**: `.exe`를 실행합니다. SmartScreen이 나타나면 **추가 정보 → 실행**을 클릭합니다.
- **Linux**: `.AppImage`에 실행 권한을 주고 실행하거나 `.deb`를 설치합니다.
- **Android**: `.apk`를 열고 브라우저나 파일 관리자의 앱 설치를 허용합니다. 앱은 컴퓨터의 DataRove와 페어링해서 사용하거나, 휴대폰 직접 연결 모드로 직접 데이터베이스에 연결할 수 있습니다(처음 한 번 DataRove 계정으로 휴대폰을 활성화).

그런 다음 앱에서 이메일로 로그인하고 컴퓨터를 활성화합니다. DataRove는 무료이며 결제 정보가 필요 없습니다.

## 라이선스, 면책 조항, 버전

- DataRove는 현재 모든 기능을 무료로 사용할 수 있습니다. 결제 정보는 필요 없습니다.
- DataRove는 독점 소프트웨어이며 오픈 소스가 아닙니다. 앱 사용에는 처음 실행할 때 앱에 표시되고 동의하는 최종 사용자 라이선스 계약, 개인정보 고지, 구매 약관이 적용됩니다. [LICENSE.md](LICENSE.md)를 참고하세요(영어판 우선).
- DataRove는 실제 데이터베이스를 변경합니다. 독립적이고 검증된 백업을 유지하고 [면책 조항](DISCLAIMER.md)을 읽어 주세요.
- 버전은 [유의적 버전](https://semver.org/lang/ko/)을 따릅니다. 각 릴리스의 변경 사항은 [CHANGELOG.md](CHANGELOG.md)에 기록하고 `SHA256SUMS.txt`를 함께 제공합니다.

## 피드백

버그 제보와 기능 요청은 [Issues](../../issues)에 남겨 주세요. 이 저장소는 릴리스 노트와 설치 파일을 위한 곳이며, 소스 코드는 공개하지 않습니다.
