# 퀴즈 사이트

Supabase + Netlify 기반 퀴즈 출제·채점 시스템. 어떤 과목이든 문제 등록부터 채점까지 가능합니다.

## 기능

- **문제 유형**: 빈칸 퀴즈 / 지문형 빈칸(cloze) / 객관식 / 복수선택 / 서술형
- **학생 관리**: 이름 + 4자리 PIN 로그인, 성적 현황
- **해설 보기 모드**: 세트별 비밀번호로 정답·해설 공개
- **채점**: 자동 채점 / 전체 채점 / 점수 누적
- **내보내기**: DOCX · PDF
- **기타**: 문제 그룹 묶기, 하이퍼링크 해설, 이미지 첨부

## 설치

### 1. 파일 준비

```bash
cp index.template.html index.html
```

### 2. 자격증명 입력 (`index.html`)

```js
const SB_URL = 'YOUR_SUPABASE_URL';       // Supabase 프로젝트 URL
const SB_KEY = 'YOUR_SUPABASE_ANON_KEY'; // Settings > API > anon key
const DEFAULT_PW = 'YOUR_ADMIN_PASSWORD'; // 관리자 초기 비밀번호
```

### 3. Supabase 테이블 생성

```sql
create table quiz_sets (
  id uuid primary key default gen_random_uuid(),
  name text,
  description text,
  config jsonb,
  articles jsonb,
  created_at timestamptz default now()
);

create table students (
  id uuid primary key default gen_random_uuid(),
  name text,
  pin text,
  created_at timestamptz default now()
);

create table submissions (
  id uuid primary key default gen_random_uuid(),
  student_id uuid references students(id),
  quiz_set_id uuid references quiz_sets(id),
  answers jsonb,
  earned numeric,
  total numeric,
  created_at timestamptz default now()
);
```

### 4. 배포

`index.html`과 `docx.js`를 Netlify / GitHub Pages 등에 업로드합니다.

## 파일 구조

```
index.template.html  ← 자격증명 제거된 템플릿 (GitHub용)
index.html           ← 실제 운영 파일 (.gitignore 처리)
docx.js              ← DOCX 생성 라이브러리
```
