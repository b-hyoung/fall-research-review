# 사용자 플로우 페이지 구현 계획

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** presentation.html Step 3의 추상적 mmWave+CSI 듀얼 센서 제안을 "어떤 시스템(파이프라인)으로 무슨 데이터를 뽑는가"가 한눈에 보이는 단일 HTML 페이지로 구체화한다.

**Architecture:** 단일 HTML 파일 (`paper-review/user-flow.html`), CSS/JS 인라인, JavaScript 없음. 세로 스크롤로 환경 브리핑 → 타임라인 네비 → 3개 장면 카드(2×2 그리드) → 종합 블록 → 참고문헌. 각 장면 카드는 환자·CSI·mmWave·운영자 4관점 동시 표시, 센서 칸은 `[감지 항목 → 추출 시스템 → 예시값 → 근거]` 구조.

**Tech Stack:** HTML5, CSS Grid/Flex, Pretendard 폰트 (CDN), presentation.html의 CSS 변수·유틸 클래스 재활용. 외부 리소스·빌드 도구 없음.

**Reference Spec:** [docs/superpowers/specs/2026-04-17-user-flow-page-design.md](../specs/2026-04-17-user-flow-page-design.md)

**Reviewer Policy (사용자 요청):** 각 논리 단계(Task 3, 7, 10, 13) 완료 시 `superpowers:code-reviewer` 에이전트로 코드·근거 정합성 교차 확인. 배지 없는 숫자가 남아 있으면 빌드 차단 신호.

---

## File Structure

**생성**
- `paper-review/user-flow.html` — 신규 (예상 600–900줄)

**수정**
- `paper-review/presentation.html` — 하단 푸터 링크 한 줄 추가 (line 932 근처)

**건드리지 않음**
- `paper-review/table-visual.html`
- `paper-review/table-flat.md`, `table-grouped.md`
- `paper-review/papers.json`
- `paper-review/fix-list.md`, `verification-report.md`

---

## Task 1: HTML 골격 + CSS 변수 이식

**Files:**
- Create: `paper-review/user-flow.html`

- [ ] **Step 1: 골격 파일 생성**

presentation.html의 상단 28줄(DOCTYPE부터 CSS 변수 블록까지)을 그대로 복제해서 새 파일 시작. `<title>`만 변경.

파일 내용:

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>사용자 플로우 — 독거노인 독실 하루 시나리오</title>
    <link rel="preconnect" href="https://cdn.jsdelivr.net" crossorigin>
    <link rel="stylesheet" as="style" crossorigin href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable-dynamic-subset.min.css">
    <style>
        :root {
            --bg: #f6f7fa;
            --surface: #ffffff;
            --surface-hover: #f9fafb;
            --border: #e4e7ed;
            --border-light: #eef0f4;
            --text-1: #16181d;
            --text-2: #4e5562;
            --text-3: #8a8f9d;
            --accent: #3563e9;
            --accent-soft: #edf1fd;
            --orange: #d97706;
            --orange-soft: #fef9ee;
            --blue: #2572cc;
            --blue-soft: #eef5fc;
            --green: #15803d;
            --green-soft: #eef7f1;
            --rose: #be123c;
            --rose-soft: #fdf1f4;
            --purple: #7c3aed;
            --purple-soft: #f3efff;
            --amber: #b45309;
            --amber-soft: #fef6e7;
            --r-sm: 8px;
            --r-md: 12px;
            --r-lg: 16px;
            --sh-sm: 0 1px 2px rgba(22,24,29,0.04);
            --sh-md: 0 4px 12px rgba(22,24,29,0.06);
            --ease: cubic-bezier(0.25,0.1,0.25,1);
        }

        *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Pretendard Variable', 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, 'Apple SD Gothic Neo', 'Noto Sans KR', sans-serif;
            background: var(--bg);
            color: var(--text-1);
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
            text-rendering: optimizeLegibility;
            word-break: keep-all;
            line-break: strict;
            -webkit-text-size-adjust: 100%;
        }

        .container { max-width: 1200px; margin: 0 auto; padding: 32px 20px 80px; }

        h1 { font-size: 28px; font-weight: 800; letter-spacing: -0.02em; color: var(--text-1); }
        h2 { font-size: 22px; font-weight: 700; letter-spacing: -0.015em; color: var(--text-1); margin-top: 40px; margin-bottom: 12px; }
        h3 { font-size: 17px; font-weight: 700; color: var(--text-1); }
        p { font-size: 14.5px; line-height: 1.6; color: var(--text-2); }
    </style>
</head>
<body>
<div class="container">

</div>
</body>
</html>
```

- [ ] **Step 2: 브라우저에서 열어 빈 페이지 확인**

파일을 더블클릭으로 브라우저에 띄워 Pretendard 폰트가 로드되고 오류 없는지 확인.

- [ ] **Step 3: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): HTML skeleton with shared CSS variables"
```

---

## Task 2: 헤더 블록

**Files:**
- Modify: `paper-review/user-flow.html` (container 내부 상단)

- [ ] **Step 1: 헤더 CSS 추가**

`<style>` 블록 내부에 추가:

```css
.header { text-align: center; padding: 24px 0 32px; border-bottom: 1px solid var(--border-light); margin-bottom: 32px; }
.header h1 { margin-bottom: 8px; }
.header .subtitle { font-size: 15px; color: var(--text-2); font-weight: 500; }
.header .badge-line { margin-top: 14px; font-size: 13px; color: var(--text-3); }
.header .badge-line strong { color: var(--text-2); }
```

- [ ] **Step 2: 헤더 마크업 삽입**

`<div class="container">` 바로 안쪽에:

