# 두니발 박사의 탈옥

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.StringTokenizer;
import java.util.stream.Collectors;

public class Main {
    
    private static double[][] cache;
    private static int n;
    private static int p;
    private static List<List<Integer>> graph;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            n = Integer.parseInt(st.nextToken());
            int d = Integer.parseInt(st.nextToken());
            p = Integer.parseInt(st.nextToken());

            cache = new double[n][d + 1];
            for (int i = 0; i < n; i++) {
                Arrays.fill(cache[i], -1);
            }

            graph = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                graph.add(new ArrayList<>());
                int[] row = Arrays.stream(br.readLine().split(" "))
                        .mapToInt(Integer::parseInt).toArray();
                for (int j = 0; j < n; j++) {
                    if (row[j] == 1) {
                        graph.get(i).add(j);
                    }
                }
            }

            int t = Integer.parseInt(br.readLine());
            List<Double> results = new ArrayList<>();
            int[] qArr = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();
            for (int q : qArr) {
                results.add(prob(q, d));
            }

            System.out.println(
                    results.stream()
                            .map(String::valueOf)
                            .collect(Collectors.joining(" "))
            );
        }
    }

    private static double prob(int country, int day) {

        if (day == 0) {
            return country == p ? 1 : 0;
        }

        double ret = cache[country][day];
        if (ret != -1) {
            return ret;
        }

        ret = 0;
        for (int nearCountry : graph.get(country)) {
            ret += prob(nearCountry, day - 1) / graph.get(nearCountry).size();
        }

        return cache[country][day] = ret;
    }
}

```
