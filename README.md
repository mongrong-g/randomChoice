# 🎯 랜덤뽑기 (Random Choice)

엽기토끼 캐릭터를 활용한 HTML5 기반 랜덤 추첨 애플리케이션

## 🌐 데모

**[여기에서 바로 사용하기](https://mongrong-g.github.io/randomChoice/)**

## ✨ 주요 기능

- 🎴 **카드 애니메이션**: 3D 회전 효과와 함께 결과 표시
- 📋 **명단 관리**: 최대 40명까지 저장 (localStorage + 파일 공유)
- 📱 **모바일 최적화**: 전체화면 + 가로모드 지원
- 🔊 **사운드 효과**: 클릭, 카드, 선택 효과음
- ⭐ **별 효과**: 랜덤 생성되는 배경 애니메이션
- 📄 **Paper 연출**: 10프레임 애니메이션으로 결과 표시
- 🔄 **중복 방지**: 인덱스 기반 추첨으로 중복 이름도 지원
- ⌨️ **키보드 지원**: 스페이스 키로 추첨 가능

## 📸 스크린샷

![랜덤뽑기 화면](https://via.placeholder.com/800x450?text=Random+Choice+Screenshot)

## 🚀 사용 방법

### 온라인 사용
1. [데모 페이지](https://mongrong-g.github.io/randomChoice/) 접속
2. "전체화면으로 시작" 클릭 (권장)
3. 명단 관리 → 명단 입력 → 저장
4. 토끼 버튼 클릭 또는 스페이스 키로 추첨!

### 오프라인 사용
1. `index.html` 파일 다운로드
2. `images/`, `sounds/` 폴더 함께 다운로드
3. `index.html` 브라우저로 열기

또는 `randomChoice_standalone.html` 파일 하나만 다운로드해서 실행 (모든 리소스 내장)

## 📱 모바일 가이드

1. 전체화면 시작 권장
2. 자동으로 가로 모드로 전환됨
3. 터치로 토끼 버튼 클릭
4. 공유하기로 명단 백업 가능

## 🎮 단축키

- **스페이스 키**: 추첨 시작 (PC)
- **전체화면 버튼**: 우측 상단 (⛶)

## 📚 문서

- [프로젝트 개요](./docs/프로젝트_개요.md) - 기능 설명 및 사용법
- [작업 히스토리](./docs/작업_히스토리.md) - 개발 과정 및 변경 이력
- [개발 가이드](./docs/CLAUDE.md) - 수정/유지보수 가이드

## 🛠️ 기술 스택

- HTML5
- CSS3 (Container Queries, Animations)
- Vanilla JavaScript
- Web APIs:
  - Fullscreen API
  - Screen Orientation API
  - Web Share API
  - localStorage

## 📦 파일 구조

```
randomChoice/
├── index.html                          # 메인 파일
├── randomChoice_standalone.html        # 통합 파일 (단일)
├── images/                             # 이미지 리소스
│   ├── Background.png
│   ├── Star.png
│   ├── Card1.png ~ Card6.png
│   ├── rabbit_button_normal.png
│   ├── rabbit_button_over_push.png
│   └── paper/
│       └── paper_01.png ~ paper_10.png
├── sounds/                             # 사운드 리소스
│   ├── Card.mp3
│   ├── pick.mp3
│   └── Click.mp3
└── docs/                               # 문서
    ├── CLAUDE.md
    ├── 작업_히스토리.md
    └── 프로젝트_개요.md
```

## 🌟 특징

### 반응형 디자인
- 581:423 비율 유지
- 모든 화면 크기에 대응
- vw/vh/vmin 단위 사용

### 인덱스 기반 추첨
- 같은 이름도 위치로 구분
- 카드: "이름 (번호)" 표시
- Paper: "이름"만 표시

### 애니메이션 체인
```
토끼 클릭 → Click.mp3 → 0.5초 대기 →
카드 애니메이션 (2초) + Card.mp3 →
이름 표시 + pick.mp3 →
토끼 버튼 재활성화 →
Paper 애니메이션 (1초)
```

## 🔧 브라우저 지원

| 기능 | Chrome | Safari | Firefox | Edge |
|------|--------|--------|---------|------|
| 기본 기능 | ✅ | ✅ | ✅ | ✅ |
| 전체화면 | ✅ | ⚠️ | ✅ | ✅ |
| 가로모드 강제 | ✅ | ❌ | ✅ | ✅ |
| Web Share | ✅* | ✅* | ❌** | ✅* |

✅ 지원 / ⚠️ 제한적 / ❌ 미지원 / * 모바일만 / ** 다운로드로 대체

## 📄 라이선스

개인 및 교육 목적 사용

## 👤 제작자

mongrong-g

## 📅 업데이트

- 2026-03-18: 초기 릴리스

---

⭐ 이 프로젝트가 유용하다면 Star를 눌러주세요!
