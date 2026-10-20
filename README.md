# Feature Flow

**설계는 함께 결정하고, 구현·독립 리뷰를 거쳐 사람이 중요한 코드를 직접 검수하는 Claude Code·Codex 공통 스킬입니다.**

```text
설계 논의 → 구현 → 독립 리뷰 2개 → 위험별 사용자 검토 → draft PR
```

- 설계에서 상태·실패·재진입과 기존 코드 재사용을 확인합니다.
- 독립 리뷰는 설계·응집성·결합도와 구현·가독성·예측 가능성을 나눠 봅니다.
- 최종 검수는 순차 강의 대신 **위험 → 확인할 코드 → 검증 근거 → 남은 판단**으로 진행합니다.
- 이미 합의한 정책을 반복하지 않고, 전체 변경 목록에서 중요한 경로를 깊게 읽도록 안내합니다.
- 테스트 통과와 실제 연동 검증을 구분합니다. 승인 전 PR 생성이나 자동 병합을 허용하는 스킬이 아닙니다.
- 단순 문구·스타일 수정에는 축소된 절차를 적용합니다.

실행 규칙은 [SKILL.md](SKILL.md)에 있습니다. 코드·GitHub·독립 에이전트 도구는 실행 환경에서 제공해야 하며, 설치만으로 권한이 생기지 않습니다.

## 설치

Claude 또는 Codex에게 이 저장소를 전달하세요.

```text
https://github.com/joonseong11/feature-flow
README를 읽고 feature-flow 스킬을 설치해줘.
공통 원본은 ~/.agents/skills/feature-flow에 두고,
Claude와 Codex의 스킬 경로에서 같은 원본을 참조하게 해줘.
이미 설치돼 있으면 덮어쓰지 말고 기존 내용·Git 원격과 차이를 확인해줘.
지금은 설치만 하고 제품 코드나 PR은 변경하지 마.
```

새 환경에서 직접 설치하는 예시(macOS/Linux):

```bash
mkdir -p ~/.agents/skills ~/.claude/skills ~/.codex/skills
git clone https://github.com/joonseong11/feature-flow.git ~/.agents/skills/feature-flow
```

위 clone이 성공한 뒤, 기존 경로를 보존하며 링크를 만듭니다.

```bash
for tool in .claude .codex; do
  destination="$HOME/$tool/skills/feature-flow"
  if [ -e "$destination" ] || [ -L "$destination" ]; then
    printf '기존 경로 유지: %s\n' "$destination"
  else
    ln -s ../../.agents/skills/feature-flow "$destination"
  fi
done
```

사용 중인 앱이 별도의 스킬 루트를 사용한다면 그 환경의 설치 경로를 따르세요. 설치 후 새 세션에서 스킬이 보이는지 확인합니다.

## 사용

- Codex: `$feature-flow`와 작업 요청
- Claude Code: `/feature-flow`와 작업 요청

예: “feature-flow로 예약 목록 필터 변경을 설계부터 진행해줘.”

사람의 승인은 **설계 확정과 최종 검토**에서 받습니다. 중요한 정책이 구현 중 새로 발견되면 설계 논의로 돌아갑니다. 긴 설명을 먼저 듣는 대신 코드를 열고 막히는 부분을 질문하며 검수합니다.

## 버전 관리

- 현재 최초 배포: **v0.1.0** — 현재 합의한 스킬의 기준점입니다. 이전 대화나 편집 이력을 소급해 만든 버전이 아닙니다.
- `main`: 최신 변경. 태그: 배포 시점의 고정 버전.
- 변경 시 `SKILL.md` 수정 → 검증 → `CHANGELOG.md` 기록 → 커밋·푸시 순서로 관리합니다.
- 호환되는 개선은 minor, 문구·오류 수정은 patch, 기존 사용 방식이 달라지는 변경은 변경 내역에 명시하고 버전을 결정합니다.
- 실제 설치 폴더를 clone으로 관리하면 Claude·Codex가 같은 변경을 사용합니다. 설치된 스킬은 자동 업데이트되지 않습니다.

업데이트 전 로컬 수정과 원격을 확인합니다.

```bash
git -C ~/.agents/skills/feature-flow status --short
git -C ~/.agents/skills/feature-flow remote -v
git -C ~/.agents/skills/feature-flow pull --ff-only
```

로컬 수정이 있으면 먼저 보존·검토하세요. `--ff-only`가 실패하면 강제 초기화하지 말고 변경 이력을 비교합니다. 실행 중인 세션에는 갱신된 스킬을 다시 읽도록 요청합니다.

## 관련 스킬

[Hermes Contribution Flow](https://github.com/joonseong11/hermes-contribution-flow)는 비개발자 요청자와 개발자 검토자를 분리한 Hermes용 독립 스킬입니다. 두 저장소는 자동 동기화되지 않습니다.