```html
<header class="header">
    <h1>사용자 플로우 — 독거노인 독실 하루 시나리오</h1>
    <div class="subtitle">환자 · CSI · mmWave · 운영자 4관점 동시 추적</div>
    <div class="badge-line">
        <strong>표기 철학:</strong> 어떤 <em>시스템(파이프라인)</em>으로 뽑는가 → 예시값 → 근거 순서로 전개
    </div>
</header>
```

- [ ] **Step 3: 브라우저에서 헤더 확인**

제목·부제·배지 라인이 중앙 정렬로 표시되는지 확인.

- [ ] **Step 4: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): header block with subtitle and philosophy line"
```

---

## Task 3: 환경 브리핑 블록 + 배지 범례

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 환경 블록 CSS 추가**

```css
.env-panel { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r-lg); padding: 24px; margin-bottom: 36px; box-shadow: var(--sh-sm); }
.env-panel h2 { margin-top: 0; }
.env-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 16px; }
.env-cell { background: var(--surface-hover); border-radius: var(--r-md); padding: 16px; }
.env-cell-head { font-size: 13px; font-weight: 700; color: var(--text-3); text-transform: uppercase; letter-spacing: 0.04em; margin-bottom: 8px; }
.env-cell-body { font-size: 14px; color: var(--text-2); line-height: 1.55; }
.env-cell-body strong { color: var(--text-1); }

.badge-legend { margin-top: 24px; padding-top: 20px; border-top: 1px dashed var(--border-light); display: flex; flex-wrap: wrap; gap: 14px; }
.legend-item { display: inline-flex; align-items: center; gap: 6px; font-size: 13px; color: var(--text-2); }
.b-pill { display: inline-block; padding: 2px 8px; border-radius: 999px; font-size: 12px; font-weight: 700; }
.b-verified { background: var(--green-soft); color: var(--green); }
.b-paper { background: #eef0f4; color: var(--text-2); }
.b-goal { background: var(--orange-soft); color: var(--orange); }

@media (max-width: 640px) {
    .env-grid { grid-template-columns: 1fr; }
}
```

- [ ] **Step 2: 환경 마크업**

헤더 아래:

```html
<section class="env-panel">
    <h2>환경 브리핑</h2>
    <p>이 플로우의 전제가 되는 공간·센서 배치. Yu 2025(독거노인 FMCW 낙상 탐지) 연구 환경과 정합.</p>
    <div class="env-grid">
        <div class="env-cell">
            <div class="env-cell-head">공간</div>
            <div class="env-cell-body"><strong>1인 독실</strong> (요양시설 또는 자택). 타 환자 간섭 최소, 센서 간섭 적음.</div>
        </div>
        <div class="env-cell">
            <div class="env-cell-head">센서 배치</div>
            <div class="env-cell-body">WiFi AP 1대 · mmWave FMCW 레이더 1대 · 간호 스테이션 대시보드</div>
        </div>
        <div class="env-cell">
            <div class="env-cell-head">CSI 역할</div>
            <div class="env-cell-body"><strong>연속 생체·재실·보행 리듬</strong> (방 전체 커버, 저해상도 지속 측정)</div>
        </div>
        <div class="env-cell">
            <div class="env-cell-head">mmWave 역할</div>
            <div class="env-cell-body"><strong>정밀 동작·속도·자세·낙상 궤적</strong> (4m 이내 고해상도)</div>
        </div>
    </div>
    <div class="badge-legend">
        <div class="legend-item"><span class="b-pill b-verified">✅ 검증됨</span> 로컬 PDF / 공식 표준에서 직접 확인</div>
        <div class="legend-item"><span class="b-pill b-paper">📖 논문 범위</span> 인용 논문의 값 (우리 환경 적용 여부 미지수)</div>
        <div class="legend-item"><span class="b-pill b-goal">🎯 설계 목표</span> 달성하고자 하는 목표치</div>
    </div>
</section>
```

- [ ] **Step 3: 브라우저 확인**

4개 셀이 2×2 그리드로 표시, 범례가 아래 flex로 나열되는지. 모바일(창 너비 < 640px) 시 1열로 붕괴하는지.

- [ ] **Step 4: Reviewer checkpoint**

`superpowers:code-reviewer` 에이전트 호출. 프롬프트 예시:

> user-flow.html 의 Task 1–3 단계(골격·헤더·환경 브리핑) 코드를 리뷰. 체크 항목: (1) presentation.html CSS 변수와 충돌 없는지, (2) 마크업 시맨틱 적절한지, (3) 모바일 반응형 깨짐 없는지, (4) 배지 범례 색상 대비 접근성. 200단어 이내 리포트.

리뷰 피드백 반영 후 다음 단계로.

- [ ] **Step 5: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): environment briefing + badge legend"
```

---

## Task 4: 타임라인 앵커 네비

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 네비 CSS 추가**

```css
.timeline-nav { display: flex; justify-content: center; gap: 12px; flex-wrap: wrap; margin-bottom: 32px; padding: 16px; background: var(--surface); border: 1px solid var(--border); border-radius: var(--r-lg); box-shadow: var(--sh-sm); }
.tl-link { display: inline-flex; align-items: center; gap: 6px; padding: 10px 16px; border-radius: var(--r-md); text-decoration: none; font-size: 14px; font-weight: 600; color: var(--text-2); background: var(--surface-hover); transition: all 0.2s var(--ease); }
.tl-link:hover { background: var(--accent-soft); color: var(--accent); }
.tl-link .tl-icon { font-size: 18px; }
.tl-sep { color: var(--text-3); align-self: center; font-size: 14px; }
```

- [ ] **Step 2: 네비 마크업**

```html
<nav class="timeline-nav" aria-label="하루 시나리오 타임라인">
    <a href="#scene-morning" class="tl-link"><span class="tl-icon">🌅</span> 07:15 기상</a>
    <span class="tl-sep">→</span>
    <a href="#scene-afternoon" class="tl-link"><span class="tl-icon">☕</span> 14:30 오후</a>
    <span class="tl-sep">→</span>
    <a href="#scene-dawn" class="tl-link"><span class="tl-icon">🌙</span> 03:42 새벽</a>
</nav>
```

- [ ] **Step 3: 브라우저 확인**

링크 클릭 시 (다음 Task에서 타겟 섹션 생성 후 재확인).

- [ ] **Step 4: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): timeline anchor nav"
```

---

## Task 5: 장면 카드 공통 컴포넌트 CSS

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 장면 카드 CSS 추가**

```css
.scene { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r-lg); padding: 0; margin-bottom: 24px; box-shadow: var(--sh-sm); overflow: hidden; }
.scene-head { padding: 20px 24px; border-bottom: 1px solid var(--border-light); }
.scene-title { font-size: 19px; font-weight: 800; letter-spacing: -0.01em; margin-bottom: 6px; }
.scene-summary { font-size: 14px; color: var(--text-2); }

