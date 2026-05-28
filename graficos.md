# Principais Graficos Usados em EDA

EDA, ou Analise Exploratoria de Dados, usa visualizacoes para entender distribuicoes, comparar grupos, identificar relacoes entre variaveis, encontrar outliers e levantar hipoteses antes da modelagem.

## Setup Padrao

Use este bloco antes dos graficos. Em uma base real, basta trocar a criacao manual do `df` por `pd.read_csv`, `pd.read_excel` ou outra fonte de dados. (não aplicável no nosso digital plus, mas pode usar depois)

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style="whitegrid", palette="Set2")

# Opcao para usar arquivo real:
# df = pd.read_csv("seu_arquivo.csv")
# df = pd.read_excel("seu_arquivo.xlsx")

# df exemplo

df = pd.DataFrame({
    "Coluna1": [10, 12, 13, 15, 18, 20, 21, 22, 24, 30, 32, 35],
    "Coluna2": [100, 110, 105, 120, 150, 160, 155, 170, 175, 190, 200, 210],
    "Coluna3": ["A", "A", "B", "B", "C", "C", "A", "B", "C", "A", "B", "C"],
    "Coluna4": ["Sim", "Nao", "Sim", "Nao", "Sim", "Nao", "Sim", "Sim", "Nao", "Nao", "Sim", "Nao"],
    "Coluna5": pd.date_range("2026-01-01", periods=12, freq="D"),
    "Coluna6": [1, 2, 1, 3, 2, 3, 1, 2, 3, 1, 2, 3],
})

df.head()
```

Nos exemplos abaixo, considere:

| Coluna | Tipo sugerido | Uso comum |
|---|---|---|
| Coluna1 | Numerica | Variavel principal para distribuicao |
| Coluna2 | Numerica | Segunda variavel numerica para comparacao |
| Coluna3 | Categorica | Grupo, classe, segmento ou categoria |
| Coluna4 | Categorica | Segunda categoria para cruzamentos |
| Coluna5 | Data | Variavel temporal |
| Coluna6 | Numerica discreta | Tamanho, nota, nivel ou terceiro indicador |

## Analise Univariada

Analise univariada observa uma variavel por vez.

| Grafico | Tipo de variavel | Quando usar | O que observar |
|---|---|---|---|
| Histograma | Numerica | Entender a distribuicao de uma variavel continua ou discreta com muitos valores | Assimetria, concentracao, caudas, lacunas e possiveis outliers |
| Boxplot | Numerica | Resumir distribuicao e detectar outliers | Mediana, quartis, amplitude interquartil e pontos extremos |
| Grafico de densidade | Numerica | Ver a forma suavizada da distribuicao | Modas, assimetria e comparacao visual com distribuicoes teoricas |
| Grafico de barras | Categorica | Contar frequencias por categoria | Categorias mais comuns, raras ou desbalanceadas |
| Grafico de setores | Categorica | Mostrar proporcoes em poucas categorias | Participacao relativa de cada categoria |
| Tabela de frequencia | Categorica ou discreta | Complementar graficos com contagens e percentuais | Frequencia absoluta, relativa e acumulada |

### Exemplos de perguntas

- Qual e a distribuicao da idade dos clientes?
- Existem valores extremos em uma variavel de receita?
- Uma categoria domina a base de dados?
- A variavel esta muito assimetrica?

### Codigos prontos

#### Histograma

```python
plt.figure(figsize=(8, 4))
sns.histplot(data=df, x="Coluna1", bins=10, kde=True)
plt.title("Histograma - Coluna1")
plt.xlabel("Coluna1")
plt.ylabel("Frequencia")
plt.tight_layout()
plt.show()
```

#### Boxplot

```python
plt.figure(figsize=(6, 4))
sns.boxplot(data=df, y="Coluna1")
plt.title("Boxplot - Coluna1")
plt.ylabel("Coluna1")
plt.tight_layout()
plt.show()
```

#### Grafico de densidade

```python
plt.figure(figsize=(8, 4))
sns.kdeplot(data=df, x="Coluna1", fill=True)
plt.title("Densidade - Coluna1")
plt.xlabel("Coluna1")
plt.ylabel("Densidade")
plt.tight_layout()
plt.show()
```

#### Grafico de barras

```python
plt.figure(figsize=(8, 4))
ordem = df["Coluna3"].value_counts().index
sns.countplot(data=df, x="Coluna3", order=ordem)
plt.title("Frequencia por categoria - Coluna3")
plt.xlabel("Coluna3")
plt.ylabel("Frequencia")
plt.tight_layout()
plt.show()
```

#### Grafico de setores

```python
contagem = df["Coluna3"].value_counts()

