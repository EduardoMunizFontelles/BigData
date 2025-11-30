# Projeto: Previsão de Sucesso de Filmes — Pipeline de Dados (AV1)

---

## 1. Introdução

A indústria cinematográfica mundial movimenta bilhões de dólares anualmente, sendo um dos setores mais dinâmicos e competitivos do entretenimento. No entanto, o processo de produção de filmes envolve riscos significativos, com muitos projetos resultando em prejuízos financeiros consideráveis. Estudos indicam que apenas uma pequena parcela dos filmes produzidos alcança sucesso comercial ou crítico, enquanto muitos não recuperam sequer o investimento inicial.

O problema central reside na dificuldade de prever, antes do lançamento, quais filmes terão sucesso e quais não. Fatores como gênero, elenco, diretor, orçamento, período de lançamento e público-alvo interagem de forma complexa, tornando essa previsão um desafio significativo. A capacidade de antecipar o desempenho de um filme poderia auxiliar estúdios, distribuidores e investidores a tomar decisões mais informadas sobre quais projetos apoiar, como investir recursos limitados e como otimizar estratégias de marketing e distribuição.

Neste contexto, o uso de técnicas de Big Data e ciência de dados surge como uma solução promissora para analisar padrões históricos em grandes volumes de dados de filmes e desenvolver modelos preditivos que possam auxiliar na tomada de decisão estratégica.

---

## 2. Motivação

A escolha deste projeto é fundamentada em múltiplas razões de relevância técnica, acadêmica e prática:

### 2.1 Relevância Acadêmica

Este projeto faz parte da Avaliação 1 (AV1) da disciplina de **Big Data**, proporcionando uma aplicação prática dos conceitos fundamentais de engenharia de dados. A construção de um pipeline completo de dados permite o exercício de habilidades essenciais como ingestão, transformação, limpeza e armazenamento de dados em larga escala.

### 2.2 Relevância do Domínio

O setor de **Mídia e Entretenimento** apresenta características ideais para a aplicação de Big Data:

- **Volume massivo de dados**: Milhares de filmes são produzidos anualmente, cada um com centenas de atributos e interações com milhões de espectadores.
- **Diversidade de fontes**: Dados provenientes de múltiplas plataformas (cinemas, streaming, críticas, redes sociais) criam um ecossistema rico para análise.
- **Valor estratégico**: A capacidade de prever sucesso tem impacto direto em decisões de investimento que envolvem milhões de dólares.

### 2.3 Disponibilidade de Dados

O dataset *The Movies Dataset*, disponível publicamente no Kaggle, reúne metadados de aproximadamente **45 mil filmes**, incluindo informações históricas sobre bilheteria, avaliações, elenco, diretores, gêneros e outros atributos relevantes. Essa abrangência e qualidade dos dados tornam o projeto viável e promissor.

### 2.4 Aplicabilidade Prática

A solução desenvolvida tem potencial aplicação em:

- **Estúdios cinematográficos**: Auxílio na decisão de greenlight de projetos.
- **Distribuidoras**: Otimização de estratégias de lançamento e marketing.
- **Plataformas de streaming**: Recomendação de conteúdo e aquisição de licenças.
- **Investidores**: Avaliação de risco e retorno sobre investimento em produções.

---

## 3. Objetivo do Projeto

### 3.1 Objetivo Geral

Desenvolver um **pipeline de dados robusto e escalável** para o setor de Mídia e Entretenimento, focado na preparação de dados históricos de filmes que permitam, em etapas futuras, a construção de modelos preditivos capazes de estimar o sucesso de um filme antes de seu lançamento.

### 3.2 Objetivos Específicos

Nesta primeira etapa do projeto (AV1), os objetivos específicos incluem:

1. **Ingestão de Dados**: Estabelecer um processo automatizado para coleta e download do dataset *The Movies Dataset* do Kaggle.

2. **Armazenamento Estruturado**: Organizar os dados em camadas hierárquicas (raw, processed), seguindo princípios de arquitetura de dados moderna (Medallion Architecture).

