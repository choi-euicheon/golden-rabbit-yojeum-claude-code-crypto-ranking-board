# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 개발 명령어

```bash
pnpm install          # 의존성 설치
pnpm dev              # 개발 서버 실행
pnpm build            # 프로덕션 빌드
pnpm lint             # 린트 실행
pnpm test             # 테스트 실행 (Jest 30 + Testing Library)
pnpm test -- --testPathPattern="경로/테스트파일"  # 단일 테스트 파일 실행
```

## 아키텍처

Next.js 15.2.4 App Router (React 19) 기반 암호화폐 순위 대시보드. 한국어 UI (`lang="ko"`).

### 데이터 흐름

`app/page.tsx` → `crypto-ranking-board.tsx`(루트 레벨) 임포트 → 8개 암호화폐 목데이터를 테이블로 렌더링. 실제 API 연동은 없음 (푸터에 "Powered by CoinAPI" 표시되지만 미구현). `CoinData` 인터페이스와 `mockCoinData` 배열이 `crypto-ranking-board.tsx`에 정의되어 있음.

### UI 스택

- **shadcn/ui** 컴포넌트가 `components/ui/`에 다수 존재하나, 실제 사용 중인 것은 `Card`, `CardContent`, `CardHeader`, `CardTitle`, `Badge`뿐.
- **Tailwind CSS** 다크 테마 고정 (`bg-gray-950`). 테마 설정은 `tailwind.config.ts`, 글로벌 스타일은 `app/globals.css`.
- **반응형**: 시가총액은 `md` 이상, 거래량은 `lg` 이상에서만 표시.
- **경로 별칭**: `@/`가 프로젝트 루트에 매핑됨.

### 주요 파일

| 파일 | 역할 |
|------|------|
| `crypto-ranking-board.tsx` | 핵심 컴포넌트: 코인 데이터, 포맷 유틸 (`formatNumber`, `formatPrice`), 테이블 렌더링 |
| `app/page.tsx` | 진입점, 메인 컴포넌트 래핑 |
| `app/layout.tsx` | 루트 레이아웃, 한국어 메타데이터 |
| `next.config.mjs` | 빌드 시 ESLint·TypeScript 에러 무시, 이미지 최적화 비활성화 |
| `jest.config.mjs` | Jest 설정 (ts-jest, jsdom, 경로 별칭 지원) |

## 빌드 설정 참고사항

- `next.config.mjs`에서 빌드 시 ESLint와 TypeScript 에러를 모두 무시하도록 설정되어 있음 (`ignoreDuringBuilds: true`, `ignoreBuildErrors: true`).
- 코인 로고는 `public/coin/`에 저장 (대부분 `.png`, USDC만 `.webp`).
