# Base(진법)

### 10진법과 2진법

1946년도에 개발된 컴퓨터인 에니악(ENIAC)은 사람에게 익숙한 10진법을 사용하도록 설계되었으나 전기회로는 전압이 불안정해서 전압을 10단계로 나누어 처리하는 데 한계가 있었다. 그래서 1950년에 개발된 에드박(EDVAC)은 단 두 가지 단계, 전기가 흐르면 1, 흐르지 않으면 0, 만으로 동작하도록 설계되었고 매우 성공적이었다. **손가락 개수가 10개인 사람에게 10진법이 적합하듯, 컴퓨터와 같은 전기회로에는 2진법이 적합한 것이다.**

그 이후부터 지금까지 대부분의 컴퓨터는 2진 체계로 설계되었기 때문에, 2진법을 알지 못하면 컴퓨터의 동작원리나 데이터 처리방식을 온전히 이해할 수 없다. 지금까지 변수에 값을 저장하면 10진수로 저장되는 것처럼 이해를 하였지만, 컴퓨터는 2진수(0과 1) 밖에 모르기 때문에 아래와 같이 2진수로 바뀌어 저장된다. 2진수 11001은 10진수로 25이다.

age `25` → age `11001`

<br>

### 비트(bit), 바이트(byte), 워드(word)

- 비트(bit) : 한 자리의 2진수를 '비트(bit, binary digit)'라고 하며, 1 비트는 컴퓨터가 값을 저장할 수 있는 최소 단위이다. n비트로 <!-- $2^n$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=2%5En">개의 값을 표현할 수 있다. 그리고 n비트로 10진수를 표현한다면, 표현 가능한 10진수의 범위는 0 ~ <!-- $2^n-1$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=2%5En-1">이 된다.
- 바이트(byte) : 1 비트는 너무 작은 값이기 때문에 1 비트 8개를 묶어서 '바이트(byte)'라는 단위로 정의해서 데이터의 기본 단위로 사용한다.
- 워드(word) : 이 외에도 '워드(word)'라는 단위가 있는데, 워드(word)는 'CPU가 한 번에 처리할 수 있는 데이터의 크기'를 의미한다. 워드의 크기는 CPU의 성능에 따라 달라진다. 예를 들어 32 비트 CPU에서 1 워드는 32 비트(4 바이트)이고, 64비트 CPU에서는 64 비트(8 바이트)이다.

<br>

### 8진법과 16진법

2진법은 오직 0과 1, 두 개의 기호만으로 값을 표현하기 때문에, 2진법으로 값을 표현하면 자리수가 상당히 길어진다는 단점이 있다. 이러한 단점을 보완하기 위해 2진법 대신 8진법이나 16진법을 사용한다. 8진수는 2진수 3자리를, 16진수는 2진수 4자리를 각각 한 자리로 표현할 수 있기 때문에 자리수가 짧아져서 알아보기도 쉽고 서로간의 변환방법 또한 매우 간단하다.

8진법은 값을 표현하는데 8개의 기호가 필요하므로 0~7의 숫자를 기호로 사용하면 되지만, 16진법은 16개의 기호가 필요하므로 0~9의 숫자만으로는 부족하다. 그래서 6개의 문자(A~F)를 추가로 사용한다. 예를 들어 16진수 A는 10진수로 10이고, F는 15이다.

<br>

### 2진수를 8진수, 16진수로 변환

2진수를 **8진수를 변환하려면, 2진수를 뒤에서부터 3자리씩 끊어서** 그에 해당하는 8진수로 바꾸면 된다. 8은 <!-- $2^3$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=2%5E3">이기 때문에, 8진수 한 자리가 2진수 3자리를 대신할 수 있는 것이다. 2진수를 16진수로 변환하는 방법 역시 이와 비슷한데, 3자리가 아닌 4자리씩 끊어서 바꾼다는 점만 다르다.

(2)`1010101100` = (8)`1254` = (16)`2AC`

<br>

### 10진수를 n진수로 변환

10진수를 다른 진수로 변환하려면, 해당 진수로 나누고 나머지 값을 옆에 적는 것을 더 이상 나눌 수 없을 때까지 반복한 다음 마지막 몫과 나머지를 아래부터 위로 순서대로 적으면 된다.

```java
2ㄴ46 나머지

2ㄴ23 ...0   ↑

2ㄴ11 ...1   |

2ㄴ5 ...1    |

2ㄴ2 ...1    |

 ㄴ1 ...0    |

============> 46(10) = 101110(2)
```

<br>

### n진수를 10진수로 변환

어떤 진법으로 된 수라도 10진수로 변환하는 방법은 똑같다. 각 자리수의 수에 해당 단위의 값을 곱해서 모두 더하면 된다.

1460(8) = <!-- $1*8^3 + 4*8^2 + 6*8^1 + 0*8^0$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=1*8%5E3%20%2B%204*8%5E2%20%2B%206*8%5E1%20%2B%200*8%5E0">

<br>

### 10진 소수점수를 2진 소수점수로 변환하는 방법

앞서 10진 정수를 2진 정수로 변환할 때, 10진수를 2로 계속 나누면서 나머지를 구했던 것을 기억할 것이다. **10진 소수점수를 2진 소수점수로 변환하는 방법은 이와 반대로 10진 소수점수에 2를 계속 곱한다.** 

예를 들어 10진수 0.625를 2진수로 변환하는 방법은 다음과 같다.

1. 10진 소수에 2를 곱한다.
- 0.625 x 2 = 1.25
2. 위의 결과에서 소수부만 가져다가 다시 2를 곱한다.
- 0.25 x 2 = 0.5
3. 1.과 2.의 과정을 소수부가 0이 될 때까지 반복한다.
- 0.625 x 2 = 1.25
- 0.25 x 2 = 0.5
- 0.5 x 2 = 1.0
4. 위의 결과에서 정수부만을 위에서 아래로 순서대로 적고 `0.`을 앞에 붙이면 된다.
- 0.625(10) → 0.101(2)

<br>

### 2진 소수점수를 10진 소수점수로 변환하는 방법

0.101(2)는 다음과 같이 표현할 수 있다. 

= <!-- $1 * 2^{-1} + 0 * 2^{-2} + 1 * 2^{-3}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=1%20*%202%5E%7B-1%7D%20%2B%200%20*%202%5E%7B-2%7D%20%2B%201%20*%202%5E%7B-3%7D">

= 1 * 0.5 + 0 * 0.25 + 1 * 0.125

= 0.625(10)

<br>

---

### Reference

Java의 정석, 도우출판, 남궁성