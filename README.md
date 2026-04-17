# Projeto Integrador I — ANÁLISE DAS NOTIFICAÇÕES DE ESTUPRO E ESTUPRO DE VULNERÁVEL EM SANTOS: UMA ABORDAGEM DE CIÊNCIA DE DADOS SOBRE A DINÂMICA DOS CASOS DE 2022 A 2025.

Estudo derivado de um projeto acadêmico da disciplina Projeto Integrador I do curso de Ciência de Dados da Fatec Baixada Santista "Rubem Lara". O estudo teve como objetivo analisar a relação entre as ocorrências de estupro (Art. 213 do Código Penal) e estupro de vulnerável (Art. 217-A do Código Penal) e suas notificações nos bairros de Santos - SP, durante o período de 2022 a 2025.

---

## Motivação

A violência sexual, especialmente o estupro, é um problema crítico no Brasil e um direito fundamental violado. Entender suas variações espaciais e temporais é fundamental para a formulação de políticas públicas de prevenção e combate a esses crimes. Este projeto aplica técnicas de Ciência de Dados para visualizar e analisar o índice de vulnerabilidade pública aos crimes de estupro no município de Santos, buscando fornecer subsídios para políticas de segurança mais eficazes e direcionadas.

---

## Estrutura do Projeto

```
projeto-integrador-I/
├── data/                                    ← dados brutos e processados
│   ├── raw/                                 ← dados originais da SSP-SP (por semestre e ano)
│   │   ├── 1st_2022.xlsx                    ← 1º semestre 2022
│   │   ├── 2nd_2022.xlsx                    ← 2º semestre 2022
│   │   ├── 1st_2023.xlsx                    ← 1º semestre 2023
│   │   ├── 2nd_2023.xlsx                    ← 2º semestre 2023
│   │   ├── 1st_2024.xlsx                    ← 1º semestre 2024
│   │   ├── 2nd_2024.xlsx                    ← 2º semestre 2024
│   │   ├── 1st_2025.xlsx                    ← 1º semestre 2025
│   │   ├── 2nd_2025.xlsx                    ← 2º semestre 2025
│   │   ├── SPDadosCriminais_2022.xlsx       ← dados anuais 2022
│   │   ├── SPDadosCriminais_2023.xlsx       ← dados anuais 2023
│   │   ├── SPDadosCriminais_2024.xlsx       ← dados anuais 2024
│   │   └── SPDadosCriminais_2025.xlsx       ← dados anuais 2025
│   │
│   ├── processed/                           ← dados após ETL (limpos e tratados)
│   │   ├── df.csv                           ← dataframe consolidado após conversão
│   │   ├── df_merged.csv                    ← dataframe após junção semestral
│   │   ├── df_cleaned.csv                   ← dataframe após limpeza e tratamento
│   │   ├── delta_table.csv                  ← tabela de frequência de intervalos de dias
│   │   ├── freq_table.xlsx                  ← tabela de frequência geral
│   │   ├── week_table.xlsx                  ← tabela de frequência da primeira semana
│   │   └── data_dict.xlsx                   ← dicionário de dados com descrição das colunas
│
├── notebooks/                               ← jupyter notebooks do pipeline ETL
│   ├── 1_conversao.ipynb                    ← conversão xlsx para csv após extração
│   ├── 2_juncao.ipynb                       ← junção dos dados semestrais
│   ├── 3_limpeza.ipynb                      ← limpeza e remoção de colunas irrelevantes
│   ├── 4_tratamento.ipynb                   ← tratamento de valores faltantes e padronização
│   ├── 5_delta_freq.ipynb                   ← cálculo de intervalos e tabelas de frequência
│   └── 6_graficos.ipynb                     ← geração de visualizações e gráficos
│
├── plots/                                   ← figuras e gráficos gerados em PNG
│   ├── por_qualificacao.png                 ← distribuição por qualificação jurídica
│   ├── temporal.png                         ← distribuição temporal (por ano e mês)
│   ├── por_mes.png                          ← distribuição por meses do ano
│   ├── por_local.png                        ← distribuição por tipo de local
│   ├── por_bairro.png                       ← distribuição por bairro e macrozona
│   └── por_periodo.png                      ← distribuição por período do dia
│
├── docs/                                    ← documentação do projeto
│   └── [documentação adicional]
│
├── README.md                                ← este arquivo
├── requirements.txt                         ← dependências de execução
└── [arquivos de configuração]
```

---

## Principais Insights