3. **Transformação e Limpeza**: 
   - Identificar e tratar valores ausentes, duplicados e inconsistentes.
   - Normalizar tipos de dados e formatos.
   - Remover outliers que possam comprometer análises futuras.
   - Extrair e estruturar informações aninhadas (gêneros, elenco, diretores).

4. **Integração de Dados**: Consolidar múltiplas fontes (metadados de filmes e créditos) em um dataset único e coerente.

5. **Qualidade de Dados**: Garantir que o dataset final seja limpo, completo e adequado para análises exploratórias e treinamento de modelos de machine learning.

6. **Análise Exploratória**: Realizar análises estatísticas e visualizações que permitam compreender padrões, correlações e características dos dados processados.

O pipeline desenvolvido estabelece a base para futuras etapas do projeto, que incluirão a construção de modelos de machine learning para prever:
- **Sucesso comercial**: Estimativa de bilheteria/receita.
- **Sucesso crítico**: Estimativa de avaliação média do público.

---

## 4. Metodologia (Pipeline de Dados)

O pipeline de dados desenvolvido segue uma arquitetura em camadas (Medallion Architecture), permitindo a separação clara entre dados brutos, transformados e processados. A metodologia é dividida em cinco etapas principais:

### 4.1 Fontes de Dados

#### 4.1.1 Dataset Principal

- **Nome**: The Movies Dataset
- **Fonte**: [Kaggle](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset)
- **Proprietário**: Rounak Banik
- **Tamanho**: ~228 MB (dataset completo)
- **Formato**: Arquivos CSV

#### 4.1.2 Arquivos Utilizados

1. **movies_metadata.csv** (45.466 registros, 24 colunas)
   - Metadados principais dos filmes
   - Informações sobre título, lançamento, gêneros, orçamento, receita, popularidade, avaliações

2. **credits.csv** (45.476 registros, 3 colunas)
   - Informações sobre elenco (cast) e equipe (crew)
   - Dados no formato JSON aninhado

3. **Arquivos adicionais disponíveis** (não utilizados nesta etapa):
   - ratings.csv, ratings_small.csv
   - keywords.csv
   - links.csv, links_small.csv

#### 4.1.3 Características dos Dados

- **Cobertura temporal**: Filmes desde o início do cinema até produções recentes
- **Variedade geográfica**: Filmes de múltiplos países e idiomas
- **Densidade informacional**: Alta variabilidade na completude dos dados (valores ausentes são comuns)

---

### 4.2 Ingestão

#### 4.2.1 Tecnologias Utilizadas

- **Python 3.12+**: Linguagem de programação principal
- **kagglehub**: Biblioteca para download automatizado de datasets do Kaggle
- **pandas**: Manipulação e estruturação dos dados
- **os**: Gerenciamento de sistema de arquivos

#### 4.2.2 Processo de Ingestão

A etapa de ingestão é implementada no notebook `01_ingestion.ipynb` e segue os seguintes passos:

1. **Criação da estrutura de diretórios**:
   - Verificação e criação automática da pasta `dados/raw/` caso não exista

2. **Download automatizado do dataset**:
   - Utilização da biblioteca `kagglehub` para download direto do Kaggle
   - O dataset é baixado para o cache local do sistema

3. **Leitura inicial dos arquivos**:
   - Carregamento dos arquivos CSV principais utilizando `pandas.read_csv()`
   - Uso do parâmetro `low_memory=False` para evitar warnings de tipo misto

4. **Validação dos dados**:
   - Exibição das dimensões de cada tabela carregada
   - Prévia dos dados para verificação visual

5. **Armazenamento na Camada Raw (Bronze)**:
   - Cópia dos arquivos originais para `dados/raw/`
   - Preservação integral dos dados brutos para reprocessamento futuro

#### 4.2.3 Tipo de Ingestão

- **Tipo**: Ingestão em **BATCH (lotes)**
- **Justificativa**: O dataset do Kaggle é estático e histórico, sendo baixado completo de uma vez através da API do Kaggle
- **Frequência**: Download único ou sob demanda (não requer atualização contínua)
- **Vantagens do Batch**:
  - Simplicidade de implementação
  - Processamento completo do dataset histórico
  - Adequado para análises exploratórias e modelagem

