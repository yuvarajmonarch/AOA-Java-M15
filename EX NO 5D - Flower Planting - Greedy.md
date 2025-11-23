# EX 5D Flower Planting.
## DATE: 11-11-2025
## AIM:
To write a Java program to for given constraints.
You are given n gardens, labelled from 1 to n.

You also have a list called paths, where each element paths[i] = [xi, yi] represents a bidirectional road connectingthe  garden xi and garden yi.

You want to plant one flower in each garden, and there are exactly 4 types of flowers labelled as 1, 2, 3, and 4.

Your goal is to plant flowers such that:

No two connected gardens (i.e., connected via a path) have the same flower type.

Return any valid flower assignment as an array where:

answer[i] is the flower type planted in the (i+1) ᵗʰ garden

It is guaranteed that:

No garden is connected to more than 3 other gardens

A valid flower assignment always exists

<img width="177" height="292" alt="image" src="https://github.com/user-attachments/assets/36aa40cb-1cdd-4746-b1a6-fc51ce6e96aa" />

## Algorithm
1. Start  
2. Read the integer `n` → number of gardens (nodes in the graph).  
3. Read the integer `m` → number of paths between gardens (edges in the graph).  
4. Create an adjacency list `adj[]` of size `n` to represent garden connections.  
5. For each path `(u, v)` in the input:  
   - Add `(v - 1)` to `adj[u - 1]` and `(u - 1)` to `adj[v - 1]` (since garden indices are 1-based in input and the graph is undirected).  
6. Initialize an array `res[n]` to store the flower type assigned to each garden.  
7. For each garden `i` from `0` to `n - 1`:  
   - Create a boolean array `used[5]` to track which flower types (1 to 4) are already used by neighboring gardens.  
   - For each neighbor `nei` in `adj[i]`:  
     - If a neighbor already has a flower type assigned (`res[nei] != 0`), mark it as used (`used[res[nei]] = true`).  
   - Assign the first unused flower type (from 1 to 4) to garden `i`.  
8. After processing all gardens, print the assigned flower types for each garden in order.  
9. End  
 

## Program:
```
/*
Developed by: YUVARAJ B
Register Number:  212222040186 
*/


import java.util.*;

public class GardenFlowerPlanner {

    public static int[] assignFlowers(int n, int[][] paths) {
        @SuppressWarnings("unchecked")
        List<Integer>[] adj = new ArrayList[n];
        for (int i = 0; i < n; i++) adj[i] = new ArrayList<>();

        for (int[] p : paths) {
            adj[p[0] - 1].add(p[1] - 1);
            adj[p[1] - 1].add(p[0] - 1);
        }

        int[] res = new int[n];

        for (int i = 0; i < n; i++) {
            boolean[] used = new boolean[5];
            for (int nei : adj[i]) {
                if (res[nei] != 0) used[res[nei]] = true;
            }
            for (int f = 1; f <= 4; f++) {
                if (!used[f]) {
                    res[i] = f;
                    break;
                }
            }
        }

        return res;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt(); 
        int m = sc.nextInt(); 

        int[][] paths = new int[m][2];
        for (int i = 0; i < m; i++) {
            paths[i][0] = sc.nextInt();
            paths[i][1] = sc.nextInt();
        }

        int[] result = assignFlowers(n, paths);
        for (int flower : result) {
            System.out.print(flower + " ");
        }
        System.out.println();
        sc.close();
    }
}

```

## Output:

<img width="780" height="590" alt="image" src="https://github.com/user-attachments/assets/44cc643c-bc09-47a2-86f1-ee10bc05514e" />


## Result:
The program successfully implemented and the expected output is verified.
