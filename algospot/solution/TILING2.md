# 타일링

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

    private static final int MOD = 1000000007;

    private static int[] cache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            int n = Integer.parseInt(br.readLine());
            cache = new int[n + 1];
            Arrays.fill(cache, -1);
            System.out.println(tiling(n));
        }
    }

    private static int tiling(int n) {
        if (n == 1) {
            return 1;
        }
        if (n == 2) {
            return 2;
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
