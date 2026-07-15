# SERO 요구사항 인터뷰 — 테스트 절차서

이 킷은 "Claude Design Export HTML → 컴포넌트 분해 → 순차 인터뷰 → wiki 정합성 검증 → 요구사항 문서 생성" 프로세스를 검증하기 위한 것입니다.

## 킷 구성

```
sero-test-kit/
├── CLAUDE.md                     ← 작업 폴더 지침 (스킬 강제 로드)
├── screens/EMP-001-사원관리.html  ← 테스트용 화면 (Design Export 시뮬레이션, 인터랙션 포함)
├── .claude/skills/sero-requirement-interview/
│   ├── SKILL.md                  ← 인터뷰 스킬 본체
│   └── references/sero-components.md ← 컴포넌트별 질문 템플릿 + 문서 양식
├── wiki/
│   ├── _index.md
│   ├── common/공통규칙.md         ← 정합성 충돌 테스트용 규칙 포함
│   └── screens/                  ← 산출물이 여기 생성됨
└── manifest/                     ← 컴포넌트 매니페스트가 여기 생성됨
```

테스트 HTML에는 8종 컴포넌트가 `data-sero-component` / `data-sero-id`로 태깅되어 있습니다:
SeroPageHeader, SeroSearchForm(+Input/Select), SeroGrid, SeroPagination, SeroButtonGroup(+Button), SeroModal(+DatePicker), SeroToast.
인터랙션: 조회 필터링, 컬럼 정렬, 행 선택 → 수정/삭제 버튼 활성화, 신규 모달 + 필수값 검증, confirm 삭제, 토스트.

---

## 사전 준비 (공통)

1. 이 킷을 압축 해제해 로컬 폴더에 둔다. 예: `C:\work\sero-test-kit` 또는 `~/work/sero-test-kit`
2. Claude Desktop 앱 실행, 로그인 (Enterprise 계정에서 Cowork/Design 활성화 확인)
3. **주의:** `.claude`는 숨김 폴더입니다. 압축 해제 후 `.claude/skills/...`가 실제로 존재하는지 확인하세요.

---

## 테스트 A — Claude Code 탭 (권장 1차 테스트: 미리보기 인터랙션 확실)

1. Code 탭 → 새 세션 → 환경 **Local** → 프로젝트 폴더로 `sero-test-kit` 선택 → 권한 모드는 "권한 요청" 또는 "자동 수락 편집"
2. 프롬프트 입력:
   > `screens/EMP-001-사원관리.html` 화면의 요구사항 정의 인터뷰를 시작해줘.
3. 채팅에 출력된 HTML 경로를 클릭 → **미리보기 패널이 열리는지 확인** (체크 ①)
4. 미리보기에서 조회/정렬/행선택/모달을 직접 조작해 **인터랙션 동작 확인** (체크 ②)
5. Claude가 매니페스트를 만들고 컴포넌트 목록 + 인터뷰 순서를 제시하는지 확인 (체크 ③)
6. 아래 "인터뷰 시나리오 대본"대로 답하며 진행

## 테스트 B — Cowork 탭 (2차: 고객 대면 채널 검증)

1. Cowork 탭 → "폴더에서 작업(Work in a folder)"으로 `sero-test-kit` 선택
2. 동일 프롬프트로 시작. HTML 경로 클릭 → 미리보기 렌더링 확인 (체크 ①)
3. **핵심 검증: Cowork 미리보기에서 버튼 클릭 등 인터랙션이 도는지** (체크 ②)
   - 안 되면 대안: (a) 인터뷰는 Cowork로 하되 화면 확인은 OS 기본 브라우저에서 HTML을 열어 나란히 배치, (b) 인터뷰 자체를 Code 탭으로 이관
4. 이후 절차는 테스트 A와 동일

---

## 인터뷰 시나리오 대본 (정합성 충돌 3종 포함)

인터뷰 중 아래처럼 답하세요. **굵은 답변 3개는 wiki 공통규칙과 일부러 충돌시키는 답**입니다 — Claude가 충돌을 잡아내는지가 핵심 검증 포인트입니다.