* **322 notificações** registradas no período de 2022 a 2025
* **72,4%** dos casos são estupro de vulnerável (Art. 217-A), indicando alta incidência contra crianças e pessoas vulneráveis
* **63,38%** das ocorrências acontecem em ambientes residenciais (casa, residência, apartamento)
* **Macrozona Leste** concentra 40,99% das notificações, região considerada nobre da cidade
* **Padrão sazonal** identificado com picos em maio, julho, outubro, novembro e dezembro
* **Intervalo médio de 334 dias** entre ocorrência e notificação, porém 67,76% são notificadas em até 13 dias
* **69,6%** das notificações ocorrem nos primeiros dois dias, alinhado com protocolos de saúde pública

---

## Metodologia

### Processo ETL

O estudo utilizou o processo estruturado de **ETL (Extração, Transformação e Carga de Dados)** para refinar os dados brutos e extrair informações significativas:

#### Extração

* **Fonte**: Portal da Transparência da Secretaria de Segurança Pública do Estado de São Paulo (SSP-SP)
* **Período**: 2022, 2023, 2024 e 2025
* **Formato original**: xlsx (SP Dados - RES 160 e RES 516)

#### Transformação

* Conversão de formato xlsx para csv
* Padronização de nomenclaturas inconsistentes entre anos
* Filtragem de registros por artigo jurídico (213 e 217-A) e município (Santos)
* Tratamento de valores ausentes em campos temporais
* Padronização de bairros conforme Plano Diretor da cidade
* Criação de campos derivados: período do dia, dia da semana, intervalo de dias
* Unificação de campos de hora e período

#### Carga

* Armazenamento em formato CSV processado
* Estrutura pronta para análises estatísticas e visualizações

### Ferramentas Utilizadas

* **Python 3**: linguagem principal para processamento e análise
* **Pandas**: manipulação e limpeza de dados
* **NumPy**: operações numéricas
* **Matplotlib**: criação de visualizações estáticas
* **Jupyter Notebook**: exploração interativa dos dados

---

## Como Executar

O projeto foi montado pensando em possíveis modificações para estudos futuros, portanto, caso tenha alguma nova ideia que contribua para extensões do estudo, clone o projeto e modifique-o como quiser.

**Observação:**  O projeto foi escrito no colab para ter acesso facilitado aos dados no drive e às dependencias externas utilizadas sem a necessidade de baixá-las localmente. No entanto, adicionamos um `requirements.txt` listando todas as biliotecas externas utilizadas para possibilitar a replicabilidade do estudo em máquinas locais. Portanto, se deseja replicar o projeto, retire as células de conexão ao drive e altere os caminhos dos dados (nos imports e exports) antes de executar.

### Requisitos

* Python 3.8+
* Jupyter Notebook
* Dependências listadas em `requirements.txt`

### Instalação

```bash
# clonar o repositório
git clone https://github.com/seu-usuario/projeto-integrador-I.git
cd projeto-integrador-I

# criar ambiente virtual (opcional, mas recomendado)
python -m venv venv
source venv/bin/activate  # no Windows: venv\Scripts\activate

# instalar dependências
pip install -r requirements.txt
```

### Pipeline de Processamento

O projeto utiliza notebooks Jupyter organizados sequencialmente para o processamento ETL. Execute nesta ordem:

```bash
# iniciar Jupyter Notebook
jupyter notebook

# no navegador, abra e execute os notebooks na sequência:
```

1. **`1_conversao.ipynb`** — Conversão dos arquivos XLSX originais para CSV
   * Lê dados brutos dos arquivos semestrais e anuais
   * Converte para formato CSV para processamento
2. **`2_juncao.ipynb`** — Junção dos dados semestrais
   * Consolida os dados dos dois semestres de cada ano
   * Gera `df_merged.csv`
3. **`3_limpeza.ipynb`** — Limpeza e seleção de colunas
   * Remove colunas irrelevantes
   * Filtra apenas casos de Art. 213 e Art. 217-A
   * Filtra apenas registros de Santos
   * Gera `df.csv`
4. **`4_tratamento.ipynb`** — Tratamento e padronização de dados
   * Trata valores ausentes em campos temporais
   * Padroniza nomenclaturas de bairros conforme Plano Diretor
   * Converte campos textuais para formato padrão
   * Cria campos derivados (período do dia, dia da semana, etc.)
   * Gera `df_cleaned.csv`
5. **`5_delta_freq.ipynb`** — Cálculo de intervalos e tabelas de frequência
   * Calcula intervalo de dias entre ocorrência e notificação
   * Gera tabelas de frequência para análise estatística
   * Cria `delta_table.csv`, `freq_table.xlsx` e `week_table.xlsx`
