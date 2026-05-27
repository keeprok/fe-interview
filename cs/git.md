# 🔀 Git

## 목차
- [branch merge 전략](#branch-merge-전략)
- [Git Flow 브랜치 전략](#git-flow-브랜치-전략)

---

## branch merge 전략

> Git에서 branch merge 전략 3가지를 설명해주세요.

<details>
<summary>답변 보기</summary>

**1. Merge (일반 머지)**
```
main:    A---B---C---M
              \     /
feature:       D---E
```
- feature 브랜치의 커밋 히스토리가 그대로 보존
- 머지 커밋(M)이 생성됨
- **장점**: 전체 히스토리 추적 가능
- **단점**: 히스토리가 복잡해짐

**2. Squash Merge**
```
main:    A---B---C---S (D+E를 하나의 커밋으로 합침)
```
- feature 브랜치의 모든 커밋을 하나로 합쳐서 머지
- **장점**: 히스토리 깔끔
- **단점**: 세부 커밋 히스토리 손실

**3. Rebase Merge**
```
main:    A---B---C---D'---E' (D, E를 main 위에 재배치)
```
- feature 브랜치 커밋을 base 브랜치 위에 재배치
- **장점**: 선형적인 히스토리
- **단점**: 커밋 해시 변경, 공유 브랜치에 사용 위험

</details>

---

## Git Flow 브랜치 전략

> Git Flow 브랜치 전략에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

Git Flow는 **Vincent Driessen**이 제안한 브랜치 관리 모델입니다.

**주요 브랜치:**
| 브랜치 | 역할 |
|--------|------|
| `main` | 프로덕션 배포 코드 |
| `develop` | 다음 릴리즈 개발 통합 |
| `feature/*` | 기능 개발 (develop에서 분기) |
| `release/*` | 릴리즈 준비 (develop → main) |
| `hotfix/*` | 긴급 버그 수정 (main에서 분기) |

**흐름:**
1. `develop`에서 `feature` 브랜치 생성
2. 기능 완성 후 `develop`에 머지
3. 릴리즈 준비 시 `release` 브랜치 생성
4. 최종 확인 후 `main`과 `develop` 양쪽에 머지
5. 핫픽스는 `main`에서 바로 분기 후 수정, `main`과 `develop`에 머지

**GitHub Flow (단순 버전):** `main` + `feature` 브랜치만 사용, 빠른 배포 주기에 적합

</details>