plt.figure(figsize=(6, 6))
contagem.plot(kind="pie", autopct="%1.1f%%", startangle=90)
plt.title("Participacao por categoria - Coluna3")
plt.ylabel("")
plt.tight_layout()
plt.show()
```

#### Tabela de frequencia

```python
frequencia = df["Coluna3"].value_counts().reset_index()
frequencia.columns = ["Categoria", "Frequencia"]
frequencia["Percentual"] = frequencia["Frequencia"] / frequencia["Frequencia"].sum() * 100

print(frequencia)
```

## Analise Bivariada

Analise bivariada observa a relacao entre duas variaveis.

| Grafico | Tipos de variaveis | Quando usar | O que observar |
|---|---|---|---|
| Scatter plot | Numerica x numerica | Avaliar relacao entre duas variaveis numericas | Tendencia, correlacao, clusters, outliers e relacoes nao lineares |
| Linha | Tempo x numerica | Ver evolucao temporal de uma metrica | Tendencia, sazonalidade, quebras e picos |
| Boxplot por categoria | Categorica x numerica | Comparar distribuicoes numericas entre grupos | Diferencas de mediana, dispersao e outliers por grupo |
| Violin plot | Categorica x numerica | Comparar forma da distribuicao entre grupos | Densidade, assimetria e multimodalidade por categoria |
| Barras agrupadas | Categorica x categorica | Comparar frequencias entre duas categorias | Diferencas de proporcao e composicao entre grupos |
| Barras empilhadas | Categorica x categorica | Mostrar composicao de categorias dentro de grupos | Participacao relativa e mudanca de mix |
| Heatmap de tabela cruzada | Categorica x categorica | Visualizar associacao entre categorias | Concentracao de frequencias e combinacoes incomuns |
| Grafico de correlacao simples | Numerica x numerica | Medir intensidade e direcao da relacao linear | Sinal e magnitude da correlacao |

### Exemplos de perguntas

- Clientes com maior renda compram mais?
- O ticket medio muda por regiao?
- A taxa de conversao varia por canal?
- Existe relacao entre preco e quantidade vendida?

### Codigos prontos

#### Scatter plot

```python
plt.figure(figsize=(8, 4))
sns.scatterplot(data=df, x="Coluna1", y="Coluna2")
plt.title("Relacao entre Coluna1 e Coluna2")
plt.xlabel("Coluna1")
plt.ylabel("Coluna2")
plt.tight_layout()
plt.show()
```

#### Linha temporal

```python
df_ordenado = df.sort_values("Coluna5")

plt.figure(figsize=(9, 4))
sns.lineplot(data=df_ordenado, x="Coluna5", y="Coluna1", marker="o")
plt.title("Evolucao temporal - Coluna1")
plt.xlabel("Coluna5")
plt.ylabel("Coluna1")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

#### Boxplot por categoria

```python
plt.figure(figsize=(8, 4))
sns.boxplot(data=df, x="Coluna3", y="Coluna1")
plt.title("Distribuicao de Coluna1 por Coluna3")
plt.xlabel("Coluna3")
plt.ylabel("Coluna1")
plt.tight_layout()
plt.show()
```

#### Violin plot

```python
plt.figure(figsize=(8, 4))
sns.violinplot(data=df, x="Coluna3", y="Coluna1")
plt.title("Forma da distribuicao de Coluna1 por Coluna3")
plt.xlabel("Coluna3")
plt.ylabel("Coluna1")
plt.tight_layout()
plt.show()
```

#### Barras agrupadas

```python
tabela = pd.crosstab(df["Coluna3"], df["Coluna4"])

tabela.plot(kind="bar", figsize=(8, 4))
plt.title("Barras agrupadas - Coluna3 x Coluna4")
plt.xlabel("Coluna3")
plt.ylabel("Frequencia")
plt.xticks(rotation=0)
plt.legend(title="Coluna4")
plt.tight_layout()
plt.show()
```

