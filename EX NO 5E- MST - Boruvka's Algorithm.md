
# EX 5E Minimum Spanning Tree -Boruvka's Algorithm
## DATE: 11-11-2025
## AIM:
To write a Java program to for given constraints.
Boruvka's Algorithm - Minimum Spanning Tree

Find the MST using Boruvka's Algorithm for a weighted undirected graph.


<img width="292" height="235" alt="image" src="https://github.com/user-attachments/assets/06246b27-37a9-40a8-bd7a-37a1d5187cd1" />

## Algorithm
1. Start  
2. Read the number of vertices `V` and edges `E` of the graph.  
3. Create a list of all edges, where each edge contains:  
   - `src` → source vertex  
   - `dest` → destination vertex  
   - `weight` → edge weight  
4. Initialize a **Disjoint Set Union (DSU)** structure to track connected components:  
   - Set `parent[i] = i` for all vertices.  
5. Initialize variables:  
   - `numTrees = V` (each vertex is its own tree initially).  
   - `mstWeight = 0` (total weight of MST).  
6. Repeat the following steps until only one tree remains (`numTrees > 1`):  
   - Create an array `cheapest[V]` to store the cheapest edge for each component.  
   - For every edge `(u, v)` in the graph:  
     - Find the root of both vertices using the `find()` function.  
     - If they belong to different components:  
       - Update `cheapest[set1]` if the edge is cheaper than the current one.  
       - Update `cheapest[set2]` similarly.  
   - After scanning all edges:  
     - For each vertex `i`:  
       - Retrieve the edge `e = cheapest[i]`.  
       - If `e` connects two different components (`find(e.src) != find(e.dest)`):  
         - Add `e` to the MST.  
         - Merge the components using `union(set1, set2)`.  
         - Add the edge’s weight to `mstWeight`.  
         - Decrement `numTrees` by 1.  
7. Once only one component remains, all MST edges are selected.  
8. Print each edge included in the MST and display the total weight `mstWeight`.  
9. End  
   

## Program:
```
/*
Developed by: YUVARAJ B
Register Number:  212222040186 
*/


import java.util.*;

public class BoruvkaMST {
    static int[] parent;

    static int find(int i) {
        if (parent[i] != i)
            parent[i] = find(parent[i]);
        return parent[i];
    }

    static void union(int x, int y) {
        int set1 = find(x);
        int set2 = find(y);
        if (set1 != set2)
            parent[set1] = set2;
    }

    static int boruvkaMST(int V, List<Edge> edges) {
        parent = new int[V];
        for (int i = 0; i < V; i++)
            parent[i] = i;

        int numTrees = V;
        int mstWeight = 0;

        List<Edge> mstEdges = new ArrayList<>();

        // Continue until we have only one tree
        while (numTrees > 1) {
            Edge[] cheapest = new Edge[V];

            // Find cheapest edge for each component
            for (Edge e : edges) {
                int set1 = find(e.src);
                int set2 = find(e.dest);

                if (set1 == set2)
                    continue;

                if (cheapest[set1] == null || cheapest[set1].weight > e.weight)
                    cheapest[set1] = e;

                if (cheapest[set2] == null || cheapest[set2].weight > e.weight)
                    cheapest[set2] = e;
            }

            // Add the selected cheapest edges to MST
            for (int i = 0; i < V; i++) {
                Edge e = cheapest[i];
                if (e != null) {
                    int set1 = find(e.src);
                    int set2 = find(e.dest);

                    if (set1 == set2)
                        continue;

                    union(set1, set2);
                    System.out.println("Edge: " + e.src + "-" + e.dest + " Weight: " + e.weight);
                    mstWeight += e.weight;
                    mstEdges.add(e);
                    numTrees--;
                }
            }
        }

        return mstWeight;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int V = sc.nextInt(); // Number of vertices
        int E = sc.nextInt(); // Number of edges

        List<Edge> edges = new ArrayList<>();
        for (int i = 0; i < E; i++) {
            edges.add(new Edge(sc.nextInt(), sc.nextInt(), sc.nextInt()));
        }

        int totalWeight = boruvkaMST(V, edges);
        System.out.println("Total Weight of MST: " + totalWeight);

        sc.close();
    }
}

class Edge {
    int src, dest, weight;
    Edge(int s, int d, int w) {
        src = s; dest = d; weight = w;
    }
}

```

## Output:

<img width="787" height="549" alt="image" src="https://github.com/user-attachments/assets/f2a13716-d913-4dd3-a5e1-9547f93ff9d3" />


## Result:
The program successfully implemented and the expected output is verified.
