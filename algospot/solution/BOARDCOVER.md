# [게임판 덮기](https://www.algospot.com/judge/problem/read/BOARDCOVER)

## 정리

- 우리는 최대 50/3 = 16 개의 블록을 놓기 때문에 가능한 답의 상한은 4^16 개가 될 것 같지만, 실제 입력을 생각해보면 가능한 경우의 수가 많이 제한되기 때문에 완전탐색이 가능함
- 게임판을 덮는 블럭의 크기가 제각각이라 블럭으로 덮은 후 다음 칸을 찾기가 어려운데 게임판이 작기 때문에 전체를 돌면서 찾아도 괜찮다.

## Wrong 1

```java
package org.example;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int C = Integer.parseInt(bf.readLine());

        StringBuilder sb = new StringBuilder();
        for (int c = 0; c < C; c++) {
            StringTokenizer st = new StringTokenizer(bf.readLine());
            int H = Integer.parseInt(st.nextToken());
            int W = Integer.parseInt(st.nextToken());
            char[][] board = new char[H][];
            for (int i = 0; i < H; i++) {
                board[i] = bf.readLine().toCharArray();
            }

            sb.append(findCoverBoardNum(board, 0));
            sb.append('\n');
        }

        System.out.println(sb);
    }

    private static int[][] blockY = {{0, 0, 1}, {0, 0, 1}, {0, 1, 1}, {0, 1, 1}};
    private static int[][] blockX = {{0, 1, 0}, {0, 1, 1}, {1, 1, 0}, {0, 1, 0}};

    private static int findCoverBoardNum(char[][] board, int depth) {
        int H = board.length;
        int W = board[0].length;
        if (depth == H * W) {
            return isCovered(board) ? 1 : 0;
        }

        int curY = depth / H;
        int curX = depth % H;

        int result = 0;
        for (int i = 0; i < 4; i++) {
            if (!canCover(board, curY, curX, i)) {
                continue;
            }
            cover(board, curY, curX, i, '#');
            result += findCoverBoardNum(board, depth + 1);
            cover(board, curY, curX, i, '.');
        }

        result += findCoverBoardNum(board, depth + 1);

        return result;
    }

    private static boolean isCovered(char[][] board) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                if (board[i][j] == '.') {
                    return false;
                }
            }
        }
        return true;
    }

    private static boolean canCover(char[][] board, int y, int x, int d) {
        int H = board.length;
        int W = board[0].length;
        for (int i = 0; i < 3; i++) {
            int ny = y + blockY[d][i];
            int nx = x + blockX[d][i];
            if (ny < 0 || ny >= H || nx < 0 || nx >= W) {
                return false;
            }
            if (board[ny][nx] == '#') {
                return false;
            }
        }

        return true;
    }

    private static void cover(char[][] board, int y, int x, int d, char c) {
        for (int i = 0; i < 3; i++) {
            int ny = y + blockY[d][i];
            int nx = x + blockX[d][i];
            board[ny][nx] = c;
        }
    }
}

/*
3
3 7
#.....#
#.....#
##...##
3 7
#.....#
#.....#
##..###
8 10
##########
#........#
#........#
#........#
#........#
#........#
#........#
##########
 */
```
