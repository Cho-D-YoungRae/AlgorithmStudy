# Non-Divisible Subset

## 1

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
     * Complete the 'nonDivisibleSubset' function below.
     * 
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER k
     *  2. INTEGER_ARRAY s
     */

    public static int nonDivisibleSubset(int k, List<Integer> s) {
    // Write your code here
        Map<Integer, Integer> modToCount = new HashMap<>();
        for (int num : s) {
            int mod = num % k;
            modToCount.put(mod, modToCount.getOrDefault(mod, 0) + 1);
        }
        
        int result = 0;
        for (int mod = 0; mod <= k / 2; mod++) {
            if ((mod * 2) % k == 0 && modToCount.containsKey(mod)) {
                result += 1;
            } else {
                int otherMod = k - mod;
                int modCount = modToCount.getOrDefault(mod, 0);
                int otherModCount = modToCount.getOrDefault(otherMod, 0);
                result += modCount > otherModCount ? modCount : otherModCount;
            }
        }
        
        return result;
    }

}
````
