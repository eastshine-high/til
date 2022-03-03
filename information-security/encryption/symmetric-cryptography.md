## 대칭키 암호(Symmetric Cryptography)

대칭키 암호는 암호화와 복호화에 하나의 같은 비밀키를 사용하는 암호 방식이다. 즉, 대칭키는 평문 P를 암호화할 때 비밀키 K를 이용하여 암호문 C를 만들었다면, 이 암호문 C를 복호화할 때에도 동일한 비밀키 K를 이용해야만 원래의 평문 P를 만들 수 있다.

## 암호 기법

### 블록 암호(Block cipher)와 스트림 암호(Stream cipher)

**블록 암호(Block cipher)는 어느 특정 비트 수와 집합을 한 번에 처리하는 암호 알고리즘을 총칭**한다. 이 집합을 블록이라고 하며, 블록의 비트 수를 블록 길이라고 한다. 평문을 일정한 크기의 블록으로 잘라낸 후 암호화 알고리즘을 적용하여 암호화한다. 일반적으로 블록의 크기는 8비트(ASCII) 또는 16비트(Unicode)에 비례한다.

**스트림 암호(Stream cipher)는 한번에 1비트 혹은 1바이트의 데이터 흐름(스트림)을 순차적으로 처리해가는 암호 알고리즘의 총칭**이며, 암호화 방식은 **평문과 키 스트림을 XOR**하여 생성한다. 블록 암호는 블록 단위로 처리가 완료되므로 어디까지 암호화가 진행되었는가 하는 내부 상태를 가질 필요가 없다. 한편 스트림 암호는 데이터의 흐름을 순차적으로 처리해가기 때문에 내부 상태를 가지고 있다. 스트림 암호는 이동 통신 환경에서 구현이 용이하고, 안정성을 수학적으로 엄밀하게 분석할 수 있어 이동 통신 등의 무선 데이터 보호에 적합하다.

### 치환 암호(Substitution Cipher)와 전치 암호(Transposition Cipher)

치환 암호(대치 암호, Substitution Cipher)는 비트, 문자 또는 문자의 블록을 다른 비트, 문자 또는 블록으로 대치한다. 치환 암호의 엄밀한 의미는 **평문에서 사용하는 문자의 집합과 암호문에서 사용하는 집합이 다를 수 있다.** **즉, 평문의 문자를 다른 문자로 교환하는 규칙**이다. 이 때 교환 규칙은 일대일 대응이 아니어도 상관없다.

전치 암호(Transposition Cipher)는 원문을 다른 문서로 대체하지 않지만 원문을 여기저기 움직이게 한다. 그것은 비트, 문자 또는 블록이 원래 의미를 감추도록 재배열한다. 전치는 평문에서 사용하는 문자의 집합과 암호문에서 사용하는 문자의 집합이 동일하다. 따라서 **전치 암호란 문자 집합 내부에서 자리를 바꾸는 규칙이고 평문에 사용된 문자와 암호문에 사용된 문자가 일대일 대응 규칙을 갖는다는 점이 특징**이다.

## 대칭키 암호 알고리즘

### DES(Data Encryption Standard)

DES(Data Encryption Standard)는 1977년 미국에서 데이터 암호 알고리즘의 표준으로 공표한 대칭키 암호 알고리즘이다. 이후 DES는 가장 널리 사용된 암호 알고리즘이었지만, 키의 길이가 짧고 컴퓨터 속도의 개선과 암호해독 기술의 발전으로 DES는 더 이상 안전하지 않게 되었다. 결국 2001년 새로운 표준인 AES가 나오면서 DES는 표준의 자리를 물려주었다.

DES는 64비트의 평문을 64비트의 암호문으로 만드는 블록 암호 알고리즘으로 56비트의 키를 사용한다(단, 키를 유지할 때에는 검사용 비트기 8비트 추가되어 64비트로 관리함).

DES는 16라운드의 반복적인 암호화 과정을 갖고 있으며, 각 라운드마다 전치 및 치환의 과정을 거친 평문과 56비트의 키에서 생성된 48비트의 라운드 키가 섞여 암호문을 만든다. DES는 파이스텔 구조이다.

복호화는 암호화 과정과 동일하나 사용되는 키만 역순으로 작용하는 것이다.

### AES(Advanced Encryption Standard)

미국의 NIST는 1997년에 기존의 DES를 대신할 수 있는 새로운 암호 알고리즘(AES, Advanced Encryption Standard)을 공모했다. 기존의 DES보다 안전하면서 128비트 이하의 키를 사용해야 한다는 조건에도 불구하고, 약 15개의 알고리즘이 응모되었다. 안전성 분석과 구현 효율성을 검증받은 최종 후보 5개 가운데 대먼(Joan Daemen)과 리즈멘(Vincent Rijmen)이 제출한 라인달(Rijndael)이 차세대 AES로 선정되어 2001년 공표되었다.

AES는 128비트의 평문을 128비트의 암호문으로 만드는 블록 암호 알고리즘으로 키는 128비트, 192비트, 256비트 중 택일하여 사용한다.

AES는 키의 길이에 따라 10라운드, 12라운드, 또는 14라운드의 반복적인 암호화 과정을 갖고 있다. 각 라운드는 비선형성을 갖는 S-Box를 적용하여 바이트 단위로 치환을 수행하는 SubBytes 연산, 행 단위로 순환 시프트를 수행하는 ShiftRows 연산, Diffusion을 제공하기 위해 열 단위로 혼합하는 MixColumns 연산과 마지막으로 라운드 키와 State를 XOR하는 AddRoundKey 연산으로 구성된다 AES는 SPN 구조이다.

## 참조

<조현준, 2021 정보보안기사 필기, 탑스팟>