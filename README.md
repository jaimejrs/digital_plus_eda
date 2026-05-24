# Análise Exploratória de Dados - Spanish Wines

Este repositório reúne os materiais de uma aula completa de **Análise Exploratória de Dados (EDA)** usando o dataset [Spanish Wine Quality Dataset](https://www.kaggle.com/datasets/fedesoriano/spanish-wine-quality-dataset). O objetivo é mostrar como transformar um conjunto de dados bruto em perguntas, hipóteses, visualizações, decisões de tratamento e conclusões analíticas bem fundamentadas.

A aula combina uma apresentação em HTML com um notebook prático em Python, percorrendo o ciclo completo de EDA: entendimento do domínio, exploração inicial, classificação de variáveis, diagnóstico de nulos, formulação de hipóteses e análises univariadas, bivariadas e multivariadas.

## Materiais

| Arquivo | Descrição |
| --- | --- |
| `aula_eda_completa.html` | Apresentação da aula, organizada em módulos conceituais e práticos. |
| `01 - eda_spanish_wines.ipynb` | Notebook principal com a análise aplicada ao dataset de vinhos espanhóis. |
| `boxplot0.png` e `boxplot1.png` | Imagens de apoio usadas na apresentação para comparar boxplots em escalas diferentes. |
| `requirements.txt` | Dependências necessárias para executar o notebook. |

## Tópicos trabalhados

| Módulo | Conteúdos |
| --- | --- |
| Fundamentos de EDA | Papel da EDA no fluxo de dados, objetivos centrais e postura investigativa. |
| Tipos de variáveis | Variáveis qualitativas e quantitativas, subtipos, escala ordinal e dicionário de dados. |
| Bibliotecas essenciais | Uso integrado de `pandas`, `numpy`, `matplotlib`, `seaborn` e `kagglehub`. |
| Exploração inicial | `head()`, `tail()`, `info()`, `describe()`, `nunique()`, cardinalidade e primeiras decisões. |
| Valores nulos | Diferença entre nulo e zero, mecanismos MCAR/MAR/MNAR, diagnóstico e estratégias de tratamento. |
| Perguntas e hipóteses | Formulação de perguntas analíticas, hipóteses verificáveis e síntese de resultados. |
| Análise univariada | Medidas de tendência central e dispersão, histogramas, KDE, boxplots, escala logarítmica e outliers. |
| Análise bivariada | Correlação de Pearson, scatter plot, heatmap, boxplot por grupo e interpretação de causalidade. |
| Análise multivariada | Violin plot com `split`, `pivot_table` + heatmap, scatter multivariado e leitura de interações. |
| Comunicação analítica | Como escrever conclusões honestas, registrar limitações e transformar gráficos em narrativa. |

## Competências desenvolvidas

- Estruturar uma EDA do início ao fim, evitando uma sequência solta de gráficos.
- Classificar variáveis por tipo, subtipo e papel analítico.
- Criar e usar dicionários de dados para documentar decisões.
- Diagnosticar valores ausentes antes de imputar ou descartar registros.
- Diferenciar relações lineares, não lineares e padrões que a correlação pode esconder.
- Usar transformações, como `log10`, para revelar padrões comprimidos pela escala original.
- Escolher visualizações adequadas para combinações `quant x quant`, `qual x quant` e `qual x qual`.
- Interpretar histogramas, boxplots, heatmaps, violin plots e scatter plots multivariados.
- Comparar grupos por mediana, dispersão, sobreposição de distribuições e presença de outliers.
- Formular hipóteses, testá-las com evidências e registrar achados confirmados ou refutados.
- Comunicar resultados com cautela, especialmente quando há grupos pequenos ou risco de causalidade indevida.

## Stack utilizada

| Tecnologia | Uso na aula |
| --- | --- |
| Python | Linguagem principal da análise. |
| Jupyter Notebook / IPython Kernel | Ambiente de execução e documentação da EDA. |
| pandas | Leitura, inspeção, limpeza, agrupamentos, tabelas e transformações. |
| numpy | Operações numéricas e transformação logarítmica. |
| seaborn | Visualizações estatísticas: histogramas, boxplots, heatmaps, scatter plots e violin plots. |
| matplotlib | Ajustes finos de layout, títulos, eixos, escalas e composição de figuras. |
| kagglehub | Download programático do dataset usado na aula. |
| HTML, CSS e JavaScript | Apresentação navegável da aula. |

Versões principais estão fixadas em `requirements.txt`.

## Dataset

A análise usa o **Spanish Wine Quality Dataset**, baixado via `kagglehub`.

O notebook investiga aproximadamente:

- 7.500 vinhos espanhóis;
- 76 regiões;
- 21 tipos de vinho;
- mais de sete décadas de safras;
- variáveis como `winery`, `wine`, `year`, `rating`, `num_reviews`, `region`, `price`, `type`, `body` e `acidity`.

## Perguntas analisadas no notebook

| Pergunta | Hipótese | Resultado resumido |
| --- | --- | --- |
| Q1: Avaliação x preço | Vinhos mais bem avaliados custam mais. | Confirmada; `rating` e `price_log` têm correlação positiva relevante. |
| Q2: Região x avaliação | Regiões renomadas têm avaliações superiores. | Parcialmente confirmada; diferenças existem, mas o dataset já concentra vinhos bem avaliados. |
| Q3: Tipo x preço | Alguns tipos são sistematicamente mais caros. | Confirmada; há amplitude de cerca de 10x entre tipos. |
| Q4: Avaliação, preço e ano | Vinhos antigos e bem avaliados custam mais. | Confirmada em análise multivariada. |
| Q5: Preço x número de reviews | Vinhos mais baratos recebem mais reviews. | Não confirmada; popularidade e preço ficaram praticamente independentes. |
| Q6: Diversidade de tipos por região | Algumas regiões são mais diversas. | Confirmada; Vino de Espana e Rioja lideram em diversidade. |
| Q7: Corpo x preço x região | Vinhos mais encorpados e bem avaliados custam mais. | Confirmada, com efeito fortemente condicionado pela região. |
| Q8: Acidez x preço | Acidez moderada está associada a preços maiores. | Não confirmada; apareceu relação inversa, com ressalva por tamanho de grupo. |

## Como executar

Crie um ambiente virtual e instale as dependências:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Depois, abra o notebook pelo VS Code, Jupyter Lab ou Jupyter Notebook. Se quiser executar pelo terminal e ainda não tiver o frontend instalado, use:

```bash
pip install notebook
jupyter notebook "01 - eda_spanish_wines.ipynb"
```

A apresentação pode ser aberta diretamente no navegador:

```bash
xdg-open aula_eda_completa.html
```

## Resultado esperado

Ao final da aula, a pessoa deve ser capaz de conduzir uma EDA com clareza metodológica: entender os dados, levantar hipóteses, escolher gráficos adequados, interpretar padrões com cuidado, tratar nulos sem introduzir vieses e comunicar conclusões de forma honesta.

> Toda boa análise começa com uma boa pergunta.
