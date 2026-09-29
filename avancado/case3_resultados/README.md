# Caminho avançado — Case 3 · Nota de reação a resultado

Leia antes o [README do case](../../data/case3_resultados/README.md) (problema e cuidados com os dados).

## A ideia: extrair → verificar → escrever

O erro clássico de IA em finanças é o número errado escrito com confiança. Por isso o caminho avançado separa o trabalho em **etapas encadeadas**, com uma checagem automática no meio (o *prompt chaining* e o loop "agir → verificar" da aula):

```
PDF do release ──► 1. extrair texto e tabelas ──► 2. Claude extrai métricas para JSON
                                                            │
                     4. Claude escreve a nota ◄── 3. verificador confere cada número no PDF
                        (só com números OK)          (se falhar, volta para o passo 2)
```

## Ponto de partida

```bash
# 1. extrair o release
python avancado/case3_resultados/extrair_release.py data/case3_resultados/releases/EMPRESA.pdf

# 2. peça ao Claude um metricas.json no formato de metricas_exemplo.json
#    (inclusive 'valor_como_aparece', 'pagina' e 'trecho' de cada número)

# 3. verificar
python avancado/case3_resultados/verificar_numeros.py avancado/case3_resultados/saida/EMPRESA metricas.json
```

Arquivos de apoio:
- [`extrair_release.py`](extrair_release.py): texto por página e tabelas em CSV.
- [`metricas_exemplo.json`](metricas_exemplo.json): o formato que o Claude deve preencher.
- [`verificar_numeros.py`](verificar_numeros.py): confere trecho, valor, unidade e variação de cada métrica contra o PDF.
- [`template_nota.md`](template_nota.md): estrutura de nota de reação no formato de banco.

## Trilha sugerida

**Manhã — nível 1: uma nota verificada**
- Uma empresa, o fluxo completo à mão: extrair, pedir o JSON ao Claude, verificar, corrigir e escrever a nota com o template.
- Anotem **cada erro** que o verificador pegou. Isso vira ouro no vídeo e no README do time.

**Manhã — nível 2: as 4 empresas**
- Rodem o fluxo para as 4 empresas e montem uma tabela comparativa.
- Adaptem o template por setor: banco não tem "margem EBITDA".

**Tarde (depois da aula 2) — nível 3: o agente de temporada de balanços**
- Um comando só: recebe o PDF, extrai, gera o JSON, verifica, **refaz automaticamente o que falhou** (até N tentativas) e escreve a nota.
- Transformem o formato da nota numa **Skill** (`SKILL.md`, veja o [exemplo](../skills_exemplo/bcb-sgs/SKILL.md)), para que o Claude sempre escreva no padrão da "casa".
- Bônus: um segundo Claude lê a nota como se fosse o gestor e aponta o que ficou vago (padrão *evaluator-optimizer*).

## Onde o Claude Code ajuda

- "Leia saida/EMPRESA/texto_completo.txt e gere metricas.json no formato de metricas_exemplo.json com as 8 métricas principais."
- "Rode o verificador e corrija no metricas.json só os itens com ERRO, olhando a página indicada."
- "Escreva a nota em template_nota.md usando apenas as métricas que passaram na verificação."
