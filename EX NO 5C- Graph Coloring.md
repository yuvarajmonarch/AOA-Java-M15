
# EX 5C Graph coloring
## DATE: 11-11-2025
## AIM:
To write a Java program to for given constraints.
Problem Description:
In a hilly region, several radio towers are installed to provide communication services. However, due to signal interference, two adjacent towers (i.e., in communication range of each other) must not use the same frequency channel.

You are given N radio towers and their communication ranges represented as an undirected graph. Your task is to assign channels (colors) to these towers using at most M channels such that no two adjacent towers use the same channel.

Write a program to determine if such an assignment is possible or not.

Input Format:
First line contains two integers: N (number of towers), and M (number of available frequency channels).

Next line contains an integer E — number of edges representing the communication range.

Next E lines contain two integers u and v — representing that tower u and tower v are within range (0-based index).

Output Format:
Print "YES" if it's possible to assign frequencies to towers such that no two adjacent towers have the same frequency.

Otherwise, print "NO".

<img width="182" height="440" alt="image" src="https://github.com/user-attachments/assets/b32078a2-c79d-4a25-88c4-e51144b5456f" />


## Algorithm
1. Start  
2. Read the input values:  
   - `n` → number of radio towers (nodes in the graph).  
   - `m` → number of available channels (colors).  
   - `e` → number of connections (edges) between towers.  
3. Create an adjacency list `graph` of size `n` to represent the connections between towers.  
4. For each of the `e` edges, read two integers `u` and `v` and add the connection to both towers (since the graph is undirected):  
   - `graph[u].add(v)`  
   - `graph[v].add(u)`  
5. Initialize an array `color[n]` to store the assigned channel for each tower (0 indicates unassigned).  
6. Define a helper function `isSafe(node, graph, color, col)` that checks if assigning color `col` to tower `node` is valid:  
   - Iterate over all neighbors of `node`.  
   - If any neighbor already has the same color `col`, return `false`.  
   - Otherwise, return `true`.  
7. Define the recursive function `isColorable(graph, color, node, m, n)` to assign colors to towers:  
   - **Base case:** If all towers are assigned a color (`node == n`), return `true`.  
   - For the current tower, try assigning each color from `1` to `m`:  
     - If the color is safe (`isSafe` returns `true`), assign it and recursively call `isColorable` for the next tower.  
     - If recursion returns `true`, a valid coloring exists — return `true`.  
     - Otherwise, backtrack by resetting `color[node] = 0`.  
   - If no color can be assigned to the current tower, return `false`.  
8. In the main function:  
   - Call `isColorable(graph, color, 0, m, n)`.  
   - If it returns `true`, print `"YES"` (a valid channel assignment exists).  
   - Otherwise, print `"NO"`.  
9. End  


## Program:
```
/*
Developed by: YUVARAJ B
Register Number:  212222040186 
*/


import java.util.*;
public class RadioTowerChannelAssignment {

    // Helper function to check if a color can be assigned to a node
    private static boolean isSafe(int node, List<List<Integer>> graph, int[] color, int col) {
        for (int neighbor : graph.get(node)) {
            if (color[neighbor] == col)  // adjacent tower already uses this color
                return false;
        }
        return true;
    }

    // Recursive function to try assigning colors to each tower
    public static boolean isColorable(List<List<Integer>> graph, int[] color, int node, int m, int n) {
        // Base case: all towers are colored successfully
        if (node == n)
            return true;

        // Try all colors (1 to m)
        for (int col = 1; col <= m; col++) {
            if (isSafe(node, graph, color, col)) {
                color[node] = col; // assign color

                // Recur for next tower
                if (isColorable(graph, color, node + 1, m, n))
                    return true;

                // Backtrack
                color[node] = 0;
            }
        }

        // If no color can be assigned, return false
        return false;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(); // number of towers
        int m = sc.nextInt(); // number of available channels
        int e = sc.nextInt(); // number of edges (connections)

        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++)
            graph.add(new ArrayList<>());

        for (int i = 0; i < e; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            graph.get(u).add(v);
            graph.get(v).add(u); // undirected graph
        }

        int[] color = new int[n]; // color assignment for towers

        if (isColorable(graph, color, 0, m, n))
            System.out.println("YES");
        else
            System.out.println("NO");

        sc.close();
    }
}

```

## Output:

<img width="776" height="620" alt="image" src="https://github.com/user-attachments/assets/5a0785e8-34aa-4cd1-9b13-0c4beda66a99" />


## Result:
The program successfully implemented and the expected output is verified.
