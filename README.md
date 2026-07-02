# Análise da Campanha de Marketing do iFood com LLM

## Problema de Negócio

O iFood é um aplicativo de entrega de refeições líder no Brasil, presente em mais de 1.000 cidades.

Manter um alto engajamento dos clientes é a chave para o crescimento e a consolidação da posição da empresa como líder de mercado.

Este case pretende simular esse cenário.

Neste case, apresentamos uma amostra de dados contendo informações que representam os clientes de uma campanha de marketing do iFood.

O seu desafio é entender os dados, encontrar oportunidades de negócio, gerar insights e propor ações com base nas informações disponíveis para otimizar os resultados das campanhas e gerar valor para a empresa.

O objetivo deste case é avaliar suas habilidades analíticas e seu conhecimento de negócio.

## Contexto da Análise que foi Gerada pela LLM

**Quem consome:** Gerente de Marketing.

**Dor principal:** Avaliar se a última campanha (`Response`) teve bom desempenho, usando o gasto do cliente (`Gasto-Cliente`) como métrica de sucesso — ou seja, não basta saber *quem respondeu*, mas *quanto essa resposta converteu em receita/gasto*.

**Decisão a embasar:** Definir o **segmento de clientes-alvo da próxima campanha**, com objetivo de **maximizar o faturamento geral da campanha** (não necessariamente a taxa de resposta, mas o retorno em valor).

Isso muda a forma como vamos olhar os dados: o KPI central não é "% que respondeu", e sim uma combinação de **quem responde E quanto gasta** — o que sugere, mais adiante, comparar `Response` × `Gasto-Cliente` cruzado com perfil demográfico e comportamento de compra, para encontrar o segmento com melhor relação entre propensão a responder e ticket/gasto.

## Premissas da análise

- Os dados utilizados para a análise são somente da última campanha.
- Os dados coletados para a análise foram dos últimos 3 meses, durante a campanha.
- Foram utilizados os dados de 2.205 clientes participantes da última campanha.

## Estratégia da solução

O método Fato-Dimensão foi usado para desenvolver a análise de dados da campanha de Marketing.

## Passo 1: Resumir o contexto em uma pergunta aberta

As perguntas abertas são um tipo de demanda muito comum em análise de dados no qual a demanda possui N possíveis soluções e cabe ao analista de dados avaliar as possibilidades e escolher a alternativa com o maior retorno e o menor esforço possível. Para essa análise, foi definida a seguinte pergunta aberta:

**Como foi o resultado da última campanha de Marketing?**

## Passo 2: Transformar pergunta aberta em fechada

As perguntas fechadas são um tipo de demanda muito comum na área de análise de dados. Essa demanda contém todos os detalhes da análise de dados e direciona o analista exatamente para o que precisa ser feito. Geralmente, a pergunta fechada é a escolha de uma solução entre todas as alternativas possíveis, feita por um profissional mais sênior da área.

Para essa análise, foi definida a seguinte pergunta fechada:

**Existe um perfil demográfico (renda, educação, presença de filhos, idade) mais associado a alta resposta e alto gasto?**

## Passo 3: Definição da Coluna Fato

O Fato é a coluna de interesse que representa o ponto focal da análise. Nesse caso, a coluna "Gasto-Cliente" representa o faturamento de cada cliente dentro da campanha e será o objetivo da nossa análise, dado que o problema de negócio envolve aumento do faturamento nas próximas campanhas de Marketing.

## Passo 4: Identificação das Dimensões

### 🧑 Perfil do cliente

