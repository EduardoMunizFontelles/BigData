# Projeto: Previsão de Sucesso de Filmes — Pipeline de Dados (AV1)

## 🧠 Descrição
Este projeto faz parte da Avaliação 1 (AV1) da disciplina de Big Data e tem como objetivo desenvolver um **pipeline de dados** para o setor de **Mídia e Entretenimento**, focado em **prever o sucesso de um filme antes de seu lançamento**.

O pipeline contempla as etapas de **ingestão**, **armazenamento** e **transformação** dos dados, utilizando o dataset *The Movies Dataset*, que reúne metadados de aproximadamente **45 mil filmes**, incluindo informações sobre elenco, diretores, gênero, orçamento, receita, popularidade etc.

O objetivo dessa primeira etapa é construir uma base estruturada e limpa que permita, nas próximas fases do projeto, treinar modelos preditivos capazes de estimar o **sucesso comercial (bilheteria)** ou **sucesso crítico (nota do público)** de novos filmes.

---

## 🎞️ Fonte dos Dados
- **Nome:** The Movies Dataset  
- **Disponibilidade:** Repositório público no [Kaggle](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset)  
- **Descrição:** Contém metadados de filmes listados em Full MovieLens Dataset, como:
  - Título, data de lançamento, idioma e país de origem  
  - Elenco e diretores  
  - Gêneros e palavras-chave  
  - Orçamento, receita e popularidade  
  - Avaliações de usuários

---

## 🧩 Estrutura do Pipeline
**Etapas previstas:**
1. **Ingestão:** coleta e leitura dos arquivos CSV originais do dataset.  
2. **Armazenamento:** organização dos dados em camadas (raw, cleaned, processed).  
3. **Transformação:** limpeza, tratamento de valores ausentes, normalização de colunas e integração entre tabelas (filmes, créditos, ratings etc.).

---

## 🛠️ Ferramentas Utilizadas
- **Python 3.12+**
- **Pandas / NumPy**
- **Jupyter Notebook**
- **Matplotlib / Seaborn**
- **Git / GitHub**
- **VS Code**

---

## 👥 Equipe
- Eduardo Muniz Fontelles
- Igor Amaral
- Luis Adolfo

---

## 📂 Estrutura do Repositório
/dados → amostras de dados brutos (.csv)
/src → scripts e notebooks de ingestão e transformação
/documentacao → diagramas, PDFs e relatórios (ex: Documento de Arquitetura)