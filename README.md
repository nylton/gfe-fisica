# Guia de Fórmulas Explicadas — Física Volume I (Mecânica)

Livro didático em LaTeX, ~600 páginas, A4, classe `book`, fontes Baskervaldx + STIX2.
Autoria: Felipe Guisoli.

Repositório oficial: `nylton/gfe-fisica` (GitHub).
Sincronização contínua com Overleaf Premium via integração GitHub.

---

## Workflow de trabalho

```
Edição local (Claude Code / editor)  →  commit local  →  git push origin main  →  Overleaf Pull from GitHub  →  Recompile
```

- **`original` branch** + tag **`v0-pristine`** são imutáveis — fallback total.
- Cada bloco lógico de mudança vira commit separado pra facilitar revert pontual.
- Todo push é authorized.

---

## Histórico de transformações

### Fase 0 — Pristine + blend mode (2026-05-27)

| Commit | Descrição |
|---|---|
| `f3c65ba` | Snapshot pristine (tag `v0-pristine`) |
| `07345f9` | Limpa `\usepackage{tikz}` duplicado em `main.tex`/`00introducao.tex` |
| `4e25c12` | Envolve **356 imagens** com `\begin{scope}[blend mode=multiply]` (papel creme) |
| `0534faf` | `blend-mode-log.md` — auditoria linha-a-linha |
| `32acc10` | `CHANGELOG.md` consolidando Fase 0 |

### Fase A — Fixes estruturais (log do Overleaf)

Identificou que o relatório v2 tinha falso positivo (`\eqLdois` emite `\label` internamente, 0 refs quebradas no log). Corrigiu erros REAIS do log de compilação.

| Commit | Descrição |
|---|---|
| `1924f86` | **Preâmbulo**: `textcomp[full]` (resolve option clash com stix2), `booktabs`, `ragged2e`; remove pacotes duplicados (tcolorbox, epigraph, xcolor) |
| `ac61f4c` | **Body**: 12 erros de compilação (math/tag/tabular) em 6 arquivos |
| `1febc72` | Renomeia **5 labels duplicados** (`exemplo_newton_*`, etc. → sufixo `b`) |
| `07ca29e` | Remove **120 linhas de TikZ comentado** em `apendiceareagraficovxt.tex` (234→114 linhas); marca `partVcolisoes.tex` e `rascunho.tex` como DRAFT |
| `7dcd11e` | CHANGELOG registra Fase A |
| `ce4380f` | Merge da branch `fix/estrutural-relatorio-trilha-a` em `main` |
| `bfbe5e6` | Remove imagem `quedalivre.jpeg` duplicada em `00introducao.tex` (teste esquecido) |

### Fase B-1 — Preâmbulo de diagramação

Tipografia P0 + sistema visual base + setup da letrina.

| Commit | Descrição |
|---|---|
| `7af3d95` | **P0 typography**: `\predisplaypenalty=10000`, `\postdisplaypenalty=300`, `\needspace` em `\section`/`\subsection`/`\subsubsection`, espaçamentos canônicos de float, `\captionsetup` melhor (`centerlast`, `width=0.85\textwidth`) |
| `bf30f93` | **Visual primitives**: 6 cores tonais (`\tinta` … `\tinta04`), 6 pesos de filete (`\fileteleve` … `\fileteatobr`), 4 tamanhos canônicos de figura (`\figpequena` … `\figlarga`) |
| `5cb4254` | **Lettrine** (`\usepackage{lettrine}`, default 3 linhas) |
| `364ed49` | Margem inferior das fórmulas: 6pt → **10pt** + `\par` antes do vspace para garantir modo vertical |

### Fase B-2 — Pull quotes Variante B + Letrina aplicada

| Commit | Descrição |
|---|---|
| `82809da` | Comando `\pullquote` Variante B (versão TikZ inicial) |
| `4bc1219` | Pull quote 1: `prefacio.tex` — *"A memória é o resíduo do pensamento"* |
| `696bd11` | Pull quote 2: `intropart1.tex` — *"Antes de explicar o mundo, descrevê-lo"* |
| `0bd9f1a` | Pull quote 3: `00introducao.tex` — *"Tudo funciona como uma espécie de mágica"* |
| `ea18e87` | Pull quote 4: `partIIdinamica02energia.tex` — *"Duas linguagens complementares"* |
| `8f7b519` | Refit `\pullquote`: sem TikZ, aspas itálicas **72pt**, cinza **30%** |
| `8a549c4` | **Letrina** aplicada em 6 capítulos: `00introducao` (U), `partIcinematica01escalar` (N), `partIcinematica02vetor` (V), `partIIdinamica02energia` (V), `partIV0gravidade` (E), `partIVhidrostatica` (A). Newton **pulado** (capítulo abre direto com equação) |

### Fase B-3 — Estrutura editorial

