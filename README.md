# [Магистратура, семестр 1](sem09/index.md)
# Мои наработки
## Makefile
Цель для Makefile, собирающая все файлы \*.cpp в данной директории в .exe с учётом CXX и CXXFLAGS:
```make
build.exe: $(addsuffix .o,$(basename $(wildcard *.cpp)))
	$(CXX) $(CXXFLAGS) $+ -o $@
```
## LaTeX
Мой шаблон документа main.tex:
```latex
\documentclass[12pt]{article}

%\usepackage[utf8]{inputenc}
%\usepackage[T2A]{fontenc}
\usepackage{fontspec}
\usepackage[english,russian]{babel}
\usepackage{fullpage}
%\usepackage{indentfirst}
%\usepackage{hyperref}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{float}
%\usepackage{proof}
%\usepackage{xcolor}
%\usepackage{graphicx}
%\usepackage{textcomp}
\usepackage{tikz}
%\usetikzlibrary{patterns}
%\usepackage{listings}
%\usepackage{algorithmicx}
%\usepackage{algpseudocodex}
%\usepackage{algorithm2e}
\usepackage[outputdir=out]{minted}

\setmainfont{Noto Sans}
\setmonofont{JetBrains Mono}

\begin{document}
    \hfill
    \begin{tabular}{ll}
        Студент: & Антон Суркис \\
        Группа:  & M4141        \\
        Дата:    & \today       \\
    \end{tabular}
    \hrule

    \newcommand{\N}{\mathbb{N}}
    \newcommand{\R}{\mathbb{R}_{>0}}
    \renewcommand{\O}{\mathcal{O}}
    \renewcommand{\o}{o}
    \newcommand{\Const}{\mathit{Const}}

    \newcommand{\fn}[1]{\mathbf{#1}}
    \newcommand{\cI}{\fn{I}}
    \newcommand{\comega}{\fn{\omega}}
    \newcommand{\cK}{\fn{K}}
    \newcommand{\cKs}{\fn{K_*}}
    \newcommand{\cC}{\fn{C}}
    \newcommand{\cB}{\fn{B}}
    \newcommand{\cS}{\fn{S}}
    \newcommand{\beq}{=_{\beta}}
    \newcommand{\bred}{\longrightarrow_{\beta}}
    \newcommand{\cT}{\fn{true}}
    \newcommand{\cF}{\fn{false}}
    \newcommand{\cY}{\fn{Y}}

    \newcommand{\paren}[1]{\left ( #1 \right )}
    \newcommand{\brackets}[1]{\left [ #1 \right ]}
    \newcommand{\braces}[1]{\left \{ #1 \right \}}
    \newcommand{\floor}[1]{\left \lfloor #1 \right \rfloor}
    \newcommand{\ceil}[1]{\left \lceil #1 \right \rceil}
    \newcommand{\abs}[1]{\left | #1 \right |}

    \input{task01}
    \input{task02}
    % ...
\end{document}
```
### Документация по часто используемым пакетам
- [tikz](http://mirrors.ctan.org/graphics/pgf/base/doc/pgfmanual.pdf) ([CTAN](https://www.ctan.org/pkg/pgf))
- [algorithmicx](http://mirrors.ctan.org/macros/latex/contrib/algorithmicx/algorithmicx.pdf) ([CTAN](https://www.ctan.org/pkg/algorithmicx))
- [algpseudocodex](http://mirrors.ctan.org/macros/latex/contrib/algpseudocodex/algpseudocodex.pdf) ([CTAN](https://www.ctan.org/pkg/algpseudocodex))
- [minted](https://mirror.truenetwork.ru/CTAN/macros/latex/contrib/minted/minted.pdf) ([CTAN](https://ctan.org/pkg/minted))

## [Математика](./math.md)
#math