| Coluna | O que representa | Significado de negócio |
|--------|------------------|------------------------|
| ID | Identificador único do cliente | Chave primária, não tem valor analítico direto |
| Year_Birth | Ano de nascimento | Permite calcular idade → segmentação geracional (idade pode explicar preferência de canal, categoria de produto, sensibilidade a desconto) |
| Education | Nível educacional (Graduation, PhD, Master, Basic, 2n Cycle) | Proxy de classe social/poder aquisitivo → pode correlacionar com ticket médio e categorias consumidas |
| Marital_Status | Estado civil | Indica composição familiar → influencia volume e tipo de compra (ex.: casais compram mais) |
| Income | Renda anual do cliente | Um dos principais drivers de gasto e de capacidade de resposta a campanhas |
| Kidhome / Teenhome | Nº de crianças / adolescentes em casa | Tamanho e fase do domicílio → afeta mix de produtos e frequência de compra |

### 📅 Relacionamento com a empresa

| Coluna | O que representa | Significado de negócio |
|--------|------------------|------------------------|
| Dt_Customer | Data de cadastro do cliente | Permite calcular "tempo de casa" (tenure) → clientes mais antigos podem ter comportamento de lealdade diferente de novos |
| Recency | Dias desde a última compra | Indicador clássico de RFM — quanto menor, mais "quente" o cliente está |

### 💰 Gasto por categoria (últimos ~2 anos)

| Coluna | O que representa | Significado de negócio |
|--------|------------------|------------------------|
| MntWines, MntFruits, MntMeatProducts, MntFishProducts, MntSweetProducts, MntGoldProds | Valor gasto em cada categoria de produto | Compõem o "M" (Monetary) do RFM; juntas formam o gasto total do cliente; permitem identificar perfil de consumo (ex.: cliente premium de vinho vs. cliente de produtos "gold"/presente) |

### 🛒 Canal de compra

| Coluna | O que representa | Significado de negócio |
|--------|------------------|------------------------|
| NumDealsPurchases | Nº de compras feitas com desconto/promoção | Sensibilidade a preço/promoção |
| NumWebPurchases | Nº de compras via site | Preferência por canal digital |
| NumCatalogPurchases | Nº de compras via catálogo | Canal mais tradicional |
| NumStorePurchases | Nº de compras em loja física | Preferência por canal físico |
| NumWebVisitsMonth | Visitas ao site no último mês | Engajamento digital, mesmo sem conversão |

Juntas, essas colunas dão o "F" (Frequency) do RFM e revelam o mix de canais preferido — essencial para decidir por onde direcionar a campanha (e-mail/site vs. catálogo vs. loja).

### 📣 Campanhas de marketing

| Coluna | O que representa | Significado de negócio |
|--------|------------------|------------------------|
| AcceptedCmp1 a AcceptedCmp5 | Se o cliente aceitou (comprou após) cada uma das 5 campanhas anteriores | Histórico de responsividade a campanhas — quem responde uma vez tende a responder de novo? |
| Response | Se aceitou a campanha mais recente (a que está sendo avaliada) | Variável-alvo principal para medir sucesso da campanha atual |
| Complain | Se reclamou nos últimos 2 anos | Sinal de insatisfação — pode ser filtro de exclusão ou variável de risco |
| Z_CostContact / Z_Revenue | Constantes (3 e 11 para todos os clientes) | Provavelmente custo de contato e receita padrão por resposta usados no dataset original para calcular ROI da campanha; como são constantes, servem apenas de referência fixa, não diferenciam clientes |

## Passo 5: Hipóteses Analíticas

As hipóteses analíticas são construídas a partir da combinação do fato com as dimensões, usando sempre um valor de comparação como maior, menor ou igual.

#### H1 — Clientes que respondem à campanha gastam mais que os que não respondem

**Lógica:** Se a campanha está de fato engajando os clientes certos, o grupo que responde (`Response = 1`) deveria ter um Gasto-Cliente médio maior que o grupo que não responde — isso valida se a campanha está atraindo clientes de alto valor, e não apenas volume de resposta sem faturamento relevante.

**Como testar:** Comparar a média/mediana de `Gasto-Cliente` entre `Response = 1` e `Response = 0` (boxplot + teste t ou Mann-Whitney, já que gasto costuma ser assimétrico).

