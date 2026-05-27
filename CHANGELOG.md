# CHANGELOG — Guia de Fórmulas Explicadas (Física Volume 01)

Log cronológico de todas as alterações automatizadas no projeto.

Convenção: tipo de mudança em **[CÓDIGO]** + descrição + commit/branch quando aplicável.

Tipos:
- `[FEAT]` nova funcionalidade / transformação aplicada ao livro
- `[CHORE]` limpeza, organização, configuração
- `[DOCS]` documentação, relatórios, logs

---

## 2026-05-27 — Setup inicial e blend mode

### `[CHORE]` Snapshot pristine
- **Commit:** `f3c65ba`
- **Branch/Tag:** `original` / `v0-pristine`
- Estado intocado do projeto, snapshot do conteúdo no Google Drive.
- 20 arquivos `.tex` no root + diretórios de imagens.

### `[CHORE]` Limpeza do preâmbulo
- **Commit:** `07345f9`
- Remoção de `\usepackage{tikz}` duplicado em `main.tex` (linha 85).
- Remoção de `\usepackage{tikz}` indevido em `00introducao.tex:4` (estava fora do preâmbulo).

### `[FEAT]` Blend mode multiply em 356 imagens
- **Commit:** `4e25c12`
- Envolvidas 356 ocorrências de `\includegraphics` com:
  ```latex
  \begin{tikzpicture}
    \begin{scope}[blend mode=multiply]
      \node {\includegraphics[opts]{path}};
    \end{scope}
  \end{tikzpicture}
  ```
- 10 arquivos modificados.
- 1 imagem já estava em blend mode (exemplo prévio em `00introducao.tex`).
- 1 imagem pulada (capa em `main.tex`, dentro de tikzpicture overlay).
- 8 includegraphics comentados preservados intactos.
- 0 scopes malformados (auditoria automática).

### `[DOCS]` Log auditável da transformação
- **Commit:** `0534faf`
- Criação do `blend-mode-log.md` com tabela linha por linha (arquivo, linha, imagem, ambiente).

---

## 2026-05-27 (continuação) — Fase A: fixes estruturais a partir do log Overleaf

Branch: `fix/estrutural-relatorio-trilha-a`. Quatro commits aplicando achados do log de compilação real do Overleaf (601 páginas, ~58 MB), em vez do relatório estático da v2 (que tinha falso positivo importante).

### `[DOCS]` Falso positivo identificado
- Relatório v2 reportou "11 refs `eq_energia_*` quebradas" em `partIIdinamica02energia.tex`. Investigação na v1 mostrou que `\eqLdois{tipo}{label}{formula}` (definido em `main.tex:129`) **já emite `\label{#2}` internamente**. O grep estático do relatório não conhecia esse comando. Log do Overleaf confirma: **zero** `Reference undefined` em 601 páginas.

### `[FIX]` Preâmbulo `main.tex` (commit `1924f86`)
- `\PassOptionsToPackage{full}{textcomp}` antes de `Baskervaldx` — resolve option clash com `stix2[full]`.
- Adicionado `\usepackage{booktabs}` (resolve `\toprule`/`\midrule`/`\bottomrule` undefined em `partIIdinamica01newton.tex:2330`).
- Adicionado `\usepackage{ragged2e}` (resolve `\RaggedRight` undefined em tabular de `partIIdinamica02energia.tex:3181`).
- Removidas 3 duplicatas de `\usepackage{tcolorbox}`, 1 de `\usepackage{epigraph}`, 2 de `\usepackage{xcolor}`, 1 `\tcbuselibrary` (já vem com `[most]`).