/* scene color accents */
.scene-morning .scene-head { background: linear-gradient(180deg, var(--amber-soft), transparent); border-left: 4px solid var(--amber); }
.scene-afternoon .scene-head { background: linear-gradient(180deg, var(--green-soft), transparent); border-left: 4px solid var(--green); }
.scene-dawn .scene-head { background: linear-gradient(180deg, var(--purple-soft), transparent); border-left: 4px solid var(--purple); }

.scene-grid { display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: auto auto; gap: 0; }
.scene-cell { padding: 18px 24px; border-top: 1px solid var(--border-light); }
.scene-cell.top-row { border-top: none; }
.scene-cell.right { border-left: 1px solid var(--border-light); }

.cell-head { display: flex; align-items: center; gap: 8px; font-size: 13px; font-weight: 700; color: var(--text-3); text-transform: uppercase; letter-spacing: 0.04em; margin-bottom: 12px; }
.cell-icon { font-size: 16px; }
.cell-tag { font-size: 11px; font-weight: 600; color: var(--text-3); background: var(--surface-hover); padding: 2px 6px; border-radius: 4px; }

.patient-story { font-size: 14.5px; line-height: 1.65; color: var(--text-1); }
.operator-body { font-size: 14px; color: var(--text-2); line-height: 1.55; }
.operator-body strong { color: var(--text-1); }
.alert-level { display: inline-block; padding: 4px 10px; border-radius: 999px; font-size: 12px; font-weight: 700; margin-bottom: 8px; }
.alert-0 { background: var(--green-soft); color: var(--green); }
.alert-1 { background: var(--amber-soft); color: var(--amber); }
.alert-2 { background: var(--rose-soft); color: var(--rose); }

/* sensor table */
.sensor-table { width: 100%; border-collapse: collapse; margin-top: 4px; font-size: 13.5px; }
.sensor-table th { text-align: left; font-weight: 700; color: var(--text-3); font-size: 12px; text-transform: uppercase; letter-spacing: 0.03em; padding: 6px 8px 6px 0; border-bottom: 1px solid var(--border-light); }
.sensor-table td { padding: 10px 8px 10px 0; vertical-align: top; border-bottom: 1px solid var(--border-light); color: var(--text-2); line-height: 1.5; }
.sensor-table tr:last-child td { border-bottom: none; }
.sensor-table .col-item { font-weight: 600; color: var(--text-1); width: 22%; }
.sensor-table .col-system { width: 43%; }
.sensor-table .col-value { width: 20%; font-family: 'SF Mono', Consolas, monospace; font-size: 12.5px; color: var(--text-2); }
.sensor-table .col-source { width: 15%; font-size: 12px; }
.illustrative { font-size: 10px; color: var(--text-3); margin-left: 4px; }

/* csi/mmwave color tagging */
.tag-csi { color: var(--blue); }
.tag-mmwave { color: var(--green); }

/* transition message */
.scene-transition { text-align: center; font-size: 13px; color: var(--text-3); margin: -8px 0 24px; padding: 8px 0; position: relative; }
.scene-transition::before { content: '↓'; display: block; font-size: 18px; margin-bottom: 4px; color: var(--text-3); }

/* responsive */
@media (max-width: 760px) {
    .scene-grid { grid-template-columns: 1fr; }
    .scene-cell.right { border-left: none; border-top: 1px solid var(--border-light); }
    .sensor-table .col-item { width: 35%; }
    .sensor-table .col-value { display: none; }
}
```

- [ ] **Step 2: CSS 문법 체크**

브라우저 개발자 도구(F12) → Console 탭에서 CSS 경고 없는지 확인.

- [ ] **Step 3: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): scene card CSS (2x2 grid, responsive)"
```

---

## Task 6: 🌅 07:15 기상 시나리오 카드

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 아침 시나리오 마크업**

