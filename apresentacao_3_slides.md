# 📊 Roteiro de Apresentação Executiva (Padrão Gamma.app / 3 Slides)
## Projeto: Torre de Controle de Jornadas e Previsor de Público & Resultado com Redes Neurais no Santander
**Autora:** Talita Fonseca | **Curso:** MBA em Inteligência Artificial & Analytics (FGV)

---

# Slide 1: Do Disparo no Escuro à Previsão Exata: A Nova Gestão de Públicos e Funis no Santander
**Subtítulo:** Como o time de Produto pode simular audiências, prever vendas por espaço e unificar a jornada sem depender de processos manuais lentos.

* **O Desafio Atual (Disparos no Escuro):** Hoje, o time de Produto tem o domínio do negócio para criar campanhas e ofertas, mas não consegue estimar com precisão o **tamanho real do público elegível** nem **prever o volume de vendas** que a campanha vai gerar antes de colocá-la no ar.
* **A Limitação Atual (Silos e Falta de Padrão):** As bases ricas de atributos dos 19M de clientes ficam isoladas no CRM. Para testar um público, o time depende de solicitações manuais que demoram semanas, gerando funis isolados que não se conectam com a produção e métricas de atribuição distorcidas.
* **A Visão com Inteligência Artificial (O Previsor Inteligente):** Uma plataforma onde o time de Produto seleciona atributos, a IA calcula o **Sizing do público na hora**, prevê a **taxa de conversão em cada espaço do app** e projeta o **resultado final de vendas**, conectando Campanha $\rightarrow$ Funil $\rightarrow$ Produção.

---

# Slide 2: Por Dentro da Solução: O Simulador Preditivo de Público, Espaços e Resultado
**Subtítulo:** Como a Rede Neural e a Engenharia de Dados transformam o Dicionário de Atributos em previsões precisas de vendas.

* **Nossos Dados (Dicionário de Atributos Integrado):** Unificação da base de atributos de clientes (gastos, saldo, score, hábitos de Pix/Boleto e nível de engajamento) com os eventos de navegação no app e a produção via `nrpess`.
* **A Abordagem Inteligente (A IA como Previsora de Vendas):** 
  * *Previsor de Público & Sizing Instantâneo:* O especialista de produto combina atributos no painel e a ferramenta calcula imediatamente o tamanho do público elegível.
  * *Previsor de Resultado por Espaço:* A Rede Neural (MLPClassifier) cruza o perfil do público com a vocação de cada espaço do app (Home, Pós-Pix, Push) e prevê a taxa de conversão e o faturamento esperado antes do disparo.
  * *Atribuição Causal por Desconto Orgânico:* Mede o ganho incremental real descontando o comportamento que o cliente já teria sozinho, sem necessidade de travar grupos de controle.
* **Integração Fluida (Self-Service de Produto):** O especialista de produto simula cenários em minutos, escolhe o espaço de maior retorno e envia a audiência validada diretamente para execução do CRM.

---

# Slide 3: Impacto de Negócio: Pacing em Tempo Real, Forecast e Decisões Ágeis
**Subtítulo:** Metas batidas com previsibilidade, fim do retrabalho analítico e economia de custos de CRM.

* **Impacto Esperado:** 
  * **Previsibilidade Total:** Saber exatamente quantas vendas cada campanha vai gerar antes de gastar orçamento.
  * **Agilidade Máxima:** Redução de **3 semanas para segundos** na montagem de públicos e criação de funis.
  * **Torre de Pacing & Forecast:** Acompanhamento do ritmo diário de cada funil (MTD) com projeção de fechamento do mês (MAPE < 5%) e recomendação de campanhas para cobrir gaps.
  * **Economia de 30% em CRM:** Fim do disparo de mensagens para clientes com altíssima probabilidade orgânica.
* **Validação Segura:** Modelo com **ROC-AUC superior a 0.88**, estabilidade comprovada na validação Multi-Seed oficial da FGV (seeds: 42, 7, 123) e período de testes em *Shadow Mode*.
* **Próximos Passos (Roadmap):**
  1. *Mês 1:* Piloto do Previsor de Público e Espaços para os funis de Pix e Boletos.
  2. *Mês 2:* Implementação da Torre de Pacing e Forecast MTD.
  3. *Mês 3:* Expansão para produtos de Cartões, Seguros e Empréstimos.
