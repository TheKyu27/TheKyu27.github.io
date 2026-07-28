# TheKyu27.github.io

Teokkyu Suh의 학술 CV 페이지. 빌드 도구 없는 정적 HTML/CSS이며, `main` 브랜치에 push하면 GitHub Pages가 그대로 서빙합니다.

**https://thekyu27.github.io**

## 구조

| 파일 | 역할 |
| --- | --- |
| `index.html` | CV 내용 전부 (학력·논문·특허·연구분야·활동) |
| `style.css` | 디자인. 상단 `:root` 변수로 색/간격 조절 |
| `assets/profile.png` | 프로필 사진. 정사각형이 아니어도 CSS가 가운데를 잘라냅니다 |
| `.nojekyll` | GitHub Pages의 Jekyll 처리를 끔 |

## 로컬에서 보기

```sh
open index.html
```

파일을 저장하고 브라우저를 새로고침하면 끝입니다. 서버가 필요하면:

```sh
python3 -m http.server 8000   # http://localhost:8000
```

## 내용 수정하기

`index.html`에서 원하는 부분을 고치면 됩니다. 반복되는 패턴은 세 가지뿐입니다.

**논문·특허** — `<ol class="pubs">` 안에 이 블록을 복사해서 늘리면 됩니다. 왼쪽 게터(`.venue`)에 학회명, 오른쪽에 제목과 저자가 들어갑니다.

```html
<li>
  <p class="venue">ISCA ’24<span class="cat arch">Architecture</span></p>
  <div>
    <p class="pub-title">
      Paper Title Goes Here
      <span class="tag highlight">Highlight Paper</span>   <!-- 필요할 때만 -->
    </p>
    <p class="authors">
      First Author, <span class="me">Teokkyu Suh</span>, Last Author
    </p>
  </div>
</li>
```

- `<span class="me">` — 저자 목록에서 본인 이름. 진하게 표시됩니다 (학술 CV의 표준 관행)
- `<span class="cat circuit">` / `<span class="cat arch">` — 학회 성격 분류. 각각 청록/보라로 색이 구분됩니다
- `<span class="tag">` — Under Review, Accepted 같은 중립 상태 표시 (회색)
- `<span class="tag highlight">` — Highlight Paper, Best Paper 등 강조가 필요한 것 (앰버 테두리)

분류를 하나 더 추가하려면 `:root`에 색을 정의하고 `.cat.<이름>` 규칙 한 줄만 넣으면 됩니다. 색은 정보를 인코딩하므로 3개 이하로 유지하는 걸 권합니다.

**학력·활동** — 논문과 같은 그리드입니다. 게터에 날짜, 오른쪽에 내용.

```html
<article class="entry compact">
  <p class="venue">2024.03–2026.02</p>
  <div>
    <h3>Degree or Role · <span class="org">Institution</span></h3>
    <p class="sub">Lab, advisor, 부전공 등 한 줄 부연</p>
  </div>
</article>
```

`compact`를 빼면 항목 간격이 넓어지고, `<div>` 안에 `<ul><li>`를 넣으면 성과 불릿을 쓸 수 있습니다.

날짜는 `2024.03–2026.02` 형식(15자)이 게터 폭에 맞게 맞춰져 있습니다. 다른 형식을 쓰려면 `--gutter` 값을 함께 조정하세요.

**연구 분야·기술 목록**:

```html
<dt>Category</dt>
<dd>Item, item, item</dd>
```

프로필 사진을 쓰지 않으려면 `index.html`의 `<img class="avatar" ...>` 한 줄을 지우면 됩니다. 레이아웃은 자동으로 맞춰집니다.

## PDF 이력서로 저장

브라우저에서 `Cmd`+`P` → "PDF로 저장". 인쇄용 스타일이 따로 들어 있어서 여백·폰트 크기가 A4에 맞게 조정되고, 링크는 URL이 옆에 펼쳐지며, 항목이 페이지 경계에서 잘리지 않습니다.

## 디자인

타입은 두 겹으로 나뉩니다.

| 레이어 | 서체 | 쓰이는 곳 |
| --- | --- | --- |
| 내용 | 시스템 산세리프 | 논문 제목, 저자, 본문, 학위명 |
| 라벨 | 시스템 모노스페이스 | 섹션 제목, 학회명, 날짜, 특허번호, 상태 칩 |

데이터시트의 조판 논리를 빌린 것으로, 라벨이 부품 번호처럼 읽히면서 내용과 분리됩니다. 페이지 전체가 **왼쪽 라벨 게터 하나**를 정렬 축으로 공유합니다 — 학회명, 날짜, 연구 분야 분류가 모두 같은 세로선에 섭니다.

섹션 제목(`h2`)은 위쪽 구분선 + 대문자 모노, 소제목(`.subhead`)은 문장형 산세리프입니다. 서체 자체가 달라서 크기 차이가 크지 않아도 위계가 흔들리지 않습니다.

### 섹션 이동 링크

화면 폭 1200px 이상에서만 왼쪽에 고정 표시되며, 스크롤 위치에 따라 현재 섹션이 강조됩니다. 좁은 화면과 인쇄 시에는 자동으로 숨습니다.

필요 없으면 `index.html`의 `<nav class="toc">` 블록과 맨 아래 `<script>` 블록을 함께 지우면 됩니다. 나머지는 그대로 동작합니다. 섹션을 추가할 때는 `<section id="...">`의 id와 `<nav>`의 `href="#..."`를 맞춰주기만 하면 스크롤 추적이 자동으로 따라갑니다.

`style.css` 맨 위 `:root` 블록만 건드리면 전체가 따라 바뀝니다.

```css
--accent:  #2b4ecc;   /* 링크 색 */
--measure: 48rem;     /* 본문 최대 너비 */
--gutter:  8.5rem;    /* 좌측 라벨 칼럼 */
--gap:     3rem;      /* 섹션 간격 */
```

라이트·다크 양쪽 모두 본문 색상 대비를 WCAG AA(4.5:1) 이상으로 맞춰뒀습니다. `--fg-faint`만 예외인데, 불릿 마커 전용이라 글자에는 쓰지 마세요.

### 라이트/다크 전환

오른쪽 위 버튼으로 전환하며, 선택은 `localStorage`에 저장돼 다음 방문에도 유지됩니다. 한 번도 누르지 않았다면 OS 설정을 따라가고, OS 설정이 바뀌면 실시간으로 반영됩니다.

다크 팔레트는 `style.css`의 `:root[data-theme="dark"]` **한 블록**에만 있습니다. 색을 바꿀 때도 여기만 고치면 됩니다. `<head>`의 인라인 스크립트가 첫 페인트 전에 테마를 확정하므로 새로고침할 때 흰 화면이 번쩍이지 않습니다 — 이 스크립트는 `<head>` 안에 있어야 하니 아래로 옮기지 마세요.

인쇄할 때는 다크 모드로 보던 중이어도 항상 흰 종이로 나갑니다.

## 배포

```sh
git add -A
git commit -m "Update CV"
git push
```

push 후 1분 내에 반영됩니다. Pages는 이미 켜져 있어서 추가 설정이 필요 없습니다.
