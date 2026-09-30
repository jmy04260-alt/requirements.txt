# 02 · 디자인 가이드 — 츄블리아이래쉬 (v3)

> 개발 에이전트는 이 문서만 보고 단일 HTML 시안을 구현한다. 값은 그대로 CSS 변수로 옮길 것.
> 2026-09-30 v3 개정: 사장님 피드백("유치하지 않게", "명조체", "사실적인 속눈썹", "고급 모델 사진") + 최종 목업 레퍼런스 반영.

## 1. 브랜드 콘셉트
- **한 줄**: "Beauty in *Every Blink*" — 눈을 뜰 때마다, 한 가닥씩 정교하게.
- **무드**: 고급 뷰티 브랜드 에디토리얼. 더스티 로즈 베이지 바탕, 딥 모카 글자, 하이콘트라스트 세리프 + 로즈 필기체 한 줄. 넓은 여백, 얇은 선, 사진 중심.
- **절제 원칙**: 선물상자·풍선형 장식·꽃·반짝이·점 패턴·골드 그라데이션 **전부 제거**. 장식은 얇은 선·작은 라인 아이콘·필기체 한두 곳만.
- **단일 라이트 룩**: 다크 모드 없음. `:root`에 `color-scheme: light`, body 배경 명시.
- **금지**: 초록 CTA, 초콜릿 채움 버튼, 이모지 마커, 외부 이미지 파일, 대문자 콘덴스드 제목(Oswald·Black Han Sans).

## 2. 컬러 토큰 (`:root`)
| 토큰 | HEX | 용도 |
|---|---|---|
| `--blush` (= `--bg`) | `#F7EFEA` | 페이지 배경(크림 로즈, 패턴 없음) |
| `--card` / `--paper` | `#FBF6F3` / `#FFFCFA` | 카드·띠 면 / 신뢰 바·타임라인·폼 면 |
| `--choco` | `#4A2C27` | 제목·강조 글자(딥 모카) |
| `--choco-2` | `#6E5650` | 본문 |
| `--cocoa` | `#7E6159` | 보조 텍스트(대비 4.5↑) |
| `--accent` → hover `--accent-h` | `#A86F62` → `#8F5A4E` | 주 버튼 채움, 선택 상태(칩·슬롯·날짜·단계), 라인 아이콘, 필기체 |
| `--label` | `#9A675C` | 세리프 대문자 라벨 |
| `--rose` / `--rose-soft` | `#D9B3A8` / `#F1E2DC` | 얇은 테두리 / 선택 옵션 면 |
| `--line` / `--hair` | `#E6D6D0` / `rgba(74,44,39,.12)` | 카드 테두리 / 구분선 |
| `--accent-ink` | `#9A3F38` | 에러·할인·홀드 타이머 |
- 그림자는 거의 쓰지 않는다(히어로 없음, 영수증 카드만 `0 16px 36px -30px`).

## 3. 타이포그래피 (Google Fonts `<link>` 1개)
| 역할 | 폰트 (fallback) | 사용 |
|---|---|---|
| 한글 제목 | **Nanum Myeongjo 800** (Noto Serif KR, Batang, AppleMyungjo, serif) | h2·h3·카드 제목·패널 제목 |
| 한글 본문 | **Noto Serif KR 400** (동일 명조 fallback) | 본문 17px(모바일 16px) / 행간 1.8 |
| 영문 디스플레이 | **Cormorant Garamond 500–600** (이탤릭 포함) | "Beauty in", "Lash Services", "Service Price List", 서비스 영문명, 평균 별점 |
| 필기체 | **Great Vibes** (Pinyon Script, cursive) | "Every Blink", 로고, "For your eyes only", "More Beautiful You", 섹션 영문 한 단어 — 로즈색 단색 |
| 라벨 | Cormorant 600 대문자, 자간 .26–.3em | NATURAL × ELEGANT × CONFIDENT, OUR SERVICES, BEFORE & AFTER, PRICING, CLIENT REVIEW |
| UI·숫자 | **Montserrat 400–600** (Noto Sans KR) | 버튼, 가격·시간·날짜(tabular-nums), 타임라인 눈금 |

## 4. 로고·아이콘
- 로고: 가는 선 속눈썹 아이콘(감은 눈 곡선 + 짧은 가닥) 위, 필기체 영문 매장명, 아래 이탤릭 "For your eyes only".
- 영문 매장명은 JS 상수 `BRAND_EN` 한 곳에서 관리(`[data-brand]`에 주입). 레퍼런스의 'Chubly' 표기는 확인 중.
- 라인 아이콘(인라인 SVG, stroke 1.1–1.3, `--accent`): 달력(Book Now), 리본 배지·잎·하트(신뢰 바), 위치·전화·인스타·시계(푸터), 큰 따옴표(후기).

## 5. 사진 슬롯 (`PHOTOS`)
- 코드 상단 `const PHOTOS = { hero, heroAlt, services[3], beforeAfter[3], review, cta, gallery[6] }`. `src`(경로 또는 data URI)가 있으면 `<img>`(object-fit: cover, alt), 비어 있으면 플레이스홀더.
- 플레이스홀더: 크림→로즈 그라데이션 + 안쪽 1px 흰 선 + **캔버스 속눈썹** + 작은 캡션 "모델 사진 영역 · 4:5".
- 히어로 사진은 우측 2/3, 좌측으로 크림 바탕에 페이드(`mask-image`). 모바일은 사진이 위, 아래로 페이드.
- beforeAfter[0]는 `{before, after, caption}`으로 첫 칸을 상하 분할(Before/After 알약 라벨).

