# RollCraft 팀 규칙: Git · PR

`main`은 항상 `npm run dev`로 실행되는 상태를 유지해요. 자기 담당 폴더의 작업은 `main`에 바로 푸시해도 되고, 남의 폴더나 공용 계약을 건드릴 때는 브랜치에서 작업해 PR로 합쳐요.

## 0. main에 직접 푸시할 때

- 자기 담당 폴더나 문서만 고쳤을 때 바로 푸시해도 돼요.
- 푸시 전에 `npm run build`와 `npm run format`을 해요.
- 푸시 전에 항상 최신 `main`을 받아요. 아직 푸시하지 않은 내 커밋이라 rebase해도 괜찮아요.

```bash
git pull --rebase
git push
```

- 큰 기능이나 오래 걸리는 작업은 아래 규칙대로 브랜치와 PR을 써요.

## 1. 브랜치

- 이름은 `<영역>/<작업>` 형식으로 지어요. 소문자와 `-`만 써요.
- 영역: `physics`, `editor`, `render`, `ui`, `stage`, `shared`, `docs`, `chore`
- 예: `physics/frustum-collider`, `editor/free-draw`, `stage/split-rail`
- 브랜치 하나에는 작업 하나만 담아요. 다른 작업은 새 브랜치를 만들어요.

```bash
git switch main && git pull          # 작업 시작 전 항상 최신으로
git switch -c physics/frustum-collider
```

## 2. 커밋 메시지

`[타입] 요약` 형식으로 써요. 요약은 한국어로 짧게 써요.

| 타입         | 언제                       | 예                                   |
| ------------ | -------------------------- | ------------------------------------ |
| `[feat]`     | 새 기능                    | `[feat] 원뿔 compound 콜라이더 추가` |
| `[fix]`      | 버그 수정                  | `[fix] 골 판정 박스 위치 수정`       |
| `[refactor]` | 동작은 같고 구조만 바꿈    | `[refactor] 리샘플 로직 분리`        |
| `[docs]`     | 문서                       | `[docs] TeamRules 추가`              |
| `[chore]`    | 설정, 패키지, 포맷 등 잡일 | `[chore] three 버전 올림`            |

- 커밋 하나에는 변경 하나만 담아요. "이것저것 수정" 같은 커밋은 만들지 않아요.

## 3. PR

**올리기 전**

- `npm run build`가 통과하는지 확인해요.
- `npm run format`으로 포맷을 맞춰요.
- 브라우저에서 직접 띄워 봐요(전체 게임 또는 자기 모듈의 `dev.html`).

**작성**

- 제목은 커밋 메시지 형식을 따라요(예: `[feat] 원뿔 compound 콜라이더 추가`).
- 본문에는 세 가지를 적어요: 무엇을 바꿨는지, 어떻게 확인했는지, 화면이 바뀌었다면 스크린샷이나 GIF.
- PR은 작게 만들어요. 하루 안에 끝나는 크기가 기준이에요.

**리뷰와 머지**

- 승인은 필수가 아니에요. 자기 담당 폴더만 고쳤다면 본인이 머지해도 돼요.
- 남의 담당 폴더를 건드렸다면 그 담당자의 승인을 받고 머지해요. 담당은 `.github/CODEOWNERS`에 있어요.
- 머지는 **Squash and merge**로 해요. `main`에는 PR 하나당 커밋 하나가 남아요.
- 머지한 브랜치는 삭제해요.

## 4. main 따라가기와 충돌

- 브랜치가 오래 살아 있으면 중간중간 `main`을 합쳐요.

```bash
git fetch origin
git merge origin/main
```

- 이미 푸시한 브랜치에 `rebase`는 쓰지 않아요. 다시 올리려면 force push가 필요해서예요.
- 남의 파일에서 충돌이 나면 혼자 고치지 말고 그 파일 담당자와 같이 풀어요.

## 5. 공용 계약 변경

`src/shared/contracts.js`, `src/shared/events.js`는 세 명의 코드가 모두 기대는 파일이에요.

1. 단톡방에서 무엇을 왜 바꾸는지 말하고 3명이 합의해요.
2. 그 파일만 고친 작은 PR을 올려요.
3. 3명 모두 승인하면 머지해요.
4. 머지했다고 단톡방에 알려요. 각자 `git pull`로 받아요.

## 6. 금지

- `git push --force`: 다른 사람의 커밋이 사라질 수 있어요.
- 공용 계약을 혼자 바꾸기
- `node_modules/`, `dist/`, `.env` 커밋하기(`.gitignore`가 막아 주지만 `git add -f`는 쓰지 않아요)
