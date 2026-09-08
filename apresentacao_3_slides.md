# 📊 Apresentação Executiva: Torre de Controle de Campanhas & Funis Santander
## Estudo de Caso FGV: Inteligência Artificial, Analytics de Jornadas e Previsão com Redes Neurais
**Autora:** Talita Fonseca  
**Instituição:** Fundação Getulio Vargas (FGV) — MBA em Inteligência Artificial & Analytics  
**Professor Responsável:** Prof. Marcelo Fidos Jr.  
**Dashboard Online Interativo:** [atalitafonseca.github.io](https://atalitafonseca.github.io/)  
**Repositório GitHub:** [github.com/atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)

---

# Slide 1: Do Caos dos Silos à Torre de Controle: IA Preditiva no Santander
**Subtítulo:** Como a Rede Neural Densa (MLP) e a Engenharia de Dados conectam 19M+ de clientes, unificando CRM, Funis no App e Produção Core.

* **O Desafio de Negócio (Disparos no Escuro & Silos):**
  * O especialista de Produto desenhava campanhas às cegas: não sabia o **tamanho real do público elegível** nem a **conversão em cada espaço do App**.
  * Silos analíticos: CRM monitorava cliques, Produto monitorava telas e Financeiro monitorava contratos no Core Bancário.
  * Consultas de público demoravam cerca de **2 horas** (sujeito à fila de prioridade de cluster/CRM).
  * Regra legada de atribuição de 10 dias gerava até **138% de superatribuição** ao creditar transações orgânicas normais do cliente.

* **A Solução com Inteligência Artificial (FGV):**
  * **Rede Neural Densa (MLPClassifier):** Modela a propensão individual cruzando *(Hábito $\times$ Espaço $\times$ Fricção $\times$ Produto)*.
  * **Atribuição Causal sem Grupo de Controle:** Desconto dinâmico da propensão orgânica ($w = e^{-\lambda \Delta t} \times (1 - P_{\text{org}})$), preservando 100% da receita comercial sem travar clientes em grupo de controle.

* **Performance Oficial e Validação Multi-Seed (Padrão FGV):**
  * **Uplift de F1-Score:** A Rede Neural MLP atingiu **0.4202 (+19.8% de ganho sobre o Baseline de Regressão Logística de 0.3505 e +71% sobre a regra heurística de 0.2450)**.
  * **Robustez Multi-Seed:** Avaliação em 3 sementes obrigatórias (`seeds 42, 7, 123`) com média $0.4094 \pm 0.0064$ e ROC-AUC $0.7023 \pm 0.0041$.

---

# Slide 2: A Tríade de Funis & A Torre de Pacing MTD (Comparativo MoM 2026)
**Subtítulo:** Visão unificada de 7 etapas da visualização até a liquidação bancária, reconciliação App vs Core e ritmo de vendas diário.

* **O Funil Unificado de 7 Etapas (Aba 2 do Dashboard):**
  * **Etapas 1 e 2 (100% Campanha):** *1. Visualização nos Espaços (5,0M)* $\rightarrow$ *2. Cliques / Interações (1,2M)*.
  * **Etapas 3 a 7 (Convergência com Orgânico):** *3. Topo App* $\rightarrow$ *4. Simulação* $\rightarrow$ *5. Autenticação ID* $\rightarrow$ *6. Fechamento App* $\rightarrow$ *7. Produção Core Bancário*.
  * **Reconciliação e Perdas Técnicas:** Isola por que nem todo fechamento vira contrato: perdas por **Antifraude / Risco em Tempo Real (45%)**, **Saldo Insuficiente (32%)** e **Time-out de Conciliação**.

* **Torre de Pacing MTD & Forecast da IA (Aba 3 do Dashboard):**
  * **Comparativo Dia a Dia (Dias 1 a 31):** 4 curvas comparando *Realizado Mês Atual (D1-20)*, *Forecast IA (D21-31)*, *Mês Anterior Fechado (D1-31)* e *Meta Linear*.
  * **Acompanhamento por Decêndios:** Análise do ritmo a cada 10 dias com cálculo do gap e recomendação automática de alavanca de CRM.

---

# Slide 3: O Simulador de Audiências & Viabilidade Econômica (ROI)
**Subtítulo:** Modulação de público com switches Ativo/Desativo, modos Conversão vs Awareness e Retorno Financeiro Consolidado.

* **Seletor de Objetivo Estratégico da Campanha:**
  * **🎯 Modo Conversão & Vendas:** Projeta contratos finais liquidados no Core e ROI.
  * **📢 Modo Awareness & Alcance:** Projeta pessoas únicas alcançadas, frequência média (2.2x) e eficiência de CPM (R$ 4,80).
  * **Alavancas Inteligentes `[ 🟢 Ativo | ⚪ Desativo ]`:** Demonstração do ganho de **+28% de público (+957.600 clientes)** com Open Finance e qualificação ARPAC > 7.0 (93.9% de liquidação no Core).

* **Viabilidade Econômica & Retorno em R$ (Padrão Oficial FGV):**
  * **Custo de Construção (Capex):** **R$ 380.000,00** (Squad de 3 meses + Cloud GPUs).
  * **Custo de Sustentação (Opex):** **R$ 28.000,00 / mês** (R$ 336.000,00 / ano em MLOps e scoring).
  * **Economia em CRM:** **R$ 2.160.000,00 / ano** (4,5M disparos evitados/mês $\times$ R$ 0,04).
  * **Receita Incremental com MLP:** **R$ 5.396.000,00 / ano** (+142.000 contratações $\times$ R$ 38,00 de margem).
  * **Retorno Líquido Ano 1:** **R$ 6.840.000,00** $\rightarrow$ **ROI de 955% com Payback em apenas 19 dias**.

---

# Slide 4: Acesso ao Dashboard Interativo & Entregáveis Oficiais da FGV
**Subtítulo:** Como o professor e a banca avaliadora podem navegar, testar e auditar todas as funcionalidades ao vivo.

* **🌐 Acesso Online Direto e Resiliente (Com Fallback Local):**
  * O dashboard completo está hospedado e disponível em: **[https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)**
  * **Resiliência Offline:** Inclui fallback local para `assets/chart.umd.min.js`, garantindo funcionamento em qualquer ambiente.

* **📦 Pacote Completo de Entregáveis FGV:**
  1. **Dashboard Interativo em Produção (`index.html`):** 4 abas funcionais (Simulador IA, A Tríade de Funil, Torre de Pacing e Métricas MLP).
  2. **Notebooks Jupyter Executados (.ipynb):** Código Python com saídas gravadas, baseline de regra, regressão logística, MLP, multi-seed e MLOps com PSI.
  3. **Plano de Projeto nos 7 Blocos Oficiais (`plano_projeto_santander_ia.md`):** Arquitetura Medallion, governança LGPD e planilha completa de ROI.
  4. **Repositório Versionado no GitHub:** [github.com/atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)
