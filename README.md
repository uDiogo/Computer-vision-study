## 📸 Demonstração e Aplicações do Algoritmo Shi-Tomasi

Abaixo, uma demonstração prática do algoritmo **Shi-Tomasi** (também conhecido como *Good Features to Track*) em um cenário real. Os pontos verdes indicam as características (cantos) detectadas na imagem.

![Detecção de Cantos com Shi-Tomasi](asstes/shi_tomasi_deteccao_rua.png)

### Importância e Aplicações

O algoritmo Shi-Tomasi é crucial em Visão Computacional por identificar pontos na imagem que são **robustos a pequenas perturbações** (rotação, escala, iluminação). Essas "good features" são ideais para:

1.  **Rastreamento de Objetos (Object Tracking):** Monitorar a posição de objetos em movimento ao longo de uma sequência de quadros.
2.  **Odometria Visual (Visual Odometry):** Estimar a posição e orientação de uma câmera ou robô no ambiente, analisando o movimento desses pontos no tempo.
3.  **Mapeamento e SLAM (Simultaneous Localization and Mapping):** Construir um mapa do ambiente enquanto se localiza nele.
4.  **Estabilização de Vídeo:** Remover tremores em gravações, alinhando quadros com base nesses pontos fixos.

### Extraindo Informações e Detecção de Faixas

A partir da detecção desses cantos, podemos ir além da simples identificação de pontos. Uma aplicação avançada e prática, especialmente em direção autônoma ou sistemas de assistência ao motorista (ADAS), é a **detecção de faixas de rodagem**.

Podemos processar o conjunto de pontos detectados para:

* **Filtragem de Reta:** Aplicar algoritmos de agrupamento (como RANSAC) ou transformadas (como a Transformada de Hough) para identificar subconjuntos de pontos que se alinham em retas.
* **Identificação de Faixas:** Após identificar as retas, é possível associá-las às faixas da estrada, mesmo em condições de iluminação ou sinalização variáveis.
* **Segmentação:** Isolar as regiões das faixas, permitindo que um sistema de navegação utilize essas informações para manter o veículo na pista ou alertar sobre desvios.

Esta capacidade de extrair e interpretar padrões a partir de características visuais é um pilar para a construção de sistemas inteligentes que interagem com o mundo real.