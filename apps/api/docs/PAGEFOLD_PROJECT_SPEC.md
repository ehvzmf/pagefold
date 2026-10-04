# Pagefold — 프로젝트 스펙 문서
> 이 문서는 AI 에이전트(Claude Code 등)에게 프로젝트 컨텍스트를 전달하기 위한 문서입니다.
> 대화 기반으로 결정된 모든 사항이 담겨 있으며, 개발 가이드로 활용하세요.

---

## 1. 서비스 개요

### 브랜드
- **서비스명**: Pagefold
- **줄임말**: 페폴 (한국어), pafol (영문 핸들/해시태그)
- **도메인 후보**: pagefold.app / pagefold.io / pafol.app
- **슬로건**: "같이 읽으면 더 재밌잖아요."
- **영문 슬로건**: "The page you folded is where the story begins."

### 서비스 정의
북클럽/독서 동호회 타겟의 **공유 독서 플랫폼**.
이북(EPUB)을 읽으면서 하이라이트·메모를 남기고, 같은 책을 읽는 사람들과 실시간으로 나눌 수 있는 서비스.

### 타겟 유저
- 주 타겟: 2030 독서 동호회 / 북클럽 멤버
- 분위기: 세련되고 미니멀, 가볍고 영한 MZ 커뮤니티 감성

### 콘텐츠 수급 정책
- **공개 도메인 도서만** 제공 (저작권 이슈 방지)
- 소스: Project Gutenberg, Standard Ebooks
- 관리자가 EPUB 파일 직접 업로드 (유저 업로드 없음)

---

## 2. 핵심 기능 (Core Value)

**공유 독서 (Shared Reading)** 가 이 서비스의 핵심 기능이자 존재 이유.
- 문장 단위 하이라이트 & 메모 공유
- 같은 책을 읽는 다른 유저의 메모를 실시간으로 확인
- 몇 명이 이 문장을 하이라이트했는지 표시
- 내 메모만 보기 / 전체 보기 토글

---

## 3. MVP 범위

### MVP에 포함 (지금 만들 것)
1. **인증** — 이메일/비밀번호 회원가입 & 로그인 (소셜 로그인은 MVP 이후)
2. **이북 뷰어** — EPUB 렌더링, 페이지 넘기기, 글씨 크기 조절
3. **하이라이트 & 메모 (개인)** — 문장 선택 후 하이라이트, 메모 작성/수정/삭제, 내 하이라이트 모아보기
4. **공유 독서 (실시간)** — 같은 책을 읽는 다른 유저의 하이라이트·메모 실시간 표시

### MVP 이후 (추후 확장)
- 커뮤니티 (북클럽, 게시판)
- 서평 & 별점
- 책 검색 & 탐색
- 소셜 로그인 (Google)
- 모바일 앱 (React Native)

---

## 4. 화면 목록 (Wireframe 기준)

| # | 화면 | URL | 설명 |
|---|---|---|---|
| 1 | 랜딩 페이지 | `/` | 서비스 소개, 베타 신청 |
| 2 | 로그인 | `/login` | 이메일/비밀번호 로그인 |
| 3 | 회원가입 | `/signup` | 닉네임, 이메일, 비밀번호 |
| 4 | 홈 (서재) | `/home` | 읽는 중인 책, 탐색, 함께 읽는 사람 |
| 5 | 책 상세 | `/books/:id` | 책 정보, 읽기 시작, 공유독자 현황 |
| 6 | 이북 뷰어 | `/reader/:bookId` | 핵심 화면 — 뷰어 + 하이라이트 + 공유 패널 |
| 7 | 내 하이라이트 | `/highlights` | 전체 책에서 남긴 하이라이트 & 메모 목록 |

### 이북 뷰어 UX 상세
- **좌측**: 현재 읽는 책 네비게이션 사이드바 (다크 배경)
- **중앙**: 본문 — 텍스트 선택 시 컨텍스트 메뉴 (하이라이트 / 메모 추가 / 복사)
- **우측**: 공유 메모 패널 — 해당 문장의 다른 유저 메모 실시간 표시 + 내 메모 입력
- **상단**: 책 제목, 챕터/페이지, 설정 (글씨 크기), 공유독자 인디케이터
- **하단**: 진행률 바, 이전/다음 페이지 버튼

---

## 5. 기술 스택 (확정)