#### Barras empilhadas

```python
tabela_prop = pd.crosstab(df["Coluna3"], df["Coluna4"], normalize="index")

tabela_prop.plot(kind="bar", stacked=True, figsize=(8, 4))
plt.title("Barras empilhadas proporcionais - Coluna3 x Coluna4")
plt.xlabel("Coluna3")
plt.ylabel("Proporcao")
plt.xticks(rotation=0)
plt.legend(title="Coluna4")
plt.tight_layout()
plt.show()
```

#### Heatmap de tabela cruzada

```python
tabela = pd.crosstab(df["Coluna3"], df["Coluna4"])

plt.figure(figsize=(7, 4))
sns.heatmap(tabela, annot=True, fmt="d", cmap="Blues")
plt.title("Tabela cruzada - Coluna3 x Coluna4")
plt.xlabel("Coluna4")
plt.ylabel("Coluna3")
plt.tight_layout()
plt.show()
```

#### Correlacao simples com linha de tendencia

```python
correlacao = df["Coluna1"].corr(df["Coluna2"])
print(f"Correlacao entre Coluna1 e Coluna2: {correlacao:.2f}")

plt.figure(figsize=(8, 4))
sns.regplot(data=df, x="Coluna1", y="Coluna2")
plt.title("Correlacao entre Coluna1 e Coluna2")
plt.xlabel("Coluna1")
plt.ylabel("Coluna2")
plt.tight_layout()
plt.show()
```

## Analise Multivariada

Analise multivariada observa tres ou mais variaveis ao mesmo tempo.

| Grafico | Tipos de variaveis | Quando usar | O que observar |
|---|---|---|---|
| Matriz de correlacao | Varias numericas | Avaliar relacoes lineares entre muitas variaveis numericas | Pares fortemente correlacionados, multicolinearidade e redundancia |
| Heatmap de correlacao | Varias numericas | Visualizar a matriz de correlacao de forma mais rapida | Blocos de variaveis relacionadas e correlacoes positivas ou negativas |
| Pairplot | Varias numericas, opcionalmente com categoria | Explorar relacoes par a par entre variaveis | Padroes, clusters, outliers e separacao por grupos |
| Scatter plot com cor | Numerica x numerica + categorica | Ver relacao entre duas variaveis segmentada por grupo | Agrupamentos e diferencas de comportamento por categoria |
| Scatter plot com tamanho | Numerica x numerica + numerica | Adicionar uma terceira dimensao numerica ao scatter plot | Relacao conjunta entre posicao e magnitude |
| Facet grid | Variaveis numericas ou categoricas segmentadas | Repetir o mesmo grafico por categorias | Comparacao entre segmentos mantendo a mesma escala visual |
| Coordenadas paralelas | Varias numericas, opcionalmente com categoria | Comparar perfis entre registros ou grupos | Padroes de comportamento e separacao entre classes |
| Grafico 3D | Tres numericas | Explorar relacoes espaciais entre tres variaveis | Estruturas, agrupamentos e superficies, com cuidado para legibilidade |
| PCA plot | Varias numericas reduzidas para componentes | Visualizar dados de alta dimensao em 2D ou 3D | Separacao de grupos, variancia explicada e possiveis clusters |

### Exemplos de perguntas

- Quais variaveis numericas estao fortemente correlacionadas?
- Existem grupos naturais nos dados?
- Uma categoria separa bem os registros quando olhamos varias variaveis?
- Ha variaveis redundantes que podem prejudicar um modelo?

### Codigos prontos

#### Matriz de correlacao

```python
colunas_numericas = ["Coluna1", "Coluna2", "Coluna6"]
matriz_corr = df[colunas_numericas].corr()

print(matriz_corr)
```

#### Heatmap de correlacao

```python
colunas_numericas = ["Coluna1", "Coluna2", "Coluna6"]
matriz_corr = df[colunas_numericas].corr()

plt.figure(figsize=(7, 5))
sns.heatmap(matriz_corr, annot=True, cmap="coolwarm", vmin=-1, vmax=1)
plt.title("Heatmap de correlacao")
plt.tight_layout()
plt.show()
```

#### Pairplot

