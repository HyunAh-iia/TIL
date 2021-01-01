# SSH 파일 업로드 시 패스워드 입력하기

SSH를 통해 서버로 파일 업로드 시, 패스워드를 입력해야하는 경우가 있다. 해당 예제는 macOS 기반으로 작성되었다.

 설치
---
- macOS용 패키지 관리자 `homebrew` 설치 (이미 설치되어 있다면 스킵)
    ```shell script
    $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
- sshpass 다운로드

    Homebrew는 기본적으로 `sshpass` 설치를 허락하지 않으므로 비공식 패키지를 다운로드받는다.
    ```shell script
    $ brew install hudochenkov/sshpass/sshpass
    혹은 
    $ brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb
    ```

SSH 파일 업로드
---
```shell script
$ sshpass -p '패스워드' scp -o StrictHostKeyChecking=no -P 포트번호 /로컬위치 /리모트위치
```

경험했던 오류 메시지
---
- `Host key verification failed.`
    - 패스워드를 감싼 `''`을 이해하지 못함
    - `""`는 그냥 아무것도 안하고 스킵됨
    - `-o` 옵션을 주어 해결
- `No such file or directory`
    - 이 에러는 정말 이해할 수 없지만 명령어 순서를 다르게 조합하였더니 해결함
    - 기본적으로 명령어 순서는 `/로컬위치 /리모트위치` 혹은 `/리모트위치 /로컬위치`로 순서 조합가능함(로컬위치는 다른 서버를 의미하기도 함)
    - scp에 대한 옵션 설명을 읽으면 명령어 순들을 이해할 수 있음 [링크](https://itgameworld.tistory.com/m/11)
    - [스택오버플로우 링크](https://stackoverflow.com/questions/50096/how-to-pass-password-to-scp)  
- `파일 is Not a directory`
    - 도착 주소(예시에서는 리모트)의 주소가 `/`로 끝나면 위와 같이 명확하지 않은 에러메시지를 리턴
    - 에러와 메시지가 일치하지 않는 버그
    - Bug 1768 - scp: wrong error message when destination directory ends with a slash and is missing
    - [스택오버플로우 링크](https://stackoverflow.com/questions/38628550/scp-copying-error-not-a-directory)
    - [관련 버그번호](https://bugzilla.mindrot.org/show_bug.cgi?id=1768)