```html
<section id="scene-morning" class="scene scene-morning">
    <div class="scene-head">
        <div class="scene-title">🌅 07:15 · 기상 baseline 측정</div>
        <div class="scene-summary">매일 반복되는 정상 루틴 — 생체·보행 지표 누적, 7일 추세 업데이트</div>
    </div>
    <div class="scene-grid">
        <div class="scene-cell top-row">
            <div class="cell-head"><span class="cell-icon">👤</span> 환자 관점</div>
            <div class="patient-story">
                수면 중이던 할머니가 천천히 몸을 일으켜 침대 가장자리에 앉음 → 기립 → 화장실 방향 이동.
            </div>
        </div>
        <div class="scene-cell top-row right">
            <div class="cell-head tag-csi"><span class="cell-icon">📡</span> CSI <span class="cell-tag">WiFi 채널정보</span></div>
            <table class="sensor-table">
                <thead><tr><th class="col-item">감지</th><th class="col-system">시스템 (파이프라인)</th><th class="col-value">예시값</th><th class="col-source">근거</th></tr></thead>
                <tbody>
                    <tr><td class="col-item">호흡</td><td>Amplitude → Hampel 필터 → 서브캐리어 PCA → Band-pass 0.2–0.4Hz → FFT 피크</td><td>0.25Hz<span class="illustrative">(15회/분)</span></td><td>의학 표준 <span class="b-pill b-verified">✅</span></td></tr>
                    <tr><td class="col-item">심박</td><td>Phase → 고주파 Band-pass 0.8–2Hz → HRV 추정 (정지 자세)</td><td>1.1Hz<span class="illustrative">(66bpm)</span></td><td>의학 표준 <span class="b-pill b-verified">✅</span></td></tr>
                    <tr><td class="col-item">재실·기상</td><td>Amplitude variance 임계값 이벤트</td><td>energy ↑</td><td>일반 CSI 문헌 <span class="b-pill b-paper">📖</span></td></tr>
                    <tr><td class="col-item">보행 cadence</td><td>Amplitude 시계열 주기 검출 → step count / time</td><td>0.85Hz<span class="illustrative">(51스텝/분)</span></td><td>Wang 2016 <span class="b-pill b-verified">✅</span></td></tr>
                </tbody>
            </table>
        </div>
        <div class="scene-cell">
            <div class="cell-head tag-mmwave"><span class="cell-icon">📡</span> mmWave <span class="cell-tag">FMCW 60–64GHz</span></div>
            <table class="sensor-table">
                <thead><tr><th class="col-item">감지</th><th class="col-system">시스템 (파이프라인)</th><th class="col-value">예시값</th><th class="col-source">근거</th></tr></thead>
                <tbody>
                    <tr><td class="col-item">기립 시퀀스</td><td>Point Cloud 수직 분포 클러스터링 → 자세 변화 추적</td><td>상체 분리 → 직립</td><td>Yu 2025 <span class="b-pill b-paper">📖</span></td></tr>
                    <tr><td class="col-item">보행 속도 <em>(메인)</em></td><td>Point Cloud → torso centroid 추적 → 시간 미분 + Doppler 평균</td><td>0.78 m/s</td><td>Zeng 2022 96% <span class="b-pill b-verified">✅</span></td></tr>
                    <tr><td class="col-item">보폭</td><td>Foot cluster 검출 → heel-strike 타이밍</td><td>0.48m</td><td>Zeng 2022 <span class="b-pill b-paper">📖</span></td></tr>
                </tbody>
            </table>
        </div>
        <div class="scene-cell right">
            <div class="cell-head"><span class="cell-icon">🖥️</span> 간호 스테이션</div>
            <div class="operator-body">
                <span class="alert-level alert-0">알림 레벨 0</span>
                <p>일일 카드: <strong>오늘 보행 속도 0.78m/s, 7일 평균 대비 −4%</strong> (관찰 대상). 영상 없음, 수치·추세만.</p>
            </div>
        </div>
    </div>
</section>
<div class="scene-transition">오늘 baseline 확보 완료</div>
```

- [ ] **Step 2: 브라우저 확인**

- 카드가 2×2 그리드로 렌더링
- 테이블이 4열로 정렬 (감지/시스템/예시값/근거)
- 모바일 폭(<760px)에서 1열로 붕괴 + 예시값 열 숨김
- 배지 색상 정상

- [ ] **Step 3: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): scene 1 - morning baseline scenario"
```

---

## Task 7: ☕ 14:30 오후 시나리오 카드 + Reviewer checkpoint

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 오후 시나리오 마크업**

아침 카드 바로 아래 (transition 다음):

```html
<section id="scene-afternoon" class="scene scene-afternoon">
    <div class="scene-head">
        <div class="scene-title">☕ 14:30 · 오후 (오탐 회피)</div>
        <div class="scene-summary">일상 활동 — 시스템이 "정상"으로 판정, 알림 없음. 단일 센서 오탐 문제 해결 시연</div>
    </div>
    <div class="scene-grid">
        <div class="scene-cell top-row">
            <div class="cell-head"><span class="cell-icon">👤</span> 환자 관점</div>
            <div class="patient-story">
                물을 마시러 일어나 거실로 이동, 화장실 다녀와 다시 앉음.
            </div>
        </div>
        <div class="scene-cell top-row right">
            <div class="cell-head tag-csi"><span class="cell-icon">📡</span> CSI</div>
            <table class="sensor-table">
                <thead><tr><th class="col-item">감지</th><th class="col-system">시스템</th><th class="col-value">예시값</th><th class="col-source">근거</th></tr></thead>
                <tbody>
                    <tr><td class="col-item">호흡·심박</td><td>동일 파이프라인 상시 가동</td><td>0.28Hz / 1.2Hz<span class="illustrative">(72bpm)</span></td><td>의학 표준 <span class="b-pill b-verified">✅</span></td></tr>
                    <tr><td class="col-item">보행 리듬 일관성</td><td>Amplitude 주기 시계열 → 변동계수(CV) 계산</td><td>CV 약 6%</td><td>Wang 2016 기반 <span class="b-pill b-paper">📖</span></td></tr>
                </tbody>
            </table>
        </div>
        <div class="scene-cell">
            <div class="cell-head tag-mmwave"><span class="cell-icon">📡</span> mmWave</div>
            <table class="sensor-table">
                <thead><tr><th class="col-item">감지</th><th class="col-system">시스템</th><th class="col-value">예시값</th><th class="col-source">근거</th></tr></thead>
                <tbody>
                    <tr><td class="col-item">자세 유지</td><td>Point Cloud 수직 분포 → 직립 상태 지속</td><td>직립 100%</td><td>Yu 2025 <span class="b-pill b-paper">📖</span></td></tr>
                    <tr><td class="col-item">걸음 간격</td><td>Heel-strike 타이밍 분산 측정</td><td>일정 (정상 범위)</td><td>Zeng 2022 <span class="b-pill b-paper">📖</span></td></tr>
                    <tr><td class="col-item">낙상 signature</td><td>Point Cloud 급변 + Micro-Doppler pattern</td><td>없음</td><td>Yu 2025 <span class="b-pill b-verified">✅</span></td></tr>
                </tbody>
            </table>
        </div>
        <div class="scene-cell right">
            <div class="cell-head"><span class="cell-icon">🖥️</span> 간호 스테이션</div>
            <div class="operator-body">
                <span class="alert-level alert-0">알림 없음</span>
                <p>활동 로그만 기록, <strong>정상 활동 패턴으로 분류</strong>. → <em>False Positive 회피 증명</em> (단일 센서 연구의 오탐 문제 해결).</p>
            </div>
        </div>
    </div>
