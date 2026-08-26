# 🏦 Torre de Controle de Jornadas e Previsor de Público & Resultado com Redes Neurais
### Estudo de Caso: Unificação de Campanhas de CRM, Funis no App e Produção no Santander

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/atalitafonseca/atalitafonseca.github.io/blob/main/projeto_santander_jornadas_redes_neurais.ipynb)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-red.svg)](https://www.python.org/downloads/)
[![Status](https://img.shields.io/badge/status-conclu%C3%ADdo-brightgreen.svg)]()

**Autora:** Talita Fonseca  
**Instituição:** Fundação Getulio Vargas (FGV) — MBA em Inteligência Artificial & Analytics  
**Repositório Oficial:** [atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)  
**Notebook do Projeto:** [](projeto_santander_jornadas_redes_neurais.ipynb)  
**Plano do Projeto (PDF):** [](plano_projeto_santander_ia.pdf)  
**Apresentação Executiva (3 Slides):** [](apresentacao_3_slides.md)

---

## 📌 1. Visão Geral do Problema de Negócio

No ecossistema de canais digitais do **Santander**, mais de **19 milhões de clientes ativos diários** navegam no aplicativo bancário. Três grandes desafios operacionais foram diagnosticados:

1. **Disparos no Escuro e Falta de Previsibilidade:** O time de Produto cria campanhas e ofertas, mas não tem como estimar com precisão o **tamanho real do público elegível (Sizing)** nem **prever o volume de vendas** antes de veicular a campanha.
2. **Silos e Demora de Semanas:** O time de CRM é o único com acesso às tabelas ricas de atributos dos clientes (). Cada analista constrói queries manuais e isoladas, demorando **semanas** para colocar um dashboard no ar.
3. **Superatribuição de 10 Dias sem Grupo de Controle:** A regra legada de 10 dias do CRM gera falsos positivos de atribuição ao creditar pagamentos orgânicos como sucesso de marketing. Travar clientes em **grupos de controle é comercialmente inviável**, pois priva o banco de faturamento imediato.

---

## 🏗️ 2. A Solução: Arquitetura Medallion & IA como Previsora

```
[ 1. Dicionário de Atributos de Clientes (CRM) ] ──┐
[ 2. Base de Campanhas & Espaços Comerciais ]   ──┼──► [ Camada Gold Unificada (por nrpess) ]
[ 3. Logs de Clickstream & Produção no App ]   ──┘                 │
                                                                   ▼
                                            [ Previsor de Público & Resultado com IA ]
                                            • Sizing Instantâneo de Audiência
                                            • Predição de Vendas por Espaço (MLP)
                                            • Atribuição Causal sem Grupo de Controle
```

---

## 🤖 3. Modelagem de IA: Baseline vs Rede Neural Densa (MLP)

Conforme a metodologia oficial da FGV, comparamos o modelo linear baseline com uma **Rede Neural Densa (Multi-Layer Perceptron)**:

| Métrica | Regressão Logística (Baseline) | Rede Neural MLP (Campeão) |
| :--- | :---: | :---: |
| **ROC-AUC** | zsh.7064$ | **zsh.7023* |
| **Acurácia** | .67\%$ | **.28\%* |
| **F1-Score** | zsh.3505$ | **zsh.4202* |
| **Robustez Multi-Seed (F1)** | zsh.3518 \pm 0.0028$ | **zsh.4094 \pm 0.0064* |

---

## 🎯 4. Resultados da Simulação Preditiva (Exemplo: Seguro Pix)

Para um público filtrado de clientes *Especial* e *Select* com *Gasto em Cartão $\ge$ R$ 1.200*:

* 👥 **Público Elegível (Sizing Instantâneo):** 
* 📊 **Previsão de Conversão por Espaço Comercial:**
  * 🥇 **Pós-Pix Transacional:**  de conversão $ightarrow$ **4.820 vendas estimadas** ($	ext{R$} 144.600,00$) ⭐ *Espaço Recomendado*
  * 🥈 **Banner Home:**  de conversão $ightarrow$ **3.054 vendas estimadas** ($	ext{R$} 91.620,00$)
  * 🥉 **Carrossel Ofertas:**  de conversão $ightarrow$ **2.358 vendas estimadas** ($	ext{R$} 70.740,00$)
  * 📱 **Push Notification:**  de conversão $ightarrow$ **2.322 vendas estimadas** ($	ext{R$} 69.660,00$)

---

## 📂 5. Estrutura do Repositório

```
atalitafonseca.github.io/
├── data/                                             # Bases sintéticas realistas (CSV)
│   ├── atributos_clientes.csv                        # 25.000 clientes com score, gastos e engajamento
│   ├── campanhas_crm.csv                             # 35.000 interações de campanhas e espaços
│   ├── jornadas_producao.csv                         # 40.000 sessões de navegação no app
│   └── camada_gold_unificada.csv                     # 29.280 sessões unificadas por nrpess
├── assets/                                           # Gráficos gerados pelos modelos
│   └── avaliacao_modelos_e_espacos.png
├── projeto_santander_jornadas_redes_neurais.ipynb     # Jupyter Notebook completo com código e modelos
├── plano_projeto_santander_ia.pdf                    # PDF oficial do Plano de Projeto (Template FGV)
├── plano_projeto_santander_ia.md                     # Plano de Projeto em Markdown
├── apresentacao_3_slides.md                          # Roteiro dos 3 Slides Executivos (Gamma.app)
├── index.html                                        # Landing Page no GitHub Pages
└── README.md                                         # Documentação oficial
```

---

## 🚀 Como Executar Localmente

1. Clone o repositório:
   ```bash
   git clone https://github.com/atalitafonseca/atalitafonseca.github.io.git
   cd atalitafonseca.github.io
   ```
2. Instale as dependências:
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn jupyter
   ```
3. Abra o Jupyter Notebook:
   ```bash
   jupyter notebook projeto_santander_jornadas_redes_neurais.ipynb
   ```
