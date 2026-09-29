# Case 2 — Tradutor do Copom

Claude Lab · Economia & Finanças · PUC-Rio · 30/10/2026

## O problema

A cada 45 dias o Comitê de Política Monetária (Copom) decide a Selic e, alguns dias depois, publica uma **ata** explicando o raciocínio. O mercado lê cada ata palavra por palavra: uma troca de "vigilante" por "atento", ou um parágrafo novo sobre câmbio, pode mexer nos juros futuros. Mas as atas são longas, técnicas e parecidas entre si, e é difícil ver **o que mudou**.

O desafio é construir um **tradutor do Copom**: algo que compare as atas, identifique mudanças de tom (mais duro/"hawkish" ou mais brando/"dovish") e explique, para quem não é especialista, o que isso pode significar para os juros.

## Arquivos

| Arquivo | O que é |
|---|---|
| `atas/` | As **6 atas mais recentes** do Copom, em PDF, nomeadas `ata_<nº da reunião>_<data da reunião>.pdf` |
| `../case1_focus/sgs_realizado.csv` | Selic meta realizada (série `selic_meta`) |
| `../case1_focus/focus_anuais.csv` | Expectativas de Selic do mercado (`Indicador = Selic`) |

## Cuidados com os dados

1. **Data da reunião ≠ data da ata.** A decisão sai no dia da reunião; a ata é publicada alguns dias depois. Para medir reação de mercado, isso importa.
2. **Mudança de tom não é mudança de juros.** Uma ata pode manter a Selic e endurecer muito o discurso. Separe "o que decidiram" de "como falaram".
3. **Não confie num índice de tom sem olhar o texto.** Se criarem uma nota numérica (ex.: de −5 a +5), mostrem os trechos que justificam a nota, com as próprias palavras.
4. Estrutura parecida entre atas ajuda a comparar seção por seção. Use isso.

## Caminhos

- **Básico (chat):** suba duas ou mais atas e peça uma comparação lado a lado: o que entrou, o que saiu, o que mudou de tom. Depois transforme isso numa nota para um investidor leigo.
- **Avançado (código):** extrair e comparar as atas automaticamente, criar um **índice de tom** reprodutível e cruzar com a Selic e as expectativas do Focus. Código de partida e trilha em **[avancado/case2_copom](../../avancado/case2_copom/README.md)**.

## Perguntas para atacar

- Entre as duas atas mais recentes, quais foram as 3 mudanças mais relevantes?
- O tom endureceu ou suavizou ao longo das 6 reuniões? Um gráfico disso bate com o que a Selic fez depois?
- As expectativas de Selic do Focus se moveram depois das atas mais duras?
- Como explicar uma ata para alguém de 18 anos em 5 frases, sem perder o que importa?
- Um agente que, no dia da publicação, já entrega o "diff" da ata nova contra a anterior: como seria?