6. **`6_graficos.ipynb`** — Geração de visualizações
   * Cria gráficos exploratórios
   * Exporta imagens PNG para a pasta `plots/`
   * Gera 6 gráficos principais (qualificação, temporal, meses, local, bairro, período)

---

 **Nota**: Os notebooks devem ser executados **em ordem sequencial**, pois cada um depende da saída do anterior.

---

## Dados Utilizados

**Fonte** : [Portal da Transparência - Secretaria de Segurança Pública de São Paulo](https://www.ssp.sp.gov.br/estatistica/consultas)[
](https://www.ssp.sp.gov.br/estatistica/consultas)**Período** : 2022 - 2025
**Registros** : 322 notificações de estupro e estupro de vulnerável em Santos
**Cobertura Geográfica** : Município de Santos, SP (59 bairros, divididos em Macroáreas Continental e Insular)

### Estrutura dos Dados Brutos

Os dados brutos estão organizados em `data/raw/` de duas formas:

* **Por semestre** : `1st_YYYY.xlsx` e `2nd_YYYY.xlsx` (1º e 2º semestres de cada ano)
* **Anuais** : `SPDadosCriminais_YYYY.xlsx` (dados consolidados por ano)

### Variáveis Principais Utilizadas

Após o processamento ETL, as principais variáveis retidas e criadas (por meio de *feature engineering*) são:

| Variável               | Descrição                                 | Valores Possíveis                           |
| ----------------------- | ------------------------------------------- | -------------------------------------------- |
| `RUBRICA`             | Classificação jurídica do crime          | Art. 213, Art. 217-A, Ato Infracional        |
| `DATA_OCORRENCIA_BO`  | Data em que o crime ocorreu                 | YYYY-MM-DD                                   |
| `DATA_COMUNICACAO_BO` | Data em que foi notificado                  | YYYY-MM-DD                                   |
| `NOME_MUNICIPIO`      | Município onde ocorreu                     | Santos (filtrado)                            |
| `NOME_BAIRRO`         | Bairro onde ocorreu                         | 59 bairros de Santos                         |
| `DESC_SUBTIPOLOCAL`   | Tipo de local de ocorrência                | Casa, Residência, Apartamento, etc.         |
| `HORA_OCORRENCIA_BO`  | Hora do crime                               | HH:MM:SS ou incerta                          |
| `DESC_PERIODO`        | Período do dia                             | Madrugada, Manhã, Tarde, Noite              |
| `MES_ESTATISTICA`     | Mês da notificação                       | 1-12                                         |
| `DIFERENÇA_DIAS`     | Intervalo entre ocorrência e notificação | Dias (0 a 7057)                              |
| `DIA_SEMANA`          | Dia da semana da ocorrência                | Segunda a Domingo                            |
| `MACROZONA`           | Zona geográfica do bairro                  | Leste, Noroeste, Centro, Morros, Continental |

### Arquivos de Dados Processados

Após a execução do pipeline ETL, os seguintes arquivos são gerados em `data/processed/`:

| Arquivo             | Origem     | Descrição                                               |
| ------------------- | ---------- | --------------------------------------------------------- |
| `df.csv`          | Notebook 3 | Dados após filtragem (apenas Art. 213 e 217-A em Santos) |
| `df_merged.csv`   | Notebook 2 | Consolidação dos dados semestrais por ano               |
| `df_cleaned.csv`  | Notebook 4 | Dados após limpeza completa e padronização             |
| `delta_table.csv` | Notebook 5 | Tabela de frequência de intervalos de dias               |
| `freq_table.xlsx` | Notebook 5 | Tabela de frequência estatística geral                  |
| `week_table.xlsx` | Notebook 5 | Tabela de frequência dos primeiros 7 dias                |
| `data_dict.xlsx`  | Manual     | Dicionário com descrição de todas as colunas           |

 **Nota** : O arquivo `df_cleaned.csv` é o dataset final utilizado para geração de gráficos.

### Transformações Aplicadas

#### Notebook 1 - Conversão

* Leitura dos arquivos XLSX originais
* Conversão para CSV mantendo estrutura original

#### Notebook 2 - Junção

* Consolidação de dados dos 1ºs e 2ºs semestres
* Criação de dataset unificado por ano
* Tratamento de duplicatas

#### Notebook 3 - Limpeza

* Filtragem por artigo jurídico (213 e 217-A)
* Filtragem por município (Santos)
* Remoção de 21 colunas irrelevantes
* Retenção de 13 colunas principais

#### Notebook 4 - Tratamento

* **Valores ausentes** : Unificação de campos de hora e período
* **Padronização** : Conversão de texto para maiúsculas
* **Bairros** : Renomeação conforme Lei Complementar 1.187/2022
* **Datas** : Tratamento de inconsistências e formatação
* **Campos derivados** : Criação de período do dia, dia da semana, intervalo de dias

#### Notebook 5 - Delta e Frequência

* Cálculo do intervalo em dias entre ocorrência e notificação
* Criação de tabelas de frequência com intervalos
* Análise específica da primeira semana (0-7 dias)

#### Notebook 6 - Gráficos

* Exportação em formato PNG
* Uso de Matplotlib para visualizações
* Cores padronizadas por Macrozona

---

## Considerações e Limitações

* **Subnotificação**: Alto índice de casos com hora incerta (81 ocorrências - 25,1%) indica possíveis problemas na coleta de dados ou consequência da natureza sensível dos dados
* **Dados faltantes**: Alguns registros apresentam inconsistências de nomenclatura entre anos
* **Período limitado**: Apenas 4 anos de dados não são suficientes para estabelecer padrões robustos, apenas tendências
* **Dados brutos**: A qualidade dos dados originais reflete diretamente na qualidade das análises

---

## Implicações Políticas e Sociais

Este estudo indica a necessidade urgente de:

1. **Políticas de Segurança Direcionadas**: Maior atenção a bairros de maior incidência, especialmente Macrozona Leste
2. **Proteção do Ambiente Doméstico**: Campanhas de conscientização sobre riscos em ambientes residenciais
3. **Proteção a Vulneráveis**: Programas específicos de proteção a crianças e pessoas com deficiência mental
4. **Melhoria nos Registros**: Padronização de dados pela SSP-SP para facilitar futuras análises
5. **Atendimento Oportuno em Saúde**: Garantir acesso ao atendimento em saúde dentro das primeiras 72 horas

---

## Referências

* **BRASIL.** Código Penal - Decreto-lei nº 2.848, de 7 de dezembro de 1940.
* **BRASIL.** Constituição da República Federativa do Brasil de 1988.
* **BRASIL.** Lei nº 8.069 - Estatuto da Criança e do Adolescente, 1990.
* **BRASIL.** Lei nº 10.778 - Notificação compulsória de violência contra a mulher, 2003.
* **BRASIL.** Ministério da Saúde. Protocolo de Profilaxia Pós-Exposição (PEP), 2021.
* **FEBRASGO.** Federação Brasileira das Associações de Ginecologia e Obstetrícia - Contracepção de Emergência, 2018.
* **FÓRUM BRASILEIRO DE SEGURANÇA PÚBLICA.** 18º Anuário Brasileiro de Segurança Pública, 2024.
* **HOOKS, B.** Tudo Sobre o Amor: Novas Perspectivas. Editora Elefante, 2021.
* **IPEA.** Estupro no Brasil: uma radiografia segundo os dados da Saúde, 2014.
* **KIMBALL, R.; CASERTA, J.** The Data Warehouse ETL Toolkit. Wiley Publishing, 2004.
* **MACHADO, L. Z.** Masculinidade, Sexualidade e Estupro: as Construções da Virilidade. Cadernos Pagu, 2013.
* **PLATT, V. B. et al.** Violência Sexual contra Crianças. Ciência & Saúde Coletiva, v. 23, n. 4, 2018.
* **SÃO PAULO (Estado).** Lei Complementar nº 1.187 - 3º Plano Diretor de Santos, 2022.
* **WAISELFISZ, J. J.** Mapa da Violência 2015: Homicídio de Mulheres no Brasil. FLACSO, 2015.

---

## Autores

**Felipe Alves Gonçalves** — Discente, Ciência de Dados, FATEC Baixada Santista "Rubens Lara"
**Kauã Santana da Silva** — Discente, Ciência de Dados, FATEC Baixada Santista "Rubens Lara"

---

## Contato e Suporte

Para dúvidas sobre o projeto, entre em contato com os autores através de:

* **Felipe Alves Gonçalves**: [felipe.goncalves4@aluno.cps.sp.gov.br](mailto:felipe.goncalves4@aluno.cps.sp.gov.br)
* **Kauã Santana da Silva**: [kaua.silva22@aluno.cps.sp.gov.br](mailto:kaua.silva22@aluno.cps.sp.gov.br)

---

## Agradecimentos

* Professora Ma. Cláudia Maria Sodero Salles, pela orientação do projeto (que se estendeu mais do que esperávamos) e pela paciência
* Secretaria de Segurança Pública do Estado de São Paulo pela disponibilização dos dados
* FATEC Baixada Santista Rubens Lara e todos os docentes envolvidos
