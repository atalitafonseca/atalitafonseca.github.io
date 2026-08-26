# Arquitetura e Planejamento: Projeto de IA no Santander
## Framework de Analytics de Jornadas Digitais, Atribuição Incremental e Forecast Preditivo com Redes Neurais

**Autora:** Talita Fonseca  
**Instituição:** Fundação Getulio Vargas (FGV) — MBA em Inteligência Artificial & Analytics  
**Repositório Oficial:** [atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)  
**Notebook do Projeto:** 

---

## 1. Visão Geral do Problema e Objetivo

### O Problema de Negócio:
No ecossistema de canais digitais do **Santander**, mais de **19 milhões de clientes ativos diários** interagem no aplicativo bancário, gerando centenas de milhões de eventos de navegação. Três grandes dores operacionais e estratégicas foram identificadas:
1. **Desconexão da Tríade de Negócio:** As equipes trabalham em silos. O time de CRM monitora impressões e cliques; o time de Produto analisa passos do funil no app; e o time Financeiro acompanha vendas/contratações na produção. Não há uma visão unificada de ponta a ponta.
2. **Silo de Acesso a Dados e Falta de Padronização:** O time de CRM é o único que possui acesso direto às tabelas ricas de atributos dos clientes (). O time de Produto, que tem o domínio do negócio para desenhar campanhas, depende de solicitações manuais lentas. Além disso, cada analista cria queries em SQL isoladas, demorando **semanas** para colocar um dashboard no ar sem qualquer padrão ou histórico.
3. **Superatribuição de CRM e Inviabilidade Prática de Grupos de Controle:** Como as campanhas no aplicativo são disparadas para a base total sem retenção de clientes em grupos de controle (para não travar receita imediata), a regra legada de 10 dias do CRM gera falsos positivos ao creditar pagamentos orgânicos frequentes (Pix/Boleto) como mérito de marketing.

### O Objetivo da IA:
Construir uma **Torre de Controle Unificada de Jornadas, Campanhas e Produção** equipada com:
* **Copiloto de Audiências e Sizing Preditivo:** Permite ao time de Produto explorar o Dicionário de Atributos de CRM e simular o impacto de campanhas de forma *self-service*.
* **Motor de Atribuição Causal sem Grupo de Controle:** Calcula a atribuição líquida descontando a probabilidade orgânica individual do cliente ( = e^{-\lambda \Delta t} 	imes (1 - P_{	ext{org}})$), permitindo mensurar a incrementalidade real mesmo quando 100% da base é impactada.
* **Modelo Preditivo com Redes Neurais (MLPClassifier):** Estima a probabilidade de conversão do cliente cruzando *(Engajamento do Cliente $	imes$ Vocação do Espaço Comercial $	imes$ Fricção do Funil $	imes$ Produto)*.
* **Torre de Ritmo (Pacing MTD) e Forecast Preditivo:** Acompanhamento do ritmo diário de cada produto e projeção de fechamento do mês.

### Métrica de Sucesso (Negócio):
* **Redução de semanas para < 3 segundos** no tempo de extração de relatórios e funis (via Camada Medallion Gold/Platinum).
* **Redução de 30% em custos de disparos de CRM** ao evitar mensagens para clientes que converteriam organicamente.
* **Precisão de Forecast com erro médio absoluto (MAPE) < 5%** no fechamento do mês.
* **Aumento de 15% na taxa de conversão** em produtos alocados nos espaços de maior vocação contextual.

---

## 2. Coleta e Preparação de Dados

### Fontes de Dados Integradas via :
1. **Dicionário de Atributos do Cliente ():**
   * Chave:  (Número de Pessoa / Hash do cliente).
   * Variáveis: Segmento (*Varejo, Especial, Select, Private*), idade, score interno, saldo médio em conta, gasto médio no cartão, frequência de Pix no mês, frequência de boletos e **Cluster de Engajamento** (*Muito Engajado, Moderado, Baixo*).
2. **Base de Campanhas e Espaços Comerciais ():**
   * Variáveis: , ,  (*Banner Home, Pós-Pix, Push, Carrossel*), , timestamps de visualização e clique por .
3. **Base de Clickstream e Produção ():**
   * Variáveis: , ,  (*campo_unico, camera, atalho_direto*), tempo de tela, produto transacionado e status de liquidação ().

### Tratamento e Engenharia de Features:
* **Resolução de Identidade Determinística:** Cruzamento das 3 bases exclusivamente pelo identificador único .
* **Cálculo da Probabilidade Orgânica Base ({	ext{org}}$):** Estimada a partir do histórico de transações nos últimos 90 dias.
* **Peso de Atribuição Causal Dinâmica ({	ext{causal}}$):**
  12282w_{	ext{causal}} = e^{-\lambda \Delta t} 	imes (1 - P_{	ext{org}})12282
  Onde $\Delta t$ é o tempo decorrido em horas entre a interação no espaço e a transação (meia-vida $\lambda = 12h$).
* **Score de Engajamento Comportamental:** Variável contínua normalizada combinando frequência de acessos, variedade de produtos e volume financeiro.
* **Razão de Hesitação na Jornada:** Tempo de tela do usuário dividido pela média do seu segmento.

---

## 3. Estratégia de Bases e Separação de Dados

### Arquitetura de Armazenamento Medallion (Lakehouse):
* **Bronze:** Logs brutos de clickstream (> 500M eventos/dia) armazenados em Delta/Parquet particionados por data e hora (sem queries de analistas).
* **Silver:** Eventos canônicos particionados por  e .
* **Gold:** Sessões consolidadas unificando a Tríade (Campanha $ightarrow$ Funil $ightarrow$ Produção) em 1 linha por sessão de usuário (~19M linhas/dia).
* **Platinum / Marts:** Tabelas pré-agregadas (< 10.000 linhas) para consumo instantâneo de **Ritmo do Mês (Pacing)** e **Forecast**.