### `[FIX]` Erros de compilação no body (commit `ac61f4c`)
- `00comousar.tex:59,75` — `}` extra/faltante em `\boxed{...}` dentro de tcolorbox.
- `partIcinematica01escalar.tex:2496-2520` — 5 alternativas (a-e) de exemplo abriam `\left(` sem `$` inicial. Linha 2626: `a^\cancel{2}` → `a^{\cancel{2}}` (operador `^` engolia `\cancel` como argumento).
- `partIIdinamica01newton.tex:2351,2355,2357,2362` — 4 `\tag{N}` em `$$...$$` (TeX puro não suporta; só amsmath). Envolvidos em `equation*`.
- `partIIdinamica02energia.tex:988` — `m^\cancel 2` → `m^{\cancel{2}}`. Linha 1108: `\hline` solto fora de tabular (gera `Misplaced \noalign`) → `\noindent\rule{\textwidth}{0.4pt}`.
- `partIV0gravidade.tex:779,1532` — `N{\cdot}m²/kg²` com superscripts unicode fora de math → `$\text{N}{\cdot}\text{m}^2/\text{kg}^2$`. Linha 989: `r^ \cancel2` → `r^{\cancel{2}}`.

### `[FIX]` 5 labels duplicados renomeados (commit `1febc72`)
Acusados pelo log via `Label X multiply defined`. Nenhum referenciado externamente (verificado por grep). Sufixo `b` na segunda ocorrência:
- `exemplo_newton_poliamovel03` → `…03b` (linha 3240 + linha 3316 comentada).
- `exemplo_newton_vloopex02` → `…02b` (linha 5270).
- `exemplo_newton_impulso` → `…impulsob` (linha 6032).
- `exemplo_dinamicarotacao_teorema3forcasex02` → `…02b` (linha 8083).
- `exemplo_gravitacao_velocidadedeescapeex03` → `…03b` (linha 2097, `partIV0gravidade.tex`).

### `[CHORE]` Limpeza e flags de DRAFT (commit `07ca29e`)
- `apendiceareagraficovxt.tex` — removidos 3 blocos de TikZ alternativo comentado (linhas 36-54, 81-135, 188-233), 120 linhas mortas no total. Labels ativos das figuras preservados. Resultado: 234 → 114 linhas, idêntico à v2.
- `partVcolisoes.tex` (stub `\chapter{Colisões}` sem conteúdo) e `rascunho.tex` (templates + TODO) marcados com header `% DRAFT - NAO incluido em main.tex` para deixar a intenção explícita.

### `[OPEN]` Itens não tratados nesta fase
- 4 `pdfTeX warning: destination with the same identifier (equation.X.Y) has been already used, duplicate ignored` (cap. 3, 5, 7, 8). Warning, não quebra compilação; requer investigação mais profunda do uso de `\eqLdois` em conjunto com `equation` env nessas equações específicas.
- 13 issues críticos visuais + 62 médios identificados nos lotes B1-B4 do relatório v2. Atacar na **Fase B** com PDF do Overleaf como referência.
- 7 padrões recorrentes (quebra ruim de exemplos, figura longe da `\ref`, captions distantes etc.) — Fase B.

---

## Nota sobre análise de diagramação

A análise visual + estrutural completa (5 relatórios) foi feita sobre a **versão v2 do projeto**, hospedada em [`Universo-Narrado/gfe-fisica`](https://github.com/Universo-Narrado/gfe-fisica). Os achados de **padrões recorrentes** (figuras longe da referência, captions distantes, viúvas em exemplos, etc.) e da **trilha estrutural** (refs quebradas, pacotes duplicados em `main.tex`, equações sem label) **provavelmente também se aplicam a este projeto** — recomendado revisitar.

---

## Workflow de versionamento (adotado a partir de 2026-05-27)

- **`main`** — branch de trabalho corrente
- **`original`** — ponteiro permanente para o commit pristine (`f3c65ba`). Nunca recebe commits.
- **`v0-pristine`** — tag imutável apontando para o pristine.
- **Para correções/transformações futuras**, criar branch a partir de `main`:
  ```bash
  git checkout main && git pull
  git checkout -b fix/<descricao-curta>
  # ... editar, commitar ...
  git push -u origin fix/<descricao-curta>
  # abrir PR no GitHub, mergear quando ok
  ```
- **CHANGELOG.md sempre atualizado** após cada bloco lógico de trabalho.
