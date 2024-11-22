# 요새

## 1

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.StringTokenizer;

class Node implements Comparable<Node> {
    final int x;
    final int y;
    final int r;
    final List<Node> children = new ArrayList<>();

    public Node(int x, int y, int r) {
        this.x = x;
        this.y = y;
        this.r = r;
    }

    @Override
    public int compareTo(Node o) {
        return - Integer.compare(this.r, o.r);
    }

    public boolean contains(Node o) {
        return ((this.x - o.x) * (this.x - o.x)) + ((this.y - o.y) * (this.y - o.y)) < this.r * this.r;
    }
}

class Result {
    final int climbCount;
    final int height;

    public Result(int climbCount, int height) {
        this.climbCount = climbCount;
        this.height = height;
    }
}

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int c = Integer.parseInt(br.readLine());

        for (int testcase = 0; testcase < c; testcase++) {
            int n = Integer.parseInt(br.readLine().trim());
            List<Node> castles = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                StringTokenizer st = new StringTokenizer(br.readLine());
                int x = Integer.parseInt(st.nextToken());
                int y = Integer.parseInt(st.nextToken());
                int r = Integer.parseInt(st.nextToken());
                castles.add(new Node(x, y, r));
            }

            Node root = createTree(castles);
            Result result = findResult(root);
            System.out.println(result.climbCount);
        }
    }

    private static Node createTree(List<Node> castles) {
        Collections.sort(castles);
        Node root = castles.get(0);
        for (int i = 1; i < castles.size(); i++) {
            addChildren(root, castles.get(i));
        }

        return root;
    }

    private static void addChildren(Node parent, Node target) {
        for (Node child : parent.children) {
            if (child.contains(target)) {
                addChildren(child, target);
                return;
            }
        }

        parent.children.add(target);
    }

    private static Result findResult(Node tree) {
        if (tree.children.isEmpty()) {
            return new Result(0, 1);
        }

        int height = 0;
        int climbCount = 0;
        List<Result> results = new ArrayList<>();
        for (Node child : tree.children) {
            Result result = findResult(child);
            height = Math.max(height, result.height + 1);
            climbCount = Math.max(climbCount, result.climbCount);
            results.add(result);
        }

        results.sort((o1, o2) -> -Integer.compare(o1.height, o2.height));
        if (results.size() >= 2) {
            climbCount = Math.max(climbCount, results.get(0).height + results.get(1).height);
        }
        climbCount = Math.max(climbCount, height - 1);

        return new Result(climbCount, height);
    }

}

```
