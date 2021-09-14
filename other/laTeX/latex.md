### Sections
- [Introduction](#introduction)
- [Instalation](#instalation)
- [Skeleton](#skeleton)
- [Adding images](#adding-images)
- [Lists](#lists)
- [Math in LaTeX](#math-in-latex)
- [Basic formatting](#basic-formatting)
- [Creating tables](#creating-tables)
- [Packages](#packages)


---

## Introduction

[Notes Overleaf LaTeX tutorial](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes)

`WYSIWYM` - what you see is what you mean 

Packages allow users to do even more with LaTeX, such as add footnotes, draw schematics, create tables etc.

>One of the most important reasons people use LaTeX is that it separates the content of the document from the style.

>To see the result of changes in the PDF you have to compile the document.

Overleaf

## Instalation

- LaTeX Workshop extension on VS Code

```
sudo apt install latexmk
```

[Using Latexmk](https://mg.readthedocs.io/latexmk.html)

```bash
#!/usr/bin/env bash

#file_name=${tex_file%????}
file_name=${1::-4}

pdflatex $1
rm "$file_name.log"
rm "$file_name.aux"
```

## Skeleton

file.tex

```latex
%LaTeX comment
%document type declaration: article, book, report
\documentclass{article}

%the body of the document
\begin{document}
    
Lorem ipsum

\end{document}
```

`LaTeX preambule` - everything before \beginn{document} command.

```latex
%font & paper size{a4paper,legalpaper}
\documentclass[12pt, letterpaper]{article}

%encoding for the document
\usepackage[utf8]{inputenc}

\title{LaTeX cheatsheet}

\author{AhmedQuas \thanks{Overleaf tutorial}}

\date{September 2021}

\begin{document}

%Print title, author & date
\maketitle

Lorem ipsum
\end{document}
```

Simple formatting commands:
- `\textbf{}` - bold,
- `\textit{}` - italics,
- `\underline{}` - underline,
- `\emph{}` - depends on the context

## Adding images

`Packages` can be used to change the default look of your LaTeX document, or to allow more functionalities.

Upload images into images directory

```latex
\documentclass{article}
\usepackage{graphicx}
\graphicspath{ {images/} }

\begin{document}

\includegraphics{universe}

\end{document}
```

Captions, labels and references

```latex
\begin{figure}[h]
    \centering
    \includegraphics[width=0.25\textwidth]{mesh}
    
    %figure excerpt, short name
    \caption{a nice plot}
    
    %used to refer to that figure in text using \ref{} command
    \label{fig:mesh1}

\end{figure}

As you can see in the figure \ref{fig:mesh}, the
```

>Note: If you are using captions and references on your own computer, you will have to `compile the document twice` for the references to work. Overleaf will do this for you automatically.'

## Lists

>Environments are sections of our document that you want to present in a different way to the rest of the document.

```latex
%unordered list
\begin{itemize}
    \item The first dot
    \item The second dot
\end{}

%ordered list
\begin{enumerate}
    \item The first number
    \item The second number
\end{}
```

## Math in LaTeX

Modes of writing mathematical expression:
- `inline` - write formulas that are part of text,
    
    ```latex
    $E=mc^2$
    
    \(E=mc^2\)
    
    \begin{math}
    E=mc^2
    \end{math}
    ```

- `display` - formulas that are put on separate lines,
  - numbered
  - unnumbered

    ```latex
    \[ E=mc^2 \]

    $$ E=mc^2 $$

    \begin{displaymath}
    E=m
    \end{displaymath}

    %require to use amsmath package
    \begin{equation}
    E=m
    \end{equation}
    ```

## Basic formatting

`Abstract` - brief overview of the main subject of the paper

```latex
\begin{document}

\begin{abstract}
Brief overview of the main subject of the paper
\end{abstract}

\end{document}
```
Basic formatting commands:
- `New paragraph` - just press enter twice
- `\newline` - start a new line without actually starting a new paragraph
- `\chapter{}` - 
- `\section{}` - 
- `\section*{}` - disabled section numering
- `\subsection{}` -

<div align='center'>

| Depth level  | Name  |
|:---:      |:---      |
| -1 | \part{part} |
| 0 | \chapter{chapter} |
| 1 | \section{section} |
| 2 | \subsection{subsection} |
| 3 | \subsubsection{subsubsection} |
| 4 | \paragraph{paragraph} |
| 5 | \subparagraph{subparagraph} |

</div>

## Creating tables

```latex
\begin{document}

%{ c c c } specify that there will be 3 centered columns
%c - center align
%r - right
%l - left
%& - specify breaks in the table entries
%\\ - new line sign

\begin{tabular}{ c c c }
 cell1 & cell2 & cell3 \\
 cell4 & cell5 & cell6 \\
 cell7 & cell8 & cell9
\end{tabular}

%version with borders
\begin{tabular}{ |c|c|c| }
 \hline
 cell1 & cell2 & cell3 \\
 cell4 & cell5 & cell6 \\
 cell7 & cell8 & cell9
 \hline
\end{tabular}

\end{document}
```

Table with captions, labels and references

```latex
\begin{table}[h!]
\centering
\begin{tabular}{||c c||}
 \hline
 col1 & col2 \\ [0.5ex]
 \hline \hline
 1 & 2 \\
 3 & 4 \\
 5 & 6 \\ [1ex]
 \hline
\end{tabular}
\caption{Table to test captions and labels}
\label{table:data}
\end{table}


The \ref{table:data} is an example of referenced \LaTeX{} elements.
```

[Online LaTeX table generator](https://www.tablesgenerator.com/)

To add table of contents just use `\tableofcontents`

## Packages

```latex
%in preambule section
\usepackage{}
```

List of usefull packages:
- `inputenc` - proper encoding e.g. utf8,
- `graphicx` - include images,
- `amsmath` - math commands,
- `parskip` - 

>Note that \part and \chapter are only available in report and book document classes.

