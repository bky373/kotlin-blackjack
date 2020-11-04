## 블랙잭 게임 도메인 구현

### 기능 구현 목록

- 블랙잭 기본 규칙
  - 카드의 숫자 계산은 카드 숫자를 기본으로 하며, 예외로 Ace는 1 또는 11로 계산할 수 있으며, King, Queen, Jack은 각각 10으로 계산한다
  - 게임을 시작하면 참가자는 두 장의 카드를 지급 받으며, 두 장의 카드 숫자를 합쳐 21을 초과하지 않으면서 21에 가깝게 만들면 이긴다
  - 21을 넘지 않을 경우, 참가자는 얼마든지 카드를 계속 뽑을 수 있다
  - 딜러는 처음에 받은 2장의 합계가 16이하이면 반드시 1장의 카드를 추가로 받아야 하고, 17점 이상이면 추가로 받을 수 없다
  - Bust는 카드 점수 합계가 21을 넘은 경우를 말한다
  - BlackJack은 처음 2장의 카드 점수 합계가 21인 경우를 말한다

- 게임 생성
  - 참가자(플레이어)들의 이름을 입력받는다 (쉼표를 기준으로 분리)
  - 참가자들의 베팅 금액을 입력받는다
  - 위 두 가지 정보를 가지고 게임을 생성한다

- 게임 기본 세팅
  - 딜러는 카드 2장을 뽑는다
    - 점수가 21이면 상태를 BlackJack으로 한다 (그렇지 않으면 Hit)
    - 두 장 중 카드 한 장을 보여준다 
  - 참가자들은 카드 2장씩 받는다
    - 점수가 21이면 상태를 BlackJack으로 한다 (그렇지 않으면 Hit)
    - 보유 중인 카드를 보여준다

- 참가자들 플레이
  - 각 참가자에게 카드 한 장을 더 받을 건지 묻고 대답을 입력받는다 (예는 y, 아니오는 n)
    - y이면 카드 한 장을 받는다
      - 점수가 21을 넘으면 상태를 Bust로 한다
      - 정확히 21이면 Stay로 한다
      - 21보다 낮으면 Hit으로 한다
      - 보유 중인 카드를 보여준다
    - n이면 카드를 받지 않는다
      - 현재 상태가 BlackJack, Bust면 그대로 유지하고, Hit이면 Stay로 바꾼다
    - 기본 카드 2장만 가지고 있는 경우 보유 중인 카드를 한 번 더 보여준다

- 딜러 플레이
  - 점수가 17 이상이 될 때까지 카드를 한 장씩 새로 뽑는다
    - 점수가 21을 넘으면 상태를 Bust로 한다

- 딜러 수익 계산
  - 참가자가 딜러 점수보다 높으면, 베팅한 금액만큼 얻는다
  - 참가자가 BlackJack이면 베팅 금액의 1.5 배를 받는다
    - 딜러와 참가자 모두 BlackJack이면 참가자는 베팅한 금액을 돌려받는다
  - 참가자가 딜러 점수보다 낮거나, Bust이면 베팅 금액을 모두 잃는다
  - 딜러가 Bust이면 그 시점까지 남아 있던 참가자들은 가지고 있는 패에 상관 없이 승리해 베팅 금액을 받는다

- 게임 종료
  - 딜러와 참가자들의 카드와 점수를 보여준다
  - 딜러와 참가자들의 최종 수익을 보여준다
