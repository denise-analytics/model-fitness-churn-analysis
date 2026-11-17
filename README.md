# Model Fitness — Análise de rotatividade e retenção

Análise de 4.000 clientes de uma academia para identificar perfis associados à saída, comparar modelos de classificação e propor ações de retenção baseadas em dados.

## Objetivo

Prever a saída de clientes no mês seguinte e segmentar a base para apoiar a priorização de contatos e o planejamento de iniciativas de retenção.

## Dados

O arquivo `data/gym_churn_us.csv` contém 4.000 registros e 14 colunas, com informações de cadastro, contratos, frequência de visitas e gastos adicionais.

A variável alvo é `Churn`: 1 indica saída e 0 indica permanência. A inspeção não identificou valores ausentes ou registros completamente duplicados.

## Métodos

- Análise exploratória, distribuições e correlações.
- Divisão estratificada: 80% para treino e 20% para validação.
- Padronização ajustada apenas no treino para a regressão logística.
- Comparação entre regressão logística, floresta aleatória e uma referência que sempre prevê permanência.
- Avaliação por acurácia, precisão, sensibilidade, matriz de confusão, ROC-AUC e precisão média.
- Agrupamento hierárquico com método Ward e segmentação com KMeans em cinco grupos, sem utilizar `Churn` na formação dos segmentos.

## Principais resultados

A rotatividade geral foi de **26,52%**. As saídas apresentaram associação com contratos curtos, menor tempo de relacionamento e baixa frequência de visitas.

### Comparação dos modelos

Resultados na amostra de validação:

| Modelo | Acurácia | Precisão | Sensibilidade | ROC-AUC | Precisão média (AP) |
|---|---:|---:|---:|---:|---:|
| Regressão logística | 93,5% | 89,6% | 85,4% | 0,978 | 0,950 |
| Floresta aleatória | 92,2% | 87,5% | 82,5% | 0,970 | 0,930 |

No limiar de 0,5, a regressão logística identificou **181 dos 212 clientes que saíram**, com 21 alertas incorretos e 31 saídas não identificadas.

### Segmentação de clientes

| Grupo | Clientes | Perfil predominante | Rotatividade |
|---|---:|---|---:|
| 0 | 545 | Sem proximidade com a academia e contratos curtos | 44,95% |
| 1 | 768 | Frequência elevada e maior tempo de relacionamento | 6,90% |
| 2 | 642 | Adesão por indicação e presença de vínculos com empresas parceiras | 24,77% |
| 3 | 937 | Contratos longos e maior gasto médio adicional | 2,24% |
| 4 | 1.108 | Contratos curtos, menor tempo de relacionamento e baixa frequência | 52,62% |

Os grupos 4 e 0 apresentam as maiores taxas de saída. O grupo 4 concentra aproximadamente **55% das saídas da base**.

## Aplicações propostas

- Testar acolhimento e acompanhamento de clientes novos com baixa frequência.
- Investigar dificuldades de acesso e reduções na frequência individual.
- Avaliar ações de renovação, parcerias e indicação.
- Medir o efeito das intervenções por comparação com um grupo de controle, com atribuição aleatória quando viável.

As propostas são hipóteses para testes, sem eficácia comprovada nesta análise.

## Limitações

As associações observadas não demonstram causalidade. O desempenho foi avaliado em uma única divisão aleatória; sua estabilidade em períodos futuros ainda precisa ser verificada.

ROC-AUC e precisão média não comprovam calibração das probabilidades. Antes do uso operacional, é necessário confirmar que as características estejam disponíveis no momento da previsão e sejam anteriores à saída prevista.

Os identificadores dos grupos não representam uma ordem de risco. A segmentação pode variar entre ambientes e versões das bibliotecas.

## Arquivos

- [Notebook da análise](model_fitness_churn.ipynb)
- [Base de dados](data/gym_churn_us.csv)
- [Dependências](requirements.txt)

## Como executar

1. Baixe ou clone o repositório.
2. Abra a pasta do projeto no VS Code.
3. Instale as dependências no ambiente Python escolhido:

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Abra `model_fitness_churn.ipynb` e selecione esse mesmo ambiente como kernel.
5. Reinicie o kernel e execute todas as células na ordem.

Mantenha o CSV na pasta `data`, pois o notebook utiliza um caminho relativo.

A execução apresentada utilizou **scikit-learn 1.9.0**, versão fixada nas dependências. O KMeans utiliza `n_clusters=5`, `random_state=0` e `n_init=10`. As demais dependências não estão fixadas, portanto o arquivo não reproduz integralmente o ambiente original.

## Tecnologias

Python, Pandas, Matplotlib, Seaborn, SciPy e scikit-learn.

## Autora

**Denise Duarte**