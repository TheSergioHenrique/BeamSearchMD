# Código de Beam-Search-Estrutura de Dados 2024.1

Este arquivo .md é de autoria de **Sérgio Henrique de Andrade Lima Filho**. Nele, vou ensinar passo a passo o funcionamento de um código de Beam-Search, que visa a realização de um seminário para a composição da nota de **2EE**.


# Pré-Requisito

Rodar no seu bash ```pip install networkx matplotlib``` para instalar tanto a biblioteca **Matplotlib para plotar gráficos** e a biblioteca **Networkx para visualizar a estrutura de busca em forma de árvore.**

## Criando arquivo e visualizando o código

```python
import  networkx  as  nx

import  matplotlib.pyplot  as  plt

  

# Estrutura do grafo expandida

graph  = {

'A': {'B': 2, 'C': 1, 'J': 3},

'B': {'D': 3, 'E': 1},

'C': {'F': 4, 'G': 2},

'D': {'K': 2, 'L': 1},

'E': {'H': 1, 'M': 2},

'F': {'N': 3},

'G': {'I': 5, 'O': 1},

'H': {'P': 2},

'I': {'Q': 1},

'J': {'R': 4, 'S': 2},

'K': {},

'L': {},

'M': {},

'N': {},

'O': {},

'P': {},

'Q': {},

'R': {},

'S': {}

}

  

# Heurística que estima o custo de cada nó ao objetivo

def  heuristic(node):

heuristic_values  = {

'A': 3, 'B': 2, 'C': 2, 'D': 5, 'E': 1, 'F': 4, 'G': 3,

'H': 0, 'I': 4, 'J': 6, 'K': 2, 'L': 4, 'M': 1, 'N': 7,

'O': 3, 'P': 5, 'Q': 1, 'R': 8, 'S': 4

}

return  heuristic_values.get(node, 0)

  

# Implementação do Beam-Search

def  beam_search(graph, start, goal, beam_width):

beam  = [(start, [start])]

while  beam:

next_candidates  = []

for  node, path  in  beam:

if  node  ==  goal:

return  path

neighbors  =  graph[node]

for  neighbor, cost  in  sorted(neighbors.items(), key=lambda  x: heuristic(x[0])):

new_path  =  path  + [neighbor]

next_candidates.append((neighbor, new_path))

beam  =  sorted(next_candidates, key=lambda  x: heuristic(x[0]))[:beam_width]

return  None

  

# Função para calcular o layout hierárquico

def  hierarchical_layout(graph):

pos  = {}

level  =  0

queue  = [(list(graph.keys())[0], level)]

visited  =  set()

while  queue:

node, level  =  queue.pop(0)

if  node  not  in  visited:

if  level  not  in  pos:

pos[level] = []

pos[level].append(node)

visited.add(node)

for  neighbor  in  graph[node]:

queue.append((neighbor, level  +  1))

pos_hierarchy  = {}

max_width  =  max(len(nodes) for  nodes  in  pos.values())

for  level, nodes  in  pos.items():

width  =  len(nodes)

for  i, node  in  enumerate(nodes):

pos_hierarchy[node] = ((i  -  width  /  2) /  max_width, -level)

return  pos_hierarchy

  

# Função para visualizar a árvore de busca

def  visualize_search_tree(graph, path):

G  =  nx.DiGraph()

for  node  in  graph:

for  neighbor, weight  in  graph[node].items():

G.add_edge(node, neighbor, weight=weight)

pos  =  hierarchical_layout(graph) # Usando layout hierárquico

plt.figure(figsize=(14, 12))

nx.draw(G, pos, with_labels=True, node_size=2500, node_color='lightgreen', font_size=12, font_weight='bold', arrows=True)

edge_labels  =  nx.get_edge_attributes(G, 'weight')

nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels, font_color='blue', font_size=12)

# Destacando o caminho encontrado

if  path:

path_edges  = [(path[i], path[i  +  1]) for  i  in  range(len(path) -  1)]

nx.draw_networkx_edges(G, pos, edgelist=path_edges, edge_color='red', width=3)

nx.draw_networkx_nodes(G, pos, nodelist=path, node_color='red')

plt.title('Árvore de Busca com Beam-Search', fontsize=15)

plt.show()

  

# Configurações iniciais

start_node  =  'A'

goal_node  =  'P'

beam_width  =  2

  

# Execução do Beam-Search

result_path  =  beam_search(graph, start_node, goal_node, beam_width)

print(f"Caminho encontrado: {result_path}")

  

# Visualização da árvore de busca

visualize_search_tree(graph, result_path)

```
