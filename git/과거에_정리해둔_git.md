# 과거에 공부하던 git 자료 합본 -> 좀 이상한것도 있고, 다시 정리해야지(말만하지 말고 어서 해)
reference site : https://git-scm.com/book/ko/v2


1. git repository는 local repository와 remote repository가 있다. local repository는 내 컴퓨터에서 일어나는 일과 관련있고, remote repository는 말 그대로 원격 저장소이다.


2. working tree, Staging Area(index), git directory(repository)
ref) https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EA%B8%B0%EC%B4%88

    - Staging Area : 곧 커밋할 파일에 대한 정보를 저장. Git 기술용어로는 "Index".

        [working tree] -"git add"-> [staging area(index)] -"git commit"-> [repository]
   
        ex) 수정하던 내용 10개 중 7개만 commit하고 싶다면 7개만 index에 add 하고 commit

    - 기본 흐름
        1) 파일 수정 : working tree
        2) add : staging area에 파일을 stage해서 commit할 스냅샷을 만듬.
        3) commit : git repository에 영구적인 스냅샷으로 저장.


3. commit : 파일 및 폴더의 추가/변경 사항의 기록. (시간 순으로)

        ex)git commit -m "commit message"


4. push : local repository -> remote repository


5. clone, pull : remote repository -> local repository
    1) clone : 아무것도 없을 때 내려받는 것, first pull 정도
    2) pull : remote repository의 데이터를 로컬에 적용
    - fetch 개념은 나중에


6. merge 
    - 내가 pull 한 이후에 remote repo 가 변경 되었을 경우(다른사람이 push해서) push를 하면 요청 거부 됨. -> merge를 통해 다른 사람의 업데이트 이력을 내 local repo에도 갱신해야 함.


7. conflict : 
    - merge는 변경 부분을 자동으로 통합해주는 기능.
    - 원격 저장소와 로컬 저장소 양쪽에서 파일의 동일한 부분을 변경할 때, merge 안 됨 = conflict -> 직접 수정해야 함.

8. branch
    - 여러 개발자들이 동시에 서로 다른 작업을 할 수 있게 만들어 주는 기능
    - 각자 독립적인 work tree에서 소스를 변경하고, 나중에 원래의 버전과 비교해서 하나의 새 버전을 만든다.(merge)

        1) 메인 branch에서 자신의 작업 전용 branch 생성
        2) 각자 작업
        3) 작업 끝난 사람은 메인 branch에 자신의 branch를 merge

    - 작업 단위, branch로 작업의 기록을 중간 중간에 남기므로, 문제가 발생했을 때 원인을 찾아서 대책을 세우기 쉬워짐.

    1) master branch : 저장소를 처음 만들면 git은 master라는 이름의 branch를 만듬.
    2) other branch example
        - integration branch : 배포 가능한 안정적인 branch. 일반적으로 master branch를 통합 브랜치로 사용
        - topic branch : 기능 추가, 버그 수정 등의 단위 작업을 하는 branch


9. checkout : branch 전환

10. head : 현재 사용 중인 branch의 선두 부분.

11. stash
    - 변경 내용을 commit하지 않고 다른 branch로 checkout하면, 그 변경 내용은 전환된 branch에서 commit가능.
    - 단, commit할 내용이 전환된 branch에서도 변경된 파일이면 checkout 실패. -> stash를 이용해 일시적으로 변경 내용을 임시로 저장. stash란 파일의 변경 내용을 일시적으로 기록해두는 영역. stash에 저장된 변경 내용은 나중에 다시 불러와 원래의 branch나 다른 branch에 commit 가능.