## 6. 사실적 캔버스 속눈썹
- `<canvas data-lash="프리셋">` 을 `paintLash()`로 렌더. 고정 시드(mulberry32) → 매번 같은 모양. devicePixelRatio(최대 2.5) + ResizeObserver 대응, 애니메이션 없음(첫 프레임에 완성).
- 가닥: 2차 베지어 컬, **뿌리 두껍고 끝으로 가늘어지는 채워진 폴리곤**, 흑갈색→반투명 브라운 그라데이션, 일부 가닥 한쪽 하이라이트.
- 매핑: 안쪽 짧고 바깥 75–80% 지점 최장(캣아이), 바깥으로 갈수록 기울기·컬 증가, 길이·각도·곡률 랜덤 변주, 10%는 짧은 잔모.
- 볼륨 팬: 뿌리 한 점에서 3–7가닥 부채꼴(프리셋별 비율). 레이어 3겹(뒤: 옅고 흐린 그림자 → 앞: 진하고 선명) + shadowBlur 그림자.
- 프리셋: hero(감은 눈, 쌍꺼풀 음영), classic, volume, mega, lift, tint, lower(하속눈썹), bare(시술 전), mini(후기 원형).
- 가닥 끝이 캔버스 밖으로 나가지 않도록 길이를 자동으로 줄인다.

## 7. 홈 레이아웃 (위 → 아래)
1. **헤더**(히어로 위 투명, 스크롤 시 크림 면): 로고 | Home·Services·Gallery·Pricing·Reviews·Magazine·About(한글 병기, 활성 밑줄) | 아웃라인 알약 "Book Now".
2. **히어로**: 라벨 → "Beauty in" + 필기체 "Every Blink" → 설명 2줄 → [Book Appointment 로즈 채움] [View Services →]. 우하단 필기체 "For your eyes only" + 곡선 + 하트.
3. **신뢰 바**: 흰 띠 3칸(세로 구분선) — Certified Lash Artist / Premium Materials / Personalized Design.
4. **2열**: 좌 OUR SERVICES · Lash Services(카드 3: 사진·01–03·영문명·한글명·설명 2줄·From ₩·원형 화살표) / 우 BEFORE & AFTER · Real Transformations(분할 + 2컷, 필기체 "More Beautiful You").
5. **3열 띠**: Pricing 요약 + "View Price List" | Client Review(원형 사진·따옴표·별 5·예시 후기) | 사진 배경 어둡게 + "Ready for Your Perfect Set?" + 흰 아웃라인 "지금 예약하기".
6. 스타일 갤러리(6컷, 가운데 열 오프셋) → 컬 가이드 → 래쉬 노트 → 매장 정보(About).
7. **푸터 한 줄**: 로고 | 주소(+네이버 지도 보조 링크) | 전화(복사) | 인스타 | Open Hours 표. 모바일은 1열.
- 컨테이너 1200px, 좌우 32px(모바일 16px), 섹션 간격 112px(모바일 72px). 400px에서 2·3열은 1열.

## 8. 컴포넌트
- **버튼**: 알약 50px(sm 42). Primary = 로즈 채움 + 흰 글자. Secondary = 로즈 1px 아웃라인. Ghost = 흰 아웃라인(어두운 사진 위). 텍스트 링크 = Cormorant 600 + → .
- **카드**: `--paper`/`--card` 면 + 1px `--line` + 반경 6px, 그림자 없음.
- **선택 상태**(칩·옵션·날짜·슬롯·단계·탭): 로즈 채움 또는 로즈 테두리 + 좌측 3px 인셋.
- **예약 3단계**: 안내문("시술 시간 N분 + 정리 10분을 기준으로…") → **디자이너 타임라인**(영업시간 축, 예약 블록 사선 해치 "예약됨 14:00–15:30", 정리 구간 연로즈, 선택 블록 로즈 채움; 모바일 `overflow-x:auto`) → 오전/오후 슬롯 버튼("14:00 / → 15:30 종료", 앞 예약 바로 다음 시각은 작은 로즈 점).
- **폼**: 언더라인 입력, 에러 `--accent-ink`. 결제 단계 "데모 결제 — 실제 청구되지 않음" 유지.

## 9. 예약 정책 표기
- **사이트 자체 예약이 메인**: 헤더·히어로·가격표 각 행·모바일 하단 바·매장 정보의 주 CTA는 모두 예약 섹션으로 이동하는 "예약하기".
- 네이버는 매장 정보·푸터의 "네이버 지도에서 보기" 보조 링크만(`NAVER_PLACE_URL`).
- 주소·전화·인스타·후기는 확인 전이므로 "(예시)" 라벨 유지.

## 10. 모션·접근성
- 등장 translateY(8px) 240ms, 호버 120–160ms, 컬 게이지 1회 드로잉. 무한 루프 금지.
- `prefers-reduced-motion: reduce` → 애니메이션·트랜지션 0.01ms, 앵커 스크롤 즉시.
- 첫 화면 opacity:0 금지, 장식·캔버스 `aria-hidden`, 플레이스홀더는 `role="img"` + 설명, 터치 타깃 44px+, sticky 헤더 `top: env(safe-area-inset-top, 0px)`, 하단 바 safe-area, `el.hidden`, alert/confirm/prompt 금지.