```python
sns.pairplot(
    data=df,
    vars=["Coluna1", "Coluna2", "Coluna6"],
    hue="Coluna3"
)
plt.show()
```

#### Scatter plot com cor

```python
plt.figure(figsize=(8, 4))
sns.scatterplot(data=df, x="Coluna1", y="Coluna2", hue="Coluna3")
plt.title("Coluna1 x Coluna2 por Coluna3")
plt.xlabel("Coluna1")
plt.ylabel("Coluna2")
plt.tight_layout()
plt.show()
```

#### Scatter plot com tamanho

```python
plt.figure(figsize=(8, 4))
sns.scatterplot(
    data=df,
    x="Coluna1",
    y="Coluna2",
    hue="Coluna3",
    size="Coluna6",
    sizes=(50, 300)
)
plt.title("Coluna1 x Coluna2 com cor e tamanho")
plt.xlabel("Coluna1")
plt.ylabel("Coluna2")
plt.tight_layout()
plt.show()
```

#### Facet grid

```python
g = sns.FacetGrid(df, col="Coluna3", hue="Coluna4", height=4)
g.map_dataframe(sns.scatterplot, x="Coluna1", y="Coluna2")
g.add_legend()
g.set_axis_labels("Coluna1", "Coluna2")
g.fig.suptitle("Relacao por grupo - Coluna3", y=1.05)
plt.show()
```

#### Coordenadas paralelas

```python
from pandas.plotting import parallel_coordinates

df_parallel = df[["Coluna1", "Coluna2", "Coluna6", "Coluna3"]].copy()

plt.figure(figsize=(9, 4))
parallel_coordinates(df_parallel, class_column="Coluna3")
plt.title("Coordenadas paralelas")
plt.xlabel("Variaveis")
plt.ylabel("Valores")
plt.tight_layout()
plt.show()
```

#### Grafico 3D

```python
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection="3d")

ax.scatter(df["Coluna1"], df["Coluna2"], df["Coluna6"])
ax.set_title("Grafico 3D")
ax.set_xlabel("Coluna1")
ax.set_ylabel("Coluna2")
ax.set_zlabel("Coluna6")

plt.tight_layout()
plt.show()
```

#### PCA plot opcional

Este exemplo usa `scikit-learn`. Caso nao tenha instalado, use `pip install scikit-learn`.

```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

colunas_numericas = ["Coluna1", "Coluna2", "Coluna6"]
x = df[colunas_numericas]
x_padronizado = StandardScaler().fit_transform(x)

pca = PCA(n_components=2)
componentes = pca.fit_transform(x_padronizado)

df_pca = pd.DataFrame(componentes, columns=["PC1", "PC2"])
df_pca["Coluna3"] = df["Coluna3"].values

plt.figure(figsize=(8, 4))
sns.scatterplot(data=df_pca, x="PC1", y="PC2", hue="Coluna3")
plt.title("PCA plot")
plt.xlabel("PC1")
plt.ylabel("PC2")
plt.tight_layout()
plt.show()

print("Variancia explicada:", pca.explained_variance_ratio_)
```

## Guia Rapido de Escolha

| Objetivo | Graficos recomendados |
|---|---|
| Ver distribuicao de uma variavel numerica | Histograma, densidade, boxplot |
| Detectar outliers | Boxplot, scatter plot |
| Contar categorias | Barras, tabela de frequencia |
| Comparar grupos | Boxplot por categoria, violin plot, barras agrupadas |
| Ver relacao entre duas variaveis numericas | Scatter plot, linha, correlacao |
| Ver relacoes entre muitas variaveis numericas | Matriz de correlacao, heatmap, pairplot |
| Analisar evolucao temporal | Linha, area, barras por periodo |
| Explorar segmentos | Facet grid, scatter com cor, barras empilhadas |

## Boas Praticas

- Use escala adequada para evitar interpretacoes distorcidas.
- Sempre confira valores ausentes antes de interpretar graficos.
- Combine visualizacoes com estatisticas descritivas.
- Para variaveis muito assimetricas, considere escala logaritmica.
- Evite grafico de setores com muitas categorias.
- Em correlacoes, lembre que correlacao nao implica causalidade.
- Para categorias com muitas classes, agrupe categorias raras ou mostre apenas as principais.
- Em series temporais, ordene corretamente as datas antes de plotar.
