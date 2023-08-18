# grafos-

import networkx as nx
import matplotlib.pyplot as plt

def dijkstra(grafo, origem):
    inf = float('inf')
    
    distancias = {no: inf for no in grafo}
    predecessores = {no: 'und' for no in grafo}
    Q = {no for no in grafo}
    
    distancias[origem] = 0
    predecessores[origem] = origem

    while len(Q) > 0:
        S = inf
        for no in Q:
            if distancias[no] < S:
                S = distancias[no]
                vertice_atual = no
                
        Q.remove(vertice_atual)
        
        for vizinho, peso in grafo[vertice_atual].items():
            alt = distancias[vertice_atual] + peso
            if alt < distancias[vizinho]:
                distancias[vizinho] = alt
                predecessores[vizinho] = vertice_atual
                
    return distancias, predecessores

grafo = {'A': {'B': 1, 'C': 4},
         'B': {'A': 1, 'C': 1, 'D': 3, 'E': 5},
         'C': {'A': 4, 'B': 1, 'D': 2, 'E': 3},
         'D': {'B': 3, 'C': 2, 'E': 2},
         'E': {'B': 5, 'C': 3, 'D': 2}}
origem = 'A'
dist, prev = dijkstra(grafo, origem)
print(dist)
print("----------------------------")
print(prev)

def caminho(origem, V):
    melhor_caminho = []
    melhor_caminho.append(V)
    predecessor = prev[V]
    melhor_caminho.append(predecessor)
    while predecessor != origem:
        predecessor = prev[predecessor]
        melhor_caminho.append(predecessor)
          
    return sorted(melhor_caminho, reverse=True)
T = caminho(origem, 'E')
print("-----------------")
print(T)

G = nx.Graph(grafo)

for no in grafo:
    for vizinho, peso in grafo[no].items():
        G.add_edge(no, vizinho, weight=peso)
        
pos = nx.circular_layout(G)

nx.draw_networkx_nodes(G, pos)
nx.draw_networkx_labels(G, pos)
nx.draw_networkx_edges(G, pos)
edge_labels = nx.get_edge_attributes(G, "weight")
nx.draw_networkx_edge_labels(G, pos, edge_labels)
plt.show()


