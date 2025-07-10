# Projeto 2 - Introdução à Inteligência Artificial (2025/1)

## Descrição do Projeto

Este projeto foi desenvolvido para a disciplina de Introdução à Inteligência Artificial (IIA) da Universidade de Brasília, realizada com o Prof. Díbio Leandro. 

O objetivo é implementar e avaliar métodos de Deep Learning para a tarefa de deteção e contagem de árvores individuais em imagens aéreas de alta resolução de uma cidade.

A execução e análise são baseadas no trabalho de [Zamboni et al.](https://github.com/pedrozamboni/individual_urban_tree_crown_detection), utilizando o mesmo conjunto de dados, mas aplicando um modelo de deteção de objetos mais recente para comparar a sua performance com os resultados já publicados.

### Artigo de Referência

A base do projeto é o artigo:

Zamboni, P.; Junior, J.M.; de Andrade Silva, J.; Miyoshi, G.T.;Matsubara, E.T.; Nogueira, K.;
Gonçalves, W.N. [Benchmarking Anchor-Based and Anchor-Free State-of-the-Art Deep Learning
Methods for Individual Tree Detection in RGB High-Resolution Images](https://doi.org/10.3390/rs13132482). Remote Sensing. 2021, 13, 2482.

O estudo de Zamboni et al. investigou 21 métodos de Deep Learning para a deteção de copas de árvores. Os autores descobriram que os detectores do tipo anchor-free obtiveram a melhor performance média, com um $AP_{50}$ de **0.686**. O estudo também destaca que os modelos de dois estágios (two-stage) e anchor-free foram os mais adequados para esta tarefa, com ênfase nos modelos **FSAF**, **Double Head**s, **CARAFE**, **ATSS** e **FoveaBox**.

## Metodologia Aplicada

Para este projeto, a metodologia tentou seguir os padrões do artigo de referência, mas com algumas adaptações para utilizar um modelo mais recente e simplificar o processo de treino e avaliação.

- **Modelo Utilizad**o: Foi escolhido o modelo **YOLOv11**, na sua versão `yolo11n.pt` (**nano**), para ser treinado e avaliado
- **Dataset**: Utilizamos o mesmo conjunto de dados disponibilizado publicamente pelos autores do artigo , que consiste em imagens aéreas RGB de $512 \times 512$ pixels da cidade de **Campo Grande**, **Brasil**
- **Hiperparâmetros**: O treino foi realizado por **24 épocas**, o mesmo número utilizado em todos os experimentos do artigo de referência
- **Métrica de Avaliação**: A principal métrica para avaliar a performance do modelo é a **Average Precision** (AP) com um limiar de IoU de $0.5$ ($AP_{50}$)
- **Validação Cruzada**: Para garantir a robustez estatística dos resultados, foi adotada uma abordagem de validação cruzada de 5-folds. O modelo foi treinado e avaliado cinco vezes, utilizando cada um dos cinco folds de dados fornecidos com o dataset
  -  Esta abordagem é análoga ao método de hold-out repetido do artigo, e serve para mitigar o viés de uma única divisão de dados

## Como Executar o Projeto

Este projeto foi desenvolvido para ser executado no ambiente do Google Colaboratory.

### Pré-requisitos

- Uma conta Google com acesso ao Google Drive
- Pasto do projeto criado no Google Drive, com o nome `iia-trabalho-2`

### Passo a Passo
1. Abrir no Google Colab: 
   - Abra o notebook `notebooks/iia-trabalho-2.ipynb` no Google Colab
2. Executar as células de Configuração:
   - Execute as células para instalar a biblioteca ultralytics e montar o seu Google Drive
3. Preparar o Dataset:
   - Certifique-se de que a variável `FOLD_NUMBER` está definida para o fold que deseja processar (ex: `FOLD_NUMBER = 0`).
4. Configuração do Treino:
   - Define os parâmetros do treino (modelo, épocas, etc.)
5. Treinamento do Modelo:
   - Execute a célula de treino para iniciar o processo de treinamento do modelo **YOLOv11**
   - Os resultados serão guardados na pasta definida em `PROJECT_NAME` no seu Google Drive
6. Avaliação do Modelo:
   - Após o treino, execute a célula de avaliação para carregar o melhor modelo treinado
7. Repetir para Validação Cruzada:
   - Para obter resultados robustos, repita os passos 3, 4 e 5 para cada fold, alterando a variável `FOLD_NUMBER` no script de conversão (de 0 a 4)
8. Analisar os Resultados:
   - Ao final das 5 execuções, calcule a média e o desvio padrão dos valores de $AP_{50}$ obtidos. Este será o resultado do experimento

## Resultados
Após a execução da validação cruzada de 5 folds, os resultados de performance ($AP_{50}$) foram os seguintes:

| Fold | $AP_{50}$ | $AP_{50}\text{-}95$ |
|------|-------| ------|
| 0    | 0.690 | 0.368 |
| 1    | 0.702 | 0.359 |
| 2    | 0.686 | 0.358 |
| 3    | 0.714 | 0.379 |
| 4    | 0.685 | 0.353 |

- A média dos valores de $AP_{50}$ obtidos foi de **0.69561**, com um desvio padrão de **0.01226**

A média dos valores de $AP_{50}$ é **ligeiramente superior** ao valor médio de **0.686** reportado no artigo, indicando que o modelo **YOLOv11 nano** conseguiu superar a performance dos modelos anchor-free testados no estudo original.
