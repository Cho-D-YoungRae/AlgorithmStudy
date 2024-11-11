# Longest Increasing Sequence

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

    private static int[] sequence;
    private static int[] cache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            int n = Integer.parseInt(br.readLine());
            sequence = Arrays.stream(br.readLine().split(" "))
                    .mapToInt(Integer::parseInt)
                    .toArray();
            cache = new int[n];
            Arrays.fill(cache, -1);

            int result = 0;
            for (int i = 0; i < n; i++) {
                result = Math.max(result, lis(i));
            }
            System.out.println(result);
        }
    }

    private static int lis(int idx) {
        if (idx == sequence.length - 1) {
            return 1;
        }

        int ret = cache[idx];
        if (ret != -1) {
            return ret;
        }

        ret = 1;
        for (int i = idx + 1; i < sequence.length; i++) {
            if (sequence[idx] < sequence[i]) {
                ret = Math.max(ret, lis(i) + 1);
            }
        }

        return cache[idx] = ret;
    }
}

```
