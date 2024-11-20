# 외계 신호 분석

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.StringTokenizer;

public class Main {

    private static long sequenceNum;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int k = Integer.parseInt(st.nextToken());
            int n = Integer.parseInt(st.nextToken());
            sequenceNum = 1983;
            System.out.println(countKSubSequence(n, k));
        }
    }

    private static int countKSubSequence(int n, int k) {
        Deque<Integer> queue = new ArrayDeque<>();
        int count = 0;
        int sum = 0;
        for (int i = 0; i < n; i++) {
            int num = nextSequenceNum();
            queue.offerLast(num);
            sum += num;

            while (sum > k) {
                sum -= queue.pollFirst();
            }

            if (sum == k) {
                count += 1;
                sum -= queue.pollFirst();
            }
        }

        return count;
    }

    private static int nextSequenceNum() {
        int next = (int) (sequenceNum % 10000) + 1;
        sequenceNum = (sequenceNum * 214013 + 2531011) % (1L << 32);
        return next;
    }
}

```
