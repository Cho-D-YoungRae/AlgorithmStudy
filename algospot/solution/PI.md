# 원주율 외우기

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

import static java.lang.Math.min;

public class Main {

    private static final int INF = 987654321;

    private static int[] pi;
    private static int[] cache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine().trim());

        for (int testcase = 0; testcase < c; testcase++) {
            pi = Arrays.stream(br.readLine().trim().split(""))
                    .mapToInt(Integer::parseInt)
                    .toArray();
            cache = new int[pi.length];
            Arrays.fill(cache, -1);

            System.out.println(solution(0));
        }
    }

    private static int solution(int idx) {
        if (idx == pi.length) {
            return 0;
        }
        int ret = cache[idx];
        if (ret != -1) {
            return ret;
        }

        ret = INF;
        if (idx + 2 < pi.length) {
            ret = min(ret, calDiff(idx, idx + 2) + solution(idx + 3));
        }
        if (idx + 3 < pi.length) {
            ret = min(ret, calDiff(idx, idx + 3) + solution(idx + 4));
        }
        if (idx + 4 < pi.length) {
            ret = min(ret, calDiff(idx, idx + 4) + solution(idx + 5));
        }

        return cache[idx] = ret;
    }

    private static int calDiff(int startIdx, int endIdx) {
        if (diff1(startIdx, endIdx)) {
            return 1;
        } else if (diff2(startIdx, endIdx)) {
            return 2;
        } else if (diff4(startIdx, endIdx)) {
            return 4;
        } else if (diff5(startIdx, endIdx)) {
            return 5;
        } else {
            return 10;
        }
    }

    private static boolean diff1(int startIdx, int endIdx) {
        int num = pi[startIdx];
        for (int i = startIdx + 1; i <= endIdx; i++) {
            if (num != pi[i]) {
                return false;
            }
        }

        return true;
    }

    private static boolean diff2(int startIdx, int endIdx) {
        int d = pi[startIdx + 1] - pi[startIdx];
        if (Math.abs(d) != 1) {
            return false;
        }

        for (int i = startIdx + 1; i <= endIdx; i++) {
            if (pi[i] - pi[i - 1] != d) {
                return false;
            }
        }

        return true;
    }

    private static boolean diff4(int startIdx, int endIdx) {
        for (int i = startIdx; i <= endIdx - 2; i++) {
            if (pi[i] != pi[i + 2]) {
                return false;
            }
        }
        return true;
    }

    private static boolean diff5(int startIdx, int endIdx) {
        int d = pi[startIdx + 1] - pi[startIdx];
        for (int i = startIdx + 2; i <= endIdx; i++) {
            if (pi[i] - pi[i - 1] != d) {
                return false;
            }
        }
        return true;
    }
}
```
