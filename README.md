# mins-skills

AI 코딩 에이전트를 위한 스킬 모음.

프로젝트 세팅부터 개발 가이드라인까지, 반복되는 작업을 표준화하는 스킬을 만들고 공유합니다.

## 설치

```bash
# 전체 스킬 설치
npx skills add {username}/mins-skills

# 특정 스킬만 설치
npx skills add {username}/mins-skills --skill project-harness-generator

# 특정 에이전트에만 설치
npx skills add {username}/mins-skills --skill project-harness-generator -a claude-code
npx skills add {username}/mins-skills --skill project-harness-generator -a codex
```

## 스킬 목록

### [project-harness-generator](./project-harness-generator/)

프로젝트 한 줄 설명 → 에이전트가 바로 일할 수 있는 문서 세트를 생성합니다.

**생성되는 문서:**

| 파일 | 역할 |
|------|------|
| `PROJECT-SPEC.md` | 기능 목록, 데이터 모델, 스프린트 계획 |
| `docs/architecture.md` | 시스템 개요, 도메인 맵, 의존성 방향 |
| `docs/conventions.md` | 네이밍, 코드 구조, Git 컨벤션 |
| `docs/api-design.md` | 응답 포맷, 상태 코드, 에러 코드 |
| `docs/quality.md` | 4축 QA 체크리스트, 스프린트 계약 템플릿 |

선택적으로 에이전트 진입점 파일도 생성 가능: `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, `.cursorrules` 등.

**사용 예시:**

```
> Bootstrap a project harness for an order management API using Spring Boot + PostgreSQL, deployed on AWS
```

자세한 내용은 [project-harness-generator/SKILL.md](./project-harness-generator/SKILL.md)를 참고하세요.


## 라이선스

MIT
