# 삼각형 위의 최대 경로 수 세기

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

    private static int n;
    private static int[][] triangle;
    private static int[][] pathSumMaxCache;
    private static int[][] pathCountCache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            n = Integer.parseInt(br.readLine());
            triangle = new int[n][];
            pathSumMaxCache = new int[n][];
            pathCountCache = new int[n][];

            for (int i = 0; i < n; i++) {
                triangle[i] = Arrays.stream(br.readLine().split(" "))
                        .mapToInt(Integer::parseInt)
                        .toArray();
                pathSumMaxCache[i] = new int[triangle[i].length];
                pathCountCache[i] = new int[triangle[i].length];
            }

            System.out.println(trianglePathCount(0, 0).pathCount);
        }
    }

    private static Result trianglePathCount(int y, int x) {
        if (pathCountCache[y][x] != 0) {
            return new Result(pathSumMaxCache[y][x], pathCountCache[y][x]);
        }

        if (y == n - 1) {
            pathSumMaxCache[y][x] = triangle[y][x];
            pathCountCache[y][x] = 1;
            return new Result(pathSumMaxCache[y][x], pathCountCache[y][x]);
        }

        Result lower = trianglePathCount(y + 1, x);
        Result lowerRight = trianglePathCount(y + 1, x + 1);

        if (lower.pathSumMax == lowerRight.pathSumMax) {
            pathSumMaxCache[y][x] = lower.pathSumMax + triangle[y][x];
            pathCountCache[y][x] = lower.pathCount + lowerRight.pathCount;
        } else {
            Result maxResult = lower.pathSumMax > lowerRight.pathSumMax ? lower : lowerRight;
            pathSumMaxCache[y][x] = maxResult.pathSumMax + triangle[y][x];
            pathCountCache[y][x] = maxResult.pathCount;
        }

        return new Result(pathSumMaxCache[y][x], pathCountCache[y][x]);
    }

    static class Result {
        final int pathSumMax;
        final int pathCount;

        public Result(int pathSumMax, int pathCount) {
            this.pathSumMax = pathSumMax;
            this.pathCount = pathCount;
        }
    }
}

```
