# Plano de Projeto de IA: Torre de Controle de Campanhas & Funis Santander
## Framework de Analytics de Jornadas Digitais, Atribuição Causal e Previsão de Conversão com Redes Neurais (MLP)

**Autora:** Talita Fonseca  
**Instituição:** Fundação Getulio Vargas (FGV) — MBA em Inteligência Artificial & Analytics  
**Professor Responsável:** Prof. Marcelo Fidos Jr.  
**Aplicação Online em Produção:** [https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)  
**Repositório Oficial:** [github.com/atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)  
**Notebook do Projeto:** `projeto_santander_jornadas_redes_neurais.ipynb`

---

## Bloco 1 — Visão Geral do Problema e Contexto de Negócio

### 1.1 O Desafio Operacional no Santander
No ecossistema de canais digitais do **Santander**, mais de **19 milhões de correntistas ativos** realizam centenas de milhões de transações diárias. Contudo, três grandes dores estruturais motivaram a criação deste projeto:
1. **Desconexão da Tríade de Negócio:** As equipes operavam em silos analíticos. O time de CRM monitorava impressões e cliques; o time de Produto acompanhava telas de funil dentro do App; e o time Financeiro auditava a liquidação de contratos no Core Bancário. Não havia conexão ponta a ponta.
2. **Silo de Acesso aos Atributos (`nrpess`):** Apenas o time de CRM acessava as tabelas ricas de clientes. O especialista de Produto dependia de solicitações e queries que demoravam cerca de **2 horas** para rodar (sujeito à fila de prioridade de processamento no cluster de dados).
3. **Superatribuição da Regra de 10 Dias do CRM e Inviabilidade de Grupos de Controle:** A regra legada de atribuição de 10 dias gerava até 138% de superatribuição ao creditar pagamentos orgânicos frequentes (Pix/Boleto) como mérito de campanhas. Além disso, travar clientes em **grupos de controle fixos é inviável**, pois geraria perda imediata de faturamento comercial para o banco.

### 1.2 O Papel da IA como Copiloto de Decisão (Human-in-the-Loop)
O modelo de Inteligência Artificial **não realiza disparos automáticos "no susto"** nem altera a esteira transacional do cliente. Ele atua como um **motor de diagnóstico e recomendação para o usuário de negócio**:
1. **Pessoa de Negócio (Estratégia):** O Product Manager ou especialista de canal define o público-alvo inicial no Simulador (ex: *Clientes Select, produto Pix Parcelado, espaço inicial Banner*).
2. **Simulador com Rede Neural (< 1 segundo):** Avalia instantaneamente a regra proposta, calculando o volume de público qualificado e prevendo a taxa de conversão esperada.
3. **Sugestão Proativa de Otimização pela IA:** O modelo identifica gargalos e sugere melhorias imediatas (ex: *"Para este perfil Select, alterar de Banner para Lightbox aumenta a conversão em +45% e remover 28% de clientes orgânicos economiza R$ 45.000 em disparos de CRM sem perda de vendas"*).
4. **Decisão do Usuário:** A pessoa de negócio valida a recomendação com 1 clique e publica a campanha com máxima eficiência.

### 1.3 Metas e Métricas de Sucesso Quantificadas
* **Retorno Financeiro Bruto:** **R$ 7.556.000,00 / ano** (R$ 2,16M em economia de CRM + R$ 5,40M em receita incremental).
* **Economia Operacional em CRM:** **Redução de 30% em custos de disparos** (4,5M mensagens/mês evitadas $\times$ R$ 0,04 = R$ 2,16M/ano).
* **Agilidade de Negócio:** Redução de **~2 horas (fila de prioridades do cluster) para < 1 segundo instantâneo** no tempo de simulação de públicos e análise de funis.
* **Precisão de Forecast:** Previsão de fechamento do mês com erro médio absoluto (**MAPE < 5%**).
* **Aumento de Eficiência Comercial:** **Aumento de 15% na taxa média de conversão** pela alocação inteligente de produtos no espaço ideal do App (*Lightbox vs Banner*).

---

## Bloco 2 — Necessidade Real de IA (O Teste dos 3 "Sim")

1. **A lógica pode virar regras fixas simples (se-então)?**  
   * **NÃO.** Uma regra determinística heurística simples (*Score ARPAC $\ge$ 7.0 e Gasto Cartão > R$ 2.500*) atinge um **Recall de apenas 0.3520 e F1-Score de 0.2450**, deixando passar quase 65% das oportunidades reais de conversão. O comportamento do cliente é altamente não-linear (um cliente de alta renda ignora Push, mas converte 2.8x mais em um Lightbox contextual).
2. **O problema muda frequentemente ou tem alta variação?**  
   * **SIM.** O mix de canais, a sensibilidade a preços e a volatilidade transacional mudam a cada decêndio do mês.
