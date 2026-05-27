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
