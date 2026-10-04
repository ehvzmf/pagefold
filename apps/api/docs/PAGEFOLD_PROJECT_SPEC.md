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

---

## 11. 전체 개발 로드맵

> 혼자 진행하는 토이 프로젝트 기준. 각 Phase는 순서대로 진행하되,
> Phase 3 (프론트)는 Phase 2 백엔드 API가 어느 정도 완성된 후 시작 권장.

---

### Phase 0 — 프로젝트 세팅 ✅ 완료

- [x] 서비스 브랜딩 (로고, 컬러, 타이포, 슬로건)
- [x] 요구사항 & MVP 정의
- [x] 와이어프레임 (6개 화면)
- [x] 기술 스택 확정
- [x] 모노레포 구조 생성 & GitHub 연결
- [x] Spring Boot 프로젝트 초기화 (apps/api/)
- [x] Neon PostgreSQL 연결

---

### Phase 1 — 백엔드 기반 구축 (DB & 인증)

> 목표: API 서버가 뜨고, 회원가입/로그인이 되고, Postman으로 테스트 가능한 상태

#### 1-1. DB 스키마 설계 완성 (진행 중)
- [x] users 테이블
- [ ] books 테이블
- [ ] user_books 테이블 (독서 기록 & 진행률)
- [ ] highlights 테이블
- [ ] notes 테이블 (highlights에 종속)

#### 1-2. JPA Entity 작성
- [ ] `User.java`
- [ ] `Book.java`
- [ ] `UserBook.java`
- [ ] `Highlight.java`
- [ ] `Note.java`
- [ ] 연관관계 매핑 (`@OneToMany`, `@ManyToOne`, `@ManyToMany`)

#### 1-3. Repository 레이어
- [ ] `UserRepository`
- [ ] `BookRepository`
- [ ] `UserBookRepository`
- [ ] `HighlightRepository`
- [ ] `NoteRepository`

#### 1-4. 인증 API (Spring Security + JWT)
- [ ] JWT 토큰 발급/검증 유틸 클래스
- [ ] Spring Security 설정 (`SecurityConfig`)
- [ ] `POST /api/auth/signup` — 회원가입
- [ ] `POST /api/auth/login` — 로그인 (JWT 반환)
- [ ] `POST /api/auth/logout`
- [ ] JWT 인증 필터 (`JwtAuthenticationFilter`)
- [ ] Postman으로 인증 플로우 테스트

#### 1-5. 공통 설정
- [ ] 전역 예외 처리 (`@RestControllerAdvice`)
- [ ] 공통 응답 포맷 (`ApiResponse<T>`)
- [ ] CORS 설정 (Next.js 로컬 개발용)

---

### Phase 2 — 백엔드 핵심 API

> 목표: 책 조회, 하이라이트/메모 CRUD API 완성. Postman으로 전체 플로우 테스트 가능

#### 2-1. 책 API
- [ ] `GET /api/books` — 전체 책 목록
- [ ] `GET /api/books/:id` — 책 상세
- [ ] `GET /api/books/:id/epub` — EPUB 파일 URL 반환 (Cloudflare R2)
- [ ] `POST /api/admin/books` — 책 등록 (관리자 전용)

#### 2-2. 독서 기록 API
- [ ] `POST /api/user-books` — 읽기 시작 (독서 기록 생성)
- [ ] `GET /api/user-books` — 내가 읽는 책 목록
- [ ] `PATCH /api/user-books/:id` — 진행률 업데이트 (현재 페이지)
- [ ] `DELETE /api/user-books/:id` — 읽기 중단

#### 2-3. 하이라이트 API
- [ ] `POST /api/highlights` — 하이라이트 생성
- [ ] `GET /api/highlights?bookId=` — 특정 책의 내 하이라이트 목록
- [ ] `GET /api/highlights/me` — 전체 내 하이라이트 (모아보기)
- [ ] `DELETE /api/highlights/:id` — 하이라이트 삭제

#### 2-4. 메모 API
- [ ] `POST /api/notes` — 메모 작성 (highlightId 필수)
- [ ] `GET /api/notes?highlightId=` — 특정 하이라이트의 메모 목록
- [ ] `PATCH /api/notes/:id` — 메모 수정
- [ ] `DELETE /api/notes/:id` — 메모 삭제

#### 2-5. 공유독서 API
- [ ] `GET /api/books/:id/shared-highlights` — 해당 책의 전체 유저 하이라이트 (공개)
- [ ] `GET /api/books/:id/readers` — 현재 이 책을 읽는 유저 목록

#### 2-6. Cloudflare R2 연동
- [ ] R2 버킷 생성
- [ ] EPUB 파일 업로드 설정
- [ ] 파일 URL 반환 로직

---

### Phase 3 — 프론트엔드 구축

> 목표: Next.js 앱이 뜨고, 백엔드 API와 연동되어 실제로 쓸 수 있는 상태

#### 3-1. Next.js 초기 세팅
- [ ] `apps/web/` — Next.js 15 프로젝트 초기화
- [ ] Tailwind CSS 설정 & 브랜드 토큰 변수화
- [ ] 폴더 구조 설정
  ```
  apps/web/
  ├── app/                  # App Router
  │   ├── (auth)/           # 로그인, 회원가입
  │   ├── (main)/           # 홈, 책 상세, 뷰어
  │   └── layout.tsx
  ├── components/
  │   ├── ui/               # 공통 UI (Button, Input, Card...)
  │   └── features/         # 기능별 컴포넌트
  ├── lib/
  │   ├── api.ts            # API 호출 함수
  │   └── auth.ts           # 인증 유틸
  └── types/                # TypeScript 타입 정의
  ```
