# 알아두기
- **고유 번호**는 Discord에서 부여한 Snowflakes (18~19글자의 10진수로 이루어진 ID)
- **순번**은 첩첩냥에서 부여한 임의의 숫자
- **식별자**는 제공자 구별 없이 식별을 위한 임의의 숫자나 영어의 조합
- PRIMARY KEY는 <ins>밑줄</ins> 표시, relation은 *기울임체* 표시
- 모든 예시는 실제와 같지 않을 수 있음.

# user.db

- 테이블 명은 config.py의 `SEASON` 값이어야 함

| <ins>userId</ins> | exp | level | cooldown | ~~reward~~ | bottleMultiple | bottlePeriod |
| ------ | --- | ----- | -------- | ------ | -------------- | ------------ |
| 유저 고유 번호 | 경험치 | 레벨 | 다음 경험치 획득 가능 `UNIX` | 미사용 | 경험치 증가 배수 | 경험치 증가 종료 `UNIX` |
| 123546789012345678 | 0 | 0 | 1768324387 | `NULL` | 1.5 | 1768324387 |

# season.db

- 테이블 명은 config.py의 `SEASON` 값이어야 함
- 컬럼명 중 item(n)인 경우 n은 1, 2, 3이 존재함

| <ins>level</ins> | item1 | item1_amount | item1_data | ... | premium | premium_amount | premium_data |
| ----- | ----- | ------------ | ---------- | --- | ------- | -------------- | ------------ |
| 레벨 | 아이템 식별자 | 아이템 개수 | 아이템 데이터 `JSON` | ... | 서버 부스트 전용 아이템<br>(패스 프리미엄) | ... | ... |
| 1 | exp-bottle | 1 | {"multiple": 1.5,<br>"minutes": 30} | ... | goods-lottery | 1 | {} |

# reward.db

- 테이블 명은 config.py의 `SEASON` 값이어야 함

| userId | level | rewardId | rewarded |
| ------ | ----- | -------- | -------- |
| 유저 고유 번호 | 레벨 | 아이템 순번 | 획득 여부 `BOOL` |
| 123546789012345678 | 1 | 1 | 1 |

# log.db

| time | event | userId | gainedExp | cmdUserId | item | itemAmount | chId | msgId |
|  ---- | ----- | ------ | --------- | --------- | ---- | ---------- | ---- | ----- |
| 발생 시각 `UNIX` | 이벤트 타입 | 유저 고유 번호 | 경험치 획득량 `NUMERIC` | 관리자 | 아이템 식별자 | 개수 | 채널 고유 번호 | 메시지 고유 번호 |
| 1674482277.61669 | ExpFromChat | 123456789012345678 | 11.1208786618137 |	null | null | null | 123456789012345670 | 2345678901234567890 |

# item.db

| <ins>itemId</ins> | emoji | ko | en | desc_ko | desc_en |
| ------ | ----- | -- | -- | ------- | ------- |
| 아이템 식별자 | 이모지 | 한국어 이름 | 영어 이름 | 한국어 설명 | 영어 설명 |
| goods-lottery	| 🎫 | 굿즈 응모권 | Goods enter-lottery ticket | 해당 시즌의... | A ticket that... |

# inventory.db

- 테이블 명은 무조건 `유저 고유 번호`여야 함

| <ins>itemId</ins> | amount | <ins>data</ins> |
| ------ | ------ | ---- |
| 아이템 식별자 | 개수 | 데이터 `JSON` |
| goods-exchange | 1 | {} |

# account.db

| <ins>discordId</ins> | twitchId | email | phone | ok |
| --------- | -------- | ----- | ----- | -- |
| 유저 고유 번호 | Twitch 계정 번호 | 이메일 | 휴대전화번호 | ??? `Bool` |
| 123456789012345678 | 123454678 | \*\*\*\*\*@\*\*\*\*\*.\*\*\* | \*\*\*\*\*\*\*\*\*\* | 1 |

# attendance.db

- 테이블 명은 무조건 `attendance-연도-월` 이어야 함 (월은 한자리수면 앞에 0 붙어야됨)
- 메시지 개수는 2개 이상일 경우 출석 인정

| <ins>userId</ins> | day1 | day2 | ... | day30 | day31 |
| ------ | ---- | ---- | ---- | ---- | ---- |
| 유저 고유 번호 | 1일차 메시지 개수 | 2일차 메시지 개수 |  ... | 30일차 메시지 개수 | 31일차 메시지 개수 |
| 123456789012345678 | 77 | 12 | ... | 31 | 118 |

# gamble.db

- 테이블 명은 무조건 `blackjack`이어야 함.

| userId | channelId | betting | player | dealer | remained |
| ------ | --------- | ------- | ------ | ------ | -------- |
| 유저 고유 번호 | 스레드 고유 번호 | 냥코인 개수 | 플레이어 보유 카드 `LIST` | 딜러 보유 카드 `LIST` | 남은 카드 `LIST` |
| 123456789012345678 | 2345678901234567890 | 53022 | ['1♠', '2♠'] | ['Q♠', '3♥'] | [ ... ] |

# 이벤트 데이터베이스

| <ins>userId</ins> | point |
| ------ | ----- |
| 유저 고유 번호 | 이벤트 포인트 개수 |
| 123456789012345678 | 53022 |