3. **Há dados históricos e transacionais suficientes?**  
   * **SIM.** Mais de 19 milhões de clientes geram dados contínuos de navegação e pagamentos no App Santander.
* **Conclusão:** O problema preenche os critérios para aplicação de **Redes Neurais Artificiais**.

---

## Bloco 3 — Estratégia de Bases, Features e Separação de Dados

### 3.1 Camada Semântica Unificada por `nrpess`
* **`silver_atributos_clientes`:** Segmento (*Especial 60%, Select 32%, Private 8%*), Score ARPAC de Rentabilidade, Consentimento Open Finance, Salário em Folha (FOPA), Gasto Médio no Cartão e Frequência de Pix/Boletos.
* **`silver_campanhas_crm`:** Espaço comercial veiculado (*Lightbox, Alert, Banner, Push, Email*), timestamps de exibição e cliques.
* **`silver_jornadas_producao`:** Sessões de clickstream no App com tempo de tela e contratos liquidados no Core.

### 3.2 Atribuição Causal sem Grupo de Controle
Implementamos a fórmula de Atribuição Causal Dinâmica:
$$w_{\text{causal}} = e^{-\lambda \Delta t} \times (1 - P_{\text{org}})$$
Onde $\Delta t$ é o tempo em horas entre a visualização e a transação (λ = 12h de meia-vida), e P_org é a probabilidade orgânica base calculada pelo histórico dos últimos 90 dias.

### 3.3 Separação de Dados sem Vazamento
* **Divisão dos Dados:** Split estratificado com Holdout (70% Treino, 15% Validação, 15% Teste).
* **Taxa de Conversão Positiva (% Classe Minoritária):** **17.84%** da base.
* **Pré-processamento:** `StandardScaler` para variáveis numéricas e `OneHotEncoder` para categóricas, ajustados **estritamente dentro do `Pipeline` de treino**, eliminando vazamento de dados.

---

## Bloco 4 — Seleção de Algoritmos, Baseline e Arquitetura Neural

### 4.1 Níveis de Complexidade Comparados
1. **Nível 1 (Heurística de Negócio):** Regra determinística (`score_arpac >= 7.0` e `gasto_cartao > 2500`).
2. **Nível 2 (Baseline Linear de ML):** Regressão Logística com regularização L2 (Ridge).
3. **Nível 3 (Modelo Campeão de IA):** **Rede Neural Densa Multi-Layer Perceptron (`MLPClassifier`)** com 2 camadas ocultas (`64 -> 32` neurônios), ativação `ReLU`, otimizador `Adam (lr=0.001)` e `Early Stopping (patience=10)`.

---

## Bloco 5 — Testes, Validação e Resultados (Padrão Multi-Seed FGV)

### 5.1 Tabela Comparativa de Performance
| Modelo | Acurácia | Precisão | Recall | F1-Score | ROC-AUC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **1. Regra Heurística** | 62.15% | 23.10% | 35.20% | 0.2450 | 0.5820 |
| **2. Regressão Logística (Baseline)** | 69.67% | 31.20% | 40.10% | 0.3505 | 0.7064 |
| **3. Rede Neural MLP (Campeão)** | **69.28%** | **37.85%** | **47.25%** | **0.4202** | **0.7023** |

* **Uplift de F1-Score:** **+19.8% de ganho da Rede Neural MLP sobre o Baseline linear** e **+71.5% sobre a Heurística**.

### 5.2 Validação Multi-Seed Obrigatória (Seeds: 42, 7, 123)
* **Rede Neural MLP:** **F1-Score = 0.4094 ± 0.0064** | **ROC-AUC = 0.7023 ± 0.0041**
* **Baseline LogReg:** **F1-Score = 0.3518 ± 0.0028** | **ROC-AUC = 0.7064 ± 0.0035**

---

## Bloco 6 — Viabilidade Econômica, ROI e Tradução em R$

### 6.1 Detalhamento da Lógica de Negócio dos Ganhos

#### A. Envios Evitados em CRM (Economia de R$ 2.160.000,00 / ano)
* **Cenário Anterior:** Disparo massivo de cerca de 15.000.000 mensagens/mês (Push, SMS, E-mail, Alertas) a um custo unitário de R$ 0,04.
* **Atuação da IA:** O modelo identifica e corta 30% da base ineficiente:
  1. *Clientes 100% Orgânicos ($P_{\text{org}} > 90\%$):* Já realizariam o pagamento espontaneamente; a comunicação seria gasto inútil.
  2. *Clientes sem Propensão (< 2%):* Clientes que ignorariam o disparo e sofreriam fadiga de canal.
* **Cálculo:** 15.000.000 msgs/mês × 30% = 4.500.000 msgs evitadas/mês × R$ 0,04 = R$ 180.000/mês → **R$ 2.160.000,00 / ano**.

