# [Two Characters](https://www.hackerrank.com/challenges/two-characters/problem?isFullScreen=true)

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
     * Complete the 'alternate' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts STRING s as parameter.
     */

    public static int alternate(String s) {
        List<Character> chars = s.chars()
                .distinct()
                .mapToObj(c -> (char) c)
                .collect(Collectors.toList());
        
        int result = 0;
        for (int i = 0; i < chars.size() - 1; i++) {
            for (int j = i + 1; j < chars.size(); j++) {
                char ch1 = chars.get(i);
                char ch2 = chars.get(j);
                String t = s.replaceAll("[^" + ch1 + ch2 + "]", "");
                if (!isValid(t)) {
                    continue;
                }
                result = Math.max(result, t.length());
            }
        }
        
        return result;
    }
    
    private static boolean isValid(String t) {
        for (int i = 1; i < t.length(); i++) {
            if (t.charAt(i) == t.charAt(i - 1)) {
                return false;
            }
        }
        return true;
    }
    
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int l = Integer.parseInt(bufferedReader.readLine().trim());

        String s = bufferedReader.readLine();

        int result = Result.alternate(s);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
