# 알아두기
- **고유 번호**는 Discord에서 부여한 Snowflakes (18~19글자의 10진수로 이루어진 ID),
- **순번**은 첩첩냥에서 부여한 임의의 숫자,
- **식별자**는 제공자 구별 없이 식별을 위한 임의의 숫자나 영어의 조합
- PRIMARY KEY는 <ins>밑줄</ins> 표시, relation은 *기울임체* 표시

# user.db

테이블 명: config.py `SEASON` 값 (ex: `Season-FiNALE`)

| <ins>userId</ins> | exp | level | cooldown | ~~reward~~ | bottleMultiple | bottlePeriod |
| ------ | --- | ----- | -------- | ------ | -------------- | ------------ |
| 유저 고유 번호 | 경험치 | 레벨 | 다음 경험치 획득 가능 `UNIX` | 미사용 | 경험치 증가 배수 | 경험치 증가 종료 `UNIX` |
| 123546789012345678 | 0 | 0 | 1768324387 | `NULL` | 1.5 | 1768324387 |

# season.db

테이블 명: config.py `SEASON` 값 (ex: `Season-FiNALE`)

item(n)일 때 n은 1, 2, 3이 존재합니다.

| <ins>level</ins> | item1 | item1_amount | item1_data | ... | premium | premium_amount | premium_data |
| ----- | ----- | ------------ | ---------- | --- | ------- | -------------- | ------------ |
| 레벨 | 아이템 식별자 | 아이템 개수 | 아이템 데이터 `JSON` | ... | 서버 부스트 전용 아이템<br>(패스 프리미엄) | ... | ... |
| 1 | exp-bottle | 1 | {"multiple": 1.5,<br>"minutes": 30} | ... | goods-lottery | 1 | {} |

# reward.db

테이블 명: config.py `SEASON` 값 (ex: `Season-FiNALE`)

| userId | level | rewardId | rewarded |
| ------ | ----- | -------- | -------- |
| 유저 고유 번호 | 레벨 | 아이템 순번 | 획득 여부 `BOOL` |
| 123546789012345678 | 1 | 1 | 1 |

# log.db

`time` 이벤트 발생 시각 (UNIX Time)

`event` 이벤트 타입

`userId` 유저저 고유 번호

`gainedExp` 이벤트 `ExpFrom***`일 때 획득한 경험치 (음수도 가능)

`cmdUserId` 관리자가 실행한 이벤트일 경우 이벤트를 실행한 관리자

`item` 해당 이벤트로 획득한 아이템

`itemAmount` 아이템 개수

`chId` 이벤트 발생 채널 고유 번호

`msgId` 이벤트 발생 메시지 고유 번호

# item.db

| <ins>itemId</ins> | emoji | ko | en | desc_ko | desc_en |
| ------ | ----- | -- | -- | ------- | ------- |
| 아이템 식별자 | 이모지 | 한국어 이름 | 영어 이름 | 한국어 설명 | 영어 설명 |
| goods-lottery	| 🎫 | 굿즈 응모권 | Goods enter-lottery ticket | 해당 시즌의... | A ticket that... |

# inventory.db

※ 테이블 명은 무조건 `유저 고유 번호`여야 함

| <ins>itemId</ins> | amount | <ins>data</ins> |
| ------ | ------ | ---- |
| 아이템 식별자 | 개수 | 데이터 `JSON` |
| goods-exchange | 1 | {} |

# account.db

| <ins>discordId</ins> | twitchId | email | phone | ok |


`discordId` 유저 고유 번호

`twitchId` Twitch에서 부여한 계정 ID

`email` (Discord 로그인 시) 이메일 (기본값 null)

`phone` 휴대전화번호 (웹사이트 인증 시)

`ok` ??? 이거 뭐였지 일단 놔둬보세요 0도 있고 1도 있는데 이거

# attendance.db

※ 테이블 명은 무조건 `attendance-연도-월` 이어야 함 (월은 한자리수면 앞에 0 붙어야됨)

`userId` 유저 고유 번호

`day(1~31)` 출석 여부 (2 이상 = 출석 인정)

# gamble.db

테이블 명 `blackjack`

`userId` 유저 고유 번호

`channelId` 스레드 고유 번호

`betting` 냥코인 개수

`player` 플레이어 보유 카드

`dealer` 딜러 보유 카드

`remained` 남은 카드

# 이벤트 데이터베이스

`userId` 유저 고유 번호

`point` (컬럼명 변경가능) 이벤트 포인트 개수
