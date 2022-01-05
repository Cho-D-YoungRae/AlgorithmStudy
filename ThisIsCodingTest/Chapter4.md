# Chapter 04. 구현

## 상하좌우 (110pg)

```python
if __name__ == '__main__':
    # 입력에 따른 이동 방향
    direction_dict = {'R': (1, 0), 'L': (-1, 0), 'U': (0, -1), 'D': (0, 1)}

    # 공간의 크기 N * N
    N = int(input())
    # 내 첫 위치
    my_x, my_y = 1, 1

    movements = input().split()

    for movement in movements:
        x, y = direction_dict[movement]
        # 공간을 벗어나지 않으면 이동
        if 1 <= my_x + x <= N and 1 <= my_y + y <= N:
            my_x += x
            my_y += y

    print(my_y, my_x)
```

## 상하좌우 (110pg) - java

> 2022/01/04

```java
import java.util.*;

public class Main {

    static boolean isAvailableMove(int[] curLoc, int[] move, int N) {
        int[] movedLoc = new int[]{curLoc[0] + move[0], curLoc[1] + move[1]};
        boolean isYAvailable = 0 <= movedLoc[0] && movedLoc[0] < N;
        boolean isXAvailable = 0 <= movedLoc[1] && movedLoc[1] < N;

        return isXAvailable && isYAvailable;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt();
        sc.nextLine();
        String[] movePlans = sc.nextLine().split(" ");
        Map<String, int[]> plan2Move = Map.of(
                "R", new int[]{0, 1},
                "L", new int[]{0, -1},
                "U", new int[]{-1, 0},
                "D", new int[]{1, 0}
        );

        int[] curLocation = new int[]{0, 0};
        Arrays.stream(movePlans).forEach((moveplan) -> {
            int[] move = plan2Move.get(moveplan);
            if (isAvailableMove(curLocation, move, N)) {
                curLocation[0] += move[0];
                curLocation[1] += move[1];
            }
        });

        System.out.println((curLocation[0] + 1)  + " " + (curLocation[1] + 1));
    }
}
```

## 시각 (113pg) - python \#

### My idea

0시부터 쭉 시간당 몇 카운트인지 미리 직접 계산 후 더해나가려했다.
그러다보니 실수가 발생

### 교재 아이디어

시, 분, 초 이렇게 3중 반복문을 사용하였다. 3중 반복문이지만 최대 24*60*60 = 86400 이고 이정도는 충분히 돌아갈 수 있다.

### My Solution

> 2022/01/04

```java
import java.util.*;

public class Main {
    static boolean isThreeContainer(int num) {
        boolean isInTen = (num / 10 == 3);
        boolean isInOne = (num % 10 == 3);
        return isInOne || isInTen;
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt();
        // 1분 동안 나타나는 3이 하나라도 포함된 경우 계산
        int countInMinute = 0;
        for (int s = 0; s < 60; s++) {
            if (isThreeContainer(s)) {
                countInMinute++;
            }
        }
        // 1시간 동안 나타나는 경우 계산
        int countInHour = 0;
        for (int m = 0; m < 60; m++) {
            if (isThreeContainer(m)) {
                countInHour += 60;
            } else {
                countInHour += countInMinute;
            }
        }
        // 정답 계산
        int result = 0;
        for (int h = 0; h <= N; h++) {
            if (isThreeContainer(h)) {
                result += 60 * 60;
            } else {
                result += countInHour;
            }
        }
        System.out.println(result);
    }
}
```

## 2. 왕실의 나이트 (115pg)

### My Solution 1

```python
if __name__ == '__main__':
    my_pos = input()
    # 내 위치 정수화
    my_y, my_x = int(my_pos[1]), (ord(my_pos[0]) - ord('a') + 1)

    # 전체 움직일 수 있는 8가지 경우
    movements = ((2, 1), (2, -1), (-2, 1), (-2, -1),\
                 (1, 2), (1, -2), (-1, 2), (-1, -2))

    result = 0
    for y, x in movements:
        if 1 <= my_y + y <= 8 and 1 <= my_x + x <= 8:
            result += 1

    print(result)
```

