# :black_joker: 블랙잭 게임 도메인 구현 

### Contents

> 1. [Intro](https://github.com/bky373/kotlin-blackjack#intro)
> 2. [블랙잭 게임 코드](https://github.com/bky373/kotlin-blackjack#블랙잭-게임-코드)
> 3. [다른 게임 코드](https://github.com/bky373/kotlin-blackjack#다른-게임-코드)
> 4. [블랙잭 기본 규칙](https://github.com/bky373/kotlin-blackjack#블랙잭-기본-규칙)
> 5. [기능 구현 목록](https://github.com/bky373/kotlin-blackjack#기능-구현-목록)
> 6. [배운 점](https://github.com/bky373/kotlin-blackjack#배운-점)
> 7. [느낀 점](https://github.com/bky373/kotlin-blackjack#느낀-점)

## Intro

4주 동안 NextStep의 **Effective Kotlin 1기** 과정에 참여하여 <br> 4가지 게임의 **도메인** 로직을 구현하였습니다.<br>

여기에서는 **TDD**를 활용하여, **블랙잭 게임 도메인** 로직을 구현하였습니다.

> 진행 방식: `next-step`의 비어있는 `게임 Repository`를 `Fork`하고, <br>수강생 각자의 로컬에서 로직을 구현한 후 `Pull Request` -> `Review` -> `Merge` 하는 방식으로 과정을 진행하였습니다. 

## 블랙잭 게임 코드

블랙잭 게임의 경우, **`step4` 브랜치**에서 완성된 코드를 볼 수 있습니다. 

> - Main Source Code: [blackjack-mvc](https://github.com/bky373/kotlin-blackjack/tree/step4/src/main/kotlin/blackjack)
> - Test Code (JUnit 5): [blackjack-test](https://github.com/bky373/kotlin-blackjack/tree/step4/src/test/kotlin/blackjack/model) 

## 다른 게임 코드

> [**자동차 경주**](https://github.com/bky373/kotlin-racingcar/tree/step5)<br>[**로또**](https://github.com/bky373/kotlin-lotto/tree/step4)<br>[**지뢰찾기**](https://github.com/bky373/kotlin-minesweeper/tree/step2)

## 블랙잭 기본 규칙

- 카드의 숫자 계산은 카드 숫자를 기본으로 하며, 예외로 `Ace는 1 또는 11로 계산`할 수 있으며, `King, Queen, Jack은 각각 10으로 계산`한다.
- 게임을 시작하면 참가자는 두 장의 카드를 지급 받으며,  두 장의 카드 숫자를 합쳐 `21을 초과하지 않으면서 21에 가깝게 만들면 이긴다`
- 21을 넘지 않을 경우, 참가자는 얼마든지 카드를 계속 뽑을 수 있다
- 딜러는 처음에 받은 2장의 합계가 16이하이면 반드시 1장의 카드를 추가로 받아야 하고, 17점 이상이면 추가로 받을 수 없다
- `Bust`는 카드 점수 합계가 `21을 넘은 경우`를 말한다
- `BlackJack`은 `처음 2장`의 카드 점수 `합계가 21인 경우`를 말한다

## 기능 구현 목록

### 1. 게임 생성

- 참가자들의 이름을 입력받는다 (쉼표를 기준으로 분리)
- 참가자들의 베팅 금액을 입력받는다
- 위 두 가지 정보를 가지고 게임을 생성한다

### 2. 게임 기본 세팅

- 딜러는 카드 2장을 뽑는다

  > 두 장 중 카드 한 장을 출력한다<br>두 장의 합계가 21이면 상태(Status)를 `BlackJack`으로 한다 (그렇지 않으면 `Hit`)

- 참가자들은 카드 2장씩 받는다

  > 보유 중인 카드를 출력한다<br>점수가 21이면 상태(Status)를 `BlackJack`으로 한다 (그렇지 않으면 `Hit`)

### 3. 참가자들 플레이

- 각 참가자에게 카드 한 장을 더 받을 건지 묻고, 대답을 입력받는다 (예는 `y`, 아니오는 `n`)

  > - `y`이면 카드 한 장을 받는다
  >   - 점수가 21을 넘으면 상태를 `Bust`로 한다
  >   - 정확히 21이면 `Stay`로 한다
  >   - 21보다 낮으면 `Hit`으로 한다
  >   - 보유 중인 카드를 출력한다

  > - `n`이면 카드를 받지 않는다
  >   - 현재 상태가 `BlackJack`, `Bust`면 그대로 유지하고, `Hit`이면 `Stay`로 바꾼다
  >   - 처음 나눠준 카드 2장만 가지고 있는 경우, 보유 중인 카드를 한 번 더 출력한다

### 4. 딜러 플레이

- 점수가 17 이상이 될 때까지 카드를 한 장씩 새로 뽑는다

  > 점수가 21을 넘으면 상태를 `Bust`로 한다

- 딜러 수익 계산

  > - 참가자가 딜러 점수보다 높으면, 베팅한 금액만큼 얻는다
  >
  > - 참가자가 `BlackJack`이면 베팅 금액의 1.5 배를 받는다
  >
  > - 딜러와 참가자 모두 `BlackJack`이면 참가자는 베팅한 금액을 돌려받는다
  >
  > - 참가자가 딜러 점수보다 낮거나, `Bust`이면 베팅 금액을 모두 잃는다
  >
  > - 딜러가 `Bust`이면 그 시점까지 남아 있던 참가자들은 가지고 있는 패에 상관 없이 승리해 베팅 금액을 받는다

###  5. 게임 종료

- 딜러와 참가자들의 카드와 점수를 출력한다
- 딜러와 참가자들의 최종 수익을 출력한다

## 배운 점

배운 내용에 관해서는, <br>
무엇보다 TDD, Clean Code, 그리고 OOP 내용이 많이 생각납니다.

이 부분은 수업이나 리뷰 때마다 계속 강조하셔서  <br>
나름.. 많이 신경쓰면서 작업했습니다. 

4번의 수업이 초보자에게는 턱없이 부족한 시간이었지만,,   <br>
지식이 전무했을 때보다는 성장했다고 말할 수 있을 것 같습니다.  <br>

그럼에도,  <br>
TDD와 OOP만 놓고 본다면 솔직히 성장했다고 말하기도 좀 그렇네요ㅠㅠ  <br>
특히 마지막 수업에서 상태 패턴을 배웠을 때,   <br>
내가 알던 OOP가 얼마나 초보적이었는지 새삼 깨달았습니다,,   <br>

보다 구체적으로 기억에 남는 건  <br>
아래 원칙들입니다.

- **README 파일** 작성하기 (기능 목록 작성 후 이를 구현하기)
- **핵심 로직**과 **UI 로직 분리**하기

- **TDD** (Red-Green-Refactoring) / **단위 테스트** 만들기

- **가독성이 좋은 변수 이름, 함수 이름, 코드** 작성하기

- 함수의 **들여쓰기** 규칙 지키기 

- **if/else 중복 제거**하기

- 객체의 상태를 꺼내지 말고 **메시지**를 보내기

- 필요하다면 **상속** 구현하기

- **불변 객체** 만들기

- **일급 컬렉션** 만들기 (이 부분은 아직 좀 어렵네요..)

- 등등 ...

여기에 더하자면, <br>
조금 더 Kotlin스러운 코딩 스타일<br>

- data class 이용

- 람다식과 프로퍼티 이용

- 연산자 오버로딩

- null을 이용한 에러 처리 등이 될 수 있겠네요.

그리고 마지막으로 <br>
git, slack과 더 친해진 것까지! <br>

아무것도 모르던 제가 이것들을 지키고 활용해봤다면<br>그것 자체로도 한층 더 성장한 게 아닐까요!

물론 이외에도 정말 얻어가는 게 많은 수업이었습니다.<br>앞으로 심화 과정이 열린다면 꼭 참여하고 싶습니다!!

## 느낀 점

처음에는 과정이 재직자 대상이었고, 낯선 언어인 코틀린으로 진행되었기 때문에, <br>두 가지 조건에서 거리가 멀었던 저는 수강생들과의 격차가 걱정되었습니다.<br>하지만 현직자의 피드백을 꼭 듣고 싶었기 때문에 강의를 포기하는 대신 코틀린 선행학습을 시작했습니다. 

초기에는 책과 유튜브를 보며 계산기 어플 등을 만들어보았고, <br>여기서 얻은 만족감으로 계속 동기 부여를 얻었습니다. <br>이후 더욱 학습에 몰입해 'Udacity' 등 온라인 강의를 수강했습니다. <br>코틀린의 문법을 익히고 자바와 비교하는 과정이 재밌었고, <br>이러한 경험은 본격적인 과정을 시작하기 전 자신감을 갖게 해주었습니다.  

과정을 시작한 이후에는 책 'Kotlin In Action'도 조금씩 참고했습니다.<br>data class나 null-safety 등 코틀린에 대한 지식을 넓혔고, 구현한 코드에 이를 적용했습니다.<br>한 번은 이렇게 책을 참고해 코드를 수정했는데, 리뷰어님이 로직을 잘 리팩토링했다는 칭찬을 해주었습니다.<br>현직자로부터 칭찬을 들었을 땐 다른 어느 때보다 뿌듯했고, 훨씬 강력한 성취감을 느낄 수 있었습니다.  

처음에는 경험과 실력이 한참 부족하다는 생각에, 사실 많이 두려웠습니다.<br>하지만 과정을 꼭 수료해야겠다는 의지가 있었기 때문에, <br>어떤 자리(버스, 지하철)가 되었든, 어떤 시간이 되었든 코딩을 했습니다. <br>그리고 결국 모든 미션을 통과하였습니다.<br>다 사람이 하는 일이기 때문에 불가능한 일은 없다는 것, 이번을 계기로 몸소 배우고 느꼈습니다.<br>

