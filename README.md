# 🏦 Torre de Controle de Campanhas & Funis Santander
### MBA em Inteligência Artificial & Analytics — FGV
**Autora:** Talita Fonseca ([atalitafonseca](https://github.com/atalitafonseca))  
**Professor Responsável:** Prof. Marcelo Fidos Jr.  
**Aplicação Online em Produção:** [https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)  
**Apresentação de Slides:** [https://atalitafonseca.github.io/slides.html](https://atalitafonseca.github.io/slides.html)

---

[![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-Online-2ea44f?style=for-the-badge&logo=github)](https://atalitafonseca.github.io/)
[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-MLP_Neural_Net-orange?style=for-the-badge&logo=scikit-learn)](https://scikit-learn.org/)
[![FGV MBA](https://img.shields.io/badge/FGV-MBA_IA_%26_Analytics-red?style=for-the-badge)](https://eaesp.fgv.br/)

---

## 🎯 Sobre o Projeto

Este projeto desenvolve uma **Torre de Controle Unificada de Campanhas, Funis de Navegação e Produção Bancária** para o **Santander**, aplicando **Redes Neurais Artificiais (Multi-Layer Perceptron - MLP)**, **Atribuição Causal sem Grupo de Controle**, **Simulação de Audiências em Tempo Real** e **Torre de Pacing MTD com Forecast Preditivo**.

A aplicação resolve a dor histórica de silos analíticos entre CRM, Produto e Financeiro, dando total **autonomia para qualquer especialista de negócio (Product Managers, especialistas de canais e analistas)** testar regras e otimizar campanhas instantaneamente.

### 🤖 O Fluxo da IA como Copiloto de Decisão (Human-in-the-Loop):
1. **Estratégia do Negócio:** A pessoa de negócio define o público-alvo inicial no Simulador (ex: *Clientes Select, produto Pix Parcelado, espaço inicial Banner*).
2. **Diagnóstico da IA (< 1 segundo):** A Rede Neural MLP avalia a hipótese em tempo real — eliminando o gargalo de **~2 horas de espera em filas de processamento de clusters analíticos**.
3. **Sugestão Proativa de Melhorias:** A IA sugere o espaço ideal de maior conversão (*Next-Best-Space* como Lightbox) e aponta corte de 30% em disparos para clientes 100% orgânicos.
4. **Decisão Rápida:** O especialista valida as recomendações com 1 clique e publica a campanha com máxima eficiência e sem desperdício.

---

## 🌐 Acesso Online & Navegação no Dashboard

O dashboard interativo está hospedado via GitHub Pages e pode ser acessado diretamente em:  
👉 **[https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)**  
👉 **Apresentação Executiva (Slides): [https://atalitafonseca.github.io/slides.html](https://atalitafonseca.github.io/slides.html)**

### 🧭 Estrutura das 4 Abas da Aplicação:
1. **🎯 1. Simulador & Botão Calcule IA:**
   * **Seletor de Objetivo Estratégico:** Alternância imediata entre **🎯 Conversão & Vendas** e **📢 Awareness & Alcance de Marca**.
   * **Alavancas de IA Interativas:** Switches `[ 🟢 Ativo | ⚪ Desativo ]` para Open Finance (+28% de público), Qualificação ARPAC > 7.0 (93.9% liquidação), Espaço Comercial e Conta Salário (FOPA).
   * **Rótulos Numéricos no Gráfico (Datalabels):** Valores diretos no topo das barras com destaque visual para o canal ativo.
   * **Dicionário de Hábitos Santander:** Busca semântica em linguagem natural com adição rápida de atributos às regras.
2. **🔗 2. A Tríade: Funil, Visão Mensal & Reconciliação:**
   * Funil completo de 7 etapas da visualização até o Core Bancário com variações Mês a Mês (MoM 2026).
   * Painel de reconciliação com isolamento de perdas técnicas (Antifraude 45%, Saldo insuficiente 32% e Time-out).
3. **⏱️ 3. Torre de Pacing MTD (Comparativo Mês a Mês & IA):**
   * Gráfico diário (Dias 1 a 31) com 4 curvas comparando Mês Atual Realizado (1-20), Forecast IA (21-31), Mês Anterior Completo (1-31) e Meta Linear.
   * Tabela executiva de acompanhamento por Decêndios (1º, 2º e 3º Decêndio) e recomendação de alavanca de CRM.
4. **🧠 4. Performance da Rede Neural (FGV):**
   * Curva ROC, matriz de confusão e resultados da validação estocástica Multi-Seed.

---

## 💰 Viabilidade Econômica & Memória de Cálculo do ROI (Ano 1)

| Indicador Financeiro | Detalhamento / Premissa | Valor Consolidado |
| :--- | :--- | :--- |
| **Custo de Construção (Capex)** | Squad de 3 meses (Tech Lead, Data Scientist, Data Engineer, PM, Frontend) + Cloud GPUs | **R$ 380.000,00** (One-Off) |
| **Custo de Sustentação (Opex Anual)** | R$ 28.000/mês (Infra de scoring diário, retreino quinzenal, monitoramento MLOps) | **R$ 336.000,00** / ano |
| **Investimento Total no Ano 1** | Capex de Construção + 12 meses de Opex | **R$ 716.000,00** |
| **Economia em CRM (30% Opex Evitado)** | 4.500.000 disparos evitados/mês $\times$ R$ 0,04 por disparo/push (corta orgânicos e ruído) | **R$ 2.160.000,00** / ano (R$ 180k/mês) |
| **Receita Incremental de Vendas (MLP)** | +142.000 contratações adicionais no canal ideal $\times$ R$ 38,00 de margem média líquida | **R$ 5.396.000,00** / ano (R$ 450k/mês) |
| **Retorno Bruto Consolidado (Ano 1)** | Economia de CRM + Receita Incremental | **R$ 7.556.000,00** / ano |
| **Retorno Líquido no Ano 1** | Retorno Bruto (R$ 7.556.000) - Investimento Total (R$ 716.000) | **R$ 6.840.000,00** |
| **ROI (Retorno sobre Investimento)** | $(\text{R\$} 7.556.000 - \text{R\$} 716.000) / \text{R\$} 716.000$ | **955,3%** |
| **Tempo de Payback** | $\text{R\$} 380.000 / \text{R\$} 601.666 \text{ ganho líquido/mês}$ | **0,63 meses (19 dias de operação)** |

---

## 🏆 Comparativo de Modelos & Performance Oficial (Padrão FGV)

### 📊 Comparação dos 3 Níveis de Complexidade:
| Modelo | Acurácia | Precisão | Recall | F1-Score | ROC-AUC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **1. Regra Heurística Determinística** | 62.15% | 23.10% | 35.20% | 0.2450 | 0.5820 |
| **2. Regressão Logística L2 (Baseline ML)** | 69.67% | 31.20% | 40.10% | 0.3505 | 0.7064 |
| **3. Rede Neural MLP (Modelo Campeão)** | **69.28%** | **37.85%** | **47.25%** | **0.4202** | **0.7023** |

* **Uplift Comprovado:** **+19.8% de ganho de F1 da Rede Neural sobre a Regressão Logística** e **+71.5% sobre a regra heurística**.
* **Validação Multi-Seed Obrigatória (Seeds: 42, 7, 123):**
  * **MLP Neural Net:** F1-Score Médio de $0.4094 \pm 0.0064$ e ROC-AUC de $0.7023 \pm 0.0041$.
  * **Baseline LogReg:** F1-Score Médio de $0.3518 \pm 0.0028$ e ROC-AUC de $0.7064 \pm 0.0035$.

---

## 📦 Pacote de Entregáveis FGV

| Arquivo | Descrição |
| :--- | :--- |
| **`index.html`** | Aplicação web completa e responsiva da Torre de Controle e Simulador (com fallback local para `assets/chart.umd.min.js`). |
| **`slides.html` / `apresentacao_3_slides.md`** | Apresentação executiva de 4 slides com a narrativa executiva, funis e ROI. |
| **`projeto_santander_jornadas_redes_neurais.ipynb`** | Notebook Jupyter executado de ponta a ponta com MLP, baselines, multi-seed, ROI e MLOps (PSI). |
| **`projeto_santander_jornadas_atribuicao.ipynb`** | Notebook Jupyter executado com as curvas dinâmicas de Atribuição Causal. |
| **`plano_projeto_santander_ia.md`** | Plano de projeto formal completo estruturado nos 7 blocos do template oficial da FGV. |

---

## 🚀 Como Executar Localmente

```bash
# 1. Clone o repositório
git clone https://github.com/atalitafonseca/atalitafonseca.github.io.git

# 2. Acesse a pasta
cd atalitafonseca.github.io

# 3. Abra o dashboard no navegador
open index.html

# 4. Para executar o notebook de Redes Neurais
jupyter notebook projeto_santander_jornadas_redes_neurais.ipynb
```

---
**Autora:** Talita Fonseca  
*MBA em Inteligência Artificial & Analytics — FGV*