</section>
<div class="scene-transition">정상 활동 기록 계속 누적</div>
```

- [ ] **Step 2: Reviewer checkpoint (Task 1–7 전체 중간 리뷰)**

`superpowers:code-reviewer` 호출. 프롬프트:

> user-flow.html의 Task 1–7까지 코드(골격·헤더·환경·네비·장면 카드 컴포넌트·아침+오후 시나리오)를 리뷰. 체크:
> (1) 마크업 시맨틱·접근성 (heading 레벨, aria-label, table semantics)
> (2) CSS grid 정합성 — 2×2 레이아웃이 제대로 잡히는지, 반응형 브레이크포인트 깨짐 없는지
> (3) spec에 있는 파이프라인 텍스트가 시나리오 표와 1:1 매칭되는지 — spec: docs/superpowers/specs/2026-04-17-user-flow-page-design.md
> (4) 배지 사용 일관성 — ✅/📖/🎯 선택이 spec 정책과 맞는지
> 300단어 이내 리포트.

리뷰 피드백 반영 후 다음 단계로.

- [ ] **Step 3: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): scene 2 - afternoon false-positive avoidance"
```

---

## Task 8: 🌙 03:42 새벽 시나리오 + 교차검증 박스

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 교차검증 박스 CSS 추가**

```css
.crossval-box { background: var(--rose-soft); border: 1px solid var(--rose); border-radius: var(--r-md); padding: 14px 16px; margin-top: 16px; }
.crossval-head { font-size: 13px; font-weight: 800; color: var(--rose); margin-bottom: 8px; text-transform: uppercase; letter-spacing: 0.03em; }
.crossval-table { width: 100%; font-size: 13px; }
.crossval-table td { padding: 4px 0; vertical-align: top; }
.crossval-table .cv-label { font-weight: 700; color: var(--text-1); width: 30%; }
.crossval-result { margin-top: 10px; padding: 8px 12px; background: var(--surface); border-radius: var(--r-sm); font-size: 13px; color: var(--text-1); font-weight: 600; }
```

- [ ] **Step 2: 새벽 시나리오 마크업**

