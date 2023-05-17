# [Forming a Magic Square](https://www.hackerrank.com/challenges/magic-square-forming/problem?isFullScreen=true)

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
     * Complete the 'formingMagicSquare' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts 2D_INTEGER_ARRAY s as parameter.
     */

    public static int formingMagicSquare(List<List<Integer>> s) {
        return findMinCost(s, new ArrayList<>(), new boolean[9 + 1]);
    }
    
    private static int findMinCost(List<List<Integer>> s, List<Integer> acc, boolean[] visited) {
        if (acc.size() == 9) {
            if (!isMagicSquare(acc)) {
                return Integer.MAX_VALUE;
            }
            int cost = 0;
            for (int i = 0; i < s.size(); i++) {
                for (int j = 0; j < s.get(i).size(); j++) {
                    cost += Math.abs(s.get(i).get(j) - acc.get(i * 3 + j));
                }
            }
            if (cost == 19) {
                System.out.println(acc);
            }
            return cost;
        }
        
        int cost = Integer.MAX_VALUE;
        for (int i = 1; i <= 9; i++) {
            if (visited[i]) {
                continue;
            }
            visited[i] = true;
            acc.add(i);
            
            cost = Math.min(cost, findMinCost(s, acc, visited));
            
            visited[i] = false;
            acc.remove(acc.size() - 1);
        }
        return cost;
    }
    
    private static boolean isMagicSquare(List<Integer> acc) {
        int row1 = acc.get(0) + acc.get(1) + acc.get(2);
        int row2 = acc.get(3) + acc.get(4) + acc.get(5);
        int row3 = acc.get(6) + acc.get(7) + acc.get(8);
        int col1 = acc.get(0) + acc.get(3) + acc.get(6);
        int col2 = acc.get(1) + acc.get(4) + acc.get(7);
        int col3 = acc.get(2) + acc.get(5) + acc.get(8);
        int diag1 = acc.get(0) + acc.get(4) + acc.get(8);
        int diag2 = acc.get(2) + acc.get(4) + acc.get(6);
        return row1 == 15 && row2 == 15 && row3 == 15
            && col1 == 15 && col2 == 15 && col3 == 15 
            && diag1 == 15 && diag2 == 15;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        List<List<Integer>> s = new ArrayList<>();

        IntStream.range(0, 3).forEach(i -> {
            try {
                s.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .map(Integer::parseInt)
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        int result = Result.formingMagicSquare(s);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
