# Open-Science-Paper

This repository contains a LaTeX document with a two column layout which
can be used for collaborative paper writing. The layout is close to
common paper formats which helps to prepare the figures and tables for
publication. The document combines the LaTeX typesetting capabilities with
the power of the statistic programming language R using the Knitr package
(http://yihui.name/knitr/). This combination has the advantage to enable better
reproducibility of your research offering executable documents to others.

## Prerequisites

To get started with this document a working installation of LaTeX and R is
required. Depending on your LaTeX distribution and the type of installation
(full/subset), packages required for the document might have to be installed
additionally. Some LaTeX distributions (e.g MiKTeX, TeX Live) offer package
managers which help you with this task (MiKTeX can install packages
automatically if they are required from a document).

In R you have to install the Knitr package which is documented here:
https://github.com/yihui/knitr. To use the knit command directly from the
console as it is defined in the documents makefile you have to ensure that the
directory of Knitr is in you systems path. For Unix like systems you could add
some code to your ~.bashrc or the configuration of the shell you are using.
For other systems you may find helpful informations about path changes here
http://www.java.com/en/download/help/path.xml.

```
PATH=$PATH:/path/to/your/R_libs/knitr/bin 
```

Or if you don't like to add a path, you could also redefine the makefile to use a
command like the following to knit the .Rnw document.

```
Rscript -e "library(knitr); knit('open_science_paper.Rnw')"
```

On Windows you need to install GNU make (link: fixme) to use the makefile which compiles
the document for you. The make commands are documented below.

## Usage

The usage of this document with GitHub for collaborative writing is easy and
requires only some small steps. Get a GitHub account and turn it into an
organization account under your account settings. You can now fork the document
to get your own copy of the Open-Science-Paper. After that you can checkout your
copy to a local repository and start writing and invite other researchers for
collaboration.

### Makefile

The standard rule which is called when you use make without any options. The
workflow for this task is Knitr > PDF-LaTeX > BibTeX > PDF-LaTeX (call: make).

```
all: $(DOCUMENT).pdf 

$(DOCUMENT).pdf: $(DOCUMENT).Rnw subdocuments/open_science_paper.cls subdocuments/*.tex 
	$(KNITR) $(DOCUMENT).Rnw $(DOCUMENT).tex --pdf
	$(PDFLATEX) $(DOCUMENT).tex
	$(BIBTEX) $(DOCUMENT)
	$(PDFLATEX) $(DOCUMENT).tex
```

Special rules you can call to to evoke predefined tasks. You have to call make
with one of the names of rules introduced below (e.g make showpdf). The rule
"showpdf" displays the compiled PDF using the PDF viewer defined in the makefile
(call: make showpdf). If you like to use the option you should adapt the variable in
the makefile to your needs (eg: evince).

```
showpdf:
	$(PDFVIEWER) $(DOCUMENT).pdf & 
```

The rule "warnings" displays the warnings from the latest compilation run which
are written into the documents log file (call: make warnings).

```
warnings:
	@-echo "----------------------------------------------------o"
	@-echo "Multiple defined lables!"
	@-echo ""
	@-grep 'multiply defined' $(DOCUMENT).log
	@-echo "----------------------------------------------------o"
	@-echo "Undefined lables!"
	@-echo ""
	@-grep 'undefined' $(DOCUMENT).log
	@-echo "----------------------------------------------------o"
	@-echo "Warnings!"
	@-echo ""
	@-grep 'Warning' $(DOCUMENT).log
	@-echo "----------------------------------------------------o"
	@-echo "Over- and Underfull boxes!"
	@-echo ""
	@-grep 'Overfull' $(DOCUMENT).log
	@-grep 'Underfull' $(DOCUMENT).log
	@-echo "----------------------------------------------------o"
```

The rule "archive" calls zip

```
archive:
	zip -r $(ARCHNAME).zip $(ARCHFILES)
```

```
clean:
	@-rm -r $(CLEANFILES)	
```