```html
<section id="scene-dawn" class="scene scene-dawn">
    <div class="scene-head">
        <div class="scene-title">🌙 03:42 · 새벽 (사전 예측 트리거) — 핵심 시나리오</div>
        <div class="scene-summary">수일간 누적된 보행 악화 + 지금 이 순간 생체·자세 이상 → 낙상 발생 <strong>전</strong> 알림</div>
    </div>
    <div class="scene-grid">
        <div class="scene-cell top-row">
            <div class="cell-head"><span class="cell-icon">👤</span> 환자 관점</div>
            <div class="patient-story">
                침대 가장자리에서 2분간 뒤척임 → 불안정 기립 시도 (졸음 + 방향 혼란, 이중과제 상황).
            </div>
        </div>
        <div class="scene-cell top-row right">
            <div class="cell-head tag-csi"><span class="cell-icon">📡</span> CSI</div>
            <table class="sensor-table">
                <thead><tr><th class="col-item">감지</th><th class="col-system">시스템</th><th class="col-value">예시값</th><th class="col-source">근거</th></tr></thead>
                <tbody>
                    <tr><td class="col-item">호흡 불규칙</td><td>Band-pass 0.2–0.4Hz 출력의 시간창 분산·변동계수 계산</td><td>0.18–0.32Hz<span class="illustrative">(CV 32%)</span></td><td>의학 표준 + 자체 임계 <span class="b-pill b-paper">📖</span></td></tr>
                    <tr><td class="col-item">심박 상승</td><td>HRV 추정 + 평상시 baseline 대비 편차</td><td>1.5Hz<span class="illustrative">(90bpm ↑)</span></td><td>의학 표준 <span class="b-pill b-verified">✅</span></td></tr>
                    <tr><td class="col-item">미세 움직임 이상</td><td>Amplitude energy 이상 패턴 지속 시간 검출</td><td>30초 지속</td><td>자체 임계 <span class="b-pill b-paper">📖</span></td></tr>
                </tbody>
            </table>
        </div>
        <div class="scene-cell">
            <div class="cell-head tag-mmwave"><span class="cell-icon">📡</span> mmWave <span class="cell-tag">메인 예측 축</span></div>
            <table class="sensor-table">
                <thead><tr><th class="col-item">감지</th><th class="col-system">시스템</th><th class="col-value">예시값</th><th class="col-source">근거</th></tr></thead>
                <tbody>
                    <tr><td class="col-item">부분 기립 시도</td><td>Point Cloud 상·하체 분리 클러스터링 → 움직임 불일치</td><td>상체 움직임 / 하체 침상</td><td>Yu 2025 구조 <span class="b-pill b-paper">📖</span></td></tr>
                    <tr><td class="col-item">미세 속도</td><td>Doppler 저속 벡터 추출</td><td>0.08 m/s</td><td>Zeng 2022 기반 <span class="b-pill b-paper">📖</span></td></tr>
                    <tr><td class="col-item"><strong>STV 장기 추세</strong></td><td>72시간 누적 cadence 시계열 → stride time 변동성 계산</td><td>변동성 상승 추세</td><td>유아현 2025 메타분석 <span class="b-pill b-verified">✅</span></td></tr>
                </tbody>
            </table>
        </div>
        <div class="scene-cell right">
            <div class="cell-head"><span class="cell-icon">🖥️</span> 간호 스테이션</div>
            <div class="operator-body">
                <span class="alert-level alert-2">알림 레벨 2 — 낙상 위험</span>
                <p>병실·시각·근거 표시 (영상 없이 수치·추세). "<strong>지난 3일 보행 속도 −8%, 지금 호흡 변동 ↑</strong>" 설명.</p>
                <div class="crossval-box">
                    <div class="crossval-head">🔀 교차검증 AND</div>
                    <table class="crossval-table">
                        <tr><td class="cv-label">CSI 판정</td><td>생체·보행 리듬 "이상" 신뢰도 ≥ 0.8 <span class="b-pill b-goal">🎯</span></td></tr>
                        <tr><td class="cv-label">mmWave 판정</td><td>자세·속도·STV "이상" 신뢰도 ≥ 0.8 <span class="b-pill b-goal">🎯</span></td></tr>
                    </table>
                    <div class="crossval-result">AND 충족 → <strong>확정 알림</strong> / 한쪽만 이상 → "관찰 레벨"로 격하</div>
                </div>
            </div>
        </div>
    </div>
</section>
<div class="scene-transition">3시점 데이터가 시계열 모델로 합쳐져 위험도 산출</div>
```

- [ ] **Step 2: 브라우저 확인**

- 교차검증 박스가 운영자 셀 내부 하단에 표시
- 알림 레벨 2 pill 색상 (rose)
- STV 추세 항목 강조 처리 확인

- [ ] **Step 3: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): scene 3 - dawn pre-prediction with AND crossval"
```

---

## Task 9: 종합 블록 (시계열 모델 + 평가지표)

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 종합 블록 CSS 추가**

```css
.synthesis { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r-lg); padding: 28px; margin-top: 12px; margin-bottom: 36px; box-shadow: var(--sh-sm); }
.syn-grid { display: grid; grid-template-columns: 1.1fr 1fr; gap: 24px; margin-top: 16px; }
.syn-box { background: var(--surface-hover); border-radius: var(--r-md); padding: 18px; }
.syn-box h3 { font-size: 15px; margin-bottom: 10px; }
.syn-list { list-style: none; padding: 0; }
.syn-list li { padding: 8px 0; font-size: 13.5px; color: var(--text-2); line-height: 1.55; border-bottom: 1px dashed var(--border-light); }
.syn-list li:last-child { border-bottom: none; }
.syn-list li strong { color: var(--text-1); }

@media (max-width: 760px) { .syn-grid { grid-template-columns: 1fr; } }
```

- [ ] **Step 2: 종합 블록 마크업**

```html
<section class="synthesis">
    <h2>종합 — 3시점 데이터가 예측 모델로 합쳐지는 흐름</h2>
    <p>각 시점에서 뽑힌 CSI·mmWave 지표가 시계열 feature로 누적되고, 모델이 향후 낙상 위험도를 산출한다.</p>
    <div class="syn-grid">
        <div class="syn-box">
            <h3>🧠 예측 모델 설계</h3>
            <ul class="syn-list">
                <li><strong>입력</strong>: CSI 생체·리듬 (연속) + mmWave 속도·STV·자세 (이벤트성) + 7–30일 추세</li>
                <li><strong>비교 후보</strong>: LSTM / Transformer (시계열 딥러닝) vs XGBoost / Random Forest (전통 ML)</li>
                <li><strong>판정 시점</strong>: 즉각(초) / 단기(시간) / 장기(일) 3단계 동시 출력</li>
                <li><strong>교차검증</strong>: AND 조건으로 단일 센서 오탐 배제 (새벽 시나리오 참조)</li>
            </ul>
        </div>
        <div class="syn-box">
            <h3>📊 평가 지표 & 설계 목표</h3>
            <ul class="syn-list">
                <li><strong>AUROC</strong>: 예측 성능 — 모델 선택 기준</li>
                <li><strong>F1-score</strong>: 탐지 balance — 목표 <strong>&gt; 0.90</strong> <span class="b-pill b-goal">🎯</span></li>
                <li><strong>알림 선행 시간</strong>: 낙상 발생 대비 사전 경고 확보 시간 — 목표 <strong>≥ 30초</strong> <span class="b-pill b-goal">🎯</span></li>
                <li><strong>오탐률</strong>: 일상 활동 오판 빈도 — 목표 <strong>일 1건 이하</strong> <span class="b-pill b-goal">🎯</span></li>
            </ul>
        </div>
    </div>
