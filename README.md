# Delivery Route Finding in Hanoi with BFS, GBFS and A*

Midterm project for **Introduction to Artificial Intelligence**, Faculty of Data Science and Artificial Intelligence, National Economics University (NEU). Group 1.

We compare three search algorithms on one question: **when routing a delivery from a depot to a customer in inner Hanoi, which algorithm best balances path quality (total km) and search cost (expanded nodes)?**

- **BFS**: Breadth-First Search (fewest legs)
- **GBFS**: Greedy Best-First Search (straight-line heuristic only)
- **A\***: f(n) = g(n) + h(n) with the Euclidean heuristic
- **UCS** (Dijkstra) is used as the ground truth for the shortest path

## Map and data

- **Nodes.** The centroids of Hanoi's 12 urban districts. Boundaries come from [dvhcvn](https://github.com/daohoangson/dvhcvn) (before 1 July 2025) and are cross-checked with [geoBoundaries](https://github.com/wmgeolab/geoBoundaries). Coordinates are projected to UTM zone 48N.
- **Edges.** 24 edges, one for each pair of adjacent districts. Two Red River pairs with no bridge (Ba Đình–Long Biên, Tây Hồ–Long Biên) are removed.
- **Weights.** Real road lengths looked up on Google Maps (`real_road_data.csv`). Any edge measured shorter than the straight-line distance is raised to it, so the heuristic stays consistent.

## Experiments

| Scenario | Setup | Runs |
| --- | --- | --- |
| KB1 | Depot in Ba Đình, 4 customers | 16 |
| KB2 | Depot in every district, all 132 depot–customer pairs | 528 |
| KB3 | Simulated road tortuosity, k ∈ {1, 1.25, 1.5, 2} | 32,208 |
| KB4 | Random Delaunay maps of 25 to 800 nodes | 7,200 |

All 39,952 runs are verified automatically against UCS or exhaustive search.

## Key results

KB2 (12-district map, real road lengths, 132 pairs):

| Algorithm | Shortest path found | Mean expanded nodes |
| --- | --- | --- |
| BFS | 84.8% | 6.00 |
| GBFS | 81.8% | 1.90 |
| **A\*** | **100%** | **2.87** |
| UCS | 100% | 6.00 |

On 800-node maps, A\* expands 86.6 nodes on average against 412.5 for UCS and stays optimal. GBFS expands only 13.4 nodes but finds the shortest path in just 5% of cases.

**Conclusion:** A\* is the best default for delivery routing. It always returns the shortest path at a low search cost.

## Repository structure

```
├── finding_route.ipynb          # all code: data, algorithms, verification, KB1–KB4, step-by-step traces
├── real_road_data.csv           # measured road lengths of the 24 edges (must sit next to the notebook)
├── requirements.txt
├── ket_qua/                     # generated outputs: map, CSV results, figures
└── report/                      # report PDF and LaTeX source
```

## How to run

```bash
pip install -r requirements.txt
jupyter notebook finding_route.ipynb
```

Then choose **Run All**. On first run the notebook downloads the boundary data from GitHub, so it needs an internet connection. A full run takes about a minute, and the results are reproducible because every random scenario uses a fixed seed. The notebook's explanatory text is in Vietnamese; the algorithm code and the figures are in English.

## Team (Group 1)

| Name | Student ID |
| --- | --- |
| Đặng Khánh Linh | 11254245 |
| Hồ Đức Hải | 11252779 |
| Ngô Anh Tú | 11258594 |
| Khuất Lương Huyền Anh | 11250339 |
| Hà Tuấn Khôi | 31251021126 |

Instructor: Dr. Trong-Nghia Nguyen

## Data credits

- District boundaries: [dvhcvn](https://github.com/daohoangson/dvhcvn) by H.-S. Dao (GPL-3.0)
- Cross-check: [geoBoundaries](https://www.geoboundaries.org/) (Runfola et al., 2020)