#### 4.2.4 Resultado da Ingestão

Após a ingestão, os seguintes arquivos são armazenados em `dados/raw/`:
- `movies_metadata.csv`: 45.466 filmes com 24 colunas
- `credits.csv`: 45.476 registros com 3 colunas

---

### 4.3 Transformação

A transformação é realizada no notebook `02_transformation.ipynb` e representa a etapa mais complexa do pipeline, envolvendo múltiplas operações de limpeza e enriquecimento dos dados.

#### 4.3.1 Tecnologias Utilizadas

- **pandas**: Manipulação de DataFrames
- **numpy**: Operações numéricas e tratamento de valores ausentes
- **ast**: Conversão de strings JSON aninhadas para objetos Python
- **matplotlib / seaborn**: Visualizações para análise exploratória e identificação de padrões
- **pyarrow**: Preparação para uso do formato Parquet na etapa de carregamento (formato otimizado para Big Data)

#### 4.3.2 Processo de Transformação - Tabela Movies

##### 4.3.2.1 Seleção de Colunas

Seleção de 12 colunas relevantes do dataset original (24 colunas):
- `id`, `title`, `release_date`, `genres`, `runtime`
- `budget`, `revenue`, `popularity`, `vote_average`, `vote_count`
- `original_language`, `status`

##### 4.3.2.2 Limpeza de IDs Inválidos

- Identificação de 3 registros com IDs não numéricos
- Remoção desses registros inválidos
- Conversão de IDs para tipo `int64`

##### 4.3.2.3 Conversão de Tipos de Dados

- `id`: `object` → `int64`
- `release_date`: `object` → `datetime64[ns]`
- `budget`: `object` → `int64` (valores numéricos)
- `revenue`: `float64` (já numérico)
- `popularity`: `object` → `float64`

##### 4.3.2.4 Tratamento de Valores Ausentes

- Remoção de linhas com qualquer valor ausente (dropna)
- Redução de 45.466 para 45.043 registros

##### 4.3.2.5 Análise de Valores Zero

Identificação de colunas com altos percentuais de zeros:
- `revenue`: 37.643 zeros (83,6%)
- `budget`: 36.170 zeros (80,3%)
- `vote_average`: 2.828 zeros
- `vote_count`: 2.730 zeros

**Decisão**: Remoção das colunas `budget`, `revenue` e `popularity` devido à alta taxa de valores ausentes/zero que comprometeria análises futuras.

##### 4.3.2.6 Filtragem de Valores Válidos

- Remoção de registros onde `runtime`, `vote_average` ou `vote_count` sejam zero
- Garantia de dados numéricos válidos para análises

##### 4.3.2.7 Tratamento de Outliers

**Runtime (duração)**:
- Identificação de valores extremos (filmes muito curtos ou muito longos)
- Filtragem para manter apenas filmes entre 60 e 300 minutos
- Justificativa: Filmes muito curtos podem ser trailers ou conteúdo especial, enquanto muito longos podem ser séries ou documentários compilados

**Status**:
- Filtragem para manter apenas filmes com status "Released"
- Remoção de filmes cancelados, em produção ou anunciados

##### 4.3.2.8 Engenharia de Features

- **Extração do ano**: Criação da coluna `year` a partir de `release_date`
- **Processamento de gêneros**: 
  - Conversão de strings JSON aninhadas para listas Python
  - Extração de `id_genre` e `genre_name` (múltiplos gêneros por filme)

##### 4.3.2.9 Filtros Adicionais de Qualidade

- **vote_count**: Mantidos apenas filmes com pelo menos 50 votos
  - Justificativa: Avaliações com poucos votos podem ser não representativas

**Resultado final da transformação da tabela Movies**:
- **8.930 filmes** com 9 colunas após todas as transformações

#### 4.3.3 Processo de Transformação - Tabela Credits

##### 4.3.3.1 Processamento de Dados Aninhados

- Conversão das colunas `cast` e `crew` de strings JSON para listas de dicionários Python
- Uso de `ast.literal_eval()` para parsing seguro

##### 4.3.3.2 Extração de Informações Relevantes

