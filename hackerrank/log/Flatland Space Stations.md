# [Flatland Space Stations](https://www.hackerrank.com/challenges/flatland-space-stations/problem?isFullScreen=true)

## 정리

- 거리 계산 등 반드시 양수일 때는 실수하지 않도록 abs() 함수 사용하기

## Solution 1

```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    // Complete the flatlandSpaceStations function below.
    static int flatlandSpaceStations(int n, int[] c) {
        Arrays.sort(c);
        int[] distances = new int[n];
        int last = -1;
        int next = 0;
        
        for (int cur = 0; cur < n; cur++) {
            // abs() 사용 안하면 c[next] 가 cur 보다 이전일 경우 음수가 나옴
            int lastDistance = last >= 0 ? Math.abs(cur - c[last]) : Integer.MAX_VALUE;
            int nextDistance = last < c.length ? Math.abs(c[next] - cur) : Integer.MAX_VALUE;
            int distance = Math.min(lastDistance, nextDistance);
            distances[cur] = distance;
            
            if (next < c.length - 1 && cur == c[next]) {
                last += 1;
                next += 1;
            }
        }
        
        return Arrays.stream(distances).max().getAsInt();
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nm = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nm[0]);

        int m = Integer.parseInt(nm[1]);

        int[] c = new int[m];

        String[] cItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < m; i++) {
            int cItem = Integer.parseInt(cItems[i]);
            c[i] = cItem;
        }

        int result = flatlandSpaceStations(n, c);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
```
