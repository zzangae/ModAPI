# ModAPI(v1) v2.0.9550 - 20260223

**The Forest 모드 관리 도구 — 업그레이드 에디션**

> 원본: FluffyFish / Philipp Mohrenstecher (Engelskirchen, Germany)
> 업그레이드: zzangae (Republic of Korea)

---

## 개요

ModAPI는 The Forest 게임의 모드를 관리하는 데스크톱 애플리케이션입니다. 이 업그레이드 에디션은 .NET Framework 4.8 마이그레이션, Windows 11 Fluent Design UI, 3-테마 시스템, 13개 언어 지원, Downloads 탭 등을 포함합니다.

---

## 주요 변경사항

### Phase 1 — .NET Framework 4.8 업그레이드

- 전체 프로젝트(5개) `.NET Framework 4.5` → `4.8` 마이그레이션
- `TargetFrameworkVersion`, `App.config`, `packages.config` 일괄 업데이트
- 어셈블리 버전 `v2.0.0.0`으로 통일

### Phase 2 — 빌드 환경 정비 및 Fluent Design 기반 구축

- **ModernWpf 0.9.6** NuGet 패키지 도입
- **FluentStyles.xaml** 생성 — Windows 11 Fluent Design 오버라이드 레이어
  - Fluent 색상 팔레트, 타이포그래피, 버튼, 탭, 콤보박스, 스크롤바 스타일
  - Window, SubWindow, SplashScreen 템플릿
- **UnityEngine 스텁 DLL** 컴파일
  - 누락 타입 추가: `WWW`, `Event`, `TextEditor`, `Physics` 등
  - `TextEditor.pos`/`selectPos` 프로퍼티 추가
- 의존성 참조 수정 및 빌드 성공 확인

### Phase 3 — UI 재설계 및 테마 시스템

#### Fluent UI 재설계
- **MainWindow.xaml** 완전 재구성 (545→565 lines)
  - Fluent Design 기반 레이아웃, 색상, 타이포그래피
  - 탭 컨트롤, 상태바, 캡션 버튼 재설계
- 런타임 오류 수정: SplashScreen 프리징, 탭 전환, 아이콘 상태, 창 드래그

#### 3-테마 시스템

| 테마 | 스타일 파일 | 설명 |
|------|------------|------|
| 기본 테마 | Dictionary.xaml only | 원본 ModAPI 디자인 (텍스처 배경) |
| 화이트 테마 | FluentStylesLight.xaml | 밝은 톤 + 파란 악센트 |
| 다크 테마 | FluentStyles.xaml | 어두운 톤 + 파란 악센트 (기본값) |

- 설정 탭에 **테마 선택 ComboBox** 추가
- 테마 변경 시 **확인 팝업** → **자동 재시작**
- `theme.cfg` 파일로 테마 설정 저장/로드

#### 하이퍼링크 및 URL 업데이트
- Welcome 텍스트에 클릭 가능한 금색(`#FFD700`) 하이퍼링크 추가
- 전체 URL을 `modapi.survivetheforest.net`으로 통일

#### 창 드래그
- 루트 Grid `MouseLeftButtonDown` 이벤트로 직접 드래그 처리
- 상단 48px 영역에서 `DragMove()` 호출
- `AllowsTransparency` 값과 무관하게 전 테마 드래그 보장

#### SubWindow 시스템
- **ThemeConfirm** — 테마 변경 확인 팝업 (예/아니오)
- **ThemeRestartNotice** — 재시작 안내 팝업
- **NoProjectWarning** — 프로젝트 미선택 경고 팝업
- 모든 팝업은 ModAPI 자체 스타일 사용 (시스템 MessageBox 미사용)

#### 캡션 버튼
- 최소화/최대화/닫기 버튼 간격 및 벽 거리 조정

#### 상태바
```
Made with ♥ in Engelskirchen, Germany | upgrade by zzangae, Republic of Korea
```

### Phase 4 — 기능 개선 및 죽은 코드 정리

#### 4-1. 로그인 시스템 정리
- 죽은 백엔드(`modapi.cc`) 연결 코드 제거
- `WebService.Initialize()` → no-op 처리
- 주석 처리된 로그인 핸들러 5개 제거 (ShowLoginLoader, ShowLoginUser 등)
- 활성 `Login()`, `DoLogout()` 메서드 제거

#### 4-2. 업데이트 시스템 현대화
- 다운로드 URL: `modapi.cc/app/archives/` → `github.com/zzangae/ModAPI/releases/download/`
- GitHub 리다이렉트 대응 (`AllowAutoRedirect` + `UserAgent`)

#### 4-3. 죽은 소셜 링크 제거
- SouldrinkerLP 소셜 링크 4개 제거 (Facebook, Twitter, YouTube, Twitch)

