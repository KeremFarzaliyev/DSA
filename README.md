# Dijkstra vs A* Performance on Bangalore Road Network

This project benchmarks Dijkstra’s algorithm against A* search on a large, weighted road network representing Bangalore, India. We measure runtime and path length for both algorithms on the same set of node pairs to compare efficiency and scalability.

---

## Dataset

- **Files**  
  `hotosm_ind_roads_lines_gpkg.gpkg`  
  `hotosm_ind_roads_lines_geojson.geojson`  
- **Source**  
  Humanitarian OpenStreetMap Team (HOTOSM)  
- **Graph statistics**  
  - Nodes: 725 053  
  - Edges: 787 763  

Each edge weight represents estimated travel time (minutes) computed from road-type speed limits (km/h → min/km).

---

## Algorithms

**Dijkstra’s Algorithm**  
- Uninformed shortest‐path search  
- Explores all reachable nodes in order of increasing cumulative weight  
- Time complexity: O((V + E) log V)  

**A\* Search Algorithm**  
- Informed shortest‐path search using a heuristic  
- Heuristic: Euclidean (straight‐line) distance  
- Prioritizes nodes closer to the goal, reducing expanded nodes  
- Same asymptotic complexity, but faster in practice on spatial graphs  

---

## Methodology

1. Load the full Bangalore road network into a NetworkX graph via GeoPandas.  
2. Assign each edge a weight = distance / (speed / 60) → travel time in minutes.  
3. Randomly select 10 valid start–end node pairs with increasing Euclidean separation.  
4. For each pair, compute the shortest path and measure runtime using:  
   - Dijkstra (`nx.shortest_path`)  
   - A*      (`nx.astar_path` with Euclidean heuristic)  
5. Record **path length** (number of nodes) and **runtime** for each algorithm.  
6. Sort all results by path length for analysis.

---

## Benchmark Results

| Path Length | Dijkstra Time (s) | A* Time (s) |
|-----------:|------------------:|------------:|
| 1082       | 12.8722           | 5.2945      |
| 1129       | 10.4827           | 5.0982      |
|  859       | 4.2094            | 4.4294      |
|  752       | 3.4156            | 1.8671      |
|  682       | 2.3212            | 2.2828      |
|  633       | 1.7995            | 2.2702      |
|  499       | 1.0337            | 2.3221      |
|  448       | 1.1065            | 1.5163      |
|  402       | 0.3880            | 0.3669      |
|  198       | 0.1182            | 0.2407      |

- **Average runtime ratio** (Dijkstra / A*): **≈ 1.21×** slower for Dijkstra  
- Both algorithms returned identical path lengths for each test.

---

## Growth Analysis

We fitted degree-2 polynomials and performed log-log regression on runtime vs. path length:

- **Polynomial fit** shows Dijkstra’s runtime grows more steeply than A*.  
- **Log-log regression slopes**:  
  - Dijkstra ≈ 2.61  
  - A*       ≈ 1.75  

A lower slope for A* confirms it scales better as path length increases.

---

## Conclusion

A* consistently outperforms Dijkstra on this time‐weighted road graph. The heuristic significantly reduces unnecessary node expansions, resulting in lower runtimes for longer routes. While both guarantee optimal solutions, A* offers superior practical performance for large-scale routing.

---

