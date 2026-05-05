# Decisions Locked · K-Local Global v5

> 빌드 진행 중 확정된 결정의 누적 기록.
> 같은 결정이 두 번 흔들리지 않게 하는 안전장치.
> 47p 후반부 일관성을 결정합니다.

---

## Format

각 결정은 다음 형식으로 추가:

```
- [Page or Component] · 결정 사항 한 줄 (선택안 코드 / 결정 일자)
```

날짜는 YYYY-MM-DD. 같은 항목이 후속 단계에서 변경되면 — 기존 줄을 ~~취소선~~ 처리하고 새 줄을 추가.

---

## Step 0 · Foundation (구조 결정 · 2026-05-05)

### Information Architecture
- **index ↔ opening 분리**: index는 Cover 축약 + 6 챕터 카드, opening은 P01~P03 풀버전 (B안 확정)
- **Chapter 4 (13p)**: 한 페이지 유지 + sub-section anchor mini-nav (A+안 확정)
- **챕터 분리 원칙**: 1 챕터 = 1 HTML, 7~13 섹션 세로 스크롤
- **URL 구조**: `/`, `/opening`, `/why`, `/gap`, `/build`, `/scale`, `/closing`

### Component System
- **4 베이스 + 변형 시스템 확정**:
  - Base 1 · `card-bordered` (좌측/상단 border)
  - Base 2 · `card-featured` (ink 배경)
  - Base 3 · `flow-diagram` (가로 / 원형)
  - Base 4 · `mockup-frame` (Maps / Chat / Webtoon)
  - Special · `cover-seal` (P01 단일)
- **Modifier 시스템**: `is-{position}` + `is-{accent-color}` + `is-{state}`

### HTML Pattern Enhancements
- **Sticky 헤더 진행도** 포함 (예: "Ch.4 · Build · 5/13")
- **3-way 챕터 footer** 포함 (← prev · ☰ index · → next)
- **OG meta / Twitter Card** 챕터별 포함
- **JSON-LD Article schema** 챕터별 포함

### Operational
- **Style Guide 페이지**: `/_styles.html` · noindex · 내부용
- **decisions.md**: 빌드 단계마다 누적 갱신

### Collaboration Protocol
- **Vague Feedback → Code Variations** 프로토콜 확정
- 한 축으로 좁힌 변형 2~3안 (max 3) 후 B안 기본 구현 / A·C diff 시연
- 시각/디자인 피드백 = 코드 직접 / 콘텐츠/메시지 피드백 = MD 먼저

---

## Step 1 · Base System (CSS Tokens · Components · Layout · 2026-05-05)

### Files Built
- `src/css/tokens.css` — 색·폰트·spacing·shadow·radii·motion·z-index 변수
- `src/css/base.css` — reset + typography + 한글 word-break 절대 규칙
- `src/css/components.css` — 4 베이스 + 변형 + 추가 컴포넌트 (Number, Quote, Mechanism, Menu, Pct Bar, Pill, Info Box)
- `src/css/layout.css` — page container, sticky header, section, chapter footer, mini-nav
- `public/_styles.html` — 시각 검수 페이지 (15 섹션)
- `decisions.md` — 결정 누적 (이 파일)

### Token Decisions
- **Spacing scale**: 8px 기반 (--s-1 ~ --s-12), --s-5 = 22px (페이지 padding 모바일과 일치)
- **Border weight**: hair / thin / mid / thick / accent(3px) / anchor(4px)
- **Radius scale**: xs(2) / sm(4) / md(8) / lg(14) / xl(22) / pill(999)
- **Motion**: fast(140ms) / mid(240ms) / slow(480ms) + ease-out / ease-in-out
- **Reduced motion**: 자동 1ms (접근성)
- **Print 호환**: shadow 제거, bg white (PDF 추출 옵션 살아있음)

### Component Decisions
- **Mind Card**: 큰따옴표(\201C/\201D) ::before/::after 자동 — 한글 큰따옴표 파싱 안전
- **Card Featured `is-radiant`**: gold radial gradient 장식 (P02 Core Statement용)
- **Cover Seal**: 60s 회전 애니메이션 + prefers-reduced-motion 존중
- **Pct Bar fill**: green-mid → green 그라디언트 / gold 변형 별도
- **Flow Node Pivot**: ink 배경 + gold-top + "매출이 갈라지는 자리" 라벨 absolute 포지셔닝

