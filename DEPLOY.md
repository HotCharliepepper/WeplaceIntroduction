# Netlify 배포 가이드 · v5

> 현재 상태: Chapter 1~2 빌드 완료 / Chapter 3~6 placeholder
> 배포 즉시 가능 — 모바일에서 흐름 검수용

---

## 가장 빠른 방법 · 드래그앤드롭 (60초)

1. **이 폴더 통째로 압축** (`k-local-global` 또는 `public` 폴더)
2. https://app.netlify.com/drop 접속
3. 압축 파일 또는 `public/` 폴더를 브라우저에 드래그
4. 자동 배포 → 임시 URL 즉시 발급 (`https://random-name.netlify.app`)
5. 배포 완료 후 사이트 이름 변경 (Site settings → Change site name)

**핵심 주의**: 드래그앤드롭 시 `public/` 폴더만 압축해서 올리세요. (root 폴더 통째로 올리면 publish 디렉토리 인식 못 함)

---

## 더 나은 방법 · Git 연동 (권장 · 한 번 세팅 후 자동 배포)

### 1. GitHub repo 생성

```bash
cd k-local-global
git init
git add .
git commit -m "v5 · Chapter 1~2 빌드 완료"

# GitHub에서 새 repo 생성 후
git remote add origin https://github.com/YOUR-NAME/k-local-global.git
git branch -M main
git push -u origin main
```

### 2. Netlify와 연동

1. https://app.netlify.com → "Add new site" → "Import an existing project"
2. GitHub 선택 → repo 선택 (`k-local-global`)
3. Build settings:
   - **Build command**: 비워둠 (정적 사이트)
   - **Publish directory**: `public`
4. "Deploy site" 클릭

이후 `git push` 할 때마다 자동 재배포.

### 3. (선택) 커스텀 도메인 연결

- Netlify → Domain management → Add custom domain
- 도메인이 없으면 그대로 `*.netlify.app` 사용 가능 (충분히 안정적)

---

## 배포 후 확인 URL

```
/                  ← Cover · 6 챕터 인덱스
/opening           ← Chapter 1 (P01-P03 · 3p)
/why               ← Chapter 2 (P04-P13 · 10p)
/gap               ← Chapter 3 (placeholder · "곧 공개")
/build             ← Chapter 4 (placeholder)
/scale             ← Chapter 5 (placeholder)
/closing           ← Chapter 6 (placeholder)

/styles            ← 컴포넌트 라이브러리 (내부용 · noindex)
/colors            ← v1/v2 색 비교 (내부용 · noindex)
```

`.html` 확장자 붙여서 접근해도 동일하게 작동 (`/opening` = `/opening.html`).

---

## 폴더 구조 (배포되는 것)

```
public/                    ← Netlify가 publish할 폴더
├── index.html             ← Cover 진입점
├── opening.html           ← Chapter 1 풀버전
├── why.html               ← Chapter 2 풀버전
├── gap.html               ← Chapter 3 placeholder
├── build.html             ← Chapter 4 placeholder
├── scale.html             ← Chapter 5 placeholder
├── closing.html           ← Chapter 6 placeholder
├── _styles.html           ← 내부용 (검색엔진 차단)
├── _color-comparison.html ← 내부용 (검색엔진 차단)
├── _chapter-pending.html  ← placeholder 원본
└── css/
    ├── tokens.css         ← 색·폰트·spacing 변수
    ├── base.css           ← reset + typography + 한글 word-break
    ├── components.css     ← 4 베이스 + 변형 시스템
    └── layout.css         ← 페이지·헤더·푸터·네비

netlify.toml              ← 배포 설정 (root에 있어야 함)
```

---

## netlify.toml 설정 핵심

- **Clean URL**: `/opening` ↔ `/opening.html` 양방향 200 응답
- **보안 헤더**: X-Frame-Options DENY, HSTS 1년, Permissions-Policy 카메라/마이크/위치 차단
- **CSS 캐시**: 1년 immutable (배포마다 새 파일이라 안전)
- **Noindex**: `_styles.html`, `_color-comparison.html` 검색엔진 차단

---

## 다음 챕터 빌드 후 갱신 절차

1. 새 챕터 HTML 파일 빌드 (예: `gap.html`)
2. 기존 placeholder 파일을 새 빌드본으로 덮어쓰기
3. `netlify.toml` 수정 불필요 (이미 `/gap` → `/gap.html` 매핑 되어있음)
4. `index.html`의 챕터 카드를 `is-pending`에서 `is-available`로 변경
5. `_chapter-pending.html` 안의 roadmap-list 상태 업데이트
6. Git push → 자동 배포

---

## 현재 미해결 / 다음 호흡

- [ ] Chapter 3 (Gap) 빌드 — 다음 호흡
- [ ] Chapter 4 (Build) 빌드 — 가장 큰 챕터 13p
- [ ] Chapter 5 (Scale) 빌드
- [ ] Chapter 6 (Closing) 빌드
- [ ] OG 이미지 생성 (현재는 텍스트 메타만 · 공유 시 프리뷰 이미지 없음)
- [ ] favicon · apple-touch-icon (현재 없음 · 브라우저 탭 기본 아이콘)

— v5 · 2026-05-05 · Chapter 1~2 배포 가능 상태
