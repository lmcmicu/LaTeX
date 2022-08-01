bibname = biblio

chapterfiles = front_matter \
intro \
chapter1 \
chapter2

main = book
deps = $(main).tex $(chapterfiles:=.tex) $(bibname).bib

pdf: $(main).pdf

dvi: $(main).dvi

ps: $(main).ps

$(main).pdf: $(deps)
	latexmk $(main).tex -pdfps

$(main).ps: $(deps)
	latexmk $(main).tex -ps

$(main).dvi: $(deps)
	latexmk $(main).tex -dvi

clean:
	rm -f pccp_full.pdf
	rm -f $(main).fls $(main).blg $(main).bbl $(main).log $(main).aux $(main).nav $(main).out $(main).snm $(main).toc $(main).lof $(main).lot $(main).fdb_latexmk $(main)*vrb *eps-converted-to.pdf $(main)*~ $(main).pdf $(main).ps $(main).dvi $(lastword $(MAKEFILE_LIST))~
	for chap in $(chapterfiles); \
	do \
		rm -f $$chap.aux $$chap.bbl $$chap.blg $$chap*~; \
	done
	rm -f "*eps-converted-to.pdf"
