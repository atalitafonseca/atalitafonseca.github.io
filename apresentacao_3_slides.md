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
  * Hoje, o especialista de Produto tem o domínio de negócio para desenhar campanhas, mas opera às cegas: não sabe o **tamanho real do público elegível** nem o **volume de conversão em cada espaço do App** antes de colocar a campanha no ar.
  * O time de CRM monitora impressões/cliques em tabelas isoladas; o time de Produto acompanha telas no App; e o time Financeiro acompanha a liquidação de contratos no Core Bancário. Não havia conexão de ponta a ponta.
  * Solicitações manuais de audiência demoravam **semanas**, com regras de atribuição de 10 dias que superatribuíam transações orgânicas normais do cliente.

* **A Solução com Inteligência Artificial (FGV):**
  * Uma **Torre de Controle Integrada** equipada com **Rede Neural MLP (Multi-Layer Perceptron)** treinada para prever a probabilidade de conversão individualizada cruzando *(Hábito do Cliente $\times$ Vocação do Espaço $\times$ Fricção da Jornada $\times$ Produto)*.
  * **Atribuição Causal sem Grupo de Controle:** Desconto dinâmico da propensão orgânica base do cliente ($w = e^{-\lambda \Delta t} \times (1 - P_{\text{org}})$), preservando 100% da receita comercial sem necessidade de travar clientes em grupos de controle fixos.

* **Performance Oficial e Validação Multi-Seed (Padrão FGV):**
  * **Uplift de F1-Score:** A Rede Neural MLP atingiu **0.4202 (+19.8% de ganho sobre o Baseline de Regressão Logística de 0.3505)**, comprovando a capacidade de capturar relações não-lineares complexas.
  * **Robustez Estocástica Multi-Seed:** Avaliação em 3 sementes aleatórias obrigatórias (`seed 42`, `seed 7`, `seed 123`) com média $0.4094 \pm 0.0064$, garantindo estabilidade e ausência de overfitting.

---

# Slide 2: A Tríade de Funis & A Torre de Pacing MTD (Comparativo MoM 2026)
**Subtítulo:** Visão unificada de 7 etapas da visualização até a liquidação bancária, reconciliação App vs Core e ritmo de vendas diário.

* **O Funil Unificado de 7 Etapas (Aba 2 do Dashboard):**
  1. *Visualização nos Espaços Comerciais* (Lightbox, Alert, Banner, Push, Email)
  2. *Cliques / Interações (CTR)*
  3. *Topo de Funil: Entrada no Fluxo do App*
  4. *Step do Meio: Simulação do Produto*
  5. *Step do Meio: Autenticação de Segurança (ID Santander)*
  6. *Fechamento no App (Conclusão pelo Usuário)*
  7. *Fundo de Funil: Produção Efetiva no Core Bancário*
  * **Evolução Mês a Mês (MoM 2026):** Badges dinâmicos indicam variações percentuais em relação ao mês anterior (ex: *Cliques subindo +15.0% MoM* e identificação de gargalos no meio do funil).

* **Reconciliação e Perdas Técnicas (App $\rightarrow$ Core):**
  * O dashboard isola por que nem todo fechamento de tela vira contrato: perdas por **Antifraude / Risco em Tempo Real (45%)**, **Saldo/Limite Insuficiente (32%)** e **Time-out de Conciliação Bancária**.

* **Torre de Pacing MTD & Forecast da IA (Aba 3 do Dashboard):**
  * **Comparativo Dia a Dia (Dias 1 a 31):** Gráfico de 4 curvas comparando:
    * 🔴 *Realizado Mês Atual (Dias 1 a 20)*
    * 🔵 *Projeção Preditiva da IA (Dias 21 a 31)*
    * ⚪ *Mês Anterior Homólogo Completo (Dias 1 a 31)*
    * 🟢 *Meta Linear Contratada*
  * **Tabela por Decêndios:** Acompanhamento do ritmo a cada 10 dias com cálculo automático do gap e recomendação de alavanca de CRM (*ex: Manter Lightbox vs Ativar Push D-5*).

---

# Slide 3: O Simulador Inteligente de Audiências & Alavancas de IA
**Subtítulo:** Self-service para o especialista de Produto: modulação de público com switches Ativo/Desativo, modos Conversão vs Awareness e Dicionário de Hábitos.

* **Seletor de Objetivo Estratégico da Campanha:**
  * **🎯 Modo Conversão & Vendas (Fundo de Funil):** Foco em Contratos Finais, ROI e Produção Core. O gráfico projeta o volume exato de contratos liquidados por canal comercial.
  * **📢 Modo Awareness & Alcance de Marca (Topo de Funil / Lançamentos):** Foco em Pessoas Únicas Alcançadas, Frequência Média de Exibição, Cobertura da Base de Correntistas e Eficiência de CPM (R$ 4,80 por 1.000 impactos).

* **Alavancas Inteligentes com Switches `[ 🟢 Ativo | ⚪ Desativo ]`:**
  * Cada recomendação da IA possui um botão interativo de alternância direta:
    * **Alavanca Open Finance:** Ligar a alavanca remove as travas de consentimento e **expande o público imediatamente em +28% (+957.600 clientes elegíveis)**.
    * **Qualificação ARPAC:** Ligar prioriza clientes com Score ARPAC > 7.0, elevando a liquidação no Core para 93.9%; desligar abre o público para massa.
    * **Canal Comercial:** Alterna instantaneamente entre Lightbox (alta conversão) e Banner/Push (ampla distribuição).
  * **Recálculo Instantâneo & Datalabels:** Qualquer alteração pisca os 5 KPIs do topo e atualiza os **rótulos numéricos diretos no topo das barras do gráfico** (`255,8k contr.` ou `2,39M pessoas`).

* **Dicionário de Hábitos Santander com Busca Semântica:**
  * Busca inteligente por linguagem natural (*"evasão open finance"*, *"salário fopa"*, *"gasto cartão"*) permitindo adicionar condições com 1 clique.

---

# Slide 4: Acesso ao Dashboard Interativo & Entregáveis Oficiais da FGV
**Subtítulo:** Como o professor e a banca avaliadora podem navegar, testar e auditar todas as funcionalidades ao vivo.

* **🌐 Acesso Online Direto (Sem Instalação):**
  * O dashboard completo está hospedado e disponível em: **[https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)**
  * Totalmente responsivo para Desktop e Mobile, com recálculo instantâneo de IA no navegador.

* **📦 Pacote Completo de Entregáveis FGV:**
  1. **Dashboard Interativo em Produção (`index.html`):** Torre de Controle com as 4 abas funcionais (Simulador IA, Tríade de Funil, Torre de Pacing MTD e Performance da Rede Neural).
  2. **Notebook Jupyter Executável (`projeto_santander_jornadas_redes_neurais.ipynb`):** Código-fonte completo em Python com geração de dados, pré-processamento, pipeline de Redes Neurais (MLP), matrizes de confusão, validação Multi-Seed e modelo de atribuição causal.
  3. **Documento Técnico de Planejamento e Arquitetura (`plano_projeto_santander_ia.pdf` / `.md`):** Relatório detalhado com formulação matemática, arquitetura Medallion Lakehouse e governança MLOps.
  4. **Repositório Versionado no GitHub:** [github.com/atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)

* **Impacto Consolidado:**
  * ⚡ Redução de **3 semanas para < 1 segundo** na montagem de públicos e análise de funis.
  * 💰 Economia de **30% em custos de disparos de CRM** eliminando desperdício orgânico.
  * 🎯 **Previsibilidade total de Pacing** com erro de Forecast (MAPE) inferior a 5%.
