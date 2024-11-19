# Mismatched Brackets

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Deque;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class Main {

    private static Map<String, String> openToClose;

    public static void main(String[] args) throws IOException {
        openToClose = new HashMap<>();
        openToClose.put("(", ")");
        openToClose.put("{", "}");
        openToClose.put("[", "]");

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            List<String> brackets = Arrays.stream(br.readLine().split(""))
                    .collect(Collectors.toList());
            String result = match(brackets) ? "YES" : "NO";
            System.out.println(result);
        }
    }

    private static boolean match(List<String> brackets) {
        Deque<String> stack = new ArrayDeque<>();
        for (String bracket : brackets) {
            if (isOpen(bracket)) {
                stack.offerLast(bracket);
            } else {
                if (stack.isEmpty() || !matchPair(stack.pollLast(), bracket)) {
                    return false;
                }
            }
        }

        return stack.isEmpty();
    }

    private static boolean isOpen(String bracket) {
        return openToClose.containsKey(bracket);
    }

    private static boolean matchPair(String open, String close) {
        return openToClose.get(open).equals(close);
    }
}

```
