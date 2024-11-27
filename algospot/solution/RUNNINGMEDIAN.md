# 변화하는 중간값

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

class RNG {

    public static final int MOD = 20090711;

    private final int a;
    private final int b;
    private long seed;

    public RNG(int a, int b) {
        this.seed = 1983;
        this.a = a;
        this.b = b;
    }

    public int next() {
        int ret = (int) seed;
        seed = (seed * a + b) % MOD;
        return ret;
    }
}

class MedianQueue {

    private final PriorityQueue<Integer> smallPq = new PriorityQueue<>(Comparator.reverseOrder());
    private final PriorityQueue<Integer> bigPq = new PriorityQueue<>();

    void push(int element) {
        bigPq.offer(element);
        if (bigPq.size() > smallPq.size()) {
            smallPq.offer(bigPq.poll());
        }

        while (!smallPq.isEmpty() && !bigPq.isEmpty() && smallPq.peek() > bigPq.peek()) {
            int small = smallPq.poll();
            int big = bigPq.poll();
            smallPq.offer(big);
            bigPq.offer(small);
        }
    }

    int median() {
        return smallPq.peek();
    }
}

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int n = Integer.parseInt(st.nextToken());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            System.out.println(medianSum(n, a, b));
        }
    }

    private static int medianSum(int n, int a, int b) {
        RNG rng = new RNG(a, b);
        int result = 0;
        MedianQueue medianQueue = new MedianQueue();
        for (int i = 0; i < n; i++) {
            medianQueue.push(rng.next());
            result = (result + medianQueue.median()) % RNG.MOD;
        }

        return result;
    }
}

```
