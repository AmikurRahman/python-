# python-
    import random

class Node:
    def __init__(self, a, b, z):
        self.x = a
        self.y = b
        self.depth = z

class DFS:
    def __init__(self):
        self.directions = 4
        self.x_move = [1, -1, 0, 0]
        self.y_move = [0, 0, 1, -1]
        self.found = False
        self.N = 0
        self.source = None
        self.goal = None
        self.goal_level = 999999
        self.traversal_order = []
        self.path = []

    def init(self):
        self.N = random.randint(4, 7)
        graph = [[1 for _ in range(self.N)] for _ in range(self.N)]
        
        for i in range(self.N):
            for j in range(self.N):
                if random.random() < 0.3:
                    graph[i][j] = 0

        source_x, source_y = random.randint(0, self.N-1), random.randint(0, self.N-1)
        goal_x, goal_y = random.randint(0, self.N-1), random.randint(0, self.N-1)
        while graph[source_x][source_y] == 0:
            source_x, source_y = random.randint(0, self.N-1), random.randint(0, self.N-1)
        while graph[goal_x][goal_y] == 0 or (source_x == goal_x and source_y == goal_y):
            goal_x, goal_y = random.randint(0, self.N-1), random.randint(0, self.N-1)

        self.source = Node(source_x, source_y, 0)
        self.goal = Node(goal_x, goal_y, self.goal_level)

        print("Generated Grid:")
        self.print_grid(graph)
        
        self.st_dfs(graph, self.source)

        if self.found:
            print("\nGoal found")
            print("Number of moves required =", self.goal.depth)
            print("DFS Path:", self.path)
        else:
            print("Goal cannot be reached from the starting block")

        print("\nTraversal Order (Topological Order):")
        print(self.traversal_order)

    def print_grid(self, graph):
        for i in range(self.N):
            row = ""
            for j in range(self.N):
                if (i, j) == (self.source.x, self.source.y):
                    row += "S "
                elif (i, j) == (self.goal.x, self.goal.y):
                    row += "G "
                else:
                    row += str(graph[i][j]) + " "
            print(row)

    def print_direction(self, m, x, y):
        if m == 0:
            print("Moving Down ({}, {})".format(x, y))
        elif m == 1:
            print("Moving Up ({}, {})".format(x, y))
        elif m == 2:
            print("Moving Right ({}, {})".format(x, y))
        else:
            print("Moving Left ({}, {})".format(x, y))

    def st_dfs(self, graph, u):
        graph[u.x][u.y] = 2
        self.traversal_order.append((u.x, u.y))
        
        if u.x == self.goal.x and u.y == self.goal.y:
            self.found = True
            self.goal.depth = u.depth
            self.path.append((u.x, u.y))
            return True

        for j in range(self.directions):
            v_x = u.x + self.x_move[j]
            v_y = u.y + self.y_move[j]
            if 0 <= v_x < self.N and 0 <= v_y < self.N and graph[v_x][v_y] == 1:
                self.print_direction(j, v_x, v_y)
                child = Node(v_x, v_y, u.depth + 1)
                if self.st_dfs(graph, child):
                    self.path.append((u.x, u.y))
                    return True
        return False

def main():
    d = DFS()
    d.init()

if __name__ == "__main__":
    main()