### 전체 구조
```
pagefold/                    # 모노레포 (GitHub 레포 하나)
├── apps/
│   ├── web/                 # Next.js 프론트엔드
│   └── api/                 # Spring Boot 백엔드
├── docs/                    # 설계 문서, 와이어프레임
├── .gitignore
└── README.md
```

### 프론트엔드 (`apps/web/`)
| 항목 | 기술 | 비고 |
|---|---|---|
| 프레임워크 | Next.js 15 (App Router) | SSR/SSG 혼용 |
| 언어 | TypeScript | |
| 스타일 | Tailwind CSS | 브랜드 토큰 변수화 |
| 패키지 매니저 | pnpm | |
| 이북 렌더링 | epub.js | EPUB 파싱/렌더링 |
| 배포 | Vercel | Next.js 최적화 |

### 백엔드 (`apps/api/`)
| 항목 | 기술 | 비고 |
|---|---|---|
| 프레임워크 | Spring Boot 3.5.x | Java 21 |
| 언어 | Java 21 | |
| 빌드 | Gradle | |
| ORM | Spring Data JPA (Hibernate) | |
| 인증 | Spring Security + JWT | |
| IDE | IntelliJ IDEA Community | |
| 배포 | Railway | |

### 데이터베이스
| 항목 | 기술 | 비고 |
|---|---|---|
| DB | PostgreSQL 16 | |
| 호스팅 | Neon | 서버리스 PostgreSQL, 프리티어 |
| 실시간 | Supabase Realtime | 공유독서 WebSocket 전용 |

### 파일 스토리지
| 항목 | 기술 | 비고 |
|---|---|---|
| EPUB 파일 | Cloudflare R2 | S3 호환 SDK, egress 무료 |

### Spring Boot 초기 Dependencies
- Spring Web
- Spring Data JPA
- PostgreSQL Driver
- Spring Security
- Lombok
- Validation

---

## 6. DB 스키마 설계 (진행 중)

### 설계 원칙
- 네이밍 컨벤션: **snake_case**
- 모든 테이블에 `id`, `created_at`, `updated_at` 기본 포함
- Soft Delete: `deleted_at` (NULL이면 살아있음, 값 있으면 삭제됨)
- 비밀번호: 평문 저장 금지, `password_hash` 컬럼에 해시값 저장

### 테이블 목록
1. `users` — 회원 정보
2. `books` — 책 정보 (공개 도메인)
3. `user_books` — 유저별 독서 기록 & 진행률 (users N:M books 중간 테이블)
4. `highlights` — 하이라이트 (책의 어느 위치, 어떤 텍스트)
5. `notes` — 메모 (하이라이트에 종속)

### users 테이블 (확정)
```sql
users
- id            BIGINT PK (Auto Increment)
- email         VARCHAR UNIQUE NOT NULL
- password_hash VARCHAR NOT NULL
- user_name     VARCHAR NOT NULL        -- 닉네임 (화면 표시용)
- profile_pic   VARCHAR                 -- 프로필 이미지 URL
- created_at    TIMESTAMP NOT NULL
- updated_at    TIMESTAMP NOT NULL
- deleted_at    TIMESTAMP               -- Soft Delete
```

### 나머지 테이블 (설계 진행 중)
> books, user_books, highlights, notes 테이블은 스키마 설계 세션에서 계속 작업 중.
> 아래 관계 기준으로 설계 예정:
> - users 1:N user_books N:1 books (유저가 여러 책, 책을 여러 유저가 읽음)
> - highlights는 user_books 또는 books + user 복합 참조
> - notes는 highlights에 종속 (highlight_id FK)
> - 메모 없이 하이라이트만 가능, 하이라이트 없이 메모 불가

---

## 7. 환경 설정 (로컬 개발)

### 개발 환경
- OS: macOS
- Java: OpenJDK 21 (Homebrew)
- Node.js: v20.x 이상
- pnpm: v9.x 이상
- IDE: IntelliJ IDEA Community (백엔드), VS Code (프론트)

### Java 환경변수 설정 (macOS)
```bash
echo 'export PATH="/opt/homebrew/opt/openjdk@21/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Spring Boot 환경변수 (IntelliJ Edit Configurations)
IntelliJ Run Configuration > Environment Variables에 직접 입력:
```
DB_URL=jdbc:postgresql://[neon-host]/[dbname]?sslmode=require
DB_USERNAME=[neon-username]
DB_PASSWORD=[neon-password]
```

### application.properties
```properties
# DB 연결
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

