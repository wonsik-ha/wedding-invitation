# Design

## Source of truth

- Status: Active
- Last refreshed: 2026-09-07
- Primary product surfaces: 기본 모바일 청첩장 `index.html`, 숨은 개발자 페이지 `developer.html`
- Evidence reviewed: `static/src/main.html`, `static/src/css/main.css`, `static/src/css/developer.css`, 전달받은 야외 웨딩 사진

## Brand

- Personality: 자연스럽고 차분한 야외 예식, 정중하고 따뜻한 초대
- Trust signals: 실제 예식 정보의 명확한 표기, 읽기 쉬운 본문, 안정적인 지도 및 공유 fallback
- Avoid: 형광색, 과한 분홍색, 무거운 장식, 사진을 압도하는 강한 gradient

## Product goals

- Goals: 사진과 조화를 이루는 녹색 계열 기본 청첩장, 모바일에서 빠르고 편안한 정보 확인
- Non-goals: 개발자 페이지의 터미널 테마 변경, 새로운 화면이나 기능 추가
- Success signals: 표지 사진이 선명하게 보이고, 텍스트 대비와 모바일 반응형 검사가 유지됨

## Personas and jobs

- Primary personas: 모바일 링크로 청첩장을 받은 하객
- User jobs: 예식 일시·장소·교통·계좌 확인, 지도 열기, 청첩장 공유
- Key contexts of use: iPhone Safari와 Android Chrome, 밝은 야외 환경

## Information architecture

- Primary navigation: 단일 세로 스크롤
- Core routes/screens: `/`, `/developer.html`
- Content hierarchy: 표지 사진과 이름 → 초대 문구 → 가족관계 → 예식 정보 → 교통 → 사진 → 계좌 → 공유

## Design principles

- Principle 1: 사진이 주인공이며 색상은 사진의 수목·잔디·해바라기에서 가져옵니다.
- Principle 2: 녹색 배경에서도 본문은 WCAG AA 수준의 명도 대비를 유지합니다.
- Tradeoffs: 짙은 숲색은 외곽 배경과 포인트에 사용하고, 긴 본문 영역은 옅은 세이지색으로 유지합니다.

## Visual language

- Color: forest `#173727`, sage paper `#EFF4EB`, card `#F8FAF5`, ink `#26372C`, accent green `#356044`, sunflower gold `#7A5C17`
- Typography: 제목은 Gowun Batang, 본문은 Gowun Dodum
- Spacing/layout rhythm: 기존 430px 모바일 폭과 넉넉한 세로 여백 유지
- Shape/radius/elevation: 아치형 표지, 부드러운 모서리, 낮은 명도의 녹색 그림자
- Motion: 기존 scroll reveal 유지, 과도한 신규 motion 금지
- Imagery/iconography: 전달받은 실제 웨딩 사진을 메인 표지와 공유 미리보기에 사용

## Components

- Existing components to reuse: `.paper`, `.cover`, `.arch-photo`, `.venue-card`, `.share-btn`
- New/changed components: 기본 청첩장 색상 token과 녹색 placeholder 상태
- Variants and states: 사진 로딩, TBU, 지도 fallback, 공유 fallback
- Token/component ownership: 기본 청첩장 token은 `static/src/css/main.css`의 마지막 `:root` override가 소유

## Accessibility

- Target standard: WCAG 2.1 AA 대비, Lighthouse Accessibility 100 유지
- Keyboard/focus behavior: 기존 button/link focus와 browser 확대 허용 유지
- Contrast/readability: 긴 본문은 옅은 배경과 짙은 글자, 포인트색도 4.5:1 이상을 목표로 함
- Screen-reader semantics: 기존 `main`, section, button label 유지
- Reduced motion and sensory considerations: 기존 reduced-motion 대응을 보존

## Responsive behavior

- Supported breakpoints/devices: iPhone Safari, Android Chrome, 최대 콘텐츠 폭 430px
- Layout adaptations: 작은 화면에서도 가로 overflow 없이 1열 스크롤
- Touch/hover differences: 핵심 동작은 touch로 완결되며 hover에 의존하지 않음

## Interaction states

- Loading: 사진 로드 전 세이지색 frame
- Empty: 사진 미설정 시 TBU 표시
- Error: 사진 오류 시 TBU, 지도 오류 시 외부 지도 버튼 안내
- Success: 복사·공유 성공 메시지 유지
- Disabled: 사용할 수 없는 동작은 기존 숨김/비활성 처리 유지
- Offline/slow network, if applicable: 외부 지도와 공유 SDK 실패 시 직접 링크·시스템 공유 fallback

## Content voice

- Tone: 존댓말, 정중하고 담백한 초대 문구
- Terminology: 예식장, 연회장, 오시는 길, 마음 전하실 곳
- Microcopy rules: 기술 용어는 개발자 페이지에만 사용

## Implementation constraints

- Framework/styling system: 정적 HTML/CSS/vanilla JavaScript
- Design-token constraints: 기존 CSS 변수와 마지막 theme override만 확장
- Performance constraints: 표지 사진은 웹용 JPEG로 최적화하고 추가 dependency를 사용하지 않음
- Compatibility constraints: GitHub Pages, iPhone Safari, Android Chrome
- Test/screenshot expectations: 로컬 정적 빌드, 모바일 screenshot, 가로 overflow, 사진 load, Lighthouse 대비 확인

## Open questions

- [ ] 추가 갤러리·프로필 사진은 전달될 때 각각 crop 위치를 결정합니다.
