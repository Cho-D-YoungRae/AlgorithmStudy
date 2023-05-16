# [Beautiful Pairs](https://www.hackerrank.com/challenges/beautiful-pairs/problem?isFullScreen=true)

## 정리

- B 배열 원소를 최대 1번이 아니라 반드시 1번 바꿔야 한다. (주의)

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
     * Complete the 'beautifulPairs' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY A
     *  2. INTEGER_ARRAY B
     */

    public static int beautifulPairs(List<Integer> A, List<Integer> B) {
        Collections.sort(A);
        Collections.sort(B);
        
        int aIdx = 0;
        int bIdx = 0;
        int aIdxNotUsedCount = 0;
        int bIdxNotUsedCount = 0;
        int pairwiseDisjointSize = 0;
        
        while (aIdx < A.size() && bIdx < B.size()) {
            if (A.get(aIdx) < B.get(bIdx)) {
                aIdx += 1;
                aIdxNotUsedCount += 1;
            } else if (A.get(aIdx) > B.get(bIdx)) {
                bIdx += 1;
                bIdxNotUsedCount += 1;
            } else {
                pairwiseDisjointSize += 1;
                aIdx += 1;
                bIdx += 1;
            }
        }
        
        aIdxNotUsedCount += A.size() - aIdx;
        bIdxNotUsedCount += B.size() - bIdx;
        
        if (aIdxNotUsedCount > 0 && bIdxNotUsedCount > 0) {
            pairwiseDisjointSize += 1;
        } else if (bIdxNotUsedCount == 0) {
            pairwiseDisjointSize -= 1;
        }
        
        return pairwiseDisjointSize;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> A = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> B = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.beautifulPairs(A, B);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}

```