</section>
```

- [ ] **Step 3: 브라우저 확인**

2열 그리드, 모바일에서 1열 붕괴. 각 항목에 🎯 배지 정상.

- [ ] **Step 4: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): synthesis block - prediction model + metrics"
```

---

## Task 10: 참고문헌 섹션 + Reviewer checkpoint

**Files:**
- Modify: `paper-review/user-flow.html`

- [ ] **Step 1: 참고문헌 CSS 추가**

```css
.refs { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r-lg); padding: 24px; margin-bottom: 32px; box-shadow: var(--sh-sm); }
.refs h2 { margin-top: 0; font-size: 18px; }
.refs-list { list-style: none; padding: 0; font-size: 13.5px; line-height: 1.7; color: var(--text-2); }
.refs-list li { padding: 8px 0; border-bottom: 1px dashed var(--border-light); }
.refs-list li:last-child { border-bottom: none; }
.refs-list a { color: var(--accent); text-decoration: none; font-weight: 500; }
.refs-list a:hover { text-decoration: underline; }
.refs-note { margin-top: 14px; padding: 12px 14px; background: var(--orange-soft); border-left: 3px solid var(--orange); border-radius: var(--r-sm); font-size: 13px; color: var(--text-2); }
.refs-note strong { color: var(--orange); }
```

- [ ] **Step 2: 참고문헌 마크업**

```html
<section class="refs">
    <h2>참고문헌</h2>
    <ul class="refs-list">
        <li><strong>Yu et al. (2025)</strong> — LSTM-based Fall Detection by FMCW Millimeter-Wave Radar Sensor for Seniors Living Alone. <em>Applied Sciences 15, 8381.</em> <a href="https://doi.org/10.3390/app15158381" target="_blank">DOI</a></li>
        <li><strong>Zeng et al. (2022)</strong> — mmWave 보행 간격 측정 96% 정확도. <a href="https://doi.org/10.3390/s22249901" target="_blank">DOI</a></li>
        <li><strong>Wang et al. (2016)</strong> — CSI 기반 보행 속도·리듬 측정, 50명 검증, 오차 11.9ms. <a href="https://doi.org/10.1145/2971648.2971670" target="_blank">DOI</a></li>
        <li><strong>유아현 외 (2025)</strong> — 인지장애 노인 보행 분석·낙상 위험 메타분석 (MD 0.12 / 0.21). <a href="../pdf/유아현2025_보행분석_인지장애노인_낙상위험_메타분석.pdf" target="_blank">PDF</a></li>
        <li><strong>Minta et al. (2023)</strong> — Differences in fall-related characteristics across cognitive disorders. <em>Front Aging Neurosci.</em> <a href="https://doi.org/10.3389/fnagi.2023.1171306" target="_blank">DOI</a></li>
        <li><strong>임정옥 (2016)</strong> — 요양병원 치매 환자 낙상 특성 (54.9% 보행 중, OR 11.68). <a href="https://www.jkgn.org/journal/view.php?viewtype=pubreader&number=368" target="_blank">링크</a></li>
    </ul>
    <div class="refs-note">
        <strong>⚠ 보류:</strong> 의학 표준 호흡 0.2–0.4Hz / 심박 0.8–2Hz 범위의 임상 가이드라인 인용 논문(AHA, NEJM 등) 미확정. 구현 최종 단계에서 확정 후 추가 예정.
    </div>
</section>
```

- [ ] **Step 3: Reviewer checkpoint (근거 정합성 전수 체크)**

`superpowers:code-reviewer` 호출. 프롬프트:

> user-flow.html 전체(Task 1–10 완료 상태)를 리뷰. 최우선 체크:
> (1) **배지 정합성**: 모든 수치 옆에 ✅/📖/🎯 배지가 붙어 있는지, 배지 없는 숫자 0건이어야 함
> (2) **근거 링크 유효성**: 참고문헌 섹션의 DOI·PDF 경로가 presentation.html과 일치하는지
> (3) **spec 대조**: docs/superpowers/specs/2026-04-17-user-flow-page-design.md 의 Section 4(시나리오) 내용이 빠짐없이 반영됐는지
> (4) **수치 환각 리스크**: 장면 카드 예시값(0.78m/s, 0.25Hz 등)이 모두 spec의 verified 범위 내에 있는지
> 배지 누락 발견 시 **빌드 차단 신호**로 보고.
> 400단어 이내.

리뷰 지적사항 반영 후 진행.

- [ ] **Step 4: Commit**

```bash
git add paper-review/user-flow.html
git commit -m "feat(user-flow): references section with deferred-item note"
```

---

## Task 11: 푸터 + presentation.html 연결

**Files:**
- Modify: `paper-review/user-flow.html`
- Modify: `paper-review/presentation.html` (1줄만)

- [ ] **Step 1: user-flow.html 푸터 추가**

```html
<footer style="text-align:center;margin-top:40px;padding-top:24px;border-top:1px solid var(--border-light);font-size:13px;color:var(--text-3);">
    ← <a href="presentation.html" style="color:var(--accent);text-decoration:none;font-weight:600;">발표모드로 돌아가기</a>
    <span style="margin:0 10px;">·</span>
    <a href="table-visual.html" style="color:var(--text-2);text-decoration:none;">카드 뷰</a>
</footer>
```

`</div>` (container 닫기) 바로 위에 삽입.

- [ ] **Step 2: presentation.html 푸터 링크 추가**

presentation.html 파일의 현재 푸터(932줄 근처) 확인:

```bash
grep -n "카드 뷰로 돌아가기" paper-review/presentation.html
```

