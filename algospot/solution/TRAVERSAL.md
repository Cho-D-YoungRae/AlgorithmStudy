# 트리 순회 순서 변경

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

class Node {
    int value;
    Node left;
    Node right;
}

public class Main {

    private static int[] preorder;
    private static int[] inorder;
    private static int idx;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            int n = Integer.parseInt(br.readLine());
            preorder = Arrays.stream(br.readLine().split(" "))
                    .mapToInt(Integer::parseInt)
                    .toArray();
            inorder = Arrays.stream(br.readLine().split(" "))
                    .mapToInt(Integer::parseInt)
                    .toArray();
            idx = 0;

            Node tree = createTree(0, n - 1);
            System.out.println(postorder(tree).stream()
                    .map(String::valueOf)
                    .collect(Collectors.joining(" ")));
        }
    }

    private static Node createTree(int start, int end) {
        Node parent = new Node();
        parent.value = preorder[idx];
        idx += 1;

        int inorderIdx = findInorderIdx(parent.value, start, end);

        if (inorderIdx != start) {
            parent.left = createTree(start, inorderIdx - 1);
        }
        if (inorderIdx != end) {
            parent.right = createTree(inorderIdx + 1, end);
        }

        return parent;
    }

    private static int findInorderIdx(int value, int start, int end) {
        for (int i = start; i <= end; i++) {
            if (inorder[i] == value) {
                return i;
            }
        }

        return -1;
    }

    private static List<Integer> postorder(Node node) {
        if (node == null) {
            return new ArrayList<>();
        }

        List<Integer> result = new ArrayList<>();
        result.addAll(postorder(node.left));
        result.addAll(postorder(node.right));
        result.add(node.value);

        return result;
    }
}

```
