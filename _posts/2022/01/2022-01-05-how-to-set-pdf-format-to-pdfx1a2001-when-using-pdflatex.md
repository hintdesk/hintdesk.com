---
title: "How to set PDF format to PDF/X-1a:2001 when using pdflatex?"
date: 2022-01-05 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: pdf
tags: latex
---

## Intro
Last week I ordered a print copy of the ebook "Machine Learning Cơ Bản" of tiepvupsu https://github.com/tiepvupsu/ebookMLCB. I thought it should be simple because I already had a PDF copy. It just needs to be printed. However, the printing house has asked me for a PDF format of PDF/X-1a:2001. In this article, I will show you how to compile a PDF/X-1a:2001 file from a .tex file. The generated PDF file will have no bookmark, no image transparency.

## Steps

- Check out the code from GIT repository.
- Locate **book_ML.tex**. Open it.
- Find **documentclass**, add the following line before it. The line will set PDF version to 3 (or PDF 13)

```terminal
\pdfminorversion=3
```

- Find **begin{document}**, add the following line before it. The line will set the format to PDF/X-1a:2001.

```terminal
\usepackage[x-1a1]{pdfx}
```

- Install **mactex**. It will take a while to get through the installation.

```terminal
brew install mactex
```

- Run **pdflatex** (1st time) to build the content of the PDF file.

```terminal
pdflatex book_ML.tex
```

- After running the command, you will receive some newly generated files 
  - book_ML.log
  - book_ML.pdf
  - book_ML.toc
  - book_ML.idx
  - book_ML.aux

- Open **book_ML.pdf**, you'll see there are about 398 pages. Bibliography and Index are still missing.
- Run **bibtex** to create the bibliography. 
	
```terminal
bibtex book_ML
```

- After running the command, some new files will be generated 
  - book_ML.blg
  - book_ML.bbl

- Run **pdflatex** (2nd time) to build the bibliography.

```terminal
pdflatex book_ML.tex
```

- After running the command, open **book_ML.pdf**. The bibliography will be generated and appended to the end of the file but the anchors/refs (in front of each ref) are still missing.

- Run **pdflatex** (3rd time) to build the bibliography references.

```terminal
pdflatex book_ML.tex
```

- After running the command, open **book_ML.pdf**. The bibliography is now compiled with correct references.
- Run **makeindex** to create the printindex.

```terminal
makeindex book_ML.idx
```

- After running the command, some new files will be generated 
  - book_ML.ilg
  - book_ML.ind

- Run **pdflatex** (4th time) to build the printindex.

```terminal
pdflatex book_ML.tex
```

- After running the command, open **book_ML.pdf**. The file is now fully generated with the format PDF/X-1a:2001.
- You can check the conformance with Adobe PDF Reader.

{%
    include image.html
    year='2022'
    month='01'
    file='06_001.png'
    alt='Adobe PDF Reader Conformance'
%}

- To clear all problems with embedded fonts, I have to execute these commands against the newly created **book_ML.pdf**.

```terminal
pdf2ps book_ML.pdf
ps2pdf13 -dPDFSETTINGS=/prepress book_ML.ps book_ML_2.pdf
```