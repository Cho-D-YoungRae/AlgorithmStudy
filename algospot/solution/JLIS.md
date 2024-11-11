# 합친 LIS

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {

    private static int[] A, B;
    private static int[][] cache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int n = Integer.parseInt(st.nextToken());
            int m = Integer.parseInt(st.nextToken());

            A = Arrays.stream(br.readLine().split(" "))
                    .mapToInt(Integer::parseInt)
                    .toArray();
            B = Arrays.stream(br.readLine().split(" "))
                    .mapToInt(Integer::parseInt)
                    .toArray();

            cache = new int[A.length + 1][B.length + 1];
            for (int i = 0; i < cache.length; i++) {
                Arrays.fill(cache[i], -1);
            }


            System.out.println(jlis(-1, -1) - 2);
        }
    }

    private static int jlis(int indexA, int indexB) {
        int ret = cache[indexA + 1][indexB + 1];
        if (ret != -1) {
            return ret;
        }

        ret = 2;
        long a = indexA == -1 ? Long.MIN_VALUE : A[indexA];
        long b = indexB == -1 ? Long.MIN_VALUE : B[indexB];
        long maxElement = Math.max(a, b);

        for (int nextA = indexA + 1; nextA < A.length; nextA++) {
            if (A[nextA] > maxElement) {
                ret = Math.max(ret, jlis(nextA, indexB) + 1);
            }
        }

        for (int nextB = indexB + 1; nextB < B.length; nextB++) {
            if (B[nextB] > maxElement) {
                ret = Math.max(ret, jlis(indexA, nextB) + 1);
            }
        }

        return cache[indexA + 1][indexB + 1] = ret;
    }
}
```
