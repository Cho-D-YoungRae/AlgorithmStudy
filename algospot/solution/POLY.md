# 폴리오미노

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {

    private static final int MOD = 10_000_000;

    private static int[][] cache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int n = Integer.parseInt(st.nextToken());
            cache = new int[n + 1][n + 1];
            for (int i = 0; i < cache.length; i++) {
                Arrays.fill(cache[i], -1);
            }

            int result = 0;
            for (int i = 1; i <= n; i++) {
                result += poly(i, n) % MOD;
                result %= MOD;
            }

            System.out.println(result);
        }
    }

    private static int poly(int width, int n) {
        if (width == n) {
            return 1;
        }

        int ret = cache[width][n];
        if (ret != -1) {
            return ret;
        }

        ret = 0;
        int nextN = n - width;
        for (int nextWidth = 1; nextWidth <= nextN; nextWidth++) {
            ret += ((width + nextWidth - 1) * poly(nextWidth, nextN)) % MOD;
            ret %= MOD;
        }

        return cache[width][n] = ret;
    }
}
```
