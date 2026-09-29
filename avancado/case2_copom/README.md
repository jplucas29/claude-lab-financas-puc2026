# Caminho avançado — Case 2 · Tradutor do Copom

Leia antes o [README do case](../../data/case2_copom/README.md) (problema e cuidados com os dados).

## Ponto de partida

```bash
python avancado/case2_copom/extrair_atas.py
```

O script [`extrair_atas.py`](extrair_atas.py) extrai o texto das atas, quebra em parágrafos e gera um **diff** entre as duas mais recentes (o que entrou, saiu e mudou, palavra por palavra) em `saida/`. É o "diff de tom da ata" do material do workshop, só que feito por vocês.

## Trilha sugerida

**Manhã — nível 1: o diff confiável**
- Rodem o script e **confiram no PDF** se a quebra em parágrafos funcionou. PDF é traiçoeiro: cabeçalho, rodapé e tabelas atrapalham. Ajustem `em_paragrafos()` se precisar.
- Com o diff em mãos, escrevam (com o Claude) a nota "o que mudou nesta ata" para um investidor leigo.

**Manhã — nível 2: índice de tom**
- Classifiquem cada parágrafo numa régua de −2 (muito brando) a +2 (muito duro).
  - **Com chave da API:** [`classificar_tom.py`](classificar_tom.py) faz isso sozinho.
  - **Sem chave:** peçam ao Claude Code para classificar `saida/paragrafos_atas.csv` usando a mesma régua do `PROMPT` do script.
- **Validem o índice:** peguem 10 parágrafos, classifiquem vocês mesmos e comparem com o Claude. Onde discordam, e por quê?

**Tarde (depois da aula 2) — nível 3: o agente do dia da ata**

Montem o fluxo que um economista-chefe gostaria de receber no dia em que sai uma ata nova:

1. extrair a ata nova;
2. comparar com a anterior (diff);
3. classificar o tom e comparar com o histórico das 6 reuniões;
4. cruzar com a trajetória da Selic e das expectativas de Selic do Focus (dados do Case 1);
5. **um segundo Claude revisa a nota** procurando afirmações sem base no texto da ata (padrão *evaluator-optimizer* da aula);
6. publicar a nota como artefato.

## Onde o Claude Code ajuda

- "A função em_paragrafos está juntando o rodapé com o parágrafo 12. Corrija."
- "Faça um gráfico do tom médio por reunião com a Selic meta no eixo secundário."
- "Leia o diff e liste as 3 mudanças mais relevantes, citando o nº do parágrafo."