- **Diretor**: Extração do membro da equipe com `job == 'Director'`
- **Ator Principal**: Extração do primeiro ator da lista de `cast`

##### 4.3.3.3 Seleção de Colunas

Criação da tabela `credits_clean` com apenas:
- `id`, `director`, `main_actor`

#### 4.3.4 Integração de Dados (Merge)

- Junção das tabelas `movies` e `credits_clean` utilizando `id` como chave
- Tipo de join: `left` (mantém todos os filmes, mesmo sem créditos)
- Remoção de registros com valores ausentes em `director` ou `main_actor` após o merge

**Resultado final após merge**:
- **8.935 filmes** com 11 colunas

#### 4.3.5 Estrutura Final dos Dados Transformados

O dataset final (`movies_full_cleaned.csv`) contém as seguintes colunas:

1. `id`: Identificador único do filme (int64)
2. `title`: Título do filme (object)
3. `runtime`: Duração em minutos (float64)
4. `vote_average`: Nota média de avaliação (float64)
5. `vote_count`: Número de votos recebidos (float64)
6. `original_language`: Idioma original (object)
7. `year`: Ano de lançamento (int32)
8. `id_genre`: IDs dos gêneros (object, múltiplos valores)
9. `genre_name`: Nomes dos gêneros (object, múltiplos valores)
10. `director`: Nome do diretor (object)
11. `main_actor`: Nome do ator principal (object)

---

### 4.4 Carregamento

A etapa de **Carregamento (Loading)** é implementada no notebook `03_loading.ipynb` e consiste no processo de carregar os dados já transformados e refinados da camada **Silver** para o sistema de armazenamento final na camada **Gold**.

#### 4.4.1 Tecnologias Utilizadas

- **pandas**: Manipulação de DataFrames
- **pyarrow**: Leitura e escrita de arquivos Parquet (formato otimizado para Big Data)
- **os**: Gerenciamento de sistema de arquivos

#### 4.4.2 Processo de Carregamento

O processo de carregamento segue os seguintes passos:

1. **Carregamento dos Dados da Silver**:
   - Leitura do arquivo `movies_full_cleaned.csv` da camada Silver (processed)
   - Validação inicial da estrutura e dimensões

2. **Validações Finais de Qualidade**:
   - Verificação de valores ausentes por coluna
   - Remoção de registros com valores ausentes para garantir integridade
   - Validação de tipos de dados
   - Verificação de dimensões finais

3. **Carregamento para Camada Gold**:
   - Salvamento em formato **Parquet** (otimizado, com compressão e preservação de tipos)
   - Salvamento em formato **CSV** (backup e compatibilidade)
   - Comparação de tamanhos para demonstrar eficiência do Parquet

#### 4.4.3 Resultado do Carregamento

Após o carregamento, os seguintes arquivos são armazenados em `dados/gold/`:
- `movies_final.parquet`: Dados otimizados em formato Parquet
- `movies_final.csv`: Backup em formato CSV

**Características dos arquivos finais**:
- **Registros**: 8.927 filmes (após remoção de valores ausentes finais)
- **Colunas**: 11 atributos
- **Qualidade**: Dados completamente limpos, sem valores ausentes, tipos normalizados

---

### 4.5 Destino

A camada **Gold** representa o **destino final** dos dados processados, onde ficam disponíveis para consumo por analistas, cientistas de dados ou ferramentas de visualização.

#### 4.5.1 Localização e Formatos

- **Caminho**: `dados/gold/`
- **Formatos disponíveis**:
  - **Parquet** (`movies_final.parquet`): Formato otimizado com compressão, preservação de tipos e melhor performance para leitura
  - **CSV** (`movies_final.csv`): Formato legível e compatível para backup e referência

#### 4.5.2 Características da Camada Gold

- **Qualidade garantida**: Dados completamente limpos e validados
- **Prontos para consumo**: Formatados e estruturados para análises diretas
- **Eficiência**: Formato Parquet oferece compressão significativa comparado ao CSV
- **Rastreabilidade**: Versão final e definitiva dos dados processados

#### 4.5.3 Uso dos Dados da Camada Gold

