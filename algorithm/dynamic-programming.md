# Dynamic Programming

다이나믹 프로그래밍 알고리즘은 문제를 각각의 작은 문제로 나누어 해결한 결과를 저장해뒀다가 나중에 큰 문제의 결과와 합하여 풀이하는 알고리즘이다. 다이나믹 프로그래밍은 **최적 부분 구조(Optimal Substructure**)를 갖고 있는 문제를 **중복된 하위 문제들(Overlapping Subproblem**)의 결과로 저장해 두면서 풀이해 나간다는 특징을 갖고 있다.

### 최적 부분 구조(Optimal Substructure)

최적 부분 구조에 대해 좀 더 살펴보자. 서울에서 부산까지 가는 최단 경로를 찾는 간단한 예를 들어본다. 서울에서 대구까지 가는 경로는 3가지가 있으며, 대구에서 부산까지도 마찬가지로 3가지 경로가 있다. 서울에서 부산까지 가는 최단 경로는 200km + 80km = 280km이다. 이 경로는 서울에서 대구까지 가는 최단 경로(200km)와 대구에서 부산까지 가는 최단 경로(80km)로 구성된다. 즉 서울에서 부산까지 가는 최단 경로는 각각의 부분 문제인 **1) 서울에서 대구까지 가는 최단 경로 문제와 2) 대구에서 부산까지 가는 최단 경로 문제의 해결 방법의 합이다. 따라서 최적 해결 방법은 부분 문제에 대한 최적 해결 방법으로 구성된다.**

이러한 구조를 최적 부분 구조라 하며, 이런 유형의 문제는 분할 정복으로 풀 수 있다. 또한 다이나믹 프로그래밍 또는 그리디 알고리즘으로 접근해볼 수 있는 문제의 후보가 된다. 그러나 만약 서울에서 부산까지 바로 연결되는 고속도로가 새롭게 개통되어 더 이상 대구를 거칠 필요가 없다면, 이 문제는 더 이상 최적 부분 구조가 아니다. 더는 분할 정복으로 볼 수 없으며, 다이나믹 프로그래밍이나 그리디 알고리즘으로도 풀이할 수 없다.

### 중복된 하위 문제들

**다이나믹 알고리즘으로 풀 수 있는 문제들과 다른 문제들의 결정적인 차이는 중복된 하위 문제들을 갖는다는 점이다.** 예를 들어 피보나치 수열의 경우 아래와 같은 계산 구조를 띤다.

![https://static.javatpoint.com/tutorial/daa/images/overlapping-sub-problems1.png](https://static.javatpoint.com/tutorial/daa/images/overlapping-sub-problems1.png)

위의 함수를 보면, F(3) = F(2) + F(1)이며, F(4) = F(3) + F(2)이다. F(5) 또한 F(5) = F(4) + F(3)이다. 이처럼 피보나치 수열을 재귀로 풀면 반복적으로 동일한 하위 문제들이 반생한다. 바로 이 부분이 핵심으로, **중복 문제가 발생하지 않는 병합 정렬은 분할 정복으로 분류되지만, 피보나치 수열을 풀이하는 알고리즘은 다이나믹 프로그래밍의 대상으로 분류되는 이유**다.

### 다이나믹 프로그래밍 방법론

방법론은 방식에 따라 크게 상향식과 하향식으로 나뉜다. 일반적으로 상향식을 타뷸레이션(Tabulation), 하향식을 메모이제이션(Memoization)이라고 구분해 부르기도 한다.

- 상향식(Bottom-Up) : **더 작은 하위 문제부터 살펴본 다음, 작은 문제의 정답을 이용해 큰 문제의 정답을 풀어나간다.** 타뷸레이션(Tabulation)이라 부르며, 일반적으로 이 방식만을 다이나믹 프로그래밍으로 지칭하기도 한다.
- 하향식(Top-Down) : **하위 문제에 대한 정답을 계산했는지 확인해가며 문제를 자연스러운 방식으로 풀어나간다.** 이 방식을 특별히 메모이제이션(Memoization)이라 지칭한다.

피보나치 수열의 예제 코드를 보면, 상향식과 하샹식의 구분이 금방 이해될 것이다.

상향식(Bottom-Up)

```python
class Solution:
    dp = collections.defaultdict(int)

    def fibo(self, n:int) -> int:
        self.dp[1] = 1

        for i in range(2, n+1):
            self.dp[i] = self.dp[i - 1] + self.dp[i - 2]
        return self.dp[n]
```
데이터를 테이블 형태로 만들면서 문제를 풀이한다고 하여 타뷸레이션(Tabulation) 방식이라고 부른다. 이 방식만을 다이나믹 프로그래밍이라 지칭하는 경우도 있다.

하향식(Top-Down)

```python
class Solution:
    dp = collections.defaultdict(int)

    def fibo(self, n:int) -> int:
        if n <= 1:
            return n

        if self.dp[n]:
            return self.dp[n]
        self.dp[n] = self.fibo(n-1) + self.fibo(n-2)
        return self.dp[n]
```

하향식 방법론은 하위 문제에 대한 정답을 계산했는지 확인해가며 문제를 자연스럽게 재귀로 풀어나간다. 기존 재귀 풀이와 거의 동일하면서도 이미 풀어봤는지 확인하여 재활용하는 효율적인 방식으로, 메모이제이션 방식이라고 부른다.

### 참조

<파이썬 알고리즘 인터뷰, 책만, 박상길>