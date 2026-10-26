# Dataset para Priorização de Requisitos de Software (Derivado do TAWOS)

Este repositório contém um dataset estruturado composto por **104.710 requisitos de software** provenientes de 17 projetos de código aberto. Os dados foram extraídos, filtrados, higienizados e rotulados em **três classes ordinais de prioridade**, constituindo um recurso para pesquisas em Processamento de Linguagem Natural (PLN) e Aprendizado de Máquina (AM) aplicados à Engenharia de Requisitos (ER).

---

## 📌 Origem dos Dados e Créditos

Este dataset é uma derivação curada do **TAWOS** (*Tawosi Agile Web-based Open-Source Dataset*), um banco de dados relacional público que reúne histórico de *issues* registradas na plataforma Jira.

* **Dataset Original:** TAWOS (Tawosi et al., 2022).


* **Repositório Oficial TAWOS:** [SOLAR-group/TAWOS](https://github.com/SOLAR-group/TAWOS.git).


* **Trabalho Derivado:** Abordagem Orientada a Dados para Priorização de Requisitos de Software com PLN e Transformers (Malage, 2026).



---

## ⚙️ Processo de Construção do Dataset

O processo de transformação da base bruta no dataset final seguiu as seguintes etapas:

```
[TAWOS DB (44 Projetos)] 
       │
       ▼
 1. Filtragem por Atributo de Prioridade (Descarte de Nulos e Severidades)[cite: 72, 73]
       │
       ▼
 2. Seleção Estatística (Remoção de Projetos com Amostragem Insuficiente)[cite: 77]
       │
       ▼
 3. Pré-Processamento e Limpeza Textual (Regex, Normalização e Deduplicação)[cite: 75, 78]
       │
       ▼
 4. Agrupamento e Ordinal Encoding das Classes de Prioridade[cite: 78]
       │
       ▼
[Dataset Consolidado: 17 Projetos / 104.710 Requisitos][cite: 77, 93, 95]

```

### 1. Extração e Filtragem Inicial

* Dos 44 projetos do TAWOS, apenas 34 continham dados preenchidos na coluna `Priority`.


* Projetos que utilizavam métricas de **severidade** (ex: *Blocker*, *Critical*, *Major*, *Minor*, *Trivial*) em vez de prioridades do Jira foram descartados.


* Registros sem descrição textual (`Description_Text`) ou sem prioridade definida foram removidos.



### 2. Seleção por Representatividade Estatística

* Projetos com amostragem extremamente reduzida (IDs 9, 10, 15, 23 e 29) foram eliminados para garantir consistência estatística nos treinamentos.


* Restaram **17 projetos de código aberto** de grande e médio porte.



### 3. Limpeza e Normalização Textual

* **Remoção de ruídos:** Eliminação de trechos de código, URLs, referências e caracteres especiais via Expressões Regulares (Regex).


* **Normalização:** Conversão dos textos para letras minúsculas.


* **Higienização:** Descarte de instâncias duplicadas e registros com atributos essenciais ausentes.



### 4. Mapeamento e Ordinal Encoding

Para mitigar o desbalanceamento acentuado das classes extremas do Jira (`Highest` e `Lowest`), as categorias foram agrupadas em um esquema hierárquico ordinal de 3 níveis:

| Prioridade Original (Jira) | Classe Agrupada | Rótulo Numérico | Descrição Semântica |
| --- | --- | --- | --- |
| `Highest` / `High` | **Alta** | `3` | Alta criticidade ou urgência de implementação

 |
| `Medium` | **Média** | `2` | Prioridade intermediária

 |
| `Low` / `Lowest` | **Baixa** | `1` | Baixa criticidade ou urgência

 |

---

## 📊 Estatísticas do Dataset Consolidado

O dataset resultante conta com **104.710 requisitos** consolidados. A distribuição global das classes apresenta a seguinte configuração:

| Classe de Prioridade | Rótulo Numérico | Quantidade de Requisitos | Percentual Global |
| --- | --- | --- | --- |
| **Baixa** | `1` | 42.442 | 40,53%

 |
| **Média** | `2` | 40.113 | 38,31%

 |
| **Alta** | `3` | 22.155 | 21,16%

 |
| **Total** | — | **104.710** | **100,00%**

 |

### Distribuição por Projeto

| ID Projeto | Nome do Projeto | Alta (`3`) | Média (`2`) | Baixa (`1`) | Total |
| --- | --- | --- | --- | --- | --- |
| **7** | Alloy Framework | 462 | 411 | 152 | 1.025

 |
| **8** | Aptana Studio | 699 | 2.683 | 2.790 | 6.172

 |
| **11** | Titanium Mobile Platform | 887 | 690 | 587 | 2.164

 |
| **12** | The Titanium SDK | 5.978 | 4.978 | 4.008 | 14.964

 |
| **13** | Appcelerator Studio | 2.200 | 1.647 | 876 | 4.723

 |
| **14** | Atlassian Bamboo | 622 | 1.609 | 3.328 | 5.559

 |
| **16** | Atlassian Crowd | 222 | 785 | 766 | 1.773

 |
| **17** | Atlassian Fish Eye | 261 | 887 | 1.517 | 2.665

 |
| **19** | Atlassian Software Cloud | 320 | 419 | 1.470 | 2.209

 |
| **20** | Atlassian Software Server | 784 | 1.631 | 2.742 | 5.157

 |
| **22** | Atlassian Confluence Server | 2.632 | 8.934 | 13.105 | 24.671

 |
| **24** | Hyperledger Fabric | 1.596 | 8.452 | 793 | 10.841

 |
| **25** | Hyperledger Indy Node | 432 | 1.454 | 133 | 2.019

 |
| **26** | Hyperledger Indy SDK | 142 | 995 | 56 | 1.193

 |
| **27** | Hyperledger Sawtooth | 96 | 926 | 61 | 1.083

 |
| **42** | DotNetNuke Platform | 4.043 | 2.114 | 3.244 | 9.401

 |
| **44** | Atlassian Jira Cloud | 779 | 1.498 | 6.814 | 9.091

 


Esquema dos Arquivos CSV

Cada arquivo CSV possui os seguintes campos:

1. `PROJETO`: Nome identificador do projeto no Jira (ex: `CONFSERVER`).
2. `PROJETO_ID`: Identificador numérico único do projeto no TAWOS.
3. `DESCRICAO_TEXTUAL`: Texto do requisito de software pré-processado (feature de entrada).
4. `PRIORIDADE_JIRA`: Rótulo original da prioridade (`Highest`, `High`, `Medium`, `Low`, `Lowest`).
5. `PRIORIDADE_NUMERICA`: Rótulo ordinal mapeado (`3` = Alta, `2` = Média, `1` = Baixa).


## 📜 Licença

Os dados originais pertencem ao dataset TAWOS e aos projetos *open-source* de onde foram minerados. Este dataset derivado é disponibilizado sob a licença Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**.
