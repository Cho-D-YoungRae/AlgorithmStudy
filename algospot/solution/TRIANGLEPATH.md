# 삼각형 위의 최대 경로

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(bf.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            int n = Integer.parseInt(bf.readLine());
            int[][] triangle = new int[n][];
            for (int i = 0; i < n; i++) {
                triangle[i] = Arrays.stream(bf.readLine().split(" +"))
                        .mapToInt(Integer::parseInt)
                        .toArray();
            }

            System.out.println(solution(triangle));
        }

    }

    private static int solution(int[][] triangle) {

        int n = triangle.length;

        int[][] cache = new int[n][];
        for (int i = 0; i < n; i++) {
            cache[i] = new int[triangle[i].length];
            Arrays.fill(cache[i], -1);
        }

        return doSolution(triangle, cache, 0, 0);
    }

    private static int doSolution(int[][] triangle, int[][] cache, int y, int x) {
        if (cache[y][x] != -1) {
            return cache[y][x];
        }

        int n = triangle.length;
        if (y == n - 1) {
            return triangle[y][x];
        }

        int prev = 0;
        prev = Math.max(prev, doSolution(triangle, cache, y + 1, x));

        if (x + 1 < triangle[y + 1].length) {
            prev = Math.max(prev, doSolution(triangle, cache, y + 1, x + 1));
        }

        cache[y][x] = triangle[y][x] + prev;
        return cache[y][x];
    }

}

```
