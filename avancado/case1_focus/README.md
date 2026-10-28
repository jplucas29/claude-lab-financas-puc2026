# Caminho avançado — Case 1 · Termômetro do Focus

Leia antes o [README do case](../../data/case1_focus/README.md) (problema e cuidados com os dados).

## Ponto de partida

```bash
python scripts/baixar_dados_bcb.py            # opcional: atualiza os dados
python avancado/case1_focus/inicio_focus.py   # roda a análise inicial
```

O script [`inicio_focus.py`](inicio_focus.py) já carrega e limpa os dados, mede se o mercado acertou IPCA e Selic, gera alertas e salva um gráfico em `saida/`. **Leiam o código antes de usar**: vocês vão ter que defender cada número.

## Trilha sugerida

**Manhã — nível 1: análise confiável**
- Rodem o script e **confiram 3 números à mão** (ex.: o IPCA de 2025 composto a partir de `ipca_mensal` contra a série `ipca_12m` de dezembro).
- Resolvam os `TODO` de acurácia: Câmbio e PIB.
- Respondam: o mercado erra mais para cima ou para baixo? Erra mais em que tipo de ano?

**Manhã — nível 2: alertas que fazem sentido**
- Calibrem os limiares de alerta com o histórico, em vez dos valores chutados no código.
- Incluam a dispersão (`DesvioPadrao`): ela sobe antes das grandes revisões?

**Tarde (depois da palestra das 13h30) — nível 3: o agente que envia o e-mail**

Transformem a análise no **agente semanal** descrito no [README do case](../../data/case1_focus/README.md#o-objetivo):

1. baixar dados novos (`scripts/baixar_dados_bcb.py`);
2. recalcular tudo e salvar um `resumo_semana.json`;
3. **checar** (a data mais recente é desta semana? algum valor absurdo?) e parar se algo falhar;
4. pedir ao Claude a análise **a partir do JSON**, nunca inventando números;
5. montar e enviar o e-mail (credenciais em `.env` ou nos Secrets do GitHub, nunca no código);
6. agendar com GitHub Actions (`schedule` + `workflow_dispatch`) para rodar toda segunda.

Bônus: transformem o procedimento numa **Skill** (veja o [exemplo](../skills_exemplo/bcb-sgs/SKILL.md)) para que qualquer pessoa rode o termômetro com um pedido só.

## Onde o Claude Code ajuda

- "Leia `inicio_focus.py` e o README do case e me explique o que cada função faz."
- "Adicione a acurácia do câmbio usando o último valor de dezembro da série dolar_ptax_venda."
- "Escreva um teste que confere o IPCA anual composto contra a série ipca_12m."
