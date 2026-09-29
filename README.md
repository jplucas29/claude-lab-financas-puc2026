# Claude Lab · Economia & Finanças — PUC-Rio 2026

**PUC-Rio · sexta, 30 de outubro de 2026 · Sala F300**

Um dia de mão na massa: times multidisciplinares (Economia, Engenharia, Administração e quem mais se interessar por finanças) resolvem, com o **Claude**, problemas reais de análise econômica e financeira, usando **dados públicos brasileiros** (Banco Central, IBGE, CVM e relatórios de empresas), com **mentoria** de quem constrói agentes de IA no mercado e apresentação dos resultados no fim do dia.

Este repositório é o ponto de partida de todos os times: aqui estão os **cases**, os **dados**, as **regras** e as instruções de **entrega**.

> **Com apoio de:** João Lisboa (Claude Community Ambassador no Brasil · Taicor) · [PREENCHER: departamento/coordenação da PUC-Rio e demais parceiros]

---

## Como funciona

1. **Forme o time** (4 pessoas, de preferência com cursos diferentes) e **escolha um case**.
2. **Leia o README do case inteiro antes de começar.** Os dados têm armadilhas, e metade do desafio é interpretá-los certo.
3. **Construa com o Claude.** Explorar dado, escrever código, montar relatório, criar um app: o que fizer o problema andar.
4. **Verifique.** Confira os números do Claude contra a fonte. Isso vale ponto (ver [regras](REGRAS_E_AVALIACAO.md)).
5. **Entregue** até o horário-limite: **vídeo de 2 min**, **artefato público** e **GitHub público** do time.

Nunca usou GitHub ou o Claude? Comece por **[COMO_COMECAR.md](COMO_COMECAR.md)**.

## Os 4 cases

| # | Case | Pergunta central | Dados | Nível |
|---|------|------------------|-------|-------|
| 1 | **[Termômetro do Focus](data/case1_focus/README.md)** | O mercado está acertando? Como as expectativas de IPCA, Selic, câmbio e PIB mudaram e como se comparam ao realizado? | Focus (BCB) + séries do SGS | Iniciante → avançado |
| 2 | **[Tradutor do Copom](data/case2_copom/README.md)** | O que mudou no tom do Banco Central de uma reunião para outra, e o que isso diz sobre os próximos passos da Selic? | Atas do Copom (PDF) + Selic e Focus | Iniciante → avançado |
| 3 | **[Nota de reação a resultado](data/case3_resultados/README.md)** | Em 1 página, como um analista de banco reagiria ao resultado trimestral de uma empresa da B3? | Releases de resultado (PDF) de 4 empresas | Iniciante → avançado |
| 4 | **[Consultor de orçamento](data/case4_orcamento/README.md)** | Para onde vai o dinheiro de uma pessoa e em quanto tempo ela atinge uma meta de investimento? | Extrato bancário **fictício** (CSV) | Iniciante |

Todos os cases têm um **caminho básico**, que só usa o chat do Claude, e um **caminho avançado**, com código (Python/R, Claude Code). Os dois concorrem de igual para igual: a banca avalia o resultado, não a quantidade de código.

## Onde estão os dados

Tudo em **[`data/`](data/)**, uma pasta por case. Cada uma tem um `README.md` com o problema, o dicionário dos dados, os **cuidados de interpretação** e perguntas de partida. Os dados do Banco Central foram baixados com o script em [`scripts/`](scripts/), que você pode rodar de novo para atualizar. Leia **[USO-DOS-DADOS.md](USO-DOS-DADOS.md)** antes de publicar qualquer resultado.

## Cronograma — sexta, 30/10 (Sala F300)

| Horário | Atividade |
|---|---|
| 08h00 – 08h45 | Credenciamento, coffee e **checagem dos notebooks** (login no Claude e GitHub) |
| 08h45 – 09h45 | **Aula 1** — Claude, Claude Code/Cowork e o que é um agente de IA |
| 09h45 – 10h05 | Apresentação dos cases e escolha pelos times |
| 10h05 – 12h05 | **Mão na massa, parte 1** — primeira versão da solução (com mentoria) |
| 12h05 – 13h05 | Almoço |
| 13h05 – 14h05 | **Aula 2** — agentes, skills e conectores aplicados a finanças + dúvidas |
| 14h05 – 15h30 | **Mão na massa, parte 2** — evoluir a solução com o que viram na aula 2 |
| **15h30** | **Deadline da entrega** (vídeo + artefato + GitHub) |
| 15h30 – 15h45 | Coffee · banca seleciona os finalistas |
| 15h45 – 16h30 | Apresentação dos finalistas (5 min + 2 min de perguntas) |
| 16h30 – 16h50 | Premiação e encerramento |

## Entrega e avaliação

Três itens, todos **públicos**, até **15h30**:

1. **Vídeo de até 2 minutos** mostrando o problema e a solução funcionando.
2. **Artefato público**: o link publicado do que vocês construíram (app, dashboard, relatório interativo).
3. **GitHub público do time**, com README detalhado do projeto e da equipe (use o [modelo](modelo-repositorio-time/README.md)).

Detalhes, critérios e pesos em **[REGRAS_E_AVALIACAO.md](REGRAS_E_AVALIACAO.md)**. Checklist em **[entrega/CHECKLIST_ENTREGA.md](entrega/CHECKLIST_ENTREGA.md)**. Os links de todos os times ficam em **[ENTREGAS.md](ENTREGAS.md)**.

## Organização

**Organizadores:** [PREENCHER: nomes e cursos]
**Mentoria e banca:** [PREENCHER]
**Apoio institucional:** [PREENCHER: Departamento de Economia da PUC-Rio / coordenação]

## Estrutura do repositório

```
claude-lab-financas-puc2026/
├── README.md                    você está aqui
├── COMO_COMECAR.md              setup do Claude e do GitHub — comece aqui
├── REGRAS_E_AVALIACAO.md        regras, horário-limite, critérios e pesos
├── USO-DOS-DADOS.md             fontes, créditos e uso responsável
├── ENTREGAS.md                  links finais de todos os times
├── data/
│   ├── case1_focus/             expectativas Focus + séries SGS (CSV)
│   ├── case2_copom/             atas do Copom (PDF)
│   ├── case3_resultados/        releases trimestrais (PDF)
│   └── case4_orcamento/         extrato bancário fictício (CSV)
├── scripts/
│   └── baixar_dados_bcb.py      baixa/atualiza os dados do Banco Central
├── modelo-repositorio-time/
│   └── README.md                modelo do README do GitHub de cada time
└── entrega/
    └── CHECKLIST_ENTREGA.md
```