---

#### H2 — Renda mais alta está associada a maior propensão de resposta e maior gasto

**Lógica:** Clientes com `Income` mais alto têm mais poder de compra, logo tendem a gastar mais e podem ser mais propensos a responder a ofertas (ou o oposto — menos sensíveis a desconto, dependendo do tipo de campanha).

**Como testar:** Correlação entre `Income` e `Gasto-Cliente`; comparar distribuição de `Income` entre respondentes e não-respondentes; segmentar `Income` em faixas e calcular taxa de `Response` por faixa.

---

#### H3 — Presença de filhos em casa (Kidhome/Teenhome) reduz a propensão de resposta e o gasto

**Lógica:** Famílias com crianças/adolescentes podem ter orçamento mais restrito ou prioridades de compra diferentes (produtos essenciais vs. categorias como vinho/gold), reduzindo tanto o gasto quanto a resposta a campanhas de categorias não-essenciais.

**Como testar:** Comparar `Gasto-Cliente` e taxa de `Response` entre clientes com `Kidhome+Teenhome = 0` vs. `≥1`; analisar se o efeito é mais forte em categorias específicas (`MntGoldProds`, `MntWines`).

---

#### H4 — Clientes que já responderam a campanhas anteriores têm maior chance de responder novamente

**Lógica:** Responsividade a marketing pode ser um traço comportamental estável — "quem comprou uma vez por causa de campanha, compra de novo" — o que ajudaria a identificar um segmento fiel para targeting.

**Como testar:** Criar uma variável `Total_Campanhas_Aceitas = soma(AcceptedCmp1..5)` e cruzar com `Response` (ex.: taxa de `Response` por número de campanhas aceitas anteriormente; correlação ponto-bisserial).

---

#### H5 — Recency (tempo desde a última compra) está inversamente relacionada à resposta

**Lógica:** Clientes que compraram recentemente estão mais "engajados" e mais propensos a responder a uma nova campanha; clientes inativos há muito tempo tendem a ignorar ofertas.

**Como testar:** Comparar distribuição de `Recency` entre `Response = 1` e `Response = 0`; correlação entre `Recency` e `Response` (point-biserial) ou `Recency` e `Gasto-Cliente`.

---

#### H6 — O canal de compra preferido influencia a resposta à campanha

**Lógica:** Clientes mais digitais (`NumWebPurchases`, `NumWebVisitsMonth` altos) podem responder mais a campanhas veiculadas online, enquanto clientes de loja física podem ser menos sensíveis a esse tipo de estímulo — informação relevante para escolher o canal de divulgação da próxima campanha.

**Como testar:** Criar um indicador de "canal dominante" por cliente (qual canal concentra mais compras) e cruzar com `Response`; comparar médias de `NumWebPurchases`, `NumCatalogPurchases`, `NumStorePurchases` entre respondentes e não-respondentes.

---

#### H7 — Clientes com reclamações (Complain) têm menor propensão a responder

**Lógica:** Insatisfação recente pode reduzir a receptividade a novas ofertas — importante para decidir se esse grupo deve ser excluído ou tratado de forma diferenciada na próxima campanha.

**Como testar:** Comparar taxa de `Response` entre `Complain = 1` e `Complain = 0` (teste qui-quadrado, dado que ambas são binárias).

---

#### H8 — Existe um segmento específico que maximiza receita esperada (propensão × ticket), não necessariamente o de maior taxa de resposta

**Lógica:** Esta é a hipótese "guarda-chuva" ligada à decisão de negócio: o segmento ideal para a próxima campanha não é obrigatoriamente o que mais responde, mas o que gera maior **receita esperada** = P(Response) × Gasto-Cliente médio. Pode haver um segmento com taxa de resposta moderada, mas gasto muito alto, que supera em valor um segmento de alta resposta e baixo ticket.