| 컴포넌트 | 이렇게 답하기 | 기대 동작 |
|---|---|---|
| SeroPageHeader | "인사팀과 관리자 그룹만 접근. 진입 시 자동 조회" | 그대로 기록 |
| SeroSearchForm | "부서 기본값은 로그인 사용자 소속 부서. Enter 조회 지원" | 그대로 기록 |
| SeroGrid | **"페이지당 50건 보여줘"** | ⚠ [GRID-01: 20건]과 충돌 제시 → "이 화면만 예외로" 선택 → 예외 사유 기록 확인 (체크 ④) |
| SeroGrid | **"정렬은 화면에서(클라이언트) 처리"** | ⚠ [GRID-02: 서버 사이드]와 충돌 제시 → "기존 규칙 유지" 선택 (체크 ⑤) |
| SeroPagination | "번호형 유지" | 그대로 기록 |
| SeroButtonGroup | **"삭제 버튼은 모든 사용자에게 보이되 권한 없으면 비활성화"** | ⚠ [AUTH-01: 관리자만 노출]과 충돌 제시 (체크 ⑥) |
| SeroButtonGroup(삭제) | "삭제는 실제로 지워줘" 라고 답해보기 | ⚠ [DEL-01: 논리 삭제]와 충돌 제시되면 "기존 유지" 선택 |
| SeroModal | "사원명 30자 제한, 저장 성공 시 모달 닫고 그리드 재조회" | [EMP-02], [VAL-01] 참조가 달리는지 확인 (체크 ⑦) |
| SeroToast | (답 없이) "스킵" | TBD 처리 확인 (체크 ⑧) |

## 완료 후 확인 체크리스트

- [ ] ① HTML 미리보기 렌더링 (Code / Cowork 각각)
- [ ] ② 미리보기 내 인터랙션 동작 (Code / Cowork 각각) ← **Cowork 결과가 아키텍처 결정 포인트**
- [ ] ③ `manifest/EMP-001-manifest.json` 생성, 컴포넌트 8종·순서 정확
- [ ] ④~⑥ 정합성 충돌 3건 모두 감지 + 사용자 결정 요청 (임의 해소 없음)
- [ ] ⑦ 충돌 아닌 항목에도 관련 규칙 ID 참조 표기
- [ ] ⑧ 무응답 항목 TBD 처리 + 문서 하단 미결 목록 취합
- [ ] ⑨ `wiki/screens/EMP-001-사원관리.md` 생성, 템플릿 준수, REQ ID 부여
- [ ] ⑩ `wiki/_index.md` 화면 목록에 EMP-001 추가
- [ ] ⑪ 지어낸 요구사항 없음 (내가 말한 것과 화면의 사실만 기록됐는지 검수)

## 문제 발생 시

- **스킬이 안 뜨거나 무시됨** → CLAUDE.md가 폴더 루트에 있는지 확인. 그래도 안 되면 프롬프트에 "`.claude/skills/sero-requirement-interview/SKILL.md`를 읽고 그 절차대로 진행해줘"라고 명시 (이 문장은 항상 동작함)
- **미리보기가 빈 화면** → 다른 파일로 재시도, 앱 재시작. Cowork에서 지속되면 Code 탭으로 전환
- **충돌을 못 잡음** → 인터뷰 시작 전에 "wiki 공통규칙을 먼저 읽고 시작해줘"를 프롬프트에 추가. 반복되면 SKILL.md의 0-4단계 문구를 더 강하게 수정 (스킬 개선 포인트로 기록)

## 이 테스트가 검증하는 것 / 다음 단계

이 킷은 파이프라인의 **후반부(요구사항 정의)** 를 검증합니다. 통과하면 남는 선행 검증 2개:
1. 실제 SERO Vue 컴포넌트 5개로 `/design-sync` 임포트 품질 확인
2. Claude Design에서 실제 Export한 HTML에 컴포넌트 식별 정보(`data-*` 또는 class)가 보존되는지 확인 — 보존이 안 되면 디자인 시스템 컴포넌트에 `data-sero-component` 속성을 심는 규칙을 추가하는 것이 대응책
