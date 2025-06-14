MAKEFLAGS += --warn-undefined-variables
SHELL := bash
.SHELLFLAGS := -eu -o pipefail -c
.DEFAULT_GOAL := dvi
.DELETE_ON_ERROR:
.SUFFIXES:

name = paper_template
diffname = $(name)-diff

# Comment out one or the other of the lines below but not both:
bibname = $(HOME)/texmf/bibtex/bib/Bibliography.bib
#bibname = $(name).bib

epspics := $(wildcard Graphics/*.eps)
pngpics := $(wildcard Graphics/*.png)
jpgpics := $(wildcard Graphics/*.jpg)
svgpics := $(wildcard Graphics/*.svg)
diapics := $(wildcard Graphics/*.dia)

deps = $(name).tex $(bibname) $(diapics:.dia=.eps) $(pngpics:.png=.eps) $(jpgpics:.jpg=.eps) $(svgpics:.svg=.eps) $(epspics)

# ===================== Main targets =====================
.PHONY: dvi ps pdf bib docx

dvi: $(name).dvi

ps: $(name).ps

pdf: $(name).pdf

bib: $(name).bib

docx: $(name).docx

# The `bib` target makes a smaller bibliography file that only includes the references actually
# cited in your .tex file. Note the following:
#
# Note 1: There is no dependency on the .dvi file here since adding a dependency on the .dvi
# would be circular. Just make sure to run `make dvi` first before generating a new .bib
# file, since `bibexport` requires a .bbl file. We are not adding a dependency on the .bbl,
# though, since if we do then the bibliography will unnecessarily have to be recreated everytime
# the .bbl file is changed, and this isn't necessary.
#
# Note 2: The name specified in the \bibliography{...}{} directive in your .tex file must be
# the _source_ bibliography and not the target bibliography. Once the target is generated you
# should then change the name in your .tex file.
#
# Note 3: Before compiling your .tex file using the target bibliography, make sure to change
# the definition of `bibname` above (i.e., in this Makefile).

$(name).bib:
	bibexport -o $(name).bib $(name).aux
	bibsort.sh < $(name).bib > /tmp/$(name).sorted.bib
	mv -f /tmp/$(name).sorted.bib $(name).bib

$(diapics:.dia=.eps): $(diapics)
	for diafile in $(diapics:.dia=); \
	do \
		dia --export=$$diafile.eps $$diafile.dia; \
	done

$(name).pdf: $(name).ps
	# Use this first command to compile directly to .pdf
	# latexmk $(name) -pdfps
	ps2pdf $(name).ps

$(name).ps: $(name).dvi
	# Use this first command to compile directly to .ps
	# latexmk $(name) -ps -pdf-
	dvips $(name)

$(name).dvi: $(deps)
	latexmk $(name) -dvi -ps- -pdf-

$(name).bbl: $(deps)
	latexmk $(name) -dvi- -ps- -pdf- -bibtex

$(name).docx: $(deps)
	pandoc --standalone $(name).tex --bibliography=$(bibname) -M reference-section-title=References -o $(name).docx

# ===================== Diffs =====================
.PHONY: rawdiff diffpdf difftex

rawdiff:
	# Note that wdiff is defined in your ~/.gitconfig file
	git wdiff $(name).tex

diffpdf: $(diffname).pdf

difftex: $(diffname).tex

$(diffname).pdf: $(diffname).tex
	latexmk $(diffname) -pdfps

$(diffname).tex: $(name).tex $(deps)
	# Note that ldiff is defined in your ~/.gitconfig file.
	#
	# Note also that latexdiff is not perfect. Especially when there are changes
	# _within_ mathematical equations, you may have trouble compiling the
	# generated diff file. In that case you should manually edit the file in
	# those spots. This is easy if you understand how \DIFaddbegin, \DIFadd,
	# \DIFend, etc. work (which is straightforward).
	#
	# Also, sometimes latexdiff fucks up the colours for some reason. You can
	# always customise the colours manually in the diff file's preamble
	# to fix this. If the colours are fucked up you can just set them to
	# black. It won't be as nice but it will still work fine.
	#
	# To run the diff against a particular REF (e.g, "HEAD"), run like:
	# make difftex REF=HEAD
	git ldiff $(REF) $(name).tex > $(diffname).tex

# ===================== Wordcounts =====================
.PHONY: wc wcdvi wcpdf

wc:
	texcount -inc -sum $(name).tex

wcbib: $(name).bbl
	texcount -inc -incbib -sum $(name).tex

wcdvi: $(name).dvi
	catdvi $(name).dvi | wc -w

wcpdf: $(name).pdf
	pdftotext $(name).pdf - 2>/dev/null|wc -w

# ===================== Clean =====================
.PHONY: cleansome cleanmore clean cleandiff cleanpdf cleanps cleanbib

cleansome:
	rm -f $(name).ent $(name).fls $(name).blg $(name).bbl $(name).log $(name).aux $(name).nav $(name).out $(name).snm $(name).toc $(name).fdb_latexmk $(name)*vrb *eps-converted-to.pdf $(diapics:.dia=.eps) $(pngpics:.png=.eps) $(jpgpics:.jpg=.eps) $(svgpics:.svg=.eps) $(diffname)* $(pics:.dia=.eps) $(lastword $(MAKEFILE_LIST))~ $(name)*~
	rm -Rf auto

cleanmore: cleansome
	rm -f $(name).dvi $(name).ps

clean: cleanmore
	rm -f $(name).docx $(name).pdf

cleandiff:
	rm -f $(diffname)*

cleanpdf:
	rm -f $(name).pdf

cleanps:
	rm -f $(name).ps

cleandvi:
	rm -f $(name).dvi

cleanbib:
	rm -f $(name).bib
