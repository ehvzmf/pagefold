# 프로젝트 정보
- 프로젝트명: Pagefold (a.k.a. Pafol)
- 프로젝트 개요: 공유 독서 서비스 (하이라이트, 메모, 공유)
  북클럽/독서 동호회 타겟의 공유 독서 플랫폼. 이북을 읽으면서 하이라이트·메모를 남기고, 같은 책을 읽는 사람들과 실시간으로 나눌 수 있는 서비스.

### MVP

1. 인증 — 회원가입, 로그인, 로그아웃. 소셜 로그인(Google)은 있으면 좋지만 MVP에서는 이메일/패스워드만.
2. 이북 뷰어 — EPUB 파일 렌더링. 페이지 넘기기, 글씨 크기 조절 정도의 기본 뷰어 기능.
   - 콘텐츠 수급 — 공개 도메인 기반. Project Gutenberg / Standard Ebooks에서 EPUB 확보, 관리자가 직접 업로드. 유저 업로드 없음.
3. 하이라이트 & 메모 (개인) — 문장 선택 후 하이라이트, 메모 작성/수정/삭제. 내 하이라이트 모아보기.
4. 공유 독서 — 같은 책을 읽는 다른 유저의 하이라이트·메모를 실시간으로 볼 수 있는 기능. Pagefold의 핵심.

### 이후 단계 (MVP 이후)

커뮤니티(북클럽, 게시판), 서평·별점, 책 검색/탐색, 소셜 로그인, 모바일 앱.


### Tech Stack

| 영역 | 확정 스택 |
|---|---|
| 프론트 | Next.js 15 + TypeScript + Tailwind |
| 백엔드 | Spring Boot 3 + Java |
| ORM | JPA (Hibernate) |
| 빌드 | Gradle |
| DB | Neon PostgreSQL |
| 실시간 | Supabase Realtime |
| 인증 | Spring Security + JWT |
| 파일 스토리지 | Cloudflare R2 |
| 프론트 배포 | Vercel |
| 백엔드 배포 | Railway |
| 패키지 매니저 | pnpm (프론트) |

### Architecture
monorepo 
```agsl
pagefold/
├── apps/
│   ├── web/          # Next.js (프론트)
│   └── api/          # Spring Boot (백엔드)
├── docs/             # 설계 문서, 와이어프레임
└── README.mdS
```

### DB Schema
users — 회원 정보
  - id
  - email
  - password_hash
  - user_name
  - profile_pic
  - created_at
  - updated_at
- deleted_at
books — 책 정보 (공개 도메인)
user_books — 유저가 어떤 책을 읽고 있는지, 진행률
highlights — 하이라이트 (어떤 책, 몇 페이지, 어떤 문장)
notes — 메모 (하이라이트에 달린 코멘트)