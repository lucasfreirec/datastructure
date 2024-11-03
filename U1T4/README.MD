![Correios](./imgs/correios.jpg)

## Análise de Grafos com OSMnx e NetworkX

Neste notebook, uma série de análises foram realizadas em um grafo representando a rede viária de Natal, Rio Grande do Norte, Brasil. Abaixo estão as principais etapas e resultados obtidos:

1. **Importação de Bibliotecas**:
    - As bibliotecas `numpy`, `matplotlib.pyplot`, `networkx` e `osmnx` foram usadas para manipulação e visualização de grafos.
    - Essas bibliotecas fornecem as ferramentas necessárias para criar, analisar e visualizar grafos.

2. **Criação do Grafo**:
    - Um grafo foi criado a partir da rede viária de Natal usando a função `ox.graph_from_place`.
    - O grafo representa a rede viária, com nós representando interseções e arestas representando as estradas que as conectam.

3. **Visualização do Grafo**:
    - O grafo foi plotado usando `ox.plot_graph`.
    - Esta visualização ajuda a entender a estrutura e o layout da rede viária.

4. **Informações Básicas do Grafo**:
    - O número de nós e arestas no grafo foram calculados e exibidos.
    - Isso fornece uma compreensão básica do tamanho e complexidade do grafo.
    - Exemplo:
      ```python
      num_nodes = G.number_of_nodes()
      num_edges = G.number_of_edges()
      print(f"O número de nós em G é: {num_nodes}")
      print(f"O número de arestas em G é: {num_edges}")
      ```

5. **Maior Componente Fortemente Conectada**:
    - A maior componente fortemente conectada do grafo foi identificada.
    - Esta componente representa o maior subconjunto do grafo onde cada nó é alcançável a partir de qualquer outro nó dentro do subconjunto.
    - O comprimento médio do caminho mais curto dentro desta componente foi calculado, fornecendo insights sobre a eficiência de viagem dentro da parte mais conectada da rede.
    - Exemplo:
      ```python
      strong_components = list(nx.strongly_connected_components(G))
      largest_strong_component = max(strong_components, key=len)
      G_largest = G.subgraph(largest_strong_component).copy()
      avg_shortest_path_length = nx.average_shortest_path_length(G_largest, weight='length')
      print(f"O Average Shortest Path Length de G é: {avg_shortest_path_length} metros")
      ```

6. **Diâmetro do Grafo**:
    - O diâmetro da maior componente fortemente conectada foi calculado.
    - O diâmetro representa o caminho mais longo mais curto entre quaisquer dois nós na componente, indicando a distância máxima que alguém pode precisar percorrer dentro desta parte da rede.
    - Exemplo:
      ```python
      diameter = nx.diameter(G_largest, weight='length')
      print(f"O Diâmetro de G é: {diameter} metros")
      ```

7. **Nó Mais Distante**:
    - Um ponto de partida foi definido usando coordenadas específicas de latitude e longitude.
    - O nó mais próximo deste ponto foi encontrado usando `ox.distance.nearest_nodes`.
    - A distância para todos os outros nós a partir do nó inicial foi calculada, e o nó mais distante foi identificado.
    - Esta análise ajuda a entender a acessibilidade e a extensão da rede a partir de um ponto específico.
    - Exemplo:
      ```python
      latitude = -5.811551614738788
      longitude = -35.21889920439562
      start_node = ox.distance.nearest_nodes(G, X=longitude, Y=latitude)
      lengths = nx.single_source_dijkstra_path_length(G, start_node, weight='length')
      farthest_node = max(lengths, key=lengths.get)
      max_distance = lengths[farthest_node]
      print(f"O ponto mais distante é o nó {farthest_node} a {max_distance:.2f} metros.")
      ```

8. **Caminho Mais Curto**:
    - O caminho mais curto do nó inicial até o nó mais distante foi calculado usando `nx.shortest_path`.
    - Este caminho foi então plotado no grafo, destacando a rota.
    - Esta análise fornece uma representação visual da rota mais eficiente entre dois pontos distantes na rede.
    - Exemplo:
      ```python
      shortest_path = nx.shortest_path(G, source=start_node, target=farthest_node, weight='length')
      fig, ax = ox.plot_graph_route(G, shortest_path, route_linewidth=2, edge_linewidth=0.5, node_size=0, bgcolor='k')
      ```

9. **Componentes Conectados**:
    - O número de componentes fracamente e fortemente conectados no grafo foi calculado.
    - Componentes fracamente conectados são subconjuntos do grafo onde há um caminho entre quaisquer dois nós se a direção das arestas for ignorada.
    - Componentes fortemente conectados são subconjuntos onde há um caminho entre quaisquer dois nós considerando a direção das arestas.
    - Esta análise fornece insights sobre a conectividade geral e fragmentação da rede.
    - Exemplo:
      ```python
      num_weakly_connected_components = nx.number_weakly_connected_components(G)
      num_strongly_connected_components = nx.number_strongly_connected_components(G)
      print(f"O grafo tem {num_weakly_connected_components} componentes fracamente conectados.")
      print(f"O grafo tem {num_strongly_connected_components} componentes fortemente conectados.")
      ```

10. **Coeficiente de Agrupamento**:
     - O grafo foi convertido para um grafo simples (não direcionado e sem arestas paralelas) para esta análise.
     - O coeficiente de agrupamento médio foi calculado, o que mede o grau em que os nós no grafo tendem a se agrupar.
     - Esta métrica fornece insights sobre a conectividade local e a presença de grupos fortemente conectados dentro da rede.
     - Exemplo:
       ```python
       G_simple = nx.Graph(G)
       clustering_coefficient = nx.average_clustering(G_simple)
       print(f"O coeficiente de clustering de G é: {clustering_coefficient}")
       ```

Essas análises fornecem uma visão detalhada da estrutura e características da rede viária de Natal, oferecendo insights sobre sua conectividade, eficiência e organização geral.