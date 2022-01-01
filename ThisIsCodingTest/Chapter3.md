# Chapter 03. 그리디

## 큰 수의 법칙 (92pg)

```python
N, M, K = map(int, input().split())
    
numList = list(map(int, input().split()))

numList.sort(reverse=True)

d, m = divmod(M, K+1)

result = numList[0]*(d*K + m) + numList[1]*d

print(result)
```

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        final int N = sc.nextInt(), M = sc.nextInt(), K = sc.nextInt();
        List<Integer> list = new ArrayList<>(N);
        for (int i = 0; i < N; i++) {
            list.add(sc.nextInt());
        }
        list.sort(Comparator.reverseOrder());
        int numSecondMax = M / (K + 1);
        int numFirstMax = M - numSecondMax;
        int result = numFirstMax * list.get(0) + numSecondMax * list.get(1);
        System.out.println(result);
    }
}
```

## 숫자 카드 게임 (96pg)

```python
def main():
  N, M = map(int, input().split())

  cards = []
  result = -float("inf")
  for i in range(N):
    cards.append(list(map(int, input().split())))
    result = max(result, min(cards[i]))

  print(result)

  
main()
```

```java
public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt(), M = sc.nextInt();
        int[][] cards = new int[N][M];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                cards[i][j] = sc.nextInt();
            }
        }
        int result = Arrays.stream(cards)
                .mapToInt(rowCards -> Arrays.stream(rowCards).min().getAsInt())
                .max().getAsInt();

        System.out.println(result);
    }
}
```

## 1이 될 때까지 \*

```python
def main():
  N, K = map(int, input().split())

  result = 0
  while N != 1:
    if N % K == 0:
      N //= K
    else:
      N -= 1
    
    result += 1

  print(result)
  
main()
```

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt(), K = sc.nextInt();
        int count = 0;

        while (N > 1) {
            if (N % K == 0) {
                N /= K;
            }
            else {
                N--;
            }
            count++;
        }
        System.out.println(count);
    }
}
```
