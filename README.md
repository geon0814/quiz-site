# 성공적인 직업생활 퀴즈 사이트

Supabase + Netlify 기반 퀴즈 출제·채점 시스템

## 기능
- 빈칸 퀴즈 / 지문형 빈칸 / 객관식 / 복수선택 / 서술형
- 학생 로그인 (이름 + 4자리 PIN)
- 해설 보기 모드 (세트별 비밀번호)
- 전체 채점 / 성적 현황
- DOCX · PDF 내보내기
- 문제 그룹 묶기 / 하이퍼링크 해설

## 설치

1. `index.template.html`을 `index.html`로 복사
2. 아래 항목을 실제 값으로 교체

```js
const SB_URL = 'YOUR_SUPABASE_URL';
const SB_KEY = 'YOUR_SUPABASE_ANON_KEY';
const DEFAULT_PW = 'YOUR_ADMIN_PASSWORD';
```

3. Supabase에 `quiz_sets`, `students`, `submissions` 테이블 생성
4. Netlify 등에 `index.html`, `docx.js` 배포

## Supabase 테이블 구조

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
