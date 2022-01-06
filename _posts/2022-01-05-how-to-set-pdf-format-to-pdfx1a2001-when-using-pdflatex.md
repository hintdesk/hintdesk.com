---
title: "How to set PDF format to PDF/X-1a:2001 when using pdflatex?"
date: 2022-01-05 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: pdf
tags: latex,pdf,pdf/x-1a:2001
---

## Intro
Last week I ordered a print copy of the ebook "Machine Learning Cơ Bản" of tiepvupsu https://github.com/tiepvupsu/ebookMLCB. I thought it should be simple because I already had a PDF copy. It just needs to be printed. However, the printing house has asked me for a PDF format of PDF/X-1a:2001. In this article, I will show you how to compile a PDF/X-1a:2001 file from a .tex file.

## Steps

- Check out the code from GIT repository.
- Locate <em>book_ML.tex</em>. Open it, find <em>begin{document}</em>, add the following lines before it. These lines will set the format to PDF/X-1a:2001 and PDF version to 3 (or PDF 13).

```terminal
\pdfinfo{
/GTS_PDFXVersion (PDF/X-1a:2001)
}
\pdfminorversion=3
```

- Install <em>mactex</em>. It will take a while to get through the installation.

```terminal
brew install mactex
```

- Run <em>pdflatex</em> (1st time) to build the content of the PDF file.

```terminal
pdflatex book_ML.tex
```

- After running the command, you will receive some newly generated files 
  - book_ML.log
  - book_ML.pdf
  - book_ML.toc
  - book_ML.idx
  - book_ML.aux

- Open <em>book_ML.pdf</em>, you'll see there are about 398 pages. Bibliography and Index are still missing.
- Run <em>bibtex</em> to create the bibliography. 
	
```terminal
bibtex book_ML
```

- After running the command, some new files will be generated 
  - book_ML.blg
  - book_ML.bbl

- Run <em>pdflatex</em> (2nd time) to build the bibliography.

```terminal
pdflatex book_ML.tex
```

- After running the command, open <em>book_ML.pdf</em>. The bibliography will be generated and appended to the end of the file but the anchors/refs (in front of each ref) are still missing.

- Run <em>pdflatex</em> (3rd time) to build the bibliography references.

```terminal
pdflatex book_ML.tex
```

- After running the command, open <em>book_ML.pdf</em>. The bibliography is now compiled with correct references.
- Run <em>makeindex</em> to create the printindex.

```terminal
makeindex book_ML.idx
```

- After running the command, some new files will be generated 
  - book_ML.ilg
  - book_ML.ind

- Run <em>pdflatex</em> (4th time) to build the printindex.

```terminal
pdflatex book_ML.tex
```

- After running the command, open <em>book_ML.pdf</em>. The file is now fully generated with the format PDF/X-1a:2001.
- You can check the conformance with Adobe PDF Reader.

{%
    include image.html
    year='2022'
    month='01'
    file='06_001.png'
    alt='Adobe PDF Reader Conformance'
%}
