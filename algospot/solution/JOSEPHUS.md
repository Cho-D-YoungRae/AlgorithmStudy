# 조세푸스 문제

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;
import java.util.stream.Collectors;

public class Main {

    private static final int MOD = 10_000_000;

    private static int[][] cache;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int n = Integer.parseInt(st.nextToken());
            int k = Integer.parseInt(st.nextToken());

            System.out.println(
                    josephus(n, k).stream()
                            .map(String::valueOf)
                            .collect(Collectors.joining(" "))
            );
        }
    }

    private static List<Integer> josephus(int n, int k) {
        List<Integer> soldiers = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            soldiers.add(i);
        }

        int idx = 0;
        for (int i = 0; i < n - 2; i++) {
            soldiers.remove(idx);
            idx = (idx + k - 1) % soldiers.size();
        }

        return soldiers;
    }

}

```
