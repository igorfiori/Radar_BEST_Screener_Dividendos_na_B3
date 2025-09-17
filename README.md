from pathlib import Path

# Conteúdo do arquivo README.md
markdown_content = """# Radar BEST+
### Um Screener Quantitativo para Descoberta de Ações na B3

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Libraries](https://img.shields.io/badge/Libraries-Pandas%20%7C%20yfinance%20%7C%20Requests-orange)
![License](https://img.shields.io/badge/License-MIT-green.svg)

Este repositório contém o código-fonte e a documentação do **Radar BEST+**, uma ferramenta de análise de dados desenvolvida em Python para automatizar a descoberta de ações na bolsa de valores brasileira (B3) com base na filosofia de investimentos de Luiz Barsi.

---

## Sumário

* [Resumo do Projeto](#resumo-do-projeto)  
* [Metodologia: O Funil BEST+](#metodologia-o-funil-best)  
  * [Fase 1: Filtro de Setor](#fase-1-filtro-de-setor)  
  * [Fase 2: Os 5 Critérios do BEST+ Score](#fase-2-os-5-critérios-do-best-score)  
* [Tecnologias e Fontes de Dados](#tecnologias-e-fontes-de-dados)  
* [Como Usar](#como-usar)  
* [Dicionário do Projeto](#dicionário-do-projeto)  
  * [Parâmetros Globais](#parâmetros-globais)  
  * [Estrutura do Resultado Final](#estrutura-do-resultado-final)  
* [Limitações e Próximos Passos](#limitações-e-próximos-passos)  
* [Autor](#autor)  
* [Licença](#licença)  

---

## Resumo do Projeto

Este projeto traduz a filosofia de investimentos "BEST" de Luiz Barsi em um modelo quantitativo funcional. O **Radar BEST+** é um *screener* que varre todo o mercado da B3, identifica empresas em setores perenes e as classifica com uma pontuação de 0 a 5 ("BEST+ Score") com base em critérios fundamentalistas e no cálculo do Preço-Teto.

O objetivo é fornecer uma ferramenta que automatize o trabalho pesado de prospecção, removendo o viés emocional e revelando uma lista curta de ativos que merecem uma análise qualitativa mais profunda.

---

## Metodologia: O Funil BEST+

O processo funciona como um funil, começando com todo o universo de ações da B3 e aplicando uma série de filtros e regras para revelar, ao final, uma pequena lista de empresas com alto potencial.

### Fase 1: Filtro de Setor

A primeira e mais importante regra é restringir a análise apenas aos setores considerados perenes e resilientes.

* **A Regra:** A empresa deve pertencer a um dos seguintes setores:  
  `Financial Services` (Bancos e Seguradoras), `Utilities` (Energia Elétrica e Saneamento) ou `Communication Services` (Telecomunicações).  
* **O Porquê:** Foco em setores que oferecem serviços essenciais, garantindo previsibilidade de receita e lucros que sustentam a distribuição de dividendos a longo prazo.

### Fase 2: Os 5 Critérios do BEST+ Score

As empresas que sobrevivem ao filtro de setor passam por uma avaliação de 5 critérios. Cada critério atendido concede **1 ponto**.

| Critério | A Regra | O Porquê (A Lógica Barsi) |
| :--- | :--- | :--- |
| **1. Dividend Yield (DY)** | DY dos últimos 12 meses > **6%** | Estabelece o piso de rentabilidade anual apenas com dividendos. |
| **2. Saúde Financeira** | Dívida / Patrimônio Líquido < **2.5** | Garante que a empresa não está excessivamente alavancada, protegendo a sustentabilidade dos dividendos. |
| **3. Lucratividade** | Margem de Lucro > **0%** | Uma empresa só pode pagar dividendos de forma sustentável se gera lucro. É a verificação mais básica de saúde operacional. |
| **4. Valuation (P/L)** | 0 < Preço/Lucro < **15** | Evita pagar caro demais pelos lucros da empresa, garantindo a compra de bons ativos a preços justos. |
| **5. Margem de Segurança** | Preço Atual < **Preço-Teto** | O critério de compra mais famoso de Barsi. Garante que o preço pago tem alta probabilidade de gerar o yield desejado com base na média histórica de dividendos de 5 anos. |

---

## Tecnologias e Fontes de Dados

* **Linguagem:** Python 3  
* **Bibliotecas Principais:**  
  * `pandas`: Para toda a estruturação, manipulação e exibição dos dados.  
  * `yfinance`: Para a coleta de dados fundamentalistas e histórico de dividendos.  
  * `requests`: Para o consumo da Bravo API.  
* **Fontes de Dados:**  
  * **Bravo API:** Fonte primária para a lista completa de tickers da B3.  
  * **Yahoo Finanças:** Fonte primária para os indicadores e preços dos ativos.  

---

## Como Usar

### 1. Clone o Repositório
```bash
git clone https://github.com/seu-usuario/radar-best.git
cd radar-best
``` 

### 2. Crie o Ambiente Virtual

python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\\Scripts\\activate      # Windows

### 3. Instale as Dependências
Crie um arquivo requirements.txt com o conteúdo abaixo:

pandas
yfinance
requests

E execute:

```bash
pip install -r requirements.txt
```

### 4. Execute o Notebook

```bash
jupyter notebook screener_barsi.ipynb
```

Execute as células em ordem. O processo pode levar alguns minutos, pois analisa centenas de ativos.

5. Verifique os Resultados

Ao final, será gerado o arquivo `ranking_radar_best_com_preco_teto.csv` na pasta data.

### Dicionário do Projeto
Parâmetros Globais

| Parâmetro                  | Descrição                                                                   |
| :------------------------- | :-------------------------------------------------------------------------- |
| `SETORES_BARSI`            | Define os setores de atuação que serão considerados na análise.             |
| `SCORE_MINIMO_PARA_EXIBIR` | Apenas empresas com score igual ou maior aparecem no ranking.               |
| `PAUSA_ENTRE_REQUISICOES`  | Tempo em segundos de espera entre cada ticker para evitar bloqueios da API. |

### Estrutura do Resultado Final

O CSV contém as seguintes colunas:

| Coluna              | Descrição                                                            |
| :------------------ | :------------------------------------------------------------------- |
| **Ticker**          | O código de negociação da ação na B3.                                |
| **Empresa**         | O nome da empresa.                                                   |
| **Setor**           | O setor de atuação da empresa.                                       |
| **BEST+ Score**     | A pontuação final (0 a 5).                                           |
| **Preço Atual**     | Cotação da ação no momento da análise.                               |
| **Preço-Teto**      | Preço máximo de compra calculado pela média de 5 anos de dividendos. |
| **Abaixo do Teto?** | Informa se o preço atual é menor que o preço-teto.                   |
| **DY (%)**          | Dividend Yield dos últimos 12 meses.                                 |
| **P/L**             | Índice Preço/Lucro.                                                  |
| **Dívida/PL**       | Relação entre Dívida e Patrimônio Líquido.                           |

### Limitações e Próximos Passos

Qualidade dos Dados: Depende de APIs gratuitas (yfinance, Brapi). O cálculo do Preço-Teto pode falhar em casos de histórico de dividendos incompleto. Uma API paga da B3 aumentaria a robustez.

Análise Quantitativa: Este screener é apenas um ponto de partida. A decisão final deve incluir análise qualitativa.

### Autor

Igor Fiori

### Licença

Este projeto é distribuído sob a licença MIT.
"""
