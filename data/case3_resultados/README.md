# Case 3 — Comparativo de resultados: apresentação executiva

Claude Lab · Economia & Finanças · PUC-Rio · 06/11/2026

## Contexto

Temporada de balanços. Em poucos dias, todas as empresas da bolsa divulgam seus **releases de resultado**: PDFs longos, cheios de tabelas, cada um com seu formato, suas unidades e seus "ajustes". Num banco ou numa gestora, alguém precisa ler esses documentos rápido, **comparar as empresas de um mesmo setor** e levar a conclusão para quem decide, num formato que se lê em poucos minutos: uma apresentação.

Neste case, vocês são os **estagiários/analistas** desse time.

## A demanda

> **De:** Diretor de Research
> **Para:** time de análise
> **Assunto:** Resultados do setor — preciso de slides para o comitê
>
> "Saíram os resultados do trimestre. Quero comparar lado a lado de 4 a 5 empresas do mesmo setor: receita, margem EBITDA, lucro e as métricas que importam para esse setor. Monte uma **apresentação curta, de 6 a 8 slides**, para eu levar ao comitê de investimentos. Todo número precisa ter a página do release de onde saiu. Não quero tabelão: quero a mensagem de cada slide no título."

## Resultado esperado

Uma **apresentação executiva** (PowerPoint ou HTML), gerada com o Claude a partir dos números extraídos e conferidos. Uma estrutura que funciona:

1. **Capa** com a mensagem principal do trimestre para o setor;
2. **Resumo executivo**: as 3 conclusões que o diretor precisa levar;
3. **Tabela comparativa** das métricas principais (mesma unidade e mesma moeda para todas);
4. **Gráficos**: crescimento de receita e margens, empresa a empresa;
5. **Destaques por empresa**: o que foi positivo e negativo em cada uma;
6. **Métricas do setor** (ex.: no varejo, vendas mesmas lojas e GMV; nos bancos, ROE e inadimplência);
7. **Riscos e o que observar** no próximo trimestre;
8. **Fontes e metodologia**: páginas dos releases, contábil × ajustado, aviso de que não é recomendação de investimento.

O artefato público é a versão publicada da apresentação (HTML ou PDF).

> A diferença para o Case 2: lá o produto é um site para conversar com os dados. Aqui o produto é uma **apresentação pronta para a diretoria**, que cruza várias empresas.

## Os dados

Os dados **não vêm prontos**: escolher o setor e buscar os releases faz parte do trabalho.

- Escolham **um setor** e **4 a 5 empresas** dele, por exemplo varejo e e-commerce (Mercado Livre, Magazine Luiza…) ou bancos (Itaú, Bradesco, Banco do Brasil…);
- baixem o **release de resultados** (não a apresentação de slides nem as demonstrações completas) do **mesmo trimestre** para todas, na *Central de Resultados* do site de Relações com Investidores de cada empresa. Usem o trimestre mais recente que todas já divulgaram;
- salvem os PDFs em `data/case3_resultados/releases/` (é onde os scripts procuram) e registrem no README do time quais empresas, qual trimestre e de onde baixaram.

> Não gastem mais de 15 minutos aqui. Se travar (internet, site fora do ar), chamem um mentor.

## Guia sugerido

O guia é uma sugestão de caminho, não uma receita. O que importa é entregar o resultado esperado com números confiáveis.

### 1. Definir as métricas

Antes de extrair qualquer coisa, decidam **o que comparar**: de 5 a 8 métricas, comuns a todas as empresas (receita, EBITDA, margem EBITDA, lucro líquido) mais as do setor. Combinem as definições: contábil ou ajustado? Qual moeda? Qual comparação (ano contra ano ou trimestre contra trimestre)?

### 2. Extrair os números com o Claude

Para cada empresa, peçam ao Claude para extrair as métricas do release para uma tabela (JSON ou CSV), **cada número com a página e o trecho de onde saiu**. O formato está em [`metricas_exemplo.json`](../../avancado/case3_resultados/metricas_exemplo.json).

### 3. Verificar

Rodem o [`verificar_numeros.py`](../../avancado/case3_resultados/verificar_numeros.py), que confere cada número contra o PDF (trecho, valor, unidade e variação). Corrijam o que falhar e **anotem os erros que o verificador pegou**: isso vale ouro no vídeo e no README do time.

### 4. Comparar

Juntem as empresas numa tabela única, **na mesma unidade e moeda**, calculem as variações e montem os gráficos. É aqui que aparece a história: quem cresceu mais, quem ganhou margem, quem decepcionou.

### 5. Escrever a história

Antes de gerar os slides, escrevam (com o Claude) a **mensagem de cada slide**, uma frase por título. Exemplo: "Mercado Livre cresce o dobro do setor, mas margem cai 2 p.p." em vez de "Receita".

### 6. Gerar a apresentação

Peçam ao Claude para gerar a apresentação **a partir da tabela verificada e das mensagens**, nunca de memória:

- **PowerPoint:** o Claude cria o arquivo `.pptx` direto no claude.ai, ou pelo Claude Code com a biblioteca `python-pptx`;
- **HTML:** slides numa página web, que já servem como artefato publicado.

Revisem como se fossem o diretor: algum número sem fonte? Algum título que não diz nada?

### Ritmo sugerido no dia

| Momento | Passos |
|---|---|
| Manhã (10h30–13h00) | dados, 1 a 3: setor escolhido, métricas definidas, números extraídos e verificados |
| Tarde (14h00–16h00) | 4 a 6: comparação, mensagens, apresentação gerada e publicada |

Guia técnico em **[avancado/case3_resultados](../../avancado/case3_resultados/README.md)**.

## Cuidados com os dados

1. **Contábil ≠ ajustado/recorrente.** Muitas empresas divulgam lucro "ajustado" ou "recorrente", que exclui efeitos pontuais. Digam qual estão usando e usem o mesmo critério para todas.
2. **Trimestre contra trimestre (T/T) ≠ ano contra ano (A/A).** Negócios sazonais, como o varejo, comparam melhor A/A.
3. **Unidades e moedas.** R$ mil, R$ milhões, US$ milhões: releases misturam. Algumas empresas reportam em dólar (o Mercado Livre, por exemplo) e com outro padrão contábil. Convertam e digam como converteram.
4. **Mesmo trimestre para todas.** Comparar o 2º trimestre de uma com o 3º de outra invalida a comparação.
5. **Setores têm métricas próprias.** Uma boa apresentação fala a língua do setor.
6. **"Surpresa" exige referência.** Sem consenso de mercado, comparem com o histórico ou com o guidance da própria empresa, e digam isso.
7. **Não é recomendação de investimento.** Coloquem o aviso na apresentação.

## O que a banca vai olhar neste case

- A apresentação está **pronta para um diretor**: curta, com a mensagem no título de cada slide?
- Todo número tem **fonte (página do release)** e foi **verificado**?
- A comparação é **justa** (mesmo trimestre, mesma unidade, mesmo critério)?
- O time mostrou **onde o Claude errou** e como pegou o erro?

## Perguntas para atacar

- Quais são as 5 métricas que um diretor quer ver primeiro para esse setor?
- Quem foi o destaque positivo e o negativo do trimestre, e por quê?
- Onde o Claude erra na leitura do release (tabelas, unidades, trimestres)? Como vocês pegaram o erro?
- Dá para ter um **fluxo** que gera a apresentação para qualquer setor, trocando só os PDFs?