### .gitignore 주요 항목
```
# Java
*.class
*.jar
*.war
build/
.gradle/
out/

# Node
node_modules/
.next/
.env
.env.local

# IDE
.idea/
*.iml
.vscode/
.DS_Store
```

---

## 8. 브랜드 가이드 요약

### 컬러 팔레트
| 이름 | HEX | 용도 |
|---|---|---|
| Blue 950 | `#0c1b3a` | 다크 배경, 헤더 |
| Blue 700 | `#1a4fd6` | 강조 텍스트 |
| Blue 500 | `#3b72f5` | 메인 액션, CTA ★ |
| Blue 400 | `#6292ff` | 아이콘, 포인트 |
| Blue 100 | `#dce8ff` | 하이라이트 배경 |
| Blue 50 | `#f0f5ff` | 카드, 섹션 배경 |
| White | `#ffffff` | 기본 배경 |
| Gray 900 | `#111318` | 기본 텍스트 |

### 타이포그래피
| 용도 | 폰트 | 굵기 |
|---|---|---|
| 영문 헤드 | Plus Jakarta Sans | ExtraBold 800 |
| 한글 본문 | Noto Sans KR | Regular 400 |
| 레이블/메타 | DM Mono | Regular 400 |

### 로고
- 워드마크: "Pagefold" + 파란 점(·) 시그니처
- 아이콘: 파란 배경 + "P" 레터폼

---

## 9. 개발 가이드 & 컨벤션

### 커밋 메시지 컨벤션
```
feat: 새 기능
fix: 버그 수정
chore: 설정, 빌드, 패키지
docs: 문서
refactor: 리팩토링
style: 코드 스타일
test: 테스트
```

### 개발 순서 (권장)
1. **DB 스키마 설계 완성** — 나머지 테이블 (books, user_books, highlights, notes)
2. **JPA Entity 클래스 작성** — 각 테이블에 대응하는 Java 클래스
3. **Repository 레이어** — Spring Data JPA Repository 인터페이스
4. **Service 레이어** — 비즈니스 로직
5. **Controller 레이어** — REST API 엔드포인트
6. **Spring Security + JWT 인증** — 로그인/회원가입 API
7. **Postman으로 API 테스트**
8. **Next.js 프론트 세팅** — apps/web/ 초기화
9. **프론트-백엔드 연결**
10. **epub.js 이북 뷰어 구현**
11. **Supabase Realtime 공유독서 기능**

### 학습 방향
- 개발자 본인이 직접 코드 작성 후 에이전트에게 피드백 요청하는 방식 선호
- 에이전트가 답을 바로 주기보다 → 질문 던지기 → 스스로 결정 → 피드백 순서로 진행
- 백엔드 경험을 쌓는 것이 이 프로젝트의 주요 학습 목표 중 하나

### 현재 개발자 스킬셋
- **주력**: React, Vue.js (프론트엔드 1년차)
- **백엔드**: Java 사용 중 (SI 회사), Spring Boot 학습 중
- **DB**: SQL 경험 있음, JPA 학습 중
- **인프라**: Vercel/Netlify 경험 있음

---

## 10. 현재 진행 상태

### 완료
- [x] 서비스 브랜딩 (로고, 컬러, 타이포, 슬로건)
- [x] 랜딩 페이지 목업
- [x] 와이어프레임 (6개 화면)
- [x] 기술 스택 확정
- [x] 요구사항 & MVP 정의
- [x] 모노레포 구조 생성 & GitHub 연결
- [x] Spring Boot 프로젝트 초기화 (apps/api/)
- [x] Neon PostgreSQL 연결 완료
- [x] DB 스키마 설계 시작 — users 테이블 확정

### 진행 중
- [ ] DB 스키마 설계 — books, user_books, highlights, notes 테이블

### 다음 작업
- [ ] 나머지 테이블 스키마 확정
- [ ] JPA Entity 클래스 작성
- [ ] Repository / Service / Controller 레이어 구성
- [ ] 인증 API (회원가입, 로그인)

---

*Last updated: 2026-10-04*
*이 문서는 대화 기반으로 작성되었으며, 개발 진행에 따라 계속 업데이트 필요*