### My Solution 2 - java

> 2022/01/05

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        final int GARDEN = 8;
        String knightLocStr = sc.nextLine();
        int[] knightLoc = new int[]{
                knightLocStr.charAt(1) - '1',
                knightLocStr.charAt(0) - 'a'
        };

        int[][] moves = new int[][] {
                {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
                {1, 2}, {1, -2}, {-1, 2}, {-1, -2}
        };

        int result = 0;
        for (int[] move : moves) {
            int[] moved = new int[] {
                    knightLoc[0] + move[0], knightLoc[1] + move[1]};
            boolean isRowAvailable = 0 <= moved[0] && moved[0] < GARDEN;
            boolean isColAvailable = 0 <= moved[1] && moved[1] < GARDEN;
            if (isRowAvailable && isColAvailable) {
                result++;
            }
        }

        System.out.println(result);
    }
}
```

## 3. 게임 개발 (118pg) \#

### try 1

```python
def is_in_range_pos(y, x, N, M) -> bool:
    return 0 <= y <= N and 0 <= x <= M

if __name__ == '__main__':
    N, M = map(int, input().split())
    my_y, my_x, my_dir = map(int, input().split())
    game_map = []
    for _ in range(N):
        game_map.append(list(map(int, input().split())))

    movements = ((1, 0), (0, 1), (-1, 0), (0, -1))

    result = 0

    while is_in_range_pos(my_y, my_x, N, M)\
                        and not game_map[my_y][my_x]:
        movements_count = 0
        for d in range(1, 5):
            y, x = movements[my_dir - d]
            if is_in_range_pos(my_y+y, my_x+x, N, M)\
                            and not game_map[my_y+y][my_x+x]:
                game_map[my_y][my_x] = 1
                my_y += y
                my_x += x
                movements_count += 1

        if movements_count:
            result += movements_count
        else:
            y, x = movements[my_dir - 2]
            my_y += y
            my_x += x

    print(result)
```

input
> 4 4
1 1 0
1 1 1 1
1 0 0 1
1 1 0 1
1 1 1 1

output
> 2

오답 요인

1. 문제를 제대로 이해하지 못 했다.
2. 네 방향 모두 이미 가본 칸이거나 바다이면 바라보는 방향의 뒤로 한 칸 이동하게 되는데 이 때 이동하는 칸을 세지 못했다.

### My Solution 1

```python
def is_in_range_pos(y, x, N, M) -> bool:
    return 0 <= y <= N and 0 <= x <= M

if __name__ == '__main__':
    N, M = map(int, input().split())
    my_y, my_x, my_dir = map(int, input().split())
    game_map = []
    for _ in range(N):
        game_map.append(list(map(int, input().split())))

    movements = ((1, 0), (0, 1), (-1, 0), (0, -1))

    game_map[my_y][my_x] = 1
    result = 1

    while is_in_range_pos(my_y, my_x, N, M):
        movements_count = 0
        for d in range(1, 5):
            y, x = movements[my_dir - d]
            if is_in_range_pos(my_y+y, my_x+x, N, M)\
                            and not game_map[my_y+y][my_x+x]:
                game_map[my_y][my_x] = 1
                my_y += y
                my_x += x
                movements_count += 1
                my_dir = (my_dir - d) % 4
                break

        if movements_count:
            result += movements_count
        else:
            y, x = movements[my_dir - 2]
            my_y += y
            my_x += x
            if not game_map[my_y][my_x]:
                result += 1
            break

    print(result)
