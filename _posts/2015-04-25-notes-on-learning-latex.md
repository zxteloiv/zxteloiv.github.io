---
layout: post
title:  "Notes on learning latex"
date:   2015-04-25 15:51
tags: latex typesetting writing
categories: note learning
---

Here are some notes when I learn to write in LaTeX.

### 1. TeX environment

LaTeX is a typesetting system, which adopts the merit that to separate the writing and typesetting of a single document.

We need to write a tex source file, and use a compiler, often an appropriate TeX system, to get some standard format file to finally read or print.

The compiler or the TeX system is

* On Mac, use the [MacTeX](https://www.tug.org/mactex/) or [BasicTeX](https://www.tug.org/mactex/morepackages.html).
* On Linux, use the texlive.
* On Windows, often some integrated environment can be obtained online.

Since every time we modify the .tex file we have to compile it to see the result, there are some editors and many integrated TeX environment, like TeXShop, that make this routine more convenient.

And my choice is BasicTex(mainly the pdflatex utility in it) + vim + Preivew(to see the resulted pdf).

As the configuration is complicate, I also prefer some online latex services which requires writing .tex file only and give you the final pdf to download, which is simple and thin.

* [Authorea](https://www.authorea.com/)
* [ShareLaTeX](https://www.sharelatex.com/)
* [Overleaf](https://www.overleaf.com/)

Others people also choose emacs and AUCTeX, but I don't know much about emacs so I didn't try it.

This note here is just a memo for me to quickly write tex file.

### 2. Basic Syntax

#### 2.1 Compositions

A document begins with

    \documentclass[12pt, a4paper, twocolumn]{article}

Also, the page size settings can be set using the geometry library like this:

    \usepackage{geometry}
    \geometry{legalpaper, landscape, margin=2in}

or this:

    \usepackage[letterpaper, landscape, margin=2in]{geometry}

Refer to [the page](https://www.sharelatex.com/learn/Page_size_and_margins) to get more about geometry.

Always use the UTF-8 encoding for .tex file.

    \usepackage[utf8]{inputenc}

And we can set properties for this article:

    \title{ LaTeX playground }
    \author{Xiang Zhang}
    \date{April 2015}

And the article main context lies in:

    \begin{document}
    % ... bla bla bla ...
    \end{document}

The title can be made by 

    \maketitle

and use a wrapper for it if we want the title resides in a single page:

    \begin{titlepage}
    \maketitle
    \end{titlepage}

In a scientific paper we would like to write an abstract section before the main content. Write like this:

    \begin{abstract}
    % ... abstract ... bla bla ...
    \end{abstract}

Use `%` sign to write comment, and use a wrapper (with the `comment` package) to get a multiline comment.

    \usepackage{comment}
    \begin{comment}
    blabla
    blabla
    \end{comment}

#### 2.2 Paragraph and Text

Just write anything without a command it will become a paragraph.

    This is a paragraph.

Use bold text `the \textbf{bold} word`.

Use italic text `the \textit{italic} word`.

Use emphasized text `the \emph{emphasized} word`. Emphasized contents will be italic if the context around is not italic, and will be not italic if the context around is italic.

Use double slash or `\newline` command to put the following context in a new line.

    First Line blabla. \newline Second line blabla. \\ Third Line.

A new line in .tex file will not generate a new line in the result document.

    First line blabla.
    Still in the first line blablabla.

Double new line generate a new paragraph, or use the `\par` command.

    First paragraph.

    Second paragraph. \par Third paragraph

#### 2.3 List

An unordered list can be written with _itemize_

    \begin{itemize}
        \item one
        \item another nested list
        \begin{itemize}
            \item waaaaa
            \item aaaa
        \end{itemize}
    \end{itemize}

An ordered list can be written with _enumerate_

    \begin{enumerate}
        \item first
        \item second
    \end{enumerate}

The number can start from any number, by using `setcounter` command, rather than 1

    \begin{enumerate}
        \setcounter{enumi}{3}
        \item 4th Elysion
        \item 5th Roman
        \item 6th Moira
        \item 7th Marchen
        \setcounter{enumi}{8}
        \item 9th Nein
    \end{enumerate}

By using `renewcommand` we can use other notation as the label for each level. Setting it globally will work thoughout the article while only the following list if it precedes the list.

    \renewcommand{\labelenumi}{\Roman{enumi}}
    \begin{enumerate}
        \setcounter{enumi}{3}
        \item 4th Elysion
        \item 5th Roman
        \item 6th Moira
        \item 7th Marchen
        \setcounter{enumi}{8}
        \item 9th Nein
    \end{enumerate}

*TODO: the enumerate renew command is still not clear*


