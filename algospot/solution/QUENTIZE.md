# Quantization

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {

    private static final int INF = 987654321;

    private static int n, s;
    private static int[] seq;
    private static int[][] cache;
    private static int[] pSum, pSqSum;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            n = Integer.parseInt(st.nextToken());
            s = Integer.parseInt(st.nextToken());
            seq = Arrays.stream(br.readLine().split("\\s+"))
                    .mapToInt(Integer::parseInt)
                    .toArray();
            cache = new int[n][s + 1];
            for (int i = 0; i < n; i++) {
                Arrays.fill(cache[i], -1);
            }
            prepare();
            System.out.println(quantize(0, s));
        }
    }

    private static void prepare() {
        Arrays.sort(seq);
        pSum = new int[n];
        pSqSum = new int[n];
        pSum[0] = seq[0];
        pSqSum[0] = seq[0] * seq[0];

        for (int i = 1; i < n; i++) {
            pSum[i] = pSum[i - 1] + seq[i];
            pSqSum[i] = pSqSum[i - 1] + seq[i] * seq[i];
        }
    }

    private static int quantize(int index, int s) {
        if (index == n) {
            return 0;
        }

        if (s == 0) {
            return INF;
        }

        int ret = cache[index][s];
        if (ret != -1) {
            return ret;
        }

        ret = INF;

        for (int next = index + 1; next <= n; next++) {
            ret = Math.min(ret, calculateMin(index, next) + quantize(next, s - 1));
        }

        return cache[index][s] = ret;
    }

    private static int calculateMin(int a, int b) {
        int sum = a == 0 ? pSum[b - 1] : pSum[b - 1] - pSum[a - 1];
        int sqSum = a == 0 ? pSqSum[b - 1] : pSqSum[b - 1] - pSqSum[a - 1];
        int m = (int) Math.round(((double) sum) / (b - a));
        return sqSum - 2 * m * sum + (b - a) * m * m;
    }
}

```
