# Wildcard

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Pattern;
import java.util.stream.Collectors;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(bf.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            String w = bf.readLine();
            int n = Integer.parseInt(bf.readLine());
            List<String> filenames = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                filenames.add(bf.readLine());
            }

            solution(w, filenames).forEach(System.out::println);
        }

    }

    private static List<String> solution(String w, List<String> filenames) {
        Pattern pattern = Pattern.compile(w
                .replace("?", "[A-Za-z0-9]?")
                .replace("*", "[A-Za-z0-9]*")
        );

        return filenames
                .stream()
                .filter(filename -> pattern.matcher(filename).matches())
                .sorted()
                .collect(Collectors.toList());
    }

}

```

## 2

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            String w = br.readLine();

            int n = Integer.parseInt(br.readLine());
            List<String> results = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                String f = br.readLine();
                if (match(w, f)) {
                    results.add(f);
                }
            }
            results.stream()
                    .sorted()
                    .forEach(System.out::println);
        }
    }

    private static boolean match(String w, String f) {
        int[][] cache = new int[w.length() + 1][f.length() + 1];
        for (int i = 0; i < cache.length; i++) {
            Arrays.fill(cache[i], -1);
        }

        return doMatch(w, f, cache, 0, 0);
    }

    private static boolean doMatch(String w, String f, int[][] cache, int idxW, int idxF) {
        if (idxW == w.length() && idxF == f.length()) {
            return true;
        }
        if (idxW == w.length() && idxF != f.length()) {
            return false;
        }

        int ret = cache[idxW][idxF];
        if (ret != -1) {
            return ret == 1;
        }

        ret = 0;

        if (idxF != f.length() && (w.charAt(idxW) == f.charAt(idxF) || w.charAt(idxW) == '?')) {
            ret = doMatch(w, f, cache, idxW + 1, idxF + 1)
                    ? 1 : 0;
            cache[idxW][idxF] = ret;
            return ret == 1;
        }

        boolean result = false;
        if (w.charAt(idxW) == '*') {
            result = doMatch(w, f, cache, idxW + 1, idxF);
            if (f.length() != idxF) {
                result = result || doMatch(w, f, cache, idxW, idxF + 1);
            }
        }

        cache[idxW][idxF] = ret = result ? 1 : 0;
        return ret == 1;
    }
}

```