Os dados da camada Gold são utilizados para:
- **Análises exploratórias**: Visualizações e insights (notebook `04_visualizations.ipynb`)
- **Modelagem**: Treinamento de modelos de machine learning (próximas etapas)
- **Dashboards**: Construção de dashboards interativos
- **Relatórios**: Geração de relatórios e apresentações

---

### 4.6 Arquitetura da Solução

#### 4.6.1 Estrutura de Camadas (Medallion Architecture)

```
dados/
├── raw/                    # Camada Bronze (dados brutos)
│   ├── movies_metadata.csv
│   └── credits.csv
│
├── processed/              # Camada Silver (dados transformados)
│   └── movies_full_cleaned.csv
│
└── gold/                   # Camada Gold (destino final)
    ├── movies_final.parquet
    └── movies_final.csv
```

**Camada Bronze (Raw)**:
- Dados originais sem modificações
- Preservação da integridade dos dados fonte
- Permite reprocessamento completo se necessário

**Camada Silver (Processed)**:
- Dados limpos e transformados
- Resultado das transformações e enriquecimentos
- Prontos para carregamento na camada final

**Camada Gold**:
- Dados validados e refinados
- Destino final do pipeline
- Prontos para consumo e análises
- Formatos otimizados (Parquet e CSV)

#### 4.6.2 Fluxo do Pipeline

```
┌─────────────┐
│   Kaggle    │
│  (Fonte)    │
└──────┬──────┘
       │ Download (BATCH) via kagglehub
       ▼
┌─────────────────┐
│  01_ingestion   │
│   (Notebook)    │
└──────┬──────────┘
       │ Salvamento
       ▼
┌──────────────┐
│ dados/raw/   │  ◄─── Camada Bronze
│ (Bronze)     │
└──────┬───────┘
       │ Carregamento
       ▼
┌──────────────────┐
│ 02_transformation│
│   (Notebook)     │
│                  │
│ • Limpeza        │
│ • Normalização   │
│ • Enriquecimento │
│ • Integração     │
└──────┬───────────┘
       │ Salvamento
       ▼
┌─────────────────────┐
│ dados/processed/    │  ◄─── Camada Silver
│ (Silver)            │
└──────┬──────────────┘
       │ Carregamento
       ▼
┌──────────────────┐
│ 03_loading       │
│   (Notebook)     │
│                  │
│ • Validações     │
│ • Qualidade      │
└──────┬───────────┘
       │ Salvamento
       ▼
┌─────────────────────┐
│ dados/gold/         │  ◄─── Camada Gold (Destino)
│ (Gold)              │
│                     │
│ • Parquet           │
│ • CSV               │
└──────┬──────────────┘
       │ Consumo
       ▼
┌──────────────────┐
│ 04_visualizations│
│   (Notebook)     │
│                  │
│ • Dashboards     │
│ • Insights       │
└──────────────────┘
```

#### 4.6.3 Tecnologias e Ferramentas

**Linguagem e Bibliotecas**:
- **Python 3.12+**: Linguagem base
- **pandas 2.3.3**: Manipulação de dados tabulares
- **numpy 2.3.3**: Operações numéricas
- **kagglehub 0.3.13**: Integração com Kaggle API
- **pyarrow**: Manipulação de arquivos Parquet (formato otimizado)
- **matplotlib 3.10.7**: Visualizações
- **seaborn 0.13.2**: Visualizações estatísticas avançadas

**Ambiente de Desenvolvimento**:
- **Jupyter Notebook**: Ambiente interativo para desenvolvimento e análise
- **VS Code**: Editor de código
- **Git / GitHub**: Controle de versão

**Infraestrutura**:
- Armazenamento local em sistema de arquivos
- Processamento em ambiente local (não distribuído nesta etapa)

---

### 4.7 Análises Exploratórias Realizadas

Durante o processo de transformação, foram realizadas análises exploratórias para orientar decisões de limpeza e validar a qualidade dos dados:

1. **Distribuições numéricas**: Histogramas para runtime, vote_average, vote_count e year
2. **Identificação de outliers**: Boxplots para todas as variáveis numéricas
3. **Correlações**: Matriz de correlação entre variáveis numéricas
4. **Análise de gêneros**: Top 10 gêneros mais frequentes
5. **Análise temporal**: Distribuição de filmes por década com notas médias

