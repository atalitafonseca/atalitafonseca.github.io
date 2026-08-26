# Arquitetura e Planejamento: Projeto de IA no Santander
## Torre de Controle de Jornadas Digitais, Atribuição Causal e Previsor de Público & Resultado com Redes Neurais

**Autora:** Talita Fonseca  
**Instituição:** Fundação Getulio Vargas (FGV) — MBA em Inteligência Artificial & Analytics  
**Professor Responsável:** Prof. Marcelo Fidos Jr.  
**Repositório Oficial:** [atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)  
**Aplicação Online:** [https://atalitafonseca.github.io/](https://atalitafonseca.github.io/)  
**Notebook do Projeto:** `projeto_santander_jornadas_redes_neurais.ipynb`

---

## 1. Visão Geral do Problema e Contexto de Negócio

### O Desafio Operacional no Santander:
No ecossistema de canais digitais do **Santander**, mais de **19 milhões de clientes ativos diários** interagem no aplicativo bancário, gerando centenas de milhões de eventos de navegação diários. Três grandes dores operacionais e estratégicas motivaram este projeto:
1. **Desconexão da Tríade de Negócio:** As equipes operam em silos analíticos. O time de CRM monitora impressões e cliques; o time de Produto analisa telas e fricções dentro do app; e o time Financeiro acompanha a liquidação de contratos no Core Bancário. Não havia um elo comum de ponta a ponta.
2. **Silo de Acesso a Dados e Falta de Padronização:** Apenas o time de CRM possui acesso direto às tabelas ricas de atributos dos clientes (`nrpess`). O time de Produto, que tem o domínio de negócio para desenhar campanhas e ofertas, depende de solicitações manuais que demoram **semanas**. Além disso, cada analista cria queries em SQL isoladas, sem histórico ou governança.
3. **Superatribuição de CRM e Inviabilidade de Grupos de Controle:** A regra legada de atribuição de 10 dias do CRM gera falsos positivos ao creditar pagamentos orgânicos frequentes (Pix/Boleto) como mérito de marketing. Além disso, reter clientes em **grupos de controle fixos é comercialmente inviável**, pois priva o banco de faturamento imediato.

### O Objetivo da Solução de IA:
Construir uma **Torre de Controle Unificada de Campanhas, Funis e Produção** equipada com:
* **Simulador de Audiências e Sizing Preditivo:** Permite ao especialista de Produto explorar o Dicionário de Atributos de CRM, ligar/desligar alavancas estratégicas (`[ 🟢 Ativo | ⚪ Desativo ]`) e simular o impacto no tamanho do público elegível na hora.
* **Seletor de Objetivo Estratégico:** Modulação instantânea entre o **Modo Conversão & Vendas** (foco em contratos liquidados) e o **Modo Awareness & Alcance de Marca** (foco em pessoas únicas alcançadas, frequência de exibição e CPM).
* **Motor de Atribuição Causal sem Grupo de Controle:** Calcula a atribuição líquida descontando a probabilidade orgânica individual do cliente ($w = e^{-\lambda \Delta t} \times (1 - P_{\text{org}})$).
* **Modelo Preditivo com Redes Neurais (MLPClassifier):** Estima a probabilidade de conversão do cliente cruzando *(Engajamento do Cliente $\times$ Vocação do Espaço Comercial $\times$ Fricção do Funil $\times$ Produto)*.
* **Torre de Ritmo (Pacing MTD) e Forecast Preditivo:** Acompanhamento do ritmo diário de vendas comparando o Mês Atual Realizado (D1-20), Forecast IA (D21-31), Mês Anterior Fechado (D1-31) e Meta Contratada.

### Métricas de Sucesso (Negócio):
* **Redução de 3 semanas para < 1 segundo** no tempo de simulação de públicos e extração de funis.
* **Redução de 30% em custos de disparos de CRM** ao evitar mensagens para clientes que converteriam organicamente.
* **Precisão de Forecast com erro médio absoluto (MAPE) < 5%** no fechamento do mês.
* **Aumento de 15% na taxa de conversão** em produtos alocados nos espaços de maior vocação contextual (ex: Lightbox vs Banner).

---

## 2. Coleta, Engenharia de Features e Modelagem

### Fontes de Dados Integradas via `nrpess`:
1. **Dicionário de Atributos do Cliente (`silver_atributos_clientes`):**
   * Chave: `nrpess` (Hash identificador do cliente).
   * Variáveis: Segmento (*Especial, Select, Private*), Score ARPAC de Rentabilidade, Outflow/Evasão de recursos para fintechs, Consentimento Open Finance, Conta Salário / FOPA, Gasto Médio no Cartão e Frequência de Pix.
2. **Base de Campanhas e Espaços Comerciais (`silver_campanhas_crm`):**
   * Variáveis: `id_campanha`, `nome_do_produto`, `espaco_veiculacao` (*Lightbox, Alert, Banner, Push, Email*), timestamps de visualização e clique por `nrpess`.
3. **Base de Clickstream e Produção (`silver_jornadas_producao`):**
   * Variáveis: `session_id`, `nrpess`, etapas de navegação (visualização, clique, entrada app, simulação, ID Santander, fechamento app) e liquidação efetiva no Core Bancário (`producao_core_flag`).

### Engenharia de Features e Atribuição Causal:
* **Probabilidade Orgânica Base ($P_{\text{org}}$):** Estimada a partir do histórico de transações nos últimos 90 dias.
* **Peso de Atribuição Causal Dinâmica ($w_{\text{causal}}$):**
  $$w_{\text{causal}} = e^{-\lambda \Delta t} \times (1 - P_{\text{org}})$$
  Onde $\Delta t$ é o tempo decorrido em horas entre a interação no espaço e a transação (meia-vida $\lambda = 12h$).

---

## 3. Seleção de Algoritmos e Resultados (Requisito FGV)

### Modelo Baseline vs Rede Neural MLP:
* **Baseline:** Regressão Logística com regularização L2 (referência linear interpretável).
* **Modelo Campeão:** Rede Neural Densa (*Multi-Layer Perceptron - MLPClassifier*) estruturada com:
  * *Entrada:* Vetor de features pré-processadas (One-Hot Encoding para categóricas e StandardScaler para numéricas).
  * *Camada Oculta 1:* `Dense(64, activation='relu')` + `Dropout(0.2)`.
  * *Camada Oculta 2:* `Dense(32, activation='relu')` + `Dropout(0.1)`.
  * *Camada de Saída:* `Dense(1, activation='sigmoid')`.

### Resultados Comparativos Oficiais:
| Métrica | Baseline (Regressão Logística) | Rede Neural (MLP) | Ganho / Impacto |
| :--- | :---: | :---: | :---: |
| **ROC-AUC** | 0.7064 | **0.7023** | Alta discriminação |
| **Acurácia** | 69.67% | **69.28%** | Estabilidade de predição |
| **F1-Score** | 0.3505 | **0.4202** | **+19.8% de Uplift na classe positiva** |
| **Validação Multi-Seed (FGV)** | 0.3518 ± 0.0028 | **0.4094 ± 0.0064** | **Comprovada estabilidade estocástica** |

---

## 4. Estrutura do Dashboard Interativo (Torre de Controle)

O dashboard oficial ([atalitafonseca.github.io](https://atalitafonseca.github.io/)) organiza as decisões executivas em 4 abas estruturadas:

1. **🎯 1. Simulador & Botão Calcule IA:**
   * Seletor de Objetivo: **Conversão & Vendas** vs **Awareness & Alcance de Marca**.
   * Construtor de Públicos com Segmentos (*Especial, Select, Private*) e Regras de Hábito.
   * Alavancas da IA com switches interativos `[ 🟢 Ativo | ⚪ Desativo ]` e botão `[ ✕ ]` para dispensar recomendações.
   * Gráfico de Canais com **Rótulos Numéricos Diretos (Datalabels)** no topo de cada barra.
   * Dicionário de Atributos Santander com busca inteligente em linguagem natural.

2. **🔗 2. A Tríade: Funil, Visão Mensal & Reconciliação:**
   * Funil de 7 etapas da visualização no espaço comercial até a produção no Core Bancário.
   * Variação Mês a Mês (MoM 2026) com badges de crescimento e pontos de atenção.
   * Reconciliação de perdas técnicas entre App e Core Bancário (Antifraude, Saldo insuficiente e Time-out).

3. **⏱️ 3. Torre de Pacing MTD (Comparativo Mês a Mês & IA):**
   * Diagnóstico Executivo de Pacing com status (*🟢 Ritmo Forte* vs *⚠️ Abaixo da Meta*) e recomendação de alavanca de CRM.
   * Gráfico comparativo dia a dia (Dias 1 a 31) com 4 curvas: Mês Atual Realizado (1-20), Forecast IA (21-31), Mês Anterior Completo (1-31) e Meta Linear.
   * Tabela de acompanhamento por decêndios (1º, 2º e 3º Decêndio).

4. **🧠 4. Performance da Rede Neural (FGV):**
   * Curva ROC comparativa e tabela oficial de métricas e validação Multi-Seed.

---

## 5. Instruções de Compartilhamento e Acesso

* **Link Público da Aplicação:** `https://atalitafonseca.github.io/`
* **Repositório GitHub:** `https://github.com/atalitafonseca/atalitafonseca.github.io`
* **Como Executar o Notebook:**
  1. Faça o clone do repositório: `git clone https://github.com/atalitafonseca/atalitafonseca.github.io.git`
  2. Abra o arquivo `projeto_santander_jornadas_redes_neurais.ipynb` no Jupyter Notebook, VS Code ou Google Colab.
  3. Execute todas as células (`Kernel -> Restart & Run All`).
