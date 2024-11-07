# 비대칭 타일링

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

    private static final int MOD = 1_000_000_007;

    private static int[] cache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            int n = Integer.parseInt(br.readLine());
            cache = new int[n];
            Arrays.fill(cache, -1);

            System.out.println(asymtiling(n));
        }
    }

    private static int asymtiling(int n) {
        if (n == 1) {
            return 1;
        }
        int ret = (tiling(n - 1) + tiling(n - 2)) % MOD;
        ret = (ret - tiling(n / 2) % MOD + MOD) % MOD;
        if (n % 2 == 0) {
            ret = (ret - tiling((n - 2) / 2) % MOD + MOD) % MOD;
        }
        return ret % MOD;
    }

    private static int tiling(int n) {
        if (n <= 1) {
            return 1;
        }

        int ret = cache[n];
        if (ret != -1) {
            return ret;
        }

        ret = (tiling(n - 1) + tiling(n - 2)) % MOD;
        return cache[n] = ret;
    }

}

```
