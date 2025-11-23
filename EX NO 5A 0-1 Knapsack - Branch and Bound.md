
# EX 5A 0/1 Knapsack Problem - Branch&Bound 
## DATE: 11-11-2025
## AIM:
To Write a Java program to solve 0/1 Knapsack problem using Branch and Bound Approach.
You are heading a college entrepreneurship cell that can invest in up to N student‑startups.

For each startup i you know: cost[i]  — the amount (in ₹ lakh) required to join the showcase profit[i] — the estimated profit (in ₹ lakh) you’ll gain if it succeeds You have a total budget of B ₹ lakh. Pick a subset of startups so that the sum of costs ≤ B and the sum of profits is maximised.

Because N can be as large as 50, a plain exhaustive search (2^N) is too slow.

The recommended approach is Branch & Bound with a fractional‑knapsack upper bound (but any algorithm that meets the constraints is accepted). 

Input Format

N

B

cost[1] cost[2] … cost[N]

profit[1] profit[2] … profit[N]

1 ≤ N ≤ 50

1 ≤ B ≤ 1 000 000

1 ≤ cost[i], profit[i] ≤ 10 000 

Output Format

maxProfit





## Algorithm
1. Start  
2. Read the number of startups `N` and total available budget `B`.  
3. Read two arrays:  
   - `cost[i]` → the investment required for each startup.  
   - `profit[i]` → the expected profit from each startup.  
4. Create an array of `Item` objects where each item has:  
   - `cost`, `profit`, and  
   - `ratio = profit / cost` (profit per unit cost).  
5. Sort all items in **descending order of profit-to-cost ratio** to prioritize higher returns.  
6. Define a helper function `bound(u, N, B, items)` to compute an **upper bound** on the maximum profit from a given node:  
   - If the total cost so far exceeds the budget `B`, return `0`.  
   - Otherwise, greedily add items until the budget is filled or items are exhausted.  
   - If the last item doesn’t fit completely, add a fractional portion of it (for bounding purpose).  
7. Use a **queue (BFS)** for Branch and Bound:  
   - Initialize a dummy node at level `-1` with profit `0` and cost `0`.  
   - Add it to the queue.  
8. While the queue is not empty:  
   - Remove the front node `v`.  
   - If `v.level == N - 1`, skip further expansion.  
   - Create a new node `u` representing inclusion of the next item:  
     - Update its cost and profit.  
     - If it’s within budget and yields a better profit, update `maxProfit`.  
     - Calculate its bound; if the bound is higher than `maxProfit`, enqueue it.  
   - Create another node `u` representing exclusion of the item and calculate its bound; if valid, enqueue it.  
9. Continue until all promising nodes are processed.  
10. Print the final `maxProfit` as the maximum achievable profit under the given budget.  
11. End  


## Program:
```
/*
Developed by: YUVARAJ B
Register Number:  212222040186
*/

import java.util.*;
public class StartupInvestment 
{
    static class Item 
    {
        int cost, profit;
        double ratio;
        Item(int c, int p) 
        {
            cost = c;
            profit = p;
            ratio = (double) p / c;
        }
    }

    static class Node 
    {
        int level, profit, cost;
        double bound;
        Node(int level, int profit, int cost, double bound)
        {
            this.level = level;
            this.profit = profit;
            this.cost = cost;
            this.bound = bound;
        }
    }

    static double bound(Node u, int N, int B, Item[] items)
    {
        if (u.cost >= B) return 0;
        double profitBound = u.profit;
        int j = u.level + 1;
        int totalCost = u.cost;

        while (j < N && totalCost + items[j].cost <= B) 
        {
            totalCost += items[j].cost;
            profitBound += items[j].profit;
            j++;
        }

        if (j < N)
            profitBound += (B - totalCost) * items[j].ratio;

        return profitBound;
    }

    static int knapsack(int N, int B, Item[] items) 
    {
        Arrays.sort(items, (a, b) -> Double.compare(b.ratio, a.ratio));

        Queue<Node> q = new LinkedList<>();
        Node u, v;
        v = new Node(-1, 0, 0, 0);
        q.add(v);

        int maxProfit = 0;

        while (!q.isEmpty())
        {
            v = q.poll();

            if (v.level == N - 1) continue;

            u = new Node(v.level + 1, v.profit, v.cost, 0);

            // Include current item
            u.cost = v.cost + items[u.level].cost;
            u.profit = v.profit + items[u.level].profit;

            if (u.cost <= B && u.profit > maxProfit)
                maxProfit = u.profit;

            u.bound = bound(u, N, B, items);
            if (u.bound > maxProfit)
                q.add(u);

            // Exclude current item
            u = new Node(v.level + 1, v.profit, v.cost, 0);
            u.bound = bound(u, N, B, items);
            if (u.bound > maxProfit)
                q.add(u);
        }
        return maxProfit;
    }

    public static void main(String[] args) 
    {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt();
        int B = sc.nextInt();
        int[] cost = new int[N];
        int[] profit = new int[N];
        for (int i = 0; i < N; i++) cost[i] = sc.nextInt();
        for (int i = 0; i < N; i++) profit[i] = sc.nextInt();

        Item[] items = new Item[N];
        for (int i = 0; i < N; i++)
            items[i] = new Item(cost[i], profit[i]);

        System.out.println(knapsack(N, B, items));
        sc.close();
    }
}

```

## Output:


<img width="904" height="363" alt="image" src="https://github.com/user-attachments/assets/6321fc3d-3283-491d-a927-d2f024185d2c" />

## Result:
The program successfully solved 0/1 Knapsack problem using branch & bound and output is verified. 
