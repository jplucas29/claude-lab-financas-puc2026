# Como começar

Guia rápido para o dia do workshop. Dá para fazer tudo em 15 minutos, e o ideal é fazer **antes** do evento.

## 1. O que você precisa (traga no dia)

- **Notebook carregado** (e carregador; vai ter extensão, mas não para todo mundo ao mesmo tempo).
- **Conta no Claude** (claude.ai), já logada no navegador.
- **Conta no GitHub** (grátis, em github.com), já logada.
- *Opcional, para o caminho avançado:* Python 3 com `pandas`, ou R, e o Claude Code instalado.

## 2. Pegar os dados

**Mais simples:** nesta página do repositório, clique em **Code → Download ZIP** e descompacte. Todos os dados dos cases estão na pasta `data/`.

**Se você usa Git:**

```bash
git clone <URL-DESTE-REPOSITORIO>
cd claude-lab-financas-puc2026
```

## 3. Construir com o Claude

**Caminho básico (só o chat):** abra o claude.ai, suba os arquivos do seu case (CSV ou PDF) e o `README.md` do case, e descreva o que vocês querem. O Claude lê PDFs, analisa planilhas, faz gráficos e pode criar um **artefato** (um app ou página interativa) direto na conversa.

**Caminho avançado (código):** use o Claude Code na pasta do repositório para escrever scripts em Python/R, puxar dados atualizados das APIs do Banco Central (veja `scripts/baixar_dados_bcb.py`) e automatizar o fluxo.

**Dicas que fazem diferença:**

- Comece colando o **README do case** e pedindo um **plano de ataque**, antes de pedir o resultado final.
- Descreva **o problema e o resultado que você quer** ("uma nota de 1 página para um gestor, com 3 números-chave"), não só a tarefa ("analisa isso").
- **Confira os números.** Peça ao Claude para mostrar de onde veio cada número e confira pelo menos alguns na fonte. Vocês são responsáveis pelas conclusões.
- Itere: primeira versão simples funcionando → depois melhora.

## 4. Publicar o artefato

Quando o artefato (app, dashboard, relatório) estiver pronto no Claude, use a opção de **publicar/compartilhar** e copie o **link público**. Abra o link numa aba anônima para conferir que qualquer pessoa consegue ver.

## 5. Criar o GitHub do time (sem precisar de linha de comando)

1. No GitHub, clique em **New repository**. Nome sugerido: `claude-lab-<nome-do-time>`. Marque **Public**.
2. Clique em **Add file → Upload files** e suba o que vocês construíram (código, notebooks, prints, o PDF do relatório).
3. Crie o `README.md` a partir do [modelo](modelo-repositorio-time/README.md) e preencha.
4. Coloque no README os links do **vídeo** e do **artefato**.

Se usar Git no terminal:

```bash
git add .
git commit -m "Primeira versão da solução"
git push
```

## 6. Gravar o vídeo de 2 minutos

Qualquer gravador de tela serve (Loom, OBS, gravador do sistema ou celular). Suba no YouTube como **não listado**, ou no Google Drive com acesso "qualquer pessoa com o link". Roteiro em [entrega/CHECKLIST_ENTREGA.md](entrega/CHECKLIST_ENTREGA.md).

## Travou?

Chame um **mentor**. Eles circulam por todas as mesas. Perguntar cedo é mais barato que perder uma hora.
