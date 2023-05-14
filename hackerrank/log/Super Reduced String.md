# [Super Reduced String](https://www.hackerrank.com/challenges/reduced-string/problem?isFullScreen=true)

## 정리

- 스택 이용

## Solution 1

```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

class Result {

    /*
     * Complete the 'superReducedString' function below.
     *
     * The function is expected to return a STRING.
     * The function accepts STRING s as parameter.
     */

    public static String superReducedString(String s) {
        Deque<Character> stack = new LinkedList<>();
        for (char c : s.toCharArray()) {
            if (!stack.isEmpty() && stack.peekLast() == c) {
                while (!stack.isEmpty() && stack.peekLast() == c) {
                    stack.pollLast();
                }
            } else {
                stack.offerLast(c);
            }
        }
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pollFirst());
        }
        String result = sb.toString();
        return !result.isEmpty() ? result : "Empty String";
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String s = bufferedReader.readLine();

        String result = Result.superReducedString(s);

        bufferedWriter.write(result);
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
