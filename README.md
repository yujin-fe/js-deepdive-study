# 🦎 JS-Deep-Dive Study 기록
### 💾 레포지토리 사용목적
모던 자바스크립트 딥다이브 책스터디에 대한 기록 및 회고와 관련 추가 공부에 대한 주차별 학습 기록에 대한 월별 아카이브
## 🪾 브랜치 생성 전략
1. 각 주차별 브랜치를 생성한다.
2. 이번주 브랜치에서 각자 본인이름 브랜치를 생성하고 본인이름으로 디렉토리 만들어서 작업한다.   
  예)`sep-1st`브랜치에서 `yujin`브랜치 생성 `yujin` 폴더에서 작업
3. 작업 내용을 이번주 브랜치에 피알하면 서로 확인하고 이번주 브랜치에 머지(squash and merge)한다.
4. 위 과정을 4주 반복하고 마지막주 머지까지 완료했으면 첫째주부터 마지막주까지의 브랜치를 이번달 브랜치에 squash and merge한다.
5. 9월 브랜치에 머지한 후에는 9월의 1주~4주차 브랜치는 삭제한다.
6. 스터디가 모두 끝나면 메인 아래 월별로 4개의 브랜치만 남을 예정
7. 번외로 회의록은 md파일로 제작 후 주차별 브랜치에 바로 푸쉬한다.  
### 📤 주차별 브랜치 작업시
```css
main
 ┣ sep-1st/
 │   ┣ yujin/
 │   ┣ eunji/
 │   ┣ hyeok/
 │   ┗ gihun/
 ┣ sep-2nd/
 │   ┣ yujin/
 │   ┣ eunji/
 │   ┣ hyeok/
 │   ┗ gihun/
 │...
 ┣ sep
 ┣ oct
 ┣ nov
 ┣ dec 
 ┗ README.md
 ```

### 🗓️ 월별 브랜치에 머지한 후
```css
main
  (9월의 주차별 브랜치는 삭제)
 ┣ oct-1st/
 │   ┣ yujin/
 │   ┣ eunji/
 │   ┣ hyeok/
 │   ┗ gihun/
 ┣ oct-2nd/
 │   ┣ yujin/
 │   ┣ eunji/
 │   ┣ hyeok/
 │   ┗ gihun/
 │...
 ┣ sep(아래는 디렉토리명)
      ┣ sep-1st
            ┣ yujin/
            ┣ eunji/
            ┣ hyeok/
            ┗ gihun/ 
      ┣ sep-2nd
      ┣ sep-3rd
      ┗ sep-4th
 ┣ oct   
 ┣ nov
 ┣ dec 
 ┗ README.md
 ```
## 머지 시 커밋 메시지 작성 규칙
PR을 `Squash and Merge`할 때 최종 커밋 메시지는 다음 규칙을 따릅니다.   
* 형식: `[주차] 이름 - 학습 정리`
* 예시: `[sep-1st] 유진 - 학습 정리`

## 📝 레포지토리 사용 방법
1. 저장소 클론
    ```bash
    git clone https://github.com/yujin-fe/js-deepdive-study.git
    cd js-deepdive-study
    ```
2. `fetch` 후 주차별 브랜치로 이동, 주차별 디렉토리로 이동
    ```bash
    git fetch origin
    ```
    1) 클론할 당시 없던 브랜치에 접근하려면 
        ```
        git checkout -b sep-1st origin/sep-1st
        cd sep-1st
        ```
    2) 클론할 때 있던 브랜치라면(git branch에서 확인 가능)
        ```
        git checkout sep-1st
        cd sep-1st
        ```
3. 주차별 브랜치 안에서 본인이름 브랜치 생성, 주차별 디렉토리 안에서 본인이름 디렉토리 생성
    ```bash
    git checkout -b yujin
    mkdir yujin (yujin 폴더가 sep-1st 내부에 있어야함.)
    cd yujin
    ```
4. 작업 후 푸쉬(본인이름 브랜치로), PR(주차별 브랜치로) 올리기
    ```bash
    git add [파일이름] or .
    git commit -m "[sep-1st]유진-학습 정리"
    git push -u origin yujin (최초 푸쉬시 왼쪽처럼 이후부터는 그냥 git push)
    ```
5. PR 요청   
  Base:주차별 브랜치(예:sep-1st)  
  Compare: 본인 브랜치(예:yujin)
6. 주차별 브랜치에서 월별 브랜치로 머지하면 본인이름 브랜치 삭제하기
    ```bash
      git branch -D yujin
    ```
