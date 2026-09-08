# Análise v2 — Talita Fonseca (`atalitafonseca`)

**Projeto:** Torre de Controle de Campanhas & Funis Santander — atribuição de marketing e previsão de conversão em jornadas de pagamento (Pix/Boleto) com Rede Neural MLP
**Repositório:** [atalitafonseca/atalitafonseca.github.io](https://github.com/atalitafonseca/atalitafonseca.github.io)
**Arquivos analisados (v2):** `projeto_santander_jornadas_redes_neurais.ipynb`, `projeto_santander_jornadas_atribuicao.ipynb`, `plano_projeto_santander_ia.md`, `README.md`, `apresentacao_3_slides.md`, `index.html`, `assets/chart.umd.min.js`
**Nota v1:** 4,8/10 → **Nota v2: 8,0/10**

---

## 1. Abertura

Talita, esta v2 traz avanço técnico real e verificável nos pontos que mais pesavam contra você na v1: reexecutei o notebook principal do zero, em ambiente completamente limpo, e todos os números bateram — inclusive testei o fallback do Chart.js com um navegador de verdade, bloqueando a CDN de propósito, e ele funcionou nos dois sentidos. Isso é progresso genuíno, não cosmético. Mas a verificação encontrou que o problema central da v1 — números "oficiais" sem rastreabilidade — não desapareceu, só mudou de lugar: o README, o plano e a apresentação continuam citando os números antigos do modelo (de antes de você rodar o notebook de verdade), e a alegação mais visível de todo o material de negócio — "superatribuição de até 138%" — continua sem nenhum cálculo por trás dela em lugar nenhum do repositório.

## 2. O que mudou desde a v1 (verificado com reexecução completa, em ambiente limpo, dos dois notebooks)

Reexecutei `projeto_santander_jornadas_redes_neurais.ipynb` do zero (venv novo, versões de biblioteca mais recentes que as originais) e todos os números bateram exatamente, casa decimal por casa decimal — inclusive testei o dashboard com Chrome headless (Puppeteer), bloqueando a CDN de propósito, para confirmar o fallback na prática, não só lendo o código.

| Item da v1 | Situação na v1 | O que você fez na v2 | Verificado |
|---|---|---|---|
| **7.1/9.1** Notebooks sem `execution_count`/outputs salvos | Impossível confirmar se os números "oficiais" vieram de execução real | Notebook principal reexecutado, `execution_count` sequencial 1→9, outputs reais em todas as células | Reexecutado do zero em ambiente independente — 100% dos números batem (tabela comparativa e multi-seed). |
| **2.1-2.4** ROI ausente | Nenhuma estimativa de custo/retorno | Capex R$380.000, Opex R$28.000/mês, retorno R$7.556.000/ano, ROI 955,3%, payback 19 dias | Aritmética conferida linha a linha — bate exatamente, e os mesmos números aparecem de forma consistente em README, plano e apresentação (diferente do problema descrito abaixo). |
| **3.1/4.2** Sem alternativa determinística para o problema de conversão | Discussão só cobria o problema de atribuição, não o de predição | Comparação de 3 níveis: heurística (`score_arpac≥7,0 & gasto>R$2.500`) vs. LogReg vs. MLP, no mesmo split | Threshold confirmado sem vazamento (fixo a priori, não ajustado olhando o teste); resolve genuinamente a lacuna da v1. |
| **6.2** MLOps sem métrica de modelo | Descrevia dashboard de pacing de vendas | Monitoramento de Data Drift via PSI, cadência de retreino (15 dias), gatilhos numéricos (PSI>0,20, AUC<0,65) | PSI reproduzido exatamente — ressalva: compara treino/teste do mesmo sorteio sintético, então um PSI baixo é quase garantido por construção, não uma simulação de drift genuíno (a fórmula em si está correta). |
| **7.2** Plano dizia "split temporal", código usava split aleatório | Divergência entre documentação e código | Plano corrigido para "Split estratificado com Holdout" — sem mais menção a "temporal" | Você optou por corrigir o texto para refletir o código real, em vez de forçar um split temporal artificial — é a forma honesta de resolver essa divergência. |
| **9.2** Sem seção de limitações | Ausente | Seção de limitações presente (dados sintéticos, recomendação de *shadow mode* de 30 dias) | Presente e honesta, embora breve — ver ressalva abaixo. |
| **7.1 (dashboard)** Chart.js sem fallback, quebra sem CDN | Dependência única de CDN externa | `assets/chart.umd.min.js` local + lógica de fallback bidirecional em `index.html` | Testado de verdade com Chrome headless: CDN bloqueada → carrega local; arquivo local bloqueado → busca CDN. Funciona nos dois sentidos. |

## 3. O problema que persiste — o mesmo tipo de achado da v1, agora nos documentos de apoio

O notebook principal agora é uma fonte confiável — mas os documentos que normalmente seriam lidos primeiro (README, plano de projeto, apresentação) não foram atualizados com os números que esse notebook realmente produz.

| | Heurística (Acc/F1/AUC) | LogReg (Acc/F1/AUC) | MLP (Acc/F1/AUC) |
|---|---|---|---|
| **README / plano / apresentação** | 62,15% / 0,2450 / 0,5820 | 69,67% / 0,3505 / 0,7064 | 69,28% / 0,4202 / 0,7023 |
| **Notebook (reexecutado, confirmado)** | 57,20% / 0,3869 / 0,5339 | 65,66% / 0,4695 / 0,6679 | 65,16% / 0,4232 / 0,6615 |

Os três documentos ainda citam os números "antigos" (os mesmos que a v1 já tinha questionado por falta de rastreabilidade) — não os que o notebook realmente produz hoje, já verificados como reais. O mesmo vale para a validação multi-seed (documentos citam MLP F1=0,4094±0,0064; o notebook real produz F1=0,4359±0,0256) e para a taxa de conversão (plano cita 17,84%; o notebook gera 40,48%).

Some-se a isso a alegação mais visível de todo o material de negócio: "superatribuição de até 138%" (plano e apresentação). Busquei esse número em todo o repositório — ele não aparece em nenhum cálculo do `projeto_santander_jornadas_atribuicao.ipynb`, que tem só 3 células e gera uma curva de decaimento genérica com uma constante fixa (`p_org_media = 0.65`, não calculada a partir de nenhum histórico, ao contrário do que o próprio plano descreve — "calculada pelo histórico dos últimos 90 dias").

Isso não é o mesmo problema de honestidade da v1 (lá, não havia evidência alguma de execução; aqui, o notebook principal é genuinamente executado e reprodutível) — mas é o mesmo *tipo* de problema: números apresentados como oficiais que não correspondem ao que o código realmente produz. Antes de qualquer nova rodada, os documentos precisam ser regenerados a partir da execução real, e o número de 138% precisa vir de algum cálculo real ou ser removido/qualificado como estimativa.

## 4. Nota por critério (atualizada)

### Critérios de negócio (peso maior)

**1. Aderência ao negócio — 10,0/10** *(v1: 7,5/10)*
1.3. Conexão entre métrica técnica e impacto de negócio: **5** *(v1: 2)* — a seção de ROI conecta explicitamente o ganho de F1/AUC a R$ (142 mil contratações × R$38 de margem = R$5,396M/ano).

**2. Viabilidade econômica (ROI) — 8,75/10** *(v1: 1,25/10)*
2.1/2.2. Custo de construção/sustentação: **4/4** *(v1: 1/1)* — R$380.000 (Capex) e R$28.000/mês (Opex), estimativas presentes e razoáveis.
2.3. Retorno esperado: **5** *(v1: 3)* — R$7.556.000/ano, aritmética conferida.
2.4. Comparação custo vs. retorno: **5** *(v1: 1)* — ROI 955,3%, payback 19 dias, ambos corretos.

### Critérios técnicos (peso menor)

**3. Necessidade real de IA — 10,0/10** *(v1: 5,0/10)*
3.1. Discute alternativa de regra determinística: **5** *(v1: 3)* — agora implementada e comparada para o problema específico de predição de conversão, sem vazamento.

**4. ML tradicional vs. Redes Neurais — 10,0/10** *(v1: 6,25/10)*
4.2. Baseline de fato executado e comparado: **5** *(v1: 2)* — reexecutado de forma independente, números batem exatamente.

**5. Aderência ao conteúdo do curso — 10/10** *(mantido)*

**6. Aderência ao template de projeto — 8,75/10** *(v1: 6,25/10)*
6.2. Profundidade do bloco 7 (MLOps): **4** *(v1: 2)* — PSI, cadência e gatilhos numéricos agora definidos; ressalva de que o teste de PSI compara amostras do mesmo sorteio sintético (drift baixo quase garantido por construção), não impede o avanço mas limita o quanto essa validação prova sobre detecção de drift real.

**7. Correção técnica — 8,1/10** *(v1: 5,63/10)*
7.1. Código executa sem erro: **3** *(v1: 1)* — o notebook principal roda e reproduz exatamente; mas o notebook de atribuição, apesar de agora ter outputs, é minimalista (3 células, constante hardcoded) e não sustenta a alegação de "138% de superatribuição" citada no material de negócio — e os documentos de apoio citam números do modelo que não correspondem ao notebook real (seção 3 acima).
7.2. Split antes de pré-processamento: **5** *(v1: 4)* — divergência de documentação (plano dizia "temporal") corrigida honestamente para refletir o split real.
7.4. Baseline avaliado no mesmo split: **5** *(v1: 4)* — confirmado com execução real reproduzida de forma independente.

**8. Qualidade do código — 9,17/10** *(mantido)*

**9. Honestidade dos resultados — 6,25/10** *(v1: 1,25/10)*
9.1. Múltiplas seeds/execuções: **4** *(v1: 2)* — o notebook principal é genuinamente reprodutível e a validação multi-seed é real (desvio-padrão zero da LogReg confirmado como correto, não suspeito — o solver `lbfgs` não depende de `random_state` neste problema convexo). Não é 5 porque, olhando o pacote entregue como um todo, os documentos de apoio ainda reportam números de modelo divergentes dos reais (seção 3).
9.2. Seção de limitações presente: **3** *(v1: 1)* — presente e honesta sobre dados sintéticos, mas não menciona a divergência de números entre documentos nem que o "138%" carece de cálculo — as duas coisas que mais precisam de nota de transparência neste momento do projeto.

## 5. Nota final

**8,0 / 10** *(v1: 4,8/10)* — O avanço é real e bem verificado: o notebook principal agora é reprodutível de ponta a ponta (confirmei em ambiente limpo, com versões de biblioteca diferentes das originais), o ROI está completo e consistente em todos os documentos, a comparação com uma regra determinística resolve a lacuna da v1, e o fallback do dashboard funciona de verdade (testei bloqueando a CDN). A nota não vai mais alta porque o problema de fundo da v1 — número oficial sem rastreabilidade — persiste, só que relocado: README, plano e apresentação ainda citam os números de modelo de antes da correção, e a alegação de "138% de superatribuição", a mais visível do material de negócio, continua sem nenhum cálculo real por trás. Regenerar os três documentos a partir da execução real do notebook, e sustentar (ou remover) o número de 138%, é o que falta para este trabalho refletir de forma consistente o que você já construiu de fato.

**Nível de maturidade: piloto controlado.** O ROI, o MLOps quantificado e a comparação de 3 níveis já têm rigor suficiente para uma decisão informada de negócio — desde que os números usados nessa decisão sejam os reais (notebook), não os que ainda aparecem nos documentos de apoio. Resolver a inconsistência da seção 3 é o passo que falta antes de tratar este material como pronto para apresentação a stakeholders.

## 6. Task list para evoluir o trabalho

- [ ] **Prioridade máxima:** regenere README.md, plano_projeto_santander_ia.md e apresentacao_3_slides.md com os números reais do notebook (tabela comparativa e multi-seed da seção 3 acima) — hoje eles citam os valores anteriores à correção.
- [ ] **Prioridade máxima:** sustente o número de "138% de superatribuição" com um cálculo real no `projeto_santander_jornadas_atribuicao.ipynb` (hoje esse notebook só ilustra a fórmula de decaimento com uma constante fixa, sem dados de transação nem comparação regra-antiga-vs-nova), ou remova/qualifique a alegação como estimativa não calculada.
- [ ] Corrija a taxa de conversão citada no plano (17,84%) para o valor real gerado pelo notebook (40,48%).
- [ ] No notebook de atribuição, substitua `p_org_media = 0.65` (hardcoded) por um cálculo real a partir de dados simulados de histórico, como o próprio plano já descreve que deveria ser.
- [ ] Na seção de limitações, adicione uma nota sobre o teste de PSI comparar apenas subamostras do mesmo sorteio sintético (não uma simulação de drift genuíno).
