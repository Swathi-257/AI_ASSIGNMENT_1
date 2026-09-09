# AI_ASSIGNMENT_1
# 1. A* PYTHON CODE 
import heapq

grid = [
    [0, 0, 0, 0, 0],
    [0, 1, 1, 1, 0],
    [0, 0, 0, 1, 0],
    [0, 1, 0, 0, 0],
    [0, 0, 0, 0, 0]
]

start = (0, 0)
goal = (4, 4)


def heuristic(a, b):
    return abs(a[0] - b[0]) + abs(a[1] - b[1])


def astar():
    open_list = []
    heapq.heappush(open_list, (0, start))

    cost = {start: 0}
    parent = {start: None}

    while open_list:
        _, current = heapq.heappop(open_list)

        if current == goal:
            path = []

            while current:
                path.append(current)
                current = parent[current]

            return path[::-1]

        for dx, dy in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            neighbor = (current[0] + dx, current[1] + dy)

            if (0 <= neighbor[0] < 5 and
                0 <= neighbor[1] < 5 and
                grid[neighbor[0]][neighbor[1]] == 0):

                new_cost = cost[current] + 1

                if neighbor not in cost or new_cost < cost[neighbor]:
                    cost[neighbor] = new_cost

                    priority = new_cost + heuristic(neighbor, goal)

                    heapq.heappush(
                        open_list,
                        (priority, neighbor)
                    )

                    parent[neighbor] = current

    return None


path = astar()

print("Game Character Path:")
print(path)
# 2.	Uniform Cost Search
import heapq

graph = {
    "Chennai": [("Delhi", 5000), ("Mumbai", 4000)],
    "Delhi": [("Kolkata", 3000), ("Mumbai", 2500)],
    "Mumbai": [("Delhi", 2500), ("Bangalore", 2000)],
    "Kolkata": [("Bangalore", 3500)],
    "Bangalore": []
}


def uniform_cost_search(start, goal):
    queue = [(0, start, [start])]
    visited = set()

    while queue:
        cost, city, path = heapq.heappop(queue)

        if city in visited:
            continue

        visited.add(city)

        if city == goal:
            return cost, path

        for next_city, flight_cost in graph.get(city, []):
            if next_city not in visited:
                heapq.heappush(
                    queue,
                    (
                        cost + flight_cost,
                        next_city,
                        path + [next_city]
                    )
                )

    return None


cost, path = uniform_cost_search("Chennai", "Bangalore")

print("Best Flight Route:")
print(" -> ".join(path))
print("Total Cost:", cost)
