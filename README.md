# loggi-df-logistics-analysis
# Análise Logística e Otimização de Rotas: Um Estudo de Caso da Loggi no Distrito Federal

Este projeto realiza uma análise aprofundada das operações de entrega da empresa de logística Loggi no Distrito Federal (DF). Partindo de um conjunto de dados públicos em JSON aninhado, o projeto demonstra um pipeline completo de Engenharia de Dados e Análise de Dados (EDA), com o objetivo de identificar padrões operacionais e propor soluções estratégicas para otimização de rotas.

Este é um projeto que demonstra habilidades em:
* Engenharia de Dados (processamento de JSON aninhado, normalização)
* Análise de Dados Exploratória (EDA)
* Visualização de Dados Geoespacial (Geopandas)
* Geração de Insights de Negócio e Recomendações Estratégicas

---

##  Problema de Negócio

A Loggi, como qualquer empresa de logística em larga escala, divide suas operações em centros de distribuição (hubs) para otimizar a cobertura regional. O desafio deste projeto é analisar os dados públicos de entrega da empresa para o DF e responder às seguintes perguntas:

1.  Como as entregas estão distribuídas geograficamente pelo Distrito Federal?
2.  Qual é a estratégia de segmentação de áreas para cada centro de distribuição (hub)?
3.  Existem ineficiências operacionais ou oportunidades de otimização de custos com base na localização dos hubs e nas zonas de entrega?

---

##  Metodologia

O pipeline do projeto foi dividido em três etapas principais:

### 1. Engenharia e Limpeza de Dados (Data Engineering)

O desafio inicial foi o formato dos dados. O dataset original, um arquivo JSON de 199 linhas, continha estruturas aninhadas complexas, especialmente na coluna `deliveries`.

* **Normalização:** As colunas `origin` (JSON) e `deliveries` (lista de dicionários) foram normalizadas.
* **Explosão de Dados:** A coluna `deliveries` foi "explodida" (`explode`) para que cada entrega individual se tornasse uma linha única no DataFrame.
* **Resultado:** O processo transformou um dataset inicial de **199 linhas** em um DataFrame estruturado e pronto para análise com **636.139 linhas**, onde cada linha representa uma entrega única com suas coordenadas.

### 2. Análise Exploratória e Visualização Geoespacial

Com os dados estruturados, foi utilizada a biblioteca `Geopandas` para plotar cada ponto de entrega no mapa do Distrito Federal, segmentando-os por hub de origem (`df-0`, `df-1`, `df-2`).

![Mapa de Entregas no DF](imagem_2025-11-02_114438773.png)

### 3. Análise de Desempenho

A proporção de entregas por hub foi calculada para entender a participação de cada centro na operação total.

![Proporção de Entregas](imagem_2025-11-02_114457411.png)

---

##  Principais Insights e Descobertas

A análise dos gráficos revelou uma clara especialização operacional:

* **Hub `df-1` (Azul | 47% das entregas):** O principal hub, focado na região central de altíssima densidade do DF (provavelmente Plano Piloto, Águas Claras, etc.). Seu desafio é a densidade e o trânsito.
* **Hub `df-2` (Verde | 41% das entregas):** O segundo maior hub, cobrindo outras grandes regiões urbanas e suburbanas de alta densidade (provavelmente Taguatinga, Ceilândia, etc.).
* **Hub `df-0` (Vermelho | 11% das entregas):** O "Explorador de Longa Distância". Este hub tem o menor volume, mas cobre a **maior área geográfica**, atendendo zonas rurais, condomínios afastados e áreas de baixa densidade.

### A Ineficiência-Chave: Custo de Deslocamento Ocioso

O *insight* mais crítico surge ao cruzar o mapa com a localização física dos hubs (marcados com 'x' preto no mapa). Todos os três hubs estão fisicamente localizados na região central.

Isso significa que os veículos do **Hub `df-0` (Vermelho)**, que entregam na periferia distante, precisam **atravessar toda a zona urbana central** (zonas azul e verde) antes mesmo de iniciar sua rota de entrega. Esse tempo, conhecido como "stem time" ou "tempo de deslocamento ocioso", representa um custo operacional significativo em combustível e horas de trabalho.

---

##  Recomendação Estratégica

Com base na ineficiência identificada, a seguinte recomendação de negócio é proposta:

**Implementar um Ponto de "Cross-Docking" ou Micro-Hub Periférico.**

Em vez de os veículos de entrega do `df-0` saírem do hub central, a empresa poderia usar um veículo maior (VUC ou caminhão) para transportar todas as 11% de cargas até um micro-hub estratégico, localizado na borda da zona vermelha.

Nesse ponto, veículos menores e mais ágeis (como motos ou fiorinos) fariam a "última milha".

**Vantagens:**
* **Redução Drástica de Custos:** Elimina o "tempo de deslocamento ocioso" de dezenas de veículos.
* **Aumento da Eficiência:** Os veículos de entrega final passam mais tempo entregando e menos tempo se deslocando.
* **Velocidade:** Potencial de redução no tempo total de entrega para os clientes da zona vermelha.

---

##  Tecnologias Utilizadas

* **Linguagem:** Python
* **Bibliotecas de Análise:** Pandas
* **Visualização Geoespacial:** Geopandas, Matplotlib
* **Visualização:** Seaborn
* **Ambiente:** Jupyter Notebook