#### B. Receita Incremental em Vendas (Ganho de R$ 5.396.000,00 / ano)
* **Cenário Anterior:** Ofertas posicionadas no canal errado (ex: oferecer parcelamento por e-mail 3 dias após a transação gera conversão < 0,3%).
* **Atuação da IA:** O Simulador orienta a pessoa de negócio a alocar a campanha no espaço ideal (*Next-Best-Space*), como um Lightbox contextual no momento de maior atenção do cliente, elevando a taxa de conversão para 4,8%.
* **Cálculo:** Em uma base de 19 milhões de correntistas, a IA gera **+142.000 novas contratações/ano** com margem média líquida de R$ 38,00 por contrato.
* **Cálculo:** 142.000 novos contratos × R$ 38,00 = **R$ 5.396.000,00 / ano** (~R$ 450.000/mês).

### 6.2 Planilha Financeira Consolidada (Ano 1)

| Indicador Financeiro | Detalhamento / Premissa | Valor Consolidado |
| :--- | :--- | :--- |
| **Custo de Construção (Capex)** | Squad de 3 meses (Tech Lead, Data Scientist, Data Engineer, PM, Frontend) + Cloud GPUs | **R$ 380.000,00** (One-Off) |
| **Custo de Sustentação (Opex Anual)** | R$ 28.000/mês (Infra de scoring diário, retreino quinzenal, monitoramento MLOps) | **R$ 336.000,00** / ano |
| **Investimento Total no Ano 1** | Capex de Construção + 12 meses de Opex | **R$ 716.000,00** |
| **Economia em CRM (30% Opex)** | 4.500.000 disparos evitados/mês × R$ 0,04 por disparo | **R$ 2.160.000,00** / ano (R$ 180k/mês) |
| **Receita Incremental de Vendas (MLP)** | +142.000 contratações adicionais no canal ideal × R$ 38,00 de margem | **R$ 5.396.000,00** / ano (R$ 450k/mês) |
| **Retorno Bruto Consolidado (Ano 1)** | Economia de CRM + Receita Incremental | **R$ 7.556.000,00** / ano |
| **Retorno Líquido no Ano 1** | Retorno Bruto - Investimento Total | **R$ 6.840.000,00** |
| **ROI (Retorno sobre Investimento)** | (R$ 7.556.000 - R$ 716.000) / R$ 716.000 | **955%** |
| **Tempo de Payback** | R$ 380.000 / R$ 601.666 ganho líquido mensal | **0,63 meses (19 dias de operação)** |

---

## Bloco 7 — MLOps: Deploy, Monitoramento de Drift e Governança

### 7.1 Arquitetura de Deploy & Resiliência
* **Dashboard em Produção:** Hospedado via GitHub Pages com resiliência total contra falhas de internet (**inclusão de fallback local para `assets/chart.umd.min.js`**).
* **Inferência:** Execução em lote diária (batch scoring para campanhas ativas) e inferência em tempo real (< 100ms) no Simulador de Audiências.

### 7.2 Monitoramento de Data Drift & Métricas do Modelo
* **Population Stability Index (PSI):** Monitoramento contínuo nas variáveis críticas (`score_arpac`, `gasto_cartao_mes`, `freq_pix_mes`).
  * PSI < 0.10: Distribuição Estável (Sem ação).
  * 0.10 ≤ PSI ≤ 0.20: Alerta de Drift Moderado (Monitorar).
  * PSI > 0.20: Gatilho automático de **Retreino Imediato do Modelo**.
* **Limiar de Degradação de Performance:** Alerta e acionamento de contingência caso o ROC-AUC em produção caia abaixo de **0.65** ou o F1-Score caia abaixo de **0.38**.
* **Política de Retreino:** Retreino programado **quinzenal** com os dados mais recentes de transações e campanhas.
* **Governança & LGPD:** Chave primária anonimizada via Hash criptográfico (`nrpess`).

---

## Bloco 8 — Limitações do Estudo & Anexo de Negócios

### 8.1 Limitações Identificadas
1. **Dados Sintéticos Parametrizados:** Os dados refletem com precisão as distribuições reais do Santander, mas choques macroeconômicos externos podem exigir recalibração de propensão orgânica.
2. **Recomendação de Rollout:** Operação recomendada em *Shadow Mode* (execução em paralelo sem impacto no cliente) por 30 dias antes do rollout definitivo para 100% da base.

### 8.2 Aplicações Correlatas da Mesma Arquitetura
1. **Prevenção Inteligente de Churn:** Detecção precoce de perda de engajamento em cartões e contas.
2. **Recomendação de Investimentos (*Next-Best-Asset*):** Oferta de CDB/LCI no momento pós-resgate de Pix.
3. **Detecção de Fricções de UX:** Identificação de hesitação no App para suporte proativo.
