# 미션 - 워들

## 🔍 진행 방식

- 미션은 **기능 요구 사항, 프로그래밍 요구 사항, 과제 진행 요구 사항** 세 가지로 구성되어 있다.
- 세 개의 요구 사항을 만족하기 위해 노력한다. 특히 기능을 구현하기 전에 기능 목록을 만들고, 기능 단위로 커밋 하는 방식으로 진행한다.
- 기능 요구 사항에 기재되지 않은 내용은 스스로 판단하여 구현한다.

---

## 기능 구현 사항
### view
* ConsoleInput.java
  - [x] '정답을 입력해 주세요.' 문구와 함께 단어를 입력받는다

* ConsoleOutput.java
  + [x] 글자 수가 5자가 아닌 경우
    + [x] '글자 수는 5글자여야 합니다.' 문구를 출력한다.
  + [x] 글자 수가 맞는 경우
    + WordPool 에 있는 단어인 경우
      + [x] 정답과 비교한 결과를 출력한다
    + WordPool 에 없는 단어인 경우
      + [x] '단어가 목록에 없습니다.' 문구를 출력한다.
  + [x] 입력된 단어가 영문자가 아닌 경우
    + [x] '영문자만 입력 가능합니다.' 문구를 출력한다.

### util
+ WordPoolGenerator.java : 유효 단어 목록 생성기
  - [x] 글자 수가 5자인 단어들의 리스트를 반환한다
+ WordValidator.java : 단어 검증기
  + [x] 글자 수가 5자가 아닌 경우를 검증한다
  + [x] 입력된 단어가 영문자인지를 검증한다
  + [x] WordPool 에 있는 단어인지를 검증한다

### model
* WordPool.java : words.txt에 있는 단어를 모두 들고 있는 객체
  * [x] 단어 리스트에서 (현재 날짜 - 2021년 6월 19일) % 배열의 크기) 번째의 단어를 반환한다

* Letter.java : 단어의 글자 1개를 가지고 있는 객체
  * [x] 입력된 단어가 유효한 알파벳인지 검증한다

* Word.java : 단어 리스트를 가지고 있는 객체(Letter[])
  * [x] 오늘의 정답 단어와 입력된 단어를 비교
    + [x] Alphabet, Letter가 저장된 배열의 인덱스를 비교해서 모두 일치하면 GREEN
    + [x] Alphabet 은 일치하는데 Letter가 저장된 배열의 인덱스가 일치하지 않으면 YELLOW
    + [x] Alphabet, Letter가 저장된 배열의 인덱스를 비교해서 모두 일치하지 않으며 GRAY

* Tiles.java : 오늘의 정답 단어와 입력된 단어를 비교한 결과 데이터를 가지고 있는 객체

* TileStatus : 판별 결과 enum(초록색, 노란색, 회색)
  
* Grid.java : 오늘의 정답 단어와 입력된 단어를 비교한 결과 데이터인 Tiles를 모두 가지고 있는 객체

* Game.java : 오늘의 정답 단어(Word), 비교 결과(Grid), 현재 입력 카운트 정보를 가지고 있는 객체
  * [x] 오늘의 정답 단어와 입력된 단어를 비교한 결과
    * [x] 정답이면 게임을 종료한다.
    * [x] 6번 이내로 정답을 맞추지 못한 경우 게임을 종료한다.
  * [x] 현재 입력이 몇번 진행되었는지 계산한다.

### 고민했던 부분
+ Validator를 어디에 위치시킬 것인가?
  + Controller, Word 생성 메서드
+ Validator 내 validate 메서드 외 private 메서드를 private으로 가져가도 되는지?
+ Letter 내 Alphabet, Position 가 과도하게 객체를 쪼갠 것인지?
---

## 🚀 기능 요구 사항

선풍적인 인기를 끌었던 영어 단어 맞추기 게임이다.

- 6x5 격자를 통해서 5글자 단어를 6번 만에 추측한다.
- 플레이어가 답안을 제출하면 프로그램이 정답과 제출된 단어의 각 알파벳 종류와 위치를 비교해 판별한다.
- 판별 결과는 흰색의 타일이 세 가지 색(초록색/노란색/회색) 중 하나로 바뀌면서 표현된다.
    - 맞는 글자는 초록색, 위치가 틀리면 노란색, 없으면 회색
    - 두 개의 동일한 문자를 입력하고 그중 하나가 회색으로 표시되면 해당 문자 중 하나만 최종 단어에 나타난다.
- 정답과 답안은 `words.txt`에 존재하는 단어여야 한다.
- 정답은 매일 바뀌며 ((현재 날짜 - 2021년 6월 19일) % 배열의 크기) 번째의 단어이다.

### 입출력 요구 사항

#### 실행 결과 예시

```light
WORDLE을 6번 만에 맞춰 보세요.
시도의 결과는 타일의 색 변화로 나타납니다.
정답을 입력해 주세요.
hello

⬜⬜🟨🟩⬜

정답을 입력해 주세요.
label

⬜⬜🟨🟩⬜
🟨⬜⬜⬜🟩

정답을 입력해 주세요.
spell

⬜⬜🟨🟩⬜
🟨⬜⬜⬜🟩
🟩🟩⬜🟩🟩

정답을 입력해 주세요.
spill

4/6

⬜⬜🟨🟩⬜
🟨⬜⬜⬜🟩
🟩🟩⬜🟩🟩
🟩🟩🟩🟩🟩
```

---

## 🎯 프로그래밍 요구 사항

- JDK 11 버전에서 실행 가능해야 한다. **JDK 11에서 정상적으로 동작하지 않을 경우 0점 처리한다.**
- 프로그램 실행의 시작점은 `wordle.Application`의 `main()`이다.
- [Java 코드 컨벤션](https://github.com/woowacourse/woowacourse-docs/tree/master/styleguide/java) 가이드를 준수하며 프로그래밍한다.
- 프로그래밍 요구 사항에서 별도의 변경 불가 안내가 없는 한 자유롭게 파일을 수정하고 패키지를 이동할 수 있다.
- indent(인덴트, 들여쓰기) depth를 3이 넘지 않도록 구현한다. 2까지만 허용한다.
    - 예를 들어 while문 안에 if문이 있으면 들여쓰기는 2이다.
    - 힌트: indent(인덴트, 들여쓰기) depth를 줄이는 좋은 방법은 함수(또는 메서드)를 분리하면 된다.
- 3항 연산자를 쓰지 않는다.
- 함수(또는 메서드)의 길이가 15라인을 넘어가지 않도록 구현한다.
    - 함수(또는 메서드)가 한 가지 일만 잘 하도록 구현한다.
- else 예약어를 쓰지 않는다.
    - 힌트: if 조건절에서 값을 return하는 방식으로 구현하면 else를 사용하지 않아도 된다.
    - else를 쓰지 말라고 하니 switch/case로 구현하는 경우가 있는데 switch/case도 허용하지 않는다.
