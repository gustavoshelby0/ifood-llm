# Análise da Campanha de Marketing do Ifood com LLM

## Problema de Negócio

O iFood é um aplicativo de entrega de refeições líder no Brasil, presente em mais de 1.000 cidades.

Manter um alto engajamento dos clientes é a chave para o crescimento e a consolidação da posição da empresa como líder de mercado.

Este case pretende simular esse cenário.

Neste case, apresentamos uma amostra de dados contendo informações que representam os clientes de uma campanha de marketing do iFood.

O seu desafio é entender os dados, encontrar oportunidades de negócio, gerar insights e propor ações com base nas informações disponíveis para otimizar os resultados das campanhas e gerar valor para a empresa.

O objetivo deste case é avaliar suas habilidades analíticas e seu conhecimento de negócio.

## Contexto da Analise que foi Gerado Pela LLM

**Quem consome:** Gerente de Marketing.

**Dor principal:** Avaliar se a última campanha (`Response`) teve bom desempenho, usando o gasto do cliente (`Gasto-Cliente`) como métrica de sucesso — ou seja, não basta saber *quem respondeu*, mas *quanto essa resposta converteu em receita/gasto*.

**Decisão a embasar:** Definir o **segmento de clientes-alvo da próxima campanha**, com objetivo de **maximizar o faturamento geral da campanha** (não necessariamente a taxa de resposta, mas o retorno em valor).

Isso muda a forma como vamos olhar os dados: o KPI central não é "% que respondeu", e sim uma combinação de **quem responde E quanto gasta** — o que sugere, mais adiante, comparar `Response` × `Gasto-Cliente` cruzado com perfil demográfico e comportamento de compra, para achar o segmento com melhor relação entre propensão a responder e ticket/gasto.

# Premissas da análise

- Os dados utilizados para a análise são somente da última campanha.
- Os dados coletados para a análise foram dos últimos 3 meses, durante a campanha.
- Foram utilizados os dados de 2.205 clientes participantes da última campanha.

# Estratégia da solução

O método Fato-Dimensão foi usado para desenvolver a análise de dados da campanha de Marketing.

# Passo 1: Resumir o contexto em uma pergunta aberta

As perguntas abertas são um tipo de demanda muito comum em análise de dados no qual a demanda possui N possíveis soluções e cabe ao analista de dados avaliar as possibilidades e escolher a alternativa com o maior retorno e o menor esforço possível. Para essa análise, foi definida a seguinte pergunta aberta:

**Como foi o resultado da última campanha de Marketing?**

# Passo 2: Transformar pergunta aberta em fechada

As perguntas fechadas são um tipo de demanda muito comum na área de análise de dados. Essa demanda contém todos os detalhes da análise de dados e direciona o analista exatamente para o que precisa ser feito. Geralmente, a pergunta fechada é a escolha de uma solução entre todas as alternativas possíveis, feita por um profissional mais sênior da área.

Para essa análise, foi definida a seguinte pergunta fechada:

**Existe um perfil demográfico (renda, educação, presença de filhos, idade) mais associado a alta resposta e alto gasto?**

# Passo 3: Definição da Coluna Fato

O Fato é a coluna de interesse que representa o ponto focal da análise. Nesse caso, a coluna "Gasto-Clientes" representa o faturamento de cada cliente dentro da campanha e será o objetivo da nossa análise, dado que o problema de negócio envolve aumento do faturamento nas próximas campanhas de Marketing.

# Passo 4: Identificação das Dimensões

As colunas foram agrupadas em dimensões comuns que fornecem mais detalhes sobre o Fato que será analisado. Foram organizadas as seguintes dimensões:

- **Cliente**: Salário, Idade, Faixa-Etária, Dias-Cliente, Estado-Civil, Formação, Crianças-Casa, Adolescentes-Casa, Recência.
- **Produto**: Qtde-Vinhos, Qtde-Frutas, Qtde-Carnes, Qtde-Peixes, Qtde-Doces, Qtde-Premium.
- **Comportamento de Compra**: Qtde-Compras, Qtde-Compras-Web, Qtde-Compras-Loja, Visitas-Site-Mes.
- **Comportamento de Mkt**: Reclamações.

