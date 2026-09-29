# Case 4 — Consultor de orçamento

Claude Lab · Economia & Finanças · PUC-Rio · 30/10/2026

> **Todos os dados deste case são fictícios**, gerados pela organização. Não existe pessoa real por trás deles.

## O problema

A maioria das pessoas não sabe para onde vai o próprio dinheiro. O extrato do banco é uma lista confusa de siglas ("IFD*IFOOD", "PAG*…"), o cartão de crédito vive numa fatura separada, e a pergunta que importa ("quanto consigo guardar por mês e quando chego na minha meta?") fica sem resposta.

**A persona:** Marina, 26 anos, mora no Rio e trabalha numa empresa de tecnologia com salário líquido de R$ 6.500. Ela acha que guarda R$ 800 por mês e quer juntar **R$ 30.000** para uma pós-graduação no exterior. Ela te entregou 6 meses de extrato (abril a setembro de 2026) e perguntou: *"estou no caminho certo?"*

O desafio é construir um **consultor de orçamento**: algo que organize os gastos, encontre o que está pesando e responda à pergunta da Marina com números confiáveis.

## Arquivos

| Arquivo | O que é |
|---|---|
| `conta_corrente.csv` | Movimentações da conta corrente: `data`, `descricao`, `valor` (negativo = saída) |
| `cartao_credito.csv` | Lançamentos do cartão: `data`, `descricao`, `valor` (negativo = compra), `parcela` (ex.: `3/10`) |

## Cuidados com os dados (leia antes de concluir qualquer coisa)

Extrato real é bagunçado, e este também é. Antes de somar qualquer coisa:

1. **Nem toda saída é gasto, e nem toda entrada é renda.** Há movimentos entre contas da própria Marina.
2. **As duas fontes se sobrepõem.** Pensem em como a conta corrente e o cartão se relacionam antes de juntar tudo.
3. **Há lançamentos que se anulam e lançamentos que parecem repetidos.** Decidam o que é real.
4. **Parcelas continuam depois do período do extrato.** Isso muda o futuro, não só o passado.
5. **Gastos pontuais não são gastos mensais.** Tratem separadamente o que é recorrente e o que é extraordinário.
6. As descrições são siglas: a categorização é parte do desafio.

Documentem cada decisão de tratamento no README do time. A banca vai perguntar.

## Caminhos

- **Principal (chat):** suba os dois CSVs e peça ao Claude para categorizar, montar um resumo mensal e responder à Marina. Depois questione: os totais fazem sentido para um salário de R$ 6.500?
- **Para ir além (ainda no chat):** peçam ao Claude um app (artefato) em que a Marina vê o painel de gastos, ajusta categorias e simula cenários ("e se eu cortar delivery pela metade?").

## Perguntas para atacar

- Quanto a Marina **de fato** guarda por mês? É o que ela acha?
- Quais 3 categorias mais pesam, e quais são cortáveis sem mudar a vida dela?
- Com a poupança real de hoje, quando ela chega nos R$ 30.000? E com 2 ou 3 ajustes realistas? Considerem que o dinheiro guardado rende (por exemplo, um percentual do CDI; use a Selic do Case 1).
- Como apresentar isso para a Marina de um jeito que ela entenda e aja, sem sermão?