**Como testar:** Segmentar clientes (por renda, idade, educação, filhos, canal) e calcular, para cada segmento: taxa de resposta × Gasto-Cliente médio; ranquear segmentos por essa receita esperada e comparar com o ranking apenas por taxa de resposta.

---

#### H9 — Sensibilidade a desconto (NumDealsPurchases) está associada a menor gasto médio, mas talvez maior resposta a campanhas promocionais

**Lógica:** Clientes que compram muito na promoção podem ter menor ticket médio individual (só compram quando há desconto), mas podem responder proporcionalmente mais a uma campanha — o que é importante para não confundir "responde muito" com "gera muito faturamento".

**Como testar:** Correlação entre `NumDealsPurchases` e `Gasto-Cliente`; comparar taxa de `Response` e Gasto-Cliente médio entre quartis de `NumDealsPurchases`.

## Passo 6: Critérios de Priorização

- **Critério 1:** Dados disponíveis.
- **Critério 2:** Insights acionáveis.

## Passo 7: Priorização das Hipóteses Analíticas

#### H1 — Clientes que respondem à campanha gastam mais que os que não respondem

**Lógica:** Se a campanha está de fato engajando os clientes certos, o grupo que responde (`Response = 1`) deveria ter um Gasto-Cliente médio maior que o grupo que não responde — isso valida se a campanha está atraindo clientes de alto valor, e não apenas volume de resposta sem faturamento relevante.

**Como testar:** Comparar a média/mediana de `Gasto-Cliente` entre `Response = 1` e `Response = 0` (boxplot + teste t ou Mann-Whitney, já que gasto costuma ser assimétrico).

---

#### H2 — Renda mais alta está associada a maior propensão de resposta e maior gasto

**Lógica:** Clientes com `Income` mais alto têm mais poder de compra, logo tendem a gastar mais e podem ser mais propensos a responder a ofertas (ou o oposto — menos sensíveis a desconto, dependendo do tipo de campanha).

**Como testar:** Correlação entre `Income` e `Gasto-Cliente`; comparar distribuição de `Income` entre respondentes e não-respondentes; segmentar `Income` em faixas e calcular taxa de `Response` por faixa.

---

#### H3 — Presença de filhos em casa (Kidhome/Teenhome) reduz a propensão de resposta e o gasto

**Lógica:** Famílias com crianças/adolescentes podem ter orçamento mais restrito ou prioridades de compra diferentes (produtos essenciais vs. categorias como vinho/gold), reduzindo tanto o gasto quanto a resposta a campanhas de categorias não-essenciais.

**Como testar:** Comparar `Gasto-Cliente` e taxa de `Response` entre clientes com `Kidhome+Teenhome = 0` vs. `≥1`; analisar se o efeito é mais forte em categorias específicas (`MntGoldProds`, `MntWines`).

---

#### H4 — Clientes que já responderam a campanhas anteriores têm maior chance de responder novamente

**Lógica:** Responsividade a marketing pode ser um traço comportamental estável — "quem comprou uma vez por causa de campanha, compra de novo" — o que ajudaria a identificar um segmento fiel para targeting.

**Como testar:** Criar uma variável `Total_Campanhas_Aceitas = soma(AcceptedCmp1..5)` e cruzar com `Response` (ex.: taxa de `Response` por número de campanhas aceitas anteriormente; correlação ponto-bisserial).

---

#### H5 — Recency (tempo desde a última compra) está inversamente relacionada à resposta

**Lógica:** Clientes que compraram recentemente estão mais "engajados" e mais propensos a responder a uma nova campanha; clientes inativos há muito tempo tendem a ignorar ofertas.

**Como testar:** Comparar distribuição de `Recency` entre `Response = 1` e `Response = 0`; correlação entre `Recency` e `Response` (point-biserial) ou `Recency` e `Gasto-Cliente`.

---

#### H6 — O canal de compra preferido influencia a resposta à campanha

