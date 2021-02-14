# Github Branch Protection Rules
Github에서 제공하는 `Branch protection rules`을 통해 어떤 브랜치에 대한 행위를 보호할 수 있다.
[About protected branches](https://docs.github.com/en/github/administering-a-repository/about-protected-branches) 에 따르면 `public repositories`에 대해서는 사용 제한이 없으며, `private repositories`의 경우 유료 사용자(Pro, Team, Enterprise Cloud, Enterprise Server)에게만 제공된다.
private repo의 경우 git에서 직접 특정 브랜치(master, main)에 바로 push하는 것을 막을 수 있다.

### Branch Protection rule
repositroy의 Settings - Branches에서 protection rule을 설정할 수 있다.
force push, delete, PR 전 리뷰, commit, linear history 등에 대한 행위를 설정할 수 있다.

### Pre-push hook 
`git templates`을 응용하여 특정 브랜치에 push하는 것을 막을 수 있다.
[Git pre-push hook](https://gist.github.com/ColCh/9d48693276aac50cac37a9fce23f9bda) 을 참고하도록 하자.