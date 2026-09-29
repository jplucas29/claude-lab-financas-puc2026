# Case 3 — Nota de reação a resultado

Claude Lab · Economia & Finanças · PUC-Rio · 30/10/2026

## O problema

Na temporada de balanços, um analista de banco ou gestora tem poucas horas para ler o release de resultado de uma empresa e escrever uma **nota de reação** ("reaction note"): os números principais, o que surpreendeu para cima ou para baixo, o que mudou em relação ao trimestre anterior e ao mesmo trimestre do ano passado, e o que isso significa para a tese de investimento. É um trabalho repetitivo, com formato previsível, e exatamente o tipo de tarefa em que agentes de IA já são usados no mercado.

O desafio é construir um **gerador de notas de reação**: a partir do release, produzir uma nota de 1 página, confiável, no formato que um gestor leria.

## Arquivos

Na pasta `releases/`, os releases de resultado mais recentes (PDF) de 4 empresas da B3 de setores diferentes, baixados dos sites de Relações com Investidores:

| Arquivo | Empresa | Setor |
|---|---|---|
| `[PREENCHER].pdf` | [PREENCHER] | Bancos |
| `[PREENCHER].pdf` | [PREENCHER] | Petróleo e gás |
| `[PREENCHER].pdf` | [PREENCHER] | Mineração |
| `[PREENCHER].pdf` | [PREENCHER] | Bens de capital / indústria |

## Cuidados com os dados

1. **Contábil ≠ ajustado/recorrente.** Muitas empresas divulgam lucro "ajustado" ou "recorrente", que exclui efeitos pontuais. Digam qual estão usando e por quê.
2. **Trimestre contra trimestre (T/T) ≠ ano contra ano (A/A).** Negócios sazonais comparam melhor A/A. Deixem claro qual comparação é qual.
3. **Unidades e moedas.** R$ mil, R$ milhões, US$ milhões: releases misturam. Confiram antes de calcular variações.
4. **Setores têm métricas próprias.** Banco: ROE, inadimplência, margem financeira. Petróleo: produção, preço do barril. Mineração: volume, custo por tonelada. Uma boa nota fala a língua do setor.
5. **"Surpresa" exige referência.** Sem consenso de mercado, comparem com o histórico ou com o guidance da própria empresa, e digam isso.
6. **Não é recomendação de investimento.** Coloquem o aviso na nota.

## Caminhos

- **Básico (chat):** suba um release e peça uma nota de reação de 1 página com os números-chave, destaques positivos e negativos e uma conclusão. Depois confira os números no PDF.
- **Avançado (código/agente):** um fluxo em etapas, com o Claude **extraindo** os números, um verificador **conferindo** cada um contra o PDF, e só depois a nota sendo **escrita**. Código de partida e trilha em **[avancado/case3_resultados](../../avancado/case3_resultados/README.md)**.

## Perguntas para atacar

- Quais são os 5 números que um gestor quer ver primeiro para esse setor?
- Onde o Claude erra na leitura do release (tabelas, unidades, trimestres)? Como vocês pegaram o erro?
- Dá para ter um **template** de nota que funciona para qualquer empresa, com seções específicas por setor?
- Como a nota muda se o leitor for um gestor experiente ou um investidor pessoa física?