| Commit | Descrição |
|---|---|
| `eb3afe7` | `\frontmatter` / `\mainmatter` / `\backmatter`; cria **4 páginas novas**: `cipfichacatalografica.tex`, `convencoes.tex`, `constantes.tex`, `errata.tex` (com QR code apontando pro repo) |
| `22a35d8` | Setup do **índice remissivo**: `\usepackage{makeidx}` + `\makeindex` + `\printindex`. Entradas `\index{}` no body ficam para sessões gradualmente futuras. |

---

## Estado atual

### Estrutura do livro

```
\frontmatter                          ← páginas i, ii, iii, ...
├─ Capa (\maketitle)
├─ Ficha catalográfica (CIP)            NOVO
├─ Sumário
├─ Dedicatória
├─ Prefácio                           ← com pull quote
├─ Como usar este material
├─ Convenções e notação                 NOVO
└─ Tabela de constantes                 NOVO

\mainmatter                           ← páginas 1, 2, 3, ...
├─ Cap 1: Introdução                  ← lettrine + pull quote
├─ Cap 2: Cinemática Escalar          ← lettrine
├─ Cap 3: Cinemática Vetorial         ← lettrine
├─ Intro Parte I (Cinemática)         ← pull quote
├─ Cap 4: Dinâmica - Descrição Vetorial (Newton)
├─ Cap 5: Dinâmica - Descrição Escalar (Energia)  ← lettrine + pull quote
├─ Intro Parte II (Dinâmica)
├─ Cap 6: Gravitação                  ← lettrine
├─ Cap 7: Dinâmica dos Fluidos        ← lettrine
└─ Intro Parte III (Aplicações)

\appendix
├─ Apêndice A: Equações Exp vs Dem
├─ Apêndice B: Área do gráfico v×t
├─ Apêndice C: Teorema da Energia Cinética
└─ Apêndice D: Arte

\backmatter
├─ Bibliografia
├─ Índice Remissivo                     NOVO (esqueleto, ainda vazio)
└─ Errata (QR code → GitHub issues)     NOVO
```

### Macros úteis disponíveis (sistema visual)

| Macro | O que faz | Uso |
|---|---|---|
| `\eqLdois{tipo}{label}{eq}` | Fórmula em caixa com nível e tipo | `\eqLdois{def}{eq_X}{F = ma}` |
| `\pullquote[atrib]{texto}` | Pull quote com aspas itálicas grandes | `\pullquote[autor]{Frase.}` |
| `\lettrine{L}{etras}` | Capitular grande (3 linhas) | `\lettrine{U}{m} número...` |
| `\figpequena/\figmedia/\figgrande/\figlarga{path}` | 4 tamanhos canônicos de figura (40/60/80/95%) | `\figmedia{imagens/x.png}` |
| `\tinta/\tinta80/.../\tinta04` | 6 densidades tonais P&B | `\color{tinta50}{...}` |
| `\fileteleve/.../\fileteatobr` | 6 pesos canônicos de filete horizontal | `\filetenormal` |

### Itens explicitamente cortados do escopo
- Pull quotes adicionais além dos 4 atuais (decisão sob demanda)
- Hierarquia visual de equações Definição/Demonstrável/Experimental
- Marginalia (gera +80-120 páginas no livro)
- Sobre autor, Formulário consolidado, Glossário, Roadmap, Síntese, Cheat sheet
- QR codes em derivações-chave (só errata mantém)
- Versalete verdadeiro, prefixo de volume "I."
- Exercícios propostos (decisão editorial: sem)

---

## Pendente

- **Entradas `\index{termo}`** espalhadas no body (~800-1000 termos). Setup pronto, conteúdo gradual.
- **Calibração visual** de pull quote e letrina pós-Overleaf real (cor, tamanho, posicionamento).
- **Conteúdo placeholder** a preencher: ISBN/editora na Ficha CIP, número de versão na Errata.
- **Letrina no Cap 4 (Newton)**: pulado porque o capítulo abre direto com equação. Para incluir, precisaria adicionar um parágrafo introdutório.

---

## Documentos de referência (em `definicoes-diagramacao/` do repo v2)

- `diagramacao_guia_formulas.md` — guia técnico de fixes (P0, P1, P2)
- `estetica_diagramacao_guia_formulas.md` — sistema visual P&B (densidades, pesos, letrinas, pull quotes)
- `parecer_editorial_guia_formulas.md` — estrutura de livro (front/back matter, índice, errata)
- `implementacao_pullquote_variante_B.md` — spec do pull quote escolhido
- `specimen_visual_guia_formulas.html` — renderização HTML/CSS de referência

---

## Como compilar

**Overleaf** (recomendado): projeto sincronizado via GitHub. Recompile direto.

**Local** (TeXLive 2025): `pdflatex main.tex` 2-3 vezes para resolver refs/index/toc:
```bash
pdflatex main.tex
makeindex main.idx
bibtex main
pdflatex main.tex
pdflatex main.tex
```

---

*Última atualização: 2026-05-27. Mantenedor: Felipe Guisoli.*