# Passo 5: Hipóteses Analíticas

**Fato (Medida) + Dimensão (Detalhes) + Comparação**

As hipóteses analíticas são construídas a partir da combinação do fato com as dimensões, usando sempre um valor de comparação como maior, menor ou igual.

- **Fato + Dimensão: Cliente - Atributos: Idade.**
  - O faturamento dos clientes abaixo de 30 anos é maior do que nas outras faixas etárias.
  - O faturamento dos clientes entre 20 e 30 anos é maior do que nas outras faixas etárias.
  - O faturamento dos clientes acima dos 30 anos é maior do que nas outras faixas.

- **Fato + Dimensão: Cliente - Atributos: Estado Civil.**
  - Clientes solteiros gastam mais do que os outros segmentos de clientes.
  - Clientes solteiros gastam menos do que os outros segmentos de clientes.
  - Clientes casados gastam mais do que os outros segmentos de clientes.

- **Fato + Dimensão: Cliente - Atributos: Estado Civil + Idade.**
  - Clientes solteiros acima dos 30 anos gastam mais do que clientes casados acima dos 30 anos.

- **Fato + Dimensão: Cliente - Atributos: Formação Profissional.**
  - Clientes com formações avançadas (Doutorado) gastam mais do que clientes com Ensino Fundamental.
  - Clientes com maiores salários têm nível de escolaridade maior.

# Passo 6: Critérios de Priorização

- **Critério 1:** Dados disponíveis.
- **Critério 2:** Insights acionáveis.

# Passo 7: Priorização das Hipóteses Analíticas

1. **Hipótese 1:** Clientes abaixo dos 30 anos gastam mais com produtos do iFood do que as outras faixas etárias.
2. **Hipótese 2:** Clientes solteiros gastam menos do que os outros segmentos de clientes.
3. **Hipótese 3:** Clientes solteiros abaixo dos 30 anos gastam mais com produtos do iFood do que as outras faixas etárias.
4. **Hipótese 4:** Clientes com crianças em casa compram mais pelo iFood.
5. **Hipótese 5:** Clientes que compram mais carne também compram mais vinho.

# Insights da análise

# Resultados

**Conclusão:** O melhor segmento da campanha foram os clientes casados, com idade entre 41 e 50 anos, sem filhos em casa e com graduação completa.

O pior segmento de clientes foram os viúvos de todas as faixas etárias, clientes abaixo dos 30 anos de todos os estados civis com 2 ou mais crianças em casa e somente ensino fundamental.

Para maximizar o lucro da próxima campanha, o Marketing precisa direcionar suas ações ao melhor segmento apresentado e reduzir o investimento nos outros segmentos, especialmente o mencionado.

# Visualize a Análise Completa

- **Acesse o relatório no Looker Studio:** [https://lookerstudio.google.com/reporting/9536ef1a-3c05-4347-b335-ae914e3c92d5](https://lookerstudio.google.com/reporting/9536ef1a-3c05-4347-b335-ae914e3c92d5)

- **📥 Baixe a apresentação em PowerPoint (clique no link e, em seguida, em "Download" ou "View raw"):**  
  [https://github.com/gustavoshelby0/Ifood-campanha-market/blob/main/Relatorio%20de%20Campanha%20do%20iFood.pptx](https://github.com/gustavoshelby0/Ifood-campanha-market/blob/main/Relatorio%20de%20Campanha%20do%20iFood.pptx)

# Próximos passos

- Explorar mais características dos clientes.
- Automatizar a coleta e a análise para acompanhamento.
- Agrupar os clientes em grupos de maior e menor faturamento para entender se há similaridades ou não.
- Montar um dashboard de acompanhamento das métricas das futuras campanhas de marketing.
