
# EX 5B Topological Sort - Khan's Algorithm
## DATE: 11-11-2025
## AIM:
To write a Java program to for given constraints.
Problem Description:
A software development team is preparing for a product release. The release consists of multiple tasks, each dependent on other tasks being completed first. You are to determine a valid order in which all tasks can be completed. If it's not possible due to cyclic dependencies, output that the release cannot be scheduled.

Each task is labeled from 0 to n-1. The dependencies are provided in the form of pairs [a, b] which means task a depends on task b.

Implement a program to find a valid task execution order using topological sort.

Input Format:

An integer n — number of tasks.

An integer m — number of dependencies.

m lines follow with two integers a and b — representing a depends on b.

Output Format:

If a valid order exists, print the task numbers in a possible execution order (space-separated).

If not, print "Release cannot be scheduled".

<img width="341" height="363" alt="image" src="https://github.com/user-attachments/assets/f0355541-4f66-49da-bcd3-171a799a7c1f" />

## Algorithm
1. Start  
2. Read the number of tasks `n` and the number of dependencies `m`.  
3. Input all dependency pairs into a 2D array `dependencies[m][2]`,  
   where each pair `(a, b)` means **task `a` depends on task `b`** (i.e., `b → a`).  
4. Create an adjacency list `adj[]` of size `n` to represent the dependency graph.  
5. Initialize an array `indegree[n]` to keep track of the number of prerequisites for each task.  
6. For each dependency `(a, b)`:  
   - Add `a` to the adjacency list of `b` (`adj[b].add(a)`).  
   - Increment `indegree[a]` by 1.  
7. Create a queue `queue` to store tasks with no dependencies (`indegree[i] == 0`).  
8. Add all tasks with `indegree = 0` to the queue (these can be executed first).  
9. Initialize an empty list `order` to store the valid execution order of tasks.  
10. While the queue is not empty:  
    - Dequeue a task `task` and add it to `order`.  
    - For each neighbor (dependent task) of `task`:  
      - Decrement its indegree (`indegree[neighbor]--`).  
      - If `indegree[neighbor] == 0`, enqueue that neighbor.  
11. After processing all tasks:  
    - If the size of `order` equals `n`, print the order (a valid release schedule exists).  
    - Otherwise, print **“Release cannot be scheduled”** (a cycle exists — circular dependency).  
12. End  
 

## Program:
```
/*
Developed by: YUVARAJ B
Register Number:  212222040186 
*/


import java.util.*;
public class prog {

    public static List<Integer> findTaskOrder(int n, int[][] dependencies) {
        // Create adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }

        // Create in-degree array
        int[] indegree = new int[n];

        // Build graph
        for (int[] dep : dependencies) {
            int a = dep[0];
            int b = dep[1];
            adj.get(b).add(a); // b → a (a depends on b)
            indegree[a]++;
        }

        // Queue for tasks with no dependencies
        Queue<Integer> queue = new LinkedList<>();

        // Add all tasks with indegree 0
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) {
                queue.add(i);
            }
        }

        List<Integer> order = new ArrayList<>();

        // Process the graph
        while (!queue.isEmpty()) {
            int task = queue.poll();
            order.add(task);

            for (int neighbor : adj.get(task)) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.add(neighbor);
                }
            }
        }

        // If not all tasks are processed, a cycle exists
        if (order.size() != n) {
            return null; // cyclic dependency
        }

        return order;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(); // number of tasks
        int m = sc.nextInt(); // number of dependencies

        int[][] dependencies = new int[m][2];
        for (int i = 0; i < m; i++) {
            dependencies[i][0] = sc.nextInt(); // a
            dependencies[i][1] = sc.nextInt(); // b
        }

        List<Integer> result = findTaskOrder(n, dependencies);

        if (result == null) {
            System.out.println("Release cannot be scheduled");
        } else {
            for (int task : result) {
                System.out.print(task + " ");
            }
        }

        sc.close();
    }
}

```

## Output:

<img width="996" height="558" alt="image" src="https://github.com/user-attachments/assets/0bc6cd28-be32-4dc1-9512-3aa38b8362b0" />


## Result:
The program successfully implemented and the expected output is verified.