### Separação dos Dados (Split) e Prevenção de Vazamento:
* **Metodologia de Split:** Divisão temporal e estratificada em **70% Treino**, **15% Validação** e **15% Teste**.
* **Prevenção de Data Leakage:** A engenharia de features de engajamento do cliente é calculada estritamente com dados anteriores ao timestamp da sessão (). Variáveis de produção e liquidação não entram na matriz de features ($).

---

## 4. Seleção de Algoritmos

### Modelo Baseline:
* **Regressão Logística com Regularização L2 (Ridge):** Modelo linear clássico, rápido e interpretável, servindo como régua de comparação mínima exigida pela FGV.

### Modelo Avançado de IA:
* **Rede Neural Densa (Multi-Layer Perceptron - MLPClassifier):**
  * **Arquitetura da Rede:**
    * *Entrada:* Vetor de features pré-processadas (numéricas escaladas com  e categóricas com ).
    * *Camada Oculta 1:*  +  para controle de overfitting.
    * *Camada Oculta 2:*  + .
    * *Camada de Saída:*  fornecendo a probabilidade $\hat{p} \in [0, 1]$.
* **Justificativa Técnica:** As interações entre o perfil de engajamento do cliente, a vocação do espaço comercial no app e a fricção do funil são altamente não-lineares. Redes Neurais densas possuem capacidade universal de aproximação para mapear essas sinergias complexas sem necessidade de feature engineering manual exaustivo.

---

## 5. Estratégia de Treinamento e Otimização

* **Função de Perda (*Loss Function*):** *Binary Cross-Entropy Loss*:
  12282\mathcal{L} = -rac{1}{N} \sum_{i=1}^N \left[ y_i \log(\hat{p}_i) + (1 - y_i) \log(1 - \hat{p}_i) ight]12282
* **Otimizador:** *Adam* (Adaptive Moment Estimation) com taxa de aprendizado inicial $lpha = 0.001$.
* **Batch Size e Épocas:** Treinamento em mini-batches de 128 amostras por até 100 épocas.
* **Critério de Parada (*Early Stopping*):** Monitoramento da perda na base de validação () com paciência (*patience*) de 10 épocas, restaurando os melhores pesos.

---

## 6. Testes, Validação e Métricas

### Métricas de Avaliação Técnica:
* **ROC-AUC (Área sob a Curva ROC):** Métrica primária para ordenação e calibração de probabilidades.
* **Acurácia, Precisão, Recall e F1-Score:** Avaliados no limiar ótimo de decisão ($	au = 0.5$).

### Matriz de Confusão e Impacto de Negócio:
* **Falso Positivo (FP):** Recomendar oferta para cliente que não contrataria $ightarrow$ Custo: Ligeiro aumento na fadiga de comunicação.
* **Falso Negativo (FN):** Deixar de ofertar no espaço ideal para quem contrataria $ightarrow$ Custo: Perda direta de receita para o banco (Custo de Oportunidade).
* *Estratégia:* Calibração do modelo para maximizar o **Recall** mantendo Precisão $> 85\%$.

### Validação de Robustez Multi-Seed (Padrão Oficial FGV):
O pipeline é executado em **3 sementes aleatórias distintas (, , )**, reportando média e desvio-padrão das métricas para comprovar que a Rede Neural não sofre de instabilidade estocástica.

---

## 7. MLOps: Deploy, Governança e Monitoramento

### Arquitetura de Implantação e Consumo:
* **Camada de Simulação de Produto (Batch / On-Demand):** O modelo é empacotado em pipeline scikit-learn/Keras, permitindo que o time de Produto simule audiências e preveja resultados de espaços em segundos.
* **Torre de Pacing & Forecast Diário:** Pipeline automatizado em batch noturno que calcula a curva de ritmo do mês (MTD) e projeta o fechamento da produção para cada funil.

### Governança e Segurança (LGPD):
* Desacoplamento entre dados sensíveis e camada analítica: O time de Produto acessa métricas agregadas e tokens; o CRM mantém a custódia das regras de contato.
* Linhagem de Dados (*Data Lineage*) e versionamento de modelos com *MLflow*.

### Monitoramento de Deriva (*Drift Monitoring*):
* **Data Drift:** Teste de Kolmogorov-Smirnov diário nas variáveis de gastos e engajamento dos clientes.
* **Concept Drift:** Monitoramento semanal do erro de calibração das probabilidades ( Score$) e re-treinamento programado a cada 30 dias.

---

## 8. Anexo: Outras Aplicações de Negócio com a Mesma Técnica

A mesma arquitetura de **Rede Neural Densa (MLP) com Atribuição Causal e Segmentação de Engajamento** desenvolvida neste projeto pode ser diretamente adaptada para:

1. **Prevenção Inteligente de Evasão (Churn em Contas e Cartões):**
   * *Aplicação:* Mapear clientes com queda gradual de engajamento no app e acionar ofertas de retenção personalizadas no espaço de maior afinidade antes do encerramento da conta.
2. **Recomendação de Produtos de Investimentos (Next-Best-Asset):**
   * *Aplicação:* Identificar o momento exato em que o saldo em conta do cliente atinge liquidez ociosa e ofertar CDB/Fundos no momento pós-resgate ou no extrato.
3. **Detecção e Prevenção de Fricções de UX em Tempo Real:**
   * *Aplicação:* Monitorar padrões de hesitação e erros em campos de input no app, disparando canais de suporte ou simplificação de rota antes do abandono do cliente.