---

## 5. Equipe e Divisão de Tarefas

- **Eduardo Muniz Fontelles**: Ingestão, Camada Bronze e Ideação
- **Igor Amaral**: Transformação, Camada Silver e Ideação
- **Luis Adolfo**: Documentação e Ideação

---

## 6. Estado Atual do Projeto

- ✅ **Ingestão**: Finalizada
- ✅ **Transformação**: Finalizada
- ✅ **Carregamento**: Finalizada
- ✅ **Visualizações**: Finalizada
- ⏳ **Modelagem**: Pendente (próximas etapas)

---

## 7. Estrutura do Repositório

```
BigData/
├── dados/
│   ├── raw/              # Dados brutos (Camada Bronze)
│   │   ├── movies_metadata.csv
│   │   └── credits.csv
│   ├── processed/        # Dados transformados (Camada Silver)
│   │   └── movies_full_cleaned.csv
│   └── gold/             # Dados finais (Camada Gold - Destino)
│       ├── movies_final.parquet
│       └── movies_final.csv
│
├── src/
│   ├── 01_ingestion.ipynb      # Notebook de ingestão
│   ├── 02_transformation.ipynb # Notebook de transformação
│   ├── 03_loading.ipynb        # Notebook de carregamento
│   └── 04_visualiations.ipynb  # Notebook de visualizações
│
├── documentacao/
│   ├── architecture.pdf        # Documento de arquitetura
│   └── slidesAV1.pdf          # Apresentação AV1
│
├── requirements.txt      # Dependências do projeto
└── README.md            # Este arquivo
```

---

## 8. Como Utilizar o Projeto

### 8.1 Pré-requisitos

- Python 3.12 ou superior
- Jupyter Notebook ou JupyterLab
- Conta no Kaggle (para download do dataset)

### 8.2 Instalação

1. Clone o repositório:
```bash
git clone <url-do-repositório>
cd BigData
```

2. Instale as dependências:
```bash
pip install -r requirements.txt
```

3. Configure as credenciais do Kaggle (se necessário):
- Baixe o arquivo `kaggle.json` da sua conta
- Coloque em `~/.kaggle/kaggle.json`

### 8.3 Execução do Pipeline

Execute os notebooks na seguinte ordem:

1. **Ingestão**: Execute o notebook `src/01_ingestion.ipynb`
   - Faz download automático do dataset do Kaggle
   - Salva os dados brutos em `dados/raw/` (Camada Bronze)

2. **Transformação**: Execute o notebook `src/02_transformation.ipynb`
   - Carrega os dados brutos da Bronze
   - Aplica todas as transformações e limpezas
   - Salva os dados processados em `dados/processed/` (Camada Silver)

3. **Carregamento**: Execute o notebook `src/03_loading.ipynb`
   - Carrega os dados da Silver
   - Aplica validações finais
   - Salva os dados finais em `dados/gold/` (Camada Gold)

4. **Visualizações**: Execute o notebook `src/04_visualiations.ipynb`
   - Carrega os dados da Gold
   - Gera visualizações e análises exploratórias
   - Apresenta insights e dashboards

---

## 9. Resultados e Visualizações

### 9.1 Dataset Final

O pipeline processou com sucesso um total de **8.927 filmes**, após todas as etapas de limpeza e validação. O dataset final da camada Gold contém 11 atributos principais:

- Informações básicas: `id`, `title`, `year`, `original_language`
- Características: `runtime`, `genre_name`, `director`, `main_actor`
- Métricas: `vote_average`, `vote_count`

### 9.2 Visualizações Geradas

O notebook `04_visualiations.ipynb` gera **8 visualizações principais** que fornecem insights valiosos sobre o dataset:

#### 9.2.1 Distribuição de Notas (vote_average)

- **Histograma e Boxplot** da distribuição de notas dos filmes
- Revela que a maioria dos filmes possui notas entre 6.0 e 7.5
- Identifica outliers e distribuição normal dos dados

#### 9.2.2 Top 10 Gêneros Mais Comuns

