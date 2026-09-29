# Case 1 — Termômetro do Focus

Claude Lab · Economia & Finanças · PUC-Rio · 30/10/2026

## O problema

Toda segunda-feira o Banco Central publica o **Relatório Focus**, com as expectativas de cerca de 100 instituições (bancos, gestoras, consultorias) para inflação, juros, câmbio e crescimento. É um dos documentos mais lidos do mercado, mas é só uma foto da semana. Ninguém lê o Focus sozinho: o analista quer saber **o que mudou**, **quanto mudou**, **se o mercado costuma acertar** e **o que isso significa**.

O desafio é construir um **termômetro das expectativas**: um relatório, painel ou agente que transforme a série do Focus em análise útil.

## Arquivos

| Arquivo | O que é |
|---|---|
| `focus_anuais.csv` | Expectativas anuais (IPCA, Selic, Câmbio, PIB Total), uma linha por data de coleta e ano de referência |
| `sgs_realizado.csv` | Séries realizadas do Banco Central (formato longo: data, série, valor) |

> Os arquivos são gerados por [`scripts/baixar_dados_bcb.py`](../../scripts/baixar_dados_bcb.py). Quem quiser pode rodar o script para atualizar.

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

## Cuidados com os dados (leia antes de concluir qualquer coisa)

1. **`Data` ≠ `DataReferencia`.** Uma linha com `Data = 2026-03-06` e `DataReferencia = 2026` é "o que o mercado esperava, em março, para o IPCA do ano todo de 2026". Misturar as duas é o erro mais comum.
2. **Filtre `baseCalculo`.** Há duas linhas por data e indicador (0 e 1). Sem filtrar, você conta tudo em dobro. O relatório oficial usa `baseCalculo = 0`.
3. **IPCA mensal não se soma, se compõe.** Para acumular 12 meses: `(1+m1/100)×(1+m2/100)×…−1`. Use `ipca_12m` para conferir o seu cálculo.
4. **Selic no Focus é a taxa no fim do ano**, não a média do ano.
5. **Expectativa de PIB é para o ano inteiro**, e o PIB oficial só sai trimestralmente, com atraso. O IBC-Br é só uma aproximação mensal.
6. Os dados param na data em que o script rodou. Diga essa data no seu artefato.

## Caminhos

- **Básico (chat):** suba os dois CSVs e este README no Claude e peça uma análise e um relatório semanal no estilo "o que mudou no Focus".
- **Avançado (código):** use o Claude Code para montar um script que baixa os dados atualizados, recalcula tudo e gera o relatório sozinho: um **workflow recorrente** que poderia rodar toda segunda.

## Perguntas para atacar

- Como a expectativa de IPCA de 2026 evoluiu ao longo do tempo? Quando ela mudou mais, e o que aconteceu na época?
- O mercado acerta? Compare a expectativa para 2025 feita em janeiro de 2025 com o IPCA que de fato fechou 2025. E para a Selic?
- A dispersão (`DesvioPadrao`) aumenta antes de mudanças de rumo? Incerteza é informação.
- A expectativa de inflação está acima ou abaixo da meta de 3%? Há quanto tempo?
- Dá para montar um **alerta**: "atenção, a expectativa de X subiu mais de Y nas últimas 4 semanas"?