#### 4-4. 죽은 코드 정리
- Building 관련 죽은 메서드 6개 제거
- 미사용 필드 및 using 제거
- `OldConfiguration`, `LanguageList` 클래스에 `[Obsolete]` 표시
- **MainWindow.xaml.cs**: 1127줄 → 905줄 (**-222줄**)

#### 4-5. 개발자 모드 바이패스
- `--dev` 실행 인자로 게임 경로/Steam 경로 검증 스킵
- 게임 미설치 환경에서 UI 개발 가능

### Phase 5 — Downloads 탭 및 다국어 확장

#### 5-1. Downloads 탭 UI 프레임워크
- **MainWindow.xaml**에 Downloads 탭 추가 (Mods ↔ Development 사이)
- 인터넷 연결 체크 (`modapi.survivetheforest.net` ping)
- **온라인 모드**: 모드 목록 ListView (이름, 작성자, 카테고리, 다운로드 수)
- **오프라인 모드**: 수동 다운로드 안내 + 웹사이트 링크 + 재연결 버튼
- Downloads 관련 UI 문자열 17개 키 (전체 13개 언어 지원)

#### 5-2. 다국어 지원 확장 (6개 → 13개)

| # | 코드 | 언어 | 파일 | 신규 |
|---|------|------|------|------|
| 1 | EN | English | Language.EN.xaml | |
| 2 | DE | Deutsch | Language.DE.xaml | |
| 3 | ES | Español | Language.ES.xaml | |
| 4 | FR | Français | Language.FR.xaml | |
| 5 | KO | 한국어 | Language.KR.xaml | |
| 6 | IT | Italiano | Language.IT.xaml | ✅ |
| 7 | JA | 日本語 | Language.JA.xaml | ✅ |
| 8 | PL | Polski | Language.PL.xaml | |
| 9 | PT | Português | Language.PT.xaml | ✅ |
| 10 | RU | Русский | Language.RU.xaml | ✅ |
| 11 | VI | Tiếng Việt | Language.VI.xaml | ✅ |
| 12 | ZH | 简体中文 | Language.ZH.xaml | ✅ |
| 13 | ZH-TW | 繁體中文 | Language.ZH-TW.xaml | ✅ |

- 각 언어마다 국기 아이콘 PNG (36×24) 포함
- Settings 언어 셀렉터 **커스텀 정렬** (한국어 5번째 고정)
- `SettingsViewModel` 인덱스 매핑 수정 (`MainWindow.LanguageOrder` 기반)
- Development 셀렉터 아이콘 로딩 예외처리 추가

---

## 파일 구조

```
ModAPI/
├── App.xaml / App.xaml.cs          # 테마 로드/저장/적용, --dev 모드
├── Dictionary.xaml                  # 원본 스타일 + Fluent 폴백 리소스
├── FluentStyles.xaml                # 다크 테마 (기본값)
├── FluentStylesLight.xaml           # 화이트 테마
├── ModAPI.csproj                    # 프로젝트 설정
├── Data/
│   └── ViewModels/
│       └── SettingsViewModel.cs     # 언어/테마/설정 바인딩
├── Windows/
│   ├── MainWindow.xaml / .cs        # 메인 UI + 테마 + 드래그 + Downloads 탭
│   ├── SplashScreen.xaml / .cs      # 스플래시 화면
│   └── SubWindows/
│       ├── BaseSubWindow.cs         # SubWindow 기본 클래스
│       ├── ThemeConfirm.xaml / .cs
│       ├── ThemeRestartNotice.xaml / .cs
│       └── NoProjectWarning.xaml / .cs
├── resources/
│   └── langs/
│       ├── Language.EN.xaml + .png
│       ├── Language.DE.xaml + .png
│       ├── Language.ES.xaml + .png
│       ├── Language.FR.xaml + .png
│       ├── Language.KR.xaml + .png
│       ├── Language.IT.xaml + .png   # 신규
│       ├── Language.JA.xaml + .png   # 신규
│       ├── Language.PL.xaml + .png
│       ├── Language.PT.xaml + .png   # 신규
│       ├── Language.RU.xaml + .png   # 신규
│       ├── Language.VI.xaml + .png   # 신규
│       ├── Language.ZH.xaml + .png   # 신규
│       └── Language.ZH-TW.xaml + .png # 신규
└── libs/
    └── UnityEngine.dll              # 스텁 DLL
```

---

## 빌드 요구사항

- **Visual Studio 2022**
- **.NET Framework 4.8** SDK
- **ModernWpf 0.9.6** (NuGet)

---

## 라이선스

GNU General Public License v3.0 — 원본 라이선스를 따릅니다.