### Layout Decisions
- **Page max-width**: 760px (모바일 친화 + 데스크톱 가독)
- **Sticky header**: backdrop-filter blur(10px) + ivory 92% opacity
- **Mini-nav (챕터 4)**: sticky 헤더 바로 아래 (top: 38px 모바일 / 46px 데스크톱)
- **Section scroll-margin-top**: 60px / 80px (sticky 헤더 보정)
- **Chapter nav**: 모바일 1열 / 640px+ 1fr auto 1fr 그리드

---

## Step 1.5 · Color System v2 채택 (2026-05-05)

### Decision
- v2 색 시스템 **전면 채택** — 시인성 + 조화 동시에 sharpen
- 비교 기준: `_color-comparison.html` 7 섹션 검수 후 직관적 채택
- 사상 그대로 (아이보리 + 네이비 + 그린 + 절제된 골드) · 톤만 한 단계 위로

### Token Migration · v1 → v2

**Surface (누런기 ↓ · 분리감 ↑)**
- `--bg`            #F2EEE3 → #F4F0E6
- `--surface`       #FBF8F0 → #FFFCF4 (거의 화이트 — 카드 분리감 강화)
- `--surface-cool`  #F6F2E6 → #F0EBDC (bg와 명도차 명확)
- `--surface-mist`  #EFEBDD → #EAE3CF

**Ink (Navy · 채도 ↑)**
- `--ink`       #0E1B33 → #0A1628 (더 진한 navy · 본문 가독성 ↑)
- `--ink-soft`  #2B3A57 → #1F2D4D
- `--ink-mid`   #4A5773 → #3F4D6F
- `--muted`     #6E7A93 → #8089A1 (메타 위계 분리)

**Lines (살짝 더 짙게)**
- `--line`         #DDD6C4 → #D4CDB9
- `--line-soft`    #E8E2D0 → #E0DAC6
- `--line-strong`  #C9C0A8 → #BFB59C

**Green (또렷한 청록)**
- `--green`       #1F5C50 → #0E6E5A (신뢰 + 활력)
- `--green-mid`   #2F7F70 → #3B9684 (강조 시인성 ↑)
- `--green-soft`  #DCE9E2 → #C8E2D8
- `--green-mist`  #E8EFEA → #E4F0EB
- `--green-deep`  #133E36 → #07453A

**Gold (채도 ↑)**
- `--gold`       #9B7A38 → #B6873A
- `--gold-tone`  #B89352 → #D4A85C (navy 위 또렷)
- `--gold-soft`  #EFE2C2 → #F2DDA8 (따뜻한 sand)
- `--gold-mist`  #F5ECD8 → #FBF1D9

**Red Quiet (정합성 유지)**
- `--red-quiet`  #8E4A3F → #9B4537
- `--red-soft`   #EFD9D2 → #F2D9D1

### Cascading Updates
- `tokens.css` — shadow rgba 새 ink 기준 동기화 (`rgba(10,22,40,*)`)
- `tokens.css` — print 미디어 surface도 white로 추가 명시
- `layout.css` — sticky header backdrop `rgba(244,240,230,0.92)`
- `layout.css` — chapter mini-nav backdrop `rgba(244,240,230,0.94)`
- `components.css` — gold radial/dashed border 4곳을 새 gold-tone rgba로
- `_styles.html` — swatch hex 16개 + Hub-and-Spoke SVG hex 일괄 치환

### Verification
- CSS 전체 `grep` 검사 → v1 hex 잔재 0
- `_styles.html` 잔재 0
- 토큰 단일 진실 공급원 무결성 유지



## Step 2 · Chapter 1 · Opening (2026-05-05 · 검수 완료)

### Files Built
- `public/opening.html` — P01 Cover + P02 Core Statement + P03 Business Structure
- OG meta + Twitter Card + JSON-LD Article schema 포함
- 검수 통과 → 잠금

### Page Decisions
- **P01 Cover Seal**: 회전 80s (60s에서 변경 — 차분한 톤)
- **P01 Cover Seal**: 1024px+ 에서만 표시 (768~1023px 태블릿은 숨김)
- **P01 "선택하게"**: green color + 그라디언트 underline (green-soft → gold-soft) 동시 적용
- **P02 eyebrow 위치**: 모바일 본문 위 / 데스크톱 카드 우측 상단 absolute
- **P02 카드 min-height**: 70vh 모바일 / 78vh 데스크톱 (사업 무게감 위해 화면 거의 가득)
- **P03 3 cell hover lift**: `@media (hover: hover)`로 모바일 비활성, 데스크톱만 transform

