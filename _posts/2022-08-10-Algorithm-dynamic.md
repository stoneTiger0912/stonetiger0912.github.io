---
title:  "[Algorithm] 알고리즘 정리(6)- 다이나믹 프로그래밍"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, dynamic_programming]

toc: true
toc_sticky: true
 
date: 2022-08-10
last_modified_at: 2022-08-10
---

# 다이나믹 프로그래밍
- 컴퓨터를 활용해도 풀기 어려운 문제는 시간이 많이 걸리는 최적의 해 나 메모리 공간이 많이 필요한 경우이다.
- 메모리 공간을 약간 더 사용하여 연산 속도를 비약적으로 중가하는 방법을 다이나믹 프로그래밍(동적 계획법)이라한다.
- 다이나믹이란 프로그램이 실행되는 도중에라는 의미로 프로그램 실행중에 메모리를 할당하는 것이 예이다.
- 대표적으로 피보나치 수열을 다이나믹 프로그래밍으로 풀 수 있다.  
  피보나치는 점화식으로 표현하면 An+2 = An+1 + An이다.  
  코드로 다음과 같다.  

  ```python
  def fibo(x):
    if x == 1 or x == 2:
        return 1
    return fibo(x - 1) + fibo(x - 2)
  ```
  이 코드의 문제는 큰 수를 계산 할 때 시간이 비약적으로 상승한다.  
  그러므로 다이나믹 프로그래밍으로 표현하면 다음과 같다.  

  ```python
  d = [0] * 100

  def fibo(x):
    if x == 1 or x == 2:
        return 1
    
    if d[x] != 0:
        return d[x]
    
    d[x] = fibo(x - 1) + fibo(x - 2)
    return d[x]
  ```

  메모이제이션 기법을 사용하여 한 번 구한 결과를 메모리 공간에 저장하여 시간을 단축시킨다.O(N)    
  이러한 방법을 **캐싱** 이라고 한다.
- 다이나믹 프로그래밍이란 큰 문제를 작게 나누고 같은 문제라면 한번씩만 풀어 문제를 효율적으로 해결하는 알고리즘이다.  
- 대표적으로 퀵정렬이 다이나믹 프로그래밍이라 할 수 있다.
- 위와 같이 큰 문제를 작게 나누기 위해 재귀함수를 사용하는 경우를 탑다운 방식이라고 한다.
- 단순히 반복문을 사용하여 작은 문제부터 큰 문제로 차근차근 하는 법을 바텀업 방식이라고 한다.  

  ```python
  d = [0] * 100

  d[1] = 1
  d[2] = 1

  for i in range(3, n + 1):
    d[i] = d[i - 1] + d[i - 2]
  ```  

- 대부분의 다이나믹 프로그래밍은 바텀업 형식이다.  
- 만약 완전 탐색을 하는데 시간이 오래 걸리면 다이나믹 프로그래밍을 적용 할 수 있는지 확인해봐야 한다.

# 문제

## 1.1로 만들기
정수x가 주어질 때 다음과 같이 연산한다.
1. x가 5로 나누어 떨어지면 5로 나눈다.
2. x가 3으로 나누어 떨어지면 3으로 나눈다.
3. x가 2로 나누어 떨어지면 2로 나눈다.
4. x에서 1을 뺀다.
연산을 사용하는 횟수의 최솟값을 구하라.

## 1-1.내가 푼 답

```python
x = int(input())

dp = [0] * 30001

for i in range(1, x + 1):
    if i + 1 > 30000:
        break
    elif dp[i + 1] == 0:
        dp[i + 1] = dp[i] + 1
    else:
        dp[i + 1] = min(dp[i] + 1, dp[i + 1])
    
    if i * 2 > 30000:
        break
    if dp[i * 2] == 0:
        dp[i * 2] = dp[i] + 1
    else:
        dp[i * 2] = min(dp[i] + 1, dp[i * 2])
    
    if i * 3 > 30000:
        break
    if dp[i * 3] == 0:
        dp[i * 3] = dp[i] + 1
    else:
        dp[i * 3] = min(dp[i] + 1, dp[i * 3])
    
    if i * 5 > 30000:
        break
    if dp[i * 5] == 0:
        dp[i * 5] = dp[i] + 1
    else:
        dp[i * 5] = min(dp[i] + 1, dp[i * 5])

print(dp[x])

```

## 1-2.답안 예시