12. merge, rebase : 작업이 완료된 토픽 branch를 통합 branch에 통합하는 방법은 merge와 rebase 두가지. 어느 쪽을 사용하느냐에 따라 통합 후의 branch 이력이 달라짐.

    1) merge : 여러 개의 branch를 하나로 모을 수 있음.
        
        master branch에서 분기한 bugfix branch.
        "수정중"

        1) bugfix를 master로 병합할 때, master 상태가 이전부터 변경되지 않았다면 쉽게 merge됨. master branch의 head가 bugfix branch의 head와 같아짐 = fast-forward merge

        2) master가 변경된 경우 merge commit을 실행하게 됨. 새로운 commit이 생기고, 이 commit을 master branch의 head가 가리키게 됨.

        * fast-forward merge일 때도, non fast-forward 옵션을 사용할 수 있음. -> branch가 그대로 남기 때문에 branch 관리면에서 유용할 수 있음.

    2) rebase : non fast-forward merge일 때, master branch commit 이후에 bugfix commit이 이동하여, 하나의 줄기로 이어짐.

        * 이 때, master와 bugfix에 충돌하는 부분이 있으면 수정해야 함. master branch의 head는 위치 이동 하지 않음. -> master branch의 head를 이동된 bugfix branch의 head로 이동하기 위해서는 fast-forward merge를 통해 master branch head도 이동시켜야 함.



    3) 정리
        - merge는 변경 내용 이력이 모두 남아 이력이 복잡해짐. 
        - rebase는 이력은 단순해지지만, 원래 커밋 이력이 변경됨.

        * (master) git merge sub : master branch에 sub branch를 병합
        * (sub) git rebase master : sub commit history를 master branch head 이후에 붙임
        ** 이 과정에 conflict 있으면 수정해야함. 
            수정 후에는 git rebase --continue

    * 참고 : https://backlog.com/git-tutorial/kr/stepup/stepup1_4.html

    4) 사용 예
        * 토픽 branch에 통합 branch의 최신 코드를 적용할 때는 rebase
        * 통합 branch에 토픽 branch를 불러올 경우에는 우선 rebase를 한 후 merge
        * 참고 : https://backlog.com/git-tutorial/kr/stepup/stepup1_5.html
        * 참고2 : https://nvie.com/posts/a-successful-git-branching-model/


13. git exercise
    ```bash
    git init
    echo "원숭이도 이해하는 git" > myfile.txt
    git add myfile.txt
    git commit -m "first commit"

    git branch issue1
    git branch
    git checkout issue1    // issue1 branch 사용

    echo "add : 변경 사항 만들어서 인덱스에 등록하기" >> myfile.txt
    git add myfile.txt
    git commit -m "add 설명 추가"

    git checkout master    // HEAD는 현재 사용중이 branch에 위치
    git merge issue1       // 병합할 커밋 이름을 넣어 실행하면, 지정한 커밋 내용이 HEAD가 가리키고 있는 branch에 넣어짐.

    git branch -d issue1    // branch 삭제
    git branch

    git branch issue2
    git branch issue3
    git checkout issue2

    echo "commit : 인덱스 상태를 기록" >> myfile.txt
    git add myfile.txt
    git commit -m "commit의 설명 추가"

    git checkout issue3
    echo "pull : 원격 저장소의 내용을 가져오기" >> myfile.txt
    git add myfile.txt
    git commit -m "pull 설명 추가"

    git checkout master
    git merge issue2
    // 위는 fast-forward merge

    git merge issue3    // conflict 발생
    vi myfile.txt       // myfile 수정
    git add myfile.txt
    git commit -m "issue3 merge"
    // non fast-forward merge

    git reset --hard HEAD~    // HEAD~ : HEAD의 바로 전 commit으로 상태 돌림.
    git checkout issue3
    git rebase master            // merge와 마찬가지로 conflict 발생
    vi myfile.txt
    git add myfile.txt
    git rebase --continue        // rebase conflict는 commit하면 안됨.
    // *git rebase --abort : rebase 취소
    // issue3 branch가 master, issue2 앞으로 위치가 옮겨졌을 뿐, master branch는 issue3의 변경사항이 적용되지 않은 상태

    git checkout master
    git merge issue3            // master에 issue3를 병합
    ```


14. git exercise 2 - remote
    - git pull    // 원격 저장소의 내용을 가져와 + 자동으로 병합 작업 실행
    - git fetch   // 원격 저장소의 내용을 확인만 하고 - 자동으로 병합 X
    * fetch를 실행해서 가져온 최신 커밋 이력은 FETCH_HEAD 이름으로 checkout 가능

    git pull = git fetch + git merge FETCH_HEAD
    
    git push    // local -> remote


