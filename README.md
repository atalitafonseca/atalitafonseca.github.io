# 🏦 Framework de Analytics de Jornadas Digitais e Atribuição Incremental de Campanhas
### Estudo de Caso Aplicado: Otimização de Funil de Pagamentos (Pix / Boleto) e Governança de Dados no Santander

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/atalitafonseca/atalitafonseca.github.io/blob/main/projeto_santander_jornadas_atribuicao.ipynb)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-red.svg)](https://www.python.org/downloads/)
[![Status](https://img.shields.io/badge/status-conclu%C3%ADdo-brightgreen.svg)]()

**Autora:** Talita Fonseca  
**Repositório Oficial:** [atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)  
**Notebook do Projeto:** [projeto_santander_jornadas_atribuicao.ipynb](projeto_santander_jornadas_atribuicao.ipynb)

---

## 📌 1. Visão Geral do Problema de Negócio

No ecossistema de canais digitais do **Santander**, milhões de clientes navegam diariamente para realizar transações financeiras (apenas em **Pix** o volume supera **19 milhões de acessos/dia**, enquanto **Boleto** registra mais de **1.3 milhão de acessos/dia**).

Nesse ambiente de alta volumetria e complexidade, foram diagnosticadas **quatro dores operacionais e estratégicas**:

1. **Morosidade na Construção de Funis:** Extrações manuais em logs brutos consomem manhãs inteiras dos analistas para responder a perguntas pontuais de produto.
2. **Quebra de Tracking de UTMs:** Parâmetros de URL perdem rastreamento em apps nativos (deep links). Contudo, campanhas e navegações compartilham o identificador do cliente (**`nrpess`**).
3. **Superatribuição (Over-attribution) da Regra de 10 Dias:** A regra legada de 10 dias do CRM gera falsos positivos em pagamentos, pois clientes fariam Pix/Boletos organicamente.
4. **Falta de Padronização e Turnover de Analistas:** Ausência de uma camada semântica documentada, gerando perda de histórico e métricas conflitantes.

---

## 🏗️ 2. Arquitetura da Solução Proposta (Lakehouse Medallion)

```
[ Bronze: Logs de Eventos Brutos ] (19M+ acessos/dia)
                 │
                 ▼
[ Silver: Eventos Canônicos de Jornada ]
  - Particionada por data/produto (Pix, Boleto)
  - Padronização de Intenção e Ponto de Entrada (Campo Único vs Câmera vs Atalhos)
                 │
                 ▼
[ Gold: Sessões Unificadas & Atribuição ]
  - Resolução de Identidade por `nrpess`
  - Atribuição com Decaimento Temporal Exponencial (Meia-vida 12h)
                 │
                 ▼
[ Consumo & Machine Learning ]
  ├── 📊 Dashboards de Funil Instantâneos (Redução de 4h para <3s)
  └── 🤖 Modelo Preditivo de Sucesso no Funil & Atribuição de Campanha
```

---

## 🔬 3. Principais Descobertas e Resultados

### A. Análise do "Campo Único" da tela de Pix
* **35% dos usuários** que entram pelo campo único da tela de Pix digitam ou colam a linha digitável de um **Boleto**.
* O Campo Único representa mais de **40% de todo o volume de Boletos liquidados no aplicativo**.

### B. Correção da Métrica de Atribuição de CRM
* A regra de 10 dias do CRM superestimava em **+138%** os resultados reais de conversão de pagamentos.
* A substituição pelo modelo com **Decaimento Temporal (^{-\lambda \Delta t}$)** isolou o efeito causal real das campanhas internas.

---

## 🤖 4. Modelagem Preditiva de Machine Learning

Comparamos um modelo baseline interpretável contra modelos de ensemble para classificar a probabilidade de conclusão do pagamento no funil:

| Métrica | Regressão Logística (Baseline) | Gradient Boosting (Campeão) |
| :--- | :---: | :---: |
| **Acurácia** | .42\%$ | **.15\%* |
| **Precisão** | .10\%$ | **.40\%* |
| **Recall** | .75\%$ | **.18\%* |
| **F1-Score** | .30\%$ | **.25\%* |
| **ROC-AUC** | zsh.8120$ | **zsh.8875* |

---

## 📂 5. Estrutura do Repositório

```
atalitafonseca.github.io/
├── projeto_santander_jornadas_atribuicao.ipynb  # Notebook Jupyter completo com código, gráficos e modelos
├── index.html                                    # Página executiva do projeto (GitHub Pages)
└── README.md                                     # Documentação oficial do projeto
```

---

## 🚀 Como Executar o Projeto

1. Clone o repositório:
   ```bash
   git clone https://github.com/atalitafonseca/atalitafonseca.github.io.git
   ```
2. Instale as dependências:
   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```
3. Abra o Jupyter Notebook:
   ```bash
   jupyter notebook projeto_santander_jornadas_atribuicao.ipynb
   ```
   *(Ou visualize diretamente no GitHub pelo link do notebook).*