```

어찌어찌 답은 나오는 것 같다. 깔끔하게 고쳐보자


### My Solution 1-2

```python
if __name__ == '__main__':
    N, M = map(int, input().split())
    my_y, my_x, my_dir = map(int, input().split())
    game_map = []
    for _ in range(N):
        game_map.append(list(map(int, input().split())))

    # 북 동 남 서 방향으로 이동 할 때
    movements = ((1, 0), (0, 1), (-1, 0), (0, -1))

    # 좌표가 지도의 범위안에 있는지
    def is_in_range_pos(y, x) -> bool:
        return 0 <= y <= N and 0 <= x <= M

    # 첫 시작 위치 다녀갔으니 1로 표시
    # 바다와 마찬가지로 다녀간 곳은 다시 가지 않으니 1로
    game_map[my_y][my_x] = 1
    
    # 시작점을 지나며 시작하므로 1부터
    result = 1

    while True:
        # 이 위치에서 움직였는지 판단하기 위한
        movements_count = 0

        # 왼쪽 방향부터이므로 1부터
        for d in range(1, 5):
            y, x = movements[my_dir - d]

            # 이동하려는 곳이 범위 내에 있고 갈 수 있는 곳
            if is_in_range_pos(my_y+y, my_x+x)\
                and not game_map[my_y+y][my_x+x]:
                my_y += y
                my_x += x
                game_map[my_y][my_x] = 1
                movements_count += 1
                my_dir = (my_dir - d) % 4
                break

        # 움직임이 있었다면 result에 더해준다.
        if movements_count:
            result += movements_count
        # 움직임이 없었다면 -> 네 방향 모두 가본 곳
        else:
            y, x = movements[my_dir-2]
            # 방향을 유지한 채 뒤로 이동할 수 있나
            if is_in_range_pos(my_y+y, my_x+x)\
                and not game_map[my_y+y][my_x+x]:
                my_y += y
                my_x += x
                game_map[my_y][my_x] = 1
                result += 1
            
            # 위로 이동할 수 없으면 끝낸다.
            else:
                break

    print(result)
```

### My Solution 3

> 2022/01/05

```java
import java.util.*;

public class Main {

    static boolean isAvailableMove(List<List<Integer>> gameMap, int[] myPos, int[] movement) {
        int[] movedPos = new int[] {myPos[0] + movement[0], myPos[1] + movement[1]};
        boolean isRowAvailable = 0 <= movedPos[0] && movedPos[0] < gameMap.size();
        boolean isColAvailable = 0 <= movedPos[1] && movedPos[1] < gameMap.get(0).size();
        boolean isNotBeen = gameMap.get(movedPos[0]).get(movedPos[1]) == 0;

        return isColAvailable && isRowAvailable && isNotBeen;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int[][] movements = new int[][]{
                {-1, 0}, {0, 1}, {1, 0}, {0, -1}};
        int N = sc.nextInt(), M = sc.nextInt();
        List<List<Integer>> gameMap = new ArrayList<>(N);
        int[] myPos = new int[]{sc.nextInt(), sc.nextInt()};
        int d = sc.nextInt();
        for (int i = 0; i < N; i++) {
            gameMap.add(new ArrayList<>(M));
            for (int j = 0; j < M; j++) {
                gameMap.get(i).add(sc.nextInt());
            }
        }
        // 방문한 장소 = 2
        gameMap.get(myPos[0]).set(myPos[1], 2);

        while (true) {
            boolean isNotMoved = true;
            // 회전
            for (int r = 0; r < 4; r++) {
                d = (d + 3) % 4;
                int[] movement = movements[d];
                if (isAvailableMove(gameMap, myPos, movement)) {
                    isNotMoved = false;
                    myPos[0] = myPos[0] + movement[0];
                    myPos[1] = myPos[1] + movement[1];
                    gameMap.get(myPos[0]).set(myPos[1], 2);
                    break;
                }
            }
            if (isNotMoved) {
                int[] movement = movements[(d + 2) % 4];
                if (isAvailableMove(gameMap, myPos, movement)) {
                    isNotMoved = false;
                    myPos[0] = myPos[0] + movement[0];
                    myPos[1] = myPos[1] + movement[1];
                    gameMap.get(myPos[0]).set(myPos[1], 2);
                }
            }
            if (isNotMoved) {
                break;
            }
        }

        int result = 0;
        for (List<Integer> rowMap: gameMap) {
            for (int e: rowMap) {
                if (e == 2) {
                    result++;
                }
            }

        }

        System.out.println(result);
    }
}
```

- 너무 지저분하게 푼 듯..
- 불필요하면 굳이 List가 아니라 배열로 푸는게 더 낫나?
- 자바언어에 더 익숙해질 필요...
