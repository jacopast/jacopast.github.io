# jacopast.github.io — 개인 프로젝트 링크 허브

자코님(jacopast)의 개인 프로젝트들을 한 페이지에 모아 보여 주는 정적 링크 허브.

- **GitHub repo**: `jacopast/jacopast.github.io` (user site → `main` 브랜치 루트가 그대로 발행됨)
- **공개 주소**: https://jacopast.github.io
- **로컬 경로**: `~/AG/git/jacopast.github.io/`
- **ag-bridge 인계 문서**: `~/ag-bridge/projects/jacopast_github_io/CURRENT_TASK.md`
- 기존 WordPress 사이트 jacopast.com(`~/ag-bridge/projects/jacopast_site/`)과는 **별개 프로젝트**.

## 핵심 원칙

- **링크 데이터의 단일 출처(SSOT)는 `links.json`.** 링크 추가·수정·삭제는 이 파일만 고친다. HTML에 링크를 하드코딩하지 않는다.
- 페이지는 `links.json`을 읽어 목록을 렌더링한다. 빌드 단계 없는 순수 정적 사이트(HTML/CSS/JS)를 기본으로 하고, 빌드 도구·프레임워크는 자코님 승인 없이 도입하지 않는다.
- GitHub Pages가 Jekyll 처리로 파일을 건너뛰지 않도록 루트에 `.nojekyll`을 둔다.
- 공개 저장소·공개 페이지다. 비공개 레포 주소, 토큰, 내부 경로(로컬 파일 경로·Drive 경로 등)를 `links.json`이나 페이지에 넣지 않는다.

## links.json 스키마

```json
{
  "sections": [
    { "links": [ { "title": "jacopast.com", "url": "https://jacopast.com", "description": "블로그" } ] },
    {
      "name": "Personal",
      "links": [
        { "title": "프로젝트 이름", "url": "https://...", "description": "한 줄 설명" }
      ]
    }
  ]
}
```

- `sections` 순서 = 화면 순서, 각 `links` 순서 = 섹션 안 순서.
- 섹션은 `links` 또는 `groups`(서브 카테고리 배열, 각 `{ "name", "links" }`)를 가진다.
- 섹션 `name`은 선택 — 없으면 제목 없이 링크만 나온다(맨 위 jacopast.com이 그렇게 단독 섹션).
- 항목의 `title`은 필수, `url`·`description`은 선택. `url`이 없으면 링크 없는 텍스트로 나온다(`Draft` 섹션용).
- 현재 카테고리: (이름 없음: jacopast.com, enwd.net) → `Personal` → `Work` → `Draft`.
- `Draft`: 작업 중인 프로젝트를 링크 없이 나열. `links` 대신 `groups`(서브 카테고리: `ENWD`/`Personal`/`Work`, 각각 `name`+`links`)를 쓴다. 그룹 하나가 한 줄 — `카테고리  프로젝트, 프로젝트` (카테고리명은 왼쪽 고정폭 회색, 프로젝트는 쉼표로 이어 씀). `Work`는 **자코님이 회사 일을 쉽게 하려고 개인적으로 만드는 코드·도구·영상**(예: GIS to Rhino, Rhino Parking)만. 회사 프로젝트 자체(예: JSCS 애니메이션, Coney Island PDB)는 넣지 않는다 — 회사 가면 하는 일이라 적을 필요 없음(자코님, 2026-10-08). 가족 관련 항목은 자코님이 직접 넣으라고 한 것만(예: Sol Read). ENWD 에피소드는 매번 같은 세트 작업이라 가장 최근 에피소드 번호 하나만 둔다(새 에피소드 시작하면 교체).
- 스키마를 바꾸면 이 섹션과 `index.html`의 렌더링 코드를 함께 고친다.
- 수정 후 JSON 유효성 확인: `python3 -m json.tool links.json > /dev/null`

## 디자인

- **초미니멀**: 텍스트 + 라인(2px)뿐. 라인은 카테고리명 밑에만 — 링크 사이엔 선 없음. 이름 + 카테고리별 링크 목록(카테고리명은 작은 회색 글씨, 제목 왼쪽·설명 오른쪽).
- 폰트: Google Fonts `Gowun Batang` (고운바탕, 400/700).
- 한글 줄바꿈: `body`에 `word-break: keep-all` + `overflow-wrap: break-word` + `text-wrap: pretty`, 제목(`h1`, `h2`)은 `text-wrap: balance`. Draft 프로젝트명은 `.item { white-space: nowrap }`으로 이름 단위로만 줄바꿈.
- 장식·아이콘·이미지 추가는 자코님 요청 시에만.
- **데이/나이트는 시스템 설정(`prefers-color-scheme`)을 따른다.** 수동 토글 없음.
- 데이는 눈부시지 않게(순백 금지, 따뜻한 회백 `#ecebe6`), 나이트는 너무 어둡지 않게(검정 금지, 차콜 `#2c2d30`). 색은 `index.html`의 CSS 변수에만 있다.

## 같은 도메인의 다른 프로젝트

아래 경로는 각각 별도 repo의 project page다. 이 repo에 같은 이름의 폴더를 만들면 충돌하니 만들지 않는다.

- `/kdrama-tv-guide/` — `jacopast/kdrama-tv-guide`
- `/youtube-news-analysis/` — `jacopast/youtube-news-analysis`
- `/jalsal-mvp/` — `jacopast/jalsal-mvp`

## 로컬 미리보기

`fetch()`로 `links.json`을 읽으므로 `file://`로 열면 안 되고 로컬 서버가 필요하다.

```bash
python3 -m http.server 8000
```

(`.claude/launch.json`에 `hub`로도 등록돼 있음)

## 작업 규칙

- `CLAUDE.md`는 이 파일을 불러오는 한 줄(`@AGENTS.md`)만 둔다. 규칙은 여기에만 쓴다.
- 커밋·푸시는 자코님이 요청할 때만. 푸시하면 곧바로 공개 사이트에 반영된다.
- 작업 시작·보류·완료 시 ag-bridge `CURRENT_TASK.md`와 `~/ag-bridge/PROJECT_CONTINUITY.md`를 함께 갱신한다 (ag-bridge 규약).
- 응답·문서는 한국어 기본, 코드·식별자는 영어.
