# [소풍](https://www.algospot.com/judge/problem/read/PICNIC)

## Solution 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {

    private static class FriendPair {
        final int f1;
        final int f2;

        public FriendPair(int f1, int f2) {
            this.f1 = f1;
            this.f2 = f2;
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int C = Integer.parseInt(bf.readLine());

        StringBuilder sb = new StringBuilder();
        for (int c = 0; c < C; c++) {
            StringTokenizer st = new StringTokenizer(bf.readLine());
            int n = Integer.parseInt(st.nextToken());
            int m = Integer.parseInt(st.nextToken());

            List<FriendPair> pairs = new ArrayList<>();

            st = new StringTokenizer(bf.readLine());
            for (int i = 0; i < m; i++) {
                int f1 = Integer.parseInt(st.nextToken());
                int f2 = Integer.parseInt(st.nextToken());
                pairs.add(new FriendPair(f1, f2));
            }

            sb.append(findFriendPariNum(pairs, 0, n / 2, new boolean[n]));
            sb.append('\n');
        }

        System.out.println(sb);
    }

    private static int findFriendPariNum(List<FriendPair> pairs, int start, int needPair, boolean[] visited) {
        if (needPair == 0) {
            return 1;
        }
        if (start >= pairs.size()) {
            return 0;
        }

        int result = 0;
        for (int i = start; i < pairs.size(); i++) {
            FriendPair pair = pairs.get(i);
            if (visited[pair.f1] || visited[pair.f2]) {
                continue;
            }
            visited[pair.f1] = true;
            visited[pair.f2] = true;

            result += findFriendPariNum(pairs, i + 1, needPair - 1, visited);

            visited[pair.f1] = false;
            visited[pair.f2] = false;
        }

        return result;
    }
}
```
