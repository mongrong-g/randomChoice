# Claude Code 작업 가이드

## 프로젝트 컨텍스트
이 프로젝트는 엽기토끼 캐릭터를 활용한 HTML5 랜덤 추첨 애플리케이션입니다.
모바일/태블릿/PC 환경에서 작동하며, 단일 HTML 파일로 배포됩니다.

## 파일 구조 이해

### 작업 파일
- **randomChoice.html**: 개발/수정용 원본 파일
  - 이미지/사운드를 외부 파일로 참조
  - 이 파일을 수정하면 됨

- **randomChoice_standalone.html**: 배포용 통합 파일
  - 모든 리소스가 base64로 내장
  - randomChoice.html 수정 후 Python 스크립트로 재생성

### 리소스 파일
```
images/
  Background.png          (배경, 581x423)
  Star.png               (별 효과)
  Card1.png ~ Card6.png  (랜덤 카드)
  rabbit_button_normal.png
  rabbit_button_over_push.png
  paper/
    paper_01.png ~ paper_10.png  (10프레임 애니메이션)

sounds/
  Click.mp3  (토끼 버튼 클릭)
  Card.mp3   (카드 애니메이션)
  pick.mp3   (이름 선택)
```

## 핵심 설계 원칙

### 1. 반응형 비율 유지
- 기본 비율: **581:423** (Background.png 크기)
- gameContainer가 이 비율 유지
```css
#gameContainer {
    max-width: calc(100vh * 581 / 423);
    max-height: calc(100vw * 423 / 581);
}
```

### 2. 단위 사용 규칙
- **위치/크기**: % (컨테이너 기준)
- **텍스트/버튼**: vmin (뷰포트 기준)
- **카드/Paper 텍스트**: cqw (Container Query Width)
- **절대 px 사용 금지** (반응형 깨짐)

### 3. 애니메이션 순서
```
토끼 클릭 → Click.mp3
   ↓ 0.5초
카드 애니메이션 (2초) + Card.mp3
   ↓
이름 표시 + pick.mp3
   ↓ (토끼 버튼 재활성화)
   ↓ 0.5초
Paper 애니메이션 (1초)
```

### 4. 인덱스 기반 추첨
- `drawnIndices` 배열로 중복 방지
- 같은 이름도 인덱스가 다르면 별개
- 카드: "이름 (인덱스+1)" 표시
- Paper: "이름"만 표시

## 수정 작업 가이드

### randomChoice.html 수정 후 standalone 재생성
```python
# 이미 작성된 Python 스크립트 사용
cd "C:\Users\mongrong\Downloads\randomChoice(엽기토끼)\11111"
python -c "
# [Python 스크립트 내용 - 기존 작업 히스토리 참조]
"
```

**중요**: randomChoice.html을 수정했다면 반드시 standalone 재생성 필요!

### 이미지 추가/변경 시
1. images/ 폴더에 파일 추가
2. randomChoice.html에서 경로 참조 추가
3. Python 스크립트의 `files_to_encode`에 추가
4. standalone 재생성

### 동적 경로 처리
템플릿 리터럴(`` `url('images/Card${num}.png')` ``)은 standalone에서 작동 안 함!

**올바른 방법**:
```javascript
// JavaScript 배열로 base64 데이터 저장
const cardImages = [
    'data:image/png;base64,...',
    'data:image/png;base64,...',
    ...
];

// 인덱스로 접근
resultCard.style.backgroundImage = "url(" + cardImages[randomCardNum - 1] + ")";
```

## 주요 컴포넌트 설명

### 1. Paper Units
- 명단 개수만큼 동적 생성
- 3줄: 10개씩, 나머지 2줄: 5개씩
- 각 unit은 84x70px paper 이미지 + 텍스트

**수정 시 주의**:
- `createPaperUnits()` 함수 확인
- 배치 로직: `const rowSizes = [10, 10, 10, 5, 5]`

### 2. 카드 애니메이션
- CSS keyframes `cardAppear`
- 3회 회전 (1080도)
- 0.05배 → 1배 확대
- 2초 duration

**수정 시 주의**:
- 회전 각도: rotateY(-1080deg) = 3회전
- 시작 크기: scale(0.05)
- 종료 크기: scale(1)

### 3. 명단 관리 모달
- localStorage 키: `randomChoiceNameList`
- 최대 40명 제한
- 반응형 크기: 80vw × 80vh (최대 1.3 비율)

**수정 시 주의**:
- `saveNameList()`: 명단 저장
- `loadNameListFile()`: 파일 불러오기 (인코딩 자동 감지)
- `shareNameList()`: Web Share API + Fallback

