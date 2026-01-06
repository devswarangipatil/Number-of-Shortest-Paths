# Number-of-Shortest-Paths

You are given a city with n intersections (numbered 0 to n - 1) connected by bidirectional roads. The city is fully connected, and there is at most one road between any two intersections.

You are also given a 2D array roads, where roads[i] = [u, v, time] represents a road between intersections u and v that takes time minutes to travel.

Your task is to compute the number of different ways to travel from intersection 0 to intersection n - 1 in the shortest possible time.

Input
The first line contains an integer n — the number of intersections.
The second line contains an integer m — the number of roads.
The next m lines each contain 3 space-separated integers — the values of the 2D array roads.

Output
Print the number of ways you can arrive at your destination in the shortest amount of time. Since the answer may be large, return it modulo 109 + 7.

Constraints
1≤n≤10 
n−1≤m≤2
n×(n−1)
roads[i].length=3  
1≤time ≤10 

import heapq

MOD = 10**9 + 7

n = int(input())
m = int(input())

adj = [[] for _ in range(n)]
for _ in range(m):
    u, v, t = map(int, input().split())
    adj[u].append((v, t))
    adj[v].append((u, t))

dist = [float('inf')] * n
ways = [0] * n

dist[0] = 0
ways[0] = 1

pq = [(0, 0)]  # (distance, node)

while pq:
    d, u = heapq.heappop(pq)

    if d > dist[u]:
        continue

    for v, time in adj[u]:
        nd = d + time

        if nd < dist[v]:
            dist[v] = nd
            ways[v] = ways[u]
            heapq.heappush(pq, (nd, v))

        elif nd == dist[v]:
            ways[v] = (ways[v] + ways[u]) % MOD

print(ways[n - 1] % MOD)