찾은 줄 수정 — 기존:
```html
<p class="foot">← <a href="table-visual.html" class="c-accent" style="text-decoration:none;font-weight:600">카드 뷰로 돌아가기</a></p>
```

다음으로 교체:
```html
<p class="foot">← <a href="table-visual.html" class="c-accent" style="text-decoration:none;font-weight:600">카드 뷰로 돌아가기</a> <span style="margin:0 8px;color:var(--text-3);">·</span> <a href="user-flow.html" class="c-accent" style="text-decoration:none;font-weight:600">사용자 플로우 보기 →</a></p>
```

- [ ] **Step 3: 양방향 연결 브라우저 확인**

presentation.html → user-flow.html → 돌아오기 테스트.

- [ ] **Step 4: Commit**

```bash
git add paper-review/user-flow.html paper-review/presentation.html
git commit -m "feat(user-flow): footer + bidirectional link with presentation"
```

---

## Task 12: 모바일 반응형 QA

**Files:**
- Modify: `paper-review/user-flow.html` (필요 시 CSS 조정만)

- [ ] **Step 1: 브라우저 개발자 도구 디바이스 시뮬레이션**

F12 → Toggle device toolbar. 다음 뷰포트에서 체크:

- iPhone SE (375×667)
- iPhone 14 Pro (393×852)
- 태블릿 (768×1024)
- 데스크톱 (1440×900)

체크 항목:
- 환경 브리핑 4셀 → 모바일 1열 붕괴 ✓
- 장면 카드 2×2 → 모바일 1×4 세로 붕괴 ✓
- 센서 테이블 예시값 열 모바일 숨김 ✓
- 타임라인 네비 wrap 정상 ✓
- 헤더 폰트 크기 과도하지 않음 ✓

- [ ] **Step 2: 한글 렌더링 확인 (d35e90a 커밋 교훈)**

모바일에서 특수문자·이모지·한글 조합 깨짐 없는지 (특히 Pretendard 폴백 경로). 필요 시 font-family에 fallback 추가.

- [ ] **Step 3: 깨지는 곳 발견 시 CSS 미세 조정**

예시 조정 (브레이크포인트 추가):
```css
@media (max-width: 480px) {
    h1 { font-size: 22px; }
    .scene-title { font-size: 17px; }
    .sensor-table { font-size: 12.5px; }
}
```

- [ ] **Step 4: Commit (변경이 있을 때만)**

```bash
git add paper-review/user-flow.html
git commit -m "fix(user-flow): mobile responsive adjustments"
```

---

## Task 13: 최종 Reviewer checkpoint + 배지·수치 전수 검증

**Files:** (코드 수정 없음 — 검증 단계)

- [ ] **Step 1: `superpowers:code-reviewer` 최종 호출**

프롬프트:

> user-flow.html 최종 버전을 총괄 리뷰 (구현 완료 상태). 최종 체크리스트:
> (1) **배지 0 누락**: 모든 수치·범위에 ✅/📖/🎯 배지 부착 확인
> (2) **spec 100% 커버**: docs/superpowers/specs/2026-04-17-user-flow-page-design.md Section 1–7의 모든 요구사항이 반영됐는지
> (3) **환각 수치 0건**: 모든 값이 인용 논문 또는 의학 표준 범위 내인지 (presentation.html의 기존 검증값과 교차 대조)
> (4) **양방향 링크**: presentation.html ↔ user-flow.html 작동
> (5) **접근성**: heading 레벨 연속성, aria-label, 색상 대비 (배지·알림 레벨)
> (6) **모바일 반응형**: 2개 이상 뷰포트에서 붕괴 없음 확인
> 빌드 차단 신호가 필요하면 명시. 500단어 이내.

- [ ] **Step 2: 리뷰 지적사항 반영 (있을 경우)**

각 지적 항목을 별도 commit으로 나눠 수정:
```bash
git add paper-review/user-flow.html
git commit -m "fix(user-flow): address reviewer finding - [구체 내용]"
```

- [ ] **Step 3: 최종 검증 완료 표시**

리뷰어가 "차단 신호 없음" 판정 시, fix-list.md 또는 verification-report.md 업데이트 필요한지 확인:

```bash
grep -l "user-flow" paper-review/*.md 2>/dev/null
```

- [ ] **Step 4: 완료 커밋 (필요 시 빈 마커 commit)**

```bash
git commit --allow-empty -m "chore(user-flow): final review passed, page ready for presentation"
```

---

## Self-Review Checklist (계획 자체 검토)

- [x] **Spec coverage**:
  - Section 1 목적·표기 철학 → Task 2 (헤더 배지 라인)
  - Section 2 파일·네비 → Task 1, 11
  - Section 3 페이지 아키텍처 (장면 카드) → Task 5–8
  - Section 4 시나리오 3개 → Task 6, 7, 8
  - Section 5 배지 정책 → Task 3 (범례) + 전 타스크 배지 부착
  - Section 6 스타일 → Task 1 (CSS 변수), Task 5 (색 포인트)
  - Section 7 구현·검증 → Task 7, 10, 13 (Reviewer checkpoint 3회)
  - Section 8 보류 항목 (의학 표준 출처) → Task 10 refs-note
- [x] **Placeholder scan**: 모든 code 블록에 실제 코드 포함, "TBD"·"TODO" 없음
- [x] **Type consistency**: CSS 클래스 이름(`scene-grid`, `scene-cell`, `sensor-table`, `b-pill` 등) Task 5 정의 후 Task 6–10 일관 사용
- [x] **Reviewer cadence**: 사용자 요청대로 Task 3, 7, 10, 13에 code-reviewer 명시 호출 포함