15. tag
    - 커밋을 참조하기 쉽도록 알기 쉬운 이름을 붙이는 것
    - 한 번 붙인 태그는 브랜치처럼 위치가 이동하지 않고 고정됨
    - 태그(lightweight tag) : 이름만
    - 주석태그(annotated tag) : 이름, 설명, 서명, 만든사람의 이름,이메일,날짜
    ```bash
    echo "git remote study" > file.txt
    git add file.txt
    git commit -m "first commit"

    git tag apple    // 현재 HEAD가 가리키고 있는 커밋에 tag 추가
    git tag
    git log --decorate    // 태그 정보를 포함한 이력본다는데 git log랑 같은데?

    git tag -a banana    // 주석태그 추가
    git tag -am "주석 내용 바로 입력" banana // 주석내용도 바로 입력
    git tag -n    // 태그 목록과 주석 내용을 확인

    git tag -d apple    // 태그 삭제
    ```


16. --amend : 커밋 변경하기
    - 이전에 커밋했던 내용에 새로운 내용을 추가하거나 설명 수정 가능
    - 누락된 파일을 새로 추가하거나 기존의 파일을 업데이트 할 때
    - 이전 커밋의 설명을 변경할 때


17. revert : 특정 커밋의 내용 삭제
    
    rebase -i / reset 또한 삭제이지만
    
    revert 명령어를 이용하면 특정 커밋의 내용을 지우는 새로운 커밋을 만들어 안전하게 처리할 수 있음


18. reset : 필요 없어진 커밋들을 버릴 수 있음.
    
    명령어 실행 시 모드를 지정하여 HEAD 위치, index, work tree 내용을 되돌릴지 여부 선택 가능.

    | 모드명 | HEAD 위치 | 인덱스     | 작업 트리 |
    |-------|----------|-----------|----------|
    | soft  | 변경함    | 변경 안 함 | 변경 안 함 |
    | mixed | 변경함    | 변경함     | 변경 안 함 |
    | hard  | 변경함    | 변경함     | 변경함     |
 
    - soft : 커밋만 되돌리고 싶을 때
    - mixed : 변경한 인덱스의 상태를 원래대로 되돌리고 싶을 때
    - hard : 최근의 커밋을 완전히 버리고 이전의 상태도 되돌리고 싶을 때


19. cherry-pick : 다른 브랜치에서 지정한 커밋을 복사하여 현재 브랜치로 가져올 수 있음
    - 특정 브랜치에 잘못 추가한 커밋을 올바른 브랜치로 옮길 때
    - 다른 브랜치의 커밋을 현재 브랜치에도 추가하고 싶을 때
    - 이 부분은 실습이 필요한데?


20. rebase + i 옵션

    커밋을 다시 쓰거나,

    다른 커밋과 바꿔 넣거나

    특정 위치의 커밋을 삭제하거나

    여러 커밋을 하나로 통합할 수 있다.


21. merge --squash

    해당 브랜치의 커밋 전체를 통합한 커밋이 추가됨.

    토픽 브랜치 안의 커밋을 한꺼번에 모아서 토합 브랜치에 병합하고자 할 때 사용


22. git --amend

    기존의 commit에 파일을 추가할 수 있고,

    기존 commit에 포함된 파일을 삭제도 되고
    
    commit 메세지 수정도 됨.


23. git revert HEAD : commit을 취소하는 commit을 추가


24. git reset
    - git reset --hard HEAD~~    : 현재 commit의 두개 전으로
    - git reset --hard ORIG_HEAD    : reset 전의 commit은 ORIG_HEAD로 참조가능. 이것도 reset


25. git cherry-pick picked_commit

    체피킹할 커밋만 가져옴.

    conflict 발생하면 수정하고

    git add samplt.txt

    git commit

    하면 됨


27. rebase -i : 커밋 통합하기
    // 음.. 이건 좀 어려운데?

    https://git-scm.com/book/ko/v1/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0


    - rebase -i
        - git rebase --abort : rebase 작업 중지
        - git reset --hard ORIG_HEAD : rebase 전의 상태로 돌림


28. merge --squash : merge 대상이 되는 branch의 모든 커밋을 하나의 커밋으로 병합하여 merge

    ```bash
    git merge --squash issue1
    vi sample.txt
    git add sample.txt
    git commit
    ```