---

## Step 3 · Chapter 2 · Why Now (2026-05-05)

### Files Built
- `public/why.html` — P04~P13 (10 페이지)
- OG meta + Twitter Card + JSON-LD Article schema 포함

### Page Decisions
- **P04/P07/P10 (텍스트 only 진입 페이지)**: 공통 `text-headline-section` 스타일 — min-height 50vh 모바일 / 60vh 데스크톱
- **P05 Number Card**: featured 카드(15.7조)는 `is-featured` modifier — ink 배경 + gold 숫자 + dashed border rgba(212,168,92,0.28)로 정합성
- **P06 Driver Cards**: 2x2 그리드 + 좌측 2px green-mid border (640px+에서 2열, 모바일 1열)
- **P06 인용 박스**: `quote-card` 컴포넌트 재사용 (cite 줄 포함)
- **P06 For Local Business**: `info-box` 컴포넌트 재사용 (green-mist 배경)
- **P08 환율 환산**: `fx-card` (ink 배경 + gold radial gradient + 3 row 그리드 1fr 32px 1fr)
- **P08 fx-arrow**: 640px 이하 모바일에서 숨김 (세로 흐름이 자연스러우니 화살표 제거)
- **P09 Point Cards**: 좌측 2px gold border + 세로 stack (모바일/데스크톱 모두 세로)
- **P11 Tourist Journey SVG**: viewBox 760×240 가로 SVG · Stage 4(★) gold halo opacity 0.10 + 0.08 이중 후광 + 36px 반지름 (다른 노드 28px) · "매출이 갈라지는 자리" 라벨 위에 absolute 배치
- **P11 SVG**: 모바일에서는 viewBox 유지하면서 가로폭 100% 자동 축소 (가로 스크롤 X)
- **P12 6 Categories**: 모바일 2x3 / 560px+ 3x2 / 768px+ 6x1 (3단계 반응형) · category-card top border green-mid
- **P13 Top Origins**: ink 배경 strip · 모바일 세로 / 데스크톱 가로 (label 좌, 국가 리스트 우 flex justify-end)
- **P13 origin-item**: Fraunces italic 13~14px white + Mono 9~10px gold-tone pct (한자/영문/한글 혼합 타이포그래피 자체가 글로벌 확장감)

### Component Reuse
- `quote-card`, `info-box`, `pill is-gold` 모두 components.css에서 재사용 — 신규 컴포넌트 추가 0
- 페이지별 page-specific style만 `<style>` 태그에 추가

---

## Step 4 · Chapter 3 · Gap (예정)

---

## Step 5 · Chapter 4 · Build (예정)

---

## Step 6 · Chapter 5 · Scale (예정)

---

## Step 7 · Chapter 6 · Closing (예정)

---

## Open Questions · 빌드 중 정해야 할 것

> 미해결 항목. 챕터 진입 시점에 결정.

- [ ] **OG 이미지 생성 방식**: SVG → PNG 변환 vs 별도 디자인 (챕터 6 마무리 단계에서)
- [ ] **챕터 간 transition 페이지 처리**: 챕터 footer만으로 충분한지 vs interstitial 페이지 추가
- [ ] **Reading progress bar**: sticky 헤더 하단에 가로 진행 막대 추가 여부
- [ ] **Print/PDF 추출**: 47p 단일 PDF 빌드 옵션 (`/pdf` 라우트) 만들지 여부
- [ ] **i18n 분기**: 한글 자료가 영문판 필요한지 (현재는 한글 기준)

---

## Forbidden Words · 절대 사용 금지

> 사업 톤 보호용. 빌드 중 무의식적 등장 방지.

- "확정 수익률"
- "원금보장"
- "고정 이자"
- "guaranteed return"
- "투자 권유"
- "수익 분배 약정"
- "monthly dividend" / "fixed return"

콘텐츠 검수 시 자동 grep 권장:
```bash
grep -rE "(확정 수익|원금보장|고정 이자|guaranteed return|투자 권유)" content/ public/ src/
```

---

— last updated 2026-05-05 · Step 1 complete
