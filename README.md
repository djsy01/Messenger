# 📱 멀티 플랫폼 메신저 프로젝트

> **모든 플랫폼에서 가볍고 빠른 대화**  
> 전화번호 기반 아이디, 다국어 자동 번역, 사용자 짤/스티커 기능 지원

---

## ✨ 프로젝트 개요
- **지원 플랫폼**: iOS, Android, Windows, macOS, Linux, Web(PWA)  
- **주요 컨셉**:  
  - **전화번호 = 사용자 ID**  
  - **멀티 디바이스 동기화**  
  - **자동 번역** (Naver Papago / Google Translate API)  
  - **사용자 짤/스티커 등록 가능**  
  - **피드 없음** → 오직 **메시지, 통화, 파일 공유**에 집중  

---

## 🧑‍💻 팀 구성
| 이름 | 역활 | GITHUB | 비고 |
|-----|-----|-----|-----|
| 김승훈 | 팀장, Front-End | [@Siya](https://github.com/SIya45) |  |
| 엄인호 | Back-End | [@djsy01](https://github.com/djsy01) |  |
| 정연수 |  |  |  |
| 한정인 | DataBase |  |  |

---

## 🎯 핵심 기능

### 1. 사용자 계정
- 전화번호 기반 가입/로그인 (OTP 인증)  
- 기기별 로그인 관리 (멀티 디바이스 지원)  
- 연락처 기반 친구 자동 매칭(해시 비교)  

### 2. 채팅
- 1:1 / 그룹 채팅 (공지, 멘션, 읽음 수 표시)  
- 메시지 전송 (텍스트, 사진, 동영상, 파일, 음성 메세지)  
- **자동 번역** 기능 (메시지별/방별 설정 가능, 원문 ↔ 번역문 토글)  
- 메시지 수정/삭제, 답장, 북마크  

### 3. 통화
- 1:1 음성/영상 통화 (WebRTC)  
- 그룹 통화 (V2 예정)  
- 화면 공유 (데스크톱/웹 지원)  

### 4. 커스텀 짤/스티커
- 개인 짤/스티커 업로드 (PNG, GIF, WebP)  
- 내 보관함 관리 및 대화에서 바로 사용  
- 그룹 단위 공유 세트 (V1 예정)  

### 5. 보안 & 프라이버시
- 모든 통신 **TLS 암호화**  
- 기기별 세션 관리 및 원격 로그아웃  
- (V2) 1:1 대화 **종단간 암호화(E2EE)** 지원 예정  
- 번역 API 호출 시 개인정보 마스킹  

---

## 🛠️ 기술 스택

### 프론트엔드
- **React Native (Expo)** → iOS / Android  
- **React + TypeScript (PWA)** → Web  
- **Electron + React** → Windows / macOS / Linux  
- **공용 모듈**: Turborepo 모노레포, Recoil/Zustand, TanStack Query  

### 백엔드
- **Node.js (NestJS / Express)**  
- **WebSocket(Socket.IO)** → 실시간 메시징  
- **MySQL** → 사용자/메시지 메타데이터 저장  
- **Redis** → 세션/프레즌스/캐시/실시간 이벤트  
- **S3 호환 스토리지** → 이미지/동영상/스티커 저장  
- **Meilisearch** → 메시지 검색  
- **WebRTC + Coturn** → 음성/영상 통화  

### 외부 API
- **Naver Papago / Google Translate API** → 자동 번역  
- **FCM / APNs / Web Push** → 푸시 알림  

---

## 🗄️ 데이터 모델 (요약)

```sql
users (
  user_id BIGINT PK,
  phone_e164 VARCHAR(20) UNIQUE,
  display_name VARCHAR(50),
  avatar_url TEXT,
  preferred_locale VARCHAR(10),
  created_at DATETIME
)

devices (
  device_id BIGINT PK,
  user_id BIGINT FK,
  platform ENUM('ios','android','web','windows','mac','linux'),
  push_token TEXT,
  last_active_at DATETIME
)

conversations (
  conv_id BIGINT PK,
  type ENUM('direct','group'),
  title VARCHAR(100),
  owner_id BIGINT,
  created_at DATETIME
)

messages (
  msg_id BIGINT PK,
  conv_id BIGINT FK,
  sender_id BIGINT,
  kind ENUM('text','image','file','sticker'),
  body JSON,
  created_at DATETIME,
  edited_at DATETIME,
  deleted_at DATETIME
)

stickers (
  sticker_id BIGINT PK,
  owner_user_id BIGINT,
  name VARCHAR(50),
  storage_key TEXT,
  animated BOOL,
  scope ENUM('private','group')
)
```

---

## 🚀 개발 로드맵

### MVP (8~10주)
- 전화번호 기반 가입/OTP 인증
- 1:1 채팅 (텍스트/이미지/파일)
- 실시간 메시징, 읽음/타이핑 표시
- 자동 번역 (on-demand, Papago/Google API 연동)
- 사용자 짤 업로드/전송

### V1
- 그룹 채팅, 초대 링크, 공지/멘션
- 멀티 디바이스 동기화 완성
- 사용자 짤/스티커 세트 공유
- 음성/영상 통화 (1:1)

### V2
- E2EE (1:1 → 그룹 순차 적용)
- 그룹 통화, 화면 공유
- 관리자 모드 (신고/차단 처리)

---

📂 프로젝트 구조
```
apps/
  mobile/     # React Native (Expo)
  web/        # React + TS (PWA)
  desktop/    # Electron + React
  server/     # Node.js (NestJS)
packages/
  ui/         # 공용 UI 컴포넌트
  core/       # 타입, 번역 어댑터, 유틸
  proto/      # WebSocket 이벤트/DTO
```

---