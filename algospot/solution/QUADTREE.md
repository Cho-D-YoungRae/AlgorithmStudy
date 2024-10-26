# 쿼드 트리 뒤집기

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(bf.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            String quadtreeImage = bf.readLine();
            System.out.println(solution(quadtreeImage));
        }
    }

    public static String solution(String quadtreeImage) {
        return reverseQuadtreeImage(quadtreeImage, 0);
    }

    private static String reverseQuadtreeImage(String quadtreeImage, int idx) {
        if (quadtreeImage.charAt(idx) != 'x') {
            return quadtreeImage.substring(idx, idx + 1);
        }

        idx += 1;
        String leftUpper = reverseQuadtreeImage(quadtreeImage, idx);
        idx += leftUpper.length();
        String rightUpper = reverseQuadtreeImage(quadtreeImage, idx);
        idx += rightUpper.length();
        String leftLower = reverseQuadtreeImage(quadtreeImage, idx);
        idx += leftLower.length();
        String rightLower = reverseQuadtreeImage(quadtreeImage, idx);

        return "x" + leftLower + rightLower + leftUpper + rightUpper;
    }
}
```
