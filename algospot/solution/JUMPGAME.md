# 외발 뛰기

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
            int[][] board = new int[n][];
            for (int i = 0; i < n; i++) {
                board[i] = Arrays.stream(bf.readLine().split(" "))
                        .mapToInt(Integer::parseInt)
                        .toArray();
            }

            String result = jump(board) ? "YES" : "NO";
            System.out.println(result);
        }
    }

    private static final int NOT_VISITED = -1;
    private static final int AVAILABLE = 1;
    private static final int UNAVAILABLE = 0;

    private static boolean jump(int[][] board) {
        int n = board.length;
        int[][] cache = new int[n][n];
        for (int[] row : cache) {
            Arrays.fill(row, NOT_VISITED);
        }

        return doJump(board, cache, 0, 0);
    }

    private static boolean doJump(int[][] board, int[][] cache, int y, int x) {
        int n = board.length;
        if (cache[y][x] != NOT_VISITED) {
            return cache[y][x] == AVAILABLE;
        }
        if (y == n - 1 && x == n - 1) {
            return true;
        }

        boolean result = false;
        int movingSize = board[y][x];

        if (x + movingSize < n) {
            result = doJump(board, cache, y, x + movingSize);
        }

        if (!result && y + movingSize < n) {
            result = doJump(board, cache, y + movingSize, x);
        }

        cache[y][x] = result ? AVAILABLE : UNAVAILABLE;
        return cache[y][x] == AVAILABLE;
    }

}

```
