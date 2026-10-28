# Case 1 — Termômetro do Focus

Claude Lab · Economia & Finanças · PUC-Rio · 06/11/2026

## O problema

Toda segunda-feira o Banco Central publica o **Relatório Focus**, com as expectativas de cerca de 100 instituições (bancos, gestoras, consultorias) para inflação, juros, câmbio e crescimento. É um dos documentos mais lidos do mercado, mas é só uma foto da semana. Ninguém lê o Focus sozinho: o analista quer saber **o que mudou**, **quanto mudou**, **se é um movimento relevante** e **o que isso significa**. E quer receber isso pronto, toda semana, sem ter que lembrar de abrir o relatório.

## O objetivo

Construir um **agente** que, toda semana e sem ninguém apertar um botão:

1. **baixa** o Focus mais recente e as séries realizadas do Banco Central;
2. **calcula** o que mudou: variação na semana e nas últimas 4 semanas, distância da meta de inflação, dispersão das projeções e alertas;
3. **confere** os próprios números antes de escrever qualquer coisa;
4. pede ao Claude uma **análise curta**, escrita só a partir desses números;
5. **envia a análise por e-mail** para quem assinou.

> A diferença para o Case 2: lá o produto é um site de análise. Aqui o produto é um **fluxo automático e recorrente**, que trabalha sozinho toda segunda-feira.

## Os dados

Os dados **não vêm prontos**: o próprio agente busca toda semana nas APIs abertas do Banco Central, sem cadastro nem chave. O script [`scripts/baixar_dados_bcb.py`](../../scripts/baixar_dados_bcb.py) é o ponto de partida dessa etapa e gera dois arquivos em `data/case1_focus/`:

| Arquivo | O que é |
|---|---|
| `focus_anuais.csv` | Expectativas anuais (IPCA, Selic, Câmbio, PIB Total), uma linha por data de coleta e ano de referência |
| `sgs_realizado.csv` | Séries realizadas do Banco Central (formato longo: data, série, valor) |

### focus_anuais.csv

| Coluna | Nota |
|---|---|
| `Indicador` | `IPCA`, `Selic`, `Câmbio`, `PIB Total` |
| `Data` | **data em que a expectativa foi coletada** |
| `DataReferencia` | **ano ao qual a expectativa se refere** (ex.: 2026, 2027) |
| `Mediana` / `Media` | estatística das projeções das instituições; o Focus divulga a **mediana** |
| `DesvioPadrao`, `Minimo`, `Maximo` | dispersão das projeções |
| `numeroRespondentes` | quantas instituições responderam |
| `baseCalculo` | `0` = projeções dos últimos 30 dias; `1` = só dos últimos 5 dias úteis |

### sgs_realizado.csv

| `serie` | Código SGS | Unidade |
|---|---|---|
| `ipca_mensal` | 433 | variação % no mês |
| `ipca_12m` | 13522 | variação % acumulada em 12 meses |
| `selic_meta` | 432 | % ao ano |
| `dolar_ptax_venda` | 1 | R$ por US$, diário |
| `ibc_br` | 24363 | índice (proxy mensal do PIB) |

## Como chegar lá: passo a passo

### 1. Baixar e entender os dados

