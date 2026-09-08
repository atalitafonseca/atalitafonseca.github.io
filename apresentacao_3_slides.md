# 📊 Apresentação Executiva: Torre de Controle de Campanhas & Funis Santander
## Estudo de Caso FGV: Inteligência Artificial, Analytics de Jornadas e Previsão com Redes Neurais
**Autora:** Talita Fonseca  
**Instituição:** Fundação Getulio Vargas (FGV) — MBA em Inteligência Artificial & Analytics  
**Professor Responsável:** Prof. Marcelo Fidos Jr.  
**Dashboard Online Interativo:** [atalitafonseca.github.io](https://atalitafonseca.github.io/)  
**Repositório GitHub:** [github.com/atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)

---

# Slide 1: Do Caos dos Silos à Torre de Controle: IA como Copiloto no Santander
**Subtítulo:** Como a Rede Neural Densa (MLP) e o Simulador de Decisão dão autonomia para as áreas de negócio otimizarem campanhas para 19M+ de clientes.

* **O Desafio Operacional Anterior (Silos & Gargalos):**
  * O especialista de Produto/Canal desenhava campanhas às cegas e dependia de queries de CRM que demoravam **~2 horas** (sujeitas a filas de prioridade de cluster).
  * Silos analíticos: CRM monitorava cliques, Produto monitorava telas e Financeiro monitorava liquidação no Core.
  * Regra legada de atribuição gerava até **138% de superatribuição** e grupos de controle fixos eram inviáveis comercialmente.

* **O Novo Fluxo de Decisão com IA (Human-in-the-Loop):**
  1. **Negócio define estratégia:** A pessoa de negócio monta o público no Simulador (ex: *Select + Pix Parcelado + Banner*).
  2. **IA diagnostica em < 1s:** A Rede Neural calcula o público qualificado e prevê a taxa de conversão.
  3. **IA sugere melhorias:** Recomenda o espaço ideal (*Next-Best-Space* como Lightbox) e aponta corte de 30% em disparos orgânicos.
  4. **Decisão rápida:** O especialista valida com 1 clique e ativa a campanha com máxima eficiência.

* **Performance Oficial e Validação Multi-Seed (Padrão FGV):**
  * **Uplift de F1-Score:** Rede Neural MLP atingiu **0.4202 (+19.8% sobre Baseline Logístico de 0.3505 e +71% sobre a Heurística de 0.2450)**.
  * **Robustez Multi-Seed:** Avaliação em 3 sementes obrigatórias (`seeds 42, 7, 123`) com média $0.4094 \pm 0.0064$ e ROC-AUC $0.7023 \pm 0.0041$.

---

# Slide 2: A Tríade de Funis & A Torre de Pacing MTD (Comparativo MoM 2026)
**Subtítulo:** Visão unificada de 7 etapas da visualização até a liquidação bancária, reconciliação App vs Core e ritmo de vendas diário.

* **O Funil Unificado de 7 Etapas (Aba 2 do Dashboard):**
  * **Etapas 1 e 2 (100% Campanha):** *1. Visualização nos Espaços (5,0M)* $\rightarrow$ *2. Cliques / Interações (1,2M)*.
  * **Etapas 3 a 7 (Convergência com Orgânico):** *3. Topo App* $\rightarrow$ *4. Simulação* $\rightarrow$ *5. Autenticação ID* $\rightarrow$ *6. Fechamento App* $\rightarrow$ *7. Produção Core Bancário*.
  * **Reconciliação e Perdas Técnicas:** Isola perdas por **Antifraude / Risco em Tempo Real (45%)**, **Saldo Insuficiente (32%)** e **Time-out de Conciliação**.

* **Torre de Pacing MTD & Forecast da IA (Aba 3 do Dashboard):**
  * **Comparativo Dia a Dia (Dias 1 a 31):** 4 curvas comparando *Realizado Mês Atual (D1-20)*, *Forecast IA (D21-31)*, *Mês Anterior Fechado (D1-31)* e *Meta Linear*.
  * **Acompanhamento por Decêndios:** Análise do ritmo a cada 10 dias com cálculo do gap e recomendação de alavancas.

---

# Slide 3: O Simulador de Audiências & Viabilidade Econômica (ROI)
**Subtítulo:** Modulação de público com switches Ativo/Desativo, modos Conversão vs Awareness e Retorno Financeiro Consolidado.

* **Seletor de Objetivo Estratégico da Campanha:**
  * **🎯 Modo Conversão & Vendas:** Projeta contratos finais liquidados no Core e ROI.
  * **📢 Modo Awareness & Alcance:** Projeta pessoas únicas alcançadas, frequência média (2.2x) e eficiência de CPM (R$ 4,80).
  * **Alavancas Inteligentes `[ 🟢 Ativo | ⚪ Desativo ]`:** Demonstração do ganho de **+28% de público (+957.600 clientes)** com Open Finance e qualificação ARPAC > 7.0 (93.9% de liquidação no Core).

* **Detalhamento dos Ganhos e Memória de Cálculo do ROI:**
  * **1. Economia em CRM (30% Opex):** 4,5M msgs evitadas/mês × R$ 0,04 = **R$ 2.160.000,00 / ano** (cortando clientes 100% orgânicos e sem propensão).
  * **2. Receita Incremental (MLP):** +142.000 novos contratos × R$ 38,00 de margem = **R$ 5.396.000,00 / ano** (alocação no espaço ideal sugerido pela IA).
  * **Investimento Total Ano 1:** Capex de Construção (R$ 380k) + Opex MLOps Sustentação (R$ 336k) = **R$ 716.000,00**.
  * **Retorno Líquido Ano 1:** **R$ 6.840.000,00** $\rightarrow$ **ROI de 955% com Payback em apenas 19 dias**.

---

# Slide 4: Acesso ao Dashboard Interativo & Entregáveis Oficiais da FGV
**Subtítulo:** Como o professor e a banca avaliadora podem navegar, testar e auditar todas as funcionalidades ao vivo.

* **🌐 Acesso Online Direto e Resiliente (Com Fallback Local):**
  * O dashboard completo está hospedado e disponível em: **[https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)**
  * **Resiliência Offline:** Inclui fallback local para `assets/chart.umd.min.js`, garantindo funcionamento em qualquer ambiente de correção.

* **📦 Pacote Completo de Entregáveis FGV:**
  1. **Dashboard Interativo em Produção (`index.html`):** 4 abas funcionais (Simulador IA, A Tríade de Funil, Torre de Pacing e Métricas MLP).
  2. **Notebooks Jupyter Executados (.ipynb):** Código Python com saídas gravadas, baseline de regra, regressão logística, MLP, multi-seed e MLOps com PSI.
  3. **Plano de Projeto nos 7 Blocos Oficiais (`plano_projeto_santander_ia.md`):** Arquitetura Medallion, governança LGPD e planilha completa de ROI.
  4. **Repositório Versionado no GitHub:** [github.com/atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)
