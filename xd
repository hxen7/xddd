import random
import numpy as np
import networkx as nx
import matplotlib.pyplot as plt
from math import cos, sin, pi, sqrt

# Usando SystemRandom para garantir a qualidade criptográfica na geração dos números aleatórios
sys_rand = random.SystemRandom()

def generate_random_point_in_disk():
    """
    Gera um ponto aleatório no disco unitário (modelo de Poincaré) usando coordenadas polares.
    O raio é obtido pela raiz quadrada de um número aleatório para garantir distribuição uniforme.
    """
    r = sqrt(sys_rand.random())
    theta = 2 * pi * sys_rand.random()
    x = r * cos(theta)
    y = r * sin(theta)
    return (x, y)

# Parâmetros e criação do grafo
n_nodes = 20
G = nx.Graph()
nodes = {}

# Gera n_nodes pontos e os adiciona ao grafo com atributo de posição
for i in range(n_nodes):
    pt = generate_random_point_in_disk()
    nodes[i] = pt
    G.add_node(i, pos=pt)

# Conecta nós próximos para simular a estrutura de um grafo (ex.: grafo de Cayley)
threshold = 0.5  # Critério de proximidade para definir arestas
for i in range(n_nodes):
    for j in range(i + 1, n_nodes):
        xi, yi = nodes[i]
        xj, yj = nodes[j]
        d = sqrt((xi - xj) ** 2 + (yi - yj) ** 2)
        if d < threshold:
            G.add_edge(i, j)

def stereographic_projection(point):
    """
    Realiza a projeção estereográfica de um ponto (x, y) do disco de Poincaré para coordenadas 3D.
    Fórmula:
        X = 2x / (1 + x² + y²)
        Y = 2y / (1 + x² + y²)
        Z = (-1 + x² + y²) / (1 + x² + y²)
    """
    x, y = point
    denom = 1 + x**2 + y**2
    X = 2 * x / denom
    Y = 2 * y / denom
    Z = (-1 + x**2 + y**2) / denom
    return (X, Y, Z)

# Aplica a projeção a todos os nós
pos_3d = {i: stereographic_projection(pt) for i, pt in nodes.items()}

# Visualização 3D usando Matplotlib
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection='3d')
ax.set_title('Projeção 3D da Rota Ótima em Espaço Hiperbólico')

# Desenha as arestas do grafo
for (u, v) in G.edges():
    x_vals = [pos_3d[u][0], pos_3d[v][0]]
    y_vals = [pos_3d[u][1], pos_3d[v][1]]
    z_vals = [pos_3d[u][2], pos_3d[v][2]]
    ax.plot(x_vals, y_vals, z_vals, color='black', alpha=0.5)

# Desenha os nós
xs = [pos_3d[i][0] for i in pos_3d]
ys = [pos_3d[i][1] for i in pos_3d]
zs = [pos_3d[i][2] for i in pos_3d]
ax.scatter(xs, ys, zs, color='red', s=50)

# Configuração opcional dos eixos para melhor visualização
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')

plt.show()