Rodem `python scripts/baixar_dados_bcb.py` e peçam ao Claude Code para explicar os dois CSVs com base nesta página. Confiram: qual é a data mais recente? Quantas linhas há por semana? O que muda entre `baseCalculo` 0 e 1? Abram o [Relatório Focus](https://www.bcb.gov.br/publicacoes/focus) da última segunda no site do BC e achem nele 2 números que também estão no CSV.

### 2. Calcular o termômetro da semana

Partam de [`avancado/case1_focus/inicio_focus.py`](../../avancado/case1_focus/inicio_focus.py), que já carrega, limpa e monta as séries semanais. Para IPCA, Selic, Câmbio e PIB, no ano corrente e no próximo, calculem:

- a **mediana atual** e a **variação** na semana e em 4 semanas;
- a **distância da meta** de inflação (3%) para o IPCA;
- a **dispersão** (`DesvioPadrao`): o mercado está mais ou menos dividido?
- **alertas**: movimentos fora do normal, com limiares calibrados pelo histórico, e não chutados.

Salvem tudo num arquivo estruturado, por exemplo `resumo_semana.json`. **Esse arquivo é a única fonte dos números do e-mail.**

### 3. Conferir antes de escrever

Um agente que manda e-mail sozinho não pode mandar número errado. Antes de seguir, o código checa automaticamente, por exemplo:

- a data mais recente é desta semana? (se o Focus não saiu, não há o que analisar)
- o filtro `baseCalculo = 0` foi aplicado?
- algum valor está fora de uma faixa plausível?

Se alguma checagem falhar, o agente **para** e manda um e-mail curto de alerta para o time, em vez do relatório.

### 4. Escrever a análise com o Claude

Montem um **prompt fixo** (guardem num arquivo, por exemplo `prompt_analise.md`) que recebe só o `resumo_semana.json` e devolve sempre o mesmo formato: um título, 3 a 5 pontos sobre o que mudou e um parágrafo "o que observar na próxima semana". Regras do prompt: usar apenas os números do JSON, citar a fonte e não fazer recomendação de investimento.

**Bônus de rigor:** um teste automático que confere se todo número citado no texto existe no JSON.

> Para o agente chamar o Claude sozinho, há duas opções: o Claude Code em modo não interativo (`claude -p "..."`) ou a API da Anthropic, com uma chave (`ANTHROPIC_API_KEY`) cobrada à parte do plano do Claude. Vejam o [guia do caminho avançado](../../avancado/README.md).

### 5. Montar e enviar o e-mail

O e-mail tem a análise, uma tabela com os números da semana, um gráfico simples (opcional), a fonte ("Banco Central do Brasil, Relatório Focus de dd/mm/aaaa") e o aviso de que não é recomendação de investimento. Para enviar:

- **Gmail:** biblioteca `smtplib` do Python com uma **senha de app** (exige verificação em duas etapas na conta);
- **ou** um serviço de envio de e-mails com plano gratuito.

**Senhas e chaves ficam em `.env` (no computador) ou nos Secrets do GitHub, nunca no código.** Testem enviando para vocês mesmos.

### 6. Agendar toda semana

O Focus sai segunda de manhã. Agendem o agente para rodar depois disso:

- **Recomendado: GitHub Actions.** Um arquivo `.github/workflows/focus_semanal.yml` com `schedule` (ex.: `cron: "0 13 * * 1"`, segunda às 10h de Brasília) roda na nuvem, mesmo com o computador desligado. Incluam também `workflow_dispatch` para disparar na hora: é assim que vocês vão demonstrar o agente funcionando no dia.
- **Alternativa:** o Agendador de Tarefas do Windows (ou `cron`), que só funciona com o computador ligado.

### 7. Publicar o artefato

O artefato público é a **versão web do relatório da semana**: o mesmo conteúdo do e-mail numa página publicada. No vídeo, mostrem o disparo do agente e o e-mail chegando.

### Ritmo sugerido no dia

| Momento | Passos |
|---|---|
| Manhã (10h30–13h00) | 1 a 3: dados baixados, termômetro calculado e checagens funcionando |
| Tarde (14h00–16h00) | 4 a 7: análise, e-mail, agendamento e artefato |

Código de partida e trilha em **[avancado/case1_focus](../../avancado/case1_focus/README.md)**.

## Cuidados com os dados (leia antes de concluir qualquer coisa)

1. **`Data` ≠ `DataReferencia`.** Uma linha com `Data = 2026-03-06` e `DataReferencia = 2026` é "o que o mercado esperava, em março, para o IPCA do ano todo de 2026". Misturar as duas é o erro mais comum.
2. **Filtre `baseCalculo`.** Há duas linhas por data e indicador (0 e 1). Sem filtrar, você conta tudo em dobro. O relatório oficial usa `baseCalculo = 0`.
3. **IPCA mensal não se soma, se compõe.** Para acumular 12 meses: `(1+m1/100)×(1+m2/100)×…−1`. Use `ipca_12m` para conferir o seu cálculo.
4. **Selic no Focus é a taxa no fim do ano**, não a média do ano.
5. **Expectativa de PIB é para o ano inteiro**, e o PIB oficial só sai trimestralmente, com atraso. O IBC-Br é só uma aproximação mensal.
6. **Toda análise diz a data dos dados.** O e-mail precisa dizer de qual Focus está falando.
7. **Segurança:** senhas, chaves e tokens nunca vão para o GitHub. E o agente só envia e-mail para quem pediu para receber (vocês e a banca).

## O que a banca vai olhar neste case

- O agente **roda sozinho** do começo ao fim (demonstrado com o disparo manual do agendamento)?
- Os números do e-mail **batem com o Relatório Focus** oficial?
- O agente **para** quando os dados estão errados ou desatualizados?
- A análise é **útil** para quem recebe e não inventa números?
- Senhas e chaves estão **protegidas**?

## Perguntas para atacar

- Quais são os 3 números que um gestor quer ver toda segunda?
- Qual variação semanal é "normal" e qual merece alerta? Como calibrar isso com o histórico?
- A dispersão (`DesvioPadrao`) aumenta antes de mudanças de rumo? Incerteza é informação.
- O mercado acerta? Vale incluir no e-mail um lembrete de quanto ele errou no ano passado?
- Bônus: cada assinante escolhe quais indicadores quer receber?
