# Case 2 — Tradutor do Copom

Claude Lab · Economia & Finanças · PUC-Rio · 06/11/2026

## O problema

A cada 45 dias o Comitê de Política Monetária (Copom) decide a Selic e, alguns dias depois, publica uma **ata** explicando o raciocínio. O mercado lê cada ata palavra por palavra: uma troca de "vigilante" por "atento", ou um parágrafo novo sobre câmbio, pode mexer nos juros futuros. Mas as atas são longas, técnicas e parecidas entre si, e é difícil ver **o que mudou** e **o que isso significa**.

## O objetivo

Construir um **site** que interprete o tom do Banco Central nas atas do Copom e mostre, de forma que um leigo entenda:

1. **Um índice de tom**: uma nota para cada ata, numa régua de −2 (muito brando, *dovish*) a +2 (muito duro, *hawkish*), e como ela evoluiu nas últimas 6 reuniões;
2. **O que mudou**: as principais diferenças da ata mais recente em relação à anterior (o que entrou, o que saiu, o que mudou de tom);
3. **O principal impacto**: o que essa mudança pode significar para a Selic, os juros futuros, o câmbio ou as expectativas do mercado.

O site é o **artefato** que o time entrega (ver [regras](../../REGRAS_E_AVALIACAO.md)). Dá para fazer tudo no chat do Claude (caminho básico) ou com código (caminho avançado).

## Arquivos

| Arquivo | O que é |
|---|---|
| `atas/` | As **6 atas mais recentes** do Copom, em PDF, nomeadas `ata_<nº da reunião>_<data da reunião>.pdf` |
| `../case1_focus/sgs_realizado.csv` | Selic meta realizada (série `selic_meta`) |
| `../case1_focus/focus_anuais.csv` | Expectativas de Selic do mercado (`Indicador = Selic`) |

## Como chegar lá: passo a passo

### 1. Entender a ata

Antes de medir qualquer coisa, o time precisa saber ler uma ata. Suba a ata mais recente no Claude e peça:

> "Explique a estrutura desta ata do Copom, seção por seção. Em quais seções o Banco Central costuma sinalizar os próximos passos da Selic? Dê exemplos de frases que indicam um tom mais duro e um tom mais brando."

Ao final, o time deve saber onde ficam a avaliação do cenário, o balanço de riscos e a decisão com a orientação para as próximas reuniões. É ali que o tom aparece com mais força.

### 2. Definir a régua do índice

Esta é a decisão mais importante do case, e a banca vai perguntar sobre ela. Sugestão de régua, aplicada **parágrafo a parágrafo**:

| Nota | Significado | Exemplo de sinal |
|---|---|---|
| +2 | muito duro | indica alta de juros ou juros altos "por período prolongado", com ênfase |
| +1 | duro | preocupação com inflação, expectativas desancoradas, riscos para cima |
| 0 | neutro | descrição de dados e fatos, sem sinalização |
| −1 | brando | inflação cedendo, atividade desacelerando, riscos para baixo |
| −2 | muito brando | indica corte de juros com ênfase |

Depois, decidam **como juntar os parágrafos num número por ata**. A média simples é o ponto de partida, mas parágrafos neutros diluem o sinal. Testem ignorar as notas 0 ou dar peso maior à seção da decisão. Escrevam a régua e a regra de agregação no README do time.

### 3. Medir o tom das 6 atas e conferir

Peçam ao Claude para classificar cada parágrafo das 6 atas com a régua, sempre com **nº do parágrafo, nota e justificativa curta**, e para calcular o índice de cada ata.

Antes de confiar no índice, **validem**: o time classifica 10 parágrafos à mão, sem olhar a resposta do Claude, e compara. Onde discordarem, ajustem a régua e rodem de novo. Montem uma tabela: reunião · data · decisão da Selic · índice de tom.

### 4. Encontrar o que mudou

Comparem a ata mais recente com a anterior: parágrafos novos, removidos e alterados (palavras trocadas). Escolham as **3 mudanças mais relevantes** e, para cada uma, registrem o nº do parágrafo, um trecho curto (ou um resumo com as próprias palavras) e o efeito no índice: endureceu ou suavizou?

