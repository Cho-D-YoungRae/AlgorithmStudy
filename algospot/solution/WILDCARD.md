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
