# 퀴즈 사이트

Supabase + Netlify 기반 퀴즈 출제·채점 시스템.

과목명, 제목, 부제, 발행번호, 기준일, 푸터 문구 등은 관리자 패널에서 세트별로 자유롭게 설정할 수 있어 어떤 과목·용도에도 적용 가능합니다.

## 대상

소수 인원(학급 단위 등)을 대상으로 교사·출제자가 직접 문제를 등록하고 학생이 풀이하는 용도로 설계되었습니다.

> **⚠️ 보안 주의**
> 이 프로젝트는 **검색엔진에 노출되지 않는 것을 전제**로 보안을 최소화한 단순 HTML 파일입니다.
> - 관리자 비밀번호·학생 PIN이 클라이언트 코드에 노출됩니다
> - 별도 인증 서버 없이 Supabase anon key로만 동작합니다
> - 불특정 다수에게 URL이 공개되는 환경에서는 사용을 권장하지 않습니다

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