**Lógica:** Clientes mais digitais (`NumWebPurchases`, `NumWebVisitsMonth` altos) podem responder mais a campanhas veiculadas online, enquanto clientes de loja física podem ser menos sensíveis a esse tipo de estímulo — informação relevante para escolher o canal de divulgação da próxima campanha.

**Como testar:** Criar um indicador de "canal dominante" por cliente (qual canal concentra mais compras) e cruzar com `Response`; comparar médias de `NumWebPurchases`, `NumCatalogPurchases`, `NumStorePurchases` entre respondentes e não-respondentes.

---

#### H7 — Clientes com reclamações (Complain) têm menor propensão a responder

**Lógica:** Insatisfação recente pode reduzir a receptividade a novas ofertas — importante para decidir se esse grupo deve ser excluído ou tratado de forma diferenciada na próxima campanha.

**Como testar:** Comparar taxa de `Response` entre `Complain = 1` e `Complain = 0` (teste qui-quadrado, dado que ambas são binárias).

---

#### H8 — Existe um segmento específico que maximiza receita esperada (propensão × ticket), não necessariamente o de maior taxa de resposta

**Lógica:** Esta é a hipótese "guarda-chuva" ligada à decisão de negócio: o segmento ideal para a próxima campanha não é obrigatoriamente o que mais responde, mas o que gera maior **receita esperada** = P(Response) × Gasto-Cliente médio. Pode haver um segmento com taxa de resposta moderada, mas gasto muito alto, que supera em valor um segmento de alta resposta e baixo ticket.

**Como testar:** Segmentar clientes (por renda, idade, educação, filhos, canal) e calcular, para cada segmento: taxa de resposta × Gasto-Cliente médio; ranquear segmentos por essa receita esperada e comparar com o ranking apenas por taxa de resposta.

---

#### H9 — Sensibilidade a desconto (NumDealsPurchases) está associada a menor gasto médio, mas talvez maior resposta a campanhas promocionais

**Lógica:** Clientes que compram muito na promoção podem ter menor ticket médio individual (só compram quando há desconto), mas podem responder proporcionalmente mais a uma campanha — o que é importante para não confundir "responde muito" com "gera muito faturamento".

**Como testar:** Correlação entre `NumDealsPurchases` e `Gasto-Cliente`; comparar taxa de `Response` e Gasto-Cliente médio entre quartis de `NumDealsPurchases`.

## Insights da análise

*（Aguardando preenchimento com os resultados das validações）*

## Visualize a Análise Completa

Clique no link e, em seguida, em **"Download"** ou **"View raw"**:

- **Acesse a Validação das Hipóteses Analíticas em HTML:** [https://drive.google.com/file/d/10EKrilUdsysmf_C4AMcAFq6yzfsHMZhD/view?usp=drive_link](https://drive.google.com/file/d/10EKrilUdsysmf_C4AMcAFq6yzfsHMZhD/view?usp=drive_link)

- **📥 Baixe a apresentação estilo Power Bi em HTML:**  
[https://drive.google.com/file/d/1pg_QmDSqtbWCcRpvQGY2Wotb9MAcdIkT/view?usp=sharing](https://drive.google.com/file/d/1pg_QmDSqtbWCcRpvQGY2Wotb9MAcdIkT/view?usp=sharing)

- **📥 Baixe a apresentação Execultiva em HTML:**  
[https://drive.google.com/file/d/15osUjUFA2E2Fvrli8fNbt8m2cDxJjM86/view?usp=sharing](https://drive.google.com/file/d/15osUjUFA2E2Fvrli8fNbt8m2cDxJjM86/view?usp=sharing)

## Próximos passos

- Explorar mais características dos clientes.
- Automatizar a coleta e a análise para acompanhamento.
- Agrupar os clientes em grupos de maior e menor faturamento para entender se há similaridades ou não.
- Montar um dashboard de acompanhamento das métricas das futuras campanhas de marketing.