```python
x = int(input())

d = [0] * 30001

for i in range(2, x + 1):
    d[i] = d[i - 1] + 1
    
    if i % 2 == 0:
        d[i] = min(d[i], d[i // 2] + 1)
    if i % 3 == 0:
        d[i] = min(d[i], d[i // 3] + 1)
    if i % 5 == 0:
        d[i] = min(d[i], d[i // 5] + 1)

print(d[x]) 
```

## 1-3.답안 예시와 내가 푼 답의 차이점
나는 바텀업 형식으로 풀었고 예시는 탑다운 방식인데 이 문제에서는 탑다운 방식이 더 효율적으로 해결할 수 있는 것 같다.

## 2.개미 전사
개미들이 메뚜기마을의 식량을 공격하려고 한다.  
각 식량창고는 일직선으로 늘어져있는데 바로 옆에 있는 식량창고를 공격받으면 알아챈다.  
즉 식량창고가 4개가 있다고 하면 1,3 1,4 2,4 이렇게 공격해야한다.  
식량을 턴 값의 최대값을 구하라.

## 2-1.내가 푼 답(못풀었음)

```python

```
  
## 2-2.답안 예시
```python
n = int(input())

array = list(map(int, input().split()))

d = [0] * 100

d[0] = array[0]
d[1] = max(array[0], array[1])
for i in range(2, n):
    d[i] = max(d[i - 1], d[i - 2] + array[i])

print(d[n - 1])
```

## 2-3.답안 예시와 내가 푼 답의 차이점
점화식을 우선 세워보면 왼쪽부터 차례대로 식량창고를 턴다는 경우와
i번째 식량창고에서 털지 안털지에 대해 2가지만 생각하면 된다.  
1. i-1번째 식량창고를 턴다고 하면 현재 식량창고를 털 수 없다.
2. i-2번째 식량창고를 턴다고 하면 현재 식량창고를 털 수 있다.
따라서 1과 2중 큰 것을 생각하면 된다.  
여기서 고려할 점은 i-3번째이하는 메모리제이션을 쓰기 때문에 생각을 안해도 된다.  

문제는 못풀었지만 가장 중요한것은 점화식을 어떻게 만들지인 것같다.

## 3.바닥 공사
가로길이가 N 세로길이가 2인 바닥이 있는데 1x2, 2x1, 2x1로 바닥을 채우는 경우의 수를 모두 구하라.

## 3-1.내가 푼 답

```python
n = int(input())

d = [0] * 1001

d[1] = 1
d[2] = 3

for i in range(3, n+1):
    d[i] = d[i - 1] + d[i - 2] * 2

print(d[i] % 796796)

```
  
## 3-2.답안 예시

```python
n = int(input())

d = [0] * 1001

d[1] = 1
d[2] = 3

for i in range(3, n+1):
    d[i] = (d[i - 1] + 2 * d[i - 2]) % 796796

print(d[n])
```

## 3-3.답안 예시와 내가 푼 답의 차이점
점화식을 생각하면 쉽게 풀리는 문제인 것같다.


## 4.효율적인 화폐 구성
N가지 화폐가 있는데 화폐의 개수를 최소한으로 사용하여 M원을 만들어라.
예를들어 15원을 만들려고 할 때 2,3원이 있다고 하면 3원 5개를 사용하는 것이 최소로 한다.

## 4-1.내가 푼 답(못풀었음)

```python
n, m = map(int, input().split())

d = [0] * 10001

array = []
for _ in range(n):
    coin = int(input())
    d[coin] = 1
    array.append(coin)

array.sort()


```
  
## 4-2.답안 예시

```python
n, m = map(int, input().split())

array = []
for i in range(n):
    array.append(int(input()))

d = [10001] * (m + 1)

d[0] = 0
for i in range(n):
    for j in range(array[i], m + 1):
        if d[j - array[i]] != 10001:
            d[j] = min(d[j], d[j - array[i]] + 1)

if d[m] == 10001:
    print(-1)
else:
    print(d[m])
```

## 4-3.답안 예시와 내가 푼 답의 차이점
적은 금액부터 큰 금액까지 차례로 확인 하는 바텀업방식을 사용한다.  
최소한 화폐 개수를 ai 화폐단위를 k라 할때
1. ai-k를 만들 수 있으면 ai = min(ai, ai + 1)
2. ai-k가 없으면 ai == 10001

다이나믹부분은 확실히 점화식을 잘 생각해야 할 것 같다.   