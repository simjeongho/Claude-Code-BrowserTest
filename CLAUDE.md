# 작업 폴더 지침 (SERO 요구사항 정의)

이 폴더는 Claude Design에서 Export한 화면(screens/)에 대한 요구사항 정의 작업 공간이다.

## 필수 규칙
1. 사용자가 화면에 대한 **요구사항 정의/인터뷰/분석**을 요청하면, 반드시
   `.claude/skills/sero-requirement-interview/SKILL.md`를 먼저 읽고 그 절차를 그대로 따른다.
2. 요구사항을 기록하기 전에 항상 `wiki/_index.md` → 관련 문서 순으로 읽고 정합성을 검증한다.
3. wiki/의 기존 문서는 사용자 확인 없이 수정하지 않는다.

## 폴더 구조
- screens/   : Claude Design Export HTML (읽기 전용으로 취급)
- manifest/  : 화면별 컴포넌트 매니페스트(JSON) — 스킬이 생성
- wiki/      : LLM-Wiki (공통 규칙 + 화면별 요구사항 문서) — 스킬의 산출물 저장 위치