- Gráfico de barras horizontal mostrando os gêneros mais representados
- **Drama** e **Comedy** são os gêneros dominantes
- Permite identificar tendências de produção cinematográfica

#### 9.2.3 Filmes por Década

- Análise temporal da produção cinematográfica
- Gráfico de barras mostrando quantidade de filmes por década
- Gráfico de linha mostrando evolução da nota média ao longo das décadas
- Revela tendências históricas e períodos de maior/menor qualidade

#### 9.2.4 Matriz de Correlação

- Heatmap mostrando correlações entre variáveis numéricas
- Identifica relações entre runtime, vote_average, vote_count e year
- Facilita identificação de variáveis importantes para modelagem

#### 9.2.5 Distribuição de Runtime (Duração)

- Histograma e Boxplot da duração dos filmes
- Mostra que a maioria dos filmes tem entre 90-110 minutos
- Identifica outliers e padrões de duração

#### 9.2.6 Top 10 Diretores com Mais Filmes

- Gráfico de barras horizontal
- Mostra diretores mais prolíficos e suas respectivas notas médias
- Permite identificar diretores produtivos e com bom desempenho

#### 9.2.7 Relação entre Runtime e Nota Média

- Scatter plot com linha de tendência
- Explora se há relação entre duração do filme e sua avaliação
- Cores indicam número de votos (densidade de dados)

#### 9.2.8 Top 10 Idiomas Mais Comuns

- Gráfico de barras vertical
- Mostra diversidade linguística do dataset
- Inglês domina significativamente, seguido por outros idiomas principais

### 9.3 Insights Principais

Através das análises realizadas, foram identificados os seguintes insights:

1. **Características Gerais**:
   - Dataset abrange mais de 100 anos de cinema (desde início do cinema até produções recentes)
   - Nota média geral dos filmes analisados está em torno de 6.5/10
   - Duração média de aproximadamente 100 minutos

2. **Gêneros**:
   - Drama e Comedy são os gêneros mais frequentes
   - Filmes frequentemente pertencem a múltiplos gêneros simultaneamente

3. **Tendências Temporais**:
   - Produção cinematográfica aumentou significativamente nas décadas recentes
   - Décadas diferentes apresentam padrões distintos de qualidade média

4. **Diversidade**:
   - Dominância de produções em inglês, mas com presença significativa de outras línguas
   - Diversidade de diretores e atores principais

5. **Correlações**:
   - Relação entre número de votos e nota média (filmes com mais votos tendem a ter notas mais estáveis)
   - Correlações fracas entre runtime e nota média (duração não determina qualidade)

### 9.4 Dashboards e Análises Interativas

O notebook de visualizações pode ser facilmente executado para gerar dashboards interativos e atualizados. Os gráficos são produzidos dinamicamente a partir dos dados da camada Gold, garantindo sempre informações atualizadas.

---

## 10. Conclusões

### 10.1 Objetivos Alcançados

O projeto alcançou com sucesso todos os objetivos propostos para esta primeira etapa:

✅ **Pipeline Completo**: Foi desenvolvido um pipeline de dados robusto e bem documentado, cobrindo todas as etapas desde a ingestão até o destino final.

✅ **Arquitetura em Camadas**: Implementação bem-sucedida da arquitetura Medallion (Bronze, Silver, Gold), permitindo rastreabilidade e reprocessamento quando necessário.

✅ **Qualidade de Dados**: Dados finais completamente limpos, validados e prontos para análises e modelagem, com remoção de outliers, tratamento de valores ausentes e normalização de tipos.

✅ **Documentação Completa**: README robusto seguindo padrões ABNT, com descrição detalhada de cada etapa, tecnologias e processos.

✅ **Visualizações e Insights**: Análises exploratórias abrangentes que revelam padrões importantes nos dados.

### 10.2 Dificuldades Encontradas

Durante o desenvolvimento do projeto, algumas dificuldades foram identificadas e superadas:

1. **Volume de Dados**: O dataset original com ~45 mil filmes exigiu processamento cuidadoso e otimização de operações para garantir eficiência.

