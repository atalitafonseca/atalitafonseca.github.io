# 🏦 Torre de Controle de Campanhas & Funis Santander
### MBA em Inteligência Artificial & Analytics — FGV
**Autora:** Talita Fonseca ([atalitafonseca](https://github.com/atalitafonseca))  
**Professor Responsável:** Prof. Marcelo Fidos Jr.  
**Aplicação Online:** [https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)

---

[![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-Online-2ea44f?style=for-the-badge&logo=github)](https://atalitafonseca.github.io/)
[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-MLP_Neural_Net-orange?style=for-the-badge&logo=scikit-learn)](https://scikit-learn.org/)
[![FGV MBA](https://img.shields.io/badge/FGV-MBA_IA_%26_Analytics-red?style=for-the-badge)](https://eaesp.fgv.br/)

---

## 🎯 Sobre o Projeto

Este projeto desenvolve uma **Torre de Controle Unificada de Campanhas, Funis de Navegação e Produção Bancária** para o **Santander**, aplicando **Redes Neurais Artificiais (Multi-Layer Perceptron - MLP)**, **Atribuição Causal sem Grupo de Controle**, **Simulação de Audiências em Tempo Real** e **Torre de Pacing MTD com Forecast Preditivo**.

A aplicação resolve a dor histórica de silos analíticos entre CRM, Produto e Financeiro, permitindo que o especialista de produto simule audiências, module alavancas de IA com switches `[ 🟢 Ativo | ⚪ Desativo ]`, alterne entre estratégias de **Conversão** e **Awareness**, e acompanhe o ritmo de vendas diário comparado com o mês anterior.

---

## 🌐 Acesso Online & Navegação no Dashboard

O dashboard interativo está hospedado via GitHub Pages e pode ser acessado diretamente em:  
👉 **[https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)**

### 🧭 Estrutura das 4 Abas da Aplicação:
1. **🎯 1. Simulador & Botão Calcule IA:**
   * **Seletor de Objetivo Estratégico:** Alternância imediata entre **🎯 Conversão & Vendas** e **📢 Awareness & Alcance de Marca**.
   * **Alavancas de IA Interativas:** Switches `[ 🟢 Ativo | ⚪ Desativo ]` para Open Finance (+28% de público), Qualificação ARPAC > 7.0 (93.9% liquidação), Espaço Comercial e Conta Salário (FOPA).
   * **Rótulos Numéricos no Gráfico (Datalabels):** Valores diretos no topo das barras com destaque visual para o canal ativo.
   * **Dicionário de Hábitos Santander:** Busca semântica em linguagem natural com adição rápida de atributos às regras.
2. **🔗 2. A Tríade: Funil, Visão Mensal & Reconciliação:**
   * Funil completo de 7 etapas da visualização até o Core Bancário com variações Mês a Mês (MoM 2026).
   * Painel de reconciliação com isolamento de perdas técnicas (Antifraude, Saldo insuficiente e Time-out).
3. **⏱️ 3. Torre de Pacing MTD (Comparativo Mês a Mês & IA):**
   * Gráfico diário (Dias 1 a 31) com 4 curvas comparando Mês Atual Realizado (1-20), Forecast IA (21-31), Mês Anterior Completo (1-31) e Meta Linear.
   * Tabela executiva de acompanhamento por Decêndios (1º, 2º e 3º Decêndio) e recomendação de alavanca de CRM.
4. **🧠 4. Performance da Rede Neural (FGV):**
   * Curva ROC, matriz de confusão e resultados da validação estocástica Multi-Seed.

---

## 📦 Entregáveis do Projeto (Estrutura de Arquivos)

| Arquivo | Descrição |
| :--- | :--- |
| **`index.html`** | Aplicação web completa e responsiva da Torre de Controle e Simulador. |
| **`projeto_santander_jornadas_redes_neurais.ipynb`** | Notebook Jupyter com todo o pipeline de Redes Neurais (MLP), EDA, baseline e atribuição causal. |
| **`apresentacao_3_slides.md`** | Roteiro executivo de apresentação (4 slides) pronto para apresentação executiva ou importação no Gamma.app. |
| **`plano_projeto_santander_ia.md`** | Documentação técnica completa de arquitetura, formulação matemática e MLOps. |
| **`plano_projeto_santander_ia.pdf`** | Documento oficial formatado em PDF para submissão acadêmica. |
| **`data/`** | Bases sintéticas realistas geradas para experimentação e modelagem. |

---

## 🏆 Performance do Modelo de IA (Padrão Oficial FGV)

* **Uplift de F1-Score:** **+19.8%** da Rede Neural MLP ($0.4202$) em relação ao Baseline de Regressão Logística ($0.3505$).
* **Validação Multi-Seed:** Avaliação em 3 sementes obrigatórias (`seed 42`, `seed 7`, `seed 123`):
  * **MLP Neural Net:** F1-Score Médio de $0.4094 \pm 0.0064$ e ROC-AUC de $0.7023 \pm 0.0041$.
  * **Baseline LogReg:** F1-Score Médio de $0.3518 \pm 0.0028$ e ROC-AUC de $0.7064 \pm 0.0035$.

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