> "Compare a ata nova com a anterior. Liste parágrafos novos, removidos e alterados, citando o nº do parágrafo. Depois aponte as 3 mudanças que mais alteram o tom e explique por quê."

### 5. Interpretar o principal impacto

Para a mudança mais relevante, respondam: **o que ela sinaliza para a próxima decisão da Selic?** Usem os dados do Case 1 para fundamentar:

- a trajetória da **Selic meta** (`sgs_realizado.csv`): o tom acompanhou o que o Copom fez depois?
- as **expectativas de Selic do Focus** (`focus_anuais.csv`): o mercado já esperava essa mudança? As expectativas se moveram depois da ata?

Escrevam o "principal impacto" em 2 ou 3 frases simples, citando os canais (Selic, juros futuros, câmbio, crédito e consumo, inflação esperada) e usando linguagem condicional ("sinaliza", "pode"). Interpretação não é previsão.

### 6. Montar e publicar o site

Peçam ao Claude um **artefato** a partir dos resultados dos passos anteriores, não de memória. Uma estrutura que funciona:

1. **Topo:** a última reunião, a decisão e o índice atual, num "termômetro" do tom;
2. **Gráfico:** o índice nas 6 reuniões, com a Selic meta ao lado;
3. **O que mudou:** as 3 mudanças, com nº do parágrafo e o efeito no tom;
4. **Principal impacto:** o texto do passo 5;
5. **Como medimos:** a régua, a regra de agregação e como o time conferiu;
6. **Rodapé:** fonte (Banco Central do Brasil, atas do Copom, datas) e aviso de que não é recomendação de investimento.

Publiquem e abram o link numa aba anônima. **Bônus:** um campo em que o usuário cola uma ata nova e o site recalcula o índice e o "o que mudou".

### Ritmo sugerido no dia

| Momento | Passos |
|---|---|
| Manhã (10h30–13h00) | 1 a 4: régua definida, índice das 6 atas conferido e mudanças identificadas |
| Tarde (14h00–16h00) | 5 e 6: impacto escrito, site publicado, vídeo gravado |

## Caminhos

- **Básico (chat):** todos os passos dentro do claude.ai. Subam as atas em PDF e os dois CSVs do Case 1, façam os passos 1 a 5 na conversa e peçam o site no passo 6.
- **Avançado (código):** scripts extraem as atas, geram o diff e classificam os parágrafos automaticamente, e o site é montado a partir desses resultados. Código de partida e trilha em **[avancado/case2_copom](../../avancado/case2_copom/README.md)**.

## Cuidados com os dados

1. **Data da reunião ≠ data da ata.** A decisão sai no dia da reunião; a ata é publicada alguns dias depois. Para medir reação de mercado, isso importa.
2. **Mudança de tom não é mudança de juros.** Uma ata pode manter a Selic e endurecer muito o discurso. Separe "o que decidiram" de "como falaram".
3. **Não confie num índice de tom sem olhar o texto.** Toda nota precisa de um trecho que a justifique, com nº do parágrafo.
4. **Estrutura parecida entre atas** ajuda a comparar seção por seção. Use isso.
5. **Impacto é interpretação, não previsão.** Fundamentem nos dados e no texto, deixem claro o que é hipótese e coloquem o aviso de que não é recomendação de investimento.

## O que a banca vai olhar neste case

- A régua do índice é clara e o resultado é **reprodutível**?
- Cada mudança apontada **existe de fato na ata** (nº do parágrafo)?
- O impacto está **fundamentado** em dados e explicado em linguagem simples?
- O site é claro para quem não é economista?

## Perguntas para atacar

- O tom endureceu ou suavizou ao longo das 6 reuniões? O índice antecipou o que a Selic fez depois?
- As expectativas de Selic do Focus se moveram depois das atas mais duras?
- Qual seção da ata mais mexe no índice?
- Como explicar uma ata para alguém de 18 anos em 5 frases, sem perder o que importa?