2. **Qualidade dos Dados Originais**: 
   - Alta taxa de valores ausentes em algumas colunas (budget, revenue)
   - Dados inconsistentes (IDs inválidos, tipos mistos)
   - Necessidade de decisões sobre quais dados manter ou descartar

3. **Complexidade dos Dados Aninhados**: 
   - Processamento de estruturas JSON aninhadas (gêneros, elenco)
   - Extração de informações relevantes de estruturas complexas

4. **Filtragem Balanceada**: 
   - Encontrar equilíbrio entre manter dados suficientes e garantir qualidade
   - Decisões sobre critérios de filtragem (ex: mínimo de votos)

5. **Coordenação da Equipe**: 
   - Alinhamento entre diferentes membros trabalhando em etapas distintas do pipeline
   - Garantir consistência na estrutura e documentação

### 10.3 Lições Aprendidas

1. **Importância da Arquitetura em Camadas**: A separação entre Bronze, Silver e Gold facilitou muito o debug, reprocessamento e manutenção do pipeline.

2. **Validação Contínua**: Validações em múltiplas etapas do pipeline evitaram propagação de erros e garantiram qualidade final.

3. **Documentação desde o Início**: Manter documentação atualizada durante o desenvolvimento economizou tempo e facilitou a revisão final.

4. **Análise Exploratória**: As análises exploratórias durante a transformação foram fundamentais para tomar decisões informadas sobre limpeza e filtragem.

5. **Formato Parquet**: O uso do formato Parquet demonstrou significativa vantagem em termos de compressão e performance.

### 10.4 Trabalhos Futuros

Para próximas etapas do projeto, as seguintes expansões são recomendadas:

1. **Modelagem Preditiva**:
   - Desenvolvimento de modelos de machine learning para prever sucesso de filmes
   - Teste de diferentes algoritmos (Regressão, Random Forest, XGBoost, Neural Networks)
   - Features engineering avançado (one-hot encoding de gêneros, encoding de diretores/atores)

2. **Expansão do Dataset**:
   - Incorporação de dados adicionais (ratings detalhados, dados de bilheteria, redes sociais)
   - Integração de APIs para dados em tempo real
   - Análise de dados de streaming e plataformas digitais

3. **Streaming e Atualização Contínua**:
   - Implementação de pipeline de streaming para atualização em tempo real
   - Jobs agendados para ingestão periódica de novos dados

4. **Dashboards Interativos**:
   - Desenvolvimento de dashboard web interativo usando Streamlit ou Dash
   - Visualizações dinâmicas com filtros e drill-down
   - Deploy do dashboard para acesso público

5. **Análises Avançadas**:
   - Análise de sentimentos de críticas de filmes
   - Análise de redes sociais (relacionamento entre diretores, atores, gêneros)
   - Clustering de filmes similares
   - Sistema de recomendação baseado em conteúdo

6. **Infraestrutura**:
   - Migração para ambiente distribuído (Spark, Dask)
   - Utilização de cloud computing (AWS, GCP, Azure)
   - Implementação de Data Lake para armazenamento escalável

7. **Deploy e Produção**:
   - Automação completa do pipeline (Airflow, Prefect)
   - Monitoramento e alertas de qualidade de dados
   - Versionamento de modelos e dados

### 10.5 Considerações Finais

Este projeto demonstrou a viabilidade e importância de pipelines de dados bem estruturados para análise de Big Data. A arquitetura implementada fornece uma base sólida para expansões futuras, e os dados processados estão prontos para análises avançadas e modelagem preditiva.

O pipeline desenvolvido não apenas processa dados, mas garante qualidade, rastreabilidade e reprodutibilidade - aspectos fundamentais para projetos de ciência de dados profissionais. A documentação detalhada facilita a manutenção, evolução e colaboração contínua no projeto.

---

## 11. Referências

- **Dataset**: [The Movies Dataset - Kaggle](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset)
- **Biblioteca KaggleHub**: [kagglehub Documentation](https://github.com/Kaggle/kagglehub)

---

## 12. Licença

Este projeto é desenvolvido para fins acadêmicos como parte da disciplina de Big Data.

---

*Documento atualizado conforme estrutura ABNT para relatórios técnicos.*
