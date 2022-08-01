TEXINPUTS := ${TEXINPUTS}:.:Chapters/Chapter1:Chapters/Chapter2:Chapters/Chapter3:Chapters/Chapter4:Chapters/Chapter5:Chapters/Chapter6:Appendices
export TEXINPUTS

chapterfiles = Preamble \
Vita \
Chapters/Chapter1/chapter1 \
Chapters/Chapter2/chapter2 \
Chapters/Chapter3/chapter3 \
Chapters/Chapter4/chapter4 \
Chapters/Chapter5/chapter5 \
Chapters/Chapter6/chapter6 \
Appendices/appendix1 \
Appendices/appendix2 \
Appendices/appendix3 \
Appendices/appendix4 \
Appendices/appendix5 \
Appendices/appendix6

main = Main

ps: $(main).ps

dvi: $(main).dvi

pdf: $(main).pdf

$(main).pdf: $(main).ps
	ps2pdf $(main).ps

$(main).ps: $(main).dvi
	dvips -t letter $(main).dvi

$(main).dvi: $(main).tex $(chapterfiles:=.tex)
	latex $(main)
	bibtex $(main)
	latex $(main)
	latex $(main)

clean:
	rm -f $(main).lof $(main).lot $(main).toc $(main).out $(main).ent $(main).bbl $(main).blg $(main).ps $(main).dvi $(main).log $(main).aux $(main)*~ Makefile~
	for chap in $(chapterfiles); \
	do \
		rm -f $$chap.aux $$chap.bbl $$chap.blg $$chap*~; \
	done
	find . -type d -name auto -exec rm -Rf {} \;

cleanall: clean
	rm -f $(main).pdf
