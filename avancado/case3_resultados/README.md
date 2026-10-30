# Caminho avançado — Case 3 · Comparativo de resultados

Leia antes o [README do case](../../data/case3_resultados/README.md) (demanda, resultado esperado e cuidados com os dados).

## A ideia: extrair → verificar → comparar → apresentar

O erro clássico de IA em finanças é o número errado escrito com confiança. Por isso o trabalho é separado em **etapas encadeadas**, com uma checagem automática no meio:

```
PDFs dos releases ──► 1. extrair texto e tabelas ──► 2. Claude extrai métricas para JSON
                                                              │
4. comparar as empresas ◄── 3. verificador confere cada número no PDF
         │                      (se falhar, volta para o passo 2)
         ▼
5. Claude gera a apresentação (só com números verificados)
```

## Ponto de partida

```bash
# 1. extrair os releases (pode passar vários PDFs de uma vez)
python avancado/case3_resultados/extrair_release.py data/case3_resultados/releases/*.pdf

# 2. peça ao Claude um metricas.json por empresa, no formato de metricas_exemplo.json
#    (inclusive 'valor_como_aparece', 'pagina' e 'trecho' de cada número)

# 3. verificar cada empresa
python avancado/case3_resultados/verificar_numeros.py avancado/case3_resultados/saida/EMPRESA metricas_EMPRESA.json
```

Arquivos de apoio:
- [`extrair_release.py`](extrair_release.py): texto por página e tabelas em CSV.
- [`metricas_exemplo.json`](metricas_exemplo.json): o formato que o Claude deve preencher.
- [`verificar_numeros.py`](verificar_numeros.py): confere trecho, valor, unidade e variação de cada métrica contra o PDF.
- [`roteiro_apresentacao.md`](roteiro_apresentacao.md): roteiro dos slides da apresentação executiva.

## Trilha sugerida

**Manhã — nível 1: uma empresa verificada**
- O fluxo completo para uma empresa: extrair, pedir o JSON ao Claude, verificar e corrigir.
- Anotem **cada erro** que o verificador pegou.

**Manhã — nível 2: o setor inteiro**
- Rodem o fluxo para as 4 a 5 empresas e juntem tudo numa tabela comparativa, na mesma unidade e moeda.
- Gerem os gráficos da comparação (receita, margens).

**Tarde (depois da palestra das 13h30) — nível 3: a apresentação**
- Escrevam a mensagem de cada slide seguindo o [`roteiro_apresentacao.md`](roteiro_apresentacao.md).
- Gerem a apresentação a partir da tabela verificada: `.pptx` com `python-pptx` (ou direto no claude.ai) ou slides em HTML.
- Bônus: um comando só que recebe a pasta de PDFs e devolve a apresentação, **refazendo automaticamente o que o verificador reprovar**.
- Bônus: transformem o formato da apresentação numa **Skill** (`SKILL.md`, veja o [exemplo](../skills_exemplo/bcb-sgs/SKILL.md)), para que o Claude sempre monte os slides no padrão da "casa".

## Onde o Claude Code ajuda

- "Leia saida/EMPRESA/texto_completo.txt e gere metricas_EMPRESA.json no formato de metricas_exemplo.json com as métricas que definimos."
- "Rode o verificador e corrija no JSON só os itens com ERRO, olhando a página indicada."
- "Junte os JSONs verificados numa tabela comparativa em R$ milhões e faça um gráfico de margem EBITDA por empresa."
- "Gere apresentacao.pptx com python-pptx seguindo roteiro_apresentacao.md, usando só os números verificados."
