## Symmetric Key (대칭키)

이름에서도 알 수 있듯이 특정 데이터를 **암호화, 복호화 할 때 사용하는 키가 동일**한 기법
암호화된 데이터를 전달하고 해독하기 위해서는 **송, 수신자가 동일한 키**를 사용해야 한다.
따라서 이 기법의 **핵심은 안전하게 대칭키를 안전하게 교환하는 것**이다.

![IMG1](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fdf6d2882-b654-4e0d-9779-2322b7265ed8%2FUntitled.png?id=5c71ac65-517b-4a6e-9d8e-e834b06278f6&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1180&userId=&cache=v2)

## Public Key (공개키, 비대칭키)

대칭키 방식과 달리 특정 데이터를 **암호화, 복호화 할 때 사용하는 키를 달리하는 기법
공개키는 외부에 공개**되있고, 수신자(암호를 해독하는 측)가 **개인키로 데이터를 해독**한다.
즉 송, 수신자 간의 암호키를 교환할 필요가 없어지게 된다.

![IMG2](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F5ebc87d2-8d34-4e42-bf6e-78b44fcedd2f%2FUntitled.png?id=1ed2c1ca-0e8d-40c8-a098-abca2271e2f2&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1920&userId=&cache=v2)

### Q. 역으로 개인키를 가진 사람이 데이터를 보내고 싶을 때는…?

![IMG3](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fbd25ba2a-ab57-467a-9573-f1e41c14e79f%2FUntitled.png?id=9321792c-56ca-4b26-bcb0-dfd3cec84def&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=2000&userId=&cache=v2)

RSA 기법(비대칭키 기법 중 하나)에서는 암호화, 복호화의 순서 상관없이 연산의 결과가 동일한 알고리즘
→ 개인키로 데이터를 암호화하고 수신 측에서 공개키로 데이터를 복호화 할 수 있게 된다.

### Q. 개인키로 암호화한 데이터를 공개키로 해독할 수 있는건 위험하지 않나요?

이론적으로 중간에 데이터를 가로채서 복호화할 수 있는 위험성이 존재하긴 한다.

→ 이 방법은 어떤 데이터를 보내느냐가 보다 **누가 보냈느냐를 증명할 수 있는 방법으로 활용**한다.

## Symmetric vs. Public

|  | 대칭키 | 비대칭키 |
| --- | --- | --- |
| 키 관계 | 암호화 키 = 복호화키 | 암호화 키 ≠ 복호화 키 |
| 암호화 키 | 비밀키 | 공개키 |
| 복호화 키 | 비밀키 | 개인키 |
| 비밀키 전송 | 필요 | 불필요 |
| 키 길이 | 짧다 | 길다 |
| 인증 | 곤란 | 용이 |
| 암복화 속도 | 빠르다 | 느리다 |
| 경제성 | 높다 | 낮다 |
| 전자서명 | 복잡 | 간단 |
| 주 용도 | 고용량 데이터 암호화(기밀성) | 키 교환 및 분배, 인증, 부인방지 |
| 장점 | 암(복)호화 키 길이가 짧다<br>구현이 용이하고, 암(복)호화가 빠르다<br>암호화 강도 전환이 용이<br>암호화 기능이 우수<br>각종 암호 시스템의 기본으로 활용 | 사용자가 증가하더라도 관리해야 할 키의 개수가 상대적으로 적다<br>Key 전달이나 교환에 적합하다<br>인증과 전자 서명에 이용<br>대칭키 보다 확장성이 좋다<br>여러가지 분야에서 응용이 가능<br>키 변화의 빈도가 적음 |
| 단점 | 키 교환 원리가 명시되지 않음 → 키 분배가 어렵다<br>관리할 암(복)호화 키가 많다  N명 → N(N-1)/2<br>확장성이 낮다<br>전자서명(디지털서명)이 불가능<br>부인방지 기능이 없다 | - 키 길이가 길다<br>복잡한 수학적 연산을 이용함으로 암(복)호화 속도가 느리다<br>중간에 인증과정이 없으므로 중간자 공격에 취약하다 (전자서명, 인증서 등으로 해결) |
| 예 | [Feistel] : SEED, DES[SPN] : ARIA, AES, IDEA메시지 인증코드(MAC) | Diff-Hellman, RSA, ECC, DAS[Block chain][TPM] |

## Combination

![IMG4](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe4383a8b-7ee0-4e32-9e5a-b73a9bf52ae5%2FUntitled.png?id=ca7e5840-0e9d-4202-abc4-5aba87e8106b&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=2000&userId=&cache=v2)

- 평문(데이터)를 암호화, 복호화하는 방식에는 대칭키
- 대칭키를 교환하는 방식은 비대칭 기법을 사용

## 참고

- 국민대학교 이창우 교수 “컴퓨터 네트워크 - 암호학” 수업 중 일부 내용 및 이미지 발췌

[무조건 이해 시켜드립니다. 대칭키와 비대칭키에 대하여](https://universitytomorrow.com/22)
