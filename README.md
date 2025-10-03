# 🏛️ 기억의 궁전 (Memory Palace)

AI 기반 기억의 궁전 앱 - Gemini AI를 활용하여 각 항목을 장소에 배치하고 시각적 이미지와 스토리로 암기를 돕습니다.

## ✨ 주요 기능

- 🤖 **Gemini AI 자동 매칭**: 각 항목을 적절한 세부 장소에 자동 배치
- 🖼️ **시각적 이미지**: 각 항목마다 생생한 시각적 이미지 생성
- 📖 **스토리 기반 암기**: 기억하기 쉬운 스토리와 함께 제공
- 🏛️ **8개 기본 장소**: 집, 학교, 공원, 쇼핑몰, 박물관, 도서관, 사무실, 지하철역
- 🧠 **퀴즈 모드**: 복습 기능으로 기억력 테스트
- 🔐 **Supabase 인증**: 사용자별 데이터 관리

## 🚀 배포 방법

### 1. Supabase 프로젝트 생성

1. [Supabase](https://supabase.com) 접속 및 로그인
2. "New Project" 클릭
3. 프로젝트 이름: `memory-palace`
4. Database Password 설정
5. Region 선택 후 "Create new project" 클릭

### 2. Supabase 테이블 생성

SQL Editor에서 다음 쿼리 실행:

\`\`\`sql
-- 궁전 테이블 생성
CREATE TABLE palaces (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    category TEXT NOT NULL,
    location TEXT NOT NULL,
    items JSONB NOT NULL,
    ai_story JSONB,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    last_reviewed TIMESTAMP WITH TIME ZONE,
    review_count INTEGER DEFAULT 0
);

-- Row Level Security 활성화
ALTER TABLE palaces ENABLE ROW LEVEL SECURITY;

-- 정책 생성: 사용자는 자신의 데이터만 읽기/쓰기 가능
CREATE POLICY "Users can view their own palaces"
    ON palaces FOR SELECT
    USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own palaces"
    ON palaces FOR INSERT
    WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update their own palaces"
    ON palaces FOR UPDATE
    USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own palaces"
    ON palaces FOR DELETE
    USING (auth.uid() = user_id);

-- 인덱스 생성
CREATE INDEX palaces_user_id_idx ON palaces(user_id);
CREATE INDEX palaces_created_at_idx ON palaces(created_at DESC);
\`\`\`

### 3. Vercel 배포

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/leoking9009/gung)

#### 수동 배포:

1. [Vercel](https://vercel.com) 로그인
2. "Import Project" 선택
3. GitHub 저장소 선택: `leoking9009/gung`
4. 환경 변수 설정:
   - `VITE_SUPABASE_URL`: Supabase Project URL
   - `VITE_SUPABASE_ANON_KEY`: Supabase Anon Public Key
5. "Deploy" 클릭

### 4. 환경 변수 찾기

Supabase 프로젝트에서:
1. Settings → API 이동
2. **Project URL** 복사 → `VITE_SUPABASE_URL`
3. **anon public** key 복사 → `VITE_SUPABASE_ANON_KEY`

## 🛠️ 로컬 개발

\`\`\`bash
# .env 파일 생성
cp .env.example .env

# .env 파일에 Supabase 정보 입력
VITE_SUPABASE_URL=your-url-here
VITE_SUPABASE_ANON_KEY=your-key-here

# 브라우저에서 index.html 열기
\`\`\`

## 📝 사용 방법

1. **회원가입/로그인**
2. **API 키 설정**: Gemini API 키 입력
3. **새 궁전 만들기**:
   - 궁전 이름 입력
   - 기억할 항목 입력 (쉼표로 구분)
   - "✨ AI 자동 매칭" 클릭
4. **복습하기**: 보관함에서 "🧠 테스트" 클릭

## 🔑 Gemini API 키 발급

1. [Google AI Studio](https://aistudio.google.com/app/apikey) 접속
2. "Create API key" 클릭
3. 생성된 키를 앱에 입력

## 🤝 기여

기여는 언제나 환영합니다!

## 📄 라이선스

MIT License