### 4. 전체화면 모드
- Fullscreen API + Screen Orientation API
- 가로 모드 강제: `screen.orientation.lock('landscape')`

**수정 시 주의**:
- iOS Safari는 전체화면 제한적
- 가로 모드는 Android/Chrome 위주 지원

## 자주 발생하는 이슈

### 1. 카드가 안 보임 (standalone)
- 원인: 템플릿 리터럴이 base64로 치환 안 됨
- 해결: JavaScript 배열 사용

### 2. 텍스트 줄바꿈
- 원인: 좁은 화면에서 고정 폰트 크기
- 해결: cqw 단위 + `white-space: nowrap`

### 3. Paper 텍스트 위치 안 맞음
- 원인: px 단위 사용 또는 클리핑 영역 오류
- 해결:
  - 클리핑 영역: `top: 41%`
  - 텍스트 위치: `margin-left: 20%`

### 4. 애니메이션 중복 실행
- 원인: `isAnimating` 플래그 미사용
- 해결: 애니메이션 시작 시 플래그 설정

## 테스트 체크리스트

### PC 테스트
- [ ] Chrome 브라우저에서 정상 작동
- [ ] 명단 저장/불러오기
- [ ] 파일 다운로드
- [ ] 전체화면 모드

### 모바일 테스트
- [ ] 안드로이드 Chrome에서 테스트
- [ ] 전체화면 + 가로 모드 전환
- [ ] 터치 동작 확인
- [ ] 공유하기 기능 (Web Share API)

### 애니메이션 테스트
- [ ] 토끼 버튼 클릭 → 카드 애니메이션
- [ ] 이름 표시 타이밍
- [ ] Paper 애니메이션
- [ ] 별 효과 랜덤 생성
- [ ] 사운드 재생

### 엣지 케이스
- [ ] 명단 1개일 때
- [ ] 명단 40개 꽉 찼을 때
- [ ] 중복 이름이 있을 때
- [ ] 모두 추첨 완료 후 클릭
- [ ] 브라우저 새로고침 (localStorage 유지)

## 수정 금지 영역

### 절대 변경하면 안 되는 것
1. **비율 581:423** - 배경 이미지 비율
2. **인덱스 기반 로직** - 중복 이름 지원 핵심
3. **Paper Unit 배치 로직** - 3+2줄 구조
4. **애니메이션 타이밍 체인** - 0.5초 딜레이 등

### 신중하게 변경해야 하는 것
1. **클리핑 영역** (top: 41%) - 사용자가 여러 번 조정한 값
2. **텍스트 위치** (margin-left: 20%) - 시각적으로 최적화된 값
3. **버튼 크기/위치** - 사용자 선호도 반영

## 디버깅 팁

### Console 로그
- Fullscreen/Orientation 실패 시 console 확인
- `console.log('Orientation lock failed:', err)`
- `console.log('Fullscreen failed:', err)`

### 브라우저 개발자 도구
- Container Queries: Chrome DevTools에서 확인 가능
- 반응형 테스트: Device Toolbar 사용
- localStorage: Application > Local Storage 확인

### 파일 인코딩 문제
- 한글 깨짐 시: UTF-8, EUC-KR, CP949 순서로 시도
- `TextDecoder` API 사용

## 성능 최적화

### 이미 적용된 최적화
- 이미지 base64 인코딩 (HTTP 요청 제거)
- CSS 애니메이션 (GPU 가속)
- 이벤트 리스너 최소화
- 프리로드 이미지 캐싱

### 추가 최적화 가능 영역
- 별 생성 주기 조정 (현재 충분히 최적화됨)
- Paper 이미지 프레임 수 줄이기 (10 → 5?)
- 사운드 파일 압축

## 향후 개선 아이디어

### 쉬운 개선
- [ ] 다크 모드 토글
- [ ] 애니메이션 속도 조절
- [ ] 커스텀 배경색
- [ ] 추첨 히스토리 저장

### 중간 난이도
- [ ] 커스텀 사운드 업로드
- [ ] 그룹별 추첨 (반별로 나누기 등)
- [ ] 애니메이션 효과 선택
- [ ] QR 코드로 명단 공유

### 고난도
- [ ] 멀티플레이어 (실시간 동기화)
- [ ] 통계 대시보드
- [ ] 클라우드 저장
- [ ] PWA 변환

## 연락처
프로젝트 작성일: 2026-03-18
사용자: mongrong

## 참고 자료
- [작업_히스토리.md](./작업_히스토리.md) - 모든 변경 내역
- [프로젝트_개요.md](./프로젝트_개요.md) - 기능 설명 및 사용법
- MDN Web Docs: Fullscreen API, Screen Orientation API, Web Share API
- CSS Container Queries: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries
