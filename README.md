# codo

# def hitting_time(graph, start_node, target_node, num_walks=1000, max_steps=100):
#     total_time = 0
#     for _ in range(num_walks):
#         current_node = start_node
#         steps = 0
#         while current_node != target_node and steps < max_steps:
#             neighbors = list(graph.neighbors(current_node))
#             if neighbors:
#                 current_node = np.random.choice(neighbors)
#             steps += 1
#         total_time += steps
#     return total_time / num_walks

# def commute_time(graph, start_node, target_node, num_walks=1000, max_steps=100):
#     hitting_time_st = hitting_time(graph, start_node, target_node, num_walks, max_steps)
#     hitting_time_ts = hitting_time(graph, target_node, start_node, num_walks, max_steps)
#     return hitting_time_st + hitting_time_ts

# def random_walk(graph, start_node, steps):
#     current_node = start_node
#     for _ in range(steps):
#         neighbors = list(graph.neighbors(current_node))
#         if neighbors:
#             current_node = np.random.choice(neighbors)
#     return current_node
def random_walk_with_memory(graph, start_node, target_node, steps, memory_strength=0.5):
    current_node = start_node
    memory = {node: 0 for node in graph.nodes()}  # Initialize memory for all nodes
    memory[start_node] = 1  # Set initial memory for start node to 1 (visited)
    for _ in range(steps):
        neighbors = list(graph.neighbors(current_node))
        if neighbors:
            # Calculate probabilities for each neighbor based on memory and uniform randomness
            probabilities = [(memory[neighbor] ** memory_strength) / len(neighbors) for neighbor in neighbors]
            probabilities_sum = sum(probabilities)
            if probabilities_sum == 0:
                # If all probabilities are zero, assign equal probabilities to all neighbors
                probabilities = [1 / len(neighbors) for _ in neighbors]
            else:
                probabilities = [prob / probabilities_sum for prob in probabilities]
            # Choose the next node based on the calculated probabilities
            current_node = np.random.choice(neighbors, p=probabilities)
            # Update memory for the current node
            memory[current_node] += 1
    print(memory)
    return current_node


# Choose starting and target nodes
start_node = 'A'
target_node = 'F'

# Perform random walks and compute hitting and commute times
for i in range(1, 6):
    end_node = random_walk_with_memory(G, start_node, target_node, i)
    print(f"Random walk of length {i} ends at node: {end_node}")

# hitting_time_result = hitting_time(G, start_node, target_node)
# print(f"Hitting time from {start_node} to {target_node}: {hitting_time_result}")

# commute_time_result = commute_time(G, start_node, target_node)
# print(f"Commute time from {start_node} to {target_node}: {commute_time_result}")