- [ ] API 클라이언트 설정 (fetch wrapper 또는 axios)
- [ ] 환경변수 설정 (`.env.local`)

#### 3-2. 인증 화면
- [ ] 로그인 페이지 (`/login`)
- [ ] 회원가입 페이지 (`/signup`)
- [ ] JWT 토큰 저장 & 관리 (httpOnly cookie 권장)
- [ ] 인증 상태 전역 관리
- [ ] 미인증 접근 시 로그인 리다이렉트 (middleware.ts)

#### 3-3. 홈 화면
- [ ] 읽는 중인 책 목록 (`/home`)
- [ ] 탐색 섹션 (전체 책 목록)
- [ ] 공유독자 인디케이터 (몇 명이 읽는 중)
- [ ] 진행률 바

#### 3-4. 책 상세 화면
- [ ] 책 정보 (`/books/:id`)
- [ ] 읽기 시작 / 이어 읽기 버튼
- [ ] 현재 이 책 읽는 독자 표시

#### 3-5. 이북 뷰어 (핵심)
- [ ] epub.js 설치 & 세팅
- [ ] EPUB 렌더링 (`/reader/:bookId`)
- [ ] 페이지 넘기기 (이전/다음)
- [ ] 글씨 크기 조절
- [ ] 텍스트 선택 → 컨텍스트 메뉴 (하이라이트 / 메모 추가)
- [ ] 하이라이트 시각적 표시 (파란 배경)
- [ ] 진행률 저장 (현재 페이지 → API 업데이트)
- [ ] 우측 공유 메모 패널 UI

#### 3-6. 하이라이트 모아보기
- [ ] 내 전체 하이라이트 목록 (`/highlights`)
- [ ] 책별 필터
- [ ] 하이라이트 클릭 시 해당 페이지로 이동

---

### Phase 4 — 실시간 기능 (공유독서 핵심)

> 목표: 같은 책을 읽는 유저의 메모가 실시간으로 보이는 상태

#### 4-1. Supabase Realtime 설정
- [ ] Supabase 프로젝트 생성 (Realtime 전용)
- [ ] 채널 설계: `book:{bookId}` 단위 채널
- [ ] 백엔드: 메모 저장 시 Realtime 이벤트 발행
- [ ] 프론트: 채널 구독 & 실시간 메모 수신

#### 4-2. 실시간 UI
- [ ] 우측 패널 실시간 메모 스트림
- [ ] "방금 누가 메모를 남겼어요" 알림 바
- [ ] 현재 이 책 읽는 독자 실시간 카운트
- [ ] 하이라이트 위 메모 수 배지 (`💬 4`) 실시간 업데이트

---

### Phase 5 — 배포

> 목표: 실제 URL로 접근 가능한 서비스

#### 5-1. 백엔드 배포 (Railway)
- [ ] Railway 프로젝트 생성
- [ ] GitHub 연동 (apps/api/ 경로 지정)
- [ ] 환경변수 설정 (DB_URL, DB_USERNAME, DB_PASSWORD, JWT_SECRET 등)
- [ ] 배포 확인 (Railway 제공 URL로 API 테스트)
- [ ] 커스텀 도메인 연결 (선택)

#### 5-2. 프론트엔드 배포 (Vercel)
- [ ] Vercel 프로젝트 생성
- [ ] GitHub 연동 (apps/web/ 경로 지정)
- [ ] 환경변수 설정 (`NEXT_PUBLIC_API_URL` = Railway URL)
- [ ] 배포 확인
- [ ] 커스텀 도메인 연결 (pagefold.app 등)

#### 5-3. 배포 후 확인
- [ ] 전체 플로우 E2E 테스트 (회원가입 → 책 읽기 → 하이라이트 → 공유)
- [ ] CORS 설정 확인 (Vercel 도메인 허용)
- [ ] HTTPS 확인

---

### Phase 6 — MVP 이후 확장 (선택)

> MVP 완성 후 여유가 생기면 순서대로 추가

#### 6-1. 서평 & 별점
- [ ] `reviews` 테이블 추가
- [ ] 별점 & 서평 작성 API
- [ ] 책 상세 페이지에 서평 섹션 추가

#### 6-2. 책 검색
- [ ] `GET /api/books/search?q=` API
- [ ] PostgreSQL Full-Text Search 또는 LIKE 쿼리
- [ ] 검색 결과 UI

#### 6-3. 북클럽 / 커뮤니티
- [ ] `clubs` 테이블 (그룹 독서 모임)
- [ ] 멤버 초대 & 참여
- [ ] 클럽 전용 공유독서 채널

#### 6-4. 소셜 로그인
- [ ] Google OAuth 연동 (Spring Security OAuth2)
- [ ] 프론트 소셜 로그인 버튼

#### 6-5. 모바일 앱
- [ ] React Native 프로젝트 생성
- [ ] 기존 Next.js 컴포넌트 로직 재활용
- [ ] 앱스토어 배포

---

## 12. API 설계 원칙

### REST 컨벤션
```
GET    /api/books          # 목록 조회
GET    /api/books/:id      # 단건 조회
POST   /api/books          # 생성
PATCH  /api/books/:id      # 부분 수정
DELETE /api/books/:id      # 삭제
```

### 공통 응답 포맷
```json
{
  "success": true,
  "data": { ... },
  "message": "ok"
}
```

### 에러 응답 포맷
```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "로그인이 필요합니다."
  }
}
```

### 인증
- JWT Bearer Token 방식
- `Authorization: Bearer {token}` 헤더
- 토큰 만료: Access Token 1시간, Refresh Token 7일 (추후 구현)

---

*Last updated: 2026-10-04*
