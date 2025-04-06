#Report Title : 
Implementation of Graph Coloring in Python using Backtracking

## IMPLEMENTATION

import random
class GraphColoring:
    def __init__(self):
         self.N = 0
        self.M = 0
        self.K = 0
        self.graph = []
        self.colors = []
        self.edges = []

    def init(self):
        self.N = random.randint(4, 6)
        max_possible_edges = self.N * (self.N - 1) // 2
        self.M = random.randint(self.N, max_possible_edges)
        self.K = random.randint(2, self.N)

        self.graph = [[] for _ in range(self.N)]
edge_set = set()
        while len(edge_set) < self.M:
            u = random.randint(0, self.N - 1)
            v = random.randint(0, self.N - 1)

            if u != v and (u, v) not in edge_set and (v, u) not in edge_set:
                self.graph[u].append(v)
                self.graph[v].append(u)
                edge_set.add((u, v))
                self.edges.append((u, v))

  self.colors = [0] * self.N
   print("Generated Graph:")
        self.print_graph()
        print(f"Trying to color the graph with {self.K} colors...\n")

        if self.solve(0):
            print(f"Coloring Possible with {self.K} Colors")
            print("Color Assignment:", self.colors)
        else:
            print(f"Coloring Not Possible with {self.K} Colors")

    def print_graph(self):
        print("Number of Vertices (N):", self.N)
        print("Number of Edges (M):", self.M)
        print("Edges:")
        for u, v in self.edges:
          print(f"{u} -- {v}")

         print()

  def is_safe(self, node, color):
        for neighbor in self.graph[node]:
           if self.colors[neighbor] == color:
                return False
        return True


   def solve(self, node):
        if node == self.N:
            return True


  for clr in range(1, self.K + 1):
       


     print(f"Trying color {clr} for vertex {node}")
            if self.is_safe(node, clr):
                self.colors[node] = clr
               print(f"Assigned color {clr} to vertex {node}")
              if self.solve(node + 1):
                    return True
 print(f"Backtracking on vertex {node}, removing color {clr}")

    self.colors[node] = 0
        return False



def main():
    gc = GraphColoring()
    gc.init()
if __name__ == "__main__":

    main()